#First MaSuRCA run

        1. Illumina reads must be raw, non-trimmed, non-corrected. Otherwise program won't run.
        2. Maximum read length is set by default at 2047bp, so it must be changed or program will crash. 
           Go to src/AS_global.H and set variable AS_READ_MAX_NORMAL_LEN_BITS to 20. 21 won't work.
           

###Preparing the reads

        1. Celera uses a specific file format called .frg because of reasons, so we'll need to convert our files.
        2. Fortunately wgs has a series of tools to do that, amongst them fastqToCA and fastaToCA.
        3. fastqToCA, however, generates an empty file so we'll use fastaToCA instead.

**fastq to fasta and qual**

        1. fastaToCA requires both a fasta file and a quality file.
        2. To extract both files from a fastq file we'll use fastq_to_qual.pl perl script:
                
                #!/bin/perl

                use strict;
                use warnings;

                my $csfastq = shift;
                die unless defined($csfastq);
                my $csfasta = $csfastq; $csfasta =~ s/csfastq$/csfasta/;
                die unless !($csfastq eq $csfasta);
                my $qual = $csfastq; $qual =~ s/.csfastq$/_QV.qual/;
                die unless !($csfastq eq $qual);

                open(FHcsfastq, "$csfastq") || die;
                open(FHcsfasta, ">$csfasta") || die;
                open(FHqual, ">$qual") || die;
                my $state = 0;
                my ($n, $r, $q) = ("", "", "");
                while(defined(my $line = <FHcsfastq>)) {
                    chomp($line);
                    if(0 == $state) {
                        &print_out(\*FHcsfasta, \*FHqual, $n, $r, $q);
                        $n = $line;
                        $n =~ s/^\@/>/;
                    }
                    elsif(1 == $state) {
                        $r = $line;
                    }
                    elsif(3 == $state) {
                        $q = $line;
                        # convert back from SANGER phred
                        my $tmp_q = "";
                        for(my $i=0;$i<length($q);$i++) {
                            my $Q = ord(substr($q, $i, 1)) - 33;
                            die unless (0 < $Q);
                            if(0 < $i) {
                                $tmp_q .= " ";
                            }
                            $tmp_q .= "$Q";
                        }
                        $q = $tmp_q;
                    }
      

mv TR9.nanopore.fastq TR9.nanopore.csfastq
perl fastq_to_qual.pl TR9.nanopore.csfastq
ls
        TR9.nanopore.csfastq
        TR9.nanopore.csfasta
        TR9.nanopore.qual
        
**fasta and qual to frg**

./fastaToCA -l TR9nanopore -s /PATH/TR9.nanopore.csfasta -q /PATH/TR9.nanopore.qual > TR9.nanopore.frg
