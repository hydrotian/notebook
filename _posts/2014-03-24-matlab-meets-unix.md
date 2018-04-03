---
layout: post
title: Matlab meets Unix
date: 2014-03-24 12:14
author: hydrotian
comments: true
categories: [Matlab, Unix]
---
#### 1. running Matlab .m script in terminal without showing desktop (for batch running).
know where the Matlab is installed. For example in my Mac the Matlab is installed in
`/Applications/MATLAB_R2012b.app/bin`
call Matlab from anywhere that stores the script (test_script.m) you want to run
```bash
/Applications/MATLAB_R2012b.app/bin/matlab -nodesktop -nojvm -nosplash -r "test_script(arguments)"
```
#### 2. compile Matlab .m scripts to executables.
in Matlab GUI use `Apps -> Matlab complier`

in terminal use `mcc -m test_script.m` to generate an executable file (test_script) and a shell script (run_test_script.sh) which is used to run the executable.

Then in terminal:
```bash
./run_test_script.sh /PATH/TO/MATLAB/ arguments
```
**Note that the arguments will be passed to Matlab as strings. That means if they are numbers you need to add `str2num` in the code to convert them from strings to the numbers.**