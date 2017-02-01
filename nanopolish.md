#First nanopolish run

      1. Do not use poretools, Fast5 files must be converted to fasta with nanopolish extract.
      2. The folder with the fast5 files must be in the working directory.
      
##Prepare the reads

WD: /home/parmelia/Nanopolish/

**Convert fast5 to fasta**

   *For this run we'll only use 2D reads from the pass folder of the 2nd nanopore run (30 hours) due to memory restrains.*  

       nanopolish extract --type 2D /pass/ > reads.fa
  
**Index the draft genome**  
      
      mv contigs.fa draft.fa
      bwa index draft.fa
  
**Align the reads in base space**  
      
      bwa mem -x ont2d -t 8 draft.fa reads.fa | samtools view -Sb - | samtools sort -o - reads.sorted.bam
      samtools index reads.sorted.bam
      
**Copy the nanopolish model files into the working directory**
      
      cp /Programas/nanopolish/etc/r9-models/* .

**Align the reads in event space**

      nanopolish eventalign -t 8 --sam -r reads.fa -b reads.sorted.bam -g draft.fa /
      --models nanopolish_models.fofn | samtools view -Sb - | samtools sort -f - reads.eventalign.sorted.bam
      samtools index reads.eventalign.sorted.bam
      
##Compute the consensus sequence

      python nanopolish_makerange.py draft.fa | parallel --results nanopolish.results -P 16 /
      nanopolish variants --consensus polished.{1}.fa -w {1} -r reads.fa -b reads.sorted.bam -g draft.fa /
      -e reads.eventalign.sorted.bam -t 4 --min-candidate-frequency 0.1 --models nanopolish_models.fofn
      
**Combine all reads**

      python nanopolish_merge.py polished.*.fa > polished_genome.fa
      
Now we check the output

      grep \> polished_genome.fa -c
            2897
            
*Seems like only closed TWO scaffolds. Let's check the size.*

      mkdir genome/
      mv polished_genome.fa genome/
      ll genome/
            59699533    polished_genome.fa
            
*Seems it's slightly smaller than* draft.fa *suggesting either there's less Ns per kb or less repetitions. We'll use QUAST.* 
