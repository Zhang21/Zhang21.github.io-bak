---

title: Prometheus
date: 2018-09-11 11:01:12
categories: DevOps
tags:
  - Prometheus
  - Monitoring

---


参考：

- Prometheus文档： <https://prometheus.io/docs>
- GitHub: <https://github.com/prometheus/>

环境：

- CentOS7x86_64
- Prometheus v2.3

<br>

<!--more-->

![Prometheus](/images/Prometheus/prometheus.jpg)


<br/>
<br/>

---

<br/>
<br/>




# 介绍

Introduction


<br/>


## 概述


<br/>


### Prometheus是什么

What is Prometheus?


Prometheus是一个最初在SoundCloud上构建的**开源监控系统和报警工具包**。现在是一个独立的开源项目，由社区进行维护。

<br>

**功能(Features)**
Prometheus的主要特点：

- 具有由度量名称(metric name)和键值对(key-value)标识的时间序列(time series)数据的多维(multi-dimensional)数据模型
- 灵活的查询语言，以利用此维度
- 不依赖分布式存储(distributed storage)，单个服务器节点是自治的(autonomous)
- 时间序列集合通过HTPP的`pull model`发生
- `push`时间序列通过中间网关(intermediary gateway)的支持
- 通过服务发现或静态配置来发现目标
- 图形和仪表盘支持多种模式

<br>

**组件(Components)**
Prometheus系统由多个组件构成，其中某些组件是可选的：

- 主要的**Prometheus Server**，用于存储时间序列数据
- **client libraries**，用于检测应用程序代码
- **push gateway**，用于支持短暂的(short-lived)工作
- **exporters**，用于服务的特殊目的
- **alertmanager**，用于处理报警
- 各种支持工具

<br>

**架构(Architecture)**
Prometheus的体系结构和系统组件图：

![Prometheus架构图](/images/Prometheus/architecture.png)


<br/>
<br/>


### 什么时候适合

When does it fit?


Prometheus适用于记录任何纯数字时间序列。它既适用于以机器为中心的监控，也适用于高度动态的面向服务架构的监控。在微服务的世界中，它对多维数据收集和查询的支持是一种特殊的优势。
Prometheus专为提高可靠性而设计，是你在断电期间可以快速诊断问题的系统。每个Prometheus Server都是独立的，不依赖于网络存储或其它远程服务。当基础架构其它部分损坏时，你仍可以依赖它，并且你不需要设置大量的基础架构来使用它。


<br/>
<br/>


### 什么时候不适合

When does it not fit?


Prometheus重视可靠性。即使在系统故障情况下，你也可以随时查看有关系统的可用统计信息。如果你需要100%的准确度，Prometheus不是一个好的选择，你可能需要使用其它系统。



<br/>
<br/>
<br/>



## 第一步

步骤：

- 下载
- 配置
- 运行
- 使用表达式浏览器
- 使用图形接口
- 监控其它目标



<br/>
<br/>
<br/>



## 术语

GLOSSARY


- **Alert**
是Prometheus正在开火的警报规则的结果。警报从Prometheus发送到AlterManger。

- **Alertmanager**
接收警报，将它们聚合成组，删除重复数据，应用静音、限制，然后发送电子邮件等通知。

- **Bridge**
是一个从Client Library中获取样本并将它们暴露给 non-Prometheus 监控系统的组件。例如，Python、Java、Go...客户端可将指标导出到Graphite。

- **Client library**
是某种语言的库(Go, Java, Python...)，可以直接检测代码，编写自定义收集器以从其它系统中收集指标并将指标公开给Prometheus。

- **Collector**
是表示一组度量标准的 exporter 的一部分。如果它是直接检测的一部分，则可以是单个度量，如果是从另一个系统提取度量，则可以是许多度量。

- **Direct instrumentation**
作为源代码程序的一部分内联添加的检测。

- **Endpoint**

- **Exporter**
是一个公开Prometheus指标的程序，通常将 non-prometheus 格式的指标转换为 Prometheus 支持的格式。

- **Instance**
唯一标识作业中目标的标签

- **Job**
具有相同目的的目标集合

- **Notification**
代表一组多个警报

- **Promdash**
原生Prometheus仪表盘构建器。它已被弃用，并被 Grafana 取代

- **Prometheus**
通常指的是Prometheus System的核心程序，也可指整个监控系统。

- **PromQL**
Prometheus Query Language

- **Pushgateway**
持续从批量作业中最新推出的指标

- **Remote Read**
允许从其它系统透明读取时间序列作为查询的一部分

- **Remote Read Adapter**
并非所有系统都支持远程读取。远程读取适配器便是用于此。

- **Remote Read Endpoint**
Prometheus进行远程读取时的对象

- **Remote Write**
允许动态地将采集的样本发送到其它系统

- **Remote Write Adapter**

- **Remote Write Endpoint**

- **Sample**
时间序列中某个时间点的单个值，Prometheus中，每个样本都包含一个`float64`和`ms`精度的时间戳。

- **Silence**
防止报警

- **Target**
抓取对象的定义



<br/>
<br/>
<br/>


## FAQ

faq: <https://prometheus.io/docs/introduction/faq/>



<br/>
<br/>

---

<br/>
<br/>



# 概念

CONCEPTS


<br/>


## 数据模型

Data model


Prometheus从根本上将所有数据存储为**时间序列(time series)**: 属于同一指标和同一标记维度的带时间戳值的流。除了存储时间序列，Prometheus还可以临时生成时间序列作为查询的结果。


<br/>
<br/>


### 指标名称和标签

Metric names and labels


每个时间序列都是有**指标名称(metric name)**和一组键值对(也称为**标签(label)**)来唯一标识。

指标名称： 可能包含ASCII字母，下划线，冒号。它必须匹配正则: `[a-zA-Z_:][a-zA-Z0-9_:]*`。
标签启用Prometheus的维度数据模型：
































<br/>
<br/>
<br/>


## 指标类型

metric types



<br/>
<br/>
<br/>


## 工作和实例

Job and Instance















<br/>
<br/>

---

<br/>
<br/>



# Prometheus


<br/>


## 入门

GETTING STARTED


本节介绍如何安装，配置，使用Prometheus的简单例子。你将在本地安装和运行Prometheus，将其配置为自我填充和示例应用程序，然后使用查询，规则和图表来使用收集的序列数据。

<br>

**下载**

下载地址: <https://prometheus.io/download/>

```
tar xvfz prometheus-*.tar.gz

cd prometheus-*
```

<br>

**配置和监控**
Prometheus通过在目标上通过HTTP endPoints来抓取指标，来收集受监控目标的指标。由于Prometheus也以相同的方式公开自身数据，它也可以获取和监测自身的健康状况。
虽然Prometheus Server只收集有关自身的数据在实践中不是很有用，但它是一个很好的示例。如`prometheus.yml`示例配置文件：

```yaml
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

<br>

**启动**
启动后，可访问9090端口查看状态。可访问`localhost:9090/metrics`查看有关自身的相关指标。

```
cd prometheus-2.3.2.linux-amd64
./prometheus --config.file="prometheus.yml"
```

![9090](/images/Prometheus/9090.png)

<br>

**使用表达式浏览器**
让我们看一下Prometheus收集的一些数据。要使用Prometheus的内建表达式浏览器(expression browser)，请跳转到`http://localhost:9090/graph`并选择`Graph -> Console`，在其中输入表达式。
绘制表达式图形同样在此操作。

```
#表达式
prometheus_target_interval_length_seconds


#表达式
prometheus_target_interval_length_seconds{quantile="0.99"}


#计算返回的时间序列数
count(prometheus_target_interval_length_seconds)
```

![表达式结果](/images/Prometheus/expression01.png)

![表达式图形](/images/Prometheus/expression02.png)

<br>

**启动简单的目标**
启动一些示例目标让Prometheus获取。
确保已安装Go表一起并设置了正常的GO PATH。

```
mkdir ./sample && cd sample

git clone https://github.com/prometheus/client_golang.git
cd client_golang/examples/random
go get -d
go build


# Start 3 example targets in separate terminals:
./random -listen-address=:9091
./random -listen-address=:9092
./random -listen-address=:9093


#访问
http://localhost:9091/metrices
http://localhost:9092/metrices
http://localhost:9093/metrices
```

<br>

**监控示例目标**
现在需要配置Prometheus来抓取目标。

```yaml
scrape_configs:
  - job_name:       'example-random'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:8080', 'localhost:8081']
        labels:
          group: 'production'

      - targets: ['localhost:8082']
        labels:
          group: 'canary'
```

重启Prometheus，检测`rpc_durations_seconds` metric来验证。

<br>

**配置规则**
Configure rules for aggregating scraped data into new time series

聚合超过数千个时间序列的查询在计算`ad-hoc`时会变慢。为了提高效率，Prometheus允许你通过配置的规则将预录表达式预先记录到全新的持久时间序列中。

创建规则文件`prometheus.rules.yml`：
```
#job_service:rpc_durations_seconds_count:avg_rate5m
groups:
- name: example
  rules:
  - record: job_service:rpc_durations_seconds_count:avg_rate5m
    expr: avg(rate(rpc_durations_seconds_count[5m])) by (job, service)
```

要是Prometheus选择此新规则，需要修改Prometheus配置：

```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
  evaluation_interval: 15s # Evaluate rules every 15 seconds.

  # Attach these extra labels to all timeseries collected by this Prometheus instance.
  external_labels:
    monitor: 'codelab-monitor'

rule_files:
  - 'prometheus.rules.yml'

scrape_configs:
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']

  - job_name:       'example-random'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:8091', 'localhost:8092']
        labels:
          group: 'production'

      - targets: ['localhost:9093']
        labels:
          group: 'canary'
```

重启Prometheus，使用`job_service:rpc_durations_seconds_count:avg_rate5m` metric验证。


<br/>
<br/>


## 安装


<br>

### 使用预编译的二进制文件


<br/>
<br/>


### 使用源码


<br/>
<br/>


### 使用Docker

所有的Prometheus服务都可以作为 Docker image 来使用。
Prometheus image 使用 volume 来存储实际的指标。对于生产部署，强烈建议使用 Data Volume Container 来升级数据的管理。

栗子：

```
#bind-mount
docker run -p 9090:9090 -v /tmp/prometheus.yml:/etc/prometheus.yml  prom/prometheus


#volume
docker run -p 9090:9090 -v /promethe-data  prom/prometheus  --config.file=/prometheus-data/prometheus.yml
```

<br>

**自定义镜像**

Dockerfile:

```
FROM prom/prometheus
ADD prometheus.yml /etc/prometheus/
xxx
```

构建：

```
docker build -t my-prometheus .
```


<br/>
<br/>


### 使用配置管理系统

- Ansible
- Chef
- Puppet
- SaltStack



<br/>
<br/>
<br/>


## 配置

Configuration


Prometheus通过命令行标志(flag)和配置文件进行配置。使用`./prometheus -h`查看所有命令行标志。
Prometheus可在运行时重新加载配置。

<br/>


### 配置文件

configuration file: <https://prometheus.io/docs/prometheus/latest/configuration/configuration/>

使用`--config.file`标志指定配置文件。配置文件使用`YAML`格式。

一个配置文件栗子:

```yaml
global:
  # How frequently to scrape targets by default.
  [ scrape_interval: <duration> | default = 1m ]

  # How long until a scrape request times out.
  [ scrape_timeout: <duration> | default = 10s ]

  # How frequently to evaluate rules.
  [ evaluation_interval: <duration> | default = 1m ]

  # The labels to add to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    [ <labelname>: <labelvalue> ... ]

# Rule files specifies a list of globs. Rules and alerts are read from
# all matching files.
rule_files:
  [ - <filepath_glob> ... ]

# A list of scrape configurations.
scrape_configs:
  [ - <scrape_config> ... ]

# Alerting specifies settings related to the Alertmanager.
alerting:
  alert_relabel_configs:
    [ - <relabel_config> ... ]
  alertmanagers:
    [ - <alertmanager_config> ... ]

# Settings related to the remote write feature.
remote_write:
  [ - <remote_write> ... ]

# Settings related to the remote read feature.
remote_read:
  [ - <remote_read> ... ]
```

<br>

各个配置项：

- `scrape_config`
- `tls_config`
- `azure_sd_config`
- `consul_sd_config`
- `dns_sd_config`
- `ec2_sd_config`
- `openstack_sd_config`
- `file_sd_config`
- `gce_sd_config`
- `kubernetes_sd_config`
- `marathon_sd_config`
- `nerve_sd_config`
- `serverset_sd_config`
- `triton_sd_config`
- `static_config`
- `relabel_config`
- `metric_relabel_configs`
- `alert_relabel_configs`
- `alertmanager_config`
- `remote_write`
- `remote_read`



















































<br/>
<br/>

---

<br/>
<br/>



# 可视化

Visualization


<br>


## 表达式浏览器

Expression browser


表达其浏览器在 Prometheus Server 的 `/graph` 处。
对于图形，请使用 Grafana 或 Console template。


<br/>
<br/>
<br/>


## Grafana

Grafana: <https://grafana.com/>

Grafana，美丽的分析和监控的开放平台，时序分析的开源那软件。

Grafana 支持查询 Prometheus。如下是一个Grafana仪表盘，用于查询Prometheus的数据：

![](/images/Prometheus/grafana_prometheus.png)


<br/>


### 安装

完整的安装说明，请查看Grafana Docs。


<br/>


#### CentOS

**RPM**

```
#sudo yum install <rpm package url>
sudo yum install https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-5.1.4-1.x86_64.rpm
```

**repo**

```
[grafana]
name=grafana
baseurl=https://packagecloud.io/grafana/stable/el/7/$basearch
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packagecloud.io/gpg.key https://grafanarel.s3.amazonaws.com/RPM-GPG-KEY-grafana
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt


sudo yum install -y grafana


#启动
systemctl start grafana-server


#命令行工具
grafana-cli
```

**包详情**

- Installs binary to `/usr/sbin/grafana-server`
- Copies init.d script to `/etc/init.d/grafana-server`
- Installs default file (environment vars) to `/etc/sysconfig/grafana-server`
- Copies configuration file to `/etc/grafana/grafana.ini`
- Installs systemd service (if systemd is available) name `grafana-server.service`
- The default configuration uses a log file at `/var/log/grafana/grafana.log`
- The default configuration specifies an sqlite3 database at `/var/lib/grafana/grafana.db`


<br>

**二进制tar文件**

```
# Download and unpack Grafana from binary tar (adjust version as appropriate).
curl -L -O https://grafanarel.s3.amazonaws.com/builds/grafana-2.5.0.linux-x64.tar.gz
tar zxf grafana-2.5.0.linux-x64.tar.gz

# Start Grafana.
cd grafana-2.5.0/
./bin/grafana-server web
```


<br/>
<br/>


#### Docker

```
#基础栗子
docker run -d -p 3000:3000 grafana/grafana


#配置化
docker run \
  -d \
  -p 3000:3000 \
  --name=grafana \
  -e "GF_SERVER_ROOT_URL=http://grafana.server.name" \
  -e "GF_SECURITY_ADMIN_PASSWORD=secret" \
  grafana/grafana:version


#默认环境变量值
GF_PATHS_CONFIG	/etc/grafana/grafana.ini
GF_PATHS_DATA	/var/lib/grafana
GF_PATHS_HOME	/usr/share/grafana
GF_PATHS_LOGS	/var/log/grafana
GF_PATHS_PLUGINS	/var/lib/grafana/plugins
GF_PATHS_PROVISIONING	/etc/grafana/provisioning

```


<br/>
<br/>


### 使用

默认情况下，访问`http://localhost:3000`来访问Grafana。默认登录的用户名和密码： `admin/admin`。

![Grafana](/images/Prometheus/grafana_login.png)

<br>

**创建Prometheus数据源**

![](/images/Prometheus/add_prometheus_datasource.png)

<br>

**创建Prometheus图表**



<br/>
<br/>
<br/>


## Console template

控制台模板允许使用Go templating language创建任意控制台。这些都是从Prometheus Server提供的。




































