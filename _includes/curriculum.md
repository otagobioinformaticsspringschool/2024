### Bioinformatic topics

#### DNA variant calling from next generation sequence data

- Project organisation
- Assessing read quality
- Trimming and filtering
- Variant calling workflow
- Automating a variant calling workflow

#### Genome assembly

- Genome assembly with long and short reads
- Draft assembly
- Assembly QC

#### Nanopore

- Base calling
- Quality control
- Variant calling

### Computational Topics

These topics will be integrated into the bioinformatics workshops and built upon across the week with the goal of demostrating a best practices workflow approach to Bioinformatics analysis.

#### Introduction to the Unix command line (Bash)

Many bioinformatic programs will only operate in a Unix command line environment, as such we need to provide an introduction to working in this environment which will cover:

- Navigating around the filesystem (files/directories/folders)
- Running command line based programs
- Creating scripts to automate workflows

#### Introduction to working in a high performance computing environment

We'll be making use of the New Zealand eScience Infrastructure (NeSI), which is the national provider of high performance computing for researchers, to run our analysis and as part of this we'll cover:

- What/who is NeSI
- Logging onto a remote computer
- Transfer of data to/from a remote computer
- What is a job scheduler and how to use it
- How to request a project allocation on NeSI

#### Introduction to Version Control

Using automated version control systems such as git is an important aspect of computational research. We'll cover:

- Understand the benefits of an automated version control system.
- Understand the basics of how automated version control systems work.
- Going through the modify-add-commit cycle for one or more files.
- Comparing various versions of tracked files.

#### Introduction to Reproducible Pipelines

Often for bioinformatic analyses we have a multistep series of computations. A useful tool to manage these workflows along with keeping track of inputs and outputs, are workflow managers such as snakemake. For this we'll cover:

- The benefits of using Snakemake or other workflow languages,
- How to create a workflow to organize your computations, and
- How an HPC scheduler (such as Slurm) fits into your workflow
