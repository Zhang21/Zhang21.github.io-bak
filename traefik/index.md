# traefik


参考:

- github: <https://github.com/traefik/traefik>
- doc: <https://doc.traefik.io/traefik/>

版本:

- traefik: v2.6


<br/>
<br/>

---

<!--more-->

<br/>
<br/>



# 欢迎

<img src="/images/Traefik/traefik-architecture.png" alt="traefik架构图"/>

<br/>

请注意traefik的版本，v2和v1不兼容。

traefik是一个开源的边缘路由器，使你的服务发布轻松而有趣。它代表你的系统接收请求，并发现哪些组件负责处理它们。它会自动发现你的服务的正确配置。当traefik检查你的基础架构时，它会找到信息并发现哪些服务提供请求。

traefik原生地符合主要的集群技术，如k8s, docker, swarm, aws, mesos等。

使用traefik，无需维护和同步单独的配置文件，一切都是实时和自动发生(无需重启和连接中断)。你将花费时间开发和部署新功能，而不是配置和维护其工作状态。




<br/>
<br/>

---

<br/>
<br/>




# 入门

Getting Started


<br/>
<br/>


## 概念

Concepts

你需要知道的一切。

<br/>

**Edge Router**

边缘路由器。traefik是一个边缘路由器，这意味着它是你平台的大门，并且它拦截和路由每一个传入请求，它知道所有逻辑和每条规则，来决定哪个服务处理哪个请求(基于path, host, headers等)。

<img src="/images/Traefik/traefik-concepts-1.png" alt="边缘路由器"/>

<br/>

**Auto Service Discovery**

自动服务发现。传统的边缘路由器(反向代理)需要一个配置文件，包含到服务的每个可能的路由，traefik从服务自身获取。

部署服务，你附上信息告诉traefik服务可以处理的请求的特征。这意味着当部署服务时，traefik立刻检测到它并实时更新路由规则。反之亦然，当你移除服务，该路由相应地消失。

你不再需要创建和同步包含路由规则的配置文件。

<img src="/images/Traefik/traefik-concepts-2.png" alt="自动服务发现"/>


<br/>
<br/>


## 快速开始

Quick Start

使用Docker的一个简单示例。

<img src="/images/Traefik/quickstart-diagram.png" alt="示例"/>

<br/>


### 使用Docker启动traefik

Launch Traefik With the Docker Provider

```yaml
# docker-compose.yml
version: '3'

services:
  reverse-proxy:
    # The official v2 Traefik docker image
    image: traefik:v2.6
    # Enables the web UI and tells Traefik to listen to docker
    command: --api.insecure=true --providers.docker
    ports:
      # The HTTP port
      - "80:80"
      # The Web UI (enabled by --api.insecure=true)
      - "8080:8080"
    volumes:
      # So that Traefik can listen to the Docker events
      - /var/run/docker.sock:/var/run/docker.sock
```

启动reverse-proxy服务: `docker-compose up -d reverse-proxy`


<br/>
<br/>


### 检测新服务和创建路由

Traefik Detects New Services and Creates the Route for You

在上面的文件添加内容。

```yaml
# ...
  whoami:
    # A container that exposes an API to show its IP address
    image: traefik/whoami
    labels:
      - "traefik.http.routers.whoami.rule=Host(`whoami.docker.localhost`)"
```

启动whoami服务: `docker-compose up -d whoami`

访问`localhost:8080/api/rawdata`，可看到traefik自动检测新容器并更新了自己的配置。

当traefik检测新服务，它自动创建相关路由。

```bash
# 执行
curl -H Host:whoami.docker.localhost http://127.0.0.1

#  输出结果
Hostname: a656c8ddca6c
IP: 172.27.0.3
...
```


<br/>
<br/>


## 负载均衡

More Instances? Traefik Load Balances Them

```bash
# 扩展whoami的副本数
docker-compose up -d --scale whoami=2

# 查看
curl -H Host:whoami.docker.localhost http://127.0.0.1

# 结果
Hostname: a656c8ddca6c
IP: 172.27.0.3
# 或
Hostname: s458f154e1f1
IP: 172.27.0.4
```


<br/>
<br/>


## 配置

Configuration Introduction

<img src="/images/Traefik/static-dynamic-configuration.png" alt="配置原理">

<br/>

traefik里的配置可以指向两种不同的事物：

- 完整地动态路由配置(动态配置)
- 启动配置(静态配置)

静态配置中的元素设置和提供程序的连接，并定义traefik将监听的入口点(这些元素不会经常更改)。

动态配置包含定义系统如何处理请求的一切事物。此配置可以更改，并可无缝热加载，而不会有任何请求中断或连接丢失。


<br/>
<br/>


### 动态配置

The Dynamic Configuration

traefik从提供商处获得动态配置，无论事协调程序、服务注册还是普通的旧配置文件。


<br/>
<br/>


### 静态配置

The Static Configuration

有三种不同的，互相排斥的(意味着你只能使用一种)，在traefik中定义静态配置的方法:

- 配置文件
- 命令行参数
- 环境变量

如果没有提供，则使用默认值。


<br/>
<br/>


#### 配置文件

Configuration File

配置文件`traefik.yaml`(或`traefik.toml`)通常位于:

- `/etc/traefik/`
- `$XDG_CONFIG_HOME`
- `$HOME/.config/`
- `.`

```
traefik --configFile=/dir/path/myconfig.yaml
```


<br/>
<br/>


#### 参数

Arguments

```bash
treafik --help

# or
docker run traefik[:version] --help
```


<br/>
<br/>


#### 环境变量

Environment Variables



<br/>
<br/>



## 安装

Install Traefik

有多种方式安装:

- docker
- helm
- binary
- source



<br/>
<br/>



## FAQ






<br/>

---

<br/>






# 配置发现

Configuration Discovery


<br/>


## 概述
































