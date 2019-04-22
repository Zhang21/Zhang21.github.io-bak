---
title: Fluentd
date: 2018-07-19 16:38:33
tags:
  - Fluentd
  - 日志处理
categories: DevOps
---


参考：

- Fluentd文档: <https://docs.fluentd.org/v1.0/articles/quickstart>

<br>

环境：

- CentOS7x86_64
- Fluentd v1.0



<br/>
<br/>

<!--more-->

---

<br/>




# 综述

![Fluentd](/images/Fluentd/fluentd.png)

<br/>

## 入门

getting started


Fluentd是一个完全免费且开源的日志收集器，支持多种事件类型。
Fluentd将日志视为JSON(一种机器可读格式)，它主要用C语言编写，扩展部分使用Ruby。


<br/>
<br/>


## 安装

由于我使用CentOS7，所以查看了此平台文档。


<br/>


### 安装前

在安装Fluentd前，请配置环境，避免一些问题。

- 设置ntp
- 增加`max file descriptors`
- 优化网络内核参数

```
#ntp
/sbin/ntpdate 1.cn.pool.ntp.org


#文件描述符
ulimit -n
#1024
#LimitNOFILE=65536
vim /etc/security/limits.conf
root    soft    nofile  65536
root    hard    nofile  65536
*       soft    nofile  65536
*       hard    nofile  65536
#之后重启机器


#优化网络内核参数
vim /etc/sysctl.conf
net.core.somaxconn = 1024
net.core.netdev_max_backlog = 5000
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_wmem = 4096 12582912 16777216
net.ipv4.tcp_rmem = 4096 12582912 16777216
net.ipv4.tcp_max_syn_backlog = 8096
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535

sysctl -p
```


<br/>


### rpm安装

```sh
#此脚本会自动安装td.repo，并安装td-agent
#non-root
curl -L https://toolbelt.treasuredata.com/sh/install-redhat-td-agent3.sh | sh

#你也可以将脚本内容复制下来执行
```

<br/>


### 启动daemon

自动支持systemd:

```
ls /usr/lib/systemd/system/td-agent.service

#默认配置文件
ls /etc/td-agent/td-agent.conf


systemctl start|stop|status td-agent

```


<br/>


### 通过HTTP发送示例日志

我们可通过POST发送日志栗子。

```sh
curl -X POST -d 'json={"json": "message"}' http://localhost:8888/debug.txt

```


<br/>



### 安装后

- 系统管理
	+ 配置文件
	+ 日志
- 连接到其它服务
	+ 如何工作
	+ 插件管理
- 配置语法
	+ 数据源
	+ 输出点

<br>

**系统管理**

```
#配置文件
ls /etc/td-agent/td-agent.conf


#日志
ls /var/log/td-agent/td-agent.log
```

<br>

**连接到其它服务**
在Fluentd中，数据 input/output 最重要的部分由插件来管理。每个插件都知道如何与外部端点连接，并负责管理传输数据流的通道。
插件以某种约定命名。如`in_kafka`, `out_mongo`。

配置栗子：
`in_forward`插件作为输入源，`out_file`插件作为输出点。

```
<source>
  @type forward
  port 9999
</source>
<match app.**>
  @type file
  path /var/log/app/data.log
  compress gzip
</match>
```

插件管理：
Fluentd将插件作为Ruby gems来管理。所以你需要使用`td-agent-gem`来管理Fluentd插件。

```sh
#安装s3插件
#查看插件: <https://www.fluentd.org/plugins>
sudo /usr/sbin/td-agent-gem install fluent-plugin-s3
```

<br>

**配置语法**
配置文件由许多块组成，每个块包含一组特定数据端点设置。

数据源:

```
<source>
  @type syslog
  port 5140
  tag system
</source>

#@type确定要使用的插件，你就不需要再去加上前缀in
```

输出点：
添加一个数据流输出端点，你需要定义一个`<match>`块。
你可在过滤器汇表达式中使用通配符`*`来匹配多个。

```
<match debug.log>
  @type syslog
  prot 5140
  tag system
</source>
```



<br/>
<br/>



## Fluentd事件的生命

Life of a Fluentd event


<br/>


### 基础设置

使用`in_http`和`out_stdout`插件作为示例，来描述事件周期。

```
<source>
  @type http
  port 8888
  bind 0.0.0.0
</source>

<match test.cycle>
  @type stdout
</match>


curl -i -X POST -d 'json={'action': 'login', 'user': 2}' http://localhost:8888/test.cycle
HTTP/1.1 200 OK
Content-Type: text/plain
Connection: Keep-Alive
Content-Length: 0
```


<br/>


### 事件结构

Event structure


Fluentd事件结构：

- tag: 事件来自何处
- time: 事件发生事件(原子时间)
- record: log内容(json)


<br/>
<br/>


### 事件处理

Processing Events

- Filters
- Labels
- Buffers

<br>

定义好配置时，路由引擎对输入数据应用配置规则。

**Filters**
过滤的目的在于传递(pass)或拒绝(reject)事件。

```
#栗子过滤 排除任何logout操作
<source>
  @type http
  port 8888
  bind 0.0.0.0
</source>

<filter test.cycle>
  @type grep
  <exclude>
    key action
    pattern ^logout$
  </exclude>
</filter>

<match test.cycle>
  @type stdout
</match>


curl -i -X POST -d 'json={"action":"login","user":2}' http://localhost:8888/test.cycle
curl -i -X POST -d 'json={"action":"logout","user":2}' http://localhost:8888/test.cycle

#查看fluentd日志的过滤情况
vim /var/log/td-agent/td-agent.log
```

Fluentd允许继承许多过滤器，同时考虑到配置文件的增长会让读者觉得有点复杂。所以添加了一个Label功能，用于解决这个问题。

<br>


**Labels**
Label这个功能，用于解决配置文件的复杂性，并允许定义不遵循从上到下的新路由部分，而是像链接引用一样。

栗子:

```
<source>
  @type http
  bind 0.0.0.0
  port 8888
  @label @STAGING
</source>

<filter test.cycle>
  @type grep
  <exclude>
    key action
    pattern ^login$
  </exclude>
</filter>

<label @STAGING>
  <filter test.cycle>
    @type grep
    <exclude>
      key action
      pattern ^logout$
    </exclude>
  </filter>

  <match test.cycle>
    @type stdout
  </match>
</label>
```

<br>

**Buffers**
前面的栗子中，我们使用`non-buffered stdout`。但是在生产环境中，会使用`outputs in buffered`。
缓冲模式下的输出插件首先将接收到的事件存储到缓冲区，并通过满足刷新条件将缓冲区写入目标。

缓冲区对可靠性和吞吐量都很重要。

<br>

**结论**
一旦事件由源上的Fluentd引擎所上报，就可逐步处理或引用Label内部处理，任何事件都有可能被过滤。新的路由引擎旨在提供更多灵活性，并在输出前使处理更容易。



<br/>
<br/>

---

<br/>



# 使用案例

Use cases


## 集中式应用程序日志

Centralized App Logging


<br/>


### Java


<br/>


### Ruby


<br/>


### Python


<br/>


### PHP


<br/>


### Perl


<br/>


### Node.js


<br/>


### Scala



<br/>
<br/>



## 监控服务日志

Monitoring Service Logs


### FEK

Splunk是一个检索日志的好工具，但它的高成本对很对团队来说便不可取了。我们通过结合三个开源项目: Elasticsearch， Kiban， Fluentd来免费替代Splunk。

![FEK](/images/Fluentd/fluentd-elasticsearch-kibana.png)

<br>


**请自行安装Elasticsearch和Kibana。**

安装Fluentd的Elasticsearch插件:
fluent-plugin-elasticsearch: <https://github.com/uken/fluent-plugin-elasticsearch>


```
td-agent-gem install fluent-plugin-elasticsearch --no-document

td-agent-gem list
```

<br>

修改配置文件: `/etc/td-agent/td-agent.conf`

```
# get logs from syslog
<source>
  @type syslog
  port 42185
  tag syslog
</source>

# get logs from fluent-logger, fluent-cat or other fluentd instances
<source>
  @type forward
</source>

<match syslog.**>
  @type elasticsearch
  logstash_format true
  <buffer>
    flush_interval 10s # for testing
  </buffer>
</match>
#fluent-plugin-elasticsearch插件附带一个logstash_format选项
```

Elasticsearch用户和密码:

```
<match my.logs>
  @type elasticsearch
  host localhost
  port 9200
  user elastic
  password xxxxx
  index_name fluentd
  type_name fluentd
</match>

#hosts host1:port1,host2:port2...
```

具体详情请参考GitHub的README。

<br>

**配置rsyslogd:**
将日志从rsyslogd转发到Fluentd。修改`/etc/syslog.conf`并重启rsyslogd。这将本地syslog转发到Fluentd，而Fluentd会将日志转发给Elasticsearch。

```
*.* @127.0.0.1:42185


systemcrl restart rsyslog
```

<br>

存储和查询事件日志：
访问Kibana配置索引`logstash-*`进行查看日志。

使用`logger`命令手动发送日志到Elasticsearch。
```
#logger - a shell command interface to the syslog(3) system log module
logger -t test foobar

#之后可在kibana索引中查看到ident为test, message为foobar的两个文档
```


<br/>
<br/>


### 邮件报警

Splunk-like Grep-and-Alert-Email System Using Fluentd


Splunk的一个主要功能便是能够满足日志在某些条件时发送报警电子邮件。
我们将使用Fluentd构建一个类似的系统。例如，当检测到Apache日志中的`5xx` HTTP status code时编发送报警邮件。

<br>

**安装依赖**
安装两个插件

```sh
td-agent-gem install fluent-plugin-grepcounter
td-agent-gem install fluent-plugin-mail
```

<br>

**配置**

```
/etc/td-agent/td-agent.conf


<source>
  @type tail
  path /var/log/apache2/access.log
  <parse>
    @type apache2
  </parse>
  tag apache.access
</source>

<match apache.access>
  @type grepcounter
  count_interval 3  # The time window for counting errors (in secs)
  input_key code    # The field to apply the regular expression
  regexp ^5\d\d$    # The regular expression to be applied
  threshold 1       # The minimum number of erros to trigger an alert
  add_tag_prefix error_5xx  # Generate tags like "error_5xx.apache.access"
</match>

<match error_5xx.apache.access>
  @type copy
  <store>
    @type stdout  # Print to stdout for debugging
  </store>
  <store>
    @type mail
    host smtp.gmail.com        # Change this to your SMTP server host
    port 587                   # Normally 25/587/465 are used for submission
    user USERNAME              # Use your username to log in
    password PASSWORD          # Use your login password
    enable_starttls_auto true  # Use this option to enable STARTTLS
    from example@gmail.com     # Set the sender address
    to alert@example.com       # Set the recipient address
    subject 'HTTP SERVER ERROR'
    message Total 5xx error count: %s\n\nPlease check your Apache webserver ASAP
    message_out_keys count     # Use the "count" field to replace "%s" above
  </store>
</match>
```

请确认：

- smtp配置正确
- `td-agent daemon`具有适当权限访问log

<br>

**测试配置**

```sh
systemctl restart td-agent


#如果安装了standalone的Fluentd
fluentd -c alert-email.conf
```



<br/>
<br/>



## 数据分析

Data Analytics


<br/>


### 将数据收集到Hadoop

Fluentd + HDFS: Instant Big Data Collection


**背景**
Fluentd专门用于解决大数据日志收集问题。

HDFS(Hadoop)是一个存储和处理大量数据的选择，但直到最近它才拥有了除Java库之外的可访问的API。
本节将展示如何使用Fluentd从HTTP接收数据流并传输到HDFS。

<br>

**架构**

![HDFS](/images/Fluentd/http-to-hdfs.png)

<br>

**安装**
本节配置一个单节点用于示例。请在同一节点安装如下软件：

- Fluentd
- WebHDFS Output Plug
- HDFS

```sh
#安装插件
sudo td-agent-gem install fluent-plugin-webhdfs
td-agent-gem list


#安装Hadoop
#http://hadoop.apache.org/releases.html
#在官网上下载对应二进制
```

<br>

**Fluentd配置**

http input:

```
<source>
  @type http
  port 8888
</source>
```

webHDFS output:

```
<match hdfs.*.*>
  @type webhdfs
  host namenode.your.cluster.local
  port 50070
  path "/log/%Y%m%d_%H/access.log.#{Socket.gethostname}"
  <buffer>
    flush_interval 10s
  </buffer>
</match>
```

<br>

**HDFS 配置**

`hdfs-site.xml`:

```
<property>
  <name>dfs.webhdfs.enabled</name>
  <value>true</value>
</property>

<property>
  <name>dfs.support.append</name>
  <value>true</value>
</property>

<property>
  <name>dfs.support.broken.append</name>
  <value>true</value>
</property>
```

<br>

**测试**

```
curl -X POST -d 'json={"action":"login","user":2}' \
  http://localhost:8888/hdfs.access.test
kill -USR1 `cat /var/run/td-agent/td-agent.pid`

sudo -u hdfs hadoop fs -lsr /log/
```


<br/>
<br/>


## 连接到数据存储

Connecting to Data Storages


<br/>


### 将Apache logs存储到MongoDB

本节使用Fluentd MongoDB Output plugin实时聚合半结构化日志。

<br>

**安装插件**

```
sudo td-agent-gem install fluent-plugin-mongo
td-agent list
```

<br>

**配置**

tail input:

```
<source>
  @type tail
  path /var/log/apache2/access_log
  pos_file /var/log/td-agent/apache2.access_log.pos
  <parse>
    @type apache2
  </parse>
  tag mongo.apache.access
</source>
```

mongodb output:

```
<match mongo.**>
  # plugin type
  @type mongo

  # mongodb db + collection
  database apache
  collection access

  # mongodb host + port
  host localhost
  port 27017

  # interval
  <buffer>
    flush_interval 10s
  </buffer>

  # make sure to include the time key
  <inject>
    time_key time
  </inject>
</match>
```



<br/>
<br/>


## 流处理

Stream Processing























