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
        
        
        *it's important thet the .bam file is sorted. If it's not sorted it won't work (use bamtools sort to sort it).*
        *Also important tha the .bai file is uploaded after the .bam file.*
