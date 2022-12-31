# 16S-rRNA-Analysis using mothur 

## Directory setup 
```
mkdir /home/Users/Mothur.Ubuntu_20/mothur/Data
```
```
mkdir /home/Users/Mothur.Ubuntu_20/mothur/Result
```
```
cd Data
```
## Download the data
```
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR815/007/ERR8157527/ERR8157527_1.fastq.gz
```
```
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR815/007/ERR8157527/ERR8157527_2.fastq.gz
```
```
wget https://mothur.s3.us-east-2.amazonaws.com/wiki/silva.bacteria.zip
```
```
wget https://mothur.s3.us-east-2.amazonaws.com/wiki/trainset9_032012.pds.zip
```
## Data unzipping
```
gunzip ERR8157527_1.fastq.gz
```
```
gunzip ERR8157527_2.fastq.gz
```
```
unzip trainset9_032012.pds.zip
```
```
unzip silva.bacteria.zip
```

## Set mothur Directories
```
./mothur
```
```
set.dir(input=/opt/mothur/Data, output=/opt/mothur/Result)
```
## make file for all reads

```
make.file(inputdir=/opt/mothur/Data, prefix=stability, type=fastq)
```
## merging reads to contigs
```
make.contigs(file=stability.files)
```
## statistical summary of your contigs
```
summary.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table)
```
## Screening of data 
```
screen.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table, maxambig=0, maxlength="602", maxhomop=8)
```
## Summary of our data 
```
summary.seqs(fasta=current, count=current)
```
## Removing Duplicates 
```
unique.seqs(fasta=stability.trim.contigs.good.fasta, count=stability.contigs.good.count_table)
```
## summary of our data 
```
summary.seqs(fasta=current, count=current)
```
## Alignment against Refrence
```
align.seqs(fasta=stability.trim.contigs.good.unique.fasta, reference=silva.bacteria.fasta)
```
## Summary of our data 
```
summary.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table)
```
## screening of our data 
```
screen.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table, start=6388, end=25316)
```
## Summary of our data 
```
summary.seqs(fasta=current, count=current)
```
## Filtering our Data 
```
filter.seqs(fasta=stability.trim.contigs.good.unique.good.align, vertical=T, trump=.)
```
## Removing Replicates
```
unique.seqs(fasta=stability.trim.contigs.good.unique.good.filter.fasta, count=stability.trim.contigs.good.good.count_table)
```
## summary of our data 
```
summary.seqs(fasta=current, count=current)
```
## Preclustering step
```
pre.cluster(fasta=stability.trim.contigs.good.unique.good.filter.unique.fasta, count=stability.trim.contigs.good.unique.good.filter.count_table, diffs=2)
```
## Chimera Removal Step
```
chimera.vsearch(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.count_table, dereplicate=t)
```
## Classify Seqsuence ##
```
classify.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table, reference=trainset9_032012.pds.fasta, taxonomy=trainset9_032012.pds.tax)
```
## Assessing Error rate
```
seq.error(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table, reference=HMP_MOCK.v35.fasta.1, aligned=F)
```
## Analysis Preparation
```
rename.file(fasta=current, count=current, taxonomy=current, prefix=final)
```
## OTU Clustering and shared file
```
dist.seqs(fasta=final.fasta, cutoff=0.03)
```
```
cluster(column=final.dist, count=final.count_table)
```
```
make.shared(list=final.opti_mcc.list, count=final.count_table, label=0.03)
```
## ASV 
```
make.shared(count=final.count_table)
```
