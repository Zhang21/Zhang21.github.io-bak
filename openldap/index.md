# OpenLDAP


参考:

- LDAP维基百科: <https://zh.wikipedia.org/wiki/LDAP>
- OpenLDAP维基百科: <https://zh.wikipedia.org/wiki/OpenLDAP>
- `x.500`维基百科: <https://zh.wikipedia.org/wiki/X.500>
- OpenLDAP文档: <http://www.openldap.org/doc/>

环境:

- RHEL7.x86_64
- LDAP v2.4.44



<br/>
<br/>

---

<!--more-->

<br/>
<br/>




# 概述


<br/>


## LDAP


**LDAP**(轻型目录访问协议, Lightweight Directory Access Protocol）是一个开放的、中立的、工业标准的应用协议，通过IP协议提供访问控制和维护分布式信息的目录信息。
LDAP基于`X.500`标准的子集。因为这个关系，LDAP有时被称为`X.500-lite`。
LDAP在TCP/IP之上定义了一个相对简单的升级和搜索目录的协议。

LDAP目录与普通数据库的主要不同之处在于数据的组织方式，它是一种有层次的、树形结构。所有条目的属性的定义是对象类`object class`的组成部分，并组成在一起构成`schema`；那些在组织内代表个人的`schema`被命名为`white pages schema`。数据库内的每个条目都与若干对象类联系，而这些对象类决定了一个属性是否为可选和它保存哪些类型的信息。

LDAP目录的**条目**（entry）由**属性**（attribute）的一个聚集组成，并由一个唯一性的名字引用，即**专有名称**（distinguished name，DN）。

- **DN**: Distinguished Name
- **CN**: Common Name
- **OU**: Domain Component

<br>

LDAP组织数据方式:

```
         dc=org

      |dc=wikipedia
       /          \
 ou=people     ou=groups
```

![](/images/OpenLDAP/LDAP_Structure.png)

<br>

LDAP主要的应用场景是查询多而修改极少，那就充分发挥LDAP的优势了。因为没有事务处理，那数据库的速度可是比不上。 还有LDAP能存储海量的数据，还可以轻松地在各个系统之间复制，可用性超高。

目录是一个为查询、浏览和搜索而优化的专业分布式数据库，它呈树状结构组织数据，就好象Linux/Unix系统中的文件目录一样。目录数据库和关系数据库不同，它有优异的读性能，但写性能差，并且没有事务处理、回滚等复杂功能，不适于存储修改频繁的数据。所以目录天生是用来查询的，就好象它的名字一样。



<br/>
<br/>
<br/>



## OpenLDAP


**OpenLDAP**是轻型目录访问协议（Lightweight Directory Access Protocol，LDAP）的**自由和开源**的实现，在其OpenLDAP许可证下发行，并已经被包含在众多流行的Linux发行版中。

OpenLDAP主要包括下述4个部分：

- `slapd`: 独立LDAP守护服务
- `slurpd`: 独立的LDAP更新复制守护服务
- 实现LDAP协议的库
- 工具软件和示例客户端



<br/>
<br/>
<br/>



## Why OpenLDAP


账号是登录系统的唯一入口。要登录系统，首先系统要存在登录所使用的账号（/etc/passwd）及密码信息（/etc/shadow），然后经过系统查找顺序（/etc/nsswith.conf）及认证模块（/etc/pam.d/*）验证，得到授权后方可登录系统。如果多个用户登录系统，就需要在每个系统上创建用户名和密码；否则，就无法登录系统。

对于账号管理人员而言，维护10 台、100 台机器的账号，或许勉强可以维护、管理。如果机器数量达到1000 以上时，对于账号的创建、回收、权限的分配、密码策略、账号安全审计等一系列操作，账号管理人员就心有余而力不足了。此时OpenLDAP 账号集中管理软件就应用而生，它可以实现账号集中维护、管理，只需要将被管理的机器加入到服务器端即可，此后所有与账号相关的策略均在服务端实现，从而解决了运维案例所产生的众多管理问题。

关于账号的添加、删除、修改、权限的赋予等一系列操作只需要在服务端操作即可，无须在客户端机器进行单独操作。客户端账号及密码均通过OpenLDAP 服务器进行验证，从而实现账号集中认证管理，此时账号管理员只须维护OpenLDAP 服务器条目即可。




<br/>
<br/>

---

<br/>
<br/>




# OpenLDAP目录服务

Introduction to OpenLDAP Directory Services


本节介绍如何构建，配置和操作OpenLDAP软件以提供目录服务。这包括有关如何配置和运行standalone LDAP daemon——`slapd`的详细信息。它适用于系统管理员。本节提供目录服务的基本介绍，特别是**slapd**提供的目录服务。
本简介提供足够的信息，以便您可以开始学习**LDAP**，**X.500**和**目录服务**。


<br/>


## 目录服务是什么


目录是专门用于**搜索**(search)和**浏览**(browse)的专用数据库，另外还支持基本**查找**(lookup)和**更新**(update)功能。

目录往往包含描述性的，基于属性的信息，并支持复杂的过滤功能。目录通常不支持在为处理大量复杂更新而设计的数据库管理系统中发现的复杂事务或回滚方案。如果允许，目录更新通常是简单的全有或全无更改。目录通常用于快速响应高容量查找或搜索操作。他们可能具有广泛复制信息的能力，以提高可用性和可靠性，同时缩短响应时间。复制目录信息时，只要及时解决不一致问题，副本之间的临时不一致就可以了。

有许多不同的方法来提供目录服务。不同的方法允许将不同类型的信息存储在目录中，对如何引用，查询和更新信息。一些目录服务是本地的，向受限制的上下文提供服务；其它服务是全球性的，为更广泛的环境提供服务。全局服务通常是分布式的，这意味着它们包含的数据分布在许多机器上，所有机器都协作提供目录服务。通常，全局服务定义统一命名空间(namespace)，无论您在何处与数据本身相关，都可以提供相同的数据视图。



<br/>
<br/>
<br/>



## LDAP是什么


LDAP(Lightweight Directory Access Protocol, 轻型目录访问协议)，顾名思义，它是一种用于访问目录服务的轻量级协议，特别是基于`X.500`的目录服务。LDAP通过TCP / IP或其他面向连接的传输服务运行。

哪些种类的信息可以存储在目录中？DAP信息模型基于**条目**(entry)。条目是具有全局唯一**可分辨名称**（DN）的**属性**(attributes)集合。DN用于明确指代Entry，每个条目的属性都有一个**类型**(type)和一个或多个**值**(value)。

```
#这些类型通常是助记符字符串
cn
mail


#值的语法取决于属性类型
cn: ldap-test
mail: example@test.com
```

<br>

信息是如何安排的？在LDAP中，目录条目以分层树状结构(tree-like structure)排列。
传统上，这种结构反映了地理/组织边界。表示国家/地区的条目显示在树的顶部。下面是代表各州和国家组织的条目。再下面可能是表示组织单位，人员，打印机，文档或您可以想到的任何其他内容的条目。

传统命名:

![](/images/OpenLDAP/intro_tree.png)

<br>

还可以基于因特网域名来安排树。这种命名方法正变得越来越流行，因为它允许使用DNS定位目录服务。

基于域名命名:

![](/images/OpenLDAP/intro_dctree.png)

<br>

此外，LDAP允许您通过使用名为**对象类**(objectClass)的特殊属性来控制条目中所需和允许的属性。它的值确定条目必须遵守的模式规则。

<br>

如何引用信息？条目由其可分辨名称(DN)引用，该名称通过获取条目本身的名称来构造(称为Relative Distinguished Name, RDN)，并连接其祖先条目的名称。

<br>

如何保护信息免受未经授权的访问？某些目录服务不提供保护，允许任何人查看信息。LDAP为客户端提供了一种机制，用于对目录服务器进行身份验证或证明其身份。LDAP还支持数据安全性（完整性和机密性）服务。



<br/>
<br/>
<br/>



## 什么时候应该使用LDAP


通常，当您需要通过基于标准的方法集中管理、存储、访问数据时，应使用目录服务器。
总是有新的方法来使用目录并应用LDAP原则来解决某些问题，因此这个问题没有简单的答案。

一些常见的栗子：

- 机器认证: Machine Authentication
- 用户认证: User Authentication
- 用户/系统组: User/System Groups
- 地址簿: Address book
- 组织代表: Organization Representation
- 资产追踪: Asset Tracking
- 电话信息存储: Telephony Information Store
- 用户资源管理: User resource management
- 电子邮件查找: E-mail address lookups
- 应用配置存储: Application Configuration store
- PBX Configuration store
- ...



<br/>
<br/>
<br/>



## LDAP如何工作


LDAP使用C-S模式。一个或多个LDAP服务器包含组成目录信息树（DIT，directory information tree）的数据。客户端连接到服务器并发出请求。服务端响应客户端的请求。无论客户端连接到哪个LDAP服务器，它都会看到相同的目录视图，这是全局目录服务的一个重要特性。



<br/>
<br/>
<br/>



## 关于`x.500`


`X.500`是计算机目录服务的标准系列。`X.500`协议包括:

- DAP (Directory Access Protocol)
- DSP (Directory System Protocol)
- DISP (Directory Information Shadowing Protocol)
- DOP (Directory Operational Bindings Management Protocol)
- LDAP (Lightweight Directory Access Protocol)

<br>

从技术上讲，LDAP是`X.500`目录服务的目录访问协议。DAP是一种重量级协议，可在完整的OSI协议栈上运行，并且需要大量的计算资源。LDAP旨在通过`TCP/IP`进行操作，并以更低的成本提供DAP的大部分功能。

虽然LDAP仍然用于通过网关访问X.500目录服务，但现在更常见的是在X.500服务器中直接实现LDAP。

可以将 standalone LDAP daemon(slapd) 视为轻量级X.500目录服务器。也就是说，它没有实现X.500的DAP，也不支持完整的X.500模型。



<br/>
<br/>
<br/>



## LDAP与RDBMS


最常见的问题是——为什么OpenLDAP不使用 RDBMS(关系数据库管理系统) 而是使用像 LMDB 那样的嵌入式键/值存储？总的来说，期望商业级 RDBMS 实现的复杂算法可以使 OpenLDAP更 快或更好，并且同时允许与其他应用程序共享数据。

简而言之，使用嵌入式数据库和自定义索引系统，OpenLDAP可以在不损失可靠性的情况下提供更高的性能和可扩展性。所以OpenLDAP使用 LMDB 并发/事务 数据库软件。

<br>

下面是一个详细而冗长的答案: <>

很有可能认为在目录中使用RDBMS后端可以解决所有问题。但是，它是一头猪。这是因为数据模型非常不同。使用关系数据库表示目录数据将需要将数据拆分为多个表。
现在最大的问题是从一个条目访问数据需要在不同的磁盘区域上进行搜索。在某些应用程序中，这可能没问题但在许多应用程序中性能会受到影响。



<br/>
<br/>
<br/>



## slapd


slapd是OpenLDAP的守护进程， 在许多不同平台上运行的LDAP目录服务器。

slapd有一些有趣的功能和特性:

- **LDAPv3**: slapd实现轻量级目录访问协议的第3版，slapd支持IPv4和IPv6以及Unix IPC上的LDAP。
- **Simple Authentication and Security Layer**: slapd通过使用SASL支持强身份验证和数据安全性（完整性和机密性）服务
- **Transport Layer Security**: slapd通过使用 TLS/SSL 持基于证书的身份验证和数据安全性（完整性和机密性）服务
- **Topology control**: slapd可以配置为根据网络拓扑信息限制 socket 层的访问，基于 TCP wrapper
- **Access control**: slapd提供了丰富而强大的访问控制功能，允许您控制对数据库中信息的访问
- **Internationalization**: slapd支持Unicode 和 Language tag
- **Choice of database backends**: slapd附带了各种不同的数据库后端，您可以从中选择
- **Multiple database instances**: slapd可以配置为同时为多个数据库提供服务。这意味着单个slapd服务器可以使用相同或不同的数据库后端响应LDAP树的许多逻辑上不同部分的请求
- **Generic modules API**: 如果您需要更多自定义，slapd可让您轻松编写自己的模块
- **Threads**: slapd具有高性能的线程
- **Replication**: slapd可以配置为维护目录信息的集群副本
- **Proxy Cache**: slapd可以配置为缓存LDAP代理服务
- **Configuration**: slapd可通过单个配置文件进行高度配置，允许您更改您想要更改的所有内容




<br/>
<br/>

---

<br/>
<br/>




# 快速入门

A Quick-Start Guide

> 注意：本快速入门指南不使用强身份验证，也不使用任何完整性或机密保护服务。这些服务在OpenLDAP的其它章节中进行了描述。

以下包括OpenLDAP v2.4软件的快速入门指南。

<br>

- 获取软件
- 打开发行包
- 审阅文档
- 运行`configure`
- 构建软件
- 测试构建
- 安装软件
- 编辑配置文件
- 导入配置数据库
- 启动SLAPD
- 添加初始化条目到目录
- 查看是否正常运行




<br/>
<br/>

---

<br/>
<br/>




# 配置选择

The Big Picture - Configuration Choices


本节简要概述了各种LDAP目录配置。


<br/>


## 本地目录服务

Local Directory Service


在此配置中，您运行 slapd 实例，该实例仅为您的本地域提供目录服务。它不以任何方式与其他目录服务器进行交互。

![](/images/OpenLDAp/config_local.png)



<br/>
<br/>
<br/>



## 带推荐的本地目录服务

Local Directory Service with Referrals


在此配置中，运行 slapd 实例，该实例为本地域提供目录服务，并将其配置为将引用返回到能够处理请求的其它服务器。

如果要提供本地服务并参与全局目录，或者要将下级条目的责任委派给其他服务器，请使用此配置。

![](/images/OpenLDAP/config_ref.png)



<br/>
<br/>
<br/>



## 副本目录服务

Replicated Directory Service


slapd 包括对基于LDAP Sync 的复制的支持，称为syncrepl。可用于在多个目录服务器上维护目录信息的副本。在其最基本的配置中，master 是 syncrepl provider，slavee 是 syncrepl consumer。
集群和提供了可靠性和可用性。

![](/images/OpenLDAP/config_repl.png)



<br/>
<br/>
<br/>



## 分布式目录服务

Distributed Local Directory Service


在此配置中，本地服务被划分为较小的服务，每个服务都可以被复制，并与上级和下级引用粘合在一起。




<br/>
<br/>

---

<br/>
<br/>




# 安装

Building and Installing OpenLDAP Software


本章详细介绍了如何构建和安装OpenLDAP软件包。


<br/>


## 源码安装

官方文档中是使用源码进行构建和安装。


```sh
#提取软件
gunzip -c openldap-VERSION.tgz | tar xf -
cd openldap-VERSION



#依赖软件
#请参考REAME，安装它所需的依赖软件

#Transport Layer Security

#Simple Authentication and Security Layer

#Kerberos Authentication Service

#Database Software

#Threads

#TCP Wrappers



#configure
./configure --help

./configure --enable-wrappers \
                CPPFLAGS="-I/usr/local/include" \
                LDFLAGS="-L/usr/local/lib -Wl,-rpath,/usr/local/lib"



#构建软件
make depend
make



#测试
make test



#安装
#如果未指定安装位置，默认安装到 /usr/local
#通常，安装需要超级用户权限
sudo make install



#配置文件
/usr/local/etc/openldap
```


<br/>
<br/>
<br/>



## 包安装

因为在base源里面可直接搜索到`openldap`软件包，所以就是用软件包进行安装。

<br>

RPM包：

```
#查看
yum search openldap

openldap.x86_64 : LDAP support libraries
openldap-devel.x86_64 : LDAP development libraries and header files
openldap-servers.x86_64 : LDAP server
openldap-clients.x86_64 : LDAP client utilities
openldap-servers-sql.x86_64 : SQL support module for OpenLDAP server
compat-openldap.x86_64 : OpenLDAP compatibility shared libraries
collectd-openldap.x86_64 : OpenLDAP plugin for collectd
nss-pam-ldapd.x86_64 : An nsswitch module which uses directory servers



#安装
yum install -y openldap.x86_64 openldap-servers.x86_64 openldap-clients.x86_64
#yum install -y collectd-openldap.x86_64 openldap-servers-sql.x86_64 compat-openldap.x86_64 openldap-devel.x86_64 nss-pam-ldapd.x86_64



#验证
rpm -qa | grep openldap



#配置文件
/etc/openldap
```




<br/>
<br/>

---

<br/>
<br/>




# 配置

Configuring slapd


安装完毕后，你就可以配置并使用它。

本章介绍 `slapd-config` 配置系统的一般格式。
OpenLDAP v2.3及更高版本已转换为使用动态运行配置引擎`slapd-config`:

- 完全启用LDAP
- 使用标准LDAP操作进行管理
- 将其配置数据存储在LDIF数据库中(openldap/slap.d/)
- 允许所有slapd的配置选项在运行中进行更改，通常无需重新启动服务器即可使更改生效

<br>

> **注意**：
> 虽然 `slapd-config` 统将其配置存储为（基于文本的）LDIF文件，但您不应直接编辑任何LDIF文件。配置更改应通过LDAP操作执行，如 `ldapadd`, `ldapdelete`, `ldapmodify`



<br/>
<br/>


## 配置的布局

Configuration Layout


slapd配置存储为具有预定义模式和DIT的特殊LDAP目录。有特定的`objectClasses`用于承载全局配置选项，模式定义，后端和数据库定义以及各种其它项。

栗子配置树:

![](/images/OpenLDAP/config_dit.png)

<br>

`slapd-config` 配置树具有非常特定的结构。树的根名为 `cn=config` 并包含全局配置设置。其他设置包含在单独的子条目中：

- Dynamically loaded modules
- Schema definitions
- Backend-specific configuration
- Database-specific configuration

<br>

LDIF文件的常用规则适用于配置信息:

- `#`表示注释
- 如果一行以单个空格开头，则将其视为前一行的延续（即使前一行是注释），并删除单个前导空格。条目由空行分隔

<br>

配置LDIF的一般布局如下：

```
#globalconfigurationsettings
dn:cn=config
objectClass:olcGlobal
cn:config
<globalconfigsettings>

#schemadefinitions
dn:cn=schema,cn=config
objectClass:olcSchemaConfig
cn:schema
<systemschema>

dn:cn={X}core,cn=schema,cn=config
objectClass:olcSchemaConfig
cn:{X}core
<coreschema>

#additionaluser-specifiedschema
...

#backenddefinitions
dn:olcBackend=<typeA>,cn=config
objectClass:olcBackendConfig
olcBackend:<typeA>
<backend-specificsettings>

#databasedefinitions
dn:olcDatabase={X}<typeA>,cn=config
objectClass:olcDatabaseConfig
olcDatabase:{X}<typeA>
<database-specificsettings>

#subsequentdefinitionsandsettings
...

```



<br/>
<br/>
<br/>



## 配置指令

Configuration Directives


本节详细介绍了常用的配置指令


<br/>


### cn=config

本条目中包含的指令通常适用于整个服务器。其中大多数是系统或面向连接，而不是数据库相关。条目必须具有 `olcGlobal` 对象类(objectClass)


```
#指定强制关闭空闲客户端连接之前等待的秒数
#默认值为0，表示禁用此功能
olcIdleTimeout: <integer>



#该指令指定syslog（当前记录到syslogd）的调试语句和操作统计信息的级别。您必须已配置OpenLDAP --enable-debug（默认值）才能使用
olcLogLevel: <level>

#Debugging Levels
Level	Keyword	Description
-1	any	enable all debugging
0	 	no debugging
1	(0x1 trace)	trace function calls
2	(0x2 packets)	debug packet handling
4	(0x4 args)	heavy trace debugging
8	(0x8 conns)	connection management
16	(0x10 BER)	print out packets sent and received
32	(0x20 filter)	search filter processing
64	(0x40 config)	configuration processing
128	(0x80 ACL)	access control list processing
256	(0x100 stats)	stats log connections/operations/results
512	(0x200 stats2)	stats log entries sent
1024	(0x400 shell)	print communication with shell backends
2048	(0x800 parse)	print entry parsing debugging
16384	(0x4000 sync)	syncrepl consumer processing
32768	(0x8000 none)	only messages that get logged whatever log level is set



#指定当slapd无法找到本地数据库来处理请求时要传回的引用
olcReferral <URI>



#栗子条目
dn: cn=config
objectClass: olcGlobal
cn: config
olcIdleTimeout: 30
olcLogLevel: Stats
olcReferral: ldap://root.openldap.org
```



<br/>
<br/>



### cn=module

如果在配置slapd时启用了对动态加载模块的支持，则可以使用 `cn=module` 条目来指定要加载的模块集。


```
#指定要加载的可动态加载模块的名称
olcModuleLoad: <filename>



#指定要搜索可加载模块的目录列表
olcModulePath: <pathspec>



#栗子
dn: cn=module{0},cn=config
objectClass: olcModuleList
cn: module{0}
olcModuleLoad: /usr/local/lib/smbk5pwd.la

dn: cn=module{1},cn=config
objectClass: olcModuleList
cn: module{1}
olcModulePath: /usr/local/lib:/usr/local/lib/slapd
olcModuleLoad: accesslog.la
olcModuleLoad: pcache.la
```



<br/>
<br/>



### cn=schema

此条目包含在 slapd 中硬编码的所有模式定义。因此，此条目中的值由slapd生成，因此配置文件中不需要提供 schema value。仍必须定义该条目，以作为用户定义的模式添加到下面的基础。schema entry 必须具有 `olcSchemaConfig` 的对象类 (objectClass)。


```
#定义了一个属性类型
olcAttributeTypes: <RFC4512 Attribute Type Description>



#定义一个对象类
olcObjectClasses: <RFC4512 Object Class Description>



#栗子条目
dn: cn=schema,cn=config
objectClass: olcSchemaConfig
cn: schema

dn: cn=test,cn=schema,cn=config
objectClass: olcSchemaConfig
cn: test
olcAttributeTypes: ( 1.1.1
  NAME 'testAttr'
  EQUALITY integerMatch
  SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 )
olcAttributeTypes: ( 1.1.2 NAME 'testTwo' EQUALITY caseIgnoreMatch
  SUBSTR caseIgnoreSubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.44 )
olcObjectClasses: ( 1.1.3 NAME 'testObject'
  MAY ( testAttr $ testTwo ) AUXILIARY )
```



<br/>
<br/>


### Backend-specific Directives

后端指令适用于所有相同类型的数据库实例，并且可能会被数据库指令覆盖，具体取决于指令。后端条目必须具有 `olcBackendConfig` 的对象类 (objectClass)。


```
#命名特定于后端的配置条目
olcBackend: <type>

#Database Backends
Types	Description
bdb	    Berkeley DB transactional backend (deprecated)
config	Slapd configuration backend
dnssrv	DNS SRV backend
hdb	    Hierarchical variant of bdb backend (deprecated)
ldap	Lightweight Directory Access Protocol (Proxy) backend
ldif	Lightweight Data Interchange Format backend
mdb	    Memory-Mapped DB backend
meta	Meta Directory backend
monitor	Monitor backend
passwd	Provides read-only access to passwd(5)
perl	Perl Programmable backend
shell	Shell (extern program) backend
sql	    SQL Programmable backend




#栗子
dn: olcBackend=bdb,cn=config
objectClass: olcBackendConfig
olcBackend: bdb
```



<br/>
<br/>



### Database-specific Directives

每种类型的数据库都支持本节中的指令。数据库条目必须含有 `olcDatabaseConfig` 对象类 (objectClass)。


```
#命名特定的数据库实例
#可以提供数字{<index>}以区分相同类型的多个数据库
olcDatabase: [{<index>}]<type>



#权限指令
#如果未指定，默认使用 to * by * read
olcAccess: to <what> [ by <who> [<accesslevel>] [<control>] ]+



#将数据库置于“只读”模式
olcReadonly { TRUE | FALSE }



#指定不受此访问控制的DN或对此数据库的操作的管理限制
#DN不需要引用此数据库中的条目，甚至不需要引用目录中的条目。
olcRootDN: <DN>



#用于为root dn 指定DN的密码
olcRootPW: <password>



#指定从搜索操作返回的最大条目数
olcSizeLimit: <integer>



#定将传递给此后端数据库的查询的DN后缀
olcSuffix: <dn suffix>


#将当前 slapd 建立为运行 syncrepl 复制引擎的复制使用者站点，将当前数据库指定为主内容的副本
olcSyncrepl



#指定slapd将用于回答搜索请求的最大秒数
olcTimeLimit: <integer>



#该指令仅适用于slave slapd
olcUpdateref: <URL>



#栗子条目
dn: olcDatabase=frontend,cn=config
objectClass: olcDatabaseConfig
objectClass: olcFrontendConfig
olcDatabase: frontend
olcReadOnly: FALSE

dn: olcDatabase=config,cn=config
objectClass: olcDatabaseConfig
olcDatabase: config
olcRootDN: cn=Manager,dc=example,dc=com
```



<br/>
<br/>



### BDB and HDB Database Directives

此类别中的指令适用于BDB和HDB数据库。除了上面定义的通用数据库指令之外，它们还用在 olcDatabase 条目中。除了`olcDatabaseConfig` 对象类之外，BDB和HDB数据库条目还必须分别具有 `olcBdbConfig` 和 `olcHdbConfig` 对象类。


```
#指定包含数据库和相关索引的BDB文件所在的目录
olcDbDirectory: <directory>



#指定BDB后端数据库实例维护的内存高速缓存条目的大小
olcDbCachesize: <integer>



#指定检查BDB事务日志的频率，检查点操作将数据库缓冲区刷新到磁盘，并在日志中写入检查点记录
olcDbCheckpoint: <kbyte> <min>



#指定要放置在数据库目录的DB_CONFIG文件中的配置指令
olcDbConfig: <DB_CONFIG setting>



#此选项会导致磁盘上的数据库内容在更改时不会立即与内存更改同步
olcDbNosync: { TRUE | FALSE }



#在索引槽中指定内存中索引缓存的大小。默认值为零
olcDbIDLcacheSize: <integer>



#指定要为给定属性维护的索引
olcDbIndex: {<attrlist> | default} [pres,eq,approx,sub,none]



#如果此设置为TRUE，则slapindex将一次索引一个属性。默认设置为FALSE，在这种情况下，条目的所有索引属性将同时处理
olcDbLinearIndex: { TRUE | FALSE }



#指定新创建的数据库索引文件应具有的文件保护模式
olcDbMode: { <octal> | <symbolic> }



#指定用于搜索过滤器评估的堆栈深度
olcDbSearchStack: <integer>



#为共享内存BDB环境指定 key 。默认情况下，BDB环境使用内存映射文件。如果指定了非零值，则它将用作标识将容纳环境的共享内存区域的键
olcDbShmKey: <integer>



#栗子条目
dn: olcDatabase=hdb,cn=config
objectClass: olcDatabaseConfig
objectClass: olcHdbConfig
olcDatabase: hdb
olcSuffix: "dc=example,dc=com"
olcDbDirectory: /usr/local/var/openldap-data
olcDbCacheSize: 1000
olcDbCheckpoint: 1024 10
olcDbConfig: set_cachesize 0 10485760 0
olcDbConfig: set_lg_bsize 2097152
olcDbConfig: set_lg_dir /var/tmp/bdb-log
olcDbConfig: set_flags DB_LOG_AUTOREMOVE
olcDbIDLcacheSize: 3000
olcDbIndex: objectClass eq
```




<br/>
<br/>

---

<br/>
<br/>




# slapd配置文件

The slapd Configuration File


本章介绍如何通过 `slapd.conf` 配置文件来配置 `slapd`。 `slapd.conf` 已被弃用，建议使用前面介绍的 `slapd-config`进行配置。

<br>

由于已经被弃用，所以此处我跳过。

文档: <http://www.openldap.org/doc/admin24/slapdconfig.html>




<br/>
<br/>

---

<br/>
<br/>




# 运行slapd

`slapd` 旨在作为独立服务运行。这允许服务器利用缓存，管理底层数据库的并发问题，并节省系统资源。

由于我使用RPM包进行安装，所以可利用 `systemd` 进行OpenLDAP的管理。

<br>

ldap默认监听地址:

| URL | Protocol | Transport |
| - | - | - |
| `ldap:///` | LDAP | TCP port 389 |
| `ldaps:///` | LDAP over SSL | TCP port 636 |
| `ldapi:///` | LDAP | IPC (Unix-domain socket) |


<br/>


## slapd方式


```
#查看帮助
#man slapd
slapd --help


#启动
slapd --option


#停止
kill -INT `cat /usr/local/var/slapd.pid`
```


<br/>
<br/>


## systemd方式

```
systemctl status slapd


systemctl start slapd

#ps -ef | grep slapd
#/usr/sbin/slapd -u ldap -h ldapi:/// ldap:///


systemctl stop slapd
```




<br/>
<br/>

---

<br/>
<br/>




# 访问控制





























