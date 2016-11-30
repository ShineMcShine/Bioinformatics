#Pilon run

        1. Pilon (https://github.com/broadinstitute/pilon) succesfully installed in Lluis Vives supercomputer.
        2. I'll try to improve the nuclear assembly (contigs.fa) with the transcriptome .bam files

###Improving the assembly

**WD: /scratch/hi/hivier/TR9/draft_genome/**
        
        mkdir pilon_out
        ls
                contigs.fa
                merge.sorted.bam
                merge.sorted.bam.bai
                pilon_out/
        
        
*It's important that the .bam file is sorted (use bamtools).*
*Also important tha the .bai file is uploaded after the .bam file*

###The Pilon script

nano

        #!/bin/bash
        #$ -m ae
        #$ -M myadress@uv.es
        #$ -N pilon
        #$ -pe cpus 8
        #$ -o /pathway/TR9/draft_genome/$JOB_ID.err -j y

        export PILON="java -Xmx64G -jar /util/biologia/pilon/pilon-1.20.jar"

        cd /scratch/hi/hivier/TR9/draft_genome/

        $PILON --genome /pathway/TR9/draft_genome/contigs.fa --frags /pathway/TR9/draft_genome/merge.sorted.bam 
               --outdir /pathway/TR9/draft_genome/pilon_out2/ --threads 8 --K 97 --changes

