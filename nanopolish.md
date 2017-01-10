#First nanopolish run

      1. Do not use poretools, Fast5 files must be converted to fasta with nanopolish extract.
      2. The folder with the fast5 files must be in the working directory.
      
##Prepare the reads

**Convert fast5 to fasta**

      \*For this run we'll only use 2D reads from the pass folder of the 2nd nanopore run (30 hours) due to memory restrains\*
      
WD: /home/parmelia/Nanopolish/
  
      nanopolish extract --type 2D /pass/ > reads.fa
  
**Index the draft genome**
  
      mv contigs.fa draft.fa
      bwa index draft.fa
  
**Align the reads in base space**
  
      bwa mem -x ont2d -t 8 draft.fa reads.fa | samtools view -Sb - | samtools sort -o - reads.sorted.bam
      samtools index reads.sorted.bam
      
**Copy the nanopolish model files into the working directory**

      cp /Programas/nanopolish/etc/r9-models/* .
