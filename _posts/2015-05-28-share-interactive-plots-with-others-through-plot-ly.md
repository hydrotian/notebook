---
layout: post
title: Share interactive plots with others through plot.ly
date: 2015-05-28 17:53
author: hydrotian
comments: true
categories: [Plotting]
---
[Plot.ly](https://plot.ly/) is a online platform where everyone can share their data and make interactive plots together (like Github). It provides a set of application program interface (API) to link your code (written by Python, Matlab, R, etc.) from your desktop to this online environment. All you need to do is generate plot from your own programming system and then push the plot (and the data) to plot.ly using a single line command (at least in Matlab). After that this plot can be interactively viewed under your plot.ly account and the data is open to the public.

For example, in Matlab (of course you need to install the API for Matlab beforehand), the command is
`p = fig2plotly(fig);`
and the plot can be viewed at [here](https://plot.ly/~aquazity/23/). You can zoom in and out and check the data on the plot.
