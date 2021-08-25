remove_contig manual
===========
## <a name="quickusage"></a> Quick usage
```
usage: remove_contig [-h] -gd FILE -r FILE [--minlen INT] [-gc FLOAT] [-depth FLOAT] genome

name:
    remove_contig.py -- Remove remove sequence

attention:
    remove_contig.py genome.fasta --gc_depth length_gc.tsv --rna rna.gff>genome_new.fasta
version: 1.0.1
contact:  Xingguo Zhang <invicoun@foxmail.com>       

positional arguments:
  genome                Input genome file(fasta, fastq)

optional arguments:
  -h, --help            show this help message and exit
  -gd FILE, --gc_depth FILE
                        Input statistics files of genome length, GC content and sequencing depth.
  -r FILE, --rna FILE   Input genomic RNA prediction result file(gff).
  --minlen INT          Input the value to remove the minimum length, default=20000.
  -gc FLOAT, --diff_gc FLOAT
                        Input removed GC difference ratio, default=0.5.
  -depth FLOAT, --diff_depth FLOAT
                        Input removal depth difference ratio, default=0.5.
 ```
===========
## <a name="quickusage"></a>example
```
#Step 1: statistics GC content, contig length and depth
minimap2 --secondary=no -t 4 -ax map-pb genome.fasta reads.fastq | samtools view --threads 4 -bS | samtools sort --threads 4 -m 4G -o genome.sort.bam 
#or
bwa index genome.fasta
bwa mem -t 4 genome.fasta r1.fastq r2.fastq | samtools view --threads 4 -bS | samtools sort --threads 4 -m 10G -o genome.sort.bam 
samtools depth -aa genome.sort.bam >genome.depth

stat_length_gc.py -g genome.fasta -d genome.depth -n  genome #Generate genome.length_gc.tsv file
#Step 2: Predict rRNA
barrnap --kingdom arc --threads 4 --quiet  genome.fasta > genome.rRNA.gff
or 
rnammer -S bac -m lsu,ssu,tsu -gff genome.rRNA.gff genome.fasta
#Step 3: Remove contamination sequence
remove_contig  genome.fasta --gc_depth genome.length_gc.tsv --rna genome.rRNA.gff --minlen 20000 --diff_gc 0.6 --diff_depth 0.6 >genome_new.fasta
#Step 4: Confirm to remove data.
#Align the removed contig with ncbi, delete mitochondria, chloroplasts or prokaryotes, and recover other sequences into the genome.
#Step 5: Confirm the genome
#The genome after the above treatment is compared with NCBI, and the CG content and depth are calculated in addition to remove possible contaminated sequences.


```
Result display
------------
<p align="center">
  <img src="https://github.com/zxgsy520/remove_contig/edit/main/docs/gc_depth.png" width = "500" height = "500" alt="GC-depth distribution map"/>
</p>

