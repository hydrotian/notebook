---
layout: post
title: E3SM user defined tests
date: 2021-07-29 15:19
author: Tian
comments: true
categories: [E3SM, Programming]
tags: Null
---
### Applying tests
Before merging a Pull Request to master, the model integrators need to perform a series of tests to evaluate the new changes made in the PR. If the PR is about a new model feature, and come with a flag to activate/deactivate this feature, then we need to make sure the PR won't change the baseline results (i.e. bit for bit, B4B) when the new feature flag is turned off. There are a few steps to do such test:
- First create a branch off master then switch to that branch
- Say we want to do a testing suite called "e3sm_land_developer", the next step is to create baseline cases based on the master code:

    - in `cime/scripts` directory, do `./create_test e3sm_land_developer --baseline-root /path/to/baseline/case/dir -b 07e202a -t 07e202a --project E3SM --walltime 00:30:00 -g -v -j 4`
    - here `07e202a` is the hashtag for the master
- Then make changes to the code, commit it, and do another set of test for comparison
    - `./create_test e3sm_land_developer --baseline-root /path/to/baseline/case/dir -b 07e202a -t 6ebc21d --project E3SM --walltime 00:30:00 -c -v -j 4`
    - here `6ebc21d` is the hashtag for the new commit
    - note in the command line the `-g` is changed to `-c`, indicating this is a comparison test
- The test runs will generate a number of new folders in the scratch directory for the simulation results. Wait until the tests are completed, then go to the scratch directory, you will find a new file is generated. In this case, it should be `cs.status.6ebc21d`. Execute this file, you will see the comparison report showing if the tests are passed or failed. Here is an example for a passed test:
```bash
  SMS.r05_r05.IELM.compy_intel.elm-topounit (Overall: PASS) details:
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit CREATE_NEWCASE
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit XML
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit SETUP
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit SHAREDLIB_BUILD time=135
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit NLCOMP
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit MODEL_BUILD time=395
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit SUBMIT
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit RUN time=196
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit GENERATE ttt
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit MEMLEAK
    PASS SMS.r05_r05.IELM.compy_intel.elm-topounit SHORT_TERM_ARCHIVER
```
- it's worth mentioning that the tests are performed by a program called `cprnc`. Very nice tool that can tell you if the fields in two netcdf files are identical or not. If the test is failed, you need to find out what variable caused the problem in cprnc output file. Usually it's located in the case directory with a suffix `cprnc.out`, in which every compared variables are listed. An example showing different fields during comparison:
```bash
x2r_Flrl_Tqsur   (x2r_nx,x2r_ny,time)  t_index =      1     1
          8   259200  (   237,   137,     1) (    10,    24,     1) (   259,   118,     1) (   259,   118,     1)
              259200   3.152938049601497E+02   0.000000000000000E+00 1.1E-13  3.004722862865883E+02 7.9E-21  3.004722862865883E+02
              259200   3.152938049601497E+02   0.000000000000000E+00          3.004722862865882E+02          3.004722862865882E+02
              259200  (   237,   137,     1) (    10,    24,     1)
          avg abs field values:    9.840048636599937E+01    rms diff: 4.6E-16   avg rel diff(npos):  7.9E-21
                                   9.840048636599937E+01                        avg decimal digits(ndif): 15.6 worst: 15.4
 RMS x2r_Flrl_Tqsur                   4.6035E-16            NORMALIZED  4.6783E-18
```
- We can also use `cprnc` to do comparisons after the runs are completed. The executable can be found in `/compyfs/e3sm_baselines/cprnc`. All you need to do is do `cprnc /path/to/first/nc/file /path/to/second/nc/file`
- If only want to do one single test from the testing suite, we can just do for example `./create_test SMS.r05_r05.IELM.compy_intel.elm-topounit -p E3SM`.

### Adding new tests into testing suite
After adding new features to E3SM, you have to create new tests that include these features so that future developments won't mess them up. Good information can be found from [this page](https://esmci.github.io/cime/versions/master/html/users_guide/testing.html#) and [this page](https://github.com/ESCOMP/ctsm/wiki/System-Testing-Guide#testmods).

If these features are not defalut on master, you have to performe tests with active "testmods" to activate the features through shell commands or user defined name list files. Here's an example:

Say  I want to call the testmod "bgc_features", and I think this test should be categorized in "elm" group, then in `.../E3SM/components/elm/cime_config/testdefs/testmods_dirs/elm/` you can create a new folder called `bgc_features`

Then in this folder, put the `user_nl_elm` and `user_nl_mosart` in it for the new features. If the feature involvs modifications of xml file, then create a file named `shell_commands` in it:

```bash
#!/bin/bash
./xmlchange --id ELM_BLDNML_OPTS --val "-bgc cn -irrig .true."
```

Then this configuration with new features is available for testing. Simply go to  `.../E3SM/cime/scripts` and run the test. An example for a restart test:

`./create_test ERS.r05_r05.IELM.compy_intel.elm-bgc_features -p E3SM`

The last part of the command `.elm-bgc_features` indicates this is a "testmod" type of test.

If you want to make it official, just add this line to the top of the `tests.py` in `.../E3SM/cime_config`. So that in the future everyone will need to pass this test for their new developments.

