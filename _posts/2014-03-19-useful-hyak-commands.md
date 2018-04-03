---
layout: post
title: Useful Hyak Commands
date: 2014-03-19 10:22
author: hydrotian
comments: true
categories: [Unix]
---
Hyak WIKI:

<a href="https://sig.washington.edu/itsigs/WIKI_for_Hyak_users">https://sig.washington.edu/itsigs/WIKI_for_Hyak_users</a>

Show jobs in queues:
```bash
showq                 #show all jobs
showq -w qos=hpc      #show jobs based on group (hpc)
showq -w class=bf     #show jobs on backfill list
```
Managing jobs:
```bash
mjobctl -c &lt;job_id&gt;   #cancel a job
showbf -q hpc         #show what resources are available for a given group (hpc)
checkjob &lt;job_id&gt;     #check the job status
```
Serial job scripts:
If running single thread job like VIC, it is required to use GNU parallel to bundle the job together and then submit to the node.

first create an ascii file (jobargs) containing the input arguments for the program (e.g. VIC)
then in the script add
```bash
cat /path/to/jobargs | parallel -j $HYAK_SLOTS --joblog paralleljobs.log --resume /path/to/executable/program {}
```
to activate parallel to bundle the job based on the jobargs
