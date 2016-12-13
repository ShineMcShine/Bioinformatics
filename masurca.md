#First MaSuRCA run

        1. Illumina reads must be raw, non-trimmed, non-corrected. Otherwise program won't run.
        2. Maximum read length is set by defect at 2047bp, so it must be changed or program will crash. 
           Go to src/AS_global.H and set variable AS_READ_MAX_NORMAL_LEN_BITS to 20. 21 won't work.
           

###Preparing the reads
