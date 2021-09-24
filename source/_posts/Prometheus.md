---
title: Prometheus
date: 2018-09-11 11:01:12
mathjax: true
categories: DevOps
tags:
  - DevOps
  - Prometheus
  - Monitor
  - Alert
---


参考：

- Prometheus文档： <https://prometheus.io/docs>
- GitHub: <https://github.com/prometheus/>
- PrometheusAlert: <https://github.com/feiyu563/PrometheusAlert>

环境：

- CentOS7x86_64
- Prometheus v2.14

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

metric types: <https://prometheus.io/docs/concepts/metric_types/>

Prometheus Client Library提供了四个核心指标类型。这些目前仅在客户端和在有线协议(wire protocol)中区分。Prometheus Server尚未使用的类型信息和所有数据合并为无类型(untyped)时间序列。这在未来可能改变。

<br>

Prometheus clinet使用文档:

- [Go](https://godoc.org/github.com/prometheus/client_golang/prometheus)
- [Java](https://github.com/prometheus/client_java)
- [Python](https://github.com/prometheus/client_python)


<br>


### Counter

Counter，只增不减的计数器。

Counter是一个累计指标，代表一个单调递增计数器，即只增不减，除非重启或被重置为0。例如，你可以使用counter来代表服务的请求数、已完成的任务数、错误的数量...

不要用counter来暴露一个可以减少的值。例如，不要对当前运行的进程数使用`counter`类型，使用`gauge`类型。

一般在定义counter类型指标的名称时，推荐使用`xxx_total`作为后缀名。（如`http_request_total`）

```
# 获取HTTP请求量的增长率
rate(http_requests_total[5m])

# 统计前十
topk(10, http_requests_total)
```


<br>
<br>


### Gauge

Gauge，可以任意变化的仪表盘。

Gauge类型代表一个样本数据可以任意变化，即可增可减。通常用于像温度、内存使用率这种指标数据，也可表示能随时升降的计数（如当前的并发数）。

```
# 获取一段时间内的变化情况
dalta(cpu_temp_celsius{host="zeus"}[2h])

# 简单线性回归，预测未来数据
predict_linear(node_filesystem_free{job="node"}[2h], 4 * 3600) < 0

```


<br>
<br>


### Histogram

Histogram和Summary主用用于统计和分析样本的分布情况。

Histogram(直方图)在一段时间范围内对数据进行采样（通常是请求持续时间(request durations)和响应大小(response sizes)等），并将其计入可配置的存储桶(bucket)中，后续可通过指定区间筛选样本，也可以统计样本总数，最后一般将数据展示为直方图。

Histogram类型的样本会提供三种指标：

- 样本值分布在桶中的数量，命名为`xxx_bucket{le="<上边界>"}`。标识指标值小于等于上边界所有样本数量。
- 所有样本值的大小总和，命名为`xxx_sum`。
- 样本总数，命名为`xxx_count`，值和`xxx_bucket{le="+Inf}`相同。

可使用`histogram_quantile()`函数来计算Histogram类型样本的分位数。


<br>
<br>


### Summary

Summary(摘要)与Histogram类似，表示一段时间内的数据采集结果（通常是请求持续时间或响应大小）。它直接存储了分位数，而不是通过区间来计算。

Summary类型的样本也提供了三种指标：

- 样本值的分位数分布情况，命名为`xxx{quantile="<φ>"}`。
- 所有样本值的大小总和，命名为`xxx_sum`。
- 样本总数，命名为`xxx_count`。



<br/>
<br/>
<br/>




## 作业和实例

Job and Instance

Prometheus配置文件中配置。













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

各个配置项，详细详细请看文档: <https://prometheus.io/docs/prometheus/latest/configuration/configuration/>

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




<br>
<br>
<br>



### 记录规则

Recording rules

<br>


#### 配置规则

Configuring rules

Prometheus支持两种类型的可被配置的以规定的间隔进行评估的规则:

- recording rules
- alterting rules

要在Prometheus中包含规则，创建包含必要规则的语句并在Prometheus配置文件中通过`rule_files`字段配置并加载文件。规则使用YAML格式。

规则文件可在Prometheus运行通过发送`SIGHUP`到Prometheus来进行重载。只有在所有规则文件都是正确格式下才会应用更改。


<br>
<br>


#### 语法检查规则

Syntax-checking rules

要快速检查规则文件的语法是否正确，而无需启动Prometheus Server，可安装和运行Prometheus的`promtool`命令行工具:

```
go get github.com/prometheus/prometheus/cmd/promtool

promtool check rules /path/to/example.rules.yml
```

如果规则文件语法正确，会返回`0`状态码。如果语法错误，会返回错误信息和`1`状态码。


<br>
<br>


#### 记录规则

Recording rules

记录规则允许你预先计算经常需要或计算昂贵的表达式并保存它们的结果到一个新的时序集(set of time series)。查询预先计算的结果会比每次执行原始表达式快得多。这对Dashboard来说尤其有用，它经常刷新时间反复查询同样的表达式。

记录和告警规则位于一个规则组(rule group)。一个组内的规则在一个规定的间隔内依序运行。

规则文件语法:

```yaml
groups:
  [ - <rule_group> ]
```

栗子:

```
groups:
  - name: example
    relues:
    - record: job:http_inprogress_requests:sum
      expr: sum(http_inprogress_requests) by (job)
```

<br>

**<rule_group>**

```yaml
# The name of the group. Must be unique within a file.
name: <string>

# How often rules in the group are evaluated.
[ interval: <duration> | default = global.evaluation_interval ]

rules:
  [ - <rule> ... ]
```

<br>

**<rule>**

记录规则的语法:

```yaml
# The name of the time series to output to. Must be a valid metric name.
record: <string>

# The PromQL expression to evaluate. Every evaluation cycle this is
# evaluated at the current time, and the result recorded as a new set of
# time series with the metric name as given by 'record'.
expr: <string>

# Labels to add or overwrite before storing the result.
labels:
  [ <labelname>: <labelvalue>]
```

告警规则的语法:

```yaml
# The name of the alert. Must be a valid metric name.
alert: <string>

# The PromQL expression to evaluate. Every evaluation cycle this is
# evaluated at the current time, and all resultant time series become
# pending/firing alerts.
expr: <string>

# Alerts are considered firing once they have been returned for this long.
# Alerts which have not yet fired for long enough are considered pending.
[ for: <duration> | default = 0s ]

# Labels to add or overwrite for each alert.
labels:
  [ <labelname>: <tmp_string>]

# Annotations to add to each alert.
annotations:
  [ <labelname>: <tmpl_string> ]
```


<br>
<br>


### 告警规则

Alerting rules

告警规则允许你根据Prometheus表达式语言来定义告警条件，并发送提醒到外部服务。每当告警表达式在给定的时间内导致一个或多个矢量元素，告警计数主动作为这些元素的标签集。

<br>


#### 定义告警规则

在Prometheus中，告警规则的配置与记录规则的配置一样。

栗子:

```yaml
groups:
- name: example
  rules:
  - alerts: HighRequestLatency
    expr: job:request_latency_seconds:mean5m{job="myjob"} > 0.5
    for: 10m
    labels:
      severity: page
    annotations:
      summary: High request latency
```

可选的`for`子句导致Prometheus等待在第一次遇到一个新的表达式输出矢量元素和计数的告警作为点燃的此元素的一定持续时间。在这个栗子中，Prometheus将检查在点燃告警之前的每个10分钟的警告持续激活。元素是活跃的，但未点燃，出于待定(pending)状态。

`labels`子句允许指定一组附加标签到告警。任何目前有冲突的标签将被覆盖。标签的值可以作为模板。

`annotations`子句指定的一组信息可用来存储更长的附加信息。注释的值可以作为模板。


<br>
<br>


#### 模板

Templating

标签和注释的值可以使用`console template`作为模板。`$labels`变量保存一个告警实例的k/v键值对。已配置的外部标签可通过`$externalLabels`变量进行访问。`$value`变量保存告警实例的评估值。

```yaml
# To insert a firing element's label values:
{{ $labels.<labelname> }}

# To insert the numeric expression value of the firing element:
{{ $value }}
```

栗子:

```yaml
groups:
- name: example
  rules:

  # Alert for any instance that is unreachable for >5 minutes.
  - alert: InstanceDown
    expr: up == 0
    for: 5m
    labels:
      severity: page
    annotations:
      summary: "Instance {{ $labels.instance }} down"
      description: "{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 5 minutes."

  # Alert for any instance that has a median request latency >1s.
  - alert: APIHighRequestLatency
    expr: api_http_request_latencies_second{quantile="0.5"} > 1
    for: 10m
    annotations:
      summary: "High request latency on {{ $labels.instance }}"
      description: "{{ $labels.instance }} has a median request latency above 1s (current value: {{ $value }})s"
```


<br>
<br>


#### 运行时检查告警

Inspecting alerts during runtime

要手动检查告警是否活跃(active)(pending或firing)，请浏览原生Prometheus的Alerts栏目项。这里将确切地显示标签集，每个定义的告警当前的状态。

![](/images/Prometheus/alerts_state.png)

对于`pengding`和`firing`的告警，Prometheus还存储合成`ALERTS{alertname="<alert name>", alertstate="pending|firing", <additional alert labels>}`形式的时间序列。只要该警告是在所指示的active(pending或firing)状态，样本值被设置为1。当不再是这样时，该系列被标记为stale。


<br>
<br>


#### 发送告警通知

Sending alert notifications

Prometheus的告警规则善于盘算现在什么坏了(broken)，但是它不是一个成熟的通知解决方案。需要另一层添加汇总，通知速率限制，沉默和告警依赖于简单告警定义。在Prometheus的生态系统中，**Alertmanager**承担了这一角色。因此，Prometheus可以配置成定期发送关于告警状态信息到Alertmanager实例，然后采取调度权发送通知。
Prometheus通过集成服务发现，可配置为自动发现可用的Alertmanager实例。


<br>
<br>


### 模板

Template example

Prometheus在alerts的annotations和labels中支持模板化，以及在控制台页面。模板要针对本地数据库运行查询、迭代数据，使用条件、格式数据等能力。Prometheus模板语言是基于Go template system。

<br>


#### 简单告警字段模板

```yaml
alert: InstanceDown
expr: up == 0
for: 5m
labels:
  severity: page
annotations:
  summary: "Instance {{$labels.instance}} down"
  description: "{{$labels.instance}} of job {{$labels.job}} has been down for more than 5 minutes."
```

告警字段模板为每个点燃的告警在每一个规则迭代过程中执行，所以保持任意查询和模板的轻量化。如果你需要为告警编写更复杂的模板，建议链接到控制台。


<br>
<br>


#### 简单迭代

simple iteration

这显示的实例列表，以及它们是否up:

```yaml
{{ range query "up" }}
  {{ .Labels.instance }} {{ .Value }}
{{ end }}
```

特殊的`.`变量包含对于每次循环迭代当前样本的值。


<br>
<br>


#### 展示一个值

```yaml
{{ with query "some_metric{instance='someinstance'}" }}
  {{ . | first | value | humanize }
{{ end }}
```

Go和Go的模板语言两者都是强类型，因此必须检查阳平返回，以避免执行错误。
这里所包含的`prom_query_drilldown`模板处理，允许结果的格式，并链接到表达式浏览器。


<br>
<br>


#### 使用控制台url参数

Using console URL parameters

```yaml
{{ with printf "node_memory_MemTotal{job='node', instance='%s'}" .Params.instance | query}}
  {{ . | first | value | humanize1024 }}B
{{ end }}
```

如果作为`console.html?instance=hostname`访问, `.Params.instance`将评估`hostname`。


<br>
<br>


#### 高级的迭代

Advanced iteration

```yaml
<table>
{{ range printf "node_network_receive_bytes{job='node',instance='%s',device!='lo'}" .Params.instance | query | sortByLabel "device"}}
<tr><th colspan=2>{{ .Labels.device }}</th></tr>
<tr>
  <td>Received</td>
  <td>{{ with printf "rate(node_network_receive_bytes{job='node',instance='%s',device='%s'}[5m])" .Labels.instance .Labels.device | query }}{{ . | first | value | humanize }}B/s{{end}}</td>
</tr>
<tr>
  <td>Transmitted</td>
  <td>{{ with printf "rate(node_network_transmit_bytes{job='node',instance='%s',device='%s'}[5m])" .Labels.instance .Labels.device | query }}{{ . | first | value | humanize }}B/s{{end}}</td>
  </tr>{{ end }}
</table>
```

这里，我们迭代了所有网络设备，并显示每个设备的网络流量。随着`range`动作不指定变量，`.Params.instance`循环内不可用，`.`现在是作为循环变量。


<br>
<br>


#### 定义可重复使用的模板

Defining reusable templates

Prometheus支持定义可重复使用的模板。当与控制台库相结合时，使得可共享模板，这很有用。

```yaml
{{ /* Define the template */ }}
{{define "myTemplate"}}
  do something
{{ end }}

{{/* Use the template */}}
{{template "myTemplate"}}
```

模板仅限于一个参数。`args`函数可包装多个参数。

```yaml
{{define "myMultiArgTemplate"}}
  First argument: {{.arg0}}
  Second argument: {{.arg1}}
{{ end }}
{{ template "myMultiArgTemplate" (args 1 2)}}
```


<br>
<br>


### 模板引用

TEMPLATE REFERENCE

<br>


#### 数据结构

Data Structures

用于处理时间序列数据的主要数据结构栗子:

```
type sample struct {
    Labels map[string]string
    Value float64
}
```

栗子的指标名称(metric)编码在`Labels`map的特殊的`__name__`标签里。
`[]sample`表示实例列表。
Go中的`interface{}`与C中的void pointer类似。


<br>
<br>


#### 函数

除了Go模板提供的默认函数，Prometheus为模板查询结果提供了更易处理的函数。
如果函数在管道中使用，管道值将作为最后一个参数传递。

**Queries**

<br>

**Numbers**

<br>

**Strings**

<br>

**Others**


<br>
<br>


#### 模板类型差异

Template type differences

每种类型的模板提供了可用于参数模板的不同信息，并有一些其它差异。



<br>
<br>


### 规则单元测试

Unit Testing for Rules

你可使用`promtool`来测试你的规则。



<br>
<br>
<br>



## 查询

Querying

<br>


### 查询Prometheus

Prometheus提供了一个名为`PromQL`(Prometheus Query Language)功能化查询语言，让用户选择并实时汇总时间序列数据。表达式的结果可被显示为图形，可在Prometheus浏览器上查看，或通过HTTP API来获取。

<br>


#### 表达式语言数据类型

Expression language data types

在Prometheus表达式语言中，一个表达式或子表达式可以评估为四种类型中的一种:

- **瞬时向量(Instant vector)**: 包含每个时间序列的单个样品的一组时间序列，全部共享相同的时间戳
- **区间向量(Range vector)**: 包含随时间序列的范围的数据点的一组时间序列
- **标量(Scalar)**: 一个简单的数字浮点值
- **字符串(String)**: 一个简单的字符串值，当前未使用

根据不同的使用情况(graphing, displaying the output of an expression)，例如：瞬时向量表达式返回的数据类型是唯一可以直接绘制成图表的数据类型。


<br>
<br>


#### Literals

**String literals**

字符串可以被指定为在单引号、双引号或反引号内的文字。

PromQL遵循Go的转义规则。
不像Go，Prometheus不丢弃反引号里面的换行符。

```
"this is a string"
'these are unescaped: \n \\ \t'
`these are not unescaped: \n ' " \t`"'`
```

<br>

**Float literals**

标量浮点值可被逐字地写为`[-](digits)[.(digits)]`数字形式。

```
-2.43
```


<br>
<br>


#### 时序选择器

Time series Selectors

Prometheus中的所有正则表达式使用[RE2 syntax](https://github.com/google/re2/wiki/Syntax)。

<br>

**Instant vector selectors**

Instant vector selectors允许一组时间序列并为每个在给定的时间戳单一样品值的选择: 在最简单的格式中，只制定了一个指标名称。这导致了包含有该指标名称的所有时间序列的元素instant vector。

这个栗子将选择具有`http_requests_total`指标名称的所有时间序列:

```
http_requests_total
```

有可能通过附加一组标签在大括号来匹配进一步过滤这些时间序列。

这个栗子只选择`job label为`prometheus`和group lable为`canary的`http_requests_total`指标名称。

```
http_requests_total{job="prometheus", group="canary"}
```

也可将标签值负匹配，或匹配正则表达式。下面的标签匹配操作符存在:

- `=`: 等于;
- `!=` 不等于;
- `=~`: 正则匹配;
- `!~`: 非正则匹配.

举个栗子，以下匹配`staging`, `testing`, `development`环境变量和`GET`以外的HTTP方法的`http_requests_total`指标名称的所有时序。

```
http_requests_total{environment=~"staging|testing|development", method!="GET"}
```

栗子:

```
{job=~".*"} # Bad!

{job=~".+"}              # Good!
{job=~".*",method="get"} # Good!

{__name__=~"job:.*"}
```

<br>

**Range Vector Selectors**

Range vector literals与 instant vector literals类似，不同之处在于它选择了一个范围。时间序列放在方括号`[]`内。

时间范围被指定为一个数字，使用以下单位:

- `s`: 秒;
- `m`: 分;
- `h`: 时;
- `d`: 天;
- `w`: 周;
- `y`: 年。

栗子:

```
http_requests_total{job="prometheus"}[5m]
```

<br>

**Offset modifier**

`offset`修饰符允许为查询中的individual instant和range vectors改变时间偏移。

栗子:

```
http_requests_total offset 5m

sum(http_requests_total{method="GET"} offset 5m) // GOOD.

sum(http_requests_total{method="GET"}) offset 5m // INVALID.

rate(http_requests_total[5m] offset 1w)
```


<br>
<br>


#### 子查询

Subquery

子查询允许你为一个给定的range和resolution运行一个即时查询(instant)。子查询的结果为range vector。

语法:

```
# resolution可选。Default is the global evaluation interval.
Syntax: <instant_query> '[' <range> ':' [<resolution>] ']' [ offset <duration> ]
```



<br>
<br>



### 操作符

Operators: <https://prometheus.io/docs/prometheus/latest/querying/operators/>

<br>


#### 二元运算符

Binary Operators

Prometheus查询语言支持基本的逻辑和算数运算符。

<br>

**Arithmetic binary operators**

Prometheus中存在以下二元算术运算符:

- `+` (addition)
- `-` (subtraction)
- `*` (multiplication)
- `/` (division)
- `%` (modulo)
- `^` (power/exponentiation)

二元运算符在下列之间定义:

- scalar/scalar
- vector/scalar
- vector/vector

<br>

**Comparison binary operators**

Prometheus中有以下二元比较符:

- `==` (equal)
- `!=` (not-equal)
- `>` (greater-than)
- `<` (less-than)
- `>=` (greater-or-equal)
- `<=` (less-or-equal)

比较运算符在下列之间定义，默认情况下进行筛选。它们的行为可由运算符之后提供的`bool`进行修改，这将返回`0`或`1`而不是过滤。

- scalar/scalar
- vector/scalar
- vector/vector

<br>

**Logical/set binary operators**

logical/set 二元运算符尽在instan vectors之间定义:

- `and` (intersection)
- `or` (union)
- `unless` (complement)


<br>
<br>


#### 矢量匹配

Vector matching

矢量之间的操作试图找到为左手侧的每个条目匹配右手侧的元素。有两种基本类型匹配的行为: One-to-one 和 many-to-one/one-to-many。

<br>

**One-to-one vector matches**

一对一从操作的每一侧查找唯一的一对条目。在默认情况下，操作遵循如下格式`vector1 <operator> vector2`。如果它们有完全相同的一组标签和相应的值，则两个条目匹配。`ignoring`关键字允许匹配忽略某些标签，`on`关键字允许降低考虑的标签集来提供列表:

```
<vector expr> <bin-op> ignoring(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) <vector expr>
```

输入案例:

```
method_code:http_errors:rate5m{method="get", code="500"}  24
method_code:http_errors:rate5m{method="get", code="404"}  30
method_code:http_errors:rate5m{method="put", code="501"}  3
method_code:http_errors:rate5m{method="post", code="500"} 6
method_code:http_errors:rate5m{method="post", code="404"} 21

method:http_requests:rate5m{method="get"}  600
method:http_requests:rate5m{method="del"}  34
method:http_requests:rate5m{method="post"} 120
```

查询栗子:

```
method_code:http_errors:rate5m{code="500"} / ignoring(code) method:http_requests:rate5m
```

<br>

**Many-to-one and one-to-many vector matches**

多对一或一对多匹配指的是一侧能够匹配多侧的多个元素。这明确要求必须使用`group_left`或`group_right`修饰符，其中左/右确定该矢量有更高的基数。

```
<vector expr> <bin-op> ignoring(<label list>) group_left(<label list>) <vector expr>
<vector expr> <bin-op> ignoring(<label list>) group_right(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) group_left(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) group_right(<label list>) <vector expr>
```

查询案例:

```
method_code:http_errors:rate5m / ignoring(code) group_left method:http_request:rate5m
```

```
{method="get", code="500"}  0.04            //  24 / 600
{method="get", code="404"}  0.05            //  30 / 600
{method="post", code="500"} 0.05            //   6 / 120
{method="post", code="404"} 0.175           //  21 / 120
```


<br>
<br>


#### 聚合运算符

Aggregation operators

Prometheus支持以下内置聚合运算符，可以用于聚合单一瞬间向量的元素:

- `sum` (calculate sum over dimensions)
- `min` (select minimum over dimensions)
- `max` (select maximum over dimensions)
- `avg` (calculate the average over dimensions)
- `stddev` (calculate population standard deviation over dimensions)
- `stdvar` (calculate population standard variance over dimensions)
- `count` (count number of elements in the vector)
- `count_values` (count number of elements with the same value)
- `bottomk` (smallest k elements by sample value)
- `topk` (largest k elements by sample value)
- `quantile` (calculate φ-quantile (`0 ≤ φ ≤ 1`) over dimensions)

栗子:

```
<aggr-op> [without|by (<label list>)] ([parameter,] <vector expression>)

<aggr-op>([parameter,] <vector expression>) [without|by (<label list>)]

sum without (instance) (http_requests_total)

sum by (application, group) (http_requests_total)

count_values("version", build_version)

topk(5, http_requests_total)
```


<br>
<br>


#### 二元运算符优先级

Binary operator precedence

以下优先级由高到低:

- `^`
- `*`, `/`, `%`
- `+`, `-`
- `==`, `!=`, `<=`, `<`, `>=`, `>`
- `and`, `unless`
- `or`


<br>
<br>


### 函数

Founctions: <https://prometheus.io/docs/prometheus/latest/querying/functions/>

一些函数有默认的参数，如`year(v=vector(time()) instant-vector)`。

```
abs()
abs(v instant-vector) 返回输入向量的所有样本的绝对值




absent()
absent(v instant-vector)，判断是否存在

absent(nonexistent{job="myjob"})
# => {job="myjob"}

absent(nonexistent{job="myjob",instance=~".*"})
# => {job="myjob"}

absent(sum(nonexistent{job="myjob"}))
# => {}




ceil()
ceil(v instant-vector) 将v中所有元素的样本值向上四舍五入到最接近的整数


floor()
floor(v instant-vector) 与ceil()相反，将v中所有元素的样本值向下四舍五入到最接近的整数




changes()
changes(v range-vector) 输入一个区间向量，返回这个区间向量内每个样本数据值变化的次数（瞬时向量）。如果样本数据值没有发生变化，则返回结果为1




clamp_max()
clamp_max(v instant-vector, max scalar) 输入一个瞬时向量和最大值，样本数据值若大于max，则改为max，否则不变


clamp_min()
clamp_min(v instant-vector, min scalar) 输入一个瞬时向量和最小值，样本数据值若小于min，则改为min，否则不变




day_of_month()
day_of_month(v=vector(time()) instant-vector) 值范围为1-31


day_of_week()
day_of_week(v=vector(time()) instant-vector) 值范围为0-6


days_in_month()
days_in_month(v=vector(time()) instant-vector) 月份的天数，值范围为28-31


minute()
minute(v=vector(time()) instant-vector) 函数返回给定UTC时间当前小时的第多少分钟，范围为0-59


month()
month(v=vector(time()) instant-vector) 函数返回给定UTC时间当前属于第几个月，范围为1-12

year()
year(v=vector(time()) instant-vector) 返回被给定 UTC 时间的当前年份


hour()
hour(v=vector(time()) instant-vector) 值范围为0-23



delta()
delta(v range-vector) 它计算一个区间向量v的第一个元素和最后一个元素之间的差值，返回一个瞬时向量
delta(cpu_temp_celsius{host="zeus"}[2h]) # 现在和两小时前的CPU温度差


idelta()
idelta(v range-vector) 计算最后两个样本之间的差




deriv()
deriv(v range-vector) 使用简单的线性回归计算区间向量v中各个时间序列的导数




exp()
exp(v instant-vector) 输入一个瞬时向量，返回各个样本值的e的指数值




histogram_quantile()
histogram_quantile(φ float, b instant-vector)
histogram_quantile(0.9, rate(http_request_duration_seconds_bucket[10m])) # 计算过去10分钟内请求持续在90%
histogram_quantile(0.9, sum(rate(http_request_duration_seconds_bucket[10m])) by (job, le)) # 聚合



holt_winters()
holt_winters(v range-vector, sf scalar, tf scalar) 基于区间向量v，生成时间序列数据平滑值




increase()
increase(v range-vector) 获取区间向量中的第一个和最后一个样本并返回其增长量
increase(http_requests_total{job="api-server"}[5m]) # 区间向量中每个时间序列过去5分钟内HTTP请求数的增长数




label_join()



label_replace()



ln()
ln(v instant-vector) 计算瞬时向量v中所有样本数据的自然对数



log2()
log2(v instant-vector) 函数计算瞬时向量v中所有样本数据的二进制对数



log10()
log10(v instant-vector) 计算瞬时向量v中所有样本数据的十进制对数




predict_linear()
predict_linear(v range-vector, t scalar) 函数可以预测时间序列v在t秒后的值
predict_linear(node_filesystem_free{job="node"}[2h], 4 * 3600) < 0 # 基于2小时的样本数据，来预测主机可用磁盘空间的是否在4个小时候被占满



rate()
rate(v range-vector) 直接计算区间向量 v 在时间窗口内平均增长速率
rate(http_requests_total[5m]) 区间向量中每个时间序列过去5分钟内HTTP请求数的每秒增长率


irate()
irate(v range-vector) 用于计算区间向量的增长率，但是其反应出的是瞬时增长率。通过区间向量中最后两个两本数据来计算区间向量的增长速率。
irate(http_requests_total{job="api-server"}[5m]) # 区间向量中每个时间序列过去 5 分钟内最后两个样本数据的 HTTP 请求数的增长率



resets()
resets(v range-vector) 的参数是一个区间向量。对于每个时间序列，它都返回一个计数器重置的次数。两个连续样本之间的值的减少被认为是一次计数器重置。



round()
round(v instant-vector, to_nearest=1 scalar) 与ceil和floor函数类似，返回向量中所有样本值的最接近的整数



scalar()
scalar(v instant-vector) 函数的参数是一个单元素的瞬时向量,它返回其唯一的时间序列的值作为一个标量



sort()
sort(v instant-vector) 函数对向量按元素的值进行升序排序



sort_desc()
降序排列



sqrt()
sqrt(v instant-vector) 计算向量v 所有元素的平方根






vector()
vector(s scalar)





<aggregation>_over_time()
avg_over_time(range-vector) : 区间向量内每个度量指标的平均值。
min_over_time(range-vector) : 区间向量内每个度量指标的最小值。
max_over_time(range-vector) : 区间向量内每个度量指标的最大值。
sum_over_time(range-vector) : 区间向量内每个度量指标的求和。
count_over_time(range-vector) : 区间向量内每个度量指标的样本数据个数。
quantile_over_time(scalar, range-vector) : 区间向量内每个度量指标的样本数据值分位数，φ-quantile (0 ≤ φ ≤ 1)。
stddev_over_time(range-vector) : 区间向量内每个度量指标的总体标准差。
stdvar_over_time(range-vector) : 区间向量内每个度量指标的总体标准方差。

```


<br>
<br>


### 查询栗子

Query examples: <https://prometheus.io/docs/prometheus/latest/querying/examples/>

<br>


#### 简单时序选择

返回指标`http_requests_total`的所有时间序列数据:

```
http_requests_total
```

返回指标名为`http_requests_total`，给定标签job和handler:

```
http_requests_total{job="apiserver", handler="/api/comments"}
```

加上时间，5分钟内:

```
http_requests_total{job="apiserver", handler="/api/comments"}[5m]
```

使用正则:

```
http_requests_total{job=~".*server"}
```

http状态码不为4xx:

```
http_requests_total{status!~"4.."}
```


<br>
<br>


#### 子查询

Return the 5-minute rate of the http_requests_total metric for the past 30 minutes, with a resolution of 1 minute.

```
rate(http_requests_total[5m])[30m:1m]
```

嵌套子查询:

```
max_over_time(deriv(rate(distance_covered_total[5s])[30s:5s])[10m:])
```


<br>
<br>


#### 使用函数，运算符

过去5分钟的平均值

```
rate(http_requests_total[5m])
```

过去5分钟平均值综合

```
sum by (job) (
  rate(http_requests_total[5m])
)
```

如果两个指标具有相同维度的标签，我们可以使用二元操作符计算样本数据:

```
(instance_memory_limit_bytes - instance_memory_usage_bytes) / 1024 / 1024


# 同样的表达式，只不过通过应用相加
sum by (app, proc) (
  instance_memory_limit_bytes - instance_memory_usage_bytes
) / 1024 / 1024
```


获取CPU使用最高的三个样本:

```
topk(3, sum by (app, proc) (rate(instance_cpu_time_ns[5m])))
```

假设一个服务实例只有一个时间序列数据，那么我们可以通过下面表达式统计出每个应用的实例数量:

```
count(instance_cpu_time_ns) by (app)
```


<br>
<br>



### HTTP API

HTTP API: <https://prometheus.io/docs/prometheus/latest/querying/api/>

目前稳定的HTTP API在Prometheus Server的`/api/v1`下。

<br>


#### 格式

API响应格式为JSON。每个成功的请求都返回`2xx`状态码。

到达API处理程序无效的请求返回一个错误的JSON对象和以下状态码之一:

- `400 Bad Request`: 当参数错误或者缺失
- `422 Unprocessable Entity`: 当表达式无法执行
- `503 Service Unavailable`:  当请求超时或者被中断时

JSON响应包格式如下:

```json
{
  "status": "success" | "error",
  "data": <data>,

  // Only set if status is "error". The data field may still hold
  // additional data.
  "errorType": "<string>",
  "error": "<string>",

  // Only if there were warnings while executing the request.
  // There will still be data in the data field.
  "warnings": ["<string>"]
}
```

请求中输入的时间为RFC3339或Unix原子时间，输出时间戳总是Unix原子时间。

查询参数的名称可用中括号`[]`重复次数。

`<duration>`占位符指的是`[0-9]+[smhdwy]`形式的Prometheus 持续时间字符串。例如，`5m`表示5分钟的持续时间。


<br>
<br>


#### 表达式查询

查询语言表达式可在单个时刻或一定范围内进行评估。以下部分用于描述每个类型的表达式查询的API endpoint。

<br>

**瞬时查询(Instant query)**

端点:

```
GET /api/v1/query
POST /api/v1/query
```

URL查询参数:

```
# 表达式
query=<string>

# 时间戳，可选。默认使用当前系统时间
time=<rfc3339 | unix_timestamp>

# 超时，可选。默认使用全局的-query.timeout参数
timeout=<duration>
```

查询结果的`data`部分格式如下:

```
{
  "resultType": "matrix" | "vector" | "scalar" | "string",
  "result": <value>
}
```

栗子:

```
$ curl 'http://localhost:9090/api/v1/query?query=up&time=2015-07-01T20:10:51.781Z'
{
   "status" : "success",
   "data" : {
      "resultType" : "vector",
      "result" : [
         {
            "metric" : {
               "__name__" : "up",
               "job" : "prometheus",
               "instance" : "localhost:9090"
            },
            "value": [ 1435781451.781, "1" ]
         },
         {
            "metric" : {
               "__name__" : "up",
               "job" : "node",
               "instance" : "localhost:9100"
            },
            "value" : [ 1435781451.781, "0" ]
         }
      ]
   }
}
```

<br>


**区间查询(range query)**

端点:

```
GET /api/v1/query_range
POST /api/v1/query_range
```

URL查询参数:

```
# 表达式
query=<string>

# 时间戳
start=<rfc3339 | unix_timestamp>
end=<rfc3339 | unix_timestamp>

# 查询时间步长，时间区间内每step秒执行一次
step=<duration | float>

# 超时，可选
timeout=<duration>
```

查询结果的`data`部分格式如下:

```
{
  "resultType": "matrix",
  "result": <value>
}
```

栗子:

```
$ curl 'http://localhost:9090/api/v1/query_range?query=up&start=2015-07-01T20:10:30.781Z&end=2015-07-01T20:11:00.781Z&step=15s'
{
   "status" : "success",
   "data" : {
      "resultType" : "matrix",
      "result" : [
         {
            "metric" : {
               "__name__" : "up",
               "job" : "prometheus",
               "instance" : "localhost:9090"
            },
            "values" : [
               [ 1435781430.781, "1" ],
               [ 1435781445.781, "1" ],
               [ 1435781460.781, "1" ]
            ]
         },
         {
            "metric" : {
               "__name__" : "up",
               "job" : "node",
               "instance" : "localhost:9091"
            },
            "values" : [
               [ 1435781430.781, "0" ],
               [ 1435781445.781, "0" ],
               [ 1435781460.781, "1" ]
            ]
         }
      ]
   }
}
```


<br>
<br>


#### 查询元数据


**通过标签匹配器查找序列(Finding series by label matchers)**

端点:

```
GET /api/v1/series
POST /api/v1/series
```

URL请求参数:

```
# 标签选择器是 series_selector。必须至少提供一个match[]参数
match[]=<series_selector>

# 时间戳
start=<rfc3339 | unix_timestamp>
end=<rfc3339 | unix_timestamp>
```

返回结果的data部分，由k/v键值对的对象列表组成。
栗子:

```
$ curl -g 'http://localhost:9090/api/v1/series?' --data-urlencode='match[]=up' --data-urlencode='match[]=process_start_time_seconds{job="prometheus"}'
{
   "status" : "success",
   "data" : [
      {
         "__name__" : "up",
         "job" : "prometheus",
         "instance" : "localhost:9090"
      },
      {
         "__name__" : "up",
         "job" : "node",
         "instance" : "localhost:9091"
      },
      {
         "__name__" : "process_start_time_seconds",
         "job" : "prometheus",
         "instance" : "localhost:9090"
      }
   ]
}
```

<br>

**获取标签名(label)**

端点:

```
GET /api/v1/labels
POST /api/v1/labels
```

返回结果的`data`部分是一个标签名字符串列表。
栗子:

```
$ curl 'localhost:9090/api/v1/labels'
{
    "status": "success",
    "data": [
        "__name__",
        "call",
        "code",
        "config",
        "dialer_name",
        "endpoint",
        "event",
        "goversion",
        "handler",
        "instance",
        "interval",
        "job",
        "le",
        "listener_name",
        "name",
        "quantile",
        "reason",
        "role",
        "scrape_job",
        "slice",
        "version"
    ]
}
```

<br>

**查询标签值**

请求如下端点:

```
GET /api/v1/label/<label_name>/values
```

JSON响应的`data`部分是标签值字符串列表。
栗子:

```
$ curl http://localhost:9090/api/v1/label/job/values
{
   "status" : "success",
   "data" : [
      "node",
      "prometheus"
   ]
}
```


<br>
<br>


#### 表达式查询结果的格式

Expression query result formats

表达式查询结果可能会在`data`部分的`result`字段中返回以下响应值。

<br>

**区间向量(range vectors)**

区间向量返回`matrix` resultType。`result`的响应格式如下:

```
[
  {
    "metric": { "<label_name>": "<label_value>", ... },
    "values": [ [ <unix_time>, "<sample_value>" ], ... ]
  },
  ...
]
```

<br>

**瞬时向量(instant vectors)**

瞬时向量返回`vector` resultType。`result`的响应格式如下:

```
[
  {
    "metric": { "<label_name>": "<label_value>", ... },
    "value": [ <unix_time>, "<sample_value>" ]
  },
  ...
]
```

<br>

**标量(Scalars)**

标量返回`scalar` resultType。`result`的响应格式如下:

```
[ <unix_time>, "<scalar_value>" ]
```

<br>

**字符串(string)**

字符串返回`string` resultType。`result`的响应格式如下:

```
[ <unix_time>, "<string_value>" ]
```


<br>
<br>


#### 目标

Targets

以下端点返回Prometheus目标发现的当前状态的概述:

```
GET /api/v1/targets
```

栗子:

```
curl http://localhost:9090/api/v1/targets
{
  "status": "success",
  "data": {
    "activeTargets": [
      {
        "discoveredLabels": {
          "__address__": "127.0.0.1:9090",
          "__metrics_path__": "/metrics",
          "__scheme__": "http",
          "job": "prometheus"
        },
        "labels": {
          "instance": "127.0.0.1:9090",
          "job": "prometheus"
        },
        "scrapeUrl": "http://127.0.0.1:9090/metrics",
        "lastError": "",
        "lastScrape": "2017-01-17T15:07:44.723715405+01:00",
        "health": "up"
      }
    ],
    "droppedTargets": [
      {
        "discoveredLabels": {
          "__address__": "127.0.0.1:9100",
          "__metrics_path__": "/metrics",
          "__scheme__": "http",
          "job": "node"
        },
      }
    ]
  }
}
```


<br>
<br>


#### 规则

Rules

此api端点返回当前载入的告警和记录规则的列表。此外，它还会返回由每个告警规则的Prometheus实例点燃的当前激活的告警。

```
GET /api/v1/rules
```

```
curl http://localhost:9090/api/v1/rules

{
    "data": {
        "groups": [
            {
                "rules": [
                    {
                        "alerts": [
                            {
                                "activeAt": "2018-07-04T20:27:12.60602144+02:00",
                                "annotations": {
                                    "summary": "High request latency"
                                },
                                "labels": {
                                    "alertname": "HighRequestLatency",
                                    "severity": "page"
                                },
                                "state": "firing",
                                "value": "1e+00"
                            }
                        ],
                        "annotations": {
                            "summary": "High request latency"
                        },
                        "duration": 600,
                        "health": "ok",
                        "labels": {
                            "severity": "page"
                        },
                        "name": "HighRequestLatency",
                        "query": "job:request_latency_seconds:mean5m{job=\"myjob\"} > 0.5",
                        "type": "alerting"
                    },
                    {
                        "health": "ok",
                        "name": "job:http_inprogress_requests:sum",
                        "query": "sum(http_inprogress_requests) by (job)",
                        "type": "recording"
                    }
                ],
                "file": "/rules.yaml",
                "interval": 60,
                "name": "example"
            }
        ]
    },
    "status": "success"
}
```


<br>
<br>


#### 告警

Alerts

此端点返回所有激活的告警的列表。

```
GET /api/v1/alerts
```

```
curl http://localhost:9090/api/v1/alerts

{
    "data": {
        "alerts": [
            {
                "activeAt": "2018-07-04T20:27:12.60602144+02:00",
                "annotations": {},
                "labels": {
                    "alertname": "my-alert"
                },
                "state": "firing",
                "value": "1e+00"
            }
        ]
    },
    "status": "success"
}
```


<br>
<br>


#### 查询目标元数据

Query target metadata

此段点返回目前由目标爬取的关于指标的元数据。这是实验性的，未来可能发生改变。

```
GET /api/v1/targets/metadata
```

URL查询参数:

- `match_target=<label_selectors>`: 标签选择器通过标签集匹配的目标。如果为空，则所有目标都被选中。
- `metics=<string>`: 检索元数据的指标名称。如果为空，则所有指标元数据都被检索。
- `limit=<number>`: 匹配的目标的最大数量。

栗子:

```
curl -G http://localhost:9091/api/v1/targets/metadata \
    --data-urlencode 'metric=go_goroutines' \
    --data-urlencode 'match_target={job="prometheus"}' \
    --data-urlencode 'limit=2'
{
  "status": "success",
  "data": [
    {
      "target": {
        "instance": "127.0.0.1:9090",
        "job": "prometheus"
      },
      "type": "gauge",
      "help": "Number of goroutines that currently exist.",
      "unit": ""
    },
    {
      "target": {
        "instance": "127.0.0.1:9091",
        "job": "prometheus"
      },
      "type": "gauge",
      "help": "Number of goroutines that currently exist.",
      "unit": ""
    }
  ]
}
```

栗子:

```
curl -G http://localhost:9091/api/v1/targets/metadata \
    --data-urlencode 'match_target={instance="127.0.0.1:9090"}'
{
  "status": "success",
  "data": [
    // ...
    {
      "target": {
        "instance": "127.0.0.1:9090",
        "job": "prometheus"
      },
      "metric": "prometheus_treecache_zookeeper_failures_total",
      "type": "counter",
      "help": "The total number of ZooKeeper failures.",
      "unit": ""
    },
    {
      "target": {
        "instance": "127.0.0.1:9090",
        "job": "prometheus"
      },
      "metric": "prometheus_tsdb_reloads_total",
      "type": "counter",
      "help": "Number of times the database reloaded block data from disk.",
      "unit": ""
    },
    // ...
  ]
}
```


<br>
<br>


#### 告警器

AlertManagers

此端点返回Prometheus alertmanager discovery的当前状态的概述:

```
GET /api/v1/alertmanagers
```

active和dropped Alertmanagers都是响应的一部分:

```
curl http://localhost:9090/api/v1/alertmanagers
{
  "status": "success",
  "data": {
    "activeAlertmanagers": [
      {
        "url": "http://127.0.0.1:9090/api/v1/alerts"
      }
    ],
    "droppedAlertmanagers": [
      {
        "url": "http://127.0.0.1:9093/api/v1/alerts"
      }
    ]
  }
}
```


<br>
<br>


#### 状态

Status

以下状态端点暴露当前Prometheus配置。

<br>

**Config**

此端点返回当前加载的配置文件:

```
GET /api/v1/status/config
```

配置返回为转存的YAML文件。由于YAML库的限制，YAML中不包含注释:

```
curl http://localhost:9090/api/v1/status/config
{
  "status": "success",
  "data": {
    "yaml": "<content of the loaded config file in YAML>",
  }
}
```

<br>

**Flags**

此端点返回Prometheus配置的标志值:

```
GET /api/v1/status/flags
```

所有值的结果类型都是字符串:

```
curl http://localhost:9090/api/v1/status/flags
{
  "status": "success",
  "data": {
    "alertmanager.notification-queue-capacity": "10000",
    "alertmanager.timeout": "10s",
    "log.level": "info",
    "query.lookback-delta": "5m",
    "query.max-concurrency": "20",
    ...
  }
}
```

<br>

**RunTime Information**

此端点返回Prometheus Server的各种运行信息属性:

```
GET /api/v1/status/runtimeinfo
```

根据运行属性，返回的值有两种类型:

```
curl http://localhost:9090/api/v1/status/runtimeinfo
{
  "status": "success",
  "data": {
    "startTime": "2019-11-02T17:23:59.301361365+01:00",
    "CWD": "/",
    "reloadConfigSuccess": true,
    "lastConfigTime": "2019-11-02T17:23:59+01:00",
    "chunkCount": 873,
    "timeSeriesCount": 873,
    "corruptionCount": 0,
    "goroutineCount": 48,
    "GOMAXPROCS": 4,
    "GOGC": "",
    "GODEBUG": "",
    "storageRetention": "15d"
  }
}
```

<br>

**Build Information**

此端点返回各种关于Prometheus Server的构建信息属性:

```
GET /api/v1/status/buildinfo
```

所有结果的值的类型都是字符串:

```
curl http://localhost:9090/api/v1/status/buildinfo
{
  "status": "success",
  "data": {
    "version": "2.13.1",
    "revision": "cb7cbad5f9a2823a622aaa668833ca04f50a0ea7",
    "branch": "master",
    "buildUser": "julius@desktop",
    "buildDate": "20191102-16:19:59",
    "goVersion": "go1.13.1"
  }
}
```


<br>
<br>


#### TSDB Admin APIs

这些都是为高级用户公开的数据库功能的API。除非设置`--web.enable-admin-api`，否则不启用这些API。

我们也公开了一个gRPC API。这是实验性的，未来可能会改变。

<br>

**Snapshot**

创建所有当前数据的快照`snapshots/datetime-rand`早TSDB的数据目录并返回目录作为响应。它将可选地跳过快照数据尽在头部块，并且未被压缩到磁盘。

```
POST /api/v1/admin/tsdb/snapshot
PUT /api/v1/admin/tsdb/snapshot
```

URL查询参数:

- `skip_head=<bool>`: 跳过存在于头部块(head block)的数据，可选。

```
curl -XPOST http://localhost:9090/api/v1/admin/tsdb/snapshot
{
  "status": "success",
  "data": {
    "name": "20171210T211224Z-2be650b6d019eb54"
  }
}
```

快照现在位于: `<data-dir>/snapshots/20171210T211224Z-2be650b6d019eb54`

<br>

**Delete Series**

删除在一个时间范围内选择的一些列数据。实际数据仍然存在于磁盘上，并在未来被清除或通过点击Clean Tombstones endpoint来明确清理。
如果成功，返回`204`。

```
POST /api/v1/admin/tsdb/delete_series
PUT /api/v1/admin/tsdb/delete_series
```

URL查询参数:

- `match[]=<series_selector>`: 至少必须提供一个
- `start=<rfc3339 | unix_timestamp>`
- `end=<rfc3339 | unix_timestamp>`

未指定时间范围将删除匹配的所有数据。

```
curl -X POST -g 'http://localhost:9090/api/v1/admin/tsdb/delete_series?match[]=up&match[]=process_start_time_seconds{job="prometheus"}'
```

<br>

**Clean Tombstones**

从磁盘中删除已删除的数据并清理现有的tombstones。这可在清理数据后腾出磁盘空间。

如果成功，返回`204`。

```
POST /api/v1/admin/tsdb/clean_tombstones
PUT /api/v1/admin/tsdb/clean_tombstones
```

不需要任何参数或body:

```
curl -XPOST http://localhost:9090/api/v1/admin/tsdb/clean_tombstones
```




<br>
<br>
<br>




## 存储

Storage: <https://prometheus.io/docs/prometheus/latest/storage/>

Prometheus包含了一个本地磁盘上的时序数据库(time series database)，但是可选地与远程存储系统集成。

<br>


### 本地存储

Local storage

Prometheus本地时序数据库存储时序数据自定义格式存储到磁盘上。

<br>

**On-disk layout**

Prometheus按两小时为一个时间窗口分组存储在一个块(block)中。每个块是一个单独地目录，里面包含该时间窗口内的所有样本数据(chunks)，元数据文件(meta.json)以及索引文件(index)。其中索引文件会将指标名称和标签索引到样板数据的时间序列中。此期间如果通过 API 删除时间序列，删除记录会保存在单独的逻辑文件`tombstone`当中。

当前样本数据所在的块会被直接保存在内存中，不会持久化到磁盘中。为了确保Prometheus发生崩溃或重启时能够恢复数据，Prometheus启动时会通过预写日志（write-ahead-log(WAL)）重新记录，从而恢复数据。预写日志文件保存在`wal`目录中，每个文件大小为`128MB`。wal 文件包括还没有被压缩的原始数据，所以比常规的块文件大得多。一般情况下，Prometheus 会保留三个 wal 文件，但如果有些高负载服务器需要保存两个小时以上的原始数据，wal文件的数量就会大于3个。


Prometheus块数据的目录结构如下所示:

```
./data
├── 01BKGV7JBM69T2G1BGBGM6KB12
│   └── meta.json
├── 01BKGTZQ1SYQJTR4PB43C8PD98
│   ├── chunks
│   │   └── 000001
│   ├── tombstones
│   ├── index
│   └── meta.json
├── 01BKGTZQ1HHWHV8FBJXW1Y3W0K
│   └── meta.json
├── 01BKGV7JC0RY8A6MACW02A2PJD
│   ├── chunks
│   │   └── 000001
│   ├── tombstones
│   ├── index
│   └── meta.json
└── wal
    ├── 00000002
    └── checkpoint.000001
```

本地存储的局限性是它无法构建集群(clustered)或副本(replicated)。因此，如果本地磁盘或节点出现故障，存储将无法扩展和迁移。使用RAID用于磁盘的可用性，使用快照用于备份，容量规划...建议提高耐用性。如果你对数据持久化的要求不是很严格，可以使用本地磁盘存储多达数年的数据。

可替代地，外部存储可通过使用`remote read/write APIs`。仔细评估这些系统，因为它们在耐用性，性能和效率差异很大。

有关存储格式的详细信息，请参考 [TSDB格式](https://github.com/prometheus/prometheus/blob/master/tsdb/docs/format/README.md)


<br>
<br>


### Compaction

最初两个小时的块最终会在后台被压缩成更长的块。

<br>


### 操作配置

Prometheus提供了几个标志来允许配置本地存储。最重要的几个:

```
# 数据存储路径，默认data/
--storage.tsdb.path


# 样本数据在存储中保存的时间。超过该时间限制的数据就会被删除。默认15d
-storage.tsdb.retention.time


# 每个块的最大字节数（不包括 wal 文件）。如果超过限制，最早的样本数据会被优先删除。支持的单位有 KB, MB, GB, PB。默认0，即为不限制
--storage.tsdb.retention.size


# 压缩wal
--storage.tsdb.wal-compression
```

<br>

一般情况下，Prometheus中存储的每一个样本大概会占用1-2Byte。因此，如果需要对Prometheus Server的本地磁盘空间做容量规划，可通过以下公式计算:

```
needed_disk_space = retention_time_seconds * ingested_samples_per_second * bytes_per_sample
```

从上面公式中可以看出在保留时间（`retention_time_seconds`）和样本大小（`bytes_per_sample`）不变的情况下，如果想减少本地磁盘的容量需求，只能通过减少每秒获取样本数（`ingested_samples_per_second`）的方式。因此有两种手段，一是减少时间序列的数量，二是增加采集样本的时间间隔。考虑到 Prometheus 会对时间序列进行压缩效率，减少时间序列的数量效果更明显。































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

























<br>
<br>

---

<br>
<br>


























# 示例配置


由于prometheus自带的alertmanager对国内通知支持不够完善，因此使用PrometheusAlert做告警通知。


<br>


## prometheus配置样例

prometheus的配置示例:

```yaml
# my global config
global:
  scrape_interval:     1m # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 1m # Evaluate rules every 15 seconds. The default is every 1 minute.
  scrape_timeout: 1m
  # scrape_timeout is set to the global default (10s).
  # for thanos
  external_labels:
    region: ali # hw|tx
    replica: full

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets: ["localhost:9093"]

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "rules/*.yml"  # 通用告警规则模板
  - "rules/nodes/*.yml"  # 各主机告警规则

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: node-process-exporter
    file_sd_configs:
      - files:
        - "discovery/exporter/*.yml"
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

  - job_name: mongodb-exporter
    file_sd_configs:
      - files:
        - "discovery/mongodb/*.yml"
  - job_name: kafka-exporter
    file_sd_configs:
      - files:
        - "discovery/kafka/*.yml"

  - job_name: aliyun-exporter
    static_configs:
        - targets: ["localhost:9525", "localhost:9526", "localhost:9527", "localhost:9528", "localhost:9529"]

  - job_name: blackbox_exporter
    metrics_path: /probe
    file_sd_configs:
      - files:
        - "discovery/blackbox/*.yml"
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [module]
        target_label: __param_module
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115

```


<br>
<br>


## alertmanager配置样例

alertmanager的配置示例:

```yml
# global config
global:
  resolve_timeout: 5m
  # smtp config
  # slack config
  # wechat config
  # http config

# templates config
templates: []

# route tree
route:
  receiver: 'web.hook.prometheusalert'
  group_by:
    - instanceId
    - hostname
  group_wait: 30s
  group_interval: 1m
  # notification again
  repeat_interval: 10m
  # continue config
  # match: [ <labelname>: <labelvalue>, ... ]
  # match_re: [ <labelname>: <regex>, ... ]
  routes:
  - match:
      severity: warning
      repeat_interval: 1h
  - match_re:
      severity: notice|info
      repeat_interval: 6h

# receiver lists
receivers:
- name: 'web.hook.prometheusalert'
  webhook_configs:
      - url: 'http://127.0.0.1:8080/prometheus/alert'

# list of inhibit rules
inhibit_rules:
  # aliyun
  - source_match:
      severity: 'critical'
    target_match_re:
      severity: 'warning'
    # source_match_re
    # target_match_re
    # Labels that must have an equal value in the source and target
    equal: ['instance','job','instanceId','kind']
  # nodes
  - source_match:
      severity: 'critical'
    target_match_re:
      severity: 'warning'
    equal:
    - instance
    - job
    - hostname
    - kind
```


<br>
<br>


## prometheus自动发现

prometheus的文件自动发现示例:

```yaml
- labels:
    hostname: localhost
    hostgroup: test
  targets:
    - "localhost:9100" # node-exporter
    - "localhost:9256" # process-exporter
```


<br>
<br>


## prometheus告警规则

prometheus的告警规则示例:

```yml
groups:
- name: node-cpu
  rules:
  # cpu核数
  - record: instance:node_cpus:count
    expr: count without (cpu, mode) (node_cpu_seconds_total{mode="idle"})
  # 每个cpu使用率
  - record: instance_cpu:node_cpu_seconds_not_idle:rate2m
    expr: sum without (mode) (1 - rate(node_cpu_seconds_total{mode="idle"}[2m]))
  # 总cpu使用率
  - record: instance:node_cpu_utilization:ratio
    expr: avg without (cpu) (instance_cpu:node_cpu_seconds_not_idle:rate2m)

  - alert: cpu使用率大于85%
    ### expr: (1 - avg(irate(node_cpu_seconds_total{mode="idle"}[5m])) by(instance,hostname)) * 100 > 85
    expr: instance:node_cpu_utilization:ratio * 100 > 85
    for: 3m
    labels:
      severity: warning
      level: 2
      kind: CpuUsage
    annotations:
      summary: "cpu使用率大于85%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}的cpu使用率: {{ $value | humanize }}%"

  - alert: cpu使用率大于90%
    ### expr: (1 - avg(irate(node_cpu_seconds_total{mode="idle"}[5m])) by(instance,hostname)) * 100 > 90
    expr: instance:node_cpu_utilization:ratio * 100 > 90
    for: 1m
    labels:
      severity: critical
      level: 3
      kind: CpuUsage
    annotations:
      summary: "cpu使用率大于90%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}的cpu使用率: {{ $value | humanize }}%"
      wxurl: "xxx"

  - alert: cpu使用率一分钟内增长30%且大于70%
    expr: delta(instance:node_cpu_utilization:ratio[2m]) * 100 > 30 and on(hostname) instance:node_cpu_utilization:ratio * 100 > 70
    labels:
      severity: warning
      level: 2
      kind: CpuUsageDelta
    annotations:
      summary: "cpu使用率一分钟内增长30%且大于70%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }} 的cpu使用率一分钟内增长30%且大于70%，增长率: {{ $value | humanize }}%"
      wxurl: "xxx"

  - alert: cpu使用率一分钟内增长40%且大于80%
    expr: delta(instance:node_cpu_utilization:ratio[2m]) * 100 > 40 and on(hostname) instance:node_cpu_utilization:ratio * 100 > 80
    labels:
      severity: critical
      level: 3
      kind: CpuUsageDelta
    annotations:
      summary: "cpu使用率一分钟内增长40%且大于80%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }} 的cpu使用率一分钟内增长40%且大于80%，增长率: {{ $value | humanize }}%"

  - alert: cpu使用率一分钟内增长50%
    expr: delta(instance:node_cpu_utilization:ratio[2m]) * 100 > 50
    labels:
      severity: critical
      level: 3
    annotations:
      summary: "cpu使用率一分钟内增长50%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }} 的cpu使用率一分钟内增长50%，增长率: {{ $value | humanize }}%"

  - alert: cpu负载大于Cores
    ### expr: node_load1 > count(node_cpu_seconds_total{mode="idle"}) without (cpu,mode)
    expr: node_load1 > instance:node_cpus:count
    for: 3m
    labels:
      severity: warning
      level: 2
      kind: CpuLoad
    annotations:
      summary: "cpu负载大于cpu核数: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}的cpu负载: {{ $value }}"

  - alert: cpu负载大于2Cores-2
    ### expr: node_load1 > 2 * (count(node_cpu_seconds_total{mode="idle"}) without (cpu,mode)) - 2
    expr: node_load1 > (instance:node_cpus:count * 2) - 2
    for: 1m
    labels:
      severity: critical
      level: 3
      kind: CpuLoad
    annotations:
      summary: "cpu负载大于2Cores-2: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}的cpu负载: {{ $value }}"

  - alert: 主机上下文切换忙
    ### expr: (rate(node_context_switches_total[5m])) / (count without(cpu, mode) (node_cpu_seconds_total{mode="idle"})) > 2000
    expr: (rate(node_context_switches_total[5m]) / instance:node_cpus:count) > 2000
    for: 5m
    labels:
      severity: warning
      level: 2
    annotations:
      summary: "主机上下文切换忙: {{ $labels.hostname }}"
      description: "主机{{ $labels.hostname }}的上下文切换大于2000/s: {{ $value | humanize }}/s"

- name: node-memory
  rules:
  # 内存可用率
  - record: instance:node_memory_available:ratio
    expr: >
      (
        node_memory_MemAvailable_bytes or
        (
          node_memory_Buffers_bytes +
          node_memory_Cached_bytes +
          node_memory_MemFree_bytes +
          node_memory_Slab_bytes
        )
      ) /
      node_memory_MemTotal_bytes
  # 内存使用率
  - record: instance:node_memory_utilization:ratio
    expr: 1 - instance:node_memory_available:ratio

  - alert: 主机内存面临压力
    expr: rate(node_vmstat_pgmajfault[1m]) > 1000
    for: 5m
    labels:
      severity: warning
      level: 2
    annotations:
      summary: "主机内存面临压力: {{ $labels.instance }}"
      description: "节点内存面临压力。High rate of major page faults: {{ $value }}"

  - alert: 内存使用率大于85%
    ### expr: (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 85
    expr: instance:node_memory_utilization:ratio * 100 > 85
    for: 3m
    labels:
      severity: warning
      level: 2
      kind: MemoryUsage
    annotations:
      summary: "内存使用率超过85%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}的内存使用率: {{ $value | humanize }}%"

  - alert: 内存使用率大于90%
    ### expr: (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 90
    expr: instance:node_memory_utilization:ratio * 100 > 90
    for: 1m
    labels:
      severity: critical
      level: 3
      kind: MemoryUsage
    annotations:
      summary: "内存使用率超过90%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}的内存使用率: {{ $value | humanize }}%"

  - alert: 内存使用率一分钟内增长30%且大于70%
    expr: delta(instance:node_memory_utilization:ratio[2m]) * 100 > 30 and on(hostname) instance:node_memory_utilization:ratio * 100 > 70
    labels:
      severity: warning
      level: 2
      kind: MemoryUsageDelta
    annotations:
      summary: "内存使用率一分钟内增长30%且大于70%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }} 的内存使用率一分钟内增长30%且大于70%，增长率: {{ $value | humanize }}%"

  - alert: 内存使用率一分钟内增长40%且大于80%
    expr: delta(instance:node_memory_utilization:ratio[2m]) * 100 > 40 and on(hostname) instance:node_memory_utilization:ratio * 100 > 80
    labels:
      severity: critical
      level: 3
      kind: MemoryUsageDelta
    annotations:
      summary: "内存使用率一分钟内增长40%且大于80%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }} 的内存使用率一分钟内增长40%且大于80%，增长率: {{ $value | humanize }}%"

  - alert: 内存使用率一分钟内增长50%
    expr: delta(instance:node_memory_utilization:ratio[2m]) * 100 > 50
    labels:
      severity: critical
      level: 3
    annotations:
      summary: "内存使用率一分钟内增长50%: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }} 的内存使用率一分钟内增长50%，增长率: {{ $value | humanize }}%"

  - alert: 主机检测到oom kill
    expr: >
      increase(node_vmstat_oom_kill[1h]) > 2
      or
      increase(syslog_oom_kills_total[1h]) > 2
    labels:
      severity: warning
      level: 2
    annotations:
      summary: "主机检测到oom kill: {{ $labels.hostname }}"
      description: "{{ $labels.hostname }}检测到oom kill: {{ $value }}"


- name: node-filesystem
  rules:
  # 磁盘分区可用率
  - record: instance:node_filesystem_avail:ratio
    expr: node_filesystem_avail_bytes{device=~"(/dev/.+|tank/dataset)"} / node_filesystem_size_bytes{device=~"(/dev/.+|tank/dataset)"}
  # 磁盘分区使用率
  - record: instance:node_filesystem_utilization:ratio
    expr: 1 - instance:node_filesystem_avail:ratio
  # 分区inode可用率
  - record: instance:node_filesystem_files_avail:ratio
    expr: node_filesystem_files_free / node_filesystem_files
  # 分区inode使用率
  - record: instance:node_filesystem_files_utilization:ratio
    expr: 1 - instance:node_filesystem_files_avail:ratio

  - alert: 磁盘分区使用率大于85%
    ### expr: (1- (node_filesystem_avail_bytes{fstype=~"ext4|xfs"} / node_filesystem_size_bytes{fstype=~"ext4|xfs"})) * 100 > 85
    expr: instance:node_filesystem_utilization:ratio{fstype=~"(ext.|xfs|zfs)"} * 100 > 85
    for: 10m
    labels:
      severity: warning
      level: 2
      kind: "{{ $labels.mountpoint }}"
    annotations:
      summary: "磁盘分区{{ $labels.mountpoint}}使用率大于85%"
      description: "{{ $labels.hostname }}的磁盘分区{{ $labels.mountpoint }}使用率为: {{ $value | humanize }}%"

  - alert: 磁盘分区使用率大于90%
    ### expr: (1- (node_filesystem_avail_bytes{fstype=~"ext4|xfs"} / node_filesystem_size_bytes{fstype=~"ext4|xfs"})) * 100 > 90
    expr: instance:node_filesystem_utilization:ratio{fstype=~"(ext.|xfs|zfs)"} * 100 > 90
    for: 3m
    labels:
      severity: critical
      level: 3
      kind: "{{ $labels.mountpoint }}"
    annotations:
      summary: "磁盘分区{{ $labels.mountpoint}}使用率大于90%"
      description: "{{ $labels.hostname }}的磁盘分区{{ $labels.mountpoint }}使用率为: {{ $value | humanize }}%"

  - alert: 分区inode使用率大于70%
    ### expr: (1 - (node_filesystem_files_free{fstype=~"ext4|xfs"} / node_filesystem_files{fstype=~"ext4|xfs"})) * 100 > 70
    expr: instance:node_filesystem_files_utilization:ratio{fstype=~"(ext.|xfs)"} * 100 > 70
    for: 3m
    labels:
      severity: info
      level: 1
    annotations:
      summary: "磁盘分区{{ $labels.mountpoint }}的inode使用率大于70%"
      description: "{{ $labels.hostname }}的磁盘分区{{ $labels.mountpoint }}的inode使用率: '{{ $value | humanize }}%'"

  - alert: 分区inode使用率大于80%
    ### expr: (1 - (node_filesystem_files_free{fstype=~"ext4|xfs"} / node_filesystem_files{fstype=~"ext4|xfs"})) * 100 > 80
    expr: instance:node_filesystem_files_utilization:ratio{fstype=~"(ext.|xfs)"} * 100 > 80
    for: 3m
    labels:
      severity: warning
      level: 2
    annotations:
      summary: "磁盘分区{{ $labels.mountpoint }}的inode使用率大于80%"
      description: "{{ $labels.hostname }}的磁盘分区{{ $labels.mountpoint }}的inode使用率: '{{ $value | humanize }}%'"
```


<br>
<br>


## 进程探针配置

process-exporter探针配置示例:

```yml
# https://github.com/ncabatoff/process-exporter
# /proc/<pid>/xxx
process_names:
  # java
  - name: "{{.Matches}}"
    cmdline:
    - '.+/bin/java .+'
  - name: "{{.Matches}}"
    cmdline:
    - 'java .+'
  #匹配完整的运行命令
  - name: "{{.Matches}}"
  #- name: "{{.Comm}}"
    cmdline:
    - '.+'
```


<br>
<br>


## 进程告警规则

prometheus进程告警规则示例:

```yml
groups:
- name: 测试进程告警规则
  rules:
  - alert: nginx进程不存在
    expr: 'sum(namedprocess_namegroup_states{groupname=~"map\\[:nginx: master process .+"}) without(state) == 0'
    for: 1m
    labels:
      severity: critical
      level: 3
    annotations:
      summary: "nginx进程不存在, 实例: {{ $labels.instance }}"
      description: "主机: {{ $labels.hostname }}, 进程不存在: {{ $labels.groupname }}"
      wxurl: webhook1,webhook2
      mobile: phone1,phone2

  - alert: filebeat进程不存在
    expr: sum without(state) (namedprocess_namegroup_states{groupname=~"map\\[:/usr/share/filebeat/bin/filebeat -e -c .+", hostgroup="xxx"}) == 0
    for: 1m
    labels:
      severity: critical
      level: 3
    annotations:
      summary: "filebeat进程不存在, 实例: {{ $labels.instance }}"
      description: "主机: {{ $labels.hostname }}, 进程不存在: {{ $labels.groupname }}"
      wxurl: webhook1,webhook2
      mobile: phone1,phone2

```






<br>
<br>

---

<br>
<br>




















# 集成

INSTRUMENTING


<br>


## 客户端库

CLIENT LIBRARIES: <https://prometheus.io/docs/instrumenting/clientlibs/>

在监控服务之前，您需要通过 Prometheus 客户端库在其代码中添加集成。

选择与你编程语言相匹配的prometheus client library。你可以通过你的应用程序实例的HTTP endpoint来定义和暴露内部指标数据。

当prometheus采集你实例的HTTP端点时，客户端库会将所有指标的当前状态发送给prometheus server。



<br>
<br>



## 编写客户端库

WRITING CLIENT LIBRARIES: <https://prometheus.io/docs/instrumenting/writing_clientlibs/>

本章涵盖了Prometheus client libraries应该提供的功能和API。支持10种编程语言，因此编写客户端有很好的感觉。


<br>


### 惯例

Conventions

应该注意的事：

- 采取每门语言的优点
- 常见的使用情况应该很容易
- 做某事的正确方式应该是简单的方式
- 更复杂的用例应该是可能的

常见用例：

- Counters without labels spread liberally around libraries/applications
- Timing functions/blocks of code in Summaries/Histograms
- Gauges to track current states of things (and their limits)
- Monitoring of batch jobs


<br>
<br>


### 整体结构

Overall structure

客户端必须在内部进行回调以进行写操作。客户端一般应该遵循如下描述的结构。

关键类是收集器(collector)。这有个方法(一般称为collect)，此方法返回0和更多指标和样本。Collectors get registered with a CollectorRegistry. 数据通过传递一个CollectorRegistry到一个bridge类/方法/函数来公开，返回的是Promehteus支持的格式指标。每次CollectorRegistry抓取它必须回调每个收集器的收集方法。

大多数用户交互的接口是Counter, Gauge, Summary, Histogram收集器。这些代表一个单一指标，并应涵盖绝大多数使用情况，其中用户集成自己的代码。

更高级的用户案例（如从其它监控系统进行代理）需要编写一个自定义收集器。有人可能还希望编写一个bridge，以一个格式不同的监控系统来产生CollectorRegistry和生成数据，允许用户只考虑一个集成系统。

CollectorRegistry应该提供`register()`, `unregister()`函数，并且一个收集器应该被允许注册到多个CollectorRegistrys。

客户端必须是线程安全的。


<br>

**命名**

Namming

客户端库应遵循此文档中提及的函数，方法，类名称，记住它们的命名规范。例如，`set_to_current_time()`是一个好的Python命名规范，`SetToCurrentTime()`是一个好的Go命名规范，`setToCurrentTime()`是一个好的Java命名规范。


<br>
<br>


### 指标

Metrics

Counter, Gauge, Summary, Histogram指标类型是用户的主要接口。

Counter和Gauge必须是客户端库的一部分。Summary和Histogram至少有一个必须提供。

这些应主要用于为静态文件变量(file-static variables)，也就是说，全局变量定义在同一个文件，因为他们集成代码。客户端库应该启用它。常用情况是集成一段整体代码，而不是一个对象的一个实例的上下文中的一段代码。用户不应该在代码中担心探测他们的指标，客户端库应该代劳。

必须有一个默认的CollectorRegistry，默认的标准指标必须隐式注册到它与该用户不需要的特殊工作。必须有一个方式来让指标不注册到默认CollectorRegistry，在批处理作业和单元测试。自定义收集器应该遵顼这一点。

究竟编程语言应该如何创建指标。对于一些(Java, Go)构建器方法是最好的，对于其它(Python)函数参数是足够丰富的。

Java样例客户端:

```java
class YourClass {
  static final Counter requests = Counter.build()
      .name("requests_total")
      .help("Requests.").register();
}
```


<br>


**Counter**

Counter是一个单调递增的计数器。它不允许值减少，但可能被重置为0（如服务器重启）。

计数器必须有以下方法：

- `inc()`：计数器递增1
- `inc(douvel v)`：计数器由给定数增加（`v>=0`）

计数器必须从0开始。

计数器建议有：

- 计算给定一段代码异常抛出/升起的方法，可选地仅特定异常类型



<br>


**Gauge**

Gauge表示一个值可增可减。

测量必须有以下方法：

- `inc()`：测量递增1
- `inc(double v)`：测量由给定数增加
- `dec()`：测量递减1
- `dec(double v)`：测量由戈丁数减少
- `set(double v)`：测量被设置为给定值

测量必须从0开始，你也可以在启动时提供一个不同值。

测量应该有以下方法：

- `set_to_current_time()`：将测量设置为unixtime

测量建议有：

- 使用一个方法来追踪某条函数/代码正在进行的请求。在Python中是`track_inprogress`。

测试一段代码实践的一个方法和测试测量的持续时间。这对于批量作业很有用。


<br>


**Summary**

Summary样例观察滑动时间窗，兵器提供分布、频率、和的瞬时观察。

摘要绝不允许用户设置quantile作为标签名，因为这是内部使用的指定摘要总数(summary quantiles)。摘要鼓励提供quantiles作为exports，但这些都布恩那个进行聚合，并趋于缓慢。摘要必须允许每个quantiles，如`_count`, `_sum`是非常有用的，这必须是默认的。

摘要`_count`, `_sum`必须从0开始。

摘要必须有的方法：

- `observe(double v)`：观察给定的量

摘要应该有的方法：

- 以秒为用户的时间码。在Python中是`time()`。不能提供除了秒之外的其它单位。


<br>


**Histogram**

Histogram允许事件的可分布聚合，如请求的等待时间。

直方图绝不允许`le`作为用户设置的label，`le`用于内部指派桶。

直方图必须提供一个方式来手动选择桶。方式以`linear(start, width, count)`和`exponential(start, factor, count)`应该提供来设置桶。计数必须排除`+Inf`桶。

直方图应该有相同的默认桶作为其它客户端库。一旦创建了指标桶就不能改变。

直方图必须有的方法：

- `observe(double v)`

直方图应该有的方法：

- 以秒为用户的时间码。不能提供除了秒之外的其它单位。


<br>


**标签**

Labels

标签是Prometheus最强大的一个方面，但容易被滥用。因此客户端库必须在提供标签给用户时必须非常小心。

客户端库必须在任何情况下让用户为Gauge, Counter, Summary, Histogram或其它由库提供的任意收集器有不同的标签名。

自定义收集器指标应该总是具有一致的标签名。客户端不该对此进行验证。

虽然标签很强大，但多数指标没有标签。因此，API应该允许标签，而不是控制它。

一个客户端库必须允许在Gauge, Counter, Summary, Histogram创建时间指定任意标签名的列表。客户端库应该支持任意数量的标签名。客户端库必须验证标签名称符合文件要求。

提供访问指标的标记尺寸的一般方法是通过`labels()`方法，它可以以标签指列表或从标签名到标签执的映射，并返回一个child。通常是`.inc()`, `.dec()`, `.observe()`等等。方法可以在child上调用。

通过`labels()`返回的Child应该由用户缓存，以避免再次看到它。

有标签的指标应该支持有相同签名作为`labels()`的`remove()`方法，这将从指标中移除一个child并不再公开它，`clear()`方法将移除指标的所有孩子。

这应该有一个方法，来初始化一个带有默认值的孩子，通常只是调用`labels()`。不带标签的指标必须总是被初始化，以避免缺失指标问题。


<br>


**指标名称**

Metric names

指标名称必须遵循规范。正如指标名称，这必须满足Gauge, Counter, Summary, Histogram的用途，并与该库提供的任意收集器。

许多客户端库提供三个部分来设置名称：`namespace_subsystem_name`。

动态/生成 指标名称或指标名子部分必须阻止，当一个自定义收集器从其它监控系统代理时除外。动态的/生成的 指标名称是你要使用的标签而不是一个标志。


<br>


**指标描述和帮助**

Metric description and help

Gauge/Counter/Summary/Histogram 必须要求提供指标描述和帮助。客户端库提供的任意自定义收集器必须在指标上有描述和帮助。


<br>
<br>


### 公开

Exposition

客户端必须执行基于文本的公开格式，详细文档: <https://prometheus.io/docs/instrumenting/exposition_formats/>

公开的指标的可再现的顺序是鼓励的（尤其是以人类可读的格式），如果它可在没有显著资源成本增加来实现。


<br>
<br>


### 标准和运行时收集器

Standard and runtime collectors

客户端库应该在标准导出中提供些什么，下面将介绍。

这些应该被实现为自定义收集器，并在默认的CollectorRegistry默认注册。应该有方式来禁用这些。


<br>


**进程指标**

Process metrics

这有一些以`process_`为前缀的指标。如果获得必要的值是有问题的，或甚至根本无法使用语言，或运行时，客户端库应该留出相应的指标或特殊值(如`NaN`)。所有以字节的内存值，所有以unixtime/seconds的时间。

| 指标名 | 帮助字符串 | 单位 |
|  -  |  -  |  -  |
| `process_cpu_seconds_total` | Total user and system CPU time spent in seconds | seconds |
| `process_open_fds` | Number of open file descriptors | file descriptors |
| `process_max_fds` | Maximum number of open file descriptors | file descriptors |
| `process_virtual_memory_bytes` | Virtual memory size in bytes | bytes |
| `process_virtual_memory_max_bytes` | Maximum amount of virtual memory available in bytes | bytes |
| `process_resident_memory_bytes` | Resident memory size in bytes | bytes |
| `process_heap_bytes` | Process heap size in bytes | bytes |
| `process_start_time_seconds` | Start time of the process since unix epoch in seconds | seconds |


<br>


**运行时指标**

Runtime metrics

此外，客户端库鼓励提供有意义的指标当语言运行时，如`go_`, `hotspot_`等等前缀开头的相关指标。


<br>
<br>


### 单元测试

Unit tests

客户端库应该有覆盖核心集成库和公开的单元测试。

客户端库鼓励提供一个方式，使用户更容易进行单元测试。例如，在Python中有`CollectorRegistry.get_sample_value`。


<br>
<br>


### 打包和依赖

Packaging and dependencies

理想情况下，客户端库可以被包括在任意应用程序中添加一些集成而不会破坏程序。因此，添加客户端库的依赖时请小心。


<br>
<br>


### 性能考虑

Performance considerations

由于客户端库必须是线程安全的，需要某种形式的并发控制和考虑多核机器和应用程序的性能。

根据经验最少的高性能是互斥。处理器原子指令往往是在中间，并且一般是可接受的。

如上所述，`labels()`的结果应该是可缓存的。指标应该避免被阻塞当它们被递增或递减时。



<br>
<br>



## 推送指标

PUSHING METRICS: <https://prometheus.io/docs/instrumenting/pushing/>

偶尔，你需要监控不能被抓取的组件。Prometheus Pushgateway允许你推送指标。与Prometheus简单额基于文本格式相结合，这使得它很容易集成，甚至使用shell脚本而不需要客户端库。



<br>
<br>



## 探针和集成

EXPORTERS AND INTEGRATIONS: <https://prometheus.io/docs/instrumenting/exporters/>

有许多库和第三方系统可为Prometheus提供相应的指标。


<br>


### 第三方探针

Third-party exporters

有一些探针是由Prometheus官方维护的，其它的是由第三方贡献和维护。



<br>
<br>



## 编写探针

WRITING EXPORTERS: <https://prometheus.io/docs/instrumenting/writing_exporters/>

如果你在集成自己的代码，那么Prometheus client library如何集成自己的代码的通用规则应该被遵守。当从另一个监控或集成系统采取指标时，事情往往并不是非黑即白。

当编写一个探针或自定义收集器时，此文档包含的事情你应该考虑。


<br>
<br>


### 可维护性和纯度

Maintainability and purity

当你编写一个探针时，你需要做出的一个决定是有多少工作你愿意放在获得完美的指标上。

如果有问题的系统只有很少改变的指标的一小撮，然后让一切完美是一个容易的选择，一个好的样例是[HAProxy exporter](https://github.com/prometheus/haproxy_exporter)

在另一方面，当系统上有上百个指标是新版本经常变动的，如果你想试图把事情做完美，那么你已经给自己制造了许多正在进行的工作。[MysQL exporter](https://github.com/prometheus/mysqld_exporter)在这系列(spectrum)的末端。

[node exporter](https://github.com/prometheus/node_exporter)是这些的混合。例如，`mdadm`收集器手动解析一个文件并公开为此收集器特定创建的指标。对于`meminfo`收集器，结果跨越不同内核版本，因此我们最终做足够的转换来创建有效的指标。


<br>
<br>


### 配置

Configuration

当程序工作时，你的目标应该是探针不需要用户自定配置就可以运行。你可能还希望提供过滤某些指标的能力（用户定义只需要收集的指标，不需要收集所有指标），降低系统的开销。例如node exporter可让用户定义收集哪些指标，虽然我是全部收集。

当与其它监控系统一起工作时，框架和协议，你将提供额外的配置或自定义来生成适合Prometheus的指标。在最好的情况下，一个监控系统有类似的足够的Prometheus数据模型，你可以自动确定如何转换指标。如Cloudwatch, SNMP, collectd的情况。至多，我们需要让用户选择获取的那些指标。

也就是探针配置简单化，探针指标用户可选择。

在其它情况下，系统指标是不是十分标准的，这却决于系统和底层应用的使用情况。在这种情况下，用户必须告诉我们如何转换转换指标。[JMX exporter](https://github.com/prometheus/jmx_exporter)是最明显的案例，与[Graphite exporter](https://github.com/prometheus/graphite_exporter)和[StatsD exporter](https://github.com/prometheus/statsd_exporter)也需要配置来提取标签。

确保探针出口开箱没有配置，以及提供需要转换的示例配置的选择，这是一个建议。

YAML是标准的Prometheus配置格式，所有配置默认都应该适应YAML。


<br>
<br>


### 指标

Metrics


<br>


#### 命名

Naming

遵循指标命名的最佳实践。

指标名不应该由程序产生，除非在编写自定义收集器或探针时。

指标必须使用基础单位并让它们转换为更可读的图形工具。无论你使用了什么单位，指标名中的单位必须匹配在使用的单位。类似地，公开比率(ratios)，而不是百分比(percentages)。

指标名不应该包含导出的标签，如果标签被聚集将没有意义。Prometheus指标和标签名以`snake_case`编写。公开的指标不应包含冒号，这些被保留用于用户定义记录规则(recording rules)，当使用聚合时。

只有`[a-zA-Z0-9:_]`是有效的指标名。

`_sum`, `_count`, `_bucket`, `_total`后缀用于Summary, Histogram, Counter。`_total`是Counter的惯例，如果你使用COUNTER类型你应该使用它。

`process_`, `scrape_`前缀是保留的。

当你有一个successful request count和一个failed request count，最佳的方式是一个指标用于成功的请求，另一个指标用于失败的请求。这很容易计算出失败率。不要使用带success或failed标签的一个指标。同样，此规则也同样适用于其它指标。

一个带有原始名的`HELP`字符串可以提供大部分相同的好处。


<br>
<br>


#### 标签

Labels

避免将type作为标签名称，它过于笼统往往无意义。你应该尝试尽可能地避免发生冲突，如`region`, `cluster`等等。但是，如果这就是应用程序调用的一些资源，最好不要通过重命名来引起混乱。

因此要避免把东西放入一个指标，只是因为它们共享一个前缀。除非你确定一个指标是有意义的，多个指标是安全的。

`le`标签对于Histogram有特殊意义，`quantile`标签对于Summary有特殊意义。通常避免使用这些标签。write/read， send/receive最好划分为单独的指标，而不是一个指标。

经验法则是，当求和或求平均值时，一个指标应该是有意义的。


<br>
<br>


#### 类型

Type

你应该尝试匹配你的指标类型到Prometheus类型。这通常是counters和gauges。通常它不会很明显指标是什么类型，特别是如果你自动处理一组指标。一般`UNTYPED`是一个安全的默认值。


<br>
<br>


#### 帮助字符串

Help strings

当你转换指标时，它对用于能够追踪原来是什么样的有帮助，以及造成这种转换有什么规则。将收集器或探针名称、应用的任意规则的ID和名称和原始指标的详情写入帮助字符串将极大地帮助用户。

Prometheus不喜欢一个字段有不同的帮助字符串。

示例:

```
# HELP node_cooling_device_max_state Maximum throttle state of the cooling device
# TYPE node_cooling_device_max_state gauge
node_cooling_device_max_state{name="0",type="Processor"} 7
node_cooling_device_max_state{name="1",type="Processor"} 7
node_cooling_device_max_state{name="2",type="Processor"} 7
```


<br>
<br>


#### 丢弃更少的有用信息

Drop less useful statistics

有些集成系统公开了1m, 5m, 15m率、平均率，在应用程序启动的时候。

这些都应该被丢弃，因为它们不是很有用，并添加了混乱。Prometheus可以自己计算比率，并且通常作为公开的平均值呈指数衰减。


<br>
<br>


#### 点字符串

Dotted strings

许多监控系统没有标签，而是使用点: `my.class.path.mymetric.labelvalue1.labelvalue2.labelvalue3`


<br>
<br>


### 收集器

Collectors

当为探针实现收集器时，你永远不应该使用通常的直接集成方法，并在每个抓取上更新指标。

每次创建新的指标。在Go的`Collect()`方法中使用`MustNewConstMetric`。Python请参考: <https://github.com/prometheus/client_python#custom-collectors>

原因有两方面。首先，两个抓取可能发生在同一时间，和直接集成使用的什么是文件级别的全局变量。其次，如果一个标签执消失，它仍然将公开。


<br>


#### 关于抓取自身的指标

Metrics about the scrape itself

有时你想导出关于抓取的指标，如处理了多少记录等。这应该被公开为gauges当它们关于事件时，通过探针名的指标名前缀，如`jmx_scrape_duration_seconds`。


<br>
<br>


#### 机器和进程指标

Machine and process metrics

许多系统，如ES，会公开机器指标（如cpu, memory, filesystem）等信息。Prometheus生态中的node exporter提供了这些信息，这些指标就应该被丢弃。

在Java世界里，许多集成框架公开了程序级别和JVM级别的统计信息，如CPU, GC等。Java客户端和JMX探针已包含这些，所以也应该丢弃。这同样也可用于其它类似的语言和框架。


<br>
<br>


### 部署

Deployment

每个探针应该监控只有一个实例的应用程序，preferably sitting right beside it on the same machine。这意味着你运行没给一个HAProxy，你运行了一个`haproxy_exporter`进程。


<br>


#### 调度

Scheduling

当Prometheus抓取指标时，指标应该从应用处拉取，探针不应该基于自己的定时器执行抓取。也就是说，所有的抓取都应该是同步的(synchronous)。

因此，你不应该在公开的指标上设置时间戳，而让Prometheus来做。如果你认为需要时间戳，那么你可能需要使用pushgateway来代替。

如果检索指标特别昂贵，即超过了一分钟，可以接受的是缓存它(cache it)。这应该在注释在HELP字符串中。

Prometheus默认抓取的超时时间是10s。如果你的探针希望超过这一点，你应该在你的用户文档中明确的调用此。


<br>
<br>


#### 推送

Pushes

有些应用程序和监控系统只能推送指标(push metrics)，如StatsD, Graphite, collectd。有两方面的考虑：

首先，指标什么时候过期？
其次，这些类型的系统倾向于允许你的用户发送变化量(deltas)或原始计数器(raw counter)。你应该尽可能依赖原生计数器，因为这一般是Prometheus model。

对于服务级别的指标，你应该有探针推送到Pushgateway，在事件而不是你自己处理状态之后退出。对于实例级别的指标，还没有明确模式。


<br>
<br>


#### 抓取失败

Failed scrapes

目前有两种模式的抓取失败，当应用程序不响应或其它问题时。

第一个就是返回`5xx`错误。

第二个是有一个`myexporter_up`，如`haproxy_up`，值是0还是1依赖于抓取工作。

后者更好，即使抓取失败，你还可以得到一些有用的指标。


<br>
<br>


#### 抓取页面

Landing page

访问地址，如`http://expoter:port`作为一个简单的包含探针名称的HTML页面，并有指向`/metrics`页面的链接。


<br>
<br>


#### 端口号

Port numbers

用户在一个机器上可能有多个探针和Prometheus组件，为了使得事情更简单每个需要有一个唯一的端口号。这个页面<https://github.com/prometheus/prometheus/wiki/Default-port-allocations>包含了Prometheus组件和探针目前使用的端口号。

为自己的探针分配一个合理的，没有使用的端口号。


<br>
<br>


### 宣布

Announcing

一旦你准备向世界宣布你的探针，请在[可用的探针列表](https://github.com/prometheus/docs/blob/master/content/docs/instrumenting/exporters.md)提交一个PR。










<br>
<br>

---

<br>
<br>












# 最佳实践

BEST PRACTICES


<br>



## 指标和标签命名

METRIC AND LABEL NAMING: <https://prometheus.io/docs/practices/naming/>

指标和标签的约定不需要使用Prometheus，但可以作为一个风格指南和最佳实践的集合。


<br>


### 指标名称

Metric names

一个指标名称：

- 必须符合有效字符的数据模型
- 应该有一个应用程序(single-word)相关的指标所属前缀。如：
  - `prometheus_notifications_total`
  - `process_cpu_seconds_total`
  - `http_request_duration_seconds`
- 必须有一个单一的单位（如秒，毫秒等）
- 应该使用基本单位（如seconds, bytes, meters，而不是milliseconds, megabytes, kilometers）
- 应该有一个描述单位的后缀。如：
  - `http_request_duration_seconds`
  - `node_memory_usage_bytes`
  - `http_requests_total`
  - `process_cpu_seconds_total`
  - `foobar_build_info`
- should represent the same logical thing-being-measured across all label dimensions
  - request duration
  - bytes of data transfer
  - instantaneous resource usage as a percentage

作为一个经验，在给定指标的所有尺寸上，无论是`sum()`还是`avg()`都应该是有意义的。如果没有意义，则将数据分裂成多个指标。


<br>
<br>


### 标签

Labels

使用标签来区分被测量的事物的特点：

- `api_http_requests_total`，不同的请求类型：`operation="create|update|delete"`
- `api_request_duration_seconds`，不同的请求阶段：`stage="extract|transform|load"`

不要将标签名对应到指标名，如果相应的标签聚合了，这会冗余并造成混乱。

> 注意：
> 请记住，标签键值对每一个独特的组合代表了一个新的时间序列，这可能会极大的提高存储的数据量。不要使用标签来存储高基数，如用户ID，邮件地址等。


<br>
<br>


### 基本单位

Base units

Prometheus没有硬编码任何单位。为了更好的兼容性，应该使用基本单位。以下是一些栗子：

| Family | Base unit | Remark |
|  -  |  -  |  -  |
| Time | seconds |  -  |
| Temperature | celsius | 摄氏度优于开尔文 |
| Length | meters | - |
| Bytes | bytes |  -  |
| Bits | bytes | 为了避免混淆，通常使用bytes |
| Percent | ratio | 0-1而不是0-100 |
| Voltage | volts |  -  |
| Electric current | amperes |  -  |
| Energy | joules | - |
| Mass | grams | 克优于千克 |



<br>
<br>



<br>
<br>



## 控制台和仪表盘

CONSOLES AND DASHBOARDS: <https://prometheus.io/docs/practices/consoles/>

可在仪表盘上显示尽可能多的数据，特别是像Prometheus这样的系统提供了集成你的应用程序的能力。

我们发现以下原则非常有效：

- 在一个控制台上不要超过五个图
- 每个图上不要超过五个线
- 当使用控制台提供的模板示例，请避免在右边表超过20-30项



<br>
<br>



## 集成

INSTRUMENTATION: <https://prometheus.io/docs/practices/instrumentation/>

本章提供了集成你的代码的一套指导方案。


<br>


### 如何集成

How to instrument

简短的回答是集成一切。每个库、子系统和服务应该至少有几个指标来给你一个粗略的想法它是如何执行的。


<br>


#### 服务的三种类型

The three types of services

为了监控的目的，服务大致可以分为三类：在线服务(online-serving)，离线处理(offline-processing)，批处理作业(batch jobs)。它们之间有重叠，但每个服务往往很好地成为这些类别。

<br>

**在线服务系统**

Online-serving systems

一个在线服务系统期待立即响应。例如，大多数数据库和HTTP请求都属于这一类。这种系统的关键指标是执行的查询数，错误和延迟。

<br>

**离线处理**

Offline processing

对于离线处理，没有人正在等待响应。也可能有多个处理阶段。对于每一个阶段，跟踪进入的项目，有多少在处理，最后一次处理的事物，以及发送了多少事物。

<br>

**批处理作业**

Batch jobs

在离线处理和批处理作业之间有一些模糊线，离线处理也可在批处理作业中完成。批处理作业不连续运行，这使得它们难以区分。

批处理作业的关键指标是它最后一次成功。这对于追踪各个阶段的花费时间，整体运行和最后一次作业完成很有用。对于那些超过几分钟时间运行的批处理作业，使用基于Pull监控的数据抓取很有用。对于运行很频繁地批处理作业，你应该考虑将其转换为守护程序(daemons)，作为离线处理作业来处理它们。


<br>
<br>


#### 子系统

Subsystems

除了三种主要类型的服务，系统也应该监控子部分(sub-parts)。


<br>


**库**

Libraries

库应该提供无需用户额外配置的集成。

如果一个库经常访问进程外的资源（如网络、磁盘、IPC...），追踪整个查询计数，错误和延迟。

<br>

**日志**

Logging

作为一般规则，对与日志代码的每一行，你应该有一个递增的计数器(counter)。如果你发现一个有趣的日志信息，你要能过够看到它如何发生，而且持续多久。

<br>

**失败**

Failures

故障应该与日志有相似的处理。每次出现故障，计数器(counter)都应增加。不像日志，取决于你的代码结构，错误也可以丢到一个更普遍的错误计数器。当报告故障，你通常应该有一些其它指标来表示尝试的总数。这使得故障率很容易计算。

<br>

**线程池**

Threadpools

对于任何形式的线程池，关键指标是排队请求数，在使用的线程数，总线程数，处理的任务数，以及它们耗时多久。

<br>

**缓存**

Caches

缓存的关键指标是总查询、点击、总延时、查询数、错误和任何在线服务缓存前的延迟。

**收集器**

Collectors

当实现一个自定义指标收集器时，建议为收集花费的时间(s)导出为gauge，另一个是遇到的错误的数量。


<br>
<br>


### 要提防的事

Things to watch out for

当做监控时，有些事需要注意，Prometheus-specific尤其如此。


<br>


#### 使用标签

Use labels

很少有监控系统有标签和表达式语言的来利用这些优点，所以需要一些时间来使用。当你有多个指标要添加/求平均值/求和，它们通常是带有标签的指标，而不是多个指标。

例如，创建一个带有`code`标签的`http_responses_total`指标，而不是`http_responses_500_total`，`http_responses_403_total`两个指标。


<br>
<br>


#### 不要滥用标签

Do not overuse labels

每个标签集(labelset)是具有RAM, CPU, disk, network开销的额外时间序列。通常情况下，开销可以忽略不计，但有很多机器和很多指标和很多标签的情况下，这可能会迅速增加。

作为一般原则，尽量保持指标的基础低于10。绝大多数的指标没有任何标签。如果你有一个有100多个基数的指标，调查替代解决方案。

为了让你对基数有一个更好的主意，让我们来看下`node_exporter`。节点探针公开每个挂载的文件系统的指标。如果你有10000台机器，有10000个`node_filesystem_avail`时间序列，这对于Prometheus处理很好。

如果你不确定，就不用标签，并随着时间的推移添加更多标签。


<br>
<br>


#### 四种类型比较

Counter vs. gauge, summary vs. histogram

对一个给定的指标用哪个四种主要的指标类型是很重要的。

在Counter和Gauge之间选择，有一个简单的经验法则：如果值可减少，这是一个Gauge。Counter只能增加，如累积的量。Gauge可以设置，增加或减少。

Summaries和Histograms是更复杂的指标类型。


<br>
<br>


#### 时间戳

Timestamps, not time since

如果你想追踪从某事发生时的时间量，导出在它发生时的Unix时间戳，而不是从它发生的时间。

导出了时间戳，可以使用`time() - my_timestamp_metric`表达式计算时间。


<br>
<br>


#### 避免缺失指标

Avoid missing metrics

时间序列，直到某事发生不存在难以应付，因为通常的简单操作不足以正确处理它们。为了避免这种情况，你事先知道可能存在的任何时间序列输出为0(或NaN)。

大多数Prometheus Client Libraries(go, java, python)会为没有标签的指标自动导出0。



<br>
<br>
<br>



## Hisgogram和Summary

HISTOGRAMS AND SUMMARIES: <https://prometheus.io/docs/practices/histograms/>











<br>
<br>
<br>



## 告警

ALERTING: <https://prometheus.io/docs/practices/alerting/>

请先阅读[My Philosophy on Alerting](https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit)


<br>


### 告警什么

What to alert on

目标是告警越少越好，告警要有目的，别瞎告。告警应该很容易找到哪些部件的故障。



<br>
<br>
<br>



## 记录规则

RECORDING RULES: <https://prometheus.io/docs/practices/rules/>

记录规则的一致性命名方案，可以一目了然地理解规则的含义。本节可以正确聚合和建议命名规范。


<br>


### 命名和聚合

Naming and aggregation

记录规则(recording rules)应该是这样的一般形式：`level:metric:operations`。level表示聚合级别和规则输出的标签；metric是指标名称并在使用`rate()`这些方法时应该保持不变；operations是一个应用到指标的操作列表。

保持指标名称不变，使得易于知道指标是什么和容易在代码库中找到它。

如一些常见的: `sum`, `ratio`, `count`这些。


<br>
<br>


### 示例

```yaml
- record: instance_path:requests:rate5m
  expr: rate(requests_total{job="myjob"}[5m])

- record: path:requests:rate5m
  expr: sum without (instance)(instance_path:requests:rate5m{job="myjob"})

- record: instance_path:request_failures:rate5m
  expr: rate(request_failures_total{job="myjob"}[5m])

- record: instance_path:request_failures_per_requests:ratio_rate5m
  expr: |2
      instance_path:request_failures:rate5m{job="myjob"}
    /
      instance_path:requests:rate5m{job="myjob"}

- record: job:request_latency_seconds_count:avg_rate5m
  expr: avg without (instance, path)(instance:request_latency_seconds_count:rate5m{job="myjob"})
```



<br>
<br>
<br>



## Pushgateway

WHEN TO USE THE PUSHGATEWAY: <https://prometheus.io/docs/practices/pushing/>

Pushgateway是一个中间服务，允许你从无法抓取的作业处推送指标。更多详情，查看[push metrics](https://prometheus.io/docs/instrumenting/pushing/)


<br>


### 应该使用Pushgateway吗

Should I be using the Pushgateway?

我们只建议在某些有限的示例中使用Pushgateway。盲目地使用Pushgateway push来代替Prometheus pull有几个陷阱：

- 通过Pushgateway监控多个实例，Pushgateway成为单一的失败点和潜在的瓶颈。
- 你失去了Prometheus通过`up`自动检测实例健康。
- Pushgateway永远不会忘记推送它，并公开它们给Prometheus，除非这些序列通过PushgatewayApi手动删除。

如果原始实例重命名或删除，实例的指标将保留在Pushgateway中。这是因为PushGateway作为指标缓存的生命周期与推送指标的进程的生命周期分开。与Prometheus pull方式对比时，当一个实例消失时，实例的指标将自动消失。使用Pushgateway时，不不是这种情况，你必须手动删除任何陈旧的指标，或自动执行生命周期同步。

通常，Pushgateway的唯一有效用例是用于捕获一个服务级别的批处理作业的结果(outcome)。一个服务级别的批处理作业是与特定机器或作业实例相关的。这样的作业的指标不应包含一个机器或实例标签，以将特定机器的生命周期与推送指标的实例分开。这降低了管理Pushgateway中的陈旧指标的负担。


<br>
<br>


### 替代策略

Alternative strategies

如果入站防火墙和NAT阻止您从目标机器抓取指标，考虑将Prometheus Server移动到网络后面。我们通常建议在同一网络上运行Prometheus Server来作为监控实例。否则，考虑[PushProx](https://github.com/prometheus-community/PushProx)，它允许Prometheus跨过防火墙或NAT。



<br>
<br>
<br>



## 远程存储

REMOTE WRITE TUNING: <https://prometheus.io/docs/practices/remote_write/>

Prometheus实现了合理的默认远程写，但许多用户有不同的要求，并希望优化远程设置。




























