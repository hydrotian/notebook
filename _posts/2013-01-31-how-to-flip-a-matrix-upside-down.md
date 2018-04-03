---
layout: post
title: How to flip a matrix upside down
date: 2013-01-31 23:35
author: hydrotian
comments: true
categories: [Programming, Unix]
---
So recently we had some problem about flipping a matrix upside down, sounds easy, but not that simple if the matrix is stored as an array. For example, you have a 5*4 matrix like this:

```matlab
1 1 1 1
2 2 2 2
3 3 3 3
4 4 4 4
5 5 5 5
```
and it's written like this
`11112222333344445555`
The question is how to flip the matrix upside down without transforming the array to the 2D matrix. After flipping, the array should be like this:
`55554444333322221111`
So I wrote a Perl script to do this:

```perl
#!/usr/bin/perl -w

# this script convert an array to an n*m matrix and then 
# flip the matrix upside down then restore it back to an array

use strict;
my @div = (1,1,1,1,2,2,2,2,3,3,3,3,4,4,4,4,5,5,5,5);
print "@div\n";

my $row = 2;
my $col = 10;

my @out = map{0} 0..$#div; # define an empty array
print "@out\n";

for (my $i = 1 ; $i &lt;= $row; $i++) {
    for (my $j = 1; $j &lt;= $col; $j++) {
        $out[($col*($row-$i)+$j)-1] = $div[(($i-1)*$col+$j)-1];
    }
}

print "@out\n";
```
It's an interesting exercise and it's useful sometimes when you're dealing with NetCDF files and Arcmap ascii outputs or converting files between binary and ascii.
