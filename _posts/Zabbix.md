---
title: "Zabbix"
date: "2017-11-14 10:18:09"
tags: "Zabbix"
categories: "Linux"

---


参考：

- [Zabbix官方网站](https://www.zabbix.com/)
- [Zabbix中文文档](https://www.zabbix.com/documentation/3.4/zh/manual/)
- Zabbix-repo仓库: <http://repo.zabbix.com>
- 阿里云镜像: <https://mirrors.aliyun.com/zabbix/zabbix/> .


环境：

- CentOS7x86_64
- Zabbix 3.4


<br/>
<br/>
<!--more-->

---

<br/>


# Zabbix简介



Zabbix （音同 zæbix），是由 Alexei Vladishev 开发的一种网络监视、管理系统，基于 Server-Client 架构。Zabbix 的授权是属于 GPLv2。
Zabbix可用于监视各种网络服务、服务器和网络机器等状态。是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。
Zabbix也可经由SNMP、TCP、ICMP、SSH等对目标进行监视。


<br>

## Zabbix的系统构成

Zabbix系统由以下各独立模块组成：

- Zabbix Server，服务端(以C开发)。Server端通过收集SNMP和Agent发送的数据，写入数据库，再通过PHP+Apache在Web端展示；
- Zabbix Agent，客户端(基本支持所有操作系统)，并将监控主机数据发送给Server；
- Zabbix Frontend，Web管理端(以PHP和JavaScript构成)；
- Zabbix Proxy(可选组件)。用于分布式监控。


<br/>

## Zabbix的特点

Zabbix是一个高度集成的网络监控解决方案，一个简单的安装包中提供多样性功能。

- 数据收集；
- 灵活的阀值(触发器)定义；
- 高度可配置化的告警；
- 实现图表绘制；
- Web监控功能；
- 丰富的可视化选项；
- 历史数据存储；
- 配置简单；
- 使用模板；
- 网络发现；
- Zabbix API；
- 权限管理系统；
- 功能强大并易于扩展的监控代理。




<br>
<br/>

---

# 定义

Zabbix的常用术语含义。

**主机(host)：**

一台你想监控的网络设备，用IP或域名表示。主机名不能使用中文创建，会报错。

**主机组(host group):**
主机的逻辑组，它包含主机和模板。组名可以使用中文。

**监控项(item):**
你想要接收的主机的特定数据，一个度量数据。

**触发器(trigger):**
一个被用于定义问题阀值和评估监控项接收到的数据的逻辑表达式。

**事件(event):**
单次发生的需要注意的事情。

**异常(problem):**
一个处在异常状态的触发器。

**动作(action):**
一个对事件作出反应的预定义的操作。

**升级(escalation):**
一个在动作内执行操作的自定义场景。

**媒介(media):**
发送报警通知的手段。

**通知(notification):**
利用已选择的媒体途径把事情相关信息发送给用户。

**远程命令(remote command):**
预先定义好的，满足一定条件后，可在被监控主机上自动执行的命令。

**模板(template):**
一组可以被应用到一个或多个主机上的实体的集合。

**应用(application):**
一组监控项组成的逻辑分组。

**Web场景(Web scenario):**
利用一个或多个HTTP请求来检查网站的可用性。

**前端(frontend):**
Zabbix提供的Web界面。

**Zabbix API:**
Zabbix API允许你使用JSON RPC协议来创建、更新和获取Zabbix对象信息或执行任何其他的自定义的任务。

**Zabbix server:**
Zabbix软件监控的核心程序，主要功能是与Zabbix proxies和agent进行交互、触发器计算、发送告警通知，并将数据集中保存等。

**Zabbix agent:**
部署在监控对象上，能够主动监控本地资源和应用。

**Zabbix proxy:**
帮助Zabbix server收集数据，分担Zabbix server的负载。




<br>
<br/>

---

# Zabbix进程

## Agent
zabbix agent部署在监控的目标上，主动监测本地的资源和应用（硬件驱动，内存，处理器统计等）。
zabbix agent手机本地的操作信息并将数据报告给zabbix server用于进一步处理。

zabbix agent有被动(passive)和主动(active)两种检查方式。


<br>
## Server
zabbix server是zabbix软件的核心程序。它通过轮询和捕获数据，计算是否满足触发器条件，向用户发送通知。
它是zabbix监控代理和Proxy代理报告系统可用性和完整性数据的核心组件。zabbix server自身可以通过简单远程检查网络服务(如Web服务器和邮件服务器)。

server是一个包含了被存储了所有配置，统计方面的和可操作数据的中央仓库，它是监控系统问题升级以致于激活警告管理器的zabbix中的实体。

基本的zabbix server分三个不同的组件：**zabbix server，web前端，数据库存储**。
zabbix的所有配置信息都存储在服务器和web前端进行交互的数据库中。

zabbix server进程是以守护进程（Daemon）运行的。


<br>
## Proxy
zabbix proxy是一个可以从一个或多个受监控的设备设备收集监控数据，并将信息发送到zabbix server的进程，基本上是代表server工作。
所有收集的数据都在本地进行缓存，然后传送到proxy所属的zabbix server。

zabbix proxy是完成远程区域、分支机构、没有本地管理员的网络的集中监控的理想解决方案。

zabbix proxy需要使用独立的数据库，以守护进程的方式运行。


<br>
## Java gateway
zabbix守护进程原生支持监控JMX程序，它被称为zabbix java gateway。zabbix gateway是用Java语言写成。

要查得一台主机特定的JMX计数器值，zabbix server向zabbix java gateway发送请求，后者使用JMX管理API去请求远程的有关应用。应用不许额外安装软件，只需要启动时在命令行指定 `-Dcom.sun.management.jmxremote`即可（是在java程序）。

每个zabbix server或zabbix agent只能配置一个java gateway。


<br/>
## Sender
zabbix sender是一种命令行应用，它可以将性能数据发送到zabbix server进行处理。该应用通常用在长时间运行的用户脚本，用于定期发送可用性和性能数据。

```
zabbix_sender -z zabbix -s "xxx" -k db.connections -0 43

-z :server主机
-s :受监控主机的技术名称
-k :监控项的键
-o :要发送的值
```


<br>
## Get
zabbix get也是一种命令行应用，用于与zabbix agent进行通信，并从agent那里获取所需的信息。
该应用通常被用于zabbix agent故障排除

```
zabbix_get -s $host -p xxx -k system.cpu.load[all,avg15]

-s  --host
-p --port
-I --source-address
-k --key
-h --help
-V --version
```




<br>
<br/>

---

# 安装Zabbix

## Zabbix安装要求

**硬件：**
- 内存，最小128MB；
- 磁盘，最小256MB；
- CPU，可能需要大量CPU资源；
- SMS(短信)通知服务，串行通讯口(serial communication port)和串口GSM调制解调器(serial GSM modem)。可选项。

**支持平台：**
- Linux;
- IBM AIX;
- FreeBSD;
- NetBSD;
- OpenBSD;
- Mac OS X;
- Solaris;
- Windows(Only Agent).

**软件：**
Zabbix基于Apache Web服务器、领先的数据库引擎和PHP脚本语言进行构建。

**数据库管理系统：**
- MySQL 5.0.3 及以上；
- Oracle 10g 及以上；
- PostgreSQL 8.1 及以上；
- SQLite 3.5及以上；
- IBM DB2 9.7 及以上。

**前端：**
- Apache 1.3.12 及以上；
- PHP 5.4.0及以上；

**PHP-Extension:**

软件 | 版本 | 备注
- | - | -
gd | 2.0及以上 | PHP GD扩展包必须支持PNG图片
bcmatch |  | php-bcmatch
ctype |  | php-ctype
libXML | 2.6.15及以上 | php-xml
xmlreader |  | php-xmlreader
xmlwrite |  | php-xmlwriter
session |  | php-session
sockets |  | php-net-socket
mbstring |  | php-mbstring
gettext |  | php-gettext
ldap |  | php-ldap
mysqli |  | 使用MySQL作为Zabbix后端数据库所需的组件
pgsql |  | 使用PostgreSQL作为Zabbix后端数据库所需的组件
sqlite3 |  | 使用SQLite作为Zabbix后端数据库所需的组件

**客户端浏览器：**
必须启用Cookie和JavaScript功能。

**服务器：**

要求 | 描述 |
- | - |
OpenlPMI | 支持IPMI功能所需组件 |
libssh2 | 支持SSH功能 |
fping | 支持ICMP ping功能 |
libcurl | 支持Web监控，VMware监控及SMTP认证 |
libiksemel | 支持Jabber功能 |
libxml2 | 支持VMware监控 |
net-snmp | 支持SNMP监控 |

**Java网关：**
Java gateway编译和运行在Java 1.6 及以上版本。

**数据库容量：**
Zabbix配置数据需要使用固定的磁盘空间，而这个空间不会过多增长。

Zabbix数据库容量主要依赖于以下参数：
- 每秒处理值的数量(Number of processed values per second);
- 历史(History)数据的回收清理设置(Housekeeper);
- 趋势(Trends)数据的回收清理设置(Housekeeper);
- 事件(Events)数据的回收清理设置(Housekeeper)。

**时钟同步：**
对于Zabbix稳定运行而言，服务获取精确的系统时间是非常重要的。对于所有运行Zabbix组件的系统，强烈建议这些系统的时间保持同步。
`ntpd`是一个临幸的用于同步主机和其他服务器之间的时间的后台程序。


<br>
## 安装、启动、配置Zabbix
Zabbix-repo仓库：[repo.zabbix.com](repo.zabbix.com)
该仓库服务器同时提供`yum`和`apt`源码库。

### 配置源码库

**1. 从官方下载源码库**

```sh
#rpm -ivh http://repo.zabbix.com/zabbix/$version/rhel/7/$arch/$zabbix-release.rpm
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-1.el7.centos.noarch.rpm

#阿里云镜像
#rpm -ivh http://mirrors.aliyun.com/zabbix/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-1.el7.noarch.rpm

#镜像失效的话自己去官网找
```

**2. 手动配置zabbix.repo**

```sh
vim /etc/yum.repos.d/zabbix.repo

[zabbix]
name=Zabbix-Repo
baseurl=http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/
gpgcheck=0
enable=1
```

<br>
### 安装Zabbix部署包

使用MySQL数据库安装Zabbix Server、Web前端：
```sh
yum install -y zabbix-server-mysql zabbix-get
```

**注意：此处Zabbix数据库使用MySQL，请自行安装MySQL。**

安装Zabbix Agent：
```sh
yum install -y zabbix-agent
```


<br>
### 安装初始化数据库

查看刚刚安装的 **zabbix-server-mysql**：
解压得到的sql脚本`create.sql`只会在对应的数据库中初始化zabbix所需要的数据库表，但是不会创建zabbix数据库。所以后面我们还需要手动创建`zabbix`数据库。
```sh
rpm -ql zabbix-server-mysql

cd /usr/share/doc/zabbix-server-mysql-3.x.xx/

#有一个create.sql.gz的压缩文件
gunzip create.sql.gz
#得到create.sql
```

**在MySQL中创建zabbix数据库：**
```sh
msyql -uxxx -p

mysql>CREATE DATABASE 'zabbix' DEFAULT CHARACTER SET 'utf8';
mysql>SHOW DATABASES;
mysql>GRANT ALL ON zabbix.* TO 'zabbix'@'localhost' identified by 'zabbix';
mysql>FLUSH PRIVILEGES;

#导入sql脚本
mysql -uroot -p -Dzabbix < ./create.sql

USE zabbix;
SHOW TABLES;


#mysql限制IP
vim /etc/my.cnf

[mysqld]
bind-address=127.0.0.1
```

<br>
![zabbix-database alt="zabbix-database"](/images/zabbix_database.png)


<br/>
### 配置zabbix server并启动

编辑zabbix server配置文件：
```sh
vim /etc/zabbix/zabbix_server.conf

#常会修改的参数

#数据库配置
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
DBPort=3306
DBSocket=/var/lib/mysql/mysql.sock

#服务监听端口
ListenPort=10051

#服务端源IP
SourceIP=

#日志记录方式，file使用指定文件作为日志文件，system将日志发往syslog，console将日志发送控制台
LogType=file
LogFile=/var/log/zabbix/zabbix_server.log
```

启动zabbix服务端：
```sh
systemctl start zabbix-server

#此处可能由于没有关闭SELinux而报错
tail /var/log/zabbix/zabbix_server.log
cannot set resource limit: [13] Permission denied

#关闭SELinux
setenforce=0

vim /etc/selinux/config
SELINUX=disabled

#查看zabbix-server默认监听的10051端口
netstat -nltp
```

<br>
### 安装zabbix web
zabbix web可以安装在单独的主机上，只要能连接到zabbix database所在数据库就行。但为了方便，都安装在了server上。

zabbix web需要LAMP环境：
```sh
#可能需要自己配置PHP remi源，注意PHP及扩展版本问题
yum install -y httpd php php-mysql php-mbstring php-gd php-bcmatch php-ldap php-xml

#指定php版本
#yum --enablerepo=remi-php56 install php-mysql php-mbstring php-gd php-bcmatch php-ldap php-xml

```

安装zabbix web所需的两个包：
```sh
yum install -y zabbix-web zabbix-web-mysql
#此处默认使用php5.4
#因为我的环境是php5.6,会报错
#此时就需要指定php版本来安装
yum --enablerepo=remi-php56 install zabbix-web zabbix-web-mysql


rpm -ql zabbix-web
#zabbix-web位于/usr/share/zabbix/
```

### 编辑zabbix的前端Apach-PHP配置文件

zabbix前端的Apache配置文件位于 /etc/httpd/conf.d/zabbix.conf:
```sh
vim /etc/httpd/conf.d/zabbix.conf

#需修改时区
php_value max_execution_time 300
php_value memory_limit 128M
php_value post_max_size 16M
php_value upload_max_filesize 2M
php_value max_input_time 300
php_value always_populate_raw_post_data -1
php_value date.timezone Asia/Shanghai

#建议顺便修改/etc/php.ini的时区
vim /etc/php.ini

date.timezone = Asia/Shanghai


#添加httpd的虚拟主机访问zabbix web
<VirtualHost IP:80>
servername zabbix.me
documentroot /usr/share/zabbix

默认数据

</VirtualHost>


#开启httpd服务
systemctl start httpd
```

<br>
![/etc/httpd/conf.d/zabbix.conf](/images/Zabbix/zabbix_conf.png)

<br/>
添加hosts后就可以利用域名访问zabbix-web端了。

```sh
echo -e "192.168.1.9 \t zabbix.me" >> /etc/hosts
```

<br>
### 在web端配置zabbix

在浏览器访问 http://zabbix.me 初始化zabbix配置。
配置好后就需要用账号密码进行登录zabbix-web端dashboard。

默认用户名是：admin，密码是配置文件里面设置的。
![登录zabbix Dashboard](/images/Zabbix/zabbix_me.png)

<br>

登录进Dashboard后，可修改语言为中文。

![Dashboard](/images/Zabbix/dashboard.jpg)

<br>

**如果你的Zabbix无法看到中文选项，那么可能需要如下操作：**
```sh
vim /usr/share/zabbix/include/locales.inc.php

#修改
'zh_CN' => ['name' => _('Chinese (zh_CN)'),     'display' => true],
```

**如果又遇到中文乱码的问题，则可以从windows中挑选一些好看的中文字体，将对应字体文件放置到zabbix web的字体目录中。**
windows中字体后缀.TTF，Linux中为.ttf。注意修改大小写。
```sh
cd /usr/share/zabbix/fonts
#只有一个默认字体 graphfont.ttf
#将新字体放置到此目录下


#修改配置文件中对应字体名称
vim /usr/share/zabbix/include/define.inc.php

#将默认字体名字修改为字体目录下 你需要的字体名
define('ZBX_FONT_NAME', 'graphfont');
define('ZBX_GRAPH_FONT_NAME',           'graphfont'); // font file name

#栗子，如perpetua字图PER.ttf
define('ZBX_FONT_NAME', 'PER');
define('ZBX_GRAPH_FONT_NAME',           'PER'); // font file name
```

<br>

**图形显示乱码，同样是用以上方法。在windowss上找一个中文字体上传到zabbix字体目录，并修改配置文件就可以了。**


<br>

Zabbix Web界面菜单：

![zabbix菜单](/images/Zabbix/zabbix_menu.png)

<br>

- 管理菜单，用于管理zabbix自身及zabbix相关设置；
- 配置菜单，用于配置监控相关设置；
- 报表菜单，为管理员生成一段时间内的监控统计信息；
- 检测中菜单，用于查看被监控的相关数据；
- 资产记录菜单，查看被监控的主机有哪些，以及相关的资产信息。


<br>

## 安装zabbix agent

Agent端安装也非常方便，直接在Client上安装两个包即可。

```sh

#配置zabbix源
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-1.el7.centos.noarch.rpm

#aliyun镜像
#rpm -ivh http://mirrors.aliyun.com/zabbix/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-1.el7.noarch.rpm

#安装
yum install -y zabbix-agent zabbix-sender

rpm -ql zabbix-agent
#/etc/zabbix/zabbix_agentd.conf
```

zabbix的“主动模式”与“被动模式”都在`/etc/zabbix/zabbix_agentd.conf`中定义。
配置最常用的agent端：
```sh
vim /etc/zabbix/zabbix_agentd.conf

####GENERAL PARAMETERS 通用配置
PidFile=
LogFile=


####Passive checks related 被动模式配置
#指定允许哪台服务器拉取本机数据
Server=
#指定agent端工作于被动模式时监听的端口号
ListenPort=10050(默认)
#指定agent端工作与被动模式时所监听的IP地址
ListenIP=0.0.0.0(默认)
#指定预生成的agent进程数量
StartAgents=


####Active checks related
#agent工作于主动模式时，将消息推送到哪台Server上
ServerActive=IP1,IP2...
#指定当前主机主机名，Server端通过对应的主机名识别主机
Hostname=
#指明agent端每隔多少秒将采集的数据发往Server端
RefreshActiveChecks=


#栗子
Server=192.168.1.9
ServerActive=192.168.1.9
Hostname=zabbix.me
```

<br>
**启动zabbix-agent**
```sh
systemctl zabbix-agent start

#查看状态,默认端口10050
netstat -nltp
```

![zabbix-agent状态](/images/Zabbix/zabbix-agent.png)




<br>
<br/>

---

# 快速开始zabbix-web菜单

zabbix-web界面中包含有**监测中、资产记录、报表、配置、管理**五项菜单。


<br>


## 登录和配置用户

在浏览器输入 [zabbix.me](zabbix.me) (修改hosts)，登录zabbix-web后台。

默认用户名：**Admin**，密码：**zabbix**。它是超级管理员。

为了防止暴力破解和词典攻击，连续尝试五次登录失败，zabbix界面将暂停30秒。

<br>

可以通过**管理(Management)**菜单下的**用户(User)**，新建、查看、管理用户信息。

zabbix在安装后自定义了两个用户：
- Admin用户是zabbix的超级管理员，拥有所有权限；
- Guest用户是一个特殊的默认用户。如果你没有登录，你访问zabbix的时候其实就是“guest”权限。guest默认没有任何权限。

你可以创建一个用户(user)并将其加入特定的用户组(Group)以提升用户权限。
![新建用户](/images/Zabbix/user.png)

可以仅用用户信息里面-报警媒介里面，自定义严重性的报警。只有勾选部分的报警信息才会发送过来。这也很棒！

如果存在严重性则使用：

- [ ] Not classified
- [ ] Information
- [ ] Warning
- [ ] Average
- [ ] High
- [ ] Disaster

![Use if severity](/images/Zabbix/Useifseverity.png)


<br/>
<br>


## 新建主机

zabbix中的主机(host)是一个你想要监控的网络实体(物理的、虚拟的)。对于主机的定义非常灵活。它可以是一台物理服务器，一个网络交换机，一个虚拟机或一些应用。

<br>
可以通过**配置(Configuration)**菜单下的**主机(Host)**，查看已配置主机相关信息。
默认有一个“Zabbix Server”的定义好的主机。


点击**创建主机(Create host)**后，填写对应的主机名称、添加对应的主机群组，zabbix-agent的IP地址和端口，以及其它信息。

![创建主机](/images/Zabbix/host.png)



<br>
<br>


## 新建监控项

监控项是zabbix中获得数据的基础。没有监控项，就没有数据。因为一个主机中只有监控项定义了”单一的指标“或者”需要获得的数据“。

<br>
可以通过**配置(Configuration)**菜单下的**主机(Item)**，找到需要配置**监控项(Item)**的主机，然后创建监控项。
主机默认是没有定义任何监控项的。

填写对应的监控名称、类型、键值、主机接口、信息类型等等信息。
![添加监控项](/images/Zabbix/create-item.png)


<br>
可在**监控(Monitoring)**菜单中**最新数据(Latest data)**查看之前定义的监控项和获得的值。
还可选择以**图形(Graph)**或**值**来查看监控项的相关信息。
![upload监控信息](/images/Zabbix/upload.png)

同样也还以在Zabbix-Server端获得数据信息：
```sh
#zabbix_get -s $ip -k $value
zabbix_get -s 192.168.1.9 -k system.cpu.load
```



<br>
<br>

## 新建触发器

监控项只用于收集数据。如果要自动评估收到的数据，我们则需要定义触发器(trigger)。
触发器包含了一个表达式，这个表达式定义了数据的可接受的阈值级别。

如果收到的数据超过了定义好的级别，触发器将被**触发**，或者进入**异常状态**(problem)。
从而引起我们的注意，让我们知道有问题发生。如果数据再次恢复到合理范围，触发器将会转到**正常状态**(OK)。

<br>

可以通过**配置(Configuration)**菜单下的**主机(Hosts)**选项，找到某主机的**触发器(Triggers)**创建触发器。

填写对应的触发器名称、表达式、描述等信息。
![创建触发器](/images/Zabbix/create-trigger.png)



<br>
<br>


## 获取问题通知

当监控项收集了数据后，触发器会根据异常状态触发报警。根据一些报警机制，它也会通知我们一些重要的事情，而不是直接在zabbix-web端进行查看。
这就是**通知(Notification)**的功能。
**E-mail**是最常用的异常通知发送方式。当然还有**SMS（短信），脚本**等媒体类型。

<br>

可以通过**管理(Administration)**菜单中的**报警媒体类型(Media types)**，点击预定义媒体类型列表中的Email，来配置Email。
![创建Email通知](/images/Zabbix/create-email.png)

为了建立一个通知，我们需要在**配置**菜单下**动作**中，创建**动作(Create action)**。
![创建动作](/images/Zabbix/create-action.png)

一旦满足了触发器的条件，变回触发执行动作。如收到E-mail等...



<br/>
<br>


## 新建模板

如果我们配置上前台主机，一些自动化操作会带来更多便利性。没错，**模板(templates)**功能就可以实现。
模板允许对有用的监控项、触发器和其他对象进行分组，只需要一步就可以对监控主机应用模板，已达到反复重用的目的。

当一个模板链接到一个主机后，主机会继承这个模板中的所有对象。简单而言，一组预先定义好的检查会被快速应用到主机上。

Zabbix为各种操作系统、设备以及应用准备好了一些预定义的模板。你可以快速部署使用他们。
**但是请注意，一些模板需要根据你的实际情况和使用环境进行适当俄调整。** 比如，一些检查项是不需要的，一些轮询周期过于频繁等。

<br/>
在**配置**菜单下的**模板(Templates)**下，点击**创建模板(Create template)**。填写对应的模板名称，群组等信息。
![创建模板](/images/Zabbix/create-template.png)

创建模板完毕后，可将模板链接到主机。之后，模板及其所有对象被添加到了主机。





<br>
<br>

---

# 配置(Configuration)


<br>


## 主机和主机组(Hosts and groups)

一般来讲，zabbix主机是指你希望监控的那些设备。如服务器、工作站、交换机等。
创建主机是使用zabbix过程的首要任务。

我们可以把主机组想象成项目组。根据不同的功能将主机划分到主机组是非常重要的，这样可以对以后创建的用户和用户组在定义权限的时候，不用给他们zabbix admin权限，而只需要根据主机组(项目组)给予用户和用户组对应项目(主机组)的权限即可。
这样很大程度上方便了Zabbix监控多个项目，也利于管理。同样，报警的时候也只会收到权限内的相关报警信息。


<br>

### 配置一台主机

配置--主机--创建主机--填写相关参数信息。

可以在已经存在的主机上使用 **Clone**或**Full Clone**创建一个新主机。

> Clone将保留所有的主机参数和模板链接；
> Full Clone将额外保留指数实体(应用集、监控项、触发器、视图、规则、Web场景)。

**新建主机下：**

1. 主机(Host)：包含了通用的主机属性；
2. 模板(Template)：允许将模板链接诶到主机，所有实体将从模板继承；
3. IPMI：包含IPMI管理属性；
4. 宏(Macros)：允许定义主机级别的**用户宏**；
5. 主机资产记录(Host inventory)：允许为主机收工输入库存信息；
6. 允许你请求与主机的加密的连接。


<br>

### 资产管理(Inventory)

你可以将联网设备的资产信息保存在zabbix里。
资产信息实在配置主机时人工录入建立的资产信息数据，或者通过使用某些自动填充选项完成的录入。

**构建资产库：**

- 手动模式： 在配置一台主机的时候，手动输入资产信息；
- 自动模式： 在配置主机的时候，选择自动。

之后便可以在**资产记录**菜单中的**概述，主机**项中查看相关信息。


<br/>

### 批量更新(Mass update)

有时候可能需要一次更改多个主机的某些属性，使用**批量更新(mass update)**功能来代替打开每个主机进行编辑。

![批量更新](/images/Zabbix/mass-update.png)

可批量处理**主机、模板、IPMI、资产、加密**相关信息。




<br/>
<br>

## 监控项(Items)

**监控项**是从主机收集的数据信息。
配置主机后，需要添加一些监控项以开始获取数据。快速添加多个监控项的一种方法是将预定义的模板附加到主机。

在单个监控项中，可指定从主机收集哪些数据信息。
为此，可使用**监控项key**。 如**system.cpu.load**将收集处理器负载的数据。
要给 key 指定更过参数，请在后面添加**方括号[]**。 如**system.cpu.load[avg5]**， 返回最近5分钟的CPU负载平均值。


<br>

### 创建一个监控项

可在主机中新建一个监控项。
不支持的监控项：如果由于某种原因无法检索该值，则该监控项可能不被支持。这些监控项仍然以固定的间隔重新检查。

**监控项的key:**

1. key名称允许使用字符： 0-9a-zA-Z_-.
2. key参数，用 逗,号 分隔： xxx[par1,par2...]
3. key参数也可以为空，此时使用默认值： key
4. key参数带引号，则允许任何Unicode字符，如果包含双引号则需要 \反斜杠 转义
5. key参数是一个数组，它需要包含在方括号中

<br>

**自定义间隔(Custom intervals)**

创建关于监控项的自定义时间规则。
灵活间隔被设计为重新定义默认监控项的的更新间隔，但调度间隔用于指定独立执行的检查计划。

**灵活的间隔(Flexible intervals)：**允许重定义特定时间段的默认间隔。

- 间隔(Interval)： 指定时间段的更新间隔；
- 期间(Period)： 灵活间隔有效的时间段；
- 举个栗子： 60(interval), 1-7,00-24(period)。监控项每隔60s检查一次。

<br/>

**调度间隔(Scheduling intervals)：**用于在特定时间检查监控项。

调度间隔定义为， `md<filter>wd<filter>h<filter>m<filter>s<filter>`。

- md: month days(1-31)
- wd: week days(1-7)
- h:  hours(0-23)
- m:  minutes(0-59)
- s:  seconds(0-58)
- <filter>: 指定其前缀的值----[from-to/step]。

其实类似于Linux中定时任务的写法，只不过这里把单位(md,wd,h,m,s)写在了数值的前面。

举个栗子：

```sh
md1-15	#1-15号

wd3	#星期三

h0-12	#上半天

m1,3,5,7,9	#每个1,3,5,7,9分钟

s/10	#每个10s

#组合体
wd1-5h9-18m/10	#每个工作日的上班时间每个10分钟
```


<br>

### 监控项类型(Items type)

监控项类型包含从系统获取数据的多种方式。每个监控项类型都有一组自己支持的监控项key和所需的参数。

zabbix提供的监控项类型：

1. zabbix代理检查(agent checks)
2. SNMP代理检查
3. SNMP traps
4. IPMI检查
5. 简单检查(simple checks)
6. VMware监控(monitoring)
7. 日志文件监控
8. 计算监控项(Calculated items)
9. zabbix内部检查(internal checks)
10. SSH检查
11. Telnet检查
12. 外部检查(External checks)
13. 汇总检查(Aggregate checks)
14. 捕捉器监控项(Trapper items)
15. JMX监控
16. ODBC监控


<br>

**zabbix代理(zabbix agent)：**
这些检查与zabbix代理进行通信实现数据的采集。

1. zabbix agent-passive： 被动模式，Server向Agent索要数据；
2. zabbix agent-active： 主动模式，Agent主动上报数据给Server。

可支持的监控项，可在新建监控项是在键值里面查看。


<br>

**SNMP代理(SNMP agent)：**

在启用SNMP的设备(如打印机，交换机，路由器...)上使用SNMP监控，为了能够监控SNMP代理在这些设备上提供的数据，zabbix服务器初始化配置时必须具有SNMP支持。
仅通过UDP协议执行SNMP检查。

**配置SNMP监控：**

1. 使用SNMP接口为设备创建一个主机；
2. 找出要监控项目的SNMP字符串；
3. 创建一个监控项。


<br>

**IPMI检查：**

你可以在zabbix中监控 **智能平台管理接口(IPMI)** 设备的运行状况和可用性。
要执行IPMI检查，zabbix服务器必须首先配置IPMI支持。


<br>

**简单检查：**

简单检查通常用于远程无代理监控服务。


<br>

**日志文件监控：**

zabbix可用于集中监控和分析 具有/不具有 日志转动能力的日志文件。
当日志文件包含某些字符串或字符串模式时，通知信息可用于警告用户。


<br/>

**计算监控项：**

计算监控项是创建虚拟数据源的一种方式。这些值将根据算术表达式定期计算。所有计算都由Server完成。


<br>
**内部检查：**
内部检查可以监控zabbix的内部检查。即Server或Agent Server的运行情况。


<br>
**SSH检查：**

运行SSH检查是作为无代理监控的，SSH检查不需要zabbix代理。
执行SSH检查zabbix服务器必须初始化配置为SSH2支持。

SSH检查提供两种身份验证方法，一种是用户/密码，另一种是基于密钥文件。

zabbix SSH 密钥配置:

```sh
vim /etc/zabbix/zabbix_server.conf

#SSHKeyLocation=
SSHKeyLocation=/home/zabbix/.ssh

usermod -m -d /home/zabbix zabbix
chown zabbix:zabbix /home/zabbix
chmod 700 /home/zabbix
cd /home/zabbix && su zabbix

ssh-keygen -t rsa
```


<br>

**外部检查：**

外部检查是由zabbix Server通过运行shell脚本或二进制的检查。
外部检查不需要再被监控的主机上运行任何代理。


<br/>

**汇总检查：**

在汇总检查中，zabbix通过直接从数据库中查询监控信息，然后进行信息聚合。
聚合检查不需要再被监控的主机上运行任何代理。


<br>

**捕捉器监控项：**

捕捉器监控项接收传入的数据，而不是查询它。对于想要推送到zabbix的任何数据都是适用的。

要使用捕捉器监控项，需要在zabbix中建立一个捕捉器监控项，将数据送给zabbix。


<br>

**JMX监控项：**

JMX监控可用于监视Java应用程序的JMX计数器。
JMX监视器以zabbix守护进程方式运行，名为zabbix java gateway。


<br/>

**ODBC监控：**

ODBC监控对应于zabbix web管理端中的数据库监控器监控项类型。
ODBC是用于访问 数据库管理系统(DBMS) 的C语言中间件API。

zabbix可以查询ODBC支持的任何数据库。为了实现监控，zabbix不直接连接到数据库，而是使用ODBC中设置的ODBC接口和驱动。
该功能允许为多个目的更加有效地监控不同的数据库。



<br/>

### 历史与趋势(history and trends)

历史与趋势是zabbix中存储数据的两种方式。
历史保持每个收集的值，而趋势是每小时的平均信息。

建议保持的历史数据尽可能少，但可以保留更多的趋势数据。



<br>

### 用户自定义参数(user parameter)

有时你想运行一个代理检查，但它不是zabbix预定义的。这时就能用到**用户参数**。
用户参数是由zabbix代理之星的命令，最多可以返回512KB的数据。
**key** 是唯一的。

用户参数用法：

```sh
UserParameter=<key>,<command>

#栗子
UserParameter=ping,echo 1
#使用ping键为一个监控项返回 1

#复杂栗子
UserParameter=mysql.ping,mysqladmin -uroot -ppwd ping | grep -c 'alive'
#mysqld状态为alive返回1，否则0

#灵活的用户参数
UserParameter=key[*],command
#[*]定义该key接受括号内的参数

#栗子
UserParameter=ping[*],echo $1

UserParameter=mysql.ping[*],mysqladmin -u$1 -p$2 ping | grep -c 'alive'
#mysql.ping[zabbix,passwd]

UserParameter=wc[*],grep -c "$2" $1
#wc[/etc/passwd,root]
```

<br>
**用户自定义参数扩展zabbix代理：**
是将key添加到被监控的主机哦！
```sh
#编写命令--SQL查询总数
mysqladmin -uxxx -pxxx status | cut -f4 -d":" | cut -f1 -d"S"

#将命令添加到zabbix_agentd.conf
vim /etc/zabbix/zabbix_agentd.conf

#找到如下字段
### Option: UserParameter
UserParameter=mysql.totalquery,mysqladmin -uroot -pxxx status | cut -f4 -d":" | cut -f1 -d"S"
#mysql.totalquery这个key是唯一的标识符

#测试此参数
##测试参数可用与否很重要哈
zabbix_agentd -t mysql.totalquery


#重启zabbix-agent，将重新加载配置
zabbix_get -s $host -k mysql.totalquery
```


<br>

### 可加载模块(loadable modules)
可加载模块提供了一种关于zabbix性能扩展的选项。

可加载模块基本上只zabbix守护程序使用的共享库，并在启动时加载。
可加载模块具有很多优点，卓越的性能和可实现任何逻辑的能力，更重要的是使用和共享了zabbix模块的开发能力。



<br/>

### windows性能计数器(windows perfomance counter)
使用perf_counter[]key有效的监控windows性能计数器



<br>

### 批量更新(mass update)
使用批量更新功能，可一次更改多个监控属性。



<br>

### 值映射(value mapping)
对于接收值更人性化的表示，可以使用包含数值和字符串之间的映射的**值映射**。

如：

- 0 ---> error
- 1 ---> true
- F ---> Full
- D ---> Differential
- I ---> Incremental
- ...



<br/>

### 应用集(Application)

**应用集**对逻辑组中的监控项进行分组。

如，对MongoDB的可用性，空间，负载，慢查询，执行命令...，可归于 MongoDB应用于中。



<br>

### 队列(queue)

**队列**显示正在等待刷新的监控项。
队列只是一个逻辑表达的数据。

队列显示的统计信息是zabbix服务器性能是否健康的指标。
在 管理--队列 下对去队列。



<br/>

### 值缓存(value cache)

为了计算触发表达式，以及让计算/聚合监控项和一些宏更快，zabbix服务器支持值的缓存选项。

在内存中的缓存可用于访问历史数据，而不用之间调用数据库。如果缓存中不存在历史值，则从数据库请求缺少的值，并相应地跟新缓存。

要启用值缓存功能，修改zabbix_server.conf中可选的ValueCacheSize参数。




<br>
<br/>


## 触发器(Trigger)

触发器是评估有项目采集的数据并表示当前系统状况的逻辑表达式。
触发器表达式允许定义一个什么状况的数据是“可接受”的阈值。如果超过了可接受状态，则触发器会被触发。


<br>

### 配置一个触发器(configuring a trigger)
在主机里面配置触发器。


<br/>

### 触发器表达式(trigger expression)

一个简单有效的表达式看起来像：

```sh
{<server>:<key>.<function>(<parameter>)}<operator><constant>

#如
{192.168.1.7:agent.ping.time()}=0
```

**函数参数(function parameters)：**

大多数数字型的函数接受秒数来作为参数。

```
#600s内所有值的总和
sum(600)

#随后5个值总和
sum(#5)

avg()
count()
last()
min()
max()

#5m 可被 300s 代替
#1k 代表 1024bytes
```

**运算符(operators)：**

优先级	|	运算符	|	定义
-	|	-	|	-
1	|	-	|	负号(minus)
2	|	not	|	逻辑非(NOT)
3	|	*, /	|	乘，除
4	|	+, -	|	加，减
5	|	<, <=, >, >=	|	-
6	|	=, <>	| 相等，不等于
7	|	and	|	逻辑与
8	|	or	|	逻辑或


**触发器示例：**

```sh
{www.zabbix.com:system.cpu.load[all,avg1].last()}>5

{www.zabbix.com:system.cpu.load[all,avg1].last()}>5 or {www.zabbix.com:system.cpu.load[all,avg1].min(10m)}>2

{www.zabbix.com:net.if.in[eth0,bytes].min(5m)}>100k

{$url1:net.tcp.service[smtp].last()}=0 and {$url2:net.tcp.service[smtp].last()}=0

{$host:icmpping.count(30m,0)}>5

{$host:system.cpu.load[all,avg1].min(5m)}>2 and {$hsot:system.cpu.load[all,avg1].time()}>000000 and {$host:system.cpu.load[all,avg1].time)()}<060000

...
```

**滞后(Hysteresis):**

有时候需要一个触发器状态OK和PROBLEM之间的间隔，而不是简单的阈值。

要做到这一点，我们首先定义一个PROBLEM事件的触发器表达式，然后为OK选择 ‘Recovery expression’，并未OK事件书如不同的表达式

如：

```
#Problem expression
{server:temp.last()}>20

#Recovery expression
{server:temp.last()}<=15

#两者之间便有了几个滞后值
```


<br>

### 触发器依赖(trigger dependency)

有时候，一台主机的可用性取决于另一台主机。如一台路由器后的上网设备。
这就是主机之间某些依赖关系可能有用的地方，依赖关系设置的通知可能会被抑制，而只发送根本问题的通知。

zabbix中触发器的依赖，一个触发器可能有多个依赖于它的触发器。

路由器和路由器后的Server同时宕机，如果有依赖关系，则zabbix不会执行服务器的触发动作。
值得注意的是，如果触发器所依赖的触发器被禁用，则次触发器的事件和动作将不会被抑制。


<br>

### 批量更新

使用批量更新，可一次更改一些触发器的某些属性。


<br/>

### 触发器严重性(trigger severity)

触发器严重性定义了触发器的重要程度:

1. 未分类(not classified), 灰色
2. 信息(information), 淡蓝
3. 警告(warning), 黄色
4. 一般严重(average), 橙色
5. 严重(High), 淡红
6. 灾难(disaster), 红色


<br>

### 自定义触发器严重性(customising trigger)

在 管理 -- 一般 -- 触发器严重性，里面自定义触发器严重性。


<br/>

### 预测触发功能(predictive trigger function)

有时候有即将到来的问题的迹象。可以发现这些迹象，以便提前采取行动，以减小影响。

zabbix具有基于历史数据预测受监视系统的未来行为的工具，这些工具通过预测触发功能实现。


<br>

### 事件标签(event tag)

在zabbix中可以自定义事件标签，在触发器级别上定义事件标签。在事件标签定以后，相应的新事件被标记为时间标签数据。
在拥有自定义时间标签的情况下，可以变得更加灵活。

例如：

1. 识别日志文件中的问题并单独关闭他们；
2. 用它来过滤通知；
3. 查看前端的事件标签信息；
4. 从项目值中提取的信息作为标签值；
5. 在通知中更好地识别问题；
6. 通过使用模板级别的标签来建华配置任务；
7. 使用低级别发现的标签创建触发器。



<br>
<br>

## 事件(Events)


zabbix可以生成一下几种类型的事件：
1. trigger events-触发器事件；
2. discovery events-发现事件；
3. auto registration events-自动注册事件；
4. internal events-内部事件；

事件以时间戳，并可以发送Email等基础动作。
在 监控-问题 里面查看信息信息。


<br>

### 触发器事件生成(trigger events generation)

触发器状态的变化是事件最常见和最重要的来源。每次触发器的状态改变时，都会生成一个事件。
改时间包含了触发器状态变更的详细信息、发生时间以及信息的状态。

触发器会创建两种类型的事件：问题(problem)和正常(OK)


<br>

### 手动关闭问题事件(manual closing of problems)

当触发器状态从“问题(problem)”变成“正常(OK)”时，很难判断是通过触发器表达式的方式解决。这时就需要手动解决。

只有在触发器中启用 “允许手动关闭” 选项，问题事件才可以被手动关闭。


<br>

### 其他事件来源(other event source)

zabbix定期扫描网络发现规则中定义的IP范围，可以为每个规则单独配置检查频率。一旦发现主机或服务，就会生成一个发现事件。

zabbix可以生成以下事件：

```
Service Up/Down
Host Up/Down
Service Discovered/Lost
Host Discovered/Lost
```



<br>
<br/>

## 事件关联(event correlation)

通常，在zabbix中正常事件会关闭所有的问题事件，但在某些情况下需要更细致的方法。可以根据**事件标签**关联问题事件。
如，当监控日志文件时，在日志文件中想要发现某些问题，并将它们单独关闭，而不是一起关闭。



<br>
<br/>

## 可视化(visualisation)

### 图形(graphs)

大量的监控数据被采集到zabbix中，如果能用可视化的表现形式来查看，那就直观和容易多了。

zabbix为用户提供了如下图形：
1. 监控项数据的内置简单图形 “simple graphs”；
2. 创建更复杂的自定义图形 “customised graphs”；
3. 特定图形 "ad-hosc graphs"快速访问几个监控项的数据比较。


**简单图形(simple graphs)：**
zabbix提供的简单图形，用来可视化显示监控项采集到的数据。并不需要配置就可以查看。

通过 监控-最新数据-图形 来展示图形。


**自定义图形(customised graphs)：**
自定义图形，提供定制功能。这就有点厉害了。这个是手动配置的。
可以为单个主机、多个主机、单个模板、多个模板创建自定义图形。

在 配置-主机-图形-创建图形 里编辑图形属性；
图形编辑后可点击预览。


**特设图形(ad-hoc graphs)：**
简单图形和自定义图形都不允许快速创建多个监控项目数据的比较图形，工作量小且没有维护。

在 检测-最新数据-旋转监控项前复选框-显示数据图(显示堆叠数据图) 下， 里面也包含了 正常和层积 的图形风格。



<br>
### 拓扑图(networking maps)

运维人员如果想要了解网络环境的基础设施状况，可以在zabbix中创建网络拓扑图。


**配置拓扑图(configurating network maps):**

在 监控-拓扑图 下，可以创建拓扑图。点击拓扑图中的 构造函数 选项，来打开编辑区域。
然后在编辑区域中添加元素和链接元素。


**链接指示器(link indicators):**
可以为网络拓扑图中的元素之间的链接分配一些触发器，当这些触发器状况为“Problem”时，可以在链接上体现出来。
如果多个触发器进入"Problem"状态，则严重程度最高的将决定链接的颜色和样式。


<br>
### 聚合图形(screen)

在zabbix的聚合图形页面上，你可把各种来源的信息聚集到一起，一边在单个屏幕上快速查看。
在 监测-图形聚合 下，对其进行创建、配置、管理和查看。

基本上，聚合图形是一个表格，你选择把每个表格有多少单元格以及其中要显示的元素。
元素如下：
- 简单图形；
- 简单图形原型；
- 用户自定义图形；
- 自定义图形原型；
- 拓扑图；
- 其他聚合图形；
- 纯文本信息；
- 服务器信息；
- 触发器信息；
- 主机/主机组信息；
- 系统状态；
- 数据概述；
- 时钟；
- 事件历史；
- 动作历史；
- URL。


<br>
### 幻灯片演示(slide shows)

在幻灯片演示中，可以配置多个聚合图形以设定的间隔逐个显示。
在 监测-聚合图形-幻灯片演示 下。



<br>
<br/>
## 模板(template)

模板是可以方便地应用于多个主机的一组实体。


**配置模板(configuring a template)：**
配置模板需要首先通过定义一些参数来创建模板，然后添加实例。
在 配置-模板-创建模板

**链接模板(linking)：**
链接是将模板应用于主机的过程，之后主机将拥有模板的所有实体。

**嵌套(nesting)：**
嵌套是一种包含一个或多个其它模板的模板方式。
可以在一个嵌套模板中奖一些模板链接在一起。

嵌套的好处在于，您只需要讲一个模板链接到主机，并且主机会自动继承链接的模板的所有实体。



<br>
<br>

## 事件通知(notifications upon events)

当配置了一些项目和触发器，并且由于触发器改变状态，现在正在发生一些事件，之后就要考虑 action。
发送通知是zabbix提供的主要操作之一。

为了能够发送和接收通知，必须：
1. 定义一些media；
2. 配置action，向指定的media发送消息。

action由condition和operation组成。当条件满足是，执行操作。
操作主要是 **发送消息**和**执行远程命令**。


<br>

### media类型

媒体是zabbix中发送通知和警报的传送通道。

**E-mail:**

在 管理-媒体类型 下，配置Email。

**SMS：**

zabbix支持使用连接到zabbix-server的串行端口的串行GSM调制解调器发送SMS消息。

确保：

* 串行设备的速度(在Linux下通常为/dev/ttyS0) 与 GSM调制解调器的速度相匹配。zabbix没有设置串行链路的速度，它使用默认设置。
* zabbix用户对串行设备有读写访问权限。
* GSM调制解调器输入PIN码，并在电源复位后保留PIN码。或者在SIM卡上禁用PIN。

管理-媒体类型下
要为用户分配电话号码：管理-用户-报警媒介，添加报警媒介(如电话号码等)

**Jabber：**

zabbix支持发送jabber消息。

**Ez Texting：**

可以使用 zabbix技术合作伙伴 Ez Texting发送信息。

**脚本：**

警报脚本在zabbix服务器上执行，这些脚本位于服务器配置文件中定义的目录中(AlertScriptsPath)。
```
cat /etc/zabbix/zabbix_server.conf

AlertScriptsPath=/usr/lib/zabbix/alertscripts


#创建报警脚本
vim /usr/lib/zabbix/alertscripts/zabbix_test.sh

#!/bin/bash
to=$1
subject=$2
body=$3


#可以同时给多个用户发送，用空格隔开
cat <<EOF | mail -s "$subject" "to"
$body
EOF
```

然后我们在创建脚本媒体的时候，写入相关参数。



<br>
### actions

可以根据所有支持的类型的时间定义操作：
- 触发事件：当trigger的状态从OK转到Problem或回转时；
- 发现事件；
- 自动注册事件；
- 内部事件；

配置-动作-创建动作

#### 条件(condition)
只有在事件与定义的条件匹配的情况下才执行操作。

注意运算类型：似与非似

#### 操作(operation)
操作：发送信息，执行远程命令。

##### 发送消息

##### 远程命令

(不支持在zabbix-agent上执行远程命令，需要在zabbix-server到代理的命令才能直接连接。远程命令限制255字符，可以将过个命令放置于新行上来执行过个命令。及时目标主机处于维护状态，也会执行远程命令).

配置-动作-操作，在操作细节中修改操作类型为远程命令。

在Zabbix代理（自定义脚本）上执行的那些远程命令必须首先在相应的命令中启用 zabbix_agentd.conf.确保 EnableRemoteCommands 参数设置为 1 并取消注释。 如果更改此参数，请重新启动代理守护程序。

```sh
vim /etc/zabbix/zabbix_agentd.conf


EnableRemoteCommands=1


cd /usr/lib/zabbix/alertscripts
#或修改zabbix-server.conf中的文件位置
vi sendmail.sh


chown zabbix.zabbix ./sendmail.sh && chmod a+x ./sendmail.sh
```

接下来在动作中选择为执行远程命令，并在相应位置输入命令。

![](/images/Zabbix/remotecommand.png)


支持自定义脚本、SSH、Telnet等方式。

**在信息中使用宏(using macros in messages)：**
在消息主题和消息文本中，可使用宏来更有效的问题报告。


**恢复操作(recovery operation):**
恢复操作允许在问题解决时通知我们。
恢复操作支持消息和远程命令。



<br>
<br>


## 宏(macros)

官方支持的宏的完整列表：<https://www.zabbix.com/documentation/3.4/manual/appendix/macros/supported_by_location>

zabbix支持许多在多种情况下使用的宏。宏是一个变量，由如下特殊语法标识。

宏类似于全局变量，宏是特别有用的，特别是在报警动作中。对于不同的细节加上特定的宏，能够使报警信息更加详细。

`{MACRO}`

根据在上下文汇总，宏解析为一个特殊的值。有效地使用宏可以节省时间，并使zabbix更加高效。

宏可以在监控项键值参数中使用。宏只能用在监控项键值参数的一部分中。
如`item.key[server_{HOST.HOST}_local]` 。


![宏](/images/Zabbix/MACRO.png)


<br>

### 宏函数(macro function)

宏函数能提供自定义宏值的功能。

宏函数语法：
```
{<macro>.<func>(<params>)}

#<macro>, 要定义的宏
#<func>, 要应用的函数
#<params>, 以逗号分隔的函数参数列表

#栗子
{{ITEM.VALUE}.regsub{pattern, output}}
```


<br>

### 用户宏(user macro)
除了支持开箱即用的宏之外，zabbix还支持更灵活的用户宏。

用户宏可在全局、模板和主机级别进行定义。有一个特殊语法：
```
{$MACRO}
```

用户宏可用于：
* 监控项名称；
* 监控项键值参数；
* 触发器名称和描述；
* 触发器表达式参数和常量；
* 许多其他位置。

### 自动发现宏(LLD)

有一种自动发现(LLD)函数中使用的宏类型，可用于创建监控项、触发器和图形原型。然后，当发现真实的文件系统、网络接口等，这些宏将替换为真实的值，并且以这些值来创建真实的监控项、触发器和图形。
```
{#MACRO}
```


<br>

## 用户和用户组(user and group)

zabbix中所有用户都通过web前端去访问zabbix应用程序。并为每一个用户分配唯一的登录名和密码，被加密储存于zabbix数据库中。

**配置用户(configuring user)**
管理-用户，创建和管理用户。

**权限(permission)**
可定义相应的用户类型，如用户，管理员和超级管理员。

**用户组(groups)**
管理-用户组，创建和配置用户组。





<br>
<br/>

---

# 服务监控(service monitoring)

服务监控，旨在帮助那些想要高级业务监控的人。
在很多情况下，我们关注的不是底层细节，而是提供的可用性服务。

服务是分层表示监控数据。

> IT
>> Workstations
>>> workstation1
>>> workstation2

>> Services

配置-服务，最高节点的服务是'root'。
你可以通过添加低级服务节点和各个节点服务创建下层层次结构。




<br>

---

<br/>


# Web监控(web monitoring)

配置-主机-web监测，创建或修改web监测信息。
可使用zabbix检查几个网站可用性方面。(zabbix中包含libcurl库才行)

要使用web监控，需要定义web场景。包括一个或多个HTTP请求或步骤。Zabbix-Server根据预定义的命令周期性的执行这些步骤。

Web监测中的**要求的字段(required string)**支持正则表达式，所以这对于检索页面信息很有用。这个真的很有用！

所有web场景会收集下列数据：

* 整个场景中所有步骤的平均下载速度；
* 失败的步骤数量；
* 最后一次错误信息

web场景的所有步骤，都会收集下列数据：

* 平均下载速度；
* 响应时间
* HTTP状态吗


<br>

## Web监控项(web monitoring items)

在创建web场景时，会自动添加一些新监控项进行监控。

创建场景后，zabbix会自动添加以下监控项进行监控，将它们链接到所选的应用程序。

- 场景<scenario>的下载速度；
- 场景<scenario>的失败步骤；
- 场景<scenario>的最后一个错误消息；

<br>

**举个栗子：**
```sh
##创建Web监测
#配置-主机-Web监测-创建web监测

URL：web.zabbix.me/monitor.php
要求的状态码：200
超时：20s


##创建web监测触发器
#配置-主机-触发器-创建触发器

严重性：一般严重
#触发条件：状态码!=200
表达式：N<>200


##创建触发报警对应的动作
#配置-动作-创建动作

#触发条件
触发器示警度=一般严重 or 触发器=web.zabbix.me

#操作：发送Email
发送给zabbix administrator用户群组
仅送到Email
默认信息/自定义信息


##在媒体类型中定义Email相关信息
#管理-报警媒体类型-Email

SMTP服务器：smtp.xxx.com
smtp端口：465
SMTP电邮：发件人Email
安全链接：SSL/TLS
认证：Usernameand passwd
用户名：xxx
密码： xxx


##接下来就可以测试接收报警Email了
```



<br>
<br/>

---

# 虚拟机监控(VM monitoring)

zabbix支持对VMware的监控，使用low-levle-discovery(LLD)自动发现VMware hypervisors和虚拟机，并根据事先定义的主机原型，为这些虚拟机建立主机，添加监控。

zabbix中提供了几个模板，可以直接用来解控VMware vCenter 或 ESX hypervisor。


<br>
虚拟机监控分为两个步骤：
- 首先，zabbix是通过VMware collector进程来监控虚拟机。这些进程通过SOAP协议从VMware服务获取必要的信息，对其进行预处理并储存到zabbix-server共享内存中；
- 然后，zabbix-pollers通过zabbix简单检查VMware keys来检索这些数据。

要使虚拟机监控正常工作，需要libxml2库和libcurl库的支持。

> 配置-自动发现-创建自动发现
> 配置-主机-自动发现




<br>
<br/>

---

# 维护(maintenance)

可在zabbix中为主机和主机组定义维护周期。
有两种维护类型：“继续对目标进行监控数据的收集” 和 “停止对目标进行监控数据的收集”

要在维护期间正常接收问题通知，必须在动作配置中的选项中取消选择暂停操作。
为了确保定期维护按照预期的时间进行，需要对zabbix的所有部分使用通用时区。

> 配置-维护-创建维护期

维护期的主机显示的是橙色背景！




<br>
<br>

---

# 事件确认(event acknowledgment)

zabbix中的问题事件可以由用户确认。

如果用户获得了有关问题时间的通知，可以访问zabbix前端，从时间导航到确认屏幕并确认问题。
当他们确认时，可输入评论或其他一些相关描述。
这样其他系统用户同样的问题，他们便会立即看到是否已被解决和目前的评论。

以这种方式，可以更协调的进行解决多个系统用户的问题的工作流程。

要确认事件，用户必须至少要有对相应触发器的读取权限。


<br>
在Dashboard下，在出现的问题里，点击确认，进入确认事件。
也可在监控-问题下查看问题详细信息。




<br>
<br>

---

# 配置导出/导入(Configuration export/import)

zabbix导入/导出功能，使得可以在一个zabbix系统与另一个zabbix系统之间交换各种配置实体。
类似于数据库的导入导出。即也可以对zabbix做备份。

可导出/导入的对象有：主机组； 模板； 主机； 拓扑； 图片； 聚合图形； 值映射。

数据也可导出：
- XML - 在前端
- XML or JSON - 在zabbix API

导出的详细信息：
- 所有支持的元素都导出到一个文件中；
- 不导出从连链接模板继承的主机和模板实体；
- 由低级别发现创建的实体依赖于他们的任何实体不会导出。

导入详细信息：
- 第一次遇到错误停止导入；
- 导入支持XML和JSON文件；
- 使用“删除缺失”选项导入主机/模板时，导入的XML文件中不存在主机/模板宏也将被删除。





<br>
<br>

---

# 将Zabbix展现在Nginx上

毕竟现在Nginx用的多，那就把Apache换成Nginx吧！

Nginx仓库:<http://nginx.org/packages/>

自己安装Nginx:
1. 下载`nginx-release-xx.rmp`仓库源来安装；
2. 手动创建`/etc/yum.repo.d/nginx.repo`；
3. 直接下载`ngix.rpm`来安装；
4. 直接下载源码来安装。


相较于Apache，Nginx也只是配置个server就行了。优化什么的自己弄。
```sh
vim /etc/nginx/conf.d/zabbix.conf

server {
	listen 80;
    server_name zabbix.me;
    root /usr/share/zabbix;

    access_log  /var/log/nginx/zabbix.access.log  main;

	allow 127.0.0.1;
	allow Your-IP;
    deny all;

    location / {
    	if (!-f $request_filename) {
        	rewrite ^([^\?]+)$ /index.php?1=$1 last;
        }
    }

    location ~ \.php$ {
    	root /usr/share/zabbix;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
        include fastcgi_params;
    }
}


nginx -t
systemctl start nginx
```

下载就可以正常访问`zabbix-web`端了!





<br>
<br/>

---

# Zabbix监控

Zabbix自带的templates基本涵盖了大部分监控信息。

大部分操作系统：
- OS Linux;
- OS AIx;
- OS FreeBSD;
- OS Solaris;
- OS Windows;
- ...

大部分服务：
- CPU;
- Filesystems;
- HTTP/HTTPS service;
- Memory;
- Network interfaces;
- Processes;
- Secutity;
- Zabbix server/agent/Proxy;
- SMTP,POP,SSH,NTP, service;
- ICMP Ping;
- SNMP;
- ...

虚拟机：
- VM VMware;
- VM WMware Hypervisor;
- ...

网络设备：
- Cisco;
- Huawei;
- TPLink;
- HP;
- ...



<br>
除了Zabbix自带的templates，你还可以下载templates并导入zabbix-server。

例如PHP-FPM, MongoDB, Apache, Nginx, Redis等额外软件的监控就需要下载额外templates。





<br>
<br/>

## 监控MySQL

### 使用Zabbix自带模板监控MySQL

Zabbix默认带有MySQL的监控和模板，所以无需再去下载。不过需要配置用户，密码，主机，端口等信息。

```sh
vim /etc/zabbix/zabbix-agentd.d/userparameter_mysql.conf

#For all the following commands HOME should be set to the directory that has .my.cnf file with password information.
#这句话叫我们新建一个带有mysql密码信息的.my.cnf文件
#并把此配置文件里面的HOME改为.my.cnf所的在目录
#.my.cnf文件里面的用户要对MySQL数据库有权限才行，没有权限请记得加

[mysql]
host=localhost
user=zabbix
password=zabiix
socket=/var/lib/mysql/mysql.sock

[mysqladmin]
host=localhost
user=root
password=password
socket=/var/lib/mysql/mysql.sock



#测试
zabbix_get -s 127.0.0.1 -k mysql.ping
#1
```




<br/>
<br>


### 使用Percona插件监控MySQL

Zabbix默认带有MySQL的监控和模板，所以无需再去下载。不过需要配置用户，密码，主机，端口等信息。
但是Zabbix自带的MySQL监控太简陋了。
所以使用Percona提供的模板及监控。

<br>

Percona Monitoring Plugins-URL:  <https://www.percona.com/downloads/percona-monitoring-plugins/LATEST/>
Percona Monitoring Plugins for Zabbix- Instructions:  <https://www.percona.com/doc/percona-monitoring-plugins/LATEST/zabbix/index.html>

此插件地址需要我们选择Percona-Version和Software平台。

![Percona Monitoring Plugins](/images/Zabbix/mysql01.png)

选择平台后，我们只需安装zabbix的rpm包就好：

```sh
#安装rpm包
yum install -y https://www.percona.com/downloads/percona-monitoring-plugins/percona-monitoring-plugins-1.1.7/binary/redhat/7/x86_64/percona-zabbix-templates-1.1.7-2.noarch.rpm


#安装软件
#注意php版本问题
yum install -y percona-zabbix-templates

ls /var/lib/zabbix/percona
#scripts目录有.sh脚本文件
#templates目录有配置文件和模板文件



#复制配置文件
cp /var/lib/zabbix/percona/templates/userparameter_percona_mysql.conf /etc/zabbix/zabbix_agentd.d/
#我看了一下，这个配置文件和zabbix自带的MySQL配置文件一样


#添加MySQL的相关信息
vim /var/lib/zabbix/percona/scripts/ss_get_mysql_stats.php

$mysql_user = 'root';
$mysql_pass = 'password';
$mysql_port = 3306;
$mysql_socket = '/var/lib/mysql/mysql.sock';
$mysql_flags = 0;


#测试脚本
/var/lib/zabbix/percona/scripts/get_mysql_stats_wrapper.sh gg
#10


#创建.my.cnf文件
vim /etc/zabbix/zabbix_agentd.d/.my.cnf

[mysql]
host=localhost
user=root
password=password
socket=/var/lib/mysql/mysql.sock

[mysqladmin]
host=localhost
user=root
password=password
socket=/var/lib/mysql/mysql.sock

[client]
host=localhost
user=root
password=password
socker=/var/lib/mysql/mysql.sock


#重启服务
systemctl restart zabbix-agent


#测试
sudo -u zabbix -H /var/lib/zabbix/percona/scripts/get_mysql_stats_wrapper.sh running-slave
#0/1
```

导入模板，模板文件位于：`/var/lib/zabbix/percona/templates/zabbix_agent_template_percona_mysql_server_ht_2.0.9-sver1.1.7.xml`

但我直接导入模板时报错——标签无效 "/zabbix_export/date": "YYYY-MM-DDThh:mm:ssZ" 预计。
此模板需要先导入Zabbix2.4后再导出，然后再导入到Zabbix3.4。太麻烦。
![导入percona模板](/images/Zabbix/mysql02.png)

所以需要下载修改过的模板： <http://jaminzhang.github.io/soft-conf/Zabbix/zbx_percona_mysql_template.xml>

```sh
wget http://jaminzhang.github.io/soft-conf/Zabbix/zbx_percona_mysql_template.xml
```

下载之后导入模板，然后链接主机。
链接之后可以部分监控可能显示不支持的。

如：Received value [rm: 无法删除"/tmp/localhost-mysql_cacti_stats.txt": 不允许的操作0] is not suitable for value type [Numeric (float)]
没有权限。
![](/images/Zabbix/mysql03.png)

解决办法：

```sh
cd /tmp
chown -R zabbix:zabbix localhost-mysql_cacti_stats.txt

systemcet restart zabbix-agent
```







<br>
<br>

## 监控MongoDB

**感谢大神：**
- MongoDB-templates: <https://share.zabbix.com/databases/mongodb/mongodb-for-zabbix-3-2> ;
- GitHub: <https://github.com/oscm/zabbix/tree/master/mongodb>
- 此github-repo中还包含了`Oracle`, `php-fpm`, `postfix`, `redis`, `Nginx`。可参看README.md来配置zabbix对它们的监控。

<br/>

![./master](/images/Zabbix/zabbix_master.png)


<br>
### 安装步骤

**1. 在zabbix-agent安装`jq`**
jq - Command-line JSON processor;

```sh
yum install -y jq
```

<br>
**2. 在zabbix-agent的MongoDB中创建用于监控的账号**
创建用于读取MongoDB相关信息的账户及其权限。

```sh
mongo

>use admin

>db.createUser(
  {
	user:'zabbix',
	pwd:'zabbix',
	roles:[{
		role:'clusterMonitor',
		db:'admin'}]
  }
)
```

<br>
**3. 在agent下载github仓库的MongoDB模板等文件**

```sh
wget https://codeload.github.com/oscm/zabbix/zip/master -O master.zip
#这里面不仅仅有mongodb，还有redis,php等。
#我们只需要进入mongodb目录就好

unzip master.zip


cd ./zabbix-master/mongodb
ls

#mongodb.sh , 执行脚本
#userparameter_mongodb.conf ，配置脚本
#zbx_export_templates.xml，zabbix模板文件
```

![./master/mongodb](/images/Zabbix/zabbix_mongodb1.png)


<br>
**4. 移动并配置mongodb.sh**

```sh
cp ./mongodb.sh /etc/zabbix

chmod a+x /etc/zabbix/mongodb.sh


vi mongodb.sh

#如果HOST,PORT不是默认，请修改
DB_HOST=127.0.0.1
DB_PORT=27017
DB_USERNAME=zabbix
DB_PASSWORD=zabbix
```

![mongodb.sh](/images/Zabbix/zabbix_mongodb2.png)


<br>
**5. 移动并修改userparameter_mongodb.conf**

```sh
cp ./zabbix-master/userparameter_mongodb.conf /etc/zabbix/zabbix_agentd.d

vi ./userparameter_mongodb.conf

UserParameter=mongodb.status[*],/etc/zabbix/mongodb.sh $1 $2 $3 $4 $5
#修改为mongdb.sh真实位置
#这个是用户自定义的参数，可以之间写入到zabbix_agent.conf里面
```

![userparameter_mongodb.conf](/images/Zabbix/zabbix_mongodb3.png)


<br>
**6. 重启zabbix-agent**

```sh
systemctl restart zabbix-agent
```


<br>
**7. 在zabbix-web导入mongodb模板**

- 配置-模板-导入模板；
- 选择`./master/mongodb/zbx_export_templates.xml`模板文件，并导入；
- 接下来便可以在 templates中看到"Template App MongoDB"这个模板；
- 可将此模板链接到某个主机上监控，并到最新数据里查看相关MongoDB信息；
- 如果相对此模板就行修改，可编辑zbx_export_templates.xml文件。

<br>

![导入mongodb模板](/images/Zabbix/zabbix_web_import_mongodb1.png)
![选择导入文件](/images/Zabbix/zabbix_web_import_mongodb2.png)
![查看MongoDB模板](/images/Zabbix/zabbix_web_import_mongodb3.png)
![在主机中关联MongoDB模板](/images/Zabbix/zabbix_web_import_mongodb4.png)
![查看最新MongoDb数据](/images/Zabbix/zabbix_web_import_mongodb5.png)

<br/>

### 监控一台主机上的额外mongod实例

由于可能一台主机上运行的mongod实例不止一个，所以我们需要修改一下前面下载的配置文件，用以监控其它端口的mongod实例。


- 此处假设默认的mongod实例运行在27017端口上
- 另外还有一个mongod实例运行在27018端口上
- 此处假设我们已经完成了前面对27017mongodb的监控了


操作：

```sh
cd /etc/zabbix
cp mongodb.sh mongodb_27018.sh

vim ./mongodb_27018.sh
#配置监控的mongodb账号和端口

DB_HOST=127.0.0.1
DB_PORT=27018
DB_USERNAME=zabbix
DB_PASSWORD=zabbix

#现在就有了提取27017/27018两个mongodb实例的脚本
#mongodb.sh
#mongodb_27018.sh



cd ./zabbxi-agentd.d

vim userparameter_mongodb.conf
#在默认的27017下面添加一行提取mongodb_27018信息的脚本

UserParameter=mongodb.status[*],/etc/zabbix/mongodb.sh $1 $2 $3 $4 $5
UserParameter=mongodb_27018.status[*],/etc/zabbix/mongodb_27018.sh $1 $2 $3 $4 $5

#现在zabbix-server端就可以同时获取27017/27018两个mongodb实例的信息
#但是Web界面还不能直接显示出来，因为27018的键值和默认不相同
#没错，就是上面我们修改的 mongodb_27018.status[*]
```

<br>

![mongodb_27018.sh](/images/Zabbix/mongodb_27018.png)

![mongodb_27018.sh](/images/Zabbix/mongodb_27018_02.png)

<br>

**接下来要在Zabbix-Web端配置监控项用以提取信息**

我们先找到一个默认的MongoDB自带的配置模板，如`MongoDB Connections current`，点进去查看它的键值对为`mongodb.status[connections,current]`

因此我们只需要修改为我们配置文件里面的`mongodb_27018.status[*]`就可以了。

![mongodb_27018.sh](/images/Zabbix/mongodb_27018_03.png)

![mongodb_27018.sh](/images/Zabbix/mongodb_27018_04.png)

<br>

其余个监控项以此类推，我觉得其他服务也应该可以如此。


**你也可以对此建立一个单独的模板，如MongoDB_27108 templates。在此监控模板下创建上面的监控项。这样就可以对所有主机生效了。也可以批量化操作，更方便一些。**

下面是我的参考**Template App MongoDB**模板建立的**Template App MongoDB_27018**

![mongodb_27018.sh](/images/Zabbix/mongodb_27018_05.png)

![mongodb_27018.sh](/images/Zabbix/mongodb_27018_06.png)












<br/>
<br>


## 监控PHP-FPM

同样使用上面大神的模板。

步骤和监控MongoDB类似：

```sh
#进入下载的文件目录
cd ./zabbix-master/php-fpm
cp ./php-fpm.xml.sh /etc/zabbix
chmod a+x /etc/zabbix/php-fpm.xml.sh
vim /etc/zabbix/php-fpm.xml.sh

#如果这三个参数修改了，请修改
#因为是使用culr，所以请允许此IP能够访问此页面
#另外还要Nginx允许Server-IP访问哦，不然无法读取数据
#我测试的时候用IP无法获取数据，所以用的域名
#如果没做域名解析，请加本地hosts
#php-fpm_status使用我修改的

HOST="localhost"
PORT="80"
#status="status"
status="php-fpm_status"






cp ./userparameter_php-fpm.conf /etc/zabbix/zabbix_agent.d/
#当然也可以把这个用户自定义参数写入zabbix_agent.conf
#修改自定义参数里面的文件位置
vim /etc/zabbix/zabbix_agent.d/userparameter_php-fpm.conf

UserParameter=php-fpm.status[*],/etc/zabbix/php-fpm.xml.sh $1




#php-fpm，nginx的状态必须用Nginx展现，Zabbix-Server是使用curl提取状态页面的信息
vim /etc/nginx/conf.d/zabbix.conf

server {
	listen 80;
    server_name zabbix.me localhost;
#如果localhost与其他配置文件冲突，那就用IP
#server_name zabbix.me 127.0.0.1 Private-IP Public-IP;
    root /usr/share/zabbix;

    access_log  /var/log/nginx/zabbix.access.log  main;

#allow无法使用localhost，所有内外网要分开写
	allow 127.0.0.1;
	allow Private-IP;
	allow Public-IP;
	allow Zabbix-Server-IP;
	allow Remote-View-IP;
	deny all;

    location / {
    	if (!-f $request_filename) {
			rewrite ^([^\?]+)$ /index.php?1=$1 last;
        }
    }


#Nignx_Status
    location /nginx_status {
        stub_status on;
        #开启nginx自带的状态检查功能
        access_log off;
    }


#php-fpm_Status
#php-fpm的默认状态页面是/status,/ping。我修改了一下。
    location ~ ^/php-fpm_(status|ping)$ {
    	access_log off;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
        include fastcgi_params;
    }


    location ~ \.php$ {
    	root /usr/share/zabbix;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

![监控php-fpm](/images/Zabbix/zabbix-master_php-fpm.png)


<br/>

**php-fpm状态页面的配置文件**

```sh
vim /etc/php-fpm.d/www.conf

#说明和用法如下，我做简单修改
#修改默认值
;pm.status_path = /status
pm.status_path = /php-fpm_status

;ping.path = /ping
ping.path = /php-fpm_ping
;ping.response = pong
ping.response = 200

#用法
zabbix.me/php-fpm_status
zabbix.me/php-fpm_ping

#配置文件提供了格式化输出
zabbix.me/php-fpm_status?html
zabbix.me/php-fpm_status?html&full



; output syntax. Example:
;   http://www.foo.bar/status
;   http://www.foo.bar/status?json
;   http://www.foo.bar/status?html
;   http://www.foo.bar/status?xml
;   http://www.foo.bar/status?full
;   http://www.foo.bar/status?json&full
;   http://www.foo.bar/status?html&full
;   http://www.foo.bar/status?xml&full


#修改完毕后重启服务
systemctl restart php-fpm nginx







#具体看下面描述
##这下面是说明

; The URI to view the FPM status page. If this value is not set, no URI will be
; recognized as a status page. It shows the following informations:
;   pool                 - the name of the pool;
;   process manager      - static, dynamic or ondemand;
;   start time           - the date and time FPM has started;
;   start since          - number of seconds since FPM has started;
;   accepted conn        - the number of request accepted by the pool;
;   listen queue         - the number of request in the queue of pending
;                          connections (see backlog in listen(2));
;   max listen queue     - the maximum number of requests in the queue
;                          of pending connections since FPM has started;
;   listen queue len     - the size of the socket queue of pending connections;
;   idle processes       - the number of idle processes;
;   active processes     - the number of active processes;
;   total processes      - the number of idle + active processes;
;   max active processes - the maximum number of active processes since FPM
;                          has started;
;   max children reached - number of times, the process limit has been reached,
;                          when pm tries to start more children (works only for
;                          pm 'dynamic' and 'ondemand');
; Value are updated in real time.
; Example output:
;   pool:                 www
;   process manager:      static
;   start time:           01/Jul/2011:17:53:49 +0200
;   start since:          62636
;   accepted conn:        190460
;   listen queue:         0
;   max listen queue:     1
;   listen queue len:     42
;   idle processes:       4
;   active processes:     11
;   total processes:      15
;   max active processes: 12
;   max children reached: 0
;
; By default the status page output is formatted as text/plain. Passing either
; 'html', 'xml' or 'json' in the query string will return the corresponding
; output syntax. Example:
;   http://www.foo.bar/status
;   http://www.foo.bar/status?json
;   http://www.foo.bar/status?html
;   http://www.foo.bar/status?xml
;
; By default the status page only outputs short status. Passing 'full' in the
; query string will also return status for each pool process.
; Example:
;   http://www.foo.bar/status?full
;   http://www.foo.bar/status?json&full
;   http://www.foo.bar/status?html&full
;   http://www.foo.bar/status?xml&full
; The Full status returns for each process:
;   pid                  - the PID of the process;
;   state                - the state of the process (Idle, Running, ...);
;   start time           - the date and time the process has started;
;   start since          - the number of seconds since the process has started;
;   requests             - the number of requests the process has served;
;   request duration     - the duration in µs of the requests;
;   request method       - the request method (GET, POST, ...);
;   request URI          - the request URI with the query string;
;   content length       - the content length of the request (only with POST);
;   user                 - the user (PHP_AUTH_USER) (or '-' if not set);
;   script               - the main script called (or '-' if not set);
;   last request cpu     - the %cpu the last request consumed
;                          it's always 0 if the process is not in Idle state
;                          because CPU calculation is done when the request
;                          processing has terminated;
;   last request memory  - the max amount of memory the last request consumed
;                          it's always 0 if the process is not in Idle state
;                          because memory calculation is done when the request
;                          processing has terminated;
; If the process is in Idle state, then informations are related to the
; last request the process has served. Otherwise informations are related to
; the current request being served.
; Example output:
;   ************************
;   pid:                  31330
;   state:                Running
;   start time:           01/Jul/2011:17:53:49 +0200
;   start since:          63087
;   requests:             12808
;   request duration:     1250261
;   request method:       GET
;   request URI:          /test_mem.php?N=10000
;   content length:       0
;   user:                 -
;   script:               /home/fat/web/docs/php/test_mem.php
;   last request cpu:     0.00
;   last request memory:  0
;
; Note: There is a real-time FPM status monitoring sample web page available
;       It's available in: @EXPANDED_DATADIR@/fpm/status.html
;
; Note: The value must start with a leading slash (/). The value can be
;       anything, but it may not be a good idea to use the .php extension or it
;       may conflict with a real PHP file.
; Default Value: not set
;pm.status_path = /status
pm.status_path = /php-fpm_status

; The ping URI to call the monitoring page of FPM. If this value is not set, no
; URI will be recognized as a ping page. This could be used to test from outside
; that FPM is alive and responding, or to
; - create a graph of FPM availability (rrd or such);
; - remove a server from a group if it is not responding (load balancing);
; - trigger alerts for the operating team (24/7).
; Note: The value must start with a leading slash (/). The value can be
;       anything, but it may not be a good idea to use the .php extension or it
;       may conflict with a real PHP file.
; Default Value: not set
;ping.path = /ping
ping.path = /php-fpm_ping

; This directive may be used to customize the response of a ping request. The
; response is formatted as text/plain with a 200 response code.
; Default Value: pong
;ping.response = pong
ping.response = 200
```

<br/>

效果图：

![将php-fpm状态展现在Nginx上](/images/Zabbix/zabbix-master_php-fpm2.png)

<br>

展现的话是在Agent端的Nginx上，这个更直观一些。
而Zabbix-Server就是通过`curl -s zabbix.me`来获取数据的，并通过对数据的提取来返回给Zabbix-Server。
所以收集php-fpm，nginx的信息状态，都是基于这个页面的。

现在导入PHP-FPM模板，导入操作同MongoDB。

```
#就是这个文件
zbx_export_templates.xml
```

导入模板后，直接链接模板就可以啦。
然后就可以使用了。



<br/>
<br/>

## 监控Nginx

Zabbix是通过`stub_status`模块实现对Nginx的监控。
Nginx的ngx_http_stub_status_module模块提供了基本的Nginx状态信息，源码安装的话需要加上–with-http_stub_status_module编译参数，如果是epel源yum安装的话，已经默认启用该模块。

在Nginx配置文件中加入如下配置：

```sh
location /nginx_status {
	allow IP;
	deny all;
	stub_status on;
    access_log off;
}



#栗子
Active connections: 14
server accepts handled requests
 22889 22889 72510
Reading: 0 Writing: 2 Waiting: 12
```

![nginx_status栗子](/images/Zabbix/nginx_status.png)

<br/>

**一些状态信息**

- Active connections
当前active client的连接数，包括`Wating`

- accepts
接受的客户端连接总数

- handled
已处理的连接总数。通常，handled与accepts相同，除非已达到了资源限制(如worker_connections限制)

- requests
客户端请求总数

- Reading
当前nginx正在读取request header的连接数

- Writing
当前Nginx将reponse写回客户端的连接数

- Waiting
当前等待请求的空闲客户端的连接数

<br>

上面的结果还可通过命令来查看

```
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a,S[a]}'
```

<br>

以上数据是通过Web端查看。
但，我们需要把数据收集到Zabbix-Server。还需要使用之前下载同MongoDB，php-fpm一起的那个包。


**操作，基本还是类似MongoDB，php-fpm。只是个别参数需要修改一下。**

```
cd ./zabbix-master/nginx/
cp ./nginx.sh /etc/zabbix/
chmod a+x /etc/zabbix/nginx.sh

cp ./userparameter_nginx.conf /etc/zabbix/zabbix_agentd.d


vim /etc/zabbix/nginx.sh

#HOST="localhost"
PORT="80"
#stub_status=stub_status
stub_status=nginx_status



vim /etc/zabbix/zabbix_agentd.d/userparameter_nginx.conf

#修改成脚本对应的位置
UserParameter=nginx.status[*],/etc/zabbix/nginx.sh $1
```

现在想以前一样导入模板。
然后在链接模板就可以了。




<br>
<br/>

## 监控Redis

监控Redis，也是把包里面对应的文件复制过去就行。

```sh
cd ./zabbix-master/redis
cp ./userparameter_redis.conf /etc/zabbix/zabbix_agentd.d/


#如果redis设置有密码，请加上密码
#如果是不同的端口，请修改
UserParameter=redis.local[*],redis-cli -h 127.0.0.1 -p 6379 info|grep $1|grep -v _human|cut -d : -f2
#UserParameter=redis.local[*],redis-cli -h 127.0.0.1 -p 6379 -a Password info|grep $1|grep -v _human|cut -d : -f2
UserParameter=redis.status[*],redis-cli -h $1 -p $2 -a Password info|grep $3|grep -v _human|cut -d : -f2
UserParameter=redis.proc,pidof redis-server | wc -l



#重启服务
systemctl restart redis
```

导入模板，链接主机，OK。




<br/>

---

<br/>


# 系统监控


<br>


## CPU

CPU的性能状态信息：

简写 | 描述 | 说明
- | - | -
us | user cpu tim | 用户使用CPU时间
sy | system cpu time | 系统使用CPU时间
id | idle cpu time | CPU的空闲时间
wa | io wait cpu time | CPU等待IO时间
ni | user nice cpu time | 用nice调整进程优先级的CPU时间
st | steal time | 虚拟机偷取的CPU时间比，被强制等待虚拟CPU的时间
si | softirq time | 系统处理软件中断所花费的CPU时间
hi | hard time | 系统硬中断所花费的CPU时间
interrupt | 中断 | 被处理过的中断数
cs | Context switches | 上下文切换
ql | processor queue length | 队列长度
processor load | processor load |  处理器负载，几核乘以几


<br/>

### Tips

- 当我们要监控并报警CPU使用率时，我们可以反过来用CPU空闲时间来定义
- cpu idle tiem%  + cpu usage time% = 1
- (CPU usage time% gt 80%) == (CPU idel time% lt 20%)
- (CPU usage time% gt 90%) == (CPU idel time% < 10%)
- 所以监控CPU使用率就可以监控CPU空闲时间，并依据这个报警



<br>
<br/>



## 内存

Zabbix中自带的Linux OS模板提供了`Total memory`和`Available memory`选项，这两者直接用模板就可以了。
但没有提供内存使用率的选项，因此需要我们自定义。

- 内存使用率 = 可用内存 / 总内存
- ast(vm.memory.size[available])/last(vm.memory.size[total])


### 自定义内存使用率

我们只需要在Linux OS模板下配置内存使用率，就可以一劳永逸。

1. 配置(Configuration)
2. 模板(Templates)
3. OS Linux模板的监控项(Items)
4. 创建监控项
5. 监控项名称: Available memory percent
6. 类型： 可计算的
7. 键值： vm.memory.size[percent]
8. 公式： 100*last(vm.memory.size[available])/last(vm.memory.size[total])
9. 记得将其加入Memory应用集，这样便于查找和管理
10. 可加入单位： %


<br/>

![自定义可用内存率](/images/Zabbix/custom_memorypercent.png)

![自定义可用内存率](/images/Zabbix/custom_memorypercent2.png)

![效果图](/images/Zabbix/custom_memorypercent3.png)

![效果图](/images/Zabbix/custom_memorypercent4.png)


<br>

### 添加触发器

1. 配置
2. 模板
3. OS Linux模板
4. 触发器
5. 创建触发器，当可用内存率在三分钟内的平均值小于20%时报警
6. 名字：Available memory percent lt 20% on {HOST.NAME}
7. 严重性：一般严重
8. 表达式： {Template OS Linux:vm.memory.size[percent].avg(3)}<20

<br/>

![自定义可用内存率报警](/images/Zabbix/custom_memorypercent5.png)

![效果图](/images/Zabbix/custom_memorypercent6.png)



<br/>
<br/>


## 磁盘

由于Zabbix-Server自带的Linux OS模板中的filesystem的监控是一个自动发现规则，而在应用集中的filesystem是没有监控项的。所有对于磁盘的监控和触发要在自动发现规则中去定义。

<br/>

![监控项](/images/Zabbix/custom_disk1.png)

![触发器](/images/Zabbix/custom_disk2.png)

![触发器](/images/Zabbix/custom_disk3.png)

![触发器](/images/Zabbix/custom_disk4.png)



<br>
<br/>


## 进程和端口

Zabbix-Server自带有检测进程和端口的键值对。



### 检测进程数

`proc.num[<name>,<user>,<state>,<cmdline>]`

- name: 进程名；
- user: 运行该进程的用户；
- state:
	1. run
	2. sleep
	3. zomb
- cmdline: ps -ef的最后那项，如/usr/bin/mongod -f /etc/mongod.conf

现在Zabbix-Server端测试：

```sh
#zabbix-get --host hostname --key proc.num[<name>,<user>,<state>,<cmdline>]


#检测mongd进程数量
zabbix-get --host 192.168.1.11 --key proc.num[mongod,,,]

#2，因为我开了两个mongd实例


zabbix-get --host 192.168.1.11 --key proc.num[mongod,root,,]
#1，只有一个是以root运行的，有一个是以mongod运行的

```

<br>

由于我们上面使用的MongoDB监控模板没有判断mongod进程存活与否的判断，此处我们在MongoDB模板中增加一个检查`mongod`进程的监控项，并创建对应的触发器。

![](/images/MongoDB/checkmongod.png)

![](/images/MongoDB/checkmongod2.png)


<br>


### 端口

`net.tcp.listen[port]`	检查 TCP 端口 是否处于侦听状态。返回 0 - 未侦听；1 - 正在侦听

此处我也用Mongod举例。我的两个mongod实例分别监听在27017,27018/tcp。

<br>

在Zabbix-Server端先测试：

```sh
#net.tcp.listen[port]

zabbix-get --host 192.168.1.11 --key net.tcp.listen[27017]
#1
zabbix-get --host 192.168.1.11 --key net.tcp.listen[27018]
#1
```

在Web端创建监控项和触发器与上面类似。





<br>

---

<br/>

# 用户自定义参数(user parameter)


我也是参考了上述大神的脚本，进行参考而来。

由于公司需要监控大量的Web页面和API接口的状态，并通过页面判断相关key-value的正确性，用以判断状态。此处可能由模拟登录等操作，Zabbix自带的Web监控不太够用，所以此处自定义用户参数来实现。

此处，我叫公司开发人员帮忙将全部接口以及Web页面内容都生成到一个json文件里，如 <http://zhang21.cn/test.json>。然后用`jq`命令解析json文件，里面key一一对应value，这样取值就很方便了。


<br>
<br>


## jq


jq 是一款命令行下处理 JSON 数据的工具。真的很好用！

jq官网：<https://stedolan.github.io/jq/>
GitHub: <https://github.com/stedolan/jq>

<br/>

**安装jq**

```sh
yum install -y jq

```

<br>

**使用jq**

```sh
jq --help


#查看所有键键值
curl --silent http://zhang21.cn/test.json | jq .


###栗子
{
  "collapsectimes": 130,
  "collapsectimes": 0,
  "bootfailtimes": 23,
  "failrate": 0.3623,
  "bootrate": 0.3324,
  "time": "2018-01-25 15:03:30",
  "db_error": false
}


#查看某个键值
curl --silent http://zhang21.cn/test.json | jq '.time'
curl --silent http://zhang21.cn/test.json | jq '.bootrate'

###
2018-01-25 15:03:30
0.3324


#查看某个不存在的值，会返回null
curl --silent http://zhang21.cn/test.json | jq '.zhang'

###
null
```

![](/images/Zabbix/jq01.png)


<br>

**json嵌套解析**

```sh
cat test.json | jq '.location.city'

###
"Chengdu"
```

<br>

**json解析数组**

```sh
cat test.json | jq '.array[1].name'

###
"Zhang"
```

<br/>

**内建函数**

jq还有一些内建函数，如`key`,`hss`。

key用来获取json中的key元素：

```sh
curl --silent http://zhang21.cn/test.json | jq 'keys'

###
[
	collapsectimes,
	collapsectimes,
	bootfailtimes,
	failrate,
	bootrate,
	time,
	db_error
]
```


has用来判断是否存在某个key:

```sh
curl --silent http://zhang21.cn/test.json | jq 'has("time")'

###
true
```


<br>

### jq的select语句

使用select函数来完成jq的过滤操作。
jq的select语句太好了!


select 接受一个条件表达式作为参数。其输入可以是迭代器，或者和 map 函数配合使用来处理数组。当输入中的某个元素使 select 参数中的条件表达式结果为真时，则在结果中保留该元素，否则不保留该元素。

对json文件的值是数组的，根据数据里面的key在取值，厉害厉害。

```sh
cat zhang.json

"array": [
{
	"ip": "192.168.1.11",
	"loads": 1234
},
{
	"ip": "192.168.1.22",
    "loads": 567
}
]



####栗子
cat /etc/zabbix/zhang.json | jq ".array[] | select(.ip == \"192.168.1.11\")"
{
	"ip": "192.168.1.11",
	"loads": 1234
}

cat /etc/zabbix/zhang.json | jq ".array[] | select(.ip == \"192.168.1.11\").loads"
1234

我们在自定义用户参数的时候便可以将ip作为参数传入
cat /etc/zabbix/zhang.json | jq ".array[] | select(.ip == \"$1\").loads"
```




<br>
<br/>


## 编写自定义参数和脚本

将脚本放置于`/etc/zabbix`，可将自定义参数写入zabbix-agentd.conf文件，也可单独写入`/etc/zabbix/zabbix_agentd.d/`(推荐)，这样修改更方便。


<br>

### 编写脚本文件

```sh
cd /etc/zabbix

vim xbreport.sh


##########

# Zabbix3.4
# Zhang21
# Thu Jan 25 15:20:44 CST 2018

###########

url="http://zhang21.cn/test.json"

JQ=`which jq`
CURL=`which curl`


function XBREPORT() {
        $CURL --silent $url | $JQ ".$1"
}


if [ $# == 0 ]; then
        echo $"Usage $0 {browsercollapsectimes|servercollapsectimes|xiaobaibootfailtimes|terminaldesktopfailrate|competebootrate|db_error}"
        exit
else
        XBREPORT "$1"
fi

```

![](/images/Zabbix/jq02.png)


<br/>

### 编写自定义参数文件

```sh
cd /etc/zabbix/zabbix_agentd.d

vim userparameter_XBreport.conf


##########
# Zabbix3.4
# Zhang21
# Thu Jan 25 15:45:19 CST 2018
##########

UserParameter=XBreport[*],/etc/zabbix/xbreport.sh $1
```

![](/images/Zabbix/jq03.jpg)


<br/>

### 测试自定义参数

```sh
zabbix_get --host host --key XBreport[time]

###
"2018-01-25 17:03:18"
```

<br>

### 自定义用户参数额外

由于我的json文件key对应的value中内嵌有数组，所以我需要再提取数组内的值。

```
curl http://zhang21.cn/test.json | jq '.array'


###
[
  {
    "ip": "1.1.1.1",
    "loads": 1051
  },
  {
    "ip": "2.2.2.2",
    "loads": 356
  }
]


#array[],array[1],array[2],array[n]
#array[].ip, array[1].ip
#array[].loads, array[2].loads
```

上面的数据中包含有zabbix无法解析的特殊符号，所以需要改变策略。

<br>

由于zabbix对UserParameter中包含**\'"`*?[]{}~$?&;()<>|#@**这些特殊字符无法进行处理，此处有两种方法来解决。

1. 在`zabbix_agentd.conf`中开启参数`UnsafeUserParameters`，将其值设置为1
2. 或者，使用多个变量`$1 $2 $3...`来解决我这个数组值的问题

<br>

我是使用多个变量来解决我这个情况的。
看下脚本。

```
cd /etc/zabbix

vim ./zhang.sh



url='http://www.zhang21.cn/test.json'
JQ=`which jq`
CURL=`which curl`

function ZHANG() {
        $CURL --silent $url | $JQ ".$1"
}

if [ $# == 0 ]; then
        echo $"Usage $0 {aaa|bbb|ccc|...}"
        exit
elif [ $# ==1 ]; then
		ZHANG "$1"
elif [ $# == 2 ]; then
        ZHANG "$1[$2]"
else
        ZHANG "$1[$2].$3"
fi





cd /etc/zabbix/zabbix_agentd.d/userparameter_Zhang.conf

UserParameter=Zhangxx[*],/etc/zabbix/zhang.sh $1 $2 $3
```

测试：

```sh
systemctl restart zabbix-agentd

zabbix_get --host host --key Zhangxx[array]
zabbix_get --host host --key Zhangxx[array,0]
zabbix_get --host host --key Zhangxx[array,0,loads]
zabbix_get --host host --key Zhangxx[array,1,ip]
```

<br>

测试正确能取到值的话，在Web端设置相对应的监控项。注意自己定义的`key`不要写错了。

数组的`key`栗子：

- Zhangxx[array]
- Zhangxx[array,0]或Zhangxx[array,1]
- Zhangxx[array,0,ip], Zhangxx[array,0,loads]


<br/>

### 在Web端添加监控项

由于这个参数是我们自定义的，所以在填写监控项key的时候需要我们手动填写自己定义的参数。
注意监控项的参数和信息类型。


![](/images/Zabbix/userparameter.png)

![](/images/Zabbix/userparameter02.png)

![](/images/Zabbix/userparameter03.png)


<br>

这里我遇到一个问题，我自定义key的执行脚本在Web端报超时问题，无法取值。这是由于zabbix默认的脚本执行超时时间为3s，所以我们需要修改超时时间30s(最大值)。

```
vim /etc/zabbix/zabbix_server.conf
vim /etc/zabbix/zabbix_agentd.conf
```

<br/>

### 设置触发器和报警

这个就根据你个人项目实际情况设置对于的触发器和报警。




<br>

---

<br/>

# 短信报警


## 腾讯短信服务

由于公司使用的是腾讯企业邮箱，可以将邮箱直接与微信绑定，从而在微信中实时显示邮件消息，所以不用微信报警！

此处使用的腾讯短信SMS服务： <https://cloud.tencent.com/product/sms>

- 短信文档： <https://cloud.tencent.com/document/product/382/13445>
- API文档： <https://cloud.tencent.com/document/product/382/13297>
- SDK文档： <https://cloud.tencent.com/document/product/382/5804>
- Python SDK: <https://cloud.tencent.com/document/product/382/11672>

由于腾讯提供了程序SDK，所以我选择了linux自带的Python SDK。
这里面有详细的Python使用方法，做一些小修改就可以使用了。

![](/images/Zabbix/SDK.png)


<br>
<br/>


## 配置


### 获取Python SDK

获取Python SDK


<br>

### 申请SDK AppID和App Key

申请SDK AppID以及APP Key。

申请完毕后，效果如下：

![](/images/Zabbix/SDK02.png)


<br>

### 申请短信签名

下发短信必须携带短信签名。
短信签名需要上传公司证件进行认证，大概十分钟左右！

效果如下：

![](/images/Zabbix/SDK03.png)


<br>

### 申请短信模板

下发短信内容必须经过审核。
在此短信模板中，我们必须要定义相关变量`{n}`，其他都是不会变化的常量。此处我定义了五个变量，分别为了带入Zabbix中的宏：

- 问题名，{TRIGGER.NAME}
- 主机名，{HOST.NAME}
- 事件事件，{EVENT.TIME}
- 事件日期，{EVENT.DATE}
- URL，{TRIGGER.URL}

![](/images/Zabbix/SDK04.png)


<br>

### SDK配置

```sh
yum install -y eple-realse
yum install -y python-pip

pip install qcloudsms
```


<br/>

### Python代码配置

腾讯文档：<https://cloud.tencent.com/document/product/382/11672>

由于我是向多人发送短信，所以做了小修改：

```py
#zabbix-server
cd /usr/lib/zabbis/alartscripts


vim sendSms.py





#!/bin/python
#coding: utf-8

from qcloudsms_py import SmsSingleSender
from qcloudsms_py.httpclient import HTTPError
import sys


appid = App ID
appkey = App Key
phone_numbers = ["12345", "1234567"]
#params = ["Problem", "Hostname", "Time", "Date","Url"]
params = [sys.argv[1], sys.argv[2], sys.argv[3], sys.argv[4], sys.argv[5]]


msender = SmsSingleSender(appid, appkey)
for i in phone_numbers:
    try:
        result = msender.send_with_param(86, i, 短信内容模板ID, params)
    except HTTPError as e:
        print(e)
    except Exception as e:
        print(e)
    print(result)
```

要给sendSms.py加上可执行权限哈！`chmod a+x ./sendSms.py`。

<br>

`sys.argv`变量是一个字符串的列表。特别地，`sys.argv`包含了命令行参数 的列表，即使用命令行传递给你的程序的参数。
使用Python的`sys.argv[n]`可以像shell一样将放在文件后的变量传入文件执行。此处对于在Zabbix-Web端将宏放在脚本后，作为变量传入，非常重要。

- `sys.argv[0]`代表sendSms.py文件
- `sys.argv[1]`才代表第一个参数。

![](/images/Zabbix/SDK05.jpg)



<br>
<br/>


## Zabbix Web端配置

配置-动作-创建动作-操作

注意事项：

- 建议针对触发器示警度最高就行短信报警，其它交给Email
- 操作类型，选择远程命令
- 目标列表，选择当前主机
- 类型，自定义脚本
- 执行在，这个一定是放在Zabbix-Server上来执行哈
- 命令，文件名SendSms.py后面接的宏一定要加上**双引号("")**

![](/images/Zabbix/SDK06.png)

![](/images/Zabbix/SDK07.png)


最后根据不同的内容，设置不同的报警机制。后台的脚本也修改为对应的名称，修改里面对应的手机号码。

- 首先根据不同报警设置不同的触发条件
- 运维组，SendSms_dev.py，修改运维对应的号码
- 开发组，SendSms_develop.py，修改对于的号码

<br>

其实这个发送短信，就是在执行远程命令。

你命令里是发送短信就发送短信，你命令里是发送邮件就发送邮件。这个还是挺不错的。




<br>

---

<br/>


# 针对不同业务向不同人员报警

有时候我们只需要关心我们自己那部分就可以了，没必要所有报警都发送给所有人，这样很不方便。

所以，我们可以根据业务相关，组别权限等，分别向不同的人报警不同的信息。

如下我的一个栗子图：

![](/images/Zabbix/action.png)





















































