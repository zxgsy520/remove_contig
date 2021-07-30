# remove_contig
==============
### Version: 2.8.3
Remove possible contaminating sequences in the genome
Manuals
-------
Manuals
-------

- [Installation instructions](docs/INSTALL.md)
- [Usage](docs/USAGE.md)
- [FAQ](docs/FAQ.md)
- 


./remove_contig -h
usage: remove_contig [-h] -gd FILE -r FILE [--minlen INT] [-gc FLOAT] [-depth FLOAT] genome

name:
    remove_contig.py -- Remove remove sequence

attention:
    remove_contig.py genome.fasta --gc_depth length_gc.tsv --rna rna.gff>genome_new.fasta

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
