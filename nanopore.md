#First nanopore run

        1. Reads are divided into pass and fail folders. Each folder contains their own pass and fail.
        2. Download all the reads.
        3. Use poretools to process files.

###Stats

####*pass*

#####WD: /TR9/Run1/pass

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

        
        

   
