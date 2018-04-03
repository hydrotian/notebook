---
layout: post
title: Activate Matlab kernel in Jupyter notebook
date: 2018-03-24 14:07
author: hydrotian
comments: true
categories: [Matlab]
---
So when you save more and more matlab plotting scripts you starat to lose track of them. You thought you have a script that can generate a plot but just can't find it. 

I decide to use Jupyter notebook to store my plotting scripts as you can see the plots immediately when you open the script. These plots can be saved and stored online as `gist`, so I can browse them whereever I want. Let's see how it goes.

First step is to install Matlab kernel in Jupyter. Here is how

- Install [Anaconda](https://www.anaconda.com/download/)
- Have a Matlab installed (>=2014b)
- Install Matlab Python engine
```cmd
cd "matlabroot\extern\engines\python"
python setup.py install
```
- In Anaconda prompt (run as admin)
  - run `pip install metakernel`
  - run `pip install matlab_kernel`
  - run `pip install pymatbridge`
- Then in Jupyter notebook, you can create new as "Matlab"
  [example](http://nbviewer.jupyter.org/gist/hydrotian/40168c7469ed1d4b580cf92faa6babf8)