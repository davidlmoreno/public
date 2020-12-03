This file contains solutions for the [Advent of Code 2020](https://adventofcode.com/2020) problems in the form of Perl one-liners. Because why not?

## [Problem 1](https://adventofcode.com/2020/day/1)
### Part 1:

    perl -e'open(IF,"<data1.dat"); @list=(); foreach(<IF>) { chop(); push(@list,$_);} close(IF); foreach $l (@list) {$left=2020-$l; if (grep {$_ eq $left} @list) { $product=$l*$left; print "$product\n"; exit(0);}}'

### Part 2:

    perl -e'open(IF,"<data1.dat"); @list=(); foreach(<IF>) { chop(); push(@list,$_);} close(IF); foreach $it (@list) {$rem=2020-$it; foreach $l (@list) {$left=$rem-$l; if (grep {$_ eq $left} @list) { $product=$it*$l*$left; print "$product\n"; exit(0);}}}'

## [Problem 2](https://adventofcode.com/2020/day/2)
### Part 1:
   
    cat data2.dat | perl -n -e'@d=$_=~m{([0-9]+)-([0-9]+) ([a-z]): ([a-z]+)}c; $str=join("",sort(split("",$d[3]))); $regex="(^|[^$d[2]])[$d[2]]{$d[0],$d[1]}([^$d[2]]|\$)"; print $_ if $str =~ m/$regex/;' | wc -l

### Part 2:

    cat data2.dat | perl -n -e'@d=$_=~m{([0-9]+)-([0-9]+) ([a-z]): ([a-z]+)}c; $pos=$d[0]-1; $delta=$d[1]-$d[0]-1; $regex="^.{$pos}$d[2].{$delta}([^$d[2]])|^.{$pos}[^$d[2]].{$delta}$d[2]"; print $_ if $d[3]=~m/$regex/;' | wc -l

## [Problem 3](https://adventofcode.com/2020/day/3)
### Part 1:
   
    perl -e'open(IF,"<data3.dat"); $tr=0; $x=0; foreach (<IF>) { $rx="^.{$x}#"; $tr++ if(/$rx/); $x=($x+3)%length();} print "$tr\n";'

### Part 2:

    perl -e'$res=go(3,1)*go(1,1)*go(5,1)*go(7,1)*go(1,2); print "$res\n"; sub go {$h=shift; $v=shift; $x=0; $ln=0; $tr=0; open(IF,"<data3.dat"); foreach (<IF>) { $ln++; next if (($ln-1)%$v); $rx="^.{$x}#"; $tr++ if(/$rx/); $x=($x+$h)%length();} close(IF); return $tr;}'
