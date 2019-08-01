---
title: Seaborn学习笔记
date: 2019-08-1 16:49:37
tags: [数据可视化,Seaborn]
categories: 大数据与网络安全
---
##Seaborn简介
Seaborn是基于matplotlib的Python可视化库。它提供了一个高级界面来绘制有吸引力的统计图形。可以使得数据可视化更加的方便，美观。关于Seaborn的学习，推荐去官网，里边有详细的教程和示例。

##Seaborn常用功能简介

###直接使用Seaborn的美化功能
Seaborn直接提供了多种对matplotlib绘制的图形的美化功能，可以直接使用。

####示例
使用matplotlib绘制图形，

```
#定义一个简单的绘图函数
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
```
**直接使用matplotlib绘图效果**
![直接使用matplotlib绘图](https://img-blog.csdn.net/20180531092416691?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


使用Seaborn提供的默认主题美化图形绘制

```
#只需要在绘制图形之前调用Seaborn的set()函数就可以直接使用其设定好的默认主题进行美化
sns.set()
sinplot()
```
**使用Seaborn默认主题绘制效果**
![使用Seaborn默认主题绘制效果](https://img-blog.csdn.net/20180531092541541?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Seaborn提供了多种风格的主题以供使用，可以通过sns.set_style()来直接使用。

> 提供的直接使用的主题：
> darkgrid，whitegrid，dark，white，和ticks
> 默认为darkgrid

```
#使用Seaborn提供的主题绘制
sns.set_style("ticks")
sinplot()
```
**使用提供的主题‘ticks’绘制效果**
![使用提供的主题绘制](https://img-blog.csdn.net/20180531092820181?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们可以使用sns.despine()函数来去除图片顶部和右侧的坐标轴线，使得图片更加美观

```
sns.set_style('ticks')
sinplot()
sns.despine()
```
**去除顶部和右侧轴线效果**
![去除轴线](https://img-blog.csdn.net/20180531093211224?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Seaborn还提供了更为友好的with用法，可以让我们在一张图片中采用多种的绘图风格，所有在with域中的绘制采用一种风格，而不在with域中的则可以使用另一种风格。

```
#在with中使用一种风格
with sns.axes_style("darkgrid"):
    plt.subplot(211)
    sinplot()
#不在with中的使用另一种风格，我们也可以使用多个with域来使用多种风格
plt.subplot(212)
sinplot(-1)
```
**一张图中使用多种风格效果**
![一张图中使用多种风格效果](https://img-blog.csdn.net/20180531093922383?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

当然了，除了默认的提供的这些主题，我们也可以自己定义一些绘图的风格。通过向axes_style()和set_style()传递参数，可以定义自己的绘图主题。

```
#通过传入字典的方式来定制自己的绘图风格
sns.set_style("darkgrid", {"axes.facecolor": ".9"})
sinplot()
```
我们可以通过直接调用set_style()函数来查看全部可以使用的参数。

**set_style()可用参数**
```
{'axes.axisbelow': True,
 'axes.edgecolor': '.15',
 'axes.facecolor': 'white',
 'axes.grid': False,
 'axes.labelcolor': '.15',
 'axes.linewidth': 1.25,
 'figure.facecolor': 'white',
 'font.family': ['sans-serif'],
 'font.sans-serif': ['Arial',
  'DejaVu Sans',
  'Liberation Sans',
  'Bitstream Vera Sans',
  'sans-serif'],
 'grid.color': '.8',
 'grid.linestyle': '-',
 'image.cmap': 'rocket',
 'legend.frameon': False,
 'legend.numpoints': 1,
 'legend.scatterpoints': 1,
 'lines.solid_capstyle': 'round',
 'text.color': '.15',
 'xtick.color': '.15',
 'xtick.direction': 'out',
 'xtick.major.size': 6.0,
 'xtick.minor.size': 3.0,
 'ytick.color': '.15',
 'ytick.direction': 'out',
 'ytick.major.size': 6.0,
 'ytick.minor.size': 3.0}
```

我们还可以通过修改context参数来缩放图片中的元素。
> Seaborn内置了多种context风格 : paper, notebook, talk, and poster
> 默认使用的是notebook

```
sns.set_context("poster")
sinplot()
```
**设置context效果为poster**
![设置context效果](https://img-blog.csdn.net/2018053110001674?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###使用Seaborn的调色板
Seaborn提供了可以便于使用的调色板，可以方便的作用于数据的可视化。

####示例

**定性（或分类）调色板最适合用于区分不具有固有排序的离散数据块。**

```
#可以使用color_palette()来调用生成一个色板
current_palette = sns.color_palette()
sns.palplot(current_palette)
```
![分类色板](https://img-blog.csdn.net/20180531102235644?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们也可以根据需要来定制一个循环的色板

```
#使用hls的颜色空间，分割成8个颜色
sns.palplot(sns.color_palette("hls", 8))
```
![分割颜色空间](https://img-blog.csdn.net/20180531102839870?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Seaborn还提供了一个提取配对颜色的方法，可以获得两个颜色相近的颜色对

```
sns.palplot(sns.color_palette("Paired",10))
```
![配对颜色效果](https://img-blog.csdn.net/20180531103223177?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**第二大类调色板被称为“顺序”。当数据范围从相对较低或不中断的值到相对较高或有趣的值时，这种颜色映射是适当的**

```
#逐渐变深色板
sns.palplot(sns.color_palette("Blues"))
#若要是使得逐渐变浅，只需要在颜色后边加上‘_r’即可

#由深变浅色板
sns.palplot(sns.color_palette("Blues_r"))
```
![渐变深色板](https://img-blog.csdn.net/20180531104733555?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**第三类调色板被称为“发散”。这些数据用于大数值低值和高值有趣的数据。数据中通常也有明确的中点。例如，如果从某个基准时间点绘制温度变化，最好使用分散的色彩映射来显示相对减少的区域和相对增加的区域。**

用于区分的色板

```
sns.palplot(sns.color_palette("BrBG", 7))
```
![效果图](https://img-blog.csdn.net/20180531143916680?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们也可以自定义这种发散的调色板

**中心颜色为浅色调的色板**

```
#sep控制中间浅色调的宽度
sns.palplot(sns.diverging_palette(10, 220, sep=90, n=7))
```
![中心颜色为浅色调的色板](https://img-blog.csdn.net/20180531144957111?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**中心颜色为深色调的色板**

```
sns.palplot(sns.diverging_palette(255, 133, l=60, n=7, center="dark"))
```
![中心颜色为深色调的色板](https://img-blog.csdn.net/2018053114505044?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


