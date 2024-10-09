+++
title = 'nf-core/smRNAseq'
+++
https://nf-co.re/smrnaseq/2.2.1/

#### nf-core/smRNAseq

Pipeline summary
1. Raw read QC (FastQC)
2. Adapter trimming (Trim Galore!)
	1. Insert Size calculation
	2. Collapse reads (seqcluster)
3. Contamination filtering (Bowtie2)
4. Alignment against miRBase mature miRNA (Bowtie1)
5. Alignment against miRBase hairpin
	1. Unaligned reads from step 3 (Bowtie1)
	2. Collapsed reads from step 2.2 (Bowtie1)
6. Post-alignment processing of miRBase hairpin
	1. Basic statistics from step 3 and step 4.1 (SAMtools)
	2. Analysis on miRBase, or MirGeneDB hairpin counts (edgeR)
		- TMM normalization and a table of top expression hairpin
		- MDS plot clustering samples
		- Heatmap of sample similarities
	3. miRNA and isomiR annotation from step 4.1 (mirtop)
7. Alignment against host reference genome (Bowtie1)
	1. Post-alignment processing of alignment against host reference genome (SAMtools)
8. Novel miRNAs and known miRNAs discovery (MiRDeep2)
	1. Mapping against reference genome with the mapper module
	2. Known and novel miRNA discovery with the mirdeep2 module
9. miRNA quality control (mirtrace)
10. Present QC for raw read, alignment, and expression results (MultiQC)

# running nf-core/sRNAseq
navigate to smallRNA run directory
```bash
cd ~/workshop/smallRNA
ls -ls
tree
```
You can see the same directory structure for the pipeline. This was run with a 'nextflow run' kickoff shell script, and the samplesheet aas shown.

 samplesheet
```bash
cat samplesheet.csv
```

![](samplesheet.csv)

nextflow run script
```
cat nf-smRNAseq.sh
```
![](smallRNA_runscript.png)

These were the same sample as run for the mRNA experiment.

```
- 16 Samples sequenced with an MGI400 sequencer at SAGC using the Tecan Universal RNA-seq library protocol.
- 2 different cancer cell lines (human)
- treatment vs control
- 4 replicates for each
```
We will go over how this was set up.


![](smRNAseq_log.png)

small RNA multiqc
[link to small RNA multiqc](../workshop-small-RNAseq_multiqc_report.html)


# Downloading the data to you local PC
working on an hpc environment has some drawbacks, one being that viewing images and html files isn't as straightforward as it is on your pc. So that we can view the outputs together, this next step is to download the data from the nectar cloud to you pc 

to promote be good practice, we will first compress the whole directory using `tar` and run an md5sum check, to show that the download hasn't corrupted the data.

```bash
cd ~/workshop
tar -cvf smallRNA.tar smallRNA

md5sum smallRNA.tar > smallRNA.md5sum
md5sum smallRNA.tar
```
the md5sum value should print to you screen, you could also save it as a txt file to view later 
```
bee6e0161d40eb934c2ad0b4c2db1898  smallRNA.tar
```
you can do the same on you downloaded copy, to make sure it's exactly the same data.

### downloading with scp
we will be using ***scp*** (ssh copy)

You will need the same information as what you used to log in with `ssh`

|   |            |                        |
|---|------------|------------------------|
| 1 | username   | **workshop**           |
| 2 | password   | **Sagc_2024**          |
| 3 | IP address | ***given on the day*** |

```bash
# open a local shell terminal, make a directory and move to it
mkdir -p  ~/workshop_RNAseq/smallRNA 
cd ~/workshop_RNAseq/smallRNA
```

#example only, use your own IP address
IP=[yourIPaddress]
```bash
scp -r workshop@${IP}:/home/workshop/workshop/smallRNA.tar .

#eg. scp -r workshop@${IP}:/home/workshop/workshop/smallRNA . 
```
- this file is 617M, it should take a minute or less to downlaod

now on your local environment, you can run the same md5sum command

```bash
md5sum smallRNA.tar
```
the value should be exactly the same, showing the files are exact to the bit.

you can now uncompress the directory
```bash
tar -xvf smallRNA.tar
```

# Results

We will work through the multiqc
[link to small RNA multiqc](../workshop-small-RNAseq_multiqc_report.html)

And navigate through the smRNA outputs from the downloaded local copy.


