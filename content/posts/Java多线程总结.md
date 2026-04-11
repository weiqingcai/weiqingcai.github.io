---
title: "Java多线程总结"
date: 2023-03-15T21:00:49+08:00
lastmod: 2023-03-15T23:09:28+08:00
categories: [后端开发]
tags: [java, 多线程]
draft: false
summary: "看的是segmentfault上的一个多线程专栏，做一个个人版的简要汇总"
---

看的是segmentfault上的一个多线程专栏，做一个个人版的简要汇总，原专栏链接如下：
[透彻理解Java并发编程](https://segmentfault.com/blog/ressmix_multithread?page=1)

## 线程模式概述

1. **Single Threaded Execution模式：** 同一时间只能有一个线程访问共享资源，通常使用加排他锁实现，例如通过synchronized关键字确保同一时间只能有一个线程访问
2. **Immutable模式：** 不可变模式，即访问的对象不会暴露任何属性的修改方法，只提供get方法，对象在创建时就确定不变了。（个人感觉在实际使用中完全不变的资源较少，更多的像volatile关键字，任何更新都是透明的）
3. **Guarded Suspension模式：** 等待模式，即在对资源进行操作之前会先去判断是否符合条件，如果不符合则会等待直到条件符合（个人感觉很像是阻塞队列LinkedBlockingQueue的put和take方法，只有当队列未满时put才可以执行，只有当队列不为空时take才可以执行，其余时间都会等到符合条件）
4. **Balking模式：** Balking（退缩不前）该模式也是通过操作资源前进行条件判断来确保程序正确，但和Guarded Suspension模式会一直等到符合条件不同的是，该模式如果不满足操作条件就会直接中止操作并返回，不会一直阻塞在条件判断处。（个人感觉就像在线程中写个死循环，如果符合条件就执行，如果不符合执行条件就sleep一段时间后继续循环）
