# Quick Start

## Validate Environment
### (1) Check that you are in the directory where the TOSTADAS repository was installed by running `pwd`
Expected Output:
* `/path/to/working/directory/tostadas`
This is the default directory set in `nextflow.config` for the provided test input files.

### (2) Change the `submission_config` parameter within `nextflow.config` (if running with your own data) to the location of your personal submission config file. Note that we provide a virus and bacterial test config depending on the use case.
❗ You must have your personal submission configuration file set up before running the default parameters for the pipeline and/or if you plan on using sample submission at all. More information on running the submission sub-workflow can be found here: [More Information on Submission]()

## Pipeline Execution Examples
We describe a few use-cases of the pipeline below. For more information on input parameters, refer to the documentation found in the following pages:

- [Profile Options and Input Files]()
- [Parameters]()
❗ Note: For all use cases, the paths to the *required files* should be specified in the `nextflow.config` file or the `params.yaml` file.

### Basic Usage:
Before we dive into the more complex use cases of the pipeline, let's look at the most basic way the pipeline can be run:

* `nextflow run main.nf -profile <singularity/docker/conda> --virus`
- `-profile`:
 - This parameter is required to specify the run-time environment (singularity, docker or conda). Conda implementation is less stable, singularity or docker is recommended.
 - Optionally, you may specify test as an additional argument to this parameter. If test is not provided as an option, the pipeline will run using the configurations found in the nextflow.config file.
Example:

* `nextflow run main.nf -profile singularity,test --virus`
- --virus:
 - The pathogen type must be specified in order for the pipeline to run. Here virus is specified.
### Providing additional parameters through the command line:
Parameters can be overridden during runtime by providing various flags to the nextflow command.

### Example: Modifying the submission wait time

By default, the default parameters will trigger a wait time equal to number of samples, times 180 seconds. This default can be overridden by running the following command instead:

* `nextflow run main.nf -profile <singularity/docker/conda> --virus --submission_wait_time <integer value>` 
### Use Case 1: Running Annotation and Submission
### 1. Annotate viral assemblies and submit to GenBank and SRA

Required files: fasta files, metadata file

* `nextflow run main.nf -profile <singularity/docker/conda> --virus --genbank --sra --submission_wait_time 5`
Breakdown:

- `-profile`:
 - Compute environment is specified
- `--virus`:
 - Pathogen type is specified as `virus`
`--sra`:
 - `sra` is specified as the database for submission
- `--genbank`:
 - `genbank` is specified as the database for submission
- `--submission_wait_time`:
 - The `submission_wait_time` parameter is modified
❗ **Pre-requisite to submit to GenBank**: In order to submit to GenBank, the program `table2asn` must be executable in your local environment. Copy [table2asn](https://www.ncbi.nlm.nih.gov/genbank/table2asn/) to your `tostadas/bin` directory by running the following lines of code:

* `cd ./tostadas/bin/`
* `wget https://ftp.ncbi.nlm.nih.gov/asn1-converters/by_program/table2asn/linux64.table2asn.gz`
* `gunzip linux64.table2asn.gz`
* `mv linux64.table2asn table2asn`
### 2. Annotate bacterial assemblies and submit to GenBank and SRA

Required files: fasta files, metadata file, a reference database

(A) Download BAKTA database

* `nextflow run main.nf -profile <singularity/docker/conda> --bacteria --genbank --sra --submission_wait_time 5 --download_bakta_db true --bakta_db_type <light/full>`
(B) Provide path to existing BAKTA database

* `nextflow run main.nf -profile <singularity/docker/conda> --bacteria --genbank --sra --submission_wait_time 5 --bakta_db_path <path/to/bakta/db>`
Breakdown:

- `--bacteria`:
 - The pathogen type is specified as `bacteria`
- `--download_bakta_db <true/false>`:
 - This parameter must be specified as `true` or `false`
- `--bakta_db_type <light/full>`:
 - The BAKTA database type is defined in this parameter. `light` or `full` can be supplied as options for this parameter
- `--bakta_db_path /path/to/bakta/db`:
 - The path to the BAKTA database is defined
### Use Case 2: Running Submission only without Annotation
Notes: you can only submit raw files to SRA, not to Genbank.

**1. Submit fastqs to SRA** Required files: fastq files, metadata file

* `nextflow run main.nf -profile <test,standard>,<singularity,docker> --<virus,bacteria> --annotation false --sra --submission_wait_time 5`
- `--annotation`:
 - The boolean option `false` is specified for the annotation parameter