---
layout: post
title: Cluster computing on Hydra
date: 2013-10-22 18:03
author: hydrotian
comments: true
categories: [Programming, Unix]
---
submit the job to the queue:
`qsub ./executable.src`
check the job status:
`qstat -f` or ` qstat -f -u "*"`
header lines in executable.src:
```bash
#$ -cwd
#$ -j y
#$ -S /bin/bash
#$ -q default.q@compute-0-1.local        # optional, define a specific node to run the code
```
delete job id between 10000 and 20000:
```bash
qdel echo `seq -f "%.0f" 10000 20000`
```
tip: copy the input files to /state/patition1 on node to prevent high I/O traffic
