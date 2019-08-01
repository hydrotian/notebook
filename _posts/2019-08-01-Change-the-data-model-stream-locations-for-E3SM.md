---
layout: post
title: Change the data model stream locations for E3SM
date: 2019-08-01 15:19
author: Tian
comments: true
categories: [E3SM, Programming]
tags: Null
---
E3SM I-cases are for ATM data model, for example, ELM + MOSART forced by climate forcing. The default climate data is Qian2006 at T62 grid. There are two ways to replace this forcing.

##### 1. Change the paths in cime
Only one file needs to be modified in this file: `/cime/src/components/data_comps/datm/cime_config/namelist_definition_datm.xml`
- Stream domain file directory `the directory that contains domain file`
- Stream domain file name `the domain file that associated with the forcing file`
- Stream data file directory `the forcing file direcotory`

##### 2. Define a user stream file 
- Copy the `datm.streams.txt.CLM...` file from CaseDocs to the case directory (one level up)
- Change the paths to the new forcing files in this file
- Modify the file name by adding `user_` header

[Reference from CESM] (http://www.cesm.ucar.edu/models/cesm1.2/data8/doc/c72.html)




