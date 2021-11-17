GRADING EXERCISE 
================

Make a pipeline that does the following:

- aligns reads in fastq files (there are two files from a ChIP-seq experiment) in the folder `testdata <https://github.com/biocorecrg/SIB_course_nextflow_Nov_2021/tree/main/testdata/>`__ using Bowtie (as in the course exercises);
- runs fastqc for fastq files and the files produced as the result of the read alignment to the chr19 (file chr19.fasta.gz);
- calls ChIP-seq peaks for the produced alignments using the MACS2 software (`here is the official container <https://hub.docker.com/r/fooliu/macs2>`__).

(Optional) Make a more complex pipeline that does all the above plus:

- allows optional (choose as a parameter) read alignment using Bowtie and Bowtie2 or/and BWA;
- compares the number of called peaks for different alignment programs;
- compares required computational resources (RAM) and execution time for running the pipeline (all or/and only alignment) for different alignment programs.
