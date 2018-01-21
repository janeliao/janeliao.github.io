---
layout:     post
title:      matplotlib 的动态绘图实现
subtitle:   
date:       2017-12-04
author:     Siyuan
header-img: img/in-posts/coding.jpg
catalog: 	 true
tags:
    - python
    - plot
---

笔者在日常的研究中经常使用 matplotlib 进行数据后处理，这个库的自由度很高，在 seaborn 加持下的出图质量也很高，在处理大量的试验数据时，Python 的应用节省了大量的时间。

最近笔者在进行两个项目，一个是自己开发的数据采集仪[有兴趣请戳 github](https://github.com/siyuanxu/arduino_analog_sensor)，还有一个是岩土方面的离散元数值分析研究（还没出结果，发过 paper 后开源）。在这两个项目中，经常会有对动态数组实时绘图的需求，在查阅资料后整理出了一种比较简便的方法，现在分享一下过程中的错误和最终成功的示例。

### Naive code：

最初我非常的 naive，思路如下：

```python
plt.ion() # interactive mode on
plt.plot(x,y) # plot original data
plt.show() # show the plot
while condition: # loop
	x.append(new_x)
	y.append(new_y) # append new data point
    plt.plot(x,y) # replot data
    plt.pause(1e-5) # pause the plot to show 
```

这种方法在数据量较少，比如全部数据点不超过200个的时候，也可以实现“动态绘图”。但是当数据量较大时，绘图窗口会有严重的卡顿，而且曲线的绘制也有较大的延迟。采集仪项目中，当数据点超过400时，绘图相比于实际电位器的变化已经迟滞了三秒以上。

在查明原因之后，笔者才发现自己写了一段尬 code，这样绘制出的曲线，虽然看起来是一条变化的曲线，但实际上是在每次更新数据之后进行了重复绘制，如果数据总共有 n 个数据点，那么最终将绘制 n 条不断重叠的曲线，难怪界面卡顿那么严重。

那么针对重叠绘图的问题，笔者又尬了一句 plt.clf() 在循环开始的位置。

```python
plt.ion() # interactive mode on
plt.plot(x,y) # plot original data
plt.show() # show the plot
while condition: # loop
    plt.clf()
    x.append(new_x)
    y.append(new_y) # append new data point
    plt.plot(x,y) # replot data
    plt.pause(1e-5) # pause the plot to show 
```
在每次重绘之前，删除当前 plot， 这样一来就不会产生重叠曲线了。但是这仍然是一个错误的示范，因为整个 plot 会在每次更新数据的时候闪现，体验很糟糕。

### Exited code：

通过笔者仔细的钻（stack）研（overflow），最终找到了正确的解决方法，其思路是避免重绘，采用数据更新的方法绘图。代码如下：

```python
import numpy as np
import matplotlib.pyplot as plt

plt.ion() # interactive mode on 
line, = plt.plot(x,y) # plot the data and specify the 2d line
ax = plt.gca() # get most of the figure elements 

while condition:
    x = np.append(x, new_x)
    y = np.append(y, new_y)
    line.set_xdata(x)
    line.set_ydata(y) # set the curve with new data
    ax.relim() # renew the data limits
    ax.autoscale_view(True, True, True) # rescale plot view
    plt.draw() # plot new figure
    plt.pause(1e-17) # pause to show the figure
```

现在，通过上面的代码便可以得到实时更新的，效率较高的动态绘图结果。

![dynamic_plot-sample](http://www.siyuanxu.com/img/in-posts/dynamic-plot.jpg)

PEACE
