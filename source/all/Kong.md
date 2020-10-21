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
- Kong v1.2
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


路由可以基于其`hosts, paths, methods`字段定义匹配规则。要使Kong匹配到路由的传入请求，必须满足所有现有字段。但是，通过允许两个或多个路由配置包含相同值的字段，Kong允许相当大的灵活性。当发生这种情况时，Kong应用优先级规则(priority rule)。

规则是：**在评估请求时，Kong将首先尝试匹配具有最多规则(most rules)的路由**。

例如，如果两个路由配置如下：

```json
{
    "hosts": ["example.com"],
    "service": {
        "id": "..."
    }
},
{
    "hosts": ["example.com"],
    "methods": ["POST"],
    "service": {
        "id": "..."
    }
}
```

第二个路由有`hosts, methods`字段，因此它将首先由Kong评估。通过这样做，我们避免了第一个用于第二个路由的**阴影(shadow)**调用。

看看请求匹配的规则：

```
# 匹配第一个路由
GET / HTTP/1.1
Host: example.com


# 匹配第二个路由
POST / HTTP/1.1
Host: example.com
```

遵循这个逻辑，如果第三个路由配置了`hosts, methods, uris`字段，则它将首先由Kong评估。



<br/>
<br/>



### 代理行为

Proxying behavior


上面的代理规则详细地说明了Kong如何将传入请求转发到你的上游服务。下面来详细说明Kong与HTTP请求与注册路由匹配(match)的时间与请求的实际转发(forwarding)之间内部发生的情况。


<br/>


#### 负载均衡

Load balancing

Kong实现负载均衡功能，以跨上游服务实例池来分发代理请求。
详情参考后面负载均衡详情。


<br/>
<br/>


#### 插件执行

Plugins execution

Kong可通过插件(plugins)进行扩展，这些插件将将自己钩(hook)在代理请求的请求/响应的生命周期中。插件可在你的环境中执行各种操作，或在代理请求上进行转换。

可将插件配置为**全局(globally)**或特定(specific)路由和服务运行。在这两种情况下，你都必须通过Admin API创建插件配置。
一旦路由匹配，Kong将运行与这些实体中的任何一个相关联的插件。在路由上配置的插件在服务上配置的插件之前运行。

详细信息参考插件开发指南。


<br/>
<br/>


#### 超时

Proxying/upstream timeouts

一旦Kong执行了所有必要的逻辑（包括插件），它就可以将请求转发给你的上游服务。这通过Nginx的`ngx_http_proxy_module`模块完成。你可通过以下服务的属性为Kong和给定上游之间的连接配置超时：

- `upstream_connect_timeout`：定义建立与上游服务的连接的超时时间，以毫秒(ms)为单位，默认60000
- `upstream_send_timeout`：定义用于向上游服务发送请求的两个连续写入操作之间的超时时间，以毫秒(ms)为单位，默认60000
- `upstream_read_timeout`：定义用于接收来自上游服务的两个连续读取操作之间的超时时间，以毫秒(ms)为单位，默认60000

Kong将通过`HTTP/1.1`发送请求，并设置以下Header：

- `Host: <your_upstream_host>`
- `Connection: keep-alive`，允许重用上游连接
- `X-Real-IP: <remote_addr>`，其中`$remote_addr`是与`ngx_http_core_module`模块提供的名称相同的变量。请注意，`$remote_addr`可能被`ngx_http_realip_module`覆盖
- `X-Forwarded-For: <address>`，其中`<address>`是由附加到请求头的`ngx_http_realip_module`提供的`$realip_remote_addr`的内容，其名称相同
- `X-Forwarded-Proto: <protocol>`，其中`<protocol>`是客户端使用的协议。在`$realip_remote_addr`是可信地址之一的情况下，如果提供，则转发具有相同名称的请求头。否则，将使用`ngx_http_core_module`提供的`$scheme`变量的值
- `X-Forwarded-Host: <host>`，其中`<host>`是客户端发送的主机名。在`$realip_remote_addr`是可信地址之一的情况下，如果提供，则转发具有相同名称的请求头。否则，将使用`ngx_http_core_module`提供的`$host`变量的值
- `X-Forwarded-Port: <port>`，其中`<port>`是接受请求的服务器的端口。在`$realip_remote_addr`是可信地址之一的情况下，如果提供，则转发具有相同名称的请求头。否则，将使用`ngx_http_core_module`提供的`$server_port`变量的值

所有其它请求头都由Kong转发。

使用WebSocket协议时会出现一个例外。如果这样，Kong将设置以下Header以允许升级客户端和上有服务之间的协议：

- `Connection: Upgrade`
- `Upgrade: websocket`


<br/>
<br/>


#### 错误和重试

Errors and retries

每当代理期间发生错误时，Kong将使用底层的Nginx重试机制将请求传递个下一个上游。

这里有两个可配置的元素：

- 重试次数：可使用`retries`属性为每个服务配置
- 是什么构成错误：Kong使用Nginx默认值，这意味着与服务器建立连接，向其传递请求/读取响应 Header时发生错误或超时


<br/>
<br/>


#### 响应

Response

Kong接收来自上游服务的响应，并以流(stream)的方式将其发送回下游客户端。此时，Kong将执行添加到路由或服务的后续插件，这些插件在`header_filter`阶段实现一个钩子。
一旦执行了所有已注册插件的`header_filter`阶段，Kong将添加以下Header，并将完整的header集发送到客户端：

- `Via: kong/x.x.x`，其中`x.x.x`是Kong使用的版本
- `X-Kong-Proxy-Latency: <latency>`，其中`<latency>`是Kong收到客户端请求和向上游服务发送请求之间的时间(ms为单位)
- `X-Kong-Upstream-Latency: <latency>`，其中`<latency>`是Kong等待上游服务响应的第一个字节的时间(ms为单位)

将Header发送到客户端后，Kong将开始为实现`body_filter`钩子的路由或服务之星已注册的插件。由于Nginx的流媒体特性，可多次调用此钩子。



<br/>
<br/>



### 后备路由

Configuring a fallback Route


作为一个实际的用例和Kong的代理功能所提供的灵活性的栗子，让我们尝试实现一个**后备路由(fallback route)**，这样为了避免Kong响应HTTP `4O4 no route found`，我们可以捕获这样的请求并将它们代理到特殊的上游服务，或者向其应用插件。

一个后备路由的栗子：

```json
{
    "paths": ["/"],
    "service": {
        "id": "..."
    }
}
```

任何向Kong发出的HTTP请求实际上都会匹配此路由，因为所有URI都以根字符(`/`)为前缀。最长的URL路径首先由Kong评估，因此`/`路径将由Kong最后评估，并有效地提供后备路由，仅作为最后的手段。然后，你可将流量发送到特殊服务或在此路由上应用任何插件。



<br/>
<br/>



### SSL

Configuring SSL for a Route


Kong提供了一种基于每个连接动态(dynamically)提供SSL证书的方式。SSL证书由**core**直接处理，并可通过Admin API进行配置。通过TLS连接到Kong的客户端必须支持Server Name Indication(SNI)扩展才能使用此功能。

SSL证书由Kong Admin API中的两个资源处理：

- `/certificates`，存储密钥(key)和证书(certificate)
- `/snis`，将注册的证书与Server Name Indication相关联

<br>

以下是在给定路由上配置SSL证书的方法:

```sh
# 通过Admin API上传证书和密钥
# snis表单参数，直接插入SNI并将其上传的证书与其关联
curl -i -X POST http://localhost:8001/certificates \
    -F "cert=@/path/to/cert.pem" \
    -F "key=@/path/to/cert.key" \
    -F "snis=ssl-example.com,other-ssl-example.com"
HTTP/1.1 201 Created
...
```

现在必须在Kong内注册以下路由。一个简单的栗子：

```sh
curl -i -X POST http://localhost:8001/routes \
    -d 'hosts=ssl-example.com,other-ssl-example.com' \
    -d 'service.id=d54da06c-d69f-4910-8896-915c63c270cd'
HTTP/1.1 201 Created
...
```

你现在可以期望Kong通过HTTPS提供路由：

```sh
curl -i https://localhost:8443/ \
  -H "Host: ssl-example.com"
HTTP/1.1 200 OK
...
```

建立连接并协商SSL握手时，如果客户端发送`ssl-example.com`作为SNI扩展的一部分，Kong将提供先前配置的`cert.pem`证书。 


<br/>


#### 限制客户端协议

Restricting the client protocol (HTTP/HTTPS/TCP/TLS)

路由有一个`protocols`属性，以限制它们应该监听的客户端协议。此属性接受一组值:

- `http/https`
- `tcp/tls`

具有`http`和`https`的路由将接受两种协议中的流量：

```json
{
    "hosts": ["..."],
    "paths": ["..."],
    "methods": ["..."],
    "protocols": ["http", "https"],
    "service": {
        "id": "..."
    }
}
```

未指定协议的路由默认为`["http", "https"]`。

但是，仅使用`https`的路由只接受通过`https`的流量。如果以前从信任的IP发生SSL终止(termination)，它也会接受未加密的流量。当请求来自`trusted_ip`中配置的IP并且如果设置了`X-Forwarded-Proto: https` Header头时，SSL终止被认为是有效：

```json
{
    "hosts": ["..."],
    "paths": ["..."],
    "methods": ["..."],
    "protocols": ["https"],
    "service": {
        "id": "..."
    }
}
```

如果上述路由与请求匹配，但该请求是没有有效的先前SSL终止的纯文本(plain-text)，则Kong响应：

```
HTTP/1.1 426 Upgrade Required
Content-Type: application/json; charset=utf-8
Transfer-Encoding: chunked
Connection: Upgrade
Upgrade: TLS/1.2, HTTP/1.1
Server: kong/x.y.z

{"message":"Please use HTTPS protocol"}
```

<br>

从Kong v1.0开始，可以使用`protocols`属性中的`tcp`为原始TCP(不一定是HTTP)连接创建路由：

```json
{
    "hosts": ["..."],
    "paths": ["..."],
    "methods": ["..."],
    "protocols": ["tcp"],
    "service": {
        "id": "..."
    }
}
```

同样，可使用`tls`值创建接受原始TLS流量(不一定是HTTPS)的路由：

```
{
    "hosts": ["..."],
    "paths": ["..."],
    "methods": ["..."],
    "protocols": ["tls"],
    "service": {
        "id": "..."
    }
}
```

仅具有TLS的路由仅接受通过TLS的流量。

也可同时接受`tcp`和`tls`：

```json
{
    "hosts": ["..."],
    "paths": ["..."],
    "methods": ["..."],
    "protocols": ["tcp", "tls"],
    "service": {
        "id": "..."
    }
}
```



<br/>
<br/>



### WebSocket

Proxy WebSocket traffic


由于底层的Nginx实现，Kong支持WebSocket流量。如果希望通过Kong在客户端和上游服务之间建立WebSocket连接，则必须建立WebSocket握手。这是通过HTTP Upgrade mechanism 完成的。

```
# 栗子，客户端对Kong的请求
GET / HTTP/1.1
Connection: Upgrade
Host: my-websocket-api.com
Upgrade: WebSocket
```

这使Kong将`Connection`和`Upgrade` header转发到上游服务，而不是由于标准HTTP proxy的逐跳(hop-by-hop)特性而将其解除。


<br/>


#### WebSocket和TLS

Kong将在其各自的`http`和`https`端口接受`ws`和`wss`连接。要从客户端强制执行TLS联结，请见路由的`protocols`属性设置为仅`https`。

将服务设置为指向上游WebSocket服务时，应仔细选择要在Kong和上游之间使用的协议。如果要使用TLS（`wss`），则必须使用上游WebSocket服务定义的服务`protocol`属性中的`https`协议和正确的端口(`443`)。要在没有TLS(`ws`)的情况下进行连接，则应在`protocol`中使用`http`协议和端口(`80`)。




<br/>
<br/>
<br/>



## 认证

[Authentication Reference](https://docs.konghq.com/1.1.x/auth/)


到上游服务(API/Microservice)的流量通常是由各种**Kong 认证插件(authentication plugins)**的应用程序和配置来控制。


<br/>
<br/>


### 通用认证

Generic authentication


最常见的情况是要求认证并且不允许任何未经认证的请求的访问。为此，可以使用任何认证插件。这些插件的通用流程如下：

1. 将认证插件应用于服务(service)或全局(global)
2. 创建一个**消费者(consumer)**实体
3. 为消费者提供特定认证方法(authentication method)的认证凭据(authentication credential)
4. 现在每当有请求(request)进入时，Kong都会检查凭据，如果请求无法验证，它将阻止请求，或在Header中添加消费者和凭据的详细信息并转发请求

上面的通用流程并不总是适用，例如在使用LDAP等外部认证时，则不会添加消费者，并且只会在转发的Header中添加凭据。



<br/>
<br/>



### 消费者

Consumers


理解消费者最简单的方法是将它们一对一地映射到用户(one-on-one to user)。然而，对Kong来说这并不重要。消费者的核心原则是你可以将插件附加到它们，从而定制请求行为。（例如，为每个移动应用的版本定义一个消费者，Android消费者、IOS消费者...）

这对Kong来说是一个不透明的概念，因此它们被称为**消费者(consumer)**而不是**用户(user)**。



<br/>
<br/>



### 匿名访问

Anonymous Access


Kong能够配置给定服务以允许**身份认证(Authentication)**和**匿名访问(Anonymous Access)**。你可以使用此配置向具有低速率限制(rate-limit)的匿名用户授予访问权限，并授予对具有更高速率限制的认证的用户的访问权限。

要配置这样的服务，首先应用你选择的身份认证插件，然后创建一个新的消费者来表示匿名用户，然后配置身份认证插件以允许匿名访问。

栗子:

```sh
# 1. Create an example Service and a Route
curl -i -X POST \
   --url http://localhost:8001/services/ \
   --data 'name=example-service' \
   --data 'url=http://mockbin.org/request'

# Add a route to the Service:
curl -i -X POST \
   --url http://localhost:8001/services/example-service/routes \
   --data 'paths[]=/auth-sample'

# http://localhost:8000/auth-sample



# 2. Configure the key-auth Plugin for your Service
curl -i -X POST \
   --url http://localhost:8001/services/example-service/plugins/ \
   --data 'name=key-auth'

# 注意插件id，后面会用到



# 3. Verify that the key-auth plugin is properly configured
curl -i -X GET \
   --url http://localhost:8000/auth-sample

# May be 403 Forbidden
HTTP/1.1 403 Forbidden
...

{
  "message": "No API key found in headers or querystring"
}



# 4. Create an anonymous Consumer
# Kong代理的每个请求都必须与消费者关联
curl -i -X POST \
   --url http://localhost:8001/consumers/ \
   --data "username=anonymous_users"

# 创建响应
HTTP/1.1 201 Created
Content-Type: application/json
Connection: keep-alive

{
  "username": "anonymous_users",
  "created_at": 1428555626000,
  "id": "bbdf1c48-19dc-4ab7-cae0-ff4f59d87dc9"
}

# 注意消费者id，后面需要



# 5. Enable anonymous access
curl -i -X PATCH \
   --url http://localhost:8001/plugins/<your-plugin-id> \
   --data "config.anonymous=<your-consumer-id>"



# 6. Check anonymous access
curl -i -X GET \
   --url http://localhost:8000/auth-sample

# 响应（Mockbin）
{
  ...
  "headers": {
    ...
    "x-consumer-id": "713c592c-38b8-4f5b-976f-1bd2b8069494",
    "x-consumer-username": "anonymous_users",
    "x-anonymous-consumer": "true",
    ...
  },
  ...
}
```



<br/>
<br/>



### 多个认证

Multiple Authentication


Kong支持给定服务的多个身份认证插件，允许不同客户端使用不同的身份认证方法来访问给定的服务或路由。

在评估多个身份认证凭据时，可将认证插件的行为设置为逻辑`AND`或逻辑`OR`。关键行为是`config.anonymous`属性：

- `config.anonymous`未设置
如果未设置此属性，则认证插件将始终执行身份认证。如果未经过认证，则返回`40x`响应。当调用多个认证插件时，这会产生逻辑`AND`。

- `config.anonymous`设置为有效的消费者ID
在这种情况下，认证插件只会在尚未经过身份认证的情况下执行认证。认证失败时，让不会返回`40x`响应，而是将匿名消费者设置为消费者。当调用多个认证插件时，这会导致逻辑`OR`加上`anonymous access`。

<br>

注意:

- 必须为匿名访问配置所有认证插件，或不配置任何认证插件。如果它们是混合的，则并未是不确定的；
- 使用`AND`方法时，最后执行的插件将是传递给上游服务的设置凭据。使用`OR`方法时，它将成为第一个成功验证消费者的插件，或者是将设置其配置的匿名消费者的最后一个插件；
- 以`AND`方式使用OAuth2插件时，用于请求令牌(token)等的OAuth端点也需要其它已配置的认证插件进行身份认证。




<br/>
<br/>
<br/>




## 负载均衡

[Loadbalancing reference](https://docs.konghq.com/1.1.x/loadbalancing/)

Kong提供了多种方式将请求负载均衡到多个后端服务：

- **基于DNS**的方法
- 动态的**环均衡器**(ring-balancer)，它还允许服务注册，而无需DNS服务器


<br/>


### 基于DNS

DNS-based loadbalancing


使用基于DNS的负载均衡时，后端服务的注册是在Kong之外完成的，而Kong只接收来自DNS服务器的更新。

如果名称解析为多个IP地址，则使用包含主机名（而不是IP地址）的`host`定义的每个服务将自动使用基于DNS的负载均衡，前提是主机名未解析为`upstream`名称或你的DNS hosts。

DNS记录`ttl`确定刷新信息的频率。当`ttl`为0时，将使用自己的DNS查询解析每个请求。虽然这会有性能损失，但更新/更改的延迟将非常低。


<br/>


#### A记录

A记录包含一个或多个IP地址。因此，当主机名解析为A记录时，每个后端服务必须具有自己的IP地址。

由于没有`weight`(权重)信息，所有条目在负载均衡器中被视为相等的权重，并且均衡器将直接转发轮询(straight forward round-robin)。


<br/>
<br/>


#### SRV记录

SRV记录包含其所有IP地址的权重和端口信息。后端服务可通过IP地址和端口号的唯一组合来识别。因此，单个IP地址可以在不同的端口上托管同一个服务的多个实例。

由于`weight`(权重)信息可用，每个条目将在负载均衡器中获取自己的权重，并执行加权轮询(weighted round-robin)。

同样，任何给定的端口信息都将被来自DNS Server的端口信息覆盖。如果服务具有`host=myhost.com, port=123`，并且`myhost.com`被解析为`127.0.0.1:456`的SRV记录，则该请求将代理到`http://127.0.0.1:456/somepath`，因为端口`123`被4`56`覆盖了。


<br/>
<br/>


#### DNS优先级

DNS解析器将按顺序开始解析以下记录类型：

1. The last successful type previously resolved
2. SRV record
3. A record
4. CNAME record

此顺序可通过`dns_order`配置属性进行配置。


<br/>
<br/>


#### DNS注意事项

- 每当刷新DNS记录时，都会生成一个列表以正确处理权重。
- 某些nameserver不返回所有条目(由于UDP包大小)，给定的Kong节点将仅使用nameserver提供的少数上游服务实例。在这种情况下，上游实例池可能会不一致地加载，因为由于nameserver提供的信息有限，Kong节点实际上没有意识到某些实例。要减轻这种使用，请使用其它nameserver，使用IP地址而不是名称，或确保使用足够的Kong节点仍然使用上游服务。
- 当nameserver返回`3 name error`错误时，那是对Kong的有效响应。如果这是意外，请首先验证正在查询的正确名称，然后在检查你的nameserver的配置。
- 从DNS记录（A/SRV）初始选择IP地址不是随机的。因此，当使用`ttl`为0的记录时，nameserver应该随机化记录条目。



<br/>
<br/>



### Ring-balancer


当使用**环均衡器**时，Kong将处理后端服务的添加(add)和移除(remove)，并且不需要DNS更新。Kong将担任服务注册。可以使用单个HTTP请求添加/删除节点，并立即开始/停止接收流量

配置环均衡器是通过`upstream`和`target`目标实例完成的：

- `target`：带有后端服务所在端口的IP地址或主机名。每个目标都有一个附加的`weight`以指示它获得的相对负载。IP地址可以是IPv4或IPv6格式。
- `upstream`：可在路由`host`字段中使用的虚拟主机名。例如名为`weather.v2.service`的上游，将获得来自具有`host=weather.v2.service`的服务的所有请求。


<br/>


#### Upstream

每个上游都有自己的环均衡器。每个`upstream`都可以附加许多`target`条目，代理到虚拟主机名的请求将在目标上进行负载均衡。换均衡器具有预定数量的插槽(slot)，并且基于目标权重，插槽被分配给上游的目标。

可使用Admin API上的简单HTTP请求来添加/移除目标。这个操作相对便宜，更改上游本身更加昂贵，因为当插槽数量发生变化时需要重建均衡器。

自动重建均衡器的唯一情况是清理目标历史记录时，除此之外，它只会在变化时重建。

在均衡器上有位置（from 1 to `slots`），它们随机分布(randomly distributed)在环上。随机性是在运行时调用环均衡器所必需的。轮子上的简单轮询将在目标上提供分布均匀的加权轮询，同时在 插入/删除 目标时也具有廉价操作。

每个目标使用的插槽数量应该(至少)为100，以确保插槽的正确分布。例如，对于预期最多8个目标，上游应定义至少`slots=800`，即使初识设置仅具有两个目标。

这里的权衡是，插槽数越多，随机分布越好，但更改成本越高(添加/删除目标)。


<br/>
<br/>


#### Target

由于上游(upstream)维护更改的历史记录，因此只能添加目标，不能修改或删除目标。要更改目标，只需为目标添加新条目，然后更改权重(weight)值。最后一个条目是将要使用的条目，因此，设置`weight=0`将禁用目标，从而有效地将其从均衡器中删除。

当非活动条目(inactive entries)比活动条目(active)多10倍时，将自动清理目标(target)。清理将涉及重建均衡器，因此仅比添加目标条目更昂贵。

目标也可以具有主机名（而不是IP地址）。在这种情况下，名称将被解析，所有找到的条目将单独添加到环形均衡器。如，添加`api.host.com:123, weight=100`。注意，权重用于单个条目，而不是整个。

如果它解析为SRV记录，那么DNS中的端口和权重字段也将被拾取，并且将覆盖给定的端口和权重。

均衡器将遵守DNS记录的`ttl`设定并重新查询(requery)和更新到期的均衡器。


<br/>
<br/>


#### 均衡算法

Balancing algorithms

默认情况下，环均衡器(ringn-balancer)将使用加权轮询方案(weighted-round-robin scheme)。替代方案是使用基于哈希(hash)的算法。哈希的输入可以是`none`, `consumer`, `ip`, `header`, `cookie`。当设置为`none`时，将使用加权轮询方案，并禁用哈希。
有两个选项，primary和fallback，以防主要失败。

不同的哈希选项：

- `none`，不使用哈希，而是使用加权轮询（default）。
- `consumer`，使用消费者ID作为哈希输入。如果没有可用的消费者ID，则将回退到凭据ID。
- `ip`，远程IP地址用作输入。使用此选项时，请查看用于确定实际IP的配置设置。
- `header`，使用指定的header作为哈希的输入。
- `cookie`，使用指定路径的指定cookie作为哈希的输入。如果请求中不存在cookie，则响应将设置该cookie。

哈希算法基于**一致性哈希**(consistent-hashing)，确保通过更改目标(add, remove, fail, weight)修改均衡器时，只有最小数量的哈希损失发生。这将最大化上游缓存命中(cache hits)。


<br/>
<br/>


#### 均衡警告

Balancing caveats

环均衡器设计用于单个节点以及集群。对于加权轮询算法没有太大的区别，但当使用基于哈希的算法时，重要的是所有节点构建完全相同的环均衡器以确保它们全部相同。为此，必须以确定的方式构建均衡器。

- 不要再均衡器中使用主机名，因为均衡器 可能/将 慢慢发散，DNS ttl仅具有第二精度，并且更新由实际请求名称确定。最重要的一些NameServer没有返回所有条目的为题，这加剧了这个问题。因此，在Kong集群中使用哈希方法时，仅通过其IP地址添加目标(target)实体，而不是通过名称添加目标实体。
- 在选择哈希输入时，请确保输入具有足够的方差(variance)以获得分布均匀的哈希。哈希值将使用CRC-32摘要计算。



<br/>
<br/>



### 蓝绿部署

Blue-Green Deployments


使用环均衡器可以轻松地为服务编排**蓝绿部署**(blue-green deployment)。切换目标基础结构仅需要服务上的`PATCH`请求，以更改其`host`值。

<br>

设置`Blue`环境，运行 v1 的服务地址：

```sh
# create an upstream
curl -X POST http://kong:8001/upstreams \
    --data "name=address.v1.service"

# add two targets to the upstream
curl -X POST http://kong:8001/upstreams/address.v1.service/targets \
    --data "target=192.168.34.15:80"
    --data "weight=100"
curl -X POST http://kong:8001/upstreams/address.v1.service/targets \
    --data "target=192.168.34.16:80"
    --data "weight=50"

# create a Service targeting the Blue upstream
curl -X POST http://kong:8001/services/ \
    --data "name=address-service" \
    --data "host=address.v1.service" \
    --data "path=/address"

# finally, add a Route as an entry-point into the Service
curl -X POST http://kong:8001/services/address-service/routes/ \
    --data "hosts[]=address.mydomain.com"
```

主机头设置为`address.mydomain.com`的请求由Kong代理到两个定义的目标： 2/3的请求将转到`http://192.168.34.15:80/address`(weight=100)，1/3的请求将转到`http://192.168.34.16:80/address`(weight=50)。

<br>

在部署服务的v2之前，设置`Green`环境：

```sh
# create a new Green upstream for address service v2
curl -X POST http://kong:8001/upstreams \
    --data "name=address.v2.service"

# add targets to the upstream
curl -X POST http://kong:8001/upstreams/address.v2.service/targets \
    --data "target=192.168.34.17:80"
    --data "weight=100"
curl -X POST http://kong:8001/upstreams/address.v2.service/targets \
    --data "target=192.168.34.18:80"
    --data "weight=100"
```

<br>

要激活蓝/绿开关，只需要更新服务：

```sh
# Switch the Service from Blue to Green upstream, v1 -> v2
curl -X PATCH http://kong:8001/services/address-service \
    --data "host=address.v2.service"


# 主机头为address.mydomain.com的传入请求现在由Kong代理到新目标
# 1/2的请求将转至http://192.168.34.17:80/address(weight=100)
# 另外1/2将转至`http://192.168.34.18:80/address`(weight=100)
```

与往常一样，通过Kong Admin API进行的更改是动态的，并将立即生效。不需要重载或重启，也不会丢弃正在进行的请求。



<br/>
<br/>



### 金丝雀发布

Canary Releases


使用环均衡器，可以精确调整目标权重，从而实现平稳、受控的金丝雀发布(canary release)。

<br>

两个简单栗子：

```sh
# first target at 1000
curl -X POST http://kong:8001/upstreams/address.v2.service/targets \
    --data "target=192.168.34.17:80"
    --data "weight=1000"

# second target at 0
curl -X POST http://kong:8001/upstreams/address.v2.service/targets \
    --data "target=192.168.34.18:80"
    --data "weight=0"
```

<br>

通过重复请求，但每次都改变权重，流量将慢慢地路由到另一个目标。例如，将其设置为`10%`：

```sh
# first target at 900
curl -X POST http://kong:8001/upstreams/address.v2.service/targets \
    --data "target=192.168.34.17:80"
    --data "weight=900"

# second target at 100
curl -X POST http://kong:8001/upstreams/address.v2.service/targets \
    --data "target=192.168.34.18:80"
    --data "weight=100"
```

通过Kong Admin API进行的更改是动态的，并将立即生效。不需要重新加载或重新启动，也不会丢弃正在进行的请求。




<br/>
<br/>
<br/>




## 蓝绿部署/AB测试/金丝雀发布

[Blue-green Deployments, A/B Testing, and Canary Releases](https://blog.christianposta.com/deploy/blue-green-deployments-a-b-testing-and-canary-releases/)


许多团队对**DevOps**越来越感兴趣，这里面就涉及到了热部署(hot deployments)、回滚(rollbacks)...

亚马逊等已经实施了十年以上的蓝绿部署，它是一种安全、经过验证的方法。但A/B测试呢？金丝雀测试呢？对于MicroService、DevOps和cloud-Native，这里阐述一下它们的不同之处。


<br/>


### 蓝绿部署

Blue Green Deployments

它基本上是一种以可预测的方式(predictable manner)发布应用程序的技术，目标是减少与发布相关的任何停机时间。这是在发布之前准备应用程序的快速方法，如果你发现问题，也可以快速回滚。

简单地说，你有两个相同的环境。

绿环境：

![](/images/Kong/greendeployment.png)

现在，你准备对应用程序进行更改并将其升级为v2，你将在蓝环境中执行此操作。

![](/images/Kong/bluedeployment.png)

你可以监控因发布而导致的任何异常。如果一切都好，你最终可以关闭绿环境并使用它来发布任何新版本。如果异常，亦可以通过负载均衡器切换回来以快速回滚的绿环境。



<br/>
<br/>



### A/B测试

A/B Testing

A/B测试不是蓝绿部署。它是一种在应用程序中测试功能的方法，出于各种原因(如可用性、流行性、吸引性...)，以及这些因素如何影响底线。它通常与应用程序的UI部分相关联，但当然后端服务需要可用于执行此操作。你可以使用应用程序级别的开关、静态开关以及金丝雀发布来实现此功能。

![](/images/Kong/abtesting.png)

蓝绿部署和A/B测试之间的区别在于A/B测试用于测量应用程序中的功能。



<br/>
<br/>



### 金丝雀发布

Canary releases


金丝雀发布是一种将新版本的应用程序发送到生产环境中的方式。它扮演金丝雀的角色，以了解它将如何执行。这是另一种发布策略，它可以环境这样一个事实，即无论你在较低环境中进行的测试水平如何，仍会在生产中遇到一些错误。金丝雀发布允许你在完全释放扳机之前测试水域。

![](/images/Kong/canarydeployment.png)

你获得的反馈速度越快，部署失败的速度就越快，或谨慎行事。




<br/>
<br/>
<br/>




## 健康检查和熔断器

[Health Checks and Circuit Breakers Reference](https://docs.konghq.com/1.1.x/health-checks-circuit-breakers/)


你可以使用Kong 环均衡器代理API，通过添加包含一个或多个目标实体的上游实体进行配置，每个目标指向不同的IP和端口。环均衡器将平衡各种目标之间的负载，并且基于上游配置，将对目标执行健康检查，将判断它们是否健康，无论它们是否响应。然后，环均衡器仅将流量路由到健康目标。

Kong支持两种健康检查，可单独使用或结合使用：

- **主动检测(active checks)**，定期请求目标中的特定HTTP/HTTPS端点，并根据其响应确定目标的运行状况；
- **被动检测(passive checks)（也称为熔断器）**，Kong分析正在进行的代理流量，并根据其行为响应请求确定目标的运行状况。


<br/>
<br/>


### 健康与不健康

Healthy and unhealthy targets


健康检查功能的目的是为给定的Kong节点动态地将目标标记为健康或不健康。没有集群范围内的健康信息同步：每个Kong节点分别确定其目标的健康状况。这是可取的，因为在给定端点，一个Kong节点可能能够成功连接到目标而另一个Kong节点不能到达它：第一个节点将认为它是健康的，而另一个节点将其标记为不健康并开始将流量路由到上游的其它目标。

活动探测（主动健康检查）或代理请求（被动健康检查）会生成用于确定目标是健康还是不健康的数据。请求可能会产生`TCP error、timeout`, `HTTP status code`。基于此信息，健康检查器更新一些列内部计数器：

- 如果返回的状态码是一个配置为**健康(healthy)**的状态码，它将递增目标的**成功(Successes)**计数器并清除其它计数器；
- 如果连接失败，它将递增目标的**TCP failures**计数器并清除**Successes**计数器；
- 如果超时，它将递增目标的**超时(timeouts)**计数器并清除**Successes**计数器；
- 如果返回的HTTP状态码是配置为**不健康(unhealthy)**的状态码，它将递增`HTTP failures`计数器并清除**Successes**计数器。

如果任何`TCP failures`, `HTTP failures`, `timeouts`计数器达到其配置的阈值，则目标将被标记为不健康。
如果`Successes`计数器达到其配置的阈值，则目标将标记为健康。

HTTP状态码的列表是健康或不健康，并且每个计数器的各个阈值可以基于每个上游进行配置。下面的栗子中，我们有一个上游实体的配置示例。

```json
{
    "name": "service.v1.xyz",
    "healthchecks": {
        "active": {
            "concurrency": 10,
            "healthy": {
                "http_statuses": [ 200, 302 ],
                "interval": 0,
                "successes": 0
            },
            "http_path": "/",
            "timeout": 1,
            "unhealthy": {
                "http_failures": 0,
                "http_statuses": [ 429, 404, 500, 501,
                                   502, 503, 504, 505 ],
                "interval": 0,
                "tcp_failures": 0,
                "timeouts": 0
            }
        },
        "passive": {
            "healthy": {
                "http_statuses": [ 200, 201, 202, 203,
                                   204, 205, 206, 207,
                                   208, 226, 300, 301,
                                   302, 303, 304, 305,
                                   306, 307, 308 ],
                "successes": 0
            },
            "unhealthy": {
                "http_failures": 0,
                "http_statuses": [ 429, 500, 503 ],
                "tcp_failures": 0,
                "timeouts": 0
            }
        }
    },
    "slots": 10
}
```

如果上游的所有目标都不健康，Kong将以`503 Service Unavailable`响应上游请求。

注意：

- 健康检查操作仅在活跃目标(acti)上运行，不会修改Kong数据库中目标的活动状态；
- 不健康的目标不会从负载均衡器中移除，因此在使用HASH算法时不会对均衡器布局产生任何影响；
- `DNS caveats`和`balancer caveats`也适用于健康检查。如果为目标使用主机名，请确保DNS服务器始终返回名称的完整IP地址集，并且不限制响应。如果不这样做可能导致健康检查无法执行。



<br/>
<br/>



### 健康检查类型


<br/>


#### 主动检查

Active health checks

顾名思义，主动检查会主动探索目标健康。当在上游实体中启用活动运行状况检查时，Kong将定期向上游的每个目标配置得路径发送HTTP/HTTPS请求。这允许Kong根据结果自动启用和禁用均衡器中的目标。

当目标健康或不健康时，可以单独配置主动健康检查的周期性。如果其中的一个`interval`值设置为0，则在相应的方案中禁用检查。当两者都为0时，将完全禁用主动健康检查。

> **注意：**
> 主动健康检查目前仅支持HTTP/HTTPS目标。它们不适用于分配给上游协议属性为`TCP`或`TLS`。


<br/>
<br/>


#### 被动检查(熔断器)

Passive health checks (circuit breakers)

被动健康检查(熔断器)，是根据Kong（HTTP/HTTPS/TCP）代理的请求执行的检查，不会产生额外的流量。当目标变得无响应时，被动检查程序将检测到该目标，并将目标标记为不健康。环均衡器将开始条多此目标，因此不再将流量路由到该目标。

一旦目标的问题解决了，并且它已准备好再次接收流量，Kong管理员可以通过Admin API端点手动通知健康检查器应该再次启用目标：

```sh
curl -i -X POST http://localhost:8001/upstreams/my_upstream/targets/10.1.2.3:1234/healthy
HTTP/1.1 204 No Content
```

此命令将在集群内广播消息，以便将`healthy`状态传播到整个Kong集群。这将导致Kong节点重置在Kong节点的所有工作程序中运行的健康检查程序的运行状况计数器，从而允许环均衡器再次将流量路由到目标。
被动检查的优点是不是产生额外的流量，但是它们无法再自动将目标标记为健康：电路以损坏，并且管理员需要再次重新启用目标。



<br/>
<br/>



### 利弊

Summary of pros and cons


- 主动健康检查可在环均衡器再次恢复健康时自动重启环均衡器中的目标。被动健康检查则不能。
- 被动健康检查不会为目标产生额外的流量。主动健康检查会。
- 主动健康检查器要求在目标中具有可靠状态响应的已知的URL，以将其配置探测端点(probe endpoint)。被动模式不要求这样配置。
- 通过为主动健康检查程序提供自定义的探测端点，应用程序可确定自己的运行状态指标并生成由Kong使用的状态码。即使目标继续提供对被动健康检查其看起来健康的流量，它也能够响应具有故障状态的主动探测，基本上要求放心接收新的流量。

可以组合这两种模式。例如，启用被动检查根据其流量监控目标的健康状态，并仅在目标运行状态不佳时使用主动检查，以便自动重新启用它。



<br/>
<br/>



### 启用和禁用健康检查


<br/>


#### 启用主动检查

Enabling active health checks

要启用主动健康检查，你需要在Upstream对象配置中指定`healthchecks.active`下的配置项。你需要指定必要的信息，以便Kong可以对目标执行定期探测，以及如何解释结果信息。

你可使用`healthchecks.active.type`字段指定执行哪种探测（HTTP/HTTPS），或者只测试与给定主机和端口的连接是否成功（TCP）。

要配置探针，你需要指定：

- `healthchecks.active.http_path`： 默认为根目录(`/`)
- `healthchecks.active.timeout`： 默认为1s
- `healthchecks.active.concurrency`： 同时检查的目标数

对于运行的探针，你还需要指定间隔值：

- `healthchecks.active.healthy.interval`： 值为0，表示不执行健康目标的主动探测
- `healthchecks.active.unhealthy.interval`： 值为0，表示不执行不健康目标的主动探测

如果使用HTTPS健康检查，则还可以指定一下字段：

- `healthchecks.active.https_verify_certificate`： 是否检查远程主机的SSL证书的有效性
- `healthchecks.active.https_sni`： 使用SNI的主机名验证

最后，你需要通过设置健康计数器上的各种阈值来配置Kong应该如何解释探测器，一旦达到该阈值将触发状态改变。计数器阈值字段：

- `healthchecks.active.healthy.successes`
- `healthchecks.active.healthy.http_statuses`
- `healthchecks.active.unhealthy.tcp_failures`
- `healthchecks.active.unhealthy.timeouts`
- `healthchecks.active.unhealthy.http_failures`


<br/>
<br/>


#### 启用被动检查

Enabling passive health checks

被动健康检查不具有探测功能，因为它们通过解释从目标流出的持续流量来工作。这意味着要启用被动检查，你只需要配置其计数器阈值：

- `healthchecks.passive.healthy.successes`
- `healthchecks.passive.healthy.http_statuses`
- `healthchecks.passive.unhealthy.tcp_failures`
- `healthchecks.passive.unhealthy.timeouts`
- `healthchecks.passive.unhealthy.http_failures`


<br/>
<br/>


#### 禁用健康检查

Disabling health checks

在`healthchecks`配置中指定的所有计数器阈值和间隔中，将值设置为0意味着禁用该字段代表的功能。将探测间隔设置为0会禁用探针。同样，你可通过将其计数器阈值设置为0来禁用某些类型的检查。




<br/>
<br/>
<br/>




## 集群

[Clustering Reference](https://docs.konghq.com/1.2.x/clustering/)


Kong集群允许你通过添加更过计算机来处理更多的传入情趣，从而水平扩展系统。它们将共享相同的配置，因为它们指向同一个数据库。指向同一数据存储区(same datastore)的Kong Node将成为同一Kong集群的一部分。

你需要在Kong集群前安装负载均衡器，以便在可用节点之间分配流量。


<br/>
<br/>


### 集群能做些什么

What a Kong cluster does and doesn’t do

拥有Kong集群并不意味着你的客户流量将在Kong节点之间进行负载均衡。你仍然需要在Kong节点前安装负载均衡器来分配流量。相反，Kong集群意味着这些节点将共享相同的配置。

出于性能原因，Kong在代理请求时避免数据库连接，并将数据库的内容缓存在内存中。缓存的实体包括`Services, Routes, Consumers, Plugins, Credentials...`，由于这些值在内存中，因此通过其中一个节点的Admin API进行的任何更改都需要传播到其它节点。



<br/>
<br/>



### 单节点集群

Single node Kong clusters

连接到数据库(PgSQL/Cassandra)的单个Kong节点创建一个节点的Kong集群。通过此节点的Admin API应用的任何更改都将立即生效。



<br/>
<br/>



### 多节点集群

Multiple nodes Kong clusters

在多节点的Kong集群中，连接到同一个数据库的其它节点不会立即被通知（如节点A删除了某个服务）。虽然此服务不再存在于数据库中（它被节点A删除），但它仍然在节点B的内存中。

所有节点执行定期后台作业以与可能由其它节点触发的更改进行同步。通过以下方式配置此作业的频率： `db_update_frequency`（(default: 5s）。每个频率时间，所有正在运行的Kong节点将轮询数据库以进行任何更新，并在必要时从其缓存中清除(purge)相关实体。

如果节点A删除了某个服务，则此更改在节点B中将无效，直到节点B的下一次数据库轮询。
这使得Kong集群最终保持一致(eventually consistent)。



<br/>
<br/>



### 关于缓存

What is being cached?

所有核心实体（如Services, Routes, Plugins, Consumers, Credentials...)都由Kong缓存在内存中，并依赖于通过轮询机制进行的无效更新。
除此之外，Kong还缓存了`database misses`。这意味着如果你配置没有插件的服务，Kong将缓存此信息。



<br/>
<br/>



### 配置数据库缓存

How to configure database caching?

你可以在Kong配置文件中配置3个属性，最重要的一个是`db_update_frequency`，它确定Kong节点在性能与一致性权衡方面的位置。
Kong提供了一致性的默认值，以便让你在避免意外的同事试验其聚类功能。在准备生产环境设置时，应考虑调整这些值以确保遵守性能约束。

- `db_update_frequency`，(default: 5s)
- `db_update_propagation`，(default: 0s)
- `db_cache_ttl`，(default: 0s)



<br/>
<br/>



### 通过Admin API与Cache进行交互

Interacting with the cache via the Admin API

出于某些原因，你希望调查缓存的值，或手动使缓存的值无效（hit/miss），则可通过Admin API `/cache`端点执行此操作。

```sh
# Inspect a cached value
GET /cache/{cache_key}


# Purge a cached value
DELETE /cache/{cache_key}


# Purge a node’s cache
DELETE /cache
```




<br/>
<br/>
<br/>




## 日志

[Logging Reference](https://docs.konghq.com/1.2.x/logging/)


日志级别在Kong的配置文件中配置。有以下几种日志级别：

- `debug`
- `info`
- `notice`，默认和建议使用的日志级别。
- `warn`
- `error`
- `crit`




<br/>
<br/>
<br/>




## 网络和防火墙

[Network and Firewall](https://docs.konghq.com/1.2.x/network/)


<br/>
<br/>


### Ports

Kong为不同目的使用多个连接：

- Proxy
  - `8000` for proxying HTTP traffic
  - `8443` for proxying HTTPS traffic
- Admin API
  - `8001` Admin API with HTTP
  - `8444` Admin API with HTTPS



<br/>
<br/>



### Firewall

以下是推荐的防火墙设置：

- Kong后面的上游服务将通过`proxy_listen`接口/端口值提供。根据你要授予上游服务的访问级别配置这些值；
- 如果要将Admin API绑定到公众接口（通过`admin_listen`），请将其保护为仅允许受信任的客户端访问；
- 你的代理需要为你配置的任何HTTP/HTTPS和TCP/TLS流监听器添加规则。

<br>

**透明代理(Transparent Proxying)**

`transparent` 监听选项可以应用于`proxy_listen`和`stream_listen`配置。




<br/>
<br/>
<br/>




## 保护Admin API

[Securing the Admin API](https://docs.konghq.com/1.2.x/secure-admin-api/)


Kong Admin API提供了RESTful接口来管理和配置Services, Routes, Plugins, Consumers, Credentials...由于此API允许完全控制Kong，因此保护它的安全非常重要。


<br/>
<br/>


### 网络层访问限制

Network Layer Access Restrictions


<br/>


#### 最小化监听

Minimal Listening Footprint

默认情况下，Kong Admin将仅接受来自本地接口的请求。

```yaml
admin_listen = 127.0.0.1:8001, 127.0.0.1:8444 ssl

# 千万别使用0.0.0.0:8001
```


<br/>
<br/>


#### 三层四层网络控制

Layer 3/4 Network Controls

如果Admin API必须在localhost之外公开，则网络安全最佳实践要求尽可能限制网络层访问。基于主机的防火墙很有用，或云厂商的防火墙。

```sh
# assume that Kong is listening on the address defined below, as defined as a
# /24 CIDR block, and only a select few hosts in this range should have access

$ grep admin_listen /etc/kong/kong.conf
admin_listen 10.10.10.3:8001

# explicitly allow TCP packets on port 8001 from the Kong node itself
# this is not necessary if Admin API requests are not sent from the node
$ iptables -A INPUT -s 10.10.10.3 -m tcp -p tcp --dport 8001 -j ACCEPT

# explicitly allow TCP packets on port 8001 from the following addresses
$ iptables -A INPUT -s 10.10.10.4 -m tcp -p tcp --dport 8001 -j ACCEPT
$ iptables -A INPUT -s 10.10.10.5 -m tcp -p tcp --dport 8001 -j ACCEPT

# drop all TCP packets on port 8001 not in the above IP list
$ iptables -A INPUT -m tcp -p tcp --dport 8001 -j DROP
```



<br/>
<br/>



### API Loopback

Kong的路由设计允许它作为Admin API自身的代理。通过这种方式，Kong本身可用于为Admin API提供细粒度的访问控制。（如需要为其定义服务的URL）

```sh
# assume that Kong has defined admin_listen as 127.0.0.1:8001, and we want to
# reach the Admin API via the url `/admin-api`

$ curl -X POST http://localhost:8001/services \
  --data name=admin-api \
  --data host=localhost \
  --data port=8001

$ curl -X POST http://localhost:8001/services/admin-api/routes \
  --data paths[]=/admin-api

# we can now transparently reach the Admin API through the proxy server
$ curl localhost:8000/admin-api/apis
{
   "data":[
      {
         "uris":[
            "\/admin-api"
         ],
         "id":"653b21bd-4d81-4573-ba00-177cc0108dec",
         "upstream_read_timeout":60000,
         "preserve_host":false,
         "created_at":1496351805000,
         "upstream_connect_timeout":60000,
         "upstream_url":"http:\/\/localhost:8001",
         "strip_uri":true,
         "https_only":false,
         "name":"admin-api",
         "http_if_terminated":true,
         "upstream_send_timeout":60000,
         "retries":5
      }
   ],
   "total":1
}
```

从这里，只需像往常一样对任何其它Kong API应用所需的特定于Kong的安全控件。



<br/>
<br/>



### 自定Nginx配置

Kong于Nginx紧密结合，作为HTTP daemon，因此可以集成到具有自定义Nginx配置的环境中。通过这种方式，具有复杂`security/access control`要求的用例可以使用Nginx/OpenResty的全部功能来构建`server/location`块以根据需要容纳Admin API。除了提供可以构建自定义、复杂安全性控件的OpenResty环境之外，这允许此类环境利用本机Nginx授权和身份认证机制、ACL模块等。



<br/>
<br/>



### 基于角色的访问控制

Role Based Access Control

> Enterprise-Only This feature is only available with an Enterprise Subscription.




<br/>
<br/>
<br/>




## 插件开发

[Plugin Development Introduction](https://docs.konghq.com/1.2.x/plugin-development/)
























<br/>
<br/>

---

<br/>
<br/>
























# Admin API

[Admin API](https://docs.konghq.com/1.2.x/admin-api/)








































<br/>
<br/>

---

<br/>
<br/>

















# k8s Ingress

[Kong Kubernetes Ingress](https://konghq.com/solutions/kubernetes-ingress/)


控制你的集群。拥有而不需管理k8s集群。Kong Ingress Controller在k8s集群中实现了认证(authentication)、转换(transformation)和其它功能，无需停机。

弥合传统和云原生之间的差距。Kong将k8s集群于跨任何环境或平台运行的服务连接起来——从数据中心到多云、裸机到容器。

<br>

![](/images/Kong/KongK8sIngress.png)

<br>

![](/images/Kong/KongK8sIngress2.png)

<br>

![](/images/Kong/KongK8sIngress3.png)


<br/>
<br/>


## Kong on K8s

[Kong and Kong Enterprise on Kubernetes](https://docs.konghq.com/install/kubernetes/)


<br>
<br/>



### Announcing the Kubernetes Ingress Controller for Kong

[Announcing the Kubernetes Ingress Controller for Kong](https://konghq.com/blog/kubernetes-ingress-controller-for-kong/)


今天，我们很高兴地宣布 Kubernetes Ingress Controller for Kong。

容器编排正在迅速变化，以满足软件基础架构的需求，这需要比以往更高的可靠性(reliability)、灵活性(flexibility)和效率(efficiency)。k8s正是这些工具的最前沿。

但是，这些工作负载的关键是一个网络堆栈（可以支持大规模集群容器协调器的高度动态化部署）。

Kong是一个高性能的开源API Gateway、 Traffic Control、MicroServices Management Layer，可支持这些工作负载所需的苛刻的网络要求。然而，Kong并没有强迫团队采用一刀切(one-size-fits-all)的解决方案。为了为深入的软件和企业生态系统提供流量，Kong提供了一个丰富的插件生态系统，扩展了Kong的认证、流量控制...

将Kong部署到k8s是一个简单的过程，但将k8s与Kongd的服务集成是一个手动过程。这就是为什么我们很高兴宣布 [Kong Ingress Controller for Kubernetes](https://github.com/Kong/kubernetes-ingress-controller)。

通过与[k8s Ingress Controller Spec](https://kubernetes.io/docs/concepts/services-networking/ingress/)集成，Kong直接与k8s生命周期相关联。随着应用程序的部署和新服务的创建，Kong将自动进行实时配置。

<br>

![](/images/Kong/kong-kubernetes-ingress-controller.png)



<br/>
<br/>


#### 配置Kong Ingress Controller


可参考GitHub的相关文档设置Kong k8s Ingress Controller。只需安装必需的k8s清单，如Ingress Controller Deployment自身、前端服务、Kong访问k8s API paths需要的所有RBAC组件。

这些清单适用于任何k8s集群。

```sh
curl https://raw.githubusercontent.com/Kong/kubernetes-ingress-controller/master/deploy/single/all-in-one-postgres.yaml \
| kubectl create -f -

```

安装Kong Ingress Controller之后，我们现在可以开始部署Services和Ingress Resource，以便Kong可以开始为我们的集群资源提供流量。

```sh
# 将测试服务部署到k8s集群
curl https://raw.githubusercontent.com/Kong/kubernetes-ingress-controller/master/deploy/manifests/dummy-application.yaml \
| kubectl create -f -

```

这部署了我们的测试应用，但我们仍需要一个Ingress Resource来为它提供流量。

```sh
# 使用以下清单为测试应用创建一个Ingress
echo "
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: foo-bar
spec:
rules:
- host: foo.bar
http:
paths:
- path: /
backend:
serviceName: http-svc
servicePort: 80
" | kubectl create -f -

```

通过部署我们的Kong Ingress Controller和我们的测试应用，我们现在可以开始为应用提供流量。

```
# export PROXY_IP=$(minikube   service -n kong kong-proxy --url --format "{{ .IP }}" | head -1)
# export HTTP_PORT=$(minikube  service -n kong kong-proxy --url --format "{{ .Port }}" | head -1)
# curl -vvvv $PROXY_IP:$HTTP_PORT -H "Host: foo.bar"
curl -vvvv kong-ip:kong-port -H "Host: foo.bar"
```


<br/>
<br/>


#### 添加插件

Kong Ingress Controller中的插件作为[k8s自定义资源(Custom Resource Definitions(CRDs))](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)来公开。它是k8s API Server上的第三方API对象，操作人员可以定义这些对象。

让我们在Kong示例中添加速率限制插件(rate limiting plugin)，并将其绑定到我们的Ingress Resource。Plugin与Ingress Resource一对一映射，使我们能够将插件应用于上游服务的方式进行细致的控制。

```sh
echo "
apiVersion: configuration.konghq.com/v1
kind: KongPlugin
metadata:
name: add-ratelimiting-to-route
config:
hour: 100
limit_by: ip
second: 10
" | kubectl create -f -


# kubectl patch
# 使用 strategic merge patch 更新一个资源的 field(s)
kubectl patch ingress foo-bar \
-p '{"metadata":{"annotations":{"rate-limiting.plugin.konghq.com":"add-ratelimiting-to-route\n"}}}'
```

现在应用了这个规则，我们再次访问我们的服务端点以获得以下响应：

```sh
curl -vvvv $PROXY_IP:$HTTP_PORT -H "Host: foo.bar"

> GET / HTTP/1.1

> Host: foo.bar

> User-Agent: curl/7.54.0

> Accept: */*

>

< HTTP/1.1 200 OK

< Content-Type: text/html; charset=ISO-8859-1

< Transfer-Encoding: chunked

< Connection: keep-alive

< X-RateLimit-Limit-hour: 100

< X-RateLimit-Remaining-hour: 99

< X-RateLimit-Limit-second: 10

< X-RateLimit-Remaining-second: 9
```

我们立即看到了我们的rate limiting headers已经应用，并可用于此端点。如果我们部署另一个Service和Ingress，我们的rate limit plugin将不适用，除非我们创建一个具有自己配置的新的KongPlugin资源并将注释添加到我们的新的Ingress。

现在我们有一个功能齐全的API Gateway Ingress Stack，完整的测试应用和速率限制插件。你已准备好使用Kong控制你的微服务！

跟上GitHub Kong Ingress Controller，请求新功能或报告问题。



<br/>
<br/>



### K8s Ingress Controller for Kong

GitHub: <https://github.com/Kong/kubernetes-ingress-controller>


使用官方[k8s Ingress Controller](https://github.com/Kong/kubernetes-ingress-controller)安装Kong。





































