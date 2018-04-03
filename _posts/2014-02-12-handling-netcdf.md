---
layout: post
title: Handling NetCDF
date: 2014-02-12 13:58
author: hydrotian
comments: true
categories: [NetCDF, Unix]
---
#### 1. See data

`ncdump -h or -c  #see the dimension and data info`

#### 2. Manipulating data

Extract data by timestep
`cdo -O seltimestep,1,32,61 in.nc out.nc #extract the timestep 1, 32, and 61 from in.nc`
Extract data by variable
`cdo -O selname,storage in.nc out.nc #extract the storage variable`
or
`ncks -A -v storage in.nc out.nc`
Delete data by timestep
`cdo -O delete,timestep=13 temp.nc temp1.nc # delete the 13th time frame`
Rename a variable
`ncrename -v OldVarName,NewVarName in.nc #change variable name`

#### 3. Math

Add/subtract/multiply/divide two nc files
`cdo -O add/sub/mul/div in1.nc in2.nc out.nc #add two nc files and output`
Multiply by a constant (e.g. 0)
`cdo -O mulc,0 in1.nc out.nc`
Create new variable by adding others
`ncap -O -s "storage=SoilMoist_1+SoilMoist_2+SoilMoist_3+SWE" in.nc out.nc` 
or
`cdo expr,'runoff=srfrun+subrun;' in.nc out.nc`
Add the field
`cdo -O fldsum in.nc out.nc #add the value at each field`

#### 4. I/O

Output to ascii
`cdo output in.nc &gt; out.txt`

#### 5. ncview

Output frame as .png file when using ncview
`ncview -frame in.nc`