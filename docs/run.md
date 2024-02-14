# Running the Pipeline

## Running with a Custom Config File

The typical command to run the pipeline based on your custom parameters defined/saved in the standard_params.config (more information about profiles and parameter sets below) and created conda environment is as follows:

* `nextflow run main.nf -profile standard,conda`
OR with the parameters specified in the .json/.yaml files with the following command:

* `nextflow run main.nf -profile standard,conda --<param name> <param value>`
## Running with Docker or Singularity

Other options for the run environment include `docker` and `singularity`. These options can be used simply by replacing the second profile option:

* `nextflow run main.nf -profile standard,<docker/singularity>`
Either one of the above commands will launch the nextflow pipeline and show the progress of the subworkflow:process and checks looking similar to below depending on the entrypoint specified.

* `N E X T F L O W  ~  version 22.10.0`
* `Launching main.nf [festering_spence] DSL2 - revision: 3441f714f2`
* `executor >  local (7)`
* `[e5/9dbcbc] process > VALIDATE_PARAMS                                  [100%] 1 of 1` 
* `[53/a833be] process > CLEANUP_FILES                                    [100%] 1 of 1`
* `[e4/a50c97] process > with_submission:METADATA_VALIDATION (1)          [100%] 1 of 1`
* `[81/badd3b] process > with_submission:LIFTOFF (1)                      [100%] 1 of 1`
* `[d7/16d16a] process > with_submission:RUN_SUBMISSION:SUBMISSION (1)    [100%] 1 of 1`
* `[3c/8c7ba4] process > with_submission:RUN_SUBMISSION:GET_WAIT_TIME (1) [100%] 1 of 1`
* `[13/85f6f3] process > with_submission:RUN_SUBMISSION:WAIT (1)          [  0%] 0 of 1`
* `[-        ] process > with_submission:RUN_SUBMISSION:UPDATE_SUBMISSION -`
* `USING CONDA`

## Modifying the default wait time

❗ The default wait time between initial submission and updating the submitted samples is three minutes or 180 seconds per sample. To override this default calculation, you can modify the `submission_wait_time` parameter within your config or through the command line (in terms of seconds):

* `nextflow run main.nf -profile <test/standard>,<conda/docker/singularity>,<virus/bacteria> --submission_wait_time 360`
Outputs will be generated in the `nf_test_results` folder (if running the test parameter set) unless otherwise specified in your `standard_params.config` file as `output_dir` param. Or you can specify `--output_dir` on the command line.

## Running Bakta for Annotation

If you would like to run bakta for annotation, you must specify the following additional parameters:

|Param	|Description	|Input Required|
|---|---|---|
|--bakta_db_path|	|Path to Bakta reference database	|Yes (path as string)|
|--bakta_db_type	|Type of database to use for annotation	|Yes (full/light as bool)|
|--download_bakta_db	|Toggle to download Bakta database	|Yes (true/false as bool)|
|--run_bakta	|Toggle for running Bakta annotation	|Yes (true/false as bool)|
* `nextflow run main.nf -profile <test/standard>,<conda/docker/singularity>,bacteria --run_bakta true --bakta_db_path <path to bakta db>, --bakta_db_type <full/light>, --download_bakta_db <true/false>`

## Running with a Custom Entrypoint

Various entrypoint configurations can be provided to run particular sections the the workflow.

The following command can be used to run the validate params process within the utility sub-workflow:

* `nextflow run main.nf -profile test,conda,virus -entry only_validate_params`
The following command can be used to specify a which annotation process to run:

* `nextflow run main.nf -profile test,conda,virus -entry <only_liftoff/only_repeatmasker_liftoff/only_vadr/only_bakta>` 
The following command can be used to run the submission sub-workflow:

* `nextflow run main.nf -profile test,conda,virus -entry <only_submission/only_initial_submission/only_update_submission>`
❗ If you are using the `only_submission` or `only_initial_submission` entrypoint, you must define the paths for the following parameters: `submission_only_meta`, `submission_only_fasta`, and `submission_only_gff`. To find more information on configuring the submission entrypoint, refer to the Required Files for Submission Entrypoint section.

## Running with the Variola Dataset

The current implementation of the pipeline runs with MPOX virus sequences. The following command can be used to change this to Variola sequences:

* `nextflow run main.nf -profile test,docker,virus --variola true --submisstion_wait_time 30 --resume`