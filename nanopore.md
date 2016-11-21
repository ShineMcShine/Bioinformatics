#First nanopore run

        1. Reads are divided into pass and fail folders. Each folder contains their own pass and fail.
        2. Download all the reads.
        3. Use poretools to process files.

###Stats

####*pass*

**WD: /TR9/Run1/pass**

*pass.pass*

poretools stats pass/ > stats.pass.pass.txt 

less stats.pass.pass.txt

        total reads     7592
        total base pairs        36785689
        mean    4845.32
        median  3415
        min     108
        max     122047
        N25     12692
        N50     8508
        N75     5353


*pass.fail*

poretools stats fail/ > stats.pass.fail.txt

less stats.pass.fail.txt

        total reads     34072
        total base pairs        118743098
        mean    3485.06
        median  3341
        min     53
        max     284933
        N25     6782
        N50     5335
        N75     3417
        
####*fail*

**WD: /TR9/Run1/fail**

*fail.pass*

poretools stats pass/ > stats.fail.pass.txt
less stats.fail.pass.txt

        total reads     60
        total base pairs        90155
        mean    1502.58
        median  1408
        min     396
        max     3576
        N25     2489
        N50     1807
        N75     1312
        
*fail.fail*

poretools stats fail/ > stats.fail.fail.txt
less stats.fail.fail.txt

        total reads     580
        total base pairs        1651698
        mean    2847.76
        median  1909
        min     127
        max     129997
        N25     10928
        N50     3389
        N75     2289
 
 
##Converting files from Fast5 to fasta

####*pass*
**WD: /TR9/Run1/pass**

*pass.pass*

poretools fasta pass/ > pass.pass.fasta

*let's see how many 2D reads are there:*

grep -i 2d_2d pass.pass.fasta | wc -l
        
        3796

*pass.fail*

poretools fasta fail/ > pass.fail.fasta

grep -i 2d_2d pass.fail.fasta | wc -l
        
        4642


   
