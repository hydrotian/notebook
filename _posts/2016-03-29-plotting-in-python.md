---
layout: post
title: Plotting in Python
date: 2016-03-29 18:00
author: hydrotian
comments: true
categories: [Plotting, Python]
---
Finally start to use python for plotting.
#### Plotting in notebook

ipython notebook is good and plots can be lined up with scripts. But if I need to run the same script on cluster without Xming, it will crash. Simple way is to add
```python
import matplotlib as mpl
mpl.use('Agg')
```
when importing libraries.
#### Seaborn
[Seaborn](https://stanford.edu/~mwaskom/software/seaborn/index.html) is a great package for color scheme picking. Color schemes are called "palettes" in seaborn. There are many [pre-set palettes](http://chrisalbon.com/python/seaborn_color_palettes.html) for you to choose. If you want to use "colorblind", just simply add
```python
import seaborn as sns
with sns.color_palette("colorblind"):
```
before the plotting script.

Customizing the palette is easy too:
```python
colors = ["windows blue", "amber", "greyish", "faded green", "dusty purple"]
with sns.xkcd_palette(colors):
```
or
```python
flatui = ["#9b59b6", "#3498db", "#95a5a6", "#e74c3c", "#34495e", "#2ecc71"]
sns.set_palette(flatui)
with sns.color_palette():
```
