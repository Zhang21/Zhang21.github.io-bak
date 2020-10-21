---

title: Consul
date: 2018-04-05 22:08:11
categories: Linux
tags: Consul

---


参考：

- <https://www.consul.io/intro/index.html>
- <https://www.consul.io/docs/>
- Consul Template: <https://www.hashicorp.com/blog/introducing-consul-template>

环境：

- CentOS7x86_64
- Consul v1.2.0


<br>
<br/>

<!--more-->

---

<br/>



# 简介


介绍consul是什么，它可以解决哪些问题，以及如何开始使用它。


<br>


## Consul是什么


Consule有多个组件，但总体而言，它是发现(discovery)和配置(config)基础架构(infrastructure)服务的工具。它提供几个关键特点：

- 服务发现(service discovery)
	+ Consul客户端可提供一个服务，如API或mysql，其它客户端能够使用Consul来发现给定服务的提供者。使用DNS或HTTP，应用程序可以轻松找到他们所依赖的服务
- 健康检查(health checking)
	+ Consul可以提供任何数量的健康检查，既可以与给定服务相关联(webserver return 200)，也可与本地节点(内存使用率小于90%)相关联。操作人员可用此信息来监视集群运行状况，服务发现组件使用此信息将流量(traffic)从不健康的主机中引导出去
- KV store
	+ 应用程序可将Consul的分层Key/Value用于存储任何目的，包括动态配置(dynamic configuration)、功能标记(feature flagging)、协调(coordination)、领导选举(leader election)...简单的HTTP API使其易于使用
- 多数据中心(Multi Datacenter)
	+ Consul支持多数据中心，这意味着Consul的用户不必担心构建额外的抽象层以扩展到多个区域

Consul旨在与DevOps和应用程序开发者保持友好，使其成为现代化 ，弹性基础架构的完美选择。


<br/>
<br/>


## Consul用例


- 服务发现(service )
服务注册，集成健康检查，使用DNS或HTTP接口使得任何服务都能被其它服务发现。

- 服务分割(service segmentation)
通过自动TLS加密和基于身份的授权实现安全的服务到服务通信。

- 服务配置(service configuration)
功能丰富的 key/value 可轻易配置服务。



<br>
<br/>



## Consul基础架构


Consul是一个分布式、高可用的系统。

每一个向Consul提供服务的节点都运行一个Consul agent。运行agent对于服务发现或get/set Key/Value不是必需的。agent负责健康检查节点上的服务和节点自身。

agent可与一个或多个Consul server交流。Consul server是数据存储和复制集所在之地。server之间选出一个leader。虽然Consul可以使用一台服务器，但推荐使用3-5台以避免数据丢失的故障情况。对每一个数据中心都推荐使用Consul server cluster。

需要发现其它服务或节点的基础架构组件 可以查询任何Consul server或Consul agent。agent自动将查询发送到server。

每个数据中心运行一组consul server cluster。当发生cross-datacenter服务发现或配置请求时，本地consul server将请求转发给远程数据中心并返回结果。



<br>
<br/>

---

<br>




# 快速开始



## 安装Consul


- 二进制包: <https://www.consul.io/downloads.html>
	+ 解压缩，得到一个consul二进制可执行文件，可将其放入系统路径
- 验证安装: `consul`



<br>
<br/>



## 运行consul-agent


安装consul后请务必运行agent，agent可运行在server或client模式。每个datacenter必须至少有一台server，推荐3-5台做一个集群。单一server部署非常不安全，在故障情况下数据丢失就不可避免了。

所有其它agents都以client模式运行。client是一个非常轻量化的进程——它注册服务、运行健康检查、转发查询给server。agent必须运行在集群的每个节点上。



<br/>


### 启动agent


测试consul development模式，不建议在生产环境使用此方法，此处做测试。

```
consul agent -dev

netstat -nltp

#可根据日志看出agent已成为server，并成为集群leader
```



<br/>


### Consul成员


`members`命令基于`gossip protocol`并最终保持一致。

```
consul members

#节点名称、监听地址、健康状态、集群角色、版本信息
Node     Address         Status  Type    Build  Protocol  DC   Segment
zhang22  127.0.0.1:8301  alive   server  1.0.6  2         dc1  <all>


#使用HTTP API将请求转发给server以获取一致的view of world
culr localhost:8500/v1/catalog/nodes


#DNS interface也可以查询节点，默认端口8600
dig @127.0.0.1 -p 8600 zhang22.node.consul

```



<br/>


### 停止agent


可使用`Ctrl + C`优雅地终止agent，你可以看到它离开集群并关闭。

优雅关闭，Consul会通知集群其它节点此节点的离开。如果你强制kill agent，则集群的其它节点将检测该节点失败。
当成员离开时，其服务和健康检查将从catalog中移除。当成员失败时，其健康状态被标记为critical，但不会从catalog中移除。
Consul会自动尝试重连失败的节点，允许它从当前网络条件中修复，知道离开的节点不在联系。

此外，如果agent正作为server在运行，那么优雅地离开对避免造成严重的影响有帮助。



<br>
<br/>



## 注册服务


注册(register)服务并查询(query)服务。


<br/>


### 定义一个服务


服务可以通过以下两种方法注册：

- 服务定义(service definition)
- 调用HTTP API

服务定义是注册服务最常见的方式，我们将构建前面agent的配置。

```
#创建一个consul配置目录
mkdir /etc/consul.d


#编写服务定义配置文件
#假设有一个web服务运行在80端口，添加一个便于query的tag
echo '{"service": {"name: "web", "tag": ["rails"], "port": 80 }}' | tee /etc/consul.d/web.json


#重启agent，指定配置目录
consul agent -dev -config-dir=/etc/consul.d
```

如果你想注册多个服务，你可以在配置目录下创建多个服务定义文件。



<br/>


### 查询服务


一旦agent启动并且服务已同步，我们可通过HTTP API或DNS查询(query)服务。

<br>

**DNS API**
使用DNS API(默认8600)查询服务

```
#DNS name(默认) -- NAME.service.consul

#只有IP
dig @127.0.0.1 -p 8600 web.service.consul


#返回IP/Port
dig @127.0.0.1 -p 8600 web.service.consul SRV
```

我们还可以用DNS API按tag来过滤service。基于标签的查询格式为`tag.name.service.consul`。

```
dig @127.0.0.1 -p 8600 rails.web.service.consul
```


<br/>


**HTTP API**
除了DNS API，HTTP API(默认8500)同样可用于查询服务。

```
#前面定义了web这个service
curl http://localhost:8500/v1/catalog/service/web
```

catalog API提供了给定服务的所有节点。

```
#仅仅健康实例的查询
 curl 'http://localhost:8500/v1/health/service/web?passing'
```


<br>


### 更新服务


服务定义可以通过更改配置文件并向agent发送SIGHUP来更新。这使得更新服务不会出现任何停机或查询服务不可达的情况。

另外，HTTP API能够用来动态地添加、移除、修改服务。



<br>
<br/>


## Consul集群


具有多个成员的consul集群。

当consul节点启动时，它不知道任何其它节点，它是一个孤立的集群。为了了解到集群中的其它成员，agent必须要加入一个存在的集群。要加入一个现有的集群，只需知道一个现有成员。当加入集群后，agent将于其此成员闲聊，并迅速发现集群中的其它成员。一个agent可以加入任何其它agent，而不仅仅是server模式的agent。


<br>


### 启动agents


```
#node1
consul agent -server -bootstrap-expect=1 \
    -data-dir=/tmp/consul -node=agent-one -bind=ip1 \
    -enable-script-checks=true -config-dir=/etc/consul.d

#node2
consul agent -data-dir=/tmp/consul -node=agent-two \
    -bind=ip2 -enable-script-checks=true -config-dir=/etc/consul.d


#两个独立的node

#现在，我们有两个agent在运行中：一个server，一个client。但是他们两者并不知道对方，并仍然是一个单一节点的集群。
#查看节点
consul member
```


<br>
<br/>


## 加入集群


由于我们在启动agent的时候便已指定server，所以从哪个节点加入都一样。

```
consul join ip
#Successfully joined cluster by contacting 1 nodes.

consul members

Node       Address              Status  Type    Build  Protocol  DC   Segment
agent-one  172.16.129.141:8301  alive   server  1.0.6  2         dc1  <all>
agent-two  172.16.129.150:8301  alive   client  1.0.6  2         dc1  <default>

```

<br>

### 在启动时自动加入集群


理想情况下，每当一个新节点出现在数据中心时，它应该自动加入集群而不需要人工干预。


<br>
<br/>


### 查询节点


就像查询服务，consul有一个API用于查询节点。

```
#NAME.node.consul或NAME.node.DATACENTER.conosul
dig @localhost -p 8600 agent-one.node.consul
dig @127.0.0.1 -p 8600 agent-two.node.consul

```


<br>


### 离开集群


- 优雅的退出: `Ctrl+C`
- 强制`kill`



<br>
<br/>


## 健康检查


对节点和服务添加健康检查(health check)。
健康检查是服务发现的关键组件，可以防止使用不健康的服务。


<br>


### 定义检查


与服务类似，一个检查能够通过定义检查或适当调用HTTP API来两种方式来注册。

定义**检查**是一个最基本和推荐的方法。

在consul配置目录中创建检查定义文件：

```
#在基于脚本的健康检查上，它与consul进程使用同样的用户
#如果命令以非0状态码退出，则该节点会被标记为unhealthy

echo '{"check": {"name": "ping", "args": ["ping", "-c1", "baidu.com"], "interval": "30s"}}' >/etc/consul.d/ping.json

echo '{"service": {"name": "web", "tags": ["rails"], "port": 80, "check": {"args": ["curl", "localhost"], "interval": "10s"}}}' >/etc/consul.d/web.json


consul reload
```


<br>


## 检查健康状态


```
curl http://localhost:8500/v1/health/state/critical

dig @127.0.0.1 -p 8600 web.service.consul
```


<br>
<br/>


## KV数据


Consul提供了一个易于使用的KV存储。这可以用来保存动态配置，协助服务协调，构建leader选举，并启用开发人员可以考虑构建的任何其它内容。


<br>


### 用法


有两种方法与Consul K/V交互的方式：

- HTTP API
- Consul KV CLI

```
#CLI
consul kv --help

consul kv put name zhang
consul kv get name
#zhang

consul kv get -detailed name

consul kv puut -flags=42 who zhang21
#所有key都支持设置一个64位的整数标志值


#列出所有kv
consul kv get -recurse


#删除
consul kv delete name


#使用 Check-And-Set 进行原子更新
consul kv put -cas -modify-index=112 NAME zhang



#导出与导入
consul kv export > xxx.json

consul kv import $xxx.json
```


<br>
<br/>


## Web界面


Consul支持美观的Web界面。用户界面可以查看所有的服务和节点，查看所有健康检查和当前状态，读取和设置kv数据，并自动支持多数据中心。

```
consul agent -ui

#localhost:8500/ui
```




<br/>
<br/>

---

<br/>



# 内部详情

Consul Internals

介绍Consul内部详情。


<br/>


## 架构

Architecture


<br/>


### 词汇表

Glossary

- Agent
- Client
- Server
- Datacenter
- Consensus
- Gossip
	+ LAN Geossip
	+ WAN Geossip
- RPC

<br>

![Consul](/images/consul-arch-420ce04a.png)



<br/>
<br/>



## Consensus协议


Consul使用consensus(共识) protocol来提供一致性(consistency)，它基于**Raft**(In search of an Understandable Consensus Algorithm)


<br>


### Raft协议


Raft是基于Paxos的共识算法。

Raft的一些关键术语：

- Log
The primary unit of work in a Raft system is a log entry.

- FSM(Finite State Machine)
An FSM(有限状态机) is a collection of finite states with transitions between them.

- Peer set
The peer set(对等集) is the set of all members participating in log replication.

- Quorum
A quorum(仲裁) is a majority of members from a peer set: for a set of size n, quorum requires at least (n/2)+1 members. 

- Committed Entry
An entry is considered committed when it is durably stored on a quorum of nodes.

- Leader
At any given time, the peer set elects a single node to be the leader.

<br>

Raft节点总是处于如下三种状态之一：

- follower(追随者)
- candidate(候选者)
- leader(领导者)

所有节点最初都是作为follower开始的。在这种状态下，节点可接受leader的日志条目并投票。如果一段时间内没有收到任何条目，则节点会自我提升到candidate。
在candidate状态下，节点请求来自对等节点的投票。如果候选人获得仲裁(quorum)的票数，那么它将被提升为leader。
leader必须接受新的日志条目并复制给其它所有follower。另外，如果陈旧读取不可接受，则所有查询也必须在leader上执行。

一旦集群具有leader，它就能够接受新的日志条目。Client可以请求leader添加新的日志条目。然后，leader将条目持久化，并尝试复制到仲裁的follower。一旦日志条目被认为提交(committed)，它就可以应用于有限状态机(FSM)。
显然，允许复制日志以无限制的方式增长是不可取的。Raft提供了一种机制，可通过快照(snapshot)当前状态并压缩日志。
达成共识是容错的，直到法定人数可用。
建议为每个数据中心配置3-5台Consul Server。3个节点的Raft集群可以容忍单个节点故障，5个节点的Raft集群可以容忍2个节点故障。这可最大限制提高可用性。


<br>


### Raft in Consul


只有Consul Server节点参与Raft，并且是对等集的一部分。所有的Client节点都将请求转发给Server。

当启动的时候，单个Consul Server进入**bootstrap**模式，此模式允许它进行自我选举为leader。leader选出后，可以以一致性和安全性的方式将其它Server添加到对等集，之后，就可以禁用bootstrap模式。
由于所有的Server作为对等集的一部分参与，因此他们都知道当前的leader。当一个RPC请求到达了non-leader Server时，请求被转发给leader。

- 如果RPC是查询(query)类型，意味着它是只读的，则leader根据FSM的当前状态生成结果
- 如果RPC是事务(transaction)类型，意味着它是可修改的，则leader生成新的日志条目并使用Raft应用它

提交日志条目并将其应用于FSM后，事务就完成了。

由于Raft副本的性质，性能对网络延迟很敏感。因此，每个数据中心选择一个独立的leader并维护一个不相交的对等集。数据由数据中心分区，每个leader仅负责其数据中心中的数据。


<br>


### 一致性模式

Consistency Modes

虽然对副本日志的所有写入都通过Raft，但读取却更加灵活。
Consul支持3种不同的读取一致性模式：

- default
- consistent
- stale


<br>


### 部署表

| Servers | Quorum Size | Failere Tolerance
| - | - | -
| 1 | 1 | 0
| 2 | 2 | 0
| 3 | 2 | 1
| 4 | 3 | 1
| 5 | 3 | 2
| 6 | 4 | 2
| 7 | 4 | 3



<br/>
<br/>



## Gossip协议


Consul 使用gossip协议来管理成员并向集群发送广播信息。所有这些都通过Serf Library提供。


<br/>


### Goossip in Consul

Consul使用两个不同的gossip pools:

- LAN pool
- WAN pool



<br/>
<br/>



## 网络坐标

Network Coordinates

Consul使用网络层层析系统来计算集群中节点的网络坐标。这些坐标允许使用非常简单的计算在任意两个节点之间估计网络往返时间。所有这些都通过使用Serf Library。


<br/>


### Consul中的网络坐标

Network Coordinates in Consul

网络坐标在Consul中有多种表现方式：

- `consul rtt`
- Catalog/Health endpoints
- Prepared query
- Coordinate endpoint


<br/>


### 使用坐标

一旦你有了两个节点的坐标，则计算它们间的往返时间是很简单的：

```
    "Coord": {
        "Adjustment": 0.1,
        "Error": 1.5,
        "Height": 0.02,
        "Vec": [0.34,0.68,0.003,0.01,0.05,0.1,0.34,0.06]
    }
```



<br/>
<br/>



## 会话

Sessions

consul提供了一个用于构建分布式锁的会话机制。会话充当节点、健康检查和K/V数据之间的监听层。


<br/>


### 会话设计









































<br/>
<br/>

---

<br/>




# Agent


## 启动和停止


Consul Agent是Consul的核心进程。它维护成员关系信息，注册服务，运行检查，响应查询...
Consul Agent必须运行在在Consul集群的每个节点上。


Agent有两种运行模式：

- server
- client

Server节点承担了作为**consensus quorum(共识法人)**的额外责任，这些节点参与Raft，并在出现故障时提供强大的一致性和可用性。
Client节点构成了集群的大部分，它们非常轻便。因为它们与Server进行大部分操作，保持自己的状态则很少。

<br>

**运行Agent**
以下是一些重要信息：

- Node name
- Datacenter
- Server
- Client addr
- Cluster addr

```sh
#直接指定配置项运行
consul agent -options


#将配置项写入文件，指定配置目录运行
mkdir /etc/consul.d
vim /etc/consul.d/consul.json

consul agent -config-dir=/etc/consul.d
```

<br>

**停止Agent**
有两种停止方式：

- gracefully
发送中断信号`ctrl+c`或运行`kill -INT`。优雅地退出，Agent首先通知集群它要离开集群。这样，集群便会通知其它成员该节点已离开。

- forcefully
通过`kill signal`来强制杀掉Consul。集群的其余部分最终会检测到该节点已死亡并通知集群节点已失效。

特别重要的是允许Server节点优雅地离开，以便对可用性产生最小的影响。
对于Client Agent来说，节点失效和节点离开的区别对用例并不是那么重要。

<br>

**生命周期**
Consul集群中的每个Agent都会经历一个生命周期(lifecycle)。
当Agent首次启动时，他并不知道集群中的其它任何节点。要发现它的同伴，它必须加入集群。这使用`join`命令或在配置文件中配置。一旦一个节点加入，这个信息就会传递给整个集群，这意味着所有节点最终都会意识到对方。
如果Agent是一个Server，则已经存在的Server就会开始复制(replicating)到新节点。

在网络故障的情况下，某些节点可能无法被其它节点访问。在这种情况下，无法访问的节点被标记为失败(failed)。无法区分网络故障和Agent崩溃，因此两种情况的处理方式都是相同的。该信息将在service catalog中被更新。

当一个节点离开时，它指定了它的意图，并且集群将该节点标记为已离开。与失败(failed)不同，节点提供的所有服务都立即注销(deregistered)。如果Agent是Server，则对其的复制(replication)将停止。

为了防止死亡(failed/left)节点的堆积，Consul会自动将死亡节点从目录中移除。这个过程被称为**收割(reaping)**。



<br/>
<br/>


## DNS接口


DNS接口允许应用程序利用服务发现，而无需与Consul进行高度整合。

有几个重要的配置项：

- `client_addr`
- `ports.dns`
- `recursors`
- `domain`
- `dns_config`

**数据中心部分是可选的，如果没有提供，则默认为Agent自身的数据中心。**

<br>

**节点查找**
为了解析名称(name)，Consul依赖于特定的查询格式。基本上有两种类型的查询：

- node lookup
- service lookup

```
#node lookup
<node>.node[.datacenter].<domain>

node1.node.dc1.consul
node1.node.consul

dig @127.0.0.1 -p 8600 node1.node.consul

```

<br>

**服务查找**
服务查找用于查询你服务提供者。

有两种查询方式：

- 标准查询
DNS查询系统利用健康检查信息来防止路由到不健康的节点。为了实现简单的负载均衡，每次返回的节点集都是随机的。

```
[tag.]<service>.service[.datacenter].<domain>

redis.service.consul
postgresql.service.dc2.consul

dig @127.0.0.1 -p 8600 redis.service.consul SRV
```

- RFC 2782查询
RFC 2782使用`_`下划线作为查询中服务和协议值的前缀，以防止DNS冲突。


```
_<service>._<protocol>[.service][.datacenter][.domain]

dig @127.0.0.1 -p 8600 _rabbitmq._amqp.service.consul SRV
```

<br>

**Prepared Query Lookups**
The query or name is the ID or given name of an existing Prepared Query.

```
<query or name>.query[.datacenter].<domain>
```

<br>

**可连接的服务查找**
Connect-Capable Service Lookups.

```
<service>.connect.<domain>
```

<br>

**Caching**
默认情况下，Consul服务的所有DNS结果都会设置一个为0的TTL。这会禁用DNS结果的缓存。但，很多情况下，缓存对性能和伸缩性都是可取的。

<br>

**WAN地址转换**
默认情况下，Consul DNS查询将会返回一个节点的本地地址。如果你需要外部地址，则可使用`advertise-wan`和`translate_wan_addrs`选项来配置此行为。



<br/>
<br/>



## 配置

Agent有许多通过命令行或配置文件配置的配置项。配置优先级如下：

1. 命令行参数
2. 环境变量
3. 配置文件

配置文件可以是**HCL**或**JSON**格式。
Consul可通过`reload`命令重新载入配置文件。


<br/>
<br/>


### 端口

Consul默认使用的端口：

- 8300(tcp)
Server RPC. Server用于处理来自其它Agent的传入请求。

- 8301(tcp/udp)
Serf LAN. 用于处理LAN中的gossip，所有Agent都需要。

- 8302(tcp/udp)
Serf WAN. Server用于处理WAN上gossip到其它Server。

- 8500(tcp)
HTTP API.

- 8600(tcp/udp)
DNS Interface.


<br/>
<br/>


### 可重新加载的配置

Reloadable Configuration

重新加载配置文件不会加载所有配置项，如下这些配置项是可重新载入的：

- log level
- checks
- services
- watches
- http client address
- node metadata
- metric prefix filter
- discard check output
- rpc rate limiting


<br/>
<br/>


### 配置文件

配置文件不仅用于设置代理，还用于提供检查和服务定义。

配置文件选项和命令行参数稍微有点不一样。
使用`consul agent -h`查看具体配置项。

栗子：

```
#开始栗子
vim /etc/consul.d/single.json


{
"bind_addr": "192.168.1.11",
"bootstrap": true,
"client_addr": "0.0.0.0",
"datacenter": "zhang",
"data_dir": "/var/lib/consul",
"log_level": "WARN",
"node_name": "zhang21",
"server": true,
"enable_syslog": true,
"ui": true
}

```



```
#集群配置
vim /etc/consul.d/cluster.json


{
	"bind_addr": "xxx",
	"bootstrap_expect": 2,
	"client_addr": "0.0.0.0",
	"datacenter": "zhang",
	"data_dir": "/var/lib/consul",
	"encrypt": "a1b8vAA2==@xyz",
	"log_level": "WARN",
	"node_name": "zhang21",
	"node_id": "zhang21",
	"server": true,
	"enable_syslog": true,
	"ui": true,
	"retry_interval": 20s,
	"retry_join": [
		"consul.domain.internal",
		"10.0.1.2:8301",
		"[::1]:8301"
	]
}
```


<br/>
<br/>


## 服务定义

服务发现的主要目标之一是提供可用服务的目录(catalog)。为此，Agent提供了一种简单的服务定义格式来声明服务的可用性，并可能将其与健康检查相关联。如果健康检查与服务关联，则认为它是应用程序级别。


<br>


### 服务定义

服务定义方式：

- 配置文件(推荐)
- HTTP API

一个服务定义包含的字段：

- name(必须)
- id(可选)
- tags(可选)
- address(可选)
- port(可选)
- check(可选)
- meta(可选)
- `enable_tag_override`(可选)
- token(可选)

id必须唯一，如果未设置id，默认使用name。

<br>

服务可以关联健康检查，这是一个强大的功能。
检查必须是脚本、HTTP、TCP或TTL类型。

- 脚本类型，必须提供参数和间隔
- HTTP类型，必须提供http和interval
- TCP类型，必须提供tcp和interval
- TTL类型，只能提供ttl

检查名称自动生成为: `service:<service-id>`，如果有多个服务检查注册，生成的id为： `service:<service:-id>:<num>`，num是从1开始递增的数字。

<br>

栗子：

```
vim /etc/consul.d/redis.json


{
    "service": {
        "name": "redis",
        "id": "redis01",
        "tags": [
            "master"
        ],
        "address": "127.0.0.1",
        "port": 6379,
        "meta": {
            "meta": "service definition for redis"
        },
        "enable_tag_override": false,

        "check": {
            "id": "redisTCP",
            "name": "redis service check",
            "tcp": "localhost:6379",
            "interval": "10s",
            "timeout": "1s"
        }
    }
}
```


<br/>
<br/>


### 多个服务定义

```
{
  "services": [
    {
      "id": "red0",
      "name": "redis",
      "tags": [
        "primary"
      ],
      "address": "",
      "port": 6000,
      "checks": [
        {
          "args": ["/bin/check_redis", "-p", "6000"],
          "interval": "5s",
          "ttl": "20s"
        }
      ]
    },
    {
      "id": "red1",
      "name": "redis",
      "tags": [
        "delayed",
        "secondary"
      ],
      "address": "",
      "port": 7000,
      "checks": [
        {
          "args": ["/bin/check_redis", "-p", "7000"],
          "interval": "30s",
          "ttl": "60s"
        }
      ]
    },
    ...
  ]
}
```



<br/>
<br/>



## 检查定义


Agent的主要角色便是管理系统级和应用级的健康检查。
一个检查的定义有两种方式：

- 配置文件
- HTTP API

<br>

检查方式：

- Script + Interval
- HTTP + Interval
- TCP + Interval
- TTL
- Docker + Interval
- gRPC + Interval


<br/>


### 定义检查


A script check:

```
{
  "check": {
    "id": "mem-util",
    "name": "Memory utilization",
    "args": ["/usr/local/bin/check_mem.py", "-limit", "256MB"],
    "interval": "10s",
    "timeout": "1s"
  }
}
```

<br>

A HTTP check:

```
{
  "check": {
    "id": "api",
    "name": "HTTP API on port 5000",
    "http": "https://localhost:5000/health",
    "tls_skip_verify": false,
    "method": "POST",
    "header": {"x-foo":["bar", "baz"]},
    "interval": "10s",
    "timeout": "1s"
  }
}
```

<br>

A TCP check:

```
{
  "check": {
    "id": "ssh",
    "name": "SSH TCP on port 22",
    "tcp": "localhost:22",
    "interval": "10s",
    "timeout": "1s"
  }
}
```

<br>

A TTL check:

```
{
  "check": {
    "id": "web-app",
    "name": "Web App Status",
    "notes": "Web app does a curl internally every 10 seconds",
    "ttl": "30s"
  }
}
```

<br>

A Docker check:

```
{
"check": {
    "id": "mem-util",
    "name": "Memory utilization",
    "docker_container_id": "f972c95ebf0e",
    "shell": "/bin/bash",
    "args": ["/usr/local/bin/check_mem.py"],
    "interval": "10s"
  }
}
```

<br>

A gRPC check:

```
{
"check": {
    "id": "mem-util",
    "name": "Service health status",
    "grpc": "127.0.0.1:12345",
    "grpc_use_tls": true,
    "interval": "10s"
  }
}
```


<br/>


### 检查脚本

使用`enable_script_checks`选项来启用脚本检查。

检查脚本的退出码(exit code)必须遵循如下约定：

- exit code o
检查通过

- exit code 1
检查警告

- any exit code
检查失败


<br/>


### 初始化健康检查状态

在某些情况下，可能需要指定健康检查的初始状态。

```
{
  "check": {
    "id": "mem",
    "args": ["/bin/check_mem", "-limit", "256MB"],
    "interval": "10s",
    "status": "passing"
  }
}
```


<br/>


### 绑定服务检查

健康检查可以选择性地绑定到特定服务。这可以确保健康检查的状态只会影响给定服务的健康状态，而不会影响整个节点。
服务绑定检查需要添加一个`service_id`字段：

```
{
  "check": {
    "id": "web-app",
    "name": "Web App Status",
    "service_id": "web-app",
    "ttl": "30s"
  }
}
```


<br/>


### 定义多个检查

使用`checks`来定义多个服务检查。

```
{
  "checks": [
    {
      "id": "chk1",
      "name": "mem",
      "args": ["/bin/check_mem", "-limit", "256MB"],
      "interval": "5s"
    },
    {
      "id": "chk2",
      "name": "/health",
      "http": "http://localhost:5000/health",
      "interval": "15s"
    },
    {
      "id": "chk3",
      "name": "cpu",
      "script": "/bin/check_cpu",
      "interval": "10s"
    },
    ...
  ]
}
```



<br/>
<br/>



## 加密

Encryption

Consul Agent支持加密所有流量。有两个独立的加密系统：

- gossip流量
- RPC


<br>


### gossip加密


启用geossip加密只需要你在启动Consul Agent时设置加密密钥(encryption key)。密钥是16Bytes的Base64编码。

```
consul keygen
FDGDpW55oCYJlh555Es1gA==


vim /etc/consul.d/cluster.json

{
    "encrypt": "FDGDpW55oCYJlh555Es1gA==",
}
```

consul集群的所有节点必须共享相同的加密密钥！


<br>


### RPC加密


Consul支持使用TLS来验证Server和Client之间的真实性。它们之间使用由证书机构颁发的密钥对，你可以自己生成CA。



<br/>
<br/>



## Telemetry


Consul Agent收集有关不同库和子系统的各种运行时指标。这些指标以10s为间隔进行汇总，并保留1min。
查看这些数据，你需要向Consul进程发送信号：

- Unix: USR1
- Windows: BREAK

Consul收到信号后，它将当前的遥测(telemetry)信息转储到Agent's STDERR。

```
#USR1 10
kill -10 ${consul-pid}

```

详情: <https://www.consul.io/docs/agent/telemetry.html>


<br/>
<br/>



## Watches


watches是一种指定检测更新的数据视图的方式。检测到更新，将调用外部处理程序。
watch使用HTTP API中的blocking query，Agent自动进行适当的API调用已检测更新，并在数据视图更新时通知处理程序。
watch可以配置为Agent configuration的一部分，watch也可以在Agent之外启动。


<br>


### 处理程序


监测配置指定要监测的数据视图，更新视图后，将调用指定的处理程序(Handler)。外部程序可为可执行程序(executable)或HTTP endpoint。

- **可执行程序**
可执行处理程序从stdin读取json信息，此外`CONSUL_INDEX`环境变量将被设置为Consul Index写入stdout。

```
{
  "type": "key",
  "key": "foo/bar/baz",
  "handler_type": "script",
  "args": ["/usr/bin/my-service-handler.sh", "-redis"]
}

#在consul v1.0以后，args数组被添加，以便可在没有shell的情况下运行处理程序
```

<br>

- **HTTP endpoint**
当watch被调用时发送HTTP请求给HTTP处理程序。

```
{
  "type": "key",
  "key": "foo/bar/baz",
  "handler_type": "http",
  "http_handler_config": {
    "path":"https://localhost:8000/watch",
    "method": "POST",
    "header": {"x-foo":["bar", "baz"]},
    "timeout": "10s",
    "tls_skip_verify": false
  }
}
```


<br/>


### 全局参数

Global Parameters

- datacenter
- token
- args
- handler


<br/>


### Watch类型

- key
- keyprefix
- services
- nodes
- service
- checks
- event

<br>

栗子：

```
consul watch -type service -service redis

consul watch -type checks -service redis
```

<br>

```
#key
{
  "type": "key",
  "key": "foo/bar/baz",
  "args": ["/usr/bin/my-service-handler.sh", "-redis"]
}
#or
consul watch -type=key -key=foo/bar/baz /usr/bin/my-key-handler.sh



#keyprefix
{
  "type": "keyprefix",
  "prefix": "foo/",
  "args": ["/usr/bin/my-service-handler.sh", "-redis"]
}
#or
consul watch -type=keyprefix -prefix=foo/ /usr/bin/my-prefix-handler.sh



#services
{
  "redis": []
}




#nodes
[
  {
    "Node": "node1",
	"Address": "192.168.1.11"
  },
  {
    "Node": "node2",
	"Address": "xxx"
  }
]



#service
{
  "type": "service",
  "service": "redis",
  "args": ["/usr/bin/my-service-handler.sh", "-redis"]
}



#check
[
  {
    "Node": "foobar",
    "CheckID": "service:redis",
    "Name": "Service 'redis' check",
    "Status": "passing",
    "Notes": "",
    "Output": "",
    "ServiceID": "redis",
    "ServiceName": "redis"
  }
]



#event
{
  "type": "event",
  "name": "web-deploy",
  "args": ["/usr/bin/my-service-handler.sh", "-web-deploy"]
}
#or
consul watch -type=event -name=web-deploy /usr/bin/my-deploy-handler.sh -web-deploy
```





<br/>
<br/>

---

<br/>



# 指南

Consul Guide

本节提供了Consul各种常见的操作指南。

如下：

- **ACLs**
Consul访问控制列表，该功能用于控制对资源的访问。

- **Adding/Removing Servers**
从集群中安全地添加和删除Consul Server，这应该小心操作。

- **Autopilot**
为Consul Server提供自动友好操作的管理。

- **Bootstrapping**
引导新的数据中心，包括安全地添加初始化Consul Server。

- **Consul with Container**
在容器内运行Consul Cluster。

- **DNS Caching**
为DNS查询缓存启用TTLS

- **DNS Forwarding**
从BIND转发DNS查询到Consul

- **External Services**
注册外部服务。允许在Consul框架内使用第三方服务。

- **Federation**
配置Consul以支持多个数据中心。

- **Geo Failover**
用准备好的查询来实现服务的地理故障转移。

- **Leader Election**
使用Consul构建Client端的领导选举。

- **Network Segments**
配置Consul使用网段-支持部分LAN连接。

- **Outage Recovery**
恢复因Server故障而无法使用的集群。

- **Semaphore**
使用KV存储实现一个信号量

- **Sentinel**
使用哨兵模式在Consul中执行策略。

- **Server Performance**
Consul Server的最低要求以及生产环境中运行Consul Server的指南。



<br/>
<br/>



## ACLs


Consul提供可选的访问控制列表系统，用于控制对数据和API的访问。它依赖于规则的token.

访问控制列表旨在提供易于使用，快速执行和灵活的新策略。


<br/>


### 概述

**ACL Tokens**
访问控制列表系统基于token(令牌)，由Consul操作者通过 Consul ACL API进行管理。
如果没有提供token，则会自动关联与特殊的可配置匿名令牌(anonymous token)的规则。

每个token具有：

- ID
- name
- type
	+ client
	+ management
- rule set(规则集)

<br>

**ACL Rules and Scope**
token绑定到一组规则，用于控制令牌可以访问的Consul资源。可在白名单(whitelist)/黑名单(blacklist)下定义策略，这取决于默认策略`acl_default_policy`的值。

构建规则的ACL策略：

- agent
用于Agent API

- event
用于Event API

- key
用于KV Store API

- keyring
用于Keyring API

- node
用于Catalog API, Health API, Prepare Query API, Network Coordinate API， Agent API

- operator
用于Operator API

- query
用于Prepared Query API

- service
Catalog API, Health API, Prepared Query API, Agent API

- session
用于Session API


由于Consul snapshots实际上包含ACL token，因此Snapshot API需要一个管理token进行快照操作。

ACL策略不包括如下资源：

- Status API
- Catalog API

<br>

**ACL Datacenter**
必须使用`acl_datacenter`配置所有节点(client/server)来启用ACL强制实施，但同时也是权威数据中心。Consul依靠RPC转发来支持多数据中心(multi-datacenter)。但是，由于可以跨数据中心边界发出请求，因此ACL令牌必须在全局范围内有效。为避免一致性问题，单个数据中心被视为具有权威性，并存储规范的令牌集。


<br/>
<br/>


### 配置ACLs

使用多个配置项配置ACL：

| 配置项 | Server | Client | 目的
| - | - | - | -
| `acl_datacenter` | required | required | 为ACL定义权威Consul数据中心来启用ACL的主控制
| `acl_default_policy` | 可选 | n/a | 定义白名单或黑名单模式
| `acl_down_policy` | 可选 | 可选 | 定义ACL数据中心脱机时执行的操作
| `acl_ttl` | 可选 | 可选 | 定义缓存ACL的生存时间

<br>

配置特殊令牌，允许引导ACL系统或在特殊情况下访问Consul：

| 特殊令牌 | Server | Client | 目的
| - | - | - | -
| `acl_agent_master_token` | 可选 | 可选 | 当ACL数据中心不可用或Server脱机时，可用于访问Agent API
| `acl_agent_token` | 可选 | 可选 | 用于Agent内部操作
| `acl_master_token` | required | n/a | 用于引导ACL系统
| `acl_token` | 可选 | 可选 | 用于未提供token的客户端请求的默认令牌。这通常配置为对服务的只读访问权限，以便在Agent上启用DNS发现

<br>

**ACL Agent Master Token**
由于`acl_agent_master_token`旨在Consul Server不可用时使用，因此其策略在Agent本地管理，并且不需要通过ACL API在Consul Server上定义token。

```
agent "<node name of agent>" {
  policy = "write"
}
node "" {
  policy = "read"
}
```

<br>

**ACL Agent Token**
`acl_agent_token`是一个特殊令牌，用于Agent的内部操作。用于Agent的如下操作：

- 使用Catalog API更新Agent的节点条目
- 执行反熵同步
- 执行`consul_exec`命令时，读写KV存储库的特殊`_rexec部分`

```
node "node1" {
  policy = "write"
}
service "" {
  policy = "read"
}
key "_rexec" {
  policy = "write"
}
```

任何一个可在Agent上注册的服务，service策略需要读访问权限。


<br/>
<br/>


### 引导ACLs

Bootstrapping ACLs

在新集群上引导ACLs需要几个步骤：

**Enable ACLs on the Consul Servers**
引导ACLs的第一步便是在ACL数据中心的Consul Server上启用ACLs，配置如下：

```
{
  "acl_datacenter": "dc1",
  "acl_master_token": "123abc!@#,
  "acl_default_policy": "deny",
  "acl_down_policy": "deny",
  "acl_down_policy": "extend-cache"
}
```

<br>

**Create an Agent Token**
使用ACL API和上一步中设置的ACL Master Token创建令牌：

```
curl --request PUT --header "X-Consul-Token: 123abc!@#" --data \
'{
   "Name": "Agent Token",
   "Type": "client",
   "Rules": "node \"\" { policy = \"write\"} service \"\" { policy = \"read\" }"
}' http://127.0.0.1:8500/v1/acl/create



返回的值便是新创建的token
{"ID": "xxxxxxxxxxxxxx"}
```

返回的值便是新创建的token。将这个值添加到Consul Server配置中，并重启Server：

```
{
  "acl_datacenter": "dc1",
  "acl_master_token": "123abc!@#,
  "acl_default_policy": "deny",
  "acl_down_policy": "deny",
  "acl_down_policy": "extend-cache",
  "acl_agent_token": "xxxxxxxxxxxxxxxx"
}
```

或使用API导入token：

```
curl --request PUT --header "X-Consul-Token: 123abc!@#" --data \
'{
   "Token": "xxxxxxxxxxxxx"
}' http://127.0.0.1:8500/v1/agent/token/acl_agent_token
```

<br>

**Enable ACLs on the Consul Clients**
还需再Agent上配置ACL

```
{
  "acl_datacenter": "dc1",
  "acl_down_policy": "extend-cache",
  "acl_agent_token": "前面的acl_agent_token"
}



#或使用API
curl \
    --request PUT \
    --header "X-Consul-Token: abc123!@#" \
    --data \
'{
  "Token": "xxxxxxxxxxxx"
}' http://127.0.0.1:8500/v1/agent/token/acl_agent_token
```

使用由Server创建的相同ACL Agent token，因为它不是特定于任何节点或前缀集。建议每个Client获取一个ACL agent token，该令牌具有对自己的节点名称前缀的节点有写入权限，以及针对预期在该Client上注册的服务前缀的读权限。

<br>

**Set an Anonymous Policy (Optional)**
此时，ACL已通过配置的ACL agent token进行引导，但还没有配置其它策略。
甚至像`consul members`这样的基本操作也会受到ACL默认策略`deny`的限制。

如果我们提供上面的Token，则能够看到具体信息：

```
CONSUL_HTTP_TOKEN=xxxxxxxx consul members
```

匿名令牌：

```
curl \
    --request PUT \
    --header "X-Consul-Token: 123abc!@#" \
    --data \
'{
  "ID": "anonymous",
  "Type": "client",
  "Rules": "node \"\" { policy = \"read\" }"
}' http://127.0.0.1:8500/v1/acl/update

```

某个服务：
```
curl \
    --request PUT \
    --header "X-Consul-Token: 123abc!@#" \
    --data \
'{
  "ID": "anonymous",
  "Type": "client",
  "Rules": "node \"\" { policy = \"read\" } service \"consul\" { policy = \"read\" }"
}' http://127.0.0.1:8500/v1/acl/update
```

<br>

**Set Agent-Specific Default Tokens (Optional)**
匿名令牌的替代方法是`acl_token`配置项。

<br>

**Create Tokens for UI Use (Optional)**
如果你使用具有限制性ACL策略的Consul UI，UI将无法使用匿名ACL令牌完整运行。
建议使用特定于UI的ACL令牌，可以在Web浏览器绘画期间在UI中设置该令牌对进口进行认证。

```
curl \
    --request PUT \
    --header "X-Consul-Token: 123abc!@#" \
    --data \
'{
  "Name": "UI Token",
  "Type": "client",
  "Rules": "key \"\" { policy = \"write\" } node \"\" { policy = \"read\" } service \"\" { policy = \"read\" }"
}' http://127.0.0.1:8500/v1/acl/create
```


<br/>
<br/>


### 规则

Rule Specification

ACL系统的和核心部分是规则语言，用于描述必须强制执行的策略。
使用基于前缀的规则，最具体的前缀匹配决定了操作。
使用HCL配置语言来指定规则，规则可定义多个策略。
ACL API运行使用HCL或JSON来定义规则部分的内容。

策略有以下集中处理方式：

- read
- write(读写)
- deny

栗子：

```
# These control access to the key/value store.
key "" {
  policy = "read"
}
key "foo/" {
  policy = "write"
}
key "foo/private/" {
  policy = "deny"
}

# This controls access to cluster-wide Consul operator information.
operator = "read"
```

<br>

**Agent Rules**
Agent策略控制对Agent API中实用程序操作的访问。
Agent规则通过节点名称，使用欧冠最长前缀匹配规则。

Agent rules栗子：

```
agent "" {
  policy = "read"
}
agent "foo" {
  policy = "write"
}
agent "bar" {
  policy = "deny"
}
```

如上，对具有空前缀的任何节点可读，对以foo开头的节点名进行读写，拒绝以bar开头的节点名。

<br>

**Event Rules**
事件策略控制对事件API中事件操作的访问。
事件规则由它们事件名称的前缀，使用最长匹配规则。

Event rules栗子：

```
event "" {
  policy = "read"
}
event "deploy" {
  policy = "write"
}
```

<br>

**Key/Value Rules**
键值策略控制对KV API中的键值存储操作的访问。

Key规则栗子：

```
key "" {
  policy = "read"
}
key "foo" {
  policy = "write"
}
key "bar" {
  policy = "deny"
}
```

<br>

**List Policy for Keys**
一个新的键列表策略，只有在通过布尔配置参数`acl_enable_key_list_policy`选择时才会强制执行。

```
key "" {
 policy = "deny"
}

key "bar" {
 policy = "list"
}

key "baz" {
 policy = "read"
}
```

<br>

**Kerring Rules**

```
keyring = "write"
```

<br>

**Node Rules**

```
node "" {
  policy = "read"
}
node "app" {
  policy = "write"
}
node "admin" {
  policy = "deny"
}
```

<br>

**Operator Rules**

```
operator = "read"
```

<br>

**Prepared Query Rules**

```
query "" {
  policy = "read"
}
query "foo" {
  policy = "write"
}
```


<br/>
<br/>



## 引导数据中心

Bootstrapping a Datacenter

在Consul集群可以开始为请求提供服务之前，必须选在Server节点作为leader。Bootstrapping是将这些初始Server节点加入集群的过程。

建议的引导方式是使用`-bootstrap-expect`配置项。此配置项告知Consul预期的Server节点数，并在有许多Server可用时自动引导。为了防止不一致和脑裂情况(多个Server认为自己是leader)，所有Server应该指定相同的`-bootstrap-expect`，或根本不指定任何值。只有指定值的Server才会尝试引导集群。为了防止脑裂情况，Server不会选举自己作为leader。

推荐每个数据中心使用3或5台Server。不建议使用单个服务器部署数据中心。

加入一个集群:

```
#On NodeB
consul join NodeA
```


<br/>


### 创建集群

要触发选举leader，必须将这些机器连接在一起并创建一个集群。

- 使用`-join`和`start_join`选项手动指定机器列表
- 使用`-retry-join`选项手动指定机器列表



<br/>
<br/>



## leader选举


使用Consul构建客户端的领导选举。

有多种方式建立领导选举，我们将专注于Consul sessions。会话允许我们构建一个可以优雅地处理故障的系统。

<br>

**协调节点**
Contending Nodes

假设一组节点试图称为给定服务的领导者，参与的所有节点应该就给定的键进行协调。

```
servece/<service name>/leader
```

<br>

首先创建会话：

```
curl -X PUT '{ "Name": "dbservice" }' http://localhost:8500/v1/session/create

这回返回一个JSON对象的session ID
```

下一步是使用`?acquirre=<session>`查询参数的KV条目上的PUT方法从此节点获取给定键的会话。`PUT`的`<body>`应该是表示本地节点的JSON对象。

```
curl -X PUT -d <body> http://localhost:8500/v1/kv/<key>?acquire=<session>

如果返回true，则已获得锁定，并且本地节点时领导者
如果返回false，则某个其它节点已获取锁定

```

通过对`<key>`的阻塞查询来监视更改，如果注意到`<key>`的session是空白的，那么就没有领导者，我们应该重新锁定获取。
如果领导是自愿下台，这应该通过简单地释放锁来完成：

```
curl -X PUT http://localhost:8500/v1/kv/<key>?release=<session>
```

<br>

**发现一个领导者**
Discovering a Leader

关于领导者选举的另一种常见做法是节点希望识别给定服务的领导者。
与领导者选举一样，所有参与的节点都应该同意用于协调的密钥(key)。

Client有一个非常简单的角色，它们只需阅读`<key>`来发现当前的领导者是谁:

```
curl  http://localhost:8500/v1/kv/<key>
```

如果密钥没有关联的话，就没有领导者。
你可查询`/v1/session/info`获取session详细信息：

```
curl http://localhost:8500/v1/session/info/xxxxxxxxxxx
```

Client还应使用阻塞查询来查看密钥的更改，如果领导者退出或失败将清除与密钥相关联的会话。当选出新的领导者时，密钥值也将更新。












<br/>
<br/>

---

<br/>



# API


文档链接: <https://www.consul.io/api/index.html>

Consul的主要接口是RESTful HTTP API。API可对node，service，check，configuration...执行基本的CRUD操作。

**版本前缀**
Version Prefix

所有API路由都以`/v1/`为前缀，这适用于v1 API。
























<br/>
<br/>

---

<br/>



# consul-template

Consul Template 查询consul instance，并更新文件系统上任意数量的指定模板。作为额外的奖励，Consul Template可以在模板更新完成时执行任意命令。

Consul Tempalte可以查询Consul中的服务条目，keys, key values。强大的抽象和模板查询语言是Consul Template非常适合创建动态配置。

如：

- Apache
- Nginx
- HAproxy


<br/>
<br/>



## 安装


- 下载地址: <https://releases.hashicorp.com/consul-template/>

步骤：

1. 下载
2. 解压
3. 添加PATH

```
wget https://releases.hashicorp.com/consul-template/0.19.5/consul-template_0.19.5_linux_amd64.tgz

tar -xzvf ./consul-template_0.19.5_linux_amd64.tgz
mv ./consul-template /bin/


#or
mv consul-template /usr/local/bin
vim /etc/profile
export PATH=$PATH:/usr/local/bin


consul-template --version
consul-template v0.19.5 (57b6c71)
```


<br/>
<br/>


## 用法

官方栗子： <https://github.com/hashicorp/consul-template/tree/master/examples>


```
consul-template -h

```

<br>


### 命令行

查询`demo.consul.io`这个consul实例。

渲染模板：

```
consul-template \
    -template "/tmp/nginx.ctmpl:/var/nginx/nginx.conf:nginx -s reload" \
    -template "/tmp/redis.ctmpl:/var/redis/redis.conf:service redis restart" \
    -template "/tmp/haproxy.ctmpl:/var/haproxy/haproxy.conf"
```

监听Consul：

```
consul-template -consul-addr="consul1:8500" -consul-addr="consul2:8500"
```


<br/>


### 配置文件

配置文件使用 [HashiCorp Configuration Language](https://github.com/hashicorp/hcl)编写的。这意味着，配置也是JSON兼容的。

命令行指定的选项优先于配置文件！

```
mkdir /etc/consul-template

vim consul-template.hcl

consul-template -config='/etc/consul-template/consul-template.hcl'
```

配置文件详情：

```
# This denotes the start of the configuration section for Consul. All values
# contained in this section pertain to Consul.
consul {
  # This block specifies the basic authentication information to pass with the
  # request. For more information on authentication, please see the Consul
  # documentation.
  auth {
    enabled  = true
    username = "test"
    password = "test"
  }

  # This is the address of the Consul agent. By default, this is
  # 127.0.0.1:8500, which is the default bind and port for a local Consul
  # agent. It is not recommended that you communicate directly with a Consul
  # server, and instead communicate with the local Consul agent. There are many
  # reasons for this, most importantly the Consul agent is able to multiplex
  # connections to the Consul server and reduce the number of open HTTP
  # connections. Additionally, it provides a "well-known" IP address for which
  # clients can connect.
  address = "127.0.0.1:8500"

  # This is the ACL token to use when connecting to Consul. If you did not
  # enable ACLs on your Consul cluster, you do not need to set this option.
  #
  # This option is also available via the environment variable CONSUL_TOKEN.
  token = "abcd1234"

  # This controls the retry behavior when an error is returned from Consul.
  # Consul Template is highly fault tolerant, meaning it does not exit in the
  # face of failure. Instead, it uses exponential back-off and retry functions
  # to wait for the cluster to become available, as is customary in distributed
  # systems.
  retry {
    # This enabled retries. Retries are enabled by default, so this is
    # redundant.
    enabled = true

    # This specifies the number of attempts to make before giving up. Each
    # attempt adds the exponential backoff sleep time. Setting this to
    # zero will implement an unlimited number of retries.
    attempts = 12

    # This is the base amount of time to sleep between retry attempts. Each
    # retry sleeps for an exponent of 2 longer than this base. For 5 retries,
    # the sleep times would be: 250ms, 500ms, 1s, 2s, then 4s.
    backoff = "250ms"

    # This is the maximum amount of time to sleep between retry attempts.
    # When max_backoff is set to zero, there is no upper limit to the
    # exponential sleep between retry attempts.
    # If max_backoff is set to 10s and backoff is set to 1s, sleep times
    # would be: 1s, 2s, 4s, 8s, 10s, 10s, ...
    max_backoff = "1m"
  }

  # This block configures the SSL options for connecting to the Consul server.
  ssl {
    # This enables SSL. Specifying any option for SSL will also enable it.
    enabled = true

    # This enables SSL peer verification. The default value is "true", which
    # will check the global CA chain to make sure the given certificates are
    # valid. If you are using a self-signed certificate that you have not added
    # to the CA chain, you may want to disable SSL verification. However, please
    # understand this is a potential security vulnerability.
    verify = false

    # This is the path to the certificate to use to authenticate. If just a
    # certificate is provided, it is assumed to contain both the certificate and
    # the key to convert to an X509 certificate. If both the certificate and
    # key are specified, Consul Template will automatically combine them into an
    # X509 certificate for you.
    cert = "/path/to/client/cert"
    key  = "/path/to/client/key"

    # This is the path to the certificate authority to use as a CA. This is
    # useful for self-signed certificates or for organizations using their own
    # internal certificate authority.
    ca_cert = "/path/to/ca"

    # This is the path to a directory of PEM-encoded CA cert files. If both
    # `ca_cert` and `ca_path` is specified, `ca_cert` is preferred.
    ca_path = "path/to/certs/"

    # This sets the SNI server name to use for validation.
    server_name = "my-server.com"
  }
}

# This is the signal to listen for to trigger a reload event. The default
# value is shown below. Setting this value to the empty string will cause CT
# to not listen for any reload signals.
reload_signal = "SIGHUP"

# This is the signal to listen for to trigger a graceful stop. The default
# value is shown below. Setting this value to the empty string will cause CT
# to not listen for any graceful stop signals.
kill_signal = "SIGINT"

# This is the maximum interval to allow "stale" data. By default, only the
# Consul leader will respond to queries; any requests to a follower will
# forward to the leader. In large clusters with many requests, this is not as
# scalable, so this option allows any follower to respond to a query, so long
# as the last-replicated data is within these bounds. Higher values result in
# less cluster load, but are more likely to have outdated data.
max_stale = "10m"

# This is the log level. If you find a bug in Consul Template, please enable
# debug logs so we can help identify the issue. This is also available as a
# command line flag.
log_level = "warn"

# This is the path to store a PID file which will contain the process ID of the
# Consul Template process. This is useful if you plan to send custom signals
# to the process.
pid_file = "/path/to/pid"

# This is the quiescence timers; it defines the minimum and maximum amount of
# time to wait for the cluster to reach a consistent state before rendering a
# template. This is useful to enable in systems that have a lot of flapping,
# because it will reduce the the number of times a template is rendered.
wait {
  min = "5s"
  max = "10s"
}

# This denotes the start of the configuration section for Vault. All values
# contained in this section pertain to Vault.
vault {
  # This is the address of the Vault leader. The protocol (http(s)) portion
  # of the address is required.
  address = "https://vault.service.consul:8200"

  # This is the grace period between lease renewal of periodic secrets and secret
  # re-acquisition. When renewing a secret, if the remaining lease is less than or
  # equal to the configured grace, Consul Template will request a new credential.
  # This prevents Vault from revoking the credential at expiration and Consul
  # Template having a stale credential.
  #
  # Note: If you set this to a value that is higher than your default TTL or
  # max TTL, Consul Template will always read a new secret!
  #
  # This should also be less than or around 1/3 of your TTL for a predictable
  # behaviour. See https://github.com/hashicorp/vault/issues/3414
  grace = "5m"

  # This is the token to use when communicating with the Vault server.
  # Like other tools that integrate with Vault, Consul Template makes the
  # assumption that you provide it with a Vault token; it does not have the
  # incorporated logic to generate tokens via Vault's auth methods.
  #
  # This value can also be specified via the environment variable VAULT_TOKEN.
  token = "abcd1234"

  # This tells Consul Template that the provided token is actually a wrapped
  # token that should be unwrapped using Vault's cubbyhole response wrapping
  # before being used. Please see Vault's cubbyhole response wrapping
  # documentation for more information.
  unwrap_token = true

  # This option tells Consul Template to automatically renew the Vault token
  # given. If you are unfamiliar with Vault's architecture, Vault requires
  # tokens be renewed at some regular interval or they will be revoked. Consul
  # Template will automatically renew the token at half the lease duration of
  # the token. The default value is true, but this option can be disabled if
  # you want to renew the Vault token using an out-of-band process.
  #
  # Note that secrets specified in a template (using {{secret}} for example)
  # are always renewed, even if this option is set to false. This option only
  # applies to the top-level Vault token itself.
  renew_token = true

  # This section details the retry options for connecting to Vault. Please see
  # the retry options in the Consul section for more information (they are the
  # same).
  retry {
    # ...
  }

  # This section details the SSL options for connecting to the Vault server.
  # Please see the SSL options in the Consul section for more information (they
  # are the same).
  ssl {
    # ...
  }
}

# This block defines the configuration for connecting to a syslog server for
# logging.
syslog {
  # This enables syslog logging. Specifying any other option also enables
  # syslog logging.
  enabled = true

  # This is the name of the syslog facility to log to.
  facility = "LOCAL5"
}

# This block defines the configuration for de-duplication mode. Please see the
# de-duplication mode documentation later in the README for more information
# on how de-duplication mode operates.
deduplicate {
  # This enables de-duplication mode. Specifying any other options also enables
  # de-duplication mode.
  enabled = true

  # This is the prefix to the path in Consul's KV store where de-duplication
  # templates will be pre-rendered and stored.
  prefix = "consul-template/dedup/"
}

# This block defines the configuration for exec mode. Please see the exec mode
# documentation at the bottom of this README for more information on how exec
# mode operates and the caveats of this mode.
exec {
  # This is the command to exec as a child process. There can be only one
  # command per Consul Template process.
  command = "/usr/bin/app"

  # This is a random splay to wait before killing the command. The default
  # value is 0 (no wait), but large clusters should consider setting a splay
  # value to prevent all child processes from reloading at the same time when
  # data changes occur. When this value is set to non-zero, Consul Template
  # will wait a random period of time up to the splay value before reloading
  # or killing the child process. This can be used to prevent the thundering
  # herd problem on applications that do not gracefully reload.
  splay = "5s"

  env {
    # This specifies if the child process should not inherit the parent
    # process's environment. By default, the child will have full access to the
    # environment variables of the parent. Setting this to true will send only
    # the values specified in `custom_env` to the child process.
    pristine = false

    # This specifies additional custom environment variables in the form shown
    # below to inject into the child's runtime environment. If a custom
    # environment variable shares its name with a system environment variable,
    # the custom environment variable takes precedence. Even if pristine,
    # whitelist, or blacklist is specified, all values in this option
    # are given to the child process.
    custom = ["PATH=$PATH:/etc/myapp/bin"]

    # This specifies a list of environment variables to exclusively include in
    # the list of environment variables exposed to the child process. If
    # specified, only those environment variables matching the given patterns
    # are exposed to the child process. These strings are matched using Go's
    # glob function, so wildcards are permitted.
    whitelist = ["CONSUL_*"]

    # This specifies a list of environment variables to exclusively prohibit in
    # the list of environment variables exposed to the child process. If
    # specified, any environment variables matching the given patterns will not
    # be exposed to the child process, even if they are whitelisted. The values
    # in this option take precedence over the values in the whitelist.
    # These strings are matched using Go's glob function, so wildcards are
    # permitted.
    blacklist = ["VAULT_*"]
  }

  # This defines the signal that will be sent to the child process when a
  # change occurs in a watched template. The signal will only be sent after the
  # process is started, and the process will only be started after all
  # dependent templates have been rendered at least once. The default value is
  # nil, which tells Consul Template to stop the child process and spawn a new
  # one instead of sending it a signal. This is useful for legacy applications
  # or applications that cannot properly reload their configuration without a
  # full reload.
  reload_signal = ""

  # This defines the signal sent to the child process when Consul Template is
  # gracefully shutting down. The application should begin a graceful cleanup.
  # If the application does not terminate before the `kill_timeout`, it will
  # be terminated (effectively "kill -9"). The default value is "SIGTERM".
  kill_signal = "SIGINT"

  # This defines the amount of time to wait for the child process to gracefully
  # terminate when Consul Template exits. After this specified time, the child
  # process will be force-killed (effectively "kill -9"). The default value is
  # "30s".
  kill_timeout = "2s"
}

# This block defines the configuration for a template. Unlike other blocks,
# this block may be specified multiple times to configure multiple templates.
# It is also possible to configure templates via the CLI directly.
template {
  # This is the source file on disk to use as the input template. This is often
  # called the "Consul Template template". This option is required if not using
  # the `contents` option.
  source = "/path/on/disk/to/template.ctmpl"

  # This is the destination path on disk where the source template will render.
  # If the parent directories do not exist, Consul Template will attempt to
  # create them, unless create_dest_dirs is false.
  destination = "/path/on/disk/where/template/will/render.txt"

  # This options tells Consul Template to create the parent directories of the
  # destination path if they do not exist. The default value is true.
  create_dest_dirs = true

  # This option allows embedding the contents of a template in the configuration
  # file rather then supplying the `source` path to the template file. This is
  # useful for short templates. This option is mutually exclusive with the
  # `source` option.
  contents = "{{ keyOrDefault \"service/redis/maxconns@east-aws\" \"5\" }}"

  # This is the optional command to run when the template is rendered. The
  # command will only run if the resulting template changes. The command must
  # return within 30s (configurable), and it must have a successful exit code.
  # Consul Template is not a replacement for a process monitor or init system.
  command = "restart service foo"

  # This is the maximum amount of time to wait for the optional command to
  # return. Default is 30s.
  command_timeout = "60s"

  # Exit with an error when accessing a struct or map field/key that does not
  # exist. The default behavior will print "<no value>" when accessing a field
  # that does not exist. It is highly recommended you set this to "true" when
  # retrieving secrets from Vault.
  error_on_missing_key = false

  # This is the permission to render the file. If this option is left
  # unspecified, Consul Template will attempt to match the permissions of the
  # file that already exists at the destination path. If no file exists at that
  # path, the permissions are 0644.
  perms = 0600

  # This option backs up the previously rendered template at the destination
  # path before writing a new one. It keeps exactly one backup. This option is
  # useful for preventing accidental changes to the data without having a
  # rollback strategy.
  backup = true

  # These are the delimiters to use in the template. The default is "{{" and
  # "}}", but for some templates, it may be easier to use a different delimiter
  # that does not conflict with the output file itself.
  left_delimiter  = "{{"
  right_delimiter = "}}"

  # This is the `minimum(:maximum)` to wait before rendering a new template to
  # disk and triggering a command, separated by a colon (`:`). If the optional
  # maximum value is omitted, it is assumed to be 4x the required minimum value.
  # This is a numeric time with a unit suffix ("5s"). There is no default value.
  # The wait value for a template takes precedence over any globally-configured
  # wait.
  wait {
    min = "2s"
    max = "10s"
  }
}

```

<br>

栗子：

```
vim /etc/consul-template/consul.hcl


max_stale = '10m'
wait = {
  min = '1s'
  max = '3s'
}

template {
  source = '/etc/consul-template/ctmpl/a.ctmpl'
  destination = '/etc/nginx/conf.d/upstream-a.conf'
  command = 'systemctl reload nginx'
  perms = 0644
}




#
vim /etc/consul-template/ctmpl/a.ctmpl


upstream upstream-a {
  {{range service 'a'}}
  server {{.Address}}:{{.Port}};
  {{else}}
  server 127.0.0.1:12345;
  {{end}}
}



#nginx
vim /etc/nginx/conf.d/upstream-a.conf


upstream upstream-a {

  server 192.168.1.11:12345;

}
```


<br/>


### 模板语法


Consul Template解析文件以 [Go Template](https://golang.org/pkg/text/template/)创作。
Consul Template提供了如下函数：

#### API函数

API函数与远程API进行交互，与Consul等外部服务进行通信。

- datacenters
查询Consul目录中的所有数据中心。

```
{{ datacenters }}


#栗子
{{ range datacenters }}
{{ . }}{{ end }}

#效果
dc1
dc2
```






























