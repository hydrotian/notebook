---
layout: post
title: Python directly read in USGS flow by given site number
date: 2016-04-07 17:53
author: hydrotian
comments: true
categories: [Python]
---
Python packages are very powerful. Here is an example for hydrological data analysis. This small piece of code reads USGS flow by a given site number and store it as pandas DataFrame. You need to install "<a href="https://anaconda.org/ioos/ulmo">ulmo</a>" first.

Original post is from <a href="http://earthpy.org/tag/pandas.html">here</a>

```python
import pandas as pd
import numpy as np
import ulmo

def importusgssite(siteno):
    sitename = {}
    sitename = ulmo.usgs.nwis.get_site_data(siteno, service=&quot;daily&quot;, period=&quot;all&quot;)
    sitename = pd.DataFrame(sitename['00060:00003']['values'])
    sitename['dates'] = pd.to_datetime(pd.Series(sitename['datetime']))
    sitename.set_index(['dates'],inplace=True)
    sitename[siteno] = sitename['value'].astype(float)
    sitename[str(siteno)+'qual'] = sitename['qualifiers']
    sitename = sitename.drop(['datetime','qualifiers','value'],axis=1)
    sitename = sitename.replace('-999999',np.NAN)
    sitename = sitename.dropna()
    #sitename['mon']=sitename.index.month
    return sitename

d = importusgssite('12472800');
d.plot(style='r', linewidth=1.0)
```
<img class="alignnone size-full wp-image-1064" src="https://tianzhounote.files.wordpress.com/2016/04/flowseries1.png" alt="flowseries.png" width="3000" height="2000" />
