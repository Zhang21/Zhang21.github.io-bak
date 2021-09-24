---
title: SRE
date: 2021-08-24 09:49:26
categories:
  - SRE
tags:
  - sre
  - devops
mathjax: true
---


参考:

- School Of SRE: <https://linkedin.github.io/school-of-sre/>


<br/>
<br/>

---

<!--more-->

<br/>
<br/>




# 介绍

站点可靠性工程师(Site Reliability Engineers, SREs)位于软件工程和系统工程的交叉点。

<br>

课程Level：

- 1xx: freshman
- 2xx: sophomore
- 3xx: junior
- 4xx: senior

<br>

目前涵盖的SRE课程：

- Level 101
  - Fundamentals Series
    - Linux Basics
    - Linux Networking
  - Python和Web
  - Data
    - Relational databases
    - NoSQL
    - Big Data
  - Systems Design
    - Scalability
    - Availability
    - Fault Tolerance
  - Metrics and Monitoring
  - Security
- Level 102
  - Linux Intermediate
    - Package Management
    - Storage Media
    - Archiving and Backup
    - VIM
    - BASH
  - Linux Advanced
    - Containers and orchestration
    - System Calls and Signals
  - Networking
    - Security
    - Scale
    - RTT
    - Infrastructure Services
  - System Design
    - Large System Design
    - Scaling
    - Scaling Beyond the Data Center
    - Resiliency
  - System troubleshooting and performance improvements
  - CI/CD



<br/>
<br/>







# 基础系列

Fundamentals Series


<br/>


## Linux基础知识

Linux Basics: <https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/>


<br/>
<br/>


### 本课程的期望

本课程分为三部分：

- Linux操作系统基础
- Linux基本命令
- Linux系统管理


<br/>
<br/>


### Linux介绍

内核(kernel)是一个操作系统最重要的部分，它执行进程管理，内存管理，文件系统管理等重要功能。

Windows操作系统基于Windows NT kernel。

Linux操作系统基于Linux Kernel(免费且开源)。基于Linux的操作系统包含Linux Kernel, GUI/CLI, system libraries, system utilities.

Linux是一个内核，而不是一个完整的操作系统。Linux Kenel与GNU相结合来创建完整的操作系统。因此，基于Linux的操作系统也成为GNU/Linux系统。


<br/>
<br/>


#### Linux发行版

Linux发行版是基于Linux内核和包管理系统的操作系统。软件通常是适配各个发行版的特定格式。

流行的Linux发行版：

- Fedora
- Ubuntu
- Debian
- Centos
- Red Hat Enterprise Linux
- Suse
- Arch Linux

<br>

| 包管理 | 发行版 | 包管理 |
| - | - | - |
| Debian Style(`.deb`) | Debian, Ubuntu | APT |
| RedHat style(`.rpm`) | Federa, CentOS, RedHatEL | YUM |

<br>

linux架构图。

![linux架构图](/images/SRE/linux-architecture.png)

- The Linux kernel is monolithic in nature.
- System calls are used to interact with the Linux kernel space.
- Kernel code can only be executed in the kernel mode. Non-kernel code is executed in the user mode.
- Device drivers are used to communicate with the hardware devices.


<br/>
<br/>


#### Linux操作系统用例

广泛使用Linux内核操作系统的有：

- Personal computers
- Servers
- Mobile phones
- Embedded devices
- Satellites
- Network devices



<br/>
<br/>


#### Shell与Terminal

Shell是一个从用户处获取命令的程序，并将其提供给操作系统进行处理。Shell是CLI的示例。Bash是Linux服务器上最受欢迎的shell程序。

Terminal是一个打开窗口让你与Shell交互的程序。



<br/>
<br/>



### 命令行基础

docs: <https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/>


<br/>
<br/>


#### 命令

命令(command)是告诉操作系统执行特定工作的程序。程序在Linux中存储为文件。因此，命令也是存储在磁盘某处的文件。

使用`--help`或`man`来获取帮助信息。


<br/>
<br/>


#### 文件系统组织

File System Organization

Linux文件系统具有分层(hierarchical)(或树形(tree-like))结构，最高级别的目录称位根目录`/`。

![](/images/SRE/liunx-filesystem-arch.png)

<br>

特定目录的作用：

- `bin`: The executable program of most commonly used commands reside in bin directory
- `sbin`: This directory contains programs used for system administration.
- `home`: This directory contains user related files and directories.
- `lib`: This directory contains all the library files
- `etc`: This directory contains all the system configuration files
- `proc`: This directory contains files related to the running processes on the system
- `dev`: This directory contains files related to devices on the system
- `mnt`: This directory contains files related to mounted devices on the system
- `tmp`: This directory is used to store temporary files on the system
- `usr`: This directory is used to store application programs on the system


<br/>
<br/>


#### 导航文件系统的命令

Commands for Navigating the File System

```yaml
ls: list files and directories
pwd: print working directory
cd: change directory
```


<br/>
<br/>


#### 操作文件的命令

Commands for Manipulating Files

```yaml
touch: create new file
mkdir: create new directories
rm: delete files and directories
cp: copy files and directories
mv: move files and directories
```


<br/>
<br/>


#### 查看文件的命令

Commands for Viewing Files

```yaml
cat: concatenate files and print on the standard output
head: output the first part of files
tail: output the last part of files
more: file perusal filter for crt viewing
less: opposite of more
```


<br/>
<br/>


#### echo命令

```yaml
echo: display a line of text
```


<br/>
<br/>


#### 文本处理命令

Text Processing Commands

```yaml
grep: print lines matching a pattern
sed: stream editor for filtering and transforming text
sort: sort lines of text files
```


<br/>
<br/>


#### IO重定向

每个打开的文件都会分配文件描述符(file descriptor)。文件描述符是系统中打开的文件的唯一标识符。

有三个默认的文件打开：stdin, stdout, stderr

Everything is a file in linux.

```yaml
`>`: 重定向操作符
`|`: 管道符
```



<br/>
<br/>



### 服务器管理

Server Administration: <https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/>


<br/>
<br/>


#### 多用户操作系统

Multi-User Operating Systems

Linux操作系统本质上是多用户，因为它允许多个用户同时访问系统。多个用户通过SSH远程登录并。

由于Linux支持多个用户，我们需要一种方法可以保护用户彼此。用户不应该能够访问和修改其它用户。


<br/>
<br/>


#### 用户和组管理

User/Group Management

- Users in Linux has an associated user ID called UID attached to them.
- Users also has a home directory and a login shell associated with them.
- A group is a collection of one or more users. A group makes it easier to share permissions among a group of users.
- Each group has a group ID called GID associated with it.
- root user or superuser is the most privileged user with unrestricted access to all the resources on the system. It has UID 0

<br>

```yaml
id: print real and effective user and group IDs
whoami: print effective userid

#用户和组相关联的重要文件
/etc/passwd: Stores the user name, the uid, the gid, the home directory, the login shell etc
/etc/shadow: Stores the password associated with the users
/etc/group: Stores information about different groups on the system
```


<br/>
<br/>


#### 用户管理相关命令

Important commands for managing users

```yaml
useradd: Creates a new user

passwd: Adds or modifies passwords for a user

usermod: Modifies attributes of an user

userdel: Deletes an user
```

```bash
cat /etc/shadow

apache:!!:18759::::::
"!!" in an account entry in shadow means the account of an user has been created, but not yet given a password.
```


<br/>
<br/>


#### 组管理相关命令

Important commands for managing groups

```yaml
groupadd: Creates a new group
groupmod: Modifies attributes of a group
groupdel: Deletes a group
gpasswd: Modifies password for group
```


<br/>
<br/>


#### 成为超级用户

Becoming a Superuser

```yaml
sudo: The sudo command allows a user to run commands with the security privileges of the root user
su: switch users in Linux
```


<br/>
<br/>


#### 文件权限

File Permissions

Linux系统上，每个文件和目录都分配了对应属主(owner)、组(group)、其他人(other)的对应权限。

![](/images/SRE/file-permissions.jpg)

<br>

文件权限的一些命令:

```yaml
chmod: modify files and directories permissions
chown: change the owner of files or directories
chgrp:  change the group ownership of files or directories
```


<br/>
<br/>


#### SSH命令

ssh命令用于登录远程系统，传输文件，执行命令等。

<br>

使用SSH进行无密码认证(Passwordless Authentication Using SSH)。

在远程主机上执行命令。

在主机之间传输文件。


<br/>
<br/>


#### 包管理

Package Management

包管理是系统上安装和管理软件的进程。

- yum(dnf)
- apt


<br/>
<br/>


#### 进程管理

Process Management

```yaml
ps: know the information of a process or list of processes
top: show information about Linux processes running on the system in real time
```


<br/>
<br/>


#### 内存管理

Memory Management

```yaml
free: display the memory usage of the system
vmstat: display the memory usage along with additional information about io and cpu usage
```


<br/>
<br/>


#### 检测磁盘

Checking Disk Space

```yaml
df(disk free): display the free and available space for each mounted file system
du(disk usage): display disk usage of files and directories on the system

fdisk: manipulate disk partition table
```


<br/>
<br/>


#### Daemons

以后台进程运行的程序称位daemon。通常来讲，守护进程名称会以d结尾，如sshd, httpd等。我们无法与守护进程进行交互，因为它们在后台运行。


<br/>
<br/>


#### Systemd

systemd是Linux操作系统上的系统和服务管理程序。systemd units是Systemd的构成组件。这些单元由配置文件表示(`/usr/lib/systemd/system`)。

service units以`.service`文件扩展结尾。`systemctl`命令用于管理由systemd管理的服务。


<br/>
<br/>


#### Logs

```
/var/log/
dmesg: kernel logs
```



<br/>
<br/>
<br/>




## Git


<br/>

### git基础知识

git: <https://linkedin.github.io/school-of-sre/level101/git/git-basics/>


```bash
# 创建一个git仓库
git init

# 一个空的git仓库
# 所有信息都在.git目录下
ls .git/
branches  config  description  HEAD  hooks  info  objects  refs

git status

git add

# Commit is a snapshot of the repo
git commit -m

git log --oneline --graph

git cat-file -p commit-id

ls .git/objects/xx/xxxx

# 版本控制
git checkout

cat .git/HEAD
echo commit-id > .git/refs/heads/master
```

All commits are stored as a tree like data structure internally by git. That means there can be two or more children commits of a given commit.

Everything in git is an object.



<br/>
<br/>



### 分支

Working With Branches: <https://linkedin.github.io/school-of-sre/level101/git/branches/>

在同一个仓库中，不同的功能同时并行工作，使用分支。

在内部，git只是一颗提交树。分支名称(人类可读)是指向树中那些提交的指针。我们使用各种git命令来处理树结构和引用。git相应地修改我们仓库的内容。

如果仓库中有很多分支，那么分支合并可能会出现很多合并提交信息。这样，与单一的开发历史相比，看起来很难看。所以，让我们来看看另外的方法(rebase)。

```bash
git branch

git checkout -b

git checkout

git merge
git rebase
git merge --rebase
```



<br/>
<br/>



### 钩子

github and hooks: <https://linkedin.github.io/school-of-sre/level101/git/github-hooks/>

git还有一个很好的特性叫做钩子(hooks)。钩子基本上是脚本，当某个事件发生时会被调用。

钩子的名称是解释性的。当你想在某个事件发生时做某些事情，这些钩子很有用。

```bash
git remote

git pull

git push

# hooks
ls .git/hooks/
applypatch-msg.sample     fsmonitor-watchman.sample pre-applypatch.sample     pre-push.sample           pre-receive.sample        update.sample
commit-msg.sample         post-update.sample        pre-commit.sample         pre-rebase.sample         prepare-commit-msg.sample
```



<br/>
<br/>
<br/>



## Linux网络


<br/>


### 网络基础

Linux Networking Fundamentals: <https://linkedin.github.io/school-of-sre/level101/linux_networking/intro/>

当你在浏览器中访问`linkedin.com`时，会发生什么？本课程遵循`TCP/IP`协议栈的流程。

更具体地说，涵盖了：

- 应用层协议: DNS, HTTP
- 传输层协议: UDP, TCP
- 网络层协议: IP
- 数据链路层协议



<br/>
<br/>



### DNS

dns: <https://linkedin.github.io/school-of-sre/level101/linux_networking/dns/>

域名是网站的简单人类可读的名称。互联网只能理解IP地址，但由于记忆不想关的号码不实用，因此使用域名。这些域名被DNS基础架构转换为IP地址。

当在浏览器打开`www.linkedin.com`时，浏览器尝试讲域名转换为IP地址，这个过程被称为DNS解析。

```
域名解析 -> 查看本地缓存 -有> 返回ip
                         -无> 请求DNS服务器 -> 返回ip

www.linked.com -> . -> .com -> linkedin.com
. root domain
```

```bash
# Linux操作系统文件
/etc/nsswitch.conf

/etc/hosts

/etc/resolv.conf

# 命令
nslookup

dig

dig linkedin.com
linkedin.com.           33      IN      A       13.107.42.14
```

域名解析结果有五个字段:

- 第一个字段：请求
- 第二个字段：TTL
- 第三个字段：信息类别
- 第四个字段：记录类型
  - A: IPv4
  - AAAA: IPv6
  - NS: authoritative nameserver
  - TXT
  - PTR
  - MX
  - CNAME: 别名
- 第五个字段： 响应

<br/>

Applications in SRE role

SRE可以从DNS获取的一些常见的解决方案：

- 自己的DNS基础架构
- DNS可用于服务发现
- 云服务商CDN服务商提供的DNS
- 在分布式系统中，DNS可用于使客户端将IP地址更靠近其位置，以提供更快的响应
- DNS验证和欺骗(https, dnssec)
- 陈旧的DNS缓存可能会是一个问题
- DNS负载均衡和服务发现也必须了解TTL，并且只有在等待TTL后才能从池中删除、更改DNS记录



<br/>
<br/>



### UDP

udp: <https://linkedin.github.io/school-of-sre/level101/linux_networking/udp/>

UDP是一个传输层协议。DNS是一个运行在UDP之上的应用层协议。

传输层确保DNS请求到达DNS服务端，并确保响应到达DNS客户端。当客户端进行DNS请求时，在填充必要的应用程序有效应用载荷(application payload)后，它通过sendto系统调用将有效应用载荷传递给内核。内核选择一个大于1024的随机端口作为源端口，DNS服务器端口(默认53)作为目的端口，并发送包到更低的层。当DNS服务端的内核接收到这个包，它检查它的端口号，并且数据包队列到DNS程序的应用缓冲取(application buffer)，该过程调用recvfrom系统调用并读取数据包。这个内核的过程被称为多路复用(multiplexing)和多路分用(demultiplexing)。多路复用和多路分解有传输层完成。

- 多路复用：从多个应用程序组合数据包到相同的底层
- 多路分用：将数据包从单个下层分离到多个应用程序

UDP是最简单的传输层协议之一，它只有多路复用和多路分用。另一个常见的传输层协议是TCP，它是可靠的通信、流量控制、拥塞控制。UDP旨在轻便，处理略微开销的通信。所以UDP在多路复用和分用之外不做任何事。



<br/>
<br/>


### HTTP

HTTP: <https://linkedin.github.io/school-of-sre/level101/linux_networking/http/>

Linked的html页面是由浏览器渲染的HTTP协议提供的。浏览器给前面解析的IP服务器发送HTTP请求。请求包含方法、路径和参数等。

HTTP is called stateless protocol. HTTP是不安全的，使用HTTPS。

HTTPS必须提供客户端和服务段直接的数据的服务器标识和加密。服务器管理员必须生成私有公钥对和证书请求。该证书请求必须由证书机构签名，证书机构将证书请求转换为证书。证书有关于服务器的详细信息（如域名，过期时间等）。私钥是服务器的秘密，丢失私钥就失去了服务器的提供的信任。

当客户端连接，客户端发送HELLO。服务器将其证书发送给客户端。如果证书未过期，客户端检查证书的有效性，如果它由受信任的机构签名，并且证书中的主机名与服务器相同。这个验证过程确保服务器的正确性，避免网络钓鱼。一旦验证，客户端通过协商与服务器的公钥加密协商，客户端通过服务器协商对称密钥和密码。除了具有私钥的服务器之外，没有人可以理解这些数据。一旦协商完成，对称密钥和算法将用于进一步加密，这可以仅由客户端和服务端从其上解密，因为只有它们知道对称密钥和算法。



<br/>
<br/>



### TCP

TCP: <https://linkedin.github.io/school-of-sre/level101/linux_networking/tcp/>

TCP是一种传输层协议，它保证了可靠性、流量控制和拥塞控制。通过使用序列号保证可靠的交付。

TCP连接通过三次握手建立。客户端发送带有计划使用的起始序列号的SYN包，服务器确认SYN包并以其序列号发送SYN包。一旦客户端确认了SYN数据包，就建立了连接。一旦有关方收到该序列的确认，从此处传输的每个数据包被认为是可靠的。

![](/images/SRE/tcp-established.png)

TCP通过四次挥手断开连接。关闭连接时，客户端、服务端调用close syscall。假设客户端想要断开连接。客户端的内核将发送FIN包给服务器。服务器的内核无法关闭连接，知道服务器应用程序调用close syscall。一旦服务器应用程序调用close，服务器同样发送一个FIN包，客户端进入时间等待状态(2MSS, 120s)，以便在该时间段中重复使用此套接字以防止由于杂散陈旧数据包引起的认证TCP状态损坏。

![](/images/SRE/tcp-closed.png)



<br/>
<br/>



### 路由

IP Routing and Data Link Layer: <https://linkedin.github.io/school-of-sre/level101/linux_networking/ipr/>

我们将挖掘离开客户端的数据包到达服务器，反之亦然。当数据包到达IP层时，传输层填充源端口和目的端口。IP网络层填充目标IP，然后查找路由表上对应的路由。

路由表中`0.0.0.0`的网关，意味着不需要网络层hop来发送数据包。源和目的都在同一个网络中。内核必须弄清楚目的地的MAC并适当地填充源和目的MAC，并将数据包发送出来，以便它到达目的地而没有中间的任何一层跳跃。







<br/>
<br/>

---

<br/>
<br/>







# Python和Web










<br/>
<br/>

---

<br/>
<br/>






# 数据


<br/>


## 关系型数据库

Relational Databases

<br/>


### 关键概念

Key Concepts: <https://linkedin.github.io/school-of-sre/level101/databases_sql/concepts/>

- 关系型数据库用于存储数据。即使是文件也可用于存储数据。但关系型数据库设计具有特定的目标:
  - 效率
  - 易于访问和管理
  - 有组织的
  - 处理数据直接的关系(表示为表)
- 事务(Transaction): 可以包含多个语句的工作单位，一起执行
- ACID属性
  - 原子性(Atomicity): 每个事务都是原子的（完成成功或完全失败）
  - 一致性(Consistency)：事务仅导致有效状态
  - 隔绝性(Isolation)：每个事务都在并发系统内安全地单独执行
  - 持久性(Durability)：保证已提交的事务将永久存在
- CRUD操作
  - Create
  - Read
  - Update
  - Delete
- 查询的四种类型
  - DDL(definition)
  - DML(manipulation)
  - DCL(control)
  - TCL(transactions)
- 约束(Constraint)：可以存储的数据的规则
- 索引(Index)：大多数索引使用`B+tree`结构
- 联结(Join)：多表关联
- 访问控制(Access control)


<br/>
<br/>


#### 流行的数据库系统

- 商业版
  - Oracle
  - MS SQL Server
  - IBM DB2
- 开源版
  - MySQL
  - MariaDB
  - PostgreSQL


<br/>
<br/>


### MySQL


<br/>
<br/>


### InnoDB


<br/>
<br/>


### 备份和还原

Backup and Recovery


<br/>
<br/>


### MySQL副本

MySQL Replication


<br/>
<br/>


### 查询语句


<br/>
<br/>


### 查询优化

Query Performance: <https://linkedin.github.io/school-of-sre/level101/databases_sql/query_performance/>

查询性能是关系型数据库一个非常重要的方面。重要任务是识别慢查询，并尝试通过重写它们或在涉及的表中创建适当的索引来提高其性能。




<br/>
<br/>



## NoSQL

通常，术语NoSQL数据库指的是任何非关系型数据库。术语NoSQL(non SQL)指的是not only SQL。无论那种方式，大多数人都同意NoSQL数据库是存储关系表以外格式的数据库。

常见的误解是NoSQL数据库或非关系型数据库不会很好地存储关系数据。NoSQL数据库可以存储关系数据，只是与关系型数据库不同地存储。实际上，与SQL数据库相比，许多查找在NoSQL数据库中的建模关系更容易，因为相关数据不必在表之间拆分。

随着敏捷开发的兴起，NoSQL数据库专注于伸缩(scaling)，高性能，同时允许频繁地应用更改，并轻松地进行编程。

NoSQL数据库的类型:

- 文档型(Document)：存储在文档中的数据(BSON)类似与JSON对象。每个文档包含一组键值对。值通常可以是各种类型：字符串、数字、布尔、数组和对象，它们通常与开发人员对齐的结构与代码一起使用。有点包括直观的数据模型和灵活模式。由于值类型很宽泛和强大的查询语言，文档数据库非常适合各种用例，可用作通用数据库。它可以快速和无限的横向扩展。
  - MongoDB
  - Couchbase
- 键值对(Key-Value)：简单的键值数据库。它非常适合你需要存储大量数据但不需要执行复杂查询来检索它的情况。
  - Redis
  - DynamoDB
- 宽列(Wide-column)：数据存储在表、行和动态的列中。宽列存储提供了对关系数据库的大量灵活性，因为每行都不需要具有相同的列。宽列存储通常用于用户配置文件和数据的网络数据。
  - HBase
  - Cassandra
- 图(Graph)：数据存储在节点(node)和边缘(edge)中。节点通常存储有关人员、地方和事物的信息，而边缘存储节点之间的关系的信息。
  - Neo4j

<br/>

**比较**

| 类型 | 性能 | 扩展性 | 灵活性 | 复杂性 | 功能性 |
| - | - | - | - | - | - |
| k-v | high | high | high | none | Variable |
| document | high | Variable(high) | high | low | Variable(low) |
| Column DB | high | high | moderate | low | minimal |
| Graph | Variable | Variable | high | high | Graph theory |

<br>

**SQL和NoSQL的不同之处**

| 项 | SQL数据库 | NoSQL数据库 |
| - | - | - |
| 数据存储模型 | 具有固定行和列的表 | Document: JSON文档 <br> K-V: 键值对 <br> Wide-column：具有行和动态列的表 <br> Graph：节点和边缘 |
| 首要目标 | 一般用途 | Document：一般用途 <br> K-V：具有简单查询的大量数据 <br> Wide-column：具有可预测查询模式的大量数据 <br> Graph：在关联的数据之间分析和遍历关系 |
| 模式(Schema) | 严格且生硬(Rigid) | 灵活 |
| 扩展性 | 垂直 | 水平 |
| 多记录ACID事务 | 支持 | 大多数不支持，MongoDB支持 |
| 联结 | 通常需要 | 通常不需要 |
| 数据到对象映射 | 需要ORM(object-relational mapping) | 大多不需要 |

<br>

**优点**

- 灵活的数据模型
- 水平伸缩
- 快速查询
- 开发者生产力



<br/>
<br/>


### 关键概念

Key Concepts: <https://linkedin.github.io/school-of-sre/level101/databases_nosql/key_concepts/>


<br/>


#### CAP定理

CAP(Consistency, Availability, Partition Tolerance)

- **一致性**(Consistency)：系统在执行后的一致性
- **可用性**(Availability)：由于软件或硬件故障，系统如何响应不同不系统的功能丢失
- **分区容错**(Partition Tolerance)：系统在网络分区中继续操作的能力

大规模应用中越来越多的用例倾向于重视可靠性，这意味着可用性和冗余比一致性更有价值。结果，这些系统致力于满足ACID属性。通过对一致性要求松开来实现这一目标，即最终一致性(Eventual Consistency)。

最终一致性意味着所有读者都会看到写入，随着时间流逝，在稳定状态下，系统最终将返回最后一个写入值。因此，当更新进行时，客户端可能面临数据不一致状态。

例如，在复制的数据库更新中可以转到一个几点，该节点将最新版本复制到包含修改数据集的副本的所有其它节点，使得副本节点最终将具有最新版本。

NoSQL系统支持不同级别的最终一致性模型：

- **Read Your Own Writes Consistency**：客户端将在编写后立即看到它们的更新。
- **Session Consistency**：客户将在会话范围内看到对数据的更新。
- **Casual Consistency**：按照系统的每个过程都可以看到潜在的因果关系相关的写操作。

<br/>

| Choice | Traits | 示例 |
|  -  |  -  |  -  |
| Consistency + Availability | 2-phase commits <br/> Cache invalidation protocols | Single-site databases Cluster databases <br/> LDAP <br/> xFS file system |
| Consistency + Partition tolerance | Pessimistic locking <br/> Make minority partitions unavailable | Distributed databases Distributed locking Majority protocols |
| Availability + Partition tolerance | expirations/leases <br> conflict resolution optimistic | DNS <br/> Web caching |


<br/>
<br/>


#### 分布式系统中的数据版本

Versioning of Data in distributed systems

当数据跨界点分布时，它可以同时在不同的节点上进行修改。对并发更新的冲突解决而产生的问题。一些流行的冲突解决机制：

- **Timestamps**：这是最明显的解决方案。根据时间序列对更新进行排序，然后选择最新的更新。这依赖于基础设施的不同部分的时钟同步。当系统遍布不同的地理位置时，这将会变得更加复杂。
- **Optimistic Locking**：给每个数据更新关联了一个唯一的值（如时钟或指针）。这意味着你需要跟踪数据版本的历史。
- **Vector Clocks**：向量时钟被定义为来自每个节点的时钟值的元组。在分布式环境中，每个节点维护一个表示节点本身及其对或副本状态的时钟值元组。时钟值可以是源自本地时钟或版本号的实时时间戳。

<br/>

![向量时钟插图](/images/SRE/vector_clocks.png)

向量时钟有一下优点：

- 没有对时钟同步的依赖
- 不需要完整的利息记录序列


<br/>
<br/>


#### 分区

Partitioning

当数据量交叉单个节点的容量时，我们需要考虑分割数据，为负载均衡和灾难恢复创建副本。根据基础设施的动态程度，我们有几种方法可以采取：

- **Memory cached**：用于暂时数据的内存数据库分区。这些数据库通常用于RDBMS的前面。最常使用的数据从RDBMS复制到内存数据库中以促进快速查询并从后端DB切出负载。（如memcached数据库）
- **Clustering**：传统集群机制摘要客户端的集群拓扑。客户端不知道实际数据驻留在哪里以及它正在与之交谈的哪个节点。集群化常用于传统的RDBMS中，在那里它可以帮助在一定程度上伸缩持久化层到特定规模。
- **Separating reads from writes**：在此方法中，你有多个相同数据的副本。写入通常被发送到单个节点(Leader)，或副本节点(multi-Leaser)，其余的副本(Follower)处理读请求。Leader异步复制写操作到所有Followers。然而避免不了写入之后(write lag)。有时Leader可在将所有数据复制到Followers之前奔溃。当发生这种情况时，一个最符合最一致数据的follorer将变为leader。你还需要考虑读取和写入流量的比率。当写入高于读取时，此模型没有意义。复制方法也可以差异很大。一些系统定期使用全量转移，而其它系统则使用增量转移。
- **Sharding**：共享是指以这样的方式划分数据(dividing data)，即数据在节点集群中均匀地分发。它还可以暗示数据局部性，这意味着类似的和相关的数据存储在一起以促进更快地访问。分片可以进一步分支以满足负载均衡或灾难恢复的要求。单分片副本可能采用所有写入(single leader)，或多个副本写入(multi-leader)。读取可以跨多个副本分发。由于数据现在分布在多个节点上，因此客户端应该能够始终如一地弄清楚数据托管的位置。分片的缺点是碎片之间的连接是不可能的。因此，上下游应用程序必须从多个分片汇总结果。

<br/>

![分片示例](/images/SRE/database_sharding.png)


<br/>
<br/>


#### Hashing

哈希函数是映射一片数据的函数。通常用于描述某种物体，称为哈希码或简单地散列。在分区数据库中，重要的是要一致地将key映射到server/replica。

这种简单哈希的缺点是，每当集群拓扑发生变化时，数据分布也会发生变化。当你处理内存缓存时，它将很容易分发分区。每当节点加入或离开拓扑时，分区可以重新排序，可从后端数据库重新填充缓存未命中(cache miss)。然而，当你查看持久性数据时，由于新节点没有所需的数据，因此无法提供服务。这使我们能够保持一致的散列。

<br>

**Consistent Hashing**

一致散列是分布式散列方案，通过将其分配在抽象圆圈或散列圈上的位置，独立于分布式哈希表中的服务器或对象的数量来操作。这允许服务器和对象扩展而不影响整体系统。

![一致性哈希图](/images/SRE/consistent_hashing.png)


<br/>
<br/>


#### Quorum

分布式系统仲裁(quorum)是集群中必须在线且能够相互通信的最小节点数，它是一种用来保证数据冗余和最终一致性的投票算法，其主要数学思想来源于格巢原理。如果在此阈值之外发生任何其他节点故障，则集群将停止运行。

为了获得仲裁，你需要大部分节点，通常是`(N/2+1)`。

![Quorum示例](/images/SRE/Quorum.png)

网络问题可能导致集群节点之间的通信故障。一组节点可能能够在网络的运行部分中一起通信，但不能够与网络的另一部分中的不同节点通信。这称为集群分区中的**脑裂**(split brain)。

现在允许具有仲裁的分区继续运行应用程序。其它分区已从集群中删除。

<br>

例如，在一个5个节点的集群中，考虑如果节点1，2，3可以彼此通信，但无法与4，5节点通信。节点1，2，3作为多数，并且它们继续作为一个集群运行。节点4，5是少数，停止作为集群运行。如果节点3丢失与其它节点的通信，则所有节点都停止作为集群运行，由于集群可用节点数小于`(N/2+1)`这，意味这集群已经不可用了。但是，所有功能节点都将继续监听通信，以便在网络恢复时，集群可以形成并开始运行。

![cluster quorum](/images/SRE/cluster_quorum.png)








<br/>
<br/>



## 大数据

Big Data

大数据是无法使用传统计算技术处理的大型数据集的集合。它不是一个单一的技术或工具，而是一个完整的主题，涉及到各种工具，技术和框架。

大数据可能包括：结构化数据，非结构化数据，半结构化数据。

大数据的特征：Volume，Variety，Velocity，Variability。

大数据的例子包括：证券交易所，社交媒体网站，喷气发动机...








<br/>
<br/>

---

<br/>
<br/>







# 系统设计

Systems Design

思考和设计可扩展性、可用性和可靠性的大规模软件系统。

随着软件和硬件系统具有多种可移动部件，我们需要考虑这些部件如何生长，其失败模式，其依赖关系，如何影响用户和业务。没有一蹴而就的方法，我们只有通过设计和迭代来学习设计系统。


<br/>


## 可扩展性

Scalability: <https://linkedin.github.io/school-of-sre/level101/systems_design/scalability/>

可扩展性对系统、服务意味着什么？一个系统由服务或组件构成，每个服务、组件的可扩展性需要单独处理，系统的可扩展性是一个整体。如果在向系统添加资源时，以与添加的资源成正比的方式提高性能，则称该服务是可扩展的。

如果添加资源以促进冗余不会导致性能损失，则称永远在线的服务是可扩展的。


<br/>


### Scale Cube

[Scale Cube](https://akfpartners.com/growth-blog/scale-cube)是一个用于分割服务、定义微服务和扩展产品的模型。以下章节讨论基于对AFK cube的推断。


<br/>
<br/>


### 水平伸缩

Scalability - Horizontal scaling

水平伸缩代表应用程序或服务的克隆，这样工作就可以轻松地跨实例分发，绝对没有偏见。

![水平伸缩](/images/SRE/horizontal-scaling.jpg)

示例里数据库从应用程序分开扩展。这是为了让你知道每个组件的扩展性可能不同。通常web应用可以通过添加资源来进行扩展，除非应用程序内部存储了状态（有状态服务）。但是DB可以通过添加更多的Followers来仅针对读取进行扩展，而写入必须只转到一位Leader以确保数据的一致性。

有一些DB支持多主(multi-leades)写操作。

应用程序应该能够区分独写，以选择适当的数据库服务器。负载均衡器可以透明地拆分相同服务器之间的流量。

<br/>

**WHAT**：复制服务或数据库以传播负载。
**WHEN TO USE**：具有很高独写率的数据库。因为仅有读副本可以扩展，而不是主。
**HOW TO USE**：简单地克隆服务并执行负载均衡器。对于数据库，请确保访问代码了解读取和写入之间的差异。
**WHY**：允许以重复数据和功能的成本快速进行扩展。
**KEY TAKEAWAYS**：这是快速实现，从开发人员的角度来看，较低的成本，并且可以很好地伸缩交易卷。然而，他们往往从数据运行成本的角度来看高成本。


<br/>
<br/>


#### 负载均衡

Scalability Pattern - Load Balancing

改善跨多个计算资源，如计算机集群、网络链接、中央处理器、磁盘驱动的工作负载分布。一种常用的技术是跨相同服务器集群的负载均衡流量。类似的原理有用于跨网络链接的ECMP，RAID的磁盘驱动的负载均衡。

旨在优化资源使用、最大化吞吐量、最小化响应时间并避免任何单个资源过载。使用具有负载均衡的多个组件的冗余效果，来替代单个组件，可以提高可靠性和可用性。

执行负载均衡的设备或系统称为负载均衡器，简称LB。


<br/>
<br/>


#### 负载均衡的任务

Scalability Pattern - LB Tasks

负载均衡做什么：

- 服务发现(Service discovery)
- 健康检查(Health checking)
- 负载均衡(Load balancing)


<br/>
<br/>


#### 负载均衡的方法

Scalability Pattern - LB Methods

常用的负载均衡方法：

- 最小连接(Least Connection Method)
- 最小响应时间(Least Response Time Method)
- 轮询(Round Robin Method)
- IP哈希(IP Hash)


<br/>
<br/>


#### CDN

Scalability Pattern - Caching - Content Delivery Networks (CDN)

CDN更加靠近客户的位置。不长更改的静态数据，则可以使用CDN缓存。

![CDN](/images/SRE/sre-cdn.jpg)


<br/>
<br/>


### 微服务

Scalability - Microservices

此模式表示应用程序中的服务或功能的拆分。微服务旨在解决与代码库和数据集中增长和复杂相关的问题（解耦）。意图是创建故障隔离和响应时间。

微服务可以扩展事务、数据大小和代码库大小。这个的开销比水平伸缩多一些，因为工程团队需要重写服务或分解程序。

![微服务](/images/SRE/microservices.jpg)


<br/>
<br/>


### 分片

Scalability - Sharding

此模式表示基于在事务时查找，或确定的属性的工作分离。通常，这些是由请求者，或客户端的拆分来实现。

分片伸缩事件增长，伸缩指令集，减少处理时间（通过限制执行任何事务所需的数据）。这在客户端伸缩增长方面更有效。它可以帮助灾难恢复，并限制事件的影响仅限于一个特定客户段。

![分片1](/image/SRE/sharding-1.jpg)

这里，认证数据基于用户名分配，使得数据库可以减少在查询期间的数据量，更快地响应。

<br/>

这里也还有其它的方式：

![分片2](/images/SRE/sharding-2.jpg)

这里整个数据中心被分割和复制，客户端基于地理位置引导到数据中心。这有助于提高性能，使客户端被引导到最接近的数据中心。使用此方法有一些复制和一致性方面的开销。



<br/>
<br/>



## 可用性

Availability: <https://linkedin.github.io/school-of-sre/level101/systems_design/availability/>

可用性通常用9来表示。

| Availability% | Downtime per year | Downtime per month | Downtime per week | Downtime per day |
|  -  |  -  |  -  |  -  |  -  |
| 99%(Two Nines) | 3.65 days | 7.31 hours | 1.68 hours | 14.40 minutes |
| 99.5%(Two and a half Nines) | 1.83 days | 3.65 hours | 50.40 minutes | 7.20 minutes |
| 99.9%(Three Nines) | 8.77 hours | 43.83 minutes | 10.08 minutes | 1.44 minutes |
| 99.95%(Three and a half Nines) | 4.38 hours | 21.92 minutes | 5.04 minutes | 43.20 seconds |
| 99.99%(Four Nines) | 52.60 minutes | 4.38 minutes | 1.01 minutes | 8.64 seconds |
| 99.995%(Four and a half Nines) | 26.30 minutes | 2.19 minutes | 30.24 seconds | 4.32 seconds |
| 99.999%(Five Nines) | 5.26 minutes | 26.30 seconds | 6.05 seconds | 864.0 ms |


<br/>


### 可用性串行组件

HA - Availability Serial Components

如果组件故障，则导致系统无法操作。

例如，如果LB故障，则应用的所有访问都异常。


<br/>
<br/>


### 可用性并行组件

HA - Availability Parallel Components

如果组件故障，则导致系统启动并联操作。

如果我们有不止一个LB，当一个LB故障时，可以让其它LB接管并运行。


<br/>
<br/>


### 核心准则

HA - Core Principles

- 消除单点故障(SPOF, Elimination of single points of failure)：意味着增加系统冗余，组件故障并不意味着整个系统的故障。
- 可靠交叉(Reliable crossover)：在冗余的系统同，交叉点往往成为单点故障。因此，可靠的系统必须提供可靠的交叉。
- 检测失败(Detection of failures as they occur)：如果观察到上述两个原则，那么用户可能永远不会看到失败。

<br/>

高可用的架构图可能如下:

![HA](/images/SRE/availability.jpg)



<br/>
<br/>



## 容错

Fault Tolerance: <https://linkedin.github.io/school-of-sre/level101/systems_design/fault-tolerance/>

故障在任何系统中都无法避免，并且可能随时会发生，因此我们需要构建可以容忍故障，或可以从故障中恢复的系统。

- 在系统中，失败是常态而不是例外。
- 可能出错的事情就会出错。
- 复杂系统包含在它们内部的故障潜伏的变化。


<br/>


### 故障指标

Failure Metrics

测量和追踪系统的常见的故障指标：

- **平均恢复时间**(MTTR, Mean time to repair)：恢复故障的系统的平均时间
- **平均故障间隔时间**(MTBF, Mean time between failures)：一个故障和下一个故障之间的平均操作时间
- **平均失败时间**(MTTF, Mean time to failure)：预期在系统故障前系统的平均时间
- **平均检测时间**(MTTD, Mean time to detect)：从故障出现到检测到的平均时间
- **平均调查时间**(MTTI, Mean time to investigate)：从检测到故障到调查原因和解决方法之间的平均时间
- **平均恢复服务的时间**(MTRS, Mean time to restore service)：从检测到故障的组件到用户可用的平均经过时间
- **系统事件的平均时间**(MTBSI, Mean time between system incidents)：两个连续事件检测之间的平均经过时间（`MTBSI = MTBF + MTRS`）
- **故障率**(Failure rate)：故障发生的频率


<br/>
<br/>


### 故障隔离

Fault Isolation Terms

系统应具有短路(short circuit)。如果通知不起作用，则站点应该通过删除功能，而不是让整个站点挂掉，来优雅地处理该故障。

泳道(Swinlane)是最常用的故障隔离方法之一。它从其它服务中增加了障碍到服务中，以便故障不会影响到其它服务。

假设我们有一个广告系统，我们可以有两个架构：

![泳道1](/images/SRE/swimlane-1.jpg)

如果在每个新闻请求期间同步生成广告，则广告功能中的故障将传播到新闻功能中。相反，如果我们使用泳道隔离新闻和广告，广告故障将不会划入到新闻，如果广告不符合SLA，最坏的情况是，我们可以没有广告。

让我们来看一个新模型的示例。

![泳道2](/images/SRE/swimlane-2.jpg)


<br/>
<br/>


### 泳道

准则(Swimlane Principles)：

- 尽可能少的共享(share as little as possible)。在泳道内共享的越少，泳道更容易变得孤立。
- 没有什么跨越泳道边界。同步通信从未跨越泳道边界。

<br>

方法(Swimlane Approaches)：

- 泳道就是摇钱树。永远不要允许你的收银器向其它系统妥协。
- 泳道是最大的事件来源。识别疼痛的重复原因并分离它们。
- 泳道是天然屏障。






<br/>
<br/>

---

<br/>
<br/>








# 指标和监控系统

Metrics and Monitoring: <https://linkedin.github.io/school-of-sre/level101/metrics_and_monitoring/introduction/>

监控是任何系统的一个组成部分。作为SRE，你需要了解监控服务基础架构。也可参考Google SRE book。

有以下主题：

- 监控是什么
  - 需要测量什么
  - 聚集的指标如何用于改善业务决策和整体可靠性
  - 告警
  - 日志处理
- 什么是可观测性
  - 分布式追踪
  - 日志
  - 指标

<br>

**监控的四个黄金信号**

Four golden signals of monitoring

- 流量(Traffic)
- 延迟(Latency)
- 错误(Error)
- 饱和度(Saturation)

<br/>

**监控为什么重要**

一些关键用例：

- 即使解决问题(Reduction in time to resolve issues)
- 商业决策(Business decisions)
- 资源计划(Resource planning)

<br/>

一些基本术语：

- metric
- Node/Host
- QPS
- Latency
- Error rate
- Graph
- Dashboard
- Incident
- MTTD
- MTTR

<br/>

监控基础架构：

![monitor-infra](/images/SRE/monitoring-infra.jpg)


<br/>
<br/>


## 命令行工具

Command-line tools

大多数的Linux发行版都带有一组监控性能的工具集。

- `ps/top`
- `ss`
- `free`
- `df/du`
- `sar`
- `iftop`
- `iotop`
- `tcpdump`


<br/>
<br/>


## 告警


<br/>
<br/>


## 最佳实践

Best practices for monitoring

- 使用正确的指标类型
  - Gauge
  - Timer
  - Counter
- 避免过度监控
- 防止告警疲劳
- 有告警流程


<br/>
<br/>


## 可观测性

Observability

- 展示
  - Grafana
- 日志
  - ELK stack
- 追踪
  - Jaeger
  - Zipkin





<br/>
<br/>

---

<br/>
<br/>





# 安全性

Security: <https://linkedin.github.io/school-of-sre/level101/security/intro/>


<br/>


## 基础知识

Fundamentals


<br/>


### 概述

Introduction to Security Overview for SRE

![安全](/images/SRE/security1.png)

<br>

- SRE和安全工程(Security Engineering)都与保持系统可用相关
- 发生中断、容量短缺和配置错误等问题可能导致系统无法使用
- 破坏用户信任的安全或隐私事件也会破坏系统的实用性
- 系统安全应该是SRE的首要考虑因素
- SRE应该参与重要的设计讨论和实际的系统更改
- 有助于防止可能影响基础设施整体安全性的不良设计和实施
- 成功地设计、实施和维护系统，需要对完整的系统生命周期做出承诺
- 信息安全核心：
  - 保密性(Confidentiality)
  - 完整性(Integrity)
  - 可用性(Availability)
- 像安全工程师一样思考
- 启动新应用应该考虑：
  - 有缺陷吗？
  - 如何滥用一个功能可能导致设计缺陷？
  - 默认情况下是否需要此功能？
- OWASP安全准则(Open Web Application Security Project)
- 最小化攻击面(Minimize attack surface area)
  - 添加到应用程序的每个功能都会为整个程序增加一定的风险。安全开发的目的是通过减少攻击面积来降低整体风险。
- 建立安全默认值(Establish secure defaults)
  - 开箱即用的功能应该默认是安全的。例如，默认情况下，应启用用户密码复杂度和老化。用户可以关闭功能。
- 最小化权限的原则(Least privilege)
  - 建议账户具有执行业务流程所需的最小的权限。
- 深度防御原则(Defense in depth)
- 安全地失败(Fail securely)
- 不要相信服务(Don’t trust services)
  - 不要过度信任第三方服务
- 职责分离(Separation of duties)
  - 欺诈行为的关键是职责分离
  - 管理员不应该是程序运行用户
- 通过模糊来避免安全性(Avoid security by obscurity)
- 保持安全性简单化(Keep security simple)
- 正确修复安全问题(Fix security issues correctly)
  - 一旦确定了安全问题，在不引入回归的情况下开发正确的修复是必不可少的。
- 可靠性和安全性(Reliability and Security)


<br/>
<br/>


### 认证和授权

Authentication and Authorization

密码是常见的认证因素。还有MFA多因子认证。

授权是提供用户权限访问特定资源或功能。

<br/>

常见认证流程：

- 用户注册
- 应用程序存储用户信息到数据库
- 应用发送验证信息来验证
- 用户登录
- 用户访问

<br/>

OpenID和OAuth：

OpenID是一个认证协议，允许我们在不使用本地认证系统的情况下验证用户。

OAuth是一种授权机制，允许你的应用用户访问特定内容提供者（如Gmail/Facebok/Ins/...）。


<br/>
<br/>


### 密码学

Cryptography

它是一门科学和研究，将任何文本隐藏起来，只有指定的收件人或授权人员才能阅读。

密码学是保护关键信息的必要方法，通过编码一些纯文本将其转化为私有数据。


<br/>


#### 密码

Ciphers

密码是密码学的基石。密码是一组对消息进行加密(encryption)或解密(decryption)的算法。

```
# 加密算法E，密钥k，消息内容m，加密文件c
E(k, m) = c

# 解密算法D，密钥k，加密文件c，消息内容m
D(k, c) = m

D(k, E(k, m)) = m
```

<br/>

**流密码**(stream Ciphers)：

- 消息被分解成字符(characters)或位(bits)，并使用密钥或密钥流(随机且独立于消息生成)进行加密，密钥或密钥流的长度和明文流相同。
- 如果密钥流是随机的，此方案是不可破解的，除非获得密钥流。密钥流必须以安全的方式提供给双发，以防止其释放。

<br/>

**块密码**(Block Ciphers)：

- 以块的形式处理消息，然后对每个块进行加密或解密。
- 块密码是一个对称密码，其中测录快作为整体处理并用于生成密文块。块大小通常为64bits或128bits。

![加密对比](/images/SRE/stream-block-ciphers.png)

![加密](/images/SRE/ciphers.png)

<br/>


**加密**(Encryption)

- 对称加密(Secret Key, Symmetric Key)：相同的密钥用于加密和解密。
- 非对称加密(Public Key, Asymmetric Key)：在非对称密钥中，加密和解密密钥是不同的，但又互相关联。加密密钥称为公钥(publick key)，解密密钥称为私钥(private key)。公私钥称为密钥对。

<br/>

**对称密钥加密**(Symmetric Key Encryption)

- DES(Data Encryption Standard)
  - 55bits的对称加密算法，由于长度不够，它易于具有足够资源的暴力攻击。
  - 通常在块密码中操作，由此它将在64bits块中加密数据。
  - 基于简单的数学函数，所以它可以在硬件中轻松实现加速。
- 3DES(Triple DES)
  - 原始的56bits长度不够，无法承受有限预算的攻击。
  - 将DES三次连续应用。
- AES
  - 具有可变block长度和key长度
  - AES替代DES和3DES，因为它们太弱了。AES更快更有效，如果使用纯软件加速，AES也更适合高吞吐量。由于它比较年轻，更成熟的算法总是更信任。

![3DES](/images/SRE/3DES.png)

<br/>

**非对称密钥加密算法**(Asymmetric Key Algorithm)

![非对称加密](/image/SRE/asymmetric-key-aalgorithm.png)

<br/>

- Diffie-Hellman
- RSA
- Hashing Algorithms
  - 哈希基于单向数学函数，反向则很难。
- MD5
  - MD5是一种单向函数，它很容易从给定的数据计算处哈希值，反之则不行。
- SHA-1, SHA-256
  - 由于MD5具有一些弱点，所以认为MD5比SHA-1弱一些
- 数字证书(Digital Certificates)
  - 数字签名，提供数字验证设备和个人用户的方法。
  - 密钥管理被认为加密系统中困难的任务。
  - PKI(Public Key Infrastructure)为管理数字安全属性提供了分层框架。每个PKI参与者都拥有一个由CA发布的数字证书。证书包含多个当事方协商安全连接时使用的属性。这些属性必须包括证书有效期，最终主机标识信息，安全通信的加密密钥，CA的签名。其它可选属性取决于PKI的要求。
  - CA是可以信任的第三方。
  - 要验证CA的签名，接收方必须知道CA的公钥。
- CA Enrollment process
  - 终端主机生成密钥对。
  - 终端主机生成证书请求，它转发到CA。
  - 手动人为干预需要批准CA收到的请求。
  - CA操作员批准请求后，CA使用私钥签署证书请求，并将已完成的证书返回到终端主机。
  - 终端主机将证书写入非易失性存储区域


<br/>
<br/>


### 登录安全

Login Security


<br/>


#### SSH

- SSH(Secure Shell)，一种流行的、强大的、基于软件的网络安全方法。
- 每当计算机向网络发送数据时，SSH都会自动对其进行加密。当数据到达预期接收者时，SSH会自动对其进行解密。
- 结果时透明加密的，用户可以正常工作，不知道他们的通信在网络上时安全加密的。此外，SSH可以根据其配置方式使用现代安全的加密算法。
- SSH是C/S架构。
- SSH创建一个通道，用于在远程机器上运行shell，并在两个系统之间进行端到端加密。
- 通过强加密保护你的数据隐私
- 保证通信的完整性，保证没有被篡改
- 认证
- 授权
- 转发或隧道加密其它基于TCP/IP的会话

![SSH](/images/SRE/ssh.png)


<br/>
<br/>


#### Kerberos

- 根据希腊神话，Kerberos(Cerberus)是一只巨大的三头犬，它守卫这冥界的大门，以防止死者离开。
- 在计算机科学中，它是一种计算机网络认证协议，对个人通信以安全的手段进行身份认证。目前是微软ActiveDirectory中使用的默认技术。
- 使用对称密钥加密，并需要一个可信的第三方身份验证服务来验证用户身份。
- 客户端：用户或服务
- 服务端：Keberos保护主机驻留

<br/>

![KDC](/images/SRE/KDC.png)

KDC(Key Distribution Center)，充当可信的第三方认证服务。KDC包含以下两个服务器：

- 认证服务器(AS, Authentication Server)：performs the initial authentication and issues ticket-granting tickets(TGT) for users.
- 票务授权服务器(TGS, Ticket-Granting Server)：issues service tickets that are based on the initial ticket-granting tickets.

![KDC2](/image/SRE/KDC2.png)


<br/>
<br/>


#### 证书链

Certificate Chain

OpenSSL命令的输出的第一部分显示了编号为0, 1, 2(不再是2)的三个证书。每个证书有一个subject和issuer。第一个证书编号0，称为端实体证书(end-entiy certificate)。主题行告诉我们，它对任何`google.com`的任何子域都有效，因为它的主题设置为`*.google.com`。

```bash
openssl s_client -connect www.google.com:443 -CApath /etc/ssl/certs
CONNECTED(00000005)
depth=2 OU = GlobalSign Root CA - R2, O = GlobalSign, CN = GlobalSign verify return:1
depth=1 C = US, O = Google Trust Services, CN = GTS CA 1O1 verify return:1
depth=0 C = US, ST = California, L = Mountain View, O = Google LLC, CN = www.google.com verify return:1
---
Certificate chain
0 s:/C=US/ST=California/L=Mountain View/O=Google LLC/CN=www.google.com
  i:/C=US/O=Google Trust Services/CN=GTS CA 1O1
1 s:/C=US/O=Google Trust Services/CN=GTS CA 1O1
  i:/OU=GlobalSign Root CA - R2/O=GlobalSign/CN=GlobalSign
---
Server certificate
-----BEGIN CERTIFICATE-----
xxx
-----END CERTIFICATE-----
subject=
issuer=
```

<br>

![证书链](/images/SRE/certificate-chain.png)


<br/>
<br/>


#### TLS握手

TLS Handshake

1. 客户端发送HELLO消息给服务端，其中包含它所支持的协议和算法。
2. 服务端返回HELLO并发送其证书链。基于客户端的功能，服务器选择密码套件。
3. 如果密码套件支持短信密钥(ephemeral key)交换，则服务端和客户端用Diffie-Hellman算法协商一个主密钥(premaster-key)。
4. 客户端和服务端创建一个会话密钥(session key)，用于加密通过连接传输的数据。

在握手的末尾，双方拥有用于加密其余连接的数据的私密会话密钥(secret session key)。这是OpenSSL指作为Master-Key的原因。





<br/>
<br/>
<br/>






## 网络安全

Network Security: <https://linkedin.github.io/school-of-sre/level101/security/network_security/>


<br/>
<br/>


### 介绍

Introduction

TCP/IP是当今占主导地位的网络技术。它是一个五层架构。除此之外，还有其它网络技术。出于方便起见，我们使用OSI网络模型来表示非TCP/IP的网络技术。不通的网络使用网关互连，网关可以放置在任何层。

OSI模型是一个七层架构。

![网络模型](/images/SRE/tcp-ip-osi.png)

<br/>

OSI参考模型各层的简短描述：

- 应用层作为应用程序和网络程序之间的接口。它支持应用程序和终端用户处理。常见的应用层程序包括远程登录、文件传输、邮件和网页浏览。
- 表示层处理不同形式的数据。它允许驻留在不同平台的不同侧的应用程序能够理解彼此的数据格式，而不管它们是如何呈现的。
- 会话层负责创建、管理和关闭通信连接。
- 传输层负责提供可靠连接，如数据包排序、流量控制、拥塞控制。
- 网络层负责将与设备无关的数据包从当前跳路由到下一跳(hop)。
- 数据链路层负责将与设备无关的数据包封装成与设备有关的数据帧(frame)。它有两个子层：逻辑链路控制和媒体访问控制。
- 物理层负责通过某些物理介质传输与设备相关的帧。
- 从应用层开始，应用程序产生的数据逐层向下传递到物理层。层层封装（来自前一层的数据被封装在当前的新信封中，其中来自前一层的数据也只是包含来自更前一次的数据的信封）。在每层添加的信封包含足够的信息来处理数据包。应用层数据被分为足够小的块，以封装在下一层的信封中。
- 应用程序数据块在TCP/IP架构中基于以下步骤进行装扮。在发送端，当应用程序数据块传递到TCP层时，它封装在TCP数据包中。换句话说，一个TCP数据包由header和payload组成，头部对应TCP信封，有效负载是应用程序数据块。同样，TCP数据包在向下传递到IP层时将被封装在IP数据包中。IP数据包由header和payload组成，有效载荷是从TCP层传递下来的TCP数据包。当IP数据包向下传递到数据链路层时，它将被封装在依赖于设备的帧中（如以太网帧）。一个数据帧有一个头部，也可能有一个尾部。当它向下传递到物理层时，一个帧将被转换成一系列媒体信号进行传输。
- 在目的端，媒介信号由物理层转换成帧，向上传递到数据链路层。数据链路层将帧的有效载荷传递给IP层。IP层将有效载荷传递给TCP层。TCP层将TCP有效载荷(即应用程序数据块)传递到应用层。当数据包到达路由器时，它只能转到IP层，其中IP包头部中的某些字段被修改（TTL-1）。然后将修改的数据包向下传递给物理层以进一步传输。

![OSI层](/images/SRE/osi-7layers.png)


<br/>
<br/>


#### PKI

公钥基础涉设施(PKI, Public Key Infrastructure)

要在网络应用程序中部署加密算法，我们需要一种使用开放网络分发私钥的方法。公钥加密是分发这些私钥的最佳方法。要使用公钥加密，我们需要构建一个公钥基础设施(PKI)来支持和管理公钥证书和证书颁发机构(CA)网络。

特别是，PKIs设置为执行以下功能：

- 在向他们发出公钥证书之前确定用户的合法性。
- 在用户请求时发出公钥证书。
- 在用户请求时扩展公钥证书有效时间。
- 在用户请求或相应的私钥泄露时撤销公钥证书。
- 存储和管理公钥证书。
- 防止数字签名的签名者否认他们的签名。
- 支持CA网络允许不同的CA验证其他CA拌饭的公钥证书。
- X.509


<br/>
<br/>


#### IPsec

IPsec: A Security Protocol at the Network Layer

- IPsec是网络层的主要安全协议。
- IPsec为构建虚拟专用网络(VPN)提供了强力的平台。VPN是覆盖在公共网络上的专用网络。
- 在IP层部署加密算法的目的是加密或验证IP数据包。
- IPsec还制定了如何交换密钥。因此，IPsec由身份认证协议、加密协议和密钥交换协议组成。它们称为：
  - AH: authentication header
  - ESP: encapsulating security payload
  - IKE: internet key exchange


<br/>
<br/>


#### 电子邮件安全

PGP & S/MIME : Email Security

应用层上有几种安全协议。最常用的这些协议是名称为PGP和S/MIME的电子邮件安全协议。

SMTP（simple mail transfer protocol）用于从客户端发送和交付到服务端，通过25端口，它是传出服务器(outcoming)。相反，POP(post office protocol)允许用户拾取消息并将其下载到他们的收件箱中，它是传入服务器(incoming)，使用110端口，最新的是POP3。

<br/>

**PGP**

- PGP实现了所有的主要加密算法，ZIP加密算法，Base64编码算法。
- 它用于认证和加密信息。PGP遵循如下过程：身份认证，ZIP压缩，加密和Base64编码。
- Base64编码过程使消息为SMTP传输做好准备。

<br/>

**GnuPG**

- GnuPG是另一个免费加密标准
- GnuPG是赛门铁克PGP的替代平
- 两者主要的区别是所支持的算法。由于GnuPG是开放的，一些企业更愿意使用PGP的技术支持和用户界面。
- GnuPG和PGP在兼容性上存在一些细微差别，如某些算法之间的兼容性，但在大部分应用程序在有解决方法。

<br/>


**S/MIME**

- SMTP仅能处理7为ASCII文本消息。虽然POP可以处理7位ASCII之外的其它内容类型，但在常见的默认设置下，POP可能会将存储在邮件服务器中的所有消息下载到用户的本地计算机。之后，如果POP从邮件服务器上删除了这些消息。这使得用户难以从多台计算机读取消息。
- 多功能互联网邮件扩展协议(MIME, multipupose internet mail extension protocol)，旨在支持以各种格式发送和接收电子邮件，包括文档处理器、图形文件、声音文件、视频文件等非文本文件。此外，MIME允许单个消息中包含这些混合类型组合形式的数据。
- 互联网邮件访问协议(IMAP, internet mail access protocol)，存储传入邮件服务器的电子邮件，直到用户故意删除它们。这允许用户从多台计算机访问其邮箱，并将消息下载到本地计算机，而不从邮件服务器 的邮箱中删除它。

<br/>

**SSL/TLS**

- SSL使用PKI通过要求服务器使用可信的CA签署的安全的证书，来决定服务器的公钥是否值得信赖。
- Netscape Navigator 1.0，它信任由RSA数据安全公司操作的CA。
- 服务器的public RSA keys用于存储在安全证书中，然后浏览器可以使用它来建立安全的通信通道。我们今天使用的安全证书依赖于Netscape Navigator 1.0当时使用的相同标准(X.509)。
- 网景打算培训用户（尽管后来没有成功），区分安全通信和不安全通信，因此他们在浏览器地址栏旁边放了一个锁图标。当锁打开时，通信是不安全的。关闭的锁意味着通信已使用SSL进行保护，这需要服务器提供签名证书。网景的工程师真正创建了安全互联网的标准。
- SSL 3.0在2015年开始弃用。为了标准化SSL，IETF创建了一个稍微修改的SSL 3.0，并于1999年将其发布为TLS 1.0。SSL和TLS之间的名称更改使很多人感到困惑。正式地讲，TLS是新的SSL，但在实践中，人们交替使用SSL和TLS来谈论任何版本的协议。



<br/>
<br/>



### 网络外围安全

Network Perimeter Security


<br/>


#### 通用防火墙框架

General Firewall Framework

- 防火墙是必须的，因为加密算法无法有效阻止恶意数据包进入边缘网络。
- 无论是加密的数据包，都可以始终转发到边缘网络中。
- 防火墙在1990年代被开发来帮助限制网络访问。防火墙可以是硬件设备，软件包或两者的解和。
- 在数据包允许进入(从外部进入内部网络)之前先评估它。防火墙的一个关键元素是它能够检查数据包而不对通信速度产生负面影响，同时为内部网络提供安全保护。
- 防火墙检查数据包的几种方法。基于防火墙使用的特定方法，它可以表征为包过滤器、电路网关、应用网关、动态包过滤器。


<br/>
<br/>


#### 包过滤器

Packet Filters

- 它检查从外部到内部网络的入数据包，检查从内部网络到外部的出数据包。
- 包过滤仅检查IP和TCP头部信息，不包括有效载荷。
- 包过滤防火墙使用一组规则来确定是否应该允许或拒绝它。
- 两种类型：
  - Stateless：它将每个数据包视为一个独立的对象，它不会跟踪任何先前处理的数据包。换句话说，它检查数据包并不留下检查记录。
  - Stateful：连接状态过滤，追踪内部主机和外部主机之间的连接。连接状态指示它是TCP还是UDP连接，以及是否建立连接。


<br/>
<br/>


#### 电路网关

Circuit Gateways

- 电路网关，也称为电路级网关，通常在传输层操作。
- 它评估IP地址和端口信息（TCP/UDP header），并使用它们来确定是允许还是拒绝。
- 通常做法是组合怕过滤器和电路网关以形成动态包过滤器(DPF, dynamic packet filter)。


<br/>
<br/>


#### 应用网关

Application Gateways(ALG)

- Aka代理服务器
- 应用级网关充当内部主机的代理，处理来自外部客户端的请求。
- 应用级网关对每个IP数据包执行深度检查
- 特别地，应用级网关检查包含在数据包中的应用程序数据格式(MIME, SQL...)，并决定是否允许有效载荷。
- 因此，应用级网关能够检测到有效载荷中包含的计算机病毒。由于它检查了有效载荷，因此它还能够检测恶意代码和隔离的数据包。另一方面，应用级网关也会引发大量的计算和开销。


<br/>
<br/>


#### 可信系统和堡垒机

Trusted Systems and Bastion Hosts

- 可信操作系统是一个符合特定安全要求的系统。
- 系统设计不含缺陷(defects)。
- 系统软件不包含漏洞(loopholes)。
- 系统配置正确。
- 系统管理是合适的。
- 堡垒机
- 堡垒机是具有强大防御机制的计算机。
- 堡垒机也主要用于受控进入点(controlled ingress)，以便安全检测可以更狭隘地关注在单点紧密的动作。



<br/>
<br/>



### 扫描和抓包

Scannings, Packet Capturing


<br/>


#### 使用nmap扫描端口

Scanning Ports with Nmap

nmap(network mapper)是一个免费且开源的使用工具，用于网络发现和安全审计。nmap通常用于确定网络中的活动主机，这些主机上的开放端口，这些开放端口上运行的服务和版本。

<br/>

nmap使用6种不同的端口状态：

- **Open**：开放的端口主动接收TCP, UDP或SCTP连接。开放的端口是最受欢迎的东西，因为它是易受攻击的事物。开放的端口还显示了网络上的可用服务。
- **Closed**：接收并响应nmap探测数据包，但此端口上没有侦听应用程序。用于检测主机或系统的端口。
- **Filtered**：nmap无法确定端口是否打开，因为数据包过滤可阻止其探针到达端口。过滤可能来自防火墙或路由器规则。
- **Unfiltered**：端口可访问，但nmap不知道它是否开启还是关闭。
- **Open/Filtered**：nmap无法确定是打开还是过滤。当打开的端口没有响应时，回发生这种情况。
- **Closed/Filtered**：nmap无法确定是关闭还是过滤。

<br/>

nmap扫描的类型：

- TCP连接
  - TCP连接扫描完成三次握手
  - 如果端口打开，则操作系统完成TCP三次握手，端口扫描器立即关闭连接以避免DOS攻击。
- UDP扫描
  - 检查UDP端口是否侦听
  - 由于UDP不像TCP那样响应一个积极的ACK，并且当端口关闭时才能响应传入的UDP包。
- SYN扫描
  - SYN扫描是其它形式的TCP扫描
  - 此扫描也称为半开放扫描(half-open)，因为它永远不会完全打开完整的TCP连接。
  - 端口扫描器生成SYN包。如果目标端口是打开的，它将使用SYN-ACK包进行响应。扫描器主机响应RST包，在握手完成之前关闭连接。
  - 如果端口关闭到未过滤，则目标立即响应RST包。
  - SYN扫描有个有点，各个服务从未实际接收连接。
- FIN扫描
- ACK扫描
  - ACK扫描确定端口是否过滤
- 空扫描
  - 将所有TCP header flags设置为off或null
  - 这通常不是有效的数据包，有些主机不知道该怎么办
- XMAS扫描
- RPC扫描
- IDLE扫描


<br/>
<br/>


#### OpenVAS

- 一个全功能的漏洞扫描器
- 一个服务和工具的框架，提供全面而强大的漏洞扫描和包管理
- 一个开源程序
- 由三部分组成：
  - NVTs(Network Vulnerability Tests)
  - Scanner
  - SQLite3用于存储配置和扫描结果


<br/>
<br/>


#### WireShark

一个协议分析器。


<br/>
<br/>


#### DumpCap

一个网络流量转存工具。它从实时网络捕获数据包，并将数据包写入文件。dumpcap原生捕获的文件格式是pcapng，这也是wireshark使用的格式。


<br/>
<br/>


#### DaemonLogger

DaemonLogger是一个数据包记录应用程序，专门用于网络和系统管理环境。


<br/>
<br/>


#### NetSniff-NG

NetSniff-NG是一个高性能的数据包捕获工具。


<br/>
<br/>


#### Netflow

Netflow是1996年思科路由器上引入的功能，提供了收集IP网络流量的功能，因为路由器是进出口。通过分析Netflow提供的数据，网络管理员可以确定流量、服务类型和拥塞原因。


<br/>
<br/>


#### IDS

一种在环境中检测到安全事件相关的安全解决方案，但不会阻止它们。IDS传感器可以是软件和基于硬件的用于收集和分析网络流量。这些传感器有两个类型，网络IDS和主机IDS。

- host IDS是一个在服务器上运行的特定服务器的代理，其最小开销是监控操作系统。
- network IDS可以嵌入网络设备、独立设备或监控网络流量的模块种。

<br/>

**基于签名的IDS**

































<br/>
<br/>

---

<br/>
<br/>







# 行为准则

Code of Conduct: <https://linkedin.github.io/school-of-sre/CODE_OF_CONDUCT/>

Linkedln的开源社区努力：

- 友好和耐心(Be friendly and patient)
- 欢迎(Be welcoming)
- 尊重(Be respectful)
- 理解分歧(Understand disagreements)















































