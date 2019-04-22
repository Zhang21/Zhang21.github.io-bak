---
title: Hadoop
date: 2019-03-06 00:57:11
categories: DevOps
tags:
  - Hadoop
  - BigData
  - DataAnalysis
  - Apache
---


参考:

- wikipeadia
- Hadoop官网: <https://hadoop.apache.org/>
- Apache Software Foundation: <https://www.apache.org/>

<br>

环境:

- RHEL7x86_64
- Hadoop v3.2.0




<br/>
<br/>

---

<!--more-->

<br/>
<br/>











# 简介


**Apache Hadoop** 是一款支持数据密集型分布式应用程序，使用Java编写，并以Apache 2.0许可协议发布的开源软件框架。
Hadoop是根据谷歌公司发表的MapReduce和Google文件系统的论文自行实现而成。所有的Hadoop模块都有一个基本假设，即硬件故障是常见情况，应该由框架自动处理。

Apach Hadoop项目开发了用于可靠(Reliable)，可扩展(Scalable)的分布式计算(Distributed Computing)的开源软件。

Hadoop框架透明地为应用提供可靠性和数据移动。它实现了名为MapReduce的编程范式：应用程序被分割成许多小部分，而每个部分都能在集群中的任意节点上运行或重新运行。此外，Hadoop还提供了分布式文件系统，用以存储所有计算节点的数据，这为整个集群带来了非常高的带宽。MapReduce和分布式文件系统的设计，使得整个框架能够自动处理节点故障。它使应用程序与成千上万的独立计算的计算机和PB级的数据连接起来。

![](/images/Hadoop/hadoop-logo.jpg)


<br/>


## Modules


Hadoop 项目包括以下模块:

- **Hadoop Common**: 支持其它Hadoop模块的常用实用程序
- **Hadoop Distributed File System (HDFS)**: 一种分布式文件系统，提供对应用程序数据的高吞吐量访问
- **Hadoop YARN**: 用于作业调度和集群资源管理的框架
- **Hadoop MapReduce**: 基于YARN的系统，用于并行处理大型数据集
- **Hadoop Ozone**: Hadoop的对象存储
- **Hadoop Submarine**: Hadoop的机器学习引擎



<br/>
<br/>
<br/>



## 相关项目


现在普遍认为整个 **Apache Hadoop Platform** 包括了许多项目:

- **Ambari**: 一个基于Web的工具，用于配置，管理和监控Apache Hadoop集群。包括对HDFS, MapReduce, Hive, HBase, ZooKeeper, Pig, Sqoop...的支持。它还提供了一个用于查看群集运行状况的仪表板，用于查看各个程序的状态
- **Avro**：数据序列化系统。新的数据序列化格式与传输工具，将逐步取代Hadoop原有的IPC机制
- **Cassandra**: 可扩展的多主数据库，没有单点故障
- **Chukwa**: 用于管理大型分布式系统的数据收集系统
- **Flume**: 一种分布式，可靠且可用的软件。用于高效收集(collecting)，聚合(aggregating)和移动(moving)大量日志数据
- **HBase**：可扩展的分布式NoSQL列数据库，支持大型表的结构化数据存储。类似谷歌公司BigTable
- **Hive**：一种数据仓库基础结构，提供数据摘要和即席查询。构建于hadoop之上的数据仓库，通过一种类SQL语言HiveQL为用户提供数据的归纳、查询和分析等功能。Hive最初由Facebook贡献
- **Mahout**：可扩展的机器学习和数据挖掘库
- **Pig**: 用于并行计算的高级数据流语言和执行框架
- **Spark**: 适用于Hadoop数据的快速通用计算引擎。Spark提供了一种简单而富有表现力的编程模型，支持广泛的应用程序
- **Sqoop**：结构化数据（如关系数据库）与Apache Hadoop之间的数据转换工具
- **Tez**: 基于Hadoop YARN的通用数据流编程框架，它提供了一个强大而灵活的引擎来执行任意DAG任务来处理批处理和交互式用例的数据
- **ZooKeeper**：适用于分布式应用程序的高性能协调服务。提供类似Google Chubby的功能，由Facebook贡献









<br/>
<br/>

---

<br/>
<br/>





# General


<br/>


## 概述

Overview


**Node Attributes Support in YARN**

节点属性(Node Attribute)有助于根据节点标记(tag)节点上的多个标签(label)，并支持根据这些标签的表达式放置容器。

<br>

**Hadoop Submarine on YARN**

Hadoop Submarine 使数据工程师能够在数据所在的同一Hadoop YARN集群上轻松开发(develop)、训练(train)和部署(deploy)深度学习模型(TensorFlow)。

<br>

**Storage Policy Satisfier**

支持HDFS(Hadoop Distributed File System)应用程序，以便在文件/目录上设置存储策略时在存储类型之间移动块(block)。

<br>

**ABFS Filesystem connector**

支持最新的Azure Datalake Gen2 Storage。

<br>

**Enhanced S3A connector**

支持增强型S3A连接器，包括更好地恢复受限制的AWS S3和DynamoDB IO。

<br>

**Upgrades for YARN long running services**

支持通过YARN Native Service API和CLI对长时间运行的容器进行就地无缝(seamless)升级。





<br/>
<br/>
<br/>




## 单节点集群

Setting up a Single Node Cluster


<br/>


### 目的

本节介绍如何设置和配置单节点Hadoop集群，以便你可以快速使用Hadoop MapReduce和HDFS执行简单的操作。



<br/>
<br/>



### 先决条件

- **支持的平台**
  - GNU/Linux: Hadoop has been demonstrated on GNU/Linux clusters with 2000 nodes
  - Windows is also a supported platform but the followings steps are for Linux only.

- **依赖软件(Linux)**
  - Java: 具体版本参考 [HadoopJavaVersions](https://wiki.apache.org/hadoop/HadoopJavaVersions)
  - ssh: 必须运行sshd才能使用管理远程Hadoop守护程序的Hadoop脚本，建议按照pdsh以实现更好的ssh资源管子

- **安装软件**

```bash
# Ubuntu
sudo apt-get install ssh pdsh
```



<br/>
<br/>



### 下载

获取Hadoop发行版，请从[Apache Download Mirrors](http://www.apache.org/dyn/closer.cgi/hadoop/common/)下载。



<br/>
<br/>



### 准备启动Hadoop集群

解压前面下载的Hadoop发行版，编辑`hadoop/etc/hadoop/hadoop-env.sh`以定义一些参数:

```bash
# hadoop/etc/hadoop/hadoop-env.sh

# set to the root of your Java installation
export JAVA_HOME=/usr/java/jdk1.8.0_191-amd64

export HADOOP_HOME= /opt/hadoop

# 由于我是使用rpm安装jdk8，所以为/usr/java/jdk1.8.0_191-amd64
# 我的hadoop放置于/opt/hadoop
```

接着运行以下命令:

```bash
./bin/hadoop

# 这将显示Hadoop的使用文档
```

<br>

**有三种方式来启动Hadoop集群:**

- Local (Standalone) Mode
- Pseudo-Distributed Mode
- Fully-Distributed Mode



<br/>
<br/>
<br/>



### Standalone

默认情况下，Hadoop配置为non-distibuted模式运行，作为单个Java进程。这对调试很有用。



<br/>
<br/>



### Pseudo-Distributed Operation

Hadoop也可以运行在伪分布模式下的单节点上，其中每个Hadoop Daemon在单独的java进程中运行。

<br>

**配置**
`hadoop/etc/hadoop/core-site.xml`:

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

`hadoop/etc/hadoop/hdfs-site.xml`:

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

<br>

**设置 passphraseless ssh**

```bash
# 检查是否可以下无密码(passphrase)的情况下ssh到localhost
ssh localhost


# 如果不行，请执行无密码登录操作
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys


# 之后再执行此操作
ssh localhost
```

<br>

**执行**

以下说明在本地运行MapReduce job。

```bash
# 注意，以下位于hadoop目录
# 我的为 /opt/hadoop


# 1. Format the filesystem
# namenode - run the DFS namenode
bin/hdfs namenode -format


# 2. Start NameNode daemon and DataNode daemon
sbin/start-dfs.sh
# 日志输出到$HADOOP_HOME/logs


# 3. Browse the web interface for the NameNode; by default it is available at:
NameNode - http://localhost:9870/

# 4. Make the HDFS directories required to execute MapReduce jobs
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/<username>


# 5. Copy the input files into the distributed filesystem
bin/hdfs dfs -mkdir input
bin/hdfs dfs -put etc/hadoop/*.xml input


# 6. Run some of the examples provided
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.0.jar grep input output 'dfs[a-z.]+'


# 7. Examine the output files: Copy the output files from the distributed filesystem to the local filesystem and examine them
bin/hdfs dfs -get output output
# cat output/*
# bin/hdfs dfs -cat output/*


# 8. When you’re done, stop the daemons with
sbin/stop-dfs.sh
```

<br>

**YARN on a Single Node**

你可以通过设置一些参数并运行ResourceManager Daemon和NodeManager Daemon，以伪分布模式在YARN上运行MapReduce Job。
以下指令假设你已运行上面的1-4步。

```bash
# 1. Configure parameters as follows
# etc/hadoop/mapred-site.xml

<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>



# etc/hadoop/yarn-site.xml

<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>



# 2. Start ResourceManager daemon and NodeManager daemon
sbin/start-yarn.sh



# Browse the web interface for the ResourceManager; by default it is available at
# ResourceManager - http://localhost:8088/



# 4. Run a MapReduce job



# 5. When you’re done, stop the daemons with
sbin/stop-yarn.sh
```



<br/>
<br/>
<br/>



## 集群设置

Hadoop Cluster Setup


本节描述了如何安装和配置Hadoop集群，范围从几个节点到数千个节点。
但本节不包括安全性和高可用性等高级主题。


<br/>


### Prerequisites


- Java
- Hadoop


<br/>
<br/>



### 安装


通常，集群中的一台主机被指定为**NameNode**，而另一台主机被指定为**ResourceManager**，这些都是**Master**。其它服务(Web App Proxy Server, MapReduce...)通常在专用硬件或共享基础架构上运行，具体取决于负载。

集群中的其余主机充当**DataNode**和**NodeManager**。这些都是**Worker**。


<br/>
<br/>



### 在非安全模式下配置Hadoop

Configuring Hadoop in Non-Secure Mode


Hadoop的Java配置由两种类型的重要配置文件驱动:

- **只读(ReadOnly)的默认配置**
    - `core-default.xml`
    - `hdfs-default.xml`
    - `yarn-default.xml`
    - `mapred-default.xml`
- **特定站点(site-specific)的配置**
  - `etc/hadoop/core-site.xml`
  - `etc/hadoop/hdfs-site.xml`
  - `etc/hadoop/yarn-site.xml`
  - `etc/hadoop/mapred-site.xml`

此外，你可以通过`etc/hadoop/hadoop-env.sh`和`etc/hadoop/yarn-env.sh`配置特定于站点的值来控制分发的`bin/`目录下的Hadoop脚本。

<br>

要配置Hadoop Cluster，你需要配置Hadoop Daemon执行的`environment`以及`configuration parameters`。

- HDFS Daemon是 **NameNode**, **SecondaryNameNode** 和 **DataNode**
- YARN Daemon是 **ResourceManager**, **NodeManager** 和 **WebAppProxy**
- 如果要使用MapReduce，则 **MapReduce Job History Server** 也将运行
- 对于大型安装，这些通常在不同的主机上运行


<br/>


#### 配置Hadoop守护进程的环境

Configuring Environment of Hadoop Daemons


管理员应该使用 `etc/hadoop/hadoop-env.sh`， 可选择 `etc/hadoop/mapred-env.sh`, 以及 `etc/hadoop/yarn-env.sh`脚本来对Hadoop守护进程的进程环境进行特定站点的自定义配置。

至少，您必须指定 `JAVA_HOME`，以便在每个远程节点上正确定义它。

管理员可使用下表中的配置项配置各个守护进程:

| Daemon | Environment Variable |
| - | - |
| NameNode | HDFS_NAMENODE_OPTS |
| DataNode | HDFS_DATANODE_OPTS |
| Secondary NameNode | HDFS_SECONDARYNAMENODE_OPTS |
| ResourceManager | YARN_RESOURCEMANAGER_OPTS |
| NodeManager | YARN_NODEMANAGER_OPTS |
| WebAppProxy | YARN_PROXYSERVER_OPTS |
| Map Reduce Job History Server | MAPRED_HISTORYSERVER_OPTS |

<br>

其它你可自定义的有用的配置项包括:

 - `HADOOP_PID_DIR`:  The directory where the daemons’ process id files are stored
 - `HADOOP_LOG_DIR`:  The directory where the daemons’ log files are stored. Log files are automatically created if they don’t exist
 - `HADOOP_HEAPSIZE_MAX`:  The maximum amount of memory to use for the Java heapsize. Units supported by the JVM are also supported here. If no unit is present, it will be assumed the number is in megabytes. By default, Hadoop will let the JVM determine how much to use. This value can be overriden on a per-daemon basis using the appropriate _OPTS variable listed above. For example, setting HADOOP_HEAPSIZE_MAX=1g and HADOOP_NAMENODE_OPTS="-Xmx5g" will configure the NameNode with 5GB heap.

在大多数情况下，你需要指定 `HADOOP_PID_DIR` 和 `HADOOP_LOG` 目录，以便它们只能由将要运行Hadoop守护进程的用户写入。否则可能会发生符号链接攻击。


<br/>
<br/>


#### 配置Hadoop守护进程

Configuring the Hadoop Daemons

本节介绍给定配置文件中指定的重要参数。

- `etc/hadoop/core-site.xml`

| Parameter | Value | Notes |
| - | - | - |
| `fs.defaultFS` | NameNode URI | hdfs://host:port/ |
| `io.file.buffer.size` | 131072 | Size of read/write buffer used in SequenceFiles. |

<br>

- `etc/hadoop/hdfs-site.xml`
- **NameNode配置**

| Parameter | Value | Notes |
| - | - | - |
| `dfs.namenode.name.dir` | Path on the local filesystem where the NameNode stores the namespace and transactions logs persistently. | If this is a comma-delimited list of directories then the name table is replicated in all of the directories, for redundancy. |
| `dfs.hosts /` <br> `dfs.hosts.exclude` | List of permitted/excluded DataNodes. | If necessary, use these files to control the list of allowable datanodes. |
| `dfs.blocksize` | 268435456 | HDFS blocksize of 256MB for large file-systems. |
| `dfs.namenode.handler.count` | 100 | More NameNode server threads to handle RPCs from large number of DataNodes. |

<br>

- **DataNode配置**

| Parameter | Value | Notes |
| - | - | - |
| dfs.datanode.data.dir | Comma separated list of paths on the local filesystem of a DataNode where it should store its blocks. | If this is a comma-delimited list of directories, then data will be stored in all named directories, typically on different devices. |

<br>

- `etc/hadoop/yarn-site.xml`
- **ResourceManager和NodeManager配置**

| Parameter | Value | Notes |
| - | - | - |
| `yarn.acl.enable` | true / false | Enable ACLs? Defaults to false. |
| `yarn.admin.acl` | Admin ACL | ACL to set admins on the cluster. ACLs are of for comma-separated-usersspacecomma-separated-groups. Defaults to special value of * which means anyone. Special value of just space means no one has access. |
| `yarn.log-aggregation-enable` | false | Configuration to enable or disable log aggregation |

<BR>

- **ResourceManager配置**

| Parameter | Value | Notes |
| - | - | - |
| `yarn.resourcemanager.address` | ResourceManager host:port for clients to submit jobs. | host:port If set, overrides the hostname set in yarn.resourcemanager.hostname. |
| `yarn.resourcemanager.scheduler.address` | ResourceManager host:port for ApplicationMasters to talk to Scheduler to obtain resources. | host:port If set, overrides the hostname set in yarn.resourcemanager.hostname. |
| `yarn.resourcemanager.resource-tracker.address` | ResourceManager host:port for NodeManagers. | host:port If set, overrides the hostname set in yarn.resourcemanager.hostname. |
| `yarn.resourcemanager.admin.address` | ResourceManager host:port for administrative commands. | host:port If set, overrides the hostname set in yarn.resourcemanager.hostname. |
| `yarn.resourcemanager.webapp.address` | ResourceManager web-ui host:port. | host:port If set, overrides the hostname set in yarn.resourcemanager.hostname. |
| `yarn.resourcemanager.hostname` | ResourceManager host. | host Single hostname that can be set in place of setting all yarn.resourcemanager*address resources. Results in default ports for ResourceManager components. |
| `yarn.resourcemanager.scheduler.class` | ResourceManager Scheduler class. | CapacityScheduler (recommended), FairScheduler (also recommended), or FifoScheduler. |
| `yarn.scheduler.minimum-allocation-mb` | Minimum limit of memory to allocate to each container request at the Resource Manager. | In MBs |
| `yarn.scheduler.maximum-allocation-mb` | Maximum limit of memory to allocate to each container request at the Resource Manager. | In MBs |
| `yarn.resourcemanager.nodes.include-path` / <br> `yarn.resourcemanager.nodes.exclude-path` | List of permitted/excluded NodeManagers. | If necessary, use these files to control the list of allowable NodeManagers. |

<br>

- **NodeManager配置**

| Parameter | Value | Notes |
| - | - | - |
| `yarn.nodemanager.resource.memory-mb` | Resource i.e. available physical memory, in MB, for given NodeManager | Defines total available resources on the NodeManager to be made available to running containers |
| `yarn.nodemanager.vmem-pmem-ratio` | Maximum ratio by which virtual memory usage of tasks may exceed physical memory | The virtual memory usage of each task may exceed its physical memory limit by this ratio. The total amount of virtual memory used by tasks on the NodeManager may exceed its physical memory usage by this ratio. |
| `yarn.nodemanager.local-dirs` | Comma-separated list of paths on the local filesystem where intermediate data is written. | Multiple paths help spread disk i/o. |
| `yarn.nodemanager.log-dirs` | Comma-separated list of paths on the local filesystem where logs are written. | Multiple paths help spread disk i/o. |
| `yarn.nodemanager.log.retain-seconds` | 10800 | Default time (in seconds) to retain log files on the NodeManager Only applicable if log-aggregation is disabled. |
| `yarn.nodemanager.remote-app-log-dir` | /logs | HDFS directory where the application logs are moved on application completion. Need to set appropriate permissions. Only applicable if log-aggregation is enabled. |
| `yarn.nodemanager.remote-app-log-dir-suffix` | logs | Suffix appended to the remote log dir. Logs will be aggregated to ${yarn.nodemanager.remote-app-log-dir}/${user}/${thisParam} Only applicable if log-aggregation is enabled. |
| `yarn.nodemanager.aux-services` | mapreduce_shuffle | Shuffle service that needs to be set for Map Reduce applications. |
| `yarn.nodemanager.env-whitelist` | Environment properties to be inherited by containers from NodeManagers | For mapreduce application in addition to the default values HADOOP_MAPRED_HOME should to be added. <br> 可能的值有: `JAVA_HOME`, `HADOOP_COMMON_HOME`, `HADOOP_HDFS_HOME`, `HADOOP_CONF_DIR`, `CLASSPATH_PREPEND_DISTCACHE`, `HADOOP_YARN_HOME`, `HADOOP_MAPRED_HOME` |

<br>

- **History Server配置**

| Parameter | Value | Notes |
| - | - | - |
| `yarn.log-aggregation.retain-seconds` | -1 | How long to keep aggregation logs before deleting them. -1 disables. Be careful, set this too small and you will spam the name node. |
| `yarn.log-aggregation.retain-check-interval-seconds` | -1 | Time between checks for aggregated log retention. If set to 0 or a negative value then the value is computed as one-tenth of the aggregated log retention time. Be careful, set this too small and you will spam the name node. |

<br>

- `etc/hadoop/mapred-site.xml`
- **MapReduce Applications配置**

| Parameter | Value | Notes |
| - | - | - |
| `mapreduce.framework.name` | yarn | Execution framework set to Hadoop YARN. |
| `mapreduce.map.memory.mb` | 1536 | Larger resource limit for maps. |
| `mapreduce.map.java.opts` | -Xmx1024M | Larger heap-size for child jvms of maps. |
| `mapreduce.reduce.memory.mb` | 3072 | Larger resource limit for reduces. |
| `mapreduce.reduce.java.opts` | -Xmx2560M | Larger heap-size for child jvms of reduces. |
| `mapreduce.task.io.sort.mb` | 512 | Higher memory-limit while sorting data for efficiency. |
| `mapreduce.task.io.sort.factor` | 100 | More streams merged at once while sorting files. |
| `mapreduce.reduce.shuffle.parallelcopies` | 50 | Higher number of parallel copies run by reduces to fetch outputs from very large number of maps. |

<br>

- **MapReduce JobHistory Server配置**

| Parameter | Value | Notes |
| - | - | - |
| `mapreduce.jobhistory.address` | MapReduce JobHistory Server host:port | Default port is 10020. |
| `mapreduce.jobhistory.webapp.address` | MapReduce JobHistory Server Web UI host:port | Default port is 19888. |
| `mapreduce.jobhistory.intermediate-done-dir` | `/mr-history/tmp` | Directory where history files are written by MapReduce jobs. |
| `mapreduce.jobhistory.done-dir` | `/mr-history/done` | Directory where history files are managed by the MR JobHistory Server. |



<br/>
<br/>



### 监控NodeManager健康

Monitoring Health of NodeManagers


Hadoop提供了一种机制，管理员可通过该机制将NodeManager定期运行提供的脚本，以确定节点是否健康。

管理员可通过在脚本中执行对其选择的任何检查来确定节点是否处于正常状态。如果脚本检测到节点处于不健康状态，则必须以**ERROR**开头的字符串将其行输出到标准输出(std out)。NodeManager定期生成脚本并检查其输出。如果脚本的输出包含字符串**ERROR**(如上所述)，则节点的状态将报告为不健康(**unhealthy**)，并且ResourceManager将节点列入黑名单。之后便不会为此节点分配其它任务。但是，NodeManager继续运行脚本，因此如果节点再次变为健康(**healthy**)，它将自动从ResourceManager上的黑名单节点中被删除。在ResourceManger Web UI中，管理员可以使用节点的运行状况以及脚本的输出(如果不健康)。自节点健康依赖的时间也显示在Web UI上。

以下 `etc/hadoop/yarn-site.xml` 文件中的参数可用于控制节点健康监控脚本:

| Parameter | Value | Notes |
| - | - | - |
| `yarn.nodemanager.health-checker.script.path` | Node health script | Script to check for node’s health status. |
| `yarn.nodemanager.health-checker.script.opts` | Node health script options | Options for script to check for node’s health status. |
| `yarn.nodemanager.health-checker.interval-ms` | Node health script interval | Time interval for running health script. |
| `yarn.nodemanager.health-checker.script.timeout-ms` | Node health script timeout interval | Timeout for health script execution. |

如果只有部分本地磁盘变坏，则运行健康检查的脚本不应该给出**ERROR**信息。NodeManager能够定期检查本地磁盘的运行状况（具体检查 `nodemanager-local-dirs` 和 `nodemanager-log-dirs` ），并在根据为配置属性 `yarn.nodemanager.disk-health-checker.min-healthy-disks` 设置的值达到坏目录数阈值(threshold of number of bad directories)，整个节点被标记为不健康，此信息也被发送到ResourceManager。引导磁盘(boot disk)中的故障也会被检查脚本所识别。



<br/>
<br/>



### Slaves File


在 `etc/hadoop/workers` 文件中列出所有Worker的hostname或IP addr，每行一个。帮助脚本将使用 `etc/hadoop/workers` 文件一次在多个主机上运行命令。它不用于任何基于Java的Hadoop配置。要使用此功能，必须为用于运行Hadoop的账户建立SSH信任(SSH无秘钥或Kerberos)。



<br/>
<br/>



### Rack Awareness


许多Hadoop组件都具有机架感知(**rack-aware**)功能，并利用网络拓扑结构提高性能和安全。Hadoop Daemons通过调用管理员配置的模块来获取集群中Workers的机架信息。

强烈建议在启动HDFS之前配置Rack Awareness！



<br/>
<br/>



### Logging

Hadoop通过Apache Commons Logging框架使用**Apache log4j**进行日志记录。编辑 `etc/hadoop/log4j.properties` 文件以自定义Hadoop Daemons的日志记录配置。



<br/>
<br/>



### 操作集群

Operating the Hadoop Cluster


完成所有必要的配置后，将文件分发到所有主机上的 `HADOOP_CONF_DIR` 目录。这应该是所有主机上的同一个目录。

通常，建议HDFS和YARN使用分开的用户来运行。在大多数安装中，HDFS进程以`hdfs`用户运行；YARN使用`yarn`用户运行。


<br/>


#### Startup and Shutdown

Hadoop Startup and Hadoop Shutdown


**要启动Hadoop集群，你需要启动HDFS和YARN集群。**


```bash
# 第一次启动HDFS时，必须对其进行格式化。将新的分布式文件系统(distributed fs)格式化为hdfs
[hdfs]$ $HADOOP_HOME/bin/hdfs namenode -format <cluster_name>



# 在指定节点以hdfs启动HDFS NameNode
[hdfs]$ $HADOOP_HOME/bin/hdfs --daemon start namenode

# 停止NameNode
[hdfs]$ $HADOOP_HOME/bin/hdfs --daemon stop namenode



# 在每个指定节点以hdfs启动HDFS DataNode
[hdfs]$ $HADOOP_HOME/bin/hdfs --daemon start datanode

# 停止DataNode
[hdfs]$ $HADOOP_HOME/bin/hdfs --daemon stop datanode



# 如果配置了 etc/hadoop/worker 和 SSH信任，则可以使用使用程序脚本以hdfs启动HDFS进程
[hdfs]$ $HADOOP_HOME/sbin/start-dfs.sh

# 停止HDFS进程
[hdfs]$ $HADOOP_HOME/sbin/stop-dfs.sh



# SSH trusted access
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
# 也可将已有的公钥直接写入



# 以yarn在指定的ResourceManager上启动YARN
[yarn]$ $HADOOP_HOME/bin/yarn --daemon start resourcemanager

# 停止ResourceManager
[yarn]$ $HADOOP_HOME/bin/yarn --daemon stop resourcemanager



# 以yarn在每台指定主机上运行脚本启动NodeManager
[yarn]$ $HADOOP_HOME/bin/yarn --daemon start nodemanager

# 停止NodeManager
[yarn]$ $HADOOP_HOME/bin/yarn --daemon stop nodemanager



# 以yarn在WebAppProxy Server上启动Standalone WebAPPProxy Server
# 如果使用多个Server进行负载均衡，则应在每台Server上运行
[yarn]$ $HADOOP_HOME/bin/yarn --daemon start proxyserver

# 停止WebAppProxy server
[yarn]$ $HADOOP_HOME/bin/yarn stop proxyserver



# 如果配置了 etc/hadoop/workers 和 SSH信任，则可以以yarn实用程序脚本启动YARN进程
[yarn]$ $HADOOP_HOME/sbin/start-yarn.sh

# 停止YARN进程
[yarn]$ $HADOOP_HOME/sbin/stop-yarn.sh




# 以mapred在指定的服务器上运行MapReduce JobHistory Server
[mapred]$ $HADOOP_HOME/bin/mapred --daemon start historyserver

# 停止MapReduce JobHistory Server
[mapred]$ $HADOOP_HOME/bin/mapred --daemon stop historyserver
```



<br/>
<br/>



### Web Interfaces


一旦Hadoop Cluster启动并运行，请检查组件的Web UI。具体如下:

| Daemon | Web Interface | Notes |
| - | - | - |
| NameNode | `http://nn_host:port/` | Default HTTP port is 9870 |
| ResourceManager | `http://rm_host:port/` | Default HTTP port is 8088 |
| MapReduce JobHistory Server | `http://jhs_host:port/` | Default HTTP port is 19888 |




<br/>
<br/>
<br/>



## 命令

Hadoop Commands Guide


<br>

### 概述

所有Hadoop命令和子项目都遵循相同的基本结构:

```bash
# Usage
shellcommand [SHELL_OPTIONS] [COMMAND] [GENERIC_OPTIONS] [COMMAND_OPTIONS]
```

<br>

**Shell Options**

```
--buildpaths	Enables developer versions of jars.

--config confdir	Overwrites the default Configuration directory. Default is $HADOOP_HOME/etc/hadoop.

--daemon mode	If the command supports daemonization (e.g., hdfs namenode), execute in the appropriate mode. Supported modes are start to start the process in daemon mode, stop to stop the process, and status to determine the active status of the process. status will return an LSB-compliant result code. If no option is provided, commands that support daemonization will run in the foreground. For commands that do not support daemonization, this option is ignored.

--debug	Enables shell level configuration debugging information

--help	Shell script usage information.

--hostnames	When --workers is used, override the workers file with a space delimited list of hostnames where to execute a multi-host subcommand. If --workers is not used, this option is ignored.

--hosts	When --workers is used, override the workers file with another file that contains a list of hostnames where to execute a multi-host subcommand. If --workers is not used, this option is ignored.

--loglevel loglevel	Overrides the log level. Valid log levels are FATAL, ERROR, WARN, INFO, DEBUG, and TRACE. Default is INFO.

--workers	If possible, execute this command on all hosts in the workers file.
```

<br>

**Generic Options**

```
-archives <comma separated list of archives>	Specify comma separated archives to be unarchived on the compute machines. Applies only to job.
-conf <configuration file>	Specify an application configuration file.
-D <property>=<value>	Use value for given property.
-files <comma separated list of files>	Specify comma separated files to be copied to the map reduce cluster. Applies only to job.
-fs <file:///> or <hdfs://namenode:port>	Specify default filesystem URL to use. Overrides ‘fs.defaultFS’ property from configurations.
-jt <local> or <resourcemanager:port>	Specify a ResourceManager. Applies only to job.
-libjars <comma seperated list of jars>	Specify comma separated jar files to include in the classpath. Applies only to job.
```



<br/>
<br/>



### 常用命令

Hadoop Common Commands

所有这些命令都是从`hadoop`命令执行的。它分为:

- 用户命令(User Commands): 对hadoop集群的用户有用的命令
- 管理命令(Administration Commands): 对hadoop集群的管理员有用的命令


<br/>


#### 用户命令

```bash
archive    #Creates a hadoop archive
checknative    #This command checks the availability of the Hadoop native code
classpath    #Prints the class path needed to get the Hadoop jar and the required libraries
conftest    #Validates configuration XML files
credential    #Command to manage credentials, passwords and secrets within credential providers.
distch    #Change the ownership and permissions on many files at once.
distcp    #Copy file or directories recursively.
dtutil    #Utility to fetch and manage hadoop delegation tokens inside credentials files
fs    #This command is documented in the File System Shell Guide.
gridmix    #Gridmix is a benchmark tool for Hadoop cluster
jar    #Runs a jar file.
jnipath    #Print the computed java.library.path.
kerbname    #Convert the named principal via the auth_to_local rules to the Hadoop user name.
kdiag    #Diagnose Kerberos Problems
key    #Manage keys via the KeyProvider
kms    #Run KMS, the Key Management Server.
trace    #View and modify Hadoop tracing settings
version    #Prints the version.
CLASSNAME    #Runs the class named CLASSNAME. The class must be part of a package.
envvars    #Display computed Hadoop environment variables.
```


<br/>
<br/>


#### 管理命令

```bash
# Usage
hadoop daemonlog -getlevel <host:port> <classname> [-protocol (http|https)]
hadoop daemonlog -setlevel <host:port> <classname> <level> [-protocol (http|https)]


# 栗子
bin/hadoop daemonlog -setlevel 127.0.0.1:9870 org.apache.hadoop.hdfs.server.namenode.NameNode DEBUG
bin/hadoop daemonlog -getlevel 127.0.0.1:9871 org.apache.hadoop.hdfs.server.namenode.NameNode DEBUG -protocol https



# 还有以下守护进程
Common    #key management server
HDFS    #name node, secondary name node, data node, journal node, HttpFS server
YARN    #resource manager, node manager, Timeline server
```



<br/>
<br/>



### 文件


```
# hadoop shell commands全局设置
etc/hadoop/hadoop-env.sh


# 此文件允许高级用户覆盖某些shell 功能
etc/hadoop/hadoop-user-functions.sh


# 用户个人环境
~/.hadooprc
```





<br/>
<br/>
<br/>




## FS Shell

FileSystem Shell

FS Shell包括了各种类似于shell的命令，它们直接与Hadoop分布式文件系统(HDFS)以及Hadoop支持的其它文件系统交互(如: Local FS, WebHDMIFS, S3 FS...)

All FS shell commands take path URIs as arguments.


<br>

```bash
# 调用方式
bin/hadoop fs <args>


# Append single src, or multiple srcs from local file system to the destination file system
 hadoop fs -appendToFile <localsrc> ... <dst>


# Copies source paths to stdout.
hadoop fs -cat [-ignoreCrc] URI [URI ...]


# Returns the checksum information of a file
hadoop fs -checksum URI


# Change group association of files
hadoop fs -chgrp [-R] GROUP URI [URI ...]


# Similar to the fs -put command, except that the source is restricted to a local file reference.
hadoop fs -copyFromLocal <localsrc> URI


# Similar to get command, except that the destination is restricted to a local file reference
hadoop fs -copyToLocal [-ignorecrc] [-crc] URI <localdst>


# Count the number of directories, files and bytes under the paths that match the specified file pattern
hadoop fs -count [-q] [-h] [-v] [-x] [-t [<storage type>]] [-u] [-e] <paths>


# Copy files from source to destination
hadoop fs -cp [-f] [-p | -p[topax]] URI [URI ...] <dest>


createSnapshot
deleteSnapshot
renameSnapshot


# Displays free space
hadoop fs -df [-h] URI [URI ...]


# Displays sizes of files and directories contained in the given directory or the length of a file in case its just a file.
hadoop fs -du [-s] [-h] [-v] [-x] URI [URI ...]


# Displays a summary of file lengths.
hadoop fs -dus <args>


# Permanently delete files in checkpoints older than the retention threshold from trash directory, and create new checkpoint.
hadoop fs -expunge


# Finds all files that match the specified expression and applies selected actions to them
hadoop fs -find <path> ... <expression> ...


# Copy files to the local file system
hadoop fs -get [-ignorecrc] [-crc] [-p] [-f] <src> <localdst>


# Copy files to the local file system
hadoop fs -get [-ignorecrc] [-crc] [-p] [-f] <src> <localdst>


# Displays the Access Control Lists (ACLs) of files and directories
hadoop fs -getfacl [-R] <path>


# Displays the extended attribute names and values (if any) for a file or directory
hadoop fs -getfattr [-R] -n name | -d [-e en] <path>


# Takes a source directory and a destination file as input and concatenates files in src into the destination local file
hadoop fs -getmerge [-nl] <src> <localdst>


# Displays first kilobyte of the file to stdout.
hadoop fs -head URI


# Return usage output.
hadoop fs -help


hadoop fs -ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [-e] <args>


# Recursive version of ls
hadoop fs -lsr <args>


# Takes path uri’s as argument and creates directories.
hadoop fs -mkdir [-p] <paths>


# Similar to put command, except that the source localsrc is deleted after it’s copied.
hadoop fs -moveFromLocal <localsrc> <dst>


# Displays a “Not implemented yet” message.
hadoop fs -moveToLocal [-crc] <src> <dst>


# Moves files from source to destination. This command allows multiple sources as well in which case the destination needs to be a directory. Moving files across file systems is not permitted.
hadoop fs -mv URI [URI ...] <dest>


# Copy single src, or multiple srcs from local file system to the destination file system. Also reads input from stdin and writes to destination file system if the source is set to “-”
hadoop fs -put [-f] [-p] [-l] [-d] [ - | <localsrc1> .. ]. <dst>


# Delete files specified as args
hadoop fs -rm [-f] [-r |-R] [-skipTrash] [-safely] URI [URI ...]


# Delete a directory
hadoop fs -rmdir [--ignore-fail-on-non-empty] URI [URI ...]


# Recursive version of delete
hadoop fs -rmr [-skipTrash] URI [URI ...]


# Sets Access Control Lists (ACLs) of files and directories
hadoop fs -setfacl [-R] [-b |-k -m |-x <acl_spec> <path>] |[--set <acl_spec> <path>]


# Sets an extended attribute name and value for a file or directory
hadoop fs -setfattr -n name [-v value] | -x name <path>


# Changes the replication factor of a file. If path is a directory then the command recursively changes the replication factor of all files under the directory tree rooted at path. The EC files will be ignored when executing this command
hadoop fs -setrep [-R] [-w] <numReplicas> <path>


# Print statistics about the file/directory at <path> in the specified format
hadoop fs -stat [format] <path> ...


# Displays last kilobyte of the file to stdout
hadoop fs -tail [-f] URI


hadoop fs -test -[defsz] URI


# Takes a source file and outputs the file in text format. The allowed formats are zip and TextRecordInputStream.
hadoop fs -text <src>


# Updates the access and modification times of the file specified by the URI to the current time.
hadoop fs -touch [-a] [-m] [-t TIMESTAMP] [-c] URI [URI ...]


# Create a file of zero length. An error is returned if the file exists with non-zero length
hadoop fs -touchz URI [URI ...]


# Truncate all files that match the specified file pattern to the specified length.
hadoop fs -truncate [-w] <length> <paths>


# Return the help for an individual command
hadoop fs -usage command

```


<br/>


### 使用对象存储

Working with Object Storage

The Hadoop FileSystem shell works with Object Stores such as Amazon S3, Azure WASB and OpenStack Swift.




<br/>
<br/>
<br/>




## 兼容性规范

[Apache Hadoop Compatibility Specification](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/Compatibility.html)


<br/>


### 目的


本节介绍Apache Hadoop项目的兼容性目标。所有Hadoop Interface都根据目标受众和稳定性进行分类，以保持与先前版本的兼容性。

本文档供Hadoop开发人员社区使用。










<br/>
<br/>
<br/>



## 开发者兼容指南

[Apache Hadoop Downstream Developer’s Guide](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/DownstreamDev.html)


<br/>


### 目的

本文档的目的是为下游开发人员提供明确的参考，以便在针对Hadoop源代码库构建应用程序时提供什么。本文档主要是Hadoop兼容性指南的精华，因此重点介绍了跨版本的各种Hadoop接口的兼容性保证。



















<br/>
<br/>
<br/>



## 管理兼容指南

[Apache Hadoop Admin Compatibility Guide](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/AdminCompatibilityGuide.html)


<br/>


### 目的


本文档的目的是将Hadoop兼容性指南提炼为与系统管理员相关的信息。

目标受众是负责维护Apache Hadoop集群以及必须规划和执行集群升级的管理员。



<br/>
<br/>



### 发行版

Hadoop Releases


Hadoop开发社区定期发布新的Hadoop Release，以引入新功能并修复现有问题。发新版分为三类:

- **Major**: 主要版本通常包含重要的新功能，通常代表最大的升级兼容性风险。(如 2.8.2 to 3.0.0)
- **Minor**: 次要版本通常会包含一些新功能以及针对某些值得注意的问题的修复程序。在大多数情况下，次要版本不应造成太大的升级风险。(如2.8.2 to 2.9.0)
- **Maintenance**: 维护版本不应包含任何新功能。维护版本的目的是解决开发人员社区认为足够重要的一组问题，以便推动新版本解决这些问题。维护版本的升级风险很小。(如2.8.2 to 2.8.3)



<br/>
<br/>



### 平台依赖

Platform Dependencies


Hadoop所依赖的本机组件集被视为Hadoop ABI的一部分。Hadoop开发社区致力于尽可能地保持ABI兼容性。在次要版本之间，除非必要，否则不会增加Hadoop本机依赖项的最低支持版本号，例如安全性或许可问题。

Hadoop依赖于JVM(Java Virtual Machine)。支持的最低版本的JVM在主要版本的Hadoop之间不会发生变化。


<br/>
<br/>


### 网络

Network


Hadoop依赖于某些传输层技术，如SSL。除非必要，否则不会增加这些依赖项的最低支持版本，例如安全性或许可问题。

Hadoop服务端口号将在主要版本中保持不变，但可能会在次要版本中更改。

Hadoop内部线程协议(wire protocol)将在同一主要版本中的次要版本进行向后和向前兼容，以实现滚动升级。



<br/>
<br/>



### 脚本和自动化

Scripting and Automation


<br>

#### REST APIs

Hadoop REST APTs提供了一种简单的机制，用于收集有关Hadoop系统状态的信息。为了支持REST客户端，Hadoop REST API是版本化的，并且在版本中不会发生不兼容的更改。
REST API版本是单个数字，与Hadoop版本号无关。


<br/>
<br/>


#### 解析Hadoop输出

Parsing Hadoop Output

Hadoop可以生成各种输出，可通过自动化工具进行解析。在使用Hadoop输出时，请考虑一下事项:

- 除非解决了正确性问题，否则Hadoop日志输出不会随维护版本而更改
- Hadoop为各种操作生成审计日志(audit log)。审计日志旨在是机器可读，但新纪录和字段的添加被认为是兼容的更改
- Hadoop生成的度量数量(metrics data)主要用于自动化处理。


<br/>
<br/>


#### CLIs

Hadoop的命令行集提供了管理系统各个方面以及发现系统状态信息的能力。请注意，CLI工具输出与CLI工具生成的日志输出不同。日志输出不适合自动消费，可能随时更改。


<br/>
<br/>


#### Web UI

Hadoop公开的Web UI供人类使用。



<br/>
<br/>



### 状态数据

Hadoop State Data


Hadoop内部系统状态是私有的，不应直接被修改。以下策略管理各种内部状态存储的升级特征:

- 内部MapReduce状态数据在同一主要版本中的次要版本之间保持兼容，以便在MapReduce工作负载执行时促进滚动升级
- HDFS以版本化的私有内部格式维护存储在HDFS中的数据的元数据。
- AWS S3防护保留了版本化的私有内部元数据存储。不兼容的更改将导致版本号递增。
- YARN 资源管理器保留版本化的应用程序和调度程序信息的内部状态存储。
- YARN联合身份验证服务保留应用程序的私有内部状态存储以及版本化的群集信息。



<br/>
<br/>



### 配置

Hadoop Configurations


Hadoop使用两种主要形式的配置文件: XML配置文件和日志记录配置文件。

<br>

**XML配置文件**

XML配置文件包含了一组属性作为键值对。属性的名称和含义由Hadoop定义，并保证在次要版本中稳定。属性只能在主要版本中删除，并且只有在至少完整主要版本被标记为已弃用时才能删除。大多数属性都有一个默认值，如果未在XML配置文件中显式设置该属性，则将使用该值。

下游项目和用户可以将自己的属性添加到XML配置文件中，以供其工具和应用程序使用。虽然Hadoop对定义新属性没有任何正式限制，但与Hadoop定义的属性冲突的新属性可能会导致意外和不良结果。建议用户避免使用与Hadoop定义的属性的名称空间冲突的自定义配置属性名称。

<br>

**日志记录配置文件**

Hadoop Daemon和CLI生成的日志输出由一组配置文件控制。这些文件控制将由Hadoop的各个组件输出的最小日志消息级别，以及这些消息的存储位置和方式。

<br>

**其它配置文件**

Hadoop使用各种格式的许多其它类型的配置文件，如JSON或XML。



<br/>
<br/>



### 发行版

Hadoop Distribution

<br>

**配置文件**

Hadoop配置文件的位置和一般结构，作业历史信息和Hadoop生成的日志文件将在维护版中得到维护。

<br>

**JARs**

Hadoop发行版的内容，如JAR文件可能随时更改，Client artifact除外，不应视为可靠。当前客户端工具有:

- hadoop-client
- hadoop-client-api
- hadoop-client-minicluster
- hadoop-client-runtime
- hadoop-hdfs-client
- hadoop-hdfs-native-client
- hadoop-mapreduce-client-app
- hadoop-mapreduce-client-common
- hadoop-mapreduce-client-core
- hadoop-mapreduce-client-jobclient
- hadoop-mapreduce-client-nativetask
- hadoop-yarn-client

<br>

**ENV**

一些Hadoop组件通过环境变量接收信息。

<br>

**库依赖**

Hadoop依赖于大量第三方库来运行。

<br>

**硬件和系统依赖**

Hadoop目前由运行在x86和AMD处理器上的Linux和Windows上的Hadoop开发人员社区提供支持。
无法保证Hadoop守护程序所需的最低资源如何在发行版之间发生变化，甚至是维护版本。
任何支持Hadoop的文件系统，例如通过FileSystem API，在大多数情况下将继续在主要版本中得到支持。




<br/>
<br/>
<br/>




## 接口分类

[Interface Classification](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/InterfaceClassification.html)














<br/>
<br/>
<br/>




## 文件系统规范

[The Hadoop FileSystem API Definition](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/filesystem/index.html)


这是Hadoop FS API的规范，它将文件系统的内容建模为一组路径(目录、文件、符号链接)。
Unix文件系统有多种规范作为inode树，但没有任何公开定义 *Unix文件系统作为数据存储访问的概念模型* 的概念。

该规范视图这样做，定义Hadoop FS模型和API，以便多个文件系统可实现API并向应用程序提供其数据的一致模型。除了记录HDFS所展示的行为之外，它不会尝试正式指定文件系统的任何并发行为，因为这些行为是Hadoop客户端应用程序通常所期望的。


<br/>


### 介绍






<br/>
<br/>



### 注释

Notation





<br/>
<br/>



### 模型

Model




<br/>
<br/>



### 文件系统类

FileSystem class






<br/>
<br/>



### FSDataInputStream class





<br/>
<br/>



### FSDataOutputStreamBuilder class



<br/>
<br/>



### 使用文件系统规范进行测试

Testing with the Filesystem specification




<br/>
<br/>



### 扩展规范及测试

Extending the specification and its tests






















<br/>
<br/>

---

<br/>
<br/>















# Common


<br/>


## CLI MiniCluster


<br/>


### 目的

使用 CLI MiniCluster，用户只需使用一个命令即可启动和停止单节点Hadoop集群，而无需设置任何环境变量或管理配置文件。CLI MiniCluster启用 YARN、MapReduce和HDFS集群。

这对于希望快速试验Hadoop集群或测试依赖于Hadoop工具的程序的用户来说非常有用。



<br/>
<br/>



### Hadoop Tarball

你要从发行版中获取Hadoop Tarball。此外，你也可以从源直接创建Tarball:

```bash
# 请首先下载源码包并解压
# tarball在hadoop-dist/target/目录
# cd hadoop-3.2.0-src
mvn clean install -DskipTests
mvn package -Pdist -Dtar -DskipTests -Dmaven.javadoc.skip
```



<br/>
<br/>



### 运行MiniCluster

从提取的Tarball目录内部，你可使用以下命令启动CLI MiniCluster:

```bash
# RM_PORT, JHS_PORT应替换为用户对这些端口号的选择，如果未指定，将使用随机空闲端口
bin/mapred minicluster -rmport RM_PORT -jhsport JHS_PORT



# 命令行参数
$ -D <property=value>    Options to pass into configuration object
$ -datanodes <arg>       How many datanodes to start (default 1)
$ -format                Format the DFS (default false)
$ -help                  Prints option help.
$ -jhsport <arg>         JobHistoryServer port (default 0--we choose)
$ -namenode <arg>        URL of the namenode (default is either the DFS
$                        cluster or a temporary dir)
$ -nnport <arg>          NameNode port (default 0--we choose)
$ -nnhttpport <arg>      NameNode HTTP port (default 0--we choose)
$ -nodemanagers <arg>    How many nodemanagers to start (default 1)
$ -nodfs                 Don't start a mini DFS cluster
$ -nomr                  Don't start a mini MR cluster
$ -rmport <arg>          ResourceManager port (default 0--we choose)
$ -writeConfig <path>    Save configuration to this XML file.
$ -writeDetails <path>   Write basic information to this JSON file.
```



<br/>
<br/>
<br/>



## 原生库

Native Libraries Guide


本节介绍原生(native)Hadoop库，并包含了有关共享库的讨论。

出于性能原因和Java实现的不可用性，Hadoop具有某些组件的原生实现。这些组件在单个动态链接的本机库可用，称为本机(原生)Hadoop库(`libhadoop.so`)。


<br/>


### 使用

使用原生Hadoop库相当容易:

- 审阅组件
- 审阅支持的平台
- 下载Hadoop发行版(库名: `libhadoop.so`)
- 安装解码器开发包(`>zlib-1.2`, `>gzip-1.2`)
- 检查运行日志



<br/>
<br/>



### 组件

原生Hadoop库包括各种组件:

- Compression Codecs (`bzip2`, `lz4`, `snappy`, `zlib`)
- Native IO utilities for HDFS Short-Circuit Local Reads and Centralized Cache Management in HDFS
- CRC32 checksum implementation



<br/>
<br/>



### 支持的平台

原生Hadoop库主要用于GNU/Linux平台，并在这些发行版上进行测试:

- RHEL4/Fedora
- Ubuntu
- Gentoo



<br/>
<br/>



### 构建

原生Hadoop库使用 ANSI C编写，使用GNU autotools-chain 构建。
你需要在目标平台上安装的软件包:

- C compiler (e.g. GNU C Compiler)
- GNU Autools Chain: autoconf, automake, libtool
- zlib-development package (stable version >= 1.2.0)
- openssl-development package(e.g. libssl-dev)

安装必备软件包后，使用标准的Hadoop `Pox.xml` 文件来构建原生Hadoop库:

```bash
mvn package -Pdist,native -DskipTests -Dtar

# You should see the newly-built library in
hadoop-dist/target/hadoop-3.2.0/lib/native
```



<br/>
<br/>



### Runtime

`bin/hadoop` 脚本通过系统属性(`-Djava.library.path=<path>`)确保原生Hadoop库位于库路径上。

在运行时，检查hadoop日志文件以查找MapReduce任务。



<br/>
<br/>


### 检查

```
# 原生库检查其检查是否正确加载
hadoop checknative -a
```




<br/>
<br/>
<br/>




## Proxy User

[Proxy user - Superusers Acting On Behalf Of Other Users](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/Superusers.html)


本节介绍超级用户(super user)如何代表另一个用户提交作业(submit job)或访问HDFS。


<br/>


### 用例

用户名为`super`的超级用户希望代表用户`userA`提交作业并访问HDFS。某人任务需要以`userA`身份运行，并且namenode上的任何文件都需要以`userA`的身份完成。这要求用户`userA`可连接到使用`super`用户的kerberos凭据连接到namenode。换句话说，`super`模仿用户`userA`。



<br/>
<br/>



### 代码栗子

`super`超级用户的凭据用于登录，并为`joe`创建代理用户对象。

```
    ...
    //Create ugi for joe. The login user is 'super'.
    UserGroupInformation ugi =
            UserGroupInformation.createProxyUser("joe", UserGroupInformation.getLoginUser());
    ugi.doAs(new PrivilegedExceptionAction<Void>() {
      public Void run() throws Exception {
        //Submit a job
        JobClient jc = new JobClient(conf);
        jc.submitJob(conf);
        //OR access hdfs
        FileSystem fs = FileSystem.get(conf);
        fs.mkdir(someFilePath);
      }
    }
```



<br/>
<br/>



### 配置

你可以使用`hadoop.proxyuser`属性(properties)来配置代理用户。`$superuser.hosts`以及`hadoop.proxyuser.$superuser.groups`, `hadoop.proxyuser.$superuser.users`其中的一个或两个。

在`core-site.xml`中，名为`super`的超级用户只能充`host1, host2`上进行连接，用于模拟`group1`, `group2`。

```
   <property>
     <name>hadoop.proxyuser.super.hosts</name>
     <value>host1,host2</value>
   </property>
   <property>
     <name>hadoop.proxyuser.super.groups</name>
     <value>group1,group2</value>
   </property>
```

<br>

如果需要更为宽松的安全性，则可以使用通配符:

```
  <property>
    <name>hadoop.proxyuser.oozie.hosts</name>
    <value>*</value>
  </property>
  <property>
    <name>hadoop.proxyuser.oozie.groups</name>
    <value>*</value>
  </property>
```

<br>

当然，它也接受CIDR格式的ip地址范围或主机名:

```
   <property>
     <name>hadoop.proxyuser.super.hosts</name>
     <value>10.222.0.0/16,10.113.221.221</value>
   </property>
   <property>
     <name>hadoop.proxyuser.super.users</name>
     <value>user1,user2</value>
   </property>
```



<br/>
<br/>



### 警告

如果集群运行在安全模式(secure mode)下，则超级用户必须具有`kerberos`凭据才能模拟其他用户。




<br/>
<br/>
<br/>




## Rack Awareness

[Rack Awareness](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/RackAwareness.html)


Hadoop组件具有机架感知功能(rack-aware)。例如，通过将一个块副本(block replica)放在不同的机架上，HDFS块放置将使用机架感知来实现容错(fault tolerance)。可以在网络故障或分区时提供数据可用性。

Hadoop主守护进程(master daemon)通过调用配置文件指定的`external scripts`或`java class`来获取集群工作者的**rack id**。输出必须遵守 java org.apache.hadoop.net.DNSToSwitchMapping interface，接口需要保持一对一的对应关系。

要使用 java class 进行拓扑映射，类名由配置文件中的 `net.topology.node.switch.mapping.impl` 参数指定。
如果想实现外部脚本，将使用配置文件中的 `net.topology.script.file.name` 参数指定它。
如果 `net.topology.script.file.name` 或 `net.topology.node.switch.mapping.impl` 没有设置，则会为任何IP返回机器ID。

<br/>


### python栗子

```py
#!/usr/bin/python
# this script makes assumptions about the physical environment.
#  1) each rack is its own layer 3 network with a /24 subnet, which
# could be typical where each rack has its own
#     switch with uplinks to a central core router.
#
#             +-----------+
#             |core router|
#             +-----------+
#            /             \
#   +-----------+        +-----------+
#   |rack switch|        |rack switch|
#   +-----------+        +-----------+
#   | data node |        | data node |
#   +-----------+        +-----------+
#   | data node |        | data node |
#   +-----------+        +-----------+
#
# 2) topology script gets list of IP's as input, calculates network address, and prints '/network_address/ip'.

import netaddr
import sys
sys.argv.pop(0)                                                  # discard name of topology script from argv list as we just want IP addresses

netmask = '255.255.255.0'                                        # set netmask to what's being used in your environment.  The example uses a /24

for ip in sys.argv:                                              # loop over list of datanode IP's
    address = '{0}/{1}'.format(ip, netmask)                      # format address string so it looks like 'ip/netmask' to make netaddr work
    try:
        network_address = netaddr.IPNetwork(address).network     # calculate and print network address
        print "/{0}".format(network_address)
    except:
        print "/rack-unknown"                                    # print catch-all value if unable to calculate network address
```



<br/>
<br/>



### bash栗子

```bash
#!/usr/bin/env bash
# Here's a bash example to show just how simple these scripts can be
# Assuming we have flat network with everything on a single switch, we can fake a rack topology.
# This could occur in a lab environment where we have limited nodes,like 2-8 physical machines on a unmanaged switch.
# This may also apply to multiple virtual machines running on the same physical hardware.
# The number of machines isn't important, but that we are trying to fake a network topology when there isn't one.
#
#       +----------+    +--------+
#       |jobtracker|    |datanode|
#       +----------+    +--------+
#              \        /
#  +--------+  +--------+  +--------+
#  |datanode|--| switch |--|datanode|
#  +--------+  +--------+  +--------+
#              /        \
#       +--------+    +--------+
#       |datanode|    |namenode|
#       +--------+    +--------+
#
# With this network topology, we are treating each host as a rack.  This is being done by taking the last octet
# in the datanode's IP and prepending it with the word '/rack-'.  The advantage for doing this is so HDFS
# can create its 'off-rack' block copy.
# 1) 'echo $@' will echo all ARGV values to xargs.
# 2) 'xargs' will enforce that we print a single argv value per line
# 3) 'awk' will split fields on dots and append the last field to the string '/rack-'. If awk
#    fails to split on four dots, it will still print '/rack-' last field value

echo $@ | xargs -n 1 | awk -F '.' '{print "/rack-"$NF}'
```




<br/>
<br/>
<br/>



## 安全模式

[Hadoop in Secure Mode](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/SecureMode.html)


本节介绍如何在 **安全模式(secure mode)** 下为Hadoop配置身份认证。当Hadoop配置为以安全模式运行时，Hadoop的每个服务和每个用户都必须由Kerberos进行身份认证。
必须正确配置所有服务主机的正向(forward)和反向(reverse)查找，以允许服务互相进行身份验证。可使用DNS或`/etc/hosts`文件配置主机查找(lookup)。在尝试以安全模式配置Hadoop服务之前，建议先了解Kerberos和DNS的工作知识。


<br/>
<br/>


### 认证

Authentication


<br>


#### End User Accounts

启用服务级别身份验证之后，最终用户必须在与Hadoop服务交互之前进行身份认证。最简单的方式是用户使用`kerberos kinit cmd`以交互方式进行身份认证，或者使用`Kerberos keytab`文件的编程身份进行认证。


<br/>
<br/>


#### 用户账户

User Accounts for Hadoop Daemons

确保HDFS和YARN 守护进程以不同的Unix用户运行(如hdfs, yarn)。此外，确保 MapDrduce JobHistory Server以不同的用户运行(如mapred)。
建议让他们共享同一个Unix Group。

| User:Group | Daemons |
| - | - |
| hdfs:hadoop | NameNode, Secondary NameNode, JournalNode, DataNode |
| yarn:hadoop | ResourceManager, NodeManager |
| mapred:hadoop | MapReduce JobHistory Server |


<br/>
<br/>


#### Kerberos

Kerberos principals for Hadoop Daemons

必须使用其**Kerberos pricipal**和**keytab file**配置每个Hadoop Service 实例。服务准则的一般格式是: `ServiceName/_HOST@REALM.TLD. e.g. dn/_HOST@EXAMPLE.COM`。

Hadoop通过允许将服务主体的主机名组件指定为`_HOST`通配符来简化配置文件的部署。每个服务实例将在运行时使用自己的完全限定主机名替换`_HOST`。这允许管理员在所有节点上部署同一组配置文件，但 keytab 文件有所不同。


<br/>


**HDFS**

每个NameNode主机上的**keytab**文件，如下所示:

```bash
# -e shows the encryption type
# -t shows keytab entry timestamps
# -k specifies keytab
klist -e -k -t /etc/security/keytab/nn.service.keytab
Keytab name: FILE:/etc/security/keytab/nn.service.keytab
KVNO Timestamp         Principal
   4 07/18/11 21:08:09 nn/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 nn/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 nn/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
```

第二个NameNode主机的**keytab**文件，如下所示:

```bash
klist -e -k -t /etc/security/keytab/sn.service.keytab
Keytab name: FILE:/etc/security/keytab/sn.service.keytab
KVNO Timestamp         Principal
   4 07/18/11 21:08:09 sn/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 sn/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 sn/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
```

每台主机上的DataNode的**keytab**文件，如下所示:

```bash
klist -e -k -t /etc/security/keytab/dn.service.keytab
Keytab name: FILE:/etc/security/keytab/dn.service.keytab
KVNO Timestamp         Principal
   4 07/18/11 21:08:09 dn/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 dn/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 dn/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
```


<br/>


**YARN**

位于ResourceManager主机上的ResourceManager的**keytab**文件，如下所示:

```
klist -e -k -t /etc/security/keytab/rm.service.keytab
Keytab name: FILE:/etc/security/keytab/rm.service.keytab
KVNO Timestamp         Principal
   4 07/18/11 21:08:09 rm/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 rm/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 rm/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
```

每台主机上的NodeManager的**keytab**文件，如下所示:

```bash
klist -e -k -t /etc/security/keytab/nm.service.keytab
Keytab name: FILE:/etc/security/keytab/nm.service.keytab
KVNO Timestamp         Principal
   4 07/18/11 21:08:09 nm/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 nm/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 nm/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
```


<br/>


**MapReduce JobHistory Server**

该主机上的MapReduce JobHistory Server的**keytab**文件，如下所示:

```bash
klist -e -k -t /etc/security/keytab/jhs.service.keytab
Keytab name: FILE:/etc/security/keytab/jhs.service.keytab
KVNO Timestamp         Principal
   4 07/18/11 21:08:09 jhs/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 jhs/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 jhs/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-256 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (AES-128 CTS mode with 96-bit SHA-1 HMAC)
   4 07/18/11 21:08:09 host/full.qualified.domain.name@REALM.TLD (ArcFour with HMAC/md5)
```


<br/>
<br/>


#### 从kerberos映射到系统用户账户

Mapping from Kerberos principals to OS user accounts

Hadoop使用`hadoop.security.auth_to_local`指定的规则将kerberos principal映射到系统账户。Hadoop如何评估这些规则取决于`hadoop.security.auth_to_local.mechanism`的设置。

在默认的hadoop模式下，必须将Kerberos主体与将主体转换为简单形式的规则匹配，即不带`@, /`的用户帐户名，否则将不会授权主体并记录错误。
另外，请注意，您不应该将 `auth_to_local` 规则作为ACL并使用适当的(OS)机制。

`auth_to_local`可能的值:

- `RULE:exp`, 本地名称将由exp指定
- `DEFAULT`, 当且仅当域与 `default_realm` 匹配时，才将主体名称的第一个组件选为系统用户名

请注意，Hadoop不支持多个默认域。此外，Hadoop不会对映射是否存在本地系统帐户进行验证。

<br/>
<br/>


#### 规则栗子

Example rules

在典型的集群中，HDFS和YARN服务将分别作为系统hdfs和yarn用户启动。`hadoop.security.auth_to_local`可做如下配置:

```
<property>
  <name>hadoop.security.auth_to_local</name>
  <value>
    RULE:[2:$1/$2@$0]([ndj]n/.*@REALM.\TLD)s/.*/hdfs/
    RULE:[2:$1/$2@$0]([rn]m/.*@REALM\.TLD)s/.*/yarn/
    RULE:[2:$1/$2@$0](jhs/.*@REALM\.TLD)s/.*/mapred/
    DEFAULT
  </value>
</property>
```

可以使用 `hadoop kerbname` 命令测试自定义规则。此命令允许指定主体并应用Hadoop的当前 `auth_to_local` 规则集。


<br/>
<br/>


#### 用户到组的映射

Mapping from user to group

可以通过 `hadoop.security.group.mapping` 配置系统用户到系统组的映射机制。
实际上，您需要使用Kerberos和LDAP for Hadoop以安全模式管理SSO环境。


<br/>
<br/>


#### 代理用户

Proxy user

Some products such as Apache Oozie which access the services of Hadoop on behalf of end users need to be able to impersonate end users.


<br/>
<br/>


#### Secure DataNode

由于DataNode数据传输协议不使用Hadoop RPC框架，因此DataNode必须使用由`dfs.datanode.address`和`dfs.datanode.http.address`指定的特权端口对自身进行身份验证。此认证基于以下假设: 攻击者无法在DataNode主机上获得root权限。

当以root身份执行`hdfs datanode`命令时，服务进程首先绑定特定端口，然后删除特权并以`HDFS_DATANODE_SECURE_USER`指定的用户账户运行。此启动过程安装到`JSVC_HOME`的jsvc程序。你必须在启动时将`JDFS_DATANODE_SECURE_USER`和`JSVC_HOME`指定为环境变量。(`hadoop-env.sh`文件中可配置)



<br/>
<br/>
<br/>



### 数据机密性


<br/>


#### Data Encryption on RPC

在Hadoop Service 和 Client之间传输的数据可以在线路上加密。在 `core-site.xml` 中将 `hadoop.rpc.protection` 设置为 `privacy` 可激活数据加密。


<br/>
<br/>


#### Data Encryption on Block data transfer

需要在 `hdfs-site.xml` 中将 `dfs.encrypt.data.transfer` 设置为 `true`，以便为DataNode 的数据传输协议激活数据加密。

或者，你可将 `dfs.encrypt.data.transfer.algorithm` 设置为 `3des`, `rc4` 以选择特定的加密算法。如果未指定，则使用系统上配置的JCE默认值(3des)。
将 `dfs.encrypt.data.transfer.cipher.suites` 设置为 `AES/CTR/NoPadding` 可激活AES加密。默认未指定，也就是不使用AES。使用AES时，在初始密钥交换期间仍会使用 `dfs.encrypt.dta.transfer.algorithm`的算法。可通过将 `dfs.encrypt.data.transfer.cipher.key.bitlength` 设置为128, 192, 256来配置AES密钥位长度(默认128)

AES提供最大的加密强度和最佳性能。目前，3DES和RC4在Hadoop集群中的使用频率更高。


<br/>
<br/>


#### Data Encryption on HTTP

web-console和client之间的数据传输使用SSL(HTTPS)保护。在使用Kerberos配置Hadoop安全时，推荐使用SLL，但不是必须。

要为HDFS Daemon的 web-console 启用SSL，将`hdfs-site.xml`文件中的`dfs.http.policy`设置为`HTTPS_ONLY`或`HTTP_AND_HTTPS`两者之一。
要为YARN Daemon的 web-console 启用SSL，将`yarn-site.xml`文件中的`yarn.http.policy`设置为`HTTPS_ONLY`。
要为MapReduce JobHistory Server的 web-console 启用SSL，将`mapred-site.xml`文件中的`mapreduce.jobhistory.http.policy`设置为`HTTPS_ONLY`。



<br/>
<br/>



### 配置


<br/>


#### HDFS和本地文件系统路径的权限

Permissions for both HDFS and local fileSystem paths

下表列出了HDFS和本地文件系统的各种路径，建议权限为:

| Filesystem | Path | User:Group | Permissions |
| - | - | - | - |
| local | `dfs.namenode.name.dir` | hdfs:hadoop | drwx------ |
| local | `dfs.datanode.data.dir` | hdfs:hadoop | drwx------ |
| local | `$HADOOP_LOG_DIR` | hdfs:hadoop | drwxrwxr-x |
| local | `$YARN_LOG_DIR` | yarn:hadoop | drwxrwxr-x |
| local | `yarn.nodemanager.local-dirs` | yarn:hadoop | drwxr-xr-x |
| local | `yarn.nodemanager.log-dirs` | yarn:hadoop | drwxr-xr-x |
| local | `container-executor` | root:hadoop | --Sr-s--* |
| local | `conf/container-executor.cfg` | root:hadoop | r-------* |
| hdfs | `/` | hdfs:hadoop | drwxr-xr-x |
| hdfs | `/tmp` | hdfs:hadoop | drwxrwxrwxt |
| hdfs | `/user` | hdfs:hadoop | drwxr-xr-x |
| hdfs | `yarn.nodemanager.remote-app-log-dir` | yarn:hadoop | drwxrwxrwxt |
| hdfs | `mapreduce.jobhistory.intermediate-done-dir` | mapred:hadoop | drwxrwxrwxt |
| hdfs | `mapreduce.jobhistory.done-dir` | mapred:hadoop | drwxr-x--- |


<br/>
<br/>


#### 常见配置

要在Hadoop中启用RPC身份认证，请将`hadoop.security.authentication`属性设置为`kerberos`，并适当地设置下面列出的安全相关的配置。

以下属性应位于集群中所有节点的`core-site.xml`中:

| Parameter | Value | Notes |
| - | - | - |
| `hadoop.security.authentication` | `kerberos` | simple : No authentication. (default)  kerberos : Enable authentication by Kerberos. |
| `hadoop.security.authorization` | `true` | Enable RPC service-level authorization. |
| `hadoop.rpc.protection` | `authentication` | authentication : authentication only (default); integrity : integrity check in addition to authentication; privacy : data encryption in addition to integrity |
| `hadoop.security.auth_to_local` | `RULE:exp1 RULE:exp2 … DEFAULT` | The value is string containing new line characters. See Kerberos documentation for the format of exp. |
| `hadoop.proxyuser.superuser.hosts` | - | comma separated hosts from which superuser access are allowed to impersonation. * means wildcard. |
| `hadoop.proxyuser.superuser.groups` | -  | comma separated groups to which users impersonated by superuser belong. * means wildcard. |


<br/>
<br/>


#### NameNode

| Parameter | Value | Notes |
| - | - | - |
| `dfs.block.access.token.enable` | `true` | Enable HDFS block access tokens for secure operations. |
| `dfs.namenode.kerberos.principal` | `nn/_HOST@REALM.TLD` | Kerberos principal name for the NameNode. |
| `dfs.namenode.keytab.file` | `/etc/security/keytab/nn.service.keytab` | Kerberos keytab file for the NameNode. |
| `dfs.namenode.kerberos.internal.spnego.principal` | `HTTP/_HOST@REALM.TLD` | The server principal used by the NameNode for web UI SPNEGO authentication. The SPNEGO server principal begins with the prefix HTTP/ by convention. If the value is '*', the web server will attempt to login with every principal specified in the keytab file dfs.web.authentication.kerberos.keytab. For most deployments this can be set to ${dfs.web.authentication.kerberos.principal} i.e use the value of dfs.web.authentication.kerberos.principal. |
| `dfs.web.authentication.kerberos.keytab` | `/etc/security/keytab/spnego.service.keytab` | SPNEGO keytab file for the NameNode. In HA clusters this setting is shared with the Journal Nodes. |

以下设置允许配置对NameNode Web UI的SSL访问(可选):

| Parameter | Value | Notes |
| - | - | - |
| `dfs.http.policy` | `HTTP_ONLY` or `HTTPS_ONLY` or `HTTP_AND_HTTPS` | HTTPS_ONLY turns off http access. This option takes precedence over the deprecated configuration dfs.https.enable and hadoop.ssl.enabled. If using SASL to authenticate data transfer protocol instead of running DataNode as root and using privileged ports, then this property must be set to HTTPS_ONLY to guarantee authentication of HTTP servers. (See dfs.data.transfer.protection.) |
| `dfs.namenode.https-address` | `0.0.0.0:9871` | This parameter is used in non-HA mode and without federation. See HDFS High Availability and HDFS Federation for details. |
| `dfs.https.enable` | `true` | This value is deprecated. Use dfs.http.policy |


<br/>
<br/>


#### Secondary NameNode

| Parameter | Value | Notes |
| - | - | - |
| `dfs.namenode.secondary.http-address` | `0.0.0.0:9868` | HTTP web UI address for the Secondary NameNode. |
| `dfs.namenode.secondary.https-address` | `0.0.0.0:9869` | HTTPS web UI address for the Secondary NameNode. |
| `dfs.secondary.namenode.keytab.file` | `/etc/security/keytab/sn.service.keytab` | Kerberos keytab file for the Secondary NameNode. |
| `dfs.secondary.namenode.kerberos.principal` | `sn/_HOST@REALM.TLD` | Kerberos principal name for the Secondary NameNode. |
| `dfs.secondary.namenode.kerberos.internal.spnego.principal` | `HTTP/_HOST@REALM.TLD` | The server principal used by the Secondary NameNode for web UI SPNEGO authentication. The SPNEGO server principal begins with the prefix HTTP/ by convention. If the value is '*', the web server will attempt to login with every principal specified in the keytab file dfs.web.authentication.kerberos.keytab. For most deployments this can be set to ${dfs.web.authentication.kerberos.principal} i.e use the value of dfs.web.authentication.kerberos.principal. |


<br/>
<br/>


#### JournalNode

| Parameter | Value | Notes |
| - | - | - |
| `dfs.journalnode.kerberos.principal` | `jn/_HOST@REALM.TLD` | Kerberos principal name for the JournalNode. |
| `dfs.journalnode.keytab.file` | `/etc/security/keytab/jn.service.keytab` | Kerberos keytab file for the JournalNode. |
| `dfs.journalnode.kerberos.internal.spnego.principal` | `HTTP/_HOST@REALM.TLD` | The server principal used by the JournalNode for web UI SPNEGO authentication when Kerberos security is enabled. The SPNEGO server principal begins with the prefix HTTP/ by convention. If the value is '*', the web server will attempt to login with every principal specified in the keytab file dfs.web.authentication.kerberos.keytab. For most deployments this can be set to ${dfs.web.authentication.kerberos.principal} i.e use the value of dfs.web.authentication.kerberos.principal. |
| `dfs.web.authentication.kerberos.keytab` | `/etc/security/keytab/spnego.service.keytab` | SPNEGO keytab file for the JournalNode. In HA clusters this setting is shared with the Name Nodes. |
| `dfs.journalnode.https-address` | `0.0.0.0:8481` | HTTPS web UI address for the JournalNode. |


<br/>
<br/>


#### DataNode

| Parameter | Value | Notes |
| - | - | - |
| `dfs.datanode.data.dir.perm` | `700` | - |
| `dfs.datanode.address` | `0.0.0.0:1004` | Secure DataNode must use privileged port in order to assure that the server was started securely. This means that the server must be started via jsvc. Alternatively, this must be set to a non-privileged port if using SASL to authenticate data transfer protocol. (See dfs.data.transfer.protection.) |
| `dfs.datanode.http.address` | `0.0.0.0:1006` | Secure DataNode must use privileged port in order to assure that the server was started securely. This means that the server must be started via jsvc. |
| `dfs.datanode.https.address` | `0.0.0.0:9865` | HTTPS web UI address for the Data Node. |
| `dfs.datanode.kerberos.principal` | `dn/_HOST@REALM.TLD` | Kerberos principal name for the DataNode. |
| `dfs.datanode.keytab.file` | `/etc/security/keytab/dn.service.keytab` | Kerberos keytab file for the DataNode. |
| `dfs.encrypt.data.transfer` | `false` | set to true when using data encryption |
| `dfs.encrypt.data.transfer.algorithm` | - | optionally set to 3des or rc4 when using data encryption to control encryption algorithm |
| `dfs.encrypt.data.transfer.cipher.suites` | - | optionally set to AES/CTR/NoPadding to activate AES encryption when using data encryption |
| `dfs.encrypt.data.transfer.cipher.key.bitlength` | - | optionally set to 128, 192 or 256 to control key bit length when using AES with data encryption |
| `dfs.data.transfer.protection` | - | authentication : authentication only; integrity : integrity check in addition to authentication; privacy : data encryption in addition to integrity This property is unspecified by default. Setting this property enables SASL for authentication of data transfer protocol. If this is enabled, then dfs.datanode.address must use a non-privileged port, dfs.http.policy must be set to HTTPS_ONLY and the HDFS_DATANODE_SECURE_USER environment variable must be undefined when starting the DataNode process. |


<br/>
<br/>


#### WebHDFS

| Parameter | Value | Notes |
| - | - | - |
| `dfs.web.authentication.kerberos.principal` | `http/_HOST@REALM.TLD` | Kerberos principal name for the WebHDFS. In HA clusters this setting is commonly used by the JournalNodes for securing access to the JournalNode HTTP server with SPNEGO. |
| `dfs.web.authentication.kerberos.keytab` | `/etc/security/keytab/http.service.keytab` | Kerberos keytab file for WebHDFS. In HA clusters this setting is commonly used the JournalNodes for securing access to the JournalNode HTTP server with SPNEGO. |


<br/>
<br/>


#### ResourceManager

| Parameter | Value | Notes |
| - | - | - |
| `yarn.resourcemanager.principal` | `rm/_HOST@REALM.TLD` | Kerberos principal name for the ResourceManager. |
| `yarn.resourcemanager.keytab` | `/etc/security/keytab/rm.service.keytab` | Kerberos keytab file for the ResourceManager. |
| `yarn.resourcemanager.webapp.https.address` | `${yarn.resourcemanager.hostname}:8090` | The https adddress of the RM web application for non-HA. In HA clusters, use yarn.resourcemanager.webapp.https.address.rm-id for each ResourceManager. See ResourceManager High Availability for details. |


<br/>
<br/>


#### NodeManager

| Parameter | Value | Notes |
| - | - | - |
| `yarn.nodemanager.principal` | `nm/_HOST@REALM.TLD` | Kerberos principal name for the NodeManager. |
| `yarn.nodemanager.keytab` | `/etc/security/keytab/nm.service.keytab` | Kerberos keytab file for the NodeManager. |
| `yarn.nodemanager.container-executor.class` | `org.apache.hadoop.yarn.server.nodemanager.LinuxContainerExecutor` | Use LinuxContainerExecutor. |
| `yarn.nodemanager.linux-container-executor.group` | `hadoop` | Unix group of the NodeManager. |
| `yarn.nodemanager.linux-container-executor.path` | `/path/to/bin/container-executor` | The path to the executable of Linux container executor. |
| `yarn.nodemanager.webapp.https.address` | `0.0.0.0:8044` | The https adddress of the NM web application. |


<br/>
<br/>


#### Configuration for WebAppProxy

WebAppProxy在应用程序和用户导出的Web应用程序之间提供代理。如果启用了安全性，它将在访问可能不安全的Web应用程序之前警告用户。使用代理的身份验证和授权与任何其他特权Web应用程序一样处理。

| Parameter | Value | Notes |
| - | - | - |
| `yarn.web-proxy.address` | WebAppProxy host:port for proxy to AM web apps. | host:port if this is the same as yarn.resourcemanager.webapp.address or it is not defined then the ResourceManager will run the proxy otherwise a standalone proxy server will need to be launched. |
| `yarn.web-proxy.keytab` | `/etc/security/keytab/web-app.service.keytab` | Kerberos keytab file for the WebAppProxy. |
| `yarn.web-proxy.principal` | `wap/_HOST@REALM.TLD` | Kerberos principal name for the WebAppProxy. |


<br/>
<br/>


#### LinuxContainerExecutor

YARN框架使用的ContainerExecutor，用于定义任何容器的启动和控制方式。

以下是Hadoop YARN中可用内容:


| ContainerExecutor	| Description |
| - | - |
| DefaultContainerExecutor | The default executor which YARN uses to manage container execution. The container process has the same Unix user as the NodeManager. |
| LinuxContainerExecutor | Supported only on GNU/Linux, this executor runs the containers as either the YARN user who submitted the application (when full security is enabled) or as a dedicated user (defaults to nobody) when full security is not enabled. When full security is enabled, this executor requires all user accounts to be created on the cluster nodes where the containers are launched. It uses a setuid executable that is included in the Hadoop distribution. The NodeManager uses this executable to launch and kill containers. The setuid executable switches to the user who has submitted the application and launches or kills the containers. For maximum security, this executor sets up restricted permissions and user/group ownership of local files and directories used by the containers such as the shared objects, jars, intermediate files, log files etc. Particularly note that, because of this, except the application owner and NodeManager, no other user can access any of the local files/directories including those localized as part of the distributed cache. |

<br>

要构建LinuxContainerExecutor可执行:

```bash
# 集群配置文件路径
mvn package -Dcontainer-executor.conf.dir=/opt/hadoop/etc/hadoop/
```

LinuxTaskController 要求包含和导向 `yarn.nodemanager.local-dirs` 和 `yarn.nodemanager.log-dirs` 中指定的目录的路径设置为755权限。

<br>

可执行文件需要一个名为`container-executor.cfg(conf/container-executor.cfg)`的配置文件，该文件存在于上述mvn的目标配置目录里。该配置文件必须有NodeNanager的用户所有，权限应为`0400`。
可执行文件要求此配置文件存在以下配置项(KV):

| Parameter | Value | Notes |
| - | - | - |
| yarn.nodemanager.linux-container-executor.group | hadoop | Unix group of the NodeManager. The group owner of the container-executor binary should be this group. Should be same as the value with which the NodeManager is configured. This configuration is required for validating the secure access of the container-executor binary. |
| banned.users | hdfs,yarn,mapred,bin | Banned users. |
| allowed.system.users | foo,bar | Allowed system users. |
| min.user.id | 1000 | Prevent other super-users. |

<br>

以下是与LinuxContainerExecutor相关的各种路径所需的本地文件系统权限：

| Filesystem | Path | User:Group | Permissions |
| - | - | - |
| `local` | `container-executor` | root:hadoop | --Sr-s--* |
| `local` | `conf/container-executor.cfg` | root:hadoop | r-------* |
| `local` | `yarn.nodemanager.local-dirs` | yarn:hadoop | drwxr-xr-x |
| `local` | `yarn.nodemanager.log-dirs` | yarn:hadoop | drwxr-xr-x |


<br/>
<br/>


#### MapReduce JobHistory Server

| Parameter | Value | Notes |
| - | - | - |
| `mapreduce.jobhistory.address` | MapReduce JobHistory Server `host:port` | Default port is 10020. |
| `mapreduce.jobhistory.keytab` | `/etc/security/keytab/jhs.service.keytab` | Kerberos keytab file for the MapReduce JobHistory Server. |
| `mapreduce.jobhistory.principal` | `jhs/_HOST@REALM.TLD` | Kerberos principal name for the MapReduce JobHistory Server. |



<br/>
<br/>
<br/>



### Multihoming


多宿主设置，其中每个主机在DNS中具有多个主机名(如，对应于公共和专用网络接口的不同主机名)。可能需要额外的配置才能使Kerberos身份认证工作。



<br/>
<br/>


### Troubleshooting


Kerberos is hard to set up，and harder to debug。常见问题有:

- Network and DNS configuration
- Kerberos configuration on hosts (`/etc/krb5.conf`)
- Keytab creation and maintenance
- Environment setup: JVM, user login, system clocks, etc

```
# Set the environment variable HADOOP_JAAS_DEBUG to true
export HADOOP_JAAS_DEBUG=true

# Edit the log4j.properties file to log Hadoop’s security package at DEBUG level
log4j.logger.org.apache.hadoop.security=DEBUG

# Enable JVM-level debugging by setting some system properties
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true -Dsun.security.krb5.debug=true -Dsun.security.spnego.debug"
```



<br/>
<br/>




### KDiag

Troubleshooting with KDiag


Hadoop有一个工具来帮助验证设置：**KDiag**。

它包含一系列用于JVM配置和环境的探测器，转储出一些系统文件（`/etc/krb5.conf`, `/etc/ntp.conf`），打印出一些系统状态，然后尝试登录到Kerberos作为当前用户或命名密钥表中的特定主体。
该命令的输出可用于本地诊断，或转发给支持群集的任何人。
KDiag命令有自己的入口点，通过将kdiag传递给`hadoop`命令来调用它。因此，它将显示用于调用它的命令的kerberos客户端状态。

```bash
hadoop kdiag


# 帮助
bin/hadoop kdiag --help


# 栗子
hadoop kdiag \
  --nofail \
  --resource hdfs-site.xml --resource yarn-site.xml \
  --keylen 1024 \
  --keytab zk.service.keytab --principal zookeeper/devix.example.org@REALM
```


<br/>
<br/>


### 关闭安全模式


Hadoop Secure Mode默认是开启的！

```bash
# 查看
# bin/hadoop dfsadmin -safemode get(旧)
bin/hdfs dfsadmin -safemode get
Safe mode is ON


# 关闭
# bin/hdfs dfsadmin -safemode leave(旧)
bin/hdfs dfsadmin -safemode  leave
Safe mode is OFF


# 启用
# bin/hdfs dfsadmin -safemode enter(旧)
bin/hdfs dfsadmin -safemode enter
Safe mode is ON
```



<br/>
<br/>
<br/>




## 服务级别授权

[Service Level Authorization Guide](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/ServiceLevelAuth.html)


本节描述了如何配置和管理Hadoop服务级别的授权(Service Level Authorization)。

<br>

**Prerequisites:**

确已正确安装、配置和设置Hadoop！


<br/>


### 概述

服务级别授权是初始化授权机制，用于确保连接到特定的Hadoop服务的客户端具有必要的预配置设置，并且有权访问给定服务。例如，MapReduce集群可以使用此机制来允许已配置的用户/组列表提交作业。

`$HADOOP_CONF_DIR/hadoop-policy.xml`配置文件用于定义各种Hadoop服务的访问控制列表(ACL)。
服务级别授权在其他访问控制检查之前执行很久，例如文件权限检查，作业队列上的访问控制等。



<br/>
<br/>



### 配置

通过配置文件`$HADOOP_CONF_DIR/hadoop-policy.xml`配置服务级别的授权。


<br/>


#### 启用服务级别授权

Enable Service Level Authorization

默认情况下，Hadoop禁用服务级别授权。要启用它，请在`$HADOOP_CONF_DIR/core-site.xml`中将配置属性`hadoop.security.authorization`设置为`true`。


<br/>
<br/>


#### Hadoop服务和配置项

Hadoop Services and Configuration Properties

下面列出各种Hadoop服务及其配置项:

| Property | Service |
| - | - |
| `security.client.protocol.acl` | ACL for ClientProtocol, which is used by user code via the DistributedFileSystem. |
| `security.client.datanode.protocol.acl` | ACL for ClientDatanodeProtocol, the client-to-datanode protocol for block recovery. |
| `security.datanode.protocol.acl` | ACL for DatanodeProtocol, which is used by datanodes to communicate with the namenode. |
| `security.inter.datanode.protocol.acl` | ACL for InterDatanodeProtocol, the inter-datanode protocol for updating generation timestamp. |
| `security.namenode.protocol.acl` | ACL for NamenodeProtocol, the protocol used by the secondary namenode to communicate with the namenode. |
| `security.job.client.protocol.acl` | ACL for JobSubmissionProtocol, used by job clients to communciate with the resourcemanager for job submission, querying job status etc. |
| `security.job.task.protocol.acl` | ACL for TaskUmbilicalProtocol, used by the map and reduce tasks to communicate with the parent nodemanager. |
| `security.refresh.policy.protocol.acl` | ACL for RefreshAuthorizationPolicyProtocol, used by the dfsadmin and rmadmin commands to refresh the security policy in-effect. |
| `security.ha.service.protocol.acl` | ACL for HAService protocol used by HAAdmin to manage the active and stand-by states of namenode. |


<br/>
<br/>


#### 访问控制列表

Access Control Lists

`$HADOOP_CONF_DIR/hadoop-policy.xml`为每个Hadoop服务定义一个访问控制列表。

每个访问控制列表都有一个简单的格式: users/groups都是用逗号分隔的名称列表。如: `user1, user2, group1, group2`

如果仅提供组列表，则在行的开头添加空格，等效地以逗号分隔的用户列表后跟空格或不显示仅包含一组给定用户。特殊值`*`表示允许所有用户访问该服务。如果未为服务定义访问控制列表，则应用`security.service.authorization.default.acl` 的值。如果未定义 `security.service.authorization.default.acl`，则应用`*`。


<br/>
<br/>


#### 被阻止的访问控制列表

Blocked Access Control Lists

在某些情况下，需要为服务指定阻止的访问控制列表。这指定了未授权访问该服务的用户和组的列表。被阻止的访问控制列表的格式与访问控制列表的格式相同。
可通过`$HADOOP_CONF_DIR/hadoop-policy.xml`指定阻止的访问控制列表。属性名称通过后缀`.blocked`派生。栗子: `security.client.protocol.acl` 的阻止访问控制列表的属性名称为`security.client.protocol.acl.blocked` 。

对于服务，可以指定访问控制列表和阻止的控制列表。如果用户在访问控制中而不在阻止的访问控制列表中，则授权用户访问该服务。

如果未为服务定义阻止访问控制列表，则应用 `security.service.authorization.default.acl.blocked` 的值。如果未定义 `security.service.authorization.default.acl.blocked`，则应用空的阻止访问控制列表。


<br/>
<br/>


#### IP地址，主机名，IP范围进行访问控制

Access Control using Lists of IP Addresses, Host Names and IP Ranges

可以基于访问服务的客户端IP地址来控制对服务的访问。通过指定IP地址，主机名和IP范围列表，可以限制从一组计算机访问服务。每个服务的属性名称都是从相应的acl属性名称派生的。如果acl的属性名称为`security.client.protocol.acl`，则hosts列表的属性名称为 `security.client.protocol.hosts`。
如果未为服务定义主机列表，则应用 `security.service.authorization.default.hosts` 的值。如果未定义 `security.service.authorization.default.hosts`，则应用 `*` 。

可以指定阻止的主机列表。只有那些位于主机列表中但未在阻止主机列表中的计算机才会被授予对该服务的访问权限。属性名称通过后缀 `.blocked` 派生。栗子: `security.client.protocol.hosts` 的被阻止主机列表的属性名称为 `security.client.protocol.hosts.blocked`。
如果未为服务定义阻止主机列表，则应用 `security.service.authorization.default.hosts.blocked` 的值。如果未定义 `security.service.authorization.default.hosts.blocked`，则应用空的阻止主机列表。


<br/>
<br/>


#### 刷新服务级别授权配置

Refreshing Service Level Authorization Configuration

可在不重启Hadoop Daemon的情况下更改NameNode和ResourceManager的服务级别授权配置。集群管理员可在Master节点上更改`$HADOOP_CONF_DIR/hadoop-policy.xml`，并指示NameNode和ResourceManager分别通过` -refreshServiceAcl`开关将其各自的配置重新加载到`dfsadmin`和`rmadmin`命令。


```bash
# 刷新NameNode的服务级别的授权配置
bin/hdfs dfsadmin -refreshServiceAcl


# 刷新ResourceManager的服务级别授权配置
bin/yarn rmadmin -refreshServiceAcl
```

当然，也可以使用`$HADOOP_CONF_DIR/hadoop-policy.xml`中的`security.refresh.policy.protocol.acl`属性来限制对某些users/groups刷新服务级别授权的访问权限。


<br/>
<br/>


#### 栗子

仅允许mapreduce gruop中的a, b users将作业提交到MapReduce集群:

```
<property>
     <name>security.job.client.protocol.acl</name>
     <value>a,b mapreduce</value>
</property>
```

<br>

仅允许数据属于group datanodes的users运行的DataNode与NameNode进行通信:

```
<property>
     <name>security.datanode.protocol.acl</name>
     <value>datanodes</value>
</property>
```

<br>

允许任何用户作为DFSClient与HDFS集群通信:

```
<property>
     <name>security.client.protocol.acl</name>
     <value>*</value>
</property>
```




<br/>
<br/>
<br/>




## HTTP认证

[Authentication for Hadoop HTTP web-consoles](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/HttpAuthentication.html)


<br/>


### 介绍

本节介绍如何配置Hadoop HTTP web-console以要求用户身份认证。

默认情况下，Hadoop HTTP web-console(ResourceManager, NameNode, NodeManager, DataNodes)允许无需任何形式的身份认证的访问。

可将Hadoop HTTP web-console配置为使用HTTP SPNEGO协议要求Kerberos身份认证。
此外，Hadoop HTTP web-console支持相当于Hadoop的 Pseudo/Simple 认证。如果启用此选项，则必须使用 `user.name` 查询字符串参数在浏览器交互中地址用户名。 如: `http://localhost:8088/cluster?user.name=usera`。
如果HTTP web-console需要自定义身份认证机制，则可以实现插件以支持备用身份认证机制。



<br/>
<br/>



### 配置


以下属性应位于集群中所有节点的 `core-site.xml` 中:

| Property Name | Default Value | Description |
| - | - | - |
| `hadoop.http.filter.initializers` | - | Add to this property the org.apache.hadoop.security.AuthenticationFilterInitializer initializer class. |
| `hadoop.http.authentication.type` | simple | Defines authentication used for the HTTP web-consoles. The supported values are: simple | kerberos | #AUTHENTICATION_HANDLER_CLASSNAME#. |
| `hadoop.http.authentication.token.validity` | 36000 | Indicates how long (in seconds) an authentication token is valid before it has to be renewed. |
| `hadoop.http.authentication.token.max-inactive-interval` | -1 (disabled) | Specifies the time, in seconds, between client requests the server will invalidate the token. |
| `hadoop.http.authentication.signature.secret.file` | `$user.home/hadoop-http-auth-signature-secret` | The signature secret file for signing the authentication tokens. The same secret should be used for all nodes in the cluster, ResourceManager, NameNode, DataNode and NodeManager. This file should be readable only by the Unix user running the daemons. |
| `hadoop.http.authentication.cookie.domain` | - | The domain to use for the HTTP cookie that stores the authentication token. For authentication to work correctly across all nodes in the cluster the domain must be correctly set. There is no default value, the HTTP cookie will not have a domain working only with the hostname issuing the HTTP cookie. |
| `hadoop.http.authentication.cookie.persistent` | false (session cookie) | Specifies the persistence of the HTTP cookie. If the value is true, the cookie is a persistent one. Otherwise, it is a session cookie. IMPORTANT: when using IP addresses, browsers ignore cookies with domain settings. For this setting to work properly all nodes in the cluster must be configured to generate URLs with hostname.domain names on it. |
| `hadoop.http.authentication.simple.anonymous.allowed` | true | Indicates whether anonymous requests are allowed when using ‘simple’ authentication. |
| `hadoop.http.authentication.kerberos.principal` | `HTTP/_HOST@$LOCALHOST` | Indicates the Kerberos principal to be used for HTTP endpoint when using ‘kerberos’ authentication. The principal short name must be HTTP per Kerberos HTTP SPNEGO specification. _HOST -if present- is replaced with bind address of the HTTP server. |
| `hadoop.http.authentication.kerberos.keytab` | `$user.home/hadoop.keytab` | Location of the keytab file with the credentials for the Kerberos principal used for the HTTP endpoint. |



<br/>
<br/>



### CORS


要启用跨域支持(CORS)，请设置以下配置参数:

将 `org.apache.hadoop.security.HttpCrossOriginFilterInitializer` 添加到 `core-site.xml` 中的 `hadoop.http.filter.initializers`。您还需要在 `core-site.xml` 中设置以下属性:

| Property | Default Value | Description |
| - | - | - |
| `hadoop.http.cross-origin.enabled` | `false` | Enables cross origin support for all web-services |
| `hadoop.http.cross-origin.allowed-origins` | `*` | Comma separated list of origins that are allowed. Values prefixed with regex: are interpreted as regular expressions. Values containing wildcards (*) are possible as well, here a regular expression is generated, the use is discouraged and support is only available for backward compatibility. |
| `hadoop.http.cross-origin.allowed-methods` | GET,POST,HEAD | Comma separated list of methods that are allowed |
| `hadoop.http.cross-origin.allowed-headers` | X-Requested-With,Content-Type,Accept,Origin | Comma separated list of headers that are allowed |
| `hadoop.http.cross-origin.max-age` | 1800 | Number of seconds a pre-flighted request can be cached |




<br/>
<br/>
<br/>



## Credential Provider API

[CredentialProvider API Guide](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/CredentialProviderAPI.html)


CredentialProvider API是一个用于插入可扩展凭据提供程序的SPI框架。凭据提供程序用于将敏感令牌(token)，机密(secret)和密码(passwd)的使用与其存储和管理的详细信息分开。选择各种存储机制来保护这些凭证的能力使我们能够使这些敏感资产远离明文(clear text)，远离窥探并可能由第三方解决方案管理。

本节描述CredentialProvider API的设计，开箱即用的实现，使用它们以及如何使用它们。















<br/>
<br/>
<br/>




## 密钥管理

[Hadoop Key Management Server (KMS)](https://hadoop.apache.org/docs/r3.2.0/hadoop-kms/index.html)


**Hadoop KMS** 是一个基于Hadoop KeyProvider API的加密秘钥管理服务器。
它提供了一个Client和Server组件，它们使用REST API通过HTTP进行通信。
Client是KeyProvider实现，使用KMS HTTP REST API与KMS交互。
KMS及其Client內建有安全性，并且支持HTTP和 SPNEGO Kerberos认证和HTTPS安全传输。
KMS是一个Java Jetty Web应用程序。












<br/>
<br/>
<br/>




## Tracing

[Enabling Dapper-like Tracing in Hadoop](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/Tracing.html)


<br/>


### HTrace

HDFS-5274 使用开源跟踪库(Apache HTrace)增加了对通过HDFS跟踪请求的支持。设置跟踪非常简单，但是它需要对客户端代码进行一些非常小的更改。



<br/>
<br/>



### SpanReceivers













<br/>
<br/>

---

<br/>
<br/>












# HDFS


<br/>


## 架构

[HDFS Architecture](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)


Hadoop Distributed File System(HDFS) 是一种分布式文件系统，设计用于在商业硬件上运行。它与现有的分布式文件系统有许多相似之处。但是，与其它分布式文件系统的差异很大。HDFS具有高度容错(fault-tolerant)能力，旨在部署在低成本硬件上。HDFS提供对应用程序数据的高吞吐量访问，适用于具有大型数据集的应用程序。HDFS放宽了一些POSIX要求，以实现对文件系统数据的流式访问。


<br/>
<br/>


### 假设和目标

Assumptions and Goals


<br/>


#### 硬件故障

Hardware Failure

硬件故障是常态而非异常。HDFS实例可能包含成百上千的主机，每台主机都存储文件系统数据的一部分。事实上，存在大量组件并且每个组件具有非平凡(non-trivial)的故障概率，这意味着HDFS某些组件始终不起作用(non-functional)。因此，检测故障并从中快速自动地恢复是HDFS的核心架构目标。


<br/>
<br/>


#### 流数据访问

Streaming Data Access

在HDFS上运行的应用程序需要对其数据集进行流式访问。它们不是通常在通用文件系统上运行的通用应用程序。HDFS设计用于批处理而不是用户的交互式使用。重点是数据访问的高吞吐量(high throughput)而不是数据访问的低延迟(low latency)。POSIX强加了许多针对HDFS的应用程序不需要的硬性要求。


<br/>
<br/>


#### 大型数据集

Large Data Sets

在HDFS上运行的应用程序具有大型数据集。HDFS中的典型文件大小为gigabytes到terabytes。因此，HDFS被调整为支持大文件。它应该为单个集群中的成百上千的节点提供高聚合数据带宽和扩展。它应该在单个实例中支持数千万个文件。


<br/>
<br/>


#### 简单的一致性模型

Simple Coherency Model

HDFS应用程序需要一个`write-once-read-many`的文件访问模型。除了追加(append)和截断(truncates)之外，无需更改创建，写入和关闭的文件。支持将内容附加到文件末尾，但无法在任意点更新。此假设简化了数据一致性问题，并实现了高吞吐量数据访问。MapReduce应用程序或Web Crawler应用程序适合此模型。


<br/>
<br/>


#### 移动计算比移动数据更便宜

Moving Computation is Cheaper than Moving Data

如果应用程序在其操作的数据附近执行，则计算所请求的计算效率更高。当数据集很大时尤其如此。这可以最大限度地减少网络拥塞(network congestion)并提高系统的整体吞吐量。这个假设通常更好的是将计算迁移到更靠近数据所在的地方，而不是将数据移动到应用程序运行的地方。HDFS为应用程序移动到更靠近数据所在的地方的接口。


<br/>
<br/>


#### 可移植性

Portability Across Heterogeneous Hardware and Software Platforms

HDFS的设计便于从一个平台移植到另一个平台。



<br/>
<br/>
<br/>



### NameNode和DataNode


HDFS具有主从架构(Master-Slave)。HDFS集群由单个**NameNode**、一个管理文件系统命名空间和管理客户端对文件的访问的**Master Server**组成。此外，还有许多**DataNode**，通常是集群中每个节点一个，用于管理附加到它们运行节点的存储。HDFS公开文件系统命名空间，并允许用户数据存储在文件中。在内部，文件被分成一个或多个块(block)，这些块存储在一组DataNode中。
NameNode执行文件系统命名空间操作(如打开、关闭、重命名文件目录)。它还确定了block到DataNode的映射。
DataNode负责提供来自文件系统客户端的读写请求，它还根据NameNode的指令执行块操作(如创建、删除、副本)。

![](/images/Hadoop/hdfsarchitecture.png)

<br>

NameNode和DataNode是设计用于在商用机器上运行的软件，这些机器通常运行GNU/Linux操作系统。HDFS使用Java语言构建，任何支持Java的机器都可运行NameNode或DataNode软件。使用高度可移植的Java语言意味着可以在各种计算机上部署HDFS。
典型部署具有仅运行NameNode软件的专用主机，群集中的每台其它主机都运行一个DataNode软件实例。虽然可以讲它们运行在同一台主机上，但这并不推荐。

群集中存在单个NameNode极大地简化了系统结构。NameNode是所有HDFS Metadata的仲裁者(arbitrator)和存储库(repository)。系统的设计使用户数据永远不会流经NameNode。



<br/>
<br/>
<br/>



### 文件系统命名空间

The File System Namespace


HDFS支持传统的层次文件组织。用户或应用程序可以创建目录，并在这些目录中存储文件。文件系统命名空间层次结构类似于大多数其它现有文件系统；可创建、删除、移动、重命名文件。HDFS支持用户配额(user quotas)和访问权限。HDFS不支持硬链接和软链接。但是，HDFS架构并不排除实现这些功能。

NameNode维护文件系统命名空间。NameNode Record对文件系统命名空间或其属性的任何更改。应用程序可以指定应由HDFS维护的文件的副本数。文件的副本数称为该文件的复制因子，该信息由NameNode存储。



<br/>
<br/>
<br/>



### 数据副本

Data Replication

HDFS旨在可靠地在大型群集中的计算机上存储非常大的文件。它将每个文件存储为一系列块。文件块的副本用以实现容错(fault tolerance)。块大小和副本因子可根据文件进行配置。

除了最后一个块之外，文件中的所有块都具有相同的大小。而用户可以在添加对可变长度块的支持以追加和hsync之后启动新块而不将最后一个块填充到配置的块大小。

应用程序可以指定文件的副本数量。副本因子可在文件创建时指定，并且可以在之后修改。HDFS中的文件是一次写入的，并且在任何时候都有一个写入器。

NameNode做出有关副本的所有决定。它定期从集群中的每个DataNode接收Heartbeat和Blockreport。收到心跳意味着DataNode正常运行，块上报包含DataNode上所有块的列表。

![](/images/Hadoop/hdfsdatanodes.png)


<br/>


#### 副本安置

Replica Placement: The First Baby Steps






























