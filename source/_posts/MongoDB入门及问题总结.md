---
title: MongoDB使用入门及问题总结
date: 2018-11-09 16:49:37
tags: [MongoDB,数据库]
categories: 杂学
---

# MongoDB使用入门及问题总结

最近学习Python爬虫开发时需要使用MongoDB数据库，为此做个使用入门的介绍和所遇问题的总结。

## 关于MongoDB

关于MongoDB的简介可以直接去官网查看，主要和传统的MySQL，SQL Server这些数据库的不同之处在于它是NoSql型数据库，即非关系型数据库。不同于MySQL中的数据都是一张张的关系表结构，MongoDB中的数据并不是以相互之间的关系表来存储的，所以这种数据的存储方式更适合复杂的爬虫环境。

### **参考文档：**

### **[MongoDB官网](https://www.mongodb.com/)**

### **[MongoDB中文社区](http://www.mongoing.com/)**

### **[MongoDB官方中文文档](http://www.mongoing.com/docs/)** 

### **[MongoDB的入门视频教程](https://www.bilibili.com/video/av13384870)**

## MongoDB的下载和安装

关于下载和安装直接按照官网的流程来即可

![官网下载社区版](C:\Users\75993\Pictures\Saved Pictures\study124.PNG)

选择对应操作系统的版本，下载下来直接安装即可。

## **MongoDB的遇到的问题**

### **一 .       安装官方下载的msi的时候始终提示安装失败**

**解决办法：**

在安装程序最后一步的时候不勾选左下角的mongodb-compass 选项

### **二.   配置MongoDB为window服务时失败**

**解决办法：**

删除用于存放数据库的文件夹中的mongod.lock和storage.bson 文件，在重新启动

### **相关参考文章**

[mongodb安装及100报错 ](https://blog.csdn.net/win7583362/article/details/78342151)

[关于安装MongoDB的过程与错误48 100的解决方法 ](https://blog.csdn.net/qq_33222328/article/details/53433848)