---
title: CCNA考试复习总结
date: 2018-11-09 16:49:37
tags: [计算机网络,CCNA,Cisco]
categories: 计算机网络
---

# CCNA考试复习总结

- 设置默认网关：ip default-network [网关IP地址]或者是：ip route [源ip地址]  [ 子 网 掩码] [下一跳ip地址]

- 为需要远程管理的交换机进行配置：
 - ip default-getway [网关地址]
 - interface vlan 1
 - ip address [需要远程管理的交换机的ip] [掩码]
 - no shutdown
 
- 网络不可连接时的检查步骤：
 - 检查网络连接是否正常
 - 验证网卡配置
 - 验证IP配置
 - 检查URL是否正确
 
- 四种不同的线：
 - 同种设备使用交叉反线（crossover）
 - 异种设备间使用直通线（straight through）
 - PC的COM口连接交换机的console口使用全反电缆（rollover cable）
 - 连接带有串口的串行线（serial）
 
- 名词解释：
 - CIR：约定信息速率
 - DCE：数据通讯设备
 - DTE：数据终端设备
 - LMI：本地管理接口
 - PVC：永久虚拟电路
 - SVC：虚电路
 - DLCI：数据链路标识符
 - PVST+：增强的按VLAN生成树
 
- 几种协议的管理距离：
 - RIP：120
 - OSPF：110
 - 静态路由：1
 - 内部EIGRP：90
 - 外部EIGRP：170
 - 内部BGP：200
 - EGP：140
 - EIGRP路由汇总：5
 - 外部BGP：20
 - IGRP：100
 - IS-IS自制系统：115
 - EGP：140
 - 直连网络：0
 
- 组播地址：
 - IPV4：224.0.0.2
 - IPV6：FF02：：2
 
- 本地地址：
 - IPV4:127.0.0.1
 - IPV6： FE80开头 + {本地的MAC地址}
 
- 查看CPU使用率：show process

- 查看trunk端口：
 - show interface trunk
 - show interface switchport
 
- 交换机在trunk模式下的模式：
 - auto：不会主动发送DTP信息
 - on：强制称为trunk，也会主动发送信息
 - desireable：DTP主动模式，发DTP和对方协商

- STP（802.1d）端口状态：
 - 阻塞(blocking)该端口被阻塞，不可以转发或接收数据包
 - 监听(listening)该端口正在等待接收bpdu数据包，bpdu可能告知该端口重新回到阻塞状态
 - 学习(learning)该端口正在向其转发数据库中添加地址，但是，并不转发数据包
 - 转发(forwarding)该端口正在转发数据包
 - 失效(disabled)该端口只是相应网管消息，并且必须先转到阻塞状态

- RSTP（802.1w）端口状态：
 - 禁止（Discarding）
 - 学习（Learning）
 - 转发（Forwarding）

- IPV6任播的三个特点：
 - 数据包传送到该组接口，转发到最近结点
 - 同组中多个接口公用一个地址
 - 发送到任播地址的接口被发送到最近的结点

- Trunk的协议：
 - 通用的802.1q
 - 思科私有的ISL

- IPV4向IPV6转换的方式：
 - 隧道
 - 双栈
 - NAT-PT

- DoD模型：
 - Application
 - Host to Host
 - Internet
 - Network Access

- 查看OSPF链路状态：show ip ospf database 

- OSPF中进程号范围为1-65535

- DHCP中IP地址冲突解决方案：检测到IP地址冲突时，会将冲突的IP地址删除，直到冲突解决之后再将IP地址放回

- 查看端口安全状态：show port-security interface [端口名]

- 链路状态协议的特点：
 - 提供参看拓扑的命令
 - 计算最短路径
 -利用触发更新

- OSPF建立邻居关系的四个条件：
 - 区域ID一致
 - hello，dead时间一致
 - 认证方式和认证密码一致
 - 区域性质一致（都是普通区域或者是末节区域）
 
- 在全局配置模式下查看直连设备：cdp run

- EIGRP查看邻居关系：show ip eigrp neighbors

- PAP采用两次握手机制，CHAP采用三次握手机制

- PVC状态：
 - ACTIVE：成功的端对端电路
 - INACTIVE：表示成功连接到交换机，但是在另一端未检测到DTE
 - 配置的端口被交换机视为无效
 
- SNMPv3比SNMPv2多添加的：
 - 数据完整性
 - 认证
 - 加密
 
- 系统日志存储位置：
 - RAM
 - 控制台终端
 - 系统日志服务器

- 系统日志级别：
 - 0：Emergency
 - 1：Alert
 - 2：Critical
 - 3：Error
 - 4：Warning
 - 5：Notice
 - 6：Informational
 - 7：Debug

- HSRP的virtual mac address以0000.0c07.acxx开头

- IPV6任播的三个特点：
 - 数据包转发到该组接口，转发到最近的结点
 - 同组中多个接口公用一个地址
 - 发到任播地址的包被转发到最近的结点

- IPV6地址：
 - FF01::1 -- all nodes（node-local）
 - FF01::2 -- all routers(node-local)
 - FF02::1 -- all nodes(link-local)
 - FF02::2 -- all routers(link-local)
 - FF02::5 -- OSPFv3 routers
 - FF02::6 -- OSPFv3 designated routers
 - FF02::9 -- RIP
 - FF02::A -- EIGRP routers
 - FF02::B -- Mobile agents
 - FF02::C -- SSDP
 - FF02::D -- all PIM routers
 - FF05::2 -- all routers(site-local)
 - FF05::1:3 -- DHCP


    
