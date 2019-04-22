---
title: Docker
date: 2018-03-27 08:38:33
updated: 2018-07-08 12:20:00
tags: Docker
categories: DevOps
---


参考：

- Docker文档:  <https://docs.docker.com/>
- <https://blog.csdn.net/sD7O95O/article/details/78623697>
- <https://www.zhihu.com/question/22969309/answer/34030581>


环境：

- CentOS7x86_64
- Docker v18.03



<br>
<br/>

<!--more-->

---

<br/>



# 概述


Docker是一个开发、shipping、运行应用程序的开放平台。Docker使你能够将应用程序与基础架构(infrastructure)分离开，从而可以快速交付软件。借助Docker，你可以像管理应用程序一样管理基础架构。利用Docker的方法快速进行运输、测试和部署代码，可以显著缩短编写代码和在生存环境中运行代码之间的延迟。


<br>


## Docker平台


Docker提供了在称为容器的松散隔离(isolated)环境中 打包和运行应用程序的能力。隔离性和安全性允许你在给定的主机上同时运行多个容器。容器是轻量级(lightweight)的，因为它们不需要hypervisor的额外负载，而是直接使用主机的内核运行。这意味着，与使用虚拟机相比，你可以在给定的硬件组合上运行更多的容器。你甚至可以在虚拟主机中运行Docker容器。

Docker提供了工具和平台来管理容器的生命周期(lifecycle)：

- 使用容器开发应用程序及其支持组件
- 容器成为分发和测试你应用程序的单元
- 准备好后，将你的应用程序部署到生产环境中，作为容器协调服


<br>


## Docker引擎


Docker引擎是一个包含如下部件的client-server应用程序：

- Server是称为守护进程的dockerd
- REST API是指定程序可用于与守护进程进行通信并指示其执行操作的接口
- Client是command line interface(CLI)

![](/images/Docker/engine-components-flow.png)

Docker的开源许可协议是Apache2.0


<br>


## 能用Docker做什么


**快速、一致的交付应用程序**

通过允许开发人员在 提供应用程序和服务的本地容器 的标准化环境 下工作，Docker简化了开发生命周期。容器非常适合**持续集成(continuous intergration,CI)和持续交付(continuous deliver,CD)**工作流程。

考虑如下示例场景：

- 开发者在本地编写代码，并使用Docker容器分享工作给他们的同事
- 使用Docker将应用程序push到测试环境，并自动执行和手动测试
- 当开发人员发现bug，他们能在开发环境中修复bug，并重新部署应用程序到测试环境进行测试和验证
- 测试完成后，向客户提供修补的应用程序 与将更新的image push到生产环境一样简单

<br>

**响应式部署和伸缩**

- Docker的基于容器的平台支持高度可移植的工作负载。Docker container可以运行在笔记本、物理机、虚拟机、云平台...
- Docker的可移植性和轻量化特性也使得动态管理工作负载非常容易，可以近乎实时地按业务需求扩展或拆分应用程序和服务

<br>

**在同一硬件上运行更多的工作负载**

Docker轻量且快速。它为基于hypersior的虚拟机提供了一种可行、经济高效的替代方案，因此你可以使用更多计算容量来实现业务目标。Docker是高密度环境和中小型部署的理想选择，你需要用更小的资源做更多的事情。


<br>


## Docker架构


Docker使用了client-server的体系架构。客户端向守护进程发送消息，守护进程负责构建、运行和分发 Docker容器。客户端和守护进程可以在同一系统上运行，也可将客户端连接到远程的Docker守护进程。客户端和守护进程使用REST API，通过Unix socket或network interface进程通信。

![Docker架构图](/images/Docker/architecture.png)


<br>


### Docker daemon


Docker daemon(`dockerd`)，监听Docker API请求并管理Docker对象——image、container、network、volume。
docker daemon还可与其它docker daemon通信来管理docker service。

<br>


### Docker client

Docker client(`docker`)是许多Docker用户与Docker进行交互的主要方式。客户端将命令发送给守护进程，守护进程执行命令。
Docker命令使用Docker API，Docker客户端可与多个守护进程进行通信。


<br>


### Docker registry


Docker registry存储Docker image。Docker Hub和Docker Cloud是任何人都可使用的public registry，你可以创建private registry。

`docker pull`或`docker run`需要的image便是从配置的registry中提取。`docker push`推送image到你配置的registry。


<br>


## Docker对象


当你使用Docker时，你会创建和使用 image、container、network、volume、plugin和其它对象。

<br>

### image

镜像是一个只读模板，带有创建Docker容器的说明。通常，镜像基于其它镜像，并具有一些额外的自定义功能。
例如，你可构建基于Ubuntu镜像的镜像，但会按照ApacheWeb服务器和应用程序，以及应用程序所需的配置。

你可能创建自己的镜像，或使用由别人创建并推送到registry上的镜像。构建自己的镜像，需要使用简单的语法创建一个Dockerfile，以定义创建镜像并运行它所需的步骤。

<br>

### container

容器是镜像的可运行实例。可将容器连接到一个或多个网络，将存储器连接到它，还可根据当前状态创建新镜像。

默认情况下，容器与其它容器以及主机是相对隔离的。你可以控制容器的网络、存储、其它底层子系统与其它容器或主机的隔离程度。

容器由镜像定义，以及你在创建或启动时提供给它的任何配置选项。当一个容器被移除时，其未被存储在永久存储器中的状态会消失。

栗子：

```
#运行一个Ubuntu镜像，交互地连接到本地命令会话

docker run -i -t ubuntu /bin/bash
```

以上命令会发生如下步骤:

1. 如果本地没有Ubuntu镜像，docker会从registry拉取，就好像你手动运行 `docker pull ubuntu`
2. Docker创建一个新容器，就好像你手动执行`docker container create`
3. Docker分配一个读写文件系统给容器，作为它的最后一层
4. 如果你没有指定任何网络选项，Docker会创建一个网络接口将容器连接到默认网络。
5. Docker开启容器并执行/bin/bash
6. 发送`exit`到`/bin/bash`，容器停止但并未被移除

<br>


### service


服务允许你伸缩多个Docker守护进程的容器，这些守护进程可以作为一个swarm与多个manager和worker一起工作。默认情况下，该服务在所有node之间进行负载均衡。

<br>


### 底层技术


Docker使用GO编写，利用Linux内核的几个特性来提供其功能。

<br>

**namespace**

Docker使用一个称为`namespace`的技术来提供称为容器的独立工作空间。当你运行一个容器时，Docker会为该容器创建一组命名空间。
命名空间提供了一个隔离层。容器的每个方面都在单独`namespace`中运行，并且其访问权限仅限于该单独的`namespace`。

Docker引擎在Linux上使用如下`namespace`：

- `pid` namespace： 进程隔离
- `net` namespace： 管理网络接口
- `ipc` namespace： 管理对IPC(InterProcess Communication)资源的访问
- `mnt` namespace： 管理文件系统挂载点
- `ust` namespace： 隔离内核和版本标识符(Unix Timesharing System)

<br>

**control groups**

Linux上的Docker Engine也依赖与另一种称为控制组(cgroups)的技术。cgroup将应用程序限制为一组特定的资源。控制组允许Docker引擎将可用的硬件资源共享给容器，并可选地强制实施限制和约束。
例如，你可限制特定容器的内存是CPU使用率等。

<br>

**union file systems**

union file systems(UnionFS)，是通过创建layer进行操作的文件系统，使得它们非常轻量和快速。Docker引擎使用UnioFS为容器提供构建block。Docker引擎可以使用多种UnionFS变体，包括AUFS, brrfs, vfs, DeviceMapper...

<br>

**container format**

Docker引擎将namespace、cgroup、UnionFS组合成一个名为容器格式的包装器。默认的容器格式为`libcontainer`。












<br>
<br/>

---

<br>





# 安装



Docker有两个可获取的版本：

- Community Edition(CE)
	+ 适合开始使用Docker并尝试基于容器的应用程序的开发人员和小型团队
- Enterprise Edition(EE)
	+ 专为企业开发和IT团队而设计，可以在生产规模上构建，发布和运行关键业务应用程序



<br>
<br/>



## CentOS7安装Docker CE



### OS要求


- CentOS7.x
- `centos-extras` repository
- 推荐使用`overlay2`存储驱动
- 安装新版本Docker需卸载老版本Docker
- Docker CE包被称为`docker-ce`


<br>



### 安装Docker CE

- <https://download.docker.com/>

多种安装方法：

- Docker's repository
- RPM package
- scripts

<br/>

**使用repository安装：**

```sh
 #安装依赖
yum install -y yum-utils device-mapper-persistent-data lvm2


 #设置repository
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo


 #安装Docker CE
yum install -y docker-ce

 #Docker安装但未启动，docker group会被创建，但没有用户添加到组中


 #在生产环境中，你可能需要安装特定版本的Docker CE，而不是最新版
yum list docker-ce --showduplicates | sort -r
yum search docker-ce --showduplicates


 #开启docker
systemctl start docker


 #测试docker
docker run hello-world

 #此命令下载一个测试image并将其运行到container中
 #Hello from Docker!
```

<br>

**使用package安装：**

```sh
 #下载rpm包
https://download.docker.com/linux/centos/7/x86_64/stable/Packages/


 #安装
yum install -y /path/docker-cexxx.rpm


systemctl start docker

docker run hello-world
```

<br/>

**使用scripts安装：**

```sh
curl -fsSL get.docker.com -o get-docker.sh

sh get-docker.sh

 #手动添加group合user
usermod -aG docker your-user
```


<br>
<br/>


### 卸载Docker CE


```sh
yum remove docker-ce

 #默认文件
rm -rf /var/lib/docker

 #你还需要手动删除其它配置文件
```







<br>
<br/>

---

<br>





# 开始



<br>
<br/>



## 关于Docker


Docker文档会有如下讲解：

- 设置你的Docker环境
- 在一个容器(container)中构建并运行一个镜像
- 延伸你的APP以便在多个容器中运行
- 在整个集群中分配你的APP
- 通过添加后端数据库来堆栈服务
- 将应用部署到生产


<br>


### Docker的概念


Docker是开发人员，系统管理员使用容器来开发、部署和运行APP的平台。使用Linux容器来部署APP被称为**集装箱化(containerzation)**

![](/images/Docker/laurel-docker-containers.png)

集装箱受欢迎的几点原因：

- 灵活(flexible)
- 轻量(lightweight)
- 通用(Interchangeable)
- 可移植(portable)
- 延伸(scalable)
- 堆栈(stackable)


<br>


### 镜像和容器


通过运行镜像(image)启动容器(container)。镜像是一个可执行包，包含运行APP所需的所有内容：代码，库，环境变量，配置文件...

容器是镜像的运行时(runtime)实例。在Linux上使用`docker ps`命令查看运行的容器列表。


<br>


### 容器和虚拟机


容器在Linux本地上运行，并与其它容器共享主机Kernel。它是一个独立的进程，不占其它可执行文件内存，使其轻量化。

虚拟机(VM)运行一个完整的访客操作系统，通过虚拟机管理程序访问主机资源。一般来说，虚拟机比大多数应用程序需要的资源更多。

![](/images/Docker/docker002.png)


<br>
<br/>


### 准备Docker环境


Docker版本：

- CE: Docker Community Edition
- EE: Docker Enterprise Edition

[Install Docker](#安装Docker)


<br>


## 测试Docker

```sh
docker --version


#查看详细信息
docker info


#测试安装工作是否正常
docker run hello-world


#查看镜像
docker image ls


#列出容器
docker container ls -all


#docker命令
docker
docker container --help
```


<br>


### 小结


集装箱化使得**CI/CD**无缝：

- 持续集成(Continuous integration, CI)
- 持续部署(continuous deployment, CD)
- APP无系统依赖
- 更新能够推送到分布式APP的任何部分
- 资源密度可以被优化


使用Docker，扩展APP的过程就是启动新的可执行文件，而不是运行繁重的VM主机。



<br>
<br/>



## 容器

Container


<br>


### 先决条件


```sh
docker run hello-world
```


<br/>


### 介绍


是时候使用Docker方式来构建一个APP了。

- 从应用程序的层次结构底部开始，这是一个容器(container)
- 在此级别之上，是一个服务(service)，它定义了容器在生产中的表现
- 最后，顶层是堆栈(stack)，定义所有服务的交互(interaction)

Like this:

- Stack
- Service
- Container


<br>


### 新开发环境


在过去，如果你要开始编写一个Python APP，你的第一要务是在你的机器运行时安装Python。但是，这会造成你的计算机上的环境，需要如预期般完美适合你的APP，并且还需要与你的生产环境相匹配。

使用Docker，你可以将一个可移植的Python运行时作为一个image，无需安装。接着，你的构建可以将基础Python image与APP代码一起包含在内，确保你的APP，依赖项...都构建一起。


<br>



### 使用`Dockerfile`定义一个容器


`Dockerfile`定义了容器内环境中发生的事情。访问的网络接口(network interface)和磁盘驱动(disk driver)等资源是在此环境中虚拟化的(virtualized)，与系统其余部分隔离。因此你需要将端口映射(map port)到外部世界，并明确指定要将哪些文件**复制**到此环境中。但是，在完成这些后，你完全可以将它们看做一致 —— 在`Dockerfile`中定义的构建的APP的行为与它运行时的行为完全相同。


**Dockerfile**

创建一个空目录，并创建一个名叫`Dockerfile`的文件，复制以下内容：

```
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

注意代理服务器会阻断你与APP的连接！

这个`Dockerfile`引用了一些我们还没有创建的文件，分别是`app.py`和`requirements.txt`。接下来创建它们。


<br>


### APP自身


创建另外的文件，如上面的`app.py`和`requirements.txt`，并将它们与`Dockerfile`放置于同一目录下。这就完成了我们的APP，这看起来非常简单。当这个`Dockerfile`被构建成一个image时，由于`Dockerfile`的`ADD`命令，`app.py`和`requirements.txt`仍然存在，而且由于使用了EXPOSE命令，`app.py`的输出仍可以通过HTTP访问。


**requirements.txt: **

```
Flask
Redis
```

**app.py: **

```
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

> 在容器内访问主机的名称将检索容器ID，这进程ID类似。

仅此而已，在你的系统中，你不需要任何Python或requirements.txt文件，也不需要在你的系统上安装 构建或运行的image。看起来你并没有真正用Python和Flask建立一个环境，但是你确实已经拥有了。



<br>



### 构建APP



我们准备去构建(build)APP。确保你仍在目录的顶层。

```
#查看是否还在顶层
ls

Dockerfile		app.py			requirements.txt



#在此目录运行build命令，这将创建一个Docker image，用 -t 命名
docker build -t friendlyhello .


#查看你build的image
docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
friendlyhello       latest              b24e21d7645f        13 minutes ago      150MB


#运行APP
docker run -p 4000:80 friendlyhello


#测试
curl http://localhost:4000
links http://localhost:4000


#在后台运行
docker run -d -p 4000:80 friendlyhello


#查看容器
docker container ls

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
146662dca737        friendlyhello       "python app.py"     16 seconds ago      Up 16 seconds       0.0.0.0:4000->80/tcp   goofy_chaplygin


#停止
Ctrl + C
docker container stop docker-ID
docker container stop 146662dca737
```

![效果图](/images/Docker/docker003.png)

<br>

端口重映射`4000:80`是为了证明Dockerfile中的EXPOSE与使用`docker run -p`发布的内容之间的区别。
在后续步骤中，我们只需将主机的80端口映射到容器的80端口就好。



<br>



### 分享你的image



为了演示刚才创建的image的可移植性(portability)，让我们上传build的image并在其它地方run它。毕竟，当你需要将container部署到生产环境时，你需要知道如何push注册。

注册表(registry)是一个repository的集合，而repository是image的集合——有点类似于GitHub repository，但代码是已经构建了的。
注册表上的账户可以创建许多repository。docker CLI 默认使用Docker's public registry。你也可以选择其它注册表，或创建自己的注册表。

<br>

**使用Docker ID登录：**

如果没有Docker账户，请先注册 <cloud.docker.com>。

```
docker login

docker login -u zhang21

#时候docker login认证过后，会有~/.docker/config.json文件，里面包含了docker认证信息
#k8s可使用此信息添加secret

cat ~/.docker/config.json
{
        "auths": {
                "https://index.docker.io/v1/": {
                        "auth": "base64encoding"
                }
        },
        "HttpHeaders": {
                "User-Agent": "Docker-Client/18.03.1-ce (linux)"
        }
```



<br>

**标记image：**

使用`username/repository:tag`将本地image与registry中的repository相关联。tag是可选的，但推荐使用tag。因为它是注册管理机构用于为Docker image提供版本的机制。为该内容提供一个有意义的repository和tag，例如`get-started:part2`。

```
docker tag image username/repository:tag

#例子
docker tag friendlyhello zhang/test:tag-test


#查看tag
docker images ls
```

<br>

**发布image：**

```
#上传你标记了的image到repository
docker push username/repository:tag

docker push zhang21/test:tag-test

#完成后，此image便可以公开获取
```

<br>

**从远处repository拉取并运行image：**

无论在哪里执行`docker run`，它都会将你的image以及Python和所有依赖关系一起拉取下来，并运行你的代码。

```
docker run -p 4000:80 username/repository:tag

docker run -p 80:80 zhang21/test:tag-test
```


<br>


### 本节基础命令


```sh
#从Dockerfile创建image
docker build -t image-name .

#运行image
docker run -p 4000:80 image-name

#后台运行
docker run -d -p 4000:80 image-name

#列出运行的容器
docker container ls
#列出所有容器，包括未运行
docker container ls -a

#优雅停止容器
docker container stop 容器ID
#强制停止
docker container kill 容器ID

#删除容器
docker container rm 容器ID
#删除所有容器
docker container rm $(docker container ls -a -q)

#列出镜像
docker image ls
#列出所有镜像
docker image ls -a

#删除镜像
docker image rm 镜像ID
#删除所有镜像
docker image rm $(docker image ls -a -q)

#登录
docker login

#标记
docker tag 镜像 username/repository:tag

#上传到注册表
docker push username/repository:tag

#从注册表拉取
docker run username/repository:tag
```



<br>
<br/>




## 服务

service


<br>


### 先决条件


- 安装Docker
- 获取Docker Compose
- 阅读Orientation
- 阅读Container
- 确保已发布friendlyhello image到你的registry
- 确保你的image工作为一个部署的container。`docker run -p 80:80 username/repo:tag`


<br>


### 介绍


在此，我们**扩展(scale)**APP并启用**负载均衡(load balancing)**。要做到这样，我们必须在分布式(distributed)应用程序的层次结构中升一级: 服务

- Stack
- Service
- Container


<br>


### 关于服务


在分布式应用程序中，应用程序的不同部分称为服务(service)。

例如，一个视频共享站点。那么它可能包含：

- 用于将应用程序数据 存储到数据库中的服务
- 用户上传后的视频转码服务
- 前端服务
- ...

服务是真正的生产环境中的容器。一个service只运行一个image，但它可修改image的运行方式 —— 哪个端口、容器应该运行多少个副本以便于服务所需的容量等.
伸缩服务会更改运行该软件的容器实例数量，从而为进程中的服务分配更多的计算资源。

在Docker平台上定义、运行和伸缩服务都是很简单的 —— 只需修改`docker-compose.yml`文件。


<br>


### 你的第一个`docker-compose.yml`文件


`docker-compose.yml`是一个YAML文件，它定义了Docker container在生产中的行为方式。

**docker-compose.yml：**

将如下信息保存为`docker-compose.yml`，确保你已经`pushed the image`到registry，并通过修改`.yml`文件的image detail来替换`username/repo:tag`。

```yml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
networks:
  webnet:
```

<br>

`docker-compose.yml`文件告诉Docker之下如下操作：

- pull the image
- Run 5 instances of that image as a service called `web`
- 限制每个实例最多使用10%的CPU和50MB的RAM
- 如果一个失败，马上重启container
- 映射主机的80端口到web的80端口
- 指示web container通过称为`webnet`的负载均衡网络共享80端口
- 使用默认设置定义`webnet`网络


<br>


### 运行你的负载均衡APP


```sh
docker swarm init


#运行并设置APP名字
docker stack -c docker-compose.yml app-name

docker stack -c docker-compose.yml LoadBalance


#在一个主机上，单个服务栈通过部署的image运行5个container instance


#获取service ID
docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE                   PORTS
3d1a48yse0t4        LoabBalance_web     replicated          5/5                 zhang21/test:tag-test   *:80->80/tcp


#查看服务中的任务
docker service ps app-name_web


docker container ls -q

#5个容器ID
c7ce0075890e
52ba026bf28c
6d4381be438f
bd297a42e89d
357b05cc38eb


#访问的时候容器ID会在此5个负载中变化
```

在服务中运行的单个container称为任务(task)。任务是具有数字增量的唯一ID，最大数量是在`docker-compose.yml`中定义的副本数量。


<br>


### 伸缩APP


通过修改`docker-compose.yml`中`replicas`的值，并重新运行`docker stack deploy -c xxx app-name`来伸缩APP。

Docker执行就地更新，不需要stack down或kill any containers.


**卸下APP和swarm：**

```sh
#app
docker stack rm app-name

docker stack rm LoadBalance


#swarm
docker swarm leave --force
```


使用Docker扩展APP非常简单。


<br>


### 本节命令


```sh
#列出栈或APP
docker stack ls

#运行指定配置文件
docker stack deploy -c <composefile> <appname>

#列出与APP相关联的服务
docker service ls

#列出与APP相关联的任务
docker service ps <service>

#检查任务
docker inspect <task or container>

#列出容器ID
docker container ls -q

#除掉APP
docker stack rm <appname>

#从管理中除掉一个单一节点swarm
docker swarm leave --force
```



<br>
<br/>


## swarm


<br>


### 先决条件


- 前面几个小节的内容


<br>



### 介绍


前面你将一个服务运行在生产环境，并扩展为5个副本进程。

在此，你将APP部署到到集群上，并在多台机器上运行它。通过将多台主机连接到成为**swarm**的**Dockerized**集群，使得多容器、多主机应用成为可能。


<br>


### 理解swarm集群


swarm是一组运行Docker并加入到集群中的机器。这样以后，你可以在集群的swarm manager上执行Docker命令。swarm中的机器可以是物理的或虚拟的，当他们加入swarm后，他们便被成为node。

swarm manager可以使用多种策略来运行容器，你可在compose file中指定相应的策略。

swarm manager是swarm中唯一可以执行命令、授权其他机器作为工作者加入swarm的机器。工作者(worker)只能在那提供能力(capacity)，并没有权力告诉任何机器能够做什么。

但目前为止，你已经在本机机器上以单主机(single host)模式使用Docker。但Docker也可以切换为swarm(集群)模式，这就是使用swarm的原因。立即启用swarm模式使得当前机器成为swarm manager。从此，Docker将运行在你管理的swarm上执行命令，而不仅仅是在当前机器上执行。


<br>


### 建立swarm


一个swarm由多个节点组成，不管它是虚拟机还是物理机。

- 基本概念很简单，运行`docker swarm init`来开启swarm模式并使得当前机器成为swarm manager
- 在其它机器上运行`docker swarm join`使他们作为worker加入swarm

<br>

**栗子：**
使用VM快速创建两台机器的集群，并将其变为swarm。

使用`docker-machine`创建一对VM:

```sh
#CentOS7

#安装VirtualBox
wget https://download.virtualbox.org/virtualbox/5.2.8/VirtualBox-5.2-5.2.8_121009_el7-1.x86_64.rpm && yum install -y Virtual.xx.rpm


#安装docker-machine
 curl -L https://github.com/docker/machine/releases/download/v0.14.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && install /tmp/docker-machine /usr/local/bin/docker-machine


#在BIOS中开启虚拟化支持

#在VMware中开启虚拟化支持(如果是VM)


docker-machine create --driver virtual myvm1
docker-machine create --driver virtual myvm2


#列出虚拟机
docker-machine ls
```

<br/>

**初始化swarm并添加node**

第一台机器作为swarm manager，执行命令和join认证，后面的机器作为worker。

你可以使用`docker-machine ssh`发送命令到VM。在`swarm mananger`上执行`docker swarm init`初始化：

```
docker-machine ssh <swarm manager> "docker swarm init --advertise-assr <mananger-IP>"

#add worker
docker swarm jion --toker <token> <wroker-ip>:<port>


#添加manager
docker swarm join-token manaer
```


**由于我的虚拟的无法使用VT，因此我用的两台机器两个Docker来做swarm。**

```sh
#初始化这台机器默认为manager
docker swarm init


#作为worker加入，ip是manager的
#以下信息会在manager初始化时生成
#注意防火墙，可能会阻碍加入

docker swarm join --toker <toker> <ip:port>
docker swarm join --token SWMTKN-1-3vrbnuneu0hyu41evxlhbn5fp04ad5jvg9v5rzvdaedg2bghkt-e24mjnni3hu7782t3gkz0ny39 172.16.129.150:2377


#查看swarm
docker node ls


#离开swarm
docker swarm leave
```


<br/>


### 在swarm集群上部署APP


主需要记住，只有swarm manager才能执行docker命令，worker仅仅是容量(capacity)。

在swarm manager上使用`docker-composr.yml`和`docker stack deploy`命令来部署APP。使用`docker  service ps <service name>`来验证部署。

```
#在manager部署
docker stack deploy -c ./docker-compose.yml LoadBalance


docker service ls

docker stack ls


#注意node名
docker stack ps LoadBalance

ID                  NAME                IMAGE                   NODE                DESIRED STATE       CURRENT STATE             ERROR               PORTS
6nrn4mwc6pvt        LoadBalance_web.1   zhang21/test:tag-test   zhang22             Running             Preparing 2 minutes ago
bpssrnzesl7n        LoadBalance_web.2   zhang21/test:tag-test   zhang22             Running             Preparing 2 minutes ago
kmhd8p5wkc12        LoadBalance_web.3   zhang21/test:tag-test   zhang21             Running             Running 2 minutes ago
i0pkf4foms87        LoadBalance_web.4   zhang21/test:tag-test   zhang22             Running             Preparing 2 minutes ago
rvtpjk781frn        LoadBalance_web.5   zhang21/test:tag-test   zhang21             Running             Running 2 minutes ago


#分别访问个主机的IP
#创建的网络在它们之间共享并负载均衡
links ip1
links ip2
```

两个IP地址工作的原因是集群中的节点参与入口(ingress)路由网络(routing mesh)。这可以确保部署在swarm中某个端口的服务始终将该端口保留给自己，而不管实际运行容器的节点是什么。

![入口路由](/images/Docker/ingress-routing-mesh.png)


<br>


### 清理并重启


```
docker stack rm LoadBalance
```



<br>
<br/>




## stack



先决条件，已完成前面的步骤。


<br>


### 介绍


你已到达分布式应用程序层次结构的顶端——stack。堆栈是一组相互关联的服务，它们可以共享依赖关系，并可以进行协调和缩放。单个堆栈能够定义和协调整个应用程序的功能(尽管非常复杂的应用程序可能需要使用多个堆栈)。

在前面使用的`docker deploy`——是运行在单主机上的单个服务堆栈，这通常不会发生在生产环境中。在这里，你会使用学到的东西使多个服务相互关联，并在多台机器上运行它们。


<br>


### 添加一个新服务并部署


docker-compose2.yml

```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      restart_policy:
        condition: on-failure
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
    ports:
      - "80:80"
    networks:
      - webnet
  #可视化
  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet
networks:
  webnet:
```

新增的东西使web对等服务，称为visualizer。注意两个事：

- volumes: 给予visualizer访问主机Docker的socket文件
- placement： 确保服务运行在manager而不是worker上

```
docker stack deploy -c ./docker-compose2.yml stack-test

Creating network stack-test_webnet
Creating service stack-test_visualizer
Creating service stack-test_web


#查看visualizer，要等一会才能正常访问，别着急
访问 IP:8080
```

![](/images/Docker/visualizer.png)


<br>


### 持久化数据


让我们再次通过相同的工作流程来添加用于存储应用程序数据的Redis数据库。

docker-compose3.yml，添加一个Redis服务器：

```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      restart_policy:
        condition: on-failure
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
    ports:
      - "80:80"
    networks:
      - webnet
  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet
  redis:
    image: redis
    ports:
      - "6379:6379"
    volumes:
      - "/home/docker/data:/data"
    deploy:
      placement:
        constraints: [node.role == manager]
    command: redis-server --appendonly yes
    networks:
      - webnet
networks:
  webnet:




#部署
docker stack deploy -c docker-compose3.yml redis-test



#测试
访问 IP:port
```


Redis是一个Docker library中的官方image，并被授予redis镜像名称。

redis规范中有几件事使数据在这个堆栈的部署之间持续存在：

- redis运行在manager，所以它总是使用相同的文件系统
- redis将数据存储在上面的目录
- 确保redis服务始终使用相同的主机
- 确保存储的数据的连续性

如果没有创建，redis会将数据存储在容器文件系统的/data中，如果该容器被重新部署，则数据将被清除。




<br>
<br/>



## 部署APP


先决条件为前面的操作步骤。


<br>


### 介绍


compose file在生产环境中的效果与在您的计算机上的效果相同。


<br>


### 选择版本

我安装的是社区版(ce)。如果你在生产环境中使用docker-ce，则可以使用Docker Cloud帮助管理你的应用程序，如AWS、Aliyun、腾讯云。
docker cloud： <cloud.docker.com>, 可注册后建立、上传、管理自己的repo。

设置和部署：

- 连接Docker Cloud并授权它自动为你配置Dockerize VM
- 使用Docker Cloud创建你的计算资源和swarm
- 部署应用程序


<br>


### 连接DockerCloud


你可以标准模式或swarm模式运行Docker Cloud。

- AWS配置指南
- Aliyun配置指南
- 腾讯云配置指南


<br>


### 创建swarm


你可在Docker Cloud UI创建你的node，或`docker swarm init|join`命令。


<br>

### 在云提供商上部署应用程序

![栗子](/images/Docker/dockercloud.png)

我觉得阿里云和腾讯云也有对应的平台。

运行部署命令: `docker stack deploy -c xxx.yml <cus_appname>`，现在你的APP就运行在云提供商上。

**运行swarm命令来验证部署**

```
docker node ls

docker service ls

docker service ps <service>
```

**在云提供商开放端口**

service | type | protocol | port
- | - | - | -
web | http | tcp | 80
visualizer | http | tcp | 8080
redis | tcp | tcp | 6379

具体操作参见各云提供商。


<br>


### 迭代和清理


- 改变`*.yml`文件伸缩应用程序
- 使用`docker stack deploy`部署应用程序
- push和pull image
- 使用`docker stack rm <name>`清除stack



<br>
<br/>


## 修改Docker默认路径

docker默认的目录为`/var/lib/docker`，但很多时候`/var`目录并没有单独挂载，可能导致空间不够。
前提是你已经把源配置目录对应的文件拷贝到替换的目录。


- 方法1：

```
systemctl stop docker
cd /etc/docker

vim daemon.json

{
	"graph": "/opt/docker"
}


systemctl start docker
#systemctl reload docker

#查看变更
docker info

```

<br>

- 方法2:

```
systemctl stop docker
cd /etc/sysconfig/

vim docker-storage

DOCKER_STORAGE_OPTIONS=--graph=/opt/docker


systemctl start docker

#查看变更
docker info

```


<br>
<br/>


## 容器服务自启动

在运行docker容器时可以加如下参数来保证每次docker服务重启后容器也自动重启:

```
docker run --restart=always -d -p 80:80 <container-id>

#对于已启动的容器服务，更新它
docker update --restart=always <container-id>

```


<br>
<br/>


## 交互式容器

进入Docker容器以获得交互式体验。

```
docker exec -it <container> /bin/bash

docker exec -it <container> /bin/sh

```


<br/>
<br/>



## 使用systemd

默认情况下，容器是不直接支持使用systemd的。可在运行容器时添加选项来使用systemd。

```
#centos:7
docker run -dit --privileged --name=centos7-systemd  centos:7 init

```




<br>
<br/>



## 日志

- docker服务日志：
	+ `journalctl -u docker.service`
- docker容器日志：
	+ `<docker-graph>/containers/<container-id>/<container-id>-json.log`
	+ 由于容器ID会变化，请注意提取容器ID
	+ 可使用ELK在此收集容器日志


<br>
<br/>


## 更新镜像


使用`docker commit`从改变的容器中生成一个新镜像。

更新镜像步骤：

1. 备份镜像: `docker tag`
2. 运行镜像
3. 修改容器
4. 生成新镜像: `docker commit`
5. 推送镜像: `docker push`



<br/>
<br/>


## 动态映射端口

如何给运行中的容器添加映射端口。有两种方法:

- 将运行的容器生成一个新镜像，之后有这个镜像重新映射端口
- 通过iptables

<br>

第一种方法就相当于重新启动一个镜像，在启动时重新映射端口。实在是麻烦。

<br>

由于docker 命令设置端口映射其实也就是下发 `iptables` 规则，所以我们可以直接创建 `iptables` 规则进行端口流量转发。

```bash
#查看本机docker iptabels rules
iptables-save

#我的一个hexo镜像
#-A DOCKER -d 172.17.0.2/32 ! -i docker0 -o docker0 -p tcp -m tcp --dport 4000 -j ACCEPT


#再它在添加一个端口
iptables -t nat -A DOCKER ! -i dokcer0 -p tcp -m tcp --dport 56789 -j DNAT --to-destination 172.17.0.2:56789
```



## 备份与恢复


<br>


### 备份容器

- `docker commit`: 生成新镜像
- `docker save`： 生成本地tar包

```
Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

docker commit -m "Just a test" -p ${container-id} Zhang21/test:01
docker image ls
docker login
docker push




Usage:  docker save [OPTIONS] IMAGE [IMAGE...] [flags]

docker save -o /path/${image}.tar ${image}
ls /path
```


<br>

### 恢复容器

- `docker run ${image}`
- `docker load`: 载入本地.tar镜像

```
Usage:  docker load [OPTIONS]

docker load -i /path/${image}.tar
docker image ls
```


<br>
<br/>


## 应用场景与注意事项


<br>

### 应用场景

- 本地依赖
- 搭建环境
- 微服务
- 自动测试
- 部署过程
- CI/CD
- 多租户环境
- 一台机器的多个APP
- 弹性伸缩
- 资源隔离


<br>


### 注意事项

- 一个进程，一个容器
不推荐在Docker容器中运行多个进程！

- 不要将数据存放到容器内
所以请使用挂在卷的方式映射到本地磁盘目录


- 使用磁盘进行数据存储

- 容器通信
每当一个Docker容器需要与另一个容器通信时，传递信息最好使用名称或环境变量。

- 以non-root用户运行Docker
默认情况下，Docker容器以root用户身份运行，众所周知，以root用户运行的容器完全可以控制主机系统。

- 注意容器的体积
选择一个容器的主要原因之一是它的体积小。但是，如果你把它做得更大，它的主要优势就没了。

- 制定控策略
开发和部署Docker容器不是你的工作的结束。您需要持续监控已部署的容器以及整个系统的运行状况。选择合适的工具并制定一个策略来有效地监控您的Docker容器，以确保最短的停机时间，从而使客户满意。

- 安全问题
安全补丁、防火墙...







<br>
<br/>

---

<br>



# Dockerfile


参考:

- <https://docs.docker.com/engine/reference/builder/>
- <https://yeasy.gitbooks.io/docker_practice/content/image/build.html>


<br>


将镜像每一层的修改、安装、配置、操作的命令写入Dockerfile，并用它来构建、定制镜像，那么镜像构建透明性问题便会得到解决。

Dockerfile是一个文本文件，包含了一条条指令(instrction)，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。



<br>
<br/>



## 使用Dockerfile定制镜像


<br>


### FROM


所谓指定镜像，就是以一个镜像为基础，在其上进行定制。基础镜像必须指定，而`FROM`就是指定**基础镜像**，因此一个`Dockerfile`中`FROM`是必备的指令，并且必须是第一条指令。

只有有可能，请使用当前官方repo作为你的基础镜像。我们推荐使用**Alpine**镜像，因为它严格控制，体积小(只有5MB)，同时也是完整的Linux发行版。

Docker Hub中有很多常用的官方镜像——常用软件、常用语言和常用系统镜像。

```
FROM nginx


#特殊镜像，scratch，空白镜像
FROM scratch
```


<br>


### RUN


在多行中使用反斜杠`\`或复杂的`RUN`语句，使Dockerfile更具可读性、易理解性和可维护性。

`RUN`指令是用来执行命令行命令的。有两种格式：

- `shell`格式
	+ `RUN <CMD>`，就像直接在命令行中输入命令一样
- `exec`格式
	+ `RUN ["可执行文件", "参数"]`，这更像函数调用中的格式

```
FROM debian:jessie


RUN apt-get update
RUN apt-get install -y gcc libc6-dev make
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz"
RUN mkdir -p /usr/src/redis
RUN tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1
RUN make -C /usr/src/redis
RUN make -C /usr/src/redis install

```
Dockerfile中的每一个指令都会建立一层，`RUN`也不例外。每一个`RUN`的行为，就和手工建立镜像的过程一样 —— 新建立一层，在其上执行这些命令，执行结束后，`commit`这一层的修改，构成新的镜像。

上面这种写法，创建了7层镜像，这是完全没有意义的，而且很多运行时不需要的东西都被装进了镜像里，比如编译环境和更新的软件包等。结果就会产生非常臃肿、非常多层的镜像，不仅增加了构建部署的时间，也容易出错。这是很多初学Docker的人常犯的一个错误。

UnionFS是Linux、FreeBSD的文件系统服务，UnionFS是有最大层数限制的。

修改后的Dockerfile：

```
FROM debian:jessie

RUN buildDeps='gcc libc6-dev make' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps

```
仅仅使用一个`RUN`指令，并使用`&&`将各指令串联起来。将之前的7层简化为1层。在编写`Dockerfile`时，要经常提醒自己，这并不是在写shell脚本，而是在定义每一层该如何构建。

`Dockerfile`支持shell类的换行`\`、注释`#`等格式，良好的格式，如换行、缩进、注释等，会让维护、排障更为容易，这也是一个好习惯。

此外，还可看到命令最后添加了清理工作的命令，删除了为编译构建所需要的软件，清理了所有下载文件。这很重要，镜像是多层存储，每一层的东西并不会在下一层被删除，会一直跟随镜像。
因此，构建镜像时，一定要确保每一层只添加真正需要添加的东西，任何无关的东西都应该清理掉。
很多人初学docker制作出了很臃肿的镜像，原因之一就是顽疾了每一层构建的最后一定要清理无关文件。



<br>


### 构建镜像

在`Dockerfile`目录下执行：

```
#docker build [OPTIONS] PATH | URL | - [flags]
#Build an image from a Dockerfile


#-t指定镜像名称
#.指的是上下文目录
docker build -t nginx:test .
```

<br>

**构建上下文(content)**

上面的`.`是在指定**上下文路径**。

当我们在进行镜像构建的时候，并非所有的定制都会通过`RUN`指令完成，经常会需要一些本地文件复制进镜像，比如通过`COPY`, `ADD`指令。而`docker build`命令并非是在本地构建镜像，而是在服务端，也就是Docker引擎`dockerd`中构建的。那么在这种C/S架构中，如何才能让服务端获得本地文件呢？

这就引进了上下文的概念。当构建的时候，用户会指定构建镜像的上下文的路径，`docker build`命令得知这个路径后，会将路径下的所有内容打包，然后上传给Docker引擎。这样Docker引擎收到这个上下文包后，展开就会获得构建镜像所需的一切文件。

<br>

```
#复制上下文目录下的package.json
COPY ./package.json /app/
```
因此`COPY`这类指令中的源文件的路径都是相对路径，因为绝对路径已经超出了上下文的范围，Docker引擎无法获取这些位置的文件。如果真需要这些文件，请将它们复制到上下文目录中去。

理解构建上下文对于镜像构建很重要，避免犯一些不应该的错误。

一般来说，应将`Dockerfile`置于一个空目录下，或者项目根目录下。如果该目录下没有所需文件，则应该把所需文件复制一份过来。如果目录下有些东西不希望构建时传给Docker引擎，可以写一个`.dockerignore`文件，用于剔除不需要作为上下文传递给Docker引擎的。

实际上，`Dockerfile`的文件名并不要求必须为`Dockerfile`，也并不要求必须位于上下文目录中。可使用`-f`指定某个文件为`Dockerfile`。


<br>

**其它`docker build`的用法**

1. 直接使用Git repo进行构建

```
#docker build URL

docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14
#docker会自己去clone、切换分支、并进入指定目录开始构建
```


<br>

2. 使用给定tar压缩包构建

```
docker build http://server/context.tar.gz

#自动下载/解压缩 压缩包，以其作为上下文，开始构建
```


<br>

3. 从标准输入中读取Dockerfile进行构建

```
docker build - < Dockerfile


cat Dockerfile | docker build -


docker build - < context.tar.gz
```



<br>
<br/>



## Dockerfile指令


`Dockerfile`提供了十多个指令供我们操作。


<br>


### LABLE

你可以为你的镜像添加标签，以助于通过项目来组织镜像，记录相关信息。

```Dockerfile
# Set multiple labels at once, using line-continuation characters to break long lines
LABEL vendor=ACME\ Incorporated \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"
```

<br>


### COPY

尽管`ADD`和`COPY`在功能上相似，但一般来说，`COPY`是首选，因为它比`ADD`更透明。
`COPY`只支持将本地文件复制到容器中，而`ADD`具有一些功能(如提取tar文件和远程URL支持)

<br>

`COPY`,复制文件。
从构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内的<目标路径>位置。

> 源路径可以是多个，或通配符(需满足Go的规则)
> 目标路径可是容器内的绝对路径，也可是相对于工作目录(`WORKDIR`)的相对路径。目标路径不需要事先创建。
> 使用COPY指令，源文件的各种元数据都会保留 —— 如读、写、执行权限、文件变更时间...

<br>

```
COPY  <sourch>  <destination>

#或
COPY  ["<source1>", ... "<destination>"]


#栗子
COPY package.json /usr/src/app/
COPY hom* /mydir/
COPY hom?.txt /mydir/


#目录
COPY dir/ /dir/
#复制目录的错误用法
#COPY  dir/* /dir/
```


<br>


### ADD

`ADD`是更高级的复制文件。
`ADD`和`COPY`的格式和性质基本一致，但增加了一些功能。`ADD`支持通过URL从远程服务器读取资源，但对远程的压缩包没有解压缩功能。
尽可能的使用`COPY`，因为`COPY`的语义很明确，就是复制文件而已，而`ADD`则包含了更复杂的功能，其行为也不一定很清晰。
最适合`ADD`的场合，就是所提及的需要自动解压缩的场合。

因此在`COPY`和`ADD`指令中选择的时候，可以遵循这样的原则，所有的文件复制均使用`COPY`指令，仅在需要自动解压缩或远程资源的场合使用`ADD`。

```
FROM scratch

ADD ADD http://foo.com/bar.go /tmp/main.go

ADD abc.tar.gz / && \
    http://example.com/big.tar.xz /usr/src/things/ && \
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```


<br>


### CMD

`CMD`，容器启动命令。用于运行镜像中包含的软件以及任何参数。

也有两个格式：

- `shell`格式： `CMD <command>`

`shell`格式，在实际中会被包装成`sh -c`的参数形式进行执行：

```
CMD echo $HOME

#转变为
CMD["sh", "-c", "echo $HOME"]

#-c string If  the  -c option is present, then commands are read from string.


#这就是为什么我们可以使用环境变量的原因，因为这些环境变量会被 shell 进行解析处理
```

<br>


- `exec`格式： `CMD ["可执行文件", "参数1", "参数2" ...]`
CMD几乎总是以此格式使用。



Docker不是虚拟机，容器就是进程。既然是进程，那么在启动容器的时候，需要指定所运行的程序及参数。`CMD`指令就是用于指定默认的容器主进程的启动命令的。
``

对于容器而言，其启动程序就是容器应用进程，容器就是为了主进程而存在的，主进程退出，容器就失去了存在的意义，从而退出，其它辅助进程不是它需要关心的东西。


<br>


### ENTRYPOINT

`ENTRYPOINT`，入口点。指令格式同样分为`shell`格式和`exec`两种。

`ENTRYPOINT`和`CMD`一样，都是在指定容器启动程序及参数。
当指定了`ENTRYPOINT`后，`CMD`的含义就发生了改变，不再是直接的运行其命令，而是将`CMD`的内容作为参数传给`ENTRYPOINT`指令。即变为如下模式：

```
<ENTRYPOINT> "<CMD>"
```

<br>

有几大好处：

- **让镜像变成像命令一样使用**

```
#可以从腾讯上拉取，快一些
#ccr.ccs.tencentyun.com/qcloud/ubuntu

FROM ubuntu:16.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
CMD [ "curl", "-s", "http://ip.cn" ]


docker build -t myip
docker run myip
#当前 IP：182.150.x.xx 来自：四川省成都市 电信
```

不过命令总有参数，例如我想查看HTTP header，使用`-i`参数

```
docker run myip -i
#这样会报错，-i替换了CMD命令，而不是-s参数，然而-i并不是命令


#重新完整输入命令
docker run myip curl -s http://ip.cn -i
#这样又太麻烦
```

这时便可以使用`ENTRYPOINT`解决这个问题。

```
FROM ubuntu:16.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]


docker build it myip
docker run myip
#当前 IP：182.150.x.xx 来自：四川省成都市 电信


docker run myip -i
#成功
```

当存在`ENTRYPOINT`后，`CMD`的内容将作为参数传递给`ENTRYPOINT`，而`-i`就是新的`CMD`，因此会作为参数传递给`curl`，从而达到预期效果。

<br>

- **应用运行前的准备工作**

有时，在启动前需要做一些准备工作。

- 如MySQL，需要一些配置文件、初始化工作，这些工作需要在MySQL server运行前解决
- 避免使用root用户去启动服务，从而提高安全性
- 这些准备工作和`CMD`无关


<br>


### ENV

`ENV`，设置环境变量。
为了使新软件更容易运行，使用此命令为你的容器内安装的软件更新环境变量。

两种格式：

- `ENV <key> <value>`
- `ENV <key1>=<value1> <key2>=<value2>...`

```
ENV PATH $PATH:/root/bin \
	EMAIL abc@zhang21.cn \
	NAME="Zhang21"

```

下列指令可以支持环境变量展开： `ADD`, `COPY`, `ENV`, `EXPOSE`, `LABEL`, `USER`, `WORKDIR`, `VOLUME`, `STOPGIGNAL`, `ONBUILD`。

通过环境变量，我们可以让一份`Dockerfile`制作更多的镜像，只需使用不同的环境变量即可。


<br>


### ARG

`ARG`，构建参数

格式：

- `ARG <参数名>[=<默认值>]`

<br>

构建参数和`ENV`的效果一样，都是设置环境变量。所不同的是，`ARG`所设置的构建环境的环境变量，在将来容器运行时是不会存在这些环境变量的。



<br>


### VOLUME

`VOLUME`，定义匿名卷。用于显示有docker容器创建的任何数据库存储区域，配置存储或文件/文件夹。
强烈建议将VOLUME用于镜像的任何可变部分和用户可用部分。

格式：

- `VOLUME ["<路径1>", "<路径2>"...]`
- `VOLUME <路径>`

<br>

容器运行时应该尽量保持容器存储层不发生写操作，对于数据库类需要保存动态数据的应用，其数据库文件应该保存于卷(volume)中。
为了防止运行时用户忘记将动态文件所保存目录挂载为卷，在`Dockerfile`中，我们可以事先指定某些目录挂载为匿名卷，这样在运行时如果用户不指定挂载，其应用也可以正常运行，不会像容器存储层写入大量数据。

```
#在运行时自动挂载为匿名卷
VOLUME /data


#覆盖挂载
docker run -d -v mydata:/data xxx
```


<br>


### EXPOSE

`EXPOSE`，声明容器监听连接的端口。

格式：

- `EXPOSE <端口1> [<端口2>...]`

`EXPOSE`指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务。
在`Dockerfile`中写入这个声明有两个好处：

- 一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便映射
- 另一个用处则是在运行时使用随机端口映射(未定义时)

要将`EXPOSE`和在运行时使用`-p <宿主端口>:<容器端口>`区分开。`EXPOSE`仅仅声明容器打算使用哪些端口，并未包含端口映射。


<br>


### WORKDIR

`WORKDIR`，指定工作目录。为了清晰可靠，请使用绝对路径。

使用`WORKDIR`指令可以来指定工作目录，以后各层的当前目录就被改为指定的目录，如目录不存在，`WORKDIR`会帮你建立目录。
如果需要改变`Dockerfile`各层的工作目录的位置，那么应该使用`WORKDIR`指令。

格式：

- `WORKDIR <工作目录>`


<br>


### USER

`USER`，指定当前用户。
如果服务可以在非特权模式下运行，请使用USER将其改为non-root用户。首先在Dockerfile中创建相应的用户和组:

```
RUN groupadd -r group && \
	useradd -r -g group group
```

`USER`和`WORKDIR`相似，都是改变环境状态并影响以后的层。`WORKDIR`是改变工作目录，`USER`则是改变之后的层执行`RUN`, `CMD`, `ENTRYPOINT`这类命令的身份。这个用户必须存在。

格式：

- `USER <用户名>`


```
USER redis
RUN ["redis-server"]
```


<br>


### HEALTHCHECK

`HEALTHCHECK`，健康检查
`HEALTHCHECK`指令告诉docker应该如何进行判断容器的状态是否正常。

格式：

- `HEALTHCHECK [选项] CMD <命令>`， 设置检查容器健康状况的命令
- `HEALTHCHECK NONE`， 如果基础镜像有健康检查，使用这行可以屏蔽其健康检查指令


当在一个镜像指定了`HEALTHCHECK`指令后，用其启动容器，初始状态会为`starting`，在`HEALTHCHECK`指令检查成功后变为`healthy`，如果连续一定次数失败，则会变为`unhealthy`。
和`CMD`, `ENTRYPOINT`一样，`HEALTHCHECK` 只可以出现一次，如果写了多个，只有最后一个生效。


<br>


### ONBUILD

`ONBUILD`，为他人做嫁衣。

`ONBUILD`是一个特殊的指令，它后面跟的是其它指令。而这些指令，在当前镜像构建时不会被执行。只有当以当前镜像为基础镜像(父镜像)，去构建下一级镜像(子镜像)的时候才会被执行。`ONBUILD`命令在子镜像的Dockerfile中任何命令之前执行。
`Dockerfile`中的其它指令都是为了定制当前镜像而准备的，唯有`ONBUILD`是为了帮助别人定制自己而准备的。

格式：

- `ONBUILD <其它指令>`



<br>
<br/>


## Dockerfile多阶段构建


**全部放入一个Dockerfile**

将所有的构建过程包含在一个`Dockerfile`中，包括项目及其依赖库的编译、测试、打包等流程。
这可能会带来一些问题：

- `Dockerfile`特别长，可维护性降低
- 镜像层次多，镜像体积较大，部署时间变长
- 源代码存在泄漏的风险

<br>

**分散到多个Dockerfile**

事先在一个`Dockerfile`将项目及其依赖库编译测试打包好后，再将其拷贝到运行环境中。这种方式需要编写两个`Dockerfile`和一些编译脚本才能将两个阶段自动整合起来。这种方式虽然可以很好避免全部写入一个Dockerfile的风险，但明显部署过程较复杂。

<br>

**多阶段构建**

使用多阶段构建我们就可以很容易解决前面提到的问题，并且只需要编写一个`Dockerfile`。





<br>
<br/>


## Dockerfile最佳实践


- 一般性建议
	+ 容器应该是短暂的
	+ 使用`.dockerignore`文件
	+ 使用多阶段构建减少镜像大小
	+ 避免安装不必要的包
	+ 一个镜像只运行一个进程
	+ 镜像层数尽可能少
	+ 将多行参数排序
	+ 构建缓存
- Dockerfile指令
	+ `FROM`
	+ `LABEL`
	+ `RUN`
	+ `CMD`
	+ `EXPOSE`
	+ `ENV`
	+ `ADD`
	+ `COPY`
	+ `ENTRYPOINT`
	+ `VOLUME`
	+ `USER`
	+ `WORKDIR`





<br>
<br/>

---

<br>



# Compose file


参考:

- <https://docs.docker.com/compose/compose-file/>



<br>
<br/>

---

<br/>



# 使用Docker进行开发

Develop with Docker


<br/>
<br/>


## 在Docker上开发应用程序

Develop new apps on Docker

- Learn to build an image from a Dockerfile
- Use multistage builds to keep your images lean
- Manage application data using volumes and bind mounts
- Scale your app as a swarm service
- Define your app stack using a compose file
- General application development best practices

<br>

了解有关Docker上特定语言的开发：

- Java
- node.js
- Ruby on Rails
- .Net
- ASP.Net


<br>
<br/>


### Docker开发最佳实践

Docker development best practices

如下开发模式已被证明有助于人么使用Docker构建应用程序。


<br>


#### 如何保持较小的镜像

How to keep your images small

在启动容器或服务时，小图像可以更快速通过网络pull镜像并加载到内存中。有几条经验法则可保持较小的镜像：

- 从适当的基础镜像开始
例如，如果需要JDK，请考虑官方镜像，而不是从一个通用的Ubuntu/Centos镜像并将Openjdk作为Dockerfile的一部分安装开始。

- 使用多阶段构建
例如，你可以使用maven镜像构建java程序，然后重置到tomcat镜像，并将java构件复制到正确位置以部署应用程序，所有这些都位于相同的Dockerfile。这意味着你的最终镜像不包含构建时所引入的所有库和依赖项，仅包含运行它们所需的构件和环境。

- 如果你有多个共同的镜像，请考虑使用共享组件创建你的基本镜像，并在其上创建独特的镜像
Docker只要家在一次通用层，然后便会缓存。

- 保持生产环境镜像精简但允许调试(degub)，请考虑使用生产环境镜像作为调试镜像的基本镜像

- 在构建镜像时，应该始终使用有用的标签对其进行标记，如(test, prod)。不要依赖自动创建的`latest`标签


<br>


#### 何处以及如何持久化应用程序数据

Where and how to persist application data

- 避免使用存储驱动(storge drivers)将应用程序的数据存储在容器的可写层(writeable layer)中
与使用卷(volume)或绑定挂载(bound mounts)相比，这增加了容器的大小，并且从I/O角度来看效率较低

- 使用卷存储数据
- 适合使用绑定挂载的一种情况是在开发过程中，对于生产环境，请改用卷
- 对于生产环境，使用secerts来存储服务使用的敏感的应用程序数据，使用config来存储不敏感的数据(如配置文件)


<br>

#### 尽可能使用swarm服务


Use swarm services when possible

- 在可能的情况下，使用swarm服务进行伸缩的能力来设计你的应用程序
- 即使你只需运行单个实例，swarm服务也比standalone容器提供更多的优势
- 网络和卷可使用swarm服务连接和断开，并且docker可以以不中断的方式重新部署各个服务容器。standalone容器需要手动停止/移除/重新创建
- 一些功能仅适用于服务而不适用于standalone容器
- 让`docker stack deploy`处理任意镜像，而不是使用`docker pull`。通过这种方式，你的部署不会尝试从down的节点进行pull。此外，当新节点添加到集群时，镜像会自动pull


<br>


#### 使用CI/CD进行测试和部署

Use CI/CD for testing and deployment

- CI(Continuous integration)
- CD(continuous deployment)
- 当更新源码库或创建拉取请求时，请使用CI/CD pipeline 自动构建并标记Docker镜像，并对其进行测试。也可将测试过的应用程序直接部署到生产环境中


<br/>
<br/>


### Develop images


#### 编写Dockerfile的最佳实践

Best practices for writing Dockerfiles

Docker通过读取Dockerfile(一个包含命令的文本文件)中的命令来自动构建镜像。
Dockerfile reference: <https://docs.docker.com/engine/reference/builder/>

<br>

Dockerfile由read-only layer组成，每层代表一个Dockerfile指令。如:

```
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

每个命令创建一个层:

- `FROM`
从`ubuntu:15.04` Docker image创建一个层

- `COPY`
从Docker client的当前目录添加文件

- `RUN`
使用`make`构建你的应用程序

- `CMD`
指定在容器内运行的命令

当你运行镜像并生成容器时，会在基础层的顶部添加一个可写层(writable layer)，也称容器层(container layer)。对正在运行的容器所做的所有更改(增删改文件)都会写入此可写容器层。


<br/>


#### 一般准则和建议

General guidelines and recommendations

**创建临时(ephemeral)容器**

Create ephemeral containers
由Dockerfile定义的镜像应该生成尽可能临时的容器。临时的意思为容器可以被停止(stop)和销毁(destroy)，然后重建(rebuild)并使用绝对最小化的设置和配置来替代。

<br>

**理解构建上下文**

Understand build context
当你发出`docker build`命令时，当前的工作目录被称为构建上下文(build context)。默认情况下，假设Dockerfile位于此，但你也可以使用文件标志(-f)指定位置。无论Dockerfile位于何处，当前目录内的所有内容(除了.dockerignore中忽略的内容)都将作为构建上下文发送给Docker守护进程。

<br>

**从stdin读取Dockerfile**

Pipe Dockerfile through stdin

```
#local build-context
docker build -t . -f-<<EOF
FROM busybox
RUN echo "hello world"
COPY . /my-copied-files
EOF


#remote
docker build -t foo https://github.com/thajeztah/pgadmin4-docker.git -f-<<EOF
FROM busybox
COPY LICENSE config_local.py /usr/local/lib/python2.7/site-packages/pgadmin4/
EOF
```

<br>

**使用`.dockerignore`排除文件**

Exclude with .dockerignore
要排除与构建无关的文件，请使用`.dockerignore`文件，这与`.gitignore类似`。

```
vim ./dockerignore

file1
dir2
...
```

<br>

**使用多阶段构建**

Use multi-stage builds
多阶段构建允许你大幅缩减镜像大小，而不需要减少中间层和文件数。
由于镜像是在构建过程的最后阶段构建的，因此可以通过利用构建缓存(build cache)来最小化镜像层

例如，如果你的版本博涵包含多个层，你可以从 不经常改动的版本到频繁改动的版本进行排序:

- 安装构建应用程序需要的工具
- 安装或更新依赖库
- 生成应用程序

A Dockerfile for Go application:

```
FROM golang:1.9.2-alpine3.6 AS build

# Install tools required for project
# Run `docker build --no-cache .` to update dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# List project dependencies with Gopkg.toml and Gopkg.lock
# These layers are only re-built when Gopkg files are updated
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# Install library dependencies
RUN dep ensure -vendor-only

# Copy the entire project and build it
# This layer is rebuilt when a file changes in the project directory
COPY . /go/src/project/
RUN go build -o /bin/project

# This results in a single layer image
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```

<br>

**不要安装不必要的包**

Don’t install unnecessary packages
为了减少复杂性、依赖性，文件大小和构建时间，避免安装额外的或不不必要的软件包。

<br>

**分离应用程序**

Decouple applications
每个容器应该只有一个问题。将应用程序分离到多个容器中可以更轻松地水平伸缩和重新使用容器。
例如，Web应用程序堆栈可能有三个独立的容器组成，每个容器都有其独特的镜像，以分离的方式管理Web应用程序、数据库和内存缓存。

将每个容器限制为一个进程是一个很好的经验法则，但不是硬性规定。(想想高可用和负载均衡)。

尽你最大的努力使容器干净和模块化。如果容器相互依赖，则可以使用Docker container network来确保容器间可进行通信。

<br>

**最小化层数**

Minimize the number of layers
在老版本的docker中，重要的是减少镜像的层数，以确保它们的性能。

<br>

**对多行参数排序**

Sort multi-line arguments
只要有可能，通过按字母数字排序多行参数来简化修改。这有助于避免软件包重复，并使列表更容易更新。

```
RUN apt-get update && apt-get install -y \
  bzr \
  cvs \
  git \
  mercurial \
  subversion
```

<br>

**Leverage build cache**

在构建镜像时，Docker安装Dockerfile中的指令逐步执行，并按指定的顺序执行每个镜像。在检查每条指令时，docker会在其缓存中查找可重用的现有镜像，而不是创建新的(重复)镜像。

如果你不想使用缓存，可在`docker build`命令中使用`--no-cache=true`选项。如果让Docker使用了缓存，那么了解何时可以 找到/找不到 匹配的图像就很重要了。

Docker遵循的基本规则如下:

- 从已经在缓存中的父镜像开始，将下一条指令与该基本镜像派生的所有子镜像进行比较，以查看是否使用完全相同的指令构建了其中的一条。否则，缓存失效。
- 大多数情况下，只需将Dockerfile中的指令与其中一个子镜像进行比较久够了。但是，某些说明需要更多的检查和解释。
- 对于`ADD`和`COPY`指令，将检查镜像文件中的内容，并为每个文件计算校验和。在缓存查找过程中，将检验和与现有镜像中的校验和进行比较，如果文件中由任何内容已更改，如内容和元数据，则缓存将失效。
- 除了`ADD`和`COPY`指令，缓存检查将不会查看容器中的文件已确定缓存。

一旦缓存失效，所有后续的Dockerfile命令将生产新的镜像，并且不会使用缓存。

<br>

**Dockerfile instruction**

请参考: [Dockerfile](# Dockerfile)


<br/>


#### 创建一个基镜像

Create a base image

大多数Dockerfile从父镜像开始，如果需要完全控制镜像的内容，则可能需要创建基镜像(base image)。区别:

- 父镜像是镜像的所基于的镜像
- 基镜像的Dockerfile中没有`FROM`行


<br/>


#### 使用多阶段构建

Use multi-stage builds

多阶段构建需要Docker v17.05及以上版本。多阶段构建对于优化Dockerfile来说非常有用，同时让它易读和维护。

<br>

**构建之前**
构建镜像最具挑战的事情是保持镜像的大小。Dockerfile中的每条指令都会为镜像添加一层，在移动到下一层前清理不需要的任何构件。为了编写一个高效的Dockerfile，需要尽可能减小图层，并确保每个层都具有上一层需要的构件，而不是其它东西。

**使用多阶段构建**
使用多阶段构建，你可以在Dockerfile中使用多个`FROM`语句。每条`FROM`命令可以使用不同的基镜像，并且每个指令都可是构建的新阶段。

```
FROM golang:1.7.3
WORKDIR /go/src/github.com/alexellis/href-counter/
RUN go get -d -v golang.org/x/net/html
COPY app.go .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=0 /go/src/github.com/alexellis/href-counter/app .
CMD ["./app"]
```

`COPY --from=0`将前面构建的工作复制到这个新阶段。Go SDK和任何中间工作件都被留下，并未保存在最终镜像中

<br>

**命名你的构建阶段**
默认情况下，构建阶段没有命名。你可以通过它们的整数来引用它们，第一个指令`FROM`从0开始。但你可以命名它。

```
FROM golang:1.7.3 as builder
WORKDIR /go/src/github.com/alexellis/href-counter/
RUN go get -d -v golang.org/x/net/html
COPY app.go    .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /go/src/github.com/alexellis/href-counter/app .
CMD ["./app"]
```

<br>

**停止一个特定的构建阶段**
构建镜像时，不一定需要构建包含每个阶段的整个Dockerfile。
如下的栗子停在名为`builder`的阶段:

```
docker build --target builder -t alexellis2/href-counter:latest .
```

<br>

**使用外部镜像用作一个阶段**
多阶段构架可使用`COPY --from`指令从单独的镜像中进行复制，可以使用本机镜像、远程Registry的镜像和标记的ID。

```
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
```


<br>
<br/>


## 使用Docker Engine SDKs和API进行开发

Develop with Docker Engine SDKs and API


<br/>


## 综述

Docker提供了一个用于与Docker daemon(称为Docker Engine API)交互的API，以及用于Go和Python的SDK。

SDK允许你款速轻松地构建和扩展Docker APP。
如果Go或Python不适合你，你可以直接使用Docker Engine API——它是由HTTP客户端(curl, wget)访问的RESTful API，或者是大多数现代编程语言的一部分HTTP库。

<br>

### 安装SDKs

#### Go SDK

- Go SDK参考：<https://godoc.org/github.com/docker/docker/client>

```
go get github.com/docker/docker/client
```

<br>

#### Python SDK

- Python SDK参考: <https://docker-py.readthedocs.io/en/stable/>

```
pip install docker
```

<br>


### 快速开始SDK和API

Python: 运行一个容器

```
import docker
client = docker.from_env()
print (client.containers.run("alpine", ["echo", "hello", "world"]))
```

HTTP:

```
$ curl --unix-socket /var/run/docker.sock -H "Content-Type: application/json" \
  -d '{"Image": "alpine", "Cmd": ["echo", "hello world"]}' \
  -X POST http:/v1.24/containers/create
{"Id":"1c6594faf5","Warnings":null}

$ curl --unix-socket /var/run/docker.sock -X POST http:/v1.24/containers/1c6594faf5/start

$ curl --unix-socket /var/run/docker.sock -X POST http:/v1.24/containers/1c6594faf5/wait
{"StatusCode":0}

$ curl --unix-socket /var/run/docker.sock "http:/v1.24/containers/1c6594faf5/logs?stdout=1"
hello world
```


<br/>
<br/>


### SDK和API栗子

链接: <https://docs.docker.com/develop/sdk/examples/>




<br/>

---

<br/>



# 网络配置

Configure networking


## 综述


Docker容器和服务如此强大的原因之一是——你可以将它们连接在一起，或将它们连接到non-docker工作负载。Docker容器和服务甚至不需要知道它们是否部署在Docker上，或它们的对等端是否也是Docker工作负载。都可以使用Docker方式管理它们。


<br>


### 网络驱动

Network drivers

使用驱动程序，Docker的网络子系统是可插拔的(pluggable)。

集中驱动程序:

- brige
默认网络驱动。桥接网络通常用于你的应用程序运行在需要通信的独立容器中。

- host
对于独立容器，删除容器和Docker主机之间的网络隔离，并直接使用主机的网络。

- overlay
overlay网络将多个docker daemon连接在一起，并使集群服务能够无相互通信。

- macvlan
macvlan网络允许你为容器分配MAC地址，使其成为你网络上的物理设备。docker daemon通过其MAC地址将流量路由到容器。

- none
对于此容器，禁用所有网络。

- network plugins
你可在Docker上安装和使用第三方网络插件，从Docker Store获取: <https://store.docker.com>

<br/>

**网络驱动总结**

- **User-defined bridge networks**
当你需要多个容器在同一个Docker主机上进行通信时

- **Host networks**
当网络堆栈不应与Docker主机隔离时，但希望容器的其它方面被隔离

- **Overlay networks**
当你需要运行在不同Docker主机上的容器进行通信时，或多个应用程序使用集群服务进行工作时

- **Macvlan networks**
当你从虚拟机迁移或需要你的容器看起来像物理主机时，每个都具有唯一的MAC地址

- **Third-party network plugins**
允许你将Docker与专用网络堆栈集成


<br/>
<br/>


## bridge


就网络而言，桥接网络是一种链路层设备，用于转发网段之间的流量。桥接可以是硬件设备，或在主机内核中运行的软件设备。
就Docker而言，桥接网络允许连接到统一桥接网络的容器进行通信，同时提供与未连接到桥接网络的容器的隔离。Docker桥接驱动程序自动在主机上安装桥接规则，以便于不同桥接网络上的容器不能直接相互通信。

桥接网络适用于在同一个Docker daemon上运行的容器之间的通信。

当你启动Docker时，除非另有定义，否则将自动创建默认桥接网络，并且新启动的容器将连接到它。
你也可以创建用户自定义的桥接网络。


<br>
<br/>


### bridge与user-defined bridges

Differences between user-defined bridges and the default bridge

两者的差别：

- 用户自定义的桥接在集装箱化的应用程序之间提供了更好的隔离和互操作性

- 用户自定义的桥接提供了容器之间的自动DNS解析

- 容器可以在运行中与用户定义的网络进行连接(attach)和分离(detach)

- 每个用户定义的网络会创建一个可配置的桥接网络

- 在默认桥接网络上链接的容器共享环境变量


<br/>
<br/>


### 管理user-defined bridge

Manage a user-defined bridge

```
docker network create --help

#创建一个用户自定义桥接网络
#你还可以指定子网，范围，网关...
docker network creat ${name}

docker network creat my-net


#删除
docker network rm ${name}
```


<br/>
<br/>


### 连接到自定义桥接网络

Connect a container to a user-defined bridge

当你创建一个新的容器时，你可以指定一个或多个`--network`标志。

```
#创建时
docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest


#运行中的容器
docker network connect my-net my-nginx


#断开连接
docker network disconnect my-net my-nginx
```


<br/>
<br/>


### 使用IPv6


需要修改docker daemon的配置项以支持使用IPv6，在创建自定义网络是指定`--ipv6`标志。
你不能有选择地禁用默认桥接网络上的IPv6支持。


<br/>
<br/>


### 启用容器转发

Enable forwarding from Docker containers to the outside world

默认情况下，使用默认桥接网络的连接的容器的流量不会转发到外部世界。启用操作如下：

```
#配置Linux内核
sysctl net.ipv4.conf.all.forwarding=1

#修改iptables FORWARD默认策略
iptables -P FORWARD ACCEPT

#重启后无效，请写入配置文件
```


<br/>
<br/>


### 默认桥接网络

Use the default bridge network

默认桥接网络被视为Docker的遗留细节，不建议用于生产环境。

<br>

**连接容器到默认桥接网络**
如果未指定网络，则默认使用默认桥接网络。

**配置默认桥接网络**
指定并配置`daemon.json`文件

```
{
  "bip": "192.168.1.5/24",
  "fixed-cidr": "192.168.1.5/25",
  "fixed-cidr-v6": "2001:db8::/64",
  "mtu": 1500,
  "default-gateway": "10.20.1.1",
  "default-gateway-v6": "2001:db8:abcd::89",
  "dns": ["10.20.1.2","10.20.1.3"]
}
```

**使用IPv6**
修改配置文件以支持IPv6，则默认桥接网络自动支持IPv6。



<br/>
<br/>


## overlay


overlay网络驱动在多个docker daemon主机之间创建分布式网络。该网络位于特定主机网络之上，允许容器连接到此并安全地进行通信。

当初始化集群或将docker主机加入现有集群时，将在docker主机上创建两个新网络：

- 称为`ingress`的overlay网络
处理与集群服务相关的控制和数据流量。
当你创建集群服务并且不将其连接到用户自定义的网络时，它默认连接到`ingress`网络。

- 称为`docker_gwbridge`的桥接网络
将单独的docker daemon连接到集群的其它docker daemon。

与创建自定义桥接网络类似，你也可以使用`docker network create`来创建自动以的overlay网络。服务或容器一次可连接到多个网络，但只能通过连接的网络进行通信。

尽管可以将集群服务和独立容器连接到overlay网络，但默认行为和配置是不同的。


<br/>
<br/>


### 所有overlay网络的操作

Operations for all overlay networks


<br/>


#### 创建overlay网络

Create an overlay network

**先决条件**

- 使用overlay网络的docker daemon的防火墙规则
	+ 2377(tcp): 集群通信管理
	+ 7946(tcp/udp)： 节点通信
	+ 4789(udp)： overlay网络流量

- 创建overlay网络前，需要初始化docker daemon集群


```
docker network create -d overlay my-overlay


#创建可供集群服务或独立容器与其它docker daemon上的独立容器进行通信
docker network create -d overlay --attachable my-attachable-overlay


#你可以指定IP地址范围，子网，网关...
```


<br/>


#### 加密overlay网络上的流量

Encrypt traffic on an overlay network

**Overlay network encryption is not supported on Windows！**

所有集群服务管理流量默认都是加密的，在GCM模式下使用AES算法。
要加密应用程序数据，在创建overlay网络时添加`--opt encrypted`。这种加密带来了不可忽视的性能问题，所以应该在生产环境使用前对其进行测试。
当启用overlay加密时，docker会在节点间创建IPsec tunnel，在这些节点上调度连接到overlay网络的服务的任务。

```
#SWARM MODE OVERLAY NETWORKS AND STANDALONE CONTAINERS
 docker network create --opt encrypted --driver overlay --attachable my-attachable-multi-host-network
```


<br/>


#### 自定义默认ingress网络

如果自动选择的子网与已存在的网络冲突，或需要自定义其它低级网络设置(如MTU)，这次功能非常有用。

```
#显示详细信息
docker network inspect ingress

#移除现有网络
docker network rm ingress

#创建新网络 --ingress
docker network create \
  --driver overlay \
  --ingress \
  --subnet=10.11.0.0/16 \
  --gateway=10.11.0.2 \
  --opt com.docker.network.driver.mtu=1200 \
  my-ingress
```


<br/>


#### 自定义docker_gwbridge

`docker_gwbridge`是一个虚拟桥接网络，它将overlay网路连接到单独的docker daemon的物理网络。当初始化集群或将主机加入集群时，docker会自动创建它，但它不是docker设备。啊存在于docker主机的内核之中。如果你需要自定义其设置，则必须在主机加入集群之前或将主机临时从集群中删除之后才执行此操作。

```
1. 停止docker

2. 删除已存在的docker_gwbridge
ip link set docker_gwbridge doen
ip link del dev docker_gwbridge

3. 启动docker，但不加入或初始化集群

4. 创建docker_gwbridge
docker network create \
--subnet 10.11.0.0/16 \
--opt com.docker.network.bridge.name=docker_gwbridge \
--opt com.docker.network.bridge.enable_icc=false \
--opt com.docker.network.bridge.enable_ip_masquerade=true \
docker_gwbridge

5. 集群初始化或加入集群
```


<br/>
<br/>


### swarm服务的操作

Operations for swarm services


#### 在overlay网络上发布端口

Publish ports on an overlay network

连接到同一overlay网络的集群服务可有效地将所有端口暴露给对方。要是端口可在服务外可访问，必须使用`-p`或`--publish`标志暴露此端口。

两种方法：

- 传统的冒号`:`分隔语法
- 较新的逗号`,`分隔语法

| Flag value | Description
| - | -
| -p 8080:80 or <br> -p published=8080,target=80 | Map TCP port 80 on the service to port 8080 on the routing mesh
| -p 8080:80/udp or <br> -p published=8080,target=80,protocol=udp | Map UDP port 80 on the service to port 8080 on the routing mesh
| -p 8080:80/tcp -p 8080:80/udp or <br> -p published=8080,target=80,protocol=tcp -p published=8080,target=80,protocol=udp | Map TCP port 80 on the service to TCP port 8080 on the routing mesh, and map UDP port 80 on the service to UDP port 8080 on the routine mesh


<br/>


#### 绕过swarm的路由网格

Bypass the routing mesh for a swarm service

默认情况下，发布端口的集群服务使用路由网格来发布。当你连接到任何swarm节点上已发布的端口时，都会透明地将你重定向到正在运行服务的工作。实际上，docker充当集群服务的负载均衡器(Load-Balancer)。使用路由网格的服务以虚拟IP(vip)模式运行。即使在每个节点上运行服务也使用路由网格。使用路由网格时，不能保证那个docker node处理客户端请求。

要绕过路由网格，可使用DNS Round Robin(DNSRR)模式启动——`--endpoint-mode dnsrr`。你必须在服务前运行负载均衡器。docker主机上DNS查询服务名称会返回运行该服务的节点的IP地址列表。配置你的负载均衡器使用此列表并平衡各节点间的流量。


<br/>


#### 分离控制流量和数据流量

默认情况下，尽管集群控制流量是加密的，但集群管理和应用程序之间的控制流量运行在同一个网络上。你可以配置docker来使用单独的网络接口来处理来种不同类型的流量。


<br/>
<br/>


### overlay网络上独立容器的操作

Operations for standalone containers on overlay networks


#### 将独立容器连接到overlay网络

Attach a standalone container to an overlay network

独立容器连接到`ingress`网络需添加`--attachable`标志。这使得运行在不同docker daemon上的独立容器能够进行通信，而无需在各个docker daemon主机上设置路由。


<br/>


#### 发布端口

Publish ports

| Flag value | Desciption
| - | -
| -p 8080:80 | Map TCP port 80 in the container to port 8080 on the overlay network
| -p 8080:80/udp | Map UDP port 80 in the container to port 8080 on the overlay network
| -p 8080:80/sctp | Map SCTP port 80 in the container to port 8080 on the overlay network
| -p 8080:80/tcp -p 8080:80/udp	| Map TCP port 80 in the container to TCP port 8080 on the overlay network, and map UDP port 80 in the container to UDP port 8080 on the overlay network


<br/>


#### 容器发现

Container discovery

对于大多数情况，应该连接到服务名称——它是负载均衡的，并支持服务的所有容器处理。要获取支持该服务的所有任务的列表，请执行DNS查找服务——`tasks.<service-name>`。



<br/>
<br/>


## host


如果你对容器使用host网络驱动，则该容器的网络堆栈将不与docker主机隔离。例如，如果运行一个绑定在80端口并使用host网络的容器，则该容器的应用程序将在主机IP地址的80端口上可用。

host网络驱动只能运行在Linux主机上。


<br/>
<br/>


## Macvlan


一些应用程序，尤其是需要监视网络流量的应用程序，希望连接到物理网络上。在这种情况下，你可以使用`macvlan`驱动为容器的虚拟网络接口分配MAC地址，使其看起来像是直接连接到物理网络的物理网络接口。在这种情况下，你需要指定Docker主机上的物理接口用于macvlan，以及macvlan的子网和网关。


<br/>


### 创建一个macvaln网络

macvlan网络可处于 **bridge mode** 或 **802.1q trunk mode**:

- 在桥接模式下，macvlan流量通过主机上的物理设备

- 在802.1q主干桥接模式下，流量通过Docker在运行中创建的802.1q子接口。
这使你可以更细粒度地控制路由和过滤。

<br>

**bridge mode**

创建bridge macvlan:

```
docker networkcreate --driver macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1  \
  -o parent=eth0 pub_net


#--aux-addresses排除IP地址
docker networkcreate --driver macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1  \
  --aux-address="my-router=192.168.32.129" \
  -o parent=eth0 pub_net
```

<br>

**802.1q truk bridge mode**

如果你指定了包含点`.`的接口名——如**eth0.50**，则Docker将其解释为eth0的子接口，并自动创建子接口。

```
docker network  create  --driver macvlan \
  --subnet=192.168.50.0/24 \
  --gateway=192.168.50.1 \
  -o parent=eth0.50 macvlan50
```

<br>

**使用ipvlan替换macvlan**

```
docker network create -d ipvlan \
  --subnet=192.168.210.0/24 \
  --subnet=192.168.212.0/24 \
  --gateway=192.168.210.254  \
  --gateway=192.168.212.254  \
  -o ipvlan_mode=l2 ipvlan210
```

<br>

**IPv6**

```
docker network  create  -d macvlan \
  --subnet=192.168.216.0/24 --subnet=192.168.218.0/24 \
  --gateway=192.168.216.1  --gateway=192.168.218.1 \
  --subnet=2001:db8:abc8::/64 --gateway=2001:db8:abc8::10 \
  -o parent=eth0.218 \
  -o macvlan_mode=bridge macvlan216
```


<br/>
<br/>


## 禁用容器网络


在启动容器时加上`--network none来禁用容器的网络堆栈，这样在容器内便仅仅创建loopback设备。

```
$ docker run --rm -dit \
  --network none \
  --name no-net-alpine \
  alpine:latest \
  ash
```


<br/>
<br/>


## 网络教程

Networking tutorials


<br/>


### bridge network

- default bridge network
- user-defined bridge network


<br>


#### default bridge network

1. 基本docker网络

```
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
8d3b84bfe5a0        bridge              bridge              local
3579d63da633        host                host                local
f766b990db47        none                null                local
```

以上列出了默认的桥接网络，主机网络(启动直接连接到docker daemon的主机的网络堆栈的容器)，none(启动一个没有网络设备的容器)。

<br>

2. 启动一个容器

```
docker run -dit --name alpine1 alpine ash

```

由于启动时没有指定网络，所以默认为桥接网络。

<br>

3. Inspect the bridge network，以查看哪个容器连接到它

```
docker network inspect bridge


[
    {
        "Name": "bridge",
        "Id": "8d3b84bfe5a034c65d043af80976a1e6127011fc1ab312446252f562e221d351",
        "Created": "2018-05-24T18:38:35.538308064+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "893903129d869f384bd78d63a382f1c9527d6979be0a6cf3c13ea4f45a1554d6": {
                "Name": "hardcore_rosalind",
                "EndpointID": "515d1435470c9f72d3b07680515d9c503457b8eb5bcaaaa915bb53901eac9424",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

<br>

4. 连接到容器

```
docker attach alpine1


/ # ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
506: eth0@if507: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:11:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.3/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever


/ # ping -c 2 www.baidu.com
PING www.baidu.com (119.75.216.20): 56 data bytes
64 bytes from 119.75.216.20: seq=0 ttl=55 time=46.521 ms
64 bytes from 119.75.216.20: seq=1 ttl=55 time=45.189 ms
```

<br>

5. ping其它容器

```
/ # ping -c 2 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.125 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.075 ms
```


<br/>


#### user-defined bridge networks

1. 创建名为`apline-net`用户自定义网络
当然，你可以手动指定子网，网关这些。

```
docker network create --driver bridge alpine-net


docket network ls
NETWORK ID          NAME                DRIVER              SCOPE
810fb1e02000        alpine-net          bridge              local
8d3b84bfe5a0        bridge              bridge              local
3579d63da633        host                host                local
f766b990db47        none                null                local

```

<br>

2. 查看alpine-net网络详情
注意网关和子网发生了变化。

```
docker network inspect alpine-net

[
    {
        "Name": "alpine-net",
        "Id": "810fb1e020008c7c6598f3b830ca25896dde638b1190d383ee6a5214d284e77d",
        "Created": "2018-06-14T15:45:19.43941906+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

<br>

3. 创建两种网络的容器

```
#alpine-net
docker run -dit --name alpine1 --network alpine-net alpine ash

#default bridge
docker run -dit --name alpine2 alpine ash

```

<br>

4. 显示两种网络情况

```
docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "8d3b84bfe5a034c65d043af80976a1e6127011fc1ab312446252f562e221d351",
        "Created": "2018-05-24T18:38:35.538308064+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "893903129d869f384bd78d63a382f1c9527d6979be0a6cf3c13ea4f45a1554d6": {
                "Name": "hardcore_rosalind",
                "EndpointID": "515d1435470c9f72d3b07680515d9c503457b8eb5bcaaaa915bb53901eac9424",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "e7472c3ddda5043bc03868f4bf7ed59562220f05772f02f57ff589d086630562": {
                "Name": "alpine2",
                "EndpointID": "ba565a247e347feb59713c188eb38e184d781da0489ae80e26ecad6d24e165c2",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]




docker network inspect alpine-net
[
    {
        "Name": "alpine-net",
        "Id": "810fb1e020008c7c6598f3b830ca25896dde638b1190d383ee6a5214d284e77d",
        "Created": "2018-06-14T15:45:19.43941906+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "264ccde8b1d5198551d689f0dd49ffbfb612255e0bf76c9543325d7c2e588acb": {
                "Name": "alpine1",
                "EndpointID": "563c48cc6b936bcd9d3f57e9bb5e162a8cb52a23c8980346f288d42cc9b0a8fc",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

```

<br>

5. 连接到容器

```
docker container attach alpine1

#网段内通
/ # ping -c 2 172.18.0.3
PING 172.18.0.1 (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.097 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.070 ms

--- 172.18.0.1 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.070/0.083/0.097 ms


#网段外不通
/ # ping -c 2 172.17.0.2
PING 172.17.0.2 (172.16.0.1): 56 data bytes

--- 172.17.0.2 ping statistics ---
2 packets transmitted, 0 packets received, 100% packet loss

```

<br>

6. 使容器连接到default bridge
这样，此容器便连接到了两个网络中。

```
docker network connect bridge apline1


/ # ping -c 2 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.102 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.071 ms
```


<br/>
<br/>


### host network

host网络不存在隔离问题。

```
#默认主机上的80端口
docker run -rm -dit --network host --name my_nginx nginx


#访问
http://localhost:80
Welcome to nginx!


docker network inspect host
[
    {
        "Name": "host",
        "Id": "3579d63da633adcc497417d39b8b1d270cf329a68b9222f6a75fae72086509d6",
        "Created": "2018-04-27T11:31:17.900886126+08:00",
        "Scope": "local",
        "Driver": "host",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": []
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "f02a3b11fce7228ad6ee196771bd9cf0b64966bfc2aa7c27719bc120dbdc7189": {
                "Name": "my_nginx",
                "EndpointID": "4ee67fb4d0a0c1a357b5fdd141f856a70c205fad5c49b1cb6a4f5245df0318a8",
                "MacAddress": "",
                "IPv4Address": "",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

```


<br/>
<br/>


### overlay network

- default overlay network
- user-defined overlay network
- overlay network for standalone containers
- Communicate between a container and a swarm service


<br>


#### default overlay

**依赖：**

- swarm集群
- 集群节点
	- worker-1
	- worker-2
	- mananger

```
docker network ls


NETWORK ID          NAME                DRIVER              SCOPE
495c570066be        bridge              bridge              local
961c6cae9945        docker_gwbridge     bridge              local
ff35ceda3643        host                host                local
trtnl4tqnc3n        ingress             overlay             swarm
c8357deec9cb        none                null                local
```

<br>

创建nginx-net的overlay的网络:

```
docker network create -d overlay nginx-net


$ docker service create \
  --name my-nginx \
  --publish target=80,published=80 \
  --replicas=5 \
  --network nginx-net \
  nginx
```


<br/>


#### user-defined overlay

```
docker network create -d overlay my-overlay


$ docker service create \
  --name my-nginx \
  --network my-overlay \
  --replicas 1 \
  --publish published=8080,target=80 \
  nginx:latest
```


<br/>


#### overlay network for standalone containers



<br/>


#### Communicate between a container and a swarm service



<br/>
<br/>


### macvalan network

假设主机网络接口为`eth0`。


<br>


#### bridge

此模式下，流量通过eth0流动，docker使用其MAC地址就流量路由到容器。

1. 创建名为`my-macvlan-net`的macvlan网络

```
$ docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1 \
  -o parent=eth0 \
  my-macvlan-net
```

<br>

2. 查看网络

```
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
810fb1e02000        alpine-net          bridge              local
8d3b84bfe5a0        bridge              bridge              local
3579d63da633        host                host                local
6be80655739d        my-macvlan-net      macvlan             local
f766b990db47        none                null                local
```

<br>

3. 以此网络运行容器

```
$ docker run --rm -itd \
  --network my-macvlan-net \
  --name my-macvlan-alpine \
  alpine:latest \
  ash
```

<br>

4. 查看my-macvlan-net

```
docker network inspect my-macvlan-net
[
    {
        "Name": "my-macvlan-net",
        "Id": "6be80655739deffe204e087d098f97fc75072d95f9818e129cfd7d5667ed01f3",
        "Created": "2018-06-14T16:52:30.507647877+08:00",
        "Scope": "local",
        "Driver": "macvlan",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.16.86.0/24",
                    "Gateway": "172.16.86.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "8301b669b4b63afb20911b46243f11b70e5a9d0880beaafa922b52bcb8ab0477": {
                "Name": "my-macvlan-alpine",
                "EndpointID": "4f2971ba4bd92c34e2a299d301f739867d2b1b65d35566aef07d7a26b079662c",
                "MacAddress": "02:42:ac:10:56:02",
                "IPv4Address": "172.16.86.2/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "parent": "ens160"
        },
        "Labels": {}
    }
]
```

<br>

5. 查看容器网卡和路由

```
docker exec my-macvlan-alpine ip addr show eth0
517: eth0@if2: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UNKNOWN
    link/ether 02:42:ac:10:56:02 brd ff:ff:ff:ff:ff:ff
    inet 172.16.86.2/24 brd 172.16.86.255 scope global eth0
       valid_lft forever preferred_lft forever

docker exec my-macvlan-alpine ip route
default via 172.16.86.1 dev eth0
172.16.86.0/24 dev eth0 scope link  src 172.16.86.2

```

<br>

#### 802.1q trunked bridge network

此模式下，流量流经eth0的子接口(eth0.10)，docker使用其MAC地址将流量路由到容器。

1. 创建名为`my-8021q-macvlan-net`的macvlan网络

```
docker network create -d macvlan \
  --subnet=172.16.87.0/24 \
  --gateway=172.16.87.1 \
  -o parent=eth0.10 \
  my-8021q-macvlan-net
```

<br>

2. 查看此网络

```
docker network ls

NETWORK ID          NAME                   DRIVER              SCOPE
2aeafd44fd67        my-8021q-macvlan-net   macvlan             local
6be80655739d        my-macvlan-net         macvlan             local



ifconfig
eth0.10: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::20c:29ff:feaa:7e75  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:aa:7e:75  txqueuelen 0  (Ethernet)
```

<br>

3. 用此网络启动一个容器

```
docker run --rm -itd \
  --network my-8021q-macvlan-net \
  --name my-second-macvlan-alpine \
  alpine:latest \
  ash
```

<br>

4. 查看my-8021q-macvlan-net

```
docker network inspect my-8021q-macvlan-net
[
    {
        "Name": "my-8021q-macvlan-net",
        "Id": "2aeafd44fd67e6ee937c82788745b1d45fb291efd61f545537528eafdff94e3d",
        "Created": "2018-06-14T17:06:33.426800076+08:00",
        "Scope": "local",
        "Driver": "macvlan",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.16.87.0/24",
                    "Gateway": "172.16.87.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "90103673d94915c3c7fb572eec8bd97b2aee1c3dab877c598d0a62e6d797b06d": {
                "Name": "my-second-macvlan-alpine",
                "EndpointID": "5c93f2ea1d29150ee57f099d42fc8e04a571efd0d1273a4f6bed755dc34f2e54",
                "MacAddress": "02:42:ac:10:57:02",
                "IPv4Address": "172.16.87.2/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "parent": "ens160.10"
        },
        "Labels": {}
    }
]
```

<br>

5. 查看容器网络接口

```
docker exec my-second-macvlan-alpine ip addr show eth0
519: eth0@if518: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UNKNOWN 
    link/ether 02:42:ac:10:57:02 brd ff:ff:ff:ff:ff:ff
    inet 172.16.87.2/24 brd 172.16.87.255 scope global eth0
       valid_lft forever preferred_lft forever


docker exec my-second-macvlan-alpine ip route
default via 172.16.87.1 dev eth0
172.16.87.0/24 dev eth0 scope link  src 172.16.87.2
```


<br/>
<br/>


## 配置守护进程和容器


### 启用IPv6

启用IPv6前，请确保支持IPv6.

给docker daemon启用IPv6:

```
/etc/docker/daemon.json

{
  "ipv6": true
}

```


<br/>


### iptables

所有Docker的`iptables`规则都被添加到`DOKCER chain`。不要手动操作此表。
如果你需要添加Docker规则，请将其添加到`DOCKER-USER chain`

栗子：

```sh
iptables -I DOCKER-USER -m iprange -i ext_if ! --src-range 192.168.1.1-192.168.1.3 -j DROP

```


<br/>


### 容器网络

容器使用的网络类型(无论是bridge，overlay，macvlan还是自定义网络)，在容器内都是透明的。从容器的角度来看，它有一个带有IP地址，网关，路由表，DNS服务和其它网络细节的网络接口。


**publish port**
默认情况下，创建容器时，它不会将任何端口发布的外部世界。要是端口可用于docker之外的服务，请使用`--publish`或`-p`标志。

```
-p 8080:80
-p 192.168.1.100:8080:80
-p 8080:80/udp
-p 8080:80/tcp -p 8080:80/udp
```

<br>

**ip add and hostname**
默认情况下，容器会为其连接的每个docker网络分配一个IP地址。IP地址是从分配给网络的地址池中分配的，因此docker daemon有效地充当了每个容器的DHCP服务器。每个网络也有一个默认的子网掩码和网关。
同样，一个容器的主机名也有docker daemon指定。

```
#指定运行网络
docker run xxx --network


#运行的容器连接到其它网络
docker network connect


#--ip，指定IP地址
docker network connect my-bridge --ip 172.18.0.111


#--hostname，指定主机名
docker run xxx --network xxx --hostname container-01

docker network connect my-bridge --hostname container-02
```

<br>

**DNS**
默认情况下，容器会继承docker daemon的DNS设置，包括`/etc/hosts`和`/etc/resolv.conf`。你也可以基于每个容器覆盖这些默认设置。

```
#DNS server
--dns

#DNS搜索域
--dns-search

#表示DNS选项值的键值对
--dns-opt

--hostname
```


<br/>


### Docker使用代理服务器

在启动docker容器的用户主目录下创建此文件： `~/.docker/config.json`

```
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://127.0.0.1:3001",
     "noProxy": "*.test.example.com,.example2.com"
   }
 }
}
```



<br/>
<br/>

---

<br/>



# 应用程序数据

Manage application data


<br>


## 存储综述

Manage data in Docker

默认情况下，容器内创建的所有文件都被存储容器的可写层上：

- 当容器不在运行时，数据不是持续存在的。容器外的进程很难从容器中获取数据

- 容器的可写层与主机紧密耦合，你很难将数据移动到其他地方

- 向容器的可写入层写入数据，需要存储驱动(storage driver)管理文件系统才
存储驱动使用Linux kernel来提供一个union filesystem。与直接写入主机文件系统的数据卷相比，这种额外的抽象会降低性能。

<br>

Docker容器有两种选项将文件存储到主机上，这样即使容器停止之后这些文件也会被保留:

- volumes
- bind mounts
- tmpfs mount(Docker on Linux)


<br>


### 选择正确的挂载方式

Choose the right type of mount

无论你选用哪种挂载方式，数据在容器内看起来都是相同的。它被公开为容器文件系统中的目录或单个文件。

一个简单的方法——考虑数据在docker主机上的位置，可以看出**volumes, bind mounts, temfs**之间的差异：

- Volumes
volumes存储在由docker管理的主机文件系统的一部分中(如Linux上: `/var/lib/docker/volumes/`)。
non-docker进程不应该修改这部分文件系统。Volume是Docker中保存数据的最佳方式。

- Bind mounts
bind mounts可存储在主机系统上的任何地方。它们可能是最要的系统文件或目录。
docker主机或docker容器上的non-docker进程可以随时修改它们。

- tmpfs
仅存储在主机系统的内存中，不会写入主机系统的文件系统。

![](/images/Docker/types-of-mounts.png)


<br>


#### volumes的好栗子

Good use cases for volumes

Volemes是在docker容器和服务中持久化数据的首选方式:

- 在多个运行容器之间共享数据。如果你没有明确创建它，会在第一次挂载到容器时创建volume。当容器停止或删除时，volume仍然存在。多个容器可以挂载相同的volume，无论是read-write还是read-only。只有在你手动删除volume时它才会被删除。

- 当docker主机不能保证具有给定的目录或文件结构时，volume帮助你将docker主机的配置与运行时的容器进行分离。

- 当你想要将容器的数据存储在远程主机而不是本地的时候。

- 当你需要备份、还原或将数据从一台docker主机迁移到另一台时，volume时更好的选择。


<br>


#### bind mounts的好栗子

一般来说，你应该尽量使用volumes。bind mounts适合以下案例：

- 从主机共享配置文件到容器
这就是默认情况下，通过将主机的/`etc/resolv.conf`挂载到每个容器中，Docker为每个容器提供DNS解析。

- 在docker主机/容器的开发环境上共享源码或构建工件

- 当docker主机的文件或目录结构保证与容器所需的bind mounts一致时


<br>


#### tmpfs mounts的好栗子

当你不希望数据在主机上或容器内持久存储时，tmpfs mounts最合适。
这可能处于安全原因，或在应用于程序需要编写大量非持久性状态数据时保护容器的性能。


<br>


#### 使用bind或volumes的提示

如果你要使用bind mounts 或 volumes，牢记以下事项：

- 如果你挂载一个空卷(empty volume)到存在文件或目录的容器中的目录上，则会将这些文件或目录赋值到卷中。同样，如果你启动容器并制定了一个尚不存在的卷，则会为你创建一个空卷。

- 如果你挂载一个bind mount或non-empty volume到存在文件或目录的容器中的目录上，则这些文件或目录会被挂载所遮蔽。就像在Linux上挂载卷一样。



<br/>
<br/>


## Volumes

volumes是持久化Docker数据的首选机制，卷由docker完全管理。另外，由于卷不会增加使用它的容器的大小，并且该卷的内容存在于给定容器的周期之外，因此卷通产是比将容器的可写入层中的数据持久化更好的选择。


```
-v/--volume


#此选项更详细和简单
#如果你需要指定volume driver，请使用此flag
--mount

docker service create \
  --mount 'type=volume,src=<VOLUME-NAME>,dst=<CONTAINER-PATH>,volume-driver=local,volume-opt=type=nfs,volume-opt=device=<nfs-server>:<nfs-path>,"volume-opt=o=addr=<nfs-address>,vers=4,soft,timeo=180,bg,tcp,rw"'
  --name myservice \
  <IMAGE>
```

- `--volume`

由三个由冒号`:`分割的字段组成。这些字段必须按照正确的顺序排列，每个字段的含义并不明显。
第一个字段是卷的名称，并且在给定主机上是唯一的。对于匿名卷，第一个字段被省略。
第二个字段是文件或目录在容器中的挂载路径。
第三个字段是可选的，是由一个逗号`,分隔的选项列表。

- `--mount`

由多个键值对组成，以逗号`,`分隔。`--mount`的语法比`--volume`更冗长，但键的顺序并不重要，并且标志的值更易于理解。
挂载的类型(type)有`bind, volume, tmpfs`。
挂载的来源(source, src)为卷的名称，对于匿名卷该字段可被省略。
目的地(destination, dst, target)的值是安装在容器中的文件或目录的路径。
只读(readonly)选项将导致bind mount以只读方式挂载到容器中。
`volume-opt`选项可以多次指定，它是由选项名称和值组成的键值对组成。


<br/>


### 创建和管理卷

```
docker volume create my-vol


docker volume ls
DRIVER              VOLUME NAME
local               my-vol


docker volume inspect my-vol
[
    {
        "CreatedAt": "2018-06-15T17:19:02+08:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/opt/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
]


docker volume rm my-vol
```


<br/>


### 启动用卷的容器

Start a container with a volume

包括两种卷：

- 已存在的卷

- 未存在的卷
会自动创建


```
#--mount
docker run -d \
  --name devtest \
  --mount source=myvol2,target=/app \
  nginx:latest


#--volume
docker run -d \
  --name devtest \
  --volume myvol2:/app \
  nginx:latest
```

```
docker volume ls
DRIVER              VOLUME NAME
local               my-vol
local               myvol2


docker inspect devtest
#找到挂载
        "Mounts": [
            {
                "Type": "volume",
                "Name": "myvol2",
                "Source": "/opt/docker/volumes/myvol2/_data",
                "Destination": "/app",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ]
```


<br/>


### 启动用卷的服务

Start a service with volumes

docker服务不支持使用`--volume`标志，请使用`--mount`标志。

```
docker service create -d \
  --replicas=4 \
  --name devtest-service \
  --mount source=myvol2,target=/app \
  nginx:latest
```


<br/>


### 在机器间共享数据

Share data among machines

在构建容错应用程序时，可能需要配置同一服务的多个副本能访问相同的文件，而这些副本可能分布于不同的节点上。

![](/images/Docker/share_data_amon_machine.png)


卷驱动程序(volume driver)允许你从应用程序逻辑中抽象出底层存储系统。


<br/>


### 使用卷驱动

Use a volume driver

在创建卷或启动带卷的容器时，你可以指定卷驱动。如`vieux/sshfs`卷驱动程序。


- 初始化

```
docker plugin install --grant-all-permissions vieux/sshfs
```

- 使用卷驱动创建卷

```
#操作node2
docker volume create --driver vieux/sshfs \
  -o sshcmd=test@node2:/home/test \
  -o password=testpassword \
  sshvolume
```

- 启动一个带用卷驱动程序创建的卷的容器

```
docker run -d \
  --name sshfs-container \
  --volume-driver vieux/sshfs \
  --mount src=sshvolume,target=/app,volume-opt=sshcmd=test@node2:/home/test,volume-opt=password=testpassword \
  nginx:latest
```

- 备份，还原或迁移数据卷

使用`--volumes-from`标志创建一个挂载该卷的新容器。

```
#备份
docker run --rm --volumes-from dbstore -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata


#从备份还原
docker run -v /dbdata --name dbstore2 ubuntu /bin/bash
docker run --rm --volumes-from dbstore2 -v $(pwd):/backup ubuntu bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1"

```



<br/>
<br/>


## bind mounts

与volumes相比，bind mounts功能有限。当你使用bind mounts时，主机上的文件或目录(绝对路径或相对路径)被挂载到容器内。相比之下，当你使用volumes时，会在主机上的Docker存储目录中创建一个新目录，并且Docker会管理该目录的内容。
该文件或目录不需要已经存在于Docker主机上。如果它尚未存在，它会根据需求创建。bind mounts非常高效，但是它们依赖于具有特定目录结构的主机文件系统。如果你正在开发新的Docker Application，请考虑使用volumes。你不能使用Docker CLI直接管理bind mounts。

你可以使用`--volume`或`--mount`(语法更详细)flag。具体区别参考volumes的介绍。


<br>


### 启动用bind mount的容器

Start a container with a bind mount

```
#--mount
docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  nginx:latest


#--volume
docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  nginx:latest
```

<br>

**挂载到容器内非空目录**
如果挂载在容器内非空目录上，则该目录的已有内容将被隐藏。

```
#--mount
docker run -d \
  -it \
  --name broken-container \
  --mount type=bind,source=/tmp,target=/usr \
  nginx:latest


#--volume
docker run -d \
  -it \
  --name broken-container \
  -v /tmp:/usr \
  nginx:latest
```


<br/>


### 只读bind mount

Use a read-only bind mount

某些时候，容器可能只需要只读权限。

```
#--mount
docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app,readonly \
  nginx:latest


#--volume
docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app:ro \
  nginx:latest
```


<br>


### bind propagation

对于bind mounts和volumes，bind propagation(传播)默认为`rprivate`。它只能对Linux主机上的bind mounts进行配置。它是一个高级话题，许多用户并不需要配置它。

bind propagation(传播)是指在给定的bind-mounts或named volume中创建的挂载是否可以传播(propagation)到该挂载(mount)的副本(replicas)。
考虑一个挂载点`/mnt`，挂载在`/tmp`上。传播设置控制`/tmp/a`上的挂载点是否也可用于`/mnt/a`。每个传播设置都有一个递归对应点。在递归的情况下，考虑`/tmp/a`也被挂载到`/foo`。传播设置控制是否存在`/mnt/a`和`/tmp/a`。

| 传播设置 | 描述
| - | -
| shared | 原始mount的sub-mount会暴露给replica mounts，并且replica mounts的sub-mount同样传播给原始mount。也就是双向
| slave | 类似于shared，但仅限于单方向。
| private | 私有挂载
| rshared | 与shared相同，但传播也扩展到嵌套在任何原始或副本挂载点内的挂载点
| rslave | 与slave相同，但传播也扩展到嵌套在任何原始或副本挂载点内的挂载点
| rprivate | 默认值。与private相同，这意味着原始或副本挂载点内的任何位置的挂载点都不会沿任一方向传播

在设置bind propagation之前，主机文件系统需要已经支持bind propagatin: <https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt>

```
#--mount
docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  --mount type=bind,source="$(pwd)"/target,target=/app2,readonly,bind-propagation=rslave \
  nginx:latest


#--volume
docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  -v "$(pwd)"/target:/app2:ro,rslave \
  nginx:latest
```


<br/>


### selinux label


如果你使用selinux，你可以添加`z`或`Z`选项来修改挂载到容器内的主机文件或目录的selinux标签。这户影响主机本身的文件或目录，并可能导致Docker范围之外的后果。

- `z`
bind mount的内容在多个容器之间共享。

- `Z`
bind mount的内容是私有和非共享的。

```
#不支持--mount
docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app:z \
  nginx:latest
```


<br/>
<br/>


## tmpfs mounts

tmpfs: <https://docs.docker.com/storage/tmpfs/#limitations-of-tmpfs-mounts>
tmpfs mounts只支持运行在Linux上的Docker。



<br/>
<br/>



## Troubleshoot

troubleshoot: <https://docs.docker.com/storage/troubleshooting_volume_errors/>



<br/>
<br/>



## 将数据存储到容器内

Store data within containers


### 关于存储驱动

为了有效地使用存储驱动(storage driver)，了解Docker如何构建和存储镜像，以及容器如何使用镜像是很重要的。你可以使用这些信息作出明智的选择，以便找到应用程序数据持久化的最佳方式，并避免出现性能问题。

存储驱动允许你在容器的可写入层创建数据。在容器停止后，这些文件将不会被保留，并且读写速度都很低。


<br/>


#### 镜像和层

Images and layers

Docker镜像由一系列层(layer)构建而成。每个层代表镜像的Dockerfile中的指令，除最后一层外的每个层都是只读的。

考虑如下Dockerfile:

```
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

此Dockerfile包含4个命令，每个命令创建一个层。
当你创建一个新容器时，你在底层之上添加了一个新的可写入层——它通常被称为**容器层(container layer)**。
对运行中的容器所做的所有更改(增删改文件)都会写入此可写容器层。

![容器层](/images/Docker/container-layers.jpg)

存储驱动处理有关这些层相互交互的详细信息。有几个不同的驱动程序，在不同的情况下具有相应的优点和缺点。


<br/>


#### 容器和层

Container and layers

容器和镜像之间的主要区别是最高的可写入层。当容器删除时，可写入层也被删除。但底层镜像保持不变。

由于每个容器都有自己的可写入容器层，并且所有的更改都存储在此容器中，因此多个容器可以共享相同的基础镜像的访问权限，并拥有自己的数据状态。

![容器和层](/images/Docker/sharing-layers.jpg)

Docker使用存储驱动来管理镜像层和可写入容器层的内容。每个存储驱动程序都已不同方式实现，但所有驱动程序都是用可堆叠(stackable)的镜像层和写入时复制(copy-on-write)策略。


<br/>


#### 容器大小

Container size on disk

使用`docker ps -s(--size)`命令查看正在运行的容器的大小。有两个大小:

- `size`
每个容器的可写入层的数据量(在磁盘上的)

- `virtual size`
容器使用的只读镜像的数据量加上容器可写入层大小


<br/>
<br/>


#### 写入时复制

The copy-on-write (CoW) strategy

写入时复制是一种共享和复制文件以实现最高效率的策略。如果文件或目录存在于镜像的较低层中，而另外的层(包括可写入层)需要对其进行读取访问，则它只是用已有文件。第一次需要修改文件时，该文件将被复制到该层并进行修改。这最大限度减少了每个后续层的I/O和大小。

**共享促进了较小的容器**
Sharing promotes smaller images

当你创建和拉取镜像时，它们通常存储于本机的`/var/lib/docker`下。每层都存储在主机存储区内的特定目录下`/var/lib/docker/<storage-driver>/layers`。

```
ls /var/lib/docker/aufs/layers

1d6674ff835b10f76e354806e16b950f91a191d3b471236609ab13a930275e24
5dbb0cbe0148cf447b9464a358c1587be586058d9a4c9ce079320265e2bb94e7
bef7199f2ed8e86fa4ada1309cfad3089e0542fec8894690529e4c04a7ca2d73
ebf814eccfe98f2704660ca1d844e4348db3b5ccc637eb905d4818fbfb00a06a
```

<br>

**复制使容器高效**
Copying makes containers efficient

容器不会更改的任何文件都不会被复制到此可写入层中。这意味着可写入层尽可能小。

当容器中存在的文件被修改时，存储驱动之赐你个写入时复制操作(CoW)。涉及的具体步骤取决于具体的存储驱动。

aufs, overlay, overlay2存储驱动 遵循的基本顺序:

- 通过镜像层搜索要更新的文件
- 对找到的文件的第一个副本执行`copy_up`操作，将文件复制到容器的可写入层
- 任何修改应用于此复制的文件，并且该容器不能看到存在于较低层中的文件的只读副本


<br/>
<br/>


### 选择存储驱动

Select a storage driver

理想情况下，将很少的数据写入容器的可写入层，并且使用Docker volume写入数据。但某些工作负载要求你能够写入容器的可写入层，这就是存储驱动进来的地方。

存储驱动控制镜像和容器在Docker主机上的存储和管理方式。


考虑三个高层次因素：

- 如果你的Kernel支持多个存储驱动，在没有指定存储驱动的情况下，Docker会列出要使用拿个存储驱动程序的优先级列表
	+ 如果可能，将使用配置最少的存储驱动。如`brrfs`, `zfs`
	+ 否则，请尝试在最常见的情况下使用具有最佳整体性能和稳定性的存储驱动程序
		+ `overlay2`是首选(Docker CE的默认选择)，其次是`overlay`。这些都不需要额外的配置。
		+ `devicemapper`居次，但需要`direc-lvm`用于生产环境，因为`loopback-lvm`的性能很差。

- 你的选择会受限于Docker版本、操作系统和发行版

- 某些存储驱动要求你为文件系统使用特定格式

- 你的选择还取决于工作负载和所需的稳定级别


<br/>


#### Linux发行版支持的存储驱动

**Docker CE**


| Linux distribution | Recommended storage drivers
| - | -
| Docker CE on Ubuntu | aufs, devicemapper, overlay2 (Ubuntu 14.04.4 or later, 16.04 or later), overlay, zfs, vfs
| Docker CE on Debian | aufs, devicemapper, overlay2 (Debian Stretch), overlay, vfs
| Docker CE on CentOS | devicemapper, vfs
| Docker CE on Fedora | devicemapper, overlay2 (Fedora 26 or later, experimental), overlay (experimental), vfs


<br/>


#### 存储驱动支持的文件系统


| Storage driver | Supported backing filesystems
| - | -
| overlay, overlay2 | ext4, xfs
| aufs | ext4, xfs
| devicemapper | direct-lvm
| btrfs | btrfs
| zfs | zfs


<br/>


#### 查看存储驱动

```
docker info


Server Version: 18.03.1-ce
Storage Driver: overlay2
```


<br/>


### AUFS存储驱动

AUFS is a union filesystem.

aufs存储驱动用于管理Ubuntu上Docker的镜像和层。

我的发行版是Centos，此驱动针对Ubuntu。注意


<br/>


#### 使用aufs存储驱动配置Docker

- 判断kernel是否支持aufs

```
grep aufs /proc/filesystems

```

- 查看Docker存储驱动

```
docker info

```

- 配置存储驱动

```
vim /etc/docker/daemon.json

#或
--storage-driver
```


<br/>


#### aufs存储驱动如何工作

AUFS是一个联合文件系统，这意味着它在单个Linux主机上对多个目录进行分层并将它们呈现为单个目录。这些目录在AUFS术语中称为分支，在Docker术语中称为层。统一过程被称为联合安装。

![](/images/Docker/aufs_layers.jpg)


<br/>


#### 容器如何使用aufs进行读写

**读取文件**


<br/>


### Btrfs存储驱动

Use the BTRFS storage driver



<br/>


### Device Mapper存储驱动

Use the Device Mapper storage driver

Device Mapper是基于kernel的框架，支持Linux上的许多高级卷管理技术。Docker的`devicemapper`存储驱动利用此框架的精简配置和快照功能进行镜像和容器管理。

对于支持它的系统，`devicemapper`支持包含在Linux内核中。但是，需要特定配置才能将其用于Docker。`devicemapper`驱动使用专用于Docker的块设备，并在块级(block level)而不是文件级(file level)运行。这些设备可通过在Docker主机添加物理设备来扩展，并且它们比咋子操作系统级别使用文件系统更好。

<br>

**依赖**

- Docker EE
- Docker CE
- 更改存储驱动会使已创建的容器在本地系统上都无法访问

<br>

**配置devicemapper存储驱动**

- loop-lvm

```
#loop-lvm模式
/etc/docker/daemon.json


{
  "storage-driver": "devicemapper"
}


#查看
docker info
```

- direct-lvm
生产环境的devicemapper存储驱动必须使用direct-lvm模式。此模式使用块设备创建精简池。这比使用loopback设备更快，更高效地使用系统资源，并且块设备可以根据需求进行扩展。

<br/>

| Option  | Description | Required | Default | Example
| - | - | - | - | -
| dm.directlvm_device | The path to the block device to configure for direct-lvm. | Yes | - | dm.directlvm_device="/dev/xvdf"
| dm.thinp_percent | The percentage of space to use for storage from the passed in block device. |  No | 95 | dm.thinp_percent=95
| dm.thinp_metapercent | The percentage of space to for metadata storage from the passed-in block device. |  No | 1 |  dm.thinp_metapercent=1
| dm.thinp_autoextend_threshold | The threshold for when lvm should automatically extend the thin pool as a percentage of the total storage space. |  No | 80 | dm.thinp_autoextend_threshold=80
| dm.thinp_autoextend_percent | The percentage to increase the thin pool by when an autoextend is triggered. | No | 20 | dm.thinp_autoextend_percent=20
| dm.directlvm_device_force | Whether to format the block device even if a filesystem already exists on it. If set to false and a filesystem is present, an error is logged and the filesystem is left intact. | No |  false |  dm.directlvm_device_force=true

```
#安装依赖
RHEL / CentOS: device-mapper-persistent-data, lvm2, and all dependencies
Ubuntu / Debian: thin-provisioning-tools, lvm2, and all dependencies


#创建物理卷(physical volume)
pvcreate /dev/cvdf


#创建卷组(volume group)
vgcreat docker /dev/xvdf


#创建逻辑卷(logical volume)
lvcreate --wipesignatures y -n thinpool docker -l 95%VG
lvcreate --wipesignatures y -n thinpoolmeta docker -l 1%VG


#转换卷为精简池
lvconvert -y \
--zero n \
-c 512K \
--thinpool docker/thinpool \
--poolmetadata docker/thinpoolmeta


#配置lvm配置文件精简池自动扩展
/etc/lvm/profile/docker-thinpool.profile


#指定thin_pool_autoextend_threshold 和 thin_pool_autoextend_percent的值
activation {
  thin_pool_autoextend_threshold=80
  thin_pool_autoextend_percent=20
}


#应用LVM profile
lvchange --metadataprofile docker-thinpool docker/thinpool


#启用监控LV
lvs -o+seg_monitor


#配置devicemapper存储驱动
/etc/docker/daemon.json
{
    "storage-driver": "devicemapper",
    "storage-opts": [
    "dm.thinpooldev=/dev/mapper/docker-thinpool",
    "dm.use_deferred_removal=true",
    "dm.use_deferred_deletion=true"
    ]
}


#查看
docker info

```


<br>


#### 管理devicemapper

```
#查看LVM logs
journalctl -fu dm-event.service


pvdisplay
vgdisplay/vgextend
lvdisplay/lvextend/lvchange
```


<br/>


### OverlayFS存储驱动

Use the OverlayFS storage driver



<br/>


### ZFS存储驱动

Use the ZFS storage driver


<br/>


### VFS存储驱动

Use the VFS storage driver

VFS存储驱动不是联合文件系统，相反，每层都是磁盘上的一个目录，它不支持CoW。要创建一个新层，先前的层会进行**深层复制(deep copy)**。与其它驱动相比，这导致磁盘性能下降和占用更多磁盘空间。但是，它强大，稳定，适用于各种环境。

**配置VFS存储驱动**

```
vim /etc/docker/daemon.json


{
  "storage-driver": "vfs"
}



#控制大小
{
  "storage-opts": ["size=256M"]
}


#查看
docker info
```


<br/>
<br/>

---

<br/>



# 在生产环境运行应用程序

Run your app in production


<br>


## 配置对象

Configure all objects


<br>


### 自定义原数据

Apply custom metadata to objects

**Docker object label**
标签(label)是一种将原数据(metadata)应用于docker object的机制，包含:

- image
- container
- local daemon
- volume
- network
- node
- service


<br>


#### label key and value

标签是一组键值对，以字符串形式存储。可以为对象指定多个标签，但每个键值对必须唯一。如果一个键有多个值，则最新写入的值会覆盖以前的值。

**key格式建议**
label key是可能包含字母，数字，`.`，`-`组成的字符串。

- 第三方工具的作者给每个label key加上前缀域，如`com.example.some-label`
- 未经允许，不得使用他人域
- `com.docker.*`, `io.docker.*`, `org.dockerproject.*`命名空间保留给Docker内部使用
- 以小写字母开头和结尾
- 用`.`分割命令空间字段

<br>

**value 指南**
label value可以包含任何可表示为字符串的数据类型，包括JSON, XML, CSV, YAML...唯一的要求是，首先使用特定于结构类型的机制将该值序列化为字符串。


<br/>
<br/>


### 清理未使用的对象

Prune unused Docker objects

Docker采取保守的方法来清理未使用的对象(通常称为垃圾回收)，通常它不会删除这些对象，除非你明确要求Docker这样做。对于每个类型的对象，docker提供了`prune`命令。你也可以使用`docker system prune`命令一次清理多种类型的对象。

```
#prune image
docker image prune
 docker image prune -a --filter "until=24h"


#prune container
docker container prune


#prune volume
docker volume prune
docker volume prune --filter "label!=keep"


#prune everything
docker system prune
docker system prune --volumes
```


<br/>
<br/>


### 格式化输出

Format command and log output

```
#join
docker inspect --format '{{join .Args " , "}}' container


#json
docker inspect --format '{{json .Mounts}}' container


#lower
docker inspect --format "{{lower .Name}}" container


#split
docker inspect --format '{{split .Image ":"}}'


#title
docker inspect --format "{{title .Name}}" container


#upper
docker inspect --format "{{upper .Name}}" container


#printIn
docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' container
```


<br/>
<br/>


## 配置daemon

Configure the daemon


<br/>


### 配置和运行Docker

**配置docker daemon**

- 使用json配置文件
- 使用`dockerd --flag`

```
/etc/docker/daemon.json

{
  "debug": true,
  "tls": true,
  "tlscert": "/var/docker/server.pem",
  "tlskey": "/var/docker/serverkey.pem",
  "hosts": ["tcp://192.168.59.3:2376"]
}


#或
dockerd --debug \
  --tls=true \
  --tlscert=/var/docker/server.pem \
  --tlskey=/var/docker/serverkey.pem \
  --host tcp://192.168.59.3:2376
```

<br>

**docker daemon目录**
docker daemon将所有数据保存在一个目录中。你可以手动修改它。

默认目录:
- Linux： `/var/lib/docker`
- Windows: `C:\ProgramData\docker`


<br/>
<br/>


### 使用systemd控制docker

Control Docker with systemd

```
cat /usr/lib/systemd/system/docker.service
#or
cat /etc/systemd/system/docker.service


systemctl enable/start/stop/status docker
```

<br>

**自定义docker daemon选项**

```
vim /etc/docker/daemon.json

{
    "data-root": "/mnt/docker-data",
    "storage-driver": "overlay"
}
```

<br>

**http/https proxy**
Docker daemon使用`HTTP_PROXY`，`HTTPS_PROXY`和`NO_PROXY`环境变量来配置代理行为。无法使用`daemon.json`文件来配置环境变量。

```
mkdir -p /etc/systemd/system/docker.service.d


#/etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80/"

#/etc/systemd/system/docker.service.d/https-proxy.conf
[Service]
Environment="HTTPS_PROXY=https://proxy.example.com:443/"


systemctl daemon-reload
systemctl restart docker
systemctl show --property=Environment docker
```


<br/>
<br/>


### 收集Docker指标

Collect Docker metrics with Prometheus

Promethus: <https://prometheus.io/>
Prometheus是一个开源的系统监控和报警工具包。你可以将Docker配置为Prometheus target。设置Prometheus作为Docker容器运行，并使用Prometheus监控Docker实例。

**配置Docker**
配置docker daemon作为Prometheus target，你需要指定`metrics-address`。最佳方式是通过`daemon.json`。

```
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```

<br>

**配置和运行Prometheus**

```
/tmp/prometheus.yml


# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
      monitor: 'codelab-monitor'

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first.rules"
  # - "second.rules"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9323']
```

```
docker service create --replicas 1 --name my-prometheus \
    --mount type=bind,source=/tmp/prometheus.yml,destination=/etc/prometheus/prometheus.yml \
    --publish published=9090,target=9090,protocol=tcp \
    prom/prometheus
```

访问: <http://localhost:9090/targets/>


<br/>
<br/>



## 配置容器

Configure containers


### 自动启动容器

Start containers automatically

Docker提供了重启策略，以控制容器在退出或重启时自动启动。重启策略可确保链接的容器以正确的书序启动。Docker建议你使用重启策略，并避免使用进程管理器(如supervisor)来启动容器。
重启策略与`docker xxx --live-restart`标志不同，后者可以让你在Docker upgrage期间保持容器运行。

<br>

**重启策略**
使用`docker run xxx --restart`标志来配置重启策略，`--restart`的值如下：

| 标志 | 描述
| - | -
| no | 不要自动重启容器(默认值)
| on-failure | 如果容器由于错误(非零退出码)退出，则重启容器
| unless-stopped | 除非明确停止或docker本身停止或重启，则重启容器
| always | 如果停止，则始终重启容器

```
#栗子
docker run -dit --restart unless-stopped redis
```

<br>

**重启策略注意事项**

- 重启策略尽在容器成功启动后才生效——这意味着容器已启动至少10s，并且Docker已开始监视它。
这可以防止根本不启动的容器进入重启循环。

- 如果你手动停止容器(状态码为0)，则在重启Docker daemon或手动启动容器之前，其重启策略将会被忽略。
这是另一个防止重启循环的尝试。

- 重启策略仅适用于容器。集群服务的重启策略与此不同。


<br/>
<br/>


### 在daemon停机期间保持容器活着

Keep containers alive during daemon downtime

默认情况下，当Docker daemon终止时，它会关闭正在运行的容器。从Docker Engine 1.12开始，你可配置守护进程，以便在守护进程不可用时容器保持运行。这个功能被称为**实时恢复(live restore)**。
它不支持Windows container。

<br>

**实时恢复**
有两种方式来启用live restore，只启用其中一个就好。
实时恢复仅适用于独立容器，不适用于集群服务。

- 修改配置文件

```
/etc/docker/daemon.json

{
  "live-restore": true
}
```

- `--live-restore`标志
不推荐

```
dockerd xxx --live-restore
```


<br/>
<br/>


### 在一个容器中运行多个服务

Run multiple services in a container

容器的主要运行进程是Dockerfile末尾的`ENTRYPOINT`或`CMD`指令。通常建议你通过每个容器运行一项服务来分割关注区域。这些服务可能会分成多个进程(如Nginx的worker processe)。你可以使用用户定义的network和shared volumes来连接多个容器。

容器的主进程负责管理它启动的所有进程。在某些情况下，主进程设计不好，在容器退出时无法正常处理**停止**子进程。如果你的进程属于这个类别，你可在容器运行时使用`--init`选型。`--init`标志将一个微小的`inti-process`作为主进程插入到容器中，并在容器退出时处理所有进程的停止。以这种方式处理这些进程优于使用完整的初始化进程。

<br>

如果你需要在一个容器中运行多个服务，则可通过几种不同方式来完成此操作。

- 将所有命令封装进一个脚本中，并附带测试和调试信息。以封装脚本作为你的CMD

```
vim my_wrapper.sh

#!/bin/bash
xxxxx
xxx


vim Dockerfile

FROM ubuntu:latest
COPY my_first_process my_first_process
COPY my_second_process my_second_process
COPY my_wrapper_script.sh my_wrapper_script.sh
CMD ./my_wrapper_script.sh
```

- 使用如`supervisord`这样的进程管理器

```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y supervisor
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY my_first_process my_first_process
COPY my_second_process my_second_process
CMD ["/usr/bin/supervisord"]
```


<br/>
<br/>


### 容器运行指标

Container runtime metrics


**docker stats**

```
docker stats redis1 redis2

CONTAINER           CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O
redis1              0.07%               796 KB / 64 MB        1.21%               788 B / 648 B       3.568 MB / 512 KB
redis2              0.07%               2.746 MB / 64 MB      4.29%               1.266 KB / 648 B    12.4 MB / 0 B
```

<br>

**Control groups**

Linux Container依赖于`control group`，这些组不仅跟踪进程组，还公开有关CPU，mem，block I/O的使用情况和度量标准。你可以访问这些指标并判断容器运行状况。
control group通过为文件系统(pseudo-fs)公开，你应该可在`/proc/fs/cgroup`中找到它。

查看cgroup子系统：

```
grep cgroup /proc/mounts

#or
mount -l | grep cgroup


#进程
/proc/<pid>/cgroup


#/表示进程尚未分配给group
cat /proc/1/cgroup
11:devices:/
10:cpuset:/
9:hugetlb:/
8:memory:/
7:blkio:/
6:net_prio,net_cls:/
5:pids:/
4:perf_event:/
3:cpuacct,cpu:/
2:freezer:/
1:name=systemd:/
```

<br>

**查找给定容器的cgroup**
对于每个容器，每个层次结构中创建一个cgroup。

```
/sys/fs/cgroup/memory/docker/<docker-longid>/

cd /sys/fs/cgroup/memory/docker/893903129d869f384bd78d63a382f1c9527d6979be0a6cf3c13ea4f45a1554d6
cat memory.stat

cache 36282368
rss 196608
rss_huge 0
mapped_file 1077248
swap 0
pgpgin 212904
pgpgout 205531
pgfault 314692
pgmajfault 204
inactive_anon 131072
active_anon 65536
inactive_file 18223104
active_file 18059264
unevictable 0
hierarchical_memory_limit 9223372036854771712
hierarchical_memsw_limit 9223372036854771712
total_cache 36282368
total_rss 196608
total_rss_huge 0
total_mapped_file 1077248
total_swap 0
total_pgpgin 212904
total_pgpgout 205531
total_pgfault 314692
total_pgmajfault 204
total_inactive_anon 131072
total_active_anon 65536
total_inactive_file 18223104
total_active_file 18059264
total_unevictable 0



#其它信息类似
```


<br/>
<br/>


### 限制容器的资源

Limit a container's resources

默认情况下，容器没有资源限制，可以使用主机内核调度程序允许给定的资源。Docker提供了一些方法来控制容器可以使用的CPU、memory、block I/O。

许多这些功能需要内核的支持。使用`docker info`命令检查是否支持。如果内核禁用了某功能，则可能会有如下警告: **WARNING: No swap limit support**


<br>


#### memory

**你需要了解内存耗尽(out of memory)的风险**
不要让正在运行的容器消耗太多的主机内存，这很重要。在Linux主机上，如果内核检测到没有足够的内存来执行重要的系统功能，它会抛出一个**OOME(out of memory exception)**，并开始killing process以释放进程。任何进程都会是killing objects，包括Docker和其它重要应用程序。

docker尝试通过调整docker daemon的OOM优先级来降低这些风险，从而使其比系统上的其它进程更小(less)可能的被killing。容器的OOM优先级不进行调整，这使得单个容器被killing的可能性要大于docker或其它进程。你不应该给docker daemon的`--oom-score-adj`或container的`--oom-kill-disable`标志来绕过这些安全措施。

你可以通过以下方式减轻由OOM引起的系统不稳定的风险:

- 在上线之前，进行测试以了解应用程序的内存需求
- 确保应用程序仅在拥有足够资源的主机上运行
- 限制容器可使用的内存量
- 在主机上配置swap时请注意。swap比内存更慢，性能更低，但可以提供缓冲区以防系统内存耗尽
- 考虑将容器转换为服务，并使用服务级别约束和节点标签来确保应用程序仅在具有足够内存的主机上运行

<br>

**限制容器对内存的**
Limit a container’s access to memory

Docker可以强制`hard limit`，允许容器使用不超过给定数量的用户/系统内存，或`soft limit`。这允许容器使用尽可能多的内存。

如下这些选项具有这样一些效果，注意内存单位`b, k, m, g`：

| 选项 | 描述
| - | -
| `-m/--memory=` | 容器可使用的最大内存量。如果你设置此选项，则允许的最小值为4m
| `--memory-swap` | 容器允许使用的swap量。只有在`--momery`设置时才有意义
| `--memory-swappiness` | 默认情况下，容器可使用的主机内核可交换的匿名页面的百分比<0-100>
| `--memory-reservation` | 允许你指定一个小于`--memory`的soft limit。当docker检测到内存不足时，此会被激活
| `--kernel-memory` | 容器可以使用的最大kernel memory。内核内存不能够被swap out，因此内核内存不足的容器可能会阻塞主机资源，这会对主机和其它容器产生副作用
| `--oom-kill-disable` | 默认情况下，如果发生内存溢出(OOM)，内核会杀死容器中的进程。使用此选项改变此行为


<br/>


#### cpu

默认情况下，每个容器对主机CPU周期的访问是无限制的。你可以设置各种约束来限制给定容器访问主机的CPU周期。

**CFS scheduler**
CFS是用于普通Linux进程的Linux kernel CPU调度器，一些运行时标志用于配置容器的CPU资源访问量。

| 选项 | 描述
| - | -
| `--cpu=<value>` | 指定容器可以使用的CPU资源，如`--cpu="1.6"`
| `--cpu-period=<value>` | 指定CFS调度器周期，它与`--cpu-quota`一起使用。默认100ms。Docker1.13以后，使用`--cpus`替代
| `--cpu-quota=<value>` | 在容器上条件CFS配额。在Docker1.13以后，使用`--cpus`替代
| `--cpuset-cpus` | 限制容器可以使用的特定CPU或CORE。如果有多个CPU，请使用逗号`,`分割。如`0,2`
| `--cpu-shares` | 将此标志设置为大于/小于1024(默认值)的值，以增加或减少容器的重量，并使其能够访问更大或更小比例的主机CPU周期。这仅在CPU周期受到限制时才会执行。

如果你只有1 CPU，如下命令可保证容器每秒最多有50%的CPU——`docker run -it --cpus=".5" xxx`

<br>

**realtime scheduler**

在Docker1.13及更高版本，对于无法使用CFS的任务，你可以使用realtime scheduler。
在你配置docker daemon和container之前，请正确地配置主机内核。

**注意： CPU调度和优先级是高级内核功能。大多数用户不需要修改它。错误地设置将导致主机系统不稳定或不可用。**

- 配置主机内核
通过运行`zcat /proc/config.gz | grep CONFIG_RT_GROUP_SCHED`或检查`/sys/fs/cgroup/cpu.rt_runtime_us`来验证内核是否启用了`CONFIG_RT_GROUP_SCHED`。有关配置内核实时调度器的指导，请参考相关文档。

- 配置docker daemon
运行docker daemon时使用`--cpu-rt-runtime`标志设置每个运行时间段的实时任务保留的最大微秒数。可使用systemd的docker.service进行配置。

- 配置独立容器
当使用`docker run`启动容器时，可以传递多个标志来控制容器CPU的优先级。

| 选项 | 描述
| - | -
| `--cap-add=sys_nice` | 授予容器`CAP_SYS_NICE`功能，允许容器提升进程的nice值，设置实时调度策略，设置CPU关联和其它操作
| `--cpu-rt-runtime=<value>` | Docker实时调度器期间，容器可以以实时优先级运行的最大微秒数。需要`--cap-add=sys_nice`标志
| `--ulimit rtprio=<value>` | 容器允许的最大实时优先级，需要`--cap-add=sys_nice`标志

栗子：

```
docker run --it --cpu-rt-runtime=950000 \
                  --ulimit rtprio=99 \
                  --cap-add=sys_nice \
                  debian:jessie
```


<br/>
<br/>



### Logging


#### 查看容器日志

记录的信息和日志格式取决于容器的端点命令。
`docker logs`命令显示正在运行的容器记录的信息。
`docker service logs`命令显示参与服务的所有容器记录的信息。在swarm模式下。

在某些情况下，`docker logs`可能不会显示有用的信息，除非你采取其它措施。

- 如果将日志发送到文件、主机、数据库或其它日志驱动程序，则`docker logs`可能不会显示有用的信息
- 如果你的镜像运行non-interactive进程(如数据库)，则该应用程序可能会将output发送到日志文件而不是stdout/stderr


<br/>


#### 配置日志驱动

Configure logging drivers

docker提供了多种日志记录机制(logging mechanisms)来帮助你从运行的容器和服务中获取信息。这些机制被称为日志驱动(logging driver)。
每个docker daemon都有一个默认日志驱动，每个容器也默认使用该驱动。除非你给容器配置了其它日志驱动。
除了使用docker附带日志驱动，在Docker v17.05之后，你还可以使用日志驱动插件(logging driver plugin)。

<br>

**配置默认日志驱动**
默认的日志驱动是`json-flie`。
可在`daemon.json`文件里通过`log-driver`选项匹配置日志驱动。

```
/etc/docker/daemon.json

#设置为syslog
{
  "log-driver": "syslog"
}
```


如果日志驱动存在可配置选项：

```
/etc/docker/daemon.json

{
  "log-driver": "json-file",
  "log-opts": {
    "labels": "production_status",
    "env": "os,customer"
  }
}


#查看
docker info | grep 'Loggin Driver'
Logging Driver: json-file
```

<br>

**为容器配置日志驱动**
启动容器时，可使用`--log-driver`标志为其配置不同于docker daemon的日志驱动。

```
docker run -it --log-driver none alpine ash


#查看容器日志驱动
docker inspect -f '{{.HostConfig.LogConfig.Type}}' <CONTAINER>
```

<br>

**配置从容器到日志驱动的log message的交付模式**
Docker为从容器到日志驱动的日志消息提供了两种交付(delivery）模式：

- 直接阻塞(blocking)从容器到驱动的交付(默认)

- 非阻塞交付(non-blocking)，将日志消息存储在中间每个容器的环形缓冲区中供驱动使用
非阻塞消息交付模式可防止应用程序因日志反压而被阻塞。当STDERR或STDOUT流阻塞时，应用程序可能会以意想不到的方式失败。

**注意：当缓冲区已满且新消息排入队列时，内存中最早的消息将被丢弃。丢弃消息通常首选阻止应用程序的日志写入过程。**

```
docker run -it --log-opt mode=non-blocking --log-opt max-buffer-size=4m alpine ping 127.0.0.1
```

<br>

**日志驱动使用环境变量或label**
一些日志驱动将容器的`--env/-e`或`--label`标签的值添加到容器的日志中。

```
docker run -dit --label production_status=testing -e os=ubuntu alpine sh
```

<br>

**支持的日志驱动**
如下是受支持的日志驱动。

| 驱动 | 描述
| - | -
| none | No logs are available for the container and docker logs does not return any output.
| json-file | The logs are formatted as JSON. The default logging driver for Docker.
| syslog | Writes logging messages to the syslog facility. The syslog daemon must be running on the host machine.
| journald | Writes log messages to journald. The journald daemon must be running on the host machine.
| gelf | Writes log messages to a Graylog Extended Log Format (GELF) endpoint such as Graylog or Logstash.
| fluentd | Writes log messages to fluentd (forward input). The fluentd daemon must be running on the host machine.
| splunk | Writes log messages to splunk using the HTTP Event Collector.
| logentries | Writes log messages to Rapid7 Logentries.
| 云日志系统 | 各类云服务商提供的云日志系统

`docker logs`命令不适用于除`json-file`和`journald`之外的其它日志驱动。


<br/>


#### 日志驱动插件

日志驱动插件允许你扩展和定制docker的日志记录功能，超越了内置的日志驱动的功能。

- 安装日志驱动插件

```
docker plugin install <org/image>

docker plugin ls
```

- 将插件配置为docker daemon默认日志驱动

```
/etc/docker/daemon.josn


#or
--loggin-driver
```

- 将插件配置为容器日志驱动

```
docker run xxx --log-driver
```


<br/>


#### 定制日志驱动输出

Customize log driver output

日志选项`tag`指定如何格式化表示容器日志消息。默认情况下，系统使用容器ID的前12个字符。你可以指定`tag`选项来覆盖此行为：

```sh
docker run --log-driver=fluentd \
		  --log-opt fluentd-address=myhost.local:24224 \
		  --log-opt tag="mailer"
```

在指定`tag`时，Docker支持的一些特殊模板标记：

```
{{.ID}}
The first 12 characters of the container ID


{{.FullID}}
The full container ID


{{.Name}}
The container name


{{.ImageID}}
The first 12 characters of the container’s image ID


{{.ImageFullID}}
The container’s full image ID


{{.ImageName}}
The name of the image used by the container


{{.DaemonName}}
The name of the docker program (docker)
```


```
--log-opt tag="{{.ImageName}}/{{.Name}}/{{.ID}}"

Aug  7 18:33:19 HOSTNAME hello-world/foobar/5790672ab6a0[9103]: Hello from Docker.
```


<br/>


#### 日志驱动

介绍如下日志驱动！


##### Logentries

Logentries日志驱动将容器日志发送到Logentries server。

`--log-opt`:

- `logentries-token`: 指定Logentries log设置的token
- `line-only`: 仅发送原始有效载荷


docker daemon:

```
dockerd --log-driver=logentries

#可在docker.service中设置
```

docker container:

```
docker run --log-driver=logentries ...

```

在使用此日志驱动之前，你需要在Logentries web界面中创建一个新的日志集，并将该日志集的令牌传递给docker：

```
docker run --log-driver=logentries --log-opt logentries-token=abcd1234-12ab-34cd-5678-0123456789ab

```


<br/>


##### json file

默认情况下，docker捕获所有容器的STDOUT和STDERR，并使用json格式将它们写入文件。每个文件包含仅包含一个容器的信息。

```
/etc/docker/daemon.json

{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m"
  }
}


#or
docker run \
      --log-driver json-file --log-opt max-size=10m \
      alpine echo hello world


#栗子
docker run -it --log-opt max-size=10m --log-opt max-file=3 alpine ash
```

json-file支持的日志选项：

| 选项 | 描述 | 栗子
| - | - | -
| max-size | The maximum size of the log before it is rolled. A positive integer plus a modifier representing the unit of measure (k, m, or g). Defaults to -1 (unlimited).	| --log-opt max-size=10m
| max-file | The maximum number of log files that can be present. If rolling the logs creates excess files, the oldest file is removed. Only effective when max-size is also set. A positive integer. Defaults to 1. | --log-opt max-file=3
| labels | Applies when starting the Docker daemon. A comma-separated list of logging-related labels this daemon accepts. Used for advanced log tag options. | --log-opt labels=production_status,geo
| env | Applies when starting the Docker daemon. A comma-separated list of logging-related environment variables this daemon accepts. Used for advanced log tag options. | --log-opt env=os,customer
| env-regex | Similar to and compatible with env. A regular expression to match logging-related environment variables. Used for advanced log tag options. | --log-opt env-regex=^(os或customer).


<br/>


##### Graylog Extended Format(gelf)

`gelf`日志驱动是一种方便的格式，可被Graylog, Logstash, Fluentd等工具所理解。许多工具使用这种格式。

在GELF中，每条日志消息都是带有一下字段的字典：

- version
- host
- timestamp
- short and long version of the message
- 自定义的字段

<br>

```
/etc/docker/daemon.json


{
  "log-driver": "gelf",
  "log-opts": {
    "gelf-address": "udp://1.2.3.4:12201"
  }
}


#or
dockerd --log-driver gelf –-log-opt gelf-address=udp://1.2.3.4:12201


#容器
docker run \
      --log-driver gelf –-log-opt gelf-address=udp://1.2.3.4:12201 \
      alpine echo hello world
```

<br>

GELF选项：

| Option| Required | Description | Example
| - | - | - | -
| `gelf-address` | required | GELF服务器地址(tcp/udp) | `--log-opt gelf-address=udp://192.168.0.42:12201`
| `gelf-compression-type` | optional | 仅限于UDP。类型有gzip(default),zlib,none | `--log-opt gelf-compression-type=gzip`
| `gelf-compression-level` | optional | `-1/0 - 9`,`-1/0`(禁用压缩)，1(BestSpeed)，9(BestCompress) | `--log-opt gelf-compression-level=2`
| `gelf-tcp-max-reconnect` | optional | 仅TCP，连接断开尝试的最大重连次数，默认3 | `--log-opt gelf-tcp-max-reconnect=3`
| `gelf-tcp-reconnect-delay` | optinal | 仅TCP，重连等待的秒数，默认1s | `--log-opt gelf-tcp-reconnect-delay=1`
| `tag` | optional | 默认使用Docker容器ID的前12位 | `--log-opt tag=mailer`
| `labels` | optional | 以逗号分隔的日志相关标签 | `--log-opt labels=production_status,geo`
| `env` | optional | 以逗号分隔的日志相关的环境变量 | `--log-opt env=os,customer`
| `evn-regex` | optional | 匹配日志相关环境变量的正则表达式 | `--log-opt env-regex=^(os l customer)`


<br/>


##### Syslog

syslog日志驱动将日志路由到系统日志服务器。系统日志必须以特定方式格式化才能生效。从有效的消息中，接收者可以提取以下消息：

- priority
日志级别，debug, info, warning, error...

- timestamp
- hostname
- facility
记录消息的子系统

- process name
- pid

<br>

```
/etc/docker/daemon.json


{
  "log-driver": "syslog",
  "log-opts": {
    "syslog-address": "udp://1.2.3.4:1111"
  }
}


#or
#syslog-address支持tcp和udp
docker run \
      -–log-driver syslog –-log-opt syslog-address=udp://1.2.3.4:1111 \
      alpine echo hello world
```

<br>

syslog日志驱动选项：

| 选项 | 描述 | 栗子
| - | - | -
| `syslog-address` | `[tcp l udp l tcp+tls]:host:port`, `unixgram://path`, `unix://path` | `--log-opt syslog-address=tcp+tls://192.168.1.3:514`, `--log-opt syslog-address=unix:///tmp/syslog.sock`
| `syslog-facility` | 子系统 | `--log-opt syslog-facility=daemon`
| `syslog-tls-ca-cert` | CA | `--log-opt syslog-tls-ca-cert=/etc/ca-certificates/custom/ca.pem`
| `syslog-tls-cert` | TLS certificate | `--log-opt syslog-tls-key=/etc/ca-certificates/custom/key.pem`
| `syslog-tls-skip-verify` | 跳过tls验证 | `--log-opt syslog-tls-skip-verify=true`
| `tag` | 如前 | 如前
| `syslog-format` | 日志格式 | `--log-opt syslog-format=rfc5424micro`
| `lables` | 如前 | 如前
| `env` | 如前 | 如前
| `env-regex` | 如前 | 如前


<br/>


##### ETW

ETW日志驱动将容器日志转发为ETW事件。每个ETW时间都包含一条日志及其上下文信息的消息，然后客户端可以创建一个ETW监听器来监听这些事件。


<br/>


##### Fluentd

fluentd日志驱动将容器日志作为结构化日志数据发送到fluentd收集器。接着，用户便可以使用任意一种Fluentd output plugin将这些日志写入不同的目的地。

fluentd发送一下这些元数据：

| 字段 | 描述
| - | -
| `container_id` | 完整的64位容器ID
| `container_ame` | 启动时的容器名
| source | stdout or stderr
| log | 容器日志

`docker logs`命令不可用于此日志驱动。

<br>

- `fluentd-address`
指定fluentd daemon地址

- tag

```
/etc/docker/daemon.json

{
   "log-driver": "fluentd",
   "log-opts": {
     "fluentd-address": "fluentdhost:24224"
   }
}


#or
docker run --log-driver=fluentd --log-opt fluentd-address=fluentdhost:24224
docker run --log-driver=fluentd --log-opt fluentd-address=tcp://fluentdhost:24224
docker run --log-driver=fluentd --log-opt fluentd-address=unix:///path/to/fluentd.sock
```


<br/>


##### Journald

journald 日志驱动将容器日志发送给 systemd journal。可以通过`journalctl`命令，`journal` API，`docker logs`来检索日志条目。

journald日志驱动还提供如下元数据：

- `CONTAINER_ID`
- `CONTAINER_ID_FULL`
- `CONTAINER_NAME`
- `CONTAINER_TAG`
- `CONTAINER_PARTIAL_MESSAGE`

```
/etc/docker/daemon.json

{
  "log-driver": "journald"
}


#or
docker run --log-driver=journald ...
```

<br>

几个选项：

- tag
- label
- env
- env-regex

```
docker run --log-driver=journald \
    --log-opt labels=location \
    --log-opt env=TEST \
    --env "TEST=false" \
    --label location=west \
    your/application
```

<br>

使用`journalctl`命令查看日志：

```
journalctl CONTAINER_NAME=webserver
journalctl -o json CONTAINER_NAME=webserver
```

使用journal API：

```py
#python

import systemd.journal

reader = systemd.journal.Reader()
reader.add_match('CONTAINER_NAME=web')

for msg in reader:
    print '{CONTAINER_ID_FULL}: {MESSAGE}'.format(**msg)
```


<br/>


##### Splunk

splunk日志驱动将容器日志发送到Splunk Enterprise和Splunk Clound的HTTP Event Collector。


<br/>
<br/>


### 安全

