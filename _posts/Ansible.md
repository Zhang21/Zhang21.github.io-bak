---
title: Ansible
date: 2017-12-26 17:40:11
categories: DevOps
tags:
  - Ansible
  - Automation
  - DevOps
---


参考:

- Wikipedia
- Ansible docs: <https://docs.ansible.com>

<br>

环境:

- RHELx86_64
- Ansible v2.7




<br/>
<br/>

---

<!--more-->

<br/>
<br/>






# 介绍


Ansible是一种通用语言，揭开了工作如何完成的神秘面纱。将棘手的任务变成可重复的剧本。只需按一下按钮即可推出企业级协议。为您的团队提供自动化，解决和共享的工具。

- **Automate**
- **Accelerate**
- **Collaborate**
- **Integrate**









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
Ansible中的每个`play`都将一系列任务映射到一组系统。
每个`play`中的`hosts:`指令通常称为主机说明符。
它可以选择一个或多个系统，一个或多个组，甚至一个组中的一些主机，而不是另一个组中的主机。

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



































