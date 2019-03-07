---
layout: post
title: HPSS file extraction
date: 2019-03-06 15:19
author: Tian
comments: true
categories: [E3SM, Programming]
tags: Null
---
[HPSS](https://docs.nersc.gov/filesystems/archive/) (High Performance Storage System) is a mass file storage system for NERSC. Lots of data outputs of E3SM are stored in HPSS.

- To see what's in HPSS, use `hsi` from any NERSC node
- To extract file from HPSS to NERSC, need to use [zstash](https://e3sm-project.github.io/zstash/docs/html/index.html)
    1. load conda: `module load python`
    2. `conda create -n zstash_env -c e3sm -c conda-forge zstash`
    3. `source activate zstash_env`
    4. then the zstash environment is activated, should see `(zstash_env)` leading command line
    5. use `zstash extract --hpss=<path to HPSS> [files]` to extract files
  - Normally the files will be tared in a directory named as 000000.tar, 000001.tar ... etc. and there is a index.db file describing what files are tared. The .db file is a database file can't be read directly. Need to extract to nersc directory first.
  - To read the .db file, use `sqlite3 zstash/index.db "select * from files;>filelist.txt"`  to see the content of the index.db, or [other commands](https://e3sm-project.github.io/zstash/docs/html/database.html)
  - Once find the .tar files that contain the E3SM outputs need to be extracted, extract them and then untar them using `tar -xvf`

