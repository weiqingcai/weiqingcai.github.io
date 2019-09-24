---
title: Matplotlib学习笔记
date: 2019-08-1 16:49:37
tags: [数据可视化,Matplotlib]
categories: 大数据与网络安全
---

# Matplotlib学习笔记

关于Matplotlib最好的学习笔记就是官网地址，上边有绘制各种图形的示例，建议去官网文档学习，另外喜欢看视频教程的可以去B站。

**[B站关于Matplotlib的视频教程](https://www.bilibili.com/video/av6989413)**

**[Matplotlib官网地址](https://matplotlib.org/index.html)**

## 基本图形的绘制

### 散点图

![散点图](https://img-blog.csdn.net/20180522104247531)

绘制散点图使用matplotlib.pyplot中的scatter()函数

```
import matplotlib.pyplot as plt
import numpy as np

#生成两个随机数组，分别用于对应x轴和y轴
x = np.random.random(100)
y = np.random.random(100)

#绘制散点图的函数，x，y分别对应点的x轴坐标和y轴坐标
plt.scatter(x,y)
#显示散点图
plt.show()
```

**scatter()函数常用参数：**

- s：表示点面积大小，默认为20	

- c：表示颜色，默认为‘b’（蓝色）

-  marker：表示形状，默认为‘o’（圆形），[可选的marker列表](https://matplotlib.org/api/markers_api.html#module-matplotlib.markers)

-  alpha：表示透明度，默认为1，取值范围在0-1之间，0为完全透明，1为不透明

[官网对于scatter参数的详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.scatter.html?highlight=scatter#matplotlib.pyplot.scatter)

### 折线图

![折线图](https://img-blog.csdn.net/20180522113442425)

绘制折线图使用matplotlib.pyplot中的plot()函数

```
import matplotlib.pyplot as plt
import numpy as np

#生成一个等区间的序列，从-10到20，平均分成5份
x = np.linspace(-10,20,5)
y = x**2

plt.plot(x,y)
plt.show()
```
**plot常用参数：**

-  linestyle：表示线类型

- marker：表示点形状，默认是‘o’（圆形）

-  c：表示颜色，默认是‘b’（蓝色） 

[官网对于plot参数的详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.plot.html?highlight=plot#matplotlib.pyplot.plot)

当x轴是以时间为单位时，需要使用plot_date()函数来绘制，并设置linestyle（线型）参数为**‘--’**才会显示为折线图

![日期折线图](https://img-blog.csdn.net/20180522155508435?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
import matplotlib.dates as mdates
import matplotlib.pyplot as plt
import numpy as np
#加载数据
date, openPrice, closePrice = np.loadtxt('price_two_month.csv',
                                         delimiter=',',
                                         converters={5: mdates.strpdate2num('%Y-%m-%d')},
                                         skiprows=1,
                                         usecols=(5, 8, 11),
                                         unpack=True,
                                         encoding='utf-8')
#使用plot_date()函数来绘制折线图，并设置线型为‘-’
plt.plot_date(date, openPrice,'-')
plt.show()
```
### 条形图

![条形图](https://img-blog.csdn.net/20180522161600757)

绘制条形图需要使用matplotlib.pyplot中的bar()函数

```
import matplotlib.pyplot as plt
import numpy as np

N = 10

y = np.random.random(10)
index = np.arange(N)
#left参数表示下边的索引值，height表示对应于索引值的条形图的高度
p1 = plt.bar(left=index,height=y)

plt.show()
```
**常用参数：**

- c：表示颜色，默认值为‘b’（蓝色）

- width：表示宽度，默认值为0.8

[官网对于bar参数的详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.bar.html?highlight=bar#matplotlib.pyplot.bar)

若是需要绘制水平的条形图，需要使用barh()函数，并修改其中的参数

```
import matplotlib.pyplot as plt
import numpy as np

N = 10

y = np.random.random(10)
index = np.arange(N)
#left需要设置为0，添加bottom参数并设置为index，同时height参数修改为width
p1 = plt.barh(left = 0,bottom=index,width=y)

plt.show()
```
![横向条形图](https://img-blog.csdn.net/20180522162814774?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###直方图

绘制直方图需要使用hist()函数
  ![直方图](https://img-blog.csdn.net/20180522190721463?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
import matplotlib.pyplot as plt
import numpy as np

#数据的均值
mu = 100
#数据的方差
sigma = 20
#构造一组数据
x = mu+sigma*np.random.randn(2000)
#绘制直方图x为数据，bins表示分的组数，normed表示是否进行标准化，即是否用频率来表示。而不是具体的数值
plt.hist(x,bins=50，normed=True)

plt.show()
```

**常用参数：**

-  c：表示颜色

-  normed：表示是否进行标准化

-  bins：表示组数 

[官网对于hist参数的详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.hist.html?highlight=hist#matplotlib.pyplot.hist)

还可以在此基础上绘制2D的直方图,即双变量的直方图，通过颜色的深浅来表示频率的高低

![双变量的直方图](https://img-blog.csdn.net/20180522191930944)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.random.randn(500)+20
y = np.random.randn(500)-30

plt.hist2d(x,y,bins=40)
plt.show()
```
### 饼图

绘制饼图需要使用pyplot中的pie()函数

![饼图](https://img-blog.csdn.net/20180524083843354)

```
import matplotlib.pyplot as plt
//设置每个标签
lables = 'A','B','C','D'
//每个标签所对应的数值
fracs = [15,28,20,40]
//若需要显示为正圆，需要设置x，y轴比例一致
plt.axes(aspect=1)
//设置相关参数
plt.pie(x=fracs,labels=lables,autopct='%.0f%%',explode=explode,shadow=True)
plt.show()
```
**常用参数：**

-  autopct：表示是否显示各部分所占的比例，使用python中的格式化数据显示

- explode：表示是否突出显示某一部分 ，0表示不突出显示

- shadow：表示是否添加阴影

[官网对于pie的参数详解](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.pie.html?highlight=pie#matplotlib.pyplot.pie)
