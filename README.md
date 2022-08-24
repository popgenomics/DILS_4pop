- [dependencies](#dependencies)
- [get the pipeline](#get-the-pipeline)
- [Snakefile](#snakefile)
- [example](#example)

Pipeline using snakemake to perform demographic inferences in 4-population models. Three topologies are possible as well as different migration relationships making a maximum of 768 comparable models, depending on the user's specifications.  
  
# dependencies  
### R libraries
- abcrf (works with version: 1.9)  
- data.table (works with version: 1.14.2)  
- FactoMineR (works with version: 2.4)  
- tidyverse (works with version: 1.3.2)  
- ggpubr (works with version: 0.4.0)  
- viridis (works with version: 0.6.2)  
  
In a R terminal:  
```
for(library in c('abcrf', 'data.table', 'FactoMineR', 'ggpubr', 'viridis')){
	install.packages(library)
}
```
  
### Python libraries  
- numpy (works with version: 1.21.5)  
- biopython (works with version: 1.79)  
  
### others  
- pypy ([fast implementation of Python](https://www.pypy.org/), works with version: Python 3.6.9  [PyPy 7.3.1 with GCC 7.3.1 20180303 (Red Hat 7.3.1-5)])  
- snakemake ([installation](https://snakemake.readthedocs.io/en/stable/getting_started/installation.html), works with version: 6.15.1)  
- java (works with version: openjdk 11.0.16 2022-07-19)  
- msnsam  
- msms  
  
The msms binary is located in:
> bin/msms3.2rc-b163.jar
  
# get the pipeline  
```
git clone https://github.com/popgenomics/DILS_4pop
```

# Snakefile  
For the time being, all the specifications of the analysis must be indicated in the Snakefile. The variables to be filled in are:  
- binpath: path to binaries for ABC inferences  
- datapath: path to directory where inferences will be performed  
- input file: path to the fasta input file  
- region="coding" or "noncoding", depending on the nature of the data  
- mutation="nSNPs" or "mu", 'mu' if mutations within simulations are conditionned on the molecular clock) or 'nSNPs' (mutations are directly conditionned on the observed number of SNPs for each loci).  
- nameA, nameB, nameC and nameD: names of the four populations already present in the identifiers of the sequences of the fasta file.  
- maxN=0.5 # loci with a proportion of missing data (i.e, non A, T, C or G) above maxN are excluded.  
- nMin=8 # loci with less than nMin gametes are excluded.  
- Lmin=200 # loci with less than Lmin valid positions (without missing data, and in at least nMin copies) are excluded.  
- mu=0.00000000731 # mutation rate per position and per generation. Used only if mutation='mu'.  
- rec=0.00000000731 # recombination rate per position and per generation.  
- Nref=100000 # don't touch  

prior distributions in coalescent units:  
- N_bound_min=0  # effective population sizes  
- N_bound_max=10  
- T_bound_min=1 # time of demographic events  
- T_bound_max=10  
- M_bound_min=0.4 # migration rates in 4.N.m  
- M_bound_max=40  
- shape_bound_min=1 # shape parameters of the Beta distributions  
- shape_bound_max=20  

# example  
Short example (dry run).  
```
cd DILS_4pop/example/
mkdir analysis
cd analysis
snakemake -n -p -s ../../bin/Snakefile
```

