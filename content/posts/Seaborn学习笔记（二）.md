---
title: "Seaborn学习笔记（二）"
date: 2019-08-01T16:49:37+08:00
lastmod: 2021-03-09T11:17:45+08:00
categories: ["大数据与网络安全"]
tags: ["数据可视化", "Seaborn"]
draft: false
summary: "之前学习了解的都是Seaborn的数据绘制时候的美化功能，只是为了使得绘制出的图形更加好看，Seaborn同样提供了许多的用于展示数据相互关系的绘图函数。 #### Seaborn绘制数据集的分布 **1. 绘制单变量分布** 最简便快速查..."
---

之前学习了解的都是Seaborn的数据绘制时候的美化功能，只是为了使得绘制出的图形更加好看，Seaborn同样提供了许多的用于展示数据相互关系的绘图函数。

#### Seaborn绘制数据集的分布

**1. 绘制单变量分布**
最简便快速查看seaborn中单变量分布的是distplot()方法。默认情况下，这将绘制数据的直方图并拟合核心密度估计值（KDE）。

```
x &#x3D; np.random.normal(size&#x3D;100)
sns.distplot(x);
```

![绘制单变量分布图](https://img-blog.csdn.net/20180531163739976?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上边是使用distplot()绘制了一个变量分布的总体密度估计图，我们也可以选择在直方图中显示出具体每个数据在直方图区间的位置

```
#还是使用随机生成的数据，但是设置rug为true
x &#x3D; np.random.normal(size&#x3D;100)
sns.distplot(x, kde&#x3D;False, rug&#x3D;True);
```

我们可以看到具体的数据的密度分布情况，而不是拟合的曲线
![具体的密度分布](https://img-blog.csdn.net/20180531165038920?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

同样的，我们也可以只显示具体数据的密度分布和他的对应拟合曲线

```
x &#x3D; np.random.normal(size&#x3D;100)
sns.distplot(x, hist&#x3D;False, rug&#x3D;True);
```

![密度分布及拟合曲线](https://img-blog.csdn.net/20180531165356769?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

若是我们只是需要绘制数据分布密度的概览，而不是需要绘制直方图，我们可以直接使用Seaborn中提供的kdeplot()函数，上边的distplot()也是通过调用它来显示密度分布拟合曲线的

```
#直接显示密度分布的拟合曲线，而不是绘制直方图
sns.kdeplot(x, shade&#x3D;True);
```

![概率密度拟合曲线](https://img-blog.csdn.net/2018053117105578?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们可以通过bw参数来控制拟合曲线的精细程度，就类似于绘制直方图时的bins参数一样

```
#使用默认的bw参数
sns.kdeplot(x)
#调整bw参数，使得显示出来的密度分布拟合曲线更加接近数据
sns.kdeplot(x, bw&#x3D;.2, label&#x3D;"bw: 0.2")
sns.kdeplot(x, bw&#x3D;2, label&#x3D;"bw: 2")
plt.legend();
```

![调整拟合的精细程度](https://img-blog.csdn.net/20180531171424811?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**2. 绘制双变量分布**
Seaborn提供了用于描述双变量分布情况的绘图函数，可以直观的查看双变量的分布情况。该函数创建一个多面板图形，显示两个变量之间的二元（或关节）关系以及每个单独轴上的单变量（或边际）分布。

```
sns.jointplot(x&#x3D;"x", y&#x3D;"y", data&#x3D;df);
```

![双变量的散点图](https://img-blog.csdn.net/20180531172607660?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

此外Seaborn还提供了直方图中的二元关系图，也常被称为“六边形图”，该图对于较大的数据集效果最好。

```
x, y &#x3D; np.random.multivariate_normal(mean, cov, 1000).T
with sns.axes_style("white"):
    sns.jointplot(x&#x3D;x, y&#x3D;y, kind&#x3D;"hex", color&#x3D;"k");
```

![六边形图](https://img-blog.csdn.net/20180531173025214?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

也可以使用核密度图来估计二元变量的关系，在Seaborn中会以轮廓图的形式显示出来

```
sns.jointplot(x&#x3D;"x", y&#x3D;"y", data&#x3D;df, kind&#x3D;"kde");
```

![核密度图表示二元关系](https://img-blog.csdn.net/20180531173254855?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Seaborn也允许将这类型的图绘制到已经用matplotlib绘制出来的轴上

```
#先用matplotlib绘制出坐标轴
f, ax &#x3D; plt.subplots(figsize&#x3D;(6, 6))
#用Seaborn将需要展示的图绘制到matplotlib轴上
sns.kdeplot(df.x, df.y, ax&#x3D;ax)
sns.rugplot(df.x, color&#x3D;"g", ax&#x3D;ax)
sns.rugplot(df.y, vertical&#x3D;True, ax&#x3D;ax);
```

![效果图展示](https://img-blog.csdn.net/20180531193722663?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以使用kdeplot()中的n_levels参数来控制轮廓的数量，使得图片显示的更加连续或者是稀疏

```
f, ax &#x3D; plt.subplots(figsize&#x3D;(6, 6))
#用n_levels参数控制显示的疏密程度
sns.kdeplot(df.x, df.y, ax&#x3D;ax,n_levels&#x3D;100)
sns.rugplot(df.x, color&#x3D;"g", ax&#x3D;ax)
sns.rugplot(df.y, vertical&#x3D;True, ax&#x3D;ax);
```

![效果图](https://img-blog.csdn.net/20180531194515226?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**3. 展示数据集中的成对关系**
要在数据集中绘制多个成对的双变量分布，可以使用该pairplot()函数。这将创建一个轴矩阵并显示DataFrame中每对列的关系。默认情况下，它也绘制每个变量在对角轴上的单变量分布。

一下使用的是鸢尾花数据集来展示这一效果

```
iris &#x3D; sns.load_dataset("iris")
sns.pairplot(iris);
```

可以看到起使用多个子图来展示两个特征的数据之间的相互关系，同时也在对角线上绘制了每个单变量的分布情况。
![效果图](https://img-blog.csdn.net/20180531195058496?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
