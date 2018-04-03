---
layout: post
title: Pointer in Matlab
date: 2013-10-28 14:06
author: hydrotian
comments: true
categories: [Matlab, Programming]
---
Create a 1d pointer which has n elements
```matlab
<pre>for i = 1:n;
ptr (i) = libpointer('doublePtr',rand(5));
end
```
if it's a 2d pointer then use
`'doublePtrPtr'`
Get the value from pointer:
`value = get(ptr(i),'Value');`