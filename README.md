## CapEng2: Capsid NGS data processing

The current "pipeline" includes a series of standalone scripts, each performs a specific task.

### count.r

This is a working horse script that counts the frequency of oligos in a pair of ***.fastq*** files. It takes a yaml file of metadata as input and generates a 

```
## Example
> spack load r@4.4.1^C
> cd /HPC/InternalData/analysis/pipeline/CapEng2/source
> Rscript count.r example/config_C974mini_run1.yaml
```
