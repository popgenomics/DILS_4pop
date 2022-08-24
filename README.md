- [dependencies](#dependencies)
- [get the pipeline](#get-the-pipeline)
- [Snakefile](#snakefile)
- [example](#example)

Pipeline using snakemake to perform demographic inferences in 4-population models. Three topologies are possible as well as different migration relationships making a maximum of 768 comparable models, depending on the user's specifications.  
  
# get the pipeline  
```
git clone https://github.com/popgenomics/DILS_4pop
```  

# dependencies  
Like many modern bioinformatics tools, DILS relies heavily on external libraries and programs developed by real computer scientists.  Please install them before running DILS:   
 
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
  
msnsam has to be compiled as follows:  
```
cd bin/msnsam_src
./clms
ln -s $PWD/msnsam ../msnsam
```  
  
  
The msms binary is located in:
> bin/msms3.2rc-b163.jar
  
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
  
# config file  
DILS requires a second input file (in addition to the fasta) to be executed. The second file, in yaml format, contains various information crucial to the analysis.  
Here is an example with config.yaml    
```
inputFile: /home/croux/Documents/zoe/data/RNAseq_nucl.fasta # full pathway to the input file (fasta format)
datapath: /home/croux/Documents/zoe/DILS/4pop_v1 # full pathway to the directory where the analysis will be performed, i.e, where the results will be stored
region: coding
nameA: E1
nameB: W3
nameC: W1
nameD: W2
maxN: 0.5
nMin: 8
Lmin: 200
mu: 0.00000000731
rec: 0.00000000731
N_bound_min: 0  # effective population sizes
N_bound_max: 10
T_bound_min: 1 # time of demographic events
T_bound_max: 10
M_bound_min: 0.4 # migration rates in 4.N.m
M_bound_max: 40
shape_bound_min: 1 # shape parameters of the Beta distributions
shape_bound_max: 20
topologies: topo2 #topo1 or topo2 or topo3 or topo1,topo2 or topo1,topo3 or topo2,topo3 or topo1,topo2,topo3
migAB: 0,1 #0 or 1 or 0,1
migAC: 0
migAD: 0
migBC: 0,1
migBD: 0
migCD: 0
```

# example  
Short example (dry run).  
```
cd DILS_4pop/example/
mkdir analysis
cd analysis
snakemake -n -p -s ../../bin/Snakefile
```
  
Full run
