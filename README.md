## CapEng2: Capsid NGS data processing

The current "pipeline" includes a series of standalone scripts, each performs a specific task.

### count.r

This is a working horse script that counts the frequency of oligos in a pair of ***.fastq*** files. It takes a yaml file of metadata as input and generates a 2-column tab-delimited text files of unique oligo sequences in column 1 and read counts in column 2.

```
## Example
> spack load r@4.4.1
> cd /HPC/InternalData/analysis/pipeline/CapEng2/source
> Rscript count.r example/config_C974mini_run1.yaml
```

### jount.r

This script joins the oligo read counts of individual samples using their read count tables. It takes a yaml file of metadata as input, which inlcudes the location of output files from the ***count.r*** script 

```
## Example
> spack load r@4.4.1
> cd /HPC/InternalData/analysis/pipeline/CapEng2/source
> Rscript joint.r example/config_batch_C974mini.yaml
```

Files in the output directory:

  - ***total_oligo_count.txt***: include a single number of the total unique oligos from all samples in the batch
  - ***no_insert.txt***: 2-column tab-delimited text of sample names and numbers of reads without insert
  - ***joint_oligo_count.txt***: tab-delimited text file with column names, and columns of oligo sequences, total read counts per oligo of all samples combined, and individual samples from the 3rd column 
  - ***joint_inframe_count.txt***: tab-delimited text file with column names, and columns of in-frame oligo sequences, corresponding peptide sequences, total read counts per oligo of all samples combined, and individual samples from the 3rd column 
  - ***combined_oligo_count.txt***: 2-column tab-delimited text file of unique oligo sequences and corresponding of total read counts per oligo, all samples combined
  - ***filter_stat.xlsx***: Summary stats of total read, oligo, and peptide counts through a stepwise filtering procedure

### digest.r/digest.Rmd

Extra script and Rmarkdown template to summarize oligo and peptide sequences in the joint read count table, such as oligo length and base frequencies at positions in oligos and peptides.

```
## Example
> /HPC/InternalData/analysis/pipeline/CapEng2/source/extra
> sh ex_digest.sh ## details of an example run given in this file
```

Output files written to ***/HPC/InternalData/analysis/pipeline/CapEng2/output/20250307_C974mini/batch/result/digest***:

- ***index.html***: a summary report of the analysis
- stats tables in the report
- plots in the report

### nonsense.r/nonsense.Rmd

Extra script and Rmarkdown template to have a deeper look at the frequency and position of stop codons, or codons of other amino acids, such as Cystein

```
## Example
> /HPC/InternalData/analysis/pipeline/CapEng2/source/extra
> sh ex_nonsense.sh ## details of an example run given in this file
```

Output files written to ***/HPC/InternalData/analysis/pipeline/CapEng2/output/20250307_C974mini/batch/result/nonsense*** and ***/HPC/InternalData/analysis/pipeline/CapEng2/output/20250307_C974mini/batch/result/cystein***:

- ***index.html***: a summary report of the analysis
- ***nonsense_stats.rds***: summary stats of the nonsense codons

### filter.r

Extra script filter oligos to remove those including unexpected codons, such as stop codon or codon of cysteine. 

```
## Example
> /HPC/InternalData/analysis/pipeline/CapEng2/source/extra
> sh ex_filter.sh ## details of an example run given in this file
```

Output files written to ***/HPC/InternalData/analysis/pipeline/CapEng2/output/20250307_C974mini/batch/result/filter***:

- ***joint_inframe_count.txt***: tab-delimited text file with same columns as input files, after rows/peptides were filtered.
- ***filtered_read_per_peptide.rds***: a 2-column data.frame of peptides grouped by number of reads per peptides (column 1) and the number of peptides in each group (column 2) 

### freq.r

Extra script to summarize oligo and peptide frequency

```
## Example
> spack load r@4.4.1
> cd /HPC/InternalData/analysis/pipeline/CapEng2/source/extra
> Rscript freq.r # Parameters hard-coded in script
```





