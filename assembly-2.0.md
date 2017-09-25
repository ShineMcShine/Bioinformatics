
## Assembly 2.0
    In which I try to assemble *de novo* the genome of my algae combining nanopore and illumina reads.
    Buckle up for this rollercoaster of fun and despair.
    
## Approaches
    Since we have different approaches I'll make a list of what I intend to do:
    
        1) Use Masurca-superreads with my illumina data to generate superreads, combine them with nanopore data and try canu.
        Then polish the result with nanopolish and improve the quality with Pilon.
        
        2) Use only nanopore reads to generate an assembly with canu. Polish with nanopolish. Then improve using Pilon.
        
        3) Assembly nanopore, then polish using racon. Improve with Pilon.
        
     Then we'll use SSPACE-standar for re-scaffolding. Or whatever.
     
     
### 1) Masurca + Canu
