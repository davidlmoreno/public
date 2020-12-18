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

## [Problem 4](https://adventofcode.com/2020/day/4)
### Part 1:

    perl -e'$c=0; $f=`cat data4.dat`; $f=~s/\n/ /g; $f=~s/  /\n/g; foreach(split("\n",$f)) { $c++ if join("",sort(m{(byr|iyr|eyr|hgt|hcl|ecl|pid):.*?}gc))=~m/byrecleyrhclhgtiyrpid/; } print "$c\n";'

### Part 2:

    perl -e'$c=0; $f=`cat data4.dat`; $f=~s/\n/ /g; $f=~s/  /;/g; @l=$f=~m{(.*?);}gc; foreach(@l){ %v=(); @m=split(" "); while($#m>-1){ @val=shift(@m)=~m{([a-z]{3}):(.*)}; $v{$val[0]}=$val[1]; } $c++ if ($v{byr}>1919 and $v{byr}<2003 and $v{iyr}>2009 and $v{iyr}<2021 and $v{eyr}>2019 and $v{eyr}<2031 and hgt($v{hgt}) and $v{hcl}=~/#[0-9a-f]{6}/ and $v{ecl}=~/amb|blu|brn|gry|grn|hzl|oth/ and $v{pid}=~/[0-9]{9}/);} print "$c\n"; sub hgt{@b=shift=~m{([0-9]+)(in|cm)}; return ($b[0]>149 and $b[0]<194) if $b[1] eq "cm"; return ($b[0]>58 and $b[0]<77) if $b[1] eq "in"; return 0;}'
    
## [Problem 5](https://adventofcode.com/2020/day/5)
### Part 1:

    cat data5.dat | perl -ne's/[FL]/0/g; s/[BR]/1/g; ($r,$c)=/(.{7})(.*)/; $row = oct("0b".$r); $col=oct("0b".$c); $r=$row*8+$col; print "$r\n";' | sort -n | tail -n 1

### Part 2:

    perl -e'open(IF,"<data5.dat"); foreach(<IF>){ s/[FL]/0/g; s/[BR]/1/g; ($r,$c)=/(.{7})(.*)/; $row = oct("0b".$r); $col=oct("0b".$c); $r=$row*8+$col; push(@l,$r);} close(IF); @sl=sort { $a <=> $b} @l; $rt=shift(@sl); while ($#sl>-1) { $n=shift(@sl); $d=$n-1; print "$d\n" if ($n-$rt>1); $rt=$n;}'

## [Problem 6](https://adventofcode.com/2020/day/6)
### Part 1:

    perl -e'@m=`cat data6.dat`=~m{((?:[a-z]+\n)+)(?:\n|$)}g; $s=0; foreach(@m){s/\n//g; $l=keys{ map { $_ => 1 } /./g }; $s+=$l;} print "$s\n"; '

### Part 2:

    perl -e'@m=`cat data6.dat`=~m{((?:[a-z]+\n)+)(?:\n|$)}g; $s=0; foreach(@m){$n=@p=(/\n/g); s/\n//g; foreach $k (keys{ map { $_=>1 } /./g }) { $ct=@d=$_=~/$k/g; $s++ if ($ct == $n);}} print "$s\n";'


## [Problem 7](https://adventofcode.com/2020/day/7):
### Part 1:

    perl -e'$input=`cat <data7.dat`; @s=("shiny gold"); while($#s>-1) { $g=shift(@s); @m=$input=~m{([\w]+ [\w]+) bag[s]* contain(.*)?\n}g; while($#m>-1) { $a=shift(@m); $b=shift(@m); do { push(@s,$a); print "$a\n" } if $b=~/$g/;}}' | sort | uniq | wc -l

### Part 2:

    perl -e'$in=`cat <data7.dat`; $c=-1; @s=("shiny gold"); while ($#s>-1){ $c++; $g=shift(@s); next if $in=~m/$g bags contain no/; @m=$in=~m{$g bag[s]* contain (.*?)\n}g; @v=split(/, /,$m[0]); map { $_=~m{([\d]+) ([\w]+ [\w]+)}; push(@s,$2) foreach (1..$1);} @v;} print "\n$c\n";'

## [Problem 8](https://adventofcode.com/2020/day/8):
### Part 1:

    perl -e'$in=`cat data8.dat`; @vis=(); $l=1; $acc=0; map {$p{$l} = $_; $l++} split("\n",$in); run($p{1},1); print "$acc\n"; sub run {($op,$val)=shift=~/([\w]+) ([+-][\d]+)/; $w=shift; return if grep { $w eq $_} @vis; push (@vis,$w); do { $acc+=$val if $op eq "acc"; $n=$w+1; run($p{$n},$n)} if $op=~/acc|nop/; do { $g=$w+$val; run($p{$g},$g)} if $op eq "jmp";}'
