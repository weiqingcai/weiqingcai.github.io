---
title: "Python小知识汇总"
date: 2019-08-01T16:49:37+08:00
lastmod: 2021-03-09T11:17:45+08:00
categories: ["大数据与网络安全"]
tags: ["Python", "小知识"]
draft: false
summary: "## 1.安装包时提示权限拒绝 ### 报错： ``` Could not install packages due to an EnvironmentError: [WinError 5] 拒绝访问。: 'd:\\python36\\sc..."
---

## 1.安装包时提示权限拒绝

### 报错：

```
Could not install packages due to an EnvironmentError: [WinError 5] 拒绝访问。: 'd:\\python36\\scripts\\pip.exe'
```

### 解决：

```
pip install --user
```

## 2.误操作导致pip被卸载：

### 报错：

```
ModuleNotFoundError: No module named 'pip'
```

### 解决：

```
#先执行
python -m ensurepip
#后执行
python -m pip install --upgrade pip
```

## 3.更换Pypi为国内镜像源

### 解决：

```
pip install pip -U (保证pip版本大于10.0.0)
pip config set global.index-url
```

#### 国内镜像：

```
清华：https://pypi.tuna.tsinghua.edu.cn/simple

阿里云：http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

华中理工大学：http://pypi.hustunique.com/

山东理工大学：http://pypi.sdutlinux.org/

豆瓣：http://pypi.douban.com/simple/
```

## 4. 443端口读超时

### 报错：

```
ReadTimeoutError: HTTPSConnectionPool(host&#x3D;'pypi.tuna.tsinghua.edu.cn', port&#x3D;443): Read timed out.
```

### 解决：

```
在下载第三方包时加上参数   --timeout&#x3D;100 （超时时长设置为100秒）

pip install --timeout&#x3D;100
```

## 5.将Jupyter配置成Windows系统服务，开机自启动

#### 解决方案参考自:

[ttps://segmentfault.com/a/1190000014518848?utm_source=tag-newest](https://segmentfault.com/a/1190000014518848?utm_source=tag-newest)

在配置过程中遇到的问题汇总参看另一篇博文：[配置Jupyter服务时遇到的问题](https://blog.csdn.net/wqc_CSDN/article/details/88663563)

## 6. 解决ModuleNotFoundError: No module named ‘numpy.core._multiarray_umath’ 错误

### 解决：

这个大多是因为numpy的版本较低导致，可以通过更新numpy版本来修复

```
pip install --upgrade numpy
```

#### 解决办法参考自：

[https://blog.csdn.net/weixin_41010198/article/details/86738635](https://blog.csdn.net/weixin_41010198/article/details/86738635)
