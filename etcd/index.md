# etcd


参考:

- [etcd文档](https://etcd.io/docs)

<br/>

环境:

- RHEL 7
- etcd v3.5





<br/>
<br/>

<!--more-->

---

<br/>
<br/>









# 快速开始

快速安装和运行单节点的etcd集群。

```bash
# 二进制安装

# 启动
etcd

# 设置一个key
etcdctl put greeting "Hello, etcd"
# 获取一个key
etcdctl get greeting
```









<br/>
<br/>

---

<br/>
<br/>







# 术语表

[Glossary](https://etcd.io/docs/v3.5/learning/glossary/)

- Alarm
每当集群需要操作员干预以保持可靠性时，etcd server都会发出警报。

- Authentication
管理验证etcd资源的用户访问权限。

- Client
etcd连接到etcd集群以发出服务请求。

- Cluster
集群由一些成员组成。每个成员的节点都遵循raft一致性协议来复制日志。集群接受来自成员的提案，提交并应用到本地存储。

- Compaction
压缩会丢弃给定修订之前的所有etcd事件历史记录核被取代的键。它用于回收etcd后端数据库中的存储空间。

- Election
etcd集群在其成员之间举行选举，选择领导者作为raft一致性协议的一部分。

- Endpoint
指向etcd服务或资源的URL。

- Key
用户定义的标识符，用于在etcd中存储核检索用户定义的值。

- Key Range
一组键。

- Keyspace
etcd集群中所有键的集合。

- Lease
一种短期的租赁合同，在到期时删除与其关联的键。类似于Redis Key TTL。

- Member
参与etcd集群提供服务的逻辑etcd server。

- Peer
对等体是同一个集群中的另一个成员。

- Proposal
提议是需要通过raft协议的请求。

- Quorum
协商一致修改集群状态所需的活跃的成员数。

- Revision
校订是一个64位集群范围的计数器，从1开始，每次修改键空间都会递增。

- Role
一组键的权限单位，可授予一组用户进行访问控制。

- Snapshot
etcd集群状态的时间点备份。

- Store
支持集群键空间的物理存储。

- Transaction
原子执行的一组操作。事务中所有已修改的键共享相同的修订版本。

- Key Version
自键创建以来的写入次数，从1开始。不存在或删除的键的版本为0。

- Watcher
客户端打开一个观察者以观察给定键范围上的更新。











<br/>
<br/>

---

<br/>
<br/>















# 教程

[Tutorials](https://etcd.io/docs/v3.5/tutorials/)

包含以下这些：

- 访问etcd
- 访问key，访问特定前缀的key
- 删除key
- 在一个事务中做多个写入
- 观察key
- 创建lease(租约)
- 创建lock(分布式锁)
- 在etcd集群中进行领导选举
- 检查集群状态
- 保存数据库
- 添加和删除节点

<br/>

```bash
# 写入
etcdctl --endpoints=xxx put foo "Hello World"

# 获取
etcdctl get foo
etcdctl --write-out="json" get foo
etcdctl get web --prefix

# 删除
etcdctl delete key
etcdctl delete keyaa --prefix

# 一个事务中多个写入
etcdctl txn --interactive

# 观察
etcdctl watch key1

# 租约
etcdctl lease grant 300
# lease 2be7547fbc6a5afa granted with TTL(300s)
etcdctl put key1 value --lease=2be7547fbc6a5afa
etcdctl lease keep-alive 2be7547fbc6a5afa
etcdctl lease revoke 2be7547fbc6a5afa

# 锁
etcdctl lock mutex1

# 选举
etcdctl elect one p1

# 集群状态
etcdctl --write-out=table endpoint status
etcdctl endpoint healty

# 保存数据库
etcdctl snapshot save my.db
etcdctl --write-out=table snapshot status my.db

```








<br/>
<br/>

---

<br/>
<br/>



















# FAQ

[Frequently asked questions]()https://etcd.io/docs/v3.5/faq/


<br/>
<br/>


## 通用

etcd是什么？

etcd是一个一致性、分布式的k-v存储。主要用作分布式系统的单独协调服务。旨在持有少量的数据就可以完成时候。

<br/>

etcd如何发音？

发音为`/ˈɛtsiːdiː/`，意为distributed etc directory。

<br/>

客户端必须向etcd领导者发送请求吗？

Raft是基于领导者的，领导者处理需要集群一致(consensus)的所有客户端请求。但是，客户端不需要知道哪个节点是领导者。任何发送到跟随者(follower)的需要一致的请求都会自动转发给领导者。任何集群成员都可以处理不需要一致的请求。


<br/>
<br/>


## 配置

listen-client-urls, listen-peer-urls, advertise-client-urls, initial-advertise-per-urls之间的区别？

listen-client-urls和listen-peer-urls指定etcd server绑定的本地地址，来接受传入的连接。要监听所有接口，指定`0.0.0.0`作为监听地址。

advertise-client-urls和initial-advertise-peer-urls指定etcd 客户端或其它成员用于与etcd server联系的地址。彼此之间必须能访问到这个地址。不要使用如`localhost`或`0.0.0.0`地址，因为可能远程机器无法到达。

<br/>

为什么不改变 --listen-peer-urls 或 --initial-advertise-peer-urls 在 `etcdctl member list`更新中？

成员的peer urls来自集群初始化引导的 --initial-advertise-peer-urls。可能会造成脑裂。使用 `etcdctl member update` 更新一个成员的peer urls。


<br/>
<br/>


## 部署

系统要求。

由于etcd写入数据到磁盘，它非常依赖磁盘性能，强烈推荐SSD。为了防止性能劣化或无意中重载键值存储，默认情况下，etcd可配置的存储大小为2GB。为了避免swapping或OOM，机器至少应具有尽可能多的RAM以覆盖配额。如果要配置超过2GB，8GB是一个建议的最大的值。

<br/>

为什么是奇数节点集群成员？

etcd需要大多数节点，一个仲裁，以达成集群状态的更新。对于一个n成员的集群，仲裁是`(n/2)+1`。对于任何奇数集群，添加一个节点将始终增加仲裁所需的节点数量。虽然对奇数集群添加一个节点多了节点，似乎更好，但由于在相同节点不丢失仲裁，容错性更差了，因此可能有更多节点可能会失败。如果集群处于无法再容忍任何故障的状态，则在删除节点之前添加节点是危险的。因为如果新节点无法注册到集群（如地址配置错误），仲裁将永久丢失。

<br/>

最大集群数量？

没有硬性规定。但是，etcd集群不应该超过7个节点。建议运行5个节点。一个5成员的etcd集群可以容忍2个成员故障，这在大多数场景下足够了。尽管较大的集群提供更好的容错能力，但由于必须在更多计算机上复制数据，因此写入性能可能会受到影响。

<br/>

容错是什么？

只有可以建立成员仲裁，etcd集群就会运行。如果仲裁因暂时性网络故障而丢失，etcd在网络恢复并恢复仲裁后自动完全地恢复，Raft强制执行集群的一致性。对于断电，etcd将Raft日志保存早磁盘，etcd将日志重播到故障点并恢复集群参与。对于永久性硬件故障，通过运行时重载配置从集群中删除故障节点。

建议奇数成员集群。

| Cluster Size | Majority | Failure Tolerance |
| - | - | - |
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 3 | 2 | 1 |
| 4 | 3 | 1 |
| 5 | 3 | 2 |

<br/>

etcd是否可以跨地区或跨数据中心部署？

跨地区部署etcd提高了容错，代价是一致性请求延迟可能更高。由于etcd依赖成员仲裁来达成一致，选举和心跳可能也会受到网络延迟的影响。


<br/>
<br/>


## 操作

如何备份etcd集群？

etcdctl提供了snapshot命令来创建备份。

<br/>

在移除一个不健康的成员之前应该添加一个成员吗？

当替换一个etcd节点时，重要的是先移除节点，之后再添加替换节点。

<br/>

为什么etcd不接受我的成员资源变更？

etcd设置了 strict-reconfig-check 以拒绝可能导致仲裁丢失的重载配置的请求。遗弃仲裁确实存在风险（尤其是当集群已经处于不健康状态时）。在添加新成员时，如果仲裁丢失，可能会尝试禁用仲裁，这可能导致全面的集群不一致性。对于许多应用，这可能让问题变得更糟。

<br/>

为什么etcd会因磁盘延迟高峰而失去领导者地位？

磁盘延迟也是领导者活动的一部分。假设集群领导者需要一分钟将Raft日志更新同步到磁盘，但etcd集群有一秒钟的选举超时。即使领导者可以在选举间隔内处理网络消息，但它实际上不可用，因为它无法提交任何新提案，它在等待慢磁盘。如果集群由于磁盘延迟而频繁地丢失其领导者，请尝试调整磁盘设置或etcd时间参数。

<br/>

etcd警告"request ignored(cluster ID mismatch)"意味着什么？

每个新的etcd集群都会根据初始集群配置和用户提供的唯一初始化令牌生成一个新的集群ID。通过唯一的集群ID，etcd可以防止可能破坏集群的跨集群交互。

通常，此警告发生在拆除旧集群，然后重用新集群的某些对等地址之后。如果旧集群中任何etcd进程仍在运行，它将尝试联系新集群。新集群识别到集群ID不匹配，然后忽略改请求并发出此警告。通过确保不通集群中的对等地址不相交来清除此警告。

<br/>

"mvcc: database space exceeded"是什么意思，如何修复它？


<br/>
<br/>


## 性能

etcd警告 "apply entries took too long"是什么意思？

在大多数etcd成员统一提交请求后，每个etcd服务器都会将请求应用到其数据库，并将结果保存到磁盘。应用一个请求通常应小于50ms。如果平均持续时间超过100ms，etcd将警告条目应用时间多长（entries are taking too long to apply）。

通常这个问题由慢磁盘导致。

第二个最常见的原因是CPU饥饿。如果CPU使用率很高，则etcd可能没有足够的计算容量。

访问太多keys的昂贵的用户请求也可能导致长延迟，这类似与数据库的全表扫描。

<br/>

etcd警告"failed to send out heartbeat on time"是什么意思？

etcd使用基于领导者的一致协议进行一致性的数据复制和日志执行。集群成员选举一个领导者，其它成员都成为追随者。当选的领导者必须定期向其追随者发送心跳，以保持其领导者地位。如果选举间隔内未收到心跳检测，则追随者推断领导者失败并触发选举。如果领导者没有及时发送心跳，但它仍在运行，那么选举就是虚假的，很可能是由资源不足引起的。如果领导者跳过两个检测信号，etcd就会警告未能按时发送检测信号。

此问题通常由慢磁盘导致的。在领导者发送附加了元数据(metadata)的检测信号之前，它可能需要将元数据保存到磁盘。

第二个常见的原因是CPU饥饿。

慢网络也可能导致这个问题。

<br/>

etcd警告"snapshotting is taking more than x seconds to finish ..."是什么意思？

etcd发送其完整键值存储的快照，来刷新慢追随者和备份。








<br/>
<br/>

---

<br/>
<br/>





# 操作指南

[operations guide](https://etcd.io/docs/v3.5/op-guide/)


<br/>


## 认证指南

Authentication Guides
















































