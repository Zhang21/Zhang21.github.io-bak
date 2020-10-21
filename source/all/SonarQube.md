title: SonarQube
date: 2019-02-22 15:28:44
categories: DevOps
tags:
  - SonarQube
  - Static Analysis
  - Code Quality
---


参考:

- GitHub: <https://github.com/SonarSource/sonarqube>
- Website: <https://www.sonarqube.org/>
- Docs: <https://docs.sonarqube.org>


环境:

- RHEL7x86_64
- SonarQube v7.6



<br/>
<br/>

---

<!--more-->

<br/>
<br/>








# 介绍


**SonarQube** 是一个开源的代码质量管理系统。支持超过25中编程语言，不过有些是商业插件。

SonarQube 是一种自动代码审查(code review)工具，用于检测代码中的**错误(bugs)**，**漏洞(vulnerabilities)**和**代码异味(code smell)**。它可以与您现有的工作流程集成，以便在项目分支和拉取请求之间进行连续的代码检查。






<br/>
<br/>

---

<br/>
<br/>













# 架构与集成

Architecture and Integration


<br/>


## 概述


SonarQube平台由4个组件组成:

![](/images/SonarQube/architecture-scanning.png)

<br>

- **SonarQube Server**启动三个主进程:
  -  **Web Server**，供开发人员，管理人员浏览质量快照并配置SonarQube实例
  -  **Search Server**，基于ElasticSearch从UI返回搜索
  -  **Compute Engine Server**，负责处理代码分析和上报并将其保存到SonarQube数据库中
- **SonarQube Database**用于存储
  - SonarQube实例的配置(安全，插件...的设置)
  - 项目，视图...的质量快照
- Server上安装了多个插件，可能包括Language，SCM，Intergration，Authentication，Governance...
- 在CI/CD Server上运行一个或多个 **SonarScanner** 来分析项目




<br/>
<br/>
<br/>



## 集成

Integration


以下模式显示了SonarQube如何与其它ALM工具进行集成，以及在哪里使用SonarQube的各种组件。

![](/images/SonarQube/architecture-integrate.png)

<br>

1. 开发者在他们的IDE中集成SonarLint运行本地分析
2. 开发者推送他们的代码到代码库
3. CI Server触发自动构建，以及执行运行SonarQube分析所需的SonarScanner
4. 分析报告将发送到SonarQube Server进行处理
5. SonarQube Server处理分析报告并将结果存储在SonarQuebe数据库中，并在UI中显示结果
6. 开发者通过SonarQube UI审核，评论，挑战他们的Issues以管理和减少他们的技术债务
7. 管理者从分析中接收报告，运维使用API自动配置并从SonarQube中提取数据，使用JMX监控SonarQube Server



<br/>
<br/>
<br/>



## 关于机器和位置

About Machines and Locations


- SonarQube平台不能够有多个SonarQube Server和SonarQube Database
- 为获得最佳性能，每个组件(Server, Database, Scanner)应该安装在单独的机器上，并且此机器应该是专用的
- SonarScanner通过添加机器进行扩展
- 所有机器必须时钟同步
- SonarQube Server和SonarQube Database必须位于同一网络下
- SonarScanner不需要与SonarQube Server位于同一网络下
- SonarScanner与SonarQube Database之间没有通信









<br/>
<br/>

---

<br/>
<br/>






# 要求

Requirements


<br/>


## 先决条件

Prerequisites and Overview


运行SonarQube的唯一先决条件是安装Java(Oracle JRE 8/OpenJDK 8)。


<br/>


### 硬件要求


- 2Cores+
- 2GB RAM+
- 建议使用高性能I/O的磁盘


<br/>
<br/>


### 支持的平台

- Java
  - Oracle JRE 8
  - OpenJDK 8
- Database
  - PostgreSQL v9.3-v9.6, v10. UTF-8 charset
  - SQL Server v2014, v2016. 
  - Oracle v11, v12, vXE. UTF8-family charset, thin mode
  - MySQL v5.6, v5.7. UTF8 charset, InnoDB storage, mysql-connector-java
- Web Browser
  - IE 11
  - Edge Latest
  - FireFox Latest
  - Chrome
  - Safari


<br/>
<br/>


### 平台说明


<br/>


#### Linux

如果在Linux上运行，请确保:

- `vm.max_map_count` 大于或等于 262144
- `fs.file-max` 大于或等于 65535
- 运行SonarQube的用户可以打开至少65535个文件描述符
- 运行SonarQube的用户可以打开至少2048个线程

用以下命令查看和配置它们:

```bash
sysctl vm.max_map_count
sysctl fs.file-max
ulimit -n
ulimit -u


# 配置，但只是临时生效
# root
sysctl -w vm.max_map_count=262144
sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 2048


# 永久生效
# /etc/sysctl.d/99-sonarqube.conf 或 /etc/sysctl.conf
# user: sonarqube
sonarqube   -   nofile   65536
sonarqube   -   nproc    2048
```

<br>

如果使用`systemd`来启动SonarQube，你必须在`[Service]`的单元文件中指定这些限制:

```
[Service]
...
LimitNOFILE=65536
LimitNPROC=2048
```


<br/>
<br/>


#### seccomp filter

默认情况下，ElasticSearch使用**seccomp filter**。在大多数发行版中，此功能在内核中激活。但在RHL6等发行版上，此功能已停用。如果你的发行版中没有此功能，请无法升级到激活了seccomp filter功能的版本，则必须通过更新` $SONARQUBEHOME/conf/sonar.properties_`中的`sonar.search.javaAdditionalOpts`配置:

```
sonar.search.javaAdditionalOpts=-Dbootstrap.system_call_filter=false

# 检查
grep SECCOMP /boot/config-$(uname -r)


# 如果内核有它，你将看到
CONFIG_HAVE_ARCH_SECCOMP_FILTER=y
CONFIG_SECCOMP_FILTER=y
CONFIG_SECCOMP=y
```








<br/>
<br/>

---

<br/>
<br/>













# 配置和升级

Setup and Upgrade


<br/>


## 快速入门

Get Started in Two Minutes Guide


- 从ZIP文件安装
- 使用Docker


<br>


### zip文件安装


1. 现在 SonarQube CE
2. 解压
3. 运行
4. 访问

```bash
# 具体位置取决于你的安装位置
/opt/sonarqube/bin/[OS]/sonar.sh console


# localhost:9000（admin/admin）
```


<br/>
<br/>



### Docker安装


在[Docker Hub](https://hub.docker.com/_/sonarqube/)上下载对应CE的镜像，上面有安装和配置的详细信息。



<br/>
<br/>
<br/>



## 安装Server


支持多个数据库引擎，请务必遵守各个数据库引擎的要求。

创建一个空的schema和一个`sonarqube`用户。授予此用户`create, update, delete`此`schema`对象的权限。


```sql
CREATE SCHEMA `sonar` DEFAULT CHARACTER SET utf8 ;

CREATE USER 'sonarqube'@'localhost' IDENTIFIED BY 'sonarqube-PW123';
GRANT ALL ON sonar.* TO 'sonarqube'@'localhost';
```


<br/>


### 安装数据库


<br/>


#### SQL Server

跳过，有需要的请看: <https://docs.sonarqube.org/latest/setup/install-server/>


<br/>
<br/>


#### Oracle

跳过！


<br/>
<br/>


#### PostgreSQL

如果你想使用`custom schema`而不是默认的`public schema`，则必须设置PostgreSQL的`search_path`属性:

```sql
ALTER USER mySonarUser SET search_path to mySonarQubeSchema
```


<br/>
<br/>


#### MySQL

> 注意:
> Data Center Edition(Enterprise)不支持MySQL!
> Data Center Edition: Designed for High Availability


可在MySQL中使用两种众所周知的数据库引擎: **MyISAM**和**InnoDB**。MyISAM是最老的，并且正在逐渐被InnoDB替代。随着质量控制项目数量的增加，InnoDB显然更快，并且使用SonarQube可以更好地扩展。
如果你是SonarQube的早期使用者，你可能有一系列仍在使用MyISAM引擎的表。你应该将所有表的引擎更改为InnoDB。

一旦所有SonarQube表都使用InnoDB引擎，首先要做的是使用`innodb_buffer_pool_size`参数为MySQL实例分配最大的RAM，并为`query_cache_size`参数提供至少`15Mb`。

阅读这篇文档[InnoDB Performance Optimization](https://www.percona.com/blog/2007/11/01/innodb-performance-optimization-basics/)来优化InnoDB。


<br/>
<br/>


### 安装Web Server


首先，检查安装要求；
下载和解压压缩的发行版(不要解压到以数字开头的目录)；
下面变量`SONARQUBE-HOME`指的是解压的路径。


<br/>


#### 设置数据库访问

编辑`$SONARQUBE-HOME/conf/sonar.properties`来配置数据库设置。模板可用于每个受支持的数据库。

```
# Example for MySQL
sonar.jdbc.username=sonarqube
sonar.jdbc.password=sonarqube-PW123
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
```


<br/>


#### 添加JDBC驱动

已提供受支持数据库(Oracle除外)的驱动程序。不要更换提供的驱动程序，它们是唯一受支持的。

对于Oracle，将JDBC驱动复制到`$SONARQUBE-HOME/extensions/jdbc-driver/oracle`。


<br/>
<br/>


#### 配置ElasticSearch存储路径

默认情况下，ES数据存储在`$SONARQUBE-HOME/data`中，但不建议用于生产环境。相反，你应该将数据存储在其它位置，最好是在具有高速I/O的专用卷。除了保持可接受的性能之外，还可简化SonarQube的升级。

编辑`$SONARQUBE-HOME/conf/sonar.properties`来配置以下设置:

```
# 请记得添加读写权限
sonar.path.data=/var/sonarqube/data
sonar.path.temp=/var/sonarqube/temp
```


<br/>
<br/>


#### 启动Web Server

可在`$SONARQUBE-HOME/conf/sonar.properties`配置监听地址和端口等。

```
sonar.web.host=192.0.0.1
sonar.web.port=80
sonar.web.context=/sonarqube
```

```bash
# 启动
bin/sonar.sh start

# 默认admin/admin
```


<br/>
<br/>


#### 调整Web服务器

默认情况下，SonarQube配置为在任何具有简单Java JRE的计算机上运行。

为了更好地性能，生产环境实例要做的第一件事是使用Java JDK并通过在`sonar.web.javaOpts=-server`中设置以下行来激活服务器模式。

```
sonar.web.javaOpts=-server
```

要修改SonarQube使用的Java JVM只需编辑`$SONARQUBE-HOME/conf/wrapper.conf`并更新以下行:

```
wrapper.java.command=/path/to/my/jdk/bin/java
```


<br/>
<br/>


#### FAQ

docs: <https://docs.sonarqube.org/latest/setup/install-server/>



<br/>
<br/>
<br/>



## 配置和操作Server

Configure & Operate the Server


<br/>


### 以SystemD运行

Running SonarQube as a Service on Linux with SystemD


假设如下信息:

- sonarqube用户
- sonarqube组
- java virtual machine安装在`/opt/java/`
- sonarqube解压在`/opt/sonarqube/`

<br>

创建`sonarqube`用户:

```bash
useradd -M -s /sbin/nologin
```



<br>

创建service文件`/etc/systemd/system/sonarqube.service`，具体详情请安装自己的实际情况进行修改。

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=simple
User=sonarqube
Group=sonarqube
PermissionsStartOnly=true
ExecStart=/bin/nohup /opt/java/bin/java -Xms32m -Xmx32m -Djava.net.preferIPv4Stack=true -jar /opt/sonarqube/lib/sonar-application-7.6.jar
StandardOutput=syslog
LimitNOFILE=65536
LimitNPROC=8192
TimeoutStartSec=5
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
# 启动
sudo systemctl enable sonarqube.service
sudo systemctl start sonarqube.service
```


<br/>
<br/>


### 在代理服务器后保护Server

Securing the Server Behind a Proxy


<br/>


#### Server配置

要通过HTTPS运行SonarQube Server，必须构建标准的反向代理服务器。
必须配置反向代理，在每个HTTP Request Header中设置`X_FORWARDED_PROTO: https`值。如果没有此属性，SonarQube Server启动的重定向将回退到HTTP。


<br/>
<br/>


#### 使用Apache代理

跳过！


<br/>
<br/>


#### 使用Nginx代理

```
# the server directive is nginx's virtual host directive
server {
  # port to listen on. Can also be set to an IP:PORT
  listen 80;

  # sets the domain[s] that this vhost server requests for
  server_name www.somecompany.com;

  location / {
    proxy_pass http://sonarhost:sonarport;
  }
}
```


<br/>
<br/>


#### 使用IIS

跳过！




<br/>
<br/>
<br/>



## 安装插件


在SonarQube中安装插件有两种选择:

- **Marketplace**，从SonarQube UI自动地安装插件
- **手动安装**， 如果SonarQube实例无法访问Internet，请使用此方法



<br/>
<br/>
<br/>


## 安装C/C++插件


由于SonarQube的C, C++是商业版才有的功能，所以我使用的CE版就不支持对这两个语言的静态检查。

后来看到 **SonarOpenCommunity**: <https://github.com/SonarOpenCommunity>，它里面有这个插件，先感谢开发者，然后再使用。

sonar-cxx: <https://github.com/SonarOpenCommunity/sonar-cxx>，查看相关说明进行安装和配置。


<br/>


### 安装

- 明白哪个插件版本与当前使用的SonarQube版本监控
- 下载jar插件，将其放置于`$ SONARQUBE_HOME/extensions/plugins`目录下
  - `sonar-cxx-plugin-x.y.z.jar`: c++ plug-in
  - `sonar-c-plugin-x.y.z.jar`: c plug-in
- 重启SonarQube Server
- 在UI上的Marketplace查看更新



<br/>
<br/>
<br/>



## 安装PS/SQL插件


由于SonarQube的PL, SQL是商业版才有的功能，所以我使用的CE版就不支持对这两个语言的静态检查。

后来看到: sonar-plsql: <https://github.com/felipebz/sonar-plsql> 社区开源项目，先感谢开发者，再使用。

安装方法与上面的C/C++一样，下载当前版本支持的插件到对应目录，重启SonarQube Server。



<br/>
<br/>
<br/>



## 将Server安装为集群


docs: <https://docs.sonarqube.org/latest/setup/install-cluster/>

先跳过！



<br/>
<br/>
<br/>



## 配置和操作集群

Configure & Operate a Cluster


docs: <https://docs.sonarqube.org/latest/setup/operate-cluster/>


先跳过！



<br/>
<br/>
<br/>



## 升级

Upgrade the Server


自动处理`non-LTS`版本的升级。但是，如果在迁移路径中有LTS版本，则必须先迁移LTS，然后再迁移到目标版本。

例如，`v5.1` -> `v7.0`，迁移路径为 `v5.1` -> `5.6.7 LTS` -> `v6.7.x LTS` -> `v7.0`。


<br/>


### 如何升级


在开始之前，请备份SnarQube Database。升级问题虽然很少见，但备份确实必须的。













<br/>
<br/>

---

<br/>
<br/>












# 分析源代码

Analyzing Source Code


<br/>


## 概述


一旦安装了SonarQube平台，你就可以安装分析器(analyzer)并开始创建项目了。为此，你必须安装和配置适合你需求的扫描器(scanner)。
Do you build with:

- **Gradle** - [SonarScanner for Gradle](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Gradle)
- **MSBuild** - [SonarScanner for MSBuild](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+MSBuild)
- **Maven** - [use the SonarScanner for Maven](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Maven)
- **Jenkins** - [SonarScanner for Jenkins](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins)
- **Azure DevOps** - [SonarQube Extension for Azure DevOps](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Extension+for+VSTS-TFS)
- **Ant** - [SonarScanner for Ant](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Ant)
- **anything else (CLI)** - [SonarScanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner)

**注意**，不建议在运行SonarQube Scanner Analysis的机器上运行反病毒扫描程序，这可能会导致不可预测的行为。


<br/>


### 分析产生了什么

What does analysis produce?


SonarQube可以对20多种不同的语言进行分析。该分析的结果是 quality measures 和 issues。但是，分析的结果也会因语言而异:

- 在所有语言中，**blame**数据将自动从支持的SCM提供程序导入(自动支持Git和SVN)。其它提供需要额外的插件
- 在所有语言中，执行源代码的静态分析
- 可对某些语言执行编译代码的静态分析
- 可对某些语言执行代码的动态分析


<br/>
<br/>


### 是否会分析所有文件

Will all files be analyzed?


默认情况下，在分析期间，只有语言分析器(language analyzer)可识别的文件才会加载到项目中。


<br/>
<br/>


### 分析期间会发生什么

What happens during analysis?


在分析期间，从Server请求数据，分析提供给分析的文件，并以报告的形式将结果返回到Server，然后在Server-Side异步分析。

分析上报排队并按顺序处理，因此很可能在分析日志显示完成后的短暂时间内，更新的值在SonarQube项目中不可见。但是，你能够分辨出正在发生的事情，因为项目名称右侧的项目主页上会有一个图标。

![](/images/SonarQube/backgroundTaskProcessingInProgress.jpeg)

![](/images/SonarQube/backgroundTaskProcessingFailedIcon.jpeg)



<br/>
<br/>
<br/>



## 分析参数

Analysis Parameters


可以在多个位置设置用于配置项目分析的参数。这是参数的层次结构：

- 在UI里定义的**全局分析参数(Global)**，`Administration > Configuration > General Settings`
- 在UI里定义的**项目分析参数(Project)**，`Project Level > Administration > General Settings`
- 在项目分析配置文件或分析器配置文件中定义的**项目分析参数**
- **分析/命令行参数**，再启动分析时定义，覆盖项目分析参数

注意，只有通过UI设置的参数才会存储在数据库中。


<br/>


### 强制参数

Mandatory Parameters


<br/>


- **Server**

| Key | Description | Default |
| - | - | - |
| `sonar.host.url` | the server URL | `http://localhost:9000` |

<br>

- **Project Configuration**

| Key | Description | Default |
| - | - | - |
| `sonar.projectKey` | The project's unique key. Allowed characters are: letters, numbers, - , _ , . and : , with at least one non-digit. | For Maven projects, this is automatically set to `<groupId>:<artifactId>` |
| `sonar.sources` | Comma-separated paths to directories containing source files. | Read from build system for Maven, Gradle, MSBuild projects |


<br/>
<br/>


### 可选参数

Optional Parameters


- **Project Identity**

| Key | Description | Default |
| - | - | - |
| `sonar.projectName` | 显示在Web实例上的项目名称 | Maven项目的`<name>`，否则为项目密钥。如果DB中已有名称，则不会覆盖该名称 |
| `sonar.projectVersion` | 项目版本 | Maven项目的`<version>`，否则未提供 |

<br>

- **Authentication**

| Key | Description | Default |
| - | - | - |
| `sonar.login` | 具有项目执行分析权限的SonarQube用户的登录或身份验证Token | xxx |
| `sonar.password` | 与`sonar.login`用户名一起使用的密码。如果正在使用身份验Token，则应将此项留空 | xxx |

<br>

- **Web Services**

| Key | Description | Default |
| - | - | - |
| `sonar.ws.timeout` | 等待Web服务调用响应的最长时间（秒）。只有在等待服务器响应Web服务调用时在分析期间遇到超时时，才能从默认值修改此值。 | 60 |

<br>

- **Project Configuration**

| Key | Description | Default |
| - | - | - |
| `sonar.projectDescription` | 项目描述。与Maven不兼容 | `<description`用于Maven项目 |
| `sonar.links.homepage` | 项目主页，与Maven不兼容 | `<url>`用于Maven项目 |
| `sonar.links.ci` | CI，与Maven不兼容 | `<ciManagement><url>`用于Maven项目 |
| `sonar.links.issue` | Issue tracker，与Maven不兼容 | `<issueManagement><url>`用于Maven项目 |
| `sonar.links.scm` | 项目原仓库，与Maven不兼容 | `<scm><url>`用于Maven项目 |
| `sonar.links.scm_dev` | 开发者连接，与Maven不兼容 | `<scm><developerConnection>`用于Maven项目 |
| `sonar.tests` | 包含测试的目录的逗号分隔路径,与Maven不兼容 | Maven项目的默认测试位置 |
| `sonar.sourceEncoding` | 源文件编码 | 系统编码 |
| `sonar.externalIssuesReportPaths` | 以逗号分隔的通用Issue上报路径列表 |
| `sonar.projectDate` | 为分析指定日期(yyyy-MM-dd) | 当前日志 |
| `sonar.projectBaseDir` | 当您需要在除启动它之外的目录中进行分析时，请使用此属性 | xxx |
| `sonar.working.directory` | 设置使用SonarScanner或SonarScanner for Ant（版本大于2.0）触发的分析的工作目录 | `.sonar` |
| `sonar.scm.provider` | 此属性可用于明确告知SonarQube应使用哪个SCM插件来获取项目上的SCM数据 | xxx |
| `sonar.scm.forceReloadAll` | 默认情况下，仅检索已更改文件的blame信息。将此属性设置为true可加载所有文件的blame信息 | xxx |
| `sonar.coverage.jacoco.xmlReportPaths` | 导入以XML文件形式提供的JaCoCo代码覆盖率报告。此属性接受多个逗号分隔的条目。必须在分析之前生成JaCoCo XML报告 | `target/site/jacoco/jacoco.xml` <br> `build/reports/jacoco/test/jacocoTestReport.xml` |

<br>

- **Duplications**

| Key | Description | Default |
| - | - | - |
| `sonar.cpd.exclusions` | 要从复制检测中排除的以逗号分隔的文件路径模式列表 | xxx |
| `sonar.cpd.${language}.minimumtokens` | xxx | 100 |
| `sonar.cpd.${language}.minimumLines` | 如上 | 10 |

<br>

- **Analysis Logging**

| Key | Description | Default |
| - | - | - |
| `sonar.log.level` | 控制分析期间生成的日志级别 | INFO |
| `sonar.verbose` | 向客户端和服务器端分析日志添加更多详细信息 | false |
| `sonar.showProfiling` | 显示日志以查看分析仪花费时间的位置 | false |
| `sonar.scanner.dumpToFile` | 将指向文件的完整属性列表输出到扫描程序API，作为调试分析的方法 | xxx |
| `sonar.scanner.metadataFilePath` | 设置扫描程序写入report-task.txt文件的位置，该文件包含ceTaskId等 | `sonar.working.directory`的值 |



<br/>
<br/>
<br/>



## 后台任务

Background Tasks


一个后台任务可以是:

- 导入一个分析报告
- the computation of a Portfolio
- 导入或导出一个项目


<br/>


###　扫描程序完成分析后会发生什么

What happens after the scanner is done analyzing?


在相关后台任务完成之前，分析尚未完成。即使SonarScanner的日志显示执行完成，在完成后台任务之前，分析结果在SonarQube项目中将不可见。在SonarScanner外出分析代码后，分析结果(Sources, Issues, Metrics) - 分析报告 - 将发送到SonarQube Server，一共计算引擎进行最终处理。分析报告按顺序排队和处理。

在项目级别，当有待处理的分析报告等待消耗时，标题中的**Pending（待处理）**通知将在最近完成的分析的日期旁。

全局管理员可在` Administration > Projects > Background Tasks`查看当前队列；项目管理员可在`Administration > Background Tasks`查看相关任务。


<br/>
<br/>


### 如何知道分析报告处理失败的时间

How do I know when analysis report processing fails?


后台任务通常会成功，但有时候异常会导致处理失败。例如:

- 处理大项目是内存不足(OOM)
- 现有模块或项目的密钥与报告中的密钥冲突
- ...

当发生这种情况时，失败的状态会反映在项目主页上，但这需要有人注意到它。你还可以选择在后台任务失败时通过电子邮件接收通知(Notifications)——无论是逐个还是全局。


<br/>
<br/>


### 如何诊断失败的后台任务

How do I diagnose a failing background task?


对于没法分析报告，都有一个下拉菜单，允许你访问**扫描程序上下文(Scanner Context)**，显示代码扫描是扫描程序的配置。
如果任务处理失败，则可使用其它选项**显示错误详细信息(Show Error Details)**，以获取处理后台任务失败的详情。


<br/>
<br/>


### 如何取消待处理的分析报告

How do I cancel a pending analysis report?


管理员可通过单击取消处理待处理任务(pending task)，一旦报告开始处理，取消它就为时已晚。



<br/>
<br/>
<br/>



## 通用问题数据

Generic Issue Data


SonarQube支持通用导入格式，用于在代码中引发*external* issues。它旨在允许你从你喜欢的*linter*导入issues，即使它不存在插件。

外部问题受到两个重要限制:

- 它们无法在SonarQube内管理
- 在SonarQube中无法管理引发这些问题的规则的激活


<br/>


### Import


分析参数`sonar.externalIssueReportPaths`接受以逗号分隔的报告路径列表。
每个报告必须在顶层(top-level)包含一个名为issues对象的问题对象数组。

**Issue字段:**

- `engineId` - string
- `ruleId` - string
- `primaryLocation` - Location object
- `type` - string. One of BUG, VULNERABILITY, CODE_SMELL
- `severity` - string. One of BLOCKER, CRITICAL, MAJOR, MINOR, INFO
- `effortMinutes` - integer, optional. Defaults to 0
- `secondaryLocations` - array of Location objects, optional

<br>

**Location字段:**

- `message` - string
- `filePath` - string
- `textRange` - TextRange object, optional for secondary locations only

<br>

**TextRange字段:**

- `startLine` - integer. 1-indexed
- `endLine` - integer, optional. 1-indexed
- `startColumn` - integer, optional. 0-indexed
- `endColumn` - integer, optional. 0-indexed


<br/>
<br/>


### 栗子


以下是预期格式的栗子:

```
{ "issues": [
    {
      "engineId": "test",
      "ruleId": "rule1",
      "severity":"BLOCKER",
      "type":"CODE_SMELL",
      "primaryLocation": {
        "message": "fully-fleshed issue",
        "filePath": "sources/A.java",
        "textRange": {
          "startLine": 30,
          "endLine": 30,
          "startColumn": 9,
          "endColumn": 14
        }
      },
      "effortMinutes": 90,
      "secondaryLocations": [
        {
          "message": "cross-file 2ndary location",
          "filePath": "sources/B.java",
          "textRange": {
            "startLine": 10,
            "endLine": 10,
            "startColumn": 6,
            "endColumn": 38
          }
        }
      ]
    },
    {
      "engineId": "test",
      "ruleId": "rule2",
      "severity": "INFO",
      "type": "BUG",
      "primaryLocation": {
        "message": "minimal issue raised at file level",
        "filePath": "sources/Measure.java"
      }
    }
]}
```



<br/>
<br/>
<br/>



## 通用测试数据

Generic Test Data


开箱即用，SonarQube支持用于测试覆盖和测试执行导入的通用格式。如果你的语言不插件不支持你的Coverage引擎的本机输出格式，只需将它们转换为这些格式即可。


<br/>


### Generic Coverage


报告路径应该以逗号分隔的列表传递给: `sonar.coverageReportPaths`

支持的格式由`sonar-generic-coverage.xsd`进行描述:

```
<xs:schema>
  <xs:element name="coverage">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="file" minOccurs="0" maxOccurs="unbounded">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="lineToCover" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:attribute name="lineNumber" type="xs:positiveInteger" use="required"/>
                  <xs:attribute name="covered" type="xs:boolean" use="required"/>
                  <xs:attribute name="branchesToCover" type="xs:nonNegativeInteger"/>
                  <xs:attribute name="coveredBranches" type="xs:nonNegativeInteger"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          <xs:attribute name="path" type="xs:string" use="required"/>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="version" type="xs:positiveInteger" use="required"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

看起来像这样:

```
<coverage version="1">
  <file path="xources/hello/NoConditions.xoo">
    <lineToCover lineNumber="6" covered="true"/>
    <lineToCover lineNumber="7" covered="false"/>
  </file>
  <file path="xources/hello/WithConditions.xoo">
    <lineToCover lineNumber="3" covered="true" branchesToCover="2" coveredBranches="1"/>
  </file>
</coverage>
```

根节点应该命名为`coverage`，其`version`属性应设置为1。

为每个文件插入一个可由测试覆盖的文件元素。其`path`属性可以是绝对的，也可是相对的。它具有以下属性:

- `lineNumber`(强制性)
- `covered`(强制性) - 布尔值，指示测试是否实际命中改行
- `branchesToCover`(可选) - 可覆盖的分支数量
- `coveredBranches`(可选) - 实际有测试覆盖的分支数量


<br/>
<br/>


### Generic Execution


报告路径应以逗号分隔的列表传递给: `sonar.testExecutionReportPaths`

支持的格式如下:

```
<testExecutions version="1">
  <file path="testx/ClassOneTest.xoo">
    <testCase name="test1" duration="5"/>
    <testCase name="test2" duration="500">
      <skipped message="short message">other</skipped>
    </testCase>
    <testCase name="test3" duration="100">
      <failure message="short">stacktrace</failure>
    </testCase>
    <testCase name="test4" duration="500">
      <error message="short">stacktrace</error>
    </testCase>
  </file>
</testExecutions>
```

根节点应该被命名为`testExecutions`，它的`version`属性应该被设置成1。
为每个测试文件插入一个文件元素，其`path`属性可以是绝对的，也可是相对于模块的根。

注意，与覆盖率报告不同，报告中的文件必须是测试文件名，而不是测试所涵盖的源代码文件。

在`file`元素内，通过单元测试为每个测试运行插入一个`testCase`。它具有以下属性/子项:

- `testCase`（强制性）
  - `name`（强制性）: 测试事例的名称
  - `duration`(强制性): long value，ms为单位
  - `failure|error|skipped`(可选): 如果测试不正确，请使用消息和长描述报告原因
  - `message`(强制): 描述原因的短消息
  - `stacktrace`（可选）: 包含有关失败、错误、跳过状态的详细信息



<br/>
<br/>
<br/>



## PR分析

Pull Request Analysis


PR分析是作为Developer Edtion的一部分提供。它允许你:

- 在SonarQube UI中查看你的PR分析结果并查看状态以显示存在未解决的问题
- 在你的SCM提供商界面中使用SonarQube issue自动装饰你的PR

从项目的**branch and pull request**的下拉菜单中可以在SonarQube中看到PR。启用PR装饰后，SonarQube会在PR上发布分析状态。



<br/>
<br/>
<br/>



## SCM集成


在代码分期期间收集SCM数据可以解锁许多SonarQube功能:

- 自动Issue分配
- 代码查看器中查看代码注释
- SCM-driver的新代码检测，没有SCM数据，SonarQube使用分析日期确定新代码

SCM集成需要你的SCM提供商，默认情况下支持SVN和Git。其它提供商，请参阅Marketplace。
如果需要，你可以通过管理设置将其在全局/项目级别将其关闭。


<br/>


### Git



<br/>
<br/>


### SVN



















<br/>
<br/>

---

<br/>
<br/>













# Branches


分支分析作为Developer Editon的一部分提供。分支分析允许你:

- 分析 long-lived branches
- 分析 short-lived branches
- 在短期分支的状态受到影响时通知外部系统


<br/>

由于分支功能是开发版(也就是付费版)功能，因此社区版只能对每个分支创建一个项目。

例如:

```
repo: zhang-repo

branch:
  - master
  - test
  - zhang

projects:
  - zhang-repo-master
  - zhang-repo-test
  - zhang-repo-zhang
```











<br/>
<br/>

---

<br/>
<br/>






















# 用户指南

User Guide


<br/>


## 修复漏水

Fixing the Water Leak


<br/>


### 什么是漏水

What is the Water Leak


想象一下，有一天你回到家发现厨房地板上有一滩水，水慢慢变大。
你想去拿拖把？还是找到漏水源头并修复它？选择很明显，你得修复它。

那么为什么与代码质量(code quality)有什么不同呢？当你使用SonarQube分析应用程序并意识到它有很多技术债务(technical debt)，这种下意识的反应通常是开始修复-这样那样，要么整理一个补救计划。这就像每天拖地一次而却忽略了漏水源头一样。

<br>

通常在这种传统方法中，在发布版本之前，定期进行代码质量(code quality)审计结果是开发人员在发布之前应该采取的行动。这种方法可能在短期内有效，特别是在强有力的管理支持下，但在中长期内始终失败，因为:

- 代码审查(code review)过程太迟，没有利益相关者热衷于解决问题，每个人都希望新版本发布
- 开发者通常会推迟不了解项目上下文的外部团队提出的建议。顺便提一下，正在审查的代码已经过时了
- 使用这种方法明显缺乏对代码质量的所有权。谁拥有质量审查权限？没有人
- 在整个应用程序投入生产之前，需要检查整个应用程序，显然不可能对所有应用程序使用相同的标准。每个项目都会进行谈判，这将耗尽整个过程的可信度

<br>

相反，为什么不将你在家中使用的相同的简单逻辑应用于管理代码质量的方式？修复泄露(leak)意味着将重点放在**新代码**上，即自上次发布以来添加或更改的代码。然后事情就变得很容易了:

- Quality Gate可以每天运行，并且可通过它。发版时没有任何意外
- 开发人员很难回避他们前一天介绍的问题。相反，他们通常很乐意在代码仍然新鲜时修复问题
- 代码质量有明确的所有权
- 做不做的标准在不同的应用程序中是一致的，并且在团队之间共享
- 成本微不足道，因为它是开发过程中的一部分

最为奖励，变化最大的代码具有最高的可维护性，并且未变更的代码具有最低的维护性，这很有意义。


<br/>
<br/>


### 怎么做


SonarQube提供两种主要工具来帮助你找到泄漏点:

- 新代码指标(metrics)显示当前代码与你在其历史记录(`previous_version`)中选择的特定点之间的度量差异
- 新代码主要基于SCM blame 数据监测，从新代码期(泄漏期)的第一次分析开始，需要时使用回退机制
- Quality Gates允许你设置测量代码的布尔阈值。将它们与差异指标一起使用，可确保你的代码质量随着时间的推移在正确的方向上行驶



<br/>
<br/>
<br/>



## 项目页

Project Page


项目主页(Project Homepage)是任何项目的切入点，它显示:

- the releasability status of the project
- the current state of its quality
- the quality of what has been produced since the beginning of its New Code Period

项目页面回答了两个问题:

- can I release my project today?
- if not, what should I improve to make the project pass the Quality Gate?


<br/>


### 今天能发版吗

Can I release today?


由于 Quality Gate 是你执行质量策略的最强大的工具，因此该页面以项目的当前质量门状态开始。如果项目通过，则会显示一个简单的绿色全清除。

如果没有，可立即获得详细信息和drill-downs，以便快速识别出错的地方，每个错误条件的一个部分显示当前项目值是什么以及它应该是什么。像往常一样，你可以点击当前值来进行深入分析。


<br/>
<br/>


### 应该优先解决什么

What should I fix first?


因为提高项目质量的最佳方法是在问题变得根深蒂固之前捕获并修复新问题，项目的第一个视图以新代码周期为中心，在项目主页右侧以黄色突出显示。项目空间页面显示关键指标的高级摘要，包括当前值和新代码周期值。

在Quality Gate信息的下方，可以获得可靠性和安全域中的旧问题和新问题的数量。然后是可维护性域。单击页面上的任何图形将转到“详细信息”页面或“问题”页面中的详细视图。

开发人员必须做的最重要的事情是确保屏幕黄色部分的新问题得到确认，审核和修复，并确保测试涵盖新代码以防止将来出现回归。无论过去引入了多少问题，或者总体上测试覆盖范围有多少，关注新增问题将确保情况不会降低您之前在生产中发布的版本。

那么，您应该先找到哪些问题：错误，漏洞或代码异味？这取决于，因为答案取决于您的问题的性质。假设你有一个重复5次的代码块问题，在这个重复的代码块中，你有3个Bug和5个安全问题。最好的方法可能是首先修复重复，然后解决新集中位置的错误和漏洞，而不是修复它们5次。
这就是为什么您需要在开始解决之前检查新问题。



<br/>
<br/>
<br/>


## Application


Applications are available as part of the Enterprise Edition.












<br/>
<br/>
<br/>



## Portfolios


Portfolios are available as part of the Enterprise Edition.







<br/>
<br/>
<br/>













## Issues


在运行分析时，每当一段代码破坏编码规则时，SonarQube就会引发一个issue。编码规则(coding rules)是通过每种语言的相关质量配置文件定义的。

每个问题有五种严重程度:

- **BLOCKER** - 很有可能影响生产中应用程序行为的错误。必须立即修复
- **CRITICAL** - 要么是在生产环境中影响应用程序行为可能性很小的bug，要么是代表安全漏洞的问题。必须立即检查代码
- **MAJOR** - 可能严重影响开发人员生产力的质量缺陷
- **MINOR** - 会轻微影响开发人员生产力产生的质量缺陷
- **INFO** - 既不是错误，也不是质量缺陷，只是一个提示


<br/>


### 理解issue上下文

Understanding issue context


有时，一旦指出问题，问题就不言而喻了。例如，你的团队已约定了变量命名规则，在某个变量名出线问题时，你不需要理解大量上下文来理解该问题。但在其它情况下，上下文可能对理解为什么会出现这个问题至关重要。这就是为什么SonarQube不仅支持显示问题消息的主要问题位置，还支持次要问题位置。

但有时候，贡献位置地点并不足以理解问题。例如，当通过代码在某些路径上取消引用空指针时，您真正需要的是问题流。每个流程都是一组辅助位置，用于显示可能发生问题的代码的确切路径。


<br/>
<br/>


### 生命周期

Lifecycle of Code Smell, Bug, and Vulnerability Issues


<br/>


#### 状态

Status

创建之后，Issue会在生命周期中流动，可能为以下五种状态之一:

- **打开(Open)** - 由SonarQube在新问题上设定
- **确认(Confirmed)** - 手动确认以指示问题有效
- **解决(Resolved)** - 手动设置以指示下一个分析应该关闭改问题
- **重开(Reopened)** - 当一个已解决的问题实际上没有得到纠正时，SonarQube会自动设置
- **关闭(Closed)** - 有SonarQube自动设置自动创建的问题


<br/>
<br/>


#### 处理方式

Resolutions


已关闭的问题将有一下两种方式之一:

- **已修复(Fixed)** - 当后续分析显示问题已更正或文件不再可用时自动设置
- **已移除(Removed)** - 当相关规则不再可用时自动设置。改规则可能无法使用，因为它已从质量配置文件中删除，或者因为已卸载基础插件

Resolved issues好友两个处理方式:

- **误判(False Positive)** - 手动设置
- **不会修复(Won't Fix)** - 不会修复


<br/>
<br/>


#### 问题工作流程

Issue Workflow

在以下情况下，问题会自动关闭(Status: Closed):

- 问题以正确修复（Resolution: Fixed）
- 问题不再存在，因为相关编码规则已停用或不再可用(Resolution: Removed)

在以下情况下，问题会自动重新打开(Status: Reopened):

- 手动修改解决方式为已修复(但是不是误判)的问题，有后续分析显示仍然存在



<br/>
<br/>



### 安全热点问题的生命周期

Lifecycle of Security Hotspot Issues


安全热点问题具有专用的生命周期。它们不被视为可操作，必须由具有相关权限的用户进行审核。

创建之后，安全热点问题将流经专用的生命周期，可能是以下四种状态之一:

- **Open** - 由SonarQube在新问题上自动设置
- **Resolved**(Won't Fix) - 当安全审核员接受开发人员针对手动漏洞所做的修复或安全审核员清楚打开的热点或手动漏洞时，SonarQube会自动设置
- **To Revied** - 当开发人员请求安全审核员查看他对手动漏洞所做的修复时自动设置
- **Reopened** - 当开发人员解除打开的手动漏洞或安全审计员手动重新打开问题以便对已解决的问题运行新审计时设置

如果删除了包含安全热点的代码，则只会关闭安全热点问题。如果从项目的质量配置文件中删除了标识热点的规则，则安全热点也可能会被删除。



<br/>
<br/>



### 理解哪些问题是新的

Understanding which Issues are "New"


为了确定问题的创建日期，在每次分析期间执行算法已确定问题是新的还是之前存在的。此算法依赖于报告问题的行的内容的哈希值(不包括空格)。对于多行问题，使用第一行的哈希值。对于每个文件(在检测到文件重命名后)，算法将从先前的分析中获取问题的基本列表，并尝试将这些问题与新分析报告的原始问题列表进行匹配。该算法尝试使用最强的证据进行首次匹配，然后再回到较弱的启发式算法。

- 如果问题是在同一规则上，具有相同的行号和相同的行哈希 - 匹配
- 检测到块在文件内移动，然后如果问题出在同一行(移动的)和同一条规则上- 匹配
- 在相同的规则上，使用相同的消息并使用相同的行哈希 - 匹配
- 在相同的规则上，使用相同的消息并使用相同的行号 - 匹配
- 在相同的规则上，使用相同的行哈希 - 匹配
- 是否有匹配CLOSED的问题 - 匹配和重新打开


<br/>
<br/>


### 了解问题回溯

Understanding Issue Backdating


一旦问题被确定为新，下一个问题便是提供它的日期。例如，如果它已经在代码中存在了很长时间，但只能在最近的分析中找到，因为新的规则被添加到配置文件中？该问题是否应该在其行的最后一次更改日期或首次提出的分析日期之间给出？那就是它应该回溯吗？

如果最后一次更改改行的日期可用，那么在某些情况下，该问题将被回溯:

- 首先分析项目或分支
- 当配置文件中的规则为新时
- 当分析程序升级后
- 当规则是外部的

因此，回溯可能会使新提出的问题原理New Code Period。



<br/>
<br/>



### 自动问题分配

Automatic Issue Assignment


- For Bug, Vulnerability and Code Smell
- For Security Hotspot
- User Correlation
- Known Limitation



<br/>
<br/>



### 问题编辑

Issue edits


SonarQube的问题工作流程可帮助你管理问题。你可对一个Issue做七件不同事情，这些行为可分为三类:

- Technical Review
  - Confirm
  - False Positive
  - Won't Fix
  - Severity change
  - Resolve
- Security Hotspots
  - Detect
  - Clear
  - Request Review
  - Reject
- Dispositioning
- General
  - Comments
  - Tag
- Bulk Change



<br/>
<br/>



### 清除已解决的问题

Purging Closed Issues


默认情况下，已关闭的问题将保留30天。当然，你也可以修改它。




<br/>
<br/>
<br/>



## Rules

SonarSource Rules: <https://rules.sonarsource.com/>


<br/>


### 概述


在SonarQube中，分析程序提供在源代码上执行的规则来生成问题。有四种类型的规则:

- Code Smell (Maintainability domain)
- Bug (Reliability domain)
- Vulnerability (Security domain)
- Security Hotspot (Security domain)


<br/>
<br/>


### 规则


默认情况下，点击带单栏**Rules**时，你将看到SonarQube实例上安装的分析程序带来的所有可用规则。你可根据以下条件缩小范围:

- Language
- Type
- Tag
- Repository
- Default Severity
- Status
- Available Since
- Template: 显示允许创建自定义规则的规则模板
- Quality Profile



<br/>
<br/>



### 规则细节


要查看规则的详细信息，请点击它。除了基本规则数据之外，您还可以查看其中活动的配置文件（如果有）以及已经引发了多少未解决的问题。
只有拥有正确的权限时，才能使用以下两个操作:

- Add/Remove Tags
- Extend Description



<br/>
<br/>



### 规则模板和自定义规则

Rule Templates and Custom Rules


规则模板(Rule templates)由创建提供，允许用户在SonarQube中定义自己的规则。它位于`Rules -> Template`。

要从模板创建自定义规则，你必须填写一下信息:

- Name
- Key (auto-suggested)
- Description (Markdown format is supported)
- Default Severity
- Status
- The parameters specified by the template



<br/>
<br/>


### 扩展编码规则

Extending Coding Rules


可以添加[自定义编码规则](https://docs.sonarqube.org/display/DEV/Adding+Coding+Rules)。



<br/>
<br/>



### 规则类型和严重性

Rule Types and Severities


Type:

- Bug
- Vulnerability
- Code Smell
- Security Hotspot


Severity:

- Blocker
- Critical
- Major
- Minor
- Info




<br/>
<br/>


## 安全相关的规则

Security-related Rules


SonarQube质量类型有三种不同的规则:

- Reliability (bug)
- Vulnerability (security)
- Maintainability (code smell)

但另外一种方式，只有两种类型:

- security rule
- 其它

两者之间的区别并不在它们捕获的内容，而在于她们来自何处以及强加于它们的标准。


<br/>


### 从安全相关的规则的期望是什么

What to expect from security-related rules


需要明确的是，SonarQube语言插件中实现的大多数规则的标准是非常严格: 没有误报。对于正常规则，你应该能够确信任何报告给你的问题确实是一个问题。

但对于与安全相关的规则，情况略有不同。例如，许多安全指南讨论了应如何处理*敏感数据*。但是，由于规则中不可能确定哪些数据是敏感，哪些是不敏感。因此选择变为： 保持无误判标准并且不实施与安全相关的规则，或者实施与安全的规则不同的标准。

这就是为什么与安全相关的规则很广泛。官方的想法是，该规则将标记任何可疑的内容，并将其留给安全审核人员来剔除误报并发送真正的问题进行补救。

安全热点是一种特殊类型的问题，用于识别安全审核人员应审核的敏感区域，以确定它们是否真的是漏洞。有关热点和审计过程的详细信息，请参阅安全审核和报告。



<br/>
<br/>



### 与安全相关的规则来自何方

Where security-related rules come from


绝大多数与安全相关的规则源于既定标准:

- **CWE(Common Weakness Enumeration)**：是美国MITRE机构提出的一套语言标准，用于描述软件安全弱点的通用化描述语言。每个CWE条目都包含了CWE标识符/弱点类型名称、类型的描述、弱点的行为、弱点的利用方法、利用弱点的可能性、可能导致的后果、应对措施、代码示例、对应的CVE漏洞数量、参考信息等内容。
- **SANS Top 25** - [CWE/SANS TOP 25 Most Dangerous Software Errors](https://www.sans.org/top25-software-errors/)
- **OWASP Top 10** - [OWASP Top 10 Application Security Risks](https://www.owasp.org/index.php/Top_10-2017_Top_10)

要查找与任何这些标准相关的规则，你可以按标签或文本搜索规则。


<br/>


#### CWE

CWE标准代表Common Weakness Enumeration:

Common Weakness Enumeration (CWE™) 是一个常见软件弱点的正式列表或字典，可能出现在软件的体系结构、设计代码或实现中。可能导致可利用的安全漏洞。创建CWE是为了描述软件安全漏洞的通用语言，作为针对这些弱点的软件安全工具的衡量标准；并为弱点识别、缓解和预防工作提供共同的基线标准。
CWE是弱化的描述的层次结构。层次结构中的最低级别是弱点基础(Weakness Base)，它描述了细腻度的弱点。

符合特定要求的工具可以认证为CWE兼容。这些要求是:

- 您必须能够使用CWE标识符搜索与CWE相关的规则。要在SonarQube平台中执行此操作，只需将CWE标识符（例如CWE-595）放在规则页面上的搜索文本输入中并运行搜索
- 规则必须与其相关的CWE项目准确链接。要查看SonarQube规则的CWE映射，请参阅规则说明底部的规则参见部分
- 您必须能够从问题中识别相关的CWE。要在SonarQube平台中执行此操作，请参阅相关规则
- 产品文档必须包含CWE和CWE兼容性的说明
- 除了通过CWE id搜索规则外，您还可以通过 cwe rule tag 进行搜索


<br/>
<br/>


#### SANS TOP 25

SANS Top 25列表是由SANS组织编制的CWE中列出的25个最危险错误的集合。当前的SANS列表分为三类：

- Insecure Interaction Between Components
- Risky Resource Management
- Porous Defenses

要查找与SANS Top 25相关的规则，您可以对类别或相关CWE项目执行文本搜索，或执行规则标记搜索。


<br/>
<br/>


#### OWASP Top 10

OWASP代表Open Web Application Security Project。它是:

`501(c)(3) `全球非营利慈善组织，致力于提高软件的安全性。我们的使命是使软件安全可见，以便全世界的个人和组织能够就真正的软件安全风险做出明智的决策。

OWASP Top 10列出了各种各样的弱点，每个弱点都可以映射到许多单独的规则。
OWASP TOP 10在SonarQube中也对应相关的tag。

要查找与OWASP Top 10相关的规则，您可以对类别执行文本搜索，或执行规则标记搜索。




<br/>
<br/>
<br/>



## 內建规则和标签

Built-in Rule Tags


标签(tag) 是一种对问题(issue)和规则(rule)进行分类的方法。问题会继承引发它们的规则上的标记。有些标签适用于特定语言，但是更多的标签出现在各种语言中。用户可以为规则和问题添加标签。但大多数规则都有一些开箱即用的标签。
以下是一些非全面的、包含一些內建标签:

- `brain-overload` - 一次有太多的东西要留在脑海里
- `bad-practice` - 代码可能按设计工作，但它的设计方式被广泛认为是一个坏主意
- `cert` - 设计CERT标准中的规则
- `clumsy` - 用于完成可以更清晰和简洁地完成的事情的额外步骤
- `confusing` - 将使维护者更长时间地理解，而不是代码实际所做的事情
- `convention` - 编码约定，如格式化、命名、空格...
- `cwe` - CWE安全规则
- `design` - 代码设计存在一些问题
- `lock-in` - 使用特定于环境的功能
- `misra` - MISRA标准相关的规则
- `owasp ` - 与OWASP TOP 10安全标准相关的规则
- `pitfall` - 没有什么不对，但未来可能出现问题;已经为下一个人设置了一个陷阱，他可能会陷入其中并搞砸了代码
- `sans-top25` - 与SANS Top 25 Coding Errors安全相关
- `suspicious` - 它不能保证这是一个bug，但它看起来很可疑。至少，代码应该重新检查并且可能为了清晰而重构
- `unpredictable` - 代码可以在当前条件下正常工作，但如果条件发生变化可能会失败
- `unused` - 未使用的代码
- `user-experience` - 代码在技术上没有任何问题，但它可能会使您的部分或全部用户讨厌您




<br/>
<br/>
<br/>



## Quality Gates


<br/>


### 概述

质量阈(Quality Gates)是你在组织中实施质量策略的最佳方式。它可以回答一个问题: 我今天可以将项目发上线吗？
为了回答这个问题，你可以根据测量项目的度量阈值定义一组布尔条件，例如:

- No new blocker issues
- Code coverage on new code greater than 80%
- ...

理想状况下，所有项目都将通过同一质量阈进行验证。但这并不总是实用的。例如，你可能会发现:

- 技术实现因应用程序而异
- 您希望确保对某些应用程序有更强的要求
- ...

这就是为什么你可以根据需要自定义质量阈，它就在顶部的菜单栏上。



<br/>
<br/>



### 最佳质量阈配置

Use the Best Quality Gate Configuration


质量阈默认激活并视为內建和只读的`Sonar war`方式，由SonarQube提供。它代表了我们对实施修复泄露。根据SonarQube的功能自动调整

有三个指标允许你强制执行给定的可靠性，安全性和可维护性的评级。不仅仅是整体而且还有新代码。建议使用这些指标，并将其作为默认质量阈的一部分，以便开发人员在项目页面上查看质量阈时更清楚的反馈。

不要忘记质量阈条件必须使用差值，检查绝对值是没有意义的(如: 代码行数大于1000)。


<br/>


**推荐的质量阈(Recommended Quality Gate)**

內建的`Sonar way`质量阈都推荐用于大多数项目。如果专注于保持新代码清洁，而不是花费大量时间来修复旧代码。它开箱即用，已被设置为默认配置文件。



<br/>
<br/>



### 质量阈状态

Quality Gate Status


![](/images/SonarQube/quality-gate-status.jpeg)



<br/>
<br/>



### 当质量阈失败时获得通知

Getting Notified When a Quality Gate Fails


使用通知机制，在质量阈失败时通知用户。为此，请订阅**New quality gate status**通知。



<br/>
<br/>




### 安全

Security


任何用户(甚至是匿名用户)都可以访问质量阈。
要就行更改(create, edit, delete)，必须授予用户管理质量阈的权限。
项目管理员可选择与他们项目相关的质量阈。



<br/>
<br/>



### 定义质量阈

Defining Quality Gates


要管理质量阈，请转到菜单栏的**Quality Gates**。

每个质量阈条件都是以下组合:

- 测量(measure)
- 比较符(comparison operator)
- 错误值(error value)

栗子，条件可能是:

- measure: `Blocker issue`
- comparison operator: `>`
- error value: `0`




<br/>
<br/>
<br/>




## 指标

Metric Definitions

项目有如下指标:

- 复杂度(Complexity)
- 重复(Duplications)
- 问题(Issues)
- 可维护性(Maintainability)
- 质量阈(Quality Gates)
- 可靠性(Reliability)
- 安全性(Security)
- 大小(Size)
- 测试(Tests)


<br/>


### 复杂度


应用的控制流是简单还是复杂。


<br/>


#### 圈复杂度

Cyclomatic Complexity

可以计算出达到全面覆盖需要的最少测试用例。
它是基于通过代码的路径数计算的，每当函数的控制流分裂时，复杂度计数器就会增加1。每个函数的最小复杂度为1.此计算因语言而异，因为关键字和功能有所不同。

<br>

**特定语言的详细信息:**

| Language | Notes |
| - | - |
| ABAP | 这些关键字将使复杂度加一: `AND , CATCH , CONTINUE , DO , ELSEIF , IF , LOOP , LOOPAT , OR , PROVIDE , SELECT…ENDSELECT , TRY , WHEN , WHILE` |
| C/C++/Objective-C | 复杂度加一: `function definitions, while , do while , for , throw statements, switch , case , default , && operator, || operator, ? ternary operator, catch , break , continue , goto` |
| COBOL | 复杂度加一: `ALSO , ALTER , AND , DEPENDING , END_OF_PAGE , ENTRY , EOP , EXCEPTION , EXIT , GOBACK , CONTINUE , IF , INVALID , OR , OVERFLOW , SIZE , STOP , TIMES , UNTIL , USE , VARYING , WHEN , EXEC CICS HANDLE , EXEC CICS LINK , EXEC CICS XCTL , EXEC CICS RETURN` |
| Java | 复杂度加一: `if , for , while , case , catch , throw , && , || , ?` |
| JS, PHP | 复杂度加一: `function, if, &&, ||, loop, switch case, throw, catch, go to` |
| PL/I | 复杂度加一: `PROC , PROCEDURE , GOTO , GO TO , DO , IF , WHEN , | , ! , |= , != , & , &=` |
| PL/SQL | 复杂度加一: create procedure, create trigger, procedure definition, basic loop statement, when clause statement, continue statement,exit statement, for loop statement, forall statement, if statement, elsif clause, raise statement, return statement, while loop statement, and expression, or expression, when clause expression |
| VB.NET | 复杂度加一: `method or constructor declaration,  AndAlso , Case , Continue , End , Error , Exit , If , Loop , On Error , GoTo , OrElse , Resume , Stop , Throw , Try` |


<br/>
<br/>


#### 认知复杂度

Cognitive Complexity

对应这个应用是否很难被理解，理解代码的控制流程有多难。



<br/>
<br/>



### 重复


有:

- 重复的块(Duplicated blocks)
- 重复的行(Duplicated lines)
- 重读文件(Duplicated files)
- 密度/重复行%(Duplicated lines %)


<br/>


#### 重复的块

重复的行的块数。

<br>

**特定语言的详细信息**

非Java项目:

- There should be at least 100 successive and duplicated tokens.
- Those tokens should be spread at least on:
  - 30 lines of code for COBOL
  - 20 lines of code for ABAP
  - 10 lines of code for other languages

Java项目:

There should be at least 10 successive and duplicated statements whatever the number of tokens and lines.检测重复时忽略缩进和字符串文字的差异。



<br/>
<br/>



### 问题


有:

- 新问题(New issues)
- 新的严重问题(New xxx issues)
- 所有问题(Issues)
- 严重问题(xxx issues)
- 误判问题(False positive issues)
- 开启问题(Open issues)
- 确认问题(Confirmed issues)
- 重开问题(Reopened issues)



<br/>
<br/>



### 可维护性


有:

- 异味(Code Smells)
- 新异味(New Code Smells)
- 维护率(Maintainability Rating)
- 技术债务(Technical Debt)
- 新代码的技术债务(Technical Debt on New Code)
- 技术债务率(Technical Debt Ratio)
- 新代码的技术债务率(Technical Debt Ratio on New Code)


<br/>


#### 维护率

使用**SQALE评级**。与您的技术债务比率值相关的项目评级。
默认的可维护性评级网格是:

- A=`0-0.05 (<5%)`
- B=`0.06-0.1 (6%-10%)`
- C=`0.11-0.20(11%-20%)`
- D=`0.21-0.5(21%-50%)`
- E=`0.51-1(50%-100%)`


<br/>
<br/>


#### 技术债务

努力修复所有异味。以分钟(min)为度量单位存储在数据库中，单位值中的天假设为8小时(h)。


<br/>
<br/>


#### 技术债务率

开发成本与修复成本之间的比率。技术债务公式为: `Remediation cost / Development cost`

开发一行代码的成本价值为`0.06 day == 0.06 * 8 * 60 min`



<br/>
<br/>



### 质量阈


有:

- 质量阈状态(Quality Gate Status)
- 质量阈详情(Quality Gate Details)



<br/>
<br/>



### 可靠性


有:

- Bugs
- New Bugs
- 可靠率(Reliability Rating)
- 可靠性的修复工作(Reliability remediation effort)
- 新代码可靠性的修复工作(Reliability remediation effort on new code)


<br/>


#### 可靠率

- A = 0 Bugs
- B = at least 1 Minor Bug
- C = at least 1 Major Bug
- D = at least 1 Critical Bug
- E = at least 1 Blocker Bug


<br/>
<br/>


#### 修复工作

努力解决所有Bugs。以分钟为单位度量值存储在数据库中。如果数值天，则假设一天为8小时。



<br/>
<br/>



### 安全性


有:

- 漏洞(Vulnerabilities)
- 新漏洞(New Vulnerabilities)
- 安全级(Security Rating)
- 安全修复工作(Security remediation effort )
- 新代码的安全修复工作(Security remedation effort on new code)


<br/>


#### 安全评级

- A = 0 Vulnerabilities
- B = at least 1 Minor Vulnerability
- C = at least 1 Major Vulnerability
- D = at least 1 Critical Vulnerability
- E = at least 1 Blocker Vulnerability



<br/>
<br/>



### 大小


有:

- 类(Classes)
- 注释行(Comment lines)
- 注释占比(Comments %) - ` Comment lines / (Lines of code + Comment lines) * 100`
- 目录(Directories)
- 文件(Files)
- 行数(Lines)
- 代码行数(Lines of code)
- 每种语言的代码行数(Lines of code per language)
- 函数(Functions)



<br/>
<br/>



### 测试


有:

- 条件覆盖(Condition coverage)
- 新代码条件覆盖(Condition coverage on new code)
- 条件覆盖命中(Condition coverage hits)
- 逐行条件(Conditions by line)
- 逐行条件覆盖(Covered conditions by line)
- 覆盖(Coverage)
- 新代码覆盖(Coverage on new code)
- 行覆盖(Line coverage)
- 新代码行覆盖(Line coverage on new code)
- 行覆盖命中(Line coverage hits)
- 要覆盖的行(Lines to cover)
- 新代码要覆盖的行(Lines to cover on new code)
- 跳过单元测试(Skipped unit tests)
- 未覆盖条件(Uncovered conditions)
- 新代码未覆盖条件(Uncovered conditions on new code)
- 未覆盖行(Uncovered lines)
- 新代码未覆盖行(Uncovered lines on new code)
- 单元测试(Unit tests)
- 单元测试持续时间(Unit tests duration)
- 单元测试错误(Unit test errors)
- 单元测试失败(Unit test failures)
- 单元测试成功密度(Unit test success density %) - `Test success density = (Unit tests - (Unit test errors + Unit test failures)) / Unit tests * 100`


<br/>


#### 条件覆盖

在包含一些布尔表达式的每行代码中，条件覆盖只是回答了以下问题: *每个布尔表达式是否都被评估为 `true` 和 `false`?*。这是在单元测试执行期间遵循的流控制结构中可能的条件密度。

`Condition coverage = (CT + CF) / (2*B)`, where:

- CT = conditions that have been evaluated to 'true' at least once(已经被评估为`true`至少一次的条件)
- CF = conditions that have been evaluated to 'false' at least once(已经被评估为`false`至少一次的条件)
- B = 条件总数(total number of conditions)


<br/>
<br/>


#### 覆盖

它是行覆盖和条件覆盖的混合。它的目标是为以下问题提供更准确的答案: *单元测试覆盖了多少源代码?*

`Coverage = (CT + CF + LC)/(2*B + EL)`, where:

- CT = 已经被评估为`true`至少一次的条件
- CF = 已经被评估为`false`至少一次的条件
- LC = 覆盖的行(covered lines)
- B = 条件总数
- EL = 可执行行的总数( total number of executable lines)


<br/>
<br/>


#### 行覆盖

在给定的代码行上，行覆盖简单地回答了以下问题: *在执行单元测试期间是否执行了这行代码?*

它是单元测试的覆盖率密度:

`Line coverage = LC / EL`, where:

- LC = 覆盖的行(covered lines)
- EL = 可执行行的总数(total number of executable lines)




<br/>
<br/>
<br/>



## 概念

Concepts


<br/>
<br/>


### 架构

Architecture


| 概念 | 定义 |
| - | - |
| Analyzer | 用于分析源代码以计算快照的客户端程序 |
| Database | 存储配置和快照 |
| Server | 用于浏览快照数据和进行配置修改的Web界面 |



<br/>
<br/>



### 质量

Quality


| 概念 | 定义 |
| - | - |
| Bug | 表示代码中出错的问题 |
| Code Smell | 代码中与可维护性相关的问题 |
| Cost | 花费 |
| Debt | 解决问题所需的时间 |
| Issue | 代码不符合规则时，快照上会记录一个问题。有: Bugs , Code Smells and Vulnerabilities |
| Measure | 给定时间内给定文件或项目的度量值 |
| Metric | 一种测量方式。随着时间的推移，度量标准可能具有不同的值或度量 |
| New Code Period | 需要密切关注代码中引入新问题的时间段 |
| Quality Profile | 一组规则 |
| Rule | 应该遵循的编码标准或惯例 |
| Remediation Cost | 修复漏洞和可靠性问题所需的估计时间 |
| Snapshot | 在给定时间内针对给定项目的一组度量和问题 |
| Security Hotspot | 与安全相关的问题，突出显示使用安全敏感API的一段代码 |
| Technical Debt | 修复问题所需的估计时间 |
| Vulnerability | 与安全相关的问题，代表攻击者的后门 |




<br/>
<br/>
<br/>



## 活动

Activity and History


项目活动页面提供项目文件分析的完整列表，以及随着时间推移看到项目措施演变的能力。
活动页面上的图标可帮助你了解几种相互选择的度量方法的演变。


<br/>


### 事件

Events


有四种类型的事件:

- Quality Gate
- Profile
- Version
- Other




<br/>
<br/>
<br/>




## SonarLint

SonarLint Smart Notifications


SonarLint Smart Notifications是作为Developer Edtion的一部分来提供。

智能通知允许使用SonarLint中的连接模式的开发人员以一下情况下从SonarQube接收IDE内的通知:

- the Quality Gate status (failed / success) of a project /solution open in the IDE changes
- a SonarQube analysis raises new issues introduced by this developer in a project /solution open in the IDE

SonarLint智能通知的激活和取消必须由每个开发人员直接在SonarLint(IDE端)进行单独完成。
可以在SonarQube上逐个服务器地在SonarLint端配置接收通知。




<br/>
<br/>
<br/>




## Security Reports


<br/>


### 安全报告显示了什么

What do the Security Reports show?


安全报告旨在快速为您提供有关应用程序安全性的全景图，并详细说明OWASP, SANS, CWE标准的详细信息。安全报告由分析器提供，分析器依赖于质量配置文件中激活的规则来引发安全问题。


<br/>
<br/>



### 热点和漏洞有什么区别

What's the difference between a Hotspot and a Vulnerability?


漏洞是代码中可以攻击的点。安全热点是安全敏感的代码段，应由具有安全审计员帽的人仔细审查。
安全热点的主要目标是帮助集中手动审查应用程序源代码的安全审核员的工作。第二个目标是教育开发人员并提高他们的安全意识。



<br/>
<br/>



### 为什么某些热点和漏洞非常相似

Why are some Hotspot and Vulnerability rules very similar?


它们是故意重叠的。热点规则应该包括漏洞规则的所有匹配，以及污点分析引擎无法检测漏洞的情况。



<br/>
<br/>



### 为什么我看不到任何热点

Why are some Hotspot and Vulnerability rules very similar?


有三个原因:

- 可能真的没有它们，因为代码是在没有使用任何安全敏感API的情况下编写的
- 热点规则可能可用，但尚未在你的质量配置文件中激活，因此自然不会引发任何问题
- 你正在使用的语言分析器可能还没有提供热点规则，所以它不会引发任何热点



<br/>
<br/>



### 为什么我看不到任何漏洞


由于一些热点原因，你可能没有看到任何漏洞的，但你可能会看到项目主页中报告了一些漏洞，而安全报告中没有漏洞。这是因为语言分析器可能尚未提供安全报告中可见问题所需的安全标准的元数据。




<br/>
<br/>



### 开发者是否应该关心热点


可能并不需要。热点并不是真正可行的，它们只是标记潜在的问题，所以在代码上没有立即做任何事情。这就是为什么在引发热点问题时没有收到通知。



<br/>
<br/>



### 如果热点确实标记为漏洞怎么办


如果您查看引发热点的代码并意识到确实存在问题，请单击当前状态以注册您在代码中检测到漏洞。完成后，它将转换为漏洞，最后触摸该行的开发人员将收到新问题通知。



<br/>
<br/>



### 热点变为漏洞后会发生什么


一旦您检测到热点位置确实存在问题，它将被分配给相应的开发人员，他们将进行修复，然后必须通过UI请求审核。



<br/>
<br/>



### 热点被标记为不会修复是什么意思

What does it mean for a Hotspot to be marked "Won't Fix"?


不会修复标记用于表示已经审查了热点，并且目前无法利用这段代码创建攻击。





<br/>
<br/>
<br/>




## 用户账户

User Account


SonarQube用户可拥有自己的空间，可查看与自己相关的内容。




<br/>
<br/>
<br/>




## User Token


每个用户都可生成令牌，这些令牌可用于运行分析或调用Web服务，而无需用户的实际凭据。




<br/>
<br/>

---

<br/>
<br/>




# 项目管理

Project Administration


<br/>


## 项目存在

Project Existence


通常，项目在第一次分析时创建，不会删除(除非手动删除)。你可以管你你有权限管理的项目。

<br>

- 在第一次分析之前配置项目
- 配置还未分析的项目
- 修改项目权限(Private/Public) - 默认情况下，任何新创建的项目都被视为Public。这意味着每个经过认证的用户都能够**Browse**和**See Source Code**
- 删除项目
- 查找不再分析的项目



<br/>
<br/>



## 管理项目历史

Managing Project History


SonarQube最强大的功能之一是它不仅向你展示了你今天的项目健康状况，还展示了它随时间的变化情况。它通过有选择地保留以前分析的数据来做到这一点。它没有保留所有以前的分析——这会使数据库膨胀。同样，对于它确实存在的分析，SonarQube不会保留所有数据。一旦项目快照(snapshot)从最后分析(Last analysis)移动到项目历史的一部分，项目级别下面的数据就会被清除——再次放置数据库膨胀。

通常这些都不是你需要考虑的事情。SonarQube只为你专门处理它们。但有时你可能需要从项目的历史记录中删除错误的快照或修改内存处理算法。

<br>

可查看数据库表大小:

```sql
# sonar
USE information_schema;

DESCRIBE TABLES;

SELECT TABLE_SCHEMA, TABLE_NAME, TABLE_ROWS, DATA_LENGTH FROM TABLES WHERE TABLE_SCHEMA = 'sonar' ORDER BY DATA_LENGTH DESC;
```

<br>

有时你可能需要手动删除项目快照，无论是因为使用了错误的质量配置文件，还是因为分析存在问题...请注意，永远不能删除最新的快照。

对于每个快照，可以手动:

- Add, rename or remove a version
- Add, rename or remove an event
- Delete the snapshot



<br/>
<br/>



## 缩小关注点

Narrowing the Focus


如果SonarQube的结果不相关，那么没有人会想要使用它。这就是为什么精确配置每个项目要分析的内容是非常重要的一步。
SonarQube为你提供了几种选项，可以准确配置要分析的内容。你可以:

- 完全忽略一些文件或目录
- 从问题中排除文件或目录，但分析所有其它方面
- 从重复性中排除文件或目录，但分析所有其它方面
- 从覆盖率中排除文件或目录，但分析其它所有方面

你可以在全局或项目级别配置它们。


<br/>


### 忽略文件

Ignore Files


建议你从库中排除生成的代码，源代码等。有四种不同的方法可将分析范围缩小到与开发团队相关的源代码。

- 源目录(Source Directories)
- 文件后缀(File Suffixes)
- 选择文件(Choosing Files)
  - 源文件排除(Source File Exclusions)
  - 测试文件排除(Test File Exclusions)
  - 源文件包含(Source File Inclusions)
  - 测试文件包含(Test File Inclusions)

![](/images/SonarQube/exclusions.jpg)

![](/images/SonarQube/inclusions.jpg)



<br/>
<br/>



### 忽略问题

Ignore Issues


可使用SonarQube忽略某些组件和某些编码规则的问题。`Administration > General Settings > Analysis Scope > Issues`。

请注意，以下属性只能通过Web界面设置，因为它们是多值的。

- Ignore Issues on Files
- Ignore Issues in Blocks
- Ignore Issues on Multiple Criteria
- Restrict Scope of Coding Rules



<br/>
<br/>



### 忽略重复

Ignore Duplications


可在SonarQube中阻止检查某些文件的重复性。`Administration > General Settings > Analysis Scope > Duplications`。



<br/>
<br/>



### 忽略代码覆盖率

Ignore Code Coverage


可以通过单元测试防止某些文件考虑用于代码覆盖。`Administration > General Settings > Analysis Scope > Code Coverage > Coverage Exclusions`。



<br/>
<br/>



### 模式

Patterns


SonarQube中可以使用以下通配符:

- `*`	- 零个或多个字符(zero or more characters)
- `**` - 零个或多个目录(zero or more directories)
- `?` - 单个字符(a single character)




<br/>
<br/>
<br/>



## 项目设置

Project Settings


<br/>


### Tags


项目标签(tags) 允许对项目进行分类和分组，以便在项目页面上更容易地选择。可以从项目主页管理项目标签。



<br/>
<br/>



### 管理项

Administration Items:

- Adding a Project
- Analysis Report Processing
- Deleting a Project
- Setting the New Code Period
- Updating Project Key
- Default Issue Assignee
- Setting Quality Gate and Quality Profiles
- Setting Exclusions
- Customizing Links



<br/>
<br/>
<br/>




## Webhooks


网络调用(Webhooks) 在项目完成分析后通知外部服——An HTTP POST request including a JSON payload is sent to each URL。可在项目级别和全局指定URL。项目级别的配置不会取代全局的配置，两个级别的所有Webhooks都被调用。

HTTP(s) 调用:

- 无论后台任务的状态如何
- 使用POST方法将JSON文档作为负载
- 使用`UTF-8`编码的内容类型`application/json`



<br/>


### Delivery and Payload

Webhook 管理控制台显示每个Webhook的最新交付的结果和时间戳，其中有效负载可通过列表图标获得。默认保留30天的记录。URL必须在10s响应，否则传递将标记为失败。

发送带有project key的 HTTP header `X-SonarQube-Project`，以便快速识别所涉及的项目。

Payload是一个JSON文档，包括:

- 什么时候运行分析(`analysedAt`)
- 分析的项目的标识(`project`)
- 每个质量阈标准和状态(`qualityGate`)
- 每个项目的质量阈状态(`qualityGate.status`)
- 后台任务的状态和标识(`status`, `taskId`)
- 用于定义的属性(`properties`)

栗子:

```json
{
    "analysedAt": "2016-11-18T10:46:28+0100",
    "project": {
        "key": "org.sonarqube:example",
        "name": "Example"
    },
    "properties": {
    },
    "qualityGate": {
        "conditions": [
            {
                "errorThreshold": "1",
                "metric": "new_security_rating",
                "onLeakPeriod": true,
                "operator": "GREATER_THAN",
                "status": "OK",
                "value": "1"
            },
            {
                "errorThreshold": "1",
                "metric": "new_reliability_rating",
                "onLeakPeriod": true,
                "operator": "GREATER_THAN",
                "status": "OK",
                "value": "1"
            },
            {
                "errorThreshold": "1",
                "metric": "new_maintainability_rating",
                "onLeakPeriod": true,
                "operator": "GREATER_THAN",
                "status": "OK",
                "value": "1"
            },
            {
                "errorThreshold": "80",
                "metric": "new_coverage",
                "onLeakPeriod": true,
                "operator": "LESS_THAN",
                "status": "NO_VALUE"
            }
        ],
        "name": "SonarQube way",
        "status": "OK"
    },
    "serverUrl": "http://localhost:9000",
    "status": "SUCCESS",
    "taskId": "AVh21JS2JepAEhwQ-b3u"
}
```



<br/>
<br/>



### 附加参数

Additional parameters


通过在Webhook的URL中提供`user/passwd`来支持基本的身份认证机制。(如: `https://myLogin:myPassword@my_server/foo`)


如果使用了`sonar.analysis.*`属性为SonarScanner提供其它属性，则这些属性将自动添加到有效负载的`properties`部分。


栗子:

```shell
sonar-scanner -Dsonar.analysis.scmRevision=628f5175ada0d685fd7164baa7c6382c1f25cab4 -Dsonar.analysis.buildNumber=12345
```




<br/>
<br/>

---

<br/>
<br/>




# 实例管理

Instance Administration


<br/>


## 质量配置

Quality Profiles


<br/>


### 概述

质量配置(Quality Profiles)服务是SonarQube的核心，因为它是您通过定义规则集来定义需求的地方。。

理想情况下，对于任何给定的语言，所有项目都将使用相同的配置文件进行测量，但这并不总是实用的。
这就是为什么您可以根据需要定义尽可能多的质量配置文件，即使建议尽可能少的质量配置文件以确保公司项目的一致性。

每个语言都带有预定义的內建配置文件(通常称为 Sonar way)，因此你可以使用SonarQube分析进行快速开始。这就是为什么只要安装新的语言插件，就可以使用至少一个配置文件。

默认的Sonar way配置文件，它包含了通常适用于大多数项目的所有规则。但作为最佳实践，你应该创建一个新的配置文件(你可以通过复制Sonar way的内容来填充它)，并使用它。
因为默认的Sonar way是不可编辑的，因此你无法根据需要对其进行自定义。此外，这使你可将Sonar way视为一个基线，可在对其进行更改时跟踪自己的配置文件。此外Sonar way通常会随插件的每个新版本更新，已添加规则，有时还会调整规则严重性。任何继承自內建Sonar way的配置文件都将在事实上同时自动更新。



<br/>
<br/>



### 我该怎么做


<br/>


####　将质量配置管理的权限移交给其他人

Delegate the management of Quality Profiles to someone else?

默认情况下，管理员才有此权限。但你可以授予用户/组权限来编辑配置文件。例如将Java配置文件权限分配给Java开发专家，将Python配置文件权限分配给Python专家...


<br/>
<br/>


#### 将规则从一个配置复制到另一个配置

Copy the rules from one profile to another?

许多时候，人们希望使用基于內建的配置文件的配置文件进行工作，而无实际需要使用內建配置文件。


<br/>
<br/>


#### 了解配置中有什么改变

Know what's changed in a profile?

当SonarQube注意到使用与先前分析不同的配置文件执行分析时，会将质量配置文件事件添加到项目的事件日志中。


<br/>
<br/>


#### 将配置文件从一个实例复制到另一个实例

Copy a profile from one SonarQube instance to another?

使用实例上的备份(Back UP)功能将配置文件导出到XML文件。然后在另一个实例中选择恢复(Restore)。


<br/>
<br/>


#### 将一组核心规则和附加规则应用于项目

Apply a core set of rules plus additional rules to a project?

使用继承，从root继承核心规则集。然后创建一个子配置文件(Sprout)，修改从Root继承，然后添加缺少的规则。


<br/>
<br/>


#### 确保我的非默认配置文件应用于项目

Make sure my non-default profile is used on a project?


<br/>
<br/>


#### 确保我的个人配置中包含所有相关的新规则

Make sure I've got all the relevant new rules in my profile?


<br/>
<br/>


#### 比较两个规则

Compare two profiles?


<br/>
<br/>


#### 确保我的配置中没有任何弃用的规则

Make sure I don't have any deprecated rules in my profile?


<br/>
<br/>


#### 安全

Security


任何用户都可以访问质量配置服务，你可以给他们配置质量配置管理权限，让他们可以创建，删除质量配置。




<br/>
<br/>
<br/>




## 安全


<br/>


### 概述

SonarQube具有许多全局安全功能:

- 认证和授权机制
- 强制身份认证
- 委派认证

除此之外，还可在group/user级别配置:

- 查看一个已存在的项目
- 访问项目的源代码
- 管理一个项目
- 管理质量配置，质量阈，实例...



<br/>
<br/>



### 认证

Authentication


第一个问题: 匿名用户是否可以浏览SonarQube实例？
当然不行！那就需要强制用户认证。

<br>

**认证机制(Authentication Mechanisms)**

可通过多种方式来管理认证机制:

- 通过SonarQube內建的user/group数据库
- 通过外部程序(如LDAP)
- 通过HTTP headers

<br>

**技术用户(Technical Users)**

当你在SonarQube数据库中创建用户时，他将被视为本地用户，并且针对SonarQube自己的user/group数据库进行身份认证，而不是通过任何外部工具。
默认情况下，`admin`是本地账户。

同样，所有非本地(non-local)账户将仅针对外部工具进行身份认证。

管理员可以管理所有用户的**Tokens**——创建和删除。一旦创建，Token就是运行分析所需的唯一凭证，作为`sonar.login`属性的值来传递。


<br>

**默认管理员(Default Admin Credentials)**

当安装SonarQube时，会自动创建具有管理系统权限的默认用户:

```yaml
user: admin
passwd: admin
```



<br/>
<br/>



### 重置管理员密码

Reinstating Admin Access


如果你修改了管理员密码，但又忘记了:

```sql
USE sonar;

update users set crypted_password = '$2a$12$uCkkXmhW5ThVK8mpBvnXOOJRLd64LJeHTeCkSuB3lfaR2N0AYBaSi', salt=null, hash_method='BCRYPT' where login = 'admin'
```

如果您删除了管理员并随后锁定了具有全局管理权限的其他用户:

```sql
USE sonar;

INSERT INTO user_roles(user_id, role) VALUES ((select id from users where login='mylogin'), 'admin');
```



<br/>
<br/>



### 授权

Authorization


对不同组、不同用于仅限权限分配，以访问不同的资源。

- user
- group
- Global Permissions
  - Administer System
  - Administer Quality Profiles
  - Administer Quality Gates
  - Execute Analysis
  - Create Projects
  - Create Applications
  - Create Portfolios
- Project Permissions
  - Public and Private
    - Administer Issues
    - Administer Security Hotspots
    - Administer
    - Execute Analysis
  - Private
    - Browse
    - See Source Code



<br/>
<br/>



### 默认权限的权限模板

Permission Templates for Default Permissions


SonarQube附带默认权限模板，该模板在创建项目，项目组合或应用程序自动授予特定组的特定权限。管理员可以编辑此模板。



<br/>
<br/>



### 加密

Encryption


加密主要用于从设置中删除明文密码。实现的解决方案是基于对称密钥算法，关键是密钥存储在磁盘上的安全文件中。此文件必须由运行SonarQube Server的系统账户拥有和读取。
该算法是AES 128位。

- **Generate the secret key**
- **Store the secret key on the SonarQube server**
- **Generate the encrypted values of your settings**
- **Use the encrypted values in your SonarQube server configuration**

必须在SonarQube基础架构的所有部分之间共享唯一的密钥。在`Administration > Configuration > Encryption`生成密钥。
生成密钥之后，会显示如何使用此密钥。

![](/images/SonarQube/secretKey.png)

之后便可以为你设置的值进行加密。同样在前面的加密下进行配置。
之后在SonarQube Server中使用加密后的值:

```
# conf/sonar.properties

sonar.jdbc.password={aes}CCGCFg4Xpm6r+PiJb1Swfg==  # Encrypted DB password
...
sonar.secretKeyPath=C:/path/to/my/secure/location/my_secret_key.txt
```




<br/>
<br/>
<br/>




## 委托认证

Delegating Authentication

docs: <https://docs.sonarqube.org/latest/instance-administration/delegated-auth/>


SonarQube认证:

- 自带用户数据库认证
- 外部
  - HTTP header
  - LDAP
  - ...


<br/>


### HTTP header认证



<br/>
<br/>


### LDAP认证




<br/>
<br/>
<br/>



## 通知

Notifications


可以通过邮件配置，向用户发送分析的信息的通知。






<br/>
<br/>

---

<br/>
<br/>




# 使用实践

> **注意:**
> 由于使用的是SonarQube CE(社区版)，因此不支持在IDE中上传分析数据，也不支持多分支(branch)分析。所以需要对这些方面做一些规范。

<br/>

SonarQube的使用主要分为两个方面:

- **开发者 IDE**
- **CI SonarScanner**


<br/>


## CI


CI端 需先安装 `SonarQube Scanner` 应用程序，并配置相应的路径和token。

由于社区版的缘故，我只对测试分支的CI进行SonarScanner分析，并将结果上传到SonarQube Server对应项目的路径。

由于测试分支(stage)的代码都是由开发者现在本地IDE中检测过代码质量(Code Quality)之后才MR过来，所以这样更方便和实用些。

CI SonarScanner分析上传之后，SonarQube会通知项目负责人此项目代码相关情况。由项目负责人去SonarQube Web UI上再去核查相关issues，核查无误之后，才能将测试分支的代码上线。
如果项目负责人检查出相关代码的某些问题，请于相关分支开发者交流，叮嘱他们现在本地IDE自测，通过之后在MR代码。



<br/>
<br/>


## IDE


只需在IDE中下载SonarLint插件，并配置上运维人员提供的地址和token就可以使用了。

由于社区版的缘故，我这里让**开发者自己的分支**在IDE中调用远程SonarQube进行本地代码质量检查，并不需要将开发者的分支代码情况上传到SonarQube Server端。

开发者自己检查和核对自己分支的代码质量，确认之后才将自己的代码MR到dev分支。
如果项目负责人检测到某位开发者的分支代码存在问题，则这个责任由分支开发者负责和处理。



<br/>
<br/>


## 权限问题


权限有一些地方需要注意:

- 将项目设置为私有(默认: public)
- 项目对应项目组(group)，对应项目成员(user)
- 项目组中的CI, IDE用户具有不同的权限
- ...

<br>

具体配置可以在使用的时候灵活修改！









<br/>
<br/>

---

<br/>
<br/>





# API


可通过SonarQube API 进行许多操作。

<br>


```bash
# 如导出python的代码规则
curl -X GET -v -u user:passwd  http://localhost:9000/api/rules/search?language=python > python.json

```






















<br/>
<br/>

---

<br/>
<br/>









# Scanner

- docs: <https://docs.sonarqube.org/display/SCAN>


建议将SonarQube Scanner用作使用SonarQube分析项目的默认扫描程序。


<br/>


## 安装


<br/>


### OS

平台:

- Linux
- Mac OS
- Windows

<br>

下载对应平台的Sonar Scanner应用程序，将它们解压之后加入系统路径(`$PATH`)。



<br/>
<br/>
<br/>



### IDE

Sonar Scanner 支持的 IDE 有:

- MSBuild
- Maven
- Gradle
- Ant
- Jenkins
- JetBrains

<br>

在IDE中下载**SonarLint**插件，之后配置SonarQube Server地址和管理员给的Token便可以正常使用。
社区版的SonarQube 只能在IDE中检测，无法上传。










