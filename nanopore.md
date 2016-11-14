#First nanopore run

        1. Download all files from both pass and fail.
        2. Use poretools to process files.

###Stats

*pass*

poretools stats pass/ > stats.pass.txt 

less stats.pass.txt


        total reads     33234
        total base pairs        129585047
        mean    3899.17
        median  3361
        min     53
        max     284933
        N25     9421
        N50     6282
        N75     3479

*fail*

poretools stats fail/ > stats.fail.txt

less stats.fail.txt

        
        total reads     566
        total base pairs        1629774
        mean    2879.46
        median  1982
        min     352
        max     129997
        N25     11322
        N50     3322
        N75     2295
