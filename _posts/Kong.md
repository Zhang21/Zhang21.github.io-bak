---
title: Kong
date: 2019-05-08 02:44:31
categories: DevOps
tags:
  - API
  - APIGateway
  - Service Mesh
  - MicroService
---


参考：

- Kong GitHub: <https://github.com/Kong/kong>
- Kong Docs: <https://docs.konghq.com/>

<br>

环境：

- ELRH7x86_64
- Kong v1.1
- Docker CE v18.09
- K8s v1.11
















<br/>
<br/>

---

<!--more-->

<br/>
<br/>















# 入门

GETTING STARTED


![](/images/Kong/kongLogo.png)

Kong是一个可扩展的开源**API Layer**（也称为**API Gateway**或**API Middleware**）。Kong运行在任何RESTful API之前，并通过**插件(plugin)**进行扩展。

Kong的优点：

- **Scalable**：只需添加更多机器便可轻易扩展，这意味着你的平台可以处理任何负载，同时保持低延迟；
- **Modular**：可以通过添加新的插件来扩展Kong，这些插件可通过RESTful API配置；
- **Runs on any infrastructure**：Kong可以运行在任何地方。

Kong建立在N影响和Apache Cassandra或PostgreSQL等可靠技术之上，为你提供易于使用的RESTful API来操作和配置系统。

![](/images/Kong/kong-architecture.jpg)

<br>

下图是一个使用Kong请求API的典型工作流程。
一旦Kong运行，对API的所有请求都将首先到达Kong，然后代理到最终的API。在请求和相应之间，Kong将执行你决定安装的任何插件，为你的API提供支持。Kong有效地成为每个API请求的入口点。

![](/images/Kong/kong-simple.png)


<br/>
<br/>


## 介绍

[Introduction](https://docs.konghq.com/1.1.x/getting-started/introduction/)


你可能听过Kong是建立在Nginx之上的，它利用它的稳定性和效率。
更确切地说，Kong是一个在Nginx中运行的Lua Application，并且可通过`lua-nginx-module`模块实现。Kong不是用这个模块编译Nginx，而是与**OpenResty**一起发布，OpenResty以及包含了`lua-nginx-module`模块。OpenResty不是Nginx的分支(fork)，而是一组扩展其功能的模块。

这位可插拔架构(pluggable architecture)奠定了基础，在运行时启动并执行Lua scripts(plugins)。因此，我们认为Kong是微服务架构的典范，它的核心是实现**数据库抽象(database abstraction)**、**路由(routing)**和**插件管理(plugin management)**。插件可以存在于单独的代码中，并可以在几行代码中诸如到请求生命周期的任何位置。




<br/>
<br/>
<br/>




## 快速开始

[Quickstart](https://docs.konghq.com/1.1.x/getting-started/quickstart/)


在本章中，你讲学习如何管理Kong实例。首先，你将启动Kong，以便可访问RESTful Admin Interface，通过该界面管理服务(service)、路由(route)、消费者(consumer)...通过Admin API发送的数据存储在Kong的数据存储区中(PostgreSQL后Cassandra)。


<br/>
<br/>


### 启动Kong

```bash
# 运行kong migrations命令来准备数据存储
kong migrations bootstrap [-c /path/to/kong.conf]
# 你应该看到Kong已成功迁移的消息。否则，你可能在配置文件中错误的配置了数据库连接。

# 这里把默认配置文件中的postgre注释取消就好
cp /etc/kong/kong.conf.default kong.conf
vim kong.conf


# 可指定配置项
kong start [-c /path/to/kong.conf]
```



<br/>
<br/>



### 验证Kong

如果一切顺利，你应该看到一条消息通知你Kong正在运行。

默认情况下，Kong监听以下端口：

- **8000**：监听来自Client的传入流量，并将其转发到上游服务
- **8443**：与8000端口类似，它监听HTTPS流量
- **8001**：Admin API用于配置Kong监听
- **8444**：Admin API监听HTTPS流量



<br/>
<br/>



### 停止和重载

```bash
kong stop


# 在不停机的情况下重新加载Kong
kong reload
```




<br/>
<br/>
<br/>




## 配置服务

[Configuring a Service](https://docs.konghq.com/1.1.x/getting-started/configuring-a-service/)


在本章中，你将向Kong添加API。为此，你首先需要添加一个**服务（Service）**，这就是Kong又来指代它管理的上游API和微服务的名称。

出于测试的目的，将创建一个指向Mockbin API（返回请求作为响应）的服务。这有助于了解Kong如何代理你的API请求。

在开始向服务发出请求之前，你需要为其添加**路由（Route）**。路由指定请求在到达Kong后如何（以及是否）发送到服务。单个服务可以有多个路由。

在配置了服务和路由之后，你能够通过Kong使用它们发出请求。

Kong在端口`8001`上公开RESTful Admin API。Kong的配置（包括添加服务和路由），是通过该API的请求进行的。



<br/>
<br/>



### 添加服务

Add your Service using the Admin API


```bash
# 发出以下cURL请求，将服务添加到Kong
curl -i -X POST \
  --url http://localhost:8001/services/ \
  --data 'name=example-service' \
  --data 'url=http://mockbin.org'


# 你应该收到类似的响应
HTTP/1.1 201 Created
Content-Type: application/json
Connection: keep-alive

{
   "host":"mockbin.org",
   "created_at":1519130509,
   "connect_timeout":60000,
   "id":"92956672-f5ea-4e9a-b096-667bf55bc40c",
   "protocol":"http",
   "name":"example-service",
   "read_timeout":60000,
   "port":80,
   "path":null,
   "updated_at":1519130509,
   "retries":5,
   "write_timeout":60000
}
```



<br/>
<br/>



### 为服务添加路由

Add a Route for the Service


```bash
curl -i -X POST \
  --url http://localhost:8001/services/example-service/routes \
  --data 'hosts[]=example.com'


# 类似的响应
HTTP/1.1 201 Created
Content-Type: application/json
Connection: keep-alive

{
   "created_at":1519131139,
   "strip_path":true,
   "hosts":[
      "example.com"
   ],
   "preserve_host":false,
   "regex_priority":0,
   "updated_at":1519131139,
   "paths":null,
   "service":{
      "id":"79d7ee6e-9fc7-4b95-aa3b-61d2e17e7516"
   },
   "methods":null,
   "protocols":[
      "http",
      "https"
   ],
   "id":"f9ce2ed7-c06e-4e16-bd5d-3a82daef3f9d"
}
```

Kong现在知道你的服务并准备代理请求。



<br/>
<br/>



### 转发请求

Forward your requests through Kong


```bash
# 发出一下cURL请求来验证Kong是否正确地将请求转发给服务
# 默认情况下，Kong在8000端口上处理代理请求
curl -i -X GET \
  --url http://localhost:8000/ \
  --header 'Host: example.com'
```




<br/>
<br/>
<br/>




## 启用插件

[Enabling Plugins](https://docs.konghq.com/1.1.x/getting-started/enabling-plugins/)


在本章中，你将学习如何配置Kong**插件（Plugins）**。Kong的核心原则之一是它通过插件的可扩展性。插件允许你轻松地向服务添加新功能或使其更易于管理。

下面的栗子中，将配置`key-auth`插件以向服务添加身份认证。



<br/>
<br/>



### 配置插件

Configure the key-auth plugin


```bash
# 插件配置栗子
curl -i -X POST \
  --url http://localhost:8001/services/example-service/plugins/ \
  --data 'name=key-auth'
```



<br/>
<br/>



### 验证插件

Verify that the plugin is properly configured


```bash
curl -i -X GET \
  --url http://localhost:8000/ \
  --header 'Host: example.com'


# 由于未指定key，因此响应为 401 Unauthorized
HTTP/1.1 401 Unauthorized
...

{
  "message": "No API key found in request"
}
```




<br/>
<br/>
<br/>




## 添加消费者

[Adding Consumers](https://docs.konghq.com/1.1.x/getting-started/adding-consumers/)


在本章中，将介绍将**消费者（Consumer）**添加到Kong实例中。消费者与使用服务的个人相关联，并可用于追踪，访问管理等。


<br/>


### 创建消费者

Create a Consumer through the RESTful API


```bash
# 创建一个名为Jason的用户
# Kong还接受custom_id参数
curl -i -X POST \
  --url http://localhost:8001/consumers/ \
  --data "username=Jason"


# 类似响应
HTTP/1.1 201 Created
Content-Type: application/json
Connection: keep-alive

{
  "username": "Jason",
  "created_at": 1428555626000,
  "id": "bbdf1c48-19dc-4ab7-cae0-ff4f59d87dc9"
}
```



<br/>
<br/>



### 为消费者提供key

Provision key credentials for your Consumer


```bash
# 为消费者创建密钥
curl -i -X POST \
  --url http://localhost:8001/consumers/Jason/key-auth/ \
  --data 'key=ENTER_KEY_HERE'
```


<br/>
<br/>



### 验证消费者凭证

 Verify that your Consumer credentials are valid


```bash
# 发出请求来验证凭据是否有效
curl -i -X GET \
  --url http://localhost:8000 \
  --header "Host: example.com" \
  --header "apikey: ENTER_KEY_HERE"
```


















<br/>
<br/>

---

<br/>
<br/>











# 安装

[install](https://konghq.com/install/)


Kong可以运行在多个环境中。


<br/>
<br/>


## CentOS

[CentOS Installation](https://docs.konghq.com/install/centos/)



- **安装Kong**

安装方式：

- yum repo
- packages

```bash
# repo
wget https://bintray.com/kong/kong-rpm/rpm -O bintray-kong-kong-rpm.repo
export major_version=`grep -oE '[0-9]+\.[0-9]+' /etc/redhat-release | cut -d "." -f1`
sed -i -e 's/baseurl.*/&\/centos\/'$major_version''/ bintray-kong-kong-rpm.repo
sudo mv bintray-kong-kong-rpm.repo /etc/yum.repos.d/
sudo yum install -y kong
```

<br>

- **准备数据库**

Kong支持PostgreSQL v9.5+和Cassandra 3.x.x作为数据存储。

此处我按照文档安装PostgreSQL v11: <https://www.postgresql.org/download/linux/redhat/>

```bash
# 安装PostgreSQL v11
sudo yum install -y https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-centos11-11-2.noarch.rpm

sudo yum install -y postgresql11

sudo yum install -y postgresql11-server

# 自启
/usr/pgsql-11/bin/postgresql-11-setup initdb
systemctl enable postgresql-11
systemctl start postgresql-11
```

```
# 登录psql
sudo su postgres
psql

# 创建数据库，官方默认无密码，此处我使用密码
# CREATE USER kong; CREATE DATABASE kong OWNER kong;
CREATE USER kong with password 'kong'; CREATE DATABASE kong OWNER kong; grant all privileges on database kong to kong;


# 这里可能会报连接错误
# psql: 致命错误:  对用户"kong"的对等认证失败
sudo find / -name pg_hba.conf
/var/lib/pgsql/11/data/pg_hba.conf

# 修改安全配置
vim /var/lib/pgsql/11/data/pg_hba.conf

# METHOD指定如何处理客户端的认证。常用的有ident，md5，password，trust，reject
# ident是Linux下PostgreSQL默认的local认证方式，凡是能正确登录服务器的操作系统用户（注：不是数据库用户）就能使用本用户映射的数据库用户不需密码登录数据库。
# md5是常用的密码认证方式，如果你不使用ident，最好使用md5。密码是以md5形式传送给数据库，较安全，且不需建立同名的操作系统用户。
# password是以明文密码传送给数据库，建议不要在生产环境中使用。
# trust是只要知道数据库用户名就不需要密码或ident就能登录，建议不要在生产环境中使用。
# reject是拒绝认证。

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident

# 将peer改为md5（）
# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident


# 重启psql
systemctl restart postgresql-11


# 登录postgre
psql -U kong
# 输入密码


# 查看帮助
\h

# 退出
\q
```

```bash
# 这里需要提前配置kong配置文件，默认/etc/kong/kong.conf.default
cp /etc/kong/kong.conf.default /etc/kong/kong.conf

# 修改里面的数据库配置，写入用户、密码、数据库、端口等信息
vim /etc/kong/kong.conf


# Kong migrations
kong migrations bootstrap [-c /path/to/kong.conf]
```

<br>

- **启动Kong**

```bash
kong start [-c /path/to/kong.conf]
```

<br>

- **使用Kong**

```bash
curl -i http://localhost:8001/
```




<br/>
<br/>
<br/>




## Docker

[Docker Installation](https://docs.konghq.com/install/docker/)


以下是一个快速示例。

- **Create a Docker network**

```bash
docker network create kong-net
```

<br>

- **Start your database**

```bash
# PostgreSQL
docker run -d --name kong-database \
               --network=kong-net \
               -p 5432:5432 \
               -e "POSTGRES_USER=kong" \
               -e "POSTGRES_DB=kong" \
               postgres:9.6


# or Cassandra
docker run -d --name kong-database \
               --network=kong-net \
               -p 9042:9042 \
               cassandra:3
```

<br>

- **Prepare your database**

```bash
docker run --rm \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
     kong:latest kong migrations bootstrap
```

<br>

- **Start Kong**

```bash
docker run -d --name kong \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     kong:latest
```

<br>

- **Use Kong**

```bash
curl -i http://localhost:8001/
```




<br/>
<br/>
<br/>




## k8s

[Kong on Kubernetes](https://docs.konghq.com/install/kubernetes/)
















<br/>
<br/>

---

<br/>
<br/>



























# 无数据库和声明性配置

[DB-less and Declarative Configuration](https://docs.konghq.com/1.1.x/db-less-and-declarative-config/)


传统上，Kong总是需要一个数据库（如PostgreSQL或Cassandra），来存储其配置的实体（如服务、路由和插件）。
Kong使用其配置文件（`kong.conf`）来指定各种设置。

Kong v1.1增加了在没有数据库的情况下运行Kong的能力，仅对实体使用内存存储。—— 称之为**无数据库模式（DB-less mode）**。当运行KOng无数据模式时，实体的配置是使用**声明性配置（declarative configuration）**（YAML或JSON）在第二本配置文件中完成的。

无数据库模式和声明性配置的组合具有许多优点：

- 减少依赖的数量： 如果用例的整个设置适合内存，则无需管理数据库安装
- 非常适合CI/CD场景中的自动化：实体配置可以保存在通过Git Repo管理的单一源中
- 为Kong提供了更多的部署选项：在Service Mesh场景中非常适合轻量级Sidecar



<br/>
<br/>



## 声明性配置

What Is Declarative Configuration


正如其名称所言，声明性配置中的关键思想是陈述它是声明性（declarative）的，而不是命令式（ imperative style）配置。
**Imperative**意味着配置是作为一系列顺序给出的：做这做那。
**Declarative**意味着配置一次全部给出：宣布这是世界的状态。

Kong Admin API是命令式配置工具的一个栗子：配置的最终状态是通过一系列API调用获得。一个调用创建服务，一个调用创建路由，另一个调用添加插件...

像这样递增地执行具有中间状态的配置，会发生不期望的副作用。如在创建路由和添加插件之间存在时间窗口，其中路由没有应用插件。

另一方面，声明性配置文件将包含单个文件中所有所需实体的设置，并且一旦将改配置加载到Kong中，它将替换整个配置。当需要增量更改时，将对声明性配置文件进行更改，然后将其完整地重新加载。在任何时候，加载到Kong中的文件中描述的配置是系统的配置状态。



<br/>
<br/>



## 在无数据库模式配置Kong

Setting Up Kong in DB-less mode


要在无数据库模式下使用Kong，有两种方式：

- 修改配置文件`kong.conf`
- 修改环境变量`KONG_DATABASE`

```bash
vim /etc/kong/kong.conf

# database = postgres
database=off


# 或
export KONG_DATABASE=off



kong start -c /etc/kong/kong.conf
```

一旦Kong启动，访问Admin API的`/`根端点已验证它是否在没有数据库的情况下运行。

```bash
# httpie: <https://github.com/jakubroztocil/httpie>
$ http :8001/

HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 6342
Content-Type: application/json; charset=utf-8
Date: Wed, 27 Mar 2019 15:24:58 GMT
Server: kong/1.1.0
{
    "configuration:" {
       ...
       "database": "off",
       ...
    },
    ...
    "version": "1.1.0"
}
```

Kong正在运行，但尚未加载声明性配置。这意味着此节点的配置为空。没有任何类型的路由、服务或实体。

```bash
# httpie
http :8001/routes

HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 23
Content-Type: application/json; charset=utf-8
Date: Tue, 14 May 2019 06:58:37 GMT
Server: kong/1.1.2

{
    "data": [],
    "next": null
}
```



<br/>
<br/>



## 创建声明性配置文件

Creating a Declarative Configuration File


要将实体载入到无数据库的Kong，我们需要一个声明性配置文件。

```bash
# 此命令在当前目录中创建kong.yml文件
kong config -c kong.conf init

ls .
kong.yml
```



<br/>
<br/>



## 声明性配置格式

The Declarative Configuration Format


Kong声明性配置格式由实体列表及其属性组成。

看看`kong.yml`文件：

```yaml
# ------------------------------------------------------------------------------
# This is an example file to get you started with using
# declarative configuration in Kong.
# ------------------------------------------------------------------------------

# Metadata fields start with an underscore (_)
# Fields that do not start with an underscore represent Kong entities and attributes

# _format_version is mandatory,
# it specifies the minimum version of Kong that supports the format

_format_version: "1.1"

# Each Kong entity (core entity or custom entity introduced by a plugin)
# can be listed in the top-level as an array of objects:

# services:
# - name: example-service
#   url: http://example.com
#   # Entities can store tags as metadata
#   tags:
#   - example
#   # Entities that have a foreign-key relationship can be nested:
#   routes:
#   - name: example-route
#     paths:
#     - /
#   plugins:
#   - name: key-auth
# - name: another-service
#   url: https://example.org

# routes:
# - name: another-route
#   # Relationships can also be specified between top-level entities,
#   # either by name or by id
#   service: example-service
#   hosts: ["hello.com"]

# consumers:
# - username: example-user
#   # Custom entities from plugin can also be specified
#   # If they specify a foreign-key relationshp, they can also be nested
#   keyauth_credentials:
#   - key: my-key
#   plugins:
#   - name: rate-limiting
#     _comment: "these are default rate-limits for user example-user"
#     config:
#       policy: local
#       second: 5
#       hour: 10000

# When an entity has multiple foreign-key relationships
# (e.g. a plugin matching on both consumer and service)
# it must be specified as a top-level entity, and not through
# nesting.

# plugins:
# - name: rate-limiting
#   consumer: example-user
#   service: another-service
#   _comment: "example-user is extra limited when using another-service"
#   config:
#     hour: 2
#   # tags are for your organization only and have no meaning for Kong:
#   tags:
#   - extra_limits
#   - my_tag
```

唯一必须声明的元数据是`_format_version`，它指定声明性配置语法格式的版本号。这也匹配解析文件所需的Kong的最小版本。



<br/>
<br/>



## 检查声明性配置文件

Checking The Declarative Configuration File


编辑完文件后，可在将声明性配置文件加载到Kong之前检查任何语法错误。

```bash
# parse <file>，Parse a declarative config file (check its syntax) but do not load it into Kong.
# kong config -c kong.conf parse kong.yml
kong config parse kong.yml

parse successful
```



<br/>
<br/>



## 加载声明性配置文件

Loading The Declarative Configuration File


有两种方式可将声明性配置文件加载到Kong：

- 通过`kong.conf`
- 通过Admin API

```bash
# kong.conf

# 修改环境变量
export KONG_DATABASE=off
export KONG_DECLARATIVE_CONFIG=kong.yml

# 或修改配置文件
database=off
declarative_config=kong.yml


kong start -c /etc/kong/kong.conf
```

```bash
# 使用/根端点通过其Admin API将声明性配置加载在正在运行的Kong节点中
# 栗子使用httpie加载kong.yml
http :8001/config config=@kong.yml

HTTP/1.1 201 Created
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 2
Content-Type: application/json; charset=utf-8
Date: Tue, 14 May 2019 06:59:57 GMT
Server: kong/1.1.2
```



<br/>
<br/>



## 在无数据库模式下使用Kong

Using Kong in DB-less Mode


在无数据库模式下使用Kong时，有许多事项需要注意。


<br/>


### 内存缓存要求

Memory Cache Requirements

实体的整个配置必须适合Kong Cache。确保正确配置了内存缓存（memory cache）。

```bash
grep 'mem_cache_size' /etc/kong/kong.conf

#mem_cache_size = 128m
```


<br/>
<br/>


### 只读Admin API

Read-Only Admin API

由于配置实体的唯一方法是通过声明性配置文件，因此在无数据库模式下运行Kong时，实体上的CRUD操作的端点在Admin API中实际上是只读的。`GET`操作正常工作，但在端点(/services, /plugins...)中`POST, PATCH, PUT, DELETE`将返回`HTTP 405 Not Allowed`。

此限制仅限于数据库操作。特别是，仍然启用`POST`来设置目标的运行状态，因为这是特定节点的内存中的操作。


<br/>
<br/>


### 插件兼容性

Plugin Compatibility

并非所有的Kong插件斗鱼无数据库模式兼容。因为其中一些插件需要中央数据库协调或动态创建实体。

<br>

**完全兼容（Fully Compatible）**
以下插件只从数据库中读取（大多是读取初始配置），因此与无数据库模式完全兼容。

- `aws-lambda`
- `azure-functions`
- `bot-detection`
- `correlation-id`
- `cors`
- `datadog`
- `file-log`
- `http-log`
- `tcp-log`
- `udp-log`
- `syslog`
- `ip-restriction`
- `prometheus`
- `zipkin`
- `request-transformer`
- `response-transformer`
- `request-termination`
- `kubernetes-sidecar-injector`

<br>

**部分兼容（Partial Compatibility）**
只要使用的凭证集是静态的并且指定为声明性配置的一部分，就可以使用认证插件。在无数据库模式下，无法使用Admin API端点来动态创建、更新或删除凭据。属于此类的插件有：

- `acl`
- `basic-auth`
- `hmac-auth`
- `jwt`
- `key-auth`

与Kong捆绑在一起的速率限制插件提供了不同的策略来存储可协调计数器：

- `Local`策略：用于存储计数器节点的内存，以每个节点的方式应用限制；
- `Redis`策略：使用Redis作为外部键值存储来协调跨节点的计数器；
- `Cluster`策略：使用Kong数据库作为集群范围限制的中心协调点。

在无数据库模式下，Local和Redis策略可用，无法使用Cluster策略。属于此类的插件有：

- `rate-limiting`
- `response-ratelimiting`

无服务器（serverless）的`pre-function`和`post-function`可在无数据库模式下使用。但需注意，如果任何已配置的功能尝试写入数据库，则写入将失败。

<br>

**不兼容（Not Compatible）**

- `oauth2`：对于常规工作，插件需生成和删除token，并将这些更改提交到数据库














<br/>
<br/>

---

<br/>
<br/>











# 流和服务网格

[Streams and Service Mesh](https://docs.konghq.com/1.1.x/streams-and-service-mesh/)


Kong v1.0.0增加了代理(proxy)和路由(route)原始TCP和TLS流(stream)的能力，并使用service-mesh sidecar和在Kong节点之间交互TLS来部署Kong。
本章将介绍使用简单工具简化Service Mesh部署的基本设置： 两台服务器，通过两个Kong节点在一台主机中互相通信。如果你有兴趣使用k8s运行Service Mesh，请查看[Kubernetes and Service Mesh example](https://github.com/Kong/kong-mesh-dist-kubernetes)

Kong支持逐步部署sidecar。它即可以作为传统网关，也可作为服务网格节点同时工作。在Kong中，服务网格是动态构建的，只有在Kong节点之间存在活动连接时才存在。简而言之，这意味着Kong节点不必了解其它Kong节点，服务也不必了解Kong。


<br/>


## 先决条件

Prerequisites


需使用Kong v1.1.0+来运行不同的部署方案。建议使用Linux发行版来演示。系统上需要的一些工具：

- `ncat`(`nmap`)
- `iptables`
- `curl`

你的主机需要监控`lo0`网络适配器绑定到这些ip：

- `127.0.0.1` (Host C running Kong Control Plane)
- `127.0.0.2` (Host A running Service A)
- `127.0.0.3` (Host A running Kong A)
- `127.0.0.4` (Host B running Kong B)
- `127.0.0.5` (Host B running Service B)

本章的教程在单个主机上运行所有内容。为了简单起见，我们还是用IP地址而不是DNS来配置所有内容。
对于某些更改，可能需要root权限。



<br/>
<br/>
<br/>




## 术语和定义

Terms and Definitions


- **Kong Control Plane**： 在`127.0.0.1`上启动。它在`8001`和`8444`上监听Kong Admin API，它不代理任何流量；
- **Service A**：假象的业务实体（微服务），使网络连接到Service B；
- **Service B**：接受来自Service A的网络连接的业务实体；
- **Kong A**：它是服务A前端的sidecar代理。它不监听和提供Kong Admin API；
- **Kong B**：它是服务B前端的sidecar代理。它不监听和提供Kong Admin API；




<br/>
<br/>
<br/>




## 步骤1：启动服务B

Step1: Start Service B


启动Service B监听TCP流量：

```bash
$ ncat --listen \
       --keep-open \
       --verbose \
       --sh-exec "echo 'Hello from Service B (TCP)'" \
       127.0.0.5 19000

# 输出
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Listening on 127.0.0.5:19000
```

<br>

让命令继续运行。开一个新控制台并启动Service B监听TLS流量。

```bash
$ ncat --listen \
       --keep-open \
       --verbose \
       --ssl \
       --sh-exec "echo 'Hello from Service B (TLS)'" \
       127.0.0.5 19443

# 输出
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Listening on 127.0.0.5:19443
```

<br>

让它继续运行。开一个新控制台并启动Service B监听HTTP流量。

```bash
ncat --listen \
     --keep-open \
     --verbose \
     --sh-exec "echo 'HTTP/1.1 200 OK\r\n\r\nHello from Service (HTTP)'" \
     127.0.0.5 18000

# 输出
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Listening on 127.0.0.5:18000
```

<br>

让它继续运行，开一个新控制台并启动Service B监听HTTPS流量。

```bash
ncat --listen \
     --keep-open \
     --verbose \
     --ssl \
     --sh-exec "echo 'HTTP/1.1 200 OK\r\n\r\nHello from Service B (HTTPS)'" \
     127.0.0.5 18443

# 输出
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Generating a temporary 1024-bit RSA key. Use --ssl-key and --ssl-cert to use a permanent one.
Ncat: SHA-1 fingerprint: 9B41 1A89 664A 434A 35A7 6DFA 9540 3D21 9466 46D1
Ncat: Listening on 127.0.0.5:18443
```

同样，保持命令继续运行。
此时，应该有4个ncat进程在运行，表示Service B使用不同的协议进行监听。




<br/>
<br/>
<br/>




## 步骤2：确保服务A可以连接到服务B

Step2: Ensure that Service A can connect Service B


Service A直接调用`ncat`和`curl`。

<br>

使用TCP与Service B连接。

```bash
ncat --source 127.0.0.2 --recv-only 127.0.0.5 19000

# 输出
Hello from Service B (TCP)
```

<br>

使用TLS与Service B连接。

```bash
ncat --source 127.0.0.2 --recv-only --ssl 127.0.0.5 19443

# 输出
Hello from Service B (TLS)
```

<br>

使用HTTP与Service B连接。

```bash
curl --interface 127.0.0.2 http://127.0.0.5:18000

# 输出
Hello from Service B (HTTP)
```

<br>

使用HTTPS与Service B连接。

```bash
curl --interface 127.0.0.2 --insecure https://127.0.0.5:18443

# 输出
Hello from Service B (HTTPS)
```

服务A运行在`127.0.0.2`并可以直接连接到服务B。




<br/>
<br/>
<br/>




## 步骤3：启动Kong控制面板

Step 3: Start Kong Control Plane


```bash
# 启动金监听Kong Admin API的Kong节点
KONG_PREFIX=kong-c \
KONG_LOG_LEVEL=debug \
KONG_STREAM_LISTEN="off" \
KONG_PROXY_LISTEN="off" \
KONG_ADMIN_LISTEN="127.0.0.1:8001, 127.0.0.1:8444 ssl" \
  kong start

# 输出
Kong started
```




<br/>
<br/>
<br/>




## 步骤4：启动Kong A

Step4: Start Kong A


Kong A将是一个作为Service A的sidecar的Kong实例。

```bash
KONG_PREFIX=kong-a \
KONG_LOG_LEVEL=debug \
KONG_STREAM_LISTEN="127.0.0.3:9000 transparent, 127.0.0.3:9443 transparent" \
KONG_PROXY_LISTEN="127.0.0.3:8000 transparent, 127.0.0.3:8443 ssl transparent" \
KONG_ADMIN_LISTEN="off" \
KONG_NGINX_PROXY_PROXY_BIND="127.0.0.3" \
KONG_NGINX_SPROXY_PROXY_BIND="127.0.0.3" \
  kong start

# 输出
Kong started
```

<br>

**关于`transparent`选项**

`transparent`选项使Kong可使用`iptables PREROUTING`规则的请求，并在`iptables`堆栈透明地将其代理到其sidecar代理之前读取原始目标地址和客户端尝试连接的端口。




<br/>
<br/>
<br/>




## 步骤5：启动Kong B

Step 5: Start Kong B


Kong B将是一个作为Service B的sidecar的Kong实例。

```bash
KONG_PREFIX=kong-b \
KONG_LOG_LEVEL=debug \
KONG_STREAM_LISTEN="127.0.0.4:9000 transparent, 127.0.0.4:9443 transparent" \
KONG_PROXY_LISTEN="127.0.0.4:8000 transparent, 127.0.0.4:8443 transparent ssl" \
KONG_ADMIN_LISTEN="off" \
KONG_NGINX_PROXY_PROXY_BIND="127.0.0.4" \
KONG_NGINX_SPROXY_PROXY_BIND="127.0.0.4" \
  kong start

# 输出
Kong started
```




<br/>
<br/>
<br/>




## 步骤6：创建Kong服务和路由

Step 6: Create Kong Services and Routes


```bash
# 为Service B的TCP流量创建Kong服务
curl -X PUT \
       -d url=tcp://127.0.0.5:19000 \
       http://127.0.0.1:8001/services/service-b-tcp


# 为服务创建路由
curl -X POST \
       -d name=service-b-tcp \
       -d protocols=tcp \
       -d destinations[1].ip=127.0.0.5 \
       -d destinations[1].port=19000 \
       http://127.0.0.1:8001/services/service-b-tcp/routes
```

<br>

```bash
# 为Service B的TLS流量创建服务
curl -X PUT \
       -d url=tls://127.0.0.5:19443 \
       http://127.0.0.1:8001/services/service-b-tls


# 添加路由
curl -X POST \
       -d name=service-b-tls \
       -d protocols=tls \
       -d destinations[1].ip=127.0.0.5 \
       -d destinations[1].port=19443 \
       http://127.0.0.1:8001/services/service-b-tls/routes
```

<br>

```bash
# 为Service B的HTTP流量创建服务
curl -X PUT \
       -d url=http://127.0.0.5:18000 \
       http://127.0.0.1:8001/services/service-b-http


# 添加路由
curl -X POST \
       -d name=service-b-http \
       -d protocols=http \
       -d hosts=127.0.0.5 \
       http://127.0.0.1:8001/services/service-b-http/routes
```

<br>

```bash
# 为Service B的HTTPS流量创建服务
curl -X PUT \
       -d url=https://127.0.0.5:18443/ \
       http://127.0.0.1:8001/services/service-b-https


# 添加路由
curl -X POST \
       -d name=service-b-https \
       -d protocols=https \
       -d hosts=127.0.0.5 \
       http://127.0.0.1:8001/services/service-b-https/routes
```




<br/>
<br/>
<br/>




## 步骤7：配置代理规则

Step 7: Configure Transparent Proxying Rules






















<br/>
<br/>

---

<br/>
<br/>













# 参考

Reference


<br/>


## 配置

[Configuration Reference](https://docs.konghq.com/1.1.x/configuration/)


<br/>


### 配置加载

Configuration loading


如果通关官方软件包安装Kong，则可以在`/etc/kong/kong.conf.default`找到此默认文件。

如果配置文件中的所有值都被注释掉，Kong将使用默认配置运行。为方便起见，可将布尔值指定为`on/off`或`true/false`。

```bash
# 配置Kong
cp /etc/kong/kong.conf.default /etc/kong/kong.conf


# 启动时，Kong会查找可能包含配置文件的多个默认位置
/etc/kong.conf
/etc/kong/kong.conf


# 通过-c/--conf参数指定自定义配置文件来覆盖默认行为
kong start --conf /path/to/kong.conf
```



<br/>
<br/>



### 验证配置

Verifying your configuration


你可以通过命令验证配置的完整性：

```bash
kong check /path/to/kong.conf


# debug模式
kong start -c /path/to/kong.conf --vv
```



<br/>
<br/>



### 环境变量

Environment variables


从配置文件中加载属性时，Kong还是查找同名的环境变量。这允许你通过环境变量配置Kong，这对于容器结构非常方便。
要使用环境变量覆盖配置，请设置声明环境变量，如`KONG_XXX`。

```bash
# kong.conf
log_level = debug


# env
export KONG_LOG_LEVEL=error
```



<br/>
<br/>



### 注入Nginx指令

Injecting Nginx directives


通过调整Kong实例的Nginx配置，你可以优化其基础架构的性能。

当Kong启动时，它会构建一个Nginx配置文件。你可通过Kong配置直接将自定义Nginx指令注入此文件。


<br/>


#### 注入单个Nginx指令

Injecting individual Nginx directives

添加到`kong.conf`文件的任何以`nginx_http_`, `nging_proxy_`, `nginx_admin_`为前缀的条目将通过删除前缀并添加到Nginx配置的相应部分而转换为等效的Nginx指令。

- 带有`nginx_http_`前缀的条目将被注入http块；
- 带有`nginx_proxy_`前缀的条目将被注入处理Kong的代理端口的server块；
- 带有`nginx_admin_`前缀的条目将被注入处理Kong的Admin API端口的server块。


```bash
# 例如，将以下行添加到`kong.conf`文件：
nginx_proxy_large_client_header_buffers=16 128k
# 它将添加到Kong的Nginx 配置的代理server
large_client_header_buffers 16 128k;


# 或通过环境变量
export KONG_NGINX_HTTP_OUTPUT_BUFFERS="4 64k"
# 它将添加到nginx的http块
output_buffers 4 64k;
```

有关更多的Nginx指令，请参考[Nginx文档](https://nginx.org/en/docs/dirindex.html)。但请注意，某些执行依赖于特定Nginx模块，其中一些可能没有包含在Kong版本中。


<br/>
<br/>


#### 注入Nginx指令文件

Including files via injected Nginx directives

对于更复杂的配置，可将Nginx指令写入配置文件，然后将其注入Kong。

```bash
# 栗子，my-server.kong.conf
# custom server
server {
  listen 2112;
  location / {
    # ...more settings...
    return 200;
  }
}
```

```bash
# 在kong.conf配置文件中添加条目使kong节点服务此端口
nginx_http_include = /path/to/your/my-server.kong.conf

# 或通过环境变量
export KONG_NGINX_HTTP_INCLUDE="/path/to/your/my-server.kong.conf"
```

```bash
# 测试
curl -I http://127.0.0.1:2112
HTTP/1.1 200 OK
...
```



<br/>
<br/>



### 自定义Nginx模板并嵌入Kong

Custom Nginx templates and embedding Kong


对于绝大多数用例，使用上面的指令注入已足够定义Kong的Nginx实例的行为。这样，你可从单个`kong.conf`文件（以及自己包含的文件）管理Kong节点的配置和调优，而无需自定义Nginx配置模板。

有两种情况，你希望直接使用自定义的Nginx配置模板：

- 在极少数情况下，你可能需要修改一些不能通过其标准`kong.conf`属性调整的Kong默认的Nginx配置。你可以修改Kong用于生成器Nginx配置并使用你自定义的模板启动Kong的模板；
- 如果需要在已经运行的OpenResty实例中嵌入Kong，则可重用Kong生成的配置并将其包含在现有配置中。


<br/>


#### 自定义Nginx模板

Custom Nginx templates

可使用`--nginx-conf`参数启动、重载和重启Kong，该参数必须指定Nginx配置模板。这样的模板使用Penlight模板引擎，该引擎使用给定的Kong配置进行编译，然后在启动Nginx之前将其转储到Kong前缀目录中。

默认模板可在GitHub上查看: <https://github.com/kong/kong/tree/master/kong/templates>。它分为两个Nginx配置文件`nginx.lua`和`nginx_kong.lua`。当`kong start`运行时，它会将这两个文件复制到前缀目录中，如下所示:

```bash
/usr/local/kong
├── nginx-kong.conf
└── nginx.conf
```

如果你必须调整Kong定义但不能通过`kong.conf`配置的全局设置，你可将`nginx_kong.lua`配置模板的内容内联到一个自定义模板文件，如下：

```conf
# ---------------------
# custom_nginx.template
# ---------------------

worker_processes ${{NGINX_WORKER_PROCESSES}}; # can be set by kong.conf
daemon ${{NGINX_DAEMON}};                     # can be set by kong.conf

pid pids/nginx.pid;                      # this setting is mandatory
error_log logs/error.log ${{LOG_LEVEL}}; # can be set by kong.conf

events {
    use epoll;          # a custom setting
    multi_accept on;
}

http {

  # contents of the nginx_kong.lua template follow:

  resolver ${{DNS_RESOLVER}} ipv6=off;
  charset UTF-8;
  error_log logs/error.log ${{LOG_LEVEL}};
  access_log logs/access.log;

  ... # etc
}
```

```bash
# 启动
kong start -c kong.conf --nginx-conf custom_nginx.template
```



<br/>
<br/>



### 在OpenResty中嵌入Kong

Embedding Kong in OpenResty


如果您正在运行自己的OpenResty Server，您还可以通过使用`include`指令包含Kong Nginx自配置来轻松嵌入Kong。栗子：

```conf
# my_nginx.conf

# ...your nginx settings...

http {
    include 'nginx-kong.conf';

    # ...your nginx settings...
}
```

```bash
#启动Nginx实例
nginx -p /usr/local/openresty -c my_nginx.conf
```



<br/>
<br/>



### 同时提供来自Kong的Website和API

Serving both a website and your APIs from Kong


提供API的一个常见用例是让Kong通过代理端口(80/443)在生产中同时为Website和API提供服务。例如，`https://example.net`（Website）和`https://example.net/api/v1`（API）。
为了实现此目标，我们不能简单地声明一个新的虚拟`server`块。一个好的解决办法是使用自定义Nginx配置模板，该模板内联`nginx_kong.lua`并添加一个新的`location`块，为Kong Proxy `location`块提供服务。

```conf
# ---------------------
# custom_nginx.template
# ---------------------

worker_processes ${{NGINX_WORKER_PROCESSES}}; # can be set by kong.conf
daemon ${{NGINX_DAEMON}};                     # can be set by kong.conf

pid pids/nginx.pid;                      # this setting is mandatory
error_log logs/error.log ${{LOG_LEVEL}}; # can be set by kong.conf
events {}

http {
  # here, we inline the contents of nginx_kong.lua
  charset UTF-8;

  # any contents until Kong's Proxy server block
  ...

  # Kong's Proxy server block
  server {
    server_name kong;

    # any contents until the location / block
    ...

    # here, we declare our custom location serving our website
    # (or API portal) which we can optimize for serving static assets
    location / {
      root /var/www/example.net;
      index index.htm index.html;
      ...
    }

    # Kong's Proxy location / has been changed to /api/v1
    location /api/v1 {
      set $upstream_host nil;
      set $upstream_scheme nil;
      set $upstream_uri nil;

      # Any remaining configuration for the Proxy location
      ...
    }
  }

  # Kong's Admin server block goes below
  # ...
}
```



<br/>
<br/>



### 属性

Properties reference


<br/>


#### GENERAL


```conf
#------------------------------------------------------------------------------
# GENERAL
#------------------------------------------------------------------------------


# 工作目录。相当于Nginx的前缀路径，包含临时文件和日志
# 每个Kong进程必须有一个单独的工作目录
prefix = /usr/local/kong/  #default


# Nginx Server的日志级别，位于<prefix>/logs/error.log
log_level = notice  #default


# 代理端口的访问日志路径。设置为off可禁用
# 如果此值是相对路径，则它位于<prefix>
proxy_access_log = logs/access.log  #default

# 代理端口的错误日志路径
proxy_error_log = logs/error.log  #default


# Admin API请求访问的日志路径
admin_access_log = logs/admin_access.log  #default

# Admin API请求错误的日志路径
admin_error_log = logs/error.log



# plugins
# 加载以逗号分隔的插件列表。默认情况下，只有捆绑在官方发行版中的插件才会通过bundled关键字加载。
# 默认不启用加载插件
# 特定名称将在Lua命名空间中替换为：kong.plugins.{name}.*
# off关键字被指定为唯一时，不会加载任何插件
# bundled和plugin名称可以混合在一起，示例如下：
#  - plugins = bundled,custom-auth,custom-log
#  - plugins = custom-auth,custom-log
#  - plugins = off
# 在禁用插件之前，请确保在重新启动Kong之前删除它的所有实例
plugin = bundled  #default



# 发送匿名使用数据，如错误栈追踪，以帮助改善Kong
anonymous_reports = on  #default
```


<br/>
<br/>


#### NGINX

```conf
#------------------------------------------------------------------------------
# NGINX
#------------------------------------------------------------------------------



# 以逗号分隔的代理服务器应监听HTTP/HTTPS流量的地址和端口列表
# 代理服务器是Kong的公共入口点，它将来自消费者的流量代理到后端服务
# 此值接受IPv4, IPv6, hostname
# 可以为每对指定一些后缀：
#  - ssl，要求通过特定地址/端口建立的所有连接都在启用TLS的情况下进行
#  - http2，允许客户端打开到Kong代理服务器的HTTP2连接
#  - proxy_protocol，为给定的地址/端口启用PROXY协议
#  - transparent，Kong监听您在iptables中配置的任何IP 地址和端口，并进行响应
# 此值可设为off，从而禁用此节点的HTTP/HTTPS代理端口
# 栗子：proxy_listen = 0.0.0.0:443 ssl, 0.0.0.0:444 http2 ssl
proxy_listen = 0.0.0.0:8000, 0.0.0.0:8443 ssl  #default



# 流模式监听以逗号分隔的地址和端口列表
# 此值接受 IPv4, Ipv6, hostname
# 可为每队指定一些后缀：
#  - proxy_protocol，为给定的地址/端口启用PROXY协议
#  - transparent，Kong监听您在iptables中配置的任何IP 地址和端口，并进行响应
# 不支持ssl后缀，并且每个地址/端口都将接受启用/未启用TLS的TCP
# 栗子：
# stream_listen = 127.0.0.1:7000
# stream_listen = 0.0.0.0:989, 0.0.0.0:20
# stream_listen = [::1]:1234
# 默认设置为off，从而禁用此节点的流代理端口
stream_listen = off  #default



# Admin接口监听的地址和端口列表
# Admin API允许您配置和管理Kong
# 应仅限于Kong管理员访问此接口
# 此值接受 IPv4, IPv6, Hostname
# 可为每队执行一些后缀：
#  - ssl
#  - http2
#  - proxy_protocol
# 栗子
# stream_listen = 127.0.0.1:8444 http2 ssl
admin_listen = 127.0.0.1:8001, 127.0.0.1:8444 ssl  #default



# 定义工作进程使用的用户和组凭据
# 如果省略group，则默认为User一致
nginx_user = nobody nobody



# Nginx生成的工作进程数
nginx_worker_processes = auto  #default



# Nginx是作为守护进程还是前台进程运行
# 主要用于在Docker环境下运行Kong
nginx_daemon = on  #default



# 数据库实体的内存缓存大小
# 单位接收KB和MB，最小推荐值几MBs
mem_cache_size = 128m  #default



# 定义Nginx提供的TLS密码
# 可接受的值包括：modern, intermediate, old, custom
ssl_cipher_suite = modern  #default



# 定义由Nginx提供服务的TLS密码的自定义列表。此列表必须符合openssl密码定义的模式
ssl_ciphers =  #Default: none



# 启用SL的proxy_listen值的SSL证书的绝对路径
ssl_cert =  #Default: none



# 启用SSL的proxy_listen值的SSL Key的绝对路径
ssl_cert_key =  #Default: none



# 确定代理请求时Nginx是否应发送客户端SSL证书
client_ssl = off  #default



# 如果启用了client_ssl，则为proxy_ssl_certificate指令的客户端SSL证书的绝对路径
# 请注意，此值是在节点上静态定义的，并且当前无法基于每个API进行配置。
client_ssl_cert =  #Default: none



# 如果启用了client_ssl，则为proxy_ssl_certificate_key地址的客户端SSL密钥的绝对路径
# 请注意，此值是在节点上静态定义的，目前无法基于每个API进行配置
client_ssl_cert_key =  #Default: none



# 启用SSL的admin_listen值的SSL证书的绝对路径
admin_ssl_cert =  #Default: none



# 启用SSL的admin_listen值的SSL Key的绝对路径
admin_ssl_cert_key =  #Default: none



# 设置在每个工作进程的缓存中保留的上游服务器的最大空闲keepalive连接数。
# 超过此数量时，将关闭最近最少使用的连接。
# 值为0表示禁用此功能
upstream_keepalive = 60  #default



# 以逗号分隔的Header列表应该在客户端响应中注入
# 接受的值如下：
#  - Server，Server: kong/x.y.z
#  - Via，Via: kong/x.y.z
#  - X-Kong-Proxy-Latency，Kong代理上游请求之前处理并运行所有插件所花费的时间(ms)
#  - X-Kong-Upstream-Latency，上游服务发送响应头所花费的时间(ms)
#  - X-Kong-Upstream-Status，上游服务返回的HTTP状态码。如果响应被插件重写，这对于客户端区分上游状态特别有用
#  - server_tokens，与指定Server和Via相同
#  - latency_tokens，与指定X-Kong-Proxy-Latency和X-Kong-Upstream-Latency相同
headers = server_tokens, latency_tokens  #default



# 定义已知可发送正确的 X-Forwarded-* Header 的可信IP地址块
# 来自信任的IP的请求使Kong向上游转发了它们的 X-Forwarded-* Header
# 不受信任的请求使Kong插入它自己的 X-Forwarded-* headers
# 此属性还在Nginx配置中配置 set_real_ip_from 指令。它接受相同类型的值(CIDR)，以逗号分割的列表
# 要信任所有IP，将值设置为 0.0.0.0/0,::/0
# 如果指定为 unix:，则所有 UNIX-domain sockets都将被信任
trusted_ips =    #default: none



# 定义请求头字段，其值将用于替换客户端地址
# 此值设置Nginx配置中同名的 ngx_http_realip_module 指令
# 如果此值接收 proxy_protocol
#  - 至少有一个 proxy_listen 条目必须启用 proxy_protocol 标志
#  - proxy_ptotocol 参数将附加到Nginx模板的 listen 指令
real_ip_header = X-Real-IP  #default



# 此值设置Nginx配置中同名的ngx_http_realip_module指令
real_ip_recursive = off  #default



# 定义由Kong代理的请求所运行的最大请求主体大小，在Content-Length请求头中指定。如果请求超过此限制，Kong将响应413（Request Entity Too Large）
# 将此值设置为0禁用检查请求主体大小
client_max_body_size = 0    #default



# 定义用于读取请求正文的缓冲区大小。如果客户端请求正文（request body）大于此值，则正文（body）将被缓冲到磁盘
# 请注意，当主体缓冲到磁盘时，访问或操作请求主体可能无法正常工作，因此建议将此值设置得尽可能高。（例如，将其设置为与client_max_body_size一样，以强制保留请求主体在内存中）
# 请注意，高并发环境将需要大量内存分配来处理许多并发的大型请求
client_body_buffer_size = 8k  #default



# 缺少Request Accept Header且Nginx返回请求错误时要使用的默认MIME类型
# 接受的值有： text/plain, text/html, application/json, application/xml
error_default_type = text/plain  #default
```


<br/>
<br/>


#### Database

Kong把所有数据（如路由、服务、消费者、插件...）存储在Cassandra或PostgreSQL中，并且属于同一集群的所有Kong节点必须将它们自己连接到同一个数据库。

Kong支持的版本:

- PostgreSQL v9.5+
- Cassandra v2.2+

```conf
#------------------------------------------------------------------------------
# DATASTORE
#------------------------------------------------------------------------------



# 确定使用哪种存储
# 接受的值有：postgres、cassandra、off
database = postgres  #default



# Postgres settings
pg_host
pg_port
pg_timeout
pg_user
pg_password
pg_database
pg_schema
pg_ssl
pg_ssl_verify



# Cassandra settings
cassandra_contact_points
cassandra_port
cassandra_keyspace
cassandra_consistency
cassandra_timeout
cassandra_ssl
cassandra_ssl_verify
cassandra_username
cassandra_password
cassandra_lb_policy
cassandra_local_datacenter
cassandra_repl_strategy
cassandra_repl_factor
cassandra_data_centers
cassandra_schema_consensus_timeout
```


<br/>
<br/>


#### Datastore Cache

为了避免与数据存储进行不必要的通信，Kong可将实体缓存一段可配置的时间。如果更新了这样的实体，它还会处理失效。
本节允许配置Kong关于此类配置实体的缓存的行为。


```conf
#------------------------------------------------------------------------------
# DATASTORE CACHE
#------------------------------------------------------------------------------



# 使用数据存储检查更新实体的频率（单位s）
# 当节点通过Admin API创建，更新或删除实体时，其他节点需要等待下一轮询（由此值配置）以最终清除旧的缓存实体并开始使用新的实体
db_update_frequency = 5  #default



# 数据存储中的实体传播到另一个数据中心的副本节点所用的时间（单位s）
# 单数据中心或单节点没有这样的问题，可安全地设置为0
db_update_propagation = 0



# 此节点缓存时实体从数据存储区的ttl（单位s）
# 如果设置为0，则永不过期
db_cache_ttl = 0



# 数据存储区中的陈旧实体在无法刷新是的时间。当此TTL到期时，将进行刷新陈旧实体的新尝试
db_resurrect_ttl = 30
```


<br/>
<br/>


#### DNS Resolver

默认情况下，DNS解析器将使用标准配置文件`/etc/hosts`和`/etc/resolv.conf`。如果已设置环境变量`LOCALDOMAIN`和`RES_OPTIONS`，则后一个文件的设置将被覆盖。

Kong会将主机名解析为`SRV`或`A`记录。如果名称被解析为SRV记录，它还将通过从DNS服务器接收的端口字段内容覆盖任何给定的端口号。

在ttl的持续时间内，内部DNS解析器将对通过DNS记录中的条目获得的每个请求进行负载均衡。对于SRV记录，权重（weight）字段将被接受，但它将仅使用记录中的最低优先级（priority）字段条目。

```conf
#------------------------------------------------------------------------------
# DNS RESOLVER
#------------------------------------------------------------------------------



# 以逗号分隔的nameserver列表，每个条目都采用 ip[:port] 的格式供Kong使用。如果省略，端口默认为53。它接受IPv4和IPv6地址
# 如果未指定，将使用本地 resolv.conf 文件的配置
dns_resolver =  #default: none



# 要使用的hosts文件。此文件只读一次便加载为静态内存内容
# 要在修改后再次读取文件，必须重新加载Kong
dns_hostsfile = /etc/hosts  #default



# 解析不同记录类型的顺序
dns_order = LAST,SRV,A,CNAME  #default



# 默认情况下，使用响应的TTL值缓存DNS记录。如果此属性收到一个值（以秒为单位），它将覆盖所有记录的TTL
dns_valid_ttl =  #default: none



# 定义记录在缓存中保留的时间长度
dns_stale_ttl = 4  #default



# 空DNS响应和名称错误响应的TTL
dns_not_found_ttl = 30  #default



# 错误响应的TTL
dns_error_ttl = 1  #default



# 如果启用，则在缓存未命中时，每个请求都将触发其自己的dns查询
# 禁用时，同一名称/类型的多个请求将同步到单个查询
dns_no_sync = off
```


<br/>
<br/>


#### Development

从`lua-nginx-module`继承的其他设置允许更多的灵活性和高级用法。


<br/>
<br/>


#### Additional

- **origin**

**Origin Configuration**在复杂的网络配置中非常有用，并且在Kong用于**服务网格（Service Mesh）**通常是必需的。

`origins`是一个以逗号分隔的成对的对象列表，每对使用`=`。每对左侧的原（origin）被右侧的原(origin)覆盖。此覆盖发生在访问阶段之后和上游解析之前。它具有导致Kong将流向左侧origin的流量发送到右侧origin的效果。

术语 `origin` 是指特定 scheme/host或ip/port，如RFC 6454中所描述。在Kong的origin配置中，该方案必须是`http`, `https`, `tcp`, `tls`。在每对origin中，必须匹配。如http可与https配对，tcp可与tls配对，但http不能与tcp配对。

When an encrypted scheme like `tls` or `https` in the left origin is paired with an unencrypted scheme like `tcp` or `http` in the right origin, Kong will terminate TLS on incoming connections matching the left origin, and will then route traffic unencrypted to the specified right origin. This is useful when connections will be made to the Kong node over TLS, but the local service (for which Kong is proxying traffic) doesn’t or can’t terminate TLS. Similarly, if the left origin is `tcp` or `http` and the right origin is `tls` or https, Kong will accept unencrypted incoming traffic, and will then wrap that traffic in TLS as it is routed outbound. This capability is an important enabler of Kong Mesh.

与所有Kong配置设置一样，可在`kong.conf`文件中声明origin设置。但是，建议Kong管理员不要这么做。相反，应该使用环境变量在每个节点上设置origin。因此，默认的`kong.conf.default`中不存在origin。

<br>

栗子：

```
# 如果给定的Kong节点具有以下origin的配置
http://upstream-foo-bar:1234=http://localhost:5678
# Kong节点不会尝试解析upstream-foo-bar，而是将该节点路由到localhost:5678
# 在Kong的服务网格部署中，这种覆盖是必要的，以使邻近 upstream-foo-bar 应用程序的实例 Kong sidecar 将流量路由到本地实例，而不是试图将流量通过网络路由回到 upstream-foo-bar 的非本地实例



# 在另一个典型的sidecar部署中，Kong节点作为Kong代理服务的一个实例，origin将配置为
https://service-b:9876=http://localhost:5432
# 这将导致Kong节点仅接受端口9876上的https连接，终止tls，然后将现在未加密的流量转发到localhost:5432



# 下面这个栗子由两对组成，由逗号分隔
https://foo.bar.com:443=http://localhost:80,tls://dog.cat.org:9999=tcp://localhost:8888
# 这将导致Kong仅接受端口443上的https流量，并仅接受 端口9999的TLS流量，在两种情况下都终止TLS，然后分别将流量转发到localhost:80和localhost:8888
# 假设80和8888都与单独的服务相关，当Kong充当节点代理是，可能会发生这种配置，这是一个代表多个服务的本地代理
```




<br/>
<br/>
<br/>




## CLI

[CLI Reference](https://docs.konghq.com/1.1.x/cli/)


```bash
# Global flags
--help: print the command’s help message
--v: enable verbose mode
--vv: enable debug mode (noisy)


kong --help
No such command: --help

Usage: kong COMMAND [OPTIONS]

The available commands are:
 check
 config
 health
 migrations
 prepare
 quit
 reload
 restart
 roar
 start
 stop
 version

Options:
 --v              verbose
 --vv             debug
```




<br/>
<br/>
<br/>




## Proxy

[Proxy Reference](https://docs.konghq.com/1.1.x/proxy/)


本章我们将通过详细解释其路由功能(routing capabilities)和内部工作来涵盖Kong的代理功能(proxying capabilities)。

Kong公开了几个可以通过两个配置属性调整的接口:

- `proxy_listen`，默认是`8000`，接受来自客户端的公共流量(public traffic)并将其代理带上游服务；
- `admin_listen`，默认`8001`，Admin API被限制为仅由管理员访问。


<br/>
<br/>


### 术语

Terminology：

- `client`，向Kong代理端口发出请求的下游客户端；
- `upstream service`，位于Kong后面的API/Service，转发客户端请求到此；
- `Service`，服务实体是每个上游服务的抽象；
- `Route`，Kong路由实体。路由是进入Kong的入口点(entrypoints)，并定义要匹配的请求的规则，并路由到给定的服务；
- `Plugin`，Kong插件。它们是在代理生命周期中运行的业务逻辑。可通过Admin API配置全局/特定路由和服务的插件。



<br/>
<br/>



### 概述


从高层次的角度来看，Kong在其配置的代理和端口上监听HTTP流量(`8000/8443`)。Kong将根据你配置的路由评估任何传入的HTTP请求，并尝试查找匹配的路由。如果给定的请求与特定路由的规则匹配，Kong将处理代理请求。由于每个路由都链接(link)到一个服务，因此Kong将运行你在路由及相关服务上配置的插件，然后代理请求到上游。

你可以通过Kong Admin API管理路由。路由的`hosts, pahts, methods`属性定义用于匹配传入HTTP请求的规则。

如果Kong收到的请求无法与任何已配置的路由匹配（或没有配置路由），它将返回；

```
HTTP/1.1 404 Not Found
Content-Type: application/json
Server: kong/<x.x.x>

{
    "message": "no route and no Service found with those values"
}
```



<br/>
<br/>



### 如何配置服务

How to configure a Service


在快速开始指南里面介绍了如何通过Admin API配置Kong。
通过向Admin API发送HTTP请求来向Kong添加服务：

```bash
curl -i -X POST http://localhost:8001/services/ \
    -d 'name=foo-service' \
    -d 'url=http://foo-service.com'
HTTP/1.1 201 Created
...

{
    "connect_timeout": 60000,
    "created_at": 1515537771,
    "host": "foo-service.com",
    "id": "d54da06c-d69f-4910-8896-915c63c270cd",
    "name": "foo-service",
    "path": "/",
    "port": 80,
    "protocol": "http",
    "read_timeout": 60000,
    "retries": 5,
    "updated_at": 1515537771,
    "write_timeout": 60000
}
```

该请求指示Kong注册一个名为`foo-service`的服务，该服务指向`http://foo-service.com`（上游）。
`url`参数是一个简写的参数，用于一次填充`protocol, host, prot, path`属性。

<br>

现在，为了通过Kong向这个服务发送流量，我们需要指定一个路由，它作为Kong的入口点：

```bash
curl -i -X POST http://localhost:8001/routes/ \
    -d 'hosts[]=example.com' \
    -d 'paths[]=/foo' \
    -d 'service.id=d54da06c-d69f-4910-8896-915c63c270cd'
HTTP/1.1 201 Created
...

{
    "created_at": 1515539858,
    "hosts": [
        "example.com"
    ],
    "id": "ee794195-6783-4056-a5cc-a7e0fde88c81",
    "methods": null,
    "paths": [
        "/foo"
    ],
    "preserve_host": false,
    "priority": 0,
    "protocols": [
        "http",
        "https"
    ],
    "service": {
        "id": "d54da06c-d69f-4910-8896-915c63c270cd"
    },
    "strip_path": true,
    "updated_at": 1515539858
}
```

我们现在已经配置了一个路由来匹配与给定`hosts`和`path`匹配的传入请求，并将它们转发到我们配置的`foo-service`服务，从而将此流量代理到`http://foo-service.com`。

Kong是一个透明的代理，它默认情况下会原封不同的(untouched)将请求转发给上游服务，但HTTP规范要求使用各种表头(Header)（如`Connection, Date ...`）



<br/>
<br/>



### 路由和匹配

Routes and matching capabilities


现在让我们讨论Kong如何匹配针对路由的已配置的`hosts, paths, methods`属性的请求。请注意，所有这三个字段都是可选的，但必须指定其中一个。

对于匹配路由的请求：

- 请求必须包含所有已配置的字段；
- 请求中的字段值至少与其中一个配置值匹配（当字段配置接受一个或多个值时，请求只需其中一个值被视为匹配）。

<br>

举个栗子。考虑如下配置的路由：

```json
{
    "hosts": ["example.com", "foo-service.com"],
    "paths": ["/foo", "/bar"],
    "methods": ["GET"]
}
```

与此路由匹配的一些可能的请求如下：

```
GET /foo HTTP/1.1
Host: example.com


GET /bar HTTP/1.1
Host: foo-service.com


GET /foo/hello/world HTTP/1.1
Host: example.com
```

所有这三个请求都满足路径定义中设置的所有条件。

但是，以下请求与配置的路由条件不匹配：

```
# path不匹配
GET / HTTP/1.1
Host: example.com


# method不匹配
POST /foo HTTP/1.1
Host: example.com


# host不匹配
GET /foo HTTP/1.1
Host: foo.com
```

现在我们了解了`hosts, pahts, mehtods`属性如何协作工作，让我们分别探索每个属性。


<br/>
<br/>


#### 请求主机头

Request Host header

基于其Host Header路由请求是通过Kong代理流量的最直接的方式，尤其是因为这是HTTP Host Header的预期用途。Kong可以通过路由实体的`hosts`字段轻松完成。

`hosts`接受多个值，在通过Admin API指定它们时必须以逗号分隔。这些值很容易在JSON有效负载中表示：

```bash
curl -i -X POST http://localhost:8001/routes/ \
    -H 'Content-Type: application/json' \
    -d '{"hosts":["example.com", "foo-service.com"]}'
HTTP/1.1 201 Created
...
```

<br>

由于Admin API还支持**form-urlencoded**内容类型，因此还可通过`[]`表示法指定数组：

```bash
curl -i -X POST http://localhost:8001/routes/ \
    -d 'hosts[]=example.com' \
    -d 'hosts[]=foo-service.com'
HTTP/1.1 201 Created
...
```

<br>

要满足此路由的`hosts`条件，来自客户端的任何传入请求现在必须将Host Header设置为两者之一：

```yaml
Host: example.com

# or

Host: foo-service.com
```


<br/>
<br/>


##### 使用通配符主机名

Using wildcard hostnames

为了提供灵活性，Kong允许你在`hosts`字段中指定带通配符的主机名。通配符主机名允许任何匹配的Host Header满足条件，从而匹配给定的路由。

通配符主机名必须在域的最左侧或最右侧标签中仅包含一个星号(`*`)：

- `*.example.com`
  - `a.example.com`
  - `x.y.z.example.com`
  - ...
- `example.*`
  - `example.com`
  - `example.org`
  - ...

<br>

栗子:

```json
{
    "hosts": ["*.example.com", "service.com"]
}
```

匹配的路由栗子：

```
GET / HTTP/1.1
Host: an.example.com


GET / HTTP/1.1
Host: service.com
```


<br/>
<br/>


##### `preserve_host`属性

代理时，Kong的默认行为是将上游请求的Host Header设置为服务`host`中指定的主机名。`preserve_host`字段接受一个布尔标志，指示Kong不要这样做。

例如，当`preserve_host`属性未更改且路由配置如下：

```json
{
    "hosts": ["service.com"],
    "service": {
        "id": "..."
    }
}
```

客户端对Kong的可能请求：

```
GET / HTTP/1.1
Host: service.com
```

Kong从服务的`host`属性中提取Host Header，并将发送以下上游请求：

```
GET / HTTP/1.1
Host: <my-service-host.com>
```

<br>

但是，通过使用`preserve_host=true`显式配置路由：

```json
{
    "hosts": ["service.com"],
    "preserve_host": true,
    "service": {
        "id": "..."
    }
}
```

假设来自客户端的相同请求：

```
GET / HTTP/1.1
Host: service.com
```

Kong将根据客户端请求保留Host，并将发送以下上游请求：

```
GET / HTTP/1.1
Host: service.com
```


<br/>
<br/>


#### 请求路径

Request path

路由匹配的另一种方式是通过请求路径。要满足此路由条件，客户端请求的路径必须以`paths`属性的值之一为前缀。

例如，如下路由配置：

```json
{
    "paths": ["/service", "/hello/world"]
}
```

以下请求将匹配：

```
GET /service HTTP/1.1
Host: example.com


GET /service/resource?param=value HTTP/1.1
Host: example.com


GET /hello/world/resource HTTP/1.1
Host: anything.com
```

对于每一个请求，Kong检测其以路由的路径配置为前缀之一的URL path。默认情况下，Kong会在不更改URL path的情况下代理上游请求。
使用路径前缀进行代理时，首先评估最长路径(longest paths)。这允许你定力具有两个路径的两个路由：`/service`, `/service/resource`，并确保前者不会遮蔽(shadow)后者。


<br/>
<br/>


##### 在路径中使用正则

Using regexes in paths

Kong通过PCRE(Perl Compatible Regular Expression)支持路由的路径字段的正则表达式模式匹配。你可以同时将路径作为前缀和正则表达式分配给路由。

例如，考虑如下路由：

```json
{
    "paths": ["/users/\d+/profile", "/following"]
}
```

此路由匹配以下请求：

```
GET /following HTTP/1.1
Host: ...


GET /users/123/profile HTTP/1.1
Host: ...
```

<br>

- **评估顺序(Evaluation order)**

如前所述，Kong按长度评估前缀路径：首先评估最长路径。但是，Kong将根据路由的`regex_priority`属性从最高优先级到最低优先级来评估正则表达式。
考虑如下路由：

```json
[
    {
        "paths": ["/status/\d+"],
        "regex_priority": 0
    },
    {
        "paths": ["/version/\d+/status/\d+"],
        "regex_priority": 6
    },
    {
        "paths": ["/version"],
    },
    {
        "paths": ["/version/any/"],
    }
]
```

在这种情况下，Kong将按照以下顺序评估对以下定义的URL的传入请求：

```
1. /version/any/
2. /version
3. /version/\d+/status/\d+
4. /status/\d+
```

始终在正则表达式之前评估前缀路径。

向往常一样，请求仍然必须匹配路由的`hosts`和`methods`属性，并且Kong将遍历你的路由，直到找到匹配最多规则的路由。

<br>

- **捕获组(Capturing groups)**

还支持捕获组，并且将从路径中提取匹配的组并可用于插件使用。

考虑如下正则和请求路径

```
/version/(?<version>\d+)/users/(?<user>\S+)


/version/1/users/john
```

Kong会将请求路径视为匹配，如果匹配整个路由(考虑`hosts`, `methods`)，则可从`ngx.ctx`变量中的插件获取捕获组：

```
local router_matches = ngx.ctx.router_matches

-- router_matches.uri_captures is:
-- { "1", "john", version = "1", user = "john" }
```

<br>

- **转义特殊字符(Escaping special characters)**

通过Admin API配置具有正则表达式路径的路由时，请务必在必要时对你的有效负载进行URL编码。
例如，使用`curl`并使用`application/x-www-form-urlencoded`MIME类型：

```json
curl -i -X POST http://localhost:8001/routes \
    --data-urlencode 'uris[]=/status/\d+'
HTTP/1.1 201 Created
...
```

请注意，`curl`不会自动对你的有效负载进行URL编码，并注意使用`--data-urlencode`，它可以防止对`+`字符进行URL编码，并将其解释为Kong的Admin API \`\` 空间。


<br/>
<br/>


##### `strip_path`属性

可能需要指定路径前缀以匹配路由，但不将其包括在上游请求中。要这样做，请在路由配置中使用`strip_path`属性：

```
    "paths": ["/service"],
    "strip_path": true,
    "service": {
        "id": "..."
    }
}
```

启用此标志会指示Kong在匹配此路由并继续代理服务时，不应在上游请求的URL中包含URL路径的匹配部分。

<br>

```
# 例如，以下客户端对上述路由的请求
GET /service/path/to/resource HTTP/1.1
Host: ...


# 将导致Kong发送以下上游请求
GET /path/to/resource HTTP/1.1
Host: ...
```

<br>

同样，如果启用了`strip_path`的路由上定义了正则表达式，则将剥离整个请求URL匹配序列。例如：

```json
{
    "paths": ["/version/\d+/service"],
    "strip_path": true,
    "service": {
        "id": "..."
    }
}
```

以下HTTP请求与提供的正则表达式路径匹配：

```
GET /version/1/service/path/to/resource HTTP/1.1
Host: ...
```

有Kong代理到上游：

```
GET /path/to/resource HTTP/1.1
Host: ...
```


<br/>
<br/>


#### 请求方法

Request HTTP method

`methods`字段允许根据HTTP方法匹配请求。它接受多个值。其默认值为空（HTTP方法不用于路由）。

以下路由允许通过`GET`和`HEAD`进行路由：

```json
{
    "methods": ["GET", "HEAD"],
    "service": {
        "id": "..."
    }
}
```

这样的路由符合以下要求：

```
GET / HTTP/1.1
Host: ...


HEAD /resource HTTP/1.1
Host: ...
```

但它与`POST`和`DELETE`请求不匹配。在路由上配置插件时，这允许更多粒度。例如，你可以想象两个执行同一服务的路由：

- 一个具有无限制的未认证的`GET`请求
- 另一个仅允许经过身份认证和速率限制的`POST`请求



<br/>
<br/>



### 匹配优先级

Matching priorities









































