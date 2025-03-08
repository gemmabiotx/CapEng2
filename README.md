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
  - ***joint_oligo_count.txt***: tab-delimited text file, with column names, of oligo sequences, total read counts per oligo of all samples combined, and individual samples from the 3rd column 
  - ***joint_inframe_count.txt***: tab-delimited text file, with column names, of in-frame oligo sequences, corresponding peptide sequences, total read counts per oligo of all samples combined, and individual samples from the 3rd column 
  - ***combined_oligo_count.txt***: 2-column tab-delimited text file of unique oligo sequences and corresponding of total read counts per oligo, all samples combined
  - ***filter_stat.xlsx***: Summary stats of total read, oligo, and peptide counts through a stepwise filtering procedure

### filter.r

This script filter oligos to remove those including unexpected codons. 


