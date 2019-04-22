---
title: Flume
date: 2019-03-07 09:32:20
categories: DevOps
tags:
  - Apache
  - DataAnalysis
  - Flume
---


参考:

- wikipedia
- Flume: <https://flume.apache.org/>
- Flume docs: <https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html>

环境:

- ELRH7x86_64
- Flume v1.9.0








<br/>
<br/>


---

<!--more-->

<br/>
<br/>









# 介绍

[](https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html#introduction)


**Apache Flume** 是一种分布式，可靠且可用的软件，使用Java编写，用于高效收集(collecting)，聚合(aggregating)和移动(moving)大量日志数据。它具有基于流数据流(stream data flows)的简单灵活的架构。它具有可靠性机制和许多故障转移和恢复机制，具有强大的容错能力。它使用简单的可扩展数据模型，允许在线分析应用程序。

Apache Flume的使用不仅限于日志数据聚合。由于数据源是可定制的，因此Flume可用于传输大量事件数据，包括但不限于网络流量数据，社交媒体生成的数据，电子邮件消息以及几乎任何可能的数据源。

![](/imags/Flume/flume-logo.png)


<br/>


## 系统要求


- **Java**: Java 1.8+
- **Mem**: sources, channels, sinks有足够的内存
- **Disk**: channels, sinks有足够的磁盘空间
- **Directory Permissions**: Agent使用的目录的读写权限




<br/>
<br/>
<br/>




## 架构


<br/>


### 数据流模型

Data flow model

- **源(Source)**
- **通道(Channel)**
- **接收器(Sink)**
- **事件(Event)**

<br>

**Flume event** 被定义为具有字节(byte)有效负载可可选字符串属性集的数据流单元(unit of data flow)。**Flume agent**是一个(JVM)进程，它承载事件从外部源流向下一个目标的组件。

![](/images/Flume/flume-architecture.png)

<br>

**Flume源消费事件**(source consumes events)通过**外部源**(external source)(如WebServer)传递给它。外部源通过**目标Flume源**(target Flume source)识别的格式向Flume发送事件。当Flume源接收事件时，它会将其存储到一个或多个**通道**(channels)中。通道是一个被动存储，可以保持事件直到它被**Flume sink**所消费。接收器从通道中移除事件，并将其放入外部存储库(如HDFS)或将其转发到流中的下一个Flume Agent(next hop)的Flume Source。给定Agent中的Source和Sink与Channel中暂存的Events异步运行。



<br/>
<br/>



### 复杂流

Complex flows

Flume允许用户构建多跳(hop)流，其中事件在到达最终目的地之前经过多个代理。它还允许 fan-in 和 fan-out flows, 上下文路由(contextual routing), 故障跳跃的备份路由(故障转移)。



<br/>
<br/>



### 可靠性

Reliability


事件在每个Agent的通道中进行，然后将事件传递到流中的下一个代理或终端存储库(如HDFS)。只有将事件存储在下一个代理的通道或终端存储库之后，才会从通道中删除这些事件。这就是Flume中的单跳消息传递语义如何提供流的端到端可靠性。

Flume使用事务方法来保证事件的可靠传递。源和接收器分别在事务中封装由信道提供的事务中放置/提供的事件的存储(storage)/检索(retrieval)。这可确保事件集在流中从一个点到另一个点可靠地传递。在多跳流的情况下，来自前一跳的接收器和来自下一跳的源都运行其事务以确保数据安全地存储在下一跳的信道中。



<br/>
<br/>



### 可恢复性

Recoverability


事件在通道中进行，该通道管理从故障中恢复。Flume支持由本地文件系统支持的持久化(durable)文件通道。还有一个内存通道(memory channel)，它将事件存储到内存中的队列中，这更快，但是当代理进程死亡时仍然存留在内存通道中的任何事件都无法恢复。








<br/>
<br/>

---

<br/>
<br/>






# 设置

[Setup](https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html#setup)


<br/>


## 设置代理

Setting up an agent


Flume Agent配置文件存储在本地配置文件中。这是一个Java properties文件格式的文本文件。可以在同一个配置文件中指定一个或多个代理的配置。配置文件包括代理中的每个Source, Sink, Channel的属性，以及它们如何连接在一起以形成数据流。


<br/>


### 配置单个组件

Configuring individual components


流中的每个组件(source, sink, channel)都具有特定于类型和实例化的名称(name)，类型(type)，属性集(properties)。



<br/>
<br/>



### 连接各个部分

Wiring the pieces together


Agent需要知道加载哪些组件，以及它们如何连接以构成流。这是通过列出代理中每个源，接收器和通道的名称，然后为每个接收器和源指定连接通道来完成的。



<br/>
<br/>



### 启动代理

Starting an agent


下载Flume发型版，使用名为`flume-ng`的shell脚本启动代理程序。

```bash
# 你需要在命令行上指定代理名称、配置目录、配置文件
bin/flume-ng agent -n $agent_name -c conf -f conf/flume-conf.properties.template

# 现在，代理开始运行在给定属性文件中配置的源和接收器
```



<br/>
<br/>



### 一个栗子


下面给出一个示例配置文件。此配置允许用户生成事件，并将其记录到console:

```
# example.conf: A single-node Flume configuration

# Name the components on this agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
a1.sources.r1.type = netcat
a1.sources.r1.bind = localhost
a1.sources.r1.port = 44444

# Describe the sink
a1.sinks.k1.type = logger

# Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

<br>

```bash
# 运行它
bin/flume-ng agent --conf conf --conf-file example.conf --name a1 -Dflume.root.logger=INFO,console
```



<br/>
<br/>



### 在配置文件中使用环境变量

Using environment variables in configuration files


Flume能够替换配置中的环境变量:

```
a1.sources = r1
a1.sources.r1.type = netcat
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = ${NC_PORT}
a1.sources.r1.channels = c1
```

**注意: 环境变量目前只使用于Value，不适用于Key。**



<br/>
<br/>



### 记录原始数据

Logging raw data


在许多生产环境中记录流经管道的原始数据流不是所希望的行为，因为这可能导致泄露敏感数据或安全相关配置到Flume日志文件。默认情况下，Flume不会记录此类信息。另一方法，如果数据管道出错，Flume也将尝试提供DEBUG信息。

为了能够记录事件和配置相关的数据，除了 `log4j` 属性外，还必须设置一些Java系统属性。
要启用与配置相关的日志记录，请设置Java系统属性 `-Dorg.apache.flume.log.printconfig=true` 。这也可以在命令行上进行传递，也可以在 ` flume-env.sh` 中的 `JAVA_OPTS` 变量中设置。

要启用数据记录，请按照上述相同方式设置Java系统属性 `-Dorg.apache.flume.log.rawdata=true` 。对于大多数组件，还必须将 `log4j` 日志记录级别设置为DEBUG或TRACE，以使特定于事件的日志记录显示在Flume日志中。

```
# 启用配置日志记录和原始数据日志记录的示例，同时还将Log4j日志级别设置为DEBUG以用于控制台输出
bin/flume-ng agent --conf conf --conf-file example.conf --name a1 -Dflume.root.logger=DEBUG,console -Dorg.apache.flume.log.printconfig=true -Dorg.apache.flume.log.rawdata=true
```



<br/>
<br/>



### Zookeeeper基础配置

Zookeeper based Configuration


Flume支持通过**Zookeeper**配置Agent的配置。这是一个实验性功能。配置文件需要在可配置前缀下的Zookeeper中上传。配置文件存储在Zookeeper Node Data中。

```
# Agent1和Agent2的Zookeeper Node Tree的示例
- /flume
 |- /a1 [Agent config file]
 |- /a2 [Agent config file]
```

一旦上传了配置文件，使用以下选项启动Agent:

```bash
# -z, Zookeeper connection string. Comma separated list of hostname:port
# -p, Base Path in Zookeeper to store Agent configurations

 bin/flume-ng agent –conf conf -z zkhost:2181,zkhost1:2181 -p /flume –name a1 -Dflume.root.logger=INFO,console
```



<br/>
<br/>



### 安装第三方插件

Installing third-party plugins


Flume拥有完整的基于插件的架构。虽然Flume附带了许多开箱即用的sources, channels, sinks, serializers...但许多实现斗鱼Flume分开运行。

虽然通过将自己的jar包添加到 `flume-env.sh` 文件中的 `FLUME_CLASSPATH` 变量值，始终可以包含自定义Flume组件。但Flume现在支持一个名为 `plugins.d` 的特殊目录，该目录会自动获取以特定格式打包的插件。

<br>

**插件目录**
The plugins.d directory

`plugins.d` 目录位于 `$FLUME_HOME/plugins.d`。在启动时，`flume-ng` 启动脚本在 `plugins.d` 目录中查找符合以下格式的插件，并在启动java时将它们包含在正确的路径中。

<br>

**插件目录布局**
Directory layout for plugins

`plugins.d` 中的每个插件(子目录)最多可以有三个子目录:

1. `lib` - the plugin’s jar(s)
2. `libext` - the plugin’s dependency jar(s)
3. `native` - any required native libraries, such as `.so` files

栗子:

```
plugins.d/
plugins.d/custom-source-1/
plugins.d/custom-source-1/lib/my-source.jar
plugins.d/custom-source-1/libext/spring-core-2.5.6.jar
plugins.d/custom-source-2/
plugins.d/custom-source-2/lib/custom.jar
plugins.d/custom-source-2/native/gettext.so
```



<br/>
<br/>
<br/>




## 数据摄取

Data ingestion


Flume支持许多从外部源摄取数据的机制。


<br/>


### RPC

可使用RPC机制将给定文件发送到Flume Source:

```bash
# 栗子
# 将日志内容发送到监听该端口的Flume Source
bin/flume-ng avro-client -H localhost -p 41414 -F /usr/logs/log.10
```



<br/>
<br/>



### 执行命令

Executing commands


有一个exec source执行给定的命令并消费输出。



<br/>
<br/>



### 网路流

Network streams


Flume支持以下机制从常用日志流(log stream)类型中读取数据。如:

- Avro
- Thrift
- Syslog
- Netcat




<br/>
<br/>
<br/>




## 多个代理流

Setting multi-agent flow

![](/images/Flume/flume-multi-agent-flow.png)

为了跨多个代理/跳(multiple agents/hops)的数据流，先前代理的接收器和当前代理的源是同一类型，接收器指向源的hostname/ip和port。




<br/>
<br/>
<br/>




## 整合

Consolidation


日志收集中非常常见的情况是生成大量日志的客户端将数据发送到连接到存储子系统的少数消费者代理。例如，从数百个Web服务器收集的日志发送给写入HDFS集群的十几个代理。

这可以通过在Flume中使用接收器配置多个第一层代理，所有这些代理都指向单个源。第二层代理商的源将接收的事件合并到单个通道中，该通道有接收器消费到最终的目的地。

![](/images/Flume/flume-consolidation.png)




<br/>
<br/>
<br/>




## 多路复用流

Multiplexing the flow


Flume支持将事件流多路复用到一个或多个目的地。这是通过定义可以复制或选择性地将事件路由到一个或多个通道的流复用器来实现的。

![](/images/Flume/flume-multiplexing-flow.png)














<br/>
<br/>

---

<br/>
<br/>

















# 配置

[Configuration](https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html#configuration)



















































































<br/>
<br/>

---

<br/>
<br/>



















# 过滤

[Configuration Filters](https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html#configuration-filters)


































