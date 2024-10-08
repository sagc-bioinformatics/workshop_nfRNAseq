+++
title = 'nextflow introduction'
+++
Nextflow is a workflow manager, enabling multiple processes from different software to be run coherently \
essentially creating a **pipeline**, where the output of one process is the input to the next 
- *this phrase will be used alot in the workshop it's worth defining it early.* 

Nextflow is very a powerful tool for software development
- for a deeper dive into nextflow, particularly how to write nextflow pipelines, feel free to refer to SAGC's previous workshop notes \
[SAGC nextflow vs snakemake workshop](https://sagc-bioinformatics.github.io/nextflow-vs-snakemake-2024/)

In this workshop we will using nf-core pipelines, which have been built with nextflow, to analyse RNAseq data.

But that all starts with getting nextflow running, so this section is designed to give you a quick view of what's required to run nextflow.

#### log into nectar instance with provided username and IP address
```bash
# example only
ssh username@172.243.264.45
```

navigate to worshop materials
```bash
cd ~/workshop/
ls -l
```

#### nextflow commands
which version of nextflow are we using?
```bash
nextflow -version
```
this command will also test wether nextflow is installed, if it isn't see installation instructions

      N E X T F L O W
      version 24.04.4 build 5917
      created 01-08-2024 07:05 UTC 
      cite doi:10.1038/nbt.3820
      http://nextflow.io

Other Dependencies for running nextflow include singularity, which also uses GO
- let's make sure they are installed as well, to help demonstrate what is required to get nextflow running

- in general, nf-core pipelines have very few dependencies, with most software packaged in containers such as singularity, this makes them very *portable* , meaning they can be run on different compute environments


```bash
singularity --version
```
The workshop is running on a Nectar Research cloud instance, where these already should be installed. If not, or if you are working from a different environment, please refer to the second half of the 'Setup' instructions, giving instructions of how to install these software.
	
if you wake a look in your ~/.bashrc file you should see the following lines. The *PATH* to these tools will be set when starting up the *bash* instance

Have a look at your .bashrc, by opening in a text editor such an &nano*, or using *tail*
```bash
tail  ~/.bashrc
# nano ~/bashrc
# to quit this screen, press the 'control' and 'x' 
```
you should see these lines at the bottom, or similar

	export PATH=~/bin:${PATH}
	export NXF_SINGULARITY_CACHEDIR=~/SingularityCacheDir
	export GOPATH=${HOME}/go
	export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin
	export PATH=${PATH}:~/bin/miniconda3/bin


#### Now let's take a moment to get familiar with the nextflow commands
```bash
nextflow -h
```
particularly the 'nextflow run' command
```bash
nextflow run -help
```

#### Singularity cachedir
This is where downloaded singularity images are stored, so that an unchanged imaged doesn't have to be downloaded again when the same pipeline is run

- setting the nextflow singularity cachedir
```bash
mkdir -p SingularityCacheDir

echo 'export NXF_SINGULARITY_CACHEDIR=~/SingularityCacheDir' >> ~/.bashrc && \
    source ~/.bashrc

echo $NXF_SINGULARITY_CACHEDIR
```
#### running nextflow
-lets run a small nextflow pipeline

this is the one from the previous workshop
```bash
#firstly clone the example workflow developed in the previous workshop from github
mkdir -p ~/workshop/nextflow_example
cd ~/workshop/nextflow_example

git clone https://github.com/sagc-bioinformatics/nextflow-example-workflow-2024.git
```
now use the 'nextflow run' command
```
nextflow run main.nf
```
The processes should print to your screen as standard output

![log](/nextflow/nextflowexampllog.png)

- let's check out how the run went
```bash
# you can use the 'nextflow log' command
nextflow log

# the .nextflow.log is output in the run directory, it's a hidden file so you will have to use ls -a to see it
ls -lat
```
you should see a few new files/directories output from that nextflow process
1. .nextflow.log
2. /output
3. /work

- take a moment to navigate through those outputs
```bash
tail .nextflow.log
ls output/*
```
#### work directory
An important point to make about the *work* directory. This is where outputs from finished processes are cached. This is a very important feature of workflow managers than incomplete runs can be resumed.
```bash
ls work/*/*
```
***try running the same process again with the -resume flag to see how quickly it runs in comparison***

- notice the *cached* processes


