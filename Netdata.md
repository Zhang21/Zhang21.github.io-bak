---
title: Netdata
date: 2019-02-02 16:09:49
categories: DevOps
tags:
  - Performance
  - Monitoring
---


参考:

- GitHub: <https://github.com/netdata/netdata>
- Netdata: <https://my-netdata.io/>
- Docs: <https://docs.netdata.cloud/>


环境:

- EL7x86_64
- Netdata v1.12




<br/>
<br/>

<!--more-->

<br/>
<br/>




# 介绍


**Netdata** 是可用于系统和应用程序的 分布式(distributed)、实时(real-time)、性能(performance)、健康监控(health monitoring) 的高度优化的监控代理开源程序，用于解决性能问题。

Netdata使用高度交互的Web仪表板，实时提供其运行的系统（包括Web服务器，数据库，应用程序）上发生的所有事情的无与伦比的见解(unparalleled insights)。
它可以自动运行，无需任何第三方组件，也可以集成到现有的监控工具链中(Prometheus, Graphite, OpenTSDB, Kafka, Grafana...)


<br/>


## Why Netdata


Netdata采用完全不同的监控方法。
Netdata 是一个位于你安装系统上的 monitoring agent。它是:

- a **metrics collector**: 收集系统和应用指标
- a **time-series database**: 全部存储在内存中，在运行时不会触及磁盘
- a **metrics visualizer**: 超快速、交互式、现代化，针对异常检测进行了优化
- a **alarms notification engine**: 用于检测性能和可用性问题的高级监控程序




<br/>
<br/>
<br/>




## 如何工作


Netdata 是一个高效，高度模块化的度量管理引擎。其无锁设计使其成为指标并发操作的理想选择。

![](/images/Netdata/it_works.png)




<br/>
<br/>
<br/>




## 资料图


详情: <https://my-netdata.io/infographic.html>

Netdata 功能特色和体系架构的高级预览。

![](/images/Netdata/infoGraphic.png)




<br/>
<br/>
<br/>




## 特性


一般来说:

- **1s granularity**: 所有指标的最高分辨率
- **Unlimited metrics**: 收集所有可用的指标，越多越好
- **1% CPU utilization of a single core**: 超级快，优化好
- **A few MB of RAM**: 默认使用25MB
- **Zero disk I/O**: 运行时，不会加载和保存任何东西，日志除外
- **Zero configuration**: 自动检测所有内容
- **Zero maintenance**: 你只需运行它，它完成剩余的事情
- **Zero dependencies**: 零依赖，自身便包含了Web服务器
- **Scales to infinity**: 你可在servers, containers, VMs and IoTs 上安装它。默认情况下，度量标准不是集中式的，因此没有限制
- **Several operating modes**: 自主主机监控（默认），无头数据收集器，转发代理，存储和转发代理，中央多主机监控

<br>

健康检查和告警:

- **Sophisticated alerting**: 自带上百个告警，开箱即用。支持动态阈值，滞后，警报模板，多种基于角色的通知方法
- **Notifications**: 支持多种通知方式

<br>

集成:

- **time-series dbs**: 可将其指标以相同或更低的方案存档到 `graphite`, `opentsdb`, `prometheus`, `json`, `document DB`



<br/>
<br/>



## 监控什么


Netdata 数据收集是可扩展的，您可以监控任何可以获得指标的内容。它的 `Plugin API` 支持所有的编程语言。

- 为了获得更好的性能，大多数与系统相关的插件都是用C语言编写 (cpu, mem, disk, fs, network...)
- 为了更快的开发和更轻松的贡献，大多数与应用程序相关的插件都是用python编写 (database, web server...)


<br>


**APM (Application Performance Monitoring):**

- **statsd**: Netdata是一个功能齐全的statsd server
- **Go expvar**: 使用expvar包收集由Go语言编写的应用程序公开的度量标准
- **Spring Boot**: 运行Java Spring Boot，这些应用程序使用Spring Boot库中包含的Spring Boot Actuator来公开其指标
- **uWSGI**: 从uWSGI应用程序收集性能指标


<br>


**System Resources:**


- **CPU Utilization**: 总核心和每核心 的CPU使用率
- **Interrupts**: 总核心和每核心 的CPU中断
- **SoftIRQs**: 总核心和每核心 的SoftIRQs
- **SoftNet**: 与网络活动相关的总核心和每核心 的SoftIRQ
- **CPU Throttling**: 收集每个CPU的CPU限制
- **CPU Frequency**: 收集当前的CPU频率
- **CPU Idle**: 收集每个处理器状态所花费的时间
- **IdleJitter**: 测量CPU延迟
- **Entropy**: 随机数池，在密码学中使用
- **Interprocess Communication(IPC)**


<br>


**Memory:**


- **ram**: 收集有关RAM使用情况
- **swap**: 收集有关SWAP使用情况
- **available memory**: 收集可用于用户空间进程的RAM量
- **committed memory **: 收集提交给用户空间进程的RAM量
- **Page Faults**: 收集系统页面错误
- **writeback memory**: 收集系统脏内存和回写活动
- **huge pages**: 收集用于大页面的RAM量
- **KSM**: 收集有关内核相同合并的信息
- **Numa**: 在支持它的系统上收集Numa信息
- **slab**: 收集有关Linux内核内存使用情况


<br>


**Disks:**


- **block devices**: 每个磁盘：I/O, 操作, 积压, 利用率, 空间...
- **BCACHE**: SSD缓存设备的详细性能
- **DiskSpace**: 监视磁盘空间使用情况
- **mdstat**: 软件RAID
- **hddtemp**: 磁盘温度
- **smartd**: 磁盘  S.M.A.R.T 值
- **device mapper**: 命名磁盘
- **Veritas Volume Manager**: 命名磁盘
- **egacli**: 适配器，物理驱动器和电池统计信息
- **adaptec_raid**: 逻辑和物理设备健康指标


<br>


**Filesystems:**


- **BTRFS**: 详细的磁盘空间分配和使用
- **Ceph**: OSD使用，池使用，对象数...
- **NFS file servers and clients**: NFS I/O, cache, read ahead, RPC调用
- **Samba **: SAMBA文件共享的性能指标
- **ZFS**: 详细的性能和资源使用情况


<br>


**Networking:**


- **Network Stack**: 关于网络堆栈(IPv4和IPv6的所有协议)和所有网络接口(每个接口: 带宽，数据包，错误，丢弃)的一切
- **Netfilter**: 关于Netfilter连接跟踪器的一切
- **SynProxy**: 收集有关linux SYNPROXY(DDoS)的性能数据
- **NFacct**: 从iptables收集统计的数据
- **Network QoS**: 实时显示网络tc类的工具
- **FPing**: 测量任意数量的主机之间的延迟和数据包丢失
- **ISC dhcpd**: 池利用...
- **AP**: 收集Linux访问点性能数据(`hostapd`)
- **SNMP**: 监视SNMP设备
- **port_check**: 检查TCP端口的可用性和响应时间


<br>


**Virtual Private Networks:**


- **OpenVPN**: 收集每个隧道的状态
- **LibreSwan**: 收集每个IPSEC隧道的指标
- **Tor**: 收集Tor流量统计信息


<br>


**Processes:**


- **System Processes**: running, blocked, forks, active
- **Applications**: 分析每个进程组的资源情况
- **systemd**: 使用CGROUPS监视systemd服务


<br>


**Users:**


- **Users and User Groups resource usage**: 总结每个用户和用户组资源使用情况
- **logind**: 收集会话，用户和连接


<br>


**Containers and VMs:**


- **Containers**: 使用CGROUPS收集各种容器的资源使用情况
- **libvirt VMs**: 使用CGROUPS收集各种VM的资源使用情况
- **dockerd**: 收集docker健康指标


<br>


**Web Servers:**


- **Apache and lighttpd**: `mod-status`和日志统计信息
- **IPFS**: 带宽，对等连接
- **LiteSpeed**: 读取litespeed rtreport文件以收集指标
- **Nginx**: `stub-status`
- **Nginx+**: 连接到多个nginx_plus服务器以收集实时性能指标
- **PHP-FPM**: 多个实例，每个实例报告连接、请求、性能...
- **Tomcat**: 访问、线程、空闲内存、卷...
- **web server `access.log` files**: 实时提取，Web服务器和代理性能指标，并应用多个运行状况检查...
- **HTTP check**: 检查一个或多个Web服务器以获取HTTP状态代码和返回的内容


<br>


**Proxies, Balancers, Accelerators:**


- **HAproxy**: 带宽，会话，后端...
- **Squid**: 每个服务器显示，客户端带宽和请求、服务器带宽和请求
- **Traefik**: 连接到多个traefik实例以收集API指标
- **Varnish**: 线程，会话，命中，对象，后端...
- **IPVS**: 从Linux IPVS负载均衡器收集指标


<br>


**Database Servers:**


- **CouchDB**: I/O，请求方法，状态代码，任务，副本，每个数据库...
- **MemCached**: 带宽，连接，项目...
- **MongoDB**: 操作，客户端，事务，游标，连接，断言，锁...
- **MySQL and mariadb**: 带宽，查询率，锁，问题，tmp操作，连接，binlog指标，线程，innodb指标...
- **PostgreSQL**: 连接，元组读取/写入/返回，事务，锁定，后端进程，索引，表，预写，后台编写器...
- **Proxy SQL**: 收集后端和前端性能指标
- **Redis**: 操作，命中率，内存，键，客户端，从库
- **RethinkDB**: 连接到多个rethinkdb服务器以收集实时指标


<br>


**Message Brokers:**

- **beanstalkd**: 全局/每个 tube 监控
- **RabbitMQ**: 性能和健康指标


<br>


**Search and Indexing:**

- **ElasticSearch**: 搜索和索引的性能、延迟、计时、集群统计、线程统计...


<br>


**DNS Servers:**


- **bind_rndc**: 解析 `named.stats` 转储文件以收集实时性能指标
- **dnsdist**: 性能和健康指标
- **ISC Bind (named)**: 客户端，请求，查询，更新，失败和每个视图指标
- **NSD**: 查询，区域，协议，查询类型，传输...
- **PowerDNS**: 查询，回答，缓存，延迟...
- **unbound**: 性能和资源使用指标
- **dns_query_time**: DNS查询时间统计信息


<br>


**Time Servers:**


- **chrony**: 频率，最后偏移，RMS偏移，残余频率，根延迟，根分散，偏斜，系统时间
- **ntpd**: 收集统计信息


<br>


**Mail Servers:**


- **Dovecot**: POP3/IMAP 服务器
- **Exim**: 消息队列(电子邮件排队)
- **Postfix**: 消息队列(条目，大小)


<br>


**Hardware Sensors:**


- **IPMI**: 企业硬件传感器和事件
- **lm-sensors**: 温度，电压，风扇，功率，湿度...
- **Nvidia**: 收集Nvidia GPU的信息
- **RPi**: Raspberry Pi温度传感器
- **w1sensor**: 从连接的1-Wire传感器收集数据


<br>


**UPSes:**


- **apcupsd**: 负载，充电，电池电压，温度，效用指标，输出指标
- **NUT**: 负载，充电，电池电压，温度，效用指标，输出指标
- **Linux Power Supply**: 收集Linux上电源驱动程序报告的指标


<br>


**Social Sharing Servers:**


- **RetroShare**: 连接到多个retroshare服务器以收集实时性能指标


<br>


**Security:**

- **Fail2Ban**: 监视fail2ban日志文件以检查所有活动jail的所有禁令


<br>


**Authentication, Authorization, Accounting (AAA, RADIUS, LDAP) Servers:**


- **FreeRadius**: 使用radclient命令提供freeradius统计信息（身份验证，账户统计，代理身份验证，代理统计）


<br>


**Telephony Servers:**


- **opensips**: 连接到opensips服务器（仅限localhost）以收集实时性能指标


<br>


**Household Appliances:**


- **SMA webbox**: 连接到多个远程SMA网箱，以收集光伏（太阳能）发电的实时性能指标
- **Fronius**: 连接到多个远程Fronius Symo服务器，以收集光伏（太阳能）发电的实时性能指标
- **StiebelEltron**: 使用他们的互联网服务网关（ISG网站）从Stiebel Eltron加热系统收集温度和其他指标


<br>


**Game Servers:**

- **SpigotMC**: 使用Minecraft远程控制台监控Spigot Minecraft服务器每秒的滴答数和在线玩家数量


<br>


**Distributed Computing:**

- **BOINC**: 使用远程GUI RPC接口监视本地和远程BOINC客户端软件的任务状态。还为少数错误情况提供警报


<br>


**Media Streaming Servers:**

- **IceCast**: 收集活动源的侦听器数量


<br>


**Monitoring Systems:**

- **Monit**: 收集有关监控目标（文件系统，应用程序，网络）的指标




<br/>
<br/>
<br/>




## 安全设计













<br/>
<br/>
<br/>


## 匿名统计






<br/>
<br/>

---

<br/>
<br/>




# 安装





























