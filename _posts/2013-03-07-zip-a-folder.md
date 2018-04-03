---
layout: post
title: Some commands in Unix
date: 2013-03-07 17:51
author: hydrotian
comments: true
categories: [Programming, Unix]
tags: Unix
---
### Head and Tail

it allows you to clip the data by line numbers (-n) or by bytes. The following example is to print the 1st 1K bytes of the last 2000000K bytes of a file.

`tail --bytes=2000000K test.dbf | head --bytes=1k`

### Zip and unzip a folder

gzip can only zip a single file. If you want to zip a folder, need to use "tar":
example:

`$ tar -zcvf tian.tar.gz /home/tian`

to untar:

`$ tar -zxvf tian.tar.gz`

if it's .tar instead of .tar.gz, then remove the -z

`$ tar -xvf tian.tar`

### Change permission

```bash
$ chmod a=rwx ./*        #changes all to read, write, and executable
$ chmod -R go-w TMPAV7/    #removes the write permission from group or other for all files in TMPAV7 directory
```
### Find string in a directory
```bash 
$ grep -r "boo" /path/to/file  #find all file with "boo" 
```
### Hotkey
`ctrl+alt+F1` to exit x window
`startx` to open x window
`ctrl+alt+*` to kill the front process
### File download
- using `wget` to download files from http sites
```bash
$ #download a single file
$ wget http://hydrology.princeton.edu/data/pgf/0.5deg/daily/prcp_daily_1948-1948.nc
$ #download a directory without including the index.html files
$ wget -r --no-parent --reject "index.html*" ftp://gmaoftp.gsfc.nasa.gov/pub/data/sarith/TianZhou/
```
- using `ncftpget` to download files from ftp sites
```bash
$ ncftpget ftp://ftp.hydro.washington.edu/pub/tizhou/xxx.bin
```

### Count number of files in a dir
`$ ls -l | wc -l`
### Split an ascii file into several parts by line
`$ split -l 2000 inflie.asc`
### Cut columns from a file with fixed delimiter (e.g. soil parameter)
-d: delimiter, default is tab
-f: cut by field; -c: cut by characters; -b: cut by bytes
```bash
cut -d " " -f 1-53 soil.current &gt; newsoil   #cut soil.current 1-53 columns by space
```
### See the first and last 10 lines of a text file
`$ (head;echo;tail) &lt; file.txt`

### AWK
- file I/O
output 1st and 3rd columns

`$ awk '{print $1, $3}' PNW.KW.flow.151dams &gt; junk.txt`

it can also read some columns from one file and write some of the columns to another file -F is to define the delimiter in the read in file.

`$ awk -F',' '{printf("%.1f,%.1f,%.4f\n", $4, $5, $3)}' in.csv &gt; out.txt`

### Shortcuts and tricks
- Change directory to your previous location
`$ cd -`
- Last command argument:`!$`
example:
```bash
$ mkdir temp
$ cd !$
```
- Last command argument: `!!`
example:
```bash
$ ping google.c
$ ping !!om     # this will give you "ping google.com"
```
- Combine multiple lines to one line
```bash
$ less test.txt | - - -&gt; newtest.txt   
# every three lines in old file goes to one line in new file
```
