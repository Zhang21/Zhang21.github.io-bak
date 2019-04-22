---
title: "SaltStack"
date: "2017-12-25 09:47:39"
tags: "SaltStack"
categories: DevOps
---


参考：
- SaltStack官网：<https://saltstack.com>
- SaltStack文档：<https://docs.saltstack.com/en/latest/topics>
- SaltStack-GitHub：<https://github.com/saltstack>
- Salt-repo：<https://repo.saltstack.com/>
- Salt TOC: <https://docs.saltstack.com/en/latest/contents.html>
- Salt Modules: <https://docs.saltstack.com/en/latest/salt-modindex.html>
- Salt Index: <https://docs.saltstack.com/en/latest/genindex.html>

环境：
- CentOS7_x64;
- Salt-2018.02



<br/>
<br/>

---

<!--more-->

<br/>
<br/>



# 说明

`SaltStack`是一种革命性的用速度(speed)取代复杂性(complexity)的基础设施(infrastucture)管理方法。

- 简单(Simple)，可以在几分钟内运行；
- 可伸缩性(Scalable)，足以管理数以万计的Server；
- 快速(Fast)，能在几秒内与各系统间进行通信。

<br/>

**You'll learn how to:**

- 安装和配置SaltStack；
- 在所有托管系统上远程执行命令(Remotely execute commands)；
- 设计、开发和部署系统配置；
- 使用Salt Reactor是基础设施自动化(automate)；
- 使用Salt Orchestration协调复杂管理操作。

![SaltStack](/images/Salt/SaltStack.png)

<br/>

Salt是建立在动态通信总线(dynamic communication bus)上的基础设施管理的一种新方法。Salt可以用于数据驱动(data-driven)业务，远程执行(remote execution)任何基础设施，配置管理(configuration management)任意应用堆栈...

- REMOTE EXECUTION;
- CONFIGURATION MANAGEMENT;
- EVENT-DRIVEN INFRASTRUCTURE;
- SALT ESSENTIALS.



<br/>
<br/>

---

<br/>
<br/>




# 词汇表

GLOSSARY


- **Auto-Order**: 按照在SLS文件中定义的顺序评估状态

- **Bootstrap**: 下载和安装Salt的一个项目

- **Compound Matcher**: 可以与布尔运算符组合的许多目标定义的组合

- **EAuth**: 外部认证(external authentication)的简写

- **Environment**: 包含可应用于minions的状态文件的目录树

- **Execution Function**: 执行模块中的Python函数，可以接受参数并执行特定的系统管理任务

- **Execution Module**: 包含执行功能的Python模块，可直接在服务器上执行各种系统管理任务。 Salt附带了许多执行模块，但用户也可以编写自己的执行模块来执行专门的任务

- **External Job Cache**: 外部数据存储，可以存档有关已运行作业的信息

- **External Pillar**: 一个接受任意参数并返回字典的模块

- **Event**: 发送到事件总线的通知

- **File Server**: 用于存储Salt特定文件的本地或远程位置

- **Grain**: 包含系统信息的键值对

- **Highdata**: SLS文件中的数据结构表示一组状态声明

- **Highstate**: 要应用于系统的状态集合

- **Idempotent**：在应用操作之前，无论系统状态如何，都确保系统处于已知状态的操作

- **Jinja**: 一种模板语言，允许变量和简单逻辑在呈现时动态插入到静态文本文件中

- **Job**: Salt命令要执行的完整任务集是单个作业

- **Job Cache**: 作业结果的存储位置，然后可由salt runner或外部系统查询

- **Job ID**: 作业的唯一标识符

- **Low State**: 评估requirements和order的已处理状态的集合

- **Master**: salt daemon，可向minions发出命令

- **Masterless**: 不需要Salt Master操作的minion，所有配置都是本地的

- **Master Tops**: 允许hook进入外部系统以生成顶级文件数据的系统

- **Mine**: 从minion收集任意数据并将数据存储在master上。这些数据可供其它 minions 使用

- **Minion**: Salt Minion daemon

- **Minion ID**: minion的全局标识符

- **Multi-Master**: 在高可用环境下，minion可以同时主动连接到多个Salt Master

- **Node Group**: 为minion进行逻辑分组

- **Outputter**: 定义Salt命令的输出数据格式的程序

- **Peer Communication**: minion与minion直接进行沟通，而不通过salt master

- **Pillar**: 为使用者提供用户定义的数据的键值对，通常用于存储敏感数据并将其分发给minion

- **Proxy Minion**: 可控制无法再本地运行salt minion的minion

- **PyDSL**: 用作Salt渲染器的Pythonic域特定语言

- **Reactor**: 一个接口，用于侦听事件和定义Salt在收到给定事件时应采取的操作

- **Render Pipe**: 将给定数据格式(yaml/json...)转换为可由salt使用的Python数据结构

- **Returner**: 允许将Salt命令的结果发送到给定的数据存储（例如数据库或日志文件）以进行存档

- **Roster**: 目标主机的文件列表

- **Runner Module**: 包含一组runner功能的模块

- **Runner Function**: 由`salt-run`命令调用并在master上(not minion)执行的函数

- **Salt Cloud**: 用于托管的云环境

- **Salt SSH**: 配置管理和远程编排系统，只需要SSH

- **Salt Thin**: 是salt发行版的一个子集，不包含任何传输例程

- **Salt Virt**: 用于管理虚拟机在一组主机上的创建和部署。通常用于创建和部署私有云

- **SLS Module**: 包含一组状态声明

- **State Compiler**: 将highdata转换为lowdata

- **State Declaration**: 一种数据结构，包含唯一ID并描述系统的一个或多个状态

- **State Function**: 包含在状态模块内的函数，该函数可以管理特定状态对系统的应用

- **State Module**: 包含一组状态函数的模块

- **State Run**: 在一组系统上应用一组状态

- **Syndic**: 一个转发器，可以在分层主服务器之间中继消息

- **Target**: 将要应用给定salt命令的minion

- **Top File**: 确定应将哪些SLS文件应用于各种系统，并将这些系统组织到环境中

- **__virtual__**: 模块负载上调用的模块中的函数，用于确定模块是否应该对minion可用

- **Worker**: 一个Master process， 可以发送通知并接收来自minion的回复




<br/>
<br/>

---

<br/>
<br/>




# 介绍

INTRODUCTION TO SALT

 We’re not just talking about NaCl.

<br>

**30s摘要**

Salt是：

- 配置管理系统，能够将远程节点维护在已定义的状态
- 分布式远程执行系统，用于在远程节点上单独或通过任意选择标准执行命令和查询数据

它的开发是为了将远程执行领域中发现的最佳解决方案结合在一起，使其更好，更快，更具可塑性。Salt通过简单易用的界面快速处理大量信息，而不仅仅是数十个甚至数千个单独的服务器。

<br>

**简单理解**

在大规模部署和较小系统之间提供多功能性似乎令人生畏，但无论项目规模如何，Salt的设置和维护都非常简单。Salt的体系结构旨在与任意数量的服务器协同工作，从少数本地网络系统到跨不同数据中心的国际部署。拓扑结构是一个简单的C/S模型，其中所需的功能内置于一组守护进程中。虽然默认配置几乎不需要修改，但可以对Salt进行微调以满足特定需求。

<br>

**并行执行(PARALLEL EXECUTION)**

Salt的核心功能:

- 使命令能够并行而不是串行地调用远程系统
- 使用安全和加密的协议
- 尽可能使用最小和最快的网络负载

Salt还为远程执行领域引入了更精细的控件，允许系统不仅可以通过主机名进行定位，还可以通过系统属性进行定位。

<br>

**建立在经过验证的技术上**

Salt使用了许多技术：

- 网络层使用优秀的 [ZeroMQ](http://zeromq.org/) 网络库构建，因此Salt守护程序包含一个可行且透明的AMQ代理。
- 使用公钥对Master Daemon进行身份验证，然后使用更快的AES加密进行负载通信。身份验证和加密是Salt的组成部分。
- 利用 [msgpack](https://msgpack.org/) 的通信功能，实现快速，轻便的网络流量。

<br>

**Python客户端接口**

为了允许简单的扩展，Salt执行例程可以编写为普通的Python模块。从Salt执行中收集的数据可以发送回Master或任意程序。可从简单的Python API或CLI调用Salt，这样Salt可用于执行一次性(one-off)命令以及作为更大应用程序的组成部分。

<br>

**快速、灵活、可扩展**

结果是一个系统可以在从一个服务器到很多服务器的目标服务器组上高速执行命令。Salt非常快速，易于设置，具有惊人的可塑性，并提供单一的远程执行架构，可以管理任意数量服务器的各种需求。Salt基础设施汇集了最佳的远程执行世界，扩大了其功能并扩展了其范围，从而使系统具有实用性，适用于任何网络。

<br>

**开放**

Salt是在Apache License 下开发的，可用于开放和专有项目。请将您的扩展提交回Salt项目，以便我们可以随着Salt的发展一起受益。

<br>

**社区**

加入Salt，有很多方法可参与Salt社区并进行沟通。Salt有一个活跃的IRC频道和邮件列表。

<br>

**GitHub**

Salt: <https://github.com/saltstack/salt>

<br>

**Blog**

Salt Blogs: <https://www.saltstack.com/blog/>

<br>

**SALT STATES**

`salt-states` 官方库: <https://github.com/saltstack/salt-states>




<br/>
<br/>

---

<br/>
<br/>




# 安装

Installation


如果是第一次设置环境，你应该在专用的管理服务器上安装`Salt master`，然后在每个使用Salt管理的系统上安装`Salt minion`。现在不需要担心你的系统架构(architecture)，你可以在以后轻易添加组件(componet)和修改配置(configuration)而不需要重新安装任何东西。

The general installation process is as follows:

- 安装`Salt master`，通过各平台说明安装或通过Salt `bootstrap.sh `脚本来安装；
- 确保你的`Salt minion`能够找到`Salt master`；
- 在想要管理的每个系统上安装`Salt minion`；
- 在`Salt minion`连接后接受`Salt minion key`。

在此之后，就可以**运行一个简单命令**，并从所有的`Salt minion`**接收返回**。

```sh
#salt <minion-id> <cmd>

salt minion1 test.ping
salt * test.ping
```


<br>
<br>

## 快速安装

Quick install


在绝大多数发行版本上，可以使用**Salt Bootstrap**脚本进行快速安装。

参考：[Salt Bootstrap](https://docs.saltstack.com/en/latest/topics/tutorials/salt_bootstrap.html#salt-bootstrap)

```
#wget
wget https://bootstrap.saltstack.com -O bootstrap-salt.sh
sh bootstrap-salt.sh

#curl
curl -o bootstrap-salt.sh -L https://bootstrap.saltstack.com
sh bootstrap-salt.sh
```


<br>
<br>


## 指定平台

Platform-Specific Installation

[选择发行版本安装](https://docs.saltstack.com/en/latest/topics/installation/index.html#platform-specific-installation-instructions)


<br>


### CentOS7

repo: <https://repo.saltstack.com/#rhel>

**1. 下载SaltStack-Repository进行安装：**

`systemd`和`systemd-python`是Salt必须的，在安装Salt前需装好。

```sh
#安装salt-repo
yum install -y https://repo.saltstack.com/yum/redhat/salt-repo-latest-2.el7.noarch.rpm


yum clean expire-cache


#安装salt组件
yum install -y salt-master salt-minion salt-ssh salt-syndic salt-cloud salt-api


#开启
systemctl start salt-master
```

<br/>

**2. 自建salt-repo：**

```sh
vim /etc/yum.repos.d/saltstack.repo


[saltstack-repo]
name=SaltStack repo for Cent0S7
baseurl=https://repo.saltstack.com/yum/redhat/$releasever/$basearch/latest
enalbed=1
gpgcheck=1
gpgkey=https://repo.saltstack.com/yum/redhat/$releasever/$basearch/latest/SALTSTACK-GPG-KEY.pub
```



<br>
<br/>


## 初始化

Initial Configuration


### 配置salt

Configuration Salt

`salt-master` 的默认配置会为安装而工作，唯一要求是在`salt-minion`的配置文件中设置`salt-master`的位置。


<br>


#### salt-master

默认的，`salt-master`配置文件位于`/etc/salt/master`，在all interfaces(0.0.0.0)上监听`4505`和`4506`端口。

```sh
vim /etc/salt/master

interface: 0.0.0.0
```


<br>


#### salt-minion
默认，一个`salt-minion`会尝试连接到DNS名称为`salt`。如果`salt-minion`能够正确解析(resolve)这个名称，则可以不需要配置文件。如果DNS名称`salt`未能解析为`salt-master`的正确位置，那么可在`/etc/salt/minion`配置文件下重新定义`salt`。

```sh
vim /etc/salt/minion


#master: salt
#如果是默认的salt,需要在本地hosts下解析salt
#此处我们修改为salt-master的IP地址
master: 192.168.1.9
```

**修改配置文件后，请重启服务。**


<br>


#### minion代理配置

PROXY MINION CONFIGURATION

A Proxy Minion模仿一个规律的行为和继承(inherit)他们的选项。
类似地，它的配置文件存放于`/etc/salt/proxy`，`proxy`也将尝试连接DNS名为`salt`的主机。

除了`salt-minion`有规律的选型，`proxy`还有一些特定的选项。参考:[Proxy minion](https://docs.saltstack.com/en/latest/ref/configuration/proxy.html#configuration-salt-proxy)


<br>


#### 运行Salt

以`salt`命令运行:

```sh
salt-master
#开启守护进程
salt-master -d

#systemd
systemctl start salt-master



salt-minion
salt-minion -d
systemctl start salt-minion



#日志信息
salt-master --log-level=debug
```

**以non-root运行salt**
- 确保此用户有相应的权限；
- 可能需要修改相应目录的权限：
	+ /etc/salt
	+ /var/cache/salt
	+ /var/log/salt
	+ /var/run/salt


<br/>


#### 密钥识别

Key Identity

在`initial key`交换之前，`Salt`会提供命令来验证(validate)`salt-master`和`salt-minion`的身份。
验证身份有助于避免疏忽地连接到错误的`salt-master`，并且在建立初始化连接的阻止MiTM攻击。

**Master Key Fingerprint**
复制`master.pub`的值，并将其作为`salt-minion`配置文件`/etc/salt/minion`中`master_finger`的值。

```sh
#salt-key is used to manage Salt authentication keys

#查看master的key
salt-key -F master

Local Keys:
master.pem:  60:87:25:6a:68:28:4a:bf:5e:87:ee:4f:3f:46:d4:8e:38:8b:58:d9:8a:f4:44:b6:64:67:d9:da:0f:5d:f3:b4
master.pub:  46:52:c1:36:f2:6f:33:c0:72:a1:18:5e:99:36:04:ea:1a:9b:ea:e7:61:3b:d9:30:34:c1:f1:3b:65:08:f8:42
#将公钥写入salt-minion配置文件


#查看minion的finger
#salt-key --finger <minion_id>
salt-key --finger '192.168.1.7'
```

**Minion Key Fingerprint**

```sh
#salt-call is used to execute module functions locally on a Salt Minion


#查看minion key fingerprint
#可在master上查看，比对两者是否相同
salt-call --local key.finger
```


<br>


#### 密钥管理

Key Management

Salt使用AES Encryption加密`salt-master`与`salt-minion`间的所有通信。这确保了发送到Minion的命令不会被篡改(tamper)，并保证了master与minion间是认证的和受信任的。

当命令发送到`salt-minion`之前，`salt-minion`的key必须要被`salt-master`所接受。

```sh
#列出salt-master上已知的keys
salt-key -L
```

其中包含四项:

- Accepted Keys:
- Denied Keys:
- Unaccepted keys:
- Rejected keys:

**让`salt-master`接收key，并允许`salt-minion`被`salt-master`控制**

```sh
#-a 192.168.1.7, --accept=192.168.1.7
#-A, --accept-all

salt-key -A
```


<br>


#### 发送命令

Sending Commands

`salt-master`和`salt-minion`之间通过运行`test.ping`命令来证实(verified)。

```sh
salt 192.168.1.7 test.ping

salt * test.ping
```



<br/>
<br/>



## 其它安装指南

Additional Installation Guides


<br/>


### Salt Bootstrap

Salt Bootstrap脚本允许用户在各种系统和版本上安装`salt-minion`和`salt-master`。shell脚本为`bootstrap-salt.sh`，运行一系列的检查来确定操作系统的类型和版本，然后通过适当的方法安装salt二进制文件。salt-bootstrap脚本安装运行`salt`的最小化安装包，如Git便不会安装。

Salt Bootstrap's GitHub: <https://github.com/saltstack/salt-bootstrap>


<br>


#### 栗子

Satl Bootstrap脚本有多种可以传递的选项，以及获取引导脚本本身的方法。

**1. 使用`curl`**

```
curl -o bootstrap-salt.sh -L https://bootstrap.saltstack.com
sh bootstrap-salt.sh git develop
```

**2. 使用`wget`**

```
wget -O bootstrap-salt.sh https://bootstrap.saltstack.com
sh bootstrap-salt.sh
```

**3. An Insecure one-liner**

```
curl -L https://bootstrap.saltstack.com | sh

wget -O - https://bootstrap.saltstack.com | sh

curl -L https://bootstrap.saltstack.com | sh -s -- git develop
```

**4. cmd line options**

```
#查看帮助
sh bootstrap-salt.sh -h
```


<br/>


### 防火墙

`salt-master`和`salt-minion`间的通信使用AES加密的`ZeroMQ`，它使用TCP的4505和4506端口，仅需要在`salt-master`上可访问就行。

下面概述了关于`salt-master`的防火墙规则：

**RHEL7/CENTOS7**

```
firewall-cmd --permanent --zone=<zone> --add-port=4505-4506/tcp
firewall-cmd --reload
```


<br/>

### Preseed minion with accepted key

某些情况下，在`salt-master`上接受`minion-key`之前等待`salt-minion`启动是不方便的。比如，你可能希望`minion`一上线(online)就引导。

有多种方式生成`minion-key`，下面是一般生成`minion-key`的四个步骤：

1. 在master上生成key：

```
#请给key取个名字
salt-key --gen-keys=[key_name]
```

2. 把公钥(publick key)添加到已接受的minion文件夹中:

公钥文件和 minion_id 有相同的名字是很有必要的，这就是Salt如何通过key与minions匹配。
还有，由于不同操作系统或特定的master配置文件，pki 文件夹可能位于不同的位置。

```
cp <key_name>.pub /etc/salt/pkimaster/minions/<minion_id>
```

3. 分配`minion-key`：

对于minion来说，没有单一方法去得到密钥对，难点是找到一种安全的分配方法。

由于`master`已经接受了`minion-key`，因此分发私钥(private key)会有潜在的安全风险。

4. 配置带key的minion：

你可能希望在启动`salt-miniont daemon`之前取得`minion-key`的位置。

```
/etc/salt/pki/minion/minion.pem
/etc/salt/pki/minion/minion.pub
```


<br/>


### 以普通用户运行root

Running salt as normal user tutorial

**以普通用户(non-root)运行salt function**

如果你不想使用`root`用户安装或运行salt，你可以在你的工作目录中创建一个虚拟根目录(virtual root dir)来配置它。
salt system使用`salt.syspath`module来查找变量。

如果你运行salt-build，它会生成在: `./build/lib.linux-x86_64-2.7/salt/_syspaths.py`；

运行`python setup.py build`命令来生成它；

复制生成的module到你的salt dir，`cp ./build/lib.linux-x86_64-2.7/salt/_syspaths.py ./salt/_syspaths.py`

修改它，并加入需要的变量和新路径：

```python
# you need to edit this
ROOT_DIR = *your current dir* + '/salt/root'

# you need to edit this
INSTALL_DIR = *location of source code*

CONFIG_DIR =  ROOT_DIR + '/etc/salt'
CACHE_DIR = ROOT_DIR + '/var/cache/salt'
SOCK_DIR = ROOT_DIR + '/var/run/salt'
SRV_ROOT_DIR= ROOT_DIR + '/srv'
BASE_FILE_ROOTS_DIR = ROOT_DIR + '/srv/salt'
BASE_PILLAR_ROOTS_DIR = ROOT_DIR + '/srv/pillar'
BASE_MASTER_ROOTS_DIR = ROOT_DIR + '/srv/salt-master'
LOGS_DIR = ROOT_DIR + '/var/log/salt'
PIDFILE_DIR = ROOT_DIR + '/var/run'
CLOUD_DIR = INSTALL_DIR + '/cloud'
BOOTSTRAP = CLOUD_DIR + '/deploy/bootstrap-salt.sh'
```

创建目录结构：

```
mkdir -p root/etc/salt root/var/cache/run root/run/salt root/srv
root/srv/salt root/srv/pillar root/srv/salt-master root/var/log/salt root/var/run
```

填充配置文件：

```
cp -r conf/* /etc/salt/


vi /etc/salt/master

user: *your user name*
```

运行：

```py
PYTHONPATH=`pwd` scripts/salt-cloud
```


<br>


### minion独立运行

Standalone minion

因为`salt-minion`包含了如此广泛的功能，它可以独立运行。
一个独立的minion可以用来做很多事情:

- 在没有连接到`master`的系统上使用`salt-call`命令；
- 无主状态(masterless states)。

当以无主模式运行salt时，不要运行`salt-minion daemon`。否则，它将尝试连接到master并失败。
`salt-call`命令是独立的，不需要`salt-minion daemon`。


<br/>


#### minion配置

有几个参考方法来设置不同的选项来配置`masterless minion`，`salt-minion`很容易通过配置文件(默认位于:`/etc/salt/minion`)进行配置。

<br/>

**告诉salt运行masterless**

`salt-call`命令用于在`salt-minion`本地运行模块功能，而不是在`salt-master`执行他们。通常，`salt-call`命令检查主机检索文件服务器和支柱数据，当时当运行`standalone salt-call`时，需要指示不要检查master的这些数据。
为了指示`minion`不要查找`master`，需要在运行`salt-call`时设置`file_client`配置选项。默认情况下，`file_client`被设置为`remote`让`minion`知道将从`master`中收集文件服务器和支柱数据。当设置`file_client`为`local`时，`minion`将不会从`master`收集这些数据。

```yaml
file_client: local

#这样，salt-call命令将不会查找master
#并认为本地系统拥有所有的文件文支柱资源
```

<br/>

**masterless运行状态**

the state system在所有需要的文件都在`minion`本地，轻易地在没有`salt-master`的情况下运行。为了达到此效果，需要配置`minion`配置文件，以了解如何像`master`一样返回file_roots信息。

```yaml
file_roots:
  base:
    - /srv/salt
```

现在设置salt state tree, top file和SLS modules，就像在`master`上设置它们一样。将`file_client`设置为`local`，并且一个可用的state tree会调用state module中的function，将使用`minion`上的file_roots中的信息而不是`master`。

当在一个`minion`上创建一个`state tree`时，不需要语法或路径的更改。`master`上的SLS modules不需要进行任何修改就可以与`minion`一起工作。这就使得salt scrit不需要设置一个`master`就能轻易部署，并允许这些SLS modules随着部署发展而容易转移到`master`。

```sh
#以声明的状态可以执行
salt-call state.apply


#无需修改配置文件
salt-call state.apply --local
```


<br>


### Salt无主模式

Salt masterless quickstart

运行一个无主模式的minion可以允许你在单一主机上使用salt配置管理，而不用在另一台主机上调用master。
在无主模式下运行salt时，请勿运行salt daemon。否则，它将尝试连接到master并失败。`salt-call`命令时独立的

<br>

**bootstrap salt minion**

```
curl -L https://bootstrap.saltstack.com -o bootstrap_salt.sh
sudo sh bootstrap_salt.sh
```

<br>

**告诉salt运行masterless模式**
在minion配置文件中配置此，表示不去寻找master，并假设本地系统拥有所有文件和资源。

```
vim /etc/salt/minion

file_client: local
```

<br>

**创建状态树(state tree)**

1. 创建`top.sls`文件

```
vim /srv/salt/top.sls

base:
  '*':
    - webserver
```

2. 创建webserver状态树

```
vim /srv/salt/webserver.sls
#这是基于Debian

apache:               # ID declaration
  pkg:                # state declaration
    - installed       # function declaration
```

3. `salt-call`
`salt-call`命令在minion本地运行远程执行功能，而不是在master执行。

```
#--local,在本地文件系统查找状态树
salt-call --local state.apply

#minion首先检查top.sls，然后应用webserver.sls
```




<br/>
<br/>

---

<br/>
<br/>




# 配置

Configuring Salt


本节介绍如何配置用户访问，查看，存储作业结果，安全，疑难解答以及如何执行其它管理任务。


<br/>


## master

CONFIGURING THE SALT MASTER

- master config: <https://docs.saltstack.com/en/latest/ref/configuration/master.html>

<br>

salt系统的两个组件都有相应的配置文件:

- `/etc/salt/master`
- `/etc/salt/minion`


<br/>


### master配置项

PRIMARY MASTER CONFIGURATION


**基础配置**

```
#INTERFACE，默认值0.0.0.0
interface: 0.0.0.0


#IPv6，默认值False
ipv6: False


#PUBLISH_PORT，默认值4505
publish_port: 4505


#MASTER_ID，默认值None
master_id: Master


#USER，默认值root
user: root


#ENALBE_SSH_MINIONS，默认值False
enable_ssh_minions: True


#RET_PORT，默认值4506
#接收命令执行返回的端口
ret_prot: 4506


#PIDFILE，默认值/var/run/salt-master.pid
pidfile: /var/run/salt-master.pid


#ROOT_DIR，默认值/
root_dir: /


#CONF_FILE，默认值/etc/salt/master
conf_file: /etc/salt/master


#PKI_DIR，默认值/etc/salt/pki/master
#存储pki认证密钥的目录
pki_dir: /etc/salt/pki/master


#EXTENSION_MODULES，默认值/var/cache/salt/master/extmods
extension_modules: /root/salt_extmods


#EXTMOD_WHITELIST/EXTMOD_BLACKLIST
#有效的选项: modules, states, grains, renderers, returners, output, proxy, runners,
#wheel, engines, queues, utils, pillar, sdb, cache, clouds, tops, roster, tokens
extmod_whitelist:
  modules:
    - custom_module
  engines:
    - custom_engine
  pillars: []

extmod_blacklist:
  modules:
    - specific_module


#MODULE_DIRS，默认值[]
module_dirs:
  - /var/cache/salt/minion/extmods


#CACHEDIR，默认值/var/cache/salt/master
cachedir: /var/cache/salt/master


#VERIFY_ENV，默认值True
#在启动时验证并设置目录权限
verify_env: True


#KEEP_JOBS，默认值24
#设置保留旧作业信息的小时数，0表示禁用缓存清理
keep_jobs: 24


#GATHER_JOB_TIMEOUT，默认10
#客户端请求正在运行的作业信息是等待的秒数
gather_job_timeout: 10


#TIMEOUT，默认值5
#salt命令和api默认超时值
timeout: 5


#LOOP_INTERVAL，默认值60
#维护过程检查周期的秒数
loop_interval: 60


#OUTPUT，默认值nested
#设置命令使用的默认输出器
output: nested


#OUTPUTTER_DIRS，默认值[]
#salt输出器附加目录列表
outputter_dirs: []


#OUTPUT_FILE，默认值None
#salt命令使用的默认输出文件
output_file: /path/output/file


#SHOW_TIMEOUT，默认值True
#告诉client已超时的minion
show_timeout: True


#SHOW_JID，默认值False
#告诉client在工作发布时显示jid
show_jid: False


#COLOR，默认值True
#彩色输出
color: False


#COLOR_THEME，默认值""
color_theme: /etc/salt/color_theme


#CLI_SUMMARY，默认False
#显示目标minion数量的摘要
cli_summary: False


#SOCK_DIR
#Default: /var/run/salt/master
#创建Unix socket的位置
sock_dir: /var/run/salt/master


#ENABLE_GPU_GRAINS
#Default: True
#启用GPU硬件数据


#JOB_CACHE
#Default: True
#维护一个临时作业缓存
job_cache: True


#MINION_DATA_CACHE
#Default: True
#存储在master端的minion数据缓存
minion_data_cache: True


#CACHE
#Default: localfs
#缓存子系统模块用于minion数据缓存
cache: consul


#MEMCACHE_EXPIRE_SECONDS
#Default: 0，禁用
#内存缓存数据过期时间
memcache_expire_seconds: 30


#MEMCACHE_MAX_ITEMS
#Default: 1024
#缓存项限制
memcache_max_items: 1024


#MEMCACHE_FULL_CLEANUP
#Default: False
#如果缓存已满(超过max_literms)，则项目将清除其存储
memcache_full_cleanup: True


#MEMCACHE_DEBUG
#Default: False
#收集缓存统计信息并记入调试日志级别
memcache_debug: True


#EXT_JOB_CACHE
#Default: ''
#指定所有minion的默认returner
ext_job_cache: redis


#EVENT_RETURN
#Default: ''
#指定用于记录时间的returner
event_return:
  - syslog
  - splunk


#EVENT_RETURN_QUEUE
#Default: 0
#在繁忙的系统上，启用event_returns可能会给存储系统造成相当大的负载。事件可以在master使用队列排队，并以批处理方式使用单个事务存储多个事件
event_return_queue: 0


#EVENT_RETURN_WHITELIST
#Default: []
event_return_whitelist:
  - salt/master/a_tag
  - salt/run/*/ret


#EVENT_RETURN_BLACKLIST
#Default: []
event_return_blacklist:
  - salt/master/not_this_tag
  - salt/wheel/*/ret


#MAX_EVENT_SIZE
#Default: 1048576，单位为Byte
#传递非常大的事件可能导致minion消耗大量的内存
max_event_size: 1048576


#PING_ON_ROTATE
#Default: False
#告知master在AES密钥刷新后立即ping所有minion
ping_on_rotate: False


#MASTER_JOB_CACHE
#Default: local_cache
master_job_cache: redis


#ENFORCE_MINE_CACHE
#Default: False
enforce_mine_cache: False


#MAX_MINIONS
#Default: 0，表示不限制
#master允许连接的最大minion数
max_minions: 100


#CON_CACHE
#Default: False
#为所有连接提供缓存
con_cache: True


#PRESENCE_EVENTS
#Default: False
#master周期性地寻找主动连接的minion


#PING_ON_ROTATE，默认False
#默认情况下，master AES key每24h轮换一次
#要告诉master在AES key刷新后立即Ping所有minions，请设置为True。但这样可能在刷新后立即导致Master的高负载，因为它管理着大量的minion，请注意
#如果禁用，建议使用密钥标记监听`aes_key_rotate`
ping_on_rotate: False


#TRANSPORT
#Default: zeromq
#修改底层传输层，支持的值有zeromq, raet, tcp
transport: zeromq


#TRANSPORT_OPTS
#Default: {}
#启用多个传输
transport_opts:
  tcp:
    publish_port: 4605
    ret_port: 4606
  zeromq: []


#MASTER_STATS
#Default: False
#Master统计信息


#MASTER_STATS_EVENT_ITER
#Default: 60


#SOCK_POOL_SIZE
#Default: 1
#为了避免将数据写入套接字时阻塞等待，启用salt应用程序的套接字池
sock_pool_size: 15


#IPC_MODE
#Default: ipc
#windows default: tcp
ipc_mode: ipc


#TCP_MASTER_PUB_PORT
#Default: 4512
#ipc_mode的tcp端口
tcp_master_pub_port: 4512


#TCP_MASTER_PULL_PORT
#Default: 4513
#ipc_mode的tcp端口
tcp_master_pull_port: 4513


#TCP_MASTER_PUBLISH_PULL
#Default: 4514
tcp_master_publish_pull: 4514


#TCP_MASTER_WORKERS
#Default: 4515
# mworkers连接到master的端口
tcp_master_workers: 4515


#AUTH_EVENTS
#Default: True
auth_events: True


#MINION_DATA_CACHE_EVENTS
#Default: True
minion_data_cache_events: True

```

<br/>



### salt-ssh

SALT-SSH CONFIGURATION


```yaml
#ROSTER
#Default: flat
#定义默认ROSTER模块
roster: cache


#ROSTER_DEFAULTS
#所有rosters都能继承的设置
roster_defaults:
  user: daniel
  sudo: True
  priv: /root/.ssh/id_rsa
  tty: True


#ROSTER_FILE
#Default: /etc/salt/roster
roster_file: /root/roster


#ROSTERS
#Default: None
#定义roster的位置，以便在使用salt api时选择他们
rosters:
 - /etc/salt/roster.d
 - /opt/salt/some/more/rosters


#SSH_PASSWD
#Default: ''
#用于登录的ssh密码
ssh_passwd: abc@123


#SSH_PORT
#Default: 22
#目标系统的ssh端口
ssh_port: 22


#SSH_SCAN_PORTS
#Default: 22
#多个值以逗号(,)分隔
ssh_scan_ports: 22


#SSH_SCAN_TIMEOUT
#Default: 0.01
ssh_scan_timeout: 0.01


#SSH_SUDO
#Default: False
ssh_sudo: False


#SSH_TIMEOUT
#Default: 60
ssh_timeout: 60


#SSH_USER
#Default: root
ssh_user: root


#SSH_LOG_FILE
#Default: /var/log/salt/ssh
#salt-ssh命令的日志文件
ssh_log_file: /var/log/salt/ssh


#SSH_MINION_OPTS
#Default: None
ssh_minion_opts:
  gpg_keydir: /root/gpg


#SSH_USE_HOME_KEY
#Default: False
#使用~/.ssh/id_rsa对salt-ssh身份验证
ssh_use_home_key: False


#SSH_IDENTITIES_ONLY
#Default: False
#设置为True表示为salt-ssh -o IdentitiesOnly=yes
ssh_identities_only: False


#SSH_LIST_NODEGROUPS
#Default: {}
ssh_list_nodegroups:
  groupA: minion1,minion2
  groupB: minion1,minion3


#THIN_EXTRA_MODS
#Default: None
#包含在salt thin中的附加模块


#MIN_EXTRA_MODS
#Default: None
```

<br/>

### 安全配置

MASTER SECURITY SETTINGS


```yaml
#OPEN_MODE
#Default: False
#open mode关闭认证并通知master接受所有身份认证
open_mode: False


#AUTO_ACCEPT
#Default: False
#自动接收所有来自minion的public key
auto_accept: False


#KEYSIZE
#Default: 2048
#创建新Key时生成的Key大小
keysize: 2048


#AUTOSIGN_TIMEOUT
#Default: 120


#AUTOSIGN_FILE
#Default: not defined
#此文件中指定的传入key将自动被接受


#AUTOREJECT_FILE
#Default: not defined
#此文件中指定的传入key将自动被拒绝


#AUTOSIGN_GRAINS_DIR
#Default: not defined
autosign_grains_dir: /etc/salt/autosign_grains


#PERMISSIVE_PKI_ACCESS
#Default: False
permissive_pki_access: False


#PUBLISHER_ACL
#Default: {}
#允许master上用户执行特定模块
publisher_acl:
  fred:
    - test.ping
    - pkg.*


#PUBLISHER_ACL_BLACKLIST
#Default: {}
publisher_acl_blacklist:
  users:
    - root
    - '^(?!sudo_).*$'   #  all non sudo users
  modules:
    - cmd.*
    - test.echo


#SUDO_ACL
#Default: False
sudo_acl: False


#EXTERNAL_AUTH
#Default: {}
external_auth:
  pam:
    fred:
      - test.*


#TOKEN_EXPIRE
#Default: 43200(12h)
token_expire: 43200


#TOKEN_EXPIRE_USER_OVERRIDE
#Default: False
token_expire_user_override:
  pam:
    - fred
    - tom
  ldap:
    - gary


#KEEP_ACL_IN_TOKEN
#Default: False
keep_acl_in_token: False


#EAUTH_ACL_MODULE
#Default: ''
eauth_acl_module: django


#FILE_RECV
#Default: False
#允许minion将文件推送给master
file_recv: False


#FILE_RECV_MAX_SIZE
#Default: 100
file_recv_max_size: 100


#MASTER_SIGN_PUBKEY
#Default: False
#使用master公钥的加密签名来签署master认证回复
master_sign_pubkey: True


#MASTER_SIGN_KEY_NAME
#Default: master_sign
#自定义签名密钥的名称
master_sign_key_name: <filename_without_suffix>


#MASTER_PUBKEY_SIGNATURE
#Default: master_pubkey_signature
master_pubkey_signature: <filename>


#MASTER_USE_PUBKEY_SIGNATURE
#Default: False
master_use_pubkey_signature: True


#ROTATE_AES_KEY
#Default: True
#当salt-key删除一个minion-public时，轮询salt-master的AES-key
rotate_aes_key: True


#PUBLISH_SESSION
#Default: 86400
#master上AES key轮询之间的秒数
publish_session: Default: 86400


#SSL
#Default: None
#TLS/SSL连接项
ssl:
    keyfile: <path_to_keyfile>
    certfile: <path_to_certfile>
    ssl_version: PROTOCOL_TLSv1_2


#ALLOW_MINION_KEY_REVOKE
#Default: False
#默认情况下，当minion key被移除时，master会删除它的缓存数据
```


<br/>


### 大规模调整设置

MASTER LARGE SCALE TUNING SETTINGS

```yaml
#MAX_OPEN_FILES
#Default: 100000
#请注意ulimit
max_open_files: 100000


#WORKER_THREADS
#Default: 5
worker_threads: 5


#PUB_HWM
#Default: 1000
pub_hwm: 1000


#ZMQ_BACKLOG
#Default: 1000
#zeromq backlog的监听队列大小
zmq_backlog: 1000


#SALT_EVENT_PUB_HWM AND EVENT_PUBLISHER_PUB_HWM
salt_event_pub_hwm: 20000
event_publisher_pub_hwm: 10000

```

<br/>


### 模块管理

MASTER MODULE MANAGEMENT

```yaml
#RUNNER_DIRS
#Default: []
runner_dirs:
  - /var/lib/salt/runners


#UTILS_DIRS
#Default: []
utils_dirs:
  - /var/lib/salt/utils


#CYTHON_ENABLE
#Default: False
cython_enable: False
```


<br/>


### 系统状态设置

MASTER STATE SYSTEM SETTINGS

```yaml
#STATE_TOP
#Default: top.sls
state_top: top.sls


#STATE_TOP_SALTENV
#无默认值
state_top_saltenv: dev


#TOP_FILE_MERGING_STRATEGY
#Default: merge
top_file_merging_strategy: same


#ENV_ORDER
#Default: []
env_order:
  - base
  - dev
  - qa

```


<br/>


### 文件服务器设置

MASTER FILE SERVER SETTINGS

```yaml


```


<br/>


### 本地文件服务器

ROOTS: MASTER'S LOCAL FILE SERVER


```yaml

```


<br/>


### git远程文件服务器后端

GITFS: GIT REMOTE FILE SERVER BACKEND

```yaml


```


<br/>


### gitfs认证项

GITFS AUTHENTICATION OPTIONS


```yaml



```



<br/>


### hgfs远程文件服务器后端

HGFS: MERCURIAL REMOTE FILE SERVER BACKEND


```yaml


```


<br/>


### svnfs远程文件服务器后端

SVNFS: SUBVERSION REMOTE FILE SERVER BACKEND


```yaml


```


<br/>


### minions远程文件服务器后端

MINIONFS: MINIONFS REMOTE FILE SERVER BACKEND


```yaml


```


<br/>



### azurefs文件服务器后端

AZUREFS: AZURE FILE SERVER BACKEND


<br>


### S3文件服务器后端

S3FS: S3 FILE SERVER BACKEND


<br/>


### pillar配置

PILLAR CONFIGURATION


```yaml


```


<br/>


### reactor设置

MASTER REACTOR SETTINGS


<br/>


### salt-api设置

SALT-API MASTER SETTINGS


<br/>


### syndic服务器

SYNDIC SERVER SETTINGS


<br/>


### peer publish

PEER PUBLISH SETTINGS


<br/>


### 日志

MASTER LOGGING SETTINGS


```yaml
#LOG_FILE
#Default: /var/log/salt/master
log_file: /var/log/salt/master


#LOG_LEVEL
#Default: warning
log_level: notice


#LOG_LEVEL_LOGFILE
#Default: warning
log_level_logfile: warning


#LOG_DATEFMT
#Default: %H:%M:%S
log_datefmt: '%H:%M:%S'


#LOG_DATEFMT_LOGFILE
#Default: %Y-%m-%d %H:%M:%S
log_datefmt_logfile: '%Y-%m-%d %H:%M:%S'


#LOG_FMT_CONSOLE
#Default: [%(levelname)-8s] %(message)s
log_fmt_console: '%(colorlevel)s %(colormsg)s'
log_fmt_console: '[%(levelname)-8s] %(message)s'


#LOG_FMT_LOGFILE
#Default: %(asctime)s,%(msecs)03d [%(name)-17s][%(levelname)-8s] %(message)s
log_fmt_logfile: '%(asctime)s,%(msecs)03d [%(name)-17s][%(levelname)-8s] %(message)s'


#LOG_GRANULAR_LEVELS
#Default: {}
```


<br/>


### NODE GROUPS

将minion进行逻辑分组

```yaml
#NODE GROUPS
#Default: {}
nodegroups:
  group1: 'L@foo.domain.com,bar.domain.com,baz.domain.com or bl*.domain.com'
  group2: 'G@os:Debian and foo.domain.com'
  group3: 'G@os:Debian and N@group1'
  group4:
    - 'G@foo:bar'
    - 'or'
    - 'G@foo:baz'



#RANGE_SERVER
#Default: 'range:80'
range_server: range:80



#INCLUDE CONFIGURATION
#DEFAULT_INCLUDE
#Default: master.d/*.conf


#INCLUDE
#Default: not defined
# Include files from a master.d directory in the same
# directory as the master config file
include: master.d/*

# Include a single extra file into the configuration
include: /etc/roles/webserver

# Include several files and the master.d directory
include:
  - extra_config
  - master.d/*
  - /etc/roles/webserver


#KEEPALIVE
#TCP_KEEPALIVE
#Default: True
tcp_keepalive: True


#TCP_KEEPALIVE_CNT
#Default: -1
#Sets the ZeroMQ TCP keepalive count
tcp_keepalive_cnt: -1


#TCP_KEEPALIVE_IDLE
#Default: 300


#TCP_KEEPALIVE_INTVL
#Default: -1

```


<br/>


### 文件路径

- Linux
- Windows

```
#Linux

#path
conf_file: /etc/salt/master
log_file: /var/log/salt/master
pidfile: /var/run/salt-master.pid

#dir
cachedir: /var/cache/salt/master
extension_modules: /var/cache/salt/master/extmods
pki_dir: /etc/salt/pki/master
root_dir: /
sock_dir: /var/run/salt/master

#file_roots
/srv/salt
/srv/spm/salt

#pillar_roots
/srv/pillar
/srv/spm/pillar

#win repo
winrepo_dir: /srv/salt/win/repo
winrepo_dir_ng: /srv/salt/win/repo-ng



#Windows

#path
conf_file: c:\salt\conf\master
log_file: c:\salt\var\log\salt\master
pidfile: c:\salt\var\run\salt-master.pid

#dir
cachedir: c:\salt\var\cache\salt\master
c:\salt\var\cache\salt\master\extmods
pki_dir: c:\salt\conf\pki\master
root_dir: c:\salt
sock_dir: c:\salt\var\run\salt\master

#file_roots
c:\salt\srv\salt
c:\salt\srv\spm\salt

#pillar_roots
c:\salt\srv\pillar
c:\salt\srv\spm\pillar

#win-repo
winrepo_dir: c:\salt\srv\salt\win\repo
winrepo_dir_ng: c:\salt\srv\salt\win\repo-ng
```



<br/>
<br/>
<br/>



## minion

CONFIGURING THE SALT MINION

- minion config: <https://docs.saltstack.com/en/latest/ref/configuration/minion.html>

Salt Minion配置非常简单。通常，需要设置的唯一值是master value，因此minion知道在哪里找到其Master Server。





<br/>
<br/>
<br/>



## proxy minion

CONFIGURING THE SALT PROXY MINION


```yaml
#ADD_PROXYMODULE_TO_OPTS
#Default: False
add_proxymodule_to_opts: True


#PROXY_MERGE_GRAINS_IN_MODULE
#Default: True


#PROXY_KEEP_ALIVE
#Default: True
#死亡时是否重启与远程设备的连接
proxy_keep_alive: False


#PROXY_KEEP_ALIVE_INTERVA
#Default: 1(min)
#keepalive检查频率
proxy_keep_alive_interval: 5


#PROXY_ALWAYS_ALIVE
#Default: True
proxy_always_alive: False


#PROXY_MERGE_PILLAR_IN_OPTS
#Default: False


#PROXY_DEEP_MERGE_PILLAR_IN_OPTS
#Default: False


#PROXY_MERGE_PILLAR_IN_OPTS_STRATEGY
#Default: smart


#PROXY_MINES_PILLAR
#Default: True
```



<br/>
<br/>
<br/>



## 栗子配置文件

- config file examples: <https://docs.saltstack.com/en/latest/ref/configuration/examples.html>



<br/>
<br/>
<br/>




## minion blackout

MINION BLACKOUT CONFIGURATION


Salt支持minion blackouts。当一个minion处于blackout mode时，所有远程执行命令都被禁用。

minion blackout mode通过pillar key——`minion_blackout`进行配置。如果此为True，则minion将拒绝除`saltutil.refresh_pillar`命令外的所有传入命令。
它也支持whitelist:

```
minion_blackout_whitelist:
  - test.ping
  - pillar.get
```




<br/>
<br/>
<br/>




## 访问控制系统

ACCESS CONTROL SYSTEM


Salt维护一个标准系统，用于向非管理用户开放粒度控制以执行Salt命令。访问控制系统已应用于所有用于配置对Salt中非管理控制接口的访问的系统。
访问控制系统强制要求在所有三个系统中使用的标准配置语法。

这个接口包括:

- **peer** system
- **external auth** system
- **publisher acl** system


<br/>
<br/>


### 什么时候使用每个认证系统

`publisher_acl` 对于允许本地系统用户运行Salt命令而不给予root访问权限非常有用。如果您可以直接登录Salt master，那么 `publisher_acl`允许您在没有root权限的情况下使用Salt。如果本地系统配置为针对远程系统（如LDAP或Active Directory）进行身份验证，则 `publisher_acl` 将透明地与远程系统进行交互。

`external_auth` 对于 `salt-api` 或者使用Salt Python API创建自己的脚本非常有用。它可以在CLI上使用（with the `-a` flag），但由于涉及更多步骤，因此更麻烦。在CLI中唯一有用的是本地系统未配置为针对外部服务进行身份验证，但您仍希望Salt对外部服务进行身份验证。


<br/>
<br/>


### Publisher ACL

PUBLISHER ACL SYSTEM


Salt **Publisher ACL** 系统，允许除root之外的系统用户有权在master上访问在minion上执行的salt命令。
它在master的配置文件中的 `publisher_acl` 部分进行配置。在此配置项下，指定用户打开发送命令，然后指定将对特定用户可用的minion函数列表。
用户和函数都可以通过完全匹配、shell glob、正则表达式来指定。

<br>

栗子：

```yaml
publisher_acl:
  # Allow thatch to execute anything.
  thatch:
    - .*
  # Allow fred to use test and pkg, but only on "web*" minions.
  fred:
    - web*:
      - test.*
      - pkg.*
  # Allow admin and managers to use saltutil module functions
  admin|manager_.*:
    - saltutil.*
  # Allow users to use only my_mod functions on "web*" minions with specific arguments.
  user_.*:
    - web*:
      - 'my_mod.*':
          args:
            - 'a.*'
            - 'b.*'
          kwargs:
            'kwa': 'kwa.*'
            'kwb': 'kwb'
```


<br/>
<br/>


#### PERMISSION ISSUES

必须修改 `publisher_acl` 所需的目录，以便指定的用户可以读取:

```bash
chmod 755 /var/cache/salt /var/cache/salt/master /var/cache/salt/master/jobs /var/run/salt /var/run/salt/master

#还要更改日志文件有对应的写入权限

```


<br/>
<br/>


#### 白/黑名单

WHITELIST AND BLACKLIST

在Salt身份认证配置系统中，可以通过使用白名单指定允许的内容，使用黑名单指定不允许的内容。

```
#白名单
publisher_acl:
  fred:
    - test.ping
    - pkg.*


#黑名单
publisher_acl_blacklist:
  users:
    - root
    - '^(?!sudo_).*$'   #  all non sudo users
  modules:
    - cmd.*
    - test.echo
```


<br/>
<br/>



### External Authentication

EXTERNAL AUTHENTICATION SYSTEM


Salt's **External Authentication** System (eAuth)，允许Salt将命令授权传递给任何外部身份验证系统(PAM、LDAP)。

> eAuth如果使用PAM，则需要以root身份来运行 `slat-master`


<br/>


#### eAuth配置

**eAuth** 允许特定用户被授予访问权限在特权minion上执行特定功能。它在master中配置。

```yaml
external_auth:
  pam:
    thatch:
      - 'web*':
        - test.*
        - network.*
    steve|admin.*:
      - .*

#用户thatch在web*的minions上执行test和network模块
#用户steve和adminxxx被授予不受限制的访问权限
#PAM模块不允许以root身份进行身份验证



#wheel modules/runner modules
external_auth:
  pam:
    thatch:
      - '@wheel'   # to allow access to all wheel modules
      - '@runner'  # to allow access to all runner modules
      - '@jobs'    # to allow access to the jobs runner and/or wheel module

#runner/wheel的标记是不同的，因为没有minions来限定ACL
```

> 注意:
> 所有具有外部身份验证权限的用户允许运行 `saltutil.findjob`。请注意，这可能会无意中暴露一些数据，例如minion ID。


<br/>


##### 匹配语法

MATCHING SYNTAX

`external_auth` 字典的结构可以采用以下形状:

- user, function 是完全匹配
- shell 是glob pattern或 RE
- minions 是复合目标

```yaml
#By user
external_auth:
  <eauth backend>:
    <user or group%>:
      - <regex to match function>


#By user, by minion
external_auth:
  <eauth backend>:
    <user or group%>:
      <minion compound target>:
        - <regex to match function>


#By user, by runner/wheel
external_auth:
  <eauth backend>:
    <user or group%>:
      <@runner or @wheel>:
        - <regex to match function>


#By user, by runner+wheel module
external_auth:
  <eauth backend>:
    <user or group%>:
      <@module_name>:
        - <regex to match function without module_name>
```


<br/>


##### GROUPS

要将权限应用于eAuth中的一组用户，请在ID中附加`％`:

```yaml
external_auth:
  pam:
    admins%:
      - '*':
        - 'pkg.*'
```


<br/>


##### 通过函数参数来限制

LIMITING BY FUNCTION ARGUMENTS

函数的位置参数或关键字参数也可以列入白名单:

```yaml
external_auth:
  pam:
    my_user:
      - '*':
        - 'my_mod.*':
            args:
              - 'a.*'
              - 'b.*'
            kwargs:
              'kwa': 'kwa.*'
              'kwb': 'kwb'
      - '@runner':
        - 'runner_mod.*':
            args:
            - 'a.*'
            - 'b.*'
            kwargs:
              'kwa': 'kwa.*'
              'kwb': 'kwb'
```



<br/>
<br/>



#### 用法

任何用户在master上可以使用 `-a` 选项在同一系统从命令行使用 eAu:

```bash
salt -a pam web\* test.ping

```


<br/>


##### TOKENS

仅使用外部身份验证，每次调用Salt时都需要身份验证凭据。这可以通过Salt Token 来缓解。
令牌是短期授权，只需在验证时添加-T选项即可轻松创建：

```bash
salt -T -a pam web\* test.ping

#现在将创建一个有效期为12小时（默认）的令牌，此令牌存储在活动用户主目录中名为 `salt_token` 的文件中
#创建令牌后，将与所有后续通信一起发送令牌。在令牌过期之前，不需要再次输入用户身份验证
#令牌的过期时间可在master config中修改
```


<br/>
<br/>


#### LDAP和AD

LDAP AND ACTIVE DIRECTORY

> LDAP要求安装python-ldap

Salt支持LDAP的和组身份验证。


<br/>


##### OPENLDAP和类似的系统

LDAP配置发生在 Salt Master配置文件中。

```yaml
# Server to auth against
auth.ldap.server: localhost

# Port to connect via
auth.ldap.port: 389

# Use TLS when connecting
auth.ldap.tls: False

# LDAP scope level, almost always 2
auth.ldap.scope: 2

# Server specified in URI format
auth.ldap.uri: ''    # Overrides .ldap.server, .ldap.port, .ldap.tls above

# Verify server's TLS certificate
auth.ldap.no_verify: False

# Bind to LDAP anonymously to determine group membership
# Active Directory does not allow anonymous binds without special configuration
# In addition, if auth.ldap.anonymous is True, empty bind passwords are not permitted.
auth.ldap.anonymous: False

# FOR TESTING ONLY, this is a VERY insecure setting.
# If this is True, the LDAP bind password will be ignored and
# access will be determined by group membership alone with
# the group memberships being retrieved via anonymous bind
auth.ldap.auth_by_group_membership_only: False

# Require authenticating user to be part of this Organizational Unit
# This can be blank if your LDAP schema does not use this kind of OU
auth.ldap.groupou: 'Groups'

# Object Class for groups.  An LDAP search will be done to find all groups of this
# class to which the authenticating user belongs.
auth.ldap.groupclass: 'posixGroup'

# Unique ID attribute name for the user
auth.ldap.accountattributename: 'memberUid'

# These are only for Active Directory
auth.ldap.activedirectory: False
auth.ldap.persontype: 'person'

auth.ldap.minion_stripdomains: []

# Redhat Identity Policy Audit
auth.ldap.freeipa: False
```

<br/>

- **验证LDAP服务器**

LDAP身份验证有两个阶段:

- 首先，Salt进行身份验证以搜索用户的专有名称(DN)和组成员
- Salt搜索目录以确定实际用户的DN和组后，它将以运行Salt命令的用户重新进行身份验证

如果您已经了解DN的结构，并且LDAP存储中的权限已设置为用户可以查找自己的组成员身份，则第一个和第二个用户可以相同。要告诉Salt这种情况，请省略 `auth.ldap.bindpw` 参数。

```yaml
#binddn
auth.ldap.basedn: dc=saltstack,dc=com
auth.ldap.binddn: uid={{ username }},cn=users,cn=accounts,dc=saltstack,dc=com


#
auth.ldap.binddn: uid=ldaplookup,cn=sysaccounts,cn=etc,dc=saltstack,dc=com
auth.ldap.bindpw: mypassword


#
auth.ldap.filter: uid={{ username }}
```

<br/>

- **确定组成员**

对于OpenLDAP，要确定组成员身份，可以指定包含组数据的OU。
对于AD，以不同方式处理组成员身份，并且不使用groupou配置变量。



<br/>
<br/>



### Peer Communication


此功能的目的不是让Salt minions作为另一个独立的中间商，而是让 Salt Minions 将命令传递给另外的 Salt Minion。
peer interface 通过主配置文件中的两个选项进行配置：

- 对于从 master 发送命令的minion来说，使用 `peer`
- 对于从 master 执行 runners 的minion来说，使用 `peer_run`

由于这可以通过允许minions访问master publisher，可能存在安全风险，因此默认情况下关闭该功能。根据正则表达式，可以允许 minions 基于每个 minion 访问 master publisher。根据minion的ID可以允许ID访问某些Salt模块和功能。


<br/>
<br/>


#### 配置

PEER CONFIGURATION

它在 master config 的 `peer` 部分进行配置:

```yaml
#最简单方法是为 allminions 启用 all communication，仅限于很安全的环境下
peer:
  .*:
    - .*


#以 example.com 结尾的ID
peer:
  .*example.com:
    - test.*
    - ps.*
    - pkg.*


peer:
  .*example.com:
    - test.*
    - ps.*
    - pkg.*
  .*foo.org:
    - test.*
    - ps.*
    - pkg.*

#使用正则表达式匹配函数
```


<br/>
<br/>


#### PEER RUNNER

```
#all to all
peer_run:
  .*:
    - .*


#
peer_run:
  .*example.com:
    - manage.*
    - jobs.*
```


<br/>
<br/>


#### 使用对等通信

USING PEER COMMUNICATION

publish module 被创建来管理 peer communication。它有多种方式来执行对等通信，目前，此模块有三个functions:

```bash
salt-call publish.publish \* test.ping


salt-call publish.runner manage.up


salt-call publish.publish 'webserv* and not G@os:Ubuntu' test.ping tgt_type='compound'
```




<br/>
<br/>
<br/>




## 作业管理

JOB MANAGEMENT


由于Salt执行在许多系统上运行的作业，因此Salt需要能够管理在许多系统上运行的作业。


<br/>


### Minion Proc系统

THE MINION PROC SYSTEM


Salt Minions 在 Salt `cachedir` 中维护一个 `proc` 目录(默认`/var/cache/salt/proc`)，`proc` 目录维护以执行的作业的ID命名的文件。这些文件包含有关minion上当前正在运行的作业的信息，并允许查找作业。


<br/>
<br/>


### saltutil模块中的函数

FUNCTIONS IN THE SALTUTIL MODULE


`saltutil`模块中的函数用于管理作业，它们有:

- `running`: 返回 `proc` 目录中找到的所有正在运行的作业的数据
- `find_job`: 根据 Job ID 返回有关特定作业的特定数据
- `signal_job`: 允许给定的 JID 发送信号
- `term_job`: Sends a termination signal (SIGTERM, 15)到指定作业进程
- `kill_job`: Sends a kill signal (SIGKILL, 9)到指定作业进程

这些函数构成了后端的核心，用于管理minion级别的工作。



<br/>
<br/>
<br/>



### JOB RUNNER


**jobs runner** 包含使查看数据更简单，更清晰的功能。它包含许多功能:

<br>

- **ACTIVE**

`acrive`函数对所有minions运行 `saltutil.running`，并以更加可用和紧凑的格式格式化有关所有正在运行的作业返回的数据。它还将比较已返回的作业和仍在运行的作业，从而更容易查看系统已完成作业以及系统仍在等待的作业。

```bash
salt-run jobs.active
```

<br>

- **LOOKUP_JID**

执行作业时，返回数据将发送回 Master 并进行缓存。默认缓存24小时，但可以通过 master config 中的 `keep_jobs` 选项进行配置。使用 `lookup_jid` runner 将显示与salt命令初始作业调用相同的返回数据。

```bash
salt-run jobs.lookup_jid <job id number>
```

<br>

- **LIST_JOBS**

在找到历史性工作之前，可能需要找到 JID。`list_jobs` 将解析缓存的执行数据，并显示已经或部分返回的作业的所有作业数据。

```bash
salt-run jobs.list_jobs
```


<br/>
<br/>
<br/>



### 调度作业

SCHEDULING JOBS


Salt调度系统允许对 Minion/Master 进行增量执行。调度系统公开了对 Minion/Runner on Master 的任何执行函数的执行。

有多种方法启用调度:

- 在 master/minion 配置文件中的 `schedule` 选项，修改之后重启生效
- Minion pillar data. 通过刷新minion pillar data来实现(`saltutil.refresh_pillar`)
- `schedule state`/`schedule module`

> 注意:
> 调度器在 minion 和 master 上执行不同的函数。`runner` func on master; `specify execution` func on minino.

<br>

调度在 minion 上是没有输出的，除非设置 minion 的日志级别。

States 在 minion 上执行，所有的都是如此。您可以传递位置参数并提供命名参数的YAML dict。

```yaml
#每3600s调度一次 state.sls httpd test=True
schedule:
  job1:
    function: state.sls
    seconds: 3600
    args:
      - httpd
    kwargs:
      test: True


#延长10s-15s
schedule:
  job1:
    function: state.sls
    seconds: 3600
    args:
      - httpd
    kwargs:
      test: True
    splay:
      start: 10
      end: 15
```


<br/>
<br/>


#### 按时间和日期调度

SCHEDULE BY DATE AND TIME

可以使用Python `dateutil` 库支持的日期字符串指定作业频率。当然，你需要安装此库。

```yaml
schedule:
  job1:
    function: state.sls
    args:
      - httpd
    kwargs:
      test: True
    when:
      - Monday 5:00pm
      - Tuesday 3:00pm
      - Wednesday 5:00pm
      - Thursday 3:00pm
      - Friday 5:00pm


#调度程序自定义短语用于when

whens:
  tea time: 1:40pm
  deployment time: Friday 5:00pm

schedule:
  job1:
    function: state.sls
    args:
      - httpd
    kwargs:
      test: True
    when:
      - 'tea time'


#时间区间
schedule:
  job1:
    function: state.sls
    seconds: 3600
    args:
      - httpd
    kwargs:
      test: True
    range:
      invert: True
      start: 8:00am
      end: 5:00pm


#指定时间
#默认日期格式为ISO 8601
schedule:
  job1:
    function: pkg.install
    kwargs:
      pkgs: [{'bar': '>1.2.3'}]
      refresh: true
    once: '2016-01-07T14:30:00'


#指定日期格式
schedule:
  job1:
    function: test.ping
    once: 2015-04-22T20:21:00
    once_fmt: '%Y-%m-%dT%H:%M:%S'
```


<br/>
<br/>


#### 最大并行工作

MAXIMUM PARALLEL JOBS RUNNING

调度程序还支持确保运行特定例程的副本不超过N个。将此用于可能长时间运行的、发生基础设施中断可能会相互踩踏或堆积在一起的作业。

```yaml
#maxrunning默认值1
schedule:
  long_running_job:
    function: big_file_transfer
    jid_include: True
    maxrunning: 1
```


<br/>
<br/>


#### 类似定时任务的调度

CRON-LIKE SCHEDULE


调度器支持使用定时任务格式来调度作业。当然，需要安装Python `croniter` 库。

```yaml
schedule:
  job1:
    function: state.sls
    cron: '*/15 * * * *'
    args:
      - httpd
    kwargs:
      test: True
```


<br/>
<br/>


#### 作业数据返回

JOB DATA RETURN

默认情况下，有关从Salt Scheduler 运行的作业的数据将返回到 Master。可在 master config 的 `return_job` 配置项进行修改。

```yaml
schedule:
  job1:
    function: scheduled_job_function
    return_job: False
```


<br/>
<br/>


#### JOB METADATA

包含特定数据用以区分一个作业与其他作业可能很有用。使用元数据(metadata)参数，特殊值可以与调度作业相关联。这些值不用于作业的执行，但如果与 `return_job` 参数结合使用，可用于稍后搜索特定作业。元数据参数必须指定为 字典数据格式，否则将会被忽略。

```yaml
schedule:
  job1:
    function: scheduled_job_function
    metadata:
      foo: bar
```


<br/>
<br/>


#### 启动时运行

RUN ON START

默认情况下，根据minion的启动时间安排的任何作业将在minion启动时运行预定作业。有时这不是理想的情况。将 `run_on_start`参数 设置为 `False` 将导致调度程序跳过此第一次运行并等待下一次计划运行：

```yaml
schedule:
  job1:
    function: state.sls
    seconds: 3600
    run_on_start: False
    args:
      - httpd
    kwargs:
      test: True
```


<br/>
<br/>


#### UNTIL/AFTER

```yaml
#until参数，允许指定调度作业的结束时间
#在此时间过后，作业将不在运行
schedule:
  job1:
    function: state.sls
    seconds: 15
    until: '12/31/2015 11:59pm'
    args:
      - httpd
    kwargs:
      test: True



#after参数，允许指定调度作业的开始时间
#在此时间过后，作业将不在运行
schedule:
  job1:
    function: state.sls
    seconds: 15
    after: '12/31/2015 11:59pm'
    args:
      - httpd
    kwargs:
      test: True
```


<br/>
<br/>


#### 调度状态

SCHEDULING STATES

```yaml
schedule:
  log-loadavg:
    function: cmd.run
    seconds: 3660
    args:
      - 'logger -t salt < /proc/loadavg'
    kwargs:
      stateful: False
      shell: /bin/sh





#'>
```


<br/>
<br/>


#### 高峰调度

SCHEDULING HIGHSTATES

```yaml
#highstate
schedule:
  highstate:
    function: state.highstate
    minutes: 60
```


<br/>
<br/>


####　调度runner

SCHEDULING RUNNERS

也在在master上指定runner的运行:

```yaml
schedule:
  run_my_orch:
    function: state.orchestrate
    hours: 6
    splay: 600
    args:
      - orchestration.my_orch
```


<br/>
<br/>


####　SCHEDULER WITH RETURNER

调度程序对于收集有关minion的监视数据等任务也很有用，此调度选项将收集状态数据并将其发送到MySQL返回者数据库：

```yaml
schedule:
  uptime:
    function: status.uptime
    seconds: 60
    returner: mysql
  meminfo:
    function: status.meminfo
    minutes: 5
    returner: mysql
```



<br/>
<br/>
<br/>
<br/>



## 管理作业缓存

MANAGING THE JOB CACHE


Salt Maste r维护所有作业执行的作业缓存，可以通过 Job Runner查询。此作业缓存称为默认作业缓存。


<br/>


### 默认作业缓存

DEFAULT JOB CACHE

默认缓存系统使用Salt Master上的本地存储，可以在作业缓存目录(默认: `/var/cache/salt/master/jobs`)中找到。默认缓存系统适用于大多数部署，因为它通常不需要任何进一步的配置或管理。

默认作业缓存是临时缓存，作业将存储24小时。可在 master config 文件中的 `keep_jobs` 参数来调整它:

```yaml
#请勿将此值设置为0，因为这意味着缓存清理程序永远不会运行
keep_jobs: 24
```

<br>

- **减小默认缓存的大小**

REDUCING THE SIZE OF THE DEFAULT JOB CACHE

默认作业缓存有时会成为大型部署(`minions > 5000`的负担。禁用作业缓存将使以前执行的作业对作业系统不可用，**通常不建议这样做**。通常，确保 Master 可以访问更快的IO系统或者将tmpfs安装到jobs目录是明智的。
但是，您可以通过在Salt Master配置文件中将`job_cache` 设置为 `False` 来禁用它。这意味着Salt Master将不再缓存minion的返回信息，但仍将创建每个作业的JID目录和jid文件。此JID目录是检查和防止JID冲突所必需的。



<br/>
<br/>
<br/>
<br/>




## 在外部系统中存储作业结果

STORING JOB RESULTS IN AN EXTERNAL SYSTEM


作业执行后，每个Salt Minion将作业结果返回给Salt Master。这些结果存储在默认作业缓存中。除了默认作业缓存之外，Salt还提供了两种额外的机制来将作业结果发送到其它系统(databases, local syslog, others):

- External Job Cache
- Master Job Cache


<br/>


### EXTERNAL JOB CACHE - MINION-SIDE RETURNER

配置外部作业缓存后，数据会像往常一样返回Salt Master上的默认作业缓存，然后使用Salt Minion上运行的Salt ruturner module 将结果发送到外部作业缓存。

![](/images/Salt/external-job-cache.png)



<br/>
<br/>



### MASTER JOB CACHE - MASTER-SIDE RETURNER

在此配置中，Salt Minions 像往常一样将数据发送到默认作业缓存，然后 Salt Master 使用在 Salt Master 上运行的Salt Returner Module 将数据发送到外部系统。

![](/images/Salt/master-job-cache.png)



<br/>
<br/>


### 配置两种机制


<br>


- **了解SALT RETURNERS**

在配置作业缓存之前，必须了解 Salt Returner Module。**Returner** 是可插拔的 Salt Modules，它接收作业返回的数据，然后执行任何必要的步骤将数据发送到外部系统。
Salt Returner 为 External Job Cache 和 Master Job Cache 提供了许多核心功能。

Salt目前提供许多不同的 Returner ，以便让您连接到各种系统。各种 Returner Module: <https://docs.saltstack.com/en/latest/ref/returners/all/index.html#all-salt-returners>

<br>

- **配置它们**

`EXTERNAL JOB CACHE`，可在如下选项中配置:

- Minion configuration file
- Minion's grains
- Minion's pillar data

<br>

`MASTER JOB CACHE`，在 Master config 中进行配置。

栗子:

```yaml
#mysql
mysql.host: 'salt'
mysql.user: 'salt'
mysql.pass: 'salt'
mysql.db: 'salt'
mysql.port: 3306


#slack
slack.channel: 'channel'
slack.api_key: 'key'
slack.from_name: 'name'
```

<br>

- **启用它们**


```yaml
#EXTERNAL JOB CACHE
#ext_job_cache: mysql
ext_job_cache: <returner>


#MASTER JOB CACHE
#master_job_cache: mysql
master_job_cache: <returner>



#重启服务
```



<br/>
<br/>
<br/>



## 日志

LOGGING


Salt 尽力让日志工作为您工作，并帮助我们解决您可能在此过程中发现的任何问题。


<br/>


### 日志级别

LOG LEVELS

日志级别按数字顺序排列，以便将日志级别设置为特定级别将记录该级别及更高级别的所有日志语句。
例如，` log_level: error` 将会记录 `error, critical, quiet` 级别的日志。

大多数日志记录级别在Python logging lib 中默认定义，Salt 使用了更多日志级别。

| Level | Numeric value | Description |
| - | - | - |
| quiet | 1000 | Nothing should be logged at this level |
| critical | 50 | Critical errors |
| error | 40 | Errors |
| warning | 30 | Warnings |
| info | 20 | Normal log information |
| profile | 15 | Profiling information on salt performance |
| debug | 10 | Information useful for debugging both salt implementations and salt code |
| trace | 5 | More detailed code debugging information |
| garbage | 1 | Even more debugging information |
| all | 0 | Everything |



<br/>
<br/>



### 日志配置

```yaml
#LOG_FILE
#日志记录可以发送到常规文件，本地路径名或网络位置，配置为使用rsyslogd时，远程日志记录效果最佳
#远程地址格式: <file|udp|tcp>://<host|socketpath>:<port-if-required>/<log-facility>
#栗子
log_file: /var/log/salt/master
log_file: /var/log/salt/minion
log_file: file:///dev/log
log_file: file:///dev/log/LOG_DAEMON
log_file: udp://loghost:10514



#LOG_LEVEL
#Default: warning
#发送到控制台的日志记录消息的级别
log_level: warning



#LOG_LEVEL_LOGFILE
#Default: info
#要发送到日志文件的消息级别
log_level_logfile: warning



#LOG_DATEFMT
#Default: %H:%M:%S
#控制台日志消息中使用的日期和时间格式
log_datefmt: '%H:%M:%S'



#LOG_DATEFMT_LOGFILE
#Default: %Y-%m-%d %H:%M:%S
#日志文件消息中使用的日期和时间格式



#LOG_FMT_CONSOLE
#Default: [%(levelname)-8s] %(message)s
#控制台日志消息的格式
log_fmt_console: '[%(levelname)-8s] %(message)s'

#可使用所有标准python日志记录LogRecord属性。 Salt还提供自定义LogRecord属性自定义控制台显示的颜色
'%(colorlevel)s'   # log level name colorized by level
'%(colorname)s'    # colorized module name
'%(colorprocess)s' # colorized process number
'%(colormsg)s'     # log message colorized by level




#LOG_FMT_LOGFILE
#Default: %(asctime)s,%(msecs)03d [%(name)-17s][%(levelname)-8s] %(message)s
#日志文件记录消息的格式
log_fmt_logfile: '%(asctime)s,%(msecs)03d [%(name)-17s][%(levelname)-8s] %(message)s'

#也可通过对 LogRecord 自定义控制台显示颜色
'%(bracketlevel)s'   # equivalent to [%(levelname)-8s]
'%(bracketname)s'    # equivalent to [%(name)-17s]
'%(bracketprocess)s' # equivalent to [%(process)5s]



#LOG_GRANULAR_LEVELS
#Default: {}
#用于根据日志调用名称更具体地控制日志记录级别
log_granular_levels:
  'salt': 'warning'
  'salt.modules': 'debug'
  'salt.loader.saltmaster.ext.module.custom_module': 'all'
```



<br/>
<br/>



### 外部日志处理器

EXTERNAL LOGGING HANDLERS

除了salt使用的内部日志处理程序外，还有一些可以使用的外部日志处理程序:

- [fluent_mod](https://docs.saltstack.com/en/latest/ref/configuration/logging/handlers/salt.log.handlers.fluent_mod.html)
- [log4mongo_mod](https://docs.saltstack.com/en/latest/ref/configuration/logging/handlers/salt.log.handlers.log4mongo_mod.html)
- [logstash_mod](https://docs.saltstack.com/en/latest/ref/configuration/logging/handlers/salt.log.handlers.logstash_mod.html)
- [sentry_mod](https://docs.saltstack.com/en/latest/ref/configuration/logging/handlers/salt.log.handlers.sentry_mod.html)




<br/>
<br/>
<br/>



## 文件服务器

Salt File Server


Salt 附带了一个简单的文件服务器，适合将文件分发给 Salt minions，它是内置在 Salt Master 中的无状态 ZeroMQ Server。

Salt文件服务器的主要目的是提供在 Salt state system 中使用的文件。有了这个说法，Salt文件服务器可用于从Master到minions的任何常规文件传输。


<br/>


### 文件系统后端

FILE SERVER BACKENDS

此功能增加了Salt Master集成不同文件服务器后端的能力。文件服务器后端允许Salt文件服务器充当外部资源的透明桥。
一个很好的例子是`git`后端，它允许Salt提供来自一个或多个`git`存储库的文件，但也有其他几个。

File Server Module:

- azurefs
- gitfs
- hgfs
- minionfs
- roots
- s3fs
- svnfs

<br>

```yaml
#启用
fileserver_backend:
  - git


#使用多个后端
fileserver_backend:
  - roots
  - git


#定义环境变量
#在文件服务器环境中定义不同源的顺序也很重要
file_roots:
  base:
    - /srv/salt/prod
  qa:
    - /srv/salt/qa
    - /srv/salt/prod
  dev:
    - /srv/salt/dev
    - /srv/salt/qa
    - /srv/salt/prod


#栗子
gitfs_remotes:
  - https://mydomain.tld/repos/first.git
  - https://mydomain.tld/repos/second.git
```



<br/>
<br/>



### 从特定环境请求文件

REQUESTING FILES FROM SPECIFIC ENVIRONMENTS

Salt文件服务器支持多种环境，允许隔离SLS文件和其他文件以便更好地组织。
对于默认后端(root)，使用 `roots` 选项定义环境。其它后端以它们自己的方式定义环境。

<br>

**QUERYSTRING SYNTAX**

任何 `salt://` 文件URL都可以使用查询字符串语法指定其文件服务器环境

```yaml
salt://path/to/file?saltenv=foo
```

<br>

**IN STATES**

可以指示Minions在全局和单个状态下使用哪个环境，并且每个环境都有多种方法:

- GLOBALLY

使用minion配置文件中的 `environment` 选项将minion固定到环境中。此外，可以将环境设置为单次调用以下函数。

```
state.apply
state.highstate
state.sls
state.top
```

- ON A PER-STATE BASIS

在单个状态中，有两种指定环境的方法。

```yaml
#第一个是向 state 添加 saltenv 参数
/etc/foo/bar.conf:
  file.managed:
    - source: salt://foo/bar.conf
    - user: foo
    - mode: 600
    - saltenv: config


#另一种是使用查询字符串语法
/etc/foo/bar.conf:
  file.managed:
    - source: salt://foo/bar.conf?saltenv=config
    - user: foo
    - mode: 600
```



<br/>
<br/>



### 文件系统配置

FILE SERVER CONFIGURATION

Salt文件服务器是用ZeroMQ编写的高性能文件服务器。它可以快速管理大型文件，而且开销很小，并且已经过优化，可以非常有效地处理小文件。

Salt文件服务器是一个环境感知文件服务器。这意味着可以在许多根目录中分配文件，并通过指定文件路径和要搜索的环境来访问这些文件。各个环境可以跨多个目录根来创建叠加，并允许以多种灵活的方式组织文件。


<br/>


#### 环境

Salt文件服务器默认为强制 `base` 环境。必须定义这个环境，并在未指定环境时用于下载文件。

环境允许 file 和 sls data 在逻辑上分开，但环境不会相互隔离。这允许工程师使用Salt对环境进行逻辑隔离，但也允许在多个环境中使用信息。


<br/>


#### 目录覆盖

DIRECTORY OVERLAY

环境设置是用于发布文件的目录列表。搜索这些目录以查找指定的文件并返回找到的第一个文件。这意味着目录数据的优先级基于它们的列出顺序。
这允许根据在配置中定义的顺序来覆盖目录并确定其优先级。

```yaml
file_roots:
  base:
    - /srv/salt/base
    - /srv/salt/failover

#如果一个文件的URI是 salt://httpd/httpd.conf，它会首先在 /srv/salt/base/httpd/httpd.conf 搜索文件，找到则返回
#如果未找到，那么使用 /srv/salt/failover/httpd/httpd.conf
```


<br/>


#### 本地文件服务器

LOCAL FILE SERVER

可以重新路由文件服务器以从minion运行。这主要是为了在没有Salt master的情况下运行Salt State。要使用本地文件服务器接口，请将文件服务器数据复制到minion，并将minion上的 `file_roots` 选项设置为指向从 Master 复制的目录。一旦设置了minion `file_roots` 选项，将 `file_client`选项更改为本地以确保使用本地文件服务器接口。



<br/>
<br/>



### CP模块

THE CP MODULE

cp模块是minion端文件服务器操作的主页。cp模块由Salt State system, `salt-cp` 使用，可用于分发Salt文件服务器提供的文件。

<br>

- **ESCAPING SPECIAL CHARACTERS**

`satl://` url格式可能包含查询字符串，例如 `salt://dir/file.txt?saltenv=base`。通过带有`satl://-`的引用文件作为查询字符串的初始标记，你可阻止`?`的解释。

```yaml
/etc/marathon/conf/?checkpoint:
  file.managed:
    - source: salt://|hw/config/?checkpoint
    - makedirs: True
```

<br>

- **ENVIRONMENTS**

由于文件服务器可以与Salt State system 一起使用，因此它支持环境。如前所述，环境在master配置文件中定义。

<br>

- **GET_FILE**

可以在minion上使用 `cp.get_file` 函数从 master 下载文件。对于较大的文件，此函数支持gzip压缩，由于gzip是CPU密集型的，因此仅应在压缩率非常高的情况下使用。

```bash
salt '*' cp.get_file salt://vimrc /etc/vimrc


#启用模板
salt '*' cp.get_file "salt://{{grains.os}}/vimrc" /etc/vimrc template=jinja


#gzip范围: 1-9
salt '*' cp.get_file salt://vimrc /etc/vimrc gzip=5


#默认情况下不会创建新的目标目录，如果有需要，请使用`makedirs`参数
salt '*' cp.get_file salt://vimrc /etc/vim/vimrc makedirs=True
```

<br>

- **GET_DIR**

可以在minion上使用 `cp.get_dir` 函数从master下载整个目录。

```bash
salt '*' cp.get_dir salt://etc/apache2 /etc


#同样支持模板和压缩
salt '*' cp.get_dir salt://etc/{{pillar.webserver}} /etc gzip=5 template=jinja
```



<br/>
<br/>



### 客户端实例

FILE SERVER CLIENT INSTANCE

允许编写模块和应用程序的客户端实例来使用Salt文件系统。
文件服务器使用与Salt系统其余部分相同的身份验证和加密进行网络通信。

<br>

- **FILECLIENT MODULE**

`salt/fileclient.py` 模块用于设置从minion到master的通信。使用 `fileclient` 模块创建客户端实例时，需要传入minion配置。

```python
#在minion模块中使用fileclient模块时，可以传递内置的__opts__数据
import salt.minion
import salt.fileclient

def get_file(path, dest, saltenv='base'):
    '''
    Used to get a single file from the Salt master

    CLI Example:
    salt '*' cp.get_file salt://vimrc /etc/vimrc
    '''
    # Get the fileclient object
    client = salt.fileclient.get_file_client(__opts__)
    # Call get_file
    return client.get_file(path, dest, False, saltenv)



#__opts__数据不可用的minion模块之外创建一个fileclient实例
import salt.fileclient
import salt.config

def get_file(path, dest, saltenv='base'):
    '''
    Used to get a single file from the Salt master
    '''
    # Get the configuration data
    opts = salt.config.minion_config('/etc/salt/minion')
    # Get the fileclient object
    client = salt.fileclient.get_file_client(opts)
    # Call get_file
    return client.get_file(path, dest, False, saltenv)
```



<br/>
<br/>
<br/>



## 包管理

SALT PACKAGE MANAGER

> Status:  Technical Review

The Salt Package Manager(SPM)，使Salt formulas 得以打包，以简化向Salt Master 的分发。SPM的设计受到其他现有包系统的影响，包括RPM, Yum, Pacman。

![](/image/Salt/spm-overview.png)


<br/>


### SPM包构建

BUILDING SPM PACKAGES

> Status:  Technical Review

使用SPM的第一步是为要分发的每个方案构建包。可以在任何可以安装Salt的系统上构建软件包。


<br/>


#### 包构建概述

PACKAGE BUILD OVERVIEW

saltstack-formulas: <https://github.com/saltstack-formulas>

构建程序包，方案使用的所有 state, pillar, jinja, and file templates 都将组装到构建系统上的文件夹中。这些文件可以从GitHub 存储库克隆。

![](/images/Salt/spm-package-contents.png)


<br/>


#### 包安装概述

PACKAGE INSTALLATION OVERVIEW

构建包时，了解Salt master上文件的安装位置很有用。在安装过程中，除了 `pillar.example` 和 `FORMULA` 之外的所有文件都被直接复制到Salt master上的 spm state tree(`\srv\spm\salt`)。
如果根目录中存在 `pillar.example` 文件，则将其重命名为 `formula_name.sls.orig` 并放在 `pillar_path` 中。

![](/images/Salt/spm-package-extraction.png)


<br/>


#### 构建一个SPM formula包

BUILDING AN SPM FORMULA PACKAGE

- 将方案(formula)文件组装到构建系统上的文件夹中
- 创建一个FORMULA文件并将其放在包文件夹的根目录中
- 运行`spm build folder_name`，此包是构建并放在 `/srv/spm_build` 文件夹中: `spm build /path/to/salt-packages-source/myapp-formula`
- 将 `.spm` 文件复制到存储库系统上的文件夹


<br/>


#### 包类型

TYPES OF PACKAGES

SPM支持不同类型的包，每个包的功能由其名称表示。

- FORMULA: 默认情况下，此类包的大多数文件都位于 `/srv/spm/salt/` 目录中，上面说的`pillar.example`文件例外
- REACTOR: 默认情况下，此类包的文件位于 `/srv/spm/reactor/` 目录
- CONF: 此类软件包中的文件是Salt的配置文件，通常位于 `/etc/salt/` 目录


<br/>


#### 技术信息

TECHNICAL INFORMATION

软件包使用BZ2压缩的tar包构建，默认情况下，使用 `sqlite3` 驱动存储包数据库。
这些内容的支持是内置于Python中的，因此不需要外部依赖项。

属于SPM的所有其它文件都使用YAML，以实现可移植性，易用性和可维护性。


<br/>


#### 特定SPM装载器模块

SPM-SPECIFIC LOADER MODULES

SPM的设计与传统的包管理器相似，后者将文件应用于文件系统并将包元数据存储在本地数据库中。但是，由于现代基础架构通常超出了这些用例，因此SPM的某些部分已经分解为它们自己的模块集。

- PACKAGE DATABASE: 默认使用 `sqlite3`模块
- PACKAGE FILES: 默认使用`local`模块



<br/>
<br/>



### 分发SPM包

DISTRIBUTING SPM PACKAGES

> Status:  Technical Review

SPM包可以通过HTTP(S), FTP, FS 分发给Salt Master。SPM Repo 可以托管在可以安装Salt的任何系统上。安装Salt后，您可以在更新包或向存储库添加包时运行 `spm create_repo` 命令。

<br>

- **设置一个包的Repo**

构建包后，生成的SPM文件将放在 `srv/spm_build` 文件夹中。

<br>

- **添加一个包到Repo**

只需将SPM文件复制到repo文件夹，然后生成repo元数据即可添加新软件包。

<br>

- **生成Repo Metadata**

每次更新或向存储库添加SPM包时，请发出 `spm create_repo` 命令:

```bash
spm create_repo /srv/spm_build
```



<br/>
<br/>
<br/>



### 安装SPM包

INSTALLING SPM PACKAGES

> Status:  Technical Review

SPM软件包安装在Salt master中，使用Salt的所有包管理功能可以使用它们。


<br/>


#### 配置远程REPO

CONFIGURING REMOTE REPOSITORIES

在SPM可以使用存储库之前，需要做两件事。首先，Salt master 需要通过配置过程知道存储库的位置。然后它需要 pull down 存储库元数据。

- **Repo配置文件**

通过将每个存储库添加到每个 Salt Master 的 `/etc/salt/spm.repos.d/spm.repo` 文件来配置存储库，它包含Repo的名称和链接:

```yaml
my_repo:
  url: https://spm.example.com/


#https，请注意文件权限
my_repo:
  url: https://spm.example.com/
  username: user
  password: pass


#http(s), ftp, file
my_repo:
  url: file:///srv/spm_build
```

<br>

- **更新本地Repo Metadata**

使用 `spm update_repo` 命令下载 Repo 元数据。运行命令后，每个repo的文件都放在Salt master上的 `/var/cache/salt/spm` 中，如果添加存储库但它似乎没有显示，请检查此路径以验证是否找到了存储库。

```bash
spm update_repo

```


<br/>
<br/>


#### 更新文件根

UPDATE FILE ROOTS

SPM软件包安装在Salt master上的 `srv/spm/salt` 文件夹中，需要手动将此路径添加到Salt master上的文件根目录中。

```yaml
file_roots:
  base:
    1. /srv/salt
    2. /srv/spm/salt
```


<br/>
<br/>


#### 安装包

> 警告:
> 目前，SPM在安装之前不会检查文件是否已就位。这意味着现有文件将被覆盖而不会发出警告。

使用 `spm install` 命令来安装包:

```bash
spm install apache

#使用spm local install命令使用本地SPM文件安装SPM软件包
spm local install /srv/spm/apache-201506-1.spm
```


<br/>
<br/>


#### PILLARS

如果已安装的软件包包含Pillar数据，确保使用 pillar Top file 将安装的pillar用于必要的系统。


<br/>
<br/>


#### 删除包

安装包之后，使用 `spm remove` 命令来删除包:

```
#如果文件已被修改，则不会删除它们。空目录也将被删除
spm remove apache

````


<br/>
<br/>
<br/>


### SPM配置

有许多特定的SPM配置，可在Salt Master中配置，也可在SPM自己的配置文件(通常: `/etc/salt/spm`)中配置。如果在两个地方都进行了配置，则SPM配置文件优先。通常，不需要更改这些默认值。

```yaml
#SPM_LOGFILE
#Default: /var/log/salt/spm


#SPM_REPOS_CONFIG
#Default: /etc/salt/spm.repos


#SPM_CACHE_DIR
#Default: /var/cache/salt/spm


#SPM_DB
#Default: /var/cache/salt/spm/packages.db


#SPM_BUILD_DIR
#Default: /srv/spm_build


#SPM_BUILD_EXCLUDE
#Default: ['.git']
spm_build_exclude:
  - .git
  - .svn


#FORMULA
#Default dir: /srv/spm/salt/


#REACTOR
#Default dir: /srv/spm/reactor/


#CONF
#Default dir: /etc/salt/
```



<br/>
<br/>
<br/>



### FORMULA File


除了FORMULA本身，还必须存在描述包的FORMULA文件。栗子:

```yaml
name: apache
os: RedHat, Debian, Ubuntu, SUSE, FreeBSD
os_family: RedHat, Debian, Suse, FreeBSD
version: 201506
release: 2
summary: Formula for installing Apache
description: Formula for installing Apache
```


<br/>


#### 需要的字段

REQUIRED FIELDS

这个文件必须包含以下字段:

- NAME: 包名
- OS: 支持的操作系统
- OS_FAMILY: 支持的`os_family`
- VERSION: 包版本，建议使用`YYYYMM`格式
- MINIMUM_VERSION: Salt最低推荐版本
- RELEASE: 发行版
- SUMMARY: 简短描述
- DESCRIPTION: 详细描述


<br/>
<br/>


#### 可选字段

OPTIONAL FIELDS

如下字段也可能存在:

- TOP_LEVEL_DIR
- DEPENDENCIES
- OPTIONAL
- RECOMMENDED
- FILES
- LOCAL STATES
- TGT STATES
- TEMPLATING STATES
- LOADER MODULES
- REMOVING PACKAGES
- TECHNICAL INFORMATION
- SPM-SPECIFIC LOADER MODULES
- PACKAGE DATABASE
- PACKAGE FILES
- TYPES OF PACKAGES
- FORMULA
- REACTOR
- CONF



<br/>
<br/>
<br/>


### SPM开发指南

docs: <https://docs.saltstack.com/en/latest/topics/spm/dev.html>

先放着，后面有需要再来学习。




<br/>
<br/>
<br/>



## 在其它数据库中存储数据

STORING DATA IN OTHER DATABASES

SDB接口用于存储和检索数据，这些数据与  pillars and grains 不同，不一定是特定于特定部分的数据。最初的设计目标是允许将密码存储在安全数据库中，而不是纯文本文件。但是，作为通用数据库接口，它可以在概念上用于许多其它目的。


<br/>


### SDB CONF

要使用SDB接口，必须设置配置文件。要对Salt Master Command 可用，需在master配置文件中定义；对于在 Salt Minion 上执行的模块，可在 minion配置文件和 pillar 中进行配置。

栗子:

```yaml
#建议使用简单的名字，因为它在BSD URI中使用
mykeyring:
  driver: keyring
  service: system
```


<br/>
<br/>


### SDB URIS

SDB旨在使用紧凑的URL进行小型数据库查询。这允许用户在多个Salt配置区域内快速引用数据库值，而不会产生大量开销。

SDB URI 基本格式:

```yaml
#profile指的是master/minion配置中定义的profile
sdb://<profile>/<args>


#profile look like
kevinopenstack:
  driver: keyring
  service: salt.cloud.openstack.kevin

#URI look like
sdb://kevinopenstack/password
```


<br/>
<br/>


### 获取、配置和删除SDB值

GETTING, SETTING AND DELETING SDB VALUES

配置SDB驱动后，您可以使用sdb执行模块从中获取、设置和删除值。

SDB Modules:

- `get`
- `set`
- `delete`

```yaml
salt-call sdb.get sdb://kevinopenstack/password
salt-call sdb.set 'sdb://myvault/secret/salt/saltstack' 'super awesome'
salt-call sdb.delete 'sdb://mykvstore/foobar'


#runner system也可用
salt-run sdb.get 'sdb://myvault/secret/salt/saltstack'
salt-run sdb.set 'sdb://myvault/secret/salt/saltstack' 'super awesome'
salt-run sdb.delete 'sdb://mykvstore/foobar'
```


<br/>
<br/>


### 在文件中使用SDB URIS

USING SDB URIS IN FILES

SDB URI可用于配置文件和由渲染器系统（jinja，mako...）处理的文件。

栗子:

```
mykey: sdb://myetcd/mykey


#使用模块检索值
mykey = __salt__['config.get']('mykey')


#Templating renderers
{{ salt['config.get']('mykey') }}
{{ salt['sdb.get']('sdb://myetcd/mykey') }}
```



<br/>
<br/>
<br/>



## 非特权用户运行

RUNNING THE SALT MASTER/MINION AS AN UNPRIVILEGED USER

> 当master未以root用户身份运行时，某些Salt的操作无法正确执行。特别是PAM外部认证系统，因为它需要root访问权来检查身份验证。

默认使用root用户运行，有些人认为使用 non-root 用户运行更安全。使用non-root 用户并不会改变master访问minion的能力。由于许多人认为以 non-root 用于运行它们不会体现真正的安全优势，因此这就是默认使用root运行的原因。

<br>

通过在master配置文件中设置用户参数来启用非特权用户。
minion也有自己的用户参数，但是作为非特权用户运行minion将阻止它对用户，已安装的软件包等进行更改，除非在minion上设置访问控制（sudo等）以允许非root用户进行所需的更改。

还应该更改Salt使用到的目录和文件对于此用户的权限:

```
#/etc/salt
#/var/cache/salt
#/var/log/salt
#/var/run/salt
#...

chown -R user /etc/salt /var/cache/salt /var/log/salt /var/run/salt
```

<br>

由于 `salt-minion` 要执行由 Salt-Master 的 `salt 'minion' xxx` 命令，出于安全考虑，应该使用非特权用户执行，对此用户添加 `sudo` 权限。


```
# 添加普通用户, salt
# 使用 /sbin/nologin 可以连接Salt Master，但是无法执行 cmd.run。 会报错: This account is currently not available.
# 空密码用户是处于锁定状态，也就是无法进行登录，所以不必要担心安全问题。可以使用su salt
useradd -M -s /bin/sh salt

# 查看是否锁定
# passwd -S salt
# salt LK 2019-02-14 0 99999 7 -1 (密码已被锁定。)
# passwd -S zhang
# zhang PS 1969-12-31 0 99999 7 -1 (密码已设置，使用 SHA512 算法。)


# 修改对应文件夹权限
# 可能之前以root运行还存在未修改到的文件，请注意日志报错
chown -R salt /etc/salt /var/cache/salt /var/log/salt /var/run/salt


# 添加sudo
visudo
salt   ALL=(ALL)  NOPASSWD:ALL


# 修改 Master Config
vim /etc/salt/master

user: salt


# 修改 Minion Config
vim /etc/salt/minion

user: salt
sudo_user: salt


# 重启服务
systemcrt restart salt-master
systemctl restart salt-minion


# 测试
# 执行特权命令使用sudo
salt 'minion-xxx' cmd.run 'ls -l /root'

salt 'minion-xxx' cmd.run 'sudo ls -l /root'
```



<br/>
<br/>
<br/>



## 使用CRON

USING CRON WITH SALT


Salt Minion可以使用 `salt-call`命令启动自己的highstate:

```bash
#minion检查与master的状态
salt-call state.apply
```

<br/>

```bash
#USE CRON TO INITIATE A HIGHSTATE
#如果希望minion定期检查master
0 0 * * * salt-call state.apply
```



<br/>
<br/>
<br/>



## HARDENING SALT


本主题包含可用于保护和强化Salt环境的提示。如何最好地保护和加强您的Salt环境在很大程度上取决于您如何使用Salt，您在哪里使用Salt，您的团队结构，从何处获取数据以及您需要什么类型的访问（内部和外部）。

<br>

**一般强化技巧:**

- 限制谁可登录到Salt Master
- 使用SSH Key登录Salt Master
- 保护好SSH Key
- 使用堡垒机或VPN限制从Internet直接访问Salt Master
- 不要将Salt Master暴露在范围之外
- 强化系统
- 更新系统补丁
- 使用严密的防火墙规则

<br>

**Salt强化技巧:**

- 了解Salt版本可用性和补丁
- 使用Salt Client ACL，避免必须使用root才能执行Salt命令
- 使用Salt Client ACL，限制哪些用户可以使用什么命令
- 使用 External Pillar 将数据从外部源拉取到Salt中，以便非系统管理员可以提供配置数据，而无需访问Salt Master
- 使用版本控制的SLS文件，并在部署到生产环境前进行同行评审/代码审查流程
- 如果将Salt Master公开给外服服务，使用`salt-api`, SSL等来认证外部系统
- 利用Salt事件系统和 Reactor，允许minion向master发送信号，而无需直接访问
- 以non-root用户运行`salt-master` Daemon
- 使用 `disable_modules` 设置禁用将哪些模块加载到minions上
- 查看完整注释的示例master和minion配置文件
- 在特别敏感的minino上运行 `masterless-mode`，



<br/>
<br/>
<br/>



## SALT TRANSPORT


Salt的基本功能之一是远程执行。Salt有两个基本的信道与minion进行沟通，每个信道都需要C端(minion)和S端(master)的实现才能在Salt中工作。这些信道对将一起工作以实现通道接口所需的特定消息传递。


<br/>


### PUB CHANNEL

**pub(publish) channel** ，是master如何将job发送给minion。遍历发布系统的所有数据都应加密，以便只有Salt集群的成员才能解密发布。


<br/>
<br/>


### REQ CHANNEL

**req channel** ，是minion如何将数据发送给master。此接口主要用于获取文件和返回作业信息。在与master通信时，req channel有两个基本的接口:

- `send`: 保证消息至少被加密的基本方法，只有连接到同一个master的minions才能读取它，但不保证 minion-master 的机密性
- `crypted_transfer_decode_dictentry`: 此方法保证 minion-master 的机密性


<br/>
<br/>


### ZEROMQ传输

> zeromq是Salt当前的默认传输


**zeromq** 是一种可绑定多种语言的消息库(message lib)，Zeromq实现了一个用于消息传递的套接字接口，它具有套接字类型的特定语义。

- PUB CHANNEL
使用zeromq pub/sub socket，默认不使用zeromq的过滤，这意味着所有发布作业都被发送到所有minions和过滤的minion端。zeromq有一个发布端过滤，可在Salt中使用 `zmq_filtering` 来启用。

- REQ CHANNEL
使用zeromq req/rep sockets，这些套接字强制执行 send/recv 模式，强制salt通过这些套接字对序列化消息。这意味着虽然接口在minion上是异步的，但是在收到第一条消息的回复之前，我们不能发送第二条消息。



<br/>
<br/>



### TCP传输

**tcp transport** 是使用原始tcp套接字实现Salt的通道。由于这不是使用预定义的消息库，因此将在本文档中描述协议、消息语义等。

在master和minion上将`transport`修改为`tcp`来启用tcp传输。

```yaml
transport: tcp
```

<br>

> 警告:
> 建议在使用Syndics时，所有Masters和Minions都使用相同的传输，在高负载时，混合使用传输类型可能会导致错误。


<br/>
<br/>


#### WIRE PROTOCOL

TCP上的这种实现侧重于绝对效率的灵活性，这意味着我们可以花费几个字节的线路空间来提高未来的灵活性。看起来如下:

```
msgpack({'head': SOMEHEADER, 'body': SOMEBODY})
```

通过这种灵活的有线协议，我们可以实现我们想要的任何消息语义，包括在单个套接字上传递的多路复用消息。


<br/>
<br/>


#### TLS支持

tcp transport 允许 master/minion 通信可选地包装在TLS连接中。启用它很简单，master和minion只需先启用tcp传输，然后启用ssl。ssl选项作为dict传递，对应于传递给Python `ssl.wrap_socket` 函数的对应项。

简单栗子:

```yaml
ssl:
  keyfile: <path_to_keyfile>
  certfile: <path_to_certfile>
  ssl_version: PROTOCOL_TLSv1_2


#最小化配置
ssl: True
# Versions below 2016.11.4:
ssl: {}
```


<br/>
<br/>


#### CRYPTO

当前实现使用与zeromq传输相同的加密。


<br/>
<br/>


#### PUB CHANNEL

对于pub信道，我们发送没有 `message id` 的消息，远程端将其解释为单向发送。


<br/>
<br/>


#### REQ CHANNEL

对于req信道，我们发送带有 `message id` 的消息，它允许我们在套接字上多路复用消息。



<br/>
<br/>


### RAET传输


此处先跳过，后面有需要再来学习。

docs: <https://docs.saltstack.com/en/latest/topics/transports/raet/index.html>

> 注意：
> RAET传输处于早期开发阶段，虽然功能齐全，但尚未就其可靠性或安全性做出承诺。至于可靠性和安全性，使用的加密已经通过审核，我们的测试显示raet是可靠的。据此，我们仍在进行更多的安全审计并提高可靠性。



<br/>
<br/>
<br/>


## MASTER TOPS

docs: <https://docs.saltstack.com/en/latest/topics/master_tops/index.html>

先跳过，后面有需要再来学习。



<br/>
<br/>
<br/>


## RETURNERS


默认情况下，发送给Salt minions的命令的返回值将返回到Salt master，但是任何事情都可以使用结果数据完成。
通过使用 Salt Returner，可以将结果数据重定向到外部数据存储以进行分析和归档。

returner interface 允许将返回数据发送到可以接收数据的任何系统(如MySQL, MongoDB, Redis...)

<br>


### RETURNER MODULES

Returner: <https://docs.saltstack.com/en/latest/ref/returners/all/index.html#all-salt-returners>


<br>

当然，你也可以自己编写Returner。

docs: <https://docs.saltstack.com/en/latest/ref/returners/index.html>



<br/>
<br/>
<br/>



## RENDERERS

docs: <https://docs.saltstack.com/en/latest/ref/renderers/index.html>


Salt State System 通过从常用数据类型（如列表，字典和字符串）收集信息来运行。

SLS文件从它们写入的任何数据模板格式转换回要由Salt使用的Python数据类型。默认情况下，SLS文件呈现为Jinja模板，然后解析为YAML文档。由于状态系统唯一关心的是原始数据，因此SLS文件可以是任何可以设想的结构化格式。

目前支持:

- `Jinja + YAML`
- `Mako + YAML`
- `Wempy + YAML`
- `Jinja + json`
- `Mako + json`
- `Wempy + json`

可以编写渲染器以支持任何模板类型。这意味着Salt状态可以由XML、HTML、Puppet文件或任何可以转换为状态系统使用的Pythonic数据结构的格式管理。


<br/>
<br/>


### RENDERER MODULES

Renderer: <https://docs.saltstack.com/en/latest/ref/renderers/all/index.html>




<br/>
<br/>

---

<br/>
<br/>




# 使用

USING SALT


本节介绍使用Salt时需要了解的基本组件和概念。


<br/>


## GRAINS


> 注意:
> Grains解析为小写字母
> grains 是静态的，是不经常变动的数据。在更新它之后需要刷新，可调用此模块: `salt minion saltutil.refresh_modules`

Salt附带了一个用于获取有关底层系统的信息接口，这被称为 **grains interface** 。Grains 收集操作系统、域名、IP地址、kernel、OS类型、许多其它系统信息。

Grain 模块和组件可以使用 grain interface，以便在正确的系统上自动获得正确的salt minion命令。

Grains 数据是相对静态的，但如果系统信息发生变化，或者如果将新值分配给它，则 Grain数据将被刷新。


<br/>


### LISTING GRAINS


```bash
#可使用 grains.ls 模块列出 grains
salt '*' grains.ls


#可使用 grains.item 模块列出 grains data
salt '*' grains.items

```


<br/>
<br/>


### GRAINS IN THE MINION CONFIG


Grains 也可在 minion 配置文件中静态配置。

```yaml
grains:
  roles:
    - webserver
    - memcache
  deployment: datacenter4
  cabinet: 13
  cab_u: 14-15
```

然后，可以通过Salt检索特定于服务器的状态数据，或者在State系统内部使用以进行匹配。


<br/>
<br/>


### GRAINS IN /ETC/SALT/GRAINS


> 注意:
> 如果在 minion 配置文件中指定了grains, 则忽略 `/etc/salt/grains`

如果你不想将自定义 grains 放置在 minion 的配置文件中，你可将它放置于 minion 端的 `/etc/salt/grains` 下。

```yaml
#注意，没有顶级项 grains
roles:
  - webserver
  - memcache
deployment: datacenter4
cabinet: 13
cab_u: 14-15
```


<br/>
<br/>


### MATCHING GRAINS IN THE TOP FILE


使用Minion上正确配置的 Grains，Pillar/Highstate 中使用的 top file 可以非常高效。

```yaml
#你需为此 grains 定义 role
'roles:webserver':
  - match: grain
  - state0

'roles:memcache':
  - match: grain
  - state1
  - state2
```


<br/>
<br/>


### 编写grains


通过执行Salt core grains code 模块中的所有公共函数（即不以下划线开头的函数），然后是任何自定义 grains 模块中的函数，得到 grain 。grain 模块中的函数必须返回一个Python字典，其中字典键是grain的名称，每个键的值是该grain的值。

自定义 grain 模块应放在名为 `_grains` 的子目录中，该子目录位于 master 配置文件指定的 `file_roots` 下(默认路径 `/srv/salt/_grains`)。通过 ` saltutil.sync_grains` 或 ` saltutil.sync_all` 将自定义 grains 分发到 minions 。

```python
#grains容易编写，只需要返回一个字典即可
def yourfunction():
     # initialize a grains dictionary
     grains = {}
     # Some code for logic that sets grains like
     grains['yourcustomgrain'] = True
     grains['anothergrain'] = 'somevalue'
     return grains
```

<br>

**什么时候使用自定义的GRAIN**

在添加新 grain 的时候，请考虑数据是什么，大部分 grains 数据都应该是静态数据。

如果数据可能会发生变化，请考虑使用 *Pillar* 或 *执行模块*。如果它是一组简单的键/值对，那么pillar 是一个很好的匹配。如果编译信息需要运行系统命令，那么将此信息放在执行模块中可能更好。

<br>

**载入自定义的GRAIN**

如果您有多个函数指定从 `main` 函数调用的GRAIN，请确保使用下划线添加粒度函数名称。

```python
#!/usr/bin/env python
def _my_custom_grain():
    my_grain = {'foo': 'bar', 'hello': 'world'}
    return my_grain


def main():
    # initialize a grains dictionary
    grains = {}
    grains['my_grains'] = _my_custom_grain()
    return grains
```

事例输出:

```
# salt-call --local grains.items
local:
    ----------
    <Snipped for brevity>
    my_grains:
        ----------
        foo:
            bar
        hello:
            world
```

但是，如果你没有在 `my_custom_grain` 函数前面添加下划线，那么该函数将在items输出中由Salt呈现两次：一次用于 `my_custom_grain` 调用本身，另一次用于在 `main` 函数中调用它。

```
# salt-call --local grains.items
local:
----------
    <Snipped for brevity>
    foo:
        bar
    <Snipped for brevity>
    hello:
        world
    <Snipped for brevity>
    my_grains:
        ----------
        foo:
            bar
        hello:
            world
```


<br/>
<br/>


### 优先权

PRECEDENCE


Core grain 可被 custom grain所覆盖，因此应该记住它们的优先顺序：

1. Core grains
2. Custom grains in `/etc/salt/grains`
3. Custom grains in `/etc/salt/minion`
4. Custom grain modules in `_grains` directory, synced to minions


<br/>
<br/>


### GRAINS栗子


示例: <https://github.com/saltstack/salt/blob/develop/salt/grains/core.py>



<br/>
<br/>
<br/>



## PILLAR

STORING STATIC DATA IN THE PILLAR

> 注意:
> Pillar用来存储敏感数据
> Pillar data 在 master 上进行编译。此外，给定minion 的 pillar data 只能由配置文件中定义的 目标minion 来访问。这使得 pillar data 可用于存储特定minion的敏感信息很有用。
> 我觉得 salt pillar 有点类似于 K8S seceret


**Pillar** 是一个Salt接口，用来为分布式 minions 提供 全局值(global value)。


<br/>
<br/>


### 声明Master Pillar

DECLARING THE MASTER PILLAR


Salt Master 维护一个 `pillar_roots` 设置，该设置与Salt文件服务器中使用的 `file_roots` 的结构相匹配。然后根据 top file 中的匹配器将 pillar data 映射到minions。

master配置文件中的例子:

```yaml
#它不能是 state tree 下的子目录
pillar_roots:
  base:
    - /srv/pillar
```

```yaml
#/srv/pillar/top.sls
#此 top file 声明在 base 环境中，匹配所有minions的glob将具有在 `package` 中找到的 pillar data 可用于它
#假设pillar_root使用上面的值，则如下packages将定位到/srv/pillar/packages.sls
base:
  '*':
    - packages
```

```yaml
#可将任意数量的匹配器添加到base环境中
#/srv/pillar/top.sls
base:
  '*':
    - packages
  'web*':
    - vim
```

<br>

示例显示了如何使用其它标准顶部匹配类型将特定 salt pillar data 传递给具有不同属性的minions:

```yaml
#grains matcher
dev:
  'os:Debian':
    - match: grain
    - servers
```

`/srv/pillar/packages.sls`, 所有minions都具有`company`这个键值对

```jinja
{% if grains['os'] == 'RedHat' %}
apache: httpd
git: git
{% elif grains['os'] == 'Debian' %}
apache: apache2
git: git-core
{% endif %}

company: Foo Industries
```

```jinja
apache:
  pkg.installed:
    - name: {{ pillar['apache'] }}
git:
  pkg.installed:
    - name: {{ pillar['git'] }}
```



<br/>
<br/>


### 动态Pillar环境

DYNAMIC PILLAR ENVIRONMENTS


如果在 `pillar_roots` 中指定了环境 `__env__`，在 `pillar_roots` 中未明确指定的所有环境都将映射到 `__env__` 中的目录。这使得人们可以使用基于 dynamic git branch 的环境来处理 state/pillar 文件，并使用相同的基于文件的pillar应用于所有环境。

```yaml
pillar_roots:
  __env__:
    - /srv/pillar

ext_pillar:
  - git:
    - __env__ https://example.com/git-pillar.git
```


<br/>
<br/>


### PILLAR NAMESPACE FLATTENING


单独的 pillar SLS 文件全部合并为单个键值对字典。当在多个SLS文件中定义相同的Key时，如果不注意 pillar SLS 文件的布局方式，可能会导致意外情况发生。

栗子：

```yaml
#top.sls

base:
  '*':
    - packages
    - services


# packages.sls包含
bind: bind9


#services.sls包含
bind: named


#此二者相同，package之后会评估 services，所以bind9这个值会丢失
#在请求 pillar bind key时，将返回 named



#使用多层次的pillar文件可能会更好
#packages.sls
packages:
  bind: bind9
```

pillar 文件按照它们在 top file 中列出的顺序应用。因此，冲突的Key将以最后一次胜利的方式的值覆盖！



<br/>
<br/>


### pillar字典合并

PILLAR DICTIONARY MERGING


如果在多个 pillar SLS 文件中定义了相同的 pillar key，并且两个文件中的key都引用了嵌套的字典，则将以递归方式合并这些字典中的内容。

```yaml
#top.sls与上面相同


#packages.sls
bind:
  package-name: bind9
  version: 9.9.5


#services.sls
bind:
  port: 53
  listen-on: any
```

pillar字典结果如下:

```
salt-call pillar.get bind
local:
    ----------
    listen-on:
        any
    package-name:
        bind9
    port:
        53
    version:
        9.9.5
```


<br/>
<br/>


### 包含其它pillars

INCLUDING OTHER PILLARS


与state文件类似，pillar SLS 文件可能包含其它 pillar 文件。有两种语法可实现此目的：

- 简单包含表单

```yaml
include:
  - users
```

- 完整包含表单

```yaml
include:
  - users:
      defaults:
          sudo: ['bob', 'paul']
      key: users
```


<br/>
<br/>


### 内存中的pillar数据与一经请求的pillar数据

IN-MEMORY PILLAR DATA VS. ON-DEMAND PILLAR DATA


由于编译pillar数据在计算上是昂贵的，因此minion将在内存中维护pillar数据的副本，以避免在每次请求pillar数据时要求master重新编译并向其发送pillar数据的副本。这个内存中(in-mem)的pillar数据是 `pillar.item`, `pillar.get`,`pillar.raw` 函数返回的内容。

对于自定义编写的可执行模块，内存中的pillar数据在 `__pillar__` 字典下可用。

内存中的pillar是在minion启动时生成的，可使用 `saltutil.refresh_pillar ` 函数进行刷新。此函数触发minion异步刷新内存中的pillar数据，并始终返回`None`。

```bash
salt '*' saltutil.refresh_pillar
```

与内存中pillar数据相反，某些动作会触发pillar数据的编译，以确保最新的pillar数据可用。这些动作包括:

- 运行 states
- 运行 `pillar.items`

执行这些操作不会刷新内存中的pillar数据。因此，如果pillar数据被修改，然后运行state，状态将看到更新后的pillar数据，但是`pillar.item`, `pillar.get`, `pillar.raw` 将不会看到更新，除非使用 `saltutil.refresh_pillar` 函数进行刷新。


<br/>
<br/>


### 如何处理pillar环境

HOW PILLAR ENVIRONMENTS ARE HANDLED


当使用多个pillar环境时，默认行为是将来自所有环境的pillar数据合并在一起。

minion配置项 ` pillarenv` 可用于强制minion仅考虑来自单个环境的pillar配置。这对于需要在 test/QA 环境中运行具有备用pillar数据的状态或在推送pillar数据之前测试pillar数据的更改的情况非常有用。

```yaml
#minion config 栗子
pillarenv: base

#在编译内存中的pillar数据时，这将导致minion忽略除base之外的所有其它pillar环境
```


```bash
#pillarenv命令行参数 可以覆盖 配置文件中的值
salt '*' state.apply mystates pillarenv=testing
```

<br>

可以使用minion配置选项 `pillarenv_from_saltenv` 将 `pillarenv` 固定到有效的 `saltenv`。当此参数设置为`True`时，如果在运行状态时指定了特定的 `saltenv`，则 `pillarenv`将与之相同。
如果指定了 `pillarenv`，它将覆盖此行为。

```bash
salt '*' state.apply mystates saltenv=dev
salt '*' state.apply mystates saltenv=dev pillarenv=dev


salt '*' state.apply mystates saltenv=dev pillarenv=qa
```


<br/>
<br/>


### 查看pillar数据

VIEWING PILLAR DATA


要查看pillar数据 ，使用 `pillar` 执行模块。此模块包含如下函数:

- `pillar.item`: 从内存中的pillar daj中检索一个或多个key的值
- `pillar.items`: 编译一个新的pillar数据并返回它，保持内存中的pillar数据不变。但是，如果将 pillar key 传递给此函数，则此函数的作用类似于 `pillar.item`
- `pillar.raw`: 从内存中返回整个pillar dictionary
- `pillar.get`: 与Python dict中的`get`方法相同，但有一个增强功能，可以使用冒号`:`作为分隔符遍历嵌套的dicts


```yaml
#pillar嵌套栗子
foo:
  bar:
    baz: qux
```

从SLS公式或模板中提取是这样的:

```jinja
{{ pillar['foo']['bar']['baz'] }}
```

使用新的 `pillar.get` 函数可以安全地收集数据并设置默认值，如果值不可用，则允许模板回退。这使得处理嵌套结构变得更加容易。

```jinja
{{ salt['pillar.get']('foo:bar:baz', 'qux') }}
```


<br/>
<br/>


### 在CLI设置pillar数据

SETTING PILLAR DATA AT THE COMMAND LINE

> 注意:
> 当通过命令行中的pillar发送敏感数据时，所有minions都将收到包含该数据的发布，并且不会限制为目标minion
> 在某些情况下，这可能存在安全问题


pillar数据可在命令行设置:

```bash
salt '*' state.apply pillar='{"cheese": "spam"}'
```


<br/>
<br/>


### Pillar加密

PILLAR ENCRYPTION


Salt的渲染器系统可用于解密pillar数据。这允许pillar项目以加密状态存储，并在编译期间解密。

<br>

**加密pillar SLS**

考虑如下 pillar SLS 文件:

```yaml
secrets:
  vault:
    foo: |
      -----BEGIN PGP MESSAGE-----

      hQEMAw2B674HRhwSAQgAhTrN8NizwUv/VunVrqa4/X8t6EUulrnhKcSeb8sZS4th
      W1Qz3K2NjL4lkUHCQHKZVx/VoZY7zsddBIFvvoGGfj8+2wjkEDwFmFjGE4DEsS74
      ZLRFIFJC1iB/O0AiQ+oU745skQkU6OEKxqavmKMrKo3rvJ8ZCXDC470+i2/Hqrp7
      +KWGmaDOO422JaSKRm5D9bQZr9oX7KqnrPG9I1+UbJyQSJdsdtquPWmeIpamEVHb
      VMDNQRjSezZ1yKC4kCWm3YQbBF76qTHzG1VlLF5qOzuGI9VkyvlMaLfMibriqY73
      zBbPzf6Bkp2+Y9qyzuveYMmwS4sEOuZL/PetqisWe9JGAWD/O+slQ2KRu9hNww06
      KMDPJRdyj5bRuBVE4hHkkP23KrYr7SuhW2vpe7O/MvWEJ9uDNegpMLhTWruGngJh
      iFndxegN9w==
      =bAuo
      -----END PGP MESSAGE-----
    bar: this was unencrypted already
    baz: |
      -----BEGIN PGP MESSAGE-----

      hQEMAw2B674HRhwSAQf+Ne+IfsP2IcPDrUWct8sTJrga47jQvlPCmO+7zJjOVcqz
      gLjUKvMajrbI/jorBWxyAbF+5E7WdG9WHHVnuoywsyTB9rbmzuPqYCJCe+ZVyqWf
      9qgJ+oUjcvYIFmH3h7H68ldqbxaAUkAOQbTRHdr253wwaTIC91ZeX0SCj64HfTg7
      Izwk383CRWonEktXJpientApQFSUWNeLUWagEr/YPNFA3vzpPF5/Ia9X8/z/6oO2
      q+D5W5mVsns3i2HHbg2A8Y+pm4TWnH6mTSh/gdxPqssi9qIrzGQ6H1tEoFFOEq1V
      kJBe0izlfudqMq62XswzuRB4CYT5Iqw1c97T+1RqENJCASG0Wz8AGhinTdlU5iQl
      JkLKqBxcBz4L70LYWyHhYwYROJWjHgKAywX5T67ftq0wi8APuZl9olnOkwSK+wrY
      1OZi
      =7epf
      -----END PGP MESSAGE-----
    qux:
      - foo
      - bar
      - |
        -----BEGIN PGP MESSAGE-----

        hQEMAw2B674HRhwSAQgAg1YCmokrweoOI1c9HO0BLamWBaFPTMblOaTo0WJLZoTS
        ksbQ3OJAMkrkn3BnnM/djJc5C7vNs86ZfSJ+pvE8Sp1Rhtuxh25EKMqGOn/SBedI
        gR6N5vGUNiIpG5Tf3DuYAMNFDUqw8uY0MyDJI+ZW3o3xrMUABzTH0ew+Piz85FDA
        YrVgwZfqyL+9OQuu6T66jOIdwQNRX2NPFZqvon8liZUPus5VzD8E5cAL9OPxQ3sF
        f7/zE91YIXUTimrv3L7eCgU1dSxKhhfvA2bEUi+AskMWFXFuETYVrIhFJAKnkFmE
        uZx+O9R9hADW3hM5hWHKH9/CRtb0/cC84I9oCWIQPdI+AaPtICxtsD2N8Q98hhhd
        4M7I0sLZhV+4ZJqzpUsOnSpaGyfh1Zy/1d3ijJi99/l+uVHuvmMllsNmgR+ZTj0=
        =LrCQ
        -----END PGP MESSAGE-----
```

编译pillar数据时，结果将被解密:

```bash
# salt myminion pillar.items
myminion:
    ----------
    secrets:
        ----------
        vault:
            ----------
            bar:
                this was unencrypted already
            baz:
                rosebud
            foo:
                supersecret
            qux:
                - foo
                - bar
                - baz
```

必须告诉Salt要解密的pillar数据的哪部分。这使用 `decrypt_pillar` 配置选项完成的:

```yaml
decrypt_pillar:
  - 'secrets:vault': gpg


#可以使用 ` decrypt_pillar_delimiter` 配置项指定分隔符
decrypt_pillar:
  - 'secrets|vault': gpg

decrypt_pillar_delimiter: '|'
```

<br>

**在CLI加密pillar数据**

以下函数支持通过pillar参数在CLI上传递pillar数据:

- `pillar.items`
- `state.apply`
- `state.highstate`
- `state.sls`


<br/>
<br/>


### MASTER CONFIG IN PILLAR


为方便起见，存储在Master配置文件中的数据可以在所有minion的pillar中使用。这使得服务和系统的全局配置非常容易。但如果敏感数据存储在Master配置中，则可能不需要。默认情况下禁用此选项。

```
#在minion config 启用它
pillar_opts: True
```


<br/>
<br/>


### MINION CONFIG IN PILLAR


可在pillar上设置Minion配置选项。您要修改的任何选项都应位于pillar的第一级，与配置文件中的选项相同。
例如，要配置MySQL Salt执行模块使用的MySQL root密码，请设置以下pillar变量:

```yaml
mysql.pass: hardtoguesspassword
```



<br/>
<br/>
<br/>



## TARGETING MINIONS


目标minions 指定哪些minions应该运行命令，或通过匹配主机名、系统信息、定义的组、甚至其组合来执行状态。

```bash
#栗子
#重新启动指定目标为web1的机器的 apache httpd，并且命令将仅在此一个minion上运行
salt web1 apache.signal restart
```

在State的 top file 中情况也类似，如`webserver.sls`:

```yaml
base:
  'web1':
    - webserver
```


<br/>


### TARGETING WITH GRAINS


Grains Interface 内置于Salt中，用于收集系统属性。因此，可以调用在 特定操作系统/特定内核 上运行的minions来执行函数。

```
salt '*' grains.items
#查看对应的grains信息


#-G, Grains
#使用上面的grains信息匹配目标
salt -G 'os:Fedora' test.ping
salt -G 'host:xxx' test.ping
```


<br/>
<br/>


### 复合条件

COMPOUND TARGETING


可以结合使用多个目标接口来确定命令目标，通过使用 `and` 和 `or`语句:

```
#默认类型为 glob
#可通过 类型字母@ 来指定类型
#G@, 表示Grains
#E@, 表示RE
salt -C 'G@os:Debian and webser* or E@db.*' test.ping
```


<br/>
<br/>


### 节点组目标

NODE GROUP TARGETING


对于某些情况，可以方便地使用预定义的一组minions来执行命令。这可以使用所谓的 **节点组(nodegroups)** 来完成。节点组允许在Master配置文件中声明预定义的复合目标，作为必须键入复杂复合表达式的一种简写。

```yaml
nodegroups:
  group1: 'L@foo.domain.com,bar.domain.com,baz.domain.com and bl*.domain.com'
  group2: 'G@os:Debian and foo.domain.com'
  group3: 'G@os:Debian and N@group1'
```


<br/>
<br/>


### 高级的目标方法

ADVANCED TARGETING METHODS

url: <https://docs.saltstack.com/en/latest/topics/targeting/index.html#advanced-targeting-methods>


后面再来学习。



<br/>
<br/>
<br/>



## MINE

THE SALT MINE


**Salt Mine** 用于从Minions收集任意数据(arbitrary data)并将其存储在Master上，它只维护最新数据。通过 `salt.modules.mine` 模块将所有数据提供给所有Minions。

<br>

**MINE VS GRAINS**

Mine数据要比Grains数据更新。
Grains在非常有限的基础上更新数据，主要是静态数据。
当Minions需要来自其它Minions的数据时，Mines旨在取代慢速对等发布调用。Salt Mine 运行在 Master，而不是让一个minion与其它minions联系以获取数据。可从所有minion的每个 `Minion Interval` 收集它，在任何给定时间产生几乎新鲜的数据，而且开销更少。


<br/>
<br/>


### Mine函数


要启用Salt Mine，需要将 `mine_functions` 选项应用于Minion。此选项可通过 minion config/minion pillar 来应用。它指示正在执行的函数，并传入参数。`salt.module`中提供了函数列表。如果没有传递参数，则必须添加一个空列表。

```yaml
mine_functions:
  test.ping: []
  network.ip_addrs:
    interface: eth0
    cidr: '10.0.0.0/8'
#做了网络范围限制
```

<br>

**Mine函数别名**

函数别名可用于提供友好地名称、使用意图、允许使用不同参数多次调用相同函数。传递位置参数和KV参数有不同的语法，不支持混合位置参数和KV参数。

```yaml
mine_functions:
  network.ip_addrs: [eth0]
  networkplus.internal_ip_addrs: []
  internal_ip_addrs:
    mine_function: network.ip_addrs
    cidr: 192.168.0.0/16
  ip_list:
    - mine_function: grains.get
    - ip_interfaces
```


<br/>
<br/>


### MINE INTERVAL


Mine Interval 函数在Minion启动时执行，并由调度程序以给定间隔(interval)执行。

```
#minion config
#默认60m
mine_interval: 60
```


<br/>
<br/>


#### MINE IN SALT-SSH


由于Minions不能提供它们自己的 `mine_functions` 配置，所以在三个地方之一中检索指定的mine函数的参数，按以下顺序搜索:

1. Roster data
2. Pillar
3. Master config




<br/>
<br/>
<br/>




## RUNNERS


**Salt runners** 是使用 `salt-run` 命令执行的便捷应用程序。
Salt runners 与 Salt  execution modules 的工作方式类似。但是，它在 Salt master本身 而不是 远程Salt minions 上执行。

Salt runner 可以是 一个简单的客户调用 或 一个复杂的应用程序。


<br/>


### RUNNER MODULES


The full list of runners: <https://docs.saltstack.com/en/latest/ref/runners/all/index.html#all-salt-runners>


<br/>
<br/>


### 编写Runner

WRITING SALT RUNNERS


Salt runner 的编写方式也与 Salt execution module 类似。两者都是包含函数的Python模块，每个公共函数都是一个可以通过 `salt-run` 命令执行的 Runner。

```bash
#栗子
#在runners目录中创建了名为test.py的Python模块，此模块包含一个名为foo的函数
#则runner可以这样调用它
salt-run test.foo
```

<br>

runner有几种控制输出的选项:

```python
#runner上的任何print语句会自动触发到master的事件总线上
def a_runner(outputter=None, display_progress=False):
    print('Hello world')
    ...
```

runner 还可以发送进度事件，该进度事件在runner执行期间显示给用户。并且如果runner的 `display_progress` 参数设置为 `True`，则也通过事件总线传递。

自定义的runner可以通过 `__jid_event_.fire_event()` 方法 发送自己的进程事件:

```python
if display_progress:
    __jid_event__.fire_event({'message': 'A progress message'}, 'progress')
```


<br/>
<br/>


### 同步/异步

SYNCHRONOUS VS. ASYNCHRONOUS


可以异步触发runner，这将立即返回控制。在这种情况下，如果从命令行使用 `salt-run`，则不会向用户显示任何输出。如果以编程方式使用，则不会返回任何结果。如果需要结果，则必须从runner到事件总线，或通过其它方式收集。

在异步模式下运行运行程序时，`--progress` 标志不会将输出传递给 `salt-run` 的CLI。但是，依然会在总线上触发进程事件。

在同步模式（默认情况），在runner完成执行之前，不会返回控制。

要添加自定义的runner，将它们放置到master config 中的 ` runner_dirs` 配置项的位置里去。


<br/>
<br/>


### 栗子

Salt 发行版的runner栗子: <https://github.com/saltstack/salt/blob/develop/salt/runners>



<br/>
<br/>
<br/>



## SALT ENGINES


Salt Engines are long-running, external system processes that leverage Salt.

- Engines 可以访问Salt配置，执行modules和runner(`__opts__`, `__salt__`, `__runners__`)
- Engines 在由Salt监视的单独进程中执行。如果引擎停止，会自动重启它
- Engines 可运行在 Master 和Minions 上

SaltEngines 增强并取代了 [外部进程(external processes)](https://docs.saltstack.com/en/latest/topics/ext_processes/index.html#ext-processes) 功能。


<br/>


### 配置


Salt Engines 可在 master/minion config 中的 `engines` 部分进行配置:

```yaml
engines:
  - logstash:
      host: log.my_network.com
      port: 5959
      proto: tcp
```

Salt Engine 必须位于 Salt path 下，或者，你可以在 master config 中添加 `engines_dirs` 选项让Salt去查找引擎，它的值为要搜索的目录列表。

```yaml
engines_dirs:
  - /home/bob/engines
```


<br/>
<br/>


### 编写引擎


Salt source 提供了一个栗子引擎: <https://github.com/saltstack/salt/blob/develop/salt/engines/test.py>

要开发一个引擎，唯一的要求是你的模块实现了`start()`函数。



<br/>
<br/>
<br/>



## YAML


docs: <https://docs.saltstack.com/en/latest/topics/yaml/index.html>


SLS文件的默认渲染器是YAML渲染器。
Salt使用一小部分YAML来映射非常常用的数据结构(如list/dict)。YAML渲染器的工作是采用YAML数据结构并将其编译为Python数据结构以供Salt使用。



<br/>
<br/>
<br/>



## JINJA

docs: <https://docs.saltstack.com/en/latest/topics/jinja/index.html>


[Jinja](http://jinja.pocoo.org/) 是SLS文件中的默认模板语言。



<br/>
<br/>
<br/>



## 教程索引

TUTORIALS INDEX

docs: <https://docs.saltstack.com/en/latest/topics/tutorials/index.html>



<br/>
<br/>
<br/>



## TROUBLESHOOTING

docs: <https://docs.saltstack.com/en/latest/topics/troubleshooting/index.html>



<br/>
<br/>
<br/>



## FAQ

faq: <https://docs.saltstack.com/en/latest/faq.html>



<br/>
<br/>
<br/>



## 最佳实践

SALT BEST PRACTICES

docs: <https://docs.saltstack.com/en/latest/topics/best_practices.html>


Salt的极端灵活性导致许多关于配置文件结构的问题。
本节旨在通过示例和代码阐明这些要点。


<br/>


### 通用规则

GENERAL RULES

- 应尽可能强调模块性和清晰度
- 在Pillars和States之间建立明确的关系
- 在有意义的情况下使用变量，但不要过度使用变量
- 将敏感数据存储在 Pillar 中
- 不要在 pillar top file 中使用 grains 匹配任何敏感pillar


<br/>
<br/>


### 构造STATES和FORMULAS


在构造Salt States和Formulas时，重要的是从目录结构开始。恰当的目录结构通过可视查状态名称清楚地为用户定义每个状态的功能。



<br/>
<br/>


### 构造Pillar


Pillar 用于存储于Minions有关的安全的和不安全的数据。在设计 `/srv/pillar` 目录的结构时，其中包含的 Pillar 应该再次专注于清晰简洁的数据，用户可以轻松查看、修改、理解这些数据。


<br/>
<br/>


### 变量的灵活性

VARIABLE FLEXIBILITY


Salt允许用户在SLS文件中定义变量。创建状态变量时应为用户提供尽可能多的灵活性。这意味着应该清楚地定义变量并且易于操作，并且在未正确定义变量的情况下应该存在合理的默认值。



<br/>
<br/>


### 状态模块化

MODULARITY WITHIN STATES


确保状态是模块化的是理解Salt的关键概念之一。在创建状态时，用户必须考虑重新使用状态的次数，以及它依赖于操作的次数。


<br/>
<br/>


### 存储安全数据

STORING SECURE DATA


安全数据是指您不希望与访问服务器的任何人分享的任何信息，这可能包括密码、密钥...

由于连接的每个服务器都可以访问状态中的所有数据，因此将安全数据存储在Pillar中非常重要。这将确保只有那些需要此安全数据的服务器才能访问它。




<br/>
<br/>

---

<br/>
<br/>




# 远程执行

REMOTE EXECUTION


在远程主机上运行 预定义的 或 任意命令 —— 也称为远程执行，是Salt的核心功能。modules 和 returners 是远程执行的两个关键要素。

**Salt Execution Modules**
远程执行系统调用Salt执行模块来执行各种任务。这些模块提供了各种各样的功能。


<br/>


## 执行模块

EXECUTION MODULES


Full list of execution modules: <https://docs.saltstack.com/en/latest/ref/modules/all/index.html#all-salt-modules>



<br/>
<br/>
<br/>



## 编写执行模块

WRITING EXECUTION MODULES


Writing execution modules: <https://docs.saltstack.com/en/latest/ref/modules/index.html#writing-execution-modules>



<br/>
<br/>
<br/>



### 教程

REMOTE EXECUTION TUTORIAL


Salt调用由三个主要组件组成:

```bash
salt '<target>' <function> [arguments]
```

<br>

**TARGET**

目标组件允许您过滤哪些minions应运行以下函数，默认的过滤器是glob的`minion.id`。

```bash
salt '*' test.ping
salt '*.example.org' test.ping


#基于Grains
salt -G 'os:Ubuntu' test.ping


#基于RE
salt -E 'virtmach[0-9]' test.ping


#指定一个列表
salt -L 'foo,bar,baz,quo' test.ping


#复合目标
salt -C 'G@os:Ubuntu and webser* or E@database.*' test.ping
```

<br>

**FUNCTION**

函数是模块提供的一些功能。Salt附带了大量可用的功能。

```bash
#列出minion上可用的所有函数
salt '*' sys.doc


#显示当前可用的minions
salt '*' test.ping



#运行任意shell命令
salt '*' cmd.run 'uname -a'
```

<br>

**ARGUMENTS**

使用空格分割函数的参数，也支持关键字参数。

```bash
salt '*' cmd.exec_code python 'import sys; print sys.version'


#关键字参数, kwarg=argument
salt '*' pip.install salt timeout=5 upgrade=True
```



<br/>
<br/>
<br/>



## 在minions上运行命令

RUNNING COMMANDS ON SALT MINIONS


Salt Master 上的root用户可以通过 CLI Client 控制Salt。 Salt CLI Client 使用 Salt Client API 与 Salt Master Server 进行通信。Salt Client 简单易用。
使用 Salt Client 命令可以轻松发送给 minions。


<br/>


### 使用Salt命令

USING THE SALT COMMAND


**Salt command** 需要一些组件来向 Salt minions 发送信息。需要定义目标minions，调用函数和函数需要的任何参数。

```bash
#DEFINING THE TARGET MINIONS
salt '*foo.com' sys.doc
salt -E '.*' cmd.run 'ls -l | grep foo'
salt -L foo.bar.baz,quo.qux cmd.run 'ps aux | grep foo'


#CALLING THE FUNCTION
salt '*' cmd.exec_code python 'import sys; print sys.version'
salt '*' pip.install salt timeout=5 upgrade=True
salt '*' cmd.run 'echo "Hello: $FIRST_NAME"' saltenv='{FIRST_NAME: "Joe"}'
salt '*' test.arg_repr 'echo "Hello: $FIRST_NAME"' saltenv='{FIRST_NAME: "Joe"}'


#FINDING AVAILABLE MINION FUNCTIONS
salt '*' sys.doc


#COMPOUND COMMAND EXECUTION
#将一系列命令发送到单个目标，可以在单个发布中发送命令。这可以更快地收集信息组，并降低网络对重复命令的压力
#复合命令执行的工作原理是发送函数和参数列表，而不是发送单个函数和参数
#这些函数按照它们在命令行中定义的顺序在minion上执行，然后所有命令中的数据都在字典中返回

salt '*' cmd.run,test.ping,test.echo 'cat /proc/cpuinfo',,foo

#cmd.run的参数为'cat /proc/cpuinfo'
#test.ping的参数为空
#test.echo的参数为foo


#传递的命令包含参数
salt '*' cmd.run,test.ping,test.echo 'echo "1,2,3"' , , foo


#还可指定参数分割符
salt --args-separator=:: '*' some.fun,test.echo params with , comma :: foo
```



<br/>
<br/>
<br/>



## EXECUTORS


minion使用 **执行程序(executor)** 来执行模块函数(module func)。执行程序可用于修改函数行为，执行任何执行前的步骤或以特定方式执行，如 `sudo执行器` 。

执行程序可以作为列表传递，它们将在指令中逐个使用。
如果执行程序返回 `None` ，则将调用下一个执行程序；如果执行程序返回`非None` ，则终止执行序列，并将返回的值用作结果。这是一种执行器可以控制模块执行作为过滤器的方式。

执行器列表可以通过如下方式在 minion config 中传递:

```yaml
module_executors:
  - splay
  - direct_call
splaytime: 30
```

也可在CLI中操作:

```bash
salt -t 40 --module-executors='[splay, direct_call]' --executor-opts='{splaytime: 30}' '*' test.ping
```

通过netapi调用的相同命令将如下所示:

```bash
curl -sSk https://localhost:8000 \
    -H 'Accept: application/x-yaml' \
    -H 'X-Auth-Token: 697adbdc8fe971d09ae4c2a3add7248859c87079' \
    -H 'Content-type: application/json' \
    -d '[{
        "client": "local",
        "tgt": "*",
        "fun": "test.ping",
        "module_executors": ["splay", "direct_call"],
        "executor_opts": {"splaytime": 10}
        }]'
```


<br/>
<br/>


### 编写执行器

WRITING SALT EXECUTORS


Salt Executor 的编写方式与 Salt Execution Module 类似。执行器是一个 python 模块，放在 `executors` 文件夹中，包含带有如下签名的 `execute` 函数:

```python
def execute(opts, data, func, args, kwargs)
```

参数:

- **opts**: 包含minion配置选项的字典
- **data**: 包含载入数据(通过 CLI/API 传递给 `executor_opts` )的字典
- **func, args, kwargs**: 要执行的函数及其参数
- **Returns**: 如果必须使用下一个执行器继续执行序列，则为 `None`




<br/>
<br/>

---

<br/>
<br/>




# 配置管理

CONFIGURATION MANAGEMENT


Salt包含一个强大而灵活的配置管理框架，该框架构建在远程执行核心之上。该框架在minions上执行，通过呈现特定于语言的状态文件，允许同时配置数万台主机。

Salt State 系统的核心是 SLS(SaLt State File)。SLS表示系统应处于的状态，并设置为以简单格式包含此数据。这通常称为配置管理(Configuration Management)。

<br>

**State:**

- Full list of states: <https://docs.saltstack.com/en/latest/ref/states/all/index.html#all-salt-states>
- Pillar System: <https://docs.saltstack.com/en/latest/topics/pillar/index.html#pillar>
- Highstate data structure: <https://docs.saltstack.com/en/latest/ref/states/highstate.html#states-highstate>
- Writing states: <https://docs.saltstack.com/en/latest/ref/states/writing.html#state-modules>

Salt执行模块与状态模块不同，不能在SLS文件中作为状态调用。

<br>

**Renderers:**
渲染器使用以各种语言、模板引擎、文件编写的状态配置文件。Salt的配置管理系统与语言无关。

- Full list of renderers: <https://docs.saltstack.com/en/latest/ref/renderers/all/index.html#all-salt-renderers>
- Renderers: <https://docs.saltstack.com/en/latest/ref/renderers/index.html#renderers>


<br/>
<br/>


## Salt States

HOW DO I USE SALT STATES?

> 注意:
> 这仅仅是使用State的开始，请确保阅读了Pillar


**Salt State** 系统的核心是 **SLS**(SaLt State File)。SLS表示系统应处于的状态，并设置为以简单格式包含此数据。这通常称为配置管理(Configuration Management)。


<br/>


### 一切都是数据

IT IS ALL JUST DATA


在深入研究细节之前，有助于理解SLS文件只是一个数据结构。虽然理解SLS只是一种数据结构对于理解和利用Salt States并不重要，但它应该有助于增强对实际力量所在位置的了解。

因此，SLS文件实际上只是 `dictionaries, lists, strings, numbers` 。通过使用这种方法，Salt可以更加灵活。当一个人写了更多的状态文件时，它就会更清楚地写出正在编写的内容。随着管理员或开发人员的需求而增长。结果便是一个易于理解的系统。


<br/>


### THE TOP FILE


可以使用名为 `top.sls` 的文件将以下部分中的示例SLS文件分配给主机。


<br/>


#### 介绍

大多数基础架构都是由一组机器组成，组中的每台机器都扮演着与其它机器相似的角色。这些机器组彼此协同工作以创建应用程序堆栈。
为了有效地管理这些机器组，管理员需要能够为这些组创建角色。

在Salt中，包含网络上的机器组之间的映射以及应该应用于它们的配置角色的文件 称为 **顶级文件** (`top file`)。

顶级文件(`top file`) 默认名为 `top.sls` ，之所以如此命名，是因为它们始终存在于包含状态文件的目录层次结构的顶部。该目录结构被称为 **状态树**(`state tree`)。


<br/>
<br/>


#### 栗子

顶级文件有三个部件:

- **Environment**: 状态树目录，包含一组用于配置系统的状态文件
- **Target**: 一组机器，它们将应用一组状态
- **State files**: 应用于目标的状态文件列表。每个状态文件都描述了要在目标机器上配置和强制执行的一个或多个状态

三个组件之间的关系:

- Environments contain targets
- Targets contain states

```yaml
base:          # Apply SLS files from the directory root for the 'base' environment
  'web*':      # All minions with a minion_id that begins with 'web'
    - apache   # Apply the state file named 'apache.sls'
```


<br/>
<br/>


#### ENVIRONMENTS

环境是包含顶级文件和一组状态文件的目录层次结构。
环境可以以多种方式使用，但是并不要求它们被使用。事实上，部署Salt的最常见方式是使用一个名为 `base` 的环境。如果用户具有专门调用多个版本状态树的用例时，才建议用户仅创建多个环境。


<br/>
<br/>


#### 入门

GETTING STARTED WITH TOP FILES

每个环境都在名为 `file_roots` 的 salt master config 变量中定义。

```yaml
#在常见的单一环境中，只定义了一个base环境，并且只有一个目录路径用于状态文件
file_roots:
  base:
    - /srv/salt
```

```yaml
#对于base环境，所有minions都会将名为core.sls和edit.sls的状态文件应用于它们
#core.sls, edit.sls
base:
  '*':
    - core
    - edit
```


<br/>
<br/>


#### 多环境

MULTIPLE ENVIRONMENTS

在某些情况下，团队可能希望创建版本化状态树，这些树可用于在将状态部署到生产环境之前在隔离的系统集中测试Salt配置。

```yaml
#每个环境一个目录
file_roots:
  dev:
    - /srv/salt/dev
  qa:
    - /srv/salt/qa
  prod:
    - /srv/salt/prod
```

顶级文件引用这些环境:

```yaml
dev:
  'webserver*':
    - webserver
  'db*':
    - db
qa:
  'webserver*':
    - webserver
  'db*':
    - db
prod:
  'webserver*':
    - webserver
  'db*':
    - db
```


<br/>
<br/>


#### 给目标选择一个环境

CHOOSING AN ENVIRONMENT TO TARGET

顶级文件用于将minion分配给环境，除非使用下面描述的方法覆盖。顶级文件中的环境必须与有效的文件服务器环境匹配，以便将任何状态应用于minion。当使用默认文件服务器后端时，环境在 `file_roots` 中定义。

可以使用 `state.show_top` 函数查看将在给定环境中应用于minion的状态。

通过在 minion config 中设置环境值(`environment`)，可以将Minions固定到特定环境。这样做时，minion将仅从其分配的环境中请求文件。

> **Note:**
> Changed in version `2018.3.0`: Renamed from `environment` to `saltenv`.
> If `environment` is used, `saltenv` will take its value. If both are used, `environment` will be ignored and `saltenv` will be used.

也可在运行命令(`salt, salt-ssh, salt-call...`)时动态传递环境值，但也并不是所有函数都接收此参数。例如: `salt '*' state.highstate saltenv=prod`


<br/>
<br/>


#### 高级的minion目标

ADVANCED MINION TARGETING

顶层文件中为目标表达式设置的可用匹配类型:

| Type | Description |
| - | - |
| glob | Full minion ID or glob expression to match multiple minions (e.g. `minion123` or `minion*`) |
| pcre | Perl-compatible regular expression (PCRE) matching a minion ID (e.g. `web[0-3].domain.com`) |
| grain | Match a grain, optionally using globbing (e.g. `kernel:Linux` or `kernel:*BSD`) |
| grain_pcre | Match a grain using PCRE (e.g. `kernel:(Free|Open)BSD`) |
| list | Comma-separated list of minions (e.g. `minion1,minion2,minion3`) |
| pillar | Pillar match, optionally using globbing (e.g. `role:webserver` or `role:web*`) |
| pillar_pcre | Pillar match using PCRE (e.g. `role:web(server|proxy)` |
| pillar_exact | Pillar match with no globbing or PCRE (e.g. `role:webserver`) |
| ipcidr | Subnet or IP address (e.g. `172.17.0.0/16` or `10.2.9.80`) |
| data | Match values kept in the minion's datastore (created using the data execution module) |
| range | Range cluster |
| compound | Complex expression combining multiple match types |
| nodegroup | Pre-defined compound expressions in the master config file |


<br/>
<br/>


#### 如何编译顶级文件

HOW TOP FILES ARE COMPILED

当执行 `highstate` 并指定环境时，那么该环境的顶级文件是用于将状态分配给minions的唯一顶级文件，只有来自此环境的状态才会被运行。

如果没有指定环境，minion将在每个环境中查找顶级文件，并且将处理每个顶级文件以确定要在minions上运行的SLS文件。默认情况下，每个环境中的顶级文件将被合并在一起。
在具有多个环境的配置中，可能会导致意外结果。所以，你可能需要修改策略使得每个环境使用其自己的顶级文件，另一种选择是将 `state_top_saltenv` 设置为特定环境。

```yaml
#修改合并策略
top_file_merging_strategy: same


#修改环境
state_top_saltenv: base
```


<br/>
<br/>


#### 顶级文件编译栗子

TOP FILE COMPILATION EXAMPLES

```yaml
#/etc/salt/master
file_roots:
  base:
    - /srv/salt/base
  dev:
    - /srv/salt/dev
  qa:
    - /srv/salt/qa
```

```yaml
#/srv/salt/base/top.sls
base:
  '*':
    - base1
dev:
  '*':
    - dev1
qa:
  '*':
    - qa1
```

```yaml
#/srv/salt/dev/top.sls
base:
  'minion1':
    - base2
dev:
  'minion2':
    - dev2
qa:
  '*':
    - qa1
    - qa2
```


<br/>
<br/>
<br/>


### YAML格式的SLS

DEFAULT DATA - YAML


默认情况下，Salt使用YAML格式来代表SLS文件。

```
# 典型的YAML格式的SLS文件
# 不同Linux发行版的包管理方式不一样
# 此SLS文件将确保安装名为apache的程序包，并确保apache服务正在运行

apache:
  pkg.installed: []
  service.running:
    - require:
      - pkg: apache

# 第一行是一组数据的ID，称为ID声明。此ID设置需要操作的事物的名称。
# 第二行和第三行包含要运行的状态模块函数，格式为: <state_module>.<function>
# require为必要的声明(Requisite Statement)，确保在安装Apache包之后启动它
```



<br/>
<br/>



### 添加配置和用户

ADDING CONFIGS AND USERS


在设置Apache之类的服务时，可能需要添加更多组件。很可能会管理Apache配置文件，并且可能需要设置用户和组。

```yaml
# 使用必须的声明: watch，观察状态
# require确保安装Apache后创建组，创建组之后创建用户

apache:
  pkg.installed: []
  service.running:
    - watch:
      - pkg: apache
      - file: /etc/httpd/conf/httpd.conf
      - user: apache
  user.present:
    - uid: 87
    - gid: 87
    - home: /var/www/html
    - shell: /bin/nologin
    - require:
      - group: apache
  group.present:
    - gid: 87
    - require:
      - pkg: apache

/etc/httpd/conf/httpd.conf:
  file.managed:
    - source: salt://apache/httpd.conf
    - user: root
    - group: root
    - mode: 644
```


<br/>
<br/>


### MOVING BEYOND A SINGLE SLS


以可伸缩的方式设置Salt States时，需要使用多个SLS。

上面的示例位于单个SLS文件中，但可以组合两个或多个SLS文件来构建状态树。上面的SLS文件还引用了一个奇怪的source(`source: salt://apache/httpd.conf`)，此文件也需要提供。
SLS文件布局在Salt master的目录结构中，SLS是一个文件，要下载的文件也是一个文件。

**不要再SLS名字中使用点(`.`)**

```
#salt file server
apache/init.sls
apache/httpd.conf
```

但是，当使用多个单个SLS文件时，可以将更多组件添加到工具箱中。
`include`语句包含其它的SLS文件，以便在其中找到的组件可以被 `required, watched, demonstrated - extended`。`include` 语句允许状态交叉链接。当SLS具有 `include` 语句时，它实际上扩展为包括所包含的SLS文件的内容。

考虑如下SSH示例:

```yaml
# ssh/init.sls
openssh-client:
  pkg.installed

/etc/ssh/ssh_config:
  file.managed:
    - user: root
    - group: root
    - mode: 644
    - source: salt://ssh/ssh_config
    - require:
      - pkg: openssh-client
```

```yaml
#ssh/server.sls:
include:
  - ssh

openssh-server:
  pkg.installed

sshd:
  service.running:
    - require:
      - pkg: openssh-client
      - pkg: openssh-server
      - file: /etc/ssh/banner
      - file: /etc/ssh/sshd_config

/etc/ssh/sshd_config:
  file.managed:
    - user: root
    - group: root
    - mode: 644
    - source: salt://ssh/sshd_config
    - require:
      - pkg: openssh-server

/etc/ssh/banner:
  file:
    - managed
    - user: root
    - group: root
    - mode: 644
    - source: salt://ssh/banner
    - require:
      - pkg: openssh-server
```

状态树看起来可能是下面这个样子:

```
apache/init.sls
apache/httpd.conf
ssh/init.sls
ssh/server.sls
ssh/banner
ssh/ssh_config
ssh/sshd_config
```


<br/>
<br/>


### EXTENDING

EXTENDING INCLUDED SLS DATA


有时，需要扩展(`extend`) SLS。也许apache服务需要观察(`watch`)额外的资源，或者在某些情况下需要放置不同的文件。
`extend` 语句不是必须的声明，它是附加的。

考虑如下栗子:

```yaml
#ssh/custom-server.sls:
#使用extend语句覆盖下载的banner文件，而使用新的文件

include:
  - ssh.server

extend:
  /etc/ssh/banner:
    file:
      - source: salt://ssh/custom-banner
```

```yaml
#python/mod_python.sls:

include:
  - apache

extend:
  apache:
    service:
      - watch:
        - pkg: mod_python

mod_python:
  pkg.installed
```


<br/>
<br/>


### 渲染系统

UNDERSTANDING THE RENDER SYSTEM


由于SLS数据只是简单的数据，因此不需要用YAML表示。 Salt默认为YAML，因为它非常简单易学，易于使用。但是，只要提供了渲染器模块，就可以从几乎任何可以想象的介质渲染SLS文件。

默认渲染系统是 `yaml_jinja` 渲染器，此渲染器首先通过Jinja2模板系统传递模板，然后通过YAML解析器。这样做的好处是在创建SLS文件时可以使用完整的编程结构。
其它可用的渲染器有 `yaml_mako` 和 `yaml_wempy`，纯Python的`pydsl`, `pyobjects` 渲染器。


<br/>
<br/>


### DEBUG SALT STATES

RUNNING AND DEBUGGING SALT STATES


一旦SLS中的规则准备就绪，就应对其进行测试以确保它们正常工作。使用`salt '*' state.apply`命令调用这些规则。

```
#minion
#这有助于解决问题

salt-call state.apply -l debug

salt-minion -l debug
```



<br/>
<br/>
<br/>



## STATES TUTORIAL


本节的目的是演示如何快速配置由Salt States管理的系统。


<br/>


### 基本用法

BASIC USAGE


本节将引导您使用Salt配置minion以运行 Apache HTTP Server 并确保服务器正在运行。

我的Salt Minions有两个跑在Docker里:

- centos
- ubuntu

`apache` 在 `ubuntu` 中包名为 `apache2`， 在 `centos` 下包名为 `httpd`。下面的SLS文件中，可能我并没有修改，但我在测试的时候是修改了的，请大家注意。


<br/>


#### 配置状态树

SETTING UP THE SALT STATE TREE

States 是存储在 Master 的文本文件中，并通过 Master FileServer 按需发送给 Minions。状态文件的集合构成了 **状态树(state tree)** 。

要在Salt中开始使用中央状态系统，必须首先设置Salt 文件服务器。

```yaml
#编辑 master config，并取消 file_roots 的注释
file_roots:
  base:
    - /srv/salt
```

```bash
#修改之后重启master
systemctl restart salt-master
```


<br/>
<br/>


#### 准备顶级文件

PREPARING THE TOP FILE

在Master上的`file_roots`目录(`/srv/salt`)下创建顶级文件(`top.sls`)。

```yaml
#/srv/top.sls
base:
  '*':
    - webserver

#env: base
#target: '*'
#state file: webserver.sls
```


<br/>
<br/>


#### 创建sls文件

CREATE AN SLS FILE

在顶级文件相同的目录下，创建sls文件。

```yaml
#/srv/salt/webserver.sls
#ubuntu下叫apache2, Fedora下叫httpd
apache2:                 # ID declaration，定义要安装的包名称
  pkg:                  # state declaration，定义要使用的Salt State
    - installed         # function declaration，声明要调用的函数
```


<br/>
<br/>


#### 安装软件包

INSTALL THE PACKAGE

在Master上执行命令，完成之后，Minion将报告所有采取的行动和所做的所有更改的摘要。

```bash
#salt '*' state.apply
salt 'ubuntu' state.apply

# salt 'ubuntu' state.apply -l debug
# salt 'ubuntu' state.apply -t 60

#之后便可看到状态
------------
Succeeded: 1 (changed=1)
Failed:    0
------------
```

<br>

**SLS File Namespace**

注意上面的栗子，SLS文件 `webserver.sls` 简称为 `webserver`。在 `top.sls` 或 `include` 声明中引用时，SLS文件的命名空间遵循一些简单的规则:

- `.sls` 被丢弃了(如 `webserver.sls` 变为 `webserver`)
- 子目录可用于更好的组织
  - 每个子目录在Salt状态和命令行中用点(`.`, 遵循 python import 模型）表示。如文件系统中的 `webserver/dev.sls` 在Salt中称为 `webserver.dev`
  - 所以，不要在状态文件名中使用点(`.`)，否则便无法匹配
- 子目录中名为 `init.sls` 的文件指由目录路径引用。如 `webserver/init.sls` 指的是 `webserver`
- 如果 `webserver.sls` 和 `webserver/init.sls` 都存在，那么 `webserver/init.sls` 会被忽略，`webserver.sls` 将指的是 `webserver`


<br/>
<br/>


#### ubuntu和centos两台minion

前面我只用了Ubuntu，现在我加一台Centos，现在便有两台Minion。则现在就需要修改状态文件。

```yaml
#/srv/salt/top.sls
base:
  'ubuntu':
    - webserver-ubuntu
  'centos':
    - webserver-centos


#/srv/salt/webserver-ubuntu.sls
apache2:                 # ID declaration
  pkg:                  # state declaration
    - installed         # function declaration


#/srv/salt/webserver-centos.sls
httpd:                 # ID declaration
  pkg:                  # state declaration
    - installed         # function declaration
```

```base
#执行
# salt 'centos' state.apply
# salt 'ubuntu' state.apply
# salt '*' state.apply -l debug
salt '*' state.apply
```



<br/>
<br/>



### 复杂的状态和要求

MORE COMPLEX STATES, REQUISITES


前面介绍了安装软件包的基础知识。现在将修改我们的 `webserver.sls` 文件以满足要求，并使用更多的Salt State。


<br/>


#### 调用多个状态

CALL MULTIPLE STATES

你可以在ID声明下指定多个状态声明。

```yaml
#webserver.sls
#注意Ubuntu下和Centos下的包名不同，请记得修改
#如果apache没有运行，则启动它
apache:
  pkg.installed: []
  service.running:
    - require:
      - pkg: apache

#尝试在执行state.apply之前停止apache
```


<br/>
<br/>


#### 要求其它状态

REQUIRE OTHER STATES

现在Apache已经跑起来了，我们需要添加一些HTML文件来定制网站。但我们不希望在Salt安装和运行Apache之前添加HTML文件。

```yaml
#/srv/salt/webserver/init.sls
#注意，根据前面的规则，webserver.sls还存在的话会忽略 webserver/init.sls，所以需要注释它，之后才会生效
apache:
  pkg.installed: []
  service.running:
    - require:
      - pkg: apache

/var/www/index.html:                        # ID declaration，此栗中为自定义的HTML文件位置
  file:                                     # state declaration
    - managed                               # function
    - source: salt://webserver/index.html   # function arg
    - require:                              # requisite declaration
      - pkg: apache                         # requisite reference
```

`/srv/salt/webserver/index.html` 文件内容:

```html
<!DOCTYPE html>
<html>
    <head><title>Salt rocks</title></head>
    <body>
        <h1>This file brought to you by Salt</h1>
    </body>
</html>
```


```bash
#执行
salt '*' state.apply

------------
Succeeded: 3 (changed=1)
Failed:    0
------------
Total states


#之后可在minion上进行验证，我测试是没有问题
#只不过apache的Web目录为 /var/www/html
```

<br>

**`require` vs `watch`**

有两个非必要的声明:

- `require`
- `watch`

并非每个state都支持 `watch` ，`service state` 支持 `watch` 并且将根据监控的情况重启服务。

```yaml
/etc/httpd/extra/httpd-vhosts.conf:
  file.managed:
    - source: salt://webserver/httpd-vhosts.conf

apache:
  pkg.installed: []
  service.running:
    - watch:
      - file: /etc/httpd/extra/httpd-vhosts.conf
    - require:
      - pkg: apache
```



<br/>
<br/>



### 模板、包含、扩展

TEMPLATING, INCLUDES, EXTENDS


本节将介绍sls文件的更高级模板和配置技术。


<br/>


#### SLS模块模板

TEMPLATING SLS MODULES

SLS模块可能需要编程逻辑或内联执行，这通过模块模板实现。默认使用的模块模板系统是Jinja2，可修改 master config 里的 `renderer` 来更改它。

所有状态在初始化读取时都通过模板系统，要使用模板系统，只需添加一些模板标记即可。

带模板标记的sls模块的栗子如下:

```jinja
{% for usr in ['moe','larry','curly'] %}
{{ usr }}:
  user.present
{% endfor %}
```

这个模板化的sls文件一旦生成将如下所示:

```yaml
moe:
  user.present
larry:
  user.present
curly:
  user.present
```


<br/>
<br/>


#### 在SLS模块中使用GRAINS

USING GRAINS IN SLS MODULES

通常，需要对不同的系统应用不同的状态。Salt Grains 在模板上下文中可用。Grains可在SLS模板中使用:

```jinja
apache:
  pkg.installed:
    {% if grains['os'] == 'RedHat' %}
    - name: httpd
    {% elif grains['os'] == 'Ubuntu' %}
    - name: apache2
    {% endif %}
```


<br/>
<br/>


#### 在SLS模板中使用环境变量

USING ENVIRONMENT VARIABLES IN SLS MODULES

你可使用 `salt['environ.get']('VARNAME')` 在Salt State 中使用环境变量。

```bash
MYENVVAR="world" salt-call state.template test.sls
```

```jinja
Create a file with contents from an environment variable:
  file.managed:
    - name: /tmp/hello
    - contents: {{ salt['environ.get']('MYENVVAR') }}
```

错误检查:

```jinja
{% set myenvvar = salt['environ.get']('MYENVVAR') %}
{% if myenvvar %}

Create a file with contents from an environment variable:
  file.managed:
    - name: /tmp/hello
    - contents: {{ salt['environ.get']('MYENVVAR') }}

{% else %}

Fail - no environment passed in:
  test.fail_without_changes

{% endif %}
```


<br/>
<br/>


#### 从模板调用模块

CALLING SALT MODULES FROM TEMPLATES

由minion加载的所有Salt模块都可在模板系统中使用。这允许在目标系统上实时收集数据。它还允许从sls模块中轻松运行shell命令。

Salt模块函数也可以在模板上下文中以salt形式提供，考虑如下栗子:

```jinja
moe:
  user.present:
    - gid: {{ salt['file.group_to_gid']('some_group_that_exists') }}
```

```python
import salt.modules.file
file.group_to_gid('some_group_that_exists')


salt['network.hw_addr']('eth0')
```


<br/>
<br/>


#### SLS模块高级语法

ADVANCED SLS MODULE SYNTAX

最后，我们将介绍一些非常有用的技术，用于更复杂的状态树。


```yaml
# INCLUDE DECLARATION
# 使用 include 声明跨越多个文件

#python/python-libs.sls:
python-dateutil:
  pkg.installed


#python/django.sls:
include:
  - python.python-libs

django:
  pkg.installed:
    - require:
      - pkg: python-dateutil



# EXTEND DECLARATION
# 您可以使用 extend 声明修改以前的声明
# extend 与 require, watch 的工作方式不同，它附加，而不是替换必须的组件

#apache/apache.sls:
apache:
  pkg.installed


#apache/mywebsite.sls:
include:
  - apache.apache

extend:
  apache:
    service:
      - running
      - watch:
        - file: /etc/httpd/extra/httpd-vhosts.conf

/etc/httpd/extra/httpd-vhosts.conf:
  file.managed:
    - source: salt://apache/httpd-vhosts.conf



# NAME DECLARATION
# 您可以使用Name声明覆盖ID声明

#apache/mywebsite.sls:
include:
  - apache.apache

extend:
  apache:
    service:
      - running
      - watch:
        - file: mywebsite

mywebsite:
  file.managed:
    - name: /etc/httpd/extra/httpd-vhosts.conf
    - source: salt://apache/httpd-vhosts.conf



# NAMES DECLARATION
# 更强大的是使用Names声明一次覆盖多个状态的ID声明。这通常可以消除模板中循环的需要

stooges:
  user.present:
    - names:
      - moe
      - larry
      - curly
```



<br/>
<br/>



### 状态工作流


本节将展示如何使用salt的 `file_roots` 来设置工作流。在该工作流中，状态可以从 dev, 到 QA, 到 Prod 。


<br/>


#### Salt文件服务器路径继承

SALT FILESERVER PATH INHERITANCE

> 注意:
> 使用多个文件服务器后端时，它们在 master config 中的 `fileserver_backend` 参数中列出的顺序也很重要

Salt FileServer 允许每个环境有多个根目录。Salt FileServer 将根目录列表折叠到包含每个根的所有文件的单个虚拟环境中。
如果同一文件存在于多个根目录中的相同相对路径中，则最顶级匹配成功。

```yaml
# In the master config file (/etc/salt/master)
file_roots:
  base:
    - /srv/salt
    - /mnt/salt-nfs/base
```

```
#如以下两者都存在
/srv/salt/foo.txt
/mnt/salt-nfs/base/foo.txt

#则 salt://foo.txt 将指向 /srv/salt/foo.txt
```


<br/>
<br/>


#### 环境配置

ENVIRONMENT CONFIGURATION

配置多个环境:

```yaml
file_roots:
  base:
    - /srv/salt/prod
  qa:
    - /srv/salt/qa
    - /srv/salt/prod
  dev:
    - /srv/salt/dev
    - /srv/salt/qa
    - /srv/salt/prod
```


<br/>
<br/>


#### 栗子

```yaml
#/srv/salt/prod/top.sls:

base:
  'web*prod*':
    - webserver.foobarcom
qa:
  'web*qa*':
    - webserver.foobarcom
dev:
  'web*dev*':
    - webserver.foobarcom
```



<br/>
<br/>



### 状态系统参考

STATE SYSTEM REFERENCE

url: <https://docs.saltstack.com/en/latest/ref/states/index.html>


Salt提供了一个接口来管理Salt minions的配置或状态，此接口是一个功能完备的机制，用于从中央管理器强制执行系统状态。




<br/>
<br/>

---

<br/>
<br/>




# 实用程序模块

UTILITY MODULES - CODE REUSE IN CUSTOM MODULES


当通过编写自定义(state module, execution module) 来扩展Salt时，有时需要一种功能不仅仅适用于一种自定义模块。对于这些情况，Salt支持所谓的 **实用程序模块(Utility Module)**。这些模块与普通执行模块类似，但不是使用 `__salt__` 在Salt代码中调用，而是使用 `__utils__` 前缀。

<br>

例如，假设有以下简单实用程序模块 `salt://_utils/foo.py`

```py
# -*- coding: utf-8 -*-
'''
My utils module
---------------

This module contains common functions for use in my other custom types.
'''

def bar():
    return 'baz'
```

一旦同步到一个minion，这个函数将可用于其他自定义Salt类型，如下所示:

```py
# -*- coding: utf-8 -*-
'''
My awesome execution module
---------------------------
'''

def observe_the_awesomeness():
    '''
    Prints information from my utility module

    CLI Example:

    .. code-block:: bash

        salt '*' mymodule.observe_the_awesomeness
    '''
    return __utils__['foo.bar']()
```

与任何其它类型的Salt扩展一样，实用程序模块支持使用 `__virtual__` 函数有条件地加载它们，或者在不同的命名空间下加载它们。例如，在 `salt://_utils/` 下有一个实用程序模块。

```py
# -*- coding: utf-8 -*-
'''
My utils module
---------------

This module contains common functions for use in my other custom types.
'''

def __virtual__():
    '''
    Load as a different name
    '''
    return 'foo'

def bar():
    return 'baz'
```

您甚至可以以面向对象的方式编写实用程序模块:

```python
# -*- coding: utf-8 -*-
'''
My OOP-style utils module
-------------------------

This module contains common functions for use in my other custom types.
'''

class Foo(object):

    def __init__(self):
        pass

    def bar(self):
        return 'baz'
```

将它们导入其它自定义模块:

```python
# -*- coding: utf-8 -*-
'''
My awesome execution module
---------------------------
'''

import mymodule

def observe_the_awesomeness():
    '''
    Prints information from my utility module

    CLI Example:

    .. code-block:: bash

        salt '*' mymodule.observe_the_awesomeness
    '''
    foo = mymodule.Foo()
    return foo.bar()
```

<br>

当然，这些都是人为的例子，但它们应该用来展示通过编写实用程序模块开辟的一些可能性。请记住，虽然状态仍然可以访问所有执行模块，因此没有必要编写实用程序模块来使函数可用于状态和执行模块。实用程序模块的一个很好的用例是需要从自定义 输出器/返回器(outputter/returner) 以及执行模块调用相同功能的用例。

放置在 `salt://_utils/` 中的实用程序模块将在运行 HighStage 时以及在调用以下任何Salt函数时同步到minions:

- `saltutil.sync_utils`
- `saltutil.sync_all`




<br/>
<br/>

---

<br/>
<br/>




# 事件和反应器

EVENTS and REACTOR


<br/>


## 事件系统

Event System


**Salt Event System(事件系统)** 用于触发事件，使第三方应用程序或外部进程能够对Salt内的行为做出反应。事件系统使用 发布-订阅模式(publish-subscribe)。



<br/>
<br/>



### 事件总线

EVENT BUS


事件系统由两个主要组件组成，它们构成了事件总线的概念:

- **Event Socket**, 用于发布事件
- **Event Library**, 可以监听事件并将事件发送到salt系统

事件发布到事件总线上，事件总线订阅者监听已发布的事件。
事件总线用于进程间通信(IPC)以及Salt中的网络传输。通过 UNIX Domain Sockets 提供进程间通信。

Salt Master 和 每个Salt Minion 都有自己的 Event Bus。



<br/>
<br/>



### 事件类型

EVENT TYPES


<br/>


#### SALT MASTER EVENTS

这些事件是在Salt Master Event Bus 上发生的。

Salt Master Events:

- Authentication events
- Start events
- Key events
- Job events
- Runner Events
- Presence Events
- Cloud Events

<br>

```
#AUTHENTICATION EVENTS

#salt/auth
#Fired when a minion performs an authentication check with the master.
Variables:
  id -- The minion ID.
  act -- The current status of the minion key: accept, pend, reject.
  pub -- The minion public key.



#START EVENTS
#salt/minion/<MID>/start
#Fired every time a minion connects to the Salt master.

Variables:
  id -- The minion ID.



#KEY EVENTS

#salt/key
#Fired when accepting and rejecting minions keys on the Salt master. These happen as a result of actions undertaken by the salt-key command.

Variables:
  id -- The minion ID.
  act -- The new status of the minion key: accept, delete,




# JOB EVENTS

# salt/job/<JID>/new
# Fired as a new job is sent out to minions.

Variables:
  jid -- The job ID.
  tgt -- The target of the job: *, a minion ID, G@os_family:RedHat, etc.
  tgt_type -- The type of targeting used: glob, grain, compound, etc.
  fun -- The function to run on minions: test.ping, network.interfaces, etc.
  arg -- A list of arguments to pass to the function that will be called.
  minions -- A list of minion IDs that Salt expects will return data for this job.
  user -- The name of the user that ran the command as defined in Salt's Publisher ACL or external auth.


# salt/job/<JID>/ret/<MID>
# Fired each time a minion returns data for a job.

Variables:
  id -- The minion ID.
  jid -- The job ID.
  retcode -- The return code for the job.
  fun -- The function the minion ran. E.g., test.ping.
  return -- The data returned from the execution module.


# salt/job/<JID>/prog/<MID>/<RUN NUM>
# Fired each time a each function in a state run completes execution. Must be enabled using the state_events option.

Variables:
  data -- The data returned from the state module function.
  id -- The minion ID.
  jid -- The job ID.




# RUNNER EVENTS

# salt/run/<JID>/new
# Fired as a runner begins execution

Variables:
  jid -- The job ID.
  fun -- The name of the runner function, with runner. prepended to it (e.g. runner.jobs.lookup_jid)
  fun_args -- The arguments passed to the runner function (e.g. ['20160829225914848058'])
  user -- The user who executed the runner (e.g. root)


# salt/run/<JID>/ret
# Fired when a runner function returns

Variables:
  jid -- The job ID.
  fun -- The name of the runner function, with runner. prepended to it (e.g. runner.jobs.lookup_jid)
  fun_args -- The arguments passed to the runner function (e.g. ['20160829225914848058'])
  return -- The data returned by the runner function


# salt/run/<JID>/args
# New in version 2016.11.0.
# Fired by the state.orchestrate runner

Variables:
  name -- The ID declaration for the orchestration job (i.e. the line above salt.state, salt.function, salt.runner, etc.)
  type -- The type of orchestration job being run (e.g. state)
  tgt -- The target expression (e.g. *). Included for state and function types only.
  args -- The args passed to the orchestration job. Note: for state and function types, also includes a tgt_type value which shows what kind of match (glob, pcre, etc.) was used. This value was named expr_form in the 2016.11 release cycle but has been renamed to tgt_type in 2017.7.0 for consistency with other events.



# PRESENCE EVENTS

# salt/presence/present
# Events fired on a regular interval about currently connected, newly connected, or recently disconnected minions. Requires the presence_events setting to be enabled.

Variables:
  present -- A list of minions that are currently connected to the Salt master.


# salt/presence/change
# Fired when the Presence system detects new minions connect or disconnect.

Variables:
  new -- A list of minions that have connected since the last presence event.
  lost -- A list of minions that have disconnected since the last presence event.



# CLOUD EVENTS

```



<br/>
<br/>




###  监听事件

LISTENING FOR EVENTS


Salt事件系统在Salt中大量使用，它也被编写为与现有工具和脚本大量集成。
有多种方法可以消费它:

- CLI
- REST API
- Python

```
# FROM THE CLI
# 查看事件总线的最快方法是调用 state.event runner
# 此Runner旨在通过外部工具和shell脚本与事件总线进行交互
salt-run state.event pretty=True



# REMOTELY VIA THE REST API
# Salt事件总线可使用 salt.netapi.rest_cherrypy.app.Events 作为来自外部工具或服务的 HTTP stream
curl -SsNk https://salt-api.example.com:8000/events?token=05A3



# FROM PYTHON
# Python脚本只能作为运行Salt的同一系统用户访问事件总线
# 要监听事件，需要创建SaltEvent对象，然后需要运行 get_event 函数。它的默认轮询时间分配为5秒，可使用 wait 选项更改
# SaltEvent对象需要知道 Salt Unix Socket 的保存位置，它在配置文件的 sock_dir 选项中配置，默认为 /var/run/salt/master

# 如下代码将检查单一事件
import salt.config
import salt.utils.event

opts = salt.config.client_config('/etc/salt/master')

event = salt.utils.event.get_event(
        'master',
        sock_dir=opts['sock_dir'],
        transport=opts['transport'],
        opts=opts)

data = event.get_event()


# 事件也将使用标签(tag)，标签允许通过前缀过滤事件。默认情况下，将返回所有事件。如果只需要认证标签，则只需要传递 salt/auth 标签。
# 栗子
data = event.get_event(wait=10, tag='salt/auth')


# 检索标记以及事件数据，请传递 full=True
evdata = event.get_event(wait=10, tag='salt/job', full=True)
tag, data = evdata['tag'], evdata['data']


# 事件标签是可以全局的，如可在Reactor中使用fnmatch库
import fnmatch

import salt.config
import salt.utils.event

opts = salt.config.client_config('/etc/salt/master')

sevent = salt.utils.event.get_event(
        'master',
        sock_dir=opts['sock_dir'],
        transport=opts['transport'],
        opts=opts)

while True:
    ret = sevent.get_event(full=True)
    if ret is None:
        continue

    if fnmatch.fnmatch(ret['tag'], 'salt/job/*/ret/*'):
        do_something_with_job_return(ret['data'])
```



<br/>
<br/>



### 引发事件

FIRING EVENTS


可以在Minion的本地总线上引发事件或引发用于Master的事件。

```
# 要在命令行上从Minion引发本地事件，请调用 event.fire 执行函数
salt-call event.fire '{"data": "message to be sent in the event"}' 'tag'


# 要从Minion发送一个要发送给Master的事件，请调用 event.send 执行函数。记住YAML可以在函数参数的CLI中使用
salt-call event.send 'myco/mytag/success' '{success: True, message: "It works!"}'


# 如果一个进程正在监听Minion，那么Master上的用户可以向它发出一个事件
# Job on minion
import salt.utils.event

event = salt.utils.event.MinionEvent(**__opts__)

for evdata in event.iter_events(tag='customtag/'):
    return evdata # do your processing here...

# Bash
salt minionname event.fire '{"data": "message for the minion"}' 'customtag/african/unladen'
```



<br/>
<br/>



### 从Python引发事件

FIRING EVENTS FROM PYTHON


```python
# FROM SALT EXECUTION MODULES
# 在编写执行模块时，事件非常有用，以便在发生特定任务时通知Master上的各个进程

# /srv/salt/_modules/my_custom_module.py
def do_something():
    '''
    Do something and fire an event to the master when finished

    CLI Example::

        salt '*' my_custom_module:do_something
    '''
    # do something!
    __salt__['event.send']('myco/my_custom_module/finished', {
        'finished': True,
        'message': "The something is finished!",
    })



# FROM CUSTOM PYTHON SCRIPTS
# 从自定义Python代码中触发事件

import salt.client

caller = salt.client.Caller()

caller.sminion.functions['event.send'](
    'myco/myevent/success',
    {
        'success': True,
        'message': "It works!",
    }
)
```




<br/>
<br/>
<br/>




## Beacons

> 注意:
> Salt beacons 是一种事件生成机制。当信标事件(beacons events)发生时，信标利用Salt Reactor System 进行更改。


**Beacons** 允许您使用 Salt event system 来监控 non-Salt 进程。Beacon system 允许minion挂钩到各种系统进程并持续监视这些进程。当在系统进程中发生受监视的活动时，将在  Salt event bus 上发送可用于触发反应器(reactor)的事件。

Salt beacons 当前可以监控和发送Salt事件，用于许多系统活动，包括:

- 文件系统更改(file system changes)
- 系统负载(system load)
- 服务状态(service status)
- shell activity, such as user login
- 网络和磁盘使用情况(network and disk usage)



<br/>
<br/>



### BEACON MODULES


BEACON MODULES LISTS: <https://docs.saltstack.com/en/latest/ref/beacons/all/index.html#all-salt-beacons>



<br/>
<br/>



### BEACONS 配置


Salt Beacons 不需要对正在监视的系统组件进行任何更改，所有内容都使用Salt进行配置。

Beacons 通常在Minio config 中的 `beacons:` 段进行配置，或在 `/etc/salt/minion.d/` 下的任意文件，如 `/etc/salt/minion.d/beacons.conf`。

```yaml
# inotify信标仅适用于具有inotify内核支持的操作系统
beacons:
  inotify:
    - files:
        /etc/important_file: {}
        /opt: {}
```

与Salt中的许多其它系统一样，信标系统也可以通过 Minion Pillar，Grains 或 本地配置文件进行配置。


<br/>


#### Beacons监控间隔

BEACON MONITORING INTERVAL

默认情况下，信标以1秒的间隔监控。要设置不同的间隔，请为信标提供 `interval` 参数。

```yaml
beacons:
  inotify:
    - files:
        /etc/important_file: {}
        /opt: {}
    - interval: 5
    - disable_during_state_run: True
  load:
    - averages:
        1m:
          - 0.0
          - 2.0
        5m:
          - 0.0
          - 1.5
        15m:
          - 0.1
          - 1.0
    - interval: 10
```


<br/>


#### 避免事件循环

AVOIDING EVENT LOOPS

仔细考虑在 Reactor 和 Beacons 之间创建循环的可能性非常重要。例如，可以设置一个信标，该信标监视是否读取文件，该文件又触发反应器以运行状态，该状态又反过来读取文件并重新激活信标。
为了避免这些类型的场景，可以设置 `disable_during_state_run` 参数。

```yaml
beacons:
  inotify:
    - files:
        /etc/important_file: {}
    - disable_during_state_run: True
```



<br/>
<br/>



### 栗子

BEACON EXAMPLE


栗子配置 `inotify` 信标以监视文件以进行更改，然后在进行更改时将文件还原为其原始内容。

`inotify` 信标需要在 `minion` 上使用 `Pyinotify`，使用 `salt myminion pkg.install python-inotify` 进行安装。


```
# 安装Pyinotify
salt 'centos' pkg.install python-inotify



# CREATE WATCHED FILE
echo 'important_config: True' > /etc/salt/important_file



# ADD BEACON CONFIGS TO MINION
# Salt Minion /etc/salt/minion.d/beacons.conf
beacons:
  inotify:
    - files:
        /etc/salt/important_file:
          mask:
            - modify
    - disable_during_state_run: True


# 保存配置文件，并重新启动Minion服务



# VIEW EVENTS ON THE MASTER
# Salt Master
salt-run state.event pretty=true
# 此Runner显示Master在Salt事件总线上接收的事件

# 要测试您在上一节中设置的信标，请对 /etc/salt/important_file 进行修改并保存
# Salt Minion
echo 'name: zhang' >> /etc/salt/important_file

# 此时Master上的Runner会显示修改信息
salt/beacon/centos/inotify//etc/salt/important_file     {
    "_stamp": "2019-02-11T06:36:06.282180",
    "change": "IN_MODIFY",
    "id": "centos"
    "path": "/etc/salt/important_file"
}
```

现在，您可以创建一个反应器(Reactor)，以便在发生此事件时采取措施。

```
# CREATE A REACTOR
# 每次修改时，此反应器都会监视的文件恢复为salt提供的内容

mkdir -p /srv/reactor

# /srv/reactor/revert.sls
revert-file:
  local.state.apply:
    - tgt: {{ data['data']['id'] }}
    - arg:
      - maintain_important_file



# STATE SLS
# /srv/salt/maintain_important_file.sls
important_file:
  file.managed:
    - name: /etc/important_file
    - contents: |
        important_config: True



# MASTER CONFIG
# 配置master以将inotify信标事件映射到 /etc/salt/master.d/reactor.conf 中的还原反应
reactor:
  - salt/beacon/*/inotify//etc/important_file:
    - /srv/reactor/revert.sls



# START THE SALT MASTER IN DEBUG MODE
# 启用调试后，你可以在日志中看到Reactor和Event相关信息
service salt-master stop
salt-master -l debug



# TRIGGER THE REACTOR
```



<br/>
<br/>



### 编写Beacon插件

WRITING BEACON PLUGINS


Beacon Plugin 使用标准的 Salt Loader System，这意味着来自其它插件系统的许多构造都是正确的，例如 `__virtual__` 函数。
Beacon Plugin 中的重要的函数是 `beacon` 函数。当beacon配置为运行时，该功能将由minion重复执行。因此，`beacon` 功能不能阻止，并且应尽可能轻。信标还必须返回一个dicts列表，列表中的每个dict将被转换为master上的事件。

<br/>

**THE BEACON FUNCTION**

信标系统将在模块中寻找名为 `beacon` 的函数。如果此函数不存在，则不会触发信标。定期调用此函数，默认情况下在minion的每次迭代时调用，每秒迭代次数可达数十至数百次。这意味着信标功能不能阻止，不应该是 CPU/IO 密集型。

信标功能将在执行的信标的配置中传递。这使得为每个被调用的信标建立灵活配置变得容易。这也是摄取信标配置的首选方式，因为它允许在 Minion 运行时通过配置 Minion Pillar 中的信标来动态更新配置。

<br/>

**THE BEACON RETURN**

从信标返回的信息预计遵循预定义的结构。返回的值必须是字典列表（首选标准python字典，不需要有序的字典）。

这些字典代表了对Minion和Master 事件总线的个别事件。每个字典都是一个单一的事件。 dict可以包含任意键，但是tag键将被提取并添加到被触发事件的标签中。

<br/>

**CALLING EXECUTION MODULES**

执行模块仍然是Salt中所有工作和系统交互的首选位置。因此，`__salt__` 变量在信标内可用。
在 `__salt__` 中调用函数时要小心，虽然这是在Salt中执行复杂例程的首选方法，但并不是所有的执行模块都是用信标编写的。注意可能是CPU密集或IO限制的执行模块。请随意添加新的执行模块和功能以支持特定的信标。



<br/>
<br/>
<br/>



## 反应器系统

Reactor System


Salt's Reactor system 使Salt能够触发操作以响应事件。这是一个简单的接口，用于监视Salt的事件总线，以查找与给定模式匹配的事件标记，然后运行一个或多个命令作为响应。

该系统将sls文件绑定到Master上的事件标记。这些sls文件然后定义反应(reactions)。这意味着反应器系统有两个部分。首先，需要在主配置文件中设置reactor选项。 reactor选项允许事件标记与sls反应文件相关联。其次，这些反应文件使用highdata（如状态系统）来定义要执行的反应。


<br/>


### 事件系统

EVENT SYSTEM


理解反应器需要对事件系统有基本的了解。事件系统是一个本地ZeroMQ PUB接口，用于引发Salt事件。此事件总线是一个开放系统，用于发送通知Salt和其它系统有关操作的信息。

事件系统使用非常特定的标准引发事件。每个事件都有一个tag。事件标记允许对事件进行快速顶级过滤。除了标记之外，每个事件都有一个数据结构。此数据结构是一个字典，其中包含有关该事件的信息。



<br/>
<br/>



### 将事件映射到反应器SLS文件

MAPPING EVENTS TO REACTOR SLS FILES


Reactor SLS Files 和 Event Tag 在 Master config 中关联。默认为` /etc/salt/master`，也可以是`/etc/salt/master.d/reactor.conf`。

例如:

```yaml
reactor:                            # Master config section "reactor"

  - 'salt/minion/*/start':          # Match tag "salt/minion/*/start"
    - /srv/reactor/start.sls        # Things to do when a minion starts
    - /srv/reactor/monitor.sls      # Other things to do

  - 'salt/cloud/*/destroyed':       # Globs can be used to match tags
    - /srv/reactor/destroy/*.sls    # Globs can be used to match file names

  - 'myco/custom/event/tag':        # React to custom event tags
    - salt://reactor/mycustom.sls   # Reactor files can come from the salt fileserver
```

Reactor SLS文件类似于State和Pillar SLS文件。它们默认为 `YAML + Jinja` 模板，并传递熟悉的上下文变量。



<br/>
<br/>



### 反应类型

TYPES OF REACTIONS

> 注意:
> The local and caller reaction types will likely be renamed in a future release.


| Name | Description |
| - | - |
| local | Runs a remote-execution function on targeted minions |
| runner | Executes a runner function |
| wheel | Executes a wheel function on the master |
| caller | Runs a remote-execution function on a masterless minion |



<br/>
<br/>



### 哪里放置反应器SLS文件

WHERE TO PUT REACTOR SLS FILES


Reactor SLS文件既可以来自master的本地文件，也可以来自通过 `fileserver_backend` 配置项启用的任何后端。可以使用 `salt://` URL引用放置在Salt文件服务器中的文件，就像它们可以在状态SLS文件中一样。

建议将reactor和orchestrator SLS文件放在它们自己唯一命名的子目录中。如 `orch/`, `orchestrate/`, `react/`, `reactor/`...



<br/>
<br/>



### 编写反应器SLS

WRITING REACTOR SLS


不同的反应类型是分开开发的，历史上有不同的传递参数的方法。对于 `2017.7.2 release`，引入了一种新的统一配置模式，该模式适用于所有反应类型。

将继续支持旧的配置架构，并且目前没有计划弃用它。


<br/>


**LOCAL REACTIONS**

旧的配置架构要求用户在 `arg` 和 `kwarg` 参数下手动分隔位置和关键字参数。
但是，这不是很用户友好，因为它强制用户区分哪种类型的参数，并确保正确排序位置参数。因此，如果主服务器正在运行受支持的版本，则建议使用新的配置模式。

以下两个例子等效:
```yaml
# Supported in 2017.7.2 and later
install_zsh:
  local.state.single:
    - tgt: 'kernel:Linux'
    - tgt_type: grain
    - args:
      - fun: pkg.installed
      - name: zsh
      - fromrepo: updates



# Supported in all releases
install_zsh:
  local.state.single:
    - tgt: 'kernel:Linux'
    - tgt_type: grain
    - arg:
      - pkg.installed
      - zsh
    - kwarg:
        fromrepo: updates
```

这个反应等同于运行以下Salt命令:

```bash
salt -G 'kernel:Linux' state.single pkg.installed name=zsh fromrepo=updates
```

<br/>

**RUNNER REACTIONS**

新旧两个例子:

```yaml
# Supported in 2017.7.2 and later
deploy_app:
  runner.state.orchestrate:
    - args:
      - mods: orchestrate.deploy_app
      - pillar:
          event_tag: {{ tag }}
          event_data: {{ data['data']|json }}


# Supported in all releases
deploy_app:
  runner.state.orchestrate:
    - mods: orchestrate.deploy_app
    - kwarg:
        pillar:
          event_tag: {{ tag }}
          event_data: {{ data['data']|json }}
```

假设事件标记是foo，并且传递给事件的数据是 `{'bar'：'baz'}` ，那么这个反应等同于运行以下Salt命令:

```bash
salt-run state.orchestrate mods=orchestrate.deploy_app pillar='{"event_tag": "foo", "event_data": {"bar": "baz"}}'
```

<br/>

**WHEEL REACTIONS**

新旧两种栗子:

```yaml
# Supported in 2017.7.2 and later
remove_key:
  wheel.key.delete:
    - args:
      - match: {{ data['id'] }}



# Supported in all releases
remove_key:
  wheel.key.delete:
    - match: {{ data['id'] }}
```

<br/>

**CALLER REACTIONS**

> 注意:
> Masterless Minions use this Reactor

新旧两种栗子:

```yaml
# Supported in 2017.7.2 and later
touch_file:
  caller.file.touch:
    - args:
      - name: /tmp/foo



# Supported in all releases
touch_file:
  caller.file.touch:
    - args:
      - /tmp/foo
```

此反应等同于运行以下Salt命令:

```bash
salt-call file.touch name=/tmp/foo
```



<br/>
<br/>



### 编写反应器SLS的最佳实践

BEST PRACTICES FOR WRITING REACTOR SLS FILES


Reactor的工作原理如下:

1. The Salt Reactor watches Salt's event bus for new events.
2. Each event's tag is matched against the list of event tags configured under the reactor section in the Salt Master config.
3. The SLS files for any matches are rendered into a data structure that represents one or more function calls.
4. That data structure is given to a pool of worker threads for execution.



<br/>
<br/>



### BEACONS AND REACTORS

An event initiated by a beacon, when it arrives at the master will be wrapped inside a second event, such that the data object containing the beacon information will be data['data'], rather than data.



<br/>
<br/>



### 手动引发事件

MANUALLY FIRING AN EVENT


```bash
# FROM THE MASTER
# Use the event.send runner
salt-run event.send foo '{orchestrate: refresh}'



# FROM THE MINION
# To fire an event to the master from a minion, call event.send
salt-call event.send foo '{orchestrate: refresh}'

# To fire an event to the minion's local event bus, call event.fire:
salt-call event.fire '{orchestrate: refresh}' foo



# REFERENCING DATA PASSED IN EVENTS
# any reactor SLS files triggered by watching the event tag foo will execute with {{ data['data']['orchestrate'] }} equal to 'refresh'.
```



<br/>
<br/>



### 获取事件信息

GETTING INFORMATION ABOUT EVENTS


确切地查看已触发的事件以及每个事件中可用的数据的最佳方法是使用 `state.event runner`。

```bash
salt-run state.event pretty=True

#一有变动，就会触发你所定义的动作。就会显示变动信息。
```




<br/>
<br/>

---

<br/>
<br/>




# 编排

ORCHESTRATION


<br/>


## ORCHESTRATE RUNNER

当你想确保minion按照你想要的方式配置和运行时，对minion执行State或Highstate是完美的。但有时你想同时配置一组minions。

例如，如果要在Web服务器集群前设置负载均衡器，可以确保首先设置负载均衡器，然后在整个集群中一致地应用相同的匹配配置。
**编排(Orchestration)** 是实现此目的的方法。


<br/>


### THE ORCHESTRATE RUNNER


> 注意:
> Orchestrate弃用了OverState
> 在 Salt `2015.8.0`, Orchestrate Runner 取代了 OverState system

Orchestrate Runner(最初称为 state.sls runner) 提供OverState的所有功能，但有以下优点:

- 可以使用State中可用的所有requisites 和其它全局参数
- states/functions 也适用于 salt-ssh minions

<br>

Orchestrate Runner 将 Salt State system 概括为 Salt Master Context。而 `state.sls`, `state.highstate` 在每个Salt Minion上同时独立执行，`state.orchestrate` runner 在Master上执行。给它一个 `master-level` 视图并控制必要条件，如状态排序和条件。这允许了Minion的必要条件，例如在不同的Minion上排序这些应用的状态，这些状态不能够同时发生，或者如果Minion失去其中一个状态，则停止所有Minion的状态。

`state.sls`, `state.highstate` 允许你有状态地管理每个Minion，`state.orchestrate` runner 允许你有状态的管理整个基础结构。


<br/>
<br/>


#### 编写SLS文件


Orchestrate SLS 文件与 State SLS 文件存储在相同的位置。这意味着 `file_roots` 和 `gitfs_remotes` 都会影响 Reactor 和 Orchestrator 可用的SLS文件。

建议将 reactor和orchestrator SLS文件保存在各自唯一命名的子目录中，如 `_orch/`, `orch/`, `_orchestrate/`, `react/`, `_reactor` ...这样可以避免重复命名，有助于防止混淆。


<br/>
<br/>


#### 执行编排运行器

EXECUTING THE ORCHESTRATE RUNNER


Orchestrate Runner命令格式与 `state.sls` 函数相同，除了因为它是一个Runner，它是用 `salt-run` 而不是 `salt` 来执行。
假设你有一个名为 `/srv/salt/orch/webserver.sls` 的 `state.sls` 文件，则在Master上运行以下命令将该应用该文件中定义的状态。

```bash
# state.orch 是 state.orchestrate 的同义词
salt-run state.orchestrate orch.webserver
```


<br/>
<br/>


#### 无主编排

MASTERLESS ORCHESTRATION

> 注意:
> 无主模式编排仅支持SLS文件中的 `salt.state` 命令，它当前不支持 `salt.function` 命令。


要在 MasterLess Minions 上支持 Salt Orchetration, Orchestrate Runner 可用作执行模块。无主编排的语法完全相同，但是它使用 `salt-call` 命令，并且Minion Config 必须包含 `file_mode: local` 选项。(或者，在命令行使用 `salt-call --local`)

```bash
salt-call --local state.orchestrate orch.webserver
```


<br/>
<br/>


#### 栗子

**FUNCTION**

要执行函数，使用 `salt.function`:

```yaml
# /srv/salt/orch/cleanfoo.sls
cmd.run:
  salt.function:
    - tgt: '*'
    - arg:
      - rm -rf /tmp/foo
```

```bash
salt-run state.orchestrate orch.cleanfoo
```

如果省略了 `name` 参数，则状态的ID将是默认名称，或者在 `salt.function` 的情况下，执行模块函数运行。你可以指定 `name` 参数以避免ID冲突:

```yaml
copy_some_file:
  salt.function:
    - name: file.copy
    - tgt: '*'
    - arg:
      - /path/to/file
      - /tmp/copy_of_file
    - kwarg:
        remove_existing: true
```

<br>

**FAIL FUNCTIONS**

> 重要:
> Fail Function 运行在Master上，因此必须使用 `salt-run saltutil.sync_modules` 同步。

在编排中运行远程执行功能时，这些功能的某些返回值可能表示失败，而函数本身并未设置返回码。对于这些情况，使用 `FAIL FUNCTION` 允许更灵活的评估成功或失败的方法。

失败函数可以编写为自定义执行模块的一部分。该函数接受一个参数，并返回一个布尔结果。例如:

```py
def check_func_result(retval):
    if some_condition:
        return True
    else:
        return False
```

可以在编排SLS中引用此函数，如下所示:

```yaml
do_stuff:
  salt.function:
    - name: modname.funcname
    - tgt: '*'
    - fail_function: mymod.check_func_result
```

<br/>

**STATE**

要执行一个状态，使用 `salt.state`.

```yaml
# /srv/salt/orch/webserver.sls
install_nginx:
  salt.state:
    - tgt: 'web*'
    - sls:
      - nginx
```

```bash
salt-run state.orchestrate orch.webserver
```

<br>

**HIGHSTATE**

要运行highstate，在状态配置中设置 `highstate: True`。

```yaml
# /srv/salt/orch/web_setup.sls
webserver_setup:
  salt.state:
    - tgt: 'web*'
    - highstate: True
```

```bash
salt-run state.orchestrate orch.web_setup
```

<br>

**RUNNER**

要使用其它RUNNER，使用`salt.runner`

```yaml
# /srv/salt/orch/deploy.sls
create_instance:
  salt.runner:
    - name: cloud.profile
    - prof: cloud-centos
    - provider: cloud
    - instances:
      - server1
    - opts:
        minion:
          master: master1
```

使用Pillar数据执行:

```bash
salt-run state.orch orch.deploy pillar='{"servers": "newsystem1", "master": "mymaster"}'
```

<br>

**RETURN CODES IN RUNNER/WHEEL JOBS**

状态作业(`salt.state`)能够通过状态返回字典(state return dictionary)上报失败。运城执行(`salt.function`)作业能够通过在`__context__`字典中设置`retcode` 键 来上报失败。然而，Runner(`salt.runner`)和 Wheel(`salt.wheel`)作业才会上报 `False` 结果。
从 `2018.3.0` 版本开始，现在可以像在远程执行功能中一样在 Runner 和 Wheel 功能中设置 Retcode。

```python
# 自定义 Runner/Wheel 函数上报其失败，以便准确地告知作业失败
def myrunner():
    ...
    do stuff
    ...
    if some_error_condition:
        __context__['retcode'] = 1
    return result
```

<br>

**MORE COMPLEX ORCHESTRATION**

可以在单个文件中配置许多 *状态/函数* ，当与完整的Requisites 和其它全局状态参数组合时，可以使用它们轻松配置复杂的编排任务。此外，状态/函数将按照定义它们的顺序执行，除非你做了配置。

```yaml
bootstrap_servers:
  salt.function:
    - name: cmd.run
    - tgt: 10.0.0.0/24
    - tgt_type: ipcidr
    - arg:
      - bootstrap

storage_setup:
  salt.state:
    - tgt: 'role:storage'
    - tgt_type: grain
    - sls: ceph
    - require:
      - salt: webserver_setup

webserver_setup:
  salt.state:
    - tgt: 'web*'
    - highstate: True
```

鉴于上述配置，编排将按照如下方式执行:

1. `bootstrap`将在 cidr 中的所有minion上执行
2. 由于 `storage_setup` state require，因此将在ID以`web`开头的minion上运行Highstate
3. 最后，ceph SLS 目标将在匹配 Grains的minion上执行


<br/>
<br/>


### 程序化解析结果

PARSING RESULTS PROGRAMMATICALLY


编排作业返回一个特定数据结构的输出。根据使用的输出器，该数据结构的表示方式不同。使用编排的默认输出器，你将获得一个很好的人类可读输出。

假设以下 Orchestration SLS:

```yaml
good_state:
  salt.state:
    - tgt: myminion
    - sls:
    - succeed_with_changes

bad_state:
  salt.state:
    - tgt: myminion
    - sls:
    - fail_with_changes

mymod.myfunc:
  salt.function:
    - tgt: myminion

mymod.myfunc_false_result:
  salt.function:
    - tgt: myminion
```

默认输出器输出结果太多，这里给出链接: <https://docs.saltstack.com/en/latest/topics/orchestrate/orchestrate_runner.html#parsing-results-programmatically>


<br/>
<br/>


#### 在没有Minion的Master上运行STATE

RUNNING STATES ON THE MASTER WITHOUT A MINION


Orchestrate runner可用于在不使用minion的情况下在master上执行状态。

假设有 `salt://foo.sls` SLS文件:

```yaml
/etc/foo.conf:
  file.managed:
    - source: salt://files/foo.conf
    - mode: 0600
```

在这种情况下，运行 `salt-run state.orchestrate foo` 将等同于运行 `state.sls foo`，但它只在Master上执行，并且不需要在Master上运行Minion个的守护进程。

这不是技术上的编排，但它在某些用例中很有用。

<br>

**LIMITATIONS**

使用此方法一次只能运行一个SLS目标，而使用 `state.sls` 允许在逗号分隔列表中传递多个SLS文件。



<br/>
<br/>

---

<br/>
<br/>




# SALT PROXY MINION


**Proxy Minion** 是一种开发中的Salt功能，可以控制无论出于何种原因无法运行标准 `salt-minion` 的设备。例如，具有API但运行专有操作系统的网络设备，具有有限的CPU或MEM的设备，可以运行Minion的设备，但出于安全原因，并不会运行。

proxy minion 不是一个 **开箱即用**(out of the box) 的功能。由于存爱无限数量的可控设备，因此你很可能必须自己编写接口(interface)。幸运的是，这与代理设备的实际接口一样困难。设备具有现有Python模块(如: `PyUSB`)的接口相对简单。用于控制具有 HTML REST接口的代码设备应该很容易。

Salt Proxy-Minion 提供了 ，允许设备 **枚举**(enumeration) 和 **发现**(discovery)， **控制**(control)， **状态**(status)， **远程执行**(remote execution) 和 **状态管理**(state management) 的 **管道**(plumbing)。


<br/>


## 入门


下图可能有助于理解包含 Proxy-Minions 的 Salt 结构:

![](/images/Salt/proxy_minions.png)













<br/>
<br/>

---

<br/>
<br/>














# 网络自动化

NETWORK AUTOMATION


**Network Automation** 是自动化计算机网络的 配置，管理，操作的连续过程。虽然抽象(abstraction)可以与服务器端的操作进行比较，但是存在许多特殊的挑战，最重要的是网络设备传统上是封闭的硬件，只能运行专有软件。换句话说，用户无法直接在传统网络设备上安装 `salt-minion` 包。由于这些原因，大多数网络设备只能通 Proxy-Minion 或使用 Salt-SSH 进行远程控制。但也有一些供应商生产了专门的设备。




















<br/>
<br/>

---

<br/>
<br/>
















# 命令参考

COMMAND LINE REFERENCE


<br/>


## SALT-CALL


> 注意:
> `salt-call` 命令从当前用户的shell上下文执行，而salt命令从系统的默认上下文执行。


`salt-call` 命令用于在 Minion 上本地运行模块函数(module functions)，而不是从 Master 执行它们。
除非指定了 `--local` 选项，否则将联系Salt Master以在执行期间检索状态文件和其他资源。

<br>

```bash
# salt-call - salt-call Documentation



#语法
salt-call [options]
```




<br/>
<br/>
<br/>




## SALT


Salt允许命令在一系列远程系统上并行执行。这意味着可以轻松地控制和查询远程系统。

<br>

```bash
#语法
salt [options] '<target>' <function> [arguments]



#栗子
salt '*' [ options ] sys.doc

salt -E '.*' [ options ] sys.doc cmd

salt -G 'os:Arch.*' [ options ] test.ping

salt -C 'G@os:Arch.* and webserv* or G@kernel:FreeBSD' [ options ] test.ping
```




<br/>
<br/>
<br/>




## SALT-CLOUD


使用Salt在云中配置虚拟机。

<br>

```bash
# salt-cloud - Salt Cloud Command
# Provision virtual machines in the cloud with Salt



# 语法
salt-cloud [options] <-m MAP | -p PROFILE> <NAME> [NAME2 ...]
```




<br/>
<br/>
<br/>




## SALT-CP


复制文件到一个/多个 Minions。


<br>

```bash
# salt-cp - salt-cp Documentation
# Copy a file or files to one or more minions



# 语法
salt-cp [options] '<target>' SOURCE DEST



# 栗子
salt-cp '*' [ options ] SOURCE [SOURCE2 SOURCE3 ...] DEST

salt-cp -E '.*' [ options ] SOURCE [SOURCE2 SOURCE3 ...] DEST

salt-cp -G 'os:Arch.*' [ options ] SOURCE [SOURCE2 SOURCE3 ...] DEST
```




<br/>
<br/>
<br/>




## SALT-EXTEND


用于生成Salt源代码扩展的实用程序。这用于:

- 添加新的执行模块，状态模块
- 添加单元测试到现有模块
- 添加集成测试到现有模块




<br/>
<br/>
<br/>




## SALT-KEY


Salt-key执行用于身份验证的 salt server public keys 的简单管理。

在初始连接时，Salt Minion 将其公钥发送给 Salt Master。必须使用 Salt Master 上的 `salt-key` 命令接受此密钥。

Salt Minion Key 可处于以下状态:

- **unaccepted**: key正在等待被接受
- **accepted**: key已被接受。Minion可与Master通信
- **rejected**: 使用 `salt-key` 命令拒绝了key。在这种状态下，Minion 不接受 Master 的任何通信
- **denied**: key被Master自动拒绝。当minion有重复的ID，或minion被重建或生成了新的密钥并且之前的密钥没有从Master中删除时，会发生这种情况。在这种状态下，Minion 不接受 Master 的任何通信

出现上述情况，删除对应的key，之后在接受就行了。

<br>

```bash
# salt-key - salt-key Documentation



# 语法
salt-key [ options ]



#常用选项
-a ACCEPT, --accept=ACCEPT
-A, --accept-all    Accept all pending keys

-r REJECT, --reject=REJECT
-R, --reject-all    Reject all pending keys

-f FINGER, --finger=FINGER
-F, --finger-all
```




<br/>
<br/>
<br/>




## SALT-MASTER


Salt master守护进程，用于控制Salt minions。

<br>

```bash
# salt-master - salt-master Documentation
# The Salt master daemon, used to control the Salt minions



# 语法
salt-master [ options ]
```




<br/>
<br/>
<br/>




## SALT-MINION

Salt minion 守护程序，从远程Salt master接收命令。
Salt minion 接收来自 Salt Master 的命令并回复所述命令的结果。

<br>

```bash
# salt-minion - salt-minion Documentation
# The Salt minion daemon, receives commands from a remote Salt master.



# 用法
salt-minion [options]
```



<br/>
<br/>
<br/>




## SALT-PROXY


从 Salt Master 接收命令，并将这些命令代理到无法运行完整 Minion 的设备。
Salt Proxy Minion 从 Salt Master 接收命令，将适当的命令发送到无法运行 Minion 的设备，并回复所述命令的结果。

<br>

```bash
# salt-proxy - salt-proxy Documentation
# Receives commands from a Salt master and proxies these commands to devices that are unable to run a full minion.



# 语法
salt-proxy [ options ]
```




<br/>
<br/>
<br/>




## SALT-RUN


执行一个 Salt runner。
`salt-run` 是执行 Salt Runners 的前端命令。 Salt Runners 是用于在 Master 上执行便捷功能的简单模块。

<br>

```bash
# salt-run - salt-run Documentation
# Execute a Salt runner



# 语法
salt-run RUNNER
```



<br/>
<br/>
<br/>



## SALT-SSH


Salt SSH允许仅使用SSH进行传输来执行salt例程。
通过ssh执行salt命令和状态而不安装salt-minion。

<br>

```bash
# Execute salt commands and states over ssh without installing a salt-minion.


# 语法
salt-ssh [options] '<target>' <function> [arguments]



# 栗子

salt-ssh '*' [ options ] sys.doc

salt-ssh -E '.*' [ options ] sys.doc cmd
```


<br/>


### 入门

GETTING STARTED


Salt SSH使用简单，只需设置系统的基本 **Roster**(目标主机名册) 文件，以与标准的 `salt` 命令类似的方式连接并运行 `salt-ssh` 命令。

- 远程系统上需要Python（除非使用 `-r` 选项发送原始 `ssh` 命令）
- 在许多系统上，`salt-ssh` 可执行文件将位于自己的包中，通常名为 `salt-ssh`
- Salt SSH系统不会取代标准的Salt通信系统，它只提供基于SSH的替代方案，不需要ZeroMQ和远程Agent。(请注意，由于所有与Salt SSH的通信都是通过SSH执行的，因此它比使用ZeroMQ的标准Salt慢得多)
- 目前必须包装文件服务器操作以确保使用 `salt-ssh` 命令传递相关文件。However, needed fileserver wrappers are still under development



<br/>
<br/>



### ROSTER

SALT SSH ROSTER


Salt中的 Roster(名册) System 允许轻松定义远程minion。
它是Salt中添加的可插拔系统，以促进 `salt-ssh` 系统。名册系统的创建是因为 `salt-ssh` 需要一种方法来确定哪些系统需要作为执行目标。

由于名册系统是可插拔的，因此可以轻松扩充到任何现有系统，以收集有关当前可用的服务器信息，并且通过 `salt-ssh` 附件。默认情况下，名册文件位于 `/etc/salt/roster`。

标准的Salt系统中不需要或不使用名册系统，因为Master了解目标Minion。


<br/>


#### Roster模块


链接: <https://docs.saltstack.com/en/latest/ref/roster/all/index.html>


<br/>
<br/>


#### 如何工作

HOW ROSTERS WORK


名册系统编译内部称为 `targets`(目标) 的数据结构。目标是关于如何连接到所述系统的目标的系统和属性的列表。Salt 名册系统的唯一要求是返回 `targets` 数据结构。

<br>

**TARGETS DATA**

可以存储在 Roster target 中的信息:

```yaml
<Salt ID>:       # The id to reference the target system with
    host:        # The IP address or DNS name of the remote host
    user:        # The user to log in as
    passwd:      # The password to use for login, if omitted, keys are used

    # Optional parameters
    port:        # The target system's ssh port number
    sudo:        # Boolean to run command via sudo
    sudo_user:   # Str: Set this to execute Salt as a sudo user other than root.
                 # This user must be in the same system group as the remote user
                 # that is used to login and is specified above. Alternatively,
                 # the user must be a super-user.
    tty:         # Boolean: Set this option to True if sudo is also set to
                 # True and requiretty is also set on the target system
    priv:        # File path to ssh private key, defaults to salt-ssh.rsa
                 # The priv can also be set to agent-forwarding to not specify
                 # a key, but use ssh agent forwarding
    timeout:     # Number of seconds to wait for response when establishing
                 # an SSH connection
    minion_opts: # Dictionary of minion opts
    thin_dir:    # The target system's storage directory for Salt
                 # components. Defaults to /tmp/salt-<hash>.
    cmd_umask:   # umask to enforce for the salt-call command. Should be in
                 # octal (so for 0o077 in YAML you would do 0077, or 63)
```

<br>

**TARGET DEFAULTS**

master配置中的 `roster_defaults` 字典用于为名单中的minion上设置默认登录变量，以便不需要使用命令行参数传递相同的参数。

```yaml
roster_defaults:
  user: daniel
  sudo: True
  priv: /root/.ssh/id_rsa
  tty: True
```

<br>

**THIN_DIR**

Salt 需要将独立环境上载到目标系统，默认为 `/tmp/salt-<hash>`。每个正常的系统操作都将清理此目录。

如果你需要持久化的Salt环境，如设置持久化的grains，请记得修改此值。


<br/>
<br/>


#### 简单栗子

```yaml
# /etc/salt/roster
# sudo NOPASSWD /etc/sudoers: fred ALL=(ALL) NOPASSWD: ALL

docker-ubuntu:
  host: 172.17.0.5
  user: ubuntu
  passwd: ubuntu
  sudo: True
  thin_dir: /etc/salt/roster-thin
docker-centos:
  host:
  user:
  xxx
```



<br/>
<br/>



### 部署SSH KEY

DEPLOY SSH KEY FOR SALT-SSH


默认情况下，`salt-ssh` 将为SSH生成秘钥对，默认路径为 `/etc/salt/pki/master/ssh/salt-ssh.rsa`。密钥生成发生在你第一次运行 `salt-ssh` 时。

你可以使用 `ssh-copy-id` (OpenSSH密钥部署工具) 将密钥部署到Server。

```bash
#ssh-copy-id -i /etc/salt/pki/master/ssh/salt-ssh.rsa.pub user@server.demo.com

ssh-copy-id -i /etc/salt/pki/master/ssh/salt-ssh.rsa.pub ubuntu@172.17.0.5
```

你也可以写一个简单的脚本:

```bash
#!/bin/bash
if [ -z $1 ]; then
   echo $0 user@host.com
   exit 0
fi
ssh-copy-id -i /etc/salt/pki/master/ssh/salt-ssh.rsa.pub $1
```


<br/>
<br/>



### 调用SALT SSH

CALLING SALT SSH


`salt-ssh` 命令可以像 `salt` 命令一样轻松执行:

```bash
salt-ssh '*' test.ping
```

<br>

**RAW SHELL CALLS**

默认情况下，`salt-ssh` 在远程系统上运行 Salt 模块，但 `salt-ssh` 也可以执行 raw(原始) shell 命令:

```bash
salt-ssh '*' -r 'ifconfig'
```



<br/>
<br/>



### STATES VIA SALT SSH


Salt State System也可以与 `salt-ssh` 一起使用。状态系统在 `salt-ssh` 中向用户提取与使用标准 `salt` 时相同的接口。目的是两者(`salt`, `salt-ssh`)可以无缝配合。



<br/>
<br/>



### TARGETING WITH SALT SSH


由于 `salt-ssh` 中的定位方法不同，因此在撰写本文时仅支持 `glob` 和 `regex` 目标，其余的目标系统仍然需要实现。
默认情况下，可以通过 `salt-ssh`设置 Grains。默认情况下，这些 grains 不会在重新启动后持续存在。



<br/>
<br/>



### 配置SALT SSH

Salt SSH 在 Master Config 中配置。如果要使用自定义的配置文件，请使用 `-c` 选项传递。



<br/>
<br/>



### 非特权用户运行

RUNNING SALT SSH AS NON-ROOT USER


默认情况下，Salt 从 `/etc/salt/` 读取所有配置。如果使用 普通用户运行 Salt SSH，则可能报 `Permission denied` 错误，因此需要修改文件权限。

不建议对 Salt SSH 修改 `/etc/salt/` 的权限，而是复制它们作为副本，通过 `-c` 来指定。当然，如果你的SaltStack本就以普通用户运行，则可以直接使用这个用户。



<br/>
<br/>



### 定义CLI选项

DEFINE CLI OPTIONS WITH SALTFILE


如果您通常将CLI选项传递给 `salt-ssh`，则可以创建Saltfile以自动使用这些选项。
因此，你可在Saltfile目录中编写内容:

```yaml
salt-ssh:
  config_dir: path/to/config/dir
  ssh_max_procs: 30
  ssh_wipe: True
```


<br/>
<br/>
<br/>




## SALT-SYNDIC


Salt syndic 守护进程，一个通过高级Master的命令的特殊Minion。

<br>

```bash
# 语法
salt-syndic [ options ]
```



<br/>
<br/>
<br/>




## SALT-UNITY


围绕其它 Salt CLI 脚本的统一调用包装器。
此脚本接受一个参数，该参数是其它 Salt CLI 脚本之一并调用该脚本。

<br>

```bash
# 语法
salt-unity salt '*' test.ping
```



<br/>
<br/>
<br/>




## SALT-API


用于远程连接 Salt Master 的接口。
Salt API系统管理Salt Master的网络API连接器。

<br>

```bash
# salt-api - salt-api Command
# Start interfaces used to remotely connect to the salt master
# The Salt API system manages network API connectors for the Salt Master



# 语法
salt-api [options]
```




<br/>
<br/>
<br/>




## SPM


Salt Package Manager
spm是用于管理Salt包的前端命令。软件包通常只包含公式，表示安装在 Salt Master 上的 `file_roots` 中的一组SLS文件，但也可以安装Salt模块。

<br>

```bash
# spm - Salt Package Manager Command



# 语法
spm [options] <function> <argument>
```











<br/>
<br/>

---

<br/>
<br/>













# 模块参考

SALT MODULE REFERENCE


以下包含用于扩展Salt中各种子系统的Python Module 的列表。

- **auth modules**: <https://docs.saltstack.com/en/latest/ref/auth/all/index.html>
- **beacon modules**: <https://docs.saltstack.com/en/latest/ref/beacons/all/index.html>
- **Cache Modules**: <https://docs.saltstack.com/en/latest/ref/cache/all/index.html>
- **cloud modules**: <https://docs.saltstack.com/en/latest/ref/clouds/all/index.html>
- **engine modules**: <https://docs.saltstack.com/en/latest/ref/engines/all/index.html>
- **executors modules**: <https://docs.saltstack.com/en/latest/ref/executors/all/index.html>
- **fileserver modules**: <https://docs.saltstack.com/en/latest/ref/file_server/all/index.html>
- **grains modules**: <https://docs.saltstack.com/en/latest/ref/grains/all/index.html>
- **execution modules**: <https://docs.saltstack.com/en/latest/ref/modules/all/index.html>
- **netapi modules**: <https://docs.saltstack.com/en/latest/ref/netapi/all/index.html>
- **output modules**: <https://docs.saltstack.com/en/latest/ref/output/all/index.html>
- **pillar modules**: <https://docs.saltstack.com/en/latest/ref/pillar/all/index.html>
- **proxy modules**: <https://docs.saltstack.com/en/latest/ref/proxy/all/index.html>
- **queue modules**: <https://docs.saltstack.com/en/latest/ref/queues/all/index.html>
- **renderer modules**: <https://docs.saltstack.com/en/latest/ref/renderers/all/index.html>
- **returner modules**: <https://docs.saltstack.com/en/latest/ref/returners/all/index.html>
- **roster modules**: <https://docs.saltstack.com/en/latest/ref/roster/all/index.html>
- **runner modules**: <https://docs.saltstack.com/en/latest/ref/runners/all/index.html>
- **sdb modules**: <https://docs.saltstack.com/en/latest/ref/sdb/all/index.html>
- **serializer modules**: <https://docs.saltstack.com/en/latest/ref/serializers/all/index.html>
- **state modules**: <https://docs.saltstack.com/en/latest/ref/states/all/index.html>
- **thorium modules**: <https://docs.saltstack.com/en/latest/ref/thorium/all/index.html>
- **master tops modules**: <https://docs.saltstack.com/en/latest/ref/tops/all/index.html>
- **wheel modules**: <https://docs.saltstack.com/en/latest/ref/wheel/all/index.html>












<br/>
<br/>

---

<br/>
<br/>










# APIs


<br/>


## 客户端API

Python client API


docs: <https://docs.saltstack.com/en/latest/ref/clients/index.html>



<br/>
<br/>
<br/>



## netapi模块


docs: <https://docs.saltstack.com/en/latest/topics/netapi/index.html>








<br/>
<br/>

---

<br/>
<br/>







# 架构

ARCHITECTURE


如果你习惯于在安装任何内容之前规划一个细节的配置管理工具。使用Salt，你可以随时切换到高可用系统结构，并田间其它组件以随时扩展部署。

由于 Single Salt Master 可以管理成千上万个系统，因此我们通常建议您首先部署 Single Salt Master，然后根据需要修改部署以实现 冗余，地理分布和扩展。


<br/>
<br/>


## 高可用功能

HIGH AVAILABILITY FEATURES IN SALT


Salt支持多种功能，可实现高可用性和容错性。


<br/>


### MULTIMASTER


通过将 minion config 中的 master配置项配置为所有可用Master设备的YAML 列表，Salt Minion可以一次性连接到多个主设备。

在多主配置中，每个Master必须具有相同的加密秘钥(cryptographic keys)，并且必须分别在所有Master上接受 Minion Keys。`file_roots` 和 `pillar_roots` 的内容也需要与Salt外部的进程保持同步，这个可使用网络挂载。


<br/>
<br/>


### MULTIMASTER WITH FAILOVER


将 `master_type` 参数从 `str` 修改为 `failover`(故障转移) 将导致 Minion 连接到Master列表中的第一个响应的 Master Server。每个 `master_alive_interval` 的秒数都会检查 Minion 以确保当前的Master仍在响应。如果Master没有响应，则Minion将尝试连接到列表中的下一个Master。如果列表中的Master都用尽了，那么该列表将被回收，以防止之前响应故障的Master又恢复。请注意，`master_alive_interval` 必须存在于minion配置中，否则将无法调度检查主状态的定期作业。

`master_type: failover` 可与 `master_shuffle: True` 结合使用，以在所有Master上传播Minion连接。

将 Salt Syndic 添加到混合中可以创建负载均衡的Salt基础架构。如果一个Master失败，Minion会注意到并从可用列中表选择另一个Master。


<br/>
<br/>


### SYNDIC


**Salt Syndic** 功能是一种创建不同基础架构拓扑的方法。它不是严格意义上的HA功能，但可以这样看待它。

使用 Syndic， Salt基础架构可以以某种方式进行分区，使得主控制器控制基础结构的某些部分。


<br/>
<br/>


### SYNDIC WITH MULTIMASTER


**Syndic with Multimaster** 允许您连接一个 Syndic 到多个主，以便在syndic配置中提供额外的冗余层。



<br/>
<br/>
<br/>



## SALT SYNDIC


最基本的Salt拓扑结构由控制一组Minion节点的单个Master节点组成。使用称为Syndic的中间节点类型在构建Salt拓扑时提供了比仅由Master和Minion节点类型构造的拓扑更大的结构灵活性和可伸缩性。

一个 Syndic 节点可被认为是一个特殊的直通(passthrough)Minion节点。Syndic 节点是由 `salt-syndic` 守护进程和在同一系统上运行的 `salt-master` 守护进程组成。在 Syndic 节点上运行的 `salt-master` 用于控制一组较低级别的Minion节点，`salt-syndic` 连接较高级别的Master节点。有时称之为 **Master of Master**。

`salt-syndic` 守护进程在Master节点和本地 `salt-mastr` 守护进程之间中继(relay) 发布和事件。这使Master节点可以控制连接到Syndic节点上运行的 `salt-master` 守护程序的Minion节点。

这个功能我个人感觉有点像 多级Salt-Master-Minion。多级情况下，`salt-syndic`去控制下级Master，`salt-master`控制Minion。


<br/>


### 配置Syndic


要配置 Salt Syndic，您需要告诉 Syndic节点 及其 Master节点 关于它们彼此。
每个Syndic都必须提供自己的 `file_roots` 目录。文件不会自动从主节点传输。

假如您的Master几点位于`10.10.0.1`:

```yaml
# /etc/salt/master

# If this master will be running a salt syndic daemon, syndic_master tells
# this master where to receive commands from.
syndic_master: 10.10.0.1  # may be either an IP address or a hostname

# Set the order_masters setting to True if this master will command lower
# masters' syndic interfaces.
order_masters: True
```

```yaml
# /etc/salt/minion

# id is shared by the salt-syndic daemon and a possible salt-minion daemon
# on the Syndic node
id: my_syndic
```


<br/>
<br/>


### 多主Syndic配置

CONFIGURING THE SYNDIC WITH MULTIMASTER


使用 MultiMaster 的 Syndic 可以将 Syndic连接到多个Master，以便提供额外的冗余层。

首先应该在多主配置中配置更高级的Master。
在Syndic上， `syndic_master`选项提供了更高级别的Master的列表。

由于每个Syndic都连接到每个Master，因此从任何Master发送的作业都会转发给连接到每个Syndic的Minions。


<br/>
<br/>


### 运行


`salt-syndic` 守护进程是一个单独的进程，除了在Syndic节点上运行的 `salt-master` 守护进程之外，还需要启动它。Master节点在许多方面将Syndic视为普通的Minion节点。尤其是，Master需要去接受Syndic的Minion Key，就像其它Minion一样。

```bash
systemctl start salt-syndic

# 或

service salt-syndic start


# 接受key
salt-key -a my_syndic



# 测试
salt '*' test.ping
minion_1:
  True
minion_2:
  True
...
```

Master节点现在可以控制连接到Syndic的Minion节点。


<br/>
<br/>


### 拓扑

TOPOLOGY


- Master节点必须运行`salt-master`守护进程
- Syndic节点必须运行`salt-master`, `salt-syndic`守护进程
- Minion节点必须运行`salt-minion`守护进程
- `salt-master` 守护进程发出命令时，它将被直接连接到它的Syndic和Minion节点接收
  - Minion节点将以通常的方式处理命令
  - 在Syndic节点上，`salt-syndic`守护进程将该命令中继到`salt-master`守护进程，然后该节点将命令传播到与其连接的Minion和Syndic
- 当`salt-master`守护进程生成事件和作业返回数据时，它们由它们所连接的`salt-master`守护进程聚合，然后`salt-master`通过其`salt-syndic`守护进程将数据中继回来，直到数据到达Master节点或发出命令的Syndic节点


<br/>
<br/>


### SYNDIC WAIT


`syndic_wait` 是一个master config设置，它指定Salt Client 在放弃之前应等待其它Syndic检查其预期的minions列表的秒数。这个值默认为5s。

`syndic_wait` 是必要的，因为 Higher-Level Mater 没有办法知道 Syndic 下面的 Minions。更高级别的Master有自己的Minion列表，它们下面的Master也有自己的列表，所以Salt Client没有等待所有返回的时间。`syndic_wait` 选项允许所有Minion返回Salt Client的时间。


<br/>
<br/>


### Syndic配置项


除了`id`之外，所有配置项都在Syndic节点的Master config中:

- `id`: Syndic id (shared by the `salt-syndic` daemon with a potential `salt-minion` daemon on the same system)
- `syndic_master`: Master node IP address or hostname
- `syndic_master_port`: Master node ret_port
- `syndic_log_file`: path to the logfile (absolute or not)
- `syndic_pidfile`: path to the pidfile (absolute or not)
- `syndic_wait`: time in seconds to wait on returns from this syndic


<br/>
<br/>


### Minion数据缓存

MINION DATA CACHE

**Minion数据缓存** 包含Salt Master数据，Minion Grains 和 缓存在Master上的Minion Pillars。默认情况下，Salt使用`localfs`缓存模块，但可使用其它外部数据存储。



<br/>
<br/>
<br/>



## 有规模的使用salt

USING SALT AT SCALE


本节的重点是构建一个Salt基础架构来处理大量的Minions。

> 注意:
> 本章节适用于大型安装，虽然这些相同的设置并不会有伤害，但小型安装可能并不值得。
> 当与Minions一起使用时，术语**许多(many)**指的是至少一千个；术语**少数(a few)**总是指500个。
> 为简单起见，本章节将默认使用Salt使用的标准端口。


<br/>


### The Master


Salt Master最常见的问题是:

- too many minions authing at once
- too many minions re-authing at once
- too many minions re-connecting at once
- too many minions returning at once
- too few resources (CPU/DISK)

前三个问题是**雷鸣般的一群**(thundering herd)。为了缓解这些问题，我们必须配置在Master高负载时适当地退回Minion。
第四个问题是有拥有少量硬件资源的Master和ZeroMQ中可能存在的Bug引起的。

<br>

要理解每个问题，重要的是要了解Salt的工作原理。
简而言之，Salt Master为Minion提供两种服务:

- 作业发布者(job publisher)在端口4505上
- 开放端口4506接受Minion的返回

所有Minion始终在端口4505上连接到Publisher，并且如果需要，仅连接到打开的返回端口4506。在空闲的Master上，端口4505上只有连接。


<br/>


**TOO MANY MINIONS AUTHING**

当Minion服务首次启动时，它将通过端口4505连接到Master的Publisher。如果同时启动 too many minions，这可能会导致**thundering herd**。这可以通过不同时启动 too many minions 来避免。

连接本身通常不是罪魁祸首，主要问题的原因更可能是Master端关于使用Master对Minion进行认证。如果Master负载过重而无法处理身份认证请求，则将其计时。然后Minion将等待 `acceptance_wait_time` 时间进行重试。如果设置了 `acceptance_wait_time_max`， 那么Minion将在每次后续重试之前通过 `acceptance_wait_time` 增加其等待时间，知道达到 `acceptance_wait_time_max`。


<br/>


**TOO MANY MINIONS RE-AUTHING**

这很有可能发生在Salt部署的测试阶段，当所有Minion keys 都已被接受，但框架正在测试，并且参数在Salt Master Config 中经常更改。

Salt Master生成一个新的 AES key，用于某些事件加密其发布。如果遇到此问题，请删除(`salt-key -d`)对应 minion key 之后重新接受(`salt-key -a`)它。

当Master生成新的 AES key时，不会通知Minion，但会在它们收到下一个 发布作业(pub job) 中发现它。当Minion收到这样的作业后，它将与Master重新认证。由于Salt进行了Minion过滤(也就是未认证的不行)，这意味所有的Minion都会在Master下发布的下一个命令上重新认证——这就导致了另一个 thundering herd 。这可以通过以下设置来避免:

```yaml
# 在 minion config 中配置更高的值并错开重新认证尝试的数量
random_reauth_delay: 60
```


<br/>


**TOO MANY MINIONS RE-CONNECTING**

例如:

```bash
salt * disk.usage


# 使用salt batch模式可避免此问题
# 以下限制为50个minion
salt * disk.usage -b 50
```

它可能导致成千上万的Minion试图将它们的结果返回到Master的开放端口4506.如果Master无法立即处理那么多返回，也会导致大量的 syn flood。



<br/>
<br/>



### 资源太少

TOO FEW RESOURCES


Master资源总是应该与Master运行的环境相匹配。所以，随着Minion的不断增加，也要考虑Master的资源问题。


<br/>

**THE MASTER IS CPU BOUND**


<br>


**THE MASTER IS DISK IO BOUND**

- USE AND EXTERNAL JOB CACHE
- DISABLE THE JOB CACHE(非常不建议)



<br/>
<br/>
<br/>



## Multi Master


**多主(Multi Master)** 允许Salt Minion连接到冗余，并促进多个通信点的Minion。当使用多主设置时，所有Master都在热状态，并且任何活跃的Master都可用于向Minion发送命令。

如果你需要多主的故障转移(failover)，还可使用 MultiMaster-PKI配置。

Master间不共享信息，并且需要在多个Master接受keys。并且需要共享文件或使用git fileserver后端/网络文件系统来保持 `file_roots` 的一致性。
使用可插入的minion cache modules允许存储在Master上的关于Minion的数据被复制到Minion所连接的其它Master上。


<br/>


### 步骤概要


1. Create a redundant master server
2. Copy primary master key to redundant master
3. Start redundant master
4. Configure minions to connect to redundant master
5. Restart minions
6. Accept keys on redundant master


<br/>
<br/>


### 准备冗余Master

PREPPING A REDUNDANT MASTER


> 注意:
> 并没有冗余Master的逻辑上的限制

第一项任务是准备冗余Master。如果冗余Mater已在运行，请停止它。只要一个要求，即Master间共享相同的私钥。创建第一个Master时，会生成Master的key pair并放置在Master端的`pki_dir`目录下(默认为: `/etc/salt/pki/master/`)。请保持各个Master秘钥相同。

- `master.pem`，私钥
- `master.pub`，公钥


<br/>
<br/>


### 配置Minion


由于Minion需要掌握Master相关信息，因此需要将新的Master添加到Minion Config中。

```yaml
master:
  - saltmaster1.example.com
  - saltmaster2.example.com
```

之后，重启Minion服务。


<br/>
<br/>


### 在Master间共享文件

SHARING FILES BETWEEN MASTERS


Salt不会自动在Master之间共享文件。不建议各个Master之间存在差异，强烈建议共享这些文件，解决一致性问题。者可以使用网络文件系统或git fileserver后端。

- minion key
- file_roots
- pillar_roots
- master config



<br/>
<br/>
<br/>



## 具有故障转移的Multi-PKI-Master

MULTI-MASTER-PKI TUTORIAL WITH FAILOVER


> 请注意，建议先熟悉salt-authentication和communication以了解本节。此处描述的所有设置都在默认的身份认证/通信过程之上。


本节将解释如何运行salt-environment，当Minion当前Master失败时在它们之间进行故障转移。

各个步骤是:

- setup the master(s) to sign its auth-replies
- setup minion(s) to verify master-public-keys
- enable multiple masters on minion(s)
- enable master-check on minion(s)










<br/>
<br/>

---

<br/>
<br/>








# Minion数据缓存

MINION DATA CACHE


Minion数据缓存在Salt Master上包含了 Salt Mine data， Minion grains， Minion Pillar。默认情况下，Salt使用 `localfs` 缓存模块将数据保存在Salt Master上的`msgpack`文件中。


<br/>


## 缓存模块

CACHE MODULES


缓存模块如下:

| `consul` | Minion data cache plugin for Consul key/value data store. |
| - | - |
| `etcd_cache` | Minion data cache plugin for Etcd key/value data store. |
| `localfs` | Cache data in filesystem. |
| `mysql_cache` | Minion data cache plugin for MySQL database. |
| `redis_cache` | Redis |



<br/>
<br/>



## 可插拔数据缓存

PLUGGABLE DATA CACHE


虽然默认的Minion数据缓存是 `localfs` 缓存，但其它外部数据存储也可用于存储此数据，如 `consul` 模块。Salt Master 配置如下:

```yaml
cache: consul
```



<br/>
<br/>



## 配置Minion数据缓存

CONFIGURING THE MINION DATA CACHE


默认的 `localfs` Minion数据缓存不需要任何配置。具有外部数据缓存模块需要在Master上进行配置。

这是一个Consul的栗子:

```yaml
consul.host: 127.0.0.1
consul.port: 8500
consul.token: None
consul.scheme: http
consul.consistency: default
consul.dc: dc1
consul.verify: True

cache: consul
```











<br/>
<br/>

---

<br/>
<br/>














# Salt开发

DEVELOPING SALT

docs: <https://docs.saltstack.com/en/latest/topics/development/index.html>


<br>





























