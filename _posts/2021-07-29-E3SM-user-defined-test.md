---
layout: post
title: E3SM user defined tests
date: 2021-07-29 15:19
author: Tian
comments: true
categories: [E3SM, Programming]
tags: Null
---
When adding new features to E3SM, you have to create new tests that include these features so that future developments won't mess them up. Good information can be found from [this page](https://esmci.github.io/cime/versions/master/html/users_guide/testing.html#).

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

