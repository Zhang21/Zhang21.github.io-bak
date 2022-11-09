# Doris



参考:

- [Doris文档](https://doris.apache.org/docs/summary/basic-summary/)
- [Dorish官方仓库](https://github.com/apache/doris/)


<br/>

---

<br/>




# 介绍

Apache Doris能够较好的满足报表分析、即席查询、统一数仓构建、数据湖联邦查询加速等使用场景，用户可以在此之上构建用户行为分析、AB 实验平台、日志检索分析、用户画像分析、订单分析等应用。


<br/>


## 使用场景

如下图所示，数据源经过各种数据集成和加工处理后，通常会入库到实时数仓 Doris 和离线湖仓，Apache Doris 被广泛应用在以下场景中。

![](/images/Doris/doris01.png)

<br/>

- 报表分析
  - 实时看板
  - 面向企业内部的报表
  - 面向客户的报表分析
- 即席查询：面向分析师的自助分析
- 统一数仓构建：一个平台满足统一的数仓（如Spark、Hive、Kudu、Hbase等组成的旧架构）建设需求，简化繁琐的大数据软件栈
- 数据湖联邦查询


<br/>
<br/>


## 技术概述

架构：

- Frontend(FE)：主要负责用户请求的接入、查询解析规划、元数据的管理、节点管理等工作。
- Backend(BE)：主要负责数据存储、查询计划的执行。

两类进程都可横向扩展，通过一致性协议来保证服务的高可用和数据的高可靠。

![](/images/Doris/doris02.png)

<br/>

在使用接口方面，Doris 采用 MySQL 协议，高度兼容 MySQL 语法，支持标准 SQL，用户可以通过各类客户端工具来访问 Doris，并支持与BI工具的无缝对接。

在存储引擎方面，Doris 采用列式存储，按列进行数据的编码压缩和读取，能够实现极高的压缩比，同时减少大量非相关数据的扫描，从而更加有效利用 IO 和 CPU 资源。

在查询引擎方面，Doris 采用 MPP 的模型，节点间和节点内都并行执行，也支持多个大表的分布式 Shuffle Join，从而能够更好应对复杂查询。

![](/images/Doris/doris03.png)

Doris 查询引擎是向量化的查询引擎，所有的内存结构能够按照列式布局，能够达到大幅减少虚函数调用、提升 Cache 命中率，高效利用 SIMD 指令的效果。

![](/images/Doris/doris04.png)




<br/>

---

<br/>




# 快速开始

注意Doris需要的系统、软件、硬件、内核参数、限制等要求。


<br/>


## 下载

```
# 在此页面下载doris相关软件包
https://doris.apache.org/zh-CN/download/


# 解压
tar -zxf apache-doris-版本.tar.gz
mv apache-doris-版本 /opt
```


<br/>
<br/>


## 配置

配置和启动FE。

```
cd /opt/apache-doris-版本/fe

# 修改配置
conf/fe.conf
# 主要修改两个参数
# priority_networks=172.23.16.0/24
# meta_dir=/data/doris/doris-meta

# 启动
/opt/apache-doris-版本/bin/start_fe.sh --daemon
# 停止
/opt/apache-doris-版本/bin/stop_fe.sh

# 查看状态
# 或者直接访问界面也可以（默认用户root，密码为空）
curl http://127.0.0.1:8030/api/bootstrap

# 连接
mysql -uroot -P9030 -h127.0.0.1
# 查看运行状态
mysql> show frontends\G
```

<br/>

配置和启动BE。

```
cd /opt/apache-doris-版本/be

# 修改配置
conf/be.conf
# 主要修改两个参数
# priority_networks=172.23.16.0/24
# storage_root=/data/doris/doris_be

# 启动
/opt/apache-doris-版本/bin/start_be.sh --daemon
# 停止
/opt/apache-doris-版本/bin/stop_be.sh

# 首先连接到fe
mysql -hxxx -Pxxx -uroot
# 添加BE节点到集群
mysql> ALTER SYSTEM ADD BACKEND "be_host_ip:heartbeat_service_port";
# 查看状态
mysql> HOW BACKENDS\G;
```


<br/>
<br/>


## 创建数据库

```sql
# 创建库
create database demo;

# 创建表
use demo;

CREATE TABLE IF NOT EXISTS demo.example_tbl
(
    `user_id` LARGEINT NOT NULL COMMENT "用户id",
    `date` DATE NOT NULL COMMENT "数据灌入日期时间",
    `city` VARCHAR(20) COMMENT "用户所在城市",
    `age` SMALLINT COMMENT "用户年龄",
    `sex` TINYINT COMMENT "用户性别",
    `last_visit_date` DATETIME REPLACE DEFAULT "1970-01-01 00:00:00" COMMENT "用户最后一次访问时间",
    `cost` BIGINT SUM DEFAULT "0" COMMENT "用户总消费",
    `max_dwell_time` INT MAX DEFAULT "0" COMMENT "用户最大停留时间",
    `min_dwell_time` INT MIN DEFAULT "99999" COMMENT "用户最小停留时间"
)
AGGREGATE KEY(`user_id`, `date`, `city`, `age`, `sex`)
DISTRIBUTED BY HASH(`user_id`) BUCKETS 1
PROPERTIES (
    "replication_allocation" = "tag.location.default: 1"
);
```

将一个示例csv文件数据导入：

```bash
curl  --location-trusted -u root: -T test.csv -H "column_separator:," http://127.0.0.1:8030/api/demo/example_tbl/_stream_load
```


<br/>
<br/>


## 查询数据

```
mysql -hxxx -Pxxx -uroot
mysql> use demo;
mysql> select * from example_tbl;
mysql> select * from example_tbl where city="上海";
```




<br/>

---

<br/>




# 安装和部署

<br/>

## 要求

安装之前的一些要求：

- CentOS 7.1+
- Ubuntu 16.04+
- Java 1.8+
- GCC 4.8.2+
- 时钟同步
- limits修改
- 关闭swap
- 推荐使用ext4文件系统
- 推荐使用SSD
  - FE主要用于存储元数据，包括日志和image。
  - BE主要用于存放用户数据，总磁盘空间按用户总数据量乘以3(3副本)，然后再预留40%的空间用作其它数据的存放。
- 8c8g以上配置

```
# 设置系统最大打开文件句柄数
vi /etc/security/limits.conf 
* soft nofile 65536
* hard nofile 65536
```


<br/>
<br/>


## 节点数量

- FE角色分为Follower和Observer（Leader为Follower组中选举出，也称为Follower）。
- FE 节点数据至少为1（1 个 Follower）。当部署 1个Follower 和 1 个 Observer 时，可以实现读高可用。当部署 3 个 Follower 时，可以实现读写高可用（HA）。
- Follower 的数量必须为奇数，Observer 数量随意。
- 当集群可用性要求很高时（比如提供在线业务），可以部署3个 Follower 和 1-3 个 Observer。如果是离线业务，建议部署 1 个 Follower 和 1-3 个 Observer。
- 如果 FE 和 BE 混部，需注意资源竞争问题，并保证元数据目录和数据目录分属不同磁盘。
- 一般情况3个机器FE/BE复用应该就满足最基本的高可用了。


<br/>
<br/>


## 端口信息

doris个实例使用的端口详情。

| 实例 | 端口名 | 默认端口 | 通讯方向 | 说明 |
| - | - | - | - | - |
| BE | be_port | 9060 | FE->BE | thrift server 的端口，用于接收来自 FE 的请求 |
| BE | webserver_port | 8040 | BE<->BE | http server 的端口 |
| BE | heartbeat_service_port | 9050 | FE->BE | 心跳服务端口（thrift），用于接收来自 FE 的心跳 |
| BE | brpc_port | 8060 | FE<->BE, BE<->BE | brpc 端口，用于 BE 之间通讯 |
| FE | http_port | 8030 | FE<->FE, 用户<->FE | http server 端口 |
| FE | rpc_port | 9020 | BE->FE, FE<->FE | thrift server 端口，每个fe的配置需要保持一致 |
| FE | query_port | 9030 | 用户<->FE | FE 上的 mysql server 端口 |
| FE | edit_log_port | 9010 | FE<->FE | bdbje 之间通信用的端口 |
| Broker | broker_ipc_port | 8000 | FE->Broker, BE->Broker | thrift server，用于接收请求 |

<br/>

Broker 是用于访问外部数据源（如 hdfs）的进程。通常，在每台机器上部署一个 broker 实例即可。

当部署多个 FE 实例时，要保证 FE 的 http_port 配置相同。

因为有多网卡的存在，或因为安装过 docker 等环境导致的虚拟网卡的存在，同一个主机可能存在多个不同的 ip。当前 Doris 并不能自动识别可用 IP。所以当遇到部署主机上有多个 IP 时，必须通过 priority_networks 配置项来强制指定正确的 IP。

当配置完 priority_networks 并启动 FE 或 BE 后，只是保证了 FE 或 BE 自身的 IP 进行了正确的绑定。而在使用 ADD BACKEND 或 ADD FRONTEND 语句中，也需要指定和 priority_networks 配置匹配的 IP，否则集群无法建立。


<br/>
<br/>


## 集群部署

下载软件包到对应的机器上，修改对应配置，启动进程。

将以将数据保存到单独的SSD数据盘目录。

JAVA_OPTS 默认 java 最大堆内存为 4GB，建议生产环境调整至 8G 以上。请合理注意自己实际机器的内存来修改此值。

如果是SSD磁盘要在目录后面加上`.SSD`,HDD磁盘在目录后面加`.HDD`。如`storage_root_path=/home/disk1/doris.HDD;/home/disk2/doris.SSD;/home/disk2/doris`，不加默认表示存储介质是HDD。

不论HDD磁盘目录还是SSD磁盘目录，都无需添加后缀，storage_root_path参数里指定medium即可。如`storage_root_path=/home/disk1/doris,medium:hdd;/home/disk2/doris,medium:ssd`。

在生产环境中，所有实例都应使用守护进程启动，以保证进程退出后，会被自动拉起，如 Supervisor，请记得修改`supervisord.conf`的文件打开数配置。

```
# fe配置
conf/fe.conf
# be配置
conf/be.conf

# 一般只需修改监听网络和存储路径
# priority_networks, meta_dir
# priority_networks, storage_root_path
```


<br/>
<br/>


## 添加节点

首先确认 FE 和 BE 进程都已经单独正常启动，并确认已经通过 `ADD BACKEND` 或者 `ADD FOLLOWER/OBSERVER` 语句添加了所有节点。

BE 节点需要先在 FE 中添加，才可加入集群。

```
# 在fe中添加be节点
# 连接到fe
mysql-client -h fe_host -P query_port -uroot
# 添加be节点
mysql> ALTER SYSTEM ADD BACKEND "be_host:heartbeat-service_port";
# 添加fe follower/observer
mysql> ALTER SYSTEM ADD FOLLOWER "fe_host:edit_log_port";
mysql> ALTER SYSTEM ADD OBSERVER "fe_host:edit_log_port";

#查看
mysql> show frontends\G;
mysql> show backends\G;
```


<br/>
<br/>


## Broker

Broker以插件的形式，独立于Doris部署。如果需要从第三方存储系统导入数据，需要部署相应的 Broker，默认提供了读取 HDFS 、对象存储的 fs_broker。fs_broker 是无状态的，建议每一个 FE 和 BE 节点都部署一个 Broker。




















