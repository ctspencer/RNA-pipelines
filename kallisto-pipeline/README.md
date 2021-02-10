# kallisto-pipeline

Jack Humphrey 2019

A snakemake workflow to:

* generate multiple Kallisto indexes from different transcript lists
*  run Kallisto to estimate transcript abundance with bootstraps
* run Sleuth to call differential isoform usage between groups

In addition this will contain projects using the results of this pipeline:

1. Estimate novel isoforms from AD brain isoseq reference

2. Estimate intron retention in human brains using KeepMeAround (Pimentel, 2019, unpublished)


## dependencies

snakemake
kallisto v0.45.0 - already installed on chimera

## testing

1. clone repo
2. load dependencies
3. run `snakemake -s Snakefile --configfile example/config.yaml -pr`

## usage

samples.tsv - list of sample IDs

config.yaml - see `example/config.yaml`

### assumed use case
you have a set of FASTQ files with the forms <sample>.R1.fastq.gz and <sample>.R2.fastq.gz for each <sample>. `samples.tsv` is a single column text file with a column named `sample` with a set of sample IDs.

you also have one or more transcript sets (in FASTA format) that you want to quantify for your samples. Specify these in the `config.yaml`.

# Summary of rotation work 

Collin Spencer 2021

## Sample processing w/ kallisto and output
- All short read samples were aligned using the kallisto pipeline to 4 reference fasta (SQANTI, pre-SQANTI, gencode_v32, and gencode_v32_polyA) 
  - output dir: /sc/arion/projects/als-omics/microglia_isoseq/collin/RNA-pipelines/kallisto-pipeline/pipeline_results
- The summary results of the pipeline are contained in {fasta}counts.Rdata and {fasta}matrix.RData
- The counts.RData file for SQANTI was used for subsequent analysis
- Where samples were pulled from, summary table of results, and visualization are available in the jupyter notebook:
  - /als-omics/microglia_isoseq/collin/RNA-pipelines/kallisto-pipeline/sample_grabber_viz.ipynb

## Analysis file (short_long_correlation jupyter notebook)
- location: /als-omics/microglia_isoseq/collin/RNA-pipelines/kallisto-pipeline/short_long_correlation.ipynb
- This notebook contains all of the filtering steps and visualization of paired short and long read samples
- Filtering steps:
1. Remove samples that are not paired
2. Remove transcripts that are not found in both short and long read counts 
3. convert transcript abundance to TPM
4. collapse transcript TPM to gene TPM 
5. Convert PacBio ID's to ENSEMBL gene ID's
6. Remove any duplicate genes (for DESEQ) 
-Performed spearman correlations between matched samples with following filters: base (TPM > 0.5), low expression (0.5-10 TPM), medium expression (11-1000 TPM), high expression ( > 1000 TPM)
-Plotted highly expressed reads as scatter plot w/ regression line 
- Did some formatting of count files specific for DESEQ2 input (saved as short/long_read_DESEQ.csv) 

## DESEQ2 output and graphs
- ran DESEQ2 between SVZ and MFG regions
- results are output as DESEQ_results_{long/short}_read.csv
- saved summary table, PCA, and volcano plots to /als-omics/microglia_isoseq/collin/RNA-pipelines/kallisto-pipeline/graphs


