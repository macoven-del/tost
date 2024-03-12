# Installation

## Environment Setup
### Dependencies:
- Nextflow v. 21.01.03 or newer
- Compute environment (docker, singularity or conda)
❗ Note: If you are a CDC user, please follow the set-up instructions found on this page: [CDC User Guide](https://github.com/CDCgov/tostadas/wiki/CDC-User-Guide)

### (1) Clone the repository to your local machine:
* `git clone https://github.com/CDCgov/tostadas.git`
❗ Note: If you have mamba or nextflow installed in your local environment, you may skip steps 2, 3 (mamba installation) and 6 (nextflow installation) accordingly.

### (2) Install mamba and add it to your PATH
2a. Install mamba

❗ Note: If you have mamba installed in your local environment, skip ahead to step 3 (Create and activate a conda environment)

`curl -L -O https://github.com/conda-forge/miniforge/releases/latest/download`
`Mambaforge-$(uname)-$(uname -m).sh`
`bash Mambaforge-$(uname)-$(uname -m).sh -b -p $HOME/mambaforge`
2b. Add mamba to PATH:

`export PATH="$HOME/mambaforge/bin:$PATH"`
### (3) Create and activate a conda environment
3a. Create an empty conda environment to install Nextflow into

`conda create --name tostadas`
3b. Activate the environment

`conda activate tostadas`
Verify which environment is active by running the following conda command: `conda env list`. The active environment will be denoted with an asterisk `*`

### (4) Install Nextflow using mamba and the bioconda Channel
#### Install Nextflow
`mamba install -c bioconda nextflow`

#### Ensure installation was successful by running nextflow with the `-v` or `-version` flag 
`nextflow -v`
Expected output:

`nextflow version <CURRENT VERSION>`
The exact version of Nextflow returned will differ from installation to installation. It is important that the command executes successfully, and a version number is returned.

### (5) Update the default submissions config file with your NCBI username and password, and run the following nextflow command to execute the scripts with default parameters and the local run environment:
`# update this config file (you don't have to use vim)`
`vim bin/config_files/default_config.yaml`
`# for virus reads`
`nextflow run main.nf -profile test,<singularity|docker|conda> --virus`

❗ Note: If you do not update the default_config.yaml file with your NCBI credentials, you will see the following command error: `Error:530 Login incorrect`.

The outputs of the pipeline will appear in the test_output folder within the project directory. You can specify an output directory in the config file or by supplying a path to the `--output_dir` flag in your `nextflow run` command.