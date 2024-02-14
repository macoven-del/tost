# CDC User Guide
## Environment Setup
### (1) Clone the repository:
Clone the tostadas repository to your working environment by following the instructions found here: `cdc_configs_access`

### (2) Load the Nextflow module:
Initialize the nextflow module by running the following command:

* `ml nextflow`
### (3) Ensure that Nextflow is available by running:
* `nextflow -v`

Expected Output:

* `nextflow version <CURRENT VERSION>`
### (4) Run one of the following nextflow commands to execute the scripts with default parameters and the local run environment:
#### For Viral Reads
* `nextflow run main.nf -profile test,<singularity/docker/conda> --virus`
#### For Bacterial Reads
* `nextflow run main.nf -profile test,<singularity/docker/conda> --bacteria`
The outputs of the pipeline will appear in the `test_output` folder within the project directory. You can specify an output directory in the `config` file or by supplying a path to the `--output_dir` flag in your `nextflow run` command.