# ZooKeeper



参考:

- 维基百科
- ZooKeeper: <https://zookeeper.apache.org/>
- Docs: <https://zookeeper.apache.org/doc/>


环境:

- RHEL7x86_64
- ZooKeeper v3.5










<br/>
<br/>

---

<!--more-->

<br/>
<br/>













# 介绍


![](/images/ZooKeeper/zookeeper_logo.jpg)

<br>

ZooKeeper: Because Coordinating Distributed Systems is a Zoo.

**Apache ZooKeeper** 是Apache软件基金会的一个软件项目，它为大型分布式计算提供开源的分布式配置服务、同步服务和命名注册，实现高度可靠的分布式协调。ZooKeeper曾经是Hadoop的一个子项目，但现在是一个独立的顶级项目。

ZooKeeper 是一种集中式服务，用于维护**配置信息**(conf info)，**命名**(naming)，**分布式同步**(distributed synchronization)，**组服务**(group service)。所有这些类型的服务都以分布式应用程序的某种形式应用。每次实施它们都需要做很多工作来修复不可避免的错误和竞争条件。由于难以实现这些类型的服务，应用程序最初通常会吝啬它们，这使得它们在变化的情况下变得脆弱并且难以管理。即使正确完成，这些服务的不同实现也会在部署应用程序时导致管理复杂性。

**ZooKeeper的架构通过冗余服务实现高可用性**。因此，如果第一次无应答，客户端就可以询问另一台ZooKeeper主机。ZooKeeper节点将它们的数据存储于一个分层的命名空间，非常类似于一个文件系统或一个前缀树结构。客户端可以在节点读写，从而以这种方式拥有一个共享的配置服务。更新是全序的。



<br/>


## 概述

[ZooKeeper: A Distributed Coordination Service for Distributed Applications](https://zookeeper.apache.org/doc/r3.5.4-beta/zookeeperOver.html)


ZooKeeper 是一种用于分布式应用程序的分布式开源协调(coordination)服务。它被设计为易于编程，并使用在熟悉的文件系统目录树结构之后设计的数据模型。它在Java中运行，并具有Java和C的绑定。

众所周知，协调服务很难做到。他们特别容易出现竞赛条件(race conditions)和死锁(deadlock)。ZooKeeper背后的动机是减轻分布式应用程序从头开始实施协调服务的责任。


<br/>


### 设计目标

Design Goals


- **ZooKeeper is simple**

ZooKeeper允许分布式进程通过 **共享的层级命名空间**(shared hierarchal namespace) 相互协调，该命名空间的组织方式与标准文件系统类似。命名空间由 **数据寄存器**(data registers) 组成——在ZooKeeper用语中被称为 `znodes`，这些与文件和目录类似。与专为存储而设计的典型文件系统不同，ZooKeeper数据保存在内存中，这意味着ZooKeeper可以实现高吞吐量(high throughput)和低延迟数(latency numbers)。

ZooKeeper的实现非常重视 **高性能(high performance)**， **高可用(highly available)**， **严格有序的访问(strictly ordered access)**。性能方面意味着它可以在大型分布式系统中使用。可靠性方面使其不会成为单点故障(a single point of failure)。严格的排序意味着可以在客户端实现复杂的同步原语。

<br>

- **ZooKeeper is replicated**

与它协调的分布式进程一样，ZooKeeper本身也可以在称为 **集合(ensemble)** 的一组主机上进行 副本复制(replicated)。

![](/images/ZooKeeper/zkservice.jpg)

组成ZooKeeper服务的Server必须了解彼此。它们维护一个内存中的状态镜像，以及持久性存储的事务日志和快照。只要大多数Servers可用，ZooKeeper服务就可用。

Client连接到单个Server。Client维护TCP连接，通过该连接发送请求，获取响应，获取监视事件(watch events)，以及发送心跳(heart beats)。如果与Server的TCP连接中断，则Client将连接到其它Server。

<br>

- **ZooKeeper is ordered**

ZooKeeper使用反映所有ZooKeeper事务顺序的数字标记每个更新。后续操作可以使用该顺序来实现更高级别的抽象，例如同步原语。

<br>

- **ZooKeeper is fast**

它在读取 `read-doninant` 工作负载中特别快。ZooKeeper应用程序运行在成千上万的计算机上，并且在读取别写入更常见的情况下(比率`10:1`)表现最佳。



<br/>
<br/>



### 数据模型和分层命名空间

Data model and the hierarchical namespace


ZooKeeper提供的命名空间非常类似于标准文件系统。名称是由斜杠(`/`)分隔的路径元素序列。ZooKeeper命名空间中的每个节点都由路径标识。

![](/images/ZooKeeper/zknamespace.jpg)



<br/>
<br/>



### 节点和短暂节点

Nodes and ephemeral nodes


与标准文件系统不同，ZooKeeper命名空间中的每个节点都可包含与之关联的数据以及孩子。这就像拥有一个允许文件也是目录的文件系统。ZooKeeper旨在存储协调数据：状态信息，配置，位置信息等，因此存储在每个节点的数据通常很小。我们使用术语 **znode** 来表明我们正在谈论的ZooKeeper数据节点。

Znodes 维护一个 **状态结构(stat structure)**，其中包括数据更改、ACL更改、时间戳更改，以允许缓存验证和协调更新。每次znode的数据更改时，版本号都会增加。例如，每当Client检索数据时，它也接收数据的版本。

存储在每个znode命名空间中的数据以原子(atomically)方式进行读写。读取与znode关联的所有数据字节，写入替换所有的数据。每个节点都有一个ACL限制谁可以做什么。

ZooKeeper也有 **短暂节点(ephemeral nodes)** 的概念。只要创建的znode处于活动状态，就会存在这些znode，回话结束时，znode将被删除。当你想要实现 `[tbd]` 时，短暂节点很有用。



<br/>
<br/>



### 协调更新和监视

Conditional updates and watches


ZooKeeper支持监视(watch)的概念。Client可以在znode上设置监视。当znode更改时，将触发并删除监视。触发监视时，Client会受到一个数据包，指出znode已更改。如果Client与其中一个ZooKeeper Server之间的连接中断，则Client将收到本地通知。这可以用于 `[tbd]` 。



<br/>
<br/>



### 保证

Guarantees


ZooKeeper非常快速和简单。但是，由于基于目标是构建更复杂的服务(如同步)的基础，因此它提供了一系列保证。这些是:

- **顺序一致性(Sequential Consistency)**: Client的更新将按发送顺序来应用
- **原子性(Atomicity)**: 更新成功或失败，没有其它结果
- **单系统镜像(Single System Image)**: 无论连接到哪个Server，Client都将看到相同的服务视图
- **可靠性(Reliability)**: 一旦更新被应用，它将从该时间开始持续，知道Client覆盖此更新
- **时宜性(Timeliness)**: 系统的Client视图保证在特定的时间范围内是最新的



<br/>
<br/>



### API


ZooKeeper的设计目标之一是提供非常简单的编程接口。因此，它仅支持以下操作:

- create: creates a node at a location in the tree
- delete: deletes a node
- exists: tests if a node exists at a location
- get data: reads the data from a node
- set data: writes data to a node
- get children: retrieves a list of children of a node
- sync: waits for data to be propagated



<br/>
<br/>



### 执行

Implementation


ZooKeeper组件显示了ZooKeeper服务的高级组件。除了请求处理器，构成ZooKeeper服务的每个Server都复制自己每个组件的副本。

![](/images/ZooKeeper/zkcomponents.jpg)

- 副本数据库是一个包含整个数据树的内存数据库。更新将记录到磁盘以获得可恢复性，并且在写入内存数据库之前会序列化的磁盘
- 每个ZooKeeper Server都为Client服务。Client只连接到一台Server以提交请求。读取请求由每个Server数据库的本地副本提供。更改服务状态的请求，写请求由 协定协议(agreement protocol) 处理
- 作为协定协议的一部分，来自Client的所有写入请求都被转发到称为 **leader** 的单个Server。其余的ZooKeeper Server，称为**follower**，接收来自**leader**的消息提议并同意消息传递。消息传递层负责替换失败的leader，并将follower与leader同步
- ZooKeeper使用自定义的原子消息(atomic messaging)协议。由于消息传递层是原子的，因此ZooKeeper可以保证本地副本永远不会发散。当leader收到写入请求时，它会计算应用写入时系统的状态，并将其转换为捕获此新状态的事务。



<br/>
<br/>



### 用户


ZooKeeper的编程接口非常简单。但是，通过它，您可以实现更高阶的操作，例如同步原语，组成员身份，所有权等。



<br/>
<br/>



### 性能

Performance


ZooKeeper旨在提供高性能。在读取数量超过写入的应用程序中，它的性能尤其高，因为写入涉及同步所有Server的状态。

![](/images/ZooKeeper/zkperfRW.jpg)

The events marked in the figure are the following:

- Failure and recovery of a follower
- Failure and recovery of a different follower
- Failure of the leader
- Failure and recovery of two followers
- Failure of another leader




<br/>
<br/>
<br/>




## 入门

[ZooKeeper Getting Started Guide](https://zookeeper.apache.org/doc/r3.5.4-beta/zookeeperStarted.html)
















































