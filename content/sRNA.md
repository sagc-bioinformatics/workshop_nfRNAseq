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

#### running nf-core/sRNAseq
![](/sRNA/smRNAseq_log.png)
