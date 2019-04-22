---
title: HBase
date: 2019-03-15 10:57:20
categories: DevOps
tags:
  - Apache
  - HBase
  - DataAnalysis
  - NoSQL
---

参考:

- 维基百科
- HBase: <https://hbase.apache.org/>
- Reference Guide: <http://hbase.apache.org/book.html>


环境:

- RHEL7x86_64
- HBase v3.0










<br/>
<br/>

---

<!--more-->

<br/>
<br/>










# 介绍


![](/images/HBase/hbase_logo.png)

<br>

**Apache HBase** 是Hadoop数据库，是一个分布式(distributed)，可扩展(scalable)的大数据存储。

当你需要对大数据进行随机(random)，实时(realtime)R/W访问时，请使用Apache HBase。它的目标是在硬件集群上托管非常大的表——数十亿行数百万列。

HBase是一个开源的非关系型分布式数据库（NoSQL），它参考了谷歌的BigTable建模，实现的编程语言为 Java。它是Apache软件基金会的Hadoop项目的一部分，运行于HDFS文件系统之上，为 Hadoop 提供类似于BigTable 规模的服务。因此，它可以对稀疏文件提供极高的容错率。

HBase在列上实现了BigTable论文提到的压缩算法、内存操作和布隆过滤器。HBase的表能够作为MapReduce任务的输入和输出，可以通过Java API来访问数据，也可以通过REST、Avro或者Thrift的API来访问。







<br/>
<br/>

---

<br/>
<br/>


















# 特点

Features


- 线性和模块化可扩展性
- 严格一致的读写操作
- 表的自动和可配置分片
- 支持RegionServers之间的自动故障转移
- 方便的基类，用于使用Apache HBase表支持Hadoop MapReduce作业
- 易于使用的Java API，用于客户端访问
- 阻止缓存和bloom过滤器以进行实时查询
- Query predicate push down via server side Filters
- Thrift gateway和REST-ful Web service，支持XML， Protobuf， binary data encoding
- 可扩展的基于JRuby的（JIRB）shell
- 支持通过Hadoop Metrics子系统将指标导出到文件或其它
- ...











<br/>
<br/>

---

<br/>
<br/>


















# 入门

Getting Started


<br/>


## Standalone HBase


本节介绍在单节点的standalone实例上运行HBase。**Standalone instance** 包含了所有的**HBase Daemons(Master, RegionServers, Zookeeper)**，在单个JVM中运行并持久化到本地文件系统。这是一个最基本的配置，将展示如何使用HBase shell CLI在HBase中创建表、在表中插入行、对表执行放置和扫描操作、启用/禁用表、启动和停止HBase。


<br/>


### JDK


HBase要求安装JDK。



<br/>
<br/>



### 使用HBase

步骤:

- 下载
- 配置
- 启动
- 使用
- 停止

<br>

```
# 访问Apache DownLoad Mirrors，下载对应版本HBase
# https://www.apache.org/dyn/closer.lua/hbase/
cd opt
wget http://mirror.bit.edu.cn/apache/hbase/3.0.0/hbase-3.0.0-bin.tar.gz


# 解压
tar xzvf hbase-3.0.0-SNAPSHOT-bin.tar.gz
mv hbase-3.0.0-SNAPSHOT hbase
cd hbase


# 启动前必须先设置JAVA_HOME环境变量
# conf/hbase-env.sh
export JAVA_HOME=/usr/java/jdk1.8.0_191-amd64


# 编辑conf/hbase-site.xml，这是主要的HBase配置文件
# 您需要在本地文件系统上指定HBase和ZooKeeper写入数据并确认一些风险的目录
# 栗子
<configuration>
  <property>
    <name>hbase.rootdir</name>
    <value>file:///home/testuser/hbase</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/home/testuser/zookeeper</value>
  </property>
  <property>
    <name>hbase.unsafe.stream.capability.enforce</name>
    <value>false</value>
    <description>
      Controls whether HBase will check for stream capabilities (hflush/hsync).

      Disable this if you intend to run on LocalFileSystem, denoted by a rootdir
      with the 'file://' scheme, but be mindful of the NOTE below.

      WARNING: Setting this to false blinds you to potential data loss and
      inconsistent system state in the event of process and/or node failures. If
      HBase is complaining of an inability to use hsync or hflush it's most
      likely not a false positive.
    </description>
  </property>
</configuration>


# start a standalone instance of HBase
# 一个JVM运行 HMaster, HRegionServer, Zookeeper
bin/start-hbase.sh

# http://localhost:16010 查看HBase Web UI
```

<br>

你不需要创建HBase数据目录，它会自动做这件事。如果你创建目录，HBase将尝试进行迁移，这不是你想要的。

要在现有的HDFS实例上安装HBase，请将 `hbase.rootdir` 设置为指向实例上的目录(如: `hdfs://namenode.example.org:8020/hbase`)。

```bash
# 使用hbase shell命令连接到HBase
/bin/hbase shell
xxxx
xxxx
2.3.7 :001 >



# 显示帮助
> help



# 创建表，你必须指定 Table name和ColumnFamily name
> create 't-test', 'c-test'
Took 1.7627 seconds
 => Hbase::Table - t-test
# 也可在Web UI上查看相关信息



# 列出表信息
> list 'c-test'
TABLE
t-test
1 row(s)
Took 0.0520 seconds
 => ["t-test"]


> describe 't-test'
Table t-test is ENABLED
t-test
COLUMN FAMILIES DESCRIPTION
{NAME => 'c-test', VERSIONS => '1', EVICT_BLOCKS_ON_CLOSE => 'false', NEW_VERSION_BEHAVIOR => 'false', KEEP_DELETED_CELLS => 'FALSE', CACHE_DATA_ON_WRITE => 'false', DATA_BLOCK_ENCODING
=> 'NONE', TTL => 'FOREVER', MIN_VERSIONS => '0', REPLICATION_SCOPE => '0', BLOOMFILTER => 'ROW', CACHE_INDEX_ON_WRITE => 'false', IN_MEMORY => 'false', CACHE_BLOOMS_ON_WRITE => 'false',
 PREFETCH_BLOCKS_ON_OPEN => 'false', COMPRESSION => 'NONE', BLOCKCACHE => 'true', BLOCKSIZE => '65536'}
1 row(s)
Took 0.2293 seconds



# 将数据放入表中
>put 't-test',  'row1', 'c-test:a', 'value1'
Took 0.2116 seconds
> put 't-test',  'row2', 'c-test:b', 'value2'
Took 0.0082 seconds
> > put 't-test',  'row3', 'c-test:c', 'value3'
Took 0.0085 seconds



# 一次扫描表中所有数据
> scan 't-test'
>   scan 't-test'
ROW                                             COLUMN+CELL
 row1                                           column=c-test:a, timestamp=1552630577582, value=value1
 row2                                           column=c-test:b, timestamp=1552630591734, value=value2
 row3                                           column=c-test:c, timestamp=1552630598817, value=value3



# 获取单行数据
> get 't-test' 'row1'
COLUMN                                          CELL
 c-test:a                                       timestamp=1552630577582, value=value1
1 row(s)
Took 0.0225 seconds



# 禁用/启用表
> disable 't-test'
> enable 't-test'



# 删除表
> drop 't-test'



# 退出HBase Shell
> quit



# stop a standalone instance of HBase
# 可能需要几分钟，请耐心等待
bin/stop-hbase.sh

```


![](/images/HBase/hbase_standalone.png)



<br/>
<br/>
<br/>



### 伪分布式本地安装

Pseudo-Distributed Local Install


通过standalone模式之后，你可以重新配置HBase以**伪分布式模式(Pseudo-Distributed)**运行。伪分布式意味着HBase仍然在单个主机上运行，但每个HBase Daemons(HMaster, HRegionServer, Zookeeper)作为一个单独的进程运行。默认数据存储在`/tmp`下，除非你像Standalone一样配置了`rootdir`。

假设将数据存储在HDFS中，并且HDFS可用。

```bash
# Stop HBase if it is running.


# 配置HBase
# 编辑hbase-site.xml

# 添加指示HBase以分布式模式运行，每个守护进程有一个JVM实例
<property>
  <name>hbase.cluster.distributed</name>
  <value>true</value>
</property>

# 使用 hdfs://// URI语法将hbase.rootdir从本地文件系统更改为HDFS实例的地址，地址请查看HDFS配置
# 请确保删除hbase.unsafe.stream.capability.enforce的条目或将其设置为true
# 你不需要在HDFS中创建目录，HBase会为你做这件事。如果您创建目录，HBase将尝试进行迁移，这不是您想要的
<property>
  <name>hbase.rootdir</name>
  <value>hdfs://localhost:8020/hbase</value>
</property>



# 启动HBase
bin/start-hbase.sh
# 如果系统配置正确，则jps显示正在运行的HBase进程
jps
20065 HMaster
20006 HQuorumPeer
20137 HRegionServer
20521 Jps



# 检查HDFS中的HBase目录
# 如果一切正常，HBase会在HDFS中创建它的目录
# 注意HDFS的安全模式
bin/hadoop fs -ls /hbase
Found 1 items
drwxr-xr-x   - zhang supergroup          0 2019-03-15 15:33 /hbase/.tmp



# 创建一个表，并用数据填充它
# 创建放入和前面一样



# 启动/停止一个 backup HBase Master Server(HMaster)
# 在同一硬件上运行多个HMaster实例在生产环境中没有意义，就像运行伪分布式集群对生产没有意义一样。此步骤仅用于测试和学习
/bin/local-master-backup.sh start 2 3 5

# 在不杀死整个集群的情况下终止backup master
cat /tmp/hbase-testuser-1-master.pid |xargs kill -9



# 启动/停止additional RegionServers
# 在同一系统上运行多个HRegionServers对于以伪分布式模式进行测试非常有用。
bin/local-regionservers.sh start 2 3 4
bin/local-regionservers.sh stop 3



# 停止HBase
bin/stop-hbase.sh
```



<br/>
<br/>



### 分布式

Advanced - Fully Distributed


实际上，你需要完全分布式(fully-distributed)配置才能完全测试HBase并在实际场景中使用它。在分布式配置中，集群包含多个节点，每个节点运行一个或多个HBase守护进程(包括: Primary Master, Backup Master, Multiple ZooKeeper nodes, Multiple RegionServer nodes)。


<br/>


#### 分布式集群架构

Distributed Cluster Demo Architecture

| Node Name | Master | ZooKeeper | RegionServer |
| - | - | - | - |
| `node-a.example.com` | yes | yes | no |
| `node-b.example.com` | backup | yes | yes |
| `node-c.example.com` | no | yes | yes |

确保集群之间的可访问性。


<br/>
<br/>


#### SSH免密

Configure Passwordless SSH Access

```bash
# 生成公钥
ssh-keygen -t rsa


# 写入公钥
cat id_rsa.pub >> ~/.ssh/authorized_keys


# 注意修改权限


# 测试免密登录
ssh user@hostname
```


<br/>
<br/>


#### 准备集群

node-a 将运行 Primary Master, ZooKeeper, no RegionServers.

```bash
# 配置conf/regionservers
# 删除node-a的RegionServer地址，并添加node-b, node-c的RegionServer地址
# node-b.example.com, node-c.example.com
conf/regionservers


# 配置HBase使用node-b作为 Backup Master
# 创建 conf/backup-masters，并使用 node-b的主机名为其添加新行
conf/backup-masters



# 配置ZooKeeper
# 实际情况中，你应该仔细考虑ZooKeeper配置
# node-a, conf/hbase-site.xml
<property>
  <name>hbase.zookeeper.quorum</name>
  <value>node-a.example.com,node-b.example.com,node-c.example.com</value>
</property>
<property>
  <name>hbase.zookeeper.property.dataDir</name>
  <value>/usr/local/zookeeper</value>
</property>
```

<br>

node-b 将运行 backup master, ZooKeeper instance。

```sh
# 复制 node-a conf/ 到 node-b, node-c conf/
# 集群中的每个节点都需要具有相同的配置
# 请注意不同节点的localhost这个地址
```

<br>

配置完成后，便要启动并测试集群。

```sh
# Be sure HBase is not running on any node



# Start the cluster
# On node-a, node-b, node-c
bin/start-hbase.sh

# ZooKeeper starts first, followed by the master, then the RegionServers, and finally the backup masters.



# Verify that the processes are running
jps



# Browse to the Web UI



# Test what happens when nodes or services disappear
# 测试可用性
```









<br/>
<br/>

---

<br/>
<br/>


















# 配置

Apache HBase Configuration

























