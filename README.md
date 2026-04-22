# The landscape of polyA tail length changes linked to PAIP1 regulation, based on nanopore cDNA sequencing (In collaboration with Maria Hondele lab) - Analysis and Pipelines

This repository contains the computational workflows and downstream analysis notebooks related to the polyA tail length changes linked to PAIP1 perturbations, conducted by the lab of Prof. Maria Hondele.

The repository is optimized for running BOTH the workflows and analysis in jupyter notebook on HPC cluster.

On sciCORE HPC, running jupyter notebook on a computational node is nicely enabled by [OnDemand service](https://docs.scicore.unibas.ch/HPC%20Cluster/interactivecomputing/#open-ondemand-ood).

We utilize a hybrid approach: [Nextflow](https://docs.seqera.io/nextflow/) for robust, scalable data processing on HPC clusters (sciCORE), and **Jupyter Notebooks** for interactive downstream analysis and visualization.

# Current state
Currently, we're analyzing the cDNA nanopore sequencing data from human PAIP1 knockdown and overexpression experiments in Hela cell lines and from Drosophila embryogenesis experiments.

## Repository Structure

```text
.
├── ONT_seq_PAIP1.current.ipynb      # a Jupyter notebook dedicated to the project, includes analysis and workflow configuration
├── ONT_seq_PAIP1.template.env       # Template for required environment variables/paths
```

## Quick Start & Setup

To ensure strict reproducibility and security, this project uses `.env` files to manage all absolute paths (data directories, genome annotations, etc.). **Do not hardcode paths into the Python or Snakemake files.**

### 1. Clone the Repository
Clone this repository into your local user space (`$HOME`):
```bash
git clone https://github.com/zavolanlab/TemperatureDependentCelegansAging_Spang.git
cd TemperatureDependentCelegansAging_Spang
```

### 2. Configure Environment Paths
You must map the project to your local HPC paths. 
**First**, copy the template, rename it, and fill in your absolute paths, for example like that:
  ```bash
  cp TemperatureDependentCelegansAging_Spang.template.env TemperatureDependentCelegansAging_Spang.scicore.env
  # Open .env and edit the "Base Directories" section to match your system
  ```
* **Recommended if you are a Zavolan group member on sciCORE:** move the `TemperatureDependentCelegansAging_Spang.scicore.env` to Project GROUP folder and symlink into your local repository directory:
  ```bash
  ln -s <a file with specified sciCORE paths> TemperatureDependentCelegansAging_Spang.scicore.env
  ```
This way `TemperatureDependentCelegansAging_Spang.scicore.env` will be automatically accessible by group members but will not be tracked by git.
*(Note: `*.env` files are ignored by git to protect private cluster paths, except the `TemperatureDependentCelegansAging_Spang.template.env` file).*
**(`TemperatureDependentCelegansAging_Spang.scicore.env` does exist in the GROUP folder of the Project on Scicore. Look for README there.)

### 3. Install the conda environment with zavolab_pyutils
Analysis in the notebook is largely based on the functions from [zavolab_pyutils](https://github.com/zavolanlab/zavolab_pyutils/tree/dev) repository.
Follow the instruction from that repo "Developer Setup from source, with conda environment".
Use the created conda environment "zavolab_pyutils" to execute the Jupyter Notebook.

### 4. Essential for developpers! Install nbstripout
When in the `TemperatureDependentCelegansAging_Spang` directory, run:
```bash
nbstripout --install
```
This will automatically hide the output of cells in juputer notebooks when pushed to github! Otherwise there is a risk of exposing your HPC cluster paths to public.

### 5. Use the juputer notebook to configure the workflow and input table preparation
Configuration of the workflows (i.e. creation of input .tsv with sample specification and .yaml config is done **inside** the [jupyter notebook](TemperatureDependentCelegansAging_Spang.current.ipynb))

### 6. Executing the Workflows

The heavy lifting is divided into (currently, three) separate Snakemake workflows located in the `WF/` directory.

`Bash` commands are also prepared inside the jupyter notebook. They should be further copied into command line and executed.

**On an HPC cluster like sciCORE**, workflows should be executed on a **login** node. Snakemake further automatically submits jobs to computational nodes.

### 7. Downstream Analysis
Once the Snakemake workflows are complete, all results are routed to the shared group directories defined in your `.env` file. 

Use respective sections of the Jupyter Notebook to analyze the outputs. 

The notebook automatically loads your `.env` paths using `python-dotenv`, allowing it to dynamically locate all workflow results, figures, and metadata regardless of where you cloned this repository.
