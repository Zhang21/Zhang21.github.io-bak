---
title: Ansible
date: 2019-12-26 17:40:11
mathjax: true
categories: DevOps
tags:
  - Ansible
  - Automation
  - DevOps
---





参考:

- Ansible docs: <https://docs.ansible.com>



<br>



环境:



- RHELx86_64
- Ansible v2.9









<br/>
<br/>


---

<!--more-->


<br/>
<br/>









# 介绍

About Ansible: <https://docs.ansible.com/ansible/latest/index.html>

Ansible是一个IT自动化工具。它可以配置系统，部署软件和编排更先进的IT任务。Ansible的主要目标是简单和易于使用。它也专注于安全性和可靠性。

Ansible以无代理(agent-less)方式管理机器。Ansible是分散的，它依赖于现有操作系统的平局来控制访问到远程主机。如果需要，Ansible可以很容易地使用Kerberos, LDAP等集中认证管理系统连接。

















<br/>
<br/>

---

<br/>
<br/>















# 词汇表

[Glossary](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html)



- **Action**
动作(action)是任务的一部分，用于指定要运行的模块和传递给该模块的参数。每个任务只能有一个动作，但也可能有其它参数。

- **Ad Hoc**
指使用`/usr/bin/ansible`运行Ansible执行一些快速命令，而不是编排语言，即`/usr/bin/ansible-play-book`。ad hoc命令的示例可能是重新启动基础结构中的50台计算机。你可以通过编写playbook来完成你可以做的任何事情，而playbook也可以将许多其它操作粘合在一起。

- **Async**
指配置为在后台运行而不是等待完成的任务。如果你的进程时间长度超过了SSH超时时间，那么以异步(async)模式启动该任务是有意义的。异步模式可以每隔很多秒轮询完成，或者可配置为'fire and  forget'，在这种情况下，Ansible甚至不会再次检查任务，它将开始并继续进行未来的步骤。异步模式使用`/usr/bin/ansible`和`/usr/bin/ansible-playbook`。

- **Callback Plugin**
指一些用户编写的代码，可拦截Ansible的结构并对它们执行某些操作。GitHub中提供的一些示例执行自定义日志记录，发送电子邮件...

- **Check Mode**
值运行带有`--check`选项的Ansible，它不会对远程系统进行任何更改，但仅输出在没有此标志的情况下运行时才有可能发生的更改。

- **Connection Plugin**
默认情况下，Ansible通过pluggable libraries与远程计算机通信。Ansible支持原生OpenSSH或称为paramiko的Python实现。如果您使用的是最新版本，则首选OpenSSH，并启用Kerberos和jump host等功能。还有其它连接类型，如`accelerate`模式，必须通过一种基于SSH的连接类型进行引导，但速度非常快，而本地模式则作用于本地系统。用户还可以编写自己的连接插件。


- **Conditionals**
条件是一个表达式，其计算结果为`true`或`false`，用于决定给定任务是否在给定计算机上执行。

- **Declarative**
实现使用最终状态描述的任务的方法，而不是实现该状态所需的步骤序列的描述。对于真实世界的栗子，任务的声明规范将是: "put me in California"。根据你当前的位置，前往加州的步骤顺序可能会有所不同，如果你已在加州，则根本不需要做任何事情。Ansible的资源是声明性的；它确定了实现最终状态所需的步骤。它还可让你知道是否需要采取任何步骤才能到达最终状态。

- **Diff Mode**
将`--diff`标志传递给Ansible，以显示支持它的模块。

- **Executor**
Ansible的核心软件组件，它是`/usr/bin/ansible`背后的力量——并且对应于剧本中每个任务的调用。

- **Facts**
事实是发现的有关远程节点的事情。通过在远程节点上执行内部设置模块来运行，Ansible会自动发现事实。

- **Filter Plugin**
这允许创建新的Jinja2过滤器，这只适用于知道Jinja2过滤器的人。

- **Fork**
Ansible并行地与远程节点通信，并且可通过传递`--forks`或编辑配置文件中的默认值来设置并行级别。

- **Gather Facts (Boolean)**
有时，当运行多重playbook时，如果不需要利用任何这些值，则希望有一些不打扰事实计算的playbook。

- **Globbing**
通配符是一种选择大量主机，或它们所在组的名称的方法

- **Group**
一组主机

- **Group Vars**
这是将提供给指定组的变量，尤其是复杂的数据结构，这样这些变量就不必嵌入到库存文件或playbook中。

- **Handlers**
处理程序就像Ansible playbook中的常规任务，但只有在任务包含`notify`指定并且还指示它已更改某些内容时才会运行。

- **Host**
主机是Ansible管理的远程机器。

- **Host Specifier**
Ansible中的每个`play`都将一系列任务映射到一组系统。每个`play`中的`hosts:`指令通常称为主机说明符。它可以选择一个或多个系统，一个或多个组，甚至一个组中的一些主机，而不是另一个组中的主机。

- **Host Vars**
主机变量类似与组变量。

- **Idempotency**
如果执行一次的结果与在没有任何干预动作的情况下重复执行它的结果完全相同，则操作是幂等的。

- **Includes**
playbook文件可以包括其它play list，任务列表可以外部化其它文件中的任务列表，类似于处理程序。

- **Inventory**
用于描述Ansible中的主机和组的文件。

- **Inventory Script**
一个程序，用于查找主机，主机的组关系以及外部资源的变量信息——无论是SQL数据库，CMDB方案，还是LDAP等。

- **Jinja2**
Jinja2是Ansible模板模块的首选语言。它是一种非常简单的Python模板语言，可读且易于编写。

- **JSON**
Ansible使用JSON从远程模块返回数据。这允许用任何语言编写。

- **Lazy Evaluation**
通常，Ansible会在最后一秒评估playbook内容中的任何变量。

- **Library**
Ansible的模块集合。

- **Limit Groups**
通过将`--limit somegroup`传递给Ansible或ansible-playbook可以限制主机的子集。

- **Local Action**
针对远程计算机的playbook中的本地活动指令意味着给定的步骤实际上将在本地计算机上发生，但是可以传入变量`{{ansible_hostname}}`以引用该步骤中引用的远程主机名。

- **Local Connection**
通过在playbook中使用`connection: local`，或将`-c local`传递给`/usr/bin/ansible`，这表明我们正在管理本地主机而不是远程主机。

- **Lookup Plugin**
查找插件是一种从外部获取数据到Ansible的方法。

- **Loops**
通常，Ansible不是一种编程语言。它更喜欢声明性，尽管循环这样的各种结构允许对列表中的多个项重复特定任务。

- **Modules**
模块是Ansible发送到远程机器的工作单元。

- **Multi-Tier**
IT系统不是一次管理一个系统的概念，而是通过明确定义的订单中多个系统和系统组之间的交互。

- **Notify**
任务注册更改事件并通知处理程序任务需要在play结束时运行另一个操作的行为。

- **Orchestration**
许多软件自动化系统使用这个词来表示不同的东西。Ansible使用它作为编排的指挥。

- **paramiko**
默认情况下，Ansible通过SSH管理机器。Ansible默认使用的库是一个名为paramiko的Python驱动库。

- **Playbooks**
playbook是Ansible编排，配置，管理或部署系统的语言。它被称为剧本，部分原因在于它是一种运动类比，并且使用它们应该很有趣。

- **Plays**
A playbook is a list of plays。剧本最小是由主机说明符选择的一组主机之间的映射，以及在这些主机上运行定义这些系统将执行的角色的任务。

- **Pull Mode**
默认情况下，Ansible以`push`模式运行，这使得它可以在与每个系统进行通信时进行非常精细的控制。当你希望在特定计划时间点检查节点时，可以使用`pull`模式。

- **Push Mode**

- **Register Variable**
在Ansible中运行任何任务的结果可以存储在变量中，以便在模板或条件语句中使用。

- **Resource Model**
Ansible模块在资源方面起作用。

- **Roles**
角色是Ansible的组织单位。

- **Rolling Update**
一次解决组中的多个节点的行为，以避免一次更新所有节点并使系统脱机。

- **Sudo**

- **SSH (Native)**

- **Tags**
Ansible允许使用任意关键字标记剧本中的资源，然后仅运行与这些关键字对应的剧本部分。

- **Task**
任务将操作(模块及其参数)与名称和可选的其他关键字(如循环指令)组合在一起。

- **Templates**
Ansible可以轻松地将文件传输到远程系统，但通常需要在其它文件中替换变量。

- **Transport**
Ansible使用`term:`连接插件来定以可用传输的类型。

- **When**
一个可选的条件语句。

- **Vars (Variables)**
与事实相反，变量是值的名称(int, bool, string)或复杂的数据(dict, hash, lists)。它是声明的东西，而不是从远程系统获取的东西。

- **YAML**
Ansible不想强迫人们编写程序代码来自动化基础设施，因此使用YAML来定义剧本配置语言和变量文件。


































<br/>
<br/>

---

<br/>
<br/>













































# 安装指南

Installtion Guide: <https://docs.ansible.com/ansible/latest/installation_guide/index.html>


<br>


## 安装Ansible

Installing Ansible: <https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html>

Ansible是一个默认通过SSH协议管理机器的无代理(agentless)的自动化工具。一旦安装，Ansible不添加数据库，并且不需要启动守护进程。你只需要在一台机器上安装它，它可以从该中心点管理远程所有机器。





<br>

<br/>





### 先决条件

Prerequisite

在控制节点上安装Ansible，然后使用SSH(默认)与管理的节点通信。


<br>



**控制节点的依赖**
Control node requirements

目前，Ansible可以从任何安装了Python2.7或Python3.5+的机器上运行。不支持Windows。

<br>

**被管理节点的依赖**

Managed node requirements

在被管理的节点上，你需要一种方法来通信（通常是SSH）。



<br/>
<br/>



### 选择版本

Selecting an Ansible version to install

选择自己需要的Ansible版本进行安装，可选择一下几种方式：

- 使用操作系统包管理器进行安装
- 使用pip进行安装
- 使用源码进行安装


<br/>
<br/>



### 在RHEL上安装

Installing Ansible on RHEL, CentOS, or Fedora

```sh
yum search ansible

sudo yum install ansible
```



<br/>
<br/>



### 使用pip安装

Installing Ansible with pip

使用Python的包管理工具pip来安装Ansible。

```sh
# env
# python -m virtualenv ansible
# source ansible/bin/activate
pip install --user ansible
pip install --user paramiko
```


<br/>
<br/>



### Ansible command shell completion

Ansible 2.9的命令行工具由称为`argcomplete`的依赖提供。

```sh
sudo yum install epel-release
sudo yum install python-argcomplete

# pip
# pip install argcomplete
```

<br>

**配置argcomplete**

有两种方式来配置Ansible的命令行工具argcomplete：

- 全局(Globally)

```sh
# Global completion requires bash 4.2.
sudo activate-global-python-argcomplete
```

- 每个命令(Per command)

```sh
# If you do not have bash 4.2, you must register each script independently.
# 可将这些写入.profile里
eval $(register-python-argcomplete ansible)
eval $(register-python-argcomplete ansible-config)
eval $(register-python-argcomplete ansible-console)
eval $(register-python-argcomplete ansible-doc)
eval $(register-python-argcomplete ansible-galaxy)
eval $(register-python-argcomplete ansible-inventory)
eval $(register-python-argcomplete ansible-playbook)
eval $(register-python-argcomplete ansible-pull)
eval $(register-python-argcomplete ansible-vault)
```



<br/>
<br/>
<br/>



## 配置Ansible

Configuring Ansible: <https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html>

<br>

### 配置文件

Configuration file

Ansible将按照一下顺序搜索配置文件：

- `ANSIBLE_CONFIG`环境变量
- `ansible.cfg`当前目录
- `~/.ansible.cfg`
- `/etc/ansible/ansible.cfg`

<br>

[Ansible配置参考](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings)







<br/>
<br/>

---

<br/>
<br/>





# Ansible移植指南

Ansible Porting Guides: <https://docs.ansible.com/ansible/latest/porting_guides/porting_guides.html>









<br/>
<br/>

---

<br/>
<br/>


















# 用户指南

User Guide: <https://docs.ansible.com/ansible/latest/user_guide/index.html>


本指南介绍如何使用Ansible工作，包括CLI, invetory, playbooks。



<br/>
<br/>



## Quickstart

Ansible Quickstart Guide: <https://docs.ansible.com/ansible/latest/user_guide/quickstart.html>

<br/>
<br/>



## 概念

Ansible concepts: <https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html>


- **Controle node**：按照Ansible的任意机器。Windows机器无法作为控制节点。可以有多个控制节点。
- **Managed nodes**：使用Ansible管理的网络设备。通常称为主机，Ansible未安装在管理节点上。
- **Inventory**：一组管理节点的列表。清单文件有时称为主机文件(hostfile)。
- **Modules**：Ansible执行代码单元。[Ansible模块列表](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html#modules-by-category)
- **Tasks**：Ansible中的动作单元。可使用`ad-hoc`命令执行单一任务一次。
- **Playbooks**：任务的有序列表。可按照顺序反复执行这些任务。剧本可以包含变量和任务。它以YAML格式编写。



<br/>
<br/>



## 入门

Getting Started: <https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html>


一个基本的Ansible命令或playbooks：

- 从清单中选择机器来执行
- 连接到这些机器（通常是SSH）
- 复制一个或多个模块到远程机器，并执行


Ansible可以做很多事。一旦你理解了Ansible是如何工作的，你可以阅读有关的`ad-hoc`命令的详细信息，使用清单组织你的基础架构，并利用Ansible强大的playbooks。



<br/>
<br/>



### 从清单选择机器

Ansible从你的清单中读取管理的机器的信息。虽然你可以通过IP地址和`ad-hoc`命令，你也需要清单来增加Ansible的灵活性和重复性。

```
# 创建一个基本的清单
# 在此文件中添加远程系统
vim /etc/ansible/hosts
192.0.2.50
aserver.example.org
bserver.example.org
```

也可以使用别名(aliases)，主机变量(host vars)，组变量(group vars)。



<br>
<br/>



### 连接到远程节点

Ansible与远程机器通过SSH协议进行通信。默认情况下，Ansible使用原生的OpenSSH连接到远程机器。

确认用户名可使用SSH进行连接。如有必要，将SSH公钥添加到系统的`authorized_keys`文件。

<br>
<br>



### 复制和执行模块

一旦建立连接，Ansible传输你的命令或剧本需要的模块到远程机器。

```
# 运行第一个ansible命令
ansible all -m ping


# 运行一个节点上的命令
ansible all -a "/bin/echo hell"
```



<br>
<br>


## 如何构建清单

How to build your inventory: <https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html>

Ansible对多个被管理的节点使用被称为清单的列表或组列表。一旦清单定义，你可以选择主机或组来运行。


清单的默认位置是`/etc/ansible/hosts`。可以通过`-i`选项来指定不同的清单文件。也可以同时使用多个清单文件。从动态或云拉取清单。



<br>
<br>



### 清单基本

formats, hosts, groups。

清单文件有多种形式。最常用的是INI和YAML。

```ini
# INI格式
mail.example.com

# 组名
[webservers]
a.example.com
b.example.com

[dbserver]
db1.example.com
db2.example.com
db3.example.com
```

```yaml
# YAML格式
all:
  hosts:
    mail.example.com:
  children:
    webserver:
      hosts:
        a.example.com:
        b.example.com:
    dbservers:
      hosts:
        db1.example.com:
        db2.example.com:
        db3.example.com:
```

<br>

默认组(default groups)，有两个默认组。

- `all`：包含每个主机
- `ungrouped`：all中没有组的主机


<br>

在多个组中的主机(Hosts in multiple groups)。

```yaml
all:
  hosts:
    mail.example:
  children:
    webservers:
      hosts:
        f.example.com:
        b.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
    east:
      hosts:
        f.example.com:
        one.example.com:
    west:
      hosts:
        b.example.com:
        two.example.com:
    prod:
      children:
        east:
    test:
      hosts:
        b.example.com:
```

<br>

添加主机范围(Adding ranges of hosts)。如果有很多主机有一个类似的模式，可将其添加为一个范围，而不是单独列出每个主机名。

```yaml
...
  webservers:
    hosts:
      www[01:20].example.com:
  dbservers:
    hosts:
      db-[a:f].example.com
```


<br>
<br>



### 添加变量到清单

Adding variables to inventory


可以在清单中存储涉及到特定主机或组的变量值。


<br/>
<br/>



### 主机变量

Assigning a variable to one machine: host variables


```yaml
atlanta:
  hosts1:
    http_port: 80
    maxRequestPerChild: 808
  hosts2:
    http_port: 303
    maxRequestPerChild: 909
```

<br>

清单别名(Inventory aliases)。在清单中定义别名：

```yaml
...
  hosts:
    jumper:
      ansible_port: 5555
      ansible_host: 192.0.2.50
```


<br/>
<br/>



### 组变量

Assigning a variable to many machines: group variables

在一组的主机中共享变量值。

```yaml
atlanta:
  hosts:
    hosts1:
    host2:
  vars:
    ntp_server: ntp.atlanta.example.com
    proxy: proxy.atlanta.example.com
```


<br>


继承变量值(Inheriting variable values: group variables for groups of groups)。可使用`children:`(yaml)来构建组的组，同样，可使用`vars:`来构建组变量的组变量。


```yaml
all:
  children:
    usa:
      children:
        southeast:
          children:
            atlanta:
              hosts:
                hosts1:
                hosts2:
            raleigh:
              hosts:
                hosts2:
                hosts3:
            vars:
              some_server: foo.southeast.example.com
              halon_system_timeout: 30
              self_destruct_countdown: 60
              escape_pods: 2
          northeast:
          norethwest:
          southwest:
```

子组有几个属性的注意事项：

- 子组成员的任何主机自动成为父组的成员
- 子组的变量的优先级高于(覆盖)父组的变量
- 组可以有多个父亲和孩子
- 主机可以在多个组，但只会有一台主机实例，合并来自多个组的数据


<br/>
<br/>



### 组织主机和组变量

Organizing host and group variables

尽管你可以将变量存储在清单文件，但存储独立的主机和组变量可以帮助您更轻松地阻止你的变量值。主机和组变量文件必须使用YAML语法。

Ansible通过搜索清单文件或剧本文件的路径来载入主机和组变量文件。



<br/>
<br/>



### 变量如何合并

How variables are merged

默认情况下，在play运行前变量被合并到特定的主机。这使Ansible集中在主机和任务，因此组并没有真正生存在清单和主机匹配之外。Ansible覆盖变量的顺序：

- all group
- parent group
- child group
- host


默认情况下Ansible在相同的父/子级按字母顺序合并组，并在最后一组加载覆盖前面的组。你可以通过设置组变量`ansible_group_priority`来改变同级组合并顺序的行为。数字越大，优先级就越高。默认值是1。


```yaml
# testvar == a
a_group:
  testvar: a
  ansible_group_priority: 10
b_group:
  testvar: b
```


<br/>
<br/>



### 使用多个清单源

Using multiple inventory sources

可通过在命令行中或配置`ANSIBLE_INVENTORY`通过给定多个清单参数在同一时间目标多个清单源（目录，动态清单脚本，清单插件...）。

```yaml
# target 2 sources
ansible-playbook get_logs.yml -i staging -i production
```

<br>

**以一个目录组合多个清单源(Aggregating inventory sources with a directory)**

还可以通过一个目录下结合多个清单源和原类型来创建清单。这对于动静结合主机和管理它们为一体化清单很有用。

```yaml
inventory/
  openstack.yml          # configure inventory plugin to get hosts from Openstack cloud
  dynamic-inventory.py   # add additional hosts with dynamic inventory script
  static-inventory       # add static hosts and groups
  group_vars/
    all.yml              # assign variables to all hosts
```


```sh
# target inventory
ansible-playbook example.yml -i inventory
```


<br/>
<br/>



### 清单参数

Connecting to hosts: behavioral inventory parameters

以下变量控制与远程主机如何与Ansible相互作用。


<br/>
<br/>



### 清单配置样例

Inventory setup examples

- 每个环境一个清单(One inventory per environment)
- 通过功能分组(Group by function)
- 通过地址分组(Group by location)


```
# Example: One inventory per environment
# inventory_test
[dbservers]
db01.test.example.com
db02.test.example.com
```


```yaml
# Example: Group by function
- hosts: dbservers
  tasks:
  - name: allow access from 10.0.0.1
    iptables:
      chain: INPUT
      jump: ACCEPT
      source: 10.0.0.1
```

```
# Example: Group by location
[dc1]
db01.test.example.com
app01.test.example.com
```



<br/>
<br/>
<br/>



## 动态清单

Working with dynamic inventory: <https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html>


<br/>


### cobbler


<br/>
<br/>


### AWS ec2



<br/>
<br/>



### OpenStack



<br/>
<br/>



### 其它清单脚本

Other inventory scripts





<br/>
<br/>
<br/>





## 模式

Patterns: targeting hosts and groups: <https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html>

当你通过`ad-hoc`或`playbook`执行Ansible时，你必须选择要对哪些节点或组执行。模式可以让你针对清单中的特定主机或组执行。一个Ansible Pattern可以指定单个主机、IP地址、清单组、一组组、所有主机...模式非常灵活，可以排除需要的主机子集、使用通配符、正则表达式...Ansible将在包含在模式上的所有清单主机上执行。



<br/>



### 模式使用

Using patterns

```sh
# ad-hoc
# ansible {pattern} -m {module_name} -a "{module_options}"
ansible webservers -m service -a "name=httpd state=restarted"
```


```yaml
# palybook
- name: {play_name}
  hosts: {pattern}

- name: restart webservers
  hosts: webservers
```



<br/>
<br/>



### 常见模式

Common patterns


| 描述 | 模式 | 目标 |
| - | - | - |
| All hosts | all(*) | - |
| One host | host1 | - |
| Multiple hosts | host1:host2(host1,host2) | - |
| One group | g1 | - |
| Multiple groups | g1:g2 | all hosts in g1 and g2 |
| Excluding groups | g1:!g2 | all hosts in g1 except those in g2 |
| Intersection of groups | g1:&g2 | g1和g2的交集 |



<br/>
<br/>



### 模式的局限性

Limitations of patterns

模式依赖于清单。如果主机或组不在清单中，则不能使用模式来目标它。如果模式中包含清单中不存在的IP地址或主机名，会报错。模式必须匹配清单语法。



<br/>
<br/>



### 高级的模式选项

Advanced pattern options

常用的模式将满足你的大部分需求，但Ansible提供了几种方法来定义你需要定位(target)的主机和组。


**在模式中使用环境变量**
Using variables in patterns


```
# playbook
webservers:!{{ excluded }}:&{{ required }}
```

**在模式中使用组位置**
Using group position in patterns

```
[g1]
aa
bb
cc


g1[0]
g1[-1]
g1[0:2]
g1[1:]
```

**在模式中使用正则**
Using regexes in patterns

以`~`符号开始使用模式的正则: `~(web|db).*\.example\.com`



<br/>
<br/>



### playbook标志

Patterns and ansible-playbook flags

可以使用命令行选项改变playbook中定义的行为。

```sh
ansible-playbook site.yml --limit datacenter2
```



<br/>
<br/>
<br/>




## ad-hoc

Introduction to ad-hoc commands: <https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html>

一个Ansible的`ad-hoc`命令使用ansible命令行工具在一个或多个管理节点上执行单一任务。`ad-hoc`命令是快速和容易的，但却无法重复使用。那么为什么首先学习`ad-hoc`命令呢？它表明Ansible的简单和功能。在这学的内容可直接到playbook里。在执行前，请先阅读构建清单。

`ansible`命令行实用程序的默认模块是[command module](https://docs.ansible.com/ansible/latest/modules/command_module.html)。

如果像重复一个命令，可使用playbook中的template module。



<br/>



### 为什么使用它

Why use ad-hoc commands?

`ad-hoc`命令针对的是很少会重复的任务。

```sh
# 栗子
ansible [pattern] -m [module] -a "[module options]"
```


<br/>
<br/>



### 用例

Use cases for ad-hoc tasks

`ad-hoc`任务可用来重启服务器、复制文件、管理包和用户...可在`ad-hoc`任务中使用任意Ansible模块。Ad-hoc tasks与playbooks类似，使用一个声明模型，计算并执行以达到规定的最终状态所需的操作。

<br>

**重启服务器**

`ad-hoc`任务调用命令模块。在执行前，确保清单和SSH。

```sh
# rebooting servers
ansible host1 -a "/sbin/reboot"


# 默认是5并发进程
ansible host1 -a "/sbin/reboot" -f 10


# ansible将默认为你的用户账户
ansible host1 -a "/sbin/reboot" -f 10 -u username



# 重启服务器可能需要特权提升，如从user到root
ansible host1 -a "/sbin/reboot" -f 10 -u username --become [--ask-become-pass]


# 使用不同的模块
ansible host1 -m shell -a 'echo ${TERM}'
```

<br>

**文件管理**

`ad-hoc`可利用Ansible和scp的力量，并行传输文件到多台机器。

```sh
# 复制文件
ansible atlanta -m copy -a "src=/etc/hosts dest=/tmp/hosts"

# file模块属主和权限，创建目录，递归删除
ansible webservers -m file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
ansible webservers -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
ansible webservers -m file -a "dest=/path/to/c state=absent"
```

<br>

**包管理**

使用`ad-hoc`任务使用包管理模块（如yum），来安装、升级、移除包。

Ansible有许多平台的许多包管理工具的模块，详情请看文档。

```sh
# 安装了包不更新
ansible webservers -m yum -a "name=acme state=present"

# 特定包版本
ansible webservers -m yum -a "name=acme-1.5 state=present"

# 确认包是最新版
ansible webservers -m yum -a "name=acme state=latest"


# 确保未安装
ansible webservers -m yum -a "name=acme state=absent"
```

<br>

**管理用户和组**

使用`ad-hoc`任务在管理的节点上创建、管理、移除用户账户。

```sh
ansible all -m user -a "name=foo password={crypted password here}"

ansible all -m user -a "name=foo state=absent"
```

<br>

**服务管理**

```sh
# 确保服务已启动
ansible webservers -m service -a "name=httpd state=started"


# 重启服务
ansible webservers -m service -a "name=httpd state=restarted"


# 确保服务已停止
ansible webservers -m service -a "name=httpd state=stopped"
```


<br>


**收集事实**

事实代表发现关于系统的变量。

```sh
# 查看所有facts
ansible all -m setup
```




<br/>
<br/>
<br/>





## 连接方法和详情

Connection methods and details: <https://docs.ansible.com/ansible/latest/user_guide/connection_details.html>



<br/>



### ControlPersist和paramiko

默认情况下，Ansible使用原生的OpenSSH，因为它支持ControlPersist（一个性能特点），Kerberos，和`~/.ssh/config`中的配置。如果你的控制机使用的旧版本OpenSSH不支持ControlPersist，Ansible将回退到称为`paramiko`的一个Python实现的OpenSSH。



<br/>
<br/>



### ssh-key配置

SSH key setup


默认情况下，Ansible假定您使用SSH keys连接到远程主机。推荐使用key，但可使用`--ask-pass`选项来使用密码。使用`--ask-become-pass`选项来使用特权提升。

```sh
# 建立ssh agent来避免输入密码
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```


<br/>
<br/>



### 本地运行

Running against localhost

```sh
ansible localhost -m ping -e 'ansible_python_interpreter="/usr/bin/env python"'
```



<br/>
<br/>


### 主机密钥检查

Host key checking

Ansible默认启用主机密钥检查。如果主机重装并在`known_hosts`中有不同的密钥，这将导致一个错误消息，知道纠正。

可在`/etc/ansible/ansible.cfg`或`~/.ansible.cfg`中禁用它:

```
[defaults]
host_key_checking = False
```

或设置环境变量: `export ANSIBLE_HOST_KEY_CHECKING=False`



<br/>
<br/>



### 其它连接方法

Other connection methods

除了SSH之外，Ansible还可以使用许多连接方法。





<br/>
<br/>
<br/>





## 命令行工具

Working with command line tools: <https://docs.ansible.com/ansible/latest/user_guide/command_line_tools.html>

大多数用户对`ansible`和`ansilbe-playbook`比较熟悉，但它们不是Ansible提供的唯一实用工具。下面是完整的Ansible使用工具列表。

- [ansible](https://docs.ansible.com/ansible/latest/cli/ansible.html): 在一组主机上定义和运行一个单任务playbook
- [ansible-config](https://docs.ansible.com/ansible/latest/cli/ansible-config.html): 查看Ansible配置信息
- [ansible-console](https://docs.ansible.com/ansible/latest/cli/ansible-console.html): REPL控制台执行Ansible任务
- [ansible-doc](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html): 插件文档工具
- [ansible-galaxy](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html): 执行各种角色并收集相关的操作
- [ansible-invotory](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html): 显示或转配置清单
- [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html): 运行Ansible playbook
- [ansible-pull](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html): 从仓库拉playbook并为本地主机执行
- [ansible-valut](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html): Ansible数据文件的加解密工具



<br/>
<br/>
<br/>





## playbook

Working With Playbooks: <https://docs.ansible.com/ansible/latest/user_guide/playbooks.html>

Playbooks是Ansible的配置(configuration)、部署(deployment)和编排(orchestration)语言。它可以描述你希望你的远程系统强制执行的策略，或在IT流程的步骤。

在最基本的级别上，playbook可以被用来管理部署的配置到远程机器。在更高级的，它们可以序列进行涉及多层(mulit-tier)的滚动更新和回滚，并可以委托操作其它主机，与监控服务器进行交互和负载均衡。它有很多功能和信息，详情看文档。

playbook被设计为人类可读的和基于文本语言开发。有多种方式来组织playbook和它包含的文件。

你应该看一看[Example Playbooks](https://github.com/ansible/ansible-examples)，并与playbook文档一起阅读。这些说明的最佳实践，以及如何把众多的各种概念混合在一起。



<br/>



### 介绍

Intro to Playbooks: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html>

playbook是比在`ad-hoc`任务执行模式下的一个完全不同的使用ansible的方式，并且特别强大。

简单来说，Playbook是一个非常基础的用于配置管理和多机部署系统，不同于任何已存在的，并且非常适合部署复杂的应用程序。

playbook可以声明配置，但它也可以通过编排任意手动排序进程的步骤，尽管不同的步骤必须来回在特定命令的机器之间。它可以同步(synchronously)或异步(asynchronously)发射任务。

虽然你为`ad-hoc`任务运行主要的`/usr/bin/ansible`程序，playbook更可能被保持在原控制和用于推送配置或保证远程系统上的配置。palybook example中有许多栗子，建议去看一看。



<br/>


#### playbook language

playbook以YAML语法格式表示，故意不设计成一种编程语言或脚本，而是过程或配置的模型。

每个playbooks由列表中的play组成。play的目标是映射一组主机到一些良好定义的角色(roles)，由ansible调用任务来表示。通过多个paly组成playbook，有可能协调多机部署，在某个组的所有机器上运行某些步骤...你可以由相当多的影响你的系统做不同事情的paly。

```yaml
# 仅包含一个paly的`verigy-apache.yml`的playbook的栗子
---
- hosts: webserver
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running
    service:
      name: httpd
      state: started
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```


```yaml
# 包含多个play的栗子
---
- hosts: webservers
  remote_user: root

  tasks:
  - name: ensure apache is at the lastest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf

- hosts: databases
  remote_user: root

  tasks:
  - name: ensure pgsql is at the latest version
    yum:
      name: postgresql
      state: latest
  - name: ensure that postgresql is started
    service:
      name: postgresql
      state: started
```



<br/>
<br/>



#### 基本

Basics

<br>

**主机和用户**
Hosts and Users

对于playbook中的每个play，你可以选择哪些机器在你的基础设施到目标，什么远程用户完成这些步骤。

```yaml
# hosts行是一个或多个主机或组的模式，以冒号分隔
---
- hosts: webservers
  remote_user: root
  tasks:
    - name: test
      ping:
      remote_user: username # 远程用户可在每个用户中定义
```

```yaml
# 特权提升
---
- hosts: webservers
  remote_user: username
  become: yes
```

```yaml
# 也可在每个paly中使用become
---
- hosts: g1
  remote_user: uername
  tasks:
    - service:
        name: nginx
        state: started
      become: yes
      become_method: sudo
```

```yaml
# 权限提升为特定用户
---
- hosts: g1
  remote_user: username
  become: yes
  become_user: postgres
```

```yaml
# 控制运行顺序，默认是清单里面的顺序
---
- hosts: all
  order: sorted
  gather_facts: False
  tasks:
    - debug:
        var: inventory_hostname
```

<br>


**任务列表**
Tasks list

每个play包含任务列表。任务在移动到下一个任务之前执行，一次一个，由模式匹配的所有主机。理解在一个play中，所用主机都将得到同样的任务指令是很重要的。这是play映射选择主机到任务的目的。

当运行playbook时，它从上到下运行，失败任务的主机被从playbook轮转中取出。如果事情失败，只是纠正playbook文件，然后重新运行。

每个任务的目标是执行带有特定参数的模块，变量可以在参数中传给模块。

```yaml
# 一个任务的基本栗子
tasks:
  - name: make sure apache is running
    service:
      name: httpd
      state: started
```

```yaml
# command, shell模块
tasks:
  - name: enable selinux
    command: /sbin/setenfore 1

tasks:
  - name: run this command and ignore the result
    shell: /usr/bin/somecommand || /bin/true
```

```yaml
tasks:
  - name: create a virtual host file for {{ vhost}}
    template:
      src: somefile.j2
      dest: /etc/httpd/conf.d/{{ vhost}}
```


<br/>
<br/>



#### action shorthand

```yaml
# Ansible prefers listing modules like this:
template:
  src: template/foo.j2
  dest: /etc/foo.conf
```

早期版本使用以下格式，仍旧有效: `action: template src=templates/foo.j2 dest=/etc/foo.conf`



<br/>
<br/>



#### Handlers

Handlers: Running Operations On Change

如前所述，当远程系统上做了改变时模块应该是幂等的和可以中继(relay)。playbook认识到了这一点，并有一个基本的事件系统用于应对改变。

这些play中的`notify`行动在任务的每个末尾块触发，即使被多个不同任务通知也只能被触发一次。

例如，多个资源可能表明Apache需要重启，因为配置文件发生了更改，但Apache将跳跃一次，以避免不必要的重启。

```yaml
# 当一个文件的内容更改时（仅此文件），重启两个服务的栗子
- name: template configuration file
  template:
    src: template.j2
    dest: /etc/foo.conf
  notify:
    - restart memcached
    - restart apache
```


在任务的`notify`中列出的事情的部分被称为处理程序(handlers)。

处理程序是任务列表，与常规的任务没什么区别，由一个全局唯一的名称进行引用，并通过通知程序(notifier)进行通知。如果没有事情通知一个处理程序，它将不会运行。不管有多少任务通知处理程序，它只能运行一次，在一个特定paly中的所有任务完成之后。

```yaml
# handlers section
handlers:
  - name: restart memcached
    service:
      name: memcached
      state: restarted
  - name: restart apached
    service:
      name: apache
      state: restarted
```

你可能想让Ansible handlers使用变量。如果handlers name使用的变量不可用，则整个paly将失败。取而代之的是，在handlers的任务参数中使用变量。

```yaml
tasks:
  - name: Set host variables based on distribution
    include_vars: "{{ ansible_facts.distribution}}.yml"

handlers:
  - name: restart web service
    service:
      name: "{{ web_service_name | default('httpd') }}"
      state: restarted
```

Ansible 2.2，handlers可以监听(listen)通用话题(generic topics)，任务可以通知这些话题。以下这种使用使它更容易触发多个处理程序。它还从名称解耦处理程序，使得在playbook和roles之间更容易共享处理程序。（特别是当使用像Galaxy的第三方角色时）

```yaml
handlers:
  - name: restart memcached
    service:
      name: memcached
      state: restarted
    listen: "restart web service"
  - name: restart apache
    service:
      name: apached
      state: restarted
    listen: "restart web services"

tasks:
  - name: restart everything
    command: echo "this task will restart the web services"
    notify: "resstart web service"
```


<br>

> **注意:**
> Notify handlers are always run in the same order they are defined, not in the order listed in the notify-statement. This is also the case for handlers using listen.
> Handler names and listen topics live in a global namespace.
> Handler names are templatable and listen topics are not.
> Use unique handler names. If you trigger more than one handler with the same name, the first one(s) get overwritten. Only the last one defined will run.
> You cannot notify a handler that is defined inside of an include. As of Ansible 2.1, this does work, however the include must be static.


<br>

角色(Roles)后面会说明，但它值得指出的是：

- handlers notified within `pre_tasks`, `tasks`, and `post_tasks` sections are automatically flushed in the end of section where they were notified
- handlers notified within `roles` section are automatically flushed in the end of `tasks` section, but before any `tasks` handlers
- handlers are play scoped and as such can be used outside of the role they are defined in



<br/>
<br/>



#### 执行playbook

Executing A Playbook

```sh
ansible-playbook playbook.yml -f 10
```


<br/>
<br/>



#### Ansible-Pull

节点检查到重要位置，而不是推送配置给它们。`ansilbe-pull`是一个检查从git指令配置仓库的一个脚本，然后针对改内容运行`ansible-playbook`。



<br/>
<br/>



#### Linting playbooks

你可以在执行前使用`ansible-lint`来检查playbook的运行情况。

```yaml
ansible-lint verify-apache.yml
[403] Package installs should not use latest
verify-apache.yml:8
Task/Handler: ensure apache is at the latest version
```



<br/>
<br/>


#### 其它playbook验证项

Other playbook verification options

查看[验证playbook工具](https://docs.ansible.com/ansible/latest/community/other_tools_and_programs.html#validate-playbook-tools)的详情列表，你可以使用它们来验证playbook。这里有些情况你应该考虑：

- 要检查playbook语法问题，使用`ansible-playbook`的`--syntax-check`标志。
- 要查看完整的输出信息，使用`--verbose`标志。
- 要查看playbook会影响哪些主机，可运行: `ansible-playbook playbook.yml --list-hosts`


<br/>
<br/>

### 可重复使用的playbook

Creating Reusable Playbooks: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html>

虽然可以在一个非常大的文件里编写playbook，最终你会想重新使用文件和整理东西。在Ansible中，有三种方式可以做到这一点：

- includes
- imports
- roles

includes和imports允许用户将大型playbook分解为小型文件，可跨多个parent playbook或设置多次在相同的playbook里。
roles允许不仅仅是任务可以打包在一起，可以包括变量(variables)，处理程序(handlers)，甚至模块(modules)和其它插件(plugins)。不同于includes和imports，roles也可上传并经由Ansible Galaxy共享。

<br>


#### including和importing

Including and Importing: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_includes.html>

include和import语句相似，但Ansible执行引擎处理它们却非常不同。

`import*`语句在playbook被解析的时候进行预处理(pre-processed)。
`include*`语句在playbook的执行过程中遇到才处理。

include和import语句可以在任意深度使用。

<br>

在一个master playbook内包含playbook：

```yaml
- import_playbook: a.yml
- import_playbook: b.yml
```

每个playbook中列出的plays和tasks将以列出的顺序 执行，就好像它们已经在这里直接定义。

<br>

大型任务划分成不同的文件是组织复杂任务或重用它们的一种好方式。一个任务文件只包含简单的任务列表：

```yaml
- name: aaa
  command: /bin/aaa
- name: bbb
  command: /bin/bbb
```

可以使用`import_tasks`或`include_tasks`来执行在主任务列表中的文件的任务：

```yaml
tasks:
- import_tasks: aaa_tasks.yml
# or
- include_tasks: aaa_tasks.yml
```

你也可以传递变量到imports和includes：

```yaml
tasks:
- import_tasks: aaa.yaml
  vars:
    user: aaa
- import_tasks: aaa.yml
  vars:
    user: bbb
- import_tasks: aaa.yml
  vars:
    user: ccc
```

<br>

include和import同样可以用于`handlers:`部分。栗子：

```yaml
# more_handlers.yml
- name: restart apache
  service:
    name: apache
    state: restarted
```

```yaml
handlers:
- include_tasks: more_handlers.yml
# or
- import_tasks: more_handlers.yml
```


<br>
<br>


#### Roles

roles: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html>

角色(role)是基于已知的文件架构自动加载某些变量文件、任务和处理程序的方式。按角色分组的内容还可以方便地与其他用户共享。


<br>


##### 角色目录结构

Role Directory Structure

栗子：

```yaml
site.yml
webservers.yml
fooservers.yml
roles/
    common/
        tasks/
        handlers/
        files/
        templates/
        vars/
        defaults/
        meta/
    webservers/
        tasks/
        defaults/
        meta/
```

角色在特定目录名下期待文件。角色必须包括这些目录中的至少一个，但它是完全没排除任何未使用。在使用时，每个目录必须包含一个`main.yml`文件，其中包含的相关内容：

- `tasks`：包含由角色执行的主任务列表
- `handlers`：包含可通过此角色甚至此角色外的任何地方使用的处理程序
- `defaults`：角色的默认变量
- `vars`：角色的其它变量
- `files`：通过此角色可以部署的文件
- `templates`：通过此角色可以部署的模板
- `meta`：为角色定义的元数据

其它YAML文件可能包含在特定目录。栗子：

```yaml
# roles/example/tasks/main.yml
- name: added in 2.4, previously you used include
  import_tasks: redhat.yml
  when: ansible_facts['os_family']|lower == 'redhat'
  import_tasks: debian.yml
  when: ansible_facts['os_family']|lower == 'debian'
```

```yaml
# roles/example/tasks/redhat.yml
- yum:
  name: "httpd"
  state: present
```

```yaml
# roles/example/tasks/debian.yml
- pat:
  name: "apache2"
  state: present
```

角色还可以包含模块和其它插件类型。


<br>
<br>


##### 角色使用

使用角色的原始的方式是在play中通过`roles:`：

```yaml
---
- hosts: webservers
  roles:
    - common
    - webservers
```

这将为每个角色(x)指定以下行为：

- 如果`roles/x/tasks/main.yml`存在，其中列出的任务将被添加到play；
- 如果`roles/x/handlers/main.yml`存在，其中列出的处理程序将被添加到play；
- 如果`roles/x/vars/main.yml`存在，其中列出的变量将被添加到play；
- 如果`roles/x/defaults/main.yml`存在，其中列出的变量将被添加到play；
- 如果`roles/x/meta/main.yml`存在，其中列出的任何角色的依赖都将被添加到角色列表；
- 角色中的任意copy, script, template, include tasks，可在`roles/x/{files,templates,tasks}/dir`进行引用，而不必关心它们的相对或绝对路径。

当以这种方式使用时，playbook的执行顺序如下：

- play中定义的任意`pre_tasks`
- 任意处理程序触发到目前为止将会运行
- 在roles中列出的每个角色将依次执行。在角色`meta/main.yml`中定义的任意角色依赖将首先运行，受标签过滤和条件
- play中定义的任意`tasks`
- 任意处理程序触发到目前为止将会运行
- play中定义的任意`post_tasks`
- 任意处理程序触发到目前为止将会运行

可使用`import_role`或`include_role`在其它任务中使用角色：

```yaml
---
- hosts: webservers
  tasks:
    - debug:
        msg: "before we run our role"
    - import_role:
        name: example
    - include_role:
        name: example
    - debug:
        msg: "after we ran our role"
```

当角色在原始方式中定义，它们被视为静态导入和在playbook解析时进行处理。

角色的名称可是很简单，也可以是一个完全合格的路径：

```yaml
---
- hosts: webservers
  roles:
    - role: '/path/to/roles/common'
```

角色可以接受其它关键字：

```yaml
---
- hosts: webservers
  tasks:
    - include_role:
        name: foo_app_instance
      when: "ansible_facts['os_family'] == 'RedHat'"
      vars:
        dir: '/opt/a'
        app_port: 5000
      tags:
        - aaa
        - bbb
```


<br>
<br>


##### 角色副本和扩展

Role Duplication and Execution

Ansible只允许一个角色执行一次，即使多次定义：

```yaml
---
- hosts: webservers
  roles:
    - foo
    - foo
```

上面给出的`foo`角色仅将运行一次。为了使角色多次运行，有两种选择：

1. 每个角色传递不同的参数
2. 添加`allow_duplicates: true`到`meta/main.yml`文件

```yaml
# playbook.yml
---
- hosts: webservers
  roles:
    - foo
    - foo

# roles/foo/meta/main.yml
---
allow_duplicates: true
```


<br>
<br>


##### 角色默认变量

Role Default Variables

角色的默认变量允许你为角色设定默认变量。在角色目录中添加`defaults/main.yml`文件。这些变量具有最低优先级，可以轻易被覆盖。


<br>
<br>


##### 角色依赖

Role Dependencies

角色依赖让你在其它角色使用角色时自动拉取。角色依赖存储在角色目录的`meta/main.yml`文件。此文件应包含角色和参数列表在指定角色之前插入：

```yaml
# orles/myapp/meta/main.yml
---
dependencies:
  - role: common
    vars:
      come_parameter: 3
  - role: apache
    vars:
      apache_port: 80
  - role: postgres
    vars:
      dbname: blarg
      other_parameter: 12
```


<br>
<br>


##### 角色中的嵌入模块和插件

Embedding Modules and Plugins In Roles


<br>
<br>


##### 角色搜索路径

Ansible将按以下方式为角色搜索：

- 相对于playbook文件的`roles/`目录
- 默认情况下，在`/etc/ansible/roles`


<br>
<br>


##### Galaxy

Ansible Galaxy是一个用于查找、下载、评级、审查各种社区ansible roles的免费网站。

`ansible-galaxy`客户端包含在Ansible中。可使用它从Ansible Galaxy下载角色。


<br>
<br>


#### 动态与静态

Dynamic vs. Static

Ansible有两种操作模式用于可重用内容：动态与静态。

如果你使用`include*`，它将是动态的。如果你使用`import*`，它是静态的。

<br>

**动态与静态的区别**

两种操作模式都非常简单：

- 动态包含在遇到任务运行时处理
- 静态导入在解析playbook前处理

当遇到`tag`或`when`：

- 动态包含仅适用于动态的任务，不会复制到子任务
- 静态导入，将被复制到所有子任务


<br>
<br>


#### 两者比较

使用`include*`与`import*`有一定的优势，权衡两者。

使用`include*`语句的最大优势是循环。当在include中使用循环，所包含的将在每个循环中执行。

- tags仅在动态包含内存在
- tasks仅在动态包含内存在
- 在动态包含内不能使用`notify`来触发处理程序
- 在动态包含内不能使用`--start-at-task`来开始执行

- 静态导入内循环不能使用
- 静态导入内不能从库存源使用变量
- 静态导入内当通知它们的名字时，使用处理程序将不会触发



<br>
<br>



### 使用变量

Using Variables: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html>

Ansible中使用变量可以更好地帮助处理各系统之间的差异。


<br>


#### 创建有效的变量名

Creating valid variable names

有效的变量名是很重要的。变量名应该是字母 、数字和下划线，且总是以字母开头。

YAML也支持映射键值对的字典：

```yaml
foo:
  field1: one
  field2: two
```

你可以使用中括号或点来引用特定字段的值:

```
foo['field1']
foo.field2
```

请注意，如果使用点来引用，它们属性和Python字典的方法相冲突可能会导致一些问题。如果你使用的键开始和结束有两个下划线，或它们是已知的公共属性，则你应该使用中括号来代替点使用。

```
# 公共属性
add, append, count, decode...
```


<br>
<br>


#### 在清单中定义变量

Defining variables in inventory

通常，你需要为单独的主机或组设置变量。你可以在清单文件(如hosts)中定义所需的变量：

```yaml
west:
  host1:
    port: 80
    maxRequest: 808
```

```yaml
east:
  hosts:
    host1: xx
    host2: xxx
  vars:
    port: 80
```


<br>
<br>


#### 在playbook中定义变量

Defining variables in a playbook

你可以直接在playbook中定义变量：

```yaml
- hosts: xxx
  vars:
    port: 80
```


<br>
<br>


#### 在文件和角色中定义变量

Defining variables in included files and roles

```yaml
- hosts: xxx
  roles:
    - role: test
      vars:
        dir: '/opt/a'
    - role: test2
      vars:
        dir: '/opt/b'
```


<br>
<br>


#### 在Jinja2中使用变量

Using variables with Jinja2

一旦你定义了变量，便可以在Jinja2的模板系统中引用它：

```jinja2
Ma amp goes to {{ max_amp_value }}
```


<br>
<br>


#### 使用Jinja2过滤器转换变量

Transforming variables with Jinja2 filters

Jinja2 filters 让你在模板表达式内转换变量的值。如`capitalize`大写过滤器，`to_yaml`和`to_json`过滤器来转换成对应格式。

Jinja2包含了许多内置过滤器： <https://jinja.palletsprojects.com/en/2.11.x/templates/#builtin-filters>
Ansible也支持许多过滤器： <https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#playbooks-filters>


<br>
<br>


#### YAML疑难杂症

Hey wait, a YAML gotcha

YAML语法要求，如果你使用`{{ foo }}`值引用整行，它要确保你不是想开始一个YAML字典。所以记得使用双引号。

```yaml
# wrong
...
vars:
  path: {{ dir }}/22

# right
vars:
  path: "{{ dir }}/22"
```


<br>
<br>


#### 系统facts中的变量

Variables discovered from systems: Facts

facts是从远程系统获得的信息。你可以从`ansible_facts`变量中找到。

```sh
# 查看facts
ansible hostname -m setup
```

```yaml
- debug:
    var: ansible_facts
```

```
{{ ansible_facts['devices']['xvda']['model']}}
```

<br>

**禁用facts**

如果你不需要fact数据，你可以禁用它。

```yaml
- hosts: xxx
  gather_facts: no
```

<br>

**Local facts(facts.d)**

facts通常都是由Ansible setup模块自动发现。用户也可以编写自定义的facts模块，请参考API指南。但是，如果你想要一个简单的方法来使用用户提供的数据，而不需要写一个facts模块。

`facts.d`是一种可让用户控制它们的系统是如果管理的某些方面的机制。

如果远程管理系统有`/etc/ansible/facts.d`目录，该目录中的所有`.fact`文件（JSON, INI...）。可使用`fact_path` paly 关键字作为可选目录。


<br>
<br>


#### 注册变量

Registering variables

另一个主要使用的变量是正在运行一个命令和将此命令的返回的结果注册为一个变量，供其它地方使用。

```yaml
- hosts: xxx
  tasks:
    - shell: /usr/bin/foo
      register: foo_result
      ignore_errors: True
    - shell: /usr/bin/bar
      when: foo_result.rc == 5
```


<br>
<br>


#### 访问复杂的变量数据

Accessing complex variable data

有些提供的facts，如网络信息，包含了复杂的嵌套结构。取值会稍微麻烦一些：

```jinja2
{{ ansible_facts['eth0']['ipv4']['address']}}

# or
{{ ansible_facts.eth0.ipv4.address }}

# 访问数组的第一个元素
{{ foo[0] }}
```


<br>
<br>


#### 使用magic变量访问其它主机的信息

Accessing information about other hosts with magic variables

无论你是否定义变量，你也可以利用特殊的Ansible变量访问有关主机的信息，包括magic, facts, connection变量。magic变量名称被保留，所以不要使用这些名称来设置变量。`enviroment`变量也同样被保留。

最常使用的魔术变量有：`hostvars`, `groups`, `group_names`, `inventory_hostname`。

`host_vars`允许你访问其它主机的变量，包括该主机的facts。你可以在playbook中的任意一点访问主机变量。即使你在playbook中并没有连接到此主机，你仍可以得到变量。
`groups`是清单中所有组的列表。这可以用于枚举组内的所有主机。
`group_names`是所有组中当前主机的列表或数组。
`inventory_hostname`是清单主机文件中配置的主机名。使用`inventory_hostname_short`获取更简短的信息。
`ansible_play_hosts`是当前play中仍然活跃的主机列表。
`ansible_play_batch`是当前批量paly上可用的主机名列表。
`ansible_playbook_python`是python执行调用ansible命令行工具的路径。
`role_path`返回当前角色的路径名。这仅在角色里工作。

```
{{ hostvars['test.example.com']['ansible_facts']['distribution'] }}

{% for host in groups['app_servers'] %}
    {{ hostvars[host]['ansible_facts']['eth0']['ipv4']['address'] }}
{% endfor %}

{% if 'webserver' in group_names %}
    # xxx
{% endif %}
```


<br>
<br>


#### 在文件中定义变量

Defining variables in files

让playbook使用版本控制是很好的想法，但你可能希望让playbook 源公开化，但同时又保证一定的重要的私有变量。

你可以通过一个外部变量文件来这么做：

```yaml
---
- hosts: all
  ...
  vars:
    color: blue
  vars_files:
    - /vars/external_vars.yml
```

```yaml
# external_vars.yml
user: xx
password: xxxx
```

这消除了分享playbook但避免分享数据的风险。


<br>
<br>


#### 在命令行上传递参数

Passing variables on the command line

可在命令行上使用`--extra-vars`参数来设置变量：

```sh
# k:v格式
ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"

# json格式
ansible-playbook arcade.yml --extra-vars '{"pacman":"mrs","ghosts":["inky","pinky","clyde","sue"]}'

# 文件
ansible-playbook release.yml --extra-vars "@some_file.json"
```


<br>
<br>


#### 变量的优先级：我应该把变量放在哪

Variable precedence: Where should I put a variable?

同一名称的变量如果在多个地方被定义，则它们会以特定的顺序发生覆盖，所需需要知道Ansible变量的优先级，以及它们的放置位置。下面是从小到大的优先级：

1. command line values
2. role defaults
3. inventory file or script group vars
4. inventory group_vars/all
5. playbook group_vars/all
6. inventory group_vars/*
7. playbook group_vars/*
8. inventory file or script host vars
9. inventory host_vars/*
10. playbook host_vars/*
11. host facts / cached set_facts
12. play vars
13. play vars_prompt
14. play vars_files
15. role vars (defined in role/vars/main.yml)
16. block vars (only for tasks in block)
17. task vars (only for the task)
18. include_vars
19. set_facts/registered vars
20. role (and include_role) params
21. include params
22. extra vars (always win precedence)



<br>
<br>



### Jinja2模板

Templating (Jinja2): <https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html>

Ansible使用Jinja2模板化来实现动态表达式和访问变量。Ansilbe大大扩展的filters和tests数量，以及新增了一个插件类型：lookups。

请注意，所有模板发生在Ansible控制器上，在任务发送和执行在目标主机之前。


<br>


#### Filters

Filters: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html>


<br>
<br>


#### Tests

Tests: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_tests.html>

Jinja中的测试是评估模板表达式并返回True或False。许多内置测试: <https://jinja.palletsprojects.com/en/2.11.x/templates/#builtin-tests>

测试器和过滤器的主要区别是测试用于比较，而过滤去用于数据操作。测试同样可以在列表处理器中使用，如`map()`和`select()`在列表中选择项。

与所有模板一样，测试始终在Ansible控制器上执行，而不是任务的目标主机。除了这些Jinja2的测试，Ansible支持用户轻松创建自己的测试。


<br>
<br>


#### Lookups

Lookups: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_lookups.html>

查找插件允许访问外部数据源。与所有模板一样，这些插件在Ansible控制器上进行评估，并且可以包括读取文件系统、对外联络网络数据存储和服务。这些数据使用Ansible标准模板系统提供。

<br>

> **注意**
> 查找发生在本地主机，而不是远程主机；
> 它们在包含role或play的目录内执行，而不是与执行脚本的目录执行本地任务；
> 可以传递`wantlist=True`给lookups来使用Jinja2中的for循环；
> 查找是一个高级的功能，你应该对Ansible有足够的了解。


<br>
<br>


#### Python版本和模板

Python Version and Templating: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_python_version.html>

Jinja2模板利用Python数据类型和标准函数。这使得可对数据进行丰富的操作。然而，这也意味着潜在的Python的某些细节对模板编写者可见。由于Ansible playbook使用Jinja2用于模板与变量，这意味着playbook作者需要了解这些细节。

除了这些，请注意在Python2和Python3上运行Ansible的不同。



<br>
<br>



### 条件语句

Conditionals: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html>

经常的一个play的结果可能依赖于一个变量的值，或之前的任务的结果。在某些情况下，变量的值可能依赖于其它变量。本主题介绍如何在playbook中使用条件语句。

<br>


#### When语句

The When Statement

有时你会想在某个特定主机上跳过特定的步骤。

在Ansible中使用`when`子句很容易达到，它不包含Jinja2中的双花括号表达式。它非常简单：

```yaml
tasks:
  - name: "shut down CentOS 6 systems"
    command: /sbin/shutdown -t now
    when:
      - ansible_facts['distribution'] == "CentOS"
      - ansible_facts['distribution_major_version'] == "6"
```

许多Jinja2的测试器和过滤器都可在`when`子句中使用，其中某些是由Ansible单独提供的。

```yaml
tasks:
  - command: /bin/false
    register: result
    ignore_errors: True

  - command: /bin/something
    when: result is failed

  # In older versions of ansible use ``success``, now both are valid but succeeded uses the correct tense.
  - command: /bin/something_else
    when: result is succeeded

  - command: /bin/still/something_else
    when: result is skipped
```

```yaml
tasks:
    - shell: echo "This certainly is epic!"
      when: epic or monumental|bool
```

```yaml
tasks:
    - shell: echo "I've got '{{ foo }}' and am not afraid to use it!"
      when: foo is defined

    - fail: msg="Bailing out. this play requires 'bar'"
      when: bar is undefined
```


<br>
<br>


#### 循环和条件

Loops and Conditionals

`when`和`loops`结合使用，请注意when语句是根据每个项分别处理。

```yaml
tasks:
    - command: echo {{ item }}
      loop: [ 0, 2, 4, 6, 8, 10 ]
      when: item > 5
```


<br>
<br>


#### 在自定义facts中载入

Loading in Custom Facts

如果你想提供自己的facts也很简单。要运行它们，只需要在任务顶部调用你自己定义的模块，这里返回的变量将能访问未来的任务：

```yaml
tasks:
    - name: gather site specific fact data
      action: site_facts
    - command: /usr/bin/thingy
      when: my_custom_fact_just_retrieved_from_the_remote_system == '1234'
```


<br>
<br>


#### Applying when to roles,imports,and includes

在roles, imports, includes中使用when语句：

```yaml
- hosts: webservers
  roles:
     - role: debian_stock_config
       when: ansible_facts['os_family'] == 'Debian'
```


<br>
<br>


#### 有条件的导入

Conditional Imports

一个剧本适用于多个平台和操作系统是很好的栗子。

```yaml
---
- hosts: all
  remote_user: root
  vars_files:
    - "vars/common.yml"
    - [ "vars/{{ ansible_facts['os_family'] }}.yml", "vars/os_defaults.yml" ]
  tasks:
  - name: make sure apache is started
    service: name={{ apache }} state=started
```


<br>
<br>


#### 基于变量来选择文件和模板

Selecting Files And Templates Based On Variables

基于不同的系统来生成不同的配置文件：

```yaml
- name: template a file
  template:
      src: "{{ item }}"
      dest: /etc/myapp/foo.conf
  loop: "{{ query('first_found', { 'files': myfiles, 'paths': mypaths}) }}"
  vars:
    myfiles:
      - "{{ansible_facts['distribution']}}.conf"
      -  default.conf
    mypaths: ['search_location_one/somedir/', '/opt/other_location/somedir/']
```


<br>
<br>


#### 注册变量

Register Variables

存储一个给定命令的结果，以便后面来访问它，在playbook中可能很有用。

> 注意：
> 即使当一个任务由于条件语句跳过，注册也会发生。

`register`关键字决定将结果保存哪个变量。

```yaml
- name: check registered variable for emptiness
  hosts: all

  tasks:

      - name: list contents of directory
        command: ls mydir
        register: contents

      - name: check contents for emptiness
        debug:
          msg: "Directory is empty"
        when: contents.stdout == ""
```


<br>
<br>


#### 常用facts

Commonly Used Facts

```
ansible_facts[‘distribution’]

ansible_facts[‘distribution_major_version’]

ansible_facts[‘os_family’]
```



<br>
<br>



### 循环

Loops: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html>

常见的Ansible循环包括改变多个文件/目录的权限、创建多个用户、重复轮询...Ansible提供了两个关键字来创建循环：

- `loop`
- `with_<lookup>`

<br>

> 注意：
> We added loop in Ansible 2.5. It is not yet a full replacement for `with_<lookup>`, but we recommend it for most use cases.
> We have not deprecated the use of `with_<lookup>`
> We are looking to improve loop syntax


<br>


#### 两者比较

Comparing `loop` and `with_*`

- `with_<lookup>`关键字依赖于Lookup插件，即便`items`也是查找；
- `loop`关键字等于`with_list`，它是简单循环的最佳选择；
- `loop`关键字不接受字符串作为输入；
- 一般来说，任何在Migrating from with_X to loop中使用`with_*`可以更新为使用`loop`；
- 当更改`with_items`为`loop`时请小心，`with_items`执行单级的。你需要在`loop`中使用`flatten(1)`。栗子如下：

```yaml
with_items:
  - 1
  - [2,3]
  - 4

# you would need
loop: "{{ [1, [2,3] ,4] | flatten(1) }}"
```


<br>
<br>


#### 标准循环

Standard loops

<br>

##### 遍历一个简单列表

Iterating over a simple list

```yaml
- name: add several users
  user:
    name: "{{ item }}"
    state: present
    groups: "wheel"
  loop:
     - testuser1
     - testuser2
```

<br>

##### 遍历一个散列列表

Iterating over a list of hashes

```yaml
- name: add several users
  user:
    name: "{{ item.name }}"
    state: present
    groups: "{{ item.groups }}"
  loop:
    - { name: 'testuser1', groups: 'wheel' }
    - { name: 'testuser2', groups: 'root' }
```

<br>

##### 遍历一个字典

Iterating over a dictionary

使用`dict2items`字典过滤器来遍历字典：

```yaml
- name: create a tag dictionary of non-empty tags
  set_fact:
    tags_dict: "{{ (tags_dict|default({}))|combine({item.key: item.value}) }}"
  loop: "{{ tags|dict2items }}"
  vars:
    tags:
      Environment: dev
      Application: payment
      Another: "{{ doesnotexist|default() }}"
  when: item.value != ""
```


<br>
<br>


#### 循环与注册变量

Registering variables with a loop

你可以将循环的输出注册为变量：

```yaml
- shell: "echo {{ item }}"
  loop:
    - "one"
    - "two"
  register: echo
```


<br>
<br>


#### 复杂循环

Complex loops

<br>

##### 遍历嵌套的列表

Iterating over nested lists

你可以使用Jinja2的表达式来遍历复杂的列表：

```yaml
- name: give users access to multiple databases
  mysql_user:
    name: "{{ item[0] }}"
    priv: "{{ item[1] }}.*:ALL"
    append_privs: yes
    password: "foo"
  loop: "{{ ['alice', 'bob'] |product(['clientdb', 'employeedb', 'providerdb'])|list }}"
```

<br>

##### 重试任务直到满足条件

Retrying a task until a condition is met

可以使用`until`关键字来重试任务直到满足特定条件：

```yaml
- shell: /usr/bin/foo
  register: result
  until: result.stdout.find("all systems go") != -1
  retries: 5
  delay: 10
```

<br>

##### 循环清单

Looping over inventory

遍历资产清单：

```yaml
# show all the hosts in the inventory
- debug:
    msg: "{{ item }}"
  loop: "{{ groups['all'] }}"

# show all the hosts in the current play
- debug:
    msg: "{{ item }}"
  loop: "{{ ansible_play_batch }}"


# show all the hosts in the inventory
- debug:
    msg: "{{ item }}"
  loop: "{{ query('inventory_hostnames', 'all') }}"

# show all the hosts matching the pattern, ie all but the group www
- debug:
    msg: "{{ item }}"
  loop: "{{ query('inventory_hostnames', 'all:!www') }}"
```


<br>
<br>


#### query与lookup

`loop`关键字需要一个列表作为输入，但是`lookup`关键字默认返回逗号分隔的值的字符串。

```yaml
# same thing
loop: "{{ query('inventory_hostnames', 'all') }}"

loop: "{{ lookup('inventory_hostnames', 'all', wantlist=True) }}"
```


<br>
<br>


#### 循环控制

Adding controls to loops

`loop_control`关键字让你可以以有效的方式管理自己的循环。

<br>

##### 限制循环输出

Limiting loop output with label

当遍历复杂的数据结构，你的任务的控制台输出可能是巨大的。为了限制显示的输出，在`loop_control`中使用`label`。

```yaml
# 此任务输出仅显示每项的name字段
- name: create servers
  digital_ocean:
    name: "{{ item.name }}"
    state: present
  loop:
    - name: server1
      disks: 3gb
      ram: 15Gb
      network:
        nic01: 100Gb
        nic02: 10Gb
        ...
  loop_control:
    label: "{{ item.name }}"
```

<br>

##### 暂停循环

Pausing within a loop

要控制每个项的执行之间的时间(seconds)，在`loop_control`中使用`pause`。

```yaml
# main.yml
- name: create servers, pause 3s before creating next
  digital_ocean:
    name: "{{ item }}"
    state: present
  loop:
    - server1
    - server2
  loop_control:
    pause: 3
```

<br>

##### 追踪流程

Tracking progress through a loop with `index_var`

要追踪你在循环的位置，在`loop_control`中使用`index_var`。

```yaml
- name: count our fruit
  debug:
    msg: "{{ item }} with index {{ my_idx }}"
  loop:
    - apple
    - banana
    - pear
  loop_control:
    index_var: my_idx
```

<br>

##### inner and outer variable names

Defining inner and outer variable names with `loop_var`

可使用`include_tasks`嵌套两个循环任务。然而，默认情况下Ansible为每个循环`item`设置循环变量。This means the inner, nested loop will overwrite the value of item from the outer loop.你可以在`loop_control`中使用`loop_var`来为每个循环指定变量名。

```yaml
- include_tasks: inner.yml
  loop:
    - 1
    - 2
    - 3
  loop_control:
    loop_var: outer_item

# inner.yml
- debug:
    msg: "outer item={{ outer_item }} inner item={{ item }}"
  loop:
    - a
    - b
    - c
```

<br>

##### 扩展的循环变量

Extended loop variables

在循环控制中使用`extended`选项来获取扩展的循环信息：

```yaml
loop_control:
  extended: yes
```

```
Variable	Description
ansible_loop.allitems	The list of all items in the loop
ansible_loop.index	The current iteration of the loop. (1 indexed)
ansible_loop.index0	The current iteration of the loop. (0 indexed)
ansible_loop.revindex	The number of iterations from the end of the loop (1 indexed)
ansible_loop.revindex0	The number of iterations from the end of the loop (0 indexed)
ansible_loop.first	True if first iteration
ansible_loop.last	True if last iteration
ansible_loop.length	The number of items in the loop
ansible_loop.previtem	The item from the previous iteration of the loop. Undefined during the first iteration.
ansible_loop.nextitem	The item from the following iteration of the loop. Undefined during the last iteration.
```


<br>
<br>


#### 从with_x迁移到loop

Migrating from with_X to loop

从Ansible 2.5开始，执行循环的推荐的方式是使用新的`loop`关键字来取代`with_x`格式的循环。

在许多情况下，`loop`语法是使用过滤器的更好的表达，而不是更复杂的`query`或`lookup`。

<br>

```yaml
# with_list被loop替代
- name: with_list
  debug:
    msg: "{{ item }}"
  with_list:
    - one
    - two

- name: with_list -> loop
  debug:
    msg: "{{ item }}"
  loop:
    - one
    - two
```

<br>

```yaml
# with_items被loop和flatten过滤器替代
- name: with_items
  debug:
    msg: "{{ item }}"
  with_items: "{{ items }}"

- name: with_items -> loop
  debug:
    msg: "{{ item }}"
  loop: "{{ items|flatten(levels=1) }}"
```

<br>

```yaml
# with_indexed_items被loop, flatten, loop_control.index_var替代
- name: with_indexed_items
  debug:
    msg: "{{ item.0 }} - {{ item.1 }}"
  with_indexed_items: "{{ items }}"

- name: with_indexed_items -> loop
  debug:
    msg: "{{ index }} - {{ item }}"
  loop: "{{ items|flatten(levels=1) }}"
  loop_control:
    index_var: index
```

<br>

```yaml
# with_flattened被loop和flatten替代
- name: with_flattened
  debug:
    msg: "{{ item }}"
  with_flattened: "{{ items }}"

- name: with_flattened -> loop
  debug:
    msg: "{{ item }}"
  loop: "{{ items|flatten }}"
```

<br>

```yaml
# with_together被loop和zip替代
- name: with_together
  debug:
    msg: "{{ item.0 }} - {{ item.1 }}"
  with_together:
    - "{{ list_one }}"
    - "{{ list_two }}"

- name: with_together -> loop
  debug:
    msg: "{{ item.0 }} - {{ item.1 }}"
  loop: "{{ list_one|zip(list_two)|list }}"
```

<br>

```yaml
# with_dict可通过loop和 dictsort或dict2items替代
- name: with_dict
  debug:
    msg: "{{ item.key }} - {{ item.value }}"
  with_dict: "{{ dictionary }}"

- name: with_dict -> loop (option 1)
  debug:
    msg: "{{ item.key }} - {{ item.value }}"
  loop: "{{ dictionary|dict2items }}"

- name: with_dict -> loop (option 2)
  debug:
    msg: "{{ item.0 }} - {{ item.1 }}"
  loop: "{{ dictionary|dictsort }}"
```

<br>

```yaml
# with_sequence被loop, range, format替代
- name: with_sequence
  debug:
    msg: "{{ item }}"
  with_sequence: start=0 end=4 stride=2 format=testuser%02x

- name: with_sequence -> loop
  debug:
    msg: "{{ 'testuser%02x' | format(item) }}"
  # range is exclusive of the end point
  loop: "{{ range(0, 4 + 1, 2)|list }}"
```

<br>

```yaml
# with_subelements被loop, subelements替代
- name: with_subelements
  debug:
    msg: "{{ item.0.name }} - {{ item.1 }}"
  with_subelements:
    - "{{ users }}"
    - mysql.hosts

- name: with_subelements -> loop
  debug:
    msg: "{{ item.0.name }} - {{ item.1 }}"
  loop: "{{ users|subelements('mysql.hosts') }}"
```

<br>

```yaml
# with_nested/with_cartesian被loop, product替代
- name: with_nested
  debug:
    msg: "{{ item.0 }} - {{ item.1 }}"
  with_nested:
    - "{{ list_one }}"
    - "{{ list_two }}"

- name: with_nested -> loop
  debug:
    msg: "{{ item.0 }} - {{ item.1 }}"
  loop: "{{ list_one|product(list_two)|list }}"
```

<br>

```yaml
# with_random_choice被random替代
- name: with_random_choice
  debug:
    msg: "{{ item }}"
  with_random_choice: "{{ my_list }}"

- name: with_random_choice -> loop (No loop is needed here)
  debug:
    msg: "{{ my_list|random }}"
  tags: random
```



<br>
<br>



### Block

Blocks: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_blocks.html>

块允许任务的逻辑分组，并在play中错误处理。大多数可以适用于单任务的也可适用于块(block)，这使得它很容易设置数据或常见指令到任务。这并不意味着该指令影响块自身，而是有一个块包围的任务继承。

```yaml
tasks:
   - name: Install, configure, and start Apache
     block:
       - name: install httpd and memcached
         yum:
           name:
           - httpd
           - memcached
           state: present

       - name: apply the foo config template
         template:
           src: templates/src.j2
           dest: /etc/foo.conf
       - name: start service bar and enable it
         service:
           name: bar
           state: started
           enabled: True
     when: ansible_facts['distribution'] == 'CentOS'
     become: true
     become_user: root
     ignore_errors: yes
```

在上面的栗子中，块中3个任务中的每一个附加在when条件后，在任务上下文评估之后都将执行。

块中的任务名在Ansible 2.3时可用。建议在所有任务中使用名称，无论是块还是其它地方。


<br>


#### 块错误处理

Blocks error handling

块同样介绍了类似于大多数编程语言的异常处理的错误处理的方法。块仅处理任务的失败(failed)状态。一个糟糕的任务定义或主机不可达不是rescuable错误。

```yaml
# block error handling example
tasks:
 - name: Handle the error
   block:
     - debug:
         msg: 'I execute normally'
     - name: i force a failure
       command: /bin/false
     - debug:
         msg: 'I never execute, due to the above task failing, :-('
   rescue:
     - debug:
         msg: 'I caught an error, can do stuff here to fix it, :-)'
```

<br>

`always`部分，无论什么任务状态都将会运行。

```yaml
 - name: Always do X
   block:
     - debug:
         msg: 'I execute normally'
     - name: i force a failure
       command: /bin/false
     - debug:
         msg: 'I never execute :-('
   always:
     - debug:
         msg: "This always executes, :-)"
```



<br>
<br>



### 高级的playbook功能

Advanced Playbooks Features: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_special_topics.html>

下面有许多playbook功能不需要每个人都去学习，但可以为特定应用提供有用的功能。浏览这些话题，因为你可能找有一些有用的技巧。


<br>


#### 特权晋升


<br>
<br>


#### 异步操作和轮询

Asynchronous Actions and Polling: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_async.html>

默认情况下，playbook块中的任务，直到任务在每个节点上完成后连接才会断开。这可能不总是可取的，或者你需要运行超过ssh timeout的操作。

<br>

**限时后台操作(Time-limited background operations)**

你可以在后台运行长时间运行的操作，之后再检查它们的状态。例如，异步地在后台执行`long_running_operation`：

```sh
# -B 超时时间， -p 轮询数
ansible all -B 3600 -P 0 -a "/usr/bin/long_running_operation --do-stuff"


# async_status模块 检查状态
ansible web1.example.com -m async_status -a "jid=488359678239.2844"
```

轮询模式是智能的，所以在轮询将在任意机器上开始之前所有的工作都将启动。如果想要所有工作快速开始，请确保使用足够高的`--forks`。在超时之后，远程节点上的进程将会被终止。

通常，你只需在后台长时间运行shell命令或软件更新。后台复制模块不会进行文件传输。

为了避免阻塞或超时问题，你可以使用异步模式来一次运行你的所有任务，并轮询直到它们完成。

异步模式的行为依赖于poll值。

<br>

**Avoid connection timeouts: poll > 0**

当poll是正值，playbook仍然会阻塞任务直到它完成、失败或超时。

要异步启动任务，请指定其最大运行实践和要轮询状态的频率。如果未指定poll，则默认由`DEFAULT_POLL_INTERVAL`设置。

```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: simulate long running op (15 sec), wait for up to 45 sec, poll every 5 sec
    command: /bin/sleep 15
    async: 45
    poll: 5
```

<br>

**Concurrent tasks: poll=0**

当poll为0时，Ansible将启动任务，并立即移动到下一个而不必等待结果。

从序列试图这点是异步编程：任务现在可以同时运行。playbook将结束而不检查异步返回。异步任务将执行根据async的值，直到它们完成、失败或超时。

```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: simulate long running op, allow to run for 45 sec, fire and forget
    command: /bin/sleep 15
    async: 45
    poll: 0
```


<br>
<br>


#### 检查模式

Check Mode: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_checkmode.html>



<br>
<br>


#### Debugger

Playbook Debugger

Ansible包含了debugger作为策略插件的一部分。此调试器允许你调试任务。你可以在任务的上下文中访问所有的调试器的功能，以帮助解决失败的问题。

有多种方式来调用调试器。

<br>

**使用debugger关键字(Using the debugger keyword)**

可在提供`name`属性的块中使用`debugger`关键字，如paly, role, block, task。`debugger`关键字接受下列值：

- always: 总是调用调试器
- never: 绝不调用调试器
- on_failed: 任务失败才调用调试器
- on_skipped: 任务跳过才调用调试器

<br>

全局配置：

```yaml
# on a task
- name: execute a command
  command: false
  debugger: on_failed
```

```yaml
# on a play
- name: play
  hosts: all
  debugger: on_skipped
  tasks:
    - name: Execute a command
      command: true
      when: False
```

在特定层级上：

```yaml
- name: Play
  hosts: all
  debugger: never
  tasks:
    - name: Execute a command
      command: false
      debugger: on_failed
```

<br>

**配置或环境变量(Configuration or environment variable)**

```
# ansible.cfg
[defaults]
enable_task_debugger = True
```

```
# environment variable
ANSIBLE_ENABLE_TASK_DEBUGGER=True ansible-playbook -i hosts site.yml
```

<br>

**策略(As a Strategy)**

要使用debug策略，改变strategy属性：

```yaml
- hosts: test
  strategy: debug
  tasks:
  ...
```

```
# ansible.cfg
[defaults]
strategy = debug
```

```
# environment variable
ANSIBLE_STRATEGY=debug
```

<br>

**可用命令(Available Commands)**

打印值：

```
[192.0.2.10] TASK: install package (debug)> p task
TASK: install package
[192.0.2.10] TASK: install package (debug)> p task.args
{u'name': u'{{ pkg_name }}'}
[192.0.2.10] TASK: install package (debug)> p task_vars
{u'ansible_all_ipv4_addresses': [u'192.0.2.10'],
 u'ansible_architecture': u'x86_64',
 ...
}
[192.0.2.10] TASK: install package (debug)> p task_vars['pkg_name']
u'bash'
[192.0.2.10] TASK: install package (debug)> p host
192.0.2.10
[192.0.2.10] TASK: install package (debug)> p result._result
{'_ansible_no_log': False,
 'changed': False,
 u'failed': True,
 ...
 u'msg': u"No package matching 'not_exist' is available"}
```


<br>
<br>


#### 滚动升级

Delegation, Rolling Updates, and Local Actions: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html>


<br>
<br>


#### 设置环境

Setting the Environment: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_environment.html>

`environment`关键字可以允许你为远程目标主机设置环境变量。例如，需要为http请求设置一个代理。获取其它工具需要的环境变量。

```yaml
- hosts: all
  remote_user: root
  tasks:
    - name: Install cobbler
      package:
        name: cobbler
        state: present
      environment:
        http_proxy: http://proxy.example.com:8080
```

也可以存储在一个变量里：

```yaml
- hosts: all
  remote_user: root
  # here we make a variable named "proxy_env" that is a dictionary
  vars:
    proxy_env:
      http_proxy: http://proxy.example.com:8080
  tasks:
    - name: Install cobbler
      package:
        name: cobbler
        state: present
      environment: "{{ proxy_env }}"
```


<br>
<br>


#### 特定语言版本管理器

Working With Language-Specific Version Managers

一些特定语言版本管理器(如nvm)要求，而这些工具在使用中都要求环境变量。挡手动使用这些工具，通常需要在配置文件中添加一些环境变量，在Ansible中，你可使用enviroment代替：

```
---
### A playbook demonstrating a common npm workflow:
# - Check for package.json in the application directory
# - If package.json exists:
#   * Run npm prune
#   * Run npm install

- hosts: application
  become: false

  vars:
    node_app_dir: /var/local/my_node_app

  environment:
    NVM_DIR: /var/local/nvm
    PATH: /var/local/nvm/versions/node/v4.2.1/bin:{{ ansible_env.PATH }}

  tasks:
  - name: check for package.json
    stat:
      path: '{{ node_app_dir }}/package.json'
    register: packagejson

  - name: npm prune
    command: npm prune
    args:
      chdir: '{{ node_app_dir }}'
    when: packagejson.stat.exists

  - name: npm install
    npm:
      path: '{{ node_app_dir }}'
    when: packagejson.stat.exists
```


<br>
<br>


#### 错误处理

Error Handling In Playbooks: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html>

Ansible通常有默认值来确保检查命令和模块的返回码和是否失败，进行错误处理，除非你做了决定。

<br>

**忽略错误命令(Ignoring Failed Commands)**

一般来说，如果主机上有任务失败，playbook将停止执行。有时，你想让它继续进行：

```yaml
- name: this will not be counted as a failure
  command: /bin/false
  ignore_errors: yes
```

<br>

**重置不可达的主机(Resetting Unreachable Hosts)**

连接失败将设置主机为不可达(UNREACHABLE)，它将从运行活跃主机列表中删除。可以使用`meta: clear_host_errors`来设置。

<br>

**处理程序和失败(Handlers and Failure)**

当主机上的一个任务失败时，先前通知(notify)的处理程序(handler)将不会在此主机上运行。例如，任务更新配置文件并通知处理程序去重启服务。如果任务以后的同一个play失败，则服务不会重启尽管配置已经更改。

可以使用`--force-handlers`命令行选项来改变此行为。或在play中包含`force_handlers: True`，或在ansible配置中包含`force_handlers = True`。当处理程序被强制执行，无论任务成功与否它们都会执行。

<br>

**控制如何定义失败(Controlling What Defines Failure)**

Ansible允许你使用`filed_when`条件来定义失败。栗子：

```yaml
- name: Fail task when the command error output prints FAILED
  command: /usr/bin/example-command -x -y -z
  register: command_result
  failed_when: "'FAILED' in command_result.stderr"
```

<br>

**覆盖改变的结果(Overriding The Changed Result)**

当一个模块运行，它通常会基于机器状态是否被影响而报告`changed`状态。

有时候你知道，基于返回码或返回结果，它并没有发生改变，并希望去覆盖`changed`结果，使它不出现在报告输出里：

```yaml
- command: /bin/fake_command
  register: result
  ignore_errors: True
  changed_when:
    - '"ERROR" in result.stderr'
    - result.rc == 2
```

<br>

**终止play(Aborting the play)**

有时需要终止失败的play，而不是为某主机跳过剩余任务。

`any_errors_fatal`选项将终止Play，并防止任何后续的plays运行。当遇到一个错误，当前批次的所有主机都有机会完成致命的任务，然后play的执行停止。`any_errors_fatal`可在play或block层级设置：

```yaml
- hosts: somehosts
  any_errors_fatal: true
  roles:
    - myrole

- hosts: somehosts
  tasks:
    - block:
        - include_tasks: mytasks.yml
      any_errors_fatal: true
```

<br>

**使用块(blocks)**

大多数可应用到单个任务的也可应用到块，这使得它更容易设置数据或指定到任务。块只处理任务的失败(failed)状态。

```yaml
tasks:
- name: Handle the error
  block:
    - debug:
        msg: 'I execute normally'
    - name: i force a failure
      command: /bin/false
    - debug:
        msg: 'I never execute, due to the above task failing, :-('
  rescue:
    - debug:
        msg: 'I caught an error, can do stuff here to fix it, :-)'
```


<br>
<br>


#### 高级语法

Advanced Syntax: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_advanced_syntax.html>

高级的YANL语法可以给你在Ansible的YAML文件中更多控制数据的地方。你可以在[PyYAML文档](https://pyyaml.org/wiki/PyYAMLDocumentation#YAMLtagsandPythontypes)中找到等多Python特定化的YAML信息。

<br>

**不安全和原生字符串**
Unsafe or Raw Strings

Ansible提供了一个内部的数据类型，用来声明变量不安全(unsafe)。这意味着变量中保存的数据应为不安全，防止字符串被替换和披露。

Jinja2包含了转义，或告诉Jinja2不渲染数据，如`{% raw %}...{% endraw %}`。

```
# 使用 !unsafe 标签
my_unsafe_variable: !unsafe 'this variable has {{ characters that should not be treated as a jinja2 template'
```

```yaml
---
hosts: all
vars:
    my_unsafe_variable: !unsafe 'unsafe value'
tasks:
    ...
```

<br>

**锚和别名**
YAML anchors and aliases: sharing variable values

YAML的锚(anchor)和别名(aliase)可以帮助你在灵活的方式中定义、维护和使用共享变量。使用`&`定义一个锚，使用别名(`*`)指向它。

```yaml
# 锚内设置了3个变量，别名使用其它2个，并覆盖第3个
---
...
vars:
    app1:
        jvm: &jvm_opts
            opts: '-Xms1G -Xmx2G'
            port: 1000
            path: /usr/lib/app1
    app2:
        jvm:
            <<: *jvm_opts
            path: /usr/lib/app2
...
```

path的值由合并操作符(`<<`)所合并。


<br>
<br>


#### 使用插件

Working With Plugins: <https://docs.ansible.com/ansible/latest/plugins/plugins.html>

插件时扩展Ansible的核心功能。Ansible使用插件架构来实现丰富的、灵活的、可扩展的功能集。

Ansible ships附带了许多插件，你也可以自己编写。

- Action Plugins
- Become Plugins
- Cache Plugins
- Callback Plugins
- Cliconf Plugins
- Connection Plugins
- Httpapi Plugins
- Inventory Plugins
- Lookup Plugins
- Netconf Plugins
- Shell Plugins
- Strategy Plugins
- Vars Plugins
- Filters
- Tests
- Plugin Filter Configuration


<br>
<br>


#### 提示和输入

Prompts: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_prompts.html>

当运行playbook时，你可能希望提示某些用户输入信息，可使用`vars_prompt`来完成。一个常见的用途可能是要求输入敏感的数据，但不希望记录。

```yaml
---
- hosts: all
  vars_prompt:
    - name: username
      prompt: "What is your username?"
      private: no
    - name: password
      prompt: "What is your password?"
  tasks:
    - debug:
        msg: 'Logging in as {{ username }}'
```


<br>
<br>


#### 标签

Tags: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html>

如果你有一个大型的playbook，它可能成为一个有用的能够运行playbook的一个特定部分，而不是playbook中的所有。Ansible支持使用标签(tags)来完成。

标签可应用于Ansible的许多结构，但最简单的方法是单个任务。

栗子：

```yaml
tasks:
- yum:
    name:
    - httpd
    - memcached
    state: present
  tags:
  - packages

- template:
    src: templates/src.j2
    dest: /etc/foo.conf
  tags:
  - configuration
```

当你执行playbook，你可以用两种方法基于标签过滤任务：

- 在命令行使用`--tags`或`--skip-tags`选项；
- 在Ansible配置中，使用`TAGS_RUN`或`TAGS_SKIP`选项。

```
# 仅执行某个标签
ansible-playbook example.yml --tags "configuration,packages"

# 跳过某个标签
ansible-playbook example.yml --skip-tags "packages"

# 产看标签执行情况
ansible-playbook example.yml --tags "configuration,packages" --list-tasks
```

<br>

**标签重用**
Tag Reuse
































































<br>
<br>


### 控制playbook执行

Controlling playbook execution: strategies and more: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html>

默认情况下，Ansible在使用5forks任意主机上开始下一个任务之前在所有被play影响的主机上运行每个任务。如果你想要改变此默认的行为，你可以使用不同的策略插件，改变fork数，或应用几个play级别的关键字（如`serial`）。


<br>


#### 选择策略

Selecting a strategy

- linear strategy: <https://docs.ansible.com/ansible/latest/plugins/strategy/linear.html#linear-strategy>
- debug strategy: <https://docs.ansible.com/ansible/latest/plugins/strategy/debug.html#debug-strategy>
- free strategy: <https://docs.ansible.com/ansible/latest/plugins/strategy/free.html#free-strategy>

```yaml
- hosts: all
  strategy: free
  tasks:
  ...
```


<br>
<br>


#### 设置fork数

Setting the number of forks

```
# ansible.cfg
[defaults]
forks = 30


# or cli
ansible-playbook -f 30 my_playbook.ym
```


<br>
<br>


#### 使用关键字控制执行

Using keywords to control execution

[play level](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#playbook-keywords)的关键字会影响paly的执行。

最常见的是`serial`，还有`throttle`, `ignore_errors`, `ignore_unreachable`, `any_errors_fatal`。



<br>
<br>



### 最佳实践

Best Practices: <https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html>

使用Ansible和playbooks的一些技巧。

你可以在[ansible-examples](https://github.com/ansible/ansible-examples)仓库中找到最佳用法。


<br>


#### 内容组织

Content Organization

下面将介绍组织Playbook内容的多种方式。你的ansible的使用应该适合你的需求，因此你可以按需组合各种方法。

组织ansible playbook内容的一个关键方式是role。你应该理解它。


<br>


##### 目录布局

Directory Layout

栗子：

```yaml
staging                   # inventory file for staging environment

group_vars/
   group1.yml             # here we assign variables to particular groups
   group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```


<br>


##### 可选的目录布局

Alternative Directory Layout

此布局为大型环境提供了更多灵活性，栗子：

```yaml
inventories/
   production/
      hosts               # inventory file for production servers
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         hostname1.yml    # here we assign variables to particular systems
         hostname2.yml

   staging/
      hosts               # inventory file for staging environment
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         stagehost1.yml   # here we assign variables to particular systems
         stagehost2.yml

library/
module_utils/
filter_plugins/

site.yml
webservers.yml
dbservers.yml

roles/
    common/
    webtier/
    monitoring/
    fooapp/
```


<br>


##### 使用云动态资产

Use Dynamic Inventory With Clouds

如果你使用云服务提供商，你不应该在静态文件中管理你的资产。请参考[Working with dynamic inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html#intro-dynamic-inventory)


<br>


##### 如何区分测试与生产

How to Differentiate Staging vs Production

如果管理静态清单，经常会问到如何区分不同类型的环境。下面的例子提供了一个好方法。分组的类似方法可以适用动态清单。

```ini
# file: production

[atlanta_webservers]
www-atl-1.example.com
www-atl-2.example.com

[boston_webservers]
www-bos-1.example.com
www-bos-2.example.com

[atlanta_dbservers]
db-atl-1.example.com
db-atl-2.example.com

[boston_dbservers]
db-bos-1.example.com

# webservers in all geos
[webservers:children]
atlanta_webservers
boston_webservers

# dbservers in all geos
[dbservers:children]
atlanta_dbservers
boston_dbservers

# everything in the atlanta geo
[atlanta:children]
atlanta_webservers
atlanta_dbservers

# everything in the boston geo
[boston:children]
boston_webservers
boston_dbservers
```

<br>


##### 组和主机变量

Group And Host Variables

```yaml
---
# file: group_vars/atlanta
ntp: ntp-atlanta.example.com
backup: backup-atlanta.example.com
```

```yaml
---
# file: group_vars/webservers
apacheMaxRequestsPerChild: 3000
apacheMaxClients: 900
```

```yaml
---
# file: group_vars/all
ntp: ntp-boston.example.com
backup: backup-boston.example.com
```


<br>


##### 顶级playbook通过角色分离

Top Level Playbooks Are Separated By Role

```yaml
---
# file: site.yml
- import_playbook: webservers.yml
- import_playbook: dbservers.yml
```

```yaml
---
# file: webservers.yml
- hosts: webservers
  roles:
    - common
    - webtier
```

这里，我们可以选择运行`site.yml`来配置我们的整个基础架构，或者通过运行`webservers.yml`来只运行一个子集。类似于下面：

```yaml
ansible-playbook site.yml --limit webservers
ansible-playbook webservers.yml
```


<br>


##### 角色的任务和处理程序组织

Task And Handler Organization For A Role

下面解释一个NTP任务是如何工作：

```yaml
# file: roles/common/tasks/main.yml

- name: be sure ntp is installed
  yum:
    name: ntp
    state: present
  tags: ntp

- name: be sure ntp is configured
  template:
    src: ntp.conf.j2
    dest: /etc/ntp.conf
  notify:
    - restart ntpd
  tags: ntp

- name: be sure ntpd is running and enabled
  service:
    name: ntpd
    state: started
    enabled: yes
  tags: ntp
```

这是一个处理程序(handler)文件栗子：

```yaml
---
# file: roles/common/handlers/main.yml
- name: restart ntpd
  service:
    name: ntpd
    state: restarted
```


<br>


##### 什么组织启用

What This Organization Enables

```
# 重新配置基础服务
ansible-playbook -i production site.yml


# 重新配置NTP
ansible-playbook -i production site.yml --tags ntp


# 重新配置webservers
ansible-playbook -i production webservers.yml
# boston
ansible-playbook -i production webservers.yml --limit boston
# boston first 10
ansible-playbook -i production webservers.yml --limit boston[0:9]


# ad-hoc
ansible boston -i production -m command -a '/sbin/reboot'
```


<br>


##### 部署于配置组织

Deployment vs Configuration Organization

上面的配置模型是一个典型的配置拓扑。当进行多级部署中，会有一些额外的playbook（hop between tiers to roll out an application）。


<br>
<br>


#### 测试与生产

Staging vs Production

如上所述，让staging(testing)和production环境分离是为不同的环境使用单独的清单文件。你的环境不一定是相同的大小，你可以使用变量来控制它们。


<br>
<br>


#### 滚动更新

Rolling Updates

理解`serial`关键字。


<br>
<br>


#### 注意状态

Always Mention The State

`state`参数对许多模块是可选的。如`state=present`或`state=absent`。


<br>
<br>


#### 通过角色分组

Group By Roles

一个系统可以在多个组。这使得playbook基于角色来选择目标主机。以及使用该组变量系统来分配角色特定的变量。


<br>
<br>


#### 操作系统和发行版本

Operating System and Distribution Variance

当在两个不同的操作系统之间处理一个参数时，处理它的一个好方法是使用`group_by`模块。

```yaml
---
 - name: talk to all hosts just so we can learn about them
   hosts: all
   tasks:
     - name: Classify hosts depending on their OS distribution
       group_by:
         key: os_{{ ansible_facts['distribution'] }}

 # now just on the CentOS hosts...

 - hosts: os_CentOS
   gather_facts: False
   tasks:
     - # tasks that only happen on CentOS go here
```

```yaml
- hosts: all
  tasks:
    - name: Set OS distribution dependent variables
      include_vars: "os_{{ ansible_facts['distribution'] }}.yml"
    - debug:
        var: asdf
```


<br>
<br>


#### 使用playbook捆绑ansible模块

Bundling Ansible Modules With Playbooks

如果playbook有相对于其它YAML文件的`./library`目录，此目录可以用来添加ansible module，它会自动在ansible模块路径。这是一个保持模块与playbook在一起的好方法。


<br>
<br>


#### 空白和注释

Whitespace and Comments

空白和注释有利于文件可读性，值得使用。


<br>
<br>


#### 任务命名

Always Name Tasks

给任务建立一个正在做什么的名称。在运行时，playbook显示此名称。


<br>
<br>


#### 使它简单

Keep It Simple

当你能够简单地做事，那就简单地做。不要为了达到使用所有ansible功能而一起使用它们。使用你需要的。把复杂的事情简单化。


<br>
<br>


#### 版本控制

Version Control

使用版本控制来管理playbook。


<br>
<br>


#### 变量和拱顶

Variables and Vaults

当运行playbook，Ansible在未加密的文件中查找变量，并且所有敏感的变量来自加密文件。

一个最佳实践方法是在组下开始一个`group_vars`子目录。在此子目录内，创建两个名为`vars`和`vault`的文件。`vars`文件内定义所有需要的变量，包括敏感的。接下来，复制所有的敏感变量到`vault`文件或以`vault_`开头的文件。你应该在`vars`文件内使用Jinja2语法调整变量指向匹配的`vault_`文件，并确保`vault`文件是vault encrypted。



<br>
<br>



### 持续交付和滚动更新

Playbook Example: Continuous Delivery and Rolling Upgrades: <https://docs.ansible.com/ansible/latest/user_guide/guide_rolling_upgrade.html>


<br>


#### 什么是持续交付

What is continuous delivery

Continuous delivery(CD)是指经常更新你的软件应用程序。



















<br>
<br>
<br>



## 特权晋升

Understanding privilege escalation: become: <https://docs.ansible.com/ansible/latest/user_guide/become.html>

Ansible使用现有的权限升级系统来执行具有root或其它权限的任务。此功能允许你成为(`become`)其它用户，与登录到远程机器不同，我们称之为`become`。become关键字利用现有的权限提升工具（如`sudo`, `su`, `pfexec`, `doas`, `pbrun`, `dzdo`, `ksu`, `runas`）。


<br>


### 使用

你可以在任务、连接变量、命令行等控制`become`的使用。如果你以多种方式设置了特权提升，请注意优先级。

所有become plugins完整的列表: <https://docs.ansible.com/ansible/latest/plugins/become.html#become-plugin-list>


<br>


**become**

你可在play或task层设置`become`指令。你可以设置连接变量，从不同主机之间覆盖它们。

```yaml
# 激活特权提升
become: yes
# 默认root
become_user: xxx
# 参考become plugins，可在ansible.cfg中配置。默认sudo
become_method: sudo
# 为role或task执行特定标志
become_flags: xxx
```

栗子：

```
- name: Ensure the httpd is running
  become: yes
  service:
    name: httpd
    state: started

- name: Run a command as the apache user
  command: somecommand
  become: yes
  become_user: apache

- name: Run a command as nobody
  command: somecommand
  become: yes
  become_method: su
  become_user: nobody
  become_flags: '-s /bin/sh'
```

<br>

**连接变量**

Become connection variables

你可以定义不同的选型来管理node或group。你可以在资产中定义这些变量，或将其作为正常的变量使用。

```
ansible_become
ansible_become_method
ansible_become_user
ansible_become_password

# 栗子
webserver ansible_user=manager ansible_become=yes
```

<br>

**命令行选项**

```
--ask-become-pass, -K
--become, -b
--become-method=BECOME_METHOD
--become-user=BECOME_USER
```


<br>
<br>


### 风险和局限性

Risks and limitations of become

虽然权限提升是很直观的，但它如何工作也有一些限制。用户应该知道这些，以避免意外。

<br>

**成为一个非特权用户的风险**
Risks of becoming an unprivileged user

Ansible模块由第一个参数带入模块文件，然后将其复制到远程主机，最后在远程机器上执行它。

如果模块文件不使用`become`，当`become_ueer`为root时，或当远程机器被设置为root时，一切都好。在这些情况下，Ansible创建具有只允许由所述用户和root读取，或只允许由所述非特权用户切换到读取权限模块文件。

然而，当连接用户和`become_user`都不是特权用户，模块文件被写入需要由Ansible设置为用户可读。在这种情况下，Ansible使得模块文件世界可读的Ansible模块执行的持续时间。一旦模块执行完毕，Ansible删除临时文件。

<br>

**不是所有连接插件都支持**
Not supported by all connection plugins

特权升级方法也必须由连接使用的插件支持。

<br>

**每个主机只能启用一个方法**
Only one method may be enabled per host

<br>

**特权提升必须通用**
Privilege escalation must be general

你不能限制权限提升某些命令的权限。



<br>
<br>
<br>



## Vault

Ansible Vault: <https://docs.ansible.com/ansible/latest/user_guide/vault.html>

Ansible Vault是Ansible的一个功能，可以让你在加密的文件中保存敏感数据（如密码、密钥），而不是像普通文本或playbooks或roles中。这些vault文件可以分布或放置在版本控制中。

要启用此功能，使用命令行选型`-ansible-vault`，和`--vault-password-file`。






<br>
<br>
<br>



## Modules

Ansible Modules: <https://docs.ansible.com/ansible/latest/user_guide/modules.html>

Ansible包含了大量的模块(module library)，可以直接在远程主机或通过playbook执行。

用户也可以编写自己的模块。这些模块可以控制系统资源（服务、包、文件...），或执行系统命令。


<br>
<br>


### 模块介绍

Introduction to modules: <https://docs.ansible.com/ansible/latest/user_guide/modules_intro.html>

```sh
# adhoc
ansible webservers -m service -a "name=httpd state=started"
```

```yaml
# playbook
- name: restart webserver
  service:
    name: httpd
    state: restarted
```



<br>
<br>



### 返回值

Return Values: <https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html>

Ansible模块通常正常返回一个可以注册为一个变量的数据结构，或直接看到由ansible程序输出。每个模块都可选的记录自己唯一的返回值。

本章节包含的返回值适用于所有模块。

<br>

- Common
  - backup_file
  - changed
  - failed
  - invocation
  - msg
  - rc
  - results
  - skipped
  - stderr
  - stderr_lines
  - stdout
  - stdout_lines
- Internal use
  - ansible_facts
  - exception
  - warning
  - deprecations



<br>
<br>



### 模块索引

Module Index: <https://docs.ansible.com/ansible/latest/modules/modules_by_category.html>





<br>
<br>
<br>





















## 插件

Working With Plugins: <https://docs.ansible.com/ansible/latest/plugins/plugins.html>

插件是一段代码，可以扩充Ansible的核心功能。Ansible使用插件架构，以实现丰富的、灵活的、可扩展的功能集。

Ansible附带了一些方便的插件，你也可以很容易地编写自己的插件。





<br>
<br>
<br>




## collections

collections: <https://docs.ansible.com/ansible/latest/user_guide/collections_using.html>

Collections是Ansible的内容分发格式，可以包括playbooks, roles, modules, plugins。你可以通过Ansible Galaxy安装和使用collections。


































<br>
<br>

---

<br>
<br>

























# 开发指南

Developer Guide: <https://docs.ansible.com/ansible/latest/dev_guide/index.html>



































<br>
<br>

---

<br>
<br>
























# Ansible Galaxy

Ansible Galaxy: <https://docs.ansible.com/ansible/latest/galaxy/user_guide.html>

Ansible Galaxy是一个查找、分享、下载社区开发的roles的网站。


















