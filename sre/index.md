# SRE



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

![分片1](/images/SRE/sharding-1.jpg)

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

![非对称加密](/images/SRE/asymmetric-key-aalgorithm.png)

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

![KDC2](/images/SRE/KDC2.png)


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
<br/>


### TCP/IP安全事件

TCP/IP Security Issues

![TCP](/images/SRE/tcp-header.png)


<br/>
<br/>


#### IP欺骗

IP Spoofing

- 攻击者替换IP地址（发送端或接收端）。
- 通常用于利用目标主机来启动DOS攻击(denial-of-service)。
- 在DOS攻击中，攻击者修改IP数据包以误导目标主机，以接受原始数据包作为在可信主机处提供的数据包。攻击者必须知道受信任的主机IP地址才能修改数据包头部，以便看起来数据包来自该主机。

<br/>

IP欺骗检测技术：

- 直接TTL探测
  - 此技术将一个数据包发送到可疑的欺骗IP主机，该主机触发回复并将TTL与可疑数据包进行比较。如果TTL不同，则这是一个欺骗性的数据包。
  - 当攻击者和受害者位于不同的子网中时，此技术是成功的。
  - IP标识号。
- TCP流控制
  - 发送欺骗TCP数据包的攻击者，将不会收到目标的SYN-ACK数据包。
  - 因此，攻击者不能响应于拥塞窗口大小的变化。
  - 即使在窗口大小耗尽之后，接收器仍然接收流量时，大多数可能是数据包被欺骗。

![TTL探测](/images/SRE/ttl-probe.png)


<br/>
<br/>


#### 隐蔽信道

Covert Channel

- 隐蔽或秘密通道可以最好地描述为可通过进程或应用程序以违反系统安全规划的方式进行信息来利用的两个实体之间的管道或通信信道。
- 更具体于TCP/IP，在某些情况下，建立隐蔽信道，并且可在两个终端系统之间秘密地传递数据。
- 如ICMP协议，ICMP ECHO请求消息应具有8字节头部和56字节有效载荷。有效载荷中不应携带任何数据，但这些数据包通常用于携带机密信息。ICMP数据包被稍微改变以在有效载荷中携带机密数据。这使得数据包大小较大，但协议栈中不存在无法控制以打败此行为。ICMP数据包的更改允许入侵者编程专门的C-S对。这些小片代码导出机密信息而不提醒管理员。


<br/>
<br/>


#### IP分段攻击

IP Fragmentation Attack

- IP协议允许对数据包进行分段
- IP分段偏移用于跟踪数据报的不同部分
- 此字段中的信息或内容用于在目的端重组数据报
- 所有这些分片都具有相同的标识字段值，分片偏移指示当前分片在原始数据包上下文中的位置
- 许多接入路由器和防火墙并不执行数据包重组。在正常运行中，IP分段不会重叠，但攻击者可以人为地创建分段的数据包来误导路由器或防火墙。
- IP分段攻击的一个栗子是死亡Ping(Ping of Death)攻击。

<br/>

**TCP标志**

TCP Flags

- 使用TCP的数据交换发生在三次握手成功之后。此握手使用不同的标志来影响TCP分段的处理方式。
- TCP header中的标志位有6bits：
  - URG(urgent pointer field)
  - ACK(acknowledgement field)
  - PSH(push function)
  - RST(reset the connection)
  - SYN(synchronize sequence numbers)
  - FIN(the sender is finished with this connection)
- 攻击者可以利用这些标志来发起DoS攻击。

<br/>

![tcp三次握手](/images/SRE/tcp-3way-handshake.png)

<br/>


**SYN洪泛攻击**

SYN FLOOD

- 攻击者经常利用三次握手中的计时器来瘫痪服务，甚至瘫痪系统。
- 第二次握手后，接收SYN后的等待没有时间限制。攻击者向webserver发起许多连接请求（几乎可以肯定是使用了欺骗性的IP地址）。
- webserver发送的数据包(第二次握手, SYN+ACK)发送回源IP不会被回复。这使得TCP会话处于半开放状态(half-open)。多个数据包就会导致多个半开放，消耗服务器资源。
- 基于服务器的硬件限制，TCP会话的数量是有限的。因此，一旦达到某个限制，服务器就会拒绝任何主机的连接尝试。在新建立连接之前，这些半开放连接需要完成或超时。


<br/>


**FIN攻击**

FIN Attack

- 在正常操作中，发送发设置TCP FIN标志，表示不再传输数据且可以关闭连接。
- 这是一种四次挥手机制。发送方和接收方都希望发送ACK数据包和接收FIN数据包。
- 在攻击期间，它试图终止连接，会构造一个欺骗性的FIN数据包。此构造的数据包还具有正确的序列号，因此数据包被目标主机视为有效。这些序列号易于预测。


<br/>
<br/>


#### 连接劫持

Connection Hijacking

![连接劫持](/images/SRE/connection-hijacking.png)

- 授权用户A通过TCP会话发送HTTP请求给webserver
- 只有数据包具有正确的SEQ/ACK号，webserver才接受来自用户的数据包。这些数字号对于服务端来区分不同的会话并确保它仍然于用户A交谈。
- 想象一下，攻击者使用正确的序列号开始向webserver发送数据包（欺骗的用户A的IP地址）。webserver接受数据包并递增ACK号。
- 与此同时，用户A继续发送数据包，但使用不正确的SEQ/ACK编号。由于发送未同步(unSYN)的数据包，当webserver接收时，所有来自用户A的数据都被丢弃。攻击者假装使用正确的数字号成为用户A。
- 最终结果是攻击者劫持了连接，用户A被完全混淆了，webserver回复假设攻击者正在发送正确的同步数据。


<br/>
<br/>


#### 缓冲区溢出

Buffer Overflow

- 缓冲区是一个临时的数据存储区域，用于存储程序代码和数据。
- 当程序或进程尝试将更多数据存储在缓冲区，而不是最初预期保存，则会发生缓冲区溢出。
- 缓冲区是内存中的临时存储位置，其以字节为单位存储固定数量的数据。
- 当建索数据可以存储在缓冲区位置多时，其它信息必须进入相临的缓冲区，会导致覆盖其上保持的有效数据。

<br/>

机制：

- 缓冲区溢出漏洞有多种类型。但所有缓冲区溢出攻击的整体目标是接管对特权程序的控制，如果可能的话，接管对主机的控制。
- 攻击者有两个目标要实现。首先，脏代码需要在程序的代码地址空间(memory)中提供。其次，特权程序应跳转到代码的特定部分，这确保了正确的参数加载到内存中。
- 第一种，通过在正确的地址空间中注入代码或使用现有代码并稍微修改特定参数。第二种复杂些，因为需要修改程序的控制流以使程序跳转到脏代码。

<br/>

对策：

- 最重要的方法是在编写正确的代码上有一个协调一致的重点。
- 确保程序的数据缓冲区地址空间的代码不可执行。


<br/>
<br/>


#### 更多欺骗

**地址解析协议欺骗**

Address Resolution Protocol Spoofing

- 地址解析协议(ARP)将IP地址解析或映射到MAC子层地址
- 使用ARP欺骗，攻击者可以通过欺骗主机的MAC地址。基本上，攻击者可以让本地网络上的任何主机或设备相信攻击者的工作站是可信任的主机。
- 通过在网络的所有主机和路由器中实施静态ARP表，可以防止ARP欺骗。

<br/>

**DNS欺骗**

- DNS欺骗是黑客让目标机器相信它要连接的系统是xxx的机器。
- 破解者修改了DNS映射记录。在某些情况下，DNS服务器遭到攻击。
- 为了对抗DNS欺骗，反向查找(reverse lookup)会检测这些攻击。反向查找验证名称和IP。



<br/>
<br/>
<br/>



## 网络攻防

Threats, Attacks, Defense: <https://linkedin.github.io/school-of-sre/level101/security/threats_attacks_defences/>


<br/>


### DNS保护

DNS Protection


<br/>


#### 缓存中毒攻击

Cache Poisoning Attack

由于缓存了DNS响应，因此可以快速响应DNS查询。DNS否定查询也会被缓存（如拼写错误的单词），并且所有缓存的数据都会定期地超时。

缓存中毒是所谓欺骗中的一个问题。该术语用于描述黑客通过伪造DNS映射将网站流量重定向到虚假网站的攻击。在这种情况下，攻击者尝试将互联网域的虚假地址插入DNS。如果服务器接受虚假记录，那就是缓存中毒，并且随后对域中的地址请求都将响应被攻击者控制的虚假地址。只要服务器缓存了假的条目，浏览器或电子邮件服务器就会自动转到受妥协的DNS服务器提供的地址。缓存条目的TTL通常是几个小时，从而会使得会有众多的用户受到攻击的影响。


<br/>
<br/>


#### DNSSEC

DNS Security Extension

上面这些DNS问题的长期解决方法是身份认证。如果解析器无法在响应中区分有效数据和无效数据，则添加源身份认证。

DNSSEC防止数据欺骗和损坏，并提供对服务器和请求的认证机制，以及建立真实性和完整性的机制。

在验证DNS响应时，每个DNS区域使用私钥签名其数据。特定记录的查询返回资源记录集和它的签名。解析器使用公钥来验证响应。

DNSSEC的目标是为没有机密性或DDoS保护的DNS响应提供身份验证和完整性。


<br/>
<br/>


#### BGP

边界网关协议(BGP, border gateway protocol)，它是一种路由协议，在多个自治系统(AS, Autonomous Systems)之间交换路由信息。

自治系统是路由器或网络的集合，通常在单个管理控制下具有相同的网络策略。

BGP告诉路由器，使用哪个hop可以到达目标网络。

BGP用于在AS(内部)和多个AS之间的路由器之间传递信息。

![BGP](/images/SRE/bgp.png)



<br/>
<br/>



### BGP如何工作

How BGP Works

BGP负责查找到目标路由器的路径（最短和最可靠）。

该决定是通过称为链路状态(link state)的协议完成的。使用链路状态协议，每个路由器向网络中的所有其它路由器广播其链接和IP子网的状态。然后，每个路由器从其它路由器接收信息，并构建整个网络的完整拓扑视图。下一跳(next-hop)路由表基于此拓扑视图。

链路状态协议使用的是计算机科学领域最出名的算法， 迪杰斯特拉最短路径算法。我们从路由器开始考虑到所有直接邻接的路径成本。然后 采取最短路径。然后，我们重新查找我们的所有邻居，我们可以使用成本信息来达到和更新我们的链路状态。我们继续采用最短路径，直到访问每个路由器。



<br/>
<br/>



### BGP漏洞

BGP Vulnerabilities

通过腐化BGP的路由表，能够影响互联网上的流量流向。此动作称为BGP劫持(hijacking)。

通过恶意源将伪造信息向分布式BGP路由数据库注入广告信息，意外地或路由器可以扰乱互联网骨干网络。

黑洞交通(Blackholing traffic)。黑洞路由即网络路由，即路由表条目，这无处可去，并且匹配路由前缀的数据包被丢弃或忽略。只能通过监控丢失的流量来检测黑洞路由。黑洞路由是针对许多常见病毒攻击的最佳防御，流量从被注入的机器到控制的主机被丢弃了。

2008年，巴基斯坦决定通过创建一个引入黑洞的BGP路由来阻塞Youtube。把路由信息传递给香港ISP，从那时起，意外地传播到世界其它地区，这意味着数百万被路由到这个黑洞，因此无法访问Youtube。

潜在地，BGP的最大风险发生在拒绝服务发现攻击(DoS)中，其路由器被超多它处理能力的请求数据包洪泛淹没。当网络开始携带过多的BGP消息时，发生网络过载和路由器资源耗尽。

路由器震荡(router flapping)是另一种类型的攻击。路由震荡是指BGP路由表的重复变化，通常一分钟好几次。以高速撤销和重新广告可能对路由器造成严重的问题，因为它们传播了路由通知。如果这些路由震荡足够快，如每秒50次，则路由器过载，最终阻止有效路由收敛。对互联网用户的潜在影响是消息传递的放缓，在某些情况下，可以根本不提供数据包。

<br/>

**BGP Security**

BGP安全建议使用BGP peer认证，因为它是防止恶意活动的最强大的机制之一。认证机制是IPsec或BGP MD5。其它方法，如前缀限制(prefix limits)来避免填充路由表。



<br/>
<br/.



### Web攻击

Web-Based Attacks


<br/>


#### HTTP响应拆分攻击

HTTP Response Splitting Attacks

此攻击可能发生在服务器脚本在没有适当预防的情况下在HTTP响应头部中嵌入用户数据。

当脚本在重定向响应(HTTP 3xx)的重定向URL中嵌入用户数据时，通常回发生这种情况。或者当脚本在响应设置Cookie在Cookie值中嵌入用户数据。

此攻击可用于执行Web缓存中毒和跨站点脚本攻击。

此攻击的攻击者发送一个HTTP请求，该请求强制Web服务器形成输出流，然后将目标解释为两个而不是一个HTTP响应。


<br/>
<br/>


#### 跨站点请求伪造

Cross-Site Request Forgery (CSRF or XSRF)

此攻击将受害者的浏览器授予向漏洞的Web应用程序发出命令。

漏洞是由浏览器引起的，包括用户身份认证数据，会话ID，IP地址，Windows域凭据等。

攻击者使用CSRF启动交易，如传输资金，登录登出用户，关闭账户，访问敏感信息和更改账户详细信息。

该漏洞由浏览器引起，浏览器会自动包含每个请求的凭据(如表单、脚本或图像)。CSRF也可以动态地构造为扩展点脚本攻击的有效载荷的一部分。

所有依赖于自动凭据的站点都很脆弱。流行的浏览器无法防止跨站点请求伪造。尽快登出高价值站点可以减轻CSRF风险。建议高价值网站必须要求客户端手动提供用于执行安全影响任何操作的相同的HTTP请求的身份认证数据（二次认证）。限制会话cookie的生命周期也可以减少其它恶意站点使用的机会。

OWASP推荐网站开发人员在于敏感业务函数关联的HTTP请求中包含所需的安全令牌，以便减轻CSRF攻击。


<br/>
<br/>


#### 跨站点脚本攻击

Cross-Site Scripting (CSS or XSS) Attacks

在动态生成的网页显示用户输入时发生跨站点脚本，如未正确验证的登录信息，允许攻击者将恶意脚本嵌入生成的页面中，然后在任何可查看站点的用户的机器上执行脚本。

如果成功，可利用跨站点脚本漏洞来操纵和窃取Cookie，创建可能误认为时有效用户，危及机密信息或在最终用户系统上执行恶意代码的请求。

此攻击涉及在受害者浏览器上执行恶意脚本。受害者只是用户主机，而不是服务器。


<br/>
<br/>


#### 文档对象模型跨站点脚本攻击

Document Object Model (DOM) XSS Attacks

基于XSS的DOM不需要Web服务器接收XSS有效载荷来实现成功的攻击。攻击者通过在客户端嵌入它们的数据来滥用运行时。攻击者可以强制客户端(浏览器)呈现页面，其中包含由攻击者控制的DOM的部分。

当页面呈现并由页面处理数据时，通常由客户端的HTML嵌入式脚本（如JS），页面的代码可能不安全地载入页面本身中的数据，从而提供跨站点脚本有效载荷。有几个DOM对象可以作为攻击车轮，用于将恶意脚本提供给受害者浏览器。


<br/>
<br/>


#### 点击劫持

Clickjacking

该技术通过在合法站点的内容下面隐藏恶意链接/脚本来工作。

网站上的按钮实际上包含隐形链接，攻击者也可以放置在那。因此，点击一个视觉上的对象的人实际上被欺骗到访问恶意页面或执行恶意脚本。当鼠标移动到点击劫持时，结果是毁灭性的。

尚未有有效防御点击劫持的方法，禁用JS是唯一可行的方法。



<br/>
<br/>



### 数据库攻防

DataBase Attacks Defenses


<br/>


#### SQL注入攻击

SQL injection Attacks

它利用数据库查询中不正确的输入验证。一个成功的利用将允许攻击者访问、修改或删除数据库中的信息。它允许攻击者窃取存储在受影响网站的后端数据库中的敏感信息。

```sql
SELECT USERNAME,PASSWORD from USERS where USERNAME='<username>' AND PASSWORD='<password>';

# Here the username & password is the input provided by the user. Suppose an attacker gives the input as " OR '1'='1'" in both fields. Therefore the SQL query will look like:

SELECT USERNAME,PASSWORD from USERS where USERNAME='' OR '1'='1' AND PASSOWRD='' OR '1'='1';

# This query results in a true statement & the user gets logged in. This example depicts the bost basic type of SQL injection
```


<br/>
<br/>


#### SQL注入攻防

SQL注入可以通过过滤查询来消除恶意的语法来保护，这涉及使用一些工具来扫描源代码。

输入字段应限制为绝对最小，通常为7-12字符，并验证任何数据。



<br/>
<br/>



### VPN

VPN(virtual private network)是一种通过共享公共基础设置提供安全、可靠连接的服务。思科将VPN定义为私有网络之间通过公共网络的加密连接。目前为止，有三种类型的VPN：

- Remote access
- Site-to-site
- Firewall-base


<br/>
<br/>



### 安全漏洞

Security Breach

任何导致违反机密性、完整性、可用性(CIA, confidentiality, integrity, availability)的事件都可能导致安全漏洞。


<br/>


#### 拒绝服务攻击

DoS, Denial of Service Attacks

DoS攻击会导致宕机或用户无法访问系统。它会影响信息系统安全的可用性。它通过占用计算机来执行大量非必要的任务来拒绝服务的协调尝试。这种过度的活动使得系统无法执行有效操作。

两种常见类型的DoS攻击：

- 逻辑攻击(Logic attacks)，使用软件缺陷来崩溃或阻塞 远程服务器的性能。你可以安装软件的最新补丁来防止这些攻击。
- 洪泛攻击(Flooding attacks)，通过发送大量的无用请求，泛滥攻击使得受害者机器CPU、内存、网络瘫痪。如SYN洪泛攻击。

对DoS攻击的最佳防御之一是使用入侵防御系统(IPS, instrusion prevention system)的软件或设备，来检测和停止攻击。


<br/>
<br/>


#### 分布式拒绝服务攻击

DDoS, Distributed Denial of Service Attacks

DDos攻击中，攻击者劫持了成千上万台互联网计算机，在这些系统中种植自动攻击代理。然后，攻击者指示代理用伪造消息轰炸目标网站。这使得该网站过载并组织了许多合法的请求。这里的关键是代理的数量。


<br/>
<br/>


#### 窃听

Wiretapping

虽然窃听术语通常与语言电话通信相关联，但攻击者还可以使用窃听以拦截数据通信。

攻击者可以偷听电话线和数据通信线。窃听可能是主动地，攻击者对该线路进行修改；也可能是被动地，其中未经授权的用户只是在不改变内容的情况下侦听传输。被动入侵可以包括用于随后活动攻击的数据复制。

主动窃听有以下两种：

- 在线路之间窃听(between-the-lines)，不改变合法用户发送的消息，但是当合法用户暂停时，该类型的窃听会向其中插入额外的数据。
- 背驼式窃听(Piggyback-entry)，拦截并修改原始信息，通过打破通信线路并将消息路由到充当主机的零一计算机。


<br/>
<br/>


#### 后门

Backdoors

软件开发人员有时在他们的程序中包含了隐藏的访问方式，称为后门。后门让开发人员或支持人员能够轻易地访问系统，而无需与安全控件斗争。问题是后门并不总是保持隐藏。当攻击者发现后门时，可以利用后门来绕过现有的安全控制。



<br/>
<br/>



### 恶意攻击

Malicious Attacks


<br/>


#### 生日攻击

Birthday Attack

一旦攻击者妥协了了哈希密码文件，就会执行生日攻击。它是一种加密攻击，用于制作单向哈希的蛮力攻击。这是一种数学漏洞，是基于概率理论的生日问题。


<br/>
<br/>


#### 暴力密码攻击

Brute-Force Password Attacks

在暴力密码破解中，攻击者会在系统上尝试不同的密码，直到其中一个成功。通常，攻击者使用软件程序，以尝试可能的密码组合，直到密码匹配。


<br/>
<br/>


#### 字典密码攻击

Dictionary Password Attacks

字典攻击是一个简单的攻击，依赖于用户选择的糟糕的密码。一个简单的密码程序从字典文件中获取所有单词，并将其作为密码尝试。


<br/>
<br/>


#### 重放攻击

Replay Attacks

重放攻击涉及从网络捕获数据包，并重新调整它们以产生未经授权的效果。收到重复、经过验证的IP数据包可能会扰乱服务或其它不期望的后果。当攻击者重用旧消息来欺骗系统用户时，可以通过重放攻击来攻破系统。这有助于入侵者获取到未经授权访问系统的信息。


<br/>
<br/>


#### 中间人攻击

Man-in-the-Middle Attacks

中间人攻击中，攻击者在将他们转移到预期目的地之前拦截两方之间的消息。

网站欺骗(web spoofing)是一种中间人攻击类型，用户认为使用特定的web服务器会存在安全的会话。实际上，安全连接仅存在于攻击者，而不是web服务器。攻击者与web服务器建立安全连接，充当不可见的中间人。攻击者传递用户和web服务器之间的流量。通过这种方式，攻击者可以欺骗用户提供密码、信用卡和其它私有数据信息。


<br/>
<br/>


#### 伪装

Masquerading

在伪装攻击中，一个用户或计算机会假装成另一个用户或计算机。它通常包括其它形式的主动攻击，如IP地址欺骗或重放。攻击者可以捕获身份验证序列，然后重放它们以再次登录到应用程序或操作系统。例如，攻击者可能会监视发送到弱web服务器的用户名和密码。然后，攻击者可以使用截取的凭据模拟用户登录web程序。


<br/>
<br/>


#### 窃听

Eavesdropping

当主机在混杂模式下设置其网络接口时，会发生窃听或嗅探，并在通过以后分析通过的数据包。混杂模式使网络设备能够在给定时间内拦截和读取每个网络数据包，即使数据包的地址与网络设备不匹配。可以附加硬件和软件来监视和分析该传输介质上的所有数据包，而无需警告其他用户。

窃听的候选方法包括卫星、无线、移动和其它传输方式。


<br/>
<br/>


#### 社会工程学

Social Engineering

攻击者经常使用一种称为社会工程学的欺骗技术来访问IT基础设施中的资源。在几乎所有的情况下，社会工程都涉及欺骗授权用户为未授权用户执行操作。社会工程学攻击的成功取决于人们想要提供帮助的基本倾向。


<br/>
<br/>


#### 窃听

Phreaking

电话窃听，或简称窃听，是一个俚语，描述了研究、试验或探索电话系统、电化设备等的人们的文化活动。窃听是利用电话系统中存在的错误和故障的艺术。


<br/>
<br/>


#### 网络钓鱼

Phishing

网络钓鱼是一种欺诈，攻击者视图诱骗受害者提供私人信息。


<br/>
<br/>


#### 域名欺诈

Pharming

域名欺诈是另一种类型的攻击，旨在通过域欺骗来获取个人或私人财务信息。它不使用消息来诱骗受害者进入访问似乎合法的欺骗网站。相反，它中毒一个DNS上的域名，称为DNS中毒的过程。结果是，当用户请求中毒服务器的web地址，会被导航到攻击者的网站。用户的浏览器仍然显示正确的网站，这使得域名欺诈难以检测，因此更严重。




<br/>
<br/>
<br/>





## 编写安全的代码

Writing Secure Code: <https://linkedin.github.io/school-of-sre/level101/security/writing_secure_code/>

减少安全性和可靠性问题的第一步也是最重要的步骤是教育开发人员。然而，即使是最佳培训的工程师也会犯错误，安全专家也可能会写出不安全的代码，SRE也可能错过可靠性问题。很难让许多考虑因素和权衡的同时建立安全和可靠的系统，特别是如果你也负责生产软件。


<br/>


### 使用框架来加强安全性和可靠性

Use frameworks to enforce security and reliability while writing code

一个更好的方法是在通用框架、语言和库中处理安全性和可靠性。多个应用程序使用同一个库或框架，如果存在安全问题，修复框架就能快速修复问题。


<br/>
<br/>


### 常见安全漏洞

Common Security Vulnerabilities

OWASP和SANS发布的常见漏洞类列表：

| OWASP前十漏洞 | 框架强化措施 |
| - | - |
| SQL Injection | TrustedSQL String |
| Broken Authentication | Require authentication using well tested mechanisms like OAuth before routing a request to the application. |
| Sensitive data exposure | Use distinct types to store and handle sensitive data. |
| XML External Entities | Use XML parser without XXE enabled. |
| Broken Access Control | Requires every request handler or RPC to have well defined access control restrictions. |
| Security Misconfiguration | Use a technology stack that provides secure configurations by default and restricts or doesn't allow risky configuration options. |
| Cross-Site Scripting XSS | Use an XSS hardened template system. |
| Insecure Deserialization | Use deserialization libraries that are build for handling untrusted inputs. |
| Usint Components with Known Vulnerabilities | Choose libraries that are popular and actively maintained. |
| Insufficient Logging Monitoring | Log and monitor requests and other events as appropriate in a low level library. |



<br/>
<br/>


### 编写简单代码

Write Simple Code

尝试使你的代码清洁和简单。


<br/>
<br/>


#### 避免多级嵌套

Avoid Multi-Level Nesting

多级嵌套是一种常见的反模式(anti-pattern)，可能会导致简单的错误。如果错误位于最常见的代码路径中，则它可能会被单元测试给捕获到。然而，单元测试并不总是检查多级嵌套代码中的错误处理路径。该错误可能会导致可靠性降低或安全漏洞。


<br/>
<br/>


#### 消除YAGNI气味

Eliminate YAGNI Smells

你不需要它（YAGNI, You Aren't Gonna Need It)

有时，开发人员通过添加可能在将来有用的功能，以防万一来开发出的解决方案。这增加了不必要的复杂性，因为它需要记录、测试和维护。这与YAGNI原则相悖，这建议你仅实施你需要的代码。

总而言之，避免YAGNI代码能够提高可靠性，更简单的代码导致的安全问题也较少，维护和开发时间也较少。


<br/>
<br/>


#### 偿还技术债务

Repay Technical Debt

你需要有一个清晰的过程（分配时间）来偿还相关债务。


<br/>
<br/>


#### 重构

Refactoring

重构是保持代码库情节和简单的最有效的方法。无论重构的原因如何，你都应该始终遵循一个黄金原则：切勿将重构和功能改变混为代码库的单个提交。重构变化通常很大，并且很难理解。如果提交还包含了功能更改，则审阅这可能会忽略错误。


<br/>
<br/>


#### 单元测试

Unit Testing

单元测试可以在发版之前确定各个软件组件中的广泛错误，提高系统安全性和可靠性。该技术涉及将软件组件打破到更小的、自包含的单元，其没有外部依赖，然后测试每个单元。


<br/>
<br/>


#### 模糊测试

Fuzz Testing

模糊测试是一种用来补充前述测试技术的技术。涉及使用模糊引擎产生大量候选输入，然后通过模糊驱动传递给模糊目标。然后，模糊器分析系统如何处理输入。如文件解码器、压缩算法、音频解码器等。


<br/>
<br/>


#### 集成测试

Integration Testing

集成测试超出各个单元和抽象，替换数据库或网络服务的伪造或停顿实现，具有真实的实现。因此，集成测试锻炼更完整的代码路径。由于你必须初始化和配置其它依赖，因此集成测试可能比单元测试更慢。要执行测试，此方法包含实际遍历，如服务通信中端到端的网络延迟。


<br/>
<br/>


#### 其它重要部分

- Code Reviews
- Rely on Automation
- Don’t check in Secrets
- Verifiable Builds






<br/>
<br/>

---

<br/>
<br/>








# Linux中级

Linux-Intermediate: <https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introduction/>


<br/>
<br/>


## 包管理

Package Management: <https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management/>

包管理是一种在任何操作系统上安装和维护软件程序的方法。

在Linux早期，人们需要下载软件源代码来编译和安装它。随着Linux变得成熟，开始了以包的形式来分发软件。包文件是包含软件，依赖，安装说明和元数据的压缩集合 。

软件包很少是独立的，它依赖于不同的软件，库和模块。包管理可以解决这个艰难的依赖关系并安装它们。

存储库是所有包 ，更新，依赖的存储位置。每个仓库都可以包含成千上万的软件包。

<br/>

```bash
# 更新包信息
dnf update

# 列出所有帮库
dnf repolist all

# 添加仓库
dnf config-manager --add-repo http://www.example.com/example.repo
```

<br/>

有两种主要类型的包管理工具：

- 低级工具(low-level)：主要用于安装、删除和升级包文件（dpkg, dnf）
- 高级工具(high-level)：除了低级的功能，还可以进行元数据搜索和依赖解析（apt-get, dnf）



<br/>
<br/>



## 存储介质

Storage Media: <https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/>

存储介质是用于存储数据和信息的设备(device)。有多种物理存储设备（硬盘等），虚拟存储设备（RAID，LVM，网络存储等）。

使用`mount`命令查看和挂载存储设备，当然还有`/etc/fstab`文件使之变得简单。

文件系统负责任何存储设备上的数据存储、索引和检索。一些常用的文件系统：

- FAT
- NTFS
- ext
- ext4
- xfs
- HFS
- HFS+
- NFS

<br/>

文件系统有可能因为一些原因（电源故障、不当关机等）而遇到问题，Linux通常在启动期间检查和修复损坏的磁盘。可以使用`fsck`命令来手动检查和修复文件系统损坏。


<br/>
<br/>


### RAID

独立冗余磁盘阵列(RAID, Redundant Arrays of Independent Disks)，在多个磁盘上分配I/O，以实现更高的性能和数据冗余的技术。软件RAID使用计算机的CPU进行RAID操作，而硬件RAID在磁盘控制器上使用专用处理器来管理磁盘。

RAID的三个基本特性：

- 镜像(mirroring)
- 条纹化(striping)
- 奇偶性(parity)

<br/>

RAID级别：

- RAID 0(Striping)
- RAID 1(Mirroring)
- RAID 4(Striping with dedicated parity)
- RAID 5(Striping with distributed parity)
- RAID 6(Striping with double parity)
- RAID 10(RAID 1+0, Mirroring and Striping)


<br/>
<br/>


### LVM

逻辑卷管理(LVM, Logical Volume Management)

使用LVM可以在存储分配中实现更大的灵活性，如可以缝合三个2TB磁盘以制作6TB的单个分区，或者可以将4TB的另一个物理磁盘连接到服务器，并将该磁盘添加到逻辑卷组中使其总量为10TB。



<br/>
<br/>



## 归档和备份

Archiving and Backup: <https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/>




<br/>
<br/>



## VIM



<br/>
<br/>



## Bash脚本








<br/>
<br/>

---

<br/>
<br/>











# Linux高级


<br/>


## 容器和编排

Containers and orchestration: <https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro/>

容器、Docker和K8s很酷！



<br/>
<br/>



### 容器介绍

容器是一个标准的软件单元，打包代码和所有依赖，因此应用程序可以从一个计算环境到另一个计算环境快速且可靠地进行交付。

Why containers:

- 快速可靠地交付
- 环境变量的一致性
- 在单台主机上运行多个容器
- 每个容器都有自己孤立的环境（这意味着单片应用可以分解为微服务并打包到容器，孤立的环境不会让一个容器的应用程序的失败影响另一个，这称为故障隔离(fault isolation)）
- 资源配额


<br/>
<br/>


#### 虚拟机和容器

虚拟机(VM, virtual machine)和容器(container)。

类似于在单个主机上运行多个容器，我们也可以在单个主机上运行多个VM，以这种方式，可以在单独的虚拟机中运行应用程序（或每个微服务）。

这里的主要关注点是虚拟机和容器的大小。虚拟机包含了访客操作系统(guest os)，因此会比容器更重。

![VM](/images/SRE/VM.png)

![Container](/images/SRE/Containers.png)


<br/>
<br/>


#### 容器如何实现

容器与主机操作系统共享相同的内核(kernel)，并为应用程序提供孤立的环境。没有像虚拟机那样在主机操作系统上运行guest os的额外开销，要感谢Linux内核的两个功能：cgroups和namespace。

容器是一个Linux进程或一组Linux进程，容器外部的进程是受限的（由namespace实现），使用的资源量（由cgroup实现），可在容器内部进行系统调用。


<br/>
<br/>


#### Namespace

容器内进程的可见性应限制在其自身内。这是Linux Kernel namespace所做的。这个的思路是命名空间中的进程不能影响那些它看不到的进程。共享单个命名空间的进程具有其所在命名空间内唯一的身份、服务和接口。

这是Linux命名空间列表：

- **Mount**：共享mount namespace的进程组共享一组单独的私有挂载点和文件系统视图。对这些命名空间挂载点所做的任何修改在此命名空间外都是不可见的。
- **PID**：pid namespace中的进程尽在命名空间内具有唯一的进程号。一个进程可以是它自己的pid命名空间中的根进程(root process, pid 1)，并且在它下面有一个完整的进程树。
- **Network**：每个network namespace都有自己的网络设备实例，可以配置单独的网络地址。同一个网络命名空间中的进程可以有自己的端口和路由表。
- **User**：user namespace可以有自己的uid和gid。在主机中使用非特权用户的进程可能在用户命名空间中拥有root用户身份。
- **Cgroup**：允许创建只能在cgroup namespace内使用的cgroup。
- **UTS**：uts namespace有自己的主机名和域名IPC。每个IPC namespace都有自己的System V和POSIX消息队列。

<br/>

看起来很复杂，但在Linux中创建namespace很简单。

示例，在Linux中创建一个PID namespace。

```bash
# 查看主机系统上运行的进程
ps aux

# 使用unshare命令创建一个PID namespace
# 并在此命名空间中创建一个bash进程
sudo unshare --fork --pid --mount-proc bash
ps aux

# 在同一个命名空间中创建一个其它进程
sleep 1000&
ps aux

# 在新开的终端，查看sleep
ps aux | grep sleep

# 这两者的PID不一样，但其实指向的是相同的进程。
```


<br/>
<br/>


#### Cgroup

cgroup(control group)可以定义为一组进程的计量和监控资源使用。资源可以是memory page, disk io, cpu等。事实上，cgroup是基于资源限制，并在违反了限制时采取何种行动。

cgroup中的组件，其追踪c资源使用率和控制进程行为，称为资源子系统(resource-subsystem)或资源控制器(resource controller)。

以下是EHEL对cgroup的资源控制器的介绍：

- **blkio**：此子系统限制块设备的IO
- **cpu**：限制进程的CPU使用量
- **cpuacct**：生成cgroup中进程的CPU资源使用的报告
- **cpuset**：为cgroup中的进程分配单独的CPU(多核)和内存节点
- **devices**：控制进程能够访问某些设备
- **freezer**：挂起或恢复cgroup中的进程
- **memory**：限制进程的内存使用量
- **ns**：控制cgroup中的进程使用不通的namespace

cgroup遵循每个资源控制器的分层，树形结构，即每个控制器都存在一个cgroup。层次结构中的每个cgroup从它的父亲(parent cgroup)处继承某些属性。

<br/>

示例，配置cgroup。

```bash
# 查看cgroup工具
mount | grep "^cgroup"
# 如果没有，则安装工具
# sudo yum install libcgroup-tools -y

# 创建一个属主为root的memory cgroup
sudo cgcreate -a root -g memory:mem_group
# 查看
# /sys/fs/cgroup/<cgroup type>
ls -l /sys/fs/cgroup/memory | grep 'mem_group'

# 查看内存限制，mem_group继承的父亲的属性
cat /sys/fs/cgroup/memory/mem_group/memory.limit_in_bytes
# 查看parent cgroup
cat /sys/fs/cgroup/memory/memory.limit_in_bytes

# 现在减少它的内存限制为20KB
echo 2000 > /sys/fs/cgroup/memory/mem_group/memory.limit_in_bytes
cat /sys/fs/cgroup/memory/mem_group/memory.limit_in_bytes
# 限制很低，因此附加到mem_group的大多数进程应该都会被OOM杀死。

# 创建一个新shell附加到mem_group
cgexec -g memory:mem_group bash
Killed
# 如预期，进程被OOM杀死了。你可以使用dmesg日志(mm_fault_error)确认情况。
```

<br/>

容器与底层主机操作系统共享相同的内核，并且在内部提供应用程序的孤立环境。cgroup帮助管理容器中的进程使用的资源，namespace帮助隔离环境（network stack, pids, uids, gids, mount points），使运行在同一个主机上的其它容器相隔离。


<br/>
<br/>


#### 容器引擎

容器引擎(container engine)简化了在主机中创建和管理容器的过程。怎样做？

- 容器创建流程通常以容器镜像开始。
- 容器镜像构建、推送和拉取。
- 容器引擎解包镜像并为此应用创建孤立的环境。
- 容器镜像中的文件将挂载到孤立的环境中，以在容器内启动并运行应用程序。

有许多容器引擎，如Docker, RKT, LXC，它们需要不同的镜像格式。OCI(Open Container Initiative)是由Docker启动的协作项目，该项目旨在跨供应商间标准化容器运行时和镜像格式。



<br/>
<br/>



### 使用Docker进行容器化

Containerization With Docker: <https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/>

有关Docker详情参考Docker文档。



<br/>
<br/>



### 使用K8s进行编排

Orchestration With Kubernetes：<https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/>

有关K8s详情参考K8s文档。




<br/>
<br/>



## 系统调用和信号

System Calls and Signals：<https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/intro/>

对信号和系统调用的基本理解。


<br/>


### 信号

Signals：<https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals/>

中断(interrupt)是改变程序正常执行流程的事件，可由硬件设备甚至CPU本身产生。当中断发生时，当前的执行流暂停，中断处理程序运行。中断处理程序运行后，恢复先前的执行流。

有三种类型的事件可能导致CPU中断：硬件中断，软件中断和异常。

信号只不过是软件中断，通知进程发生的事件。这些事件可能是来自用户的请求，或发生的系统问题。每个信号都有一个信号编号和定义的默认操作。

进程可以通过一下任何方式对它们做出反应：

- 操作系统提供的默认方式
- 以程序预定义的方式来捕获并处理信号
- 完全忽略信号


<br/>
<br/>


#### 信号类型

Types of signals

你可以使用`kill -l`命令，来列出Linux系统上的可用信号。下表列出了1到20的信号。

| Signal name | Signal number | Default Action | Meaning |
| - | - | - | - |
| SIGHUP | 1 | Terminate | Hangup detected on controlling terminal or death of controlling process |
| SIGINT | 2 | Terminate | Interrupt from keyboard |
| SIGQUIT | 3 | Core dump | Quit from keyboard |
| SIGILL | 4 | Core dump | Illegal instruction |
| SIGTRAP | 5 | Core dump | Trace/breakpoint trap for debugging |
| SIGABRT , SIGIOT | 6 | Core dump | Abnormal termination |
| SIGBUS | 7 | Core dump | Bus error |
| SIGFPE | 8 | Core dump | Floating point exception |
| SIGKILL | 9 | Terminate | Kill signal(cannot be caught or ignored) |
| SIGUSR1 | 10 | Terminate | User-defined signal 1 |
| SIGSEGV | 11 | Core dump | Invalid memory reference |
| SIGUSR2 | 12 | Terminate | User-defined signal 2 |
| SIGPIPE | 13 | Terminate | Broken pipe;write pipe with no readers |
| SIGALRM | 14 | Terminate | Timer signal from alarm |
| SIGTERM | 15 | Terminate | Process termination |
| SIGSTKFLT | 16 | Terminate | Stack fault on math co-processor |
| SIGCHLD | 17 | Ignore | Child stopped or terminated |
| SIGCONT | 18 | Continue | Continue if stopped |
| SIGSTOP | 19 | Stop | Stop process (can not be caught or ignore) |
| SIGTSTP | 20 | Stop | Stop types at tty |



<br/>
<br/>



### 系统调用

系统调用是内核的受控入口点，允许进程请求内核来执行一些在此进程上的操作。内核通过系统调用应用程序编程接口(system call API)为程序提供一系列服务。应用程序开发人员通常没有直接访问这些系统调用，但可以通过API访问它们。

例如，这些服务包括创建新的进程，执行IO，创建进程通信的管道。系统调用组合是固定的，每个系统调用由唯一的数字表示。

系统调用将处理器状态从用户模式(user mode)更改为内核模式(kernel mode)，以便CPU可以访问受保护的内核存储。每个系统调用可能具有一组参数，其指定要从用户空间(进程的虚拟地址空间)传输到内核空间的信息，反之亦然。从编程角度来看，调用系统调用看起来很想调用C函数。


<br/>
<br/>


#### 系统调用类型

主要有五种不同的系统调用。

- 进程控制(process control)：用于处理与进程创建、终止等进程相关的任务。
- 文件管理(file management)：用于读写文件等文件相关的操作。
- 设备管理(device management)：用于处理设备读写入设备缓冲区等。
- 信息维护(information maintenance)：处理信息及在操作系统和用户程序之间的转换。
- 通信(communication)：用于内部进程间的通信。

<br/>

| 系统调用类型 | Linux示例 |
| - | - |
| 进程控制 | `fork()`, `exit()`, `wait()` |
| 文件管理 | `open()`, `read()`, `write()` |
| 设备管理 | `ioctl()`, `read()`, `write()` |
| 信息管理 | `getpid()`, `alarm()`, `sleep()` |
| 通信 | `pipe()`, `shmget()`, `mmap()` |


<br/>
<br/>


#### 用户态和内核态

User mode, kernel mode and their transitions

现代处理器架构通常允许CPU至少有两种模式可以操作：用户态和内核态。相应地，虚拟内存区域可以被标记为用户空间或内核空间的一部分。在用户态下运行时，CPU只能访问标记在用户空间中的内存，尝试访问内核空间中的内存会导致硬件异常。

在任何给定时间，一个进程在用户态或内核态执行。可以执行的指令类型却决于模式，这在硬件级别强制执行。CPU模式(处理器模式)是一些计算机架构的中央处理单元的操作模式，该架构将限制CPU运行的某些进程执行的操作的类型和范围。内核本身不是一个进程，而是一个进程管理器。内核态假定需要内核服务的进程使用特定的编程构造——称为系统调用。

在用户态下执行程序时，它无法直接访问内核数据结构或内核程序。但是，当应用程序在内核态下执行时，这些限制不再适用。程序通常在用户态下执行，并且仅在请求内核提供的服务时切换到内核态。如果应用程序需要访问系统上的硬件资源（如外设、磁盘、内存等），则必须发出系统调用，上下文从用户态切换到内核态。读写文件时，遵循此过程。它在内核态下运行系统调用自身，而不是应用程序代码。当系统调用完成后，使用反向上下文切换，进程返回到用户态。


<br/>
<br/>


#### 使用strace在Linux上Debug

strace是一个用于追踪用户进程和Linux内核之间转换的工具。

```
 ~]$ strace printf %s "Hello world"
execve("/usr/bin/printf", ["printf", "%s", "Hello world"], [/* 47 vars */]) = 0
brk(NULL)                               = 0x90d000
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8fc672f000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=98854, ...}) = 0
mmap(NULL, 98854, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f8fc6716000
close(3)                                = 0
open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20&\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=2156160, ...}) = 0
mmap(NULL, 3985888, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f8fc6141000
mprotect(0x7f8fc6304000, 2097152, PROT_NONE) = 0
mmap(0x7f8fc6504000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c3000) = 0x7f8fc6504000
mmap(0x7f8fc650a000, 16864, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f8fc650a000
close(3)                                = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8fc6715000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8fc6713000
arch_prctl(ARCH_SET_FS, 0x7f8fc6713740) = 0
mprotect(0x7f8fc6504000, 16384, PROT_READ) = 0
mprotect(0x60a000, 4096, PROT_READ)     = 0
mprotect(0x7f8fc6730000, 4096, PROT_READ) = 0
munmap(0x7f8fc6716000, 98854)           = 0
brk(NULL)                               = 0x90d000
brk(0x92e000)                           = 0x92e000
brk(NULL)                               = 0x92e000
open("/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=106075056, ...}) = 0
mmap(NULL, 106075056, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f8fbfc17000
close(3)                                = 0
open("/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=2502, ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8fc672e000
read(3, "# Locale name alias data base.\n#"..., 4096) = 2502
read(3, "", 4096)                       = 0
close(3)                                = 0
munmap(0x7f8fc672e000, 4096)            = 0
open("/usr/lib/locale/UTF-8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 1), ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8fc672e000
write(1, "Hello world", 11Hello world)             = 11
close(1)                                = 0
munmap(0x7f8fc672e000, 4096)            = 0
close(2)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

<br/>

```
execve("/usr/bin/printf", ["printf", "%s", "Hello world"], [/ 47 vars /]) = 0
```

第一个系统调用`execve()`做以下三件事：

- 操作系统停止重复的进程
- 操作系统载入新程序(printf)，并启动这个新进程
- execve替换使用从printf可执行文件加载的新内容定义当前进程的内存堆栈

execve是被调用执行的系统调用的名称。第一个参数必须是二进制可执行文件或脚本的路径。第二个参数是传递给新程序的字符串数组。按照惯例，字写字符串数组的第一个应该包含与正在执行的文件相关联的文件名。第三个参数必须是环境变量。等号(=)后面的数字是execve系统调用返回的值，此处0表示调用成功。

<br/>

```
open("/usr/lib/locale/UTF-8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
```

这行，程序尝试`open()`文件`/usr/lib/locale/UTF-8/LC_CTYPE`。然而，系统调用失败了(状态码为-1)，错误消息为No such file or directory，文件未找到(ENOENT)。

<br/>

```
brk(NULL) = 0x90d000

brk(0x92e000) = 0x92e000

brk(NULL) = 0x92e000
```

系统调用`brk()`用于增加或减少进程的数据段(data segment)。它返回进程数据段结束的新地址。

<br/>

```
open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3

read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20&\2\0\0\0\0\0"..., 832) = 832
```

在`open()`中，第一个参数是你要使用的文件路径，第二个参数定义权限。O_RDONLY表示只读，O_CLOEXEC表示为打开文件启用close-on-exec标志。这有助于避免多线程程序中的竞态。3表示用于打开文件的文件描述符。第一个未使用的文件描述符是3。

文件描述符(fd, file descriptor):

- 0: stdin
- 1: stdout
- 2: stderr

在`read()`中，第一个参数是文件描述符，也就是3。第二个参数是要从中读取数据的缓冲区。第三个参数是缓冲区的长度。返回值为832，即读取的字节数。

<br/>

```
close(3) = 0
```

系统调用`close()`用于内核关闭文件描述符。对于大多数文件系统，程序使用`close()`系统调用终止对文件系统中文件的访问。0表示系统调用成功。

<br/>

```
write(1, "Hello world", 11Hello world) = 11
```

第一个参数为文件描述符，1表示标准输出。第二个参数是包含要写入信息的缓冲区。最后一个参数是字符串数目。成功时，返回写入的字节数。（此处为11，0表示没有写入）

<br/>

```
+++ exited with 0 +++
```

这表示程序成功退出，退出码为0。在Linux上，状态码为0表示程序成功执行和退出。

<br/>

你不需要记住所有的系统调用，你可以在需要时参考文档。

```bash
man 2 execve
```

除了查看系统调用，strace还可用于检测程序正在访问的文件。

strace还可用于检查程序是否挂起(hanging)或卡住(stuck)。当我们追踪时，我们也可以观察程序卡在哪个操作上，我们还可以找到错误来指出程序挂起/卡住的原因。strace非常有助于找出程序性能缓慢背后的原因。

尽管strace如此有帮助，但如果在生产环境中运行追踪，strace不是一个好的选择，它会引入大量的开销。





<br/>
<br/>

---

<br/>
<br/>









# 网络

[Networking](https://linkedin.github.io/school-of-sre/level102/networking/introduction/)

术语(Terminology):

- Cloud(云)：如AWS，GCP
- On-prem(本地)：如自建的物理数据中心
- Leaf switch(叶交换机)
- Spine switch：聚合多个叶交换机
- DataCenter(DC)：数据中心
- DC fabric
- Cabinet：机架，一个机柜指整个机架
- BGP(Border Gateway Protocol)
- VPN(Virtual Private Network)
- NIC(Network Interface Card)
- Flow：指两个节点之间的流量交换
- ECMP(Equal Cost Multi-Path)
- RTT：实时响应时间
- TCP throughput
- Unicast(单播)
- Anycast
- Multicast(多播)


<br/>
<br/>


## 安全

[Security](https://linkedin.github.io/school-of-sre/level102/networking/security/)

本节将涵盖服务内外部面临的威胁。这将触及周边安全性，DDoS保护，网络分界和操作实践。


<br/>
<br/>


### 安全威胁

Security Threat

安全是任何基础架构中的首要考虑因素之一。有各种各样的安全威胁，可能会增加数据盗窃，服务损失，欺诈活动等。

攻击者可以使用如下技术进行攻击：

- 网络钓鱼(Phishing)
- 垃圾邮件(Spamming)
- 恶意软件(Malware)
- 拒绝服务攻击(Dos/DDos)
- 漏洞(Exploiting vulnerabilities)
- 中间人攻击(man-in-the-middle attack)


<br/>
<br/>


### 保护基础设施

Securing the infrastructure

一旦知道了不同的威胁，必须设计和实现安全防御机制。保护基础设施的一些手段：

**周边安全**(Perimeter security)

这是任何基础设施中防御的第一行，当不需要/意外的流量流入基础架构设施，它们应该被过滤/阻止。这些可以在边缘路由器中过滤，允许预期服务（如80/443），或设置过滤器阻止不需要的流量。

如，云安全组，第一条先默认拒绝所有，然后在此之上添加允许。如允许公网访问80,443。允许白名单访问ssh和一些特定服务。

<br/>

**DDoS缓解**(mitigation)

防止DDoS攻击也是一个重要的方面。攻击流量类似于正常的用户/客户端请求，但有意洪泛外部暴露的应用程序。因此，必须在攻击流量和用户流量之间进行区分。为此，存在不同的方法在应用程序级别执行，其中一个方法是使用验证码来捕获机器流量。

对于这些有用的方法，节点应该能够处理攻击流量和正常流量。在基于云的基础架构中，可能可以通过快速添加资源以处理突峰流量；而在自建数据中心，添加资源可能具有挑战性。

为了处理大量的攻击流量，有可用的解决方案。可以检查数据包/流量，并识别不像正常连接的异常流量。如客户端启动TCP连接，但无法完成握手，或一组来源，有巨大的异常流量。一旦识别出这种不需要的流量，可在网络边缘丢弃这些流量，从而保护应用程序节点的资源。

<br/>

**网络划分**(Network Demarcation)

网络分界是根据其安全性需求和攻击漏洞，将应用程序分组到不同网络中部署的一种策略。一些常见的划分，外部/互联网面对节点被分组到单独的区域，而具有敏感数据的节点被分离到另外单独的区域。在安全工具的帮助下，限制这些区域之间的任何通信，以限制不需要的访问。这些内部区域间的通信过滤器有时称为环形栅栏(ring-fencing)。

创建多区域的主要优点是，即使存在受损的主机，它也不会充当基础设施其余部分的后门。

<br/>

**节点保护**(Node protection)

服务器，路由器，交换机，负载均衡器，防火墙等，每个设备都具有某些功能来保护自己。

<br/>

**其它实践**

安全事件处理的标准操作流程(STANDARD OPERATING PROCEDURES)：

- 当安全事件发生时，告知谁
- 确定安全事件的规模和严重程度
- 使用哪些解决方案来缓解安全事件
- 有关安全事件的数据用于进一步分析

定期审查(PERIODIC REVIEW)基础设施的整体安全性：

- 识别可能瞄准基础设施的安全威胁
- 定期审查安全事件处理标准流程
- 确保软件更新/补丁及时完成
- 审计基础设置中任何不合规的安全标准
- 审查最近的安全事件，并找到提高防护的机制



<br/>
<br/>
<br/>




## 规模

[Scale](https://linkedin.github.io/school-of-sre/level102/networking/scale/)

部署大规模应用程序，需要更好地了解基础架构功能，如资源可用性，域名故障，伸缩选型(如使用多播)，四/七层负载均衡器，基于DNS的负载均衡。

建立大规模应用是一个复杂的活动，应该涵盖设计、开发和运营中的许多方面。


<br/>
<br/>


### 域名故障

Failure domains

在任意基础设施中，由于硬件或软件问题导致的故障很常见，应该将这些失败保持到最低限度。

<br/>

**服务器故障**

Server failures

由于硬件或软件问题，服务器可能会失败。这可能导致某些数据包丢失。因此，在设计服务时，应该考虑对于此故障的容错性。

<br/>

**ToR failures**

连接服务器的叶片交换机(leaf switch)不可用，这也是一个常见的问题。需要计划在不重新加载其它服务器的情况下处理多少服务器丢失。基于此，服务可以分布在多个机柜上。

<br/>

**站点故障**

站点故障是一个通用术语，它可能意味着一个网站中的特定服务挂了（新版问题、防火墙问题、网络问题、DNS问题、LB问题等）。这些问题可能不常见，但会产生重大影响。

总之，在设计应用程序本身时必须考虑处理这些失败场景。应用程序内提供容错，以从意外的故障中恢复。


<br/>
<br/>


### 资源可用性

Resource availability

另一个考虑的方面时基础结构的可用性，并且取决于服务的功能。

有如下伸缩项：

- 多播(anycast)
- 负载均衡器(load balancer)
- 基于DNS的负载均衡(DNS based load balancing)

<br/>

**多播**

这是在多个机柜中推出流量分布最快的方式。

为了在这些服务器上实现流的比例分布，重要的是在每个cabinet和pod中保持均匀性。但请记住，分发只会基于流，如果有任何大流量，则某些服务器可能会收到更高的流量。

![多播配置](/images/SRE/Anycast.png)

<br/>

**负载均衡器**

另一个常见的方法时使用负载均衡器。

负载均衡器可以在单臂(single-arm)模式下部署，其中到VIP的流量由LB重定向，从服务器返回的流量直接发送到客户端。另一种是双臂模式，其中返回流量也通过LB。

![single-arm](/images/SRE/lb-single-arm.png)

![two-arm](/images/SRE/lb-two-arm.png)

<br/>

**基于DNS的负载均衡**

与上述类似，唯一的区别是负载均衡在DNS处完成。客户端在查询服务的DNS记录的时候，会获取到不同的IP地址来连接。DNS服务器必须进行健康检查，以了解哪个服务器处于良好状态。



<br/>
<br/>



## RTT

延迟(latency)在确定分布式服务的整体性能方面发挥着关键作用。

往返时延(RTT, Round-Trip Time)是时间的度量，数据包从A到B并返回A所需的时间。它以毫秒为单位。它的影响体现在不同服务器/服务间的调用中，以及可以实现的TCP吞吐量。

服务对其集群内的服务器或不同服务（如认证、日志、数据库等）进行多次调用，以响应每个客户端请求，这是相当常见的。这些服务器可以分布在不同的机柜，甚至不同的数据中心。随着RTT的增加，每个调用的响应时间变长，从而对发送给用户的最终响应产生级联效应。



<br/>
<br/>










## 基础设施服务

Infrastructure Services

<br/>

### ToR连通性

ToR(The Onion Router, 洋葱路由器)

这是常见的故障点，可使用不同的选项将服务器连接到ToR。


















<br/>
<br/>

---

<br/>
<br/>




























# 系统设计

[System Design](https://linkedin.github.io/school-of-sre/level102/system_design/intro/)

我们对应用的基本要求是：

- 为大量合理的用户工作
- 避免服务故障/集群奔溃

换句话说，我们希望构建一个可用，可扩展和容错的系统。


<br/>
<br/>


## 大型系统设计



















































<br/>
<br/>

---

<br/>
<br/>






















# 系统设计

System Design: <https://linkedin.github.io/school-of-sre/level102/system_design/intro/>























<br/>
<br/>

---

<br/>
<br/>



















# 故障排除和性能优化

[System troubleshooting and performance improvements](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/introduction/)

本章提供有关解决系统问题的一般介绍，如api故障，资源利用，网络问题，硬件和操作系统问题。还介绍了系统性能分析(profiling)和基准测试(benchmarking)。

故障排查无法通过完成一门课程来学习，它是一个持续学习的过程。包括：

- 日常运行和开发
- 查找和修复应用bug
- 查找和修复系统和网络问题
- 性能分析和改进
- 等

<br/>

从SRE的角度来看，理解以下方面：

- 了解资源，如主机规范，CPU、内存、磁盘、网络等
- 理解系统设计和架构
- 确保正确收集重要的指标

What gets measured gets fixed.


<br/>
<br/>


## 故障排除

Troubleshooting

系统故障的排除有时可能会棘手或乏味。在这种情况下，我们需要检查服务的端到端(end-to-end)流程，所有下游，分析日志，内存泄露，CPU使用，磁盘IO，网络错误，主机问题等。了解某些实践和工具可以帮助更快地减轻故障。

故障排除流程图：

![故障排查](/images/SRE/TroubleshootingFlow.jpg)


<br/>
<br/>


### 一般做法

General Practices

- 复现问题(Reproduce problem)
  - 尝试破坏的请求来重现问题。
  - 检查端到端的请求流并查看返回码(3xx, 4xx, 5xx)。
  - 客户端侧的问题主要是静态内容的缺失或错误，异步调用等。
- 收集信息(Gather Information)
  - 查找应用日志中的错误或异常（Can’t Allocate Memory，OutOfMemoryError，disk I/O error等）
  - 检查应用和主机的指标，查看展示图。看是否有资源使用异常等情况。
  - 查看最近的代码和配置更改，可能正在破坏系统。
- 了解问题(Understand the problem)
  - 尝试将收集的数据和最近的操作相关。
  - QPS增加？SQL查询增加？最近代码的更改是否需要更好更多的硬件配置？
- 寻找解决方案并修复(Find a solution and apply a fix)
  - 基于上述发现，如有可能，可以快速修复。如果错误异常相关，则可以滚动回滚。
  - 尝试修复代码，如果你想要快速修复，先在staging环境。
  - 尝试扩展系统
  - 如果有必要，优化数据库查询
- 验证完整的请求流程(Verify complete request flow)
  - 请求并查看返回码
  - 查看日志
  - 查看监控指标


<br/>
<br/>


### 一般主机问题

要知道主机是否监控，可尝试以下：

- Dmesg: 显示近期内核抛出的错误
- ls命令：lspci, lsblk, lscpu, lsscsi
- `/var/log/messages`：显示系统相关的错误
- Smartd：检查磁盘健康



<br/>
<br/>



## 重要工具

Important Tools

一些重要的Linux命令，能帮助快速找到问题。

- 日志解析：grep, sed, awk, cut, tail, head, more, less
- 网络检查：nc, netstat, traceroute, mtr, ping, route, tcpdump, ss, ip
- dns：dig, host, nslookup
- 追踪系统调用：strace
- 通过ssh并行执行：gnu parallel, xargs ssh
- http检查：curl, wget
- 打开文件数列表：lsof
- 修改内核属性：sysctl

一些第三方工具也很有用：

- 基于ssh的工具：clusterSSH, Ansible
- 基于agent的工具：saltstack, puppet

<br>

日志分析工具，elk。可通过多个logstash消费kafka日志，使用同一个group，这样其实也就是logstash的高可用了。



<br/>
<br/>



## 性能提升

Performance Improvements

性能工具是开发、运营生命周期的重要组成部分，对于理解应用程序的行为非常重要。


<br/>


### 性能分析命令

Performance analysis commands

这些命令你应该知晓：

- top
- htop
- iotop
- vmstat
- iostat
- free
- sar
- mpstat
- lsof
- perf


<br/>
<br/>


### 分析工具

Profiling tools

一些分析工具可帮助理解：

- FlameGraph: 可视化
- Valgrind: 用于内存调试、内存泄露检测和分析的编程工具
- Gprof: GNU Profiler tool采取用于收集运行时分析信息


<br/>
<br/>


### 基准测试

Benchmarking

这是一种测量服务最佳性能的过程。如服务能处理的QPS，负载增加时的延迟，主机资源使用等。回归测试(regression/load testing)是在部署服务到生产环境之前必须要做的事。

一些工具：

- Apache Benchmark Tool
- Httperf
- Apache JMeter
- Wrk
- Locust

上面的工具有助于负载、压力测试，但这样做不测量实际的用户体验，它看不出最终用户资源如何影响应用程序性能。


<br/>
<br/>


### 伸缩

Scaling

> 推荐阅读<br>
> [A Brief History of Scaling LinkedIn](https://engineering.linkedin.com/architecture/brief-history-scaling-linkedin)

<br/>

根据资源的可用性，系统设计的最佳地执行一定的限制。始终需要持续优化，以确保其峰值处于最佳利用资源。随着QPS的增长，系统需要扩展，可以垂直或水平伸缩。

伸缩Web应用需要以下内容：

- 添加更多主机来缓解服务器负载
- 使用负载均衡器在服务器上分发流量
- 通过分片和增加副本数来扩展数据库



<br/>
<br/>
<br/>




## 故障排查示例

[Troubleshooting Example](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting-example/)


<br/>


### 内存泄露

Example - Memory leak

内存泄露问题通常会被忽视，直到服务在运行一段时间后变得无响应，直到服务重启或错误修复。在这种情况下，服务内存使用量类似与此图。

![内存泄露](/images/SRE/MemUsageChart.png)

内存泄露是应用程序对内存分配的不当管理，其中不需要的内存没有被释放，在时间段里对象继续堆积在内存中，导致服务奔溃。通常，这些未释放的对象通过垃圾回收器(gc)自动地排序，但有时由于bug它失败了。调试有助于诊断应用程序的大部分内存用在了什么地方。然后，你开始追踪并过滤基于内存使用的所有内容。如果，你发现了不使用的对象，但在引用，你可以通过删除它们来避免内存泄露。

在以下的Python应用程序示例中，它附带了像tracemalloc等内置功能，此模块可以帮助指出对象首先分配的位置。几乎每种语言都带有一组工具/库来帮助查找内存问题。同样，对于Java而言，有一个名为Java VisualVM内存泄露检测工具。

要模拟进行大量的请求，我们可以使用Apacher benchmarking工具ab，测试之后可以看到内存猛增。

```bash
ab -n 10000 -c 10 -s 3 http://localhost:port
```

现实世界应用程序比示例更为复杂，你必须了解使用TraceMalloc可能会降低应用程序性能，因为TraceMalloc也有开销。注意其在生产环境中的使用。






<br/>
<br/>

---

<br/>
<br/>












# CICD

Continuous Integration and Continuous Delivery

涉及的知识:

- cicd
- devops
- pipeline
- 软件开发和维护
- Git
- Docker
- K8s
- Helm
- Jenkins/Gitlab-ci等

<br/>

基础设施即代码(infrastructure as code)是一个SRE在自动化重复配置任务方面应该遵顼的标准实践之一。每个配置都保持为代码，因此可以使用cicd pipeline来部署它。通过cicd pipeline将配置更改提供给生产环境，以维护版本控制，环境变化的一致性以及避免手动错误。

通常，作为SRE，你需要查看应用程序cicd pipeline，并推荐额外的阶段，如静态代码分析和代码中的安全行和隐私检查，以提高产品的安全性和可靠性。





















