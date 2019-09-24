---
title: Seaborn学习笔记（三）
date: 2019-08-1 16:49:37
tags: [数据可视化,Seaborn]
categories: 大数据与网络安全
---

Seaborn中专门提供了针对分类数据的绘图函数，可以很好的展示分类数据的分布情况。
#### 分类数据的可视化

**1. 分类的散点图**

可以使用stripplot()来展示散点图中一个变量是分类的情况。

```
titanic = sns.load_dataset("titanic")
tips = sns.load_dataset("tips")
iris = sns.load_dataset("iris")
sns.stripplot(x="day", y="total_bill", data=tips);
```
可以看到很方便的展示了分类作为变量的时候散点图的分布
![效果图](https://img-blog.csdn.net/20180531200720710)

当是我们可以看到上图中有很多点是相互重复的，我们无法准却的观察每个点的位置，针对这种情况，Seaborn提供了多种解决办法。

1.  第一种是：通过给点加一些随机的偏移量（分类坐标上的），设置jitter参数为True即可

	```
	sns.stripplot(x="day", y="total_bill", data=tips, jitter=True);
	```
	我们可以很明显的看到聚集的点被随机的分散开了
	![效果图](https://img-blog.csdn.net/2018053120110783)
2. 第二种是直接使用 swarmplot()函数来绘制，这种方法使得数据更为比较明显的分散
	
	```
	sns.swarmplot(x="day", y="total_bill", data=tips);
	```
	![效果图](https://img-blog.csdn.net/20180531201518915)

**2. 类别内部数据分类的情况的展示**
Seaborn中提供多种方式来统计一个类别内数据的整体分布情况，主要方式有盒图，小提琴图

1. 使用boxplot()来绘制盒图
	关于什么事盒图以及他的使用方法可以参考这个[关于盒图的介绍](https://baike.baidu.com/item/%E7%9B%92%E5%9B%BE/8594518?fr=aladdin)
	
	```
	sns.boxplot(x="day", y="total_bill", hue="time", data=tips);
	```
	![效果图](https://img-blog.csdn.net/20180531202338399)

2.  使用violinplot()来绘制小提琴图
	关于什么事盒图以及他的使用方法可以参考这个[小提琴图的介绍](http://www.mamicode.com/info-detail-1899511.html)
	
	```
	sns.violinplot(x="total_bill", y="day", hue="time", data=tips);
	```
	![效果图](https://img-blog.csdn.net/20180531202815915)

### 总结
1.  我们主要学习了解了怎么使用Seaborn来实现对matplotlib所绘图形的美化[Seaborn学习笔记](https://blog.csdn.net/wqc_CSDN/article/details/80515920)，包括直接使用其提供的默认主题，自定义主题，通过域的限制来绘制不同风格的主题以及各种色板的使用
2.  了解了Seaborn提供的展示数据间关系的绘图函数[Seaborn学习笔记（二）](https://blog.csdn.net/wqc_CSDN/article/details/80524487)，包括绘制单变量数据的分布，多变量数据的分布以及多种展示数据分布的图
3. 最后学习了展示数据集中数据的分布情况，了解整个数据集的统计信息的展示。
