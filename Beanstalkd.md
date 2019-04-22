---

title: Beanstalkd
date: 2018-04-10 16:08:11
categories: Linux
tags: Beanstalkd

---



环境：

- CentOS7.x86_64
- Beanstalkd v1.10



<br>
<br/>

<!--more-->

<br>


# Beanstalkd介绍


Beanstalk，一个高性能、轻量级的分布式内存队列系统。
高性能离不开异步，异步离不开队列，而其内部都是Producer-Comsumer模式的原理。



<br>
<br/>


## Beanstalkd核心概念：


- job(任务)
一个需要异步处理的任务，是beanstalkd中的基本单元，需要放在一个tube中。

- tube(管道)
一个有名的任务队列，用来存储同一类型的job。是producer和consumer操作的对象。

- producer(生产者)
job的生产者，通过`put`命令将一个job放到一个tube中。

- consumer(消费者)
job的消费者，通过`reserve`/`release`/`bury`/`delete`命令来获取或改变job的状态。

<br>

beanstalkd官方状态图：

![](/images/beanstalkd.png)

<br>
<br/>


## Beanstalkd特性


- 优先级
支持0-2^32的优先级。值越小，优先级越高，默认是1024。

- 持久化
可通过**binlog**将job及其状态记录到文件里面。在Beanstalkd下次启动时，可通过读取binlog来恢复之前的job及状态。

- 分布式容错
分布式设计和Memcached类似，beanstalkd个server之间并不知道彼此的存在，都是通过client来实现分布式以及根据tube名称到特定server获取job。

- 超时控制
为了防止某个consumer长时间占用任务但不能处理的情况，beanstalkd为`reserve`操作设置了timeout。如果该consumer不能在指定时间内完成job，job将被迁移会READY状态，供其它consumer执行。




<br>
<br/>

---

<br>



# 安装Beanstalkd


由于epel源可直接安装beanstalkd，So:

```sh
yum install -y beanstalkd


#配置文件
/etc/sysconfig/beanstalkd


#启动
systemctl start beanstalkd

#等同于
#/usr/bin/beanstalkd -l 0.0.0.0 -p 11300 -u beanstalkd
```


<br>
<br/>


## Beanstalk的客户端和管理端


官方没有推出客户端和管理端，GitHub上有一些第三方插件，请自己选择使用。


<br>


### 客户端

- pheanstalkd:  <https://github.com/pda/pheanstalk/>

pheanstalk是一个在PHP中操作beanstalkd的客户端。
具体使用方法参考README。


<br>


### 管理端


- beanstalk_console： <https://github.com/ptrofimov/beanstalk_console>

- aurora: <https://github.com/xuri/aurora>
aurora 是一个基于 Web 的 Beanstalk 消息队列服务器管理工具，单文件无需依赖其他组件，支持管理本地和远程多个队列服务器。































