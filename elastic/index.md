# Elatic Stack




参考：

- Elastic指南: <https://www.elastic.co/guide/index.html>
- Elasticsearch文档: <https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html>
- Logstash文档: <https://www.elastic.co/guide/en/logstash/current/index.html>
- Kibana文档: <https://www.elastic.co/guide/en/kibana/current/index.html>
- Filebeat文档: <https://www.elastic.co/guide/en/beats/filebeat/index.html>
- Metricbeat文档: <https://www.elastic.co/guide/en/beats/metricbeat/current/index.html>
- Lucence查询语法: <https://wizardforcel.gitbooks.io/mastering-elasticsearch/content/chapter-1/114_README.html>

环境：

- CentOS7.x86_64
- Elastcisearch v6.2.3
- Kibana v6.2.3
- Logstash v6.2.3
- Beats v6.2.3



<br>
<br/>

<!--more-->

---

<br>



# 综述


开源的 Elastic Stack:
能够安全可靠地获取任何来源、任何格式的数据，并且能够实时地对数据进行搜索、分析和可视化。

<br>

![ELK流程图](/images/ELK/ELK.jpg)

![ELK流程图](/images/ELK/elk.png)

<br>

Elastic指的是[elastic](https://www.elastic.co)公司下的几款产品：

- Elasticsearch
- Logstash
- Kibana
- Beats
- X-Pack



<br>
<br/>



## Elasticsearch


- 开放源码且自由使用
- License: Apache License 2.0
- GitHub: <https://github.com/elastic/elasticsearch>
- Doc: <https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html>

<br>

搜索、分析和存储您的数据。
Elasticsearch 是基于 JSON 的分布式搜索和分析引擎，专为实现水平扩展、高可用和管理便捷性而设计。
Elasticsearch 是一个分布式的 RESTful 风格的搜索和数据分析引擎，能够解决不断涌现出的各种用例。作为 Elastic Stack 的核心，它集中存储您的数据，帮助您发现意料之中以及意料之外的情况。

基于Lucene。Lucene是一套用于全文检索和搜寻的开放源码程式库，由Apache软件基金会支持和提供。
Lucene提供了一个简单却强大的应用程式介面，能够做全文索引和搜寻，在Java开发环境里Lucene是一个成熟的免费开放原始码工具；就其本身而论，Lucene是现在并且是这几年，最受欢迎的免费Java资讯检索程式库。




<br>
<br/>



## Logstash


- 开放源码且自由使用
- GitHub: <https://github.com/elastic/logstash>
- Doc: <https://www.elastic.co/guide/en/logstash/current/index.html>

<br>

集中、转换和存储数据
Logstash 是动态数据收集管道，拥有可扩展的插件生态系统，能够与 Elasticsearch 产生强大的协同作用。
Logstash 是开源的服务器端数据处理管道，能够同时 从多个来源采集数据、转换数据，然后将数据发送到您最喜欢的 “存储库” 中。（我们的存储库当然是 Elasticsearch。）



<br>
<br/>



## Kibana


- 开放源码且自由使用
- GitHub: <https://github.com/elastic/kibana>
- Doc: <https://www.elastic.co/guide/en/kibana/current/index.html>

<br>

实现数据可视化
Kibana 让您能够可视化 Elasticsearch 中的数据并操作 Elastic Stack，因此您可以在这里解开任何疑问：例如，为何会在凌晨 2:00 被传呼，雨水会对季度数据造成怎样的影响。




<br>
<br/>



## Beats


- 开放源码且自由使用
- GitHub: <https://github.com/elastic/beats>
- Doc: <https://www.elastic.co/guide/en/beats/libbeat/current/index.html>

<br>

Beats 是轻量型采集器的平台，从边缘机器向 Logstash 和 Elasticsearch 发送数据。
Beats 平台集合了多种单一用途数据采集器。这些采集器安装后可用作轻量型代理，从成百上千或成千上万台机器向 Logstash 或 Elasticsearch 发送数据。



<br>
<br/>



## X-Pack


- Doc: <https://www.elastic.co/guide/en/x-pack/current/index.html>

<br>

一个程序包，带来丰富的可能性
单就其自身而言，Elastic Stack 就是一款值得考虑的强大工具。X-Pack 将诸多强大功能集合到一个单独的程序包中，更将它带上了一个新的层次。
X-Pack 是集成了多种便捷功能的单个插件 — security、alerting、monitoring、reporting、graph 探索和 machine learning — 您可以在 Elastic Stack 中放心地使用这些功能。



<br>
<br/>

---

<br>


# 使用Docker


docker hub里面有ELK的镜像，可以直接拉取使用。推荐使用官方ELK镜像。


我自己做了一个ELK的image，上传到了我的docker hub里。我自己做这个镜像不推荐，因为使用了centos7，导致了镜像很大，这应该避免。

<br>

**在docker中运行centos7**

直接拉取的centos没有systemd的权限，需要在运行添加`docker run -id --privileged <image-id> /usr/sbin/init`选项。

或者使用Docker Hub上CentOS提供的支持systemd的Dockerfile来构建centos: <https://hub.docker.com/_/centos/>
其实Dockfile就是有这条命令`CMD ["/usr/sbin/init`



```sh
docker pull centos
docker image ls


REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              e934aafc2206        2 weeks ago         199MB



#运行docker
#此处如果没有/bin/bash的话，生成的container立马就停止了
#端口映射什么的后面再弄
docker run -d -i <image-id> /bin/bash



#查看容器
docker container ls

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
27b10f5015be        e934aafc2206        "/bin/bash"         About an hour ago   Up About an hour                        ecstatic_boyd



#进入docker
docke exec -it <container-id> /bin/bash
#当然，你也可以运行SSHD，通过端口映射，连接到docker内

#[root@27b10f5015be /]#


#在docker中安装各类需要的软件了
#可能需要设置一下/etc/resolv.conf
```

<br>

**将安装了各类软件的容器构建为一个新的镜像**

```sh
#从运行的容器中重构镜像
#docker commit -m "centos7+elk" <container-id> user/repo:tag
docker commit -m 'centos7+elk' 27b10f5015be zhang21/centos7:elk



#查看新镜像
docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
zhang21/centos7     elk                 0b22d93f7353        16 minutes ago      1.04GB
centos              latest              e934aafc2206        2 weeks ago         199MB


#运行新镜像
docker run -id -p 80:80 9200:9200 <image-id> /bin/bash

#此处遇到一个错，因为docker的网络是通过iptables来转发的，因此主机上不能关闭firewalld，不能无法启动容器

#进入新容器
docker exec -it <container-id> /bin/bash

#此处无法使用systemctl，原因已写到前面
#Failed to get D-Bus connection: Operation not permitted


#获得systemd权限启动
docker run -id --privileged -p 80:80 <image-id> /usr/sbin/init

#进入
docker exec -it <container-id> /bin/bash

#启动Nginx
systemctl start nginx

```

<br>

**将新镜像上传到Hub**

我用的是Docker Hub免费版，当然线上的话可能是阿里云或腾讯云。

```
docker login -u zhang21

#上传镜像到我的Hub
docker push zhang21/centos7:elk


#拉取镜像
docker pull zhang21/centos7:elk
```







<br>
<br/>

---

<br>



# 安装

安装步骤：

- Elasticsearch
- Kibana
- Logstash
- Install X-Pack into Elasticsearch
- Install X-Pack into Kibana


<br>


## 安装ELKF


需要依赖JDK（java），请先安装。
我是直接使用的RPM包安装。

```
#安装Java
yum install java-1.8.0-openjdk-headless-1.8.0.161-0.b14.el7_4.x86_64 -y



#编写repo
vim /etc/yum.repo.d/elk.repo


[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md



#安装
yum install -y elasticsearch logstash kibana filebeat
```

<br>

**由于elk默认将软件安装到`/usr/share/`下，因此我把它们的`bin`路径加入PATH。**

```
vim /etc/profile


export PATH=$PATH:/usr/share/elasticsearch/bin:/usr/share/kibana/bin:/usr/share/logstash/bin:/usr/share/elasticsearch/bin/x-pack:/usr/share/filebeat/bin



#执行
source /etc/profile

```

<br>

ELKF使用RPM安装的布局说明：

- 主目录
	+ `/usr/share/elasticsearch`
	+ `/usr/share/kibana`
	+ `/usr/share/logstash`
	+ `/usr/share/filebeat`
- 二进制文件
	+ `/usr/share/elasticsearch/bin`
	+ `/usr/share/kibana/bin`
	+ `/usr/share/logstash/bin`
	+ `/usr/share/filebeat/bin`
- 配置文件
	+ `/etc/elastcisearch`
	+ `/etc/kibana`
	+ `/etc/logstash`
	+ `/etc/filebeat`
- 环境变量
	+ `/etc/sysconfig/elasticsearch`
- 插件
	+ `/usr/share/elastcisearch/plugins`
	+ `/usr/share/kibana/plugins`



<br>
<br/>


## 安装X-Pack


> **注意**
> 由于自动升级到Elastic v6.3自带了X-Pack，不需要额外安装。之前安装的一些插件会导致Elastic无法运行，请卸载这些插件。

```
elasticsearch-plugin list
elasticsearch-plugin remove x-pack

kibana-plugin remove x-pack

logstash-plugin remove x-pack
```

<br>

安装X-Pack前，请先安装ELK。
请安装匹配版本的X-Pack。

- Install X-Pack on Elasticsearch
- Install X-Pack on Kibana
- Install X-Pack on Logstash

<br>

![安装流程图](/images/ELK/InstallationFlow.jpg)

<br/>

**启用或禁用X-Pack功能**

有些功能默认开启，有些默认关闭。请在配置文件中查看详情。
添加某些功能可能导致软件无法启动，请注意查看日志。

在以下文件中配置它们：

- `elasticsearch.yml`
- `kibana.yml`
- `logstash.yml`
- `filebeat.yml`


X-Pack功能：

| 功能 | 描述
| - | -
| xpack.graph.enabled | X-Pack图形功能
| xpack.ml.enabled | X-Pack机器学习功能
| xpack.monitoring.enabled | X-Pack监视功能
| xpack.reporting.enabled | X-Pack报告功能
| xpack.security.enabled | X-Pack安全功能
| xpack.watcher.enabled | X-Pack观察器

<br>

**在ELK中启动X-Pack monitoring功能**

```
#xpack.graph.enabled
#xpack.ml.enabled
#xpack.monitoring.enabled
#xpack.reporting.enabled
#xpack.security.enabled
#xpack.watcher.enabled


#在Elasticsearch和kibana中禁用验证后，不用在logstash中输入，否则会报错。
xpack.security.enabled: false



#启用验证
#具体可参考官方文档

#在logstash.yml中配置xpack.monitoring
xpack.monitoring.enabled: true
#xpack.monitoring.elasticsearch.url: "http://127.0.0.1:9200"
#xpack.monitoring.elasticsearch.username: logstash_system
#xpack.monitoring.elasticsearch.password: logstash


#在Filebeat中添加monitoring
xpack.monitoring:
  enabled: true
  #elasticsearch:
        #url: "http://localhost:9200"
		#usernaem: "elastic"
		#password: "elastic"
```

<br>

安装：
建议使用密码！

```

#Elastcisearch安装X-Pack
elasticsearch-plugin install x-pack


#启动
#9200, 9300端口
#elasticsearch不能使用root启动，所以我把elastic用户修改为/bin/bash
su elasticsearch
elasticsearch -d
#elasticsearch



#生成默认用户密码，此密码针对elastic和kibana用户
#/usr/share/elasticsearch/bin/x-pack
#将此加入PATH
setup-passwords auto

#或手动输入密码
setup-passwords interactive
elastic
#elastic
kibana
#kibana
logstash_system
#logstash




#Kibana安装X-Pack
kibana-plugin install x-pack


#对kibana.yml添加用户和密码
#此密码是前面默认生成的
vim /etc/kibana/kibana.yml`

elasticsearch.username: "elastic"
elasticsearch.password:  "elastic"
#修改监听地址
server.host: "0.0.0.0"

logging.dest: /var/log/kibana/kibana.log
#kibana日志默认是stdout
#修改为/var/log/kibana/kibana.log
mkdir /var/log/kibana


#启动kibana
#5601端口
#kibana可用root启动
kibana
#或
systemctl start kibana


#Logstash安装X-Pack
logstash-plugin install x-pack


```


<br>
<br/>


## 启动ELK


建议给他们加上密码！
不知道为什么，我的ElasticStack都能用systemd来管理了！

```
#最便捷
systemctl start elasticsearch logstash filebeat metricbeat heartbeat packetbeat auditbeat

```

<br>

```
#Elasticsearch
su elasticsearch
#elasticsearch，查看输出
elasticsearch -d
#kill -15 pid && elasticsearch -d


#Kibana
kibana&
systemctl start kibana
#kill -15 pid && kibana&


#Logstash
#logstash -f xxx.conf
systemctl start logstash


#Filebeat
#filebeat -e -c filebeat.yml，查看输出信息
systemctl start filebeat

```

<br>

**启动时可能遇到的问题**

1. can not run elasticsearch as root
	+ 专门建立一个管理ELK的用户，切换到此用户后运行，注意修改ELK相关目录权限
	+ 或者修改ELK各自用户的/etc/passwd，切换到对应用户后运行。注意权限 -- `su elasticsearch && elasticsearch`

2. elasticsearch process is too low, increase to at least [65536]

```
vim /etc/security/limits.conf


* soft nofile 655350
* hard nofile 655350


ulimit -a
```

<br>

**访问elasticsearch**

```
$ip:9200
#此处访问是需要用户名和密码的
#使用前面X-Pack生成的默认用户名和密码

elastic elastic
#登录之后便可看到node，cluster相关信息
```

![认证后相关信息](/images/ELK/9200auth.png)

<br>

**访问kibana**

```
#5601端口
http://0.0.0.0:5601

```

![](/images/ELK/5601kibana.png)


<br>
<br/>



## 启用xpack注意事项

启用X-PACK后，请注意在kibana配置文件中认证Elasticsearch用户和密码，并且使用Elasticsearch的用户和密码登录Kibana的前端界面。

由于我使用kibana用户登录，导致很多地方访问Elasticsearch都没有权限。请注意。

这样使用Elasticsearch登录后，便可以之间在`Dev Tools`中通过REST API获取和更新相关信息，并且创建和管理相关用户和角色。





<br>
<br/>


## 安装Filebeat


由于前面我们添加了ELK-repo，所以这里我们可以直接安装。

```sh
yum install -y filebeat


#开启X-Pack monitor
#默认关闭
vim /etc/filebeat/filebeat.yml

xpack.monitoring.enabled: true

```


<br>
<br/>


## 修改ELK jvm内存大小


在此版本中，可直接在配置文件目录下的`jvm.options`里修改JVM 内存大小。

```
#最小
-Xms
#最大
-Xmx


vim /etc/elasticsearch/jvm.options

-Xms4g
-Xmx4g


#其它如此
```




<br>
<br/>

---

<br>



# 与Nginx结合使用

将Kibana展现到Nginx上的话，便可以不对Kibana开放外网访问。

```
#安装Nginx

vim /etc/yum.repo.d/nginx.repo


[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1


yum install -y nginx  nginx-mod-stream




#配置
vim /etc/nginx/conf.d/kibana.conf


#可把IP换成kibana相应的域名
#再将域名解析到此IP
server {

    listen 80;
    server_name 172.16.129.150;

#Kibana
    location / {
        proxy_pass http://127.0.0.1:5601;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

}

```

<br>

**可能会遇到的问题**

- Nignx错误日志: Permission denied) while connecting to upstream

```
sudo cat /var/log/audit/audit.log | grep nginx | grep denied

#后来判断是SELinux的问题
getenforce
setenforce 0


#修改SELinux
vim /etc/selinux/config

SELINUX=disabled

```




<br>
<br/>

---

<br>



# Logstash文档


<br>

![logstash流程图](/images/ELK/basic_logstash_pipeline.png)

<br>

Logstash的pipeline有两个必须的元素：

- input
	+ 消耗来自source的数据
- output
	+ 将修改后的数据写入destination

以及一个可选元素：

- filter
	+ 根据你的定义来修改数据


<br>
<br/>


## 介绍


Logstash是一个具有实时流水线(pipeling)功能的开源数据收集引擎。它可以动态统一来自不同source的数据，并将数据正常化的你的destination。

任何类型的事件都可以通过大量的输入、过滤和输出插件进行丰富和转换，通过本地编解码器进一步简化了摄取过程。


<br>
<br/>


### Logstash的能量


具有强大的Elasticsearch和Kibana系统的水平可伸缩数据处理流水线。

![](/images/ELK/logstash.png)


<br>
<br/>


#### Logstash喜欢的数据

所有数据来者不拒！

<br>

#### Logs and Metrics

- 处理所有类型的日志数据
	+ Apache
	+ Nginx
	+ Syslog
- 使用**Filebeat**享受互补的安全日志转发功能
- 从`Ganglia`, `JMx`, `NetFlow`和`TCP`,`UDP`收集**metrics**

<br>

#### Web

- 将`http request`转换为`events`
	+ 分析Web服务
	+ 支持Webhook
- 通过轮询`HTTP endpoint`创建事件
	+ 通过Web API捕获健康状况、性能和其它类型的数据

<br>

#### 数据存储和流

从你已经拥有的数据中发现更多价值。

<br>

#### Sensors and IoT

探索广泛的其它数据。


<br>
<br/>


### 轻松丰富一切


在摄取过程中清理并转换数据，以便在index或output时立即获得实时信息。Logstash具有许多聚合和突变以及模式匹配，地理映射和动态查找功能。

- **Grok**是Logstash filter的金刚钻，用于从非结构化数据中派生出结构化数据
- 通过解析来自IP的地理坐标，标准化提起复杂性，简单K-V对和CSV数据，并通过本地查找或Elasticsearch查询进一步丰富你的数据，从而扩展你的视野
- 编解码器通常用于缓解JSON和多行事件等常见事件结构的处理


<br>
<br/>


### 选择你的储藏室


将数据放在最重要的位置。通过存储，分析和对数据采取行动，解锁各种downstream分析和操作用例。

- Analysis
	+ Elasticsearch
	+ Data stores(MongoDB, Redis)
- Archiving
	+ HDFS
	+ S3
- Monitoring
	+ Nagios
	+ Zabbix
	+ Ganglia
- Alerting
	+ Watcher(Elasticsearch)
	+ Email




<br>
<br/>


## 入门


安装，储藏，解析，汇聚多个Input/Output。


<br>
<br/>


### 储藏第一个事件


**测试Logstash和运行一个基本的pipeline**

```
logstash -e 'input { stdin { } } output { stdout {} }'

#等待启动，输入hello world
#Logstash将时间戳和主机名添加到message
#2018-04-13T08:17:51.702Z zhang22 helloworld

```

<br>

启动logstsh时的一个问题： `WARNING: Could not find logstash.yml which is typically located in $LS_HOME/config or /etc/logstash`

虽然通过RPM安装Logstash存在`/etc/logstash`文件，但是还是会报错。

```
cd /usr/share/logstash/bin

ln -s /etc/logstash ./config

```


<br>
<br/>


### 通过Logstash解析Logs


前面我们创建了一个基本的Logstash pipeline来测试Logstash，但真正处理logs的Logstash pipeline不会这么简单，它可能会有多个input, filter, output。

本节利用一个Filebeat，将Nginx Web Logs作为Logstash pipeline的input，解析这些logs中创建的特定命名字段，并将解析的数据写入Elasticsearch集群。

<br>

**配置Filebeat以发送Log Lines到Logstash**

在创建Logstash pipeline之前，你将配置Filebeat以发送Log lines到Logstash。Filebeat从服务器上的文件收集日志，并将这些日志转发给Logstash实例进行处理。
Filebeat专为可靠性和低延迟而设计。它占用的资源极少，`beats input`插件(默认安装)最大限度地减少了Logstash实例的资源需求。任何Beat框架编写的beat都可以讲事件数据发送到Logstash。

在你的**data source**主机上安装Filebeat。安装之后，配置`filebeat.yml`文件:

```
vim /etc/filebeat/filebeat.yml


filebeat.prospectors:
- type: log
#需要处理的日志的路径，如Nginx
  paths:
  	- /var/log/nginx/*.log

output.logstash:
  hosts: ["localhost:5044"]



#运行Filebeat
Filebeat -e -c filebeat.yml -d "publish"


#Filebeat将会尝试连接到5044端口，在Logstash以一个活动的beats plugin开始前，不会有任何应答。
```

<br>

**为Filebeat Input配置Logstash**

配置一个Logstash pipeline，使用beat input plugin接受来自beats的事件。
格式如下：

```
cd /etc/logstash/conf.d
vim ./first-pipeline.conf

input {

}

#filter部分可选
filter {

}

output {

}




#实例
input {
    beats {
    	port => "5044"
    }
}

output {
	stdout {
    	codec => dubydebug
    }
}




#验证配置
logstash -f first-pipe.conf --config.tst_and_exit


#消息
2018-04-17T14:15:46.187+0800    ERROR   pipeline/output.go:74   Failed to connect: dial tcp [::1]:5044: getsockopt: connection refused
2018-04-17T14:15:46.607+0800    INFO    log/harvester.go:241    File is inactive: /var/log/nginx/access.log. Closing because close_inactive of 5m0s reached.
2018-04-17T14:15:46.607+0800    INFO    log/harvester.go:241    File is inactive: /var/log/nginx/error.log. Closing because close_inactive of 5m0s reached.
2018-04-17T14:15:46.923+0800    INFO    log/harvester.go:241    File is inactive: /var/log/secure-20180409. Closing because close_inactive of 5m0s reached.
2018-04-17T14:15:51.096+0800    INFO    log/harvester.go:241    File is inactive: /var/log/secure-20180401. Closing because close_inactive of 5m0s reached.
2018-04-17T14:15:52.687+0800    INFO    log/harvester.go:241    File is inactive: /var/log/secure-20180415. Closing because close_inactive of 5m0s reached.


#启动Logstash
#修改配置后可动态载入
logstash -f first-pipe.conf --config.reload.automatic


#消息
2018-04-17T14:18:41.542+0800    INFO    [monitoring]    log/log.go:124  Non-zero metrics in the last 30s        {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":150,"time":159},"total":{"ticks":450,"time":468,"value":450},"user":{"ticks":300,"time":309}},"info":{"ephemeral_id":"84cbf5cd-dfff-4391-9631-2b8e77329696","uptime":{"ms":480009}},"memstats":{"gc_next":11030992,"memory_alloc":6588088,"memory_total":40882600}},"filebeat":{"harvester":{"open_files":5,"running":8}},"libbeat":{"config":{"module":{"running":2}},"pipeline":{"clients":8,"events":{"active":4118}}},"registrar":{"states":{"current":10}},"system":{"load":{"1":4.86,"15":4.41,"5":4.53,"norm":{"1":2.43,"15":2.205,"5":2.265}}}}}}

```

<br>

**使用Grok filter plugin解析Web Logs**

在某些时候，可能输出的日志信息的格式并不理想。你想要解析log以创建特定的命名字段。

`grok`过滤插件使你能够将非结构化的日志数据解析为结构化和可查询的内容。
由于`grok`过滤器插件在传入的日志数据中查找模式，因此配置插件需要你作出关于如何识别你的用例。

你可以使用`%{COMBINEDAPACHELOG}` grok模式，它从如下模式的日志中构建行：

| 信息 | Field Name
| - | -
| IP Add | clientip
| User ID | ident
| User Auth | auth
| timestamp | timestamp
| HTTP Verb | verb
| Request body | request
| HTTP Status code | respone
| Referer URL | referer
| User agent | agent

<br>

```
vim first-pipline.conf


input {
	beats {
    	port => "5044"
    }
}

filter {
	grok {
    	match => {
        	"message" => "%{COMBINEDAPACHELOG}"
        }
    }
}

output {
	stdout {
    	codec => rubydebug
    }
}


#启动查看效果
```

<br>

**通过Geoip过滤插件增强数据**

除了解析日志数据以获得更好的搜索外，过滤插件还可从现有的数据中后去补充信息。
`geoip`插件查找IP地址，从IP地址获取地理位置信息，并将该位置信息添加到日志中。

配置Logstash实例来使用geoip过滤插件:

```
vim first-pipeline.conf


input {
    beats {
        port => "5044"
    }
}
 filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}"}
    }
    geoip {
        source => "clientip"
    }
}
output {
    stdout { codec => rubydebug }
}


#重启服务
```

<br>

**索引数据到Elasticsearch**

现在Web log已经被处理为指定的字段，现在Logstash pipeline便可以索引数据到一个Elasticsearch集群中。

```
vim first-pipeline.conf


input {
    beats {
        port => "5044"
    }
}
 filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}"}
    }
    geoip {
        source => "clientip"
    }
}
output {
    elasticsearch {
        hosts => [ "localhost:9200" ]
    }
}

#重启服务
```

**验证：**

这里遇到一个错误：

- `index_not_found_exception`

这里要将`logstash-$DATE`反映索引的实际名称，也就是在通过下面的命令得到的`logstash-2018.04.13`。把我坑惨了！

```
curl -XGET 'localhost:9200/logstash-$DATE/_search?pretty&q=response=200'

#索引名称使用的日期基于UTC，而不是Logstash正在运行的timezone


#查看可用索引列表
curl 'localhost:9200/_cat/indices?v'


health status index                           uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   logstash-2018.04.13             dRW2veUgS2ObZmP3lepqsQ   5   1        154            0    266.6kb        266.6kb



curl -XGET 'localhost:9200/logstash-2018.04.13/_search?pretty&q=response=200'

```

![访问图](/images/ELK/logstash-20180413.png)

<br>

Kibana中的可视化效果：

![](/images/ELK/kibana-20180413.png)


<br>
<br/>


### 拼接多个输入和输出插件


你需要管理的信息通常来自多个不同的source，并且可能需要多个不同的destination来存储数据。Lostash pipeline可以使用多个输入和输出插件来处理这些需求。

<br>

官方文档中使用Twitter and Filebeat这两者作为Logstash input，并将信息输出到Elasticsearch和file。

- 配置Logstash实例使用Filebeat input plugin
- 配置Logstash实例写入Elasticsearch多节点(cluster)
- 配置Logstash pipeline将数据写入file

```
#配置Filebeat发送Log Line到Logstash
vim /etc/filebeat/filebeat.yml

filebeat.prospectors:
- type: log
  paths:
    - /var/log/*.log
  fields:
    type: syslog

output.logstash:
  hosts: ["localhost:5044"]


########################


cd /etc/logstash/conf.d

vim 2nd-pipeline.conf


input {
	twitter {
        consumer_key => "enter_your_consumer_key_here"
        consumer_secret => "enter_your_secret_here"
        keywords => ["cloud"]
        oauth_token => "enter_your_access_token_here"
        oauth_token_secret => "enter_your_access_token_secret_here"
    }
	beats {
    	prot => "5044"
    }
}

output {
	elasticsearch {
    	hosts => ["hosts1:port1", "host2:port2"...]
    }
    file {
    	path => "/path/to/target/file"
    }
}


#重启服务
#测试，Replace $DATE with the current date, in YYYY.MM.DD format.
curl -XGET 'localhost:9200/logstash-$DATE/_search?pretty&q=fields.type:syslog'

```



<br>
<br/>

---

<br>


## Input


输入插件可以使特定的事件源由Logstash读取。

可用的输入插件：
我只列出了常见的，具体请参考: <https://www.elastic.co/guide/en/logstash/current/input-plugins.html>

| 插件 | 描述
| - | -
| beats | 从Elastic框架接收事件
| couchdb_changes | 从CouchDB的`_changes`URI流式传输事件
| dead_letter_queue | 从Logstash的dead letter queue读取事件
| elasticsearch | 从Elasticsearch集群中读取查询结果
| exec | 抓取shell命令的输出作为事件
| file | 来自文件的流事件
| github | 从GitHub webhook读取事件
| heartbeat | 为测试生成心跳事件
| http | 通过HTTP/HTTPS接收事件
| http_poller | 解码HTTP API输出为事件
| imap | 从IMAP服务器读取邮件
| jmx | 通过JVM从java程序检索标准
| kafka | 从kafka中读取事件
| log4j | 通过TCP socket从Log4j对象读取事件
| pipe | 从长时间运行的命令管道中获取流事件
| rabbitmq | 从Redis实例读取事件
| sqlite | 基于SQLite数据库中的行创建事件
| stdin | 从标准输入中读取事件
| syslog | 读取系统日志作为事件
| tcp | 从TCP socket读取事件
| udp | 从UDP读取事件
| unix | 通过Unix socket读取事件
| websocket | 从一个websocket读取事件

<br>

**input filter通用选项: **

| Setting	| Input type | Required
| - | - | -
| add_field | hash | No
| codec | codec | No
| enable_metric | boolean | No
| id | string | No
| tags | array | No
| type | string | No

- `add_field`
添加一个字段到一个事件，默认值为`{}`

- `codec`
用于输入数据的编解码器。默认值是`plain`

- `enable_metric`
为特定插件实例禁用或启用度量标准日志记录，默认值为`true`

- `id`
为插件配置添加一个唯一的ID，如果未指定，Logstash会自动生成一个

- `tags`
为事件添加任意数量的任意标签

- `type`
为所有input处理的事件添加一个`type`


<br>
<br/>

### beats

此插件使Logstash能够从Elasticsearch框架中接收事件。

栗子：

```
input {
  beats {
    port => 5044
  }
}

output {
  elasticsearch {
    hosts => "localhost:9200"
	#hosts => ["hosts1", "hosts2", ...]
    manage_template => false
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
    document_type => "%{[@metadata][type]}"
  }
}

```

<br>

**Beats Input配置项：**

| Setting | Input_type | Required
| - | - | -
| cipher_suites | array | No
| client_inactivity_timeout | number | No
| host | string | No
| include_codec_tag | boolean | No
| port | number | Yes
| ssl | boolean | No
| ssl_certificate | a valid filesystem path | No
| ssl_certificate_authorities | array | No
| ssl_handshake_timeout | number | No
| ssl_key | a valid filesystem path | No
| ssl_key_passphrase | password | No
| ssl_verify_mode | string, one of [none, peer,force_peer] | No
| tls_max_version | number | No
| tls_min_version | number | No


<br>
<br/>


### elasticsearch

**Elasticsearch Input配置项：**

| Setting | Input_type | Required
| - | - | -
| ca_file | a valid filesystem path | No
| docinfo | boolean | No
| docinfo_fields | array | No
| docinfo_target | string | No
| hosts | array | No
| index | string | No
| password | password | No
| query | string | No
| schedule | string | No
| scroll | string | No
| size | number | No
| ssl | boolean | No
| user | string | No

<br>

栗子：

```
input {
  elasticsearch {
    hosts => "es.production.mysite.org"
    index => "mydata-2018.09.*"
    query => '{ "query": { "query_string": { "query": "*" } } }'
    size => 500
    scroll => "5m"
    docinfo => true
  }
}
output {
  elasticsearch {
    index => "copy-of-production.%{[@metadata][_index]}"
    document_type => "%{[@metadata][_type]}"
    document_id => "%{[@metadata][_id]}"
  }
}
```


<br>
<br/>


### exec

定期运行shell命令，并抓取整个输出为事件。

栗子：

```
input {
  exec {
    command => "ls"
    interval => 30
  }
}
```

<br>

**exec Input配置项：**

| Setting | Input_type | Required
| - | - | -
| command | string | Yes
| interval | number | No
| schedule | string | No

此调度表示方法如同Linux中定时任务`* 5 * 1-3 *`。


<br>
<br/>


### file

从文件读取流事件。

**file input配置项：**

| Setting | Input_type | Required
| - | - | -
| close_older | number |No
| delimiter | string | No
| discover_interval | number | No
| exclude | array | No
| ignore_older | number | No
| max_open_files | number | No
| path | array | Yes
| sincedb_path | string | No
| sincedb_write_interval | number | No
| start_position | string, one of ["beginning", "end"] | No
| stat_interval | number | No


<br>
<br/>


### github

**github input配置项：**

| Setting | Input_type | Required
| - | - | -
| drop_invalid | boolean | No
| ip | string | No
| port | number | Yes
| secret_token | string | No


<br>
<br/>


### kafka

<https://www.elastic.co/guide/en/logstash/current/plugins-inputs-kafka.html>


<br>
<br/>


### redis

从redis实例读取事件，它支持redis的channel和list类型。

**redis input配置项：**

| Setting | Input_type | Required
| - | - | -
| batch_count | number | No
| data_type | string, one of [list,channel,pattern_channel] | Yes
| db | number | No
| host | string | No
| path | string | No
| key | string | Yes
| password | password | No
| port | number | No
| ssl | boolean | No
| threads | number | No
| timeout | number | No


<br>
<br/>


### sqlite


栗子：

```
input {
  sqlite {
    path => "/tmp/example.db"
    type => weblogs
  }
}
output {
  stdout {
    debug => true
  }
}
```

**sqlite input配置项：**

| Setting | Input_type | Required
| - | - | -
| batch | number | No
| exclude_tables | array | No
| path | string | Yes


<br>
<br/>


### stdin


<br>
<br/>


### syslog

栗子：

```
input {
  syslog {
    port => 12345
    codec => cef
    syslog_field => "syslog"
    grok_pattern => "<%{POSINT:priority}>%{SYSLOGTIMESTAMP:timestamp} CUSTOM GROK HERE"
  }
}
```

<br>

**syslog input配置项：**

| Setting | Input_type | Required
| - | - | -
| facility_labels | array | No
| grok_pattern | string | No
| host | string | No
| locale | string | No
| port | number | No
| proxy_protocol | boolean | No
| severity_labels | array | No
| syslog_field | string | No
| timezone | string | No
| use_labels | boolean | No


<br>
<br/>


### tcp

栗子：

```
input {
  tcp {
    port => 12345
    codec => json
  }
}
```

<br>

**tcp input配置项：**

| Setting | Input_type | Required
| - | - | -
| host | string | No
| mode | string, one of [server, client] | No
| port | number | Yes
| proxy_protocol | boolean | No
| ssl_cert | a valid file system path | No
| ssl_enable | boolean | No
| ssl_extra_chain_certs | array | No
| ssl_key | a valid file system path | No
| ssl_key_passphrase | password | No
| ssl_verify | boolean | No


<br>
<br/>


### udp


<br>
<br/>


### unix


<br>
<br/>


### websocket


<br>
<br/>



## Output


输出将事件数据发送到特定的目标。输出是事件管道的最后阶段。


输出列表：

- boundary
- circonus
- CSV
- datadog
- Elasticsearch
- email
- exec
- file
- gelf
- ganglia
- http/https
- influxdb
- irc
- kafka
- librato
- loggly
- lumberjack
- metriccatcher
- mongodb
- nagios
- opentsdb
- pipe
- rabbitmq
- redis
- redmine
- stdout
- syslog
- tcp
- udp
- websocket
- zabbix

<br>

**output通用配置项： **

| Setting | Input type | Required
| - | - | -
| codec | codec | No
| enable_metric | boolean | No
| id | string | No

- `codec`
用于输出数据的编解码器，默认值是`json_lines`

- `enable_metric`
为特定插件实例启用或禁用度量日志记录，默认值是`true`

- `id`
为插件配置添加一个唯一的ID，如果未指定ID，Logstash会自动生成。


<br>
<br/>


### csv

**csv output配置选项：**

| Setting | Input_type | Required
| - | - | -
| create_if_deleted | boolean | No
| csv_options | hash | No
| dir_mode | number | No
| fields | array | Yes
| file_mode | number | No
| filename_failure | string | No
| flush_interval | number | No
| gzip | boolean | No
| path | string | Yes
| spreadsheet_safe | boolean | No


<br>
<br/>


### elasticsearch

**Elasticsearch output配置项：**

Setting | Input type | Required
- | - | -
action | string | No
bulk_path | string | No
cacert | a valid filesystem path | No
doc_as upsert | boolean | No
document_id | string | No
document_type | string | No
failure_type logging whitelist | array | No
healthcheck_path | string | No
hosts | uri | No
http_compression | boolean | No
index | string | No
keystore | a valid filesystem path | No
keystore_password | password | No
manage_template | boolean | No
parameters | hash | No
parent | string | No
password | password | No
path | string | No
pipeline | string | No
pool_max | number | No
pool_max per route | number | No
proxy | uri | No
resurrect_delay | number | No
retry_initial interval | number | No
retry_max_interval | number | No
retry_on_conflict | number | No
routing | string | No
script | string | No
script_lang | string | No
script_type | string, one of [inline, indexed, file] | No
script_var_name | string | No
scripted_upsert | boolean | No
sniffing | boolean | No
sniffing_delay | number | No
sniffing_path | string | No
ssl | boolean | No
ssl_certificate verification | boolean | No
template | a valid filesystem path | No
template_name | string | No
template_overwrite | boolean | No
timeout | number | No
truststore | a valid filesystem path | No
truststore_password | password | No
upsert | string | No
user | string | No
validate_after inactivity | number | No
version | string | No
version_type | string, one of [internal, external, external gt, external gte, force] | No


<br>
<br/>


### exec

栗子：

```
output {
  if [type] == "abuse" {
    exec {
      command => "iptables -A INPUT -s %{clientip} -j DROP"
    }
  }
}
```

<br>

**exec output配置项：**

Setting | Input type | Required
- | - | -
command | string | Yes
quiet | boolean | No


<br>
<br/>


### file

栗子：

```
output {
 file {
   path => ...
   codec => line { format => "custom format: %{message}"}
 }
}
```

<br>

**file output配置项：**

Setting | Input type | Required
- | - | -
create_if_deleted | boolean | No
dir_mode | number | No
file_mode | number | No
filename_failure | string | No
flush_interval | number | No
gzip | boolean | No
path | string | Yes
write_behavior | string | No


<br>
<br/>


### kafka

栗子：

```
output {
  kafka {
    codec => json
    topic_id => "mytopic"
  }
}
```

<br>

**kafka output配置项：**

Setting | Input type | Required
- | - | -
acks | string, one of [0, 1, all] | No
batch_size | number | No
bootstrap_servers | string | No
buffer_memory | number | No
client_id | string | No
compression_type | string, one of [none, gzip, snappy, lz4] | No
jaas_path | a valid filesystem path | No
kerberos_config | a valid filesystem path | No
key_serializer | string | No
linger_ms | number | No
max_request size | number | No
message_key | string | No
metadata_fetch_timeout_ms | number | No
metadata_max_age_ms | number | No
receive_buffer_bytes | number | No
reconnect_backoff_ms | number | No
request_timeout_ms | string | No
retries | number | No
retry_backoff_ms | number | No
sasl_kerberos_service name | string | No
sasl_mechanism | string | No
security_protocol | string, one of [PLAINTEXT, SSL, SASL PLAINTEXT, SASL SSL] | No
send_buffer_bytes | number | No
ssl_key_password | password | No
ssl_keystore_location | a valid filesystem path | No
ssl_keystore_password | password | No
ssl_keystore_type | string | No
ssl_truststore_location | a valid filesystem path | No
ssl_truststore_password | password | No
ssl_truststore_type | string | No
topic_id | string | Yes
value_serializer | string | No


<br>
<br/>


### mongodb

**mongodb output配置项：**

Setting | Input type | Required
- | - | -
bulk | boolean | No
bulk_interval | number | No
bulk_size | number | No
collection | string | Yes
database | string | Yes
generateId | boolean | No
isodate | boolean | No
retry_delay | number | No
uri | string | Yes


<br>
<br/>


### redis

将Redis作为消息队列缓存能极大降低系统负载，减轻系统压力。

**redis output配置项：**

Setting | Input type | Required
- | - | -
batch | boolean | No
batch_events | number | No
batch_timeout | number | No
congestion_interval | number | No
congestion_threshold | number | No
data_type | string, one of [list, channel] | No
db | number | No
host | array | No
key | string | No
password | password | No
port | number | No
reconnect_interval | number | No
shuffle_hosts | boolean | No
timeout | number | No


<br>
<br/>


### redmine

栗子：

```
output {
  redmine {
    url => "http://redmineserver.tld"
    token => 'token'
    project_id => 200
    tracker_id => 1
    status_id => 3
    priority_id => 2
    subject => "Error ... detected"
  }
}
```

<br>

**redmine output配置项：**

Setting | Input type | Required
- | - | -
assigned_to_id | number | No
categorie_id | number | No
description | string | No
fixed_version_id | number | No
parent_issue_id | number | No
priority_id | number | Yes
project_id | number | Yes
ssl | boolean | No
status_id | number | Yes
subject | string | No
token | string | Yes
tracker_id | number | Yes
url | string | Yes


<br>
<br/>


### output

栗子：

```
output {
  stdout { codec => json }
}
```


<br>
<br/>


### syslog

**syslog output配置：**

Setting | Input type | Required
- | - | -
appname | string | No
facility | string | No
host | string | Yes
message | string | No
msgid | string | No
port | number | Yes
priority | string | No
procid | string | No
protocol | string, one of [tcp, udp, ssl-tcp] | No
reconnect interval | number | No
rfc | string, one of [rfc3164, rfc5424] | No
severity | string | No
sourcehost | string | No
ssl_cacert | a valid filesystem path | No
ssl_cert | a valid filesystem path | No
ssl_key | a valid filesystem path | No
ssl_key passphrase | password | No
ssl_verify | boolean | No
use_labels | boolean | No


<br>
<br/>


### zabbix

**zabbix output配置项：**

Setting | Input type | Required
- | - | -
multi_value | array | No
timeout | number | No
zabbix_host | string | Yes
zabbix_key | string | No
zabbix_server host | string | No
zabbix_server port | number | No
zabbix_value | string | No


<br>
<br>
<br/>


## Filter


- <https://www.elastic.co/guide/en/logstash/current/filter-plugins.html>

过滤器插件对事件执行中介(intermediary)处理，过滤器通常根据事件的特征有条件的应用。

下面是Elastic支持的插件列表:

| 插件 | 描述
| - | -
| aggregate | 汇总来自单个任务的多个事件的信息
| alter | 对mutate过滤器无法处理的字段进行常规更改
| cidr | 根据网络块列表检查IP地址
| cipher | 对事件应用或移除cipher(密码)
| clone | 重复事件
| csv | 将csv(comma separated value)解析为单个字段
| date | 解析字段中的日期，以用作事件的Logstash timestamp
| de_dot | Computationally expensive filter that removes dots from a field name
| dissect | 使用分隔符将非结构化事件数据提取到字段中
| dns | 执行标准或反向DNS查询
| drop | 删除所有事件
| elapsed | 计算一对事件之间的经过时间
| elasticsearch | 将Elasticsearch中以前的日志事件的字段复制到当前事件中
| environment | 将环境变量存储为元数据子字段
| extractnumbers | 从字符串中提取数字
| fingerprint | 由一致的散列值的替换值的指纹字段
| geoip | 添加有关IP地址的地理信息
| grok | 将非结构化事件数据解析到字段中
| i18n | 从字段中删除特定字符
| jdbc_static | 使用从远程数据库预加载的数据来丰富事件
| jdbc_streaming | 用你的数据库数据丰富事件
| json | 解析JSON事件
| json_encode | 将字段序列化为JSON
| kv | 解析键值对
| metricize | 处理包含多个度量标准的复杂事件并将它们分成多个事件，每个事件都包含一个度量标准
| metrics | 汇总指标(Aggregates metrics)
| mutate | 对字段执行突变
| prune | 将基于字段列表的事件数据精简为黑名单或白名单
| range | 检查指定的字段是否在给定的大小或长度限制内
| ruby | 执行任意Ruby代码
| sleep | 休息一段指定的时间
| split | 将多行消息拆分成不同的事件
| syslog_pri | 解析syslog消息的优先字段
| throttle | 限制事件的数量
| tld | 用你在配置中指定的任何内容替换默认消息字段的内容
| translate | 根据散列或YAML文件，替换字段内容
| truncate | 截断长度超过给定长度的字段
| urldecode | 解码URL编码的字段
| useragent | 将用户代理字符串解析到字段中
| uuid | 为事件添加UUID
| xml | 将XML解析到字段

<br>

**所有过滤器都支持的配置选项：**

| Setting | Input_type | Required
| - | - | -
| add_field | hash | No
| add_tag | array | No
| enable_metric | boolean | No
| id | string | No
| periodic_flush | boolean | No
| remove_field | array | No
| remove_tag | array | No

- `add_field`
如果此过滤器成功，添加任意字段到此事件。字段名称可以是动态的，并使用`%{field}`包含事件的部分内容

- `add_tag`
如果此过滤器成功，添加任意标签到此事件。标签可以是动态的，并使用`%{field}`语法包含事件的部分内容

- `enable_metric`
为特定插件实例启用/禁用度量标准日志记录

- `id`
为插件配置添加一个唯一的ID，如果没有指定ID，Logstash会生成一个。强烈建议在配置中设置此ID
当你有多个相同类型的插件时，这特别有用

- `periodic_flush`
定期调用过滤器flush方法

- `remove_field`
如果此过滤器成功，从事件中移除任意字段

- `remove_tag`
如果此过滤器成功，从事件中移除任意标签


<br>
<br/>


### Aggregate

此过滤器的目的是聚合属于同一任务的多个事件(通常是日志行)中可用的信息，并将最终聚合信息推送到最终任务事件中。

<br>

**Aggregate Filter Configuration Options: **

| Setting | Input_type | Required
| - | - | -
| aggregate_maps_path | string, a valid filesystem path | No
| code | string | Yes
| end_of_task | boolean | No
| inactivity_timeout | number | No
| map_action | string, one of ["create", "update", "create_or_update"] | No
| push_map_as_event_on_timeout | boolean | No
| push_previous_map_as_event | boolean | No
| task_id | string | Yes
| timeout | number | No
| timeout_code | string | No
| timeout_tags | array | No
| timeout_task_id_field | string | No
| timeout_timestamp_field | string | No

- `aggregate_maps_path`
Logstash停止时存储聚合地图的文件路径，以及Logstash启动时加载的路径。
如果未定义，聚合映射将不会存储在Logstash中，并且会丢失。

- `code`
使用当前事件执行更新map的代码；或使用当前的map执行更新事件的代码
你将有一个可用的map variable 和 event variable

- `end_of_task`
告诉过滤器该任务已结束，因此在代码执行后删除聚合map

- `inactivity_timeout`
一个任务被认为已到期的秒数
当某个任务超时时，其聚合map将被逐出
必须小于timeout

- `map_action`
	+ `create`
	+ `update`
	+ `create_or_update`
告诉过滤器如何处理聚合map

- `push_map_as_event_on_timeout`
每次检测到任务超时时，它都会将任务集合映射推送为新的Logstash事件

- `push_previous_map_as_event`
每次聚合插件检测到新任务ID时，它会将先前的聚合映射推送为新的Logstash事件，然后为下一个任务创建新的空映射

- `task_id`
定义了关联日志的任务ID的表达式
该值必须唯一标识任务

- `timeout`

- `time_code`

- `timeout_tags`
在生成超时事件添加的标记

- `timeout_task_id_field`

- `timeout_timestamp_field`
默认情况下，使用系统时间计算超时



<br>

**栗子：**

给定日志:

```
 INFO - 12345 - TASK_START - start
 INFO - 12345 - SQL - sqlQuery1 - 12
 INFO - 12345 - SQL - sqlQuery2 - 34
 INFO - 12345 - TASK_END - end
```

过滤器:

```
 filter {
   grok {
     match => [ "message", "%{LOGLEVEL:loglevel} - %{NOTSPACE:taskid} - %{NOTSPACE:logger} - %{WORD:label}( - %{INT:duration:int})?" ]
   }

   if [logger] == "TASK_START" {
     aggregate {
       task_id => "%{taskid}"
       code => "map['sql_duration'] = 0"
       map_action => "create"
     }
   }

   if [logger] == "SQL" {
     aggregate {
       task_id => "%{taskid}"
       code => "map['sql_duration'] += event.get('duration')"
       map_action => "update"
     }
   }

   if [logger] == "TASK_END" {
     aggregate {
       task_id => "%{taskid}"
       code => "event.set('sql_duration', map['sql_duration'])"
       map_action => "update"
       end_of_task => true
       timeout => 120
     }
   }
 }
```


<br>
<br/>


### Alter

alter filter允许对未包含在正常变异过滤器中的字段进行一般更改。

<br>

**安装: **

```
logstash-plugin install logstash-filter-alter
```

<br>

**配置项: **

| Setting	| Input type | Required
| - | - | -
| coalesce | array | No
| condrewrite | array | No
| condrewriteother | array | No


- `coalesce`
将file_name的值设置为其参数的第一个非空表达式

- `condrewrite`
如果实际内容等于预期内容，则将字段内容更改为指定值

- `condrewriteother`
如果另一个字段内容等于预期内容，则将字段内容更改为指定值


<br>
<br/>


### cidr

CIDR filter用于检查时间中的IP地址与可能包含它的网络块列表。可以针对多个网络检查多个地址，任何匹配都可以成功。成功后，可将其它标记/字段添加到事件中。

<br>

**配置项: **

| Setting | Input_type | Required
| - | - | -
| address | array | No
| network | array | No
| network_path | a valid filesystem path | No
| refresh_interval | number | No
| separator | string | No

- `address`
要检查的IP地址

- `network`
要检查的IP网络

- `network_path`
包含过滤器应检查的网络的外部文件的完整路径

- `refresh_interval`
检查外部文件的更新频率

- `seperator`
从network_path指定的外部文件解析网络的分隔符


<br>
<br/>


### csv

csv filter处理包含csv数据的事件字段，解析它，并将其存储为单个字段
此过滤器还可解析使用任何分隔符的数据，而不仅仅是逗号

<br>

**配置项: **

| Setting | Input_type | Required
| - | - | -
| autodetect_column_names | boolean | No
| autogenerate_column_names | boolean | No
| columns | array | No
| convert | hash | No
| quote_char | string | No
| separator | string | No
| skip_empty_columns | boolean | No
| skip_empty_rows | boolean | No
| skip_header | boolean | No
| source | string | No
| target | string | No

- `autodetect_column_names`
是否应该从标题列自动检测列名称，默认false

- `autogenerate_column_names`
是否应该自动生成列名，默认true。
如果设置为false，那么没有指定header的列将不会被解析

- `columns`
列名称的列表

- `convert`
应用于列的数据类型转换的集合，可能的转换: integer, float, date, date_time, boolean

- `quote_char`
用于引用csv字段的字符，默认`"`

- `separator`
列分隔符值。默认值comma`,`

- `skip_empty_columns`
是否应该跳过空列，默认false

- `skip_empty_rows`
是否应该跳过空行，默认false

- `skip_header`
是否应该跳过header，默认false

- `source`
源字段值中的csv数据将被扩展为数据结构

- `target`
放置数据的目标字段


<br>
<br/>


### date

date filter从字段中解析日期，然后使用该日期或时间戳作为事件的Logstash时间戳。它对事件排序和回填旧数据尤其重要。
在没有此过滤器的情况下，如果timestamp尚未在事件中设置，则Logstash将根据首次查看事件是(input time)选择一个时间戳。

<br>

**date filter配置项：**

| Setting | Input_type | Required
| - | - | -
| locale | string | No
| match | array | No
| tag_on_failure | array | No
| target | string | No
| timezone | string | No

**locale**
使用POSIX语言标记指定用于日期解析的环境(locale)，如`en`,`en_US`
如果未指定，则将使用平台默认值

**match**
有字段名称和格式模式的数组，[ field, formats...]

如果时间字段有多种格式，你可这样做:

```
match => [ "filed-name", "MMM dd yyyy HH:mm:ss",
          "MMM  d yyyy HH:mm:ss", "ISO8601" ]

嵌套字段表示
[foo][bar]
```

有几个例外:

- `ISO8601`: 解析任何有效的ISO8601时间戳，如`2011-04-19T03:44:01.103Z`
- `UNIX`: 解析float/int Unix原子时间(s)
- `UNIX_MS`: 解析int Unix原子时间
- `TAI64N`: 解析tai64n时间值

语法细节:
用于解析日期和时间文本的语法使用字母来指示时间值的种类，以及重复的字母来指示该值的形式。

以下是可用于解析日期和时间的内容：

- y year
	+ yyyy
      完整年号，如`2018`
	+ yy
		两位数年份，如`18`

- M month of the year
	+ M
	  最小数字月份,`1-12`
	+ MM
	  两位数字月份，`01-12`
	+ MMM
	  缩写的月份文本，`Jan, Feb...`
	+ MMMM
	  完整的月份文本，`January, February...`

- d day of the month
	+ d
	  最小数字日，`1, 2...`
	+ dd
	  两位数字日，`01, 02...`

- H hour of the day
	+ H
	  最小数字小时，`0, 1...`
	+ HH
	  两位数字小时，`00, 01...`

- m minutes of the hour
	+ m
	  最小数字分钟，`0, 1...`
	+ mm
	  两位数字分钟，`00, 01...`

- s seconds of the minute
	+ s
	  最小数字秒数，`0, 1...`
	+ ss
	  两位数字秒数，`00, 01...`

- S 秒的最大精度(毫秒)，附加零
	+ S
	  十分之一秒
	+ SS
	  百分之一秒
	+ SSS
	  千分之一秒

- Z time zone offset or identity
	+ Z
	  时区偏移量结构为HHmm(如上海)，`+0800`
	+ ZZ
	  时区偏移量结构为HH:mm，`+08:00`
	+ ZZZ
	  时区身份(如上海)，`Asia/Shanghai`

- z time zone names. **Time zone names (z) cannot be parsed**

- w week of the year
	+ w
	  最小数字周数，`1, 2...`
	+ ww
	  两位数字周数，`01, 02...`

- D day of the year

- e day of the week(number)

- E day of the week(text)
	+ E, EE, EEE
	  星期几的缩写，`Mon, Tue, Wed, Thu, Fri, Sat, Sun`
	+ EEEE
	  星期几的全文，`Monday, Tuesday...`

对于非格式化的语法，你需要在值的周围放置单引号字符。如"yyyy-MM-dd’T'HH:mm:ss"

<br>

**tag_on_failure**
没有成功匹配时，将值附加到tag字段，默认值`["_dateparsefailure"]`

**target**
将匹配的timestamp存储到给定目标字段中。如果未提供，则默认更新事件的`@timestamp`字段

**timezone**
指定用于日期分析的时区标准ID，如`Asia/Shanghai`



<br>
<br/>


### dissect

dissect filter是一种拆分操作。与对整个字符串应用一个分隔符的常规拆分操作不同，此操作将一组分隔符应用于字符串值。dissect不使用正则表达式，所以速度非常快。
但是，如果文本结构因行而异，则Grok更适合。有一种混合的情况，dissect可用来结构可靠地重复部分，然后Grok用于余下的字段值，并具有更多的正则表达式可预测性和更少的整体工作。

一组字段和分隔符被称为dissection，它使用一组`%`来描述:

```
field: %{a}
delimiter: -
%{a} - %{b} - %{c}
```

<br>

**dissect filter配置项**

| Setting	| Input type | Required
| - | - | -
| convert_datatype | hash | No
| mapping | hash | No
| tag_on_failure | array | No

- `convert_datatype`
可以指定int, float数据类型转换。这些将在mapping发生后完成，如果没有mapping部分，请自由使用此设置。

```
filter {
  dissect {
    convert_datatype => {
      cpu => "float"
      code => "int"
    }
  }
}
```

- `mapping`
A hash of dissections of `field => value`
不要在值中使用具有转移的`\n`，它会被看做两个字符`\+n+`而不是实际的换行符。

```
filter {
  dissect {
    mapping => {
      # using an actual line break
      "message" => '"%{field1}" "%{field2}"
 "%{description}"'
      "description" => "%{field3} %{field4} %{field5}"
    }
  }
}
```

- `tag_on_failure`
dissection失败时，将值添加到tag字段。默认值为`["_dissectfailure"]`


<br>
<br/>


### geoip

geoip filter根据Maxmind GeoLite2数据库的数据，添加有关IP地址的地理位置信息。

此插件与GeoLite City Database数据库捆绑在一起。GeoLite2是免费的IP地址位置数据库，与MaxMind的GeoIP2数据库相比，不如其精确。
如果需要使用捆绑的DeoLite之外的数据库，可从MaxMind下载它: <https://dev.maxmind.com/geoip/geoip2/geolite2/>

如果GeoIP返回查找到的经度(latitude)和纬度(longitude)，则会创建`[geoip][location]`字段。

<br>

**Geoip Filter配置项**

| Setting | Inpu_type | Required
| - | - | -
| cache_size | number | No
| database | a valid filesystem path | No
| default_database_type | City or ASN | No
| fields | array | No
| source | string | Yes
| tag_on_failure | array | No
| target | string | No


- `cache_size`
默认值为1000。GeoIP查询的成本非常高昂。缓存设置的越高，项目在缓存中的可能性就越大，并且此filter运行的越快。但是，如果设置得太高，则会耗费太多内存。如果缓存已满，则无法添加更多记录。尝试使用此选项的不同值来为数据集找到最佳性能。
这个值必须大于0。

- `database`
地理数据库的文件路径，如果未指定，则默认为logstash自带的GeoLite2-City数据库。

- `default_database_type`
默认值是City。唯一可接受的值是City和ASN。

- `fields`
包含在事件中的geoip字段数组。可能的字段取决于数据库类型。

- `source`
包含要通过geoip映射的IP地址或主机名的字段。

- `tag_on_failure`
默认值为`["_geoip_lookup_failure"]`.

- `target`
默认值为`geoip`.指定Logstash应该存储的geoip数据的字段。



<br>
<br/>


### grok

Parse arbitrary text and structure it.
Grok是将非结构化日志数据解析为结构化和可查询的好方法。

它非常适用于syslog, apache or webserver logs, mysql logs以及通常为人类而不是计算机编写的任何日志格式。

默认情况下，Logstash ship附带了大约120种模式。它们在这: <https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns>
要grok某类日志文件的时候，可以先到上面的地址查看有无对应的模式。然后复制对应内容到`patterns_dir`下，再在filter中使用。
当然，你也可以自定义模式来匹配你的日志。在这测试: <http://grokdebug.herokuapp.com>

<br>

**Grok filter配置项**

| Setting | Input_type | Required
| - | - | -
| break_on_match | boolean | No
| keep_empty_captures | boolean | No
| match | hash | No
| named_captures_only | boolean | No
| overwrite | array | No
| pattern_definitions | hash | No
| patterns_dir | array | No
| patterns_files_glob | string | No
| tag_on_failure | array | No
| tag_on_timeout | string | No
| timeout_millis | number | No

- `break_on_match`
Break on first match. grok的第一个成功的匹配将导致filter结束。如果你想grok尝试所有的模式，请将其设置为`false`。默认值为`true`。

- `keep_empty_captures`
默认值为`false`。如果为`true`，则将空捕获保留为事件字段。

- `match`
field ⇒ value的散列匹配，默认值为`{}`

```
filter {
  grok { match => { "message" => [ "Duration: %{NUMBER:duration}", "Speed: %{NUMBER:speed}" ] } }
}
```

- `named_captures_only`
默认值为`true`。如果为`true`，只保存来自grok的命名捕获。

- `overwrite`
要覆盖的字段，这使你可覆盖已存在的字段中的值。

```
filter {
  grok {
    match => { "message" => "%{SYSLOGBASE} %{DATA:message}" }
    overwrite => [ "message" ]
  }
}
```

- `pattern_definitions`
默认值为`{}`
模式名称和模式元组的散列，用于定义当前过滤器要使用的自定义模式。匹配现用名称的模式将覆盖预先存在的定义。

- `patterns_dir`
默认值为`[]`
logstash默认提供了一堆模式，除非添加额外模式，否则不需要自定义模式。你可以使用此设置指向多个模式目录。grok将读取与`patterns_files_glob`匹配的目录汇总的所有文件，并假定它为模式文件。

```
patterns_dir => ["/opt/logstash/patterns", "/opt/logstash/extra_patterns"]
```

- `patterns_files_glob`
默认值为`"*"`
Glob模式，用于从patterns_dir目录中选择模式文件。

- `tag_on_failure`
默认值为`["_grokparsefailure"]`
匹配没有成功时，将值添加到tags字段。

- `tag_on_timeout`
默认值为`"_groktimeout"`
如果grok正则表达式超时，则应用此tag.

- `timeout_millis`
默认值为`30000`
尝试在这段时间后终止正则表达式。设置为0以禁用超时。

<br>

#### 基础知识

Grok工作方式，将文本模式组合成与你的日志模式相匹配的内容。

Grok模式的语法为 `%{SYNTAX:SEMANTIC}`

- `SYNTAX`, 文本匹配的模式的名称
- `SEMANTIC`, 正在匹配的文本的标识符

```
%{NUMBER:duration} %{IP:client}
```

你也可以将数据类型转换添加到Grok模式。默认情况下，所有的语义(semantic)都保存为字符串(strings)。
如果你想转换语义的数据类型，如将string转换为int。例如`%{NUMBER:num:int}`将num语义从string转换为integer。当前情况下，只支持转换为int和float.

```
日志格式
55.3.244.1 GET /index.html 15824 0.043


grok pattern
  grok {
    match => { "message" => "%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER:duration}" }
  }


grok filter之后的格式
client: 55.3.244.1
method: GET
request: /index.html
bytes: 15824
duration: 0.043
```

<br>

#### 正则表达式

Grok位于正则表达式之上，所以任何正则表达式在grok中都是有效的。
Regular Expression Library: <https://github.com/kkos/oniguruma/blob/master/doc/RE>


<br>

#### 示例

grok处理nginx/access.log日志:
首先针对nginx.conf中日志格式来决定如何写logstash pattern

```
mkdir /etc/logstash/patterns

vim nginx


NGINX_ACCESS %{IPORHOST:clientip} (?:-|(%{WORD}.%{WORD})) %{USER:ident} \[%{HTTPDATE:timestamp}\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})" %{NUMBER:response} (?:%{NUMBER:bytes}|-) %{QS:referrer} %{QS:agent} %{QS:forwarder}




grok {
  patterns_dir => "/etc/logstash/patterns"
  match => { "message" => %{NGINX_ACCESS}}
}
```


<br>


#### grok debugger

![grok debugger](/images/ELK/grok.png)

<br>

**grok-patterns**
这是grok官方写得patterns，当然，你也可以自己写。就像Nginx日志那样！

```
USERNAME [a-zA-Z0-9._-]+
USER %{USERNAME}
EMAILLOCALPART [a-zA-Z][a-zA-Z0-9_.+-=:]+
EMAILADDRESS %{EMAILLOCALPART}@%{HOSTNAME}
INT (?:[+-]?(?:[0-9]+))
BASE10NUM (?<![0-9.+-])(?>[+-]?(?:(?:[0-9]+(?:\.[0-9]+)?)|(?:\.[0-9]+)))
NUMBER (?:%{BASE10NUM})
BASE16NUM (?<![0-9A-Fa-f])(?:[+-]?(?:0x)?(?:[0-9A-Fa-f]+))
BASE16FLOAT \b(?<![0-9A-Fa-f.])(?:[+-]?(?:0x)?(?:(?:[0-9A-Fa-f]+(?:\.[0-9A-Fa-f]*)?)|(?:\.[0-9A-Fa-f]+)))\b

POSINT \b(?:[1-9][0-9]*)\b
NONNEGINT \b(?:[0-9]+)\b
WORD \b\w+\b
NOTSPACE \S+
SPACE \s*
DATA .*?
GREEDYDATA .*
QUOTEDSTRING (?>(?<!\\)(?>"(?>\\.|[^\\"]+)+"|""|(?>'(?>\\.|[^\\']+)+')|''|(?>`(?>\\.|[^\\`]+)+`)|``))
UUID [A-Fa-f0-9]{8}-(?:[A-Fa-f0-9]{4}-){3}[A-Fa-f0-9]{12}
# URN, allowing use of RFC 2141 section 2.3 reserved characters
URN urn:[0-9A-Za-z][0-9A-Za-z-]{0,31}:(?:%[0-9a-fA-F]{2}|[0-9A-Za-z()+,.:=@;$_!*'/?#-])+

# Networking
MAC (?:%{CISCOMAC}|%{WINDOWSMAC}|%{COMMONMAC})
CISCOMAC (?:(?:[A-Fa-f0-9]{4}\.){2}[A-Fa-f0-9]{4})
WINDOWSMAC (?:(?:[A-Fa-f0-9]{2}-){5}[A-Fa-f0-9]{2})
COMMONMAC (?:(?:[A-Fa-f0-9]{2}:){5}[A-Fa-f0-9]{2})
IPV6 ((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:)))(%.+)?
IPV4 (?<![0-9])(?:(?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5]))(?![0-9])
IP (?:%{IPV6}|%{IPV4})
HOSTNAME \b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.?|\b)
IPORHOST (?:%{IP}|%{HOSTNAME})
HOSTPORT %{IPORHOST}:%{POSINT}

# paths
PATH (?:%{UNIXPATH}|%{WINPATH})
UNIXPATH (/([\w_%!$@:.,+~-]+|\\.)*)+
TTY (?:/dev/(pts|tty([pq])?)(\w+)?/?(?:[0-9]+))
WINPATH (?>[A-Za-z]+:|\\)(?:\\[^\\?*]*)+
URIPROTO [A-Za-z]([A-Za-z0-9+\-.]+)+
URIHOST %{IPORHOST}(?::%{POSINT:port})?
# uripath comes loosely from RFC1738, but mostly from what Firefox
# doesn't turn into %XX
URIPATH (?:/[A-Za-z0-9$.+!*'(){},~:;=@#%&_\-]*)+
#URIPARAM \?(?:[A-Za-z0-9]+(?:=(?:[^&]*))?(?:&(?:[A-Za-z0-9]+(?:=(?:[^&]*))?)?)*)?
URIPARAM \?[A-Za-z0-9$.+!*'|(){},~@#%&/=:;_?\-\[\]<>]*
URIPATHPARAM %{URIPATH}(?:%{URIPARAM})?
URI %{URIPROTO}://(?:%{USER}(?::[^@]*)?@)?(?:%{URIHOST})?(?:%{URIPATHPARAM})?

# Months: January, Feb, 3, 03, 12, December
MONTH \b(?:[Jj]an(?:uary|uar)?|[Ff]eb(?:ruary|ruar)?|[Mm](?:a|ä)?r(?:ch|z)?|[Aa]pr(?:il)?|[Mm]a(?:y|i)?|[Jj]un(?:e|i)?|[Jj]ul(?:y)?|[Aa]ug(?:ust)?|[Ss]ep(?:tember)?|[Oo](?:c|k)?t(?:ober)?|[Nn]ov(?:ember)?|[Dd]e(?:c|z)(?:ember)?)\b
MONTHNUM (?:0?[1-9]|1[0-2])
MONTHNUM2 (?:0[1-9]|1[0-2])
MONTHDAY (?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])

# Days: Monday, Tue, Thu, etc...
DAY (?:Mon(?:day)?|Tue(?:sday)?|Wed(?:nesday)?|Thu(?:rsday)?|Fri(?:day)?|Sat(?:urday)?|Sun(?:day)?)

# Years?
YEAR (?>\d\d){1,2}
HOUR (?:2[0123]|[01]?[0-9])
MINUTE (?:[0-5][0-9])
# '60' is a leap second in most time standards and thus is valid.
SECOND (?:(?:[0-5]?[0-9]|60)(?:[:.,][0-9]+)?)
TIME (?!<[0-9])%{HOUR}:%{MINUTE}(?::%{SECOND})(?![0-9])
# datestamp is YYYY/MM/DD-HH:MM:SS.UUUU (or something like it)
DATE_US %{MONTHNUM}[/-]%{MONTHDAY}[/-]%{YEAR}
DATE_EU %{MONTHDAY}[./-]%{MONTHNUM}[./-]%{YEAR}
ISO8601_TIMEZONE (?:Z|[+-]%{HOUR}(?::?%{MINUTE}))
ISO8601_SECOND (?:%{SECOND}|60)
TIMESTAMP_ISO8601 %{YEAR}-%{MONTHNUM}-%{MONTHDAY}[T ]%{HOUR}:?%{MINUTE}(?::?%{SECOND})?%{ISO8601_TIMEZONE}?
DATE %{DATE_US}|%{DATE_EU}
DATESTAMP %{DATE}[- ]%{TIME}
TZ (?:[APMCE][SD]T|UTC)
DATESTAMP_RFC822 %{DAY} %{MONTH} %{MONTHDAY} %{YEAR} %{TIME} %{TZ}
DATESTAMP_RFC2822 %{DAY}, %{MONTHDAY} %{MONTH} %{YEAR} %{TIME} %{ISO8601_TIMEZONE}
DATESTAMP_OTHER %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{TZ} %{YEAR}
DATESTAMP_EVENTLOG %{YEAR}%{MONTHNUM2}%{MONTHDAY}%{HOUR}%{MINUTE}%{SECOND}

# Syslog Dates: Month Day HH:MM:SS
SYSLOGTIMESTAMP %{MONTH} +%{MONTHDAY} %{TIME}
PROG [\x21-\x5a\x5c\x5e-\x7e]+
SYSLOGPROG %{PROG:program}(?:\[%{POSINT:pid}\])?
SYSLOGHOST %{IPORHOST}
SYSLOGFACILITY <%{NONNEGINT:facility}.%{NONNEGINT:priority}>
HTTPDATE %{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME} %{INT}

# Shortcuts
QS %{QUOTEDSTRING}

# Log formats
SYSLOGBASE %{SYSLOGTIMESTAMP:timestamp} (?:%{SYSLOGFACILITY} )?%{SYSLOGHOST:logsource} %{SYSLOGPROG}:

# Log Levels
LOGLEVEL ([Aa]lert|ALERT|[Tt]race|TRACE|[Dd]ebug|DEBUG|[Nn]otice|NOTICE|[Ii]nfo|INFO|[Ww]arn?(?:ing)?|WARN?(?:ING)?|[Ee]rr?(?:or)?|ERR?(?:OR)?|[Cc]rit?(?:ical)?|CRIT?(?:ICAL)?|[Ff]atal|FATAL|[Ss]evere|SEVERE|EMERG(?:ENCY)?|[Ee]merg(?:ency)?)

```



<br>
<br/>


### json

这是一个json解析过滤器。

<br>

**Json Filter配置项**

| Setting | Input_type | Required
| - | - | -
| skip_on_invalid_json | boolean | No
| source | string | Yes
| tag_on_failure | array | No
| target | string | No


- `skip_on_invalid_json`
默认值是false.允许跳过无效json上的过滤器。

- `source`
json filter的配置
如，从message字段中解析json

```
filter {
  json {
    source => "message"
  }
}
```

- `target`
定义放置解析数据的目标字段。如果目标字段已存在，则它会被覆盖。

```
filter {
  json {
    target => "doc"
  }
}
```


<br>
<br/>


### kv

此过滤器有助于自动解析`key=value`类型的消息。
这对于postfix, iptables和倾向于key=value语法类型的日志非常有用。

```
#before
ip=1.2.3.4 error=REFUSED


filter {
  kv {}
}


#after
ip: 1.2.3.4
error: REFUSED
```

<br>

**kv filter配置项**

| Setting | Input_type | Required
| - | - | -
| allow_duplicate_values | boolean | No
| default_keys | hash | No
| exclude_keys | array | No
| field_split | string | No
| include_brackets | boolean | No
| include_keys | array | No
| prefix | string | No
| recursive | boolean | No
| remove_char_key | string | No
| remove_char_value | string | No
| source | string | No
| target | string | No
| transform_key | string, | one of ["lowercase", "uppercase", "capitalize"] | No
| transform_value | string, | one of ["lowercase", "uppercase", "capitalize"] | No
| trim_key | string | No
| trim_value | string | No
| value_split | string | No


- `allow_duplicate_values`
默认值为true.用于删除重复 键/值对的布尔选项。

- `default_keys`
默认值为{}.一个散列，用于指定在解析源字段中不存在的键时应添加到事件中的默认值及其值。

- `exclude_keys`
默认值为[].一个数组，用于指定不应添加到事件中的解析键。默认情况下，没有键被排除。

- `field_split`
默认值为`" "`.用作解析出键值对后的单字符字段分隔符的字符串。

```
#栗子
name=zhang21&age=25&email=ab123@gamil.com

filter {
  kv {
    field_split => "&"
  }
}
```

- `field_split_pattern`
一个正则表达式，用作字段分隔符来解析键值对。用于定义多字符字段分隔符。
它优先于`field_split`选项。

```
#栗子
k1=v1:k2=v2:::k3=v3::k4=v4

filter {
  kv {
    field_split_pattern => ":+"
  }
}

```

- `include_brackets`
默认值为true.一个布尔值，指定是否将 方括号[square bracket]，尖括号<angle bracket>和括号(bracket) 视为的包装器(wrapper)，是否应该从值中删除。

```
#栗子
one=(o n e) two=[t w o] three=<t h r e e>

filter {
  kv {
    include_brackets => tree
  }
}

#after
one: o n e
two: t w o
three: t h r e e
```

- `include_keys`
默认值为[].一个数字，用于指定应该添加到解析的键。默认情况下，所有的键都会被添加。

- `prefix`
默认值为空。预先添加到所有提取的键的字符串。

- `recursive`
默认值为false.一个布尔值，执行是否向下提取值并递归获取更多的键值对。

- `remove_char_key`
要从键中移除的字符串。

- `remove_char_value`
要从值中移除的字符串。

- `source`
默认值为message.要在其上执行key=value搜索的字段。

- `target`
将所有键值对放入的容器的名称。

- `transform_key`
将键转换为大写，小写。

- `transform_value`
将值转换为大写，小写

- `trim_key`
从键中修建的字符串。如果键包含在括号中或以空格开头，这很有用。

- `trim_value`
从值中修建的字符串。如果你的值包含在括号中或以逗号结尾。这很有用。

- `value_split`
默认值为`=`.一个非空字符串，用作解析出键值对的单字符分隔符。

- `value_split_pattern`
用作值分隔符来解析出键值对的正则表达式。优先级高于`value_split`。


<br>
<br/>


### metrics

metrics filter用于聚合度量(aggregating metrics).

```
#计算每种http响应吗

filter {
  metrics {
    meter => [ "http_%{response}" ]
    add_tag => "metric"
  }
}
```

<br>

**metrics filter配置项**

| Setting | Input_type | Required
| - | - | -
| clear_interval | number | No
| flush_interval | number | No
| ignore_older_than | number | No
| meter | array | No
| percentiles | array | No
| rates | array | No
| timer | hash | No

- `clear_interval`
默认值为`-1`.清理间隔，所有的计数器都被重置。

- `flush_interval`
默认值为`5`.刷新间隔，当metrics事件被创建时。此值必须是5的倍数。

- `ignore_older_than`
默认值为`0`.不要跟着`@timestamp`超过某个秒数的事件。

- `meter`
语法: `meter => [ "name of metric", "name of metric" ]`

- `percentiles`
默认值为`percentiles`.计时器值应该测量和发出的百分位数。

- `rates`
默认值为`[1, 5, 15]`.应该按分钟测量的比率。

- `timer`
语法: `timer => [ "name of metric", "%{time_value}" ]`

<br>

**meter values**
`meter => "something"`, 会收到如下字段:

- "[thing][count]" - the total count of events
- "[thing][rate_1m]" - the per-second event rate in a 1-minute sliding window
- "[thing][rate_5m]" - the per-second event rate in a 5-minute sliding window
- "[thing][rate_15m]" - the per-second event rate in a 15-minute sliding window


**timer values**
`timer => { "thing" => "%{duration}"}`, 会收到如下字段:

- "[thing][count]" - the total count of events
- "[thing][rate_1m]" - the per-second average value in a 1-minute sliding window
- "[thing][rate_5m]" - the per-second average value in a 5-minute sliding window
- "[thing][rate_15m]" - the per-second average value in a 15-minute sliding window
- "[thing][min]" - the minimum value seen for this metric
- "[thing][max]" - the maximum value seen for this metric
- "[thing][stddev]" - the standard deviation for this metric
- "[thing][mean]" - the mean for this metric
- "[thing][pXX]" - the XXth percentile for this metric (see percentiles)



<br>
<br/>



### mutate

mutate filter允许你在字段上执行常规突变。你可以重命名，删除，替换和修改事件中的字段。

<br>

**mutate filter配置项**

| Setting | Input_type | Required
| - | - | -
| convert | hash | No
| copy | hash | No
| gsub | array | No
| join | hash | No
| lowercase | array | No
| merge | hash | No
| coerce | hash | No
| rename | hash | No
| replace | hash | No
| split | hash | No
| strip | array | No
| update | hash | No
| uppercase | array | No
| capitalize | array | No

- `convert`
将字段的值转换为其它类型，如将string转换为int.如果只为数组，则所有成员都将转换；如果是散列，则不处理。

- `copy`
将现有字段复制到另一个字段(会覆盖)。

- `gsub`
将正则表达式与字段值进行匹配，并用替换字符替换所有匹配项。
只支持string或string array.

```
filter {
  mutate {
    gsub => [
	  "field1", "value", "replacement string",
	]
  }
}
```

- `join`
加入一个带分隔符的数组。对非数组字段没有任何作用。

- `lowercase`
将字符串转换为小写

- `merge`
合并数组或散列的两个字段。字符串字段将被自动转换为数组。

```
filter {
  mutate {
    merge => { "dest_field" => "added_field"}
  }
}
```

- `coerce`
为已存在但为空的字段设置默认值。

- `rename`
重命名一个或多个字段。

- `replace`
用新值替换一个字段。新值可以包含`%{foo}`字符串，以帮助你从事件的其它部分创建新值。

```
filter {
  mutate {
    replace => {
	  "message" => "%{source_host}: My new message"
	}
  }
}
```

- `split`
使用分隔符将字段拆分为数组。只适用于字符串字段。

- `strip`
从字段剥离空白符。

- `update`
用新值更新现有字段。



<br>
<br/>



### xml

XML filter.获取包含XML的字段并将其展开为实际的数据结构。

**XML Filter配置项**

| Setting | Input_type | Required
| - | - | -
| force_array | boolean | No
| force_content | boolean | No
| namespaces | hash | No
| remove_namespaces | boolean | No
| source | string | Yes
| store_xml | boolean | No
| suppress_empty | boolean | No
| target | string | No
| xpath | hash | No

- `force_array`
默认值为true.过滤器强制单个元素为数组。将其设置为false防止在数组中存储单个元素。

- `force_content`
默认值为false.过滤器将以不同于标签内的内容的方式展开属性。

- `namespace`
默认值为`{}`.这允许配置所有命名空间声明来解析XML文档。

```
filter {
  xml {
    namespaces => {
      "xsl" => "http://www.w3.org/1999/XSL/Transform"
      "xhtml" => "http://www.w3.org/1999/xhtml"
    }
  }
}
```

- `remove_namespaces`
从文档中的所有节点中删除所有命名空间。

- `source`

- `store_xml`
默认为true.过滤器会将整个解析的XML存储在目标字段中。

- `suppress_empty`
默认值为true.默认情况下，如果元素为空，这不输出。如果设置为false,则空元素将产生一个空的散列对象。

- `target`
定义放置数据的目标。




<br>
<br/>



## 条件判断

使用条件判断决定filter和output处理特定的事件。

Logstash条件类似于编程语言，条件语句，可以嵌套：

- `if`
- `else if`
- `else`

<br>

比较操作：

- `==`
- `!=`
- `<`
- `>`
- `<=`
- `>=`
- `=~` 匹配正则
- `!~` 不匹配正则
- `in` 包含
- `not in` 不包含

<br>

布尔操作：

- `and`
- `or`
- `nand`
- `xor`

<br>

一元运算符：

- `!` 取反
- `()` 复合表达式

<br>

栗子：

```
output {
  if [path] == "/var/nginx/access.log" {
    elasticsearch {
	  hosts =>
	  user =>
	  password =>
	  index => "nginx-access-%{+YYYY.MM.dd}"
	}
  }
  else if [path] == "/var/nginx/error.log" {
    elasticsearch {
	  hosts =>
	  user =>
	  password =>
	  index => "nginx-error-%{+YYYY.MM.dd}"
	}
  }
  else {
  }
}
```





<br>
<br/>

---

<br>


# Filebeat文档


<br>


## 概述


filebeat是一个beat，它基于libbeat框架。

Filebeat是一个本地文件的日志数据搬运(shipper)。作为Agent安装，filebeat监视日志目录或指定的日志文件，并将它们转发给Elasticsearch或logstash进行索引。
启动filebeat时，它会启动一个或多个**prospectors**(勘探者)，查看为日志指定的本地路径。对于prospectors所在的每个日志文件，filebeat启动harvester。每个harvester为新内容读取单一日志文件，并将新日志发送到filebeat配置的输出。

<br>

![filebeat流程图](/images/ELK/filebeat.png)


<br>
<br/>


## 入门


开始filebeat前，请确保安装和配置了如下产品：

- Elasticsearch(存储和索引数据)
- Kibana(UI)
- Logstash(可选)


<br>


### 配置


filebeat module为常用日志格式提供了入门体验。

![](/images/ELK/filebeat002.png)

<br>

```
vim /etc/filebeat/filebeat.yml


filebeat.prospectors:
 -type: log
   enabled: true
   paths:
     - /var/log/*.log

output.elasticsearch:
  hosts: [ "ip:9200" ]
  #username
  #password

setup.kibana:
  host: "localhost:5601"
  #username
  #password

```


<br>


### 配置filebeat使用logstash


```
vim /etc/filebeat/filebeat.yml

output.logstash:
  hosts: [ "127.0.0.1:5044" ]

#logstash需要配置监听beats

```


<br>
<br/>


### 在Elasticsearch中载入索引模板


在Elasticsearch中，索引模板用于定义设置(setting)和映射(mapping)，以确定如何分析字段(fields)。

filebeat推荐的索引模板文件有filebeat软件包安装。在成功连接到Elasticsearch后，它会默认自动载入索引模板(`fields.yml`)。如果模板存在，它不会覆盖除，除非你配置要覆盖。
通过修改配置文件，你也可以禁用自动载入模板，或者载入你自己的模板。

<br>

**配置模板载入**

```
vim /etc/filebeat/filebeat.yml


setup.template.name: "template-name"
setup.template.fields: "/path/xxx/xxx.yml"

#强制覆盖已存在模板
setup.template.overwrite: true

#关闭自动载入模板
setup.template.enabled: false

```

<br>

**修改索引名**

- filebeat的默认索引名为 `filebeat-<version>-yyyy.MM.dd`
- 在output.elasticsearch设置选项
- 你指定的索引名称应该包含索引的根名、索引版本和日期信息

```
output.elasticsearch.index: "customname-%{[version]}-%{+yyyy.MM.dd}"
setup.template.name: "customname"
setup.template.pattern: "customname-*"
setup.dashboards.index: "customname-*"

```

<br>

**手动载入模板**

```
filebeat setup --template

```

<br>

**强制Kibana查看最新文件**

```
curl -XDELETE 'http://localhost:9200/filebeat-*'

```


<br>


### 设置Kibana面板


Filebeat附带了实例的Kibana dashboards, visualization和可视化搜索。
在使用仪表板前，你需要创建索引`filebeat-*`，并将仪表板加载到Kibana中。你可使用`setup`命令或配置文件加载它。


<br>


### 启动Filebeat


```
systemctl start filebeat

#前台启动并查看相关信息
filebeat -e -c filebeat.yml

```


<br>


### 查看示例Kibana仪表板


访问你的kibana web端(`localhost:5601`)，可用Nginx做反向代理，再加上域名解析。


<br>


### 快速开始常见日志格式


filebeat提供了一套预构建模块，可使用它快速实施和部署日志监视方案。

先决条件：

- 安装和配置`Elastic Stack`
- 安装`filebeat`
- 安装`Ingest Node GeoIP`和`User Agent plugins`
- 验证Elasticsearch和Kibana能从filebeat接收数据

```
elasticsearch-plugin install ingest-geoip
elasticsearch-plugin install ingest-user-agent

```

<br>

**运行filebeat模块**

```
#启用模块
filebeat modules enable nginx system

#配置path
cd /etc/filebeat/modules.d

vim nginx.yml
vim system.yml

```

最后就可以在Kibana中可视化查看日志。

查看dashboard时，遇到一个错误: `Could not locate that index-pattern (id: filebeat-*)`

解决办法：

```
#重新载入索引模板
filebeat setup

```



<br>
<br>


## output


我们可根据系统的负载情况将Filebeat的output到合适的地方，output只能有一个！
如果有时候系统负载过高的话，可以考虑output到Redis或Elasticsearch。

redis和logstash都还需要logstash的pipeline转交给Elasticsearch，但你可以filter。而直接使用Elasticsearch便不能过滤。

- Logstash
- Elasticsearch
- Redis

<br>

```
vim /etc/filebeat/filebeat.yml


#找到output


#redis
output.redis:
  hosts: "localhost"
  port: 6379
  key: "filebeat"
  #自定义key-name
  #password:
  #db:
  #data_type: 'list'



#logstash
output.logstash:
  hosts: [ "localhost:5044" ]



#Elasticsearch
elasticsearch.output:
  hosts: [ "localhost:9200" ]
  #username:
  #name:


==================

#redis对应的pipeline
vim /etc/logstash/conf.d/redis-pipeline.conf


input {
  redis {
    data_type => "list"
    key => "filebeat"
    host => "localhost"
    port => 6379
	#password =>
	#db =>
  }
}
#filter{ }
output {
  elasticsearch {
  	hosts => [ "localhost:9200" ]
	#user
	#password
	index => "filebeat-%{+YYYY.MM.dd}"
  }
}

```


<br>
<br/>


### 定义索引

为filebeat定义index:

```
vim /etc/filebeat/filebeat.yml

# Optional index name. The default is "filebeat" plus date
# and generates [filebeat-]YYYY.MM.DD keys.
# In case you modify this pattern you must update setup.template.name and setup.template.pattern accordingly.
#index: "filebeat-%{[beat.version]}-%{+yyyy.MM.dd}"
#写到事件中的索引名，默认 "filebeat-%{[beat.version]}-%{+yyyy.MM.dd}"
#如果更改此设置，还需要配置setup.template.name和setup.template.pattern选项
#如果使用的是预先构建的kibana dashboard，还需要配置setup.dashboards.index选项

#定义索引
output.elasticsearch:
  hosts: ["10.0.1.8:9002", "10.0.1.7:9002", "10.0.1.9:9002"]
  loadbalance: true
  username: "elastic"
  password: xxx
  index: "filebeat-publish-%{+yyyy.MM.dd}"


#添加这几项
setup.template.name: "filebeat"
setup.template.pattern: "filebeat-*"
setup.template.fields: "fields.yml"
setup.template.overwrite: false
```


<br/>
<br/>


## 配置


RPM安装的配置文件默认是`/etc/filebeat/filebeat.yml`，还有一个完整的示例配置文件`/etc/filebeat/filebeat.reference.yml`，显示了所有未弃用的选项。配置文件使用YAML语法。


<br/>


### 指定运行module

Specify which modules to run


Filebeat module提供了一种快速处理常见日志格式的方法。它包含默认配置。

有几种不同方法来启用modules:

- 配置`modules.d`目录
- `filebeat命令启动`
- 配置`filebeat.yml`文件

```
#modules.d
filebeat modules list
filebeat modules enable nginx
#filebeat modules disable nginx


#filebeat命令
./filebeat -e --modules nginx


#filebeat.yml
filebeat.modules:
- module: nginx
- module: system
```


<br/>
<br/>


#### 指定变量设置

Specify variable settings

每个模块和文件集合都有变量，你可以设置这些变量来更改木块的默认行为。

```
- module: nginx
  access:
    var.path: ["/var/log/nginx/access.log*"]


#or
filebeat -M "nginx.access.var.paths=[/var/log/access.log*]"
filebeat --modules nginx -M "nginx.access.var.paths=[/var/log/nginx/access.log*]" -M "nginx.error.var.paths=[/var/log/nginx/error.log*]"
```


<br/>
<br/>


#### 高级设置

在幕后，每个木块都会启动filebeat input。高级用户可以添加或覆盖任何input设置。

```
- module: nginx
  access:
    input:
	  close_eof: true


#or
filebeat -M "nginx.access.input.close_eof=true"

filebeat --modules nginx -M "nginx.access.input.close_eof=true"

```


<br/>
<br/>


### 读取动态文件名


filbeat配置文件虽然可以将索引设置为: `indexname-%{+yyyy.MM.dd}` 的日志格式，但这个是发送给ES的，ES可以处理此配置，但filebeat是无法直接处理的，它会把它当做普通字符。
假如我要读取一个按日期取名的日志文件，如`service_20180808.log`，filebeat配置文件中是无法直接配置和处理。
后来想到，可以用`sh`写一个脚本来做此操作。

```sh
yesterday=`/bin/date +%Y%m%d --date='-1days'`
today=`/bin/date +%Y%m%d`

/bin/sed -i "s/service_err_${yesterday}/service_err_${today}/" /etc/filebeat/filebeat.yml
/bin/filebeat test config

if [ $? -eq 0 ] ;then
    /bin/systemctl restart filebeat
else
   exit 0
fi

```


<br/>
<br/>



###   input

**DEPRECATED: prospectors are deprecated, Use `inputs` instead. Will be removed in version: 7.0.0**
要手动配置filebeat(而不是使用modules)，需要在`filebeat.yml`的`filebeat.inputs`部分指定输入列表(一个YAML 数据)。你可指定多个输入，并可多次指定相同的输入类型。

**input types**

- log
- stdin
- redis
- udp
- docker
- tcp
- syslog

<br>

input 通用选项：

```
#启用/禁用inputs
enabled


#增加tags字段
tags


#向输出添加其他信息
fields

filebeat.inputs:
- type: log
  fields:
    author: zhang21


#自定义字段存储为输出文档中的顶级字段，而不是在字段子字典下分组。如果与filebeat冲突，则会覆盖源字段
fields_under_root


#应用于inputs的处理器列表
#已被弃用
processors


#为input生成的事件设置ingest node pipeline id
pipeline
```


<br/>
<br/>


#### log

使用log input从日志文件中读取行。

```yaml
filebeat.inputs:
- type: log
  paths:
    - /var/log/messages
    - /var/log/*.log
```

你可以将其它配置设置(fields, include_lines, exclude_lines, mutiline)应用于从日志文件获取的行。这里指定的选项将应用于input的所有文件。
将不同的配置应用于不同的文件，需要定义多个input sections:

```yaml
filebeat.inputs:
- type: log
  paths:
    - /var/log/1.log
    - /var/log/2.log
- type: log
  paths:
    - "/var/log/appache/*"
  fields:
    apache: true
  fields_under_root: true
```

<br>

**log input 配置项**

```yaml
paths
#将读取的基于全局路径的列表


recursive_glob.enabled
#true允许扩展为递归模式


encoding
#读取数据的文件编码


exclude_lines
#正则表达式列表，用于匹配你希望filebeat排除的行

filebeat.inputs:
- type: log
  ...
  exclude_lines: ['^debug']


include_lines
#正则表达式列表，用于匹配你希望filebeat包含的行。
#如果`exclude_lines`和`include_lines`都定义了，filebeat首先执行`include_lines`，之后才执行`exclude_lines`。

filebeat.inputs:
- type: log
  ...
  include_lines: ['^ERR', '^WARN']


harvester_buffer_size
#每个收集器在获取文件时使用的buffer大小，默认 16 384Byte。


max_bytes
#单日志消息可以具有的最大字节数。默认 10MB


json
#此选项使filebeat解码日志结构为json消息。filebeat逐行处理日志，因此每一行要有json对象才有效。

json.keys_under_root
json.overwrite_keys
json.add_error_key
json.message_key
json.ignore_decoding_error


mutiline
#控制filebeat如果处理跨越多行的日志消息。


exclude_files
#正则表达式列表，用于匹配你希望filebeat忽略的文件。默认无。

filebeat.inputs:
- type: log
  ...
  exclude_files: ['\.gz$']


ignore_older
#如果启用此选项，filebeat将忽略在指定的事件跨度之前修改的所有文件。

close_*
#用于在某个标准或时间后关闭收集器。


close_inactive
#如果文件尚未在指定的时间内收获，则filebeat将关闭文件句柄。


close_renamed
#filebeat会在重命名文件时关闭文件处理程序，请注意日志轮询。


close_removed
#删除文件后，filebeat会关闭收集器。


close_eof
#一旦到达文件末尾，filebeat就会关闭文件。


clean_*
#用于清理注册表文件中的状态条目。

clean_inactive
#filebeat在指定的不活动事件段过去后删除文件的状态。


clean_removed
#如果在最后一个已知名称下无法在磁盘上找到文件，则filebeat会清除注册表中的文件。


scan_frequency
#filebeat检查指定路径文件的频率。官方不建议将此值设置为小于1s。默认 10s。


tail_files
#filebeat开始在每个文件的末尾而不是开头读取新文件。默认 false


symlinks
#允许filebeat收集符号链接，它读取符号链接的原始文件。
由于此选项可能会导致数据丢失，默认 disabled


backoff
#指定filebeat如何积极地抓取打开的文件以进行更新。


max_backoff
#在到达eof后再次检查文件之间filebeat等待的最长时间。


backoff_factor
#指定等待时间增加的速度。


harvester_limit
#限制一个input并行启动的收集器数量。

```


<br/>
<br/>


#### stdin

使用`stdin input`从标准输入读取事件。**此输入不可与其它输入类型同时运行。**

```yaml
filebeat.inputs:
- type: stdin
```

stdin input 配置项：

```yaml
encoding

exclude_lines
include_lines

harvester_buffer_size

max_bytes

json

multiline
```


<br/>
<br/>


#### udp

使用 udp input通过udp读取事件。

```yaml
filebeat.inputs:
- type: udp
  max_message_size: 10KB
  host: "localhost:5678"
```

udp input 配置项：

```yaml
#通过udp接收的最大消息大小，默认 10KB
max_message_size


#udp host
host
```


<br/>
<br/>


#### tcp

使用 tcp input 通过tcp读取事件。

```yaml
filebeat.inputs:
- type: tcp
    max_message_size: 10MB
	host: "localhost:5679"
```

tcp input 配置项：

```yaml
max_message_size
#通过tcp接收的最大消息大小， 默认 10MB


#host and tcp port
host


#指定用于拆分事件的字符，默认 \n
line_delimiter


#关闭连接前不活动的秒数， 默认 300s
timeout
```


<br/>
<br/>


#### docker

使用docker input从docker container读取日志。

```yaml
filebeat.inputs:
- type: docke
  containers:
    path: "/var/lib/docker/containers"
	stream: "all"
	ids:
      - 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
#必须填写容器ID
```

docker input 配置项：

```yaml
container.ids


#默认 /var/lib/docker/containers
container.path


#从指定stream读取: all/stdout/stderr，默认 all
container.stream


encoding

exclude_line

include_line

harvester_buffer_size

max_bytes

json

multiline

exclude_files

ignore_older

close_*

close_inactive

close_renamed

close_removed

close_eof

close_timeout

clean_*

clean_inactive

clean_removed

sacn_frequency

tail_files

symlinks

backoff

max_backoff

backoff_factor

harvester_limit

```


<br/>
<br/>


#### syslog

使用 syslog input通过tcp/udp/读取事件。

修改syslog配置：

```
vim /etc/rsyslog.d/filebeat.conf

*.* @127.0.0.1:5678

#重启服务
systemctl restart rsyslog
```

```yaml
filebeat.inputs:
- type: syslog
  protocol.udp:
    host: "localhost:5678"
	max_message_size: 100KB


#定义索引
setup.template.name: "filebeat"
setup.template.pattern: "filebeat-*"
setup.template.fields: "fields.yml"
setup.template.overwrite: false
out.elastisearch：
  hosts: ["localhost:9200"]
  index: "syslog-%{+yyyy.MM.dd}"
```

它的配置项就是tcp/udp的配置项。

之后查看主机端口情况：

```
netstat -nltup | grep 5678

udp        0      0 127.0.0.1:5678          0.0.0.0:*                           12434/filebeat

```


<br/>
<br/>


### output

你可以通过在`filebet.yml`配置文件的`output`部分设置选项来配置filebeat以特定方式输出。只能定义一个输出。

filebeat支持如下输出：

- Elasticsearch
- Logstash
- Kafka
- Redis
- File
- Console


<br/>


#### elasticsearch

filebeat使用es http api将事务发送到es。

```
output.elasticsearch:
  hosts: ["https://localhost:9200"]
  username: "filebeat_internal"
  password: "YOUR_PASSWORD"
  index: "filebeat-%{[beat.version]}-%{+yyyy.MM.dd}"
  #ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]
  #ssl.certificate: "/etc/pki/client/cert.pem"
  #ssl.key: "/etc/pki/client/cert.key"
```

配置项：

```yaml
#启用/禁用output，默认 true
enabled


hosts
#["hsot1:port1", "host2:port2", "host3:port3"]
username
#建议为filebeat创建一个专门的用户用于发送事件，而不是使用es的用户
password

compression_level
#gzip压缩等级, 0-9，默认 0


worker
#每个配置主机向es发布事件的worker数，默认 1


parameters
#http 参数字典


protocol
#网络协议, http/https


path
#http api调用前面的http路径前缀


headers
#定义headers


proxy_url
#代理的url


index
#写到事件中的索引名，默认 "filebeat-%{[beat.version]}-%{+yyyy.MM.dd}"
#如果更改此设置，还需要配置setup.template.name和setup.template.pattern选项
#如果使用的是预先构建的kibana dashboard，还需要配置setup.dashboards.index选项


indices
#支持条件的索引选择器规则数组，基于格式字符串的字段访问和名称映射。
indices.index: 要使用的索引格式字符串
indices.mapping： 映射
indices.default： 如果映射找不到匹配项的默认字符串值
indices.when： 成功的条件才执行当前规则

output.elasticsearch:
  hosts: ["http://localhost:9200"]
  index: "logs-%{[beat.version]}-%{+yyyy.MM.dd}"
  indices:
    - index: "critical-%{[beat.version]}-%{+yyyy.MM.dd}"
      when.contains:
        message: "CRITICAL"
    - index: "error-%{[beat.version]}-%{+yyyy.MM.dd}"
      when.contains:
        message: "ERR"


pipeline
#与indices类似，管道选择器配置数组

filebeat.inputs:
- type: log
  paths: ["/var/log/app/normal/*.log"]
  fields:
    type: "normal"
- type: log
  paths: ["/var/log/app/critical/*.log"]
  fields:
    type: "critical"

output.elasticsearch:
  hosts: ["http://localhost:9200"]
  index: "filebeat-%{[beat.version]}-%{+yyyy.MM.dd}"
  pipelines:
    - pipeline: critical_pipeline
      when.equals:
        fields.type: "critical"
    - pipeline: normal_pipeline
      when.equals:
        fields.type: "normal"


max_retries

bulk_max_size
#单个es批量挨批索引请求中要批量处理的最大事件数，默认 50

backoff.init
#在网络错误之后尝试重连到es之前等待的秒数，默认 1s

backoff.max
#在网络错误后尝试连接到es之前等待的最大秒数，默认 60s

timeout
#超时时间

ssl

```


<br/>
<br/>


#### logstash


<br/>
<br/>


#### kafka


<br/>
<br/>


#### redis

redis output将事件插入redis list或redis channel。

```yaml
output.redis:
  hosts: "localhost"
  port: 6379
  key: "filebeat"
  #自定义key-name
  #password:
  #db:
  #data_type: 'list'
```

配置项：

```yaml
#启用/禁用output
enabled


hosts
port
#可将端口写在hosts里，默认6379
username
password
db
key
datatype
#默认 list
codec


keys
keys.key
keys.mapping
keys.default
keys.when

output.redis:
  hosts: ["localhost"]
  key: "default_list"
  keys:
    - key: "info_list"   # send to info_list if `message` field contains INFO
      when.contains:
        message: "INFO"
    - key: "debug_list"  # send to debug_list if `message` field contains DEBUG
      when.contains:
        message: "DEBUG"
    - key: "%{[fields.list]}"
      mapping:
        "http": "frontend_list"
        "nginx": "frontend_list"
        "mysql": "backend_list"

loadbalance
#如果配置了多个主机，则输出插件会将已发布的事件负载均衡到所有redis主机上

timeout
max_retries
bulk_max_size
ssl
proxy_url
proxy_use_local_resolver
```


<br/>
<br/>


#### file

file output将事务转储到文件中，每个事务都是json格式。

```yaml
output.file:
  path: "/tmp/filebeat"
  filename: filebeat
```

配置项：

```yaml
enabled

path

filename

rotate_every_kb
#默认 10 240KB

number_of_files
#路径下要保存的最大文件数

permissions
#创建的文件权限， 默认 0600

codec

```


<br/>
<br/>


#### console

console output将事件以json格式输出到标准输出。

```yaml
output.console:
  pretty: true
```

配置项：

```yaml
pretty
#美化输出， 默认 false

codec

enabled

bulk_max_size

```


<br/>
<br/>


### loadbalance

filebeat提供配置项，用于将事件发送到多个主机时微调负载均衡。
loadbalance对redis, logstash, es output可用。

```yaml
output.logstash:
  hosts: ["localhost:5044", "localhost:5045"]
  loadbalance: true
```






















<br>
<br/>

---

<br>



# Kibana文档


Kibana是一个开源分析和可视化平台，旨在与Elasticsearch合作。你可使用Kibana来检索(search)，查看(view)存储在Elasticsearch索引中的数据并与其进行交互(interact)。你可以很轻松地执行高级数据分析，并在各种图表、表格和地图中可视化你的数据。
Kibana可以很容易地理解大量的数据。基于浏览器的接口能够快速创建和分享动态仪表盘，实时显示Elasticsearch查询的变化。


<br>
<br/>


## 入门


在开始前，请确保已安装Kibana并与Elasticsearch建立了连接。


<br>
<br/>


### 载入示例数据


本节依赖如下示例数据：

- shakespeare.json: <https://download.elastic.co/demos/kibana/gettingstarted/shakespeare_6.0.json>
- accounts.zip: <https://download.elastic.co/demos/kibana/gettingstarted/accounts.zip>
	+ `uzip accounts.zip`
- logs.jsonl.gz: <https://download.elastic.co/demos/kibana/gettingstarted/logs.jsonl.gz>
	+ `gunzip logs.jsonl.gz`

<br>

shakespeare按以下模式组织：

```json
{
	"line_id": INT,
    "play_name": "String",
    "speech_number": INT,
    "line_number": "String",
    "speaker": "String",
    "text_entry": "String"
}
```

<br>

accounts按以下模式组织：

```json
{
	"account_number": INT,
    "balance": INT,
    "firstname": "String",
    "lastname": "String",
    "age": INT,
    "gender": "M or F",
    "address": "String",
    "employer": "String",
    "email": "String",
    "city": "String",
    "state": "String"
}
```

<br>

日志数据的模式有许多不同的字段，此例使用字段如下：

```json
{
    "memory": INT,
    "geo.coordinates": "geo_point",
    "@timestamp": "date"
}
```

<br>

载入数据前，需要为字段设置映射。
映射将索引中的文档分成逻辑组，并指定字段特性。如可搜索性、标记化、分解为单独的单词。

在Kibana界面中的`Dev Tools`中输入如下命令，为shakespeare数据设置映射。

```sh
PUT /shakespeare
{
 "mappings": {
  "doc": {
   "properties": {
    "speaker": {"type": "keyword"},
    "play_name": {"type": "keyword"},
    "line_id": {"type": "integer"},
    "speech_number": {"type": "integer"}
   }
  }
 }
}
```

<br>

日志数据集`logs.jsonl`需要映射才能将日志中的经纬度标记为地理位置。

```
PUT /logstash-2015.05.18
{
  "mappings": {
    "log": {
      "properties": {
        "geo": {
          "properties": {
            "coordinates": {
              "type": "geo_point"
            }
          }
        }
      }
    }
  }
}
```


```
PUT /logstash-2015.05.19
{
  "mappings": {
    "log": {
      "properties": {
        "geo": {
          "properties": {
            "coordinates": {
              "type": "geo_point"
            }
          }
        }
      }
    }
  }
}
```

```
PUT /logstash-2015.05.20
{
  "mappings": {
    "log": {
      "properties": {
        "geo": {
          "properties": {
            "coordinates": {
              "type": "geo_point"
            }
          }
        }
      }
    }
  }
}
```

<br>

`accounts`数据集不需要映射，这一点上使用Elasticsearch的`bulk` API去载入数据集：

```sh
#这些命令要花一些时间

curl -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/bank/account/_bulk?pretty' --data-binary @accounts.json
curl -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/shakespeare/doc/_bulk?pretty' --data-binary @shakespeare_6.0.json
curl -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/_bulk?pretty' --data-binary @logs.jsonl


#验证
#在Kibana中的DevTools中运行
GET /_cat/indices?v
```


<br>
<br/>


### 定义你的索引模式


加载到Elasticsearch的每组数据集都有一个索引模式(index pattern)。索引模式是一个带有可匹配多个索引的可使用通配符的字符串。

在前面，Shakespeare数据集有一个名为: `shakespeare` 的索引；Account数据集有一个名为：`bank` 的索引。
如，在常见的日志文件中，一个典型的索引包含`YYYY.MM.DD`日期格式，类似于`logstash-2015.05.*`。

进入Kibana界面，点击**Management**， **Index Patterns**， **Create Index Pattern** 来创建一个索引模式。

`shakespeare`和`account`数据集不包含 `time-series data`。确保为此数据集创建索引模式时，不包含基于时间的事件。`logs`数据集包含了时序数据，因此索引需要包含基于时间的事件。

- `shakes*`
- `ba*`
- `logstash-2015*`

<br>

**定义索引模式时，与Elasticsearch匹配的索引必须存在。**

在Kibana的DevTools中输入: `GET _cat/indices` 来查看索引。


<br>
<br/>


### 数据发现


点击Kibana界面中的Discover以显示数据发现功能。



<br/>
<br/>


## 可视化

Visualize

Visualize允许你在Elasticsearch索引中创建数据的可视化。然后可以构建显示相关可视化的仪表盘。

Kibana的可视化基于Elasticsearch查询。通过使用一系列Elasticsearch聚合来提取和处理你的数据。你可以创建图标来显示你需要了解的趋势。


<br/>

### 创建可视化









































<br>
<br/>

---

<br>


# Elasticsearch文档



## 入门


Elasticsearch是一个高度可扩展的开源全文搜索和分析引擎。它允许你快速、近乎实时地存储、搜索和分析大量数据。

Elasticsearch的几个例子：

- 使用Elasticsearch来存储产品目录和库存，并为其提供搜索和建议
- 收集日志或交易数据，并分析和挖掘数据以便于查找趋势、统计数据、汇总或异常信息
- 价格提醒平台，允许顾客制定规则，收到相应规则信息
- 分析智能需求，快速调查、分析、可视化并对大量数据提出特别的问题


<br>
<br/>


### 基本概念


**Near Realtime(NRT)**
Elasticsearch是一个近乎实时的搜索平台。这意味着从索引文档到可搜索之间存在轻微的延迟(通常为1s)

<br>

**Cluster**
集群是一个或多个节点(服务器)的集合，它们一起保存所有数据，并提供跨节点的联合索引和搜索功能。集群由默认名为`elasticsearch`的唯一名称标识，它很重要。
确保不要在不同的环境中重复使用相同集群名称，否则可能会导致节点加入错误的集群。
集群可以只有一个节点！你也可以拥有多个独立的集群，每个集群有自己唯一的集群名称。

<br/>

**Node**
节点是属于集群一部分的单个服务器，存储数据并参与集群的索引和索引。
与集群一样，一个节点由一个名称来标识，启动时随机分配的UUID。你也可以自定义节点名。
配置节点通过集群名称加入特定的集群，默认加入`elasticsearch`集群。
在单集群中，你可以拥有任意数量的节点。

<br>

**Index**
索引是一些具有相似特征的文档集合。例如，客户数据的索引，产品目录的索引，订单数据的索引......
索引由名称标识(必须全小写)，文档执行索引、搜索、更新和删除操作时引用索引。
在一个单集群中，你可以定义任何你想要的索引。

<br/>

**Document**
文档是可被索引的基本信息单位。例如，单个客户的文档，单个产品的文档，单个订单的文档...
文档以JSON格式表示。
一条记录就是一个文档。

<br>

**Shards和Replicas**
索引可潜在地存储大量数据，这些数据可能会超多单个节点的硬件限制。例如，占用1TB磁盘空间的十亿文档的单个索引可能不适合单个节点的磁盘，或者可能太慢而无法单独向单个节点提供搜索请求。
为了解决这个问题，Elasticsearch提供了将索引细分为称为分片的多个碎片上。当你创建索引时，你可以简单定义所需的分片数量。
每个分片本身都是一个功能齐全且独立的索引，可以在集群中的任何节点上进行托管。

分片重要的两个原因：

- 允许你水平分割/缩放内容量
- 允许分布和并行操作跨分片，从而提高性能和吞吐量(throughput)

在任何时候都可能出现的网络环境中，强烈建议使用故障切换机制，以防止分片/节点因任何原因而消失。为此，Elasticsearch允许你将索引分片制作为一个或多个称为副本分片的副本集。
副本集分片永远不会分配到与原始分片相同的节点上。

副本集重要的原因：

- 在分片/节点失效的情况下提供高可用性
- 因为搜索可以在所有副本上并行执行，它允许你扩展搜索量和吞吐量

总而言之，每个索引都可以分成多个分片，索引也可以被复制。一旦复制，每个索引将具有主分片和副本分片。在创建索引时，可为每个索引定义分片和副本数量。在索引创建之后，你可以动态更改副本的数量，但无法更改分片的数量。

默认情况下，Elasticsearch中的每个索引都分配了5个主分片和副本。

每个Elasticsearch分片都是一个Lucene索引。单个Lucene索引有最大文档数量限制。


<br>
<br/>


## 探索你的集群


**The REST API**
REST(Representational State Transfer)表现层状态转换，是一种万维网软件架构风格，目的是便于不同程序在网络中互相传递信息。REST通常使用HTTP, URI, XML和HTML这些协议和标准。

启动节点，下一步便是理解如何与它通信。幸运的是，Elasticsearch提供了一个非常全面(comprehensive)和强大的REST API，可以使用它与集群进行交互。

使用API可以完成如下几件事：

- 检查集群、节点和索引的健康、状态和统计信息
- 管理集群、节点、索引数据和元数据
- 执行CRUD(create, read, update, delete)
- 执行高级搜索操作(分页、排序、过滤、脚本、聚合...)


<br>
<br>


### 集群健康


基本健康检查，看看集群正在做什么。
使用`_cat`API检查集群健康。可使用Kibana Console或curl等工具。

```
#Kibana
GET /_cat/health?v


#cmd
curl -X GET "localhost:9200/_cat/health?v" -u elastic


epoch      timestamp cluster    status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1525330981 15:03:01  docker-elk yellow          1         1     32  32    0    0        6             0                  -                 84.2%
```

集群健康：

- green: 万事OK(集群功能齐全)
- yellow: 所有数据可用，但一些副本尚未分配(集群功能齐全)
- red: 一些数据因某种原因不可用(集群部分功能)

集群名称：

- 集群名称被修改为`docker-elk`

<br>


列出集群中的节点：

```
GET /_cat/nodes?v


ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           47          74  93    3.18    3.13     2.90 mdi       *      LGrAIE5
```

随机节点名： `LGrAIE5`


<br>
<br/>


### 列出所有索引


```
GET /_cat/indicies?v


health status index                             uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .monitoring-kibana-6-2018.04.27   bsKsurh7TKaCsnekwHs3yg   1   0        870            0    328.1kb        328.1kb
green  open   .watcher-history-7-2018.04.28     zuq3rjI8S0OSS7vcZl7kSQ   1   0        954            0      1.4mb          1.4mb
green  open   .kibana                           8t_7lqq4TFSfelA7phgv5g   1   0        142           18    191.8kb        191.8kb
green  open   .monitoring-es-6-2018.04.28       vtUSjqaITT28CMHArpfNoA   1   0      20436            0      9.6mb          9.6mb
yellow open   filebeat-6.2.4-2018.05.03         sK3lIvMXS8GoRbWYCjdgzg   3   1        568            0    348.6kb        348.6kb
```


<br>
<br/>


### 创建索引


创建一个名为`customer`的索引，然后列出索引

```
#pretty漂亮JSON显示
PUT /customer?pretty

#或
curl -X PUT "localhost:9200/zhang" -u elastic:elastic


GET /_cat/indices?v


#pri主分片，rep副本
health status index                             uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   customer                          WQ3qEnPQRW6FpVIHYVJ7yA   5   1          0            0      1.1kb          1.1kb
yellow open   zhang                             nkOUPOWERsS1PT_wEui67g   5   1          0            0      1.1kb          1.1kb
```

你可能注意到了，索引的健康状态是`yellow`，表明有一些副本尚未分配。
这个索引发生这种情况的原因是Elasticsearch默认为这个索引创建了一个副本。由于此刻我们只有一个节点在运行，因此只有在其它几点加入集群后才能分配一个副本。一旦副本分配到另外的节点，健康状态会变成`green`。


<br>
<br/>


### 索引和查询文档


现在让我们把一些东西放入`customer`索引中。讲一个简单的customer文档放入customer索引中，ID为1：

```
PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}

#或
curl -X PUT -u elastic:elastic "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}'


{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}



GET /customer/_doc/1?pretty

{
  "_index": "customer",
  "_type": "_doc",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": {
    "name": "John Doe"
  }
}

#name:John Doe _id:1 _type:_doc _index:customer _score:1
```


<br>
<br/>


### 删除索引


```
DELETE /customer?pretty

curl -X DELETE "localhost:9200/customer?pretty" -u elastic:elastic


{
  "acknowledged": true
}

```


<br>
<br/>


## 修改数据


Elasticsearch几乎提示提供数据操作和搜索功能。从索引、更新、删除数据时可能会有1s延迟。数据在事物完成后立即可用。


**索引/替换 文档**

```
PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}


#如果我修改此处文档信息，则Elasticsearch会替换之前的文档
PUT /customer/_doc/1?pretty
{
  "name": "Zhang"
}

#name:Zhang _id:1 _type:_doc _index:customer _score:1


#或者新增一个文档
PUT /customer/_doc/2?pretty
{
  "name": "Zhang"
}

#name:Zhang _id:2 _type:_doc _index:customer _score:1
```

<br>

**未指定ID：**
ID是可选的。如果未指定ID，Elasticsearch会生成随机ID。
注意，此时使用`POST`方法。

```
POST /customer/_doc?pretty
{
  "name": "Zhang"
}

#name:Zhang _id:76xJJWMBddhqcmsO07A_ _type:_doc _index:customer _score:1
```


<br>
<br/>


### 更新文档


除了能够索引和替换文档，我们还可以更新文档。
Elasticsearch实际上并没有在原地就地更新，它是先删除旧文档，然后一次性更新索引新文档。

更新同样能够使用简单的脚本。
Elasticsearch提供了通过查询条件(类似于`SQL-UPDATE-WHERE`)更细多个文档的能力。

```
POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}


#继续更新
POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe", "age": 20}
}



#简单脚本
#ctx._source指即将更新的当前源文档
POST /customer/_doc/1/_update?pretty
{
  "script": "ctx._source.age += 5"
}
```


<br>
<br/>


### 删除文档

也可通过API匹配查询，删除所匹配的文档。

```
DELETE /customer/_doc/2?pretty

```


<br>
<br/>


### 批量处理


Elasticsearch同样提供了使用[`_bulk`API](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/docs-bulk.html)批量执行上述任何操作的功能。这是一种高效的机制，尽可能快地完成多项操作。

Bulk API不会因其中一个操作失败而停止，它将继续处理后面的动作。当它完成是，它会返回每个操作的状态，以便你可以检查是否失败。

```
POST /customer/_doc/_bulk?pretty
{ "index": { "_id": "1" } }
{ "name": "John Doe" }
{ "index": { "_id": "2" } }
{ "name": "Jane Doe" }


#更新
POST /customer/_doc/_bulk?pretty
{"update": { "_id": "1" } }
{ "doc": { "name": "John Doe becomes Jane Doe" } }
{ "delete": { "_id": "2" } }
```


<br>
<br/>


## 探索你的数据


**简单数据集**
准备一个更加真实的数据集。如下生成的JSON文档，每个文档都有如下要点：

```
{
    "account_number": 0,
    "balance": 16623,
    "firstname": "Bradshaw",
    "lastname": "Mckenzie",
    "age": 29,
    "gender": "F",
    "address": "244 Columbus Place",
    "employer": "Euron",
    "email": "bradshawmckenzie@euron.com",
    "city": "Hobucken",
    "state": "CO"
}

```

**载入这个数据集**
下载Elasticsearch提供的[accounts.json](https://raw.githubusercontent.com/elastic/elasticsearch/master/docs/src/test/resources/accounts.json)

```
curl -H "Content-Type: application/json" -u elastic:elastic -XPOST "localhost:9200/bank/_doc/_bulk?pretty&refresh" --data-binary "@accounts.json"
curl "localhost:9200/_cat/indices?v"


health status index                             uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank                              PGSvNwQwQIOhMDr1nmXIuw   5   1       1000            0    474.7kb        474.7kb
```

这样我们成功批量索引了1000个文档到bank索引。


<br>
<br/>


### Search API


现在让我们做一些简单的搜索(search)。有两种基本搜索方式：

- REST request URI
- REST request body
	+ 以可读的JSON格式定义你的搜索，推荐方式

<br>

搜索的REST API可从`_search`端点访问:

```
#在bank索引下的_search端点搜索
#匹配所有文档，并以账户字段顺序排列
#最后以可读的JSON格式输出结果
GET /bank/_search?q=*&sort=account_number:asc&pretty



{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```

- `took`: Elasticsearch执行搜索花费的事件(ms)
- `timed_out`: 查询超时与否
- `_shards`: 搜索了多少分片，包含成功和失败的次数
- `hits`: 搜索结果
- `hits.total`: 匹配搜索的文档数
- `hits.hits`: 搜索结果数组(默认前十个文档)
- `hits.sort`: 结果的排序键
- `hits._score`, `max_score`: 忽略的字段

<br>

REST request body方法

```
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```


<br>
<br/>


### 查询语法


Elasticsearch提供了可用于执行查询的JSON格式语言，这被称为 **Query DSL**

```
#上一个查询栗子
GET /bank/_search
{
  "query": { "match_all": {} }
}

```

处理`query`参数，我们还可以传递其它参数来搜索结果:

```
#size参数，返回从from开始多少个文档
#from未指定，就默认为0
GET /bank/_search
{
  "query": { "match_all": {} },
  "size": 1
}


#from参数，指定从哪个文档索引开始
GET /bank/_search
{
  "query": { "match_all": {} },
  "from": 10,
  "size": 10
}


#sort参数
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": { "balance": { "order": "desc" } }
}
```


<br>
<br/>


### 执行搜索


搜索某些字段：

```
GET /bank/_search
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}

```

匹配查询：

```
GET /bank/_search
{
  "query": { "match": { "account_number": 20 } }
}


GET /bank/_search
{
  "query": { "match": { "address": "mill" } }
}


GET /bank/_search
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
```

<br>

布尔查询：

- `must`
- `should`
- `must_not`

```
#must
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}


#should
GET /bank/_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}


#must_not
GET /bank/_search
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}


#组合使用must,must_not,should
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}

```


<br>
<br/>


### 过滤


前面我们跳过了称为文档分数的`_score`字段。它是文档与搜索查询匹配度相度量的一个数值。数值越大，与文档越相关。

但查询并不总是需要产生分数，特别是当它们仅用于过滤时。Elasticsearch检测这些情况并自动优化查询执行，以便不计算无用的分数。

- `range query`: 通过一系列值来过滤文档

```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}

```

除了前面这些查询类型，还有很多其它类型。由于只是入门章节，所以并不会涉及太多太难。


<br>
<br/>


### 聚合

聚合(Aggregation)提供了从数据中分组和提取统计的功能。
考虑聚合最简单方法是将其大致等同于SQL `GROUP BY`和SQL聚合函数。

在Elasticsearch中，你可以执行返回匹配的搜索，同时还可以在一个响应中返回与匹配不同的聚合结果。你可以运行查询和多个聚合，并一次性获得多个操作的结果。

```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      }
    }
  }
}

#类似的SQL
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC
```

```
#group, average
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}



GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}
```

还有很多其它聚合方法，请参考<https://www.elastic.co/guide/en/elasticsearch/reference/6.2/search-aggregations.html>。



<br/>
<br/>


## elasticsearch-py


Python可使用`elasticsearch-py`模块来操作Elasticsearch，具体文档请查看Python这篇文章的elasticsearch第三方模块。



<br>
<br/>

---

<br/>



# Lucene查询

ElasticSearch提供的一些查询方式(query types)能够被Lucene的查询解析器(query parser)语法所支持。可直接在Kibana的发现面板上直接使用。


<br>

## 全文搜索

- string
- "string1 string2"

Kibana会匹配和展示对应的string。


<br>


## 键值对

- `key:value`: 全文搜索
- `"key:value"`： 精确搜索
- `_exists_:key`: 返回结果中需要有key字段
- `_missing__:key`: 不能含有key字段


如:`http.code:502`，`log-levle:warn`


<br>


## 通配符

- `?`
- `*`

这两者都不能用作第一个字符，如`?.txt, *.txt`


<br>


## 正则表达式

它也支持性能较差的正则表达式。


<br>


## 模糊搜索

- `~`: 在一个单词后面加上`~`启用模糊搜索
- `~n`： 设置编辑距离(整数)，指定需要多少相似度，越大越接近原始值
- 在短语后面加`~`，可以搜索到被隔开或顺序不同的单词

`first~`也可以匹配到frist
`"hello world"~5`表示两者之间可以隔着5个单词



<br>


## 范围搜索

数值/时间/IP/字符串 类型的字段可以对某一范围进行查询

```
length:[100 TO 200]
sip:["172.24.20.110" TO "172.24.20.140"]
date:{"now-6h" TO "now"}
tag:{b TO e} 搜索b到e中间的字符
count:[10 TO *] * 表示一端不限制范围
count:[1 TO 5} [ ] 表示端点数值包含在范围内，{ } 表示端点数值不包含在范围内，可以混合使用，此语句为1到5，包括1，不包括5

可以简化成以下写法：
age:>10
age:<=10
age:(>=10 AND <20)
```


<br>


## 优先级

使用`^`使一个词语比另一个搜索优先级更高，默认为1。可以为0~1之间的浮点数，来降低优先级


<br>



## 逻辑操作

- `AND`
- `OR`
- `NOT`
- `+`: 搜索结果中必须包含此项
- `-`: 不能包含此项

```
(a OR b) AND c

host:(baidu OR qq OR google) AND host:(com OR cn)
```


<br>


## 转义字符

- `\`：使用转义字符来转移特殊字符




<br/>

---

<br/>



# Metricbeat


Metricbeat是一个轻量级的托运器(lightweight shipper), 你可从安装该软件的操作系统和服务器上定期收集指标信息。它可将收集到的指标信息或统计信息发送到指定的输出(如elasticsearch/Logstash)。

具体使用方法也和Filebeat差不多！

Metricbeat通过从服务器上运行的系统和服务收集指标来帮助你监控服务器。如：

- Apache
- Docker
- Kafka
- Kubernets
- HAProxy
- MongoDB
- MySQL
- Nginx
- PHP-FPM
- PostgreSQL
- Redis
- RabbitMQ
- System
- Zookeeper
- ...






<br>
<br/>

---

<br/>



# Packetbeat



Packetbeat是一个实时网络数据包分析器，可与Elasticsearch一起提供应用程序监控和性能分析。

Packetbeat通过捕获应用服务器之间的网络流量，解码应用层协议(HTTP, MySQL, Redis...)，将请求与响应关联起来，并记录每个事务感兴趣的字段。
Packetbeat可以帮助你轻松地注意到后端应用程序的问题，例如错误或性能问题，并且可以更快地排除故障并进行修复。
Packetbeat捕获服务器之间的流量，即时分析应用层协议，并将这些消息关联到事务中。并将这些事务插入到Elasticsearch或使用Redis和Logstash的队列中。

Packetbeat支持的协议如下:

- ICMP
- DNS
- HTTP
- AMQP
- Cassandra
- MySQL
- PostgreSQL
- Redis
- MongoDB
- Thrift-RPC
- TLS




<br/>
<br>

---

<br/>



# Heartbeat


Heartbeat是一个轻量级守护进程，用以定期检查服务的状态并确定它们是否可用。与Metricbeat不同，Metricbeat只会告诉你服务器是down/up，而Heartbeat会告诉你服务是否可以访问(reached)。

当你需要验证是否满足服务级别协议的服务正常运行时间时，Heartbeat非常有用。当需要验证外部没有人能访问企私有服务器上的服务时，这也很有用。
你可以配置Heartbeat来ping指定主机名的所有DNS可解析的IP地址。这样，你可以检查所有负载均衡的服务，看他们是否可用。
配置Heartbeat时，你可以指定用于表示要检查的主机名的监视器(monitor)。每台监视器都根据你指定的时间表运行。

Heartbeat目前支持通过通过如下方式监控主机：

- ICMP
当你指向检查服务是否可用时，请使用icmp监视器。此功能需要root权限

- TCP
支持SSL/TLS/proxy
你可以选择配置此监视器，通过发送 and/or 接收自定义有效内容来验证端点

- HTTP
支持SSL/TLS/proxy
你可以选择配置此监视器，来验证该服务是否会返回预期的响应。如特定状态码，响应header或内容




<br>
<br/>

---

<br/>




# Auditbeat


Auditbeat是一个轻量化的托运器(shipper)，在系统上安装它，以审核(audit)系统上用户和进程的活动。

例如，你可以使用Auditbeat从Linux Audit Framework收集和集中审计事件。你还可以使用它来检查关键文件的改动，并识别潜在的安全策略违规。




<br>
<br/>

---

<br/>



# Topbeat


在v5.0, Topbeat被Metricbeat取代！

Topbeat的版本与其它Elastic Stack组件不同步，ES是v6.2.4， 而Topbeat是v1.3。所以需要额外安装repo.

Topbeat是一个轻量化的托运器(shipper)，来定期读取系统和每个进程的CPU和内存统计信息，然后为Elasticsearch中的统计信息编制索引。

<br>

Topbeat通过收集如下指标来帮助你监控你的服务器:

**ystem-wide statistics**

- system load
	+ 1, 5, 15
- system wide CPU usage
	+ user, system, idle, IOWait
- system wide memory uusage
	+ total, used, free
- system wide swap usage
	+ total, used, free

**Per-process statistics**

- process name
- process parent pid
- process state
- process pid
- process CPU usage
- process Memory usage

**File system statistics**

- avaliable disks
- name, type, mounted
- total, used, free, available











<br>
<br/>

---

<br/>




# APM


APM(Application Performance Monitoring)应用程序性能监控，自动收集应用程序内部的深入性能指标和错误。

它由三个组件组成:

- Agents
	+ Node.js
	+ Django
	+ Flask
	+ Ruby on Rails
	+ Rack
	+ JS
- Server
- UI





<br>
<br/>

---

<br/>



# ElastAlert


- GitHub: <https://github.com/Yelp/elastalert>
- Docs: <https://elastalert.readthedocs.io>

<br>

ElastAlert是一个简单灵活的用于Elasticsearch中数据异常的告警框架。它使用Python2.x编写，不支持Python3。
ElastAlert功能与Watcher类似，只不过Watcher是Elastic Enterprise中才支持，而ElastAlert是一个开源软件。

Kibana非常适合可视化和查询数据，但它需要一个配套工具来对数据进行告警，出于这种需要，ElastAlert诞生了。
如果你几乎实时地将数据写入Elasticsearch，并希望在数据与某些模式匹配时收到告警，则ElastAlert就是适合你的工具。


<br/>
<br/>



## 综述


ElastAlert被设计为可靠、高度模块化、易于设置和配置。
它使用两种类型的组件与Elasticsearch进行结合：

- rule type
- alerts

定期检查Elasticsearch并将数据传递给规则类型，它确定了何时找到匹配项。当匹配发生时，它触发一个或多个报警，而这些报警便采取具体行动。

每组规则定义了一个查询、一个规则类型和一组警报。

<br>

ElasAlert几种通用规则类型：

- frequency
Match where there are X events in Y time

- spike
Match when the rate of events increases or decreases

- flatline
Match when there are less than X events in Y time

- blacklist/whitelist
Match when a certain field matches a blacklist/whitelist

- any
Match on any event matching a given filter

- change
Match when a field has two different values within some time

<br>

ElasAlert几种内建报警类型：

- Command
- Email
- JIRA
- OpsGenie
- SNS
- HipChat
- Slack
- Telegram
- Debug
- Stomp

<br>

**你也可以导入和编写规则类型和报警类型。**

<br>

除了这些基础用法外，还有许多其它功能:

- Alerts link to Kibana dashboards
- Aggregate counts for arbitrary fields
- Combine alerts into periodic reports
- Separate alerts by using a unique key field
- Intercept and enhance match data


<br/>
<br/>


## 可靠性

Reliability


ElasAlert有多种功能，可在restart或Elasticsearch不可用时使其更可靠:

- ElastAlert将其状态保存到Elasticsearch，并在启动时先恢复先前停止的状态
- 如果Elasticsearch没有响应，ElastAlert将等待它恢复，然后再继续
- 抛出错误的警报可能会在一段时间内自动重试


<br/>
<br/>


## 模块性

Modularity


ElastAlert有3个主要组件，可作为模块导入或自定义。

- **rule types**
规则类型负责处理从Elasticsearch返回的数据。

- **alerts**
警报负责根据匹配采取行动。

- **enhancements**
增强功能是一种拦截警报并以某种方式修改或增强警报的方法。


<br/>
<br/>


## 配置


### 配置项

ElastAlert有一个全局配置文件`config.yaml`，它定义了几个操作方面:

```
#ElastAlert将持续查询熊当前到buffer_time前的窗口
buffer_time


#ES
es_host
es_port

#可选
es_username
es_password
#URL prefix for the Elasticsearch endpoint
es_url_prefix
#Method for querying Elasticsearch，默认GET
es_send_get_body_as
#默认20
es_conn_timeout

#可选配置
use_ssl
verify_certs
client_cert
client_key
ca_certs



#规则配置文件目录
rules_folder
#递归，默认true
scan_subdirectories

#查询频率，如 minutes: 5
run_every

#elastalert将存储数据的索引名称
writeback_index

#报警失败的重试窗口
alert_time_limit

#单个查询中从es下载的最大文档数，默认10 000
max_query_size

scroll_keepalive

#聚合在一起的最大警报数，默认10 000
max_aggregation

#ElastAlert从最近开始运行的查询开始的最长时间
old_query_limit

#当抛出未知异常时，禁用rule。 默认true
disable_rules_on_error


#Email
#接收通知的邮件
nottify_email
#默认值ElastAlert
from_addr
smpt_host
email_reply_to


#Amazon Elasticsearch Service
aws_region
boto_profile
profile


#在将文档写入Elasticsearch前，ElastAlert使用下划线替换字段名中的任意一个点(.)。默认值False
replace_dots_in_field_names

#es中用于字符串多字段的子字段的后缀
string_multi_field_name


```


<br/>
<br/>


## 运行ElastAlert

运行：

```py
python elastalert/elastalert.py

```

一些参数：

```
--config

--debug

--verbose

--start
--end

--rule

--slience

--es_debug
--es_debug_trace

--pin_rules

```



<br/>
<br/>



## 首次运行ElastAlert

Running ElastAlert for the First Time


<br/>


### 依赖

Requirements:

- es
- ISO8601 or Unxi timestamped data
- Python 2.7
- `python2-pip python-dev libffi-dev libssl-dev`


<br/>


### 安装

```
#依赖
yum install python2-pip python-dev

#setuptools >= 11.3
pip2 install --upgrade setuptools

#elasticsearch >= 5.0
pip2 install elasticsearch


pip2 install elastalert

#or
#git clone https://github.com/Yelp/elastalert.git
#cd elastalert
#python2 setup.py install
```

之后修改配置文件，我将ElastAlert目录移动到了`/etc/`下。
修改配置文件，并将ElastAlert的`config.yaml.example`配置保存为`config.yaml`。


<br/>


### 设置es

Setting Up Elasticsearch


ElastAlert将有关其查询及报警的信息和元数据报错到Elasticsearch。这虽然不是必须的，但却强烈建议使用。

```
#创建一个用于ElastAlert写入的index
elastalert-create-index

#会有es主机，端口，用户，密码和索引相关信息
Enter Elasticsearch host: zhang21
Enter Elasticsearch port: 9200
Use SSL? t/f: f
Enter optional basic-auth username (or leave blank):
Enter optional basic-auth password (or leave blank):
Enter optional Elasticsearch URL prefix (prepends a string to the URL of every request): 
New index name? (Default elastalert_status)
Name of existing index to copy? (Default None)
Elastic Version:6
Mapping used for string:{'type': 'keyword'}
New index elastalert_status created
Done!
```


<br/>
<br/>


### 创建一个规则

Creating a Rule


每个规则定义要执行的查询，触发匹配的参数以及每个匹配要触发的报警列表。
`cat ./example_rules/example_frequency.yaml`

```
es_host: elasticsearch.example.com
es_port: 14900


#唯一的规则名
name: Example rule


#规则类型
type: frequency


#要查询的索引
index: logstash-*


#触发报警的阈值
num_events: 50
#阈值的时间区间
timeframe:
    hours: 4


#过滤列表
filter:
- term:
    some_field: "some_value"


#报警列表
alert:
- "email"
#报警地址列表
email:
- "elastalert@example.com"
```

<br>

**栗子**
elastalert:

```
vim /etc/elastalert/example_rules/example_frequency.yaml


es_host: "192.168.1.11"
es_port: 9200
name: "test rule"
type: "frequency"
#此处我用python新建一个索引，用于测试
index: "my-index"
num_events: 3
timeframe:
    hours: 1
filter:
- query_string:
    query: "log_level: ERROR"

#- term:
#    name: "zhang21"

alert:
- "email"

email:
- "elastalert@example.com"
```


<br/>
<br/>


### 测试规则


运行`elasticalert-test-rule`工具将测试你的配置文件是否成功加载并在过去24h内以调试模式运行：

```
elastalert-test-rule ./example_frequency.yaml
```

配置首选项将按如下方式加载：

1. yaml文件中指定的配置
2. 配置文件中指定的配置
3. 默认配置


<br/>
<br/>


### 运行ElastAlert


有两种方式来调用ElastAlert：

- Supervisor
- Python

<br>

为了便于调试，下面将直接调用。

```py
python2 -m elastalert.elastalert --verbose --rule /etc/elastalert/example_rules/example_frequency.yaml
INFO:elastalert:Starting up

#这里遇到一个错误
ERROR:root:Error running query: TransportError(400, u'search_phase_execution_exception', u'No mapping found for [@timestamp] in order to sort on')


#解决方法，在规则文件example_frequency.yaml中添加
timestamp_field: timestamp
```


使用Python3创建索引：

```py
from datetime import datetime
from elasticsearch import Elasticsearch


es=Elasticsearch('http://192.168.1.11:9200')
es.info()

#写入文档
data = {
  # 由于ES接收UTC时间，因此需要使用UTC事件，不然会给我+8(CST)
  # 'timestamp': datetime.now(),
  'timestamp': datetime.utcnow(),
  'name': 'zhang21'
}

for i in range(1, 21):
    es.index(index='my-index', doc_type='test-type', id=i, body=data)

```



<br/>
<br/>



## 规则类型和配置项

Rule Types and Configuration Options


<br/>


### 规则配置项

Rule Configuration Cheat Sheet


选项太多，自己去看: <https://elastalert.readthedocs.io/en/latest/ruletypes.html>


<br/>


### 通用配置项

每个在`rules_folder`下的`.yaml`文件默认都会被执行。

**必须的配置**

- es_host
- es_port
- index
- name
- type
- alert

**可选配置**
自己去看。

<br/>
<br/>


### 规则类型

Rule Types


在`elastalert/ruletypes.py`中定义的各种RuleType class构成了ElastAlert的主要逻辑。每个规则都在内存中保存一个实例，传递通过给定过滤器查询es返回的所有数据，并根据该数据生成匹配。


<br>

- **any**
任意规则都将匹配所有内容。查询返回的每个匹配都会生成一个警报。

<br>

- **blacklist**
黑名单规则根据黑名单检查某个字段，如果它存在于黑名单中，则匹配。

黑名单规则需要两个额外项：
`compare_key`——与黑名单进行比较的字段。如果为空，事件将被忽略。
`blacklist`——黑名单列表值或黑名单文件列表(`"!file ./blacklist.txt"`)

栗子：

```
blacklist:
  - value1
  - value2
  - "!file /tmp/blacklist1.txt"
```

<br>

- **whitelist**
白名单规则根据白名单检查某个字段，如果列表中不包含此字段，则匹配。

白名单规则需要三个额外项：
`compare_key`——与白名单进行比较的字段
`ignore_null`——如果为true，则没有`compare_key`字段的事件将不匹配
`whitelist`——白名单列表值或白名单文件列表

栗子:

```
whitelist:
    - value1
    - value2
    - "!file /tmp/whitelist1.txt"
    - "!file /tmp/whitelist2.txt"
```

<br>

- **change**
此规则将监视某个字段，如果此字段改变就匹配。

此规则需要三个额外项：
`compare_key`——监控要改变的字段名。可以是一个列表，如果任意字段发生标号，都将触发警报。
`ignore_null`——如果为true，则没有`compare_key`字段的事件将不计为已更改。
`query_key`——此规则基于每个查询键应用。

一个可选字段：
`timeframe`——改变之间的最大时间

<br>

- **frequency**
此规则匹配在给定时间范围内至少一定数量的事件。

此规则需要两个额外项：
`num_events`——将会触发报警的事件数
`timeframe`——上面事件的时间范围

<br>

- **spike(突增)**
当给定时间段内的事件量的`spike_height`次数大于或小于前一个时间段时，此规则匹配。它使用两个滑动窗口(引用和当前)来比较。

此规则需要三个额外项：
`spike_height`——上次时间段时间数与前时间段事件数的比率，将处罚告警
`spike_type`——up/down/both
`timeframe`：时间段

<br>

- **flatline(脉波)**
当一段时间内事件总数匹配给定阈值时，此规则匹配。

此规则需要两个额外项：
`threshold`——不触发警报的最小事件数
`timeframe`——时间段

<br>

- **new term(术语)**
当一个以前从未见过的新值出现在字段中时，此规则匹配。

此规则需要一个额外项：
`fields`——要监控的新术语的字段列表

<br>

**cardinality(基数)**
在一个时间范围内，当某个字段的唯一值的总数高于或低于阈值时，此规则匹配。

此规则需要：
`timeframe`——时间段
`cardinality_field`——计算基数的字段

最大或最小基数取一个
`max_cardinality`——数据的基数大于此报警
`min_cardinality`——数据基数小于此报警

<br>

**metric aggregation**
当计算窗口中的度量值高于或低于阈值时，此规则匹配。默认值为`buffer_time`。

此规则需要：
`metric_agg_key`——计算度量标准的字段
`metric_agg_type`——字段的类型
`doc_type`——指定要搜索的文档类型

最大和最小至少需要一个
`max_threshold`——计算的度量标准大与此，报警
`min_threshold`——计算的度量标准小于此，报警

<br>

**percentage match**
当计算窗口内匹配桶(bucket)中的文档百分比高于或低于阈值时，此规则匹配。默认情况下，计算窗口为`buffer_time`。

此规则需要：
`match_bucket_filter`—— ES filter DSL。为匹配桶定义了一个过滤器，它应用匹配查询过滤器并返回文档的子集。
`doc_type`——指定查询文档类型

最大和最小至少需要一个
`min_percentage`——匹配文档的百分比小于此，报警
`max_percentage`——匹配文档的百分比大于此，报警



<br/>
<br/>



### Alerts


每条规则都可以附加任意数量的报警。Alerts是`Alerter`的子类，并从`ElastAlert`传递包含相关信息的字典或字典列表。与规则配置类似，它们在规则配置文件中配置。

```
alert:
- email
- jira
- xxx
```

多个邮件：
```
alert:
- email

from_addr: "no-reply@example.com"
email: "someone@example.com"
```

```
alert:
- email:
    from_addr: "no-reply@example.com"
	email: "someone@example.com"
- email:
    from_addr: "xx"
	email: "xxx"
```


<br/>
<br/>


#### Alert Subject

可通过添加包含自定义摘要的`alert_subject`来自定义电子邮件主题。

```
alert_subject: "Issue {0} ouccurreda at {1}"
```

```
alert_subject_args:
- issue.name
- "@timestamp"
```

如果规则匹配索引中的多个对象，则仅使用第一个匹配来填充格式化程序的参数。


<br/>
<br/>


#### Alert Content

有几种方法可以格式化给种类型事件的正文：

```
rule_name           = name
alert_text          = alert_text
ruletype_text       = Depends on type
top_counts_header   = top_count_key, ":"
top_counts_value    = Value, ": ", Count
top_counts          = top_counts_header, LF, top_counts_value
field_values        = Field, ": ", Value
```

默认：
```
body                = rule_name

                      [alert_text]

                      ruletype_text

                      {top_counts}

                      {field_values}
```


<br/>
<br/>


#### command

命令报警允许你执行任意命令并从匹配中传递参数或`stdin`。该命令的参数可以使用Python格式的字符串语法来访问匹配的部分内容。报警器将打开一个子进程并可选地传递匹配，或在聚合报警的情况下，将其作为json阿虎组匹配到进程的`stdin`。

此报警需要一个选项：
`command`——要执行的参数列表或要执行的字符串。如果是列表格式，则第一个参数是要执行的程序名。如果传递了一个字符串，则该命令通过shell执行。

字符串可使用`%`或`.format()`进行格式化。这是Python的替换。
如果在命令中使用格式化数据，清泪建议使用args列表格式而不是shell字符串。

```
alert:
- command

command: ["/bin/send_alert", "--username", "%(username)s"]
#command: ["/bin/send_alert", "--username", "{match[username]}"]
```


<br/>
<br/>


#### Email

此报警将会发送电子邮件。它默认连接到`smtp_host`服务器。

它需要一个选项：
`email`——接收报警的地址


<br/>
<br/>


#### Jira


<br/>
<br/>


#### Debug

调试报警器经使用Python logger的info level记录报警信息。它被记录到名为`elastalert`的Python logger对象中，可以使用`getLogger`命令轻松访问该对象。


<br/>
<br/>


#### HTTP POST

此报警类型使用HTTP POST将结果发送到JSON ENDPOINT。默认情况下，json会包含所有匹配，除非你指定`http_post_payload`。

需要：
`http_post_url`

```
alert: post
http_post_url: "http://example.com/api"
http_post_payload:
  ip: clientip
http_post_static_payload:
  apikey: abc123
```



<br/>
<br/>



## ElastAlert元数据索引

ElastAlert Metadata Index


ElastAlert使用Elasticsearch存储有关其状态的各种信息。这不仅允许对ElastAlert操作进行某种程度的审计和调试，而且还可以在ElastAlert关闭、重启或崩溃时避免数据丢失或重复报警。此集群和索引信息在全局配置文件中使用`es_host`, `es_port`, `writeback_index`定义。ElastAlert必须能够写入到此索引。`elastalert-create-index`将为你创建具有正确映射的索引，并可选择从现有的ElastAlert写回索引中复制文档。

ElastAlert将会在`writeback index`中创建三种不同类型的文档：

- elastalert_status
- elastalert
- elastalert_error


<br/>
<br/>


### elastalert_status


`elastalert_status`是为给定规则执行查询的日志，包含：

- `@timestamp`
- `rule_name`
- `starttime`
- `endtime`
- `hits`： 查询的结果数
- `matches`： 匹配数
- `time_taken`： 查询所用秒数


<br/>
<br/>


### elastalert


`elastalert`是有关触发的每个报警的日志信息，包含：

- `@timestamp`
- `rule_name`
- `alert_info`
- `alert_sent`
- `alert_time`
- `match_body`
- `alert_exception`
- `aggregate_id`


<br/>
<br/>


### elastalert_error


当ElastAlert发生错误时，它将写入Elasticsearch和stderr。`elastalert_error`类型包含：

- `@timestamp`
- `message`
- `traceback`
- `data`


<br/>
<br/>


### silence


`silence`是指由于重新设置或使用`-silence`而抑制给定规则的警报的记录。

- `@timestamp`
- `rule_name`
- `until`：警报在此开始发送的时间戳
- `exponent`：除非设置了`exponential_realert`，否则它将为0



<br/>
<br/>



## 添加一个新规则类型

Adding a New Rule Type





<br/>
<br/>



## 添加一个新报警器

Adding a New Alerter




<br/>
<br/>

---

<br/>



## 为规则编写过滤器

Writing Filters For Rules



<br/>
<br/>



## 增强功能

Enhancements

增强功能是一些模板，可让你在发送警报之前修改匹配项。




<br/>
<br/>
<br/>



## 在容器内运行


构建基础镜像:

```
# Docker-base
FROM ubuntu:latest

RUN apt-get update && apt-get upgrade -y && \
    apt-get -y install build-essential python-setuptools python2.7 python2.7-dev libssl-dev git tox python-pip vim && \
    pip install elastalert -i https://mirrors.aliyun.com/pypi/simple/
```

<br>

以后只需将配置文件导入基础镜像就好:

```
# Dockerfile
FROM zhang21/base-elastalert:latest

# config.yaml
# rules/
# smtp_auth_file.yaml
COPY . /opt/elastalert/

WORKDIR /opt/elastalert

CMD ["sh", "-c", "python -m elastalert.elastalert --verbose"]
```


<br/>
<br/>


## kibana-plugin

elastalert kibana-plugin是一个第三方插件。
ElastAlert Kibana plugin repository: <https://github.com/bitsensor/elastalert-kibana-plugin>

注意，安装的时候要注意kibana的版本。具体信息见README。





















