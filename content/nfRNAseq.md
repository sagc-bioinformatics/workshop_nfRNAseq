+++
title = 'nfRNAseq'
+++
![nf-core/RNAseq](..//nfRNAseq/nfRNAseq_pipeline.png)

https://nf-co.re/rnaseq/3.14.0

nf-core/RNAseq the most popular nf-core pipeline. 

```bash
nf-core list -s stars
```

let's use nf-core tools to build a command to run the nf-core/RNAseq pipeline
```bash
mkdir -p  ~/workshop/nfRNAseq
cd ~/workshop/nfRNAseq 

nf-core launch -h
```
Launch a pipeline using a web GUI or command line prompts.                                         
 Uses the pipeline schema file to collect inputs for all available pipeline parameters. 
 When finished, saves a file with the selected parameters which can be passed to Nextflow using the 
 -params-file option.    

```bash
nf-core launch nf-core/rnaseq
```

```
- use the arrow keys to select the following parameters
	- select release - version '3.14.0' 
```
selecting the release will download the pipeline, effectively running 'nextflow pull
you can see the key nextflow files are pulled to the ~/.nextflow directory, including the key executables and configs (nextflow.config, main.nf)\
(exit with control+c if needed)

```bash
ls /home/workshop/.nextflow/assets/nf-core/rnaseq/
```
This is one of many ways to pull a nf-core pipeline. 

you can then select GUI or command line, with the GUI giving a URL to access via a browser by copying and pasting.

![](GUI.png)

we can continue with adding variables with nf-core launch
```
	-name test
	-profile singularity \
	-resume
	--input nfSampleSheet.csv \
	--outdir Out \
	--fasta Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa \
	--gtf Homo_sapiens.GRCh38.111.gtf \
```
this will give you a nf-core launch command with those variable
```
nf-core launch --id 1728482936_6064841c2138
```
- alternatively you can use a nextflow run command

- no need to run this command, in the interest of time (and the lack of disk space on your nectar instance), I've pre-prepared the outputs for this run

navigate to run directory to see the nextflow run command
```bash
cd ~/workshop/RNAseq
ls
cat nf_rnaseq.sh
```
![](runcommand.png)

```bash
cat nfSampleSheet.csv
```
![](samplesheet.png)

#### dataset
The dataset used throughout this workshop is as follow:

```
- 16 Samples sequenced with an MGI400 sequencer at SAGC using the Tecan Universal RNA-seq library protocol.
- 2 different cancer cell lines (human)
- treatment vs control
- 4 replicates for each
```
- most of this information is reflected in the samplesheet and run command

#### Run Summary

list results directory. All nf-core outputs have a consistent structure of the outputs
```bash
tree /home/workshop/workshop/nfRNAseq/outs
```

[link to execution timeline](../execution_timeline_2024-10-05_16-02-39.html)

[link to execution report](../execution_report_2024-10-05_16-02-39.html)

# Genomics Files Background
Quick background on genomic file formats to help describe the inputs and outputs of an NGS experiment.
Knowing what these files are isn't only important in finding which files to use for a pipeline, but a key foundation of genomic bioinformatics
Being able to use and manipulate each file open's up many opportunities, and is often required for troubleshooting                           
Many are plain text files, this means they can be manipulated with basic text editing.

##### .fastq /.fastq.gz

![](../fastqfile.png)

#### .fasta
the genome.fa file is a plain text representation of the genome sequence. This is the 'reference' to which the sequencing files (.fastq) are alligned.

```bash
head ./smallRNA/outs/mirtrace/[:]/mirtrace/qc_passed_reads.rnatype_unknown.collapsed/Acontrol1.fastp.fasta
```
#### .gtf
genes.gtf ; a genomic interval file referencing the genome. This file depicts the genes/ transcripts. It's a required for counting where reads are mapped to, and often has alot more annotation data in regards to the gene/transcript.

#### .bed
![](../bedfile.png)


#### bam
![](../bamfile.png)


# Walkthough of outputs nf-core/RNAseq
the multiqc summary is a real strength of nf-core pipeline.
alot of the key analyses are captured

[link to multiqc report](../multiqc_report.html)

```
Overview of the key processes run with nf-core/RNAseq.
- identifying steps to trouble shooting the success of a run.
- far more than just mapping (star) and counting (RSEM)
- demonstrating the need for a workflow manager
```
### Preprocessing
1. cat - Merge re-sequenced FastQ files
2. FastQC - Raw read QC

[link to fastqc.html](../Acontrol1_1_fastqc.html)

3. UMI-tools extract - UMI barcode extraction
4. TrimGalore - Adapter and quality trimming
5.  BBSplit - Removal of genome contaminants
6.  SortMeRNA - Removal of ribosomal RNA

### Alignment and quantification
1. STAR and Salmon - Fast spliced aware genome alignment and transcriptome quantification
2. STAR via RSEM - Alignment and quantification of expression levels
3. HISAT2 - Memory efficient splice aware alignment to a reference

### Alignment post-processing
1. SAMtools - Sort and index alignments
2. UMI-tools dedup - UMI-based deduplication
3. picard MarkDuplicates - Duplicate read marking

### Other steps
1. StringTie - Transcript assembly and quantification
2. BEDTools and bedGraphToBigWig - Create bigWig coverage files

### Quality control
1. RSeQC - Various RNA-seq QC metrics
2. Qualimap - Various RNA-seq QC metrics
3. dupRadar - Assessment of technical / biological read duplication
4. Preseq - Estimation of library complexity
5. featureCounts - Read counting relative to gene biotype
6. DESeq2 - PCA plot and sample pairwise distance heatmap and dendrogram
7 MultiQC - Present QC for raw reads, alignment, read counting and sample similiarity

### Pseudoalignment and quantification
1. Salmon - Wicked fast gene and isoform quantification relative to the transcriptome
2. Kallisto - Near-optimal probabilistic RNA-seq quantification
3. Workflow reporting and genomes
4. Reference genome files - Saving reference genome indices/files
5. Pipeline information - Report metrics generated during the workflow execution

[link to execution report](../execution_report_2024-10-05_16-02-39.html)
