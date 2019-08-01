---
title: Matplotlib学习笔记（二）
date: 2019-08-1 16:49:37
tags: [数据可视化,Matplotlib]
categories: 大数据与网络安全
---

##Matplotlib面向对象简介
Matplotlib面向对象主要是可以实现更加定制化的绘图，但相比于通过直接使用pyplot而言使用也更加复杂。Matplotlib中大的对象主要分为三个，FigureCanvas（画布），Figure（图），Axes（绘制的坐标轴）。

##一张图中绘制多个子图

![绘制多个子图](https://img-blog.csdn.net/20180524094217309?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1, 100)

# 创建一个图
fig = plt.figure()
#331表示，将这个图分为3*3的格式(即一共可以添加9个子图)，并添加一号子图
ax1 = fig.add_subplot(331)
ax1.plot(x,x)
#334表示向这个3*3的图中添加四号子图
ax2 = fig.add_subplot(334)
ax2.plot(x,np.log(x))

# 对于同一个figure而言，划分成的格式应该一样
# 例如对于上边的figure而言，fig.add_subplot()中的参数只能是“33X”
# 33表示的是将一个figure按照3*3划分，X表示要操作的子图，取值范围为1-9

plt.show()
```
##生成多张图

生成多张图可以通过创建多个Figure对象来实现

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 100)

fig1 = plt.figure()
ax1 = fig1.add_subplot(111)

ax1.plot(x,x)

fig2 = plt.figure()
ax2 = fig2.add_subplot(111)

ax2.plot(x,-x)

plt.show()
```

##网格背景及图例的显示

![网格及图例显示](https://img-blog.csdn.net/20180524102934730?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1,10,1)

plt.plot(x,x*2,label="Normal")
plt.plot(x,x*3,label="Fast")
plt.plot(x,x*4,label="Faster")

#网格的显示可以通过plt.grid(True)来显示
plt.grid(True)
#图例的显示可以通过plt.legend()来显示，需要提前给要绘制的图形添加上label
plt.legend()

plt.show()
```

**[官网对于legend的参数的详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.legend.html?highlight=legend#matplotlib.pyplot.legend)**

**[官网对于grid的参数的详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.grid.html?highlight=grid#matplotlib.pyplot.grid)**

在使用面向对象的绘图中可以通过对Axes的操作来实现图例的显示

```
x = np.arange(1,10,1)

fig = plt.figure()
ax1 = fig.add_subplot(111)
ax1.plot(x,x*2,label="Normal")
ax1.legend()

plt.show()
```

##调整坐标轴范围

调整坐标轴范围可以有多种方式，
```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1,11,1)

fig = plt.figure()
ax1 = fig.add_subplot(111)
ax1.plot(x,x)
```

1.  **方式一**
	```
	#使用plt.axis()函数，四个值分别对应着x轴的起点，终点，y轴的起点，终点
	plt.axis([5,10,4,8])
	```

2.  **方式二**
	```
	#使用plt.xlim()函数
	plt.xlim(xmin=2,xmax=8)
	plt.ylim(ymin=2,ymax=8)
	```
##调整坐标轴的刻度

![调整坐标轴刻度](https://img-blog.csdn.net/20180524162315535?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
import numpy as np
import matplotlib.pyplot as plt


x = np.arange(1,11,1)

plt.plot(x,x)

ax = plt.gca()
#只调整x轴刻度
ax.locator_params('x',nbins=20)

plt.show()
```

调整坐标轴的刻度有两种方式：

1.  通过Axes对象的locator_params()函数（面向对象的方式）

2. 通过matplotlib.pyplot的locator_params()函数

两种方式其实使用的都是locator_params()，其有多个参数可供选择

**常用参数有：**

-  nbins：划分成多少份刻度

- x：表示调整的是x轴刻度

- y：表示调整的是y轴刻度

- 注：若x，y都不填则默认参数为both，即两个轴都会调整

[locator_params()官网参数详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.locator_params.html?highlight=locator_params#matplotlib.pyplot.locator_params)

##绘制样式的美化

之前的绘制都是直接使用原生的样式来绘制，而Matplotlib内建的提供了更多的多种样式可供我们选择。

```
#通过使用该方法可以美化我们绘制出的图形
plt.style.use('ggplot')

#该方法可以查看当前可供选择的style
plt.style.available
```


