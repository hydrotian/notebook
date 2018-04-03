---
layout: post
title: Using Monte Carlo method to find critical values
date: 2013-01-15 18:31
author: hydrotian
comments: true
categories: [Math, Matlab]
---
Monte Carlo 方法是一种利用计算机的高速计算的特性，简单粗暴的解决问题的一种方法，依赖的是计算机能够在样本量足够大的情况下以特定的概率分布函数近似的模拟事件。比如说找圆周率。我写了一小段Matlab代码：
```matlab
clc; clear;
% 在一个正方形里用uniform distribution均匀随机撒100000个点
 samplesize = 100000;
 X = (unidrnd (200,samplesize,1))./100;
 Y = (unidrnd (200,samplesize,1))./100;
 num_in = 1;
 num_out = 1;
%计算有多少个点在圆内，多少个在外
 for i = 1:samplesize;
 if ((X(i)-1)^2+(Y(i)-1)^2)^0.5 &lt;= 1;
 num_in = num_in+1;
 else
 num_out = num_out+1;
 end
 end
%进而得到圆周率
 Pi = (num_in/samplesize)*4;
```
结果还不错，可以精确到小数点后两位数字，即3.14。

在最近的research中，我也使用了Monte Carlo方法来解决问题。其实也没有更好的方法。

事情很简单，有两列数据，描述同一件事情，比如两个gauge 同时测量某地的降水，在使用 bivariate test (Maronna and Yohai, 1978; Potter, 1981)对其进行systematic consistency test的时候，需要一个critical value来reject the null hypothesis at certain significant level (e.g. 0.05) (类似于student t test)。由于是小样本量，那么这个critical value 就比较依赖于自由度，不同的样本量其value是不同的。几十年前的文献中仅提供了30, 50, 70, 100等几个样本量对应的critical value，类似于z test or t test table。那么对于我121个样本的test来说，我就需要自己找到这个value。最理想的办法莫过于通过推导找到这个test statistic的解析表达式，就好象正态分布的那个优美的表达式一样，然后再通过积分找到相应的values。可是这个test过于复杂，于是Monte Carlo就上场了，轻而易举的解决战斗。具体做法是随机生成两组正态分布的数列，每组数列有121个样本，对这两组数列做bivariate test, test statistic 可以给出一个score来。然后重复此过程100000遍，共产生100000个scores。在这100000个数字里，找出第95000小的数(0.05)，就是我们要找的critical value了。这里有几个假设，样本是正态分布的(某地的降水，遵循正态分布)，两个样本之间没有correlation。其实几十年前的前辈们也是通过同样的方法来得到这些值的，只可惜那时电脑速度太慢，现在一分钟就可以完成的事情，在那时可能得好几天。
```matlab
clc; clear;
 for k =1:100000;
 x = normrnd (0,1,121,1); % generate two normal distributed sample sets
 y = normrnd (0,1,121,1);
 [m,n] = size (x);
 X = zeros (m,1);
 Y = zeros (m,1);
 for i = 1:m;
 if i==1;
 X(i) = x(i);
 Y(i) = y(i);
 else
 X(i) = (x(i)+X(i-1)*(i-1))/i;
 Y(i) = (y(i)+Y(i-1)*(i-1))/i;
 end
 end
 X_bar = X(m);
 Y_bar = Y(m);
 Sx = zeros (m,1);
 Sy = zeros (m,1);
 Sxy = zeros (m,1);
 for i = 1:m;
 Sx(i)=(x(i)-X_bar)^2;
 Sy(i)=(y(i)-Y_bar)^2;
 Sxy(i)=(x(i)-X_bar)*(y(i)-Y_bar);
 end
 Sx = sum (Sx);
 Sy = sum (Sy);
 Sxy = sum (Sxy);
 F = NaN (m,1);
 D = NaN (m,1);
 test_result = NaN (m,1);
 for i = 1:(m-1);
 F(i) = Sx-(X(i)-X_bar)^2*i*m/(m-i);
 end
 for i = 1:(m-1);
 D(i) = (Sx*(Y_bar-Y(i))-Sxy*(X_bar-X(i)))*m/((m-i)*F(i));
 end
 for i = 1:(m-1);
 test_result(i) = (i*(m-i)*D(i)^2*F(i))/(Sx*Sy-Sxy^2);
 % test_result is the test statistic
 end
 result(k) = max (test_result); % T0 is the maximum value of the test statistics
 end
 result = sort(result);
 A = result(95000);
 % then I sort the result (test) in excel from small to large, find the 9500th number
```
再多说一句，计算机是怎么随机产生正态分布的样本呢？是从随机数通过一系列变换而来的，比如两组uniform distributed随机数可以通过Box-Muller变换生成两组正态分布的数。而随机数则是通过一些随机数生成器(Pseudorandom number generator)生成的。Box 是Pearson的学生，而Pearson是那个众所周知的Pearson的儿子。
