#Nanopore Run 2
  
  1. Software crashed after running for 8 hours. Could be restarted succesfully but total run was only ~30 hours.
  2. Downloaded the reads from the 8 hours run (Run2A-8h) and the 30 hours run (Run2B-30h)
  3. Use poretools to process the reads.
  
##Stats
###Run2A-8h
*pass*

  poretools stats pass/ > stats.pass.txt
  
  less stats.pass.txt
    
    total reads     16530
    total base pairs        41949006
    mean    2537.75
    median  1210
    min     92
    max     17457
    N25     7667
    N50     5718
    N75     3377
    
*fail*

  poretools stats fail/ > stats.fail.txt
  
  less stats.fail.txt
  
    total reads     68912
    total base pairs        118541822
    mean    1720.19
    median  835
    min     5
    max     97564
    N25     4158
    N50     3343
    N75     2657

###Run2B-30h

*pass*

  poretools stats pass/ > stats.pass.txt
  
  less stats.pass.txt
  
    total reads     28743
    total base pairs        72631285
    mean    2526.92
    median  1199
    min     92
    max     17457
    N25     7681
    N50     5661
    N75     3349

*fail*

  poretools stats fail/ > stats.fail.txt
  
  less stats.fail.txt
    
    total reads     118647
    total base pairs        204784651
    mean    1726.00
    median  858
    min     5
    max     103063
    N25     4058
    N50     3335
    N75     2627

##Converting files from fast5 to fasta 

**WD: TR9/Run2/Run2A-8h**

*pass*

    poretools fasta pass/ > pass2A.fasta
    abyss-fac pass2A.fasta
      n = 16530     N50 = 5861

    poretools fasta --min-length 1000 pass/ > pass2A.1000.fasta
    abyss-fac pass2A.1000.fasta
      n = 8799      N50 = 5861

*fail*
    
    poretools fasta fail/ > fail2A.fasta
    abyss-fac fail2A.fasta
      n = 68912     N50 = 3361
    
    poretools fasta --min-length 1000 fail/ > fail2A.1000.fasta
    abyss-fac fail2A.1000.fasta
      n = 32503     N50 = 3377

**WD: TR9/Run2/Run2B-30h**

*pass*
    
    poretools fasta pass/ > pass2B.fasta
    abyss-fac pass2B.fasta
      n = 28743     N50 = 5798
      
    poretools fasta --min-length 1000 pass/ > pass2B.1000.fasta
    abyss-fac pass2B.1000.fasta
      n = 15251     N50 = 5934

*fail*

    poretools fasta fail/ > fail2B.fasta
    abyss-fac fail2B.fasta
      n = 118647      N50 = 3352
      

##concatenating fail/pass and old nanopore data
**WD: TR9/**

1. cat Run2/Run2A-8h/\*fasta Run2/Run2B-30h/\*fasta > Run2.fasta
2. cat Run1/pass/\*fasta Run1/fail/\*fasta > Run1.fasta
3. cat Run1.fasta Run2.fasta > TR9.nanopore.fasta

4. grep \\> TR9.nanopore.fasta | wc -l
        
        275136

##removing small reads (\<1000bp)

1. nano
    
        ## removesmalls.pl
        #!/usr/bin/perl
        
        use strict;
        use warnings;
    
        my $minlen = shift or die "Error: `minlen` parameter not provided\n";
      
        {
          local $/=">";
          while(<>) {
          
            chomp;
            next unless /\w/;
            s/>$//gs;
            my @chunk = split /\n/;
            my $header = shift @chunk;
            my $seqlen = length join "", @chunk;
            print ">$_" if($seqlen >= $minlen);
          }
        local $/="\n";
        }

    save as "removesmalls.pl"
    
2.  perl removesmalls.pl 1000 TR9.nanopore.fasta > TR9.1000.nanopore.fasta
3.  grep \\> TR9.1000.nanopore.fasta | wc -l
          
          144748

4.  abyss-fac TR9.1000.nanopore.fasta

          n = 144748      N50 = 3627
