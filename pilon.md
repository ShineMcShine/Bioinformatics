#Pilon run

        1. Pilon (https://github.com/broadinstitute/pilon) succesfully installed in Lluis Vives supercomputer.
        2. I'll try to improve the nuclear assembly (contigs.fa) with the transcriptome .bam files

###Improving the assembly

**WD: /TR9/draft_genome/**
        
        mkdir pilon_out
        ls
                contigs.fa
                merge.sorted.bam
                merge.sorted.bam.bai
                pilon_out/
        
        
   *It's important that the .bam file is sorted (use bamtools). Also important tha the .bai file is uploaded after the .bam file*

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
               --outdir /pathway/TR9/draft_genome/pilon_out/ --threads 8 --K 97 --changes

save as *pilonTR9.sh*

qsub pilonTR9.sh

###Output

1. After ~10 hours run was completed.

        cd pilon_out/
        ls
                pilon.fasta
                pilon.changes
                
        grep \> pilon.fasta -c
                2899
        
        grep \> ../contigs.fa -c
                2899
                
        module load abyss
        
        abyss-fac pilon.fasta
                n       n:500   n:N50   min     N80     N50     N20     E-size  max     sum     name
                2899    1348    121     501     59234   37093   252194  195588  020218  56.84e6 pilon.fasta

        abyss-fac ../contigs.fa
                n       n:500   n:N50   min     N80     N50     N20     E-size  max     sum     name
                2899    1348    120     502     61158   142331  263564  202427  1056750 58.58e6 contigs.fa

###Assessing the quality with QUAST

**WD: /secuencias/TR9/**
        
        QUAST (http://quast.sourceforge.net/) succesfully installed.
        
        python quast.py -o ../Secuencias/TR9/ -e -s ../Secuencias/TR9/contigs.fa
        
        less report.txt

                Assembly                    contigs   contigs_broken
                # contigs (>= 0 bp)         2899      3559          
                # contigs (>= 1000 bp)      1030      2689          
                # contigs (>= 5000 bp)      695       1840          
                # contigs (>= 10000 bp)     612       1463          
                # contigs (>= 25000 bp)     503       799           
                # contigs (>= 50000 bp)     361       321           
                Total length (>=0 bp)      59502099  58587840      
                Total length (>= 1000 bp)   58840114  58111515      
                Total length (>= 5000 bp)   58113360  56106057      
                Total length (>= 10000 bp)  57523423  53408367      
                Total length (>=25000 bp)  55653843  42381738      
                Total length (>= 50000 bp)  50283455  25433977      
                # contigs                   1359      3559          
                Largest contig              1063563   239804        
                Total length                59078091  58587840      
                GC (%)                      49.76     49.76         
                N50                         142866    43258         
                N75                         74176     23217         
                L50                         120       405           
                L75                         264       864           
                # N's per 100 kbp           829.84    0.00   
       
       mkdir pilon
       
       mv pilon.fasta /pilon
       
       python quast.py -o ../Secuencias/TR9/pilon/ -e ../Secuencias/TR9/pilon/pilon.fasta
       
       less report.txt
       
                Assembly                    pilon     pilon_broken
                # contigs (>= 0 bp)         2899      3535        
                # contigs (>= 1000 bp)      1029      2676        
                # contigs (>= 5000 bp)      693       1825        
                # contigs (>= 10000 bp)     612       1436        
                # contigs (>= 25000 bp)     500       774         
                # contigs (>= 50000 bp)     355       309         
                Total length (>= 0 bp)      57689249  56844742    
                Total length (>= 1000 bp)   57026530  56370430    
                Total length (>= 5000 bp)   56294415  54345493    
                Total length (>= 10000 bp)  55720144  51543949    
                Total length (>= 25000 bp)  53836421  40509747    
                Total length (>= 50000 bp)  48403794  24049854    
                # contigs                   1359      3535        
                Largest contig              1026274   235478      
                Total length                57265252  56844742    
                GC (%)                      49.86     49.86       
                N50                         138014    42085       
                N75                         71695     22498       
                L50                         121       406         
                L75                         265       864         
                # N's per 100 kbp           734.32    0.00
