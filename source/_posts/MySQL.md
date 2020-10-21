---
title: MySQL
date: 2018-01-16 10:47:12
categories: Database
tags:
  - MySQL
  - RDS
  - Database
  - DBMS
---


参考：

- MySQL5.7参考文档： <https://dev.mysql.com/doc/refman/5.7/en/>
- MySQL必知必会

<br/>

环境：

- CentOS7.x86_64
- MySQL5.7








<br/>
<br/>

---

<!--more-->

<br/>
<br/>









# 序言


MySQL官网： <https://www.mysql.com/>

由于MySQL5.7和以前版本之间的许多功能和其他差异，因此此手册不太适用于之前的老版本。之前的版本请参考MySQL相关版本的手册。

<br>

![MySQL](/images/MySQL/MySQL.png)










<br/>
<br/>

---

<br/>
<br/>











# 综述

General information


MySQL™ software提供了一个快速、多线程、多任务和健壮的SQL(结构化查询语言)的数据库服务器。MySQL server是为关键服务(mission-critical)、重负荷(heavy-load)生产系统以及嵌入式(embedding)大规模部署的软件而设计。
MySQL是Oracle Corporation的商标(trademark)。

MySQL software是双重许可的(dual license)：

1. Open Source product of the GNU General Public License
2. A Standard commercial License from Oracle




<br>
<br/>

## 关于此手册


- 该手册作为一个参考，它不提供关于SQL或关系型数据库概念的一般指令；
- MySQL Database Software正在不断发展，所以参考手册也经常更新。可在此 < http://dev.mysql.com/doc/> 获取最新版的手册；
- 参考手册(Reference Manual)的源文件使用DocBook XML格式书写的，其他版本(如HTML)等是自动生成的；
- 如果在使用过程中有任何问题或建议，请发邮件给我们；
- 手册由MySQL Documentation Team维护。



<br/>
<br>



## MySQL数据库管理系统

MySQL Database Management System


<br>


### MySQL介绍

MySQL是最流行的开源的SQL数据库管理系统，由Oracle Corporation开发、分发和支持。

<br>

- MySQL is a database management system
数据库是一个结构化的数据集合。它可能是从简单的购物清单到图片库，或是公司网络中的大量信息。若要添加、访问和处理存储在计算机数据库中的数据，你需要一个像MySQL Server这样的数据库管理系统。由于计算机非常擅长处理大量的数据，数据库管理系统在计算机中扮演这一个重要的角色。

- MySQL databases are relational
关系型数据库将数据存储在单独的表(table)中，而不是将所有数据放入一个大的库房中。数据库结构被组织成针对速度优化的物理文件。具有数据库(database)，表(table)，视图(view)，行(row)，列(column)等物理对象的逻辑模型提供了灵活的编程环境。你设置了管理不同数据字段之间关系的规则，如一对一，一对多，唯一，必须和可选关系，以及不同表之间的指针(pointer)。数据库强制执行这些规则，这样在设计良好的数据库中，应用程序就不会看到不一致、重复、孤立、过时或丢失的数据。

MySQL也是代表SQL(Structure Query Language)的一部分。SQL是访问数据库最常用的标准化语言。你可以直接使用SQL语句，或者将SQL语法隐藏到语言特定的API中。

- MySQL software is Open Source
MySQL software使用GPL(GNU General Public License)，开源意味着任何人都可以下载、转发、使用和修改软件，而不需要支付任何费用。

- MySQL database server is very fast,reliable,scalabe and easy to use

- MySQL server works in Client/Server or embedded system
MySQL Database Server是一个由多线程(multi-threaded)SQL Server组成的客户/服务器系统。它支持不同的后端，多个不同的客户程序和库、管理工具和广泛的APIs。
还提供MySQL Server作为一个嵌入式多线程库以便链接到你的产品，以获得一个更小，更快，更容易管理的独立产品。

- A large amount of contributed MySQL software is available


<br>
<br/>


### MySQL主要特点


#### Internals and Portability

- 由C和C++写成
- 适用于许多不同的平台
- 为了可移植性，使用`CMake`
- 采用独立(independent)模块的多层(layer)服务器设计
- 设计为使用内核线程的完全多线程，如果有多核CPU，能够轻松使用它们
- 提供了事务性(transactional)和非事务性(notransactional)存储引擎
- 使用非常快速的带有索引压缩的B-tree磁盘表
- 添加其他存储引擎相对容易
- 使用非常快速的基于线程的内存分配系统
- 使用优化的嵌套循环(nested-loop)连接执行非常快的联结
- 实现内存中的hash table，这些表用作临时表
- 使用高度优化的类库实现SQL函数

<br>

#### 数据类型

- 1,2,3,4和8byte的有无符号(signed/unsigned)的整数(integers)
- FLOAT
- DOUBLE
- CHAR, VARCHAR
- BINARY, VARBINARY
- TEXT
- BLOB
- DATE, TIME, DATETIME
- TIMESTAMP
- YEAR
- SET
- ENUM
- OpenGIS

<br>

#### 状态和功能

statement and function

- `SELECT`和`WHERT`中包含了所有支持的操作符和函数
- SQL中的`GROUP BY`和`ORDER BY`也全部支持
- GROUP functions(`COUNT()`, `AVG()`, `STD()`, `SUM()`, `MAX()`, `MIN()`, `GROUP_CONCAT()`)
- 支持`LEFT OUTER JOIN`和`ROGHT OUTER JOIN`
- 按照SQL标准支持table和columns的别名
- 支持`DELETE`,`INSERT`,`REPLACE`,`UPDATE`，以返回受影响的行数
- 支持MySQL特定的`SHOW`显示语句
- 一个`EXPLAIN`语句显示优化器如何解析查询

<br>

#### 安全

security

- 权限(privilege)和密码系统，非常灵活和安全，并且支持基于主机的验证
- 当连接到Server时，通过加密(encryption)所有密码通信量来确保密码安全

<br/>

#### 扩展性和限制

Scalability and Limits

- 支持大型数据库。包含五千万条记录，二十万个表，五十亿行
- 每个表最多支持64个索引，每个索引可以由1到16个列组成

<br/>

####　连通性

Conectivity

- 客户端使用如下几种协议连接到MySQL Server
	+ TCP/IP sockets
	+ --enable-named-pipe on Windows
	+ Unix domain socket files on UNIX
- MySQL客户端可用多种语言编写
- APIs对于多数语言是可用的

<br/>

#### 本地化

Localization

- Server可以向多种语言的客户端提供错误信息
- 完全支持几个不同的字符集(character sets)
- 所有数据都被保存在选取的字符集(chracter set)
- 排序和比较是根据默认的字符集和排序规则完成
- 服务器时区(time zone)可动态更改，个客户端也可修改自己的时区

<br/>

#### 客户端和工具

Clients and Tools

- MySQL包含几个客户机和使用程序
	+ command-line： `mysqldump`, `mysqladmin`
	+ graphical: MySQL Workbench
- MySQL Server内置了对SQL语句的支持来检查、优化和修复表
- MySQL程序可使用`--help`或`-?`来获取帮助


<br>
<br/>


### MySQL历史

History of MySQL

- MySQL is named after co-founder Monty Widenius's daughter, My.
- The name of the MySQL Dolphin (our logo) is “Sakila,” which was chosen from a huge list of names suggested by users in our “Name the Dolphin” contest.



<br/>
<br/>



## MySQL5.7新特色

What Is New in MySQL 5.7

<br>

### MySQL5.7新功能

Features Added in MySQL 5.7


<br>
<br/>


### MySQL5.7中过期的功能

Features Deprecated in MySQL 5.7


<br/>
<br>


### MySQL5.7中移除的功能

Features Removed in MySQL 5.7



<br>
<br/>



##  Server and Status Variables and Options Added, Deprecated, or Removed in MySQL 5.7



<br>
<br/>



## MySQL信息源

MySQL Information Sources

本章节将列出有关MySQL的帮助信息。


<br>


### MySQL站点

MySQL Websites

MySQL Documentation is <https://dev.mysql.com/doc>


















<br/>
<br/>

---

<br/>
<br/>









# 术语

[MySQL Glossary](https://dev.mysql.com/doc/refman/5.7/en/glossary.html)


这些术语通常用于有关MySQL的信息中。


<br/>


- **`.ARM`文件**
ARCHIVE表的metadata。由MySQL Enterprise Backup产品的mysqlbackup命令生成的备份中。

- **`.ARZ`文件**
ARCHIVE表的数据。由MySQL Enterprise Backup产品的mysqlbackup命令生成的备份中。

- **ACID**
代表原子性(atomic)，一致性(consistency)，隔离性(isolation)和持久性(durability)的首字母缩略词。
事务是可以提交(commit)或回滚(rollback)的原子工作单位。当事务对数据库进行多次更改时，要么在提交事务时所有更改都成功，要么在事务回滚时撤消所有更改。
数据库始终保持一致性状态 - 每次提交或回滚后，以及事务正在进行中。如果跨多个表更新相关数据，查询将查看所有旧值或所有新值，而不是旧值和新值的混合。
交易在进行过程中受到保护（隔离），它们不能互相干扰或看到彼此未提交的数据。这种隔离是通过锁定机制(locking mechanism)实现的。经验丰富的用户可以调整隔离级别，在保证事务确实不会相互干扰的情况下，减少保护，转而提高性能和并发。
事务的结果是持久的：一旦提交操作成功，该事务所做的更改就可以避免电源故障，系统崩溃，竞争条件或许多非数据库应用程序易受攻击的其他潜在危险。耐用性通常涉及写入磁盘存储，具有一定的冗余以防止写入操作期间的电源故障或软件崩溃。

- **adaptive flushing**
An algorithm for InnoDB tables that smooths out the I/O overhead introduced by checkpoints.MySQL不会一次将所有修改过的页面从缓冲池(buffer pool)刷新(flush)到数据文件，而是定期刷新一小组修改过的页面。自适应刷新(adaptive flushing)算法通过基于刷新率和生成重做信息的速度来估计执行这些周期性刷新的最佳速率来扩展该过程。

- **adaptive hash index**
InnoDB表的优化，通过在内存中构造hash index，可使用`=`和`IN`操作符加速查找。MySQL监控InnoDB表的索引搜索，如果查询可从哈希索引中受益，它会自动为经常访问的索引页创建一个。从某种意义上来说，自适应哈希索引在运行时配置MySQL以利用充足的主内存，更接近主内存数据库的体系结构。此功能由`innodb_adaptive_hash_index`配置项控制。

- **AIO**
异步(asynchronous)I/O的缩写。

- **Antelope**
原始InnoDB文件格式的代码名称。它支持`REDUNDANT`和`COMPACT`行格式，但不支持较新的`DYNAMIC`和`COMPRESSED`行格式。

- **API**
一组功能或程序。

- **apply**
当MySQL Enterprise Backup产品生成的备份不包括备份进行时发生的最新更改时，更新备份文件以包含这些更改的过程称为apply步骤。

- **asynchronous I/O**
一种I/O操作，允许在I/O完成前继续进行其它处理。也称为非阻塞(nonblocking)I/O，缩写为AIO。

- **atomic**
在SQL上下文中，事务是完全成功(commited)或根本没有效果(rollback)的工作单元。

- **atomic DDL**
atomic DDL语句将数据字典更新，存储引擎操作和DDL操作关联的二进制日志写入组合到单个原子事务中。即使服务器在操作期间暂停，事务也可以完全提交或回滚。MySQL 8.0中添加了Atomic DDL支持。

- **atomic instruction**
CPU提供的特殊指令。确保关键的低级操作不会被中断。

- **auto-increment(自增)**
表的列的属性(由AUTO_INCREMENT关键字指定)，在自动在列中添加值的升序。

- **auto-increment locking**
自动增量主键的便利性涉及一些与并发的权衡。

- **autocommit**
在每个SQL语句之后导致COMMIT的设置。建议不要将此模式用于具有跨多个语句的事务的InnoDB表。它可以帮助InnoDB表上的只读事务的性能，从而最大限度地减少锁定和生成撤消数据的开销。

- **availability**
能够对应于主机上的故障，并在必要时从中恢复故障。

<br>

- **B-tree**
一种在数据库索引中很常用的树数据结构。结构始终保持排序，从而能够快速查找完全匹配(`=`)和让位(`>, <, BETWEEN`)。这种索引类型适用于大多数索引类型。
因为B树有很多子节点(children)，所以B树与二叉树(binary tree)不同，二叉树每个节点限制为2个子节点。
与哈希索引(hash index)形成对比，HASH仅在MEMORY存储引擎中可使用，MEMORY存储引擎中也可使用B树索引。如果某些查询使用范围运算符，则应为MEMORY表选择B树索引。

- **backticks(反引号)**
如果MySQL SQL语句中的标识符包含特殊字符或保留字，则必须使用反引号(`\``)。

- **backup**
从MySQL实例复制部分或全部表数据和原数据的过程，以便妥善保管。

- **Barracuda**
InnoDB文件格式的编码名称，它支持启用InnoDB表压缩的`COMPRESSED`行格式，以及改进长度可变长度列的存储布局的`DYNAMIC`行格式。

- **base column**
存储生成的列或虚拟生成列多所基于的非生成表列。换句话说，基本列是非生成的表列，它是生成的列定义的一部分。

- **binary log**
包含尝试更改表数据的所有语句的记录的文件。可以重播(replayed)这些语句，以便在副本集方案中是Slave Server保持最新，或者在从备份中还原表数据使数据库保持最新。建议开启此功能。
你可使用`mysqlbinlog`命令检查二进制日志的内容，以及重播这些内容。

- **binlog**
二进制日志文件的非正式名称。

- **blind query expansion**
由`WITH QUERY EXPANSION`子句启用的特殊全文搜索(full-text search)模式。它执行两次搜索，其中第二次搜索的搜索短语是与第一次搜索的少数最高度相关的文档连接的原始搜索短语。该技术主要适用于短搜索短语，可能只有一个单词。它可以发现文档中未出现精确搜索词的相关匹配。

- **bottleneck(瓶颈)**
系统的一部分，其大小或容量受到限制，具有限制总吞吐量的效果。

- **bounce**
关机(shutdown)操作后立即重启(restart)。

- **buddy allocator**
一种管理InnoDB缓冲池(buffer pool)不同大小页面(pages)的机制。

- **buffer**
用于临时存储的内存或磁盘区域。

- **buffer pool**
保存表和索引的缓存InnoDB数据的内存区域。为了提高大容量读取操作的效率，缓冲池被分成可以容纳多行的页面。在具有大内存的系统上，可以通过将缓冲池划分为多个缓冲池实例来提高并发性。
几个InnoDB状态变量，`INFORMATION_SCHEMA`和`performance_schema`有助于监视缓冲池的内部工作。

- **buffer pool instance**
可以划分缓冲池的多个区域中的任何一个，由`innodb_buffer_pool_instances`配置项控制。`innodb_buffer_pool_size`指定的总内存大小在所有缓冲池实例之间划分。通常，具有多个缓冲池实例适用于为InnoDB缓冲池分配多个GigaBytes的系统，每个实例为1GigaByte或更大。

- **built-in**
MySQL内置的InnoDB存储引擎是存储引擎的原始分发形式。

<br>

- **`.cfg`文件**
与InnoDB可传输表空间功能一起使用的元数据(metadata)文件。它由命令`FLUSH TABLES...FOR EXPORT`生成，将一个或多个表置于可以复制到另一个Server的一致状态。

- **cache**
存储区域的通用术语，用于存储频繁或高速检索的数据副本。在InnoDB中，主要的缓存结构就是缓冲池(buffer pool)。

- **cardinality**
表列中的不同值的数量。当查询引用具有关联索引的列时，每列的基数会影响哪种访问方法最有效。

- **change buffer**
一种特殊的数据结构，用于记录二级(secondary)索引中页面的更改。

- **change buffering**
涉及change buffer功能的通用术语，包括`insert buffering, delete buffering, pure buffering`。

- **checkpoint**
当对缓冲池(buffer pool)中缓存(cached)的数据也进行更改时，这些更改将在稍后某个时间写入数据文件，这个过程称为刷新(flushing)。检查点是已成功写入数据文件的最新更改(LSN值)的记录。

- **checksum**
在InnoDB中的一种验证机制，用于在将表空间中的页面从磁盘读入InnoDB缓冲池时检测损坏。

- **child table**
在外键(foreign key)中，子表是其行引用另一个表中具有相同值的特定列的行的表。这是包含`FOREIGN KEY...REFERENCES`子句和可选`ON UPDATE`和`ON DELETE`子句的表。子表创建之前，父表中的相应行必须存在。

- **clean page**
InnoDB缓冲池中的一个页面，启用所有在内存中进行的更改也写入(flushed)到数据文件中。
dirty page的反面。

- **clean shutdown**
关闭完成且没有错误，并在完成之前对InnoDB表应用所有的更改，而不是奔溃或快速关闭。slow shutdown的同义词。

- **client**
客户端。

- **clustered index**
InnoDB术语表示主键索引(primary key index)。InnoDB表存储基于主键列的值进行组织，以加速涉及主键列的查询和排序。为获得最佳性能，请根据性能最关键的查询仔细选择主键列。

- **cold backup**
数据库关闭时进行的备份。

- **column**
行中的数据项，其存储和语义由数据类型定义。每个表格索引主要由它包含的列集合来定义。
每个列都有一个基数值。列可以是其表的主键，也可以是主键的一部分。列可以是受唯一约束(unique constraint)，`NOT NULL constraint`或两者都有。不同列中的值，甚至跨不同的表，可以通过外键关系(foreign key relationship)链接。

- **column index**
单列的索引。

- **column prefix**
当使用长度规范创建索引时(如: `CREATE INDEX idx ON t1 (c1(N));`)，只有列值的前N个字符存储在索引中。保持索引前缀较小使索引紧凑，内存和磁盘I/0节省有助于提高性能。

- **commit**
结束事务的SQL语句，使事务所做的任何更改永久化。它与回滚(rollback)相反，回滚撤销了在事务中所做的任何更改。

- **compact row format**
它是MySQL 5.0.3 to MySQL 5.7.8的默认行格式。从MySQL 5.7.9开始，默认行格式由`innodb_default_row_format`配置选项定义，该选项的默认设置为`DYNAMIC`。

- **composite index**
包含多个列的索引。

- **compressed backup**
MySQL Enterprise Backup产品的压缩功能生成每个表空间的压缩副本.

- **compressed row format**
一种行格式，可为InnoDB表启用数据和索引压缩。

- **compressed table**
以压缩形式存储数据的表。对于InnoDB，它是使用`ROW_FORMAT=COMPRESSED`创建的表。

- **compression**
具有广泛优势的功能,包括使用更少的磁盘空间，执行更少的I/O以及使用更少的内存进行缓存。
InnoDB支持表级别和页级别的压缩。

- **compression failure**
实际上并不是错误，而是在将压缩与DML操作结合使用时可能发生的昂贵操作。

- **concurrency(并发)**
多个操作同时运行的能力，而不会相互干扰。
并发性还涉及性能，因为理想情况下，使用有效的锁定(locking)机制，对多个并发事务的保护以最小的性能开销工作。

- **configuration file**
包含MySQL在启动时使用的选项的文件。一般来说，Unix/Linux上此文件为`my.cnf`，Windows上为`my.ini`。你可在`[mysqld]`部分下设置与InnoDB相关的许多选项。

- **consistent read(一致读)**
一种读取操作，它使用快照信息基于某个时间点显示查询结果，而不管同时运行的其它事务所执行的更改。

- **constraint**
一种自动测试，可以阻止数据库更改以防止数据变得不一致。

- **counter**
由特定类型的InnoDB操作递增的值。用于测量Server的繁忙程度，对性能问题的来源进行故障排除，以及测试更改是否具有所需的低级别效果。

- **covering index**
包含查询检索的所有列的索引。查询不是使用索引值作为指针来查找完整的表行，而是从索引结构返回值，从而节省磁盘I/O。

- **CPU-bound**
一种工作负载(workload)，其主要瓶颈是内存中的CPU操作。通常涉及读取密集型操作，其中结果都可以缓存在缓冲池中。

- **crash**
MySQL使用术语崩溃(crash)来指代Server无法正常清理的任何意外关机操作。

- **crash recovery**
崩溃后再次启动MySQL时发生的清理活动。

- **CRUD**
`create, read, update, delete`的缩写，使数据库应用程序中常见的操作序列。

- **cursor**
一种内部数据结构，用于表示查询的结果集合或使用SQL `WHERE`子句执行搜索的其它操作。它的工作方式类似于其它高级语言中的迭代器，根据请求从结果集合中生成每个值。

<br>

- **data dictionary**
跟踪与InnoDB相关的对象(如table, indexs, columns)的元数据。此元数据实际位于InnoDB系统表空间。由于历史原因，它在某种程度上与存储在`.frm`文件中的信息重叠。

- **data directory**
每个MySQL实例保存InnoDB数据文件的目录和代表各个数据库的目录。由`datadir`配置项控制。

- **data files**
物理上包含table, index数据的文件。
InnoDB系统表空间包含InnoDB数据字典，能保存多个InnoDB表的数据，由`.ibd`数据文件表示。
由`.ibd`文件保存单个InnoDB表数据的每个表文件空间。

- **data warehouse**
主要运行大型查询(`query`)的数据库系统或应用程序。可以以非规范化(denormalized)形式组织只读或读取大部分数据以提高查询效率。

- **database**
在MySQL数据目录中，每个数据库有一个单独的目录表示。
对于长期使用MySQL的用户来说，数据库是一个熟悉的概念。来自Oracle数据库背景的用户会发现数据的的MySQL含义更接近Oracle数据库调用`schema`。

- **DCL(data control language)**
数据控制语言，一组用于管理权限的SQL语句。在MySQL中，由`GRANT, REVOKE`语句组成。

- **DDL(data definition language)**
数据定义语言，一组用于操作数据库本身而不是单个表行的SQL语句。包括所有形式的`CREATE, ALTER, DROP, TRUNCATE`语句。
DDL语句自动提交(commit)当前事务(transaction)，它们无法回滚(rolled back)。

- **DML**
数据操作语言，一组用于执行`INSERT, UPDATE, DELETE`操作的SQL语句。

- **deadlock**
不同事务无法继续的情况，因为每个事务都持有另一个需要的锁(lock)。因为两个事务都在等待资源可用，所以它们都不会释放它拥有的锁。
当事务锁定多个表中的行(通过如`UPDATE, SELECT... FOR UPDATE`等语句)时，可能会发生死锁，但顺序相反。当语句锁定索引记录(index record)和间隙(gaps)的范围时，也可能会发生死锁，每个事务由于计时问题而获取某些锁而不是其它锁。

- **deadlock detection**
可自动检测何时发生死锁，并自动回滚其中一个事务(受害者(victim))的一种机制。可使用`Innodb_deadlock_dectect`配置项禁用死锁检测。

- **delete**
当InnoDB处理`DELETE`语句时，行(row)会立即标记为删除，查询不再返回。在称为清除(purge)操作的顶级垃圾收集期间，稍后将回收存储。为了删除大量数据，具有相关性能特征的相关操作是`TRUNCATE`和`DROP`。

- **delete buffering**
在更改缓冲区中存储由`DELETE`操作产生的二级索引页的更改而不是立即写入更改的技术，以便可以执行物理写入以最小化随机I/0。
其它有insert buffering, purge buffering。

- **denormalized**
一种数据存储策略，跨不同的表复制数据，而不是将表与外键(FOREIGN KEYS)和连接查询(JOIN query)相关联。

- **descending index**
一种可用于某些数据库系统的索引的类型，其中索引存储已经过优化，可处理`ORDER BY column DESC`子句。
目前，尽管MySQL在`CREATE TABLE`语句中允许使用`DESC`关键字，但它不会对结果索引使用任何特殊的存储布局。

- **dirty page**
InnoDB缓冲池已在内存中更新的页面，其中的更改尚未写入(flush)到数据文件。
与clean page相反。

- **dirty read**
检索不可靠数据的操作，由另一个事务更新但未提交的数据。只有隔离(isolation)级别称为`read uncommitted`才有可能。
这种操作不符合ACID数据库设计原理。它被认为是非常危险的，因为数据可在提交之前回滚或更新；然后，执行dirty read的事务将使用从未确认为准确的数据。
它的反面是一致读取(consistent read)，其中InnoDB确保事务不会读取由另一个事务更新的信息，即使使用其它事务也是如此。

- **disk-based**
一种主要在磁盘存储上组织数据的数据库。数据在磁盘和存储器之间来回传递以进行操作。
反面是`in-memory`数据库。

- **disk-bound**
一种工作负载，其主要瓶颈(bottleneck)是磁盘I/O。

- **document id**
在InnoDB权威搜索功能中，表中包含FULLTEXT index的特殊列，用于唯一标识与每个`ilist`值相关联的文档。

- **doublewrite buffer**
InnoDB使用名为**doublewrite**的文件刷新(flush)技术。在将页面(page)写入数据文件之前，InnoDB首先将它们写入称之为doublewrite缓冲区的连续区域。只有在完成对doublewrite缓冲区的写入和刷新之后，InnoDB才会将页面写入数据文件中的正确位置。
如果在页面写入过程中存在操作系统，存储子系统或mysqld进程崩溃，InnoDB稍后可以在崩溃恢复期间从doublewrite缓冲区中找到该页面的良好副本。

- **drop**
一种通过`DROP TABLE, DROP INDEX`等语句删除模式对象的DDL操作。它在内部映射到`ALTER TABLE`语句。

- **dynamic row format**
InnoDB插件中引入的行格式，作为**Barracuda file format**的一部分提供。由于长的可变长度列值存储在保存行数据的页面之外，因此对于包含大对象的行非常有效。由于通常不访问大字段来评估查询条件，因此它们不会经常进入缓冲池，从而导致更少的I / O操作和更好的缓存内存利用率。
默认行格式由`innodb_default_row_fotmaat`定义，其默认值为`DYNAMIC`。

<br>

- **early adopter**
类似于BETA的阶段，通常在非关键任务设置中评估软件产品的性能、功能和兼容性。

- **error log**
一种日志，显示有关MySQL启动和关键运行时错误以及奔溃信息的消息。

- **eviction**
从缓存或其它临时存储区域(如InnoDB buffer pool)中移除项(item)的过程。

- **exclusive lock**
一种阻止任何其它事务锁定同一行的锁。根据事务隔离级别，这种锁可能会阻止其它事务写入同一行，或者也可能阻止其它事务读取同一行。

- **extent**
表空间的一组页面。对于16KB的默认页面大小，范围包含64页。
MySQL v5.7.6中增加了对32KB(2MB)和64KB(4MB) InnoDB页面大小的支持。

<br>

- **`.frm`文件**
包含MySQL表的元数据(如表定义)的文件。
对于备份，必须始终保留完善的`.frm`文件集合备份数据，以便能够还原备份后更改或删除的表。
虽然每个InnoDB表都有一个`.frm`文件，但InnoDB在系统表空间中维护自己的表元数据。InnoDB不需要`.frm`文件来操作InnoDB表。
`.frm`文件由MySQL Enterprise Backup产品备份。

- **Fast Index Creation**
InnoDB插件中首次引入的功能，现在是MySQL 5.5及更高版本的MySQL的一部分。
通过避免完全重写关联表的需要，加速了InnoDB **二级索引(secondary index)**的创建。加速也适用于删除二级索引。
由于索引维护可能会增加许多数据传输操作的性能开销，因此请考虑执行诸如`AALTER TABLE ... ENGINE=INNODB`或`INSERT INTO ... SELECT * FROM ...`之类的操作，而不使用任何二级索引，并在之后创建索引。
在MySQL 5.6中，此功能变得更加通过。你可以在创建索引时读取和写入表，并且可以在不复制表的情况下执行等多种类的`ALTER TABLE...`操作，而不会阻止DML操作，或两者。因此在MySQL 5.6及更高版本中，这组功能称为**online DDL**，而不是快速索引创建。

- **fast shutdown**
InnoDB的某人关闭过程，基于配置`innodb_fast_shutdown=1`。为了节省时间，跳过某些`FLUSH`操作。这种类型的关闭在正常使用期间是安全的，因为刷新操作在下次启动期间执行，使用与崩溃恢复(crash recovery)中相同的机制。
如果数据库正在关闭以进行升级或降级，请执行**慢速关闭(slow shutdown)**，以确保在关闭期间对数据文件应用所有相关更改。

- **file format**
InnoDB表的文件格式，使用`innodb_file_format`配置项启用。支持的文件格式是：
  - **Antelope**： 是最初的InnoDB文件格式，支持**REDUNDANT**和**COMPACT**行格式；
  - **Barracuda**：是新的InnoDB文件格式，支持**COMPRESSED**和**DYNAMIC**行格式。

- **file-per-table**
由`innodb_file_per_table`选项控制的通用名称，这影响InnoDB文件存储、功能可用性和I/O特性方面的重要配置项。
启用此配置项后，你可以在其`.idb`文件中创建表，而不是在系统表空间的共享`idbdata`文件中创建表。当表数据存储在单个`.idb`文件中时，你可更灵活地选择数据压缩等功能所需的行格式。

- **fill factor**
在InnoDB索引中，分隔页面之前索引数据占用的页面比例。

- **fixed row format**
此行格式由MyISAM存储引擎使用，而不是InnoDB。

- **flush**
将已在内存区域或临时磁盘存储区中缓冲中的更改写入数据库文件。
定期刷新(flush)的InnoDB存储结构包括`redo log`, `undo log`, `buffer log`。

- **flush list**
一个内部InnoDB数据结构，用于跟踪缓冲池中的脏页面(dirty page)：即已更改并需要写入磁盘的页面。InnoDB内部小型事务经常更新此数据结构，因此受其自身的互斥锁(mutex)保护，以允许并发访问缓冲池。

- **foreign key**
一种指针关系类型，位于单独的InnoDB表中的行之间。外键关系在**父表(par ent table)**和**子表(child table)**中的一列上定义。
除了能快速查找相关信息外，外键还可以防止这些指针在插入、更新和删除数据时变为无效，从而有助于强制引用完整性(referential integrity)。此强制机制是一种约束(constraint)。
如果另一个表中不存在关联的外键值，则无法插入指向另一个表的行。如果删除了一行或更改了其外键值，并且另一个表中的行指向该外键值，则可以设置外键以防止删除，导致另一个表中的相应列值变为NULL，或者自动删除另一个表中的相应行。
设计规范化(normalized)数据库是一个阶段是识别副本的数据，将数据分成新表，并设置外键关系，以便可使用**连接(JOIN)**操作像单个表一样查询多个表。

- **FOREIGN KEY constraint**
通过外键关系维护数据库一致性的约束类型。
与其它类型的约束一样，如果数据不一致，它可以防止数据被插入和更新；

- **FTS**
在大多数情况下，它是全文搜索(full-text search)的缩写。有时在性能泰伦中，它是全表扫描(full-table scan)的缩写。

- **full-text search**
一种MySQL功能，用于在表数据中查找单词(words)、短语(phrases)、单词的布尔组合...比如使用SQL的`LIKE`运算符或编写自己的应用程序级的搜索算法。

- **full table scan**
需要读取表的全部内容而不是仅使用索引选择的部分内容的操作。
索引的目的是允许在大表中查找特定值或值范围，从而在实践时避免全表扫描。

- **full backup**
一种备份，包括每个msyql数据库中的所有表，以及MySQL实例中的所有库。

- **FULLTEXT index**
在MySQL全文搜索机制中保存搜索索引的特殊索引。

- **fuzzy checkpointing**
一种从缓冲池刷新小批量脏页面的技术，而不是一次刷新所有脏页面，这会破坏数据库处理。

<br>

- **GA**
一般可用(Generally available)，及产品出现测试阶段可供销售，官方支持和生产使用的阶段。

- **gap**
InnoDB索引数据结构中可以插入新值的位置。

- **gap lock**
锁定索引记录之间的间隙(gap)，或锁定第一个或最后一个索引记录之前的间隙。
与记录锁(read lock)和下键锁(next-key lock)对比。
间隙锁是性能和并发之间权衡的一部分，用于某些事务隔离级别而不是其它级别。

- **general log**
参考一般查询日志。

- **general query log**
一种用于诊断和排除MySQL Server处理SQL语句的日志。可存储在文件或数据库表中。你可使用`gereral_log`配置项启用此功能，可通过`sql_log_off`配置项为特定连接禁用它。
记录比慢查询更广泛的查询。与用于副本的二进制日志不同，通用查询日志包含`SELECT`语句，并且不保持严格的排序。

- **general tablespace**
由`CREATE TABLESPACE`语法创建的共享InnoDB表空间。通用表空间可以在MySQL数据目录之外创建，能够保存多个表，并支持所有行格式的表。 MySQL 5.7.6中引入了一般表空间。
与系统表空间(system tablespace)和每个表文件(file-per-table)空间对比。

- **generated column**
其值是根据列定义中包含的表达式计算的列。生成列可以是虚拟的(virtual)或存储的(stored)。

- **generated stored column**
See stored generated column.

- **generated virtual column**
See virtual generated column.

- **global transaction**
XA操作中涉及的一种事务。它由几个本身具有事务性的操作组成，但所有操作必须作为一个组成功完成，或者全部作为一个组回滚。

- **group commit**
InnoDB优化，对一组提交(commit)操作执行一次低级I/O操作(日志写入)，而不是为每次提交单独刷新和同步。

<br>

- **hash index**
一种索引类型，用于使用相等运算符的查询而不是范围运算符。它用于MEMORY表，它是MOMORY存储引擎的默认值，但它还支持B-TREE索引，B-TREE索引对一般查询来说是更好的选择。

- **heartbeat**
发送的周期性消息，指示系统正常运行。

- **high-water mark**
表示上限的值，也可是运行时不应超过的硬性限制。
与**low-water mark**相反。

- **history list**
具有删除标记记录的事务列表，计划由InnoDB清除(purge)操作处理。记录在撤销(undo)日志中。历史列表的长度由`SHOW ENGINE INNODB STATUS`报告。如果历史列表长度超过`innodb_max_purge_lag`配置项，则么个DML操作都会稍微延迟，以允许清除操作完成刷新已删除的记录。

- **hole punching**
从页面释放空块(empty block)。InnoDB transparent page compression功能依赖于它的支持。

- **hot**
频繁地访问行、表或内部数据结构，需要某种形式的锁定或互斥的情况，这会导致性能或可伸缩性问题。

- **hot backup**
在数据库运行且应用程序正在读取和写入数据库时进行备份。
备份不仅仅是复制数据文件：它必须包括备份过程中插入或更新的任何数据;它必须排除备份过程中删除的任何数据;它必须忽略任何未提交的更改。

<br>

- **.idb文件**
file-per-table表空间和通用表空间的数据文件。每个表的文件表空间(`.idb`文件)包含一个表和关联的索引数据。一般表空间(`.idb`文件)可能包含多个表的表数据和索引数据。
`.idb`文件扩展不适用于系统表空间，该表空间由一个或多个ibdata文件组成。

- **.ibz文件**
当MySQL Enterprise Backup产品执行压缩备份时，它会将使用file-per-table设置创建的每个表空间文件从`.ibd`扩展名转换为`.ibz`扩展名。

- **.isl文件**
一个文件。它的功能类似于符号链接，没有实际符号链接机制的平台限制。

- **I/O-bound**
参考disk-bound。

- **ib-file set**
由InnoDB管理的文件集： 系统表空间， 每个表的表空间文件和重做日志文件(redo log)。根据MySQL版本和InnoDB配置，还可能包括通用表空间，临时表空间和撤消表空间文件。这个术语有时用于InnoDB文件结构和格式的详细讨论，以引用由InnoDB在MySQL数据库中管理的文件集。

- **ibbackup_logfile**
在热备份操作期间由MySQL Enterprise Backup产品创建的补充备份文件。

- **ibdata文件**
一组名称为ibdata1，ibdata2等的文件，构成InnoDB系统表空间。这些文件包含有关InnoDB表的元数据，以及一个或多个撤销日志(undo log)，更改缓冲区(change buffer)和双写入缓冲区域(doublewrite buffer)。它们还可以包含部分或全部数据。

- **ibtmp文件**
InnoDB临时表空间数据文件，用于非压缩的InnoDB临时表和相关对象。配置项`innodb_temp_data_file_path`允许用户定义临时表空间数据文件的相对路径，如果未指定，则默认在数据目录下创建。

- **ib_logfile**
一组文件，通常名为`ib_logfile0`和`ib_logfile1`，构成重做日志。有时也称为日志组。这些文件记录了尝试更改InnoDB表中数据的语句。在崩溃后启动时，会自动重播这些语句以更正由不完整事务写入的数据。
此数据文件不能用于手动恢复；对于该类型的操作，请使用二进制日志(binlog)。

- **ilist**
在InnoDB全文索引中，数据结构由document id和token的位置信息组成。

- **implicit(隐式) row lock**
InnoDB获取的行锁，以确保一致性，而无需你特别请求它。

- **in-memory database**
一种数据库系统，用于维护内存中的数据，以避免磁盘I/O和磁盘块与内存区域之间的转换而产生的开销。

- **incremental backup(增量备份)**
由MySQL Enterprise Backup产品执行的一种热备份，仅保存自某个时间点以来更改的数据。

- **index**
一种数据结构，通常通过形式表示成特定列或列集的所有值的树结构(B-TREE)，为表的行提供快速查找功能。
InnoDB表总是有一个表示主键(primary key)的聚集索引(clustered index)。它们还可以在一列或多列上定义一个或多个二级索引(secondary index)。根据其结构，二级索引可分为部分索引(partial index)、列索引(column index)和复合索引(composite index)。
索引是查询性能的关键方面。数据库架构师设计表、查询和索引，以允许快速查找应用程序所需的数据。理想的数据库设计在可行的情况下使用覆盖索引(overing index)，查询结果完全从索引计算，而不读取实际的表数据。每个外键约束(foreign key constraint)还需要一个索引，以有效地检查父表和子表中是否存在值。
虽然B-TREE索引最为常见，但不同类型的数据结构使用HASH索引(如memory存储引擎和InnoDB自适应哈希索引)。R-Tree索引用于多维信息的空间索引。

- **index cache**
保存InnoDB全文搜索的令牌数据(token)的内存区域。当数据在作为全文索引一部分的列中插入或更新时，它会缓冲数据以最小化磁盘I/O。当索引缓存已满时，令牌数据将写入磁盘。每个InnoDB全文所以都有自己独立的索引缓存，其大小由`innodb_ft_cache_size`配置项控制。

- **index condition pushdown**
Index Condition Pushdown(ICP)是一种优化，如果可以使用索引中的字段评估条件的某些部分，则将部分`WHERE`条件下推到存储引擎。ICP可以减少存储引擎必须访问基表的次数以及MySQL服务器必须访问存储引擎的次数。

- **index hint**
用于覆盖优化程序建议的索引的扩展SQL。

- **index prefix**
在应用于多个列的索引(称为复杂索引)，索引的初始列或前导列。

- **index statistics**

- **infimum record**
索引中的伪记录(pseudo record)，表示该索引中最小值之下的差距。

- **INFORMATION_SCHEMA**
提供MySQL数据字典(data dictionary)查询接口的数据库。要检查有关数据库的信息(元数据)，可以查询诸如`INFORMATION_SCHEMA.TABLES`和`INFORMATION_SCHEMA.COLUMNS`之类的表。而不是使用生成非结构化输出的`SHOW`命令。
`INFORMATION_SCHEMA`数据库还包含特定于InnoDB的表，这些表为InnoDB数据字典提供查询接口。使用这些表不是为了查看数据库的结构，而是获取有关InnoDB表的工作方式的实时信息，以帮助进行性能监视，调整和故障排除。

- **InnoDB**
一个MySQL组件，它结合了高性能和事务功能，可靠性，健壮性和并发访问。它体现了ACID的设计理念。代表作为存储引擎，它处理使用`ENGINE=INNODB`子句创建或更改的表。
InnoDB表非常适合热备份。

- **innodb_autoinc_lock_mode**
`innodb_autoinc_lock_mode`选项控制用于自动增量锁定的的算法。
当你有一个自增主键，则只能使用`innodb_autoinc_lock_mode=1`来使用基于语句的复制。此设置称为连续锁定模式，因为事务中的多行插入会接收连续的自动增量值。
如果您有`innodb_autoinc_lock_mode=2`，它允许插入操作具有更高的并发性，请使用基于行的复制而不是基于语句的复制。此设置称为交错锁定模式，因为同时运行的多个多行插入语句可以接收交错的自动增量值。
除兼容性目的外，不应使用`innodb_autoinc_lock_mode=0`设置。

- **innodb_file_format**
`innodb_file_format`选项定义用于新InnoDB文件每表表空间的文件格式。目前，您可以指定Antelope和Barracuda文件格式。

- **innodb_file_per_table**
一个重要的配置项，它影响InnoDB文件存储的许多方面，功能的可用性和I/O特性。

- **innodb_lock_wait_timeout**
`innodb_lock_wait_timeout`选项设置在等待共享资源变为可用或放弃和处理错误，重试或在应用程序中执行备用处理之间的平衡。

- **innodb_strict_mode**
`innodb_strict_mode`选项控制InnoDB是否在严格模式下运行，其中通常被视为警告的条件会导致错误。

- **insert**
SQL中的主要DML操作。

- **insert buffer**
change buffer之前的名称。

- **insert buffering**
在更改缓冲区中存储由INSERT操作产生的二级索引页的更改而不是立即写入更改的技术，以便可以执行物理写入以最小化随机I/O。

- **insert intention lock**
一种间隙锁(gap lock)，由行插入前的INSERT操作设置。

- **instance**
一个mysqld守护程序，管理一个数据目录，表示一个或多个带有一组表的数据库。

- **instrumentation**
在源代码级别进行修改以收集用于调整和调试的性能数据。

- **intention exclusive lock**
- **intention lock**
一种适用于表的锁，用于指示事务要在表中的行上获取的锁类型。
不同的事务可以在同一个表上获取不同类型的意图锁，但是获取表上的意图独占（IX）锁的第一个事务会阻止其他事务获取表上的任何S或X锁。

- **intention shared lock**

- **intrinsic temporary table**
优化程序使用的优化内部InnoDB临时表。

- **inverted index**
为文档检索系统优化的数据结构，用于InnoDB全文搜索的实现。

- **IOPS**
每秒I/O操作的缩写。

- **isolation level**
数据库处理的基础之一。
从最高的一致性和保护到最小，InnoDB支持的隔离级别是: SERIALIZABLE, REPEATABLE READ, READ COMMITTED, READ UNCOMMITTED。
InnoDB表中，许多用户可以保留默认所有操作的默认级别(REPEATABLE READ)。专家用户可以选在READ COMMITTED级别，因为它们通过OLTP处理推动可伸缩性的界限，或者在数据仓库操作期间，其中的轻微不一致性不会影响大量数据的聚合结果。边缘上的级别(SERIALIZABLE和READ UNCOMMITTED)将处理行为改变到很少使用它们的程度。

<br>

- **join**
通过引用包含相同值的表中的列来从多个表检索数据的查询(query)。理想情况下，这些列是InnoDB外键(foreign key)关系的一部分，它确保了引用完整性并且连接列已编制的索引。通常用于规范化数据设计中通过用数字ID替换重复的字符串来节省空间并提高查询性能。

<br>

- **KEY_BLOCK_SIZE**
用于指定InnoDB表中使用压缩行格式(compressed row format)的数据页(date page)大小的选项(默认8KB)。

<br>

- **latch**
InnoDB用于实现其内部存储器结构锁定的轻量级结构，通常以毫秒或微秒为单位保持一段时间。一般术语，包括互斥锁(mutexes)和读写锁(rw-locks)。它是InnoDB性能调整的重点，有关锁存器(latch)使用和争用的统计信息可通过 Performance Schema接口获取。

- **list**
InnoDB缓冲池表示为内存页列表。

- **lock**
控制对资源访问的对象的高级概念，作为锁定策略的一部分。

- **lock escalation**
在某些数据库系统中使用的操作，它将许多行锁转换为单个表锁，从而节省了内存空间，但减少了对表的并发访问。

- **lock mode**
共享锁(shared lock)允许事务读取表行。多个事务可以同时在同一行上获取共享锁。
独占锁(exclusive lock)允许事务更新化删除行。没有其它事务可以同时在同一行上获取任何类型的锁。
意图锁(intension lock)适用于表，用于指定事务要在表中的行上获取那种锁定。不同的事务可以在同一个表上获取不同类型的意图锁，但是获取表上的意图独占锁的第一个事务会阻止其它事务获取表上的任何共享锁或独占锁。

- **locking**
保护事务不被查看或更改其它事务正在查询或更改的数据的系统。

- **locking read**
锁定策略必须平衡数据库操作的可靠性和一致性与良好并发性所需的性能。
一个`SELECT`语句，它还对InnoDB表执行锁定操作。它有可能产生死锁，具体取决于事务的隔离级别。

- **log**
包括`redo log`, `undo log`, `error log`, `binary log`, `general query log`, `slow query log`。

- **log buffer**
保存要写入日志文件的数据的内存区域。由`innodb_log_buffer_size`配置项控制。

- **log file**
数据从log buffer内存区域写入到这些文件。

- **log group**

- **logical**
一种设计高级抽象方面的操作。

- **logical backup**
一种备份，可以重现表结构和数据，而无需复制实际的数据文件。逻辑备份的输出包含如`CREATE TABLE, INSERT...`语句，与物理备份形成对比。它提供了灵活性，但恢复时间比物理备份长得多。

- **loose_**
在Server启动后添加到InnoDB配置项的前缀，因此当前级别的MySQL无法识别的任何新配置项都不会导致启动失败。MySQL处理以此前缀开头的配置选项，但如果前缀后面的部分不是可识别的选项，则会发出警告而不是失败。

- **low-water mark**
表示下限的值，通常是某些纠正措施开始或变得更具侵略性的阈值。

- **LRU**
LRU(least recently used)是管理存储区域的常用方法。当需要空间来缓存较新的项目时，最近未使用的项目将被逐出。

- **LSN**
LSN(log sequence number)，这个任意的，不断增加的值表示与重做日志中记录的操作相对应的时间点。

<br>

- **.MRG文件**
包含MERGE存储引擎使用的其它表的引用的文件。

- **.MYD文件**
MySQL用于存储MyISAM表数据的文件。

- **.MYI文件**
MySQL用于存储MyISAM表索引的文件。

- **master server**
副本集中的数据库Master，用于处理数据的初始INSERT, UPDATE, DELETE。这些更改将传递管道其它Slave并在其上执行。

- **master thread**
InnoDB线程，在后台执行各种任务。这些任务中大多数都与I/O有关。
为了提高并发性，有时将操作从主线程移动到单独的后台线程。

- **MDL**
MDL(metadata lock)。用于放置对另一个事务同时使用的表执行DDL操作。

- **memcached**
MySQL和NoSQL软件堆栈的流行组件，允许对单个值进行快速读取和写入，并将结果完全缓存在内存中。

- **merge**
应用内存中缓存的数据的更改，更改缓冲区中记录的任何适用更改都将合并到缓冲池中的页面中。更新的数据最终由`FLUSH`机制写入表空间。

- **metrics counter**

- **midpoint insertion strategy**
最初将页面放入InnoDB缓冲池的技术不是在列表的最新端，而是在中间的某个位置。根据`innodb_old_blocks_pct`选项的设置，此点的确切位置可能会有所不同。

- **mini-transaction**
在DML操作期间在物理级别对内部数据结构进行更改时，InnoDB处理的内部阶段。

- **mixed-mode insert**
一个`INSERT`语句，为某些新行指定`auto-increment`值。

- **multiversion concurrency control(MVCC)**
这种技术是具有特定隔离级别的InnoDB事务执行一致的读操作。这是一种增强并发性的强大技术，允许查询继续进行而无需等待其它事务持有的锁。
这种技术在数据库世界中并不普遍，其它数据库产品和其它MySQL引擎不支持它。

- **mutex**
互斥变量(mutex variable)的非正式缩写。InnoDB用于表示和强制执行内部内存数据结构的独占访问锁的低级对象。获取锁定后，将阻止任何其他进程，线程等获取相同的锁定。与读写锁对比，InnoDB使用它来表示和强制执行内部内存数据结构的共享访问锁。
互斥锁(mutexes)和读写锁(rw locks)被统称为锁存器(latches)。

- **my.cnf**
Unix/Linux系统上，MySQL配置文件名称。

- **my.ini**
Windows系统上，MySQL配置文件名称。

- **mysql**
mysql程序时MySQL数据库的CLI解释器。它通过将请求传递给mysqld daemon来处理SQL语句以及特定的MySQL命令。

- **mysqlbackup**
MySQL Enterprise备份产品的CLI工具。

- **mysqldump**
一个命令，用于对数据库、表、表结构、表数据等某些组合执行逻辑备份。

- **mysqld**
mysqld是MySQL数据库的数据库引擎。它作为守护进程运行，不断等待请求并在后台执行维护工作。

<br>

- **natural key**
索引列，通常是主键，其中值具有一些真实世界的重要性。通常建议不要因为:
- 如果值应该更改，这可能需要大量索引维护来重新排序聚集索引(clustered index)，并在更新在每个二级索引(secondary index)中重复的主键值的副本
- 即使看似稳定的值也会以不可预测的方式改变，这些方式难以在数据库中正确表示。
因此，通常最好使用任意数值(arbitrary numeric)来形成组合键，如使用自增列(auto-increment column)。

- **neighbor page**
与特定页面具有相同程度(extent)的任何页面。当选择要刷新(flush)页面时，通常也刷新任何脏页面，作为传统硬盘的I/O优化。它通过`innodb_flush_neihbors`配置项进行控制。

- **next-key lock**
索引记录上的记录锁定(record lock)和索引记录之前的间隙上的间隙锁定(gap lock)的组合。

- **non-locking read**
不使用`SELECT ... FOR UPDATE`或`SELECT ... LOCK IN SHARE MODE`子句的查询。允许在只读事务(readonly transaction)中使用全局表的唯一查询类型。

- **non-repeatable read**
查询检索数据的情况，以及同一事务中的后续查询检索应该是相同数据的内容，但查询返回不同的结果。
这种操作违背了ACID数据库设计原则。在事务中，数据应该是一致的，具有可预测和稳定的关系。
在不同的隔离级别(isolation levels)中，可序列化读取(serializable read)和可重复读取级别阻止了不可重复读取，并且一致读取(consistent read)和读取未提交(read uncommitted)级别允许。

- **nonblocking I/O**
行业术语，与异步(asynchronous)I/O相同。

- **normalized**
数据库设计策略，将数据拆分为多个表，并将重复值压缩为由ID表示的单个行，以避免存储、查询和更新冗余或冗长(redundant or lengthy)的值。它通常用于OLTP应用程序。

- **NoSQL**
一组数据访问技术的广义术语，它不使用SQL语言作为读取和写入数据的主要机制。

- **NOT NULL约束(constraint)**
一种约束类型，指定列不能包含任何`NULL`值。

- **NULL**
SQL中的特殊值，表示缺少数据。任何涉及`NULL`值的算术运算或相等测试都会产生NULL结果。
NULL值在索引操作中起作用，因为对于性能，数据库必须最小化跟踪丢失数据值的开销。
由于主键必须能够唯一标识表中的每一行，因此单列主键不能包含任何`NULL`值，并且多列主键不能包含所有列中具有`NULL`值的任何行。

<br>

- **.OPT文件**
包含数据库配置信息的文件。由MySQL Enterprise Backup产品的`mysqlbackup`生成。

- **off-page列**
包含可变长度数据(如BLOB, VARCHAR)的列，由于列长度太长而无法放在B-Tree页面上。这个数据存储在溢出页面(overflow page)中。与旧的`COMPACT`行格式相比，`DYNAMIC`行格式对此类存储更有效。

- **OLTP**
OLTP(Online Transaction Processing)，数据库系统或数据库应用程序，它运行具有许多事务的工作负载，频繁写入和读取，通常一次影响少量数据。
凭借其行级锁定(row-level locking)和事务功能，InnoDB是OLTP应用程序中使用的MySQL表的理想存储引擎。

- **online**
一种不涉及数据库停机(shutdown)、阻塞(blocking)或受限(restricted)操作的操作类型。通常适用于DDL。
在备份中，热备(hot backup)是在线(online)操作，暖备(warm backup)是部分在线操作。

- **online DDL**
在DDL(主要是`ALTER TABLE`)期间改进InnoDB表的性能、并发性和可用性的功能。

- **optimistic**
一种指导关系型数据库系统的低级实现决策的方法。

- **optimizer**
MySQL组件，根据相关表的特征和数据分布，确定用于查询的最佳索引(index)和联结(join)顺序。

- **option**
MySQL的配置参数，可存储在配置文件中，也可在命令行上传递。

- **option file**
包含MySQL实例的配置选项的文件。

- **overflow page**
包含可变长度的单独分配磁盘页面的列(如BLOB, VARCHAR)，这些列太长而无法放在B-Tree页面上。这个关联列也称为页外列(off-page)。

<br>

- **.par文件**
包含分区定义的文件。由 MySQL Enterprise Backup的`mysqlbackup`生成。

- **page**
一个单元，表示InnoDB在磁盘(data file)和内存(buffer pool)之间的任何事件传输的数据量。页面可以包含一行或多行，具体取决于每行中的数据量。

- **page cleaner**
InnoDB后台线程，用于刷新(flush)缓冲池(buffer pool)中的脏页面。

- **page size**
对于MySQL v5.5，每个页面的固定大小为16KB。
从MySQL v5.6开始，InnoDB实例的页面大小可以是4KB, 8KB, 16KB，由`innodb_page_size`配置项控制。
从MySQL v5.7.6开始，InnoDB还支持32KB和64KB的页面大小(但不支持`ROW_FORMAT=COMPRESSED`)。

- **parent table**
外键关系的表，其中包含从子表执行初始列值。

- **partial backup**
包含MySQL数据库中的某些库或某些表的备份。

- **partial index**
仅表示列值的一部分索引，通常是长度为`VARCHAR`值的前N个字符(前缀)。

- **Performance Schema**
`performance_schema`模式提供了一组表，可以查询这些表以获取有关MySQL Server的许多内部的性能详细信息。

- **persistent statistics**
一种功能，可将InnoDB表的索引统计信息存储在磁盘上，从而为查询提供更好的计划稳定性。

- **pessimistic**
一种牺牲性能或并发性以支持安全性的方法。
InnoDB使用所谓`pessimistic locking`策略，以最大限度地减少死锁(deadlock)的可能性。在应用程序级别，你可通过一台是就获取事务所需的pessimistic lock策略来避免死锁。

- **phantom**
显示在查询结果集中但不在先前查询的结果集中的行。

- **physical**
设计硬件方面的操作。

- **physical backup**
复制实际数据文件的备份。

- **PITR**
PIRT(point-in-time recovery)时间点恢复的缩写。
还原备份以在特定日志和时间重新创建数据库状态的过程。

- **plan stability**
查询执行计划的一个属性，优化程序每次为给定查询做出相同的选择，以便性能一致且可预测。

- **prefix**
index prefix.

- **primary key**
一组列，基于这组列的索引可以唯一标识表中的每一行。
InnoDB要求每个表都有这样的索引，并根据主键的列值组织表存储。
选择主键值时，请考虑使用任意值（synthetic key），而不是依赖于从某些其他源（natural key）派生的值。

- **pseudo-record**
索引中的人工记录，用于锁定当前不存在的键值或范围。

- **Pthreads**
POSIX线程标准，它定义了用于Unix和Linux系统上的线程和锁定操作的API，InnoDB将此实现用于互斥锁(mutexes)。

- **purge**
由一个或多个单独的后台线程（由`innodb_purge_threads`控制）执行的一种垃圾收集，它以定期计划运行。

- **purge buffering**
在更改缓冲区中存储由`DELETE`操作产生的二级索引页的更改而不是立即写入更改的技术，以便可以执行物理写入以最小化随机I/O。

- **purge lag**
InnoDB历史列表的另一个名称。与` innodb_max_purge_lag`配置项有关。

- **purge thread**
InnoDB进程中的一个线程，专门用于执行定期清除操作。

<br>

- **query**
在SQL中，从一个或多个表中读取信息的操作。

- **query execution plan**
优化程序关于如何最有效地执行查询的决策集，包括要使用的索引或索引，以及联结表的顺序。

- **query log**

- **quiesce**
要减少数据库活动量，通常要为`ALTER TABLE`、备份或关闭等操作做准备。

<br>

- **R-tree**
一种树数据结构，用于诸如地理坐标(geographical coordinates)、矩形(rectangle)、多边形(polygons)之类的多维数据(multi-dimensional)进行空间索引。

- **RAID**
磁盘阵列(Redundant Array of Inexpensive Drives)的缩写。跨多个驱动器传播I/O操作可在硬件层面实现更高的并发性，并提高低级写入操作的效率，否则将按顺序执行。

- **random dive**
一种快速估量列中不同值的数量的技术。InnoDB从索引中随机采样页面，并使用该数据估计不同值的数量。

- **raw backup**
在应用binary log和incremental backup中的更改之前，MySQL Enterprise Backup产品生成的初识备份文件集。

- **READ COMMITTED**
为了提供性能，使用锁定策略的隔离级别可以放松事务之间的某些保护。事务无法查看来自其它事务未提交的数据，但它们可以查看当前事务启动后由另一个事务提交的数据。因此，事务永远不会看到任何不良数据，但它看到的数据可能在某种程度上取决于其它事务的时间。

- **read phenomena**
当事务读取另一个事务已修改的数据时可能发生 dirty reads, non-repeatable reads, phantom reads等现象。

- **READ UNCOMMITTED**
在事务之间提供最少保护的隔离级别。查询采用锁定策略，允许它们在通常等待另一个事务的情况下继续。但是，这种额外的性能是以不太可靠的结果为代价的，包括已被其他事务更改但尚未提交的数据(dirty data)。请谨慎使用此隔离级别，并注意结果可能不一致或可重现，具体取决于其它事务同时执行的操作。通常，具有此隔离级别的事务仅执行查询，而不执行插入，更新或删除操作。

- **read view**
InnoDB的MVCC机制使用的内部快照。某些事务会查看事务启动时的数据值。使用读取视图(read view)的隔离级别是`REPEATABLE READ`, `READ COMMITTED`, `READ UNCOMMITTED`。

- **read-ahead**
一种I/O请求，它将一组页面异步预取到缓冲池中，预计很快就会需要这些页面。线性预读技术基于前一范围中的页面的访问模式预取一个范围的所有页面。

- **read-only transaction**
一种可以优化InnoDB表的事务，通过消除为每个事务创建读取视图所涉及的一些簿记(bookkeeping)。

- **record lock**
索引记录上的锁定。

- **redo**
当DML语句对InnoDB表进行更改时，以记录为单位的数据记录位于重做(redo)日志中。在崩溃恢复(crash cecovery)期间使用它来纠正未完成的事务写入的数据。

- **redo log**
在崩溃恢复期间使用的基于磁盘的数据结构，用于纠正由未完成的事务写入的数据。

- **redundant row format**
旧的InnoDB行格式。
MySQL v5.0.3到MySQL v5.7.8的新格式为`COMPACT`。
MySQL v5.7.9，默认行格式由`innodb_default_fow_format`配置项定义，默认为`DYNAMIC`，也可指定`REDUNDANT`行格式与旧的InnoDB表兼容。

- **referential integrity**
始终以一致的格式维护数据的技术，这是ACID理念的一部分。

- **relational**
数据库Server对**一对一**(one-to-one)，**一对多**(one-to-many)，**多对一**(many-to-one)和**唯一性**(uniqueness)等关系进行编码和实施。
数据库Server可以使用这些关系来防止插入错误数据，并找到查找信息的有效方法。
在数据库级别，这些关系通过SQL功能表示。
在数学上下文中，数据库中的关系原子集合论。

- **relevance**
在全文搜索功能中，一个数字表示搜索字符串与全文索引中的数据之间的相似性。

- **REPEATABLE READ**
InnoDB的默认隔离级别。它可以防止被查询的任何行被其他事务更改，从而阻止不可重复的读取但不阻止幻像读取。

- **repertoire**
Repertoire是一个应用于字符集的术语。

- **replication**

- **restore**
还原数据库。

- **rollback**
用于结束事务的SQL语句，撤销事务所做的任何更改。它与提交(commit)相反，提交使得在事务中进行任何更改都是永久性的。
默认情况下，MySQL使用自动提交设置，该设置会在每个SQL语句后自动发出提交。你必须先更改此设置，然后才能使用回滚技术。

- **rollback segment**
包含撤销(undo)日志的存储区域。

- **row**
由一组列定义的逻辑数据结构。

- **row format**
InnoDB表行的磁盘存储格式。

- **row lock**
一种锁，可防止另一个事务以不兼容的方式访问行。其它事务可以自由地写入同一表中的其它行。这是由InnoDB表上的DML操作完成的锁定类型。

- **row-based replication**
一种部分形式，事件从Master传播，指定如何更改Salve上的各行。可以安全地用于`innodb_autoinc_loc_mode`选项的所有设置。

- **row-level locking**
用于InnoDB表的锁定机制，依赖于行锁(row lock)而不是表锁(table lock)。多个事务可以同时修改同一个表。只有当两个事务尝试修改同一行时，其中一个事务才会等待另一个事务完成（并释放其行锁）。

- **rw-lock**
InnoDB用于根据某些规则表示并强制执行对内部内存数据结构(in-memory)的共享访问锁定的低级对象。与互斥(metexes)形成对比，InnoDB用它来表示和强制执行对内部内存数据结构的独占访问(exclusive access)。互斥锁和读写锁被统称为锁存器(latches)。

读写锁类型包括:
- **s-locks**(shared locks)，提供对公共资源的读访问权限；
- **x-locks**(exclusive locks)， 提供对公共资源的写访问权限，同事不允许其它线程进行不一致的读取；
- **sx-locks**(shared-exclusive locks)，提供对公共资源的写访问权限，同事允许其它线程进行不一致的读取。

| - | S | SX | X |
| - | - | - | - |
| S | Compatible | Compatible | Conflict |
| SX | Compatible | Conflict | Conflict |
| X | Conflict | Conflict | Conflict |

<br>

- **savepoint**
保存点(savepoint)有助于实现嵌套事务。它们可用于作为较大事务一部分的为表的操作提供范围。

- **scalability**
能够添加更多工作并向系统发出更多同时请求，而不会因超出系统容量而限制性能突然下降。

- **scale out**
横向扩展，一种通过添加新服务器和更多MySQL实例来提高可伸缩性的技术。

- **scale up**
纵向扩展，一种通过增加现有硬件或软件的容量来提供可扩展性的技术。

- **schema**
从概念上讲，模式(schema)是一组相互关联的数据库对象。如表、表列、列的数据类型、索引、外键...这些对象通过SQL语法连接，列构成表、外键引用表和列，依次类推。理想情况下，它们也是逻辑连接的，作为统一应用程序或灵活框架的一部分一起工作。
在MySQL中，物理上，模式(schema)与数据库(database)是同义词。你可在MySQL SQL语法中使用关键字`SCHEMA`替换`DATABASE`。
```sql
# 例如

SHOW DATABASES;
SHOW SCHEMAS;

CREATE SHCEMA;
CREATE DATABASE;
```

- **search index**
在MySQL中，全文搜索(full-text search)查询使用特殊类型的`FULLTEXT index`。
MySQL v5.6.4中，InnoDB和MyISAM都支持全文索引；以前，这些索引只支持MyISAM。

- **secondary index**
一种表示表列的子集的InnoDB索引。InnoDB表可以包含零个、一个或多个二级索引(辅助索引)。
二级索引可用于满足仅查询来自索引列的值。对于更复杂的查询，他可用于标志表中的相关行，然后使用聚簇索引(clusterd index)通过查找检索这些行。
传统上，创建和删除二级索引会因复制InnoDB表中的所有数据而产生大量开销。快速索引(fast index)创建功能使InnoDB二级索引的`CREATE INDEX`和`DROP INDEX`语句更快。

- **segment**
InnoDB表空间的一个区。如果表空间类似于目录，则段与该目录中的文件类似。段可以增长，可以创建新段。

- **selectivity**
数据分布属性，列中不同值的数量除以表中的记录数。高选择性意味着列值相对独特，并且可以通过索引有效地检索。

- **semi-consistent read**
一种用于`UPDATE`语句测读操作，即`READ COMMITTED`和`consistent read`。当`UPDATE`语句检查已锁定的行时，InnoDB会将最新提交的版本返回给MySQL，以便MySQL可以确定该行是否与`UPDATE`的`WHERE`条件匹配。如果行匹配，MySQL再次读取该行，这次InnoDB锁定它或等待锁定它。这种类型的读取操作只能在事务具有`READ COMMITTED`隔离级别或启用`innodb_locks_unsage_for_binlog`配置项时发生。

- **SERIALIZABLE**
隔离级别(isolation level)，是用最保守的锁定策略，以防止任何其它事务插入或更改此事务读取的数据，直到完成为止。这样，可以在事务中反复运行相同的查询，并确保每次都检索相同的结果集。

- **shared lock**
一种锁，允许其它事务读取锁定的对象，并且还获取其上的其它共享锁，但不写入它。

- **shared tablespace**
引用系统表空间或通用表空间的另一种方法。MySQL 5.7中引入了通用表空间。多个表可以驻留在共享表空间中。只有一个表可以驻留在每个表的文件表空间中。

- **sharp checkpoint**
将重做条目包含在重做日志的某些部分中的所有脏缓冲池页面刷新到磁盘的过程。

- **shutdown**
停止MySQL Server的过程。

- **slave server**
通常缩短为**slave**。

- **slow query log**
一种用于MySQL Server处理的SQL语句的性能调整的日志。你必须先启用此功能才能使用它。

- **slow shutdown**
一种关闭，在完成之前执行额外的InnoDB flushing操作，也称为clean shutdown。由配置项`innodb_fast_shutdown=0`或命令`SET GLOBAL innodb_fast_shudown=0;`指定。

- **snapshot**
在特定时间的数据表示，即使其它事务提交更改，也保持相同。

- **sort buffer**
用于在创建InnoDB索引期间对数据进行排序的缓冲区。

- **space ID**
用于唯一标识MySQL实例中的InnoDB表空间的标识符。系统表空间的space ID始终未零。

- **sparse file**
一种文件类型，通过将标识空快的元数据写入磁盘而不是写入实际的空白空间来有效地使用文件系统空间。InnoDB透明页面压缩(transparent page compression)功能依赖于稀疏文件(sparse file)支持。

- **spin**
一种持续测试资源是否可用的等待操作类型。

- **SQL**
结构化查询语言(Structured Query Language)，是执行数据库的标准语言。

- **startup**
启动MySQL Server的过程。

- **statement-based replication**
一种复制形式，其中SQL语句从Master发送到Slave上重放(replay)。需要注意`innodb_autoinc_lock_mode`选项的设置，以避免自动增量锁定(auto-increment locking)的潜在时序问题。

- **statistics**
与每个InnoDB表和索引相关的估计值，用于构建有效的查询执行计划。

- **stemming**
能够根据共同的词根搜索单词的不同变体，如单数、复数，过去、现在、将来的动词时态。

- **stopword**
在全文索引中，一个被认为是普遍或微不足道的单词，它从搜索索引中被省略并在搜索查询中被忽略。

- **storage engine**
MySQL数据库的一个组件，用于执行存储、更新、查询数据的低级工作。

- **stored generated column**
其值是根据列定义中包含的表达式计算的一个列。

- **stored object**
一个存储的程序或试图。

- **stored program**
存储的例程(routine)(过程或函数)，触发器或事件调度程序事件。

- **stored routine**
一个存储的过程或函数。

- **strict mode**
由`innodb_strict_mode`选项控制的通用名称。启用此配置会导致某些通常被视为警告的情况被视为错误。

- **sublist**
在表示缓冲池的列表结构中，相对较旧且相对较新的页面由列表的不同部分表示。

- **supremum record**
索引中的伪记录(pseudo-record)，表示该索引中最大值之上的差距。

- **surrogate key**
synthetic key的同义词名称。

- **synthetic key**
索引列，通常是主键，其中值是任意分配的。通常使用自增(auto-increment)列来完成。通过将值视为完全任意，你可以避免过度限制性规则和错误的应用程序假设。
也称为surrogate key，与natural key相反。

- **system tablespace**
一个或多个数据文件(ibdate文件)，包含InnoDB相关对象(InnoDB数据字典)的元数据，以及更改缓冲区(change buffer)，双写缓冲区(doublewrite buffer)和可能的撤销日志(undo logs)。

<br>

- **.TRG文件**
包含触发器参数(trigger parameters)的文件。具有此扩展名的文件始终包含在由MySQL Enterprise Back产品的`mysqlbackup`令生成的备份中。

-**.TRN文件**
包含触发器命名空间(trigger namespace)信息的文件。具有此扩展名的文件始终包含在由MySQL Enterprise Back产品的`mysqlbackup`令生成的备份中。

- **table**
每个MySQL表都与特定的存储引擎有关联。

InnoDB表具有影响性能(Performance)、可伸缩性(scalability)、备份(backup)、管理和开发的特定物理和逻辑特性。
在文件存储方面，InnoDB表属于以下表空间类型之一:
- 共享的InnoDB系统表空间，由一个或多个`idbdata`文件组成；
- 每个表的文件表(file-per-table)空间，由单个`.idb`文件组成；
- 共享的通用表空间(general tablespace)，由单个`.idb`文件组成。

- **table lock**
一个阻止其它事务访问表的锁。
InnoDB通过使用诸如DDL，row locks，用于处理DML语句和查询的一致读取(consistent reads)等技术，做出了相当大的努力来使这种锁不必要。
你可使用`LOCK TABLES`SQL语句创建这样的锁，`UNLOCK TABLES`解除锁定。具体查看帮助`HELP LOCK`。

- **table scan**
see full table scan.

- **table statistics**
see statistics.

- **table type**
存储引擎的过时同义词。

- **tablespace**
一个数据文件，可以保存一个或多个InnoDB表和相关索引的数据。
系统表空间包含InnoDB数据字典。

- **temporary table**
其数据不需要真正持久化的表。例如，临时表可以用作复杂计算或转换中的中间结果的存储区域;崩溃后不需要恢复这些中间数据。
有时，数据本身会在设定的时间自动删除，例如在事务结束或会话结束时。对于某些数据库产品，表本身也会自动删除。

- **temporary tablespace**
MySQL 5.7中引入的非压缩InnoDB临时表和相关对象的表空间。
`innodb_temp_data_file_path`配置选项定义临时表空间数据文件的相对路径，名称，大小和属性。如果未指定`innodb_temp_data_file_path`，则默认行为是在数据目录中创建名为`ibtmp1`的单个自动扩展12MB数据文件。在每个服务器启动时重新创建临时表空间，并接收动态生成的空间ID。临时表空间不能驻留在原始设备上。如果无法创建临时表空间，则拒绝启动。
临时表空间在正常关闭或中止初始化时被删除。发生崩溃时，不会删除临时表空间。在这种情况下，数据库管理员可以手动删除临时表空间或使用相同的配置重新启动服务器，从而删除并重新创建临时表空间。

- **text collection**
全文索引(FULLTEXT INDEX)中包含的列集。

- **thread**
处理单元，通常比进程更亲量化，允许更大的并发性(concurrency)。

- **torn page**
由于I/O设备配置和硬件故障的组合可能发生的错误情况。
如果数据以小于InnoDB页面大小的块（默认情况下为`16KB`）写出，则写入时硬件故障可能导致只有部分页面存储到磁盘。InnoDB doublewrite buffer可以防止这种可能性。

- **TPS**
每秒事务数(transaction per second)，是有时在基准测试中使用的度量单位。它的值取决于特定基准测试所代表的工作负载，以及一些配置情况。

- **transaction**
事务是可以提交(committed)或回滚(rolled back)的原子工作单元。当事务对数据库进行多次更改时，要么在提交事务时所有更改都成功，要么在事务回滚时撤消所有更改。

- **transaction ID**
与每行(row)相关的内部字段。通过`INSERT, UPDATE, DELETE`操作对此字段进行物理更改，以记录哪个事务已锁定该行。

- **transparent page compression**
MySQL 5.7.8中添加的一项功能，允许对驻留在每个表文件表空间中的InnoDB表进行页面级压缩。通过使用`CREATE TABLE`或`ALTER TABLE`指定`COMPRESSION`属性来启用页面压缩。

- **transportable tablespace**
允许将表空间从一个实例移动到里一个实例的功能。

- **truncate**
一种DDL操作，用于删除表的全部内容，同时保持表和相关索引不变。

- **tuple**
指定有序元素集的技术术语。

- **two-phase commit**
作为XA规范下的分布式事务的一部分的操作。

<br>

- **undo**
在事务的整个生命周期中维护的数据，记录所有更改，以便在回滚操作时可以撤消这些更改。

- **undo buffer**
see undo log.

- **undo log**
撤销日志，保存由活动事务修改的数据副本的存储区域。如果另一个事务需要查看原始数据（作为一致读取操作的一部分），则从该存储区域检索未修改的数据。

- **undo log segment**
撤销日志的集合。撤销日志段位于回滚段(rollback segments)中。撤销日志段可能包含来自多个事务的撤销日志。

- **undo tablespace**
撤销表空间包含撤销日志。

- **unique constraint**
唯一约束，列不能包含任何重复值。

- **unique key**
唯一键，包含唯一索引的列集(一个或多个)。
如果可以定义仅与一行匹配的WHERE条件，并且查询可以使用关联的唯一索引，则可以非常有效地执行查找和错误处理。

<br>

- **variable-length type**
可变长度的数据类型。如`VARCHAR, VARBINARY, BLOB, TEXT...`
InnoDB将长度大于或等于768Bytes的固定长度字段视为可变长度字段，可以在页外(off-page)存储。

- **victim**
检测到死锁时自动选择回滚的事务。
可使用`innodb_deadlock_detect`配置项禁用死锁检测。

- **view**
一个存储的查询，在调用时会生成结果集。视图闯荡虚拟表(virutal table)。

- **virtual column**
see virtual generated column.

- **virtual generated column**
一列，其列值是根据列定义中包含的表达式计算的。不存储列值，但在任何`BEFORE`触发器之后立即读取行时来计算列值。虚拟生成列不占用存储空间。InnoDB支持虚拟生成列的二级索引。

- **virtual index**
虚拟索引是一个或多个虚拟生成列上的二级索引，或是虚拟生成列与常规列或存储生成列的组合。

<br>

- **wait**
当无法立即完成诸如`locak, mutex, latch`等操作时，InnoDB会暂停并再次尝试。暂停的机制很精细，它也称为等待。

- **warm backup**
在数据库运行时进行备份，但在备份过程中限制某些数据库操作。例如，表可能变为只读。对于繁忙的程序，你可能更喜欢热备(hot backup)。

- **warm up**
在启动后的一段时间内在典型工作负载下运行系统，以便缓冲池和其他内存区域在正常条件下填充。当MySQL服务器重新启动或承受新工作负载时，此过程会自然发生。

- **workload**
SQL和其他数据库操作的组合和数量，由数据库应用程序在典型或高峰使用期间执行。您可以在性能测试期间将数据库置于特定工作负载中，以识别瓶颈或在容量规划期间。

- **write combining**
一种优化技术，可在从InnoDB缓冲池中刷新(FLUSH)脏页面时减少写操作。

<br>

- **XA**
用于协调分布式事务的标准接口，允许多个数据库参与事务，同时保持ACID合规性。
默认情况下启用XA分布式事务支持。如果您不使用此功能，则可以禁用`innodb_support_xa`配置选项，从而避免每个事务的额外fsync的性能开销。

<br>

- **young**
InnoDB缓冲池中页面的一个特征意味着它最近已被访问，因此在缓冲池数据结构中移动，因此LRU算法不会很快刷新它。此术语用于与缓冲池相关的表的某些`INFORMATION_SCHEMA`列名称中。






<br/>
<br/>

---

<br>
<br/>













# 安装和升级

- mysql-repo: <http://repo.mysql.com/>
- yum-repo: <http://repo.mysql.com/yum/>


<br>


安装MySQL一般遵循以下步骤：

- 确定MySQL是否支持你的平台(platform)
	+ Unix、Linux、FreeBSD
	+ Windows
	+ OS X
- 选择要安装的发行版(distribution)
- 下载你想要安装的发行版
- 安装发行版
- 执行任何必要的安装后设置


<br>
<br>


## 通用安装指南

General Installation Guidance

<br>
<br/>


### 安装哪个发行版和MySQL版本

Which MySQL Version and Distribution to Install

<br>

在准备安装MySQL时，请决定使用哪种版本(version)和发行(distribution)格式(binary or source)


首先，决定安装开发版还是稳定版。

- Development release
	+ 具有新功能，但不推荐用于生产环境
- General Availability(GA) release
	+ 也称为稳定版(stable release)，推荐为生产环境使用

MySQL命名方案(naming scheme)， 例如MySQL5.7.1：

- 5为主版本号(major)
- 7为次版本号(minor)
- 1为发行(release)系列版本号
	+ 系列号描述了稳定的功能集。对于每个新的修补程序，这都会增加。

在选择要安装的MySQL版本之后，决定要为操作系统安装哪个发行版格式。

- 二进制(binary)
	+ RPM, DMG
- 源码(source)
	+ tar, zip

在某些情况下，最好使用源码安装MySQL：

- 想在某个明确的位置安装MySQL
- 希望使用二进制发行版中未包含的特性配置mysqld
- 希望配置mysqld，而不需要二进制发行版中包含的一些功能
- 你希望读取或修改组成MySQL的C、C++源代码
- 源码发行版比二进制发行版包含更多的测试和示例


<br/>
<br/>


### 如何获取MySQL

How to Get MySQL

<br>

- MySQL当前版本下载页： <https://dev.mysql.com/downloads/>
- 完整的MySQL镜像： <https://dev.mysql.com/downloads/mirrors/>
- 基于RPM的Linux平台，MySQL Yum Repository： <https://dev.mysql.com/downloads/repo/yum/>
- 基于Debian的Linux平台，MySQL APT Repository： <https://dev.mysql.com/downloads/repo/apt/>
- SUSE Linux平台，MySQL SUSE Repository： <https://dev.mysql.com/downloads/repo/suse/>


<br/>
<br/>


### 使用MD5校验和或GnuPG验证程序完整性

Verifying Package Integrity Using MD5 Checksums or GnuPG

<br>

下载好MySQL包并在安装它之前，请确保它是完整的并未被篡改。有如下三种方法：

- MD5 checksums
- Cryptographic signatures using GnuPG, the GNU Privacy Guard
- For RPM packages, the built-in RPM integrity verification mechanism


<br/>
<br/>

#### 验证MD5校验和

Verifying the MD5 Checksum

<br>

应确保下载的MySQL包的MD5校验和与MySQL官方提供的校验和相匹配。

```sh
md5sum mysql-standard-5.7.22-linux-i686.tar.gz
#aaab65abbec64d5e907dcd41b8699945  mysql-standard-5.7.22-linux-i686.tar.gz
```


<br>

#### 使用GnuPG进行签名检查

Signature Checking Using GnuPG

<br>

要验证软件包的签名，首先需要我们的公共GPG密钥的副本。可从<http://pgp.mit.edu/>下载。
你想要获得的密钥名为**mysql-build@oss.oracle.com**，如下:

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1.4.5 (GNU/Linux)

mQGiBD4+owwRBAC14GIfUfCyEDSIePvEW3SAFUdJBtoQHH/nJKZyQT7h9bPlUWC3
RODjQReyCITRrdwyrKUGku2FmeVGwn2u2WmDMNABLnpprWPkBdCk96+OmSLN9brZ
fw2vOUgCmYv2hW0hyDHuvYlQA/BThQoADgj8AW6/0Lo7V1W9/8VuHP0gQwCgvzV3
BqOx后面还有很多，省略
-----END PGP PUBLIC KEY BLOCK-----
```

使用`gpg --import`将密钥导入到个人公共GPG密钥环中。如公共密钥为`mysql_pubkey.asc`：

```
gpg --import ./mysql_pubkey.asc


#或使用public key id下载公共密钥
gpg --recv-keys $pub-key-id
```

在rpm包中验证:

```
rpm --import ./mysql_pubkey.asc
```

确保两个文件都放置于同一目录下，然后运行命令验证签名：

```
gpg --verify package_name.asc

gpg --verify mysql-standard-5.7.22-linux-i686.tar.gz.asc
gpg: Signature made Tue 01 Feb 2011 02:38:30 AM CST using DSA key ID 5072E1F5
gpg: Good signature from "MySQL Release Engineering <mysql-build@oss.oracle.com>"
```

<br>

#### 使用RPM进行签名检查

Signature Checking Using RPM

<br/>

```
rpm --checksig package_name.rpm

[zhang@zabbix ~]$ rpm --checksig mysql-community-server-5.7.20-1.el7.x86_64.rpm
mysql-community-server-5.7.20-1.el7.x86_64.rpm: (sha1) dsa sha1 md5 gpg OK
```

rpm还支持从URL加载密钥:

```
rpm --import http://dev.mysql.com/doc/refman/5.7/en/checking-gpg-signature.html
```


<br/>
<br/>


### 安装布局

Installation Layouts

<br/>

不同的安装类型(native packages, binary tarballs, and source tarballs)有不同的安装布局，这样可能会导致混淆。


<br/>
<br/>



## 在Unix/Linux上使用通用二进制文件安装MySQL

Installing MySQL on Unix/Linux Using Generic Binaries

<br>

包括以压缩的tar文件形式的通用二进制发行版，以及针对特定平台封装格式的二进制文件。

MySQL压缩tar文件二进制发行版具有** mysql-VERSION-OS.tar.gz**的文件格式。

MySQL依赖于`libaio` Library：

```sh
yum install -y libaio
```

<br>

默认地，tar文件二进制发行版，解压后安装于`/usr/local/mysql`目录。会在目录下生产 通用Unix/Linux二进制包的MySQL安装布局目录

目录 | 内容
- | -
bin | mysqld server, client and utility programs
docs | MySQL manual in Info format
man | Unix manual pages
include | Include (header) files
lib | Libraries
share | Error messages, dictionary, and SQL for database installation
support-files | Miscellaneous support files


大致命令如下：

```
shell> groupadd mysql
shell> useradd -r -g mysql -s /bin/false mysql
shell> cd /usr/local
shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz
shell> ln -s full-path-to-mysql-VERSION-OS mysql
shell> cd mysql
shell> mkdir mysql-files
shell> chown mysql:mysql mysql-files
shell> chmod 750 mysql-files
shell> bin/mysqld --initialize --user=mysql
shell> bin/mysql_ssl_rsa_setup
shell> bin/mysqld_safe --user=mysql &
# Next command is optional
shell> cp support-files/mysql.server /etc/init.d/mysql.server



#添加环境变量
export PATH=$PATH:/usr/local/mysql/bin
```


<br/>
<br/>


## 在Linux上安装MySQL

Installing MySQL on Linux

<br>

Linux支持多种方法来安装MySQL。建议使用Oracle提供的一个发行版：

- Apt
- Yum
- Zypper
- RPM
- DEB
- Generic
- Source
- Docker
- Oracle Unbreakable Linux Network

作为一个选择，你可以使用系统中的包管理工具自动下载和安装MySQL。


<br>
<br>


### 在Linux上使用Yum Repository安装MySQL

Installing MySQL on Linux Using the MySQL Yum Repository

<br>


**安装一个全新的MySQL的步骤：**

- 添加MySQL Yum Repository
	- 首先，添加MySQL Yum repository到你的系统仓库列表
	- 选择和下载对应平台的release 或者 手动添加repository文件
	- 安装release package

```sh
#yum localinstall platform-and-version-specific-package-name.rpm
yun install http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql57-community-release-el7-10.noarch.rpm


yum repolist enabled | grep "mysql.*-community.*"
```

- 选择一个release series

默认是最新的GA series，当前最新是MySQL5.7。

查看所有的MySQL Yum repository: `yum repolist all | grep mysql`

安装最新MySQL不需要配置，而安装先前的版本则需要指定GA series。disable最新的GA series并且enable需要的GA series。

```
yum-config-manager --disable mysql57-community

yum-config-manager --enable mysql56-community
```

**或者手动创建repo，可直接定义版本**

```
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

- 安装MySQL

在安装MySQL过程中出现错误，请务必查看日志文件。

```
yum install -y mysql-community-server mysql-community-client

#也可不安装客户端

```

- 开启MySQL Server

```
service mysqld start
#Starting mysqld:[ OK ]

service mysqld status
```

**在服务器初始启动时，如果服务器的数据目录为空，则会发生一下情况：**

- 服务器已初始化
- SSL certificate and key files 在数据目录中生成
- validate_password已安装并启用
- 超级用户账户'root'@'localhost'被创建，超级用户密码被设置并被存储在error log files
	+ 这一点和以前版本有很大区别，我被坑惨了

**注意：**
> ValidPassword的默认密码策略要求包含大写字母、小写字母、数字和特殊字符，并且密码长度至少为8个字符

```
#查看初始密码
grep 'temporary password' /var/log/mysqld.log


#无法使用mysqladmin修改密码，需要登录mysql后修改

mysql -uroot -p


#重置密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPass4!;


#如果找不到初始密码
vim /etc/my.cnf

#在[mysqld]最后行加上skip-grant-tables实现无认证登录
#重启MySQL
UPDATE  mysql.user  SET  authentication_string =PASSWORD('新密码')  WHERE  USER='xxx';


#修改默认密码策略
#更改密码强度
set global validate_password_policy=0;

#设置密码最小长度
set global validate_password_length=4;
```


<br>

**使用Yum安装额外的MySQL产品和组件**

你可使用Yum安装和管理MySQL的个别组件。

```sh
yum --disablerepo=\* --enablerepo='mysql*-community*' list available

yum install package-name

#栗子
yum install mysql-community-libs
```


<br/>
<br/>


### 在Linux上使用Oracle提供的RPM包安装MySQL

Installing MySQL on Linux Using RPM Packages from Oracle

<br>

MySQL Community Edition的rpm包如下：

包名 | 描述
- | -
mysql-community-server | Database server and related tools
mysql-community-client | MySQL client applications and tools
mysql-community-common | Common files for server and client libraries
mysql-community-server-minimal | Minimal installation of the database server and related tools
mysql-community-devel | Development header files and libraries for MySQL database client applications
mysql-community-libs | Shared libraries for MySQL database client applications
mysql-community-libs-compat | Shared compatibility libraries for previous MySQL installations
mysql-community-embedded | MySQL embedded library
mysql-community-embedded-devel | Development header files and libraries for MySQL as an embeddable library
mysql-community-test | Test suite for the MySQL server

<br>

```
#rpm -qpl mysql-community-server-version-distribution-arch.rpm

#yum install mysql-community-{server,client,common,libs}-*


wget http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql-community-server-5.7.20-1.el7.x86_64.rpm
wget http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql-community-client-5.7.20-1.el7.x86_64.rpm

yum install -y mysql-community-server-5.7.20-1.el7.x86_64.rpm mysql-community-client-5.7.20-1.el7.x86_64.rpm
```

<br>

**Linux RPM包MySQL开发区的安装布局：**

文件或资源 | 位置
- | -
Client programs and scripts | /usr/bin
mysqld server | /usr/sbin
configuration file | /etc/my.cnf
data directory | /var/lib/mysql
error log file | /var/log/mysqld.log
Value of secure_file_priv | /var/lib/mysql-files
System V init script | /etc/init.d/mysqld
Systemd service | mysqld
pid file | /var/run/mysql/mysqld.pid
socket | /var/lib/mysql/mysql.sock
Keyring directory | /var/lib/mysql-keyring
Unix manual pages | /usr/share/man
include (header) files | /usr/include/mysql
Libraries | /usr/lib/mysql
Miscellaneous support files (for example, error messages, and character set files)	| /usr/share/mysql

<br>

The installation also creates a user named mysql and a group named mysql on the system.

> **注意**
> 安装MySQL会在系统上生成一个名为mysql的用户和群组
> 安装以前的MySQL版本可能会创建`my.cnf`配置文件。强烈建议先将`my.cnf`进行迁移，然后删除它。之后才安装MySQL


<br/>
<br/>


### 用systemd管理MySQL Server

Managing MySQL Server with systemd

<br>

#### systemd综述

Overview of systemd

<br>

`systemd`提供了MySQL Server的自动开启和关闭，使用`systemctl`命令进行管理。

或者，使用`system V`系统兼容的`service`命令。

```
systemctl {start|stop|restart|status} mysqld

service mysqld {start|stop|restart|status}
```

对systemd的支持包括这些文佳：

- mysqld.service
	+ systemd服务单元配置文件，以及有关MySQL服务的详细信息
- mysqld@.service
	+ 用于管理多个MySQL实例
- mysqld.tmpfiles.d
	+ 包含支持临时文件功能的信息
- mysqld_pre_systemd
	+ 支持单元文件的脚本

<br>

#### 为MySQL配置systemd

Configuring systemd for MySQL

<br>

为MySQL添加或修改systemd选项，参考如下方法：

- 使用一个本地化的systemd配置文件
- 安排systemd为MySQL Server进程设置环境变量
- 设置MYSQLD_OPTS systemd变量

创建`/etc/systemd/system/mysqld.service`本地化systemd配置文件，这里讨论的是将此文件名作为`override.conf`：

```sh
[Service]
LimitNOFILE=max_open_files
PIDFile=/path/to/pid/file
Nice=nice_level
LimitCore=core_file_limit
Environment="LD_PRELOAD=/path/to/malloc/library"
Environment="TZ=time_zone_setting"


#LimitNOFILE: 文件描述符数量
#LimitCore: 最大核心文件大小
#Nice: 优先级
#LD_PRELOAD: 特定内存分配库
#TZ: 指定时区
```

修改mysqld:

```
systemctl edit mysqld
```

重新加载systemd配置，然后重启MySQL service：

```
systemctl daemon-reload

systemctl restart mysqld
```

可在`override.conf`中设置如下参数：

```sh
[Service]
PIDFile=/var/run/mysqld/mysqld-custom.pid
ExecStart=
ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld-custom.pid $MYSQLD_OPTS
```

在`/etc/sysconfig/mysql`下指定值：

```sh
LD_PRELOAD=/path/to/malloc/library
TZ=time_zone_setting


systemctl restart mysqld
```

<br>

#### 使用systemd配置多个MySQL实例

Configuring Multiple MySQL Instances Using systemd

<br>

由于systemd具有在平台上管理多个MySQL实例的能力，而不必须需要`mysqld_multi`和`mysqld_multi.server`。

若要使用多实例(multiple-instance)功能，请修改`/etc/my.cnf`文件以包含每个实例的关键选项配置。
例如，管理replication01和replication02两个实例：

```
vim /etc/my.cnf


[mysqld@replica01]
datadir=/var/lib/mysql-replica01
socket=/var/lib/mysql-replica01/mysql.sock
port=3307
log-error=/var/log/mysqld-replica01.log


[mysqld@replica02]
datadir=/var/lib/mysql-replica02
socket=/var/lib/mysql-replica02/mysql.sock
port=3308
log-error=/var/log/mysqld-replica02.log
```

**这里的名称使用`@`作为分隔符(delimiter)，因为这个是`systemd`支持的唯一分隔符。**

管理两个实例:

```
systemctl start mysqld@replica01
systemctl start mysqld@replica02


systemctl enable mysqld@replica01
systemctl enable mysqld@replica02


#使用通配符
systemctl status 'mysqld@replica*'


systemctl stop mysqld@replica0{1,2}
```

对于同一个机器上的不同MySQL实例，systemd自动使用不同的单元文件。
在unit file中，`%I`和`%i`用于`@`标记后传入参数，用于管理特定实例。

```sh
#像这样
mysqld --defaults-group-suffix=@%I ...


systemctl status mysqld@replica01

# mysqld@replica01.service - MySQL Server
#  Loaded: loaded (/usr/lib/systemd/system/mysqld@.service; disabled; vendor preset: disabled)
#  Active: active (running) since Tue 2018-02-27 12:18:34 CST; 1min 6s ago
#    Docs: man:mysqld(8)
#          http://dev.mysql.com/doc/refman/en/using-systemd.html
# Process: 3927 ExecStart=/usr/sbin/mysqld --defaults-group-suffix=@%I --daemonize --pid-file=/var/run/mysqld/mysqld-%i.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
# Process: 3845 ExecStartPre=/usr/bin/mysqld_pre_systemd %I (code=exited, status=0/SUCCESS)
#Main PID: 3930 (mysqld)
#  CGroup: /system.slice/system-mysqld.slice/mysqld@replica01.service
#          `-3930 /usr/sbin/mysqld --defaults-group-suffix=@replica01 --daemonize --pid-file=/var/run/mysqld/mysqld-replica01.pid
#
#eb 27 12:18:27 zabbix.me systemd[1]: Starting MySQL Server...
#eb 27 12:18:34 zabbix.me systemd[1]: Started MySQL Server.
```

<br/>

#### 从mysqld_safe迁移到systemd

Migrating from mysqld_safe to systemd

<br>

因为`mysqld_safe`没有安装在使用`systemd`管理MySQL的平台上，所以以前需要为该程序指定选项：`[mysqld_safe]`

- 一些`[mysqld_safe]`的选项也能被`[mysqld]`支持
- 一些`[mysqld_safe]`的选项类似于`[mysqld]`选项



<br/>
<br/>


## 从源码安装MySQL

Installing MySQL from Source

<br>

从源代码构建MySQL使我们能够自定义构建参数(parameter)、编译器优化(compiler optimization)和安装位置(installation location)。

在使用源码安装前，请检查Oracle是否为你的平台生成预编译的二进制发行版，以及是否适合你。Oracle付出了很多努力确保提供的二进制文件具有最佳的性能选择。

**源码安装系统需求：**
使用源码安装MySQL需要多种开发工具。

使用源码安装MySQL，必须满足一下系统需求：

- CMake, which is used as the build framework on all platforms
- A good make program
- A working ANSI C++ compiler
- The Boost C++ libraries are required to build MySQL
- The ncurses library
- Sufficient free memory
- Perl is needed if you intend to run test scripts

使用standard source distribution安装MySQL，需要以下工具来unpack分发文件：

- For a .tar.gz compressed tar file: `tar`
- For a .zip Zip archive: `zip`
- For an .rpm RPM package: `rpmbuild`


<br/>
<br/>


### 用于源码安装的MySQL布局

MySQL Layout for Source Installation

<br>

默认地，再从源码编译后安装MySQL时，安装步骤会将文件安装在`/usr/local/mysql`下。


<br/>
<br/>


### 使用标准源码发行版安装MySQL

Installing MySQL Using a Standard Source Distribution

<br>

从一个标准源码发行版安装MySQL：

1. 确保系统满足工具需求
2. 获取发行文件
3. 配置、构建和安装
4. 执行安装后程序

<br>

如果是source RPM:

```sh
rpmbuild --rebuild --clean MySQL-VERSION.src.rpm
```

<br>

如果是compressed tar file 或 zip archive source:

```sh
# Preconfiguration setup
shell> groupadd mysql
shell> useradd -r -g mysql -s /bin/false mysql


# Beginning of source-build specific instructions
shell> tar zxvf mysql-VERSION.tar.gz
shell> cd mysql-VERSION
shell> mkdir bld
shell> cd bld
shell> cmake ..
shell> make
shell> make install


# End of source-build specific instructions
# Postinstallation setup
shell> cd /usr/local/mysql
shell> mkdir mysql-files
shell> chown mysql:mysql mysql-files
shell> chmod 750 mysql-files
shell> bin/mysqld --initialize --user=mysql
shell> bin/mysql_ssl_rsa_setup
shell> bin/mysqld_safe --user=mysql &


# Next command is optional
shell> cp support-files/mysql.server /etc/init.d/mysql.server
```

**/sbin/nologin和/bin/false的区别**

- /bin/false是最严格的禁止login选项，一切服务都不能用
	+ `mongod:x:996:994:mongod:/var/lib/mongo:/bin/false`
- /sbin/nologin只是不允许系统login，可以使用其他服务
	+ `ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin`

<br>

**执行预配置(preconfiguration)设置**

在Unix上，设置MySQL用户和组，用于运行和执行MySQL服务器和数据库目录。

<br>

**获得和解包distribution**

选择要解压分发的目录，并将位置更改到其中。

```sh
tar zxvf mysql-VERSION.tar.gz

#gunzip < mysql-VERSION.tar.gz | tar xvf -
#cmake -E tar zxvf mysql-VERSION.tar.gz
```

<br/>
<br/>


### 使用开发源码树安装MySQL

Installing MySQL Using a Development Source Tree

<br>

install MySQL from the latest development source codew hich is hosted on GitHub: <https://github.com/mysql/mysql-server>

**设置一个MySQL git repository**

1. 克隆MySQL git repository到本机

```sh
git clone https://github.com/mysql/mysql-server.git
```

2. 查看

```sh
cd mysql-server
```

3. 使用`git branch -r`查看远程MySQL分支

```sh
cd mysql-server

git branch -r
```

4. 查看分支

```sh
cd mysql-server

git branch
```

5. 切换分支

```sh
cd mysql-server

git checkout 5.7
```

6. 获取远程MySQL git repository更新

```sh
cd mysql-server

git pull
```

7. 检查提交历史

```sh
cd mysql-server

git log

#也可在MySQL GitHub上查看commit history
```

9. 在克隆MySQL git repository并切换到需要的分支后，便可以从源代码构建MySQL Server。

在生产机器上从分发源码树安装构件时要小心，安装命令可能会覆盖您的实时发行版安装。


<br/>
<br/>


### MySQL源码配置选项

MySQL Source-Configuration Options

<br/>

CMake程序提供了一个强大的如何配置MySQL源码发行版的控制。

具体链接参考: <https://dev.mysql.com/doc/refman/5.7/en/source-configuration-options.html>


<br/>
<br/>

### 处理MySQL编译问题

Dealing with Problems Compiling MySQL

<br>

- 如果CMake先前已经运行过，那么现在运行的CMake可能使用先前的调用过程中收集到的信息。这些信息存储在 CMakeCache.txt。在CMake启动时，它会寻找和读取此文件。
- 每次运行`CMake`，必须再次运行`make`才能重新编译。

防止使用old object file或配置文件:

```sh
make clean
rm CMakeCache.txt
```


<br/>
<br/>


## 安装之后的设置和测试

Postinstallation Setup and Testing

<br>

在安装MySQL后你应该做的事：

- 如有必要，初始化数据目录并创建MySQL授权表
- 开启Server并确保它可以正常访问
- 将密码分配给授权表中的root用户
- 可选地，设置Server自启动
- 可选地，填写时区表，以便识别时区


<br>


### 初始化数据目录

Initializing the Data Directory

<br>


安装MySQL之后，必须初始化数据目录，包括mysql系统数据库中的表。有些安装方法会自动初始化，有些则需要手动初始化。
当然，如果修改了默认数据目录位置，那么也是需要手动初始化的。

初始化数据库目录，主要是包含了初始MySQL授权表(grant table)的MySQL服务器，这些表确定了如何允许用户连接到服务器。
但是，初始化数据目录是不会覆盖(overwrite)任何现有权限表，因此在任何情况下运行都是安全的。

数据目录初始化会在MySQL数据库汇总创建time zone，但不会填充它，所以它是空的。

```sh
cd /usr/local/mysql

mkdir mysql-files

chown mysql:mysql ./mysql-files
chmod 750 ./mysql-files


#--user
#使数据库目录文件属于mysql用户，以确保Server有读取权限
/usr/local/mysql/bin/mysqld --initialize --user=mysql


#开启安全连接
/usr/local/mysql/bin/mysql_ssl_rsa_setup
```

<br>

#### 使用mysqld手动初始化数据目录

Initializing the Data Directory Manually Using mysqld

<br>

```sh
cd /usr/local/mysql/bin


#使数据库目录文件属于mysql用户，以确保Server有读取权限
#默认是secure，会生成root初始密码
./mysqld --initialize --user=mysql


#不生成root初始密码
./bin/mysqld --initialize-insecure --user=mysql


#指定目录
--basedir=/usr/local/mysql
--datadir=/var/lib/mysql


#或者将其写入配置文件
vim /etc/my.cnf

[mysqld]
basedir=/usr/local/mysql
datadir=/var/lib/mysql

#指定配置文件初始化
./mysqld --defaults-file=/etc/mysql.cnf --initialize --user=mysql
```

<br>

#### 使用mysql_install_db初始化数据目录

Initializing the Data Directory Manually Using mysql_install_db

<br>

```sh
cd /usr/local/mysql/bin


#mysql_install_db命令会创建数据目录，并在数据目录下创建mysql数据库和授权表
./mysql_install_db --user=mysql


#指定目录是必须的
--basedir=/usr/local/mysql
--datadir=/var/lib/mysql

./mysqld_safe --user=mysql &
#systemctl start mysqld

mysql -u root -p xxx

mysql>SET PASSWORD FOR 'root'@'localhost' = PASSWORD('new_password');
```


<br/>
<br/>


### Starting the Server


- Start the MySQL server like this if your installation includes mysqld_safe
	+ `/usr/local/mysql/binmysqld_safe --user=mysql &`
- Start the server like this if your installation includes systemd support
	+ `systemctl start mysqld`
- 使用non-root用户运行MySQL服务很重要
- 如有错误请查看日志


<br>
<br/>


### Testing the Server

执行一些简单测试以保证Server正常工作。

```sh
#使用mysqladmin验证Server正在运行
mysqladmin --help

mysqladmin -uuser -ppasswd version

mysqladmin -uuser -ppasswd variables

mysqladmin -user -ppasswd shutdown


# 使用mysqlshow查看数据库
mysqlshow -uuser -ppasswd

#查看指定数据库信息
mysqlshow -uuser -ppasswd mysql


#读取信息
#-e,Execute command and quit
mysql -uuser -ppasswd -e "SELECT user, host from mysql.user"
```


<br/>
<br/>


### 保护初始化MySQL账户

Securing the Initial MySQL Accounts

<br>

在安装MySQL后，root账户密码可能已经被分配。

`mysql.user`授权表定义了初始化MySQL用户账户和它们的访问权限。
MySQL5.7只创建了一个`'root'@'localhost'`账户，但早期的版本可能有多个用户。

请务必为每一个MySQL账户创建密码。

查看用户：

```sh
#存储在authentication_string列中的密码可能包含无法正常显示的二进制数据
#所以将其转换为十六进制
mysql> SELECT user, host, hex(authentication_string) FROM mysql.user;
mysql> SELECT user, host, authentication_string FROM mysql.user;

#或
mysql -uuser -ppasswd -e "SELECT user, host, hex(authentication_string) FROM mysql.user;"


#5.7以前的版本
mysql> mysql> SELECT user, host, password FROM mysql.user;

#或
mysql -uuser -ppasswd -e "SELECT user, host, password FROM mysql.user;"
```

<br>

**为root账户分配密码**

```
#5.7.6
mysql> ALTER USER user IDENTIFIED BY 'new_passwd';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_passwd';


#5.7.6前
mysql> SET PASSWORD FOR username = PASSWORD('new_passwd');

mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('new_passwd');
```

<br>

**给anonymous账户分配密码**

```
mysql> SET PASSWORD FOR ''@'localhost' = PASSWORD('new_passwd');
```

<br>

**移除匿名账户**

```
mysql> DROP USER ''@'localhost';
```


<br/>
<br/>


## 升级或降级MySQL

Upgrading or Downgrading MySQL

<br>

- 升级是一个常见的过程。请在测试系统上确保运行正常后再实施到生产环境
- 降级不太常见。一般是由于新版本在生产环境上发生某些兼容性或性能问题，并且是在测试环境中没有发现的情况下，从而需要降级。请现在测试系统上运行正常后再实施到生产环境。


<br/>
<br/>


### 升级MySQL

请使用有管理权限的MySQL账户执行升级相关命令。(如root账户)

<br>

#### MySQL升级策略

MySQL Upgrade Strategies

<br>

**升级方法**

- 直接升级(In-Place Upgrade)
	+ 包含关闭旧版MySQL，替换为新的MySQL版本，在现有数据目录上重启MySQL，运行`mysql_upgrade`
- 逻辑升级(Logical Upgrade)
	+ 包含使用`mysqldump`导出现有数据文件，安装新版MySQL，导入数据文件到新版MySQL，运行`mysql_upgrade`

<br>

**升级路径**

- 只支持GA release之间
- 这是一个发行系列的升级
	+ 如5.6.x到5.6.y
- 升级到下一个版本之前，建议先升级到最新版本
	+ 如先升级到5.6最新版，再升级到5.7
- 不支持跳版本升级
	+ 如5.5到5.7

<br>

**升级之前**

- 升级之前，请一定备份数据
- 查看新版本的Release Note
	+ 删除和增加了什么功能
- 新版本依赖什么
- 如果在InnoDB中使用XA事务，则在升级之前运行XA恢复以检查未提交的XA事务
- 如果MySQL数据量很大，就地升级以后可能需要很长的时间才能进行转换
	+ 你可能会发现创建一个"dummy"数据库实例是很有用的，以及评估可能需要哪些转换以及执行这些转换所涉及的工作
- 无论在你安装或升级到一个MySQL新版本，建议重建和重装MySQL language interface
	+ 如PHP MySQL扩展

<br/>

**直接升级**

- 配置MySQL执行slow shutdown

innoDB在关闭前执行一个完整的清除和更改缓冲区合并，这确保数据文件在不同的版本的文件格式做好充分准备。

```sh
mysql -u root -p --execute="SET GLOBAL innodb_fast_shutdown=0"
```

- 关闭MySQL Server

```sh
mysql -uroot -p shutdown
```

- 升级MySQL

- 开启新版MySQL

- 运行mysql_upgrade

`mysql_upgrade`检查所有数据库中的所有表与当前版本MySQL的不兼容性。

```sh
mysql_upgrade -uroot -p

#Upgrade process completed successfully.
#Checking if update is needed.
```

- 关闭和重启MySQL Server来确保改变生效

```sh
mysqladmin -uroot -p shutdown

systemctl start mysqld
```

<br>

**逻辑升级**

- 导出所有数据

```sh
mysqldump -uroot -p  --all-databases --force > mysqldb_backup.sql


#-f, --force         Continue even if we get an SQL error
#Use the --routines and --events options if your databases include stored programs
#--add-drop-database Add a DROP DATABASE before each create.
mysqldump -uroot -p --add-drop-table --routines --events --all-databases --force > mysqldb_backup.sql
```

- 关闭MySQL Server

```sh
mysqladmin -uroot -p shutdown
```

- 安装新版MySQL

-  初始化MySQL并启动

- 载入数据文件

```sh
mysql -uroot -p --force < ./mysqldb_backup.sql
```

- 运行mysql_upgrade

```sh
mysql_upgrade -uroot -p

#Upgrade process completed successfully.
#Checking if update is needed.
```

- 关闭并重启MySQL Server以确保更改生效


<br>

#### 通过MySQL Yum Repository进行升级

Upgrading MySQL with the MySQL Yum Repository

<br>

**选择一个target series**

默认情况下，MySQL Yum Repository会将MySQL升级到该release系列的最新版本。如5.7.1升级到5.7.10。

如果要升级到其他release(如5.6到5.7)，就必须要先禁用此subrepository，并选择和启用新的subrepository。

As a general rule, to upgrade from one release series to another, go to the next series rather than skipping a series.

<br>

**升级MySQL**

```sh
yum update mysql-server mysql-client
```

<br>

**重启MySQL**

MySQL Server总是在Yum更新之后重启，一旦重启，请运行`mysql_upgrade`来检查旧数据与升级软件之间的任何不兼容问题。

```sh
mysql_upgrade -uroot -p

#Upgrade process completed successfully.
#Checking if update is needed.
```

<br>

**升级Shared Client Libraries**


所以说，用yum repository安装软件是很方便的。不管是在管理还是升级等方面...


<br/>

#### 通过直接下载RPM包升级MySQL

直接下载mysql相应组件的rpm进行升级。
建议备份好配置文件。

```sh
wget http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql-community-server-5.7.20-1.el7.x86_64.rpm
wget http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql-community-client-5.7.20-1.el7.x86_64.rpm


yum install mysql-community-server-5.7.20-1.el7.x86_64.rpm mysql-community-client-5.7.20-1.el7.x86_64.rpm
```


<br>
<br/>


### mysql降级

MySQL降级类似于MySQL升级。也包含有直接降级和逻辑降级。



<br/>
<br/>


### 重建或修复表或索引

Rebuilding or Repairing Tables or Indexes

<br>

- MySQL处理数据类型和字符集的方式的更改
- 表维修或升级(mysqlcheck, mysql_upgrade)

重建表的方法：

- Dump and Reload
- ALTER TABLE
- REPAIR TABLE

<br>

**Dump and Reload Method**

由于MySQL升级/降级之后，不同版本的MySQL无法处理这些表，则需要转储和重载的方法来重建表。

```sh
mysqldump -uroot -p --all-databases --force > mysql_backdb.sql
mysql -uroot -p --force < mysql_backdb.sql


#某个库或表
mysqldump -uroot -p --databases test --force > db_test.sql
mysql -uroot -p test < db_test.sql

mysqldump -uroot -p --databases test --tables table222 > table222.sql
mysql -uroot -p test  < table222.sql
```

<br>

**ALTER TABLE Method**

更改表以使用它已经拥有的存储引擎。

```
ALTER TABLE test ENGINE = InnoDB;
```

<br>

**REPAIR TABLE Method**

`REPAIR TABLE`仅适用于MyISAM， ARCHIVE和 csv 表。

`mysqlcheck --repair`提供了对`REPAIR TABLE`的命令行访问。

```
REPAIR TABLE t1;


mysqlcheck --repair --databases db_name ...
mysqlcheck --repair --all-databases
```


<br>
<br/>


### 复制MySQL数据库到其他机器

Copying MySQL Databases to Another Machine

<br>

在需要为不同体系架构之间传输MySQL数据库时，可使用`mysqldump`创建包含SQL语句的`.sql`文件，然后复制到另外的计算机上，将其作为输入提供给MySQL客户端。

不要忘记复制mysql数据库，因为这个存储授权表的地方。

```
mysqldump --host 'remote-host' -uxxx -p --compress --all-databases | mysql -uxxx -p

mysqldump --host 'remote-host' -uxxx -p --compress db_name | mysql -uxxx -p db_name


mysqladmin -uxxx -p flush-privileges
```




<br/>
<br/>

---

<br/>



# Tutorial


如何使用MySQL client程序来创建和使用数据库。


<br>


## 连接和断开服务器

Connecting to and Disconnecting from the Server

<br>

Like this:

- 不建议把密码直接写在命令行上
- host表示了MySQL Server运行在的机器
- 某些MySQL允许匿名用户连接
- `-ppassword, not as -p password`

```sh
mysql --host host --user username -p


#maybe not default port
mysql --host host --user username -p  --port port


#匿名用户连接
mysql


#退出
mysql> QUIT
#Unix
mysql> Ctrl+D
```


<br/>
<br/>


## 输入查询

Entering Queries

<br>

```sql
#简单查询
mysql> SELECT VERSION(), CURRENT_DATE;


#简单计算
SELECT SIN(PI()/2), (4+1)*5;


#一行中输入多个语句
SELECT VERSION(); SELECT NOW();


#多行输入一个命令
mysql> SELECT
    -> USER()
    -> ,
    -> CURRENT_DATE;
```

![MySQL简单查询](/images/MySQL/20180301101009.jpg)

<br>

这QUERY说明了有关MySQL的几件事：

- MySQL查询通常由一个`SQL statement`和`;`组成
- MySQL将查询发送给服务器并返回结果，然后打印下一个`mysql>`提示
- MySQL以表格形式(rows and columns)显示查询输出
- MySQL显示返回多少行，以及执行查询花费了多长时间
- MySQL查询不区分大小写，但建议使用大写
- MySQL支持在一行中输入多个语句
- MySQL支持一个命令多行输入

<br>

**MySQL提示符：**

Prompt | Meaning
- | -
`mysql>` | 准备新查询
`->` | 等待多行查询的下一行
`'>` | 等待下一行，等待单引号开头的字符串的完成
`">` | 等待下一行，等待双引号字开头的字符串的完成
`\`>` | 等待下一行，等待以反引号开始的标识符的完成
`/*>` | 等待下一行，等待以`/*`开头的注释的完成-->`/*comments*/`



<br>
<br/>


## 创建和使用数据库

Creating and Using a Database

<br>

大致操作：

- Create a database
- Create a table
- Load data into the table
- Retrieve data from the table in various ways
- Use multiple tables

<br>

```sql
#显示数据库
#不能显示你没有权限的数据库
mysql> SHOW DATABASES;

#mysql数据库描述用户访问权限
#test数据库通常作为用户尝试使用工作区


#访问数据库
mysql> USE test;

#USE和QUIT一样可以不使用分号，使用也无妨
#USE只能是一个单行


#授权
#GRANT ALL ON da_name.table TO 'username'@'host';
mysql> GRANT ALL ON test.* TO 'test'@'127.0.0.1';
```


<br/>
<br>


### 创建和选择数据库

Creating and Selecting a Database

<br>

**Unix是区分大小写的(case-sensitive)，这与SQL keyword不一致。请注意。**

```sql
mysql> CREATE DATABASE db01;

mysql> USE db01;

#也可在mysql连接时直接指定数据库
mysql -u username -p db01


#查看当前选择的数据库
mysql> SELECT DATABASE();
```


<br/>
<br/>


### 创建表

Creating a Table

<br>

**困难的部分是决定数据库的结构应该是什么： 你需要哪些表以及每个表中应该包含哪些列。**


`VARCHAR`对于name，owner，species来说是一个不错的选择，因为column值的长度有所不同。
`DATE`对于出生和死亡column来说很不错。
如果以后你发现你需要更长的字段，MySQL提供了一个`ALTER TABLE`语句来修改。

```sql
#创建一个宠物表
mysql> CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),
    -> species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);


mysql> SHOW TABLES;


#验证表格
#如果你忘记了表中列的名称或类型，使用DESCRIBE
mysql> DECRIBE pet;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```


<br/>
<br/>


### 将数据载入表格

Loading Data into a Table

<br>

假设pet表信息如下：

name | owner | species | sex | birth | death
- | - | - | - | -
PetA | Aa | cat | f | 1993-02-04 |
PetB | Bb | cat | m | 1994-03-17 |
PetC | Cc | dog | f | 1989-05-13 |
PetD | Aa | dog | m | 1979-08-25 | 1995-02-21
PetE | Cc | bird | | 1991-02-17 |

<br>

你可以创建一个`pet.txt`文本文件，每行包含一个记录，值由制表符分割，并按照`CREATE TABLE`语句中列出的顺序给出。

```sh
vim pet.txt


PetA    Aa     cat    f      1993-02-04    \N
PetB    Bb     cat    m      1994-03-17    \N
PetC    Cc     dog    f      1989-05-13    \N
PetD    Aa     dog    m      1979-08-25    1995-02-21
PetE    Cc     bird  \N      1991-02-17    \N
```

将`pet.txt`载入`pet`表中：

```sql
mysql> LOAD DATA LOCAL INFILE '/path/file.txt' INTO TABLE table_name;


mysql> LOAD DATA LOCAL INFILE '/home/zhang/pet.txt' INTO TABLE pet;
Query OK, 5 rows affected, 0 warnings (0.00 sec)
Records: 5  Deleted: 0  Skipped: 0  Warnings: 0


mysql> SELECT * FROM pet;
+-------+-------+---------+------+------------+------------+
| name  | owner | species | sex  | birth      | death      |
+-------+-------+---------+------+------------+------------+
| PetA  |  Aa   |  cat    | f    | 1993-02-04 | NULL       |
| PetB  |  Bb   |  cat    | m    | 1994-03-17 | NULL       |
| PetC  |  Cc   |  dog    | f    | 1989-05-13 | NULL       |
| PetD  |  Aa   |  dog    | m    | 1979-08-25 | 1995-02-21 |
| PetE  |  Cc   |  bird   | NULL | 1991-02-17 | NULL       |
+-------+-------+---------+------+------------+------------+
5 rows in set (0.00 sec)



#通过命令行载入
mysql> INSERT INTO pet
    -> VALUES ('PetF', 'Ff', 'hamster', 'f', '1999-03-21', NULL)
    -> ;
Query OK, 1 row affected (0.00 sec)
```


<br/>
<br/>


### 从表中检索信息

Retrieving Information from a Table

<br>

`SELECT`语句用于从表中提取信息：

```sql
SELECT what_to_select
FROM which_table
WHERE condition;
```


<br>

#### 查询所有数据

Selecting All Data

<br>

```sql
mysql> SELECT * FROM pet;


mysql> DELETE FROM pet;


mysql> UPDATE pet SET birth = '1989-06-17' WHERE name = 'PetC';
```


<br>

#### 查询特定行

Selecting Particular Rows

<br>

当一个表很大时，你通常不想看到整个表。

```sql
#条件查询

mysql> SELECT * FROM pet WHERE name = 'PetA';


mysql> SELECT * FROM pet WHERE owner = 'Cc';


mysql> SELECT * FROM pet WHERE birth >= '1990-01-01';


#AND
mysql> SELECT * FROM pet WHERE species = 'dog' AND sex = 'f';


#OR
mysql> SELECT * FROM pet WHERE species = 'dog' OR species = 'bird';


#AND和OR也可以混合使用
mysql> SELECT * FROM pet WHERE (species = 'cat' AND sex = 'm') OR (species = 'dog' AND sex='f');
```


<br>

#### 查询特定列

Selecting Particular Columns

<br>

```sql
mysql> SELECT name FROM pet;


mysql> SELECT name, species FROM pet;


#获取唯一结果
mysql> SELECT DISTINCT species FROM pet;
+---------+
| species |
+---------+
| cat     |
| dog     |
| bird    |
+---------+
3 rows in set (0.00 sec)


mysql> SELECT name, species, birth FROM pet WHERE species = 'dog' OR species = 'cat';
```


<br>

#### 行排序

Sorting Rows

<br>

使用`ORDER BY`语句对结果进行排序。默认排序顺序是升序。

```sql
mysql> SELECT name, birth FROM pet ORDER BY birth;
+------+------------+
| name | birth      |
+------+------------+
| PetD | 1979-08-25 |
| PetC | 1989-06-17 |
| PetE | 1991-02-17 |
| PetA | 1993-02-04 |
| PetB | 1994-03-17 |
+------+------------+
5 rows in set (0.00 sec)


#倒序
mysql> SELECT name, birth FROM pet ORDER BY birth DESC;
```

可对多列进行排序，也可按不同的方向对不同的列进行排序。

```sql
mysql> SELECT name, species, birth FROM pet
    -> ORDER BY species, birth DESC;
+------+---------+------------+
| name | species | birth      |
+------+---------+------------+
| PetE | bird    | 1991-02-17 |
| PetB | cat     | 1994-03-17 |
| PetA | cat     | 1993-02-04 |
| PetC | dog     | 1989-06-17 |
| PetD | dog     | 1979-08-25 |
+------+---------+------------+
5 rows in set (0.00 sec)


mysql> SELECT name, species, birth FROM pet
    -> ORDER BY species DESC, birth DESC
```


<br>

#### 日期计算

Date Calculations

<br>

MySQL提供了几个函数用于日期计算。如计算年龄或提取日期一部分等。

<br>

- `TIMESTAMPDIFF()`
	+ 使用`TIMESTAMPDIFF()`函数计算pet的年龄。它的两个参数为两个相隔的日期

```sql
mysql> SELECT name, species, birth, CURDATE(),
    -> TIMESTAMPDIFF(YEAR, birth, CURDATE()) AS age
    -> FROM pet
    -> ORDER BY age DESC;
+------+---------+------------+------------+------+
| name | species | birth      | CURDATE()  | age  |
+------+---------+------------+------------+------+
| PetD | dog     | 1979-08-25 | 2018-03-01 |   38 |
| PetC | dog     | 1989-06-17 | 2018-03-01 |   28 |
| PetE | bird    | 1991-02-17 | 2018-03-01 |   27 |
| PetA | cat     | 1993-02-04 | 2018-03-01 |   25 |
| PetB | cat     | 1994-03-17 | 2018-03-01 |   23 |
+------+---------+------------+------------+------+
5 rows in set (0.00 sec)


#死去的pet的age
mysql> SELECT name, species, birth, death,
    -> TIMESTAMPDIFF(YEAR, birth, death) AS age
    -> FROM pet
    -> WHERE death IS NOT NULL
    -> ORDER BY age;
+------+---------+------------+------------+------+
| name | species | birth      | death      | age  |
+------+---------+------------+------------+------+
| PetD | dog     | 1979-08-25 | 1995-02-21 |   15 |
+------+---------+------------+------------+------+
1 row in set (0.00 sec)
```

<br>

- `YEAR()`
	+ 年
- `MONTH()`
	+ 月
- `DAYOFMONTH()`
	+ 日


```sql
mysql> SELECT name, birth,
    -> YEAR(birth) AS bir_year,
    -> MONTH(birth) AS bir_month,
    -> DAYOFMONTH(birth) AS bir_day
    -> FROM pet;
+------+------------+----------+-----------+---------+
| name | birth      | bir_year | bir_month | bir_day |
+------+------------+----------+-----------+---------+
| PetA | 1993-02-04 |     1993 |         2 |       4 |
| PetB | 1994-03-17 |     1994 |         3 |      17 |
| PetC | 1989-06-17 |     1989 |         6 |      17 |
| PetD | 1979-08-25 |     1979 |         8 |      25 |
| PetE | 1991-02-17 |     1991 |         2 |      17 |
+------+------------+----------+-----------+---------+
5 rows in set (0.00 sec)



#查找生日是2月的pet
mysql> SELECT name, birth FROM pet WHERE MONTH(birth) =2;
+------+------------+
| name | birth      |
+------+------------+
| PetA | 1993-02-04 |
| PetE | 1991-02-17 |
+------+------------+
```

<br>

- `DATE_ADD()`
	+ 将日期间隔添加到给定日期

```sql
mysql> SELECT name, birth FROM pet
    -> WHERE MONTH(birth) = MONTH(DATE_ADD(CURDATE(), INTERVAL 1 MONTH));
```

<br>

#### 使用NULL值

Working with NULL Values

<br>

从概念上讲，NULL value意味着**一个缺失的未知值**，它与其它值在某种程度上是不同的。

- 使用`IS NULL`和`IS NOT NULL`操作符
- 不能对NULL value使用算术运算符(arithmetic cpmparison operators)
	+ 如：`=`, `<`, `>`, `<>`
	+ 任何对NULL value的算术运算符的结果也是NULL value，所以无法得到有意义的结果
- 在MySQL中，0或NULL表示false，其他任何值都意味着true
- 两个NULL在`GROUP BY`中被认为是相等的
- NULL在`ORDER BY`正向排序中首先显示。反之，最后显示


```sql
mysql> SELECT 1 IS NULL, 1 IS NOT NULL;
+-----------+---------------+
| 1 IS NULL | 1 IS NOT NULL |
+-----------+---------------+
|         0 |             1 |
+-----------+---------------+
```

因此，完全可以将一个**zero**或**empty string**插入到一个**NOT NULL**的column中，因为这些值NOT NULL。


<br>

#### 模式匹配

Pattern Matching

<br>

MySQL提供标准的SQL模式匹配以及基于扩展正则表达式的模式匹配形式。类似于Unix实用程序(vi, grep, sed...)

SQL模式匹配允许:

- 使用`_`来匹配可以使用的任意单字符(single character)
- 使用`%`来匹配可以使用的任意数目的字符(arbitrary number of characters)
- SQL模式不区分大小写
- 使用`LIKE`或`NOT LIKE`而不是`=`或`<>`

```sql
mysql> SELECT * FROM pet WHERE name LIKE '%b';
+------+-------+---------+------+------------+-------+
| name | owner | species | sex  | birth      | death |
+------+-------+---------+------+------------+-------+
| PetB | Bb    | cat     | m    | 1994-03-17 | NULL  |
+------+-------+---------+------+------------+-------+



mysql> SELECT * FROM pet WHERE name LIkE '___A' or name LIKE '___C';
+------+-------+---------+------+------------+-------+
| name | owner | species | sex  | birth      | death |
+------+-------+---------+------+------------+-------+
| PetA | Aa    | cat     | f    | 1993-02-04 | NULL  |
| PetC | Cc    | dog     | f    | 1989-06-17 | NULL  |
+------+-------+---------+------+------------+-------+
```

<br>

MySQL提供的其它类型的模式匹配使用扩展的正则表达式：

- `REGEXP` 或 `RLIKE`
- `NOT REGEXP` 或 `NOT RLIKE`
- 了解正则表达式知识

```sql
mysql> SELECT * FROM pet WHERE name RLIKE '^pet[AB]';
+------+-------+---------+------+------------+-------+
| name | owner | species | sex  | birth      | death |
+------+-------+---------+------+------------+-------+
| PetA | Aa    | cat     | f    | 1993-02-04 | NULL  |
| PetB | Bb    | cat     | m    | 1994-03-17 | NULL  |
+------+-------+---------+------+------------+-------+



mysql> SELECT * FROM pet WHERE owner RLIKE 'c$';
+------+-------+---------+------+------------+-------+
| name | owner | species | sex  | birth      | death |
+------+-------+---------+------+------------+-------+
| PetC | Cc    | dog     | f    | 1989-06-17 | NULL  |
| PetE | Cc    | bird    | NULL | 1991-02-17 | NULL  |
+------+-------+---------+------+------------+-------+



#包含某个字符
mysql> SELECT * FROM pet WHERE name RLIKE 'ete';
+------+-------+---------+------+------------+-------+
| name | owner | species | sex  | birth      | death |
+------+-------+---------+------+------------+-------+
| PetE | Cc    | bird    | NULL | 1991-02-17 | NULL  |
+------+-------+---------+------+------------+-------+



#匹配字符个数
mysql> SELECT * FROM pet WHERE name RLIKE '^....$';

mysql> SELECT * FROM pet WHERE name RLIKE '^.{4}$';



#强制区分大小写
mysql> SELECT * FROM pet WHERE name RLIKE BINARY '^Pet[AB]';
```

<br/>

#### 行数计算

Counting Rows

<br>

- 使用`COUNT()`计算行数

```sql
#总行数
mysql> SELECT COUNT(*) AS count FROM pet;
+-------+
| count |
+-------+
|     5 |
+-------+




#针对某个统计行数
mysql> SELECT owner, COUNT(*) FROM pet GROUP BY owner;
+-------+----------+
| owner | COUNT(*) |
+-------+----------+
| Aa    |        2 |
| Bb    |        1 |
| Cc    |        2 |
+-------+----------+



#多个条件
mysql> SELECT species, sex, COUNT(*) FROM pet GROUP BY species, sex;
+---------+------+----------+
| species | sex  | COUNT(*) |
+---------+------+----------+
| bird    | NULL |        1 |
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        1 |
+---------+------+----------+
```


<br/>

#### 使用多个表

Using More Than one Table

<br/>

创建一个额外的宠物信息表：

name | date | type | remark
- | - | - | -
Fluffy | 1995-05-15 | litter | 4 kittens, 3 female, 1 male
Buffy | 1993-06-23 | litter | 5 puppies, 2 female, 3 male
Buffy | 1994-06-19 | litter | 3 puppies, 3 female
Chirpy | 1999-03-21 | vet | needed beak straightened
Slim | 1997-08-03 | vet | broken rib
Bowser | 1991-10-12 | kennel |
Fang | 1991-10-12 | kennel |
Fang | 1998-08-28 | birthday | Gave him a new chew toy
Claws | 1998-03-17 | birthday | Gave him a new flea collar
Whistler | 1998-12-09 | birthday | First birthday



```sql
mysql> CREATE TABLE event ( name VARCHAR(20), date DATE,
    -> type VARCHAR(15), remark VARCHAR(255) );


mysql> LOAD DATA INFILE '/path/event.txt' INTO TABLE event;
```


<br/>
<br/>


## 获取数据库和表的信息

Getting Information About Databases and Tables

<br>

- 查看当前数据库
	+ `mysql> SELECT DATABASE();`
- 查看当前数据库下的表
	+ `mysql> SHOW TABLES;`
- 查看表的结构
	+ `mysql> DESCRIBE pet;`
- 创建数据库
	+ `mysql> CREATE DATABASE db_01;`
- 创建表
	+ `mysql> CREATE TABLE table_01 {c1 VARCHAR(10), c2 INT, ...};`
- 查看索引(如果存在)
	+ `SHOW INDEX FROM table_01;`



<br/>
<br/>


## 在批处理下使用mysql

Using mysql in Batch Mode

<br>

在前面，我们都是使用MySQL交互式(interactively)输入命令并查看结果。但还可在批处理模式下运行MySQL。
我们可以创建一个脚本文件，然后以这种方式执行脚本文件。

```sh
mysql < batch-file

msyql -h host -u user -p < /path/batch-file


#出现错误也继续运行
msyql -h host -u user -p --force < /path/batch-file
```

<br>

为什么要使用脚本：

- 如果需要反复(repeat)执行查询，将其写入脚本以避免每次执行时重新输入查询
- 通过复制和修改脚本文件从现有查询中生成新的查询
- 批处理模型在开发查询时也很有用，特别是对于多行语句。写错了直接修改脚本就好，而不必重新输入
- 如果查询产生大量输出，可通过传呼机而不是翻滚到屏幕的最上方
	+ `mysql < batch-file | more`
- 可以把输出捕获到一个文件中
	+ `mysql < batch-file > mysql.out`
- 可将脚本文件分发给其他人
- 批处理模式下的MySQL输出更简洁
	+ 可使用`mysql -t`获得交互式数据格式
	+ 使用`mysql -v`将执行语句回显
- 在mysql命令行中载入脚本
	+ `mysql> source filename;`
	+ 或'mysql> \. filename;


<br>
<br/>


## 常见查询

Examples of Common Queries

<br/>

- 在命令行使用mysql并选择数据库

```sh
mysql db_name -u user -p
```

- 创建和填充表

```sql
CREATE TABLE shop (
    article INT(4) UNSIGNED ZEROFILL DEFAULT '0000' NOT NULL,
    dealer  CHAR(20)                 DEFAULT ''     NOT NULL,
    price   DOUBLE(16,2)             DEFAULT '0.00' NOT NULL,
    PRIMARY KEY(article, dealer));
INSERT INTO shop VALUES
    (1,'A',3.45),(1,'B',3.99),(2,'A',10.99),(3,'B',1.45),
    (3,'C',1.69),(3,'D',1.25),(4,'D',19.95);
```

- 查看表内容

```sql
SELECT * FROM shop;
```

- 列的最大值(maximum)

```sql
SELECT MAX(article) AS article FROM shop;



SELECT article, MAX(price) AS price
FROM   shop
GROUP BY article;
```

- 使用用户定义的变量(user-defined variables)

```sql
mysql> SELECT @min_price:=MIN(price),@max_price:=MAX(price) FROM shop;

mysql> SELECT * FROM shop WHERE price=@min_price OR price=@max_price;
```

- 使用外键(Foreign Keys)

在MySQL中，InnoDB表支持检查外键约束。
外键约束不仅仅需要连接两个表。

```sql
CREATE TABLE person (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    name CHAR(60) NOT NULL,
    PRIMARY KEY (id)
);


CREATE TABLE shirt (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    style ENUM('t-shirt', 'polo', 'dress') NOT NULL,
    color ENUM('red', 'blue', 'orange', 'white', 'black') NOT NULL,
    owner SMALLINT UNSIGNED NOT NULL REFERENCES person(id),
    PRIMARY KEY (id)
);


INSERT INTO person VALUES (NULL, 'Antonio Paz');


SELECT @last := LAST_INSERT_ID();


INSERT INTO shirt VALUES
(NULL, 'polo', 'blue', @last),
(NULL, 'dress', 'white', @last),
(NULL, 't-shirt', 'blue', @last);


INSERT INTO person VALUES (NULL, 'Lilliana Angelovska');


SELECT @last := LAST_INSERT_ID();


INSERT INTO shirt VALUES
(NULL, 'dress', 'orange', @last),
(NULL, 'polo', 'red', @last),
(NULL, 'dress', 'blue', @last),
(NULL, 't-shirt', 'white', @last);
```

- 在两个键上查找(Searching on Two Keys)

```sql
SELECT field1_index, field2_index FROM test_table
WHERE field1_index = '1' OR  field2_index = '1'
```

- 使用自动增量

`AUTO_INCREMENT`属性能够为新行生成一个唯一的标识符。

```sql
CREATE TABLE animals (
     id MEDIUMINT NOT NULL AUTO_INCREMENT,
     name CHAR(30) NOT NULL,
     PRIMARY KEY (id)
);


INSERT INTO animals (name) VALUES
    ('dog'),('cat'),('penguin'),
    ('lax'),('whale'),('ostrich');



#设置指定增量开始值
mysql> ALTER TABLE tbl AUTO_INCREMENT = 100;
```



<br>
<br/>

---

<br/>


# MySQL程序


<br>


## MySQL程序概述

Overview of MySQL Programs

<br>

**MySQL安装中有多个不同的程序：**

- **mysqld**
SQL daemon, MySQL Server, **mysqld**是执行大部分工作的主要程序

- **mysqld_safe**
服务器启动脚本
`mysqld_safe`尝试去启动`mysqld`

- **mysql.server**
服务器启动脚本
此脚本用于`System V`系统，包含启动特定运行级别的系统服务脚本
它调用`mysqld_safe`来启动MySQL Server

- **mysql_multi**
可启动和关闭安装在系统上的多个服务器的启动脚本

- **comp_err**
在MySQL build/installation过程中使用
从错误源文件中编译错误消息文件

- **mysql_install_db**
初始化MySQL(数据目录，授权表，并设置InnoDB系统表空间)
通常用于首次安装MySQL时

- **mysql_plugin**
配置MySQL Server插件

- **mysql_secure_installation**
能够提高MySQL安装的安全性

- **mysql_ssl_rsa_setup**
如果这些文件丢失，该程序会创建支持安全连接所需的SSL证书和密钥文件以及RSA密钥对文件

- **mysql_tzinfo_to_sql**
从mysql数据库中加载时区表

- **mysql_upgrade**
在MySQL升级操作后使用
它检查表的不兼容性并在必要时修复它们，并用更新版的MySQL的任何更改来更新授权表

- **mysql**
交互式输入SQL语句的命令行工具
或执行一个批处理模式的文件

- **mysqladmin**
执行管理操作的客户端
如创建或删除数据库，重新加载授权表，刷新表的磁盘...
也可用获取服务器版本、状态、进程信息

- **mysqlcheck**
表格客户端
用于检查、修复、分析和优化表格

- **mysqldump**
将MySQL数据库转储为SQL、文本或XML文件的客户端

- **mysqlimport**
使用`LOAD DATA INFILE`将文本文件导入各自表格的客户端

- **mysqlpump**
将MySQL数据库转转储为SQL文件的客户端

- **mysqlsh**
用于MySQL Server的高级命令行客户端和代码编辑器
除了SQL外，MySQL Shell还为JS和Python提供了脚本功能

- **mysqlshow**
显示有关数据库、表、列和索引的信息的客户端

- **mysqlslap**
用于模拟MySQL Server的客户端负载并报告每个阶段的时间

<br>

**MySQL管理和实用程序：**

- **innochecksum**
InnoDB脱机文件校验和程序

- **myisam_ftdump**
在MyISAM表中显示有关全文索信息

- **myisamchk**
描述，检查，优化和修复MyISAM表

- **myisamlog**
处理MyISAM日志文件

- **myisampack**
压缩MyISAM表以生成更小的只读表

- **mysql_config_editor**
能够将认证凭证存储在名为安全的加密登录路径文件中

- **mysqlbinlog**
从二进制日志中读取语句

- **mysqldumpslow**
读取和总结慢查询日志内容

<br>

**MySQL程序开发实用程序：**

- **mysql_config**
一个shell脚本，用于在编译MySQL程序是生产所需的选项值

- **my_print_defaults：**
显示选项文件的选项组中存在哪些选项

- **resolve_stack_dump**
将数值堆栈跟踪转储解析为符号

<br>

**杂项(Miscellaneous)工具：**

- **lz4_decompress**
解压缩使用LZ4压缩格式的mysqldump输出

- **perror**
显示系统或MySQL错误代码含义

- **replace**
再输入文本中执行字符串替换

- **resolveip**
将主机名解析为IP地址，反之亦然

- **zlib_decompress**
解压缩使用ZLIB压缩格式的mysqldump输出

<br>

Oracle公司还提供了MySQL Workbench GUI工具，用于管理、创建、知悉和评估查询，以及从其它关系数据库管理系统迁移到MySQL系统。

MySQL Client和Server间的通信使用如下环境变量：

| Environment Variable | Meaning |
| - | - |
| MYSQL_UNIX_PORT | The default Unix socket file; used for connections to localhost |
| MYSQL_TCP_PORT | The default port number; used for TCP/IP connections |
| MYSQL_PWD | The default password, insecure |
| MYSQL_DEBUG | Debug trace options when debugging |
| TMPDIR | The directory where temporary tables and files are created |



<br/>
<br/>



## 使用MySQL程序


### 调用MySQL程序

从命令行调用一个MySQL程序，输入程序名称和选项及参数。

```sh
$ mysql --user=root test
$ mysqladmin extended-status variables
$ mysqlshow --help
$ mysqldump -u root personnel

```


<br/>
<br/>


### 连接到MySQL Server


介绍如何连接到MySQL Server。

MySQL程序环境变量的优先级最低，命令行选项最高。你可在配置文件中指定程序的默认值，同时你又可以使用命令行选项覆盖它。
MySQL选项按顺序处理，所以如果多次指定选型，则最后一个选项优先。

```sh
mysql --hostname xx --port xx --user xx --password ${dbname} --protocol=TCP

mysql -h -P -u -p ${dbname}

```

`--protocol`值：

- TCP(all)
- SOCKET(Unix)
- PIPE(windows)
- MEMORY(windows)

<br>

你可以在选项文件的`[client]`部分指定连接参数:

```
[client]
host=xxx
port=xxx
user=xxx
password=xxx
```

<br>

```
mysqladmin -u user -p --count=1k --sleep=10 ping

mysql -u user -pxxx --execute="DESCRIBE db.table"


#执行多个语句
mysql -u root -p -e 'SELECT VERSION(); SELECT NOW()'


```


<br/>
<br/>


### 配置文件


大多数MySQL程序都可从选项文件中读取启动选项。

MySQL不保证配置文件的读取顺序。

Unix和Unix-Like平台的MySQL配置文件：

| 文件 | 描述
| - | -
| `/etc/my.cnf` | 全局选项
| `/etc/mysql/my.cnf` | 全局选项
| `$SYSCONFDIR/my.cnf` | 全局选项
| `$MYSQL_HOME/my.cnf` | MySQL Server Only
| `~/.my.cnf` | 特定用户选项
| `~/.mylogin.cnf` | 特定用户登录选项，Client Only
| `default-extra-file` | 使用`--defaults-extra-file`指定的文件

<br>

配置文件解释：

- 空行被忽略
- `#`号表示注释
- 前后空格将自动从选项名称和值中删除
- `[group]`
为其设置配置项的程序名或组名。在此之后，任何选项设置都会应用到指定组，知道给出结尾。选项组名称不区分大小写。

- 你可在选项值中使用转义序列
`\b, \t, \n, \r, \\, \s`

- `!include`来包含其它配置文件




```
DATADIR
mysqld --datadir


[mysqld]
port=3306
socket=/tmp/mysql.sock
key_buffer_size=16M
max_allowed_packet=8M

[mysql]
port=3306
socket=/tmp/mysql.sock
no-auto-rehash


[mysqldump]
quick


!include /home/mysql/myopt.cnf
```

<br>

**影响配置文件的命令行选项**

- `--print-defaults`
- `--defaults-extra-file`
- `--defaults-file`
- `--defaults-group-suffix`
- `--login-path`
- `--no-defaults`

<br>

**使用选项指定环境变量**

```
[mysql]
max_allowed_packet=16M

[mysqld]
key_buffer_size=512M


mysql --max_allowed_packet=16M

shell> mysql --max_allowed_packet=16*1024*1024
mysql> SET GLOBAL max_allowed_packet=16*1024*1024;
```



<br/>
<br/>


## MySQL Server


- `mysqld`
The MySQL Server

- `mysql_safe`
MySQL Server Startup Script

- `mysql.server`
MySQL Server Startup Script

- `mysqld_multi`
Manage Multiple MySQL Servers


<br/>


### mysqld

mysqld，也被称为MySQL服务器，是执行MySQL大部分工作的主要程序。MySQL服务器管理对包含数据库和表的MySQL数据目录的访问。

查看帮助： `mysqld --verbose --help`


<br/>


### mysql_safe


**对于某些Linux平台，从RPM或DBP包安装的MySQL包括了用于管理MySQL服务启动和管理的systemd支持。在这些平台上，`mysqld_safe`不会被安装，因为它不是必须的。**

`mysql_safe`是Unix上启动mysqld服务器的推荐方式。它添加了一些安全特性，如发生错误是重启服务器并将运行时的错误记录到日志。

`mysqld_safe`尝试启动一个名为mysqld的可执行程序。它会读取配置文件中`[mysqld], [server], [mysqld_safe]`部分的所有选项。


`mysqld_safe`选项：

```
--basedir

--core-file-size

--datadir

--defaults-extra-file

--defaults-file

--ledir

--log-error

--mallocl-lib

--mysqld

--mysqld-safe-login-timestamps

--mysql-version

--nice

--no-defaults

--open-files-limit

--pid-file

--plugin-dir

--plugin-dir

--port

--skip-kill-mysqld

--skip-syslog

--socket

--syslog-tag

--timezone

--user
```


<br/>
<br/>


### mysql.server


**对于某些Linux平台，从RPM和DPG包安装的MySQL包括了用于管理MySQL Server启动和关闭的systemd支持。在这些平台上，没有安装`mysql.server`和`mysqld_safe`，因为它们不是必须的。**

Unix和Unix-Like平台上的MySQL发行版包含一个名为`mysql.server`的脚本，该脚本使用`mysqld_safe`启动MySQL Server。


<br/>
<br/>


### mysqld_multi


**对于某些Linux平台，从RPM和DPG包安装的MySQL包括了用于管理MySQL Server启动和关闭的systemd支持。在这些平台上，没有安装`mysqld_multi`，因为它们不是必须的。**

`mysqld_multi`设计用于管理多个监听不同Unix socket文件和TCP/IP port上连接的mysqld进程。



<br/>
<br/>


## MySQL安装相关程序


这些程序用于安装或升级MySQL！

- `com_err`
Compile MySQL Error Message File

- `mysql_install_db`
Initialize MySQL Data Directory

- `mysql_plugin`
Configure MySQL Server Plugins

- `mysql_secure_installation`
Improve MySQL Installation Security

- `mysql_ssl_rsa_setup`
Create SSL/RSA Files

- `mysql_tzinfo_to_sql`
Load the Time Zone Tables

- `mysql_upgrade`
Check and Upgrade MySQL Tables


<br/>


### com_err

`comp_err`创建errmsg.sys文件，`mysqld`使用此文件来确定为不同错误代码(error code)显示错误消息。通常，在构建MySQL时，`comp_err`会自动运行。它从位于MySQL源发行版`sq;/share/errmsg-utf8.txt`文本文件汇编`errmsg.sys`文件。

`comp_err`同样会生成`mysqld_error.h, mysqld_ername.h, sql_state.h`头文件。


<br/>
<br/>


### mysql_install_db

在MySQL5.7中，由于`mysql_install_db`的功能已经被集成到mysqld中，因此不推荐使用它。
在MySQL5.7.5之前，`mysql_install_db`是一个Perl脚本并依赖于Perl。在此之后，它是由C++写的可执行二进制文件。还有一些选项的更迭。

```
mysqld --initailize

#or
mysqld --initialize-insecure
```

<br>

`mysql_install_db`处理在MySQL Server(mysqld)准备好使用之前，必须执行的初始化任务：

- 初始化MySQL数据目录，创建它包含的系统表
- 初始化管理InnoDB表所需的`system tablespace`和相关数据结构
- 加载服务器端help表
- 安装`sys schema`
- 创建一个管理员账户
老版本的`mysql_install_db`可能会创建匿名账户。

<br>

如果`mysql_install_db`生成了一个随机管理员密码，它将把此密码写入文件并显示此文件名。密码包含一个时间戳以指示它的写入时间。
默认情况下，该文件是用户主目录中的`.mysql_secret`文件。


<br/>
<br/>


### mysql_plugin

从MySQL5.7.11开始，不推荐使用`mysql_plugin`，并会在MySQL8.0中移除此功能。
使用如下命令替代：

```
--plugin-load
--plugin-load-add

#或
mysql> INSTALL PLUGIN
mysql> UNINSTALL PLUGIN
```

<br>

`mysql_plugin`功能允许MySQL管理员管理由MySQL Server载入的插件。


<br/>
<br/>


### mysql_secure_installation

`mysql_secure_installation`通过以下方式来提高MySQL安装的安全性：

- 为root用户设置密码
- 删除可从本机外部访问的root账户
- 删除匿名账户
- 删除test数据库(默认情况下可由任何用户访问，包括匿名用户)
- 删除允许任何人访问以`test_`开头的数据库的权限


<br/>
<br/>


### mysql_ssl_rsa_setup

`mysql_ssl_rsa_setup`创建SSL证书和key文件和RSA key-pair文件，用于支持使用SSL进行安全连接。它生成的整数是自签名的，不太安全。请考虑从注册机构申请CA证书。
`mysql_ssl_rsa_setup`使用`opensll`命令，所以请安装OpenSSL。


<br/>
<br/>


### mysql_tzinfo_to_sql

`mysql_tzinfo_to_sql`加载MySQL数据库中的zone table。它使用系统上的`zoneinfo`信息。


<br/>
<br/>


### msyql_upgrade

`mysql_upgrade`检查数据库中的所有表与当前版本的MySQL Server的不兼容，它还升级系统表，以便你可以利用新权限和功能。
如果`mysql_upgrade`发现表有可能的不兼容性，它会执行检查表，如果发现问题，则会尝试修复表。

每次升级MySQL时都应该执行`mysql_upgrade`。
在执行upgrade之前，你应该始终备份你的MySQL。



<br/>
<br/>


## MySQL客户端程序


- mysql
The MySQL Command-Line Tool

- mysqladmin
Client for Administering a MySQL Server

- mysqlcheck
A Table Maintenance Program

- mysqldump
A Database Backup Program

- mysqlimport
A Data Import Program

- mysqlpump
A Database Backup Program

- mysqlsh
The MySQL Shell

- mysqlshow
Display Database, Table, and Column Information

- mysqlslap
Load Emulation Client


<br/>


## mysql

mysql是一个具有输入编辑功能的SQL shell。

```sql
mysql --host= --port= --user= --password db_name


#SQL文件
#SQL语句以 ;或\g或\G结束
mysql db_name < script.sql > output.tab
```


<br/>


### mysql选项

MySQL支持很多选项。这些选项可以写入配置文件的`[mysql]`和`[client]`组中。

```
mysql --help
```


<br/>
<br/>


### mysql命令

mysql将你发出的每个SQL语句发送到要执行的Server。如下为mysql自己解释的命令：

```
mysql> help;

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
charset
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
resetconnection(\x) Clean session context.
```

<br>

**修改MySQL提示符**

```
#shell
export MYSQL_PS1="(\u@\h) [\d]> "


#mysql
mysql --prompt="(\u@\h) [\d]> "


#配置文件
[mysql]
prompt=(\\u@\\h) [\\d]>\\_


#mysql prompt
mysql> prompt (\u@\h) [\d]>\_
```


<br/>
<br/>


### mysql服务端帮助

mysql Server-Side Help

如果给`help`命令提供一个参数，mysql将其用作搜索字符串，以从MySQL参考手册的内容访问服务端帮助。

```
mysql> help me


mysql> help contents


mysql> help logs


mysql> help rep%
```


<br/>
<br/>


### 从文本文件执行SQL语句

mysql忽略文件开头的Unicode字节顺序标记(BOM)字符。BOM的存在不会导致MySQL更改其默认字符集(charset)。因此，请使用`--default-char-set`选项。

```
#shell
mysql db_name < test_file


mysql> source file_name
mysql> \. file_name


#显示进度信息
SELECT '<info_to_display>' AS ' ';
```


<br/>
<br/>


## MySQL管理和实用程序

- inochecksum
Offline InnoDB File Checksum Utility

- myisam_ftdump
Display Full-Text Index information

- myisamchk
MyISAM Table-Maintenance Utility

- myisamlog
Display MyISAM Log File Contents

- myisampack
Generate Compressed, Read-Only MyISAM Tables

- mysql_config_editor
MySQL Configuration Utility

- mysqlbinlog
Utility for Processing Binary Log Files

- mysqldumpslow
Summarize Slow Query Log Files



<br/>
<br/>


## mysql开发实用程序


- `mysql_config`
Display Options for Compiling Clients

- `my_print_defaults`
Display Options from Option Files

- `resolve_stack_dump`
Resolve Numeric Stack Trace Dump to Symbols



<br/>
<br/>



## 杂项程序

Miscellaneous Programs

- `lz4_decompress`
Decompress mysqlpump LZ4-Compressed Output

- `perror`
 Explain Error Codes

- `replace`
A String-Replacement Utility

- `resolveip`
Resolve Host name to IP Address or Vice Versa

- `zlib_decompress`
Decompress mysqlpump ZLIB-Compressed Output



<br/>
<br/>



## MySQL环境变量


这些环境变量直接或间接的被MySQL使用。

| Variable |  Description
| - | -
| AUTHENTICATION_LDAP_CLIENT_LOG |  Client-side LDAP authentication logging level.
| AUTHENTICATION_PAM_LOG |  PAM authentication plugin debug logging settings.
| CC |  The name of your C compiler (for running CMake).
| CXX |  The name of your C++ compiler (for running CMake).
| CC |  The name of your C compiler (for running CMake).
| DBI_USER |  The default user name for Perl DBI.
| DBI_TRACE |  Trace options for Perl DBI.
| HOME |  The default path for the mysql history file is $HOME/.mysql_history.
| LD_RUN_PATH |  Used to specify the location of libmysqlclient.so.
| LIBMYSQL_ENABLE_CLEARTEXT_PLUGIN |  Enable mysql_clear_password authentication plugin; see Section 6.5.1.6, “Client-Side Cleartext Pluggable Authentication”.
| LIBMYSQL_PLUGIN_DIR |  Directory in which to look for client plugins.
| LIBMYSQL_PLUGINS |  Client plugins to preload.
| MYSQL_DEBUG |  Debug trace options when debugging.
| MYSQL_GROUP_SUFFIX |  Option group suffix value (like specifying --defaults-group-suffix).
| MYSQL_HISTFILE |  The path to the mysql history file. If this variable is set, its value overrides the default for $HOME/.mysql_history.
| MYSQL_HISTIGNORE |  Patterns specifying statements that mysql should not log to $HOME/.mysql_history, or syslog if --syslog is given.
| MYSQL_HOME |  The path to the directory in which the server-specific my.cnf file resides.
| MYSQL_HOST |  The default host name used by the mysql command-line client.
| MYSQL_OPENSSL_UDF_DH_BITS_THRESHOLD |  Maximum key length for CREATE_DH_PARAMETERS(). See Section 12.18.2, “Enterprise Encryption Usage and Examples”.
| MYSQL_OPENSSL_UDF_DSA_BITS_THRESHOLD |  Maximum DSA key length for CREATE_ASYMMETRIC_PRIV_KEY(). See Section 12.18.2, “Enterprise Encryption Usage and Examples”.
| MYSQL_OPENSSL_UDF_RSA_BITS_THRESHOLD |  Maximum RSA key length for CREATE_ASYMMETRIC_PRIV_KEY(). See Section 12.18.2, “Enterprise Encryption Usage and Examples”.
| MYSQL_PS1 |  The command prompt to use in the mysql command-line client.
| MYSQL_PWD |  The default password when connecting to mysqld. Using this is insecure. See Section 6.1.2.1, “End-User Guidelines for Password Security”.
| MYSQL_TCP_PORT |  The default TCP/IP port number.
| MYSQL_TEST_LOGIN_FILE |  The name of the .mylogin.cnf login path file.
| MYSQL_TEST_TRACE_CRASH |  Whether the test protocol trace plugin crashes clients. See note following table.
| MYSQL_TEST_TRACE_DEBUG |  Whether the test protocol trace plugin produces output. See note following table.
| MYSQL_UNIX_PORT |  The default Unix socket file name; used for connections to localhost.
| MYSQLX_TCP_PORT |  The X Plugin default TCP/IP port number.
| MYSQLX_UNIX_PORT |  The X Plugin default Unix socket file name; used for connections to localhost.
| PATH |  Used by the shell to find MySQL programs.
| PKG_CONFIG_PATH |  Location of mysqlclient.pc pkg-config file. See note following table.
| TMPDIR |  The directory in which temporary files are created.
| TZ |  This should be set to your local time zone. See Section B.5.3.7, “Time Zone Problems”.
| UMASK |  The user-file creation mode when creating files. See note following table.
| UMASK_DIR |  The user-directory creation mode when creating directories. See note following table.
| USER |  The default user name on Windows when connecting to mysqld.




<br/>
<br/>

---

<br/>



# MySQL Server管理


## MySQL Server

mysqld is the MySQL Server.
并非所有的MySQL Server二进制文件和配置都支持所有的存储引擎。

```
#查看帮助
mysqld --verbose --help


#运行Server的环境变量
mysql> SHOW VARIABLES;


#运行Server的状态
mysql> SHOW STATUS;

```


<br/>
<br/>


## MySQL数据目录


由MySQL管理的信息存储在称为数据目录的目录下。

- 数据目录子目录：每个子目录都是数据库目录对应于Server管理的数据库
	+ mysql
	+ performance_schema
	+ sys
	+ 数据库
- 日志文件由Server写入
- innoDB表空间和日志文件
- 自动生成的SSL/RSA证书和密钥文件
- Server PID



<br/>
<br/>



## mysql数据库

The mysql System Database

The mysql database is the system database.它的表中存储了MySQL Server运行时需要的信息。

<br>

**授权系统表**
如下这些系统表包含了用户账户和权限的授权信息。

- user
User accounts, global privileges, and other non-privilege columns.

- db
 Database-level privileges.

- tables_priv
Table-level privileges.

- columns_priv
Column-level privileges.

- procs_priv
Stored procedure and function privileges.

- proxies_priv
Proxy-user privileges.

<br>

**对象信息系统表**
如下这些系统表包含了存储程序，用户定义函数和服务器端插件的信息。

- event
关于Event Scheduler事件的信息

- func
用户定义函数的信息

- plugin
服务器端的插件的信息

- proc
有关存储过程和函数的信息

<br>

**日志系统表**
Server使用如下系统表记录日志。日志表使用CSV存储引擎。

- general_log
一般查询日志表

- slow_log
慢查询日志表

<br>

**服务器端帮助系统表**
如下系统表包含了服务器端帮助信息。

- help_category
Information about help categories.

- help_keyword
Keywords associated with help topics.

- help_relation
Mappings between help keywords and topics.

- help_topic
Help topic contents.

<br>

**时区系统表**
如下系统表包含了时区信息。

- time_zone
Time zone IDs and whether they use leap seconds.

- time_zone_leap_second
When leap seconds occur.

- time_zone_name
Mappings between time zone IDs and names.

- time_zone_transition, time_zone_tansition_type
Time zone descriptions.

<br>

**副本系统表**
Server使用如下这些系统表来提供副本服务。这些表使用InnoDB存储引擎。

- gtid_executed
Table for storing GTID values.

- ndb_binlog_index
Binary log information for NDB Cluster replication.

- slave_master_info, slave_relay_log_info, slave_worker_info
Used to store replication information on slave servers.

<br>

**优化器系统表**
如下系统表用于优化。

- innodb_index_stats, innodb_table_stats
Used for InnoDB persistent optimizer statistics

- server_cost, engine_cost
The optimizer cost model uses tables that contain cost estimate information about operations that occur during query execution.

<br>

**杂项系统表**

- audit_log_filter, audit_log_user
- firewall_users, firewall_whitelist
- servers



<br/>
<br/>



## MySQL Server Logs


MySQL Server提供如下几种日志：

- Error log
启动、运行或停止mysqld遇到的问题

- General query log
建立Client连接和从Client收到的语句

- Binary log
更改数据的语句

- Relay log
从replication master server收到的数据更改

- Slow query log
执行时间超过`long_query_time`秒的查询

- DDL(Metadata) log
由DDL语句执行的元数据操作

<br>

默认情况下，不启用任何日志。

如果启用了这些日志，MySQL Server可以灵活地控制一般查询日志和慢查询日志的输出目的地——它可为日志文件或`mysql`数据库中的`general_log`和`slow_log`表。

```
#--log-output
#它的值可为TABLE/FILE/NONE
#--general-log, --slow-query-log


#TABLE和FILE
mysqld --log-output=TABLE,FILE --general-log=msyql.general_log --slow-query-log=mysql.slow_log



#or
[mysqld]
log_output=
general_log=
slow_query_log=
```

查看两个日志表的标准格式：

```
SHOW CREATE TABLE mysql.general_log;
SHOW CREATE TABLE mysql.slow_log;
```


<br/>
<br/>


### 错误日志

The Error Log

错误日志包含mysqld启动和关闭时间的记录。它还包含诊断信息。

<br>

**Unix/Unix-Like OS**
使用`mysqld --log-error`选项来将错误日志写入控制台(stderr)或文件。

如果未指定文件名，则默认为数据目录下的`host_name.err`文件。
YUM或APT包安装，则配置的错误日志文件为`--log-error=/var/log/mysqld.log`。

<br>

**将错误日志记录到系统日志**
Error Logging to the System Log

使用如下系统变量：

- `log_syslog`
启用此变量将错误日志发送到系统日志

- `log_syslog_facility`
syslog消息的默认设置时daemon。设置此变量以指定其它工具。

- `log_syslog_include_pid`
是否在syslog输出中包含Server的PID。

- `log_syslog_tag`
在syslog消息中添加一个tag。

```
msyqld --log_syslog=

```

<br>

**错误日志过滤**
Error Log Filtering

`log_error_verbosity`变量控制错误日志的详细程度。值如下：

- 1
error only

- 2
errors, warning

- 3(默认)
errors, warnings, notes

<br>

**错误日志消息格式**
Error Log Message Format

错误日志中包含的ID是mysqld中负责编写消息的线程的ID。这表示Server的哪部分生成了消息。
`log_timestamps`变量控制写入错误日志的时区和时间格式。

```
mysqld --log-timestamps=
```

<br>

**错误日志文件刷新**
Error Log File Flushing and Renaming

如果你使用`FLUSH_ERROR_LOGS`, `FLUSH_LOGS`或`mysqladmin flush-logs`刷新日志，Server将关闭并重新打开它正在写的任何错误日志文件。

```
mv host_name.err host_name.err-old
mysqladmin flush-logs
```


<br/>
<br/>



### 一般查询日志

The General Query Log

一般查询日志是mysqld执行操作的记录。当Client连接或断开时，Server将此信息写入日志，并记录从Client收到的每个SQL语句。
mysqld按照接收的顺序而不是执行顺序将语句写入日志。

默认情况下，一般查询日志是禁用的。
指定初始化查询日志状态`--general_log={0|1}`。`1`启用，`0`禁用。
指定日志文件名`--general-log-file=file-name`.如果未指定，默认为数据目录下`host_name.log`，除非指定了其它路径。
指定日志文件位置`--log-output=`.

```
mysqld --log-output='/var/log/mysql' --general-log=1 --general-log-file='general.log'


shell> mv host_name.log host_name-old.log
shell> mysqladmin flush-logs
```



<br/>
<br/>



### 二进制日志

The Binary Log




















































































<br/>
<br/>

---

<br/>




# 安全

Security


当考虑MySQL安装中的安全性时，你应该考虑各种可能的主题以及他们如何影响MySQL Server和相关应用程序的安全性:

- 影响安全性的一般因素。包括选择好的密码，不向用户授予不必要的权限，防止SQL注入和数据损坏来确保应用程序的安全性...
- 安装本身的安全性。应保护数据文件，日志文件和安装的所有应用程序文件，以确保未经授权方无法读写这些文件...
- 数据库系统本身的访问控制和安全性。包括允许访问数据库中使用的数据库，视图和存储应用程序的用户和数据库...
- 安全相关插件提供的功能...
- MySQL和你的系统的网络安全性。安全性还与用户的授权有关，但你可能希望限制MySQL，使其仅在本地主机上可用，或者在一组有限的其它主机上可用...
- 确保你备份了足够和适当的数据库文件，配置和日志文件。还要确保你已准备好恢复解决方案，并测试是否能够从备份种恢复信息...


<br/>



## 一般安全问题

General Security Issues


本节介绍了要注意的一般安全问题，以及如何使MySQL安装更安全，防止攻击或滥用。


<br/>


### 安全指南

Security Guidelines


在连接了Internet的计算机上使用MySQL的任何人都应阅读本节，以避免最常见的安全错误。
在讨论安全性时，有必要考虑完全保护整个服务器主机免受所有类型的攻击：窃听，更改，拒绝服务...

MySQL使用基于访问控制列表(ACL)的安全性，来处理用户可以尝试执行的所有连接、查询和其它操作。MySQL Client和Server之间SSL加密连接。

当运行MySQL时，遵循以下准则：

- 不要让任何人(root除外)访问`mysql.user`数据表，这很关键。

- 了解MySQL访问权限系统的工作原理。使用`GRANT`和`REVOKE`语句来控制对MySQL的访问。不要授予超出必要的权限，永远不要授予所有主机权限。
如果你能够在不被要求输入密码的情况下成功连接到MySQL Server，则任何人都可以以具有完全权限的root用户身份连接到MySQL Server。请重新查看MySQL安装说明，特别注意有关设置root密码的信息。
检查哪些账户拥有访问权限，并移除不必要的权限。

```
#测试
mysql -u root


#访问权限
SHOW GRANTS;


#移除权限
#help REVOKE
REVOKE
```

- 不要在数据库中存储明文密码。如果计算机被攻击，入侵者可以获得完整的密码列表并使用他们。相反，使用一些HASH函数并存储散列值。

- 不要从字典中选择密码，即不要使用简单和常规密码。存在某些破解密码的程序能计算你的密码。

- 启用防火墙。这可以保护你免受大部分漏洞攻击。将MySQL放在防火墙后面或DMZ。
使用端口扫描软件(如nmap)扫描主机端口。MySQL默认使用3306端口。不应从不受信任的主机访问此端口。测试你的端口安全性：

```
[zhang@zhang21 ~]$ telnet zhang21 3306
Trying 192.168.31.119...
Connected to zhang21.
Escape character is '^]'.
@Host 'zhang21' is not allowed to connect to this MySQL serverConnection closed by foreign host.



[zhang@zhang21 ~]$ telnet localhost 3306
Trying ::1...
Connected to localhost.
Escape character is '^]'.
J
5.7.22
[cqo3I  @kX@n#I\mysql_native_password
```

- 访问MySQL的应用程序不应该信任用户输入的任何数据，应该使用适当的防御性编程技术编写。

- 不要通过Internet传输普通数据(未加密)。请使用SSL或SSH加密协议。MySQL支持内部SSL连接；或是使用SSH端口转发为通信创建加密隧道。

- 学习使用`tcpdump`和`strings`使用程序。
可使用如下命令来检查MySQL数据流是否加密:

```
tcpdump -l -i eth0 -w - src or dst port 3306 | strings

```


<br/>
<br/>


### 密码安全

Keeping Passwords Secure


密码出现在MySQL的多个上下文中。此解提供了一些准则，使用户和管理员能够保护这些密码的安全性，并避免暴露这些密码。还讨论了MySQL如何在内部使用密码散列以及可用来强制执行更严格密码的插件。


<br/>


#### 密码安全用户指南

End-User Guidelines for Password Security

MySQL用户应使用以下准则来保证密码安全。当运行Client连接到MySQL server时，不建议以公开的方式来指定你的密码。

- 使用`mysql_config_editor`实用程序，它可将身份认证凭据存储在名为`.mylogin.cnf`的加密登录路径文件中。
- 使用`-pPASSWD`或`--password=PASSWD`选项
- 使用`-p`或`--password`选项不指定值
- 将密码存储到配置文件
- 将密码存储到`MYSQL_PWD`环境变量

```sh
#强烈不推荐
#这虽然方便却不安全
mysql -u user -pPASSWD db_name



#推荐
#但这适用于交互式
mysql -u user -p db_name
Enter password: xxx



#写入配置文件
chmod 600 ~/.my.cnf
vim ~/.my.cnf

[client]
password=xxx

mysql --defaults-file=~/.my.cnf



#指定MySQL密码环境变量的方法非常不安全，不应该使用
```

<br>

在Unix上，MySQL Client将执行语句的记录写入历史文件。默认情况下，此文件为`~/.mysql_history`。密码可以在SQL语句中以纯文本形式写入(如`CREATE USER`, `ALTER USER`)，如果使用了这些语句，它们将被记录到历史文件中。要保证此文件的安全，请使用限制访问模式。

如果命令解释器程序配置为维护历史记录，则保存命令的任何文件都将包含在命令行中输入的MySQL密码。如bash下的`~/.bash_history`。


<br/>
<br/>


#### 密码安全管理员指南

Administrator Guidelines for Password Security

MySQL数据库管理员应使用以下准则来保证密码安全：

- MySQL在`mysql.user`表中存储用户账户密码。永远不要向任何非管理账户授予此表的访问权限
- 账户密码可以过期，以便用户必须重置密码
- `validate_password`插件可用于对可接受的密码强制实施策略
- 应该保护可能写入密码的日志文件等文件


<br/>
<br/>


#### 密码和日志

Passwords and Logging

密码可在SQL语句中以纯文本形式写入，如`CREATE USET`, `GRANT`, `SET PASSWORD`和调用`PASSWORD()`函数的语句。如果MySQL Server记录了这些语句，那么访问日志的任何人都可以看到密码。

语句记录避免以明文形式为以下语句编写密码：

```sql
CREATE USER ... IDENTIFIED BY ...
ALTER USER ... IDENTIFIED BY ...
GRANT ... IDENTIFIED BY ...
SET PASSWORD ...
SLAVE START ... PASSWORD = ...
CREATE SERVER ... OPTIONS(... PASSWORD ...)
ALTER SERVER ... OPTIONS(... PASSWORD ...)
```

<br>

对于常规查询日志，可通过使用`--log-raw`选项启动Server来抑制密码重写。出于安全原因，此选项不建议用于生产环境。处于诊断目的，查看Server收到的语句的确切文本可能很有用。
审计日志插件生成的审计日志文件的内容未加密。出于安全原因，应将此文件写入只有MySQL Server和用户才能访问的目录，并且有正当理由查看目录。
只有在需要纯文本密码时才会进行密码重写，对于具有期望密码散列语法的语句，不会发生重写。
要保护日志文件免受不必要的暴露，请将他们放在限制访问Server和管理员的目录中。
副本集slave将复制副本集master的密码存储在主信息存储库中，它可以是文件或表。确保只能由管理员访问此库。

使用受限的访问模式来保护包含日志表或密码的日志文件的数据库备份。


<br/>
<br/>


#### 密码散列

Password Hashing in MySQL


MySQL在`mysql.user`数据表中列出用户账户。可以为每个MySQL账户分配一个密码，尽管用户表不存储明文密码，而是存储密码的散列值。

MySQL在Client/Server通信的两个阶段中使用密码：

- 当客户端尝试连接到Server时，有一个初始身份认证步骤，其中客户端必须提供密码，该密码的散列值与`mysql.user`用户表中存储的散列值相匹配
- 客户端连接之后，它可以(如果有足够权限)设置或更改`mysql.user`用户表中账户的密码的散列值。客户端可通过使用`PASSWORD()`函数来生成密码散列，或使用密码生成语句(`CREATE USER`, `GRANT`, `SET PASSWORD`)来完成此操作。

换句话说，Server在客户端首次尝试连接时在身份认证期间检查散列值。如果连接的客户端调用`PASSWORD()`函数，或使用密码生成语句来设置/更改密码，则Server会生成散列值。

```
help PASSWORD;
#This function is deprecated as of MySQL 5.7.6 and will be removed in a future MySQL release



#The Original (Pre-4.1) Hashing Method
#原始散列方法产生一个16Byte的字符串
mysql> SELECT PASSWORD('mypass');
+--------------------+
| PASSWORD('mypass') |
+--------------------+
| 6f8c114b58f2ce9e   |
+--------------------+



#The 4.1 Hashing Method
#MySQL4.1引入了密码散列，提供了更好的安全性并降低了密码被截获的风险
#生成更长的41Byte的散列值
mysql> SELECT PASSWORD('mypass');
+-------------------------------------------+
| PASSWORD('mypass')                        |
+-------------------------------------------+
| *6C8989366EAF75BB670AD8EA7A7FC1176A95CEF4 |
+-------------------------------------------+
```

<br>

**散列方法的兼容性问题**
Compatibility Issues Related to Hashing Methods



<br/>
<br/>


### 使MySQL安全抵御攻击者

Making MySQL Secure Against Attackers


连接到MySQL server时，应使用密码。密码在连接时不会以明文形式传输。所有其它信息都以文本形式传输，对任何能够看到连接的人都可读。如果连接通过不信任的网络，则可以使用压缩协议使流量更难以解密。你还可以使用MySQL的内部SSL支持来使连接更安全。或者，使用SSH在MySQL server和client之间获得加密的TCP/IP连接。

要使得MySQL系统安全，你应该强烈考虑以下建议：

- 要求所有MySQL账户都有密码
- 确保只有Unix用户账户对数据库目录具有读写权限，它是用于运行mysqld的账户
- 永远不要以root用户运行MySQL server，应该使用普通的非特权用户运行
- MySQL用户账户和Unix系统账户没有关联
- 不要对非管理员用户授予`FILE`权限，具有此权限的用户都可使用mysqld daemon的权限在文件系统的任何位置编写文件，同样也可读取文件，并将文件载入数据库
- 不要对非管理员用户授予`PROCESS`或`SUPER`权限(可用于终止连接，修改系统变量...)
- 不允许对表使用符号链接
- 安全地存储程序和视图
- 如果不信任DNS，则应在授权表中使用IP地址而非主机名
- 如果想要限制单个账户的连接数，可在mysqld中配置`max_user_connection`变量
- 如果插件目录对server可写，这可修改它为只读


```
#服务
cat /usr/lib/systemd/system/mysqld.service
[Service]
User=mysql
Group=mysql



#或配置文件
/etc/my.cnf
[mysqld]
user=mysql



#查看当前正在执行的语句
msyql> SHOW PROCESSLIST;
+----+------+-----------+-------+---------+------+----------+------------------+
| Id | User | Host      | db    | Command | Time | State    | Info             |
+----+------+-----------+-------+---------+------+----------+------------------+
| 18 | root | localhost | mysql | Query   |    0 | starting | SHOW PROCESSLIST |
+----+------+-----------+-------+---------+------+----------+------------------+



# Disabling symbolic-links is recommended to prevent assorted security risks
cat /etc/my.cnf
symbolic-links=0
```


<br/>
<br/>


### 安全相关的mysqld选项和变量

Security-Related mysqld Options and Variables


下表显示了影响安全性的mysqld的选项和系统变量:

| Name | Cmd-Line | Option File | System Var | Status Var | Var Scope | Dynamic |
| - | - | - | - | - | - | - |
| allow-suspicious-udfs | Yes | Yes |  |  |  |  |
| automatic_sp_privileges |  |  | Yes |  | Global | Yes |
| chroot | Yes | Yes |  |  |  |  |
| des-key-file | Yes | Yes |  |  |  |  |
| local_infile |  |  | Yes |  | Global | Yes |
| old_passwords |  |  | Yes |  | Both | Yes |
| safe-user-create | Yes | Yes |  |  |  |  |
| secure-auth | Yes | Yes |  |  | Global | Yes |
| - Variable: secure_auth |  |  | Yes |  | Global | Yes |
| secure-file-priv | Yes | Yes |  |  | Global | No |
| - Variable: secure_file_priv |  |  | Yes |  | Global | No |
| skip-grant-tables | Yes | Yes |  |  |  |  |
| skip-name-resolve | Yes | Yes |  |  | Global | No |
| - Variable: skip_name_resolve |  |  | Yes |  | Global | No |
| skip-networking | Yes | Yes |  |  | Global | No |
| - Variable: skip_networking |  |  | Yes |  | Global | No |
| skip-show-database | Yes | Yes |  |  | Global | No |
| - Variable: skip_show_database |  |  | Yes |  | Global | No |


<br/>
<br/>


### 以普通用户运行MySQL

How to Run MySQL as a Normal User


在Linux上，使用MySQL-repo、RPM包、Debian包来安装MySQL。MySQL server mysqld默认是由操作系统的mysql用户来启动。

对于使用`.tar.gz`包进行的安装，你需要修改为non-root用户。

```
chown -R user_name /path/to/mysql/datadir



vim /etc/my.cnf
[mysqld]
user=user_name
```


<br/>
<br/>


### `LOAD DATA LOCAL`的安全问题

`LOAD DATA`语句可以载入主机上的文件。

这有两个潜在的安全问题：

- 从Client到Server的文件传输是由MySQL server启动。理论上，server可告诉client传输server选择的文件而不是`LOAD DATA`语句中client指定的文件。这样，server可以访问client端用户可访问的任何文件。
- 在client从web server连接的Web环境中，用户可使用`LOAD DATA LOCAL`来读取Web server进程具有访问权限的任何文件。

为了避免`LOAD DATA`问题，客户端应该避免使用`LOCAL`。为避免连接到不受信任的Server，Client可通过`--ssl-mode=xxx`选项和相应的CA证书建立安全的连接。

<br>

要是管理员和应用程序能够管理本地数据加载功能，`LOCAL`配置的工作方式如下：

- On the server side
`local_infile`系统变量控制服务器端的`LOCAL`功能。默认启用`local_infile`。

- On the client side
`ENABLED_LOCAL_INFILE` CMake选项控制MySQL Client Library的已编译的默认`LOCAL`功能。
使用C API的客户端程序可通过调用`mysql_options()`来启用/禁用`MYSQL_OPT_LOCAL_INFILE`。
对于mysql Client，默认禁止本地数据载入。使用`--local-infile=1/0`
对于mysqlimport client，默认禁用本地数据载入。使用`--local=1/0`


<br/>
<br/>


### 客户端程序安全指南

Client Programming Security Guidelines


访问MySQL的应用程序不应该信任用户输入的任何数据，用户可以尝试通过在Web表单，URL或构建的任何应用程序中输入特殊字符序列来欺骗你。如果用户输入`DROP DATABASE mysql;`类似语句，请确保你的应用程序保持安全，这是一个极端栗子。
有时人们会认为，如果数据库只包含公开可用的数据，则无需受到保护。这是不正确的。即使允许在数据库中显示任何行，你仍应该防止拒绝服务攻击。

检查清单：

- 启用更严格的SQL模式以告知server对其接收的数据做更多限制
- 注意单/双引号
- 通过添加`%22("), %23("), %27(')`来修改动态URLs
- 动态修改URL中的数据类型
- 尝试输入字符、空格和特殊符号，而不是 数字
- 将数据传递给MySQL前检查数据大小
- 使用不同于管理员的用户将应用程序连接到数据库



<br/>
<br/>
<br/>



## 访问权限系统

The MySQL Access Privilege System


MySQL权限系统的主要功能就是对从给定主机连接的用户进行身份认证，并将用户与数据库的权限(`SELECT, INSERT, UPDATE, DELETE`)相关联。其它功能包含匿名用户(anonymous user)和MySQL特定功能的授权。

有些事情你无法使用MySQL权限系统：

- 你无法明确指定拒绝给定用户访问
- 你无法指定用户创建/删除表的权限，但不能指定创建/删除数据库自身
- 适用于账户全局性的密码

<br>

MySQL权限系统的用户接口(user interface)由： `CREATE USER, GRANT, REVOKE`语句组成。

在内部，Server将权限信息存储在`mysql`数据库的授权表中。MySQL server在启动时将这些表内容读入内存，并根据授权表的内存中的副本建立访问控制决策。
MySQL权限系统确保所有用户只能执行允许的操作。作为用户，当你连接到MySQL server时，你的身份由你连接的主机和你指定的用户名决定。在连接后，系统会根据你的身份和要执行的操作授予权限。
MySQL会识别你的主机名和用户名，因为没有理由认为给定的用户名属于所有主机上的同一个人。

```
SHOW GRANTS;

SHOW GRANTS FOR 'joe'@'office.example.com';
SHOW GRANTS FOR 'joe'@'home.example.com';
```

<br>

当运行客户端程序连接到server时，MySQL访问控制包含两个阶段：

1. server根据你的身份来接受/拒绝连接，以及你是否可通过提供正确的密码来验证你的身份
2. 假设你可以连接，server会检查你发出的每个语句，以确定是否有足够的权限来执行它



<br/>
<br/>


### MySQL提供的权限

Privileges Provided by MySQL


授予MySQL账户的权限决定了账户可以指定的操作。MySQL权限在它们适用的上下文和不同操作级别上有所不同：

- 管理员权限(Administrative privileges)允许用户管理MySQL Server的操作。这些权限是全局的，因为它们不是特定于特定数据库
- 数据库权限(privileges for database)适用于数据库及其中的所有对象。可以为特定数据库或全局赋予这些权限，以便它们适用于所有数据库
- 数据库对象权限(privileges for database object)，如表，索引，视图...


<br/>
<br/>


#### 可用权限

Summary of Available Privileges

下表显示了`GRANT`和`REVOKE`语句中使用的权限名称，以及每个权限关联的列名和权限适用的上下文:

| Privilege | Grant Table Column | Context |
| - | - | - |
| ALL [PRIVILEGES] | Synonym for “all privileges” | Server administration |
| ALTER | Alter_priv | Tables |
| ALTER ROUTINE | Alter_routine_priv | Stored routines |
| CREATE | Create_priv | Databases, tables, or indexes |
| CREATE ROUTINE | Create_routine_priv | Stored routines |
| CREATE TABLESPACE | Create_tablespace_priv | Server administration |
| CREATE TEMPORARY TABLES | Create_tmp_table_priv | Tables |
| CREATE USER | Create_user_priv | Server administration |
| CREATE VIEW | Create_view_priv | Views |
| DELETE | Delete_priv | Tables |
| DROP | Drop_priv | Databases, tables, or views |
| EVENT | Event_priv | Databases |
| EXECUTE | Execute_priv | Stored routines |
| FILE | File_priv | File access on server host |
| GRANT OPTION | Grant_priv | Databases, tables, or stored routines |
| INDEX | Index_priv | Tables |
| INSERT | Insert_priv | Tables or columns |
| LOCK TABLES | Lock_tables_priv | Databases |
| PROCESS | Process_priv | Server administration |
| PROXY | See proxies_priv | table  Server administration |
| REFERENCES | References_priv | Databases or tables |
| RELOAD | Reload_priv | Server administration |
| REPLICATION CLIENT | Repl_client_priv | Server administration |
| REPLICATION SLAVE | Repl_slave_priv | Server administration |
| SELECT | Select_priv | Tables or columns |
| SHOW DATABASES | Show_db_priv | Server administration |
| SHOW VIEW | Show_view_priv | Views |
| SHUTDOWN | Shutdown_priv | Server administration |
| SUPER | Super_priv | Server administration |
| TRIGGER | Trigger_priv | Tables |
| UPDATE | Update_priv | Tables or columns |
| USAGE | Synonym for “no privileges” | Server administration |


<br/>
<br/>


#### 授权指南

Privilege-Granting Guidelines

最好只向账户授权它所需要的权限，在授予`FILE`和管理权限时应特别小心:

- `FILE`： 可在MySQL Server主机上读取的任何文件读入数据库表
- `GRANT OPTION`： 使用户能够将其权限授权其他用户。具有不同权限且具有`GRANT OPTION`权限的两个用户可以组合权限
- `ALTER`: 可通过重命名表来破坏权限系统
- `SHUTDOWN`： 通过终止Server完全拒绝向其它用户提供服务
- `PROCESS`： 用于查看当前正在执行的语句的纯文本，包括设置和更改密码的语句
- `SUPER`： 用于终止其它会话或更改服务器的运行方式
- 为mysql系统数据本自身授予的权限可用于更改密码和其它访问权限信息：
	+ 密码以加密方式存储，因此恶意用户无法简单地读取明文密码。然而，具有对`mysql.user`表`authentication_string`列具有写权限的用户可以更改账户密码，然后进行登录
	+ 为mysql系统数据库授予`INSERT`或`UPDATE`权限允许用户添加或修改现有权限
	+ mysql系统数据库的`DROP`权限使用户能够访问远程权限表，甚至是数据库本身



<br/>
<br/>



### 授权表

Grant Tables


mysql系统数据库包含多个授权表，其中包含有关用户账户及其拥有的权限信息。
mysql数据库表包含的授权信息：

- `user`: 用户账户，全局权限，其它非权限列
- `db`: 数据库级别权限
- `tables_priv`：表级别权限
- `columns_priv`： 列级别权限
- `procs_priv`： 存储过程和功能权限
- `proxies_priv`： 代理用户权限

每个授权表包含的列范围和列权限：

- 列范围确定表中每行的范围
- 列权限指示表中行授予的权限

Server以下列方式使用授权表：

- `user`表范围列确定是拒绝还是允许传入连接
- `db`表范围列确定哪些用户可以从哪些主机上访问数据库
- `tables_priv`和`columns_priv`表更精细，它们适用于表级别和列级别
- `procs_priv`表用于存储的例程
- `proxies_priv`表指示哪些用户可以充当其它用户的代理，以及用户是否可以将`PROXY`权限授予其它用户



<br/>
<br/>



### 指定账户名

Specifying Account Names


MySQL账户名由用户名和主机名组成。这样可以为具有相同名称且可以从不同主机连接的用户创建账户。

在SQL语句中，账户名称遵循以下规则：

- 账户名语法为: `username@hostname`
- 仅包含用户名的账户相当于`username@%`
- 注意反引号、单引号、双引号
- 引号的正确用法: `'username'@'hostname'`

MySQL使用单独的用户名和主机名部分将账户名称存储到mysql系统数据库的授权表中：

- `user`表包含每个账户的一行，`user.User`，`user.Host`列存储用户名和主机名，此表还指示了账户具有哪些全局权限
- 其它授权表指示账户对数据库和库中对象的权限，这些表也有`User, Host`列来存储用户名和主机名
- 处于访问检查的目的，User value区分大小写，Host value不区分大小写

<br>

用户名和主机名还具有某些特殊值或通配符约定，如下:
账户名的用户名部分是非空白值，或者是与任何用户名匹配的空值。具有空白用户名的账户是匿名用户(anonymous user)。在SQL语句中指定一个匿名用户，使用带引号的空用户名，如`''@'localhost'`。

账户名的主机名部分可以采用多种形式，并允许使用通配符：

- host value可以是主机名或IP地址(ipv4, ipv6)

- 主机名或IP地址值中允许使用`%`和`_`通配符。
例如，主机值`%`匹配任何主机名，如`%.mysql.com`匹配`mysql.com`域中的任何主机。

- 对于IPv4地址，可以给出网络掩码以指示用于网络号的地址位数

```
CREATE USER 'test'@'198.51.100.0/255.255.255.0；
```

Server使用系统DNS解析程序为客户端主机名或IP地址返回的值，意味着你应该使用DNS使用的相同格式指定的账户主机值。



<br/>
<br/>



### 访问控制

Access Control


<br/>


#### 连接验证

Access Control, Stage 1: Connection Verification


当你连接到MySQL Server，它会根据以下条件接受或拒绝连接：

- 身份和密码
- 账户是否被锁定

Server首先检查凭据，然后检查账户锁定状态。任一步骤失败都会导致Server完全拒绝你的访问权限。使用`mysql.user`表中的三个范围列：`Host, User, authentication_string`执行凭据检查。

```sql
SELECT User, Host FROM mysql.user;
+-----------+----------+-
| Host      | User     | ...
+-----------+----------+-
| %         | root     | ...
| %         | jeffrey  | ...
| localhost | root     | ...
| localhost |          | ...
+-----------+----------+-


#内存中排序的表
+-----------+----------+-
| Host      | User     | ...
+-----------+----------+-
| localhost | root     | ...
| localhost |          | ...
| %         | jeffrey  | ...
| %         | root     | ...
+-----------+----------+-
```

- `%`： 表示任意主机
- 空用户名： 表示任意

当可能存在多个匹配时，Server必须确定要使用安歇匹配项：

- 只要Server将用户表读入内存，它就会对行进行排序
- 当用户尝试连接时，Server按排序顺序查看行
- Server使用与客户端host和username匹配的第一行


<br/>
<br/>


#### 请求认证

Access Control, Stage 2: Request Verification


通过连接发出的每个请求，Server确定你要执行的操作，然后检查你是否具有足够的权限来执行此操作。这就是授权表中权限列生效的地方。这些权限可以来自任何`user, db, table, column, procs`。

- 全局权限(global)
适用于全局范围内

- 数据库权限
	+ 空用户名匹配匿名用户，用户名中没有通配符
	+ 通配符`%`和`_`可在Host和Db列中使用，与`LIKE`操作符执行的模式匹配类似。如果要使用原字符，请使用反斜杠对其转义
	+ `%`或空白Host值表示任意主机
	+ `%`或空白Db值表示任意数据库

- 表、列、proc权限
	+ 通配符`%`, `_`

<br>

以布尔术语表示，总结用户权限：

```
global privileges
OR (database privileges AND host privileges)
OR table privileges
OR column privileges
OR routine privileges
```


<br/>
<br/>


### 权限更改生效时

When Privilege Changes Take Effect


启动msyqld时，它读取所有授权表到内存中，内存中的表在此时对访问控制有效。
如果使用`GRANT, REVOKE, SET PASSWORD, RENAME USER`账户管理语句间接修改了授权表，则Server会注意到这些更改并立即在此将授权表加载到内存中。
如果直接使用`INSERT, UPDATE, DELETE`语句修改授权表，则在重启Server或指示重新加载表之前，对权限的更改没有影响。也就是说，直接修改授权表但没有重新加载它的话，更改时无效的。

告诉Server重新加载授权表，有几种方式：

```
mysql> FLUSH PRIVILEGES

#或
$ mysqladmin flush-privileges

#或
$ mysqladmin reload
```

如果使用`--skip-grant-tables`选项启动Server，则它不会读取授权表或实现任何访问控制。任何人都可以连接并做任何事情，这是不安全的。


<br/>
<br/>


### 连接MySQL的一些问题

Troubleshooting Problems Connecting to MySQL


链接: <https://dev.mysql.com/doc/refman/5.7/en/problems-connecting.html>



<br/>
<br/>
<br/>



## 用户账户管理

MySQL User Account Management


本节介绍如何为MySQL Server的客户端设置账户：

- MySQL中使用的账户名和密码的含义，与操作系统使用的名称和密码的比较
- 如何设置新账户和删除现有账户
- 如何修改密码
- 密码使用安全指南


<br/>


### 用户名和密码

User Names and Passwords


有两种方式创建MySQL账户：

- 使用创建账户(`CREATE USER`)和建立权限(`GRANT`)的账户管理语句。这些语句使Server对基础授权表进行适当的修改
- 直接操作MySQL授权表，如`INSERT, DELETE, UPDATE`命令

推荐使用账户管理语句，因为它们比直接操作授权表更简洁，更不容易出错。

<br>

栗子：

```sh
mysql --user=root mysql
```

创建账户：

```sql
CREATE USER 'finley'@'localhost' IDENTIFIED BY 'passwd';
GRANT ALL PRIVILEGES ON *.* TO 'finley'@'localhost' WITH GRANT OPTION;

CREATE USER 'finley'@'%' IDENTIFIED BY 'passwd';
GRANT ALL PRIVILEGES ON *.* TO 'finley'@'%' WITH GRANT OPTION;

CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';
GRANT RELOAD,PROCESS ON *.* TO 'admin'@'localhost';


#查看
SHOW GRANTS FOR 'admin'@'localhost';


#特定权限
CREATE USER 'reader'@'localhost' INDENTIFIED BY 'passwd';
GRANT SELECT ON readdb.* TO 'reader'@'localhost';
```



<br/>
<br/>



### 删除账户

Removing User Accounts


使用`DROP USER`语句删除用户账户。

```sql
DROP USER 'reader'@'localhost';
```



<br/>
<br/>



### 保留账户

Reserved User Accounts


MySQL安装过程中的数据目录初始化期间，MySQL会创建应被视为保留的用户账户：

- `'root'@'localhost'`： 用于管理，拥有一切权限，可执行任何操作
- `'mysql.sys'@'localhost'`： 用作sys模式对象的DEFINER。可避免DBA重命名或删除root账户时出现的问题
- `'mysql.session@'localhost'`：由插件在内部使用以访问Server



<br/>
<br/>



### 账户资源限制

Setting Account Resource Limits


限制Client使用MySQL Server资源的一种方式是将全局`max_user_connections`系统变量设置为非零值。这限制了任何账户(缺乏单个用户)可以进行同时连接的数量，但是对连接后Client可以执行的操作没有限制。

为了解决这些问题，MySQL允许限制单个账户的资源：

- 账户每小时可以发出的查询数
- 账户每小时可以发出的更新数
- 账户每小时可以连接到Server的次数
- 账户与Server同时连接的数量

要在创建账户时设置资源限制，使用`CREATE USER`语句；要修改现有账户的限制，使用`ALTER USER`语句。使用`WITH`字句，命名每个要限制的资源。
每个限制的默认值为零，即无限制。
限制类型不必全部在`WITH`字句中命名，每个小时的限制值应该是一个整数。

Server在该账户对应的user表的行中存储账户的资源限制。当任何账户对其使用任何资源具有非零限制时，将进行资源使用计数。如果超过其连接次数，则Server会拒绝该账户的其它连接，直到该小时结束为止。在所有这些情况下，Server都会发出相应的错误消息。

```sql
CREATE USER 'francis'@'localhost' IDENTIFIED BY 'frank'
    WITH MAX_QUERIES_PER_HOUR 20
         MAX_UPDATES_PER_HOUR 10
         MAX_CONNECTIONS_PER_HOUR 5
         MAX_USER_CONNECTIONS 2;


ALTER USER 'francis'@'localhost' WITH MAX_QUERIES_PER_HOUR 100;
```

<br>

假设全局变量`max_user_connections`值为10：

```sql
ALTER USER 'user1'@'localhost' WITH MAX_USER_CONNECTIONS 0;
ALTER USER 'user2'@'localhost' WITH MAX_USER_CONNECTIONS 5;
ALTER USER 'user3'@'localhost' WITH MAX_USER_CONNECTIONS 20;
```

<br>


可以为单个账户、所有账户全局重置当前的计数：

- 使用`FLUSH USER RESOURCES`语句，将所有账户当前的计数重置为零
- 将单个账户的限制值重新设置，可以将账户的计数重置为零

每小时计数重置不会影响`MAX_USER_CONNECTIONS`限制。所有计数从零开始，计数不会通过Server重启而延续。



<br/>
<br/>
<br/>



### 分配账户密码

Assigning Account Passwords


MySQL会自动散列(hash)指定的密码。

```sql
#在创建用户是使用INDENTIFIED BY字句分配密码
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'password';


#修改密码
ALTER USER 'jeffrey'@'localhost' IDENTIFIED BY 'password';


#修改连接账户的密码
ALTER USER USER() IDENTIFIED BY 'password';
```

<br>

或者使用`mysqladmin`修改密码，出于安全问题，不推荐使用。

```sh
mysqladmin -u user_name -h host_name password "password"
```



<br/>
<br/>
<br/>



### 密码管理

Password Management


MySQL使数据库管理员可以手动使帐户密码过期，并建立自动密码过期的策略。可以在全局建立到期策略，并且可以将个人帐户设置为遵循全局策略或使用特定的每帐户行为覆盖全局策略。

使用`ALTER USER`语句设置密码过期：

```sql
ALTER USER 'jeffrey'@'localhost' PASSWORD EXPIRE;
```

密码过期策略是自动的，并且基于密码的年龄和最近密码的更改日期和时间进行评估。`mysql.user`表上有上次更改密码的时间。
要全局地建立自动密码过期策略，请使用`default_password_lifetime`系统变量。默认值为零，表示禁用自动密码过期。如果将值设置为正整数N，则表示允许的密码生存期，因此密码必须每N天更改一次。

栗子：

```
vim /etc/my.cnf


[mysqld]
default_password_lifetime=365
```

或者在MySQL中设置全局变量：

```sql
SET GLOBAL default_password_lifetime = 365;
```

或者在创建账户时设置：

```sql
CREATE USER 'jeffrey'@'localhost' PASSWORD EXPIRE INTERVAL 90 DAY;
ALTER USER 'jeffrey'@'localhost' PASSWORD EXPIRE INTERVAL 365 DAY;


#禁用
CREATE USER 'jeffrey'@'localhost' PASSWORD EXPIRE NEVER;
ALTER USER 'jeffrey'@'localhost' PASSWORD EXPIRE NEVER;


#默认
CREATE USER 'jeffrey'@'localhost' PASSWORD EXPIRE DEFAULT;
ALTER USER 'jeffrey'@'localhost' PASSWORD EXPIRE DEFAULT;
```

<br>

Client成功连接后，Server将确定账户密码是否已过期：

- Server检查密码是否手动过期
- 否则，Server根据自动密码过期策略检查密码年龄是否大于其允许的生存期



<br/>
<br/>
<br/>



### 密码过期和沙箱

Password Expiration and Sandbox Mode


对于使用具有过期密码的账户的连接，Server要么断开连接，要么将Client限制为沙箱模式。

沙箱模式允许这些操作：

- Client可使用`ALTER USER`或`SET PASSWORD`重置账户密码。重置密码后，Server恢复回话的正常访问



<br/>
<br/>
<br/>



### 代理用户

Proxy Users


MySQL Server使用身份验证插件验证客户端连接。验证给定连接的插件可以请求将外部连接用户视为不同的用户以进行特权检查。这就使外部用户成为第二个用户的代理。也就是说，假设第二个用户的权限：

- 外部用户是**代理用户**
- 第二个用户是**被代理的用户**


<br>


#### 代理用户支持的要求

Requirements for Proxy User Support


对于给定身份认证的插件的代理，必须满足一下条件：

- 必须通过插件本身或代表插件的MySQL Server服务器来支持代理
- 必须将代理用户账户设置为由插件进行身份验证(`CREATE USER`和`ALTER USER`语句)
- 必须创建代理用户账户并授予相关权限(`CREATE USER`和`GRANT`语句)
- 代理用户账户必须具有代理账户的proxy权限(`GRANT`语句)
- 对于连接到代理账户的Client将被视为代理用户，身份验证插件必须返回与客户端用户名不同的用户名，以指示代理账户的用户名，该用户名定义代理所承担的权限用户。

<br>

代理机制允许将客户端用户名映射到代理用户名，没有规定映射主机名。当连接Client与代理账户匹配时，Server会尝试使用身份验证插件返回的用户名和代理账户的主机名来查找账户的匹配项。

考虑如下账户定义：

```sql
-- create proxy account
CREATE USER 'employee_ext'@'localhost'
  IDENTIFIED WITH my_auth_plugin AS 'my_auth_string';

-- create proxied account and grant its privileges
CREATE USER 'employee'@'localhost'
  IDENTIFIED BY 'employee_pass';
GRANT ALL ON employees.*
  TO 'employee'@'localhost';

-- grant PROXY privilege to proxy account for proxied account
GRANT PROXY
  ON 'employee'@'localhost'
  TO 'employee_ext'@'localhost';
```

当Client从localhost使用`employee_ext`连接时，MySQL使用名为`my_auth_plugin`的插件来执行身份验证。假设`my_auth_plugin`根据`my_auth_string`的内容向Server返回`employee`的用户名，并可能通过咨询某些外部身份验证系统。`employee`与`employee_ext`不同，因此返回`employee`作为请求，Server将`employee_ext`视为`employee`本地用户，以便进行权限检查。

Server通过检查`employee_ext`是否具有`employee`的`PROXY`权限来验证`employee_ext`是否可以对`employee`进行代理身份验证。

在此例中，`employee_ext`是代理用户，`employee`是被代理用户。

发生代理时，`USER()`和`CURRENT_USER()`函数可用于查看连接用户(代理用户)与当前会话账户(被代理的用户)之间的区别：

```sql
SELECT USER(), CURRENT_USER();
+------------------------+--------------------+
| USER()                 | CURRENT_USER()     |
+------------------------+--------------------+
| employee_ext@localhost | employee@localhost |
+------------------------+--------------------+
```



<br/>
<br/>



#### 授予代理权限

Granting the Proxy Privilege


需要`PROXY`权限才能使外部用户连接并拥有其他用户的权限。要授予此权限，请使用`GRANT`语句。

```sql
GRANT PROXY ON 'proxied_user' TO 'proxy_user';


GRANT PROXY ON 'a' TO 'b', 'c', 'd';
GRANT PROXY ON 'a' TO 'd' WITH GRANT OPTION;
GRANT PROXY ON 'a' TO ''@'';
REVOKE PROXY ON 'a' FROM 'b', 'c', 'd';
```


<br/>
<br/>



#### 默认代理用户

Default Proxy Users


要制定某些或所有用户应使用给定的身份验证进行连接，请创建一个空白MySQL账户(`''@''`)，将其与该插件关联，然后让插件返回真实身份验证的用户名。

栗子：

```sql
-- create default proxy account
CREATE USER ''@'' IDENTIFIED WITH ldap_auth AS 'O=Oracle, OU=MySQL';

-- create proxied accounts
CREATE USER 'developer'@'localhost' IDENTIFIED BY 'developer_pass';
CREATE USER 'manager'@'localhost' IDENTIFIED BY 'manager_pass';

-- grant PROXY privilege to default proxy account for proxied accounts
GRANT PROXY ON 'manager'@'localhost' TO ''@'';
GRANT PROXY ON 'developer'@'localhost' TO ''@'';
```


<br/>
<br/>


#### 默认代理用户和匿名用户冲突

Default Proxy User and Anonymous User Conflicts


如果打算创建默认代理用户，请检查其他现有的**匹配任何用户**账户，这些账户优先于默认代理用户，因为他们可以阻止该用户按预期工作。

在前面的讨论中，默认代理账户在主机部分具有`''`，与任何主机匹配。如果你设置了默认代理用户，请注意检查非代理账户是否存在相同的用户部分和主机部分中的`%`，因为`%`也匹配任何主机，但优先于`''`，Server用于在内部对账户进行排序。

要避免此问题，请使用以下策略之一：

- 删除匿名用户，使其不与默认代理用户冲突
- 使用在匿名用户之前匹配的更具体的默认代理用户，如`''@localhost`
- 创建多个代理用户。用于本地连接和远程连接


<br/>
<br/>


#### Server支持代理用户映射

Server Support for Proxy User Mapping


一些身份验证插件为自身实现代理用户映射(如PAM)。默认情况下，其他身份验证插件不支持代理用户。
如果启用了`check_proxy_users`系统变量，则Server会对发出此类请求的任何身份认证插件执行代理用户映射：

- 默认情况下，`check_proxy_users`被禁用。因此即使对请求Server支持代理用户的身份验证插件，Server也不执行用户代理映射。
- 如果启用了`check_proxy_users`，则可能还需要启用特定于插件的系统变量以利用Server代理用户映射支持：
	+ 对于`mysql_native_password`插件，请启用`mysql_native_proxy_users`
	+ 对于`sha256_password`插件，请启用`sha256_password_proxy_users`

Server执行的代理用户映射受以下限制：

- 即使授权了关联的`PROXY`权限，Server也不会代理匿名用户(FROM)或从匿名用户代理(TO)
- 如果单个账户授予了多个代理账户的代理权限，则Server代理用户是不确定的。因此，不鼓励为多个被代理账户授予单个账户代理权限


<br/>
<br/>


#### 代理用户系统变量

Proxy User System Variables


两个系统变量有助于追踪代理登录过程：

- `proxy_user`
如果未使用代理，则此值为`NULL`。否则，它表示代理用户账户。

- `external_user`
有时，身份验证插件可能会使用外部用户对MySQL Server进行身份验证。



<br/>
<br/>
<br/>



### 用户账户

User Account Locking


从MySQL v5.7.6开始，MySQL支持使用`ACCOUNT LOCK`和`ACCOUNT UNLOCK`子句为`CREATE USER`和`ALTER USER`语句锁定和解锁用户账户：

- 与`CREATE USER`一起使用时，这些子句指定新账户的初始锁定状态。如果没有任何一个子句，则账户将以未锁定状态创建。
- 与`ALTER USER`一起使用时，这些子句指定现有账户的新锁定状态。如果没有任何一个子句，则账户锁定状态保持不变。

<br>

账户锁定状态记录在`mysql.user`系统表的`account_locked`列中。使用`SHOW CREATE USER`显示账户锁定状态。

如果Client尝试连接到已锁定的账户，则会失败。返回错误消息，并将错误写入日志。

锁定账户不会影响能够使用承担锁定账户身份的代理用户进行连接。她也不会影响执行存储程序和试图的能力，这些程序或视图具有命名锁定账户的`DEFINER`子句。也就是说，锁定账户，不会影响使用代理账户或存储的程序或视图的能力。

要从旧版本升级到MySQL 5.7.6以及更高版本，请运行`mysql_upgrade`以确保此列存在。对于没有`account_locked`列的非升级安装，Server会将所有账户视为已解锁。



<br/>
<br/>



### 基于SQL的MySQL账户活动审计

SQL-Based MySQL Account Activity Auditing


应用程序可以使用以下准则来执行基于SQL的审计，该审计将数据库活动与MySQL账户联系起来。

MySQL账户对应于`mysql.user`系统表中的行。当Client连接成功后，Server会将Client验证到此表中的特定行。此行中的`User`和`Host`列的值唯一标识该账号，并对应于`user@host`格式，其中账户名称在SQL语句中写入。

用于验证Client的账户确定Client具有哪些权限。通常，可以调用`CURRENT_USER()`函数来确定这对于Client用户来说是哪个账户。其值有账户的用户表行的`User`和`Host`列构成。
但是，在某些情况下，`CURRENT_USER()`值不对应于客户端用户，而是对英语不同的账户。当权限检查不基于客户端账户时，会发生这种情况：

- 使用 SQL SECURITY DEFINER特性定义的存储例程
- 使用 SQL SECURITY DEFINER特性定义的视图
- 触发器和事件

在这些上下文中，权限检查是针对DEFINER账户完成的，而`CURRENT_USER()`是指该账户，而不是指调用存储例程或视图的Client或导致触发器激活的账户。

如果应用程序必须调用`USER()`进行用户审计，但是还必须能够将`USER()`值与用户表中的账户相关联，则必须避免使用在`User`或`Host`列中包含通配符。具体来说，不允许User为空，并且不允许Host值中使用模式字符或网络掩码表示法。所有账户必须具有非空用户值和文字主机值。

更改账户用户主机：

```sql
RENAME USER ''@'localhost' TO 'user1'@'localhost';

RENAME USER 'user2'@'%.example.com' TO 'user2'@'remote.example.com';
-- 如果user2必须能够从example.com域中的多个主机进行连接，则每个主机应该有一个独立的账户
```

<br>

要从`CURRENT_USER()`或`USER()`函数中提取用户名或主机名，请使用`SUBSTRING_INDEX()`函数：

```sql
SELECT SUBSTRING_INDEX(CURRENT_USER(),'@',1);
+---------------------------------------+
| SUBSTRING_INDEX(CURRENT_USER(),'@',1) |
+---------------------------------------+
| user1                                 |
+---------------------------------------+


SELECT SUBSTRING_INDEX(CURRENT_USER(),'@',-1);
+----------------------------------------+
| SUBSTRING_INDEX(CURRENT_USER(),'@',-1) |
+----------------------------------------+
| localhost                              |
+----------------------------------------+
```




<br/>
<br/>
<br/>




## 使用加密连接

Using Encrypted Connections


MySQL Client和Server之间的未加密连接，有权访问网络的人可以监视你的所有流量和C/S之间发送和接受的数据。
要使任何类型的在网络中数据不可读，请使用加密。加密算法必须包含安全元素，以抵御多种已知的攻击。

MySQL支持使用TLS协议在C/S之间建立加密连接。TLS有时被称为SSL，但MySQL实际上并不使用SSL协议进行加密，因为它的加密很弱。
TLS使用加密算法来确保可以信任通过公共网络接收的数据。它具有检测数据**更改、丢失、重放**的机制。TLS还包含使用`X.509`标准提供的身份验证的算法。
`X.509`可以识别互联网上的某个人。在基本术语中，有一些被称为**证书颁发机构(CA)**的实体，它将电子证书分配给需要它们的任何人。证书依赖于两个加密密钥(公钥和私钥)的非对称加密算法。证书所有者可以将证书提供给另一方作为身份证明。证书由其所有者的公钥组成，使用该公钥加密的任何数据只能使用有该证书的私钥来解密。

可以使用OpenSSL和yaSSL编译MySQL以获得加密连接支持。
默认情况下，如果Server支持加密连接，MySQL将尝试使用加密连接。如果无法建立加密连接，则会回退到未加密的连接。
MySQL基于每个连接执行加密，并且对给定用户使用加密可以是可选的或强制的。这使你可以根据各个应用程序的要求选择加密或未加密的连接。

加密连接同样可用于Master和Slave的副本集之间。
也可以通过MySQL C API获得加密连接。
也可以使用SSH连接内的加密连接到MySQL Server。


<br/>
<br/>


### 配置MySQL以使用加密连接

Configuring MySQL to Use Encrypted Connections


有几个选项用于指示是否使用加密连接，以及制定适当的证书和密钥文件。它包括：

- Server端
- Client端


<br/>
<br/>


#### S端加密连接配置

Server-Side Configuration for Encrypted Connections


在S端，`--ssl`选项指定Server允许但不需要加密连接。默认情况下启用此选项。
Server端的这些选项标识了Server在允许Client建立加密连接时使用的证书和密钥文件：

- `--ssl-ca`
CA颁发的证书文件的路径名

- `ssl-cert`
Server公钥证书文件的路径名。可以发送到Client端，并根据它具有的CA证书进行身份验证

- `ssl-key`
Server私钥文件的路径名

启用加密连接，修改`my.cnf`的栗子：

```
[mysqld]
ssl-ca=ca.pem
ssl-cert=server-cert.pem
ssl-key=server-key.pem
```


<br/>
<br/>


#### C端加密连接配置

Client-Side Configuration for Encrypted Connections


默认情况下，如果Server支持加密连接，MySQL Client将尝试建立加密连接，并通过`--SSL-mode`选项进一步控制：

- 如果没有`ssl-mode`选项
Client将尝试使用加密连接，如果无法建立加密连接，则会回退到未加密的连接。这等同于`--ssl-mode=PREFFERED`

- `--ssl-mode=REQUIRED`
Client需要加密连接，如果无法建立，则会失败

- `--ssl-mode=DISABLED`
Client使用未加密连接

- `--ssl-mode=VERIFY_CA`或`--ssl-mode=VERIFY_IDENTITY`
客户端需要加密连接，并且还要针对Server CA证书和对其他证书中的Server主机名进行验证

<br>

Client以下几个选项类似于Server端的几个选项，标识C/S加密连接时使用的证书和密钥文件：

- `--ssl-ca`
- `--ssl-cert`
- `--ssl-key`



<br/>
<br/>
<br/>



### 加密连接的命名选项

Command Options for Encrypted Connections


本节介绍使用加密连接的选项。

```
--skip-ssl	Do not use encrypted connection	
--ssl	Enable encrypted connection	
--ssl-ca	File that contains list of trusted SSL Certificate Authorities	
--ssl-capath	Directory that contains trusted SSL Certificate Authority certificate files	
--ssl-cert	File that contains X.509 certificate	
--ssl-cipher	List of permitted ciphers for connection encryption	
--ssl-crl	File that contains certificate revocation lists	
--ssl-crlpath	Directory that contains certificate revocation list files	
--ssl-key	File that contains X.509 key	
--ssl-mode	Security state of connection to server	5.7.11
--ssl-verify-server-cert	Verify host name against server certificate Common Name identity	
--tls-version	Protocols permitted for encrypted connections	5.7.10
```



<br/>
<br/>
<br/>



### 创建SSL/RSA证书和密钥

Creating SSL and RSA Certificates and Keys


可以使用MySQL自身提供的工具或直接调用`openssl`命令来创建所需文件。


<br/>
<br/>


#### 使用MySQL创建

Creating SSL and RSA Certificates and Keys using MySQL


MySQL提供了这些方法来创建**SSL证书和密钥文件**以及使用SSL支持加密连接所需的**RSA密钥对文件**，以及使用RSA通过未加密连接进行安全密码交换。如果缺少这些文件：

- 对于使用OpenSSL编译的MySQL发行版，Server可在启动时自动生成这些文件
- 用户可以手动调用`mysql_ssl_rsa_setup`实用程序
- 对于某些发行版(如RPM包)，在数据目录初始化期间会调用`mysql_ssl_rsa_setup`。在这种情况下，只要`openssl`命令可用，就不需要使用OpenSSL编译MySQL发行版


<br>
<br>


**自动SSL和RSA文件生成**

Automatic SSL and RSA File Generation

对于使用OpenSSL编译的MySQL发行版，MySQL Server能够在启动时自动生成缺少的SSL和RSA文件。`auto_generate_certs`和`sha256_password_auto_generate_rsa_keys`系统变量控制这些文件的自动生成。默认情况下启用这两个变量，它们可以在启动时启用并检查，但不能在运行时设置。

启动时，如果启用了`auto_generate_certs`系统变量，则Server会自动在数据目录中生成S端和C端的SSL证书和密钥文件。

- Server检查数据目录下的SSL文件：

```
ca.pem
server-cert.pem
server-key.pem
```

- 如果存在，则不创建。反之，则创建

```
ca.pem               Self-signed CA certificate
ca-key.pem           CA private key
server-cert.pem      Server certificate
server-key.pem       Server private key
client-cert.pem      Client certificate
client-key.pem       Client private key
```

- 如果Server自动生成了RSA文件，它将使用其名称来设置相应的系统变量

<br>

启动时，如果满足以下条件(`sha256_password_auto_generate_rsa_keys`系统变量已启用；没有指定RSA选项；数据目录中缺少RSA文件)，则Server会自动在数据目录中生成RSA私钥/公钥对文件。

- Server检查数据目录下的RSA文件

```
private_key.pem      Private member of private/public key pair
public_key.pem       Public member of private/public key pair
```

- 如果存在，则不创建。反之，则创建
- 如果Server自动生成了RSA文件，它将使用其名称来设置相应的系统变量


<br>
<br>

**手动生成**

Manual SSL and RSA File Generation Using mysql_ssl_rsa_setup

MySQL发行版包含此实用程序，但它需要`openssl`命令可用。


<br>
<br>

**SSL/RSA文件特性**

SSL and RSA File Characteristics

它们具有以下特性：

- SSL/RSA密钥大小为2048bits
- SSL CA证书是自签名的
- SSL Server/Client的CA证书和密钥对，使用`sha256WithRSAEncryption`签名算法
- 创建的SSL文件自生成之日起有效期为十年
- RSA文件不会过期
- SSL文件对于每个证书/密钥对具有不同的序列号(1 for CA, 2 for Server, 3 for Client)
- 创建的文件由运行程序执行创建的用户拥有
- Unix/Unix-Like上，证书文件权限为644，密钥文件权限为600

<br>

查看SSL证书内容：

```
openssl x509 -text -in ca.pem
openssl x509 -text -in server-cert.pem
openssl x509 -text -in client-cert.pem
```

使用SQL语句查看SSL证书过期时间：

```sql
SHOW STATUS LIKE 'Ssl_server_not%';
+-----------------------+--------------------------+
| Variable_name         | Value                    |
+-----------------------+--------------------------+
| Ssl_server_not_after  | Apr 28 14:16:39 2027 GMT |
| Ssl_server_not_before | May  1 14:16:39 2017 GMT |
+-----------------------+--------------------------+
```


<br/>
<br/>


#### 使用openssl创建SSL证书和密钥

Creating SSL Certificates and Keys Using openssl


创建栗子：

```
# Create clean environment
rm -rf newcerts
mkdir newcerts && cd newcerts

# Create CA certificate
openssl genrsa 2048 > ca-key.pem
openssl req -new -x509 -nodes -days 3600 \
        -key ca-key.pem -out ca.pem

# Create server certificate, remove passphrase, and sign it
# server-cert.pem = public key, server-key.pem = private key
openssl req -newkey rsa:2048 -days 3600 \
        -nodes -keyout server-key.pem -out server-req.pem
openssl rsa -in server-key.pem -out server-key.pem
openssl x509 -req -in server-req.pem -days 3600 \
        -CA ca.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem

# Create client certificate, remove passphrase, and sign it
# client-cert.pem = public key, client-key.pem = private key
openssl req -newkey rsa:2048 -days 3600 \
        -nodes -keyout client-key.pem -out client-req.pem
openssl rsa -in client-key.pem -out client-key.pem
openssl x509 -req -in client-req.pem -days 3600 \
        -CA ca.pem -CAkey ca-key.pem -set_serial 01 -out client-cert.pem
```

验证：

```
openssl verify -CAfile ca.pem server-cert.pem client-cert.pem


```


<br/>
<br/>


#### 使用openssl创建RSA密钥

Creating RSA Keys Using openssl

创建：

```sh
openssl genrsa -out private_key.pem 2048
openssl rsa -in private_key.pem -pubout -out public_key.pem

chmod 400 private_key.pem
chmod 444 public_key.pem
```




<br/>
<br/>
<br/>




## 安全插件

Security Plugins


MySQL包括几个实现安全功能的插件：

- 用于MySQL C/S 连接尝试的插件
- 用于实施密码强度策略和评估潜在密码强度的密码验证插件
- 用于敏感信息安全存储的密钥环(keyring)插件
- MySQL企业版插件

<br>


### 身份认证插件

Authentication Plugins

<br>


#### 原生可插拔认证

Native Pluggable Authentication

MySQL包含两个实现原生身份认证的插件。在引入可插拔身份验证之前，基于密码散列的方法的身份验证。

本级密码验证的插件和库名称：

| Plugin or File | Plugin or File Name |
| - | - |
| Server-side plugin | mysql_native_password |
| Client-side plugin | mysql_native_password |
| Library file	| None (plugins are built in) |

<br>

**安装本机可插拔认证**
Installing Native Pluggable Authentication

`mysql_native_password`插件存在于Server和Client的表单中：

- S端插件内置于Server，无需显式加载，也无法通过卸载来禁用
- C端插件内置于`libmysqlclient`库中，可用于链接到此库的任何程序

<br>

**使用本机可插拔认证**
Using Native Pluggable Authentication

MySQL Client程序默认使用`mysql_native_password`。



<br/>
<br/>



#### 旧的本机可插拔认证

Old Native Pluggable Authentication

MySQL version 4.1机之前。


<br/>
<br/>


#### 从旧的插件迁移到新的插件

Migrating Away from Pre-4.1 Password Hashing and the mysql_old_password Plugin


<br/>
<br/>


#### SHA-256可插拔认证

SHA-256 Pluggable Authentication


















<br/>
<br/>

---

<br/>
<br/>








# 备份和恢复

Backup and Recovery


**数据备份非常重要！**

MySQL提供了各种备份策略，你可以从中选择最适合安装要求的方法。本章讨论几个你应该熟悉的备份和恢复的主题：

- 备份类型： 逻辑与物理，全量和增量...
- 创建备份的方法
- 恢复方法，包括时间点(point-in-time)恢复
- 备份调度，压缩和加密
- 表维护，以便恢复损坏的表


<br/>


## 备份和恢复类型

Backup and Recovery Types


<br/>


### 物理与逻辑备份

Physical (Raw) Versus Logical Backups


- 物理备份由目录的原始副本和存储数据库内容的文件组成。
此类备份适用于需要在出现问题时快速恢复的大型重要数据库。

- 逻辑备份保存表示为逻辑数据库结构的信息(`CREATE DATABASE`, `CREATE TABLE`语句)和内容(`INSERT`语句和分割文本文件)。
此类备份适用于较少量的数据，你可以在其中编辑数据值或表结构，或在不同计算机体系结构上重新创建数据。

<br>

**物理备份方法具有以下特征：**

- 此备份包含数据库目录和文件的精确副本。通常，这是MySQL数据目录的全部或部分副本
- 物理备份比逻辑备份更快，因为它们只涉及文件复制而不进行转换
- 输出比逻辑备份更紧凑(compact)
- 由于备份速度和紧凑性对于繁忙、重要的数据库很重要，因此MySQL Enterprise执行物理备份
- 备份和还原粒度范围从整个数据目录的级别到单个文件的级别。这可能会/也可能不会提供表级别的粒度，具体取决于存储引擎
- 除数据库外，此备份还可以包括任何相关文件(如日志和配置)
- 来自MEMORY表的数据很难以这种方式备份，因为它们的内容不存储在磁盘上
- 备份仅可移植到具有相同或类似硬件特征的其它计算机
- 可以在MySQL Server未运行时执行备份。如果Server正在运行，则必须执行适当锁定(lock)，以便Server在备份期间不会更改数据库内容。MySQL Enterprise备份会自动为它需要的表执行锁定
- 物理备份工具包括用于`InnoDB`或任何其它表的`mysqlbackup`，或用于`MyISAM`表的文件系统级别命令(`cp`, `scp`, `tar`, `rsync`)
- 对于恢复
	+ MySQL Enterprise备份恢复`InnoDB`和备份的其它表
	+ `ndb_restore`恢复NDB表
	+ 可使用文件系统级别的命令将文件复制回其原始位置

<br>

**逻辑备份方法具有以下特性：**

- 通过查询MySQL Server来获取数据库结构和内容信心来完成备份
- 此备份比物理备份慢，因为Server必须访问数据库信息并将其转换为逻辑格式
- 输出大于物理备份，特别是以文本格式保存时
- 备份和还原可在Server级别，数据库级别或表级别，而无论存储引擎如何
- 备份不包括日志和配置文件，或其它不属于数据库的相关文件
- 以逻辑格式的备份与机器无关，请具有高度可移植性
- 在MySQL Server运行时执行逻辑备份
- 逻辑备份工具包括`mysqldump`程序和`SELECT ... INTO OUTFILE`语句。这适用于任何存储引擎，甚至是MEMORY
- 要恢复逻辑备份，可使用`mysql`客户端处理SQL格式转储文件。要加载分割文本文件，使用`LOAD DATA INFILE`语句或`mysqlimport`客户端



<br/>
<br/>


### 在线与离线备份

Online Versus Offline Backups


在MySQL Server运行时进行在线备份，以便可以从Server获取数据库信息。Server停止时进行离线备份。
这种区别也可描述为**热备(hot)**与**冷备(cold)**，热备是Server保持允许但在外部访问数据库时锁定表以防止修改数据

**在线备份具有以下特性：**

- 此备份对其它客户端的干扰较小，其它客户端可在备份期间连接到MySQL Server，并且可以根据需要执行的操作来访问数据库
- 必须小心施加适当的锁定，以便不会发生损害备份完整性的数据修改。MySQL Enterprise会自动执行锁表

<br>

**脱机备份方法有以下特性：**

- 客户端可能会受到不利影响，因为备份期间Server不可用。因此，此类备份通常发生于Slave Server，可以脱机而不会损害可用性
- 备份过程简单，因为不会受到客户端活动的干扰


在线和离线之间的区别适用于恢复操作，并且适用于类似的特征。但是，由于恢复需要更强的锁定，因此客户端更有可能受到在线恢复的影响而不是在线备份。在备份期间，客户端可能能够在备份时读取数据；而恢复数据不仅仅是读取数据，因此必须防止客户端在恢复数据时访问数据。



<br/>
<br/>



### 本地与远程备份

Local Versus Remote Backups


本地备份是在运行MySQL Server的统一主机上执行，而远程备份则从其它主机执行。

- `mysqldump`能连接到本地或远程Server。对于SQL输出，既可在本地也可在远程；对于分割文件输出，将在Server上创建数据文件
- `SELECT ... INTO OUTFILE`可从本地或远程启动，但输出文件是在Server上创建
- 物理备份通常在Server上启动，以便Server可以脱机



<br/>
<br/>



### 快照备份

Snapshot Backups


某些文件系统可以实现快照，它们在给定时间点提供文件系统的逻辑副本，而不需要整个文件系统的物理副本。MySQL本身并未提供此功能，可通过`Veritas`, `LVM`或`ZFS`...



<br/>
<br/>



### 全量与增量备份

Full Versus Incremental Backups


全量备份包括MySQL Server在给定时间点管理的所有数据；增量备份包括在给定时间点跨度内（从一个时间点到另一个时间点）对数据所做的更改。



<br/>
<br/>



### 全量与增量恢复

Full Versus Point-in-Time (Incremental) Recovery


全量恢复可从完整备份恢复所有数据。这会将Server实例还原到备份时的状态。
增量恢复是恢复在给定时间点跨度内所做的更改，这也称为时间点恢复。它基于二进制日志，通常在备份文件完全恢复之后，将备份文件还原到备份时的状态。然后，在二进制日志文件中写入的数据更改将作为增量恢复应用于重做数据修改，并使Server达到所需的时间点。



<br/>
<br/>



### 表维护

Table Maintenance


如果表损坏，数据完整性可能会受到影响。



<br/>
<br/>



### 备份调度，压缩和加密

Backup Scheduling, Compression, and Encryption


备份调度对于自动化备份过程很有价值；压缩备份输出可减少空间需求；输出加密可提供更好的安全性，防止未经授权访问备份数据。
MySQL本身不提供这些功能，可使用第三方方案。




<br/>
<br/>
<br/>




## 数据库备份方法

Database Backup Methods


<br/>


### 使用MySQL Enterprise进行热备



<br/>
<br/>



### 使用mysqldump进行备份

Making Backups with mysqldump


`mysqldump`程序可以进行备份，它可以备份各种表。



<br/>
<br/>



### 通过复制表文件进行备份

Making Backups by Copying Table Files


对于使用自己的文件表示每个表的存储引擎，可通过复制这些文件来备份表。要获得一致性的备份，请停止服务器或锁定并刷新相关表：

```sql
FLUSH TABLES tbl_list WITH READ LOCK;

```

你只需要一个读锁，这使得其它客户端可以在你复制数据库目录中的文件时继续查询表。需要刷新以确保在开始备份之前将所有活动索引页写入磁盘。
只要Server没有更新，你也可以通过复制所有表文件来创建二进制备份。



<br/>
<br/>



### 通过分隔文本文件备份

Making Delimited-Text File Backups


要创建包含表数据的分隔文本文件，可使用`SELECT * INTO OUTFILE ‘filename’ FROM tablename`语句进行创建。
此方法适用于任何类型的数据文件，但仅保存表数据，而不保存表结构。

要载入分隔文本文件，请使用`LOAD DATA INFILE`或`mysqlimport`。



<br/>
<br/>



### 通过启用二进制日志进行增量备份

Making Incremental Backups by Enabling the Binary Log


MySQL支持增量备份，必须使用`--log-bin`选项启动Server以支持二进制日志记录。
二进制日志文件为你提供了在执行备份之后副本数据库所需的信息。目前，你希望进行增量备份，你应该使用`FLUSH LOGS`轮换二进制日志。完成此操作后，你需要将所有二进制日志复制到备份位置，这些日志的范围从上次完全备份或增量备份到最后一个备份之一。



<br/>
<br/>



### 通过使用副本集Slaves进行备份

Making Backups Using Replication Slaves


如果在进行备份时Master Server出现性能问题，可在Slave Server上进行复制和备份。



<br/>
<br/>



### 恢复损坏的表

Recovering Corrupt Tables


如果必须还原已损坏的`MyISAM`表，请尝试首先使用`REPAIR TABLE`或`myisamchk -r`恢复它们。这应该在99.9％的情况下有效。



<br/>
<br/>



### 使用文件系统快照进行备份

Making Backups Using a File System Snapshot


VXFS文件系统操作步骤，其它文件系统类似：

1. 客户端程序执行`FLUSH TABLES WITH READ LOCK`
2. 从shell执行`mount vxfs snapshot`
3. 解锁`UNLOCK TABLES`
4. 从快照复制文件
5. umount快照




<br/>
<br/>
<br/>




## 备份和恢复栗子

Example Backup and Recovery Strategy


请注意磁盘问题，万一是磁盘不可用那就...


<br/>


### 建立备份策略

Establishing a Backup Policy


为了有用，必须定期进行备份。可使用多种工具在MySQL中完成全量备份。

```sh
#备份之前锁表
mysqldump --single-transaction  --all-databases > bacup.sql
```

全量备份是必要的，但创建它们并不总是方便。生成大型备份文件要话费大量时间和空间，它并非最佳。所以，进行初始化全量备份，然后进行增量备份更有效。增量备份更小，时间更短。

要进行增量备份，需要保存增量更改。在MySQL中，这些更改在二进制日志中表示，因此应始终使用`--log-bin`选项启动MySQL Server已启动二进制日志。启用它之后，Server会在更新数据时将每个数据更改写入文件。
每次重启时，MySQL Server都会使用序列中的下一个数字创建创建一个新的二进制日志文件。在Server运行时，你还可以告诉它关闭当前的二进制日志文件并通过`FLUSH LOGS`语句或`mysqladmin flush-logs`命令手动开始新的二进制日志文件。`mysqldump`还有一个刷新日志的选项，数据目录中的`.index`文件包含目录中所有MySQL二进制日志的列表。

```
xxx-bin.000001
xxx-bin.000002
xxx-bin.000003
xxx-bin.000004
xxx-bin.index
```

MySQL二进制日志对于恢复非常重要，因为它们构成了一组增量备份。如果确保在进行全量备份时刷新日志，则之后创建的二进制日志文件将包含自备份以来所做的所有数据的更改。

```sh
mysqldump --single-transaction --flush-logs --master-data=2 \
         --all-databases > backup.sql
#之后便会创建一个新的二进制日志文件
```

MySQL二进制日志占用磁盘空间，可不时删除它们。

```sh
mysqldump --single-transaction --flush-logs --master-data=2 \
         --all-databases --delete-master-logs > backup.sql
#在副本集中，使用mysqldump --delete-master-logs删除MySQL二进制日志可能会很危险，因为可能Slave Server尚未处理完二进制日志的内容
#可使用 PURGE BINARY LOGS语句删除
```



<br/>
<br/>



### 使用备份进行恢复

Using Backups for Recovery


```sh
#全量恢复
mysql < backup.sql


#增量恢复
mysqlbinlog xxx-bin.000007 xxx-bin.000008 | mysql
```



<br/>
<br/>



### 备份策略摘要

Backup Strategy Summary


不怕一万，就怕万一。`InnoDB`本身可以完成恢复数据的所有工作。但为了确保高枕无忧，请遵守以下准则：

- 始终使用`--log-bin`选项运行MySQL Server
- 使用`mysqldump`定期进行全量备份
- 使用`FLUSH LOGS`或`mysqladmin flush-logs`刷新日志来定期进行增量备份




<br/>
<br/>
<br/>




## mysqldump备份

Using mysqldump for Backups


本节介绍如何使用`mysqldump`生产转储文件，以及如何重新加载转储文件。转储文件可通过多种方式使用：

- 作为备份，在数据丢失的情况下启用数据恢复
- 作为设置副本集的数据源
- 作为实验数据源

<br>

`mysqldump`处理两种类型的输出，具体取决于有无`-tab`选项：

- 没有`--tab`选项，`mysqldump`将SQL语句写入标准输出。
输出包含用于创建转储对象(db, table, sotred routines)的`CREATE`语句，以及用于将数据加载到表中的`INSERT`语句。输出可保存到文件中。

- 带有`--tab`选项，`mysqldump`为每个转储的表生成两个输出文件。
Server将一个文件写为制表符(tab)分隔的文本，每个表的行(row)为文本的一行，输出名为`table_name.txt`。Server还将创建表的`CREATE TABLE`语句发送到`mysqldump`，`mysqldump`将其写为输出目录中名为`table_name.sql`的文件。


<br/>


### 使用mysqldump以SQL格式转储数据

Dumping Data in SQL Format with mysqldump


```
mysqldump [args] > file_name


#all db
mysqldump --all-databases > dbs.sql


#specific db
mysql --databases test > testDB.sql
mysqldump test > testDB.sql

mysqldump --databases db1 db2 db3 > db123.sql
mysqldump db1 db2 db3 > db123.sql
```

关于有无`--databases`选项的区别：

- 转储输出不包含`CREATE DATABASE`或`USE`语句
- 重新加载转储文件时，必须指定默认数据库名称，以便Server知道要重新加载的数据库
- 对于重新加载，你可以指定与原始名称不同的数据库名，这使你可将数据重新加载到其它数据库中
- 如果要重载的数据库不存在，则必须先创建它
- 由于输出不包含`CREATE DATABASE`语句，因此`--add-drop-database`选项无效



<br/>
<br/>



### 只备份数据库表结构

`mysqldump`使用`-d(--no-data)`选项，可以只备份数据库中表结构，而不备份数据。

栗子：

```sh
#-d(--no-data)


#某个库中的所有表的结构
mysqldump -h xxx --port 3306 -u xxx -p  -d 数据库名 > 数据库名.sql
#之后再导入


#某个表的结构
mysqldump -h xxx -u xxx -p -d --databases=xxx --tables xxx > 表.sql
```




<br/>
<br/>


### 重载SQL格式备份

Reloading SQL-Format Backups


要重载由`mysqldump`备份的包含SQL语句的转储文件，使用`mysql`客户端输入。
如果使用了`--databases`选项，则它包含了`CREATE DATABASE`和`USE`语句，就没有必要指定默认数据库。

```sh
mysql < dump.sql
#或
mysql> source dump.sql


#未使用--databases选项
mysqladmin create db1
mysql db1 < dump.sql
```



<br/>
<br/>



### 使用mysqladmin以分隔文本格式转储数据

Dumping Data in Delimited-Text Format with mysqldump


本节介绍如何使用`mysqldump`创建分隔文本转储文件。

```
mysqldump --tab=/tmp db1
#db1.txt


#其它选项：
--fields-terminated-by=str
--fields-enclosed-by=char
--fields-optionally-enclosed-by=char
--fields-escaped-by=char
--lines-terminated-by=str


#栗子
mysqldump --tab=/tmp --fields-terminated-by=,
         --fields-enclosed-by='"' --lines-terminated-by=0x0d0a db1
```



<br/>
<br/>



### 重载分隔文本格式的备份

Reloading Delimited-Text Format Backups


```sh
mysql db1 < t1.sql

mysqlimport db1 t1.txt
```

```sql
USE db1;
LOAD DATA INFILE `t1.txt` INTO TABLE t1;
```



<br/>
<br/>



### mysqldump小技巧

本节介绍使用`mysqldump`解决特定问题的技术：

- 如何复制数据库
- 如何将数据库从一个Server复制到另一个Server
- 如何转储存储的程序
- 如何单独转储定义和数据


<br/>


#### 复制数据库

Making a Copy of a Database

```sh
mysqldump db1 > dump.sql
mysqladmin create db2
mysql db2 < dump.sql
```


<br/>


#### 将数据库从一个Server复制到另一个Server

Copy a Database from one Server to Another

```sh
#Server1
mysqldump --databases db1 > dump.sql


#Server2
mysql < dump.sql


#无--databases
mysqldump db1 > dump.sql
mysqladmin create db1
mysql db1 < dump.sql
```


<br/>


#### 转储存储的程序

Dumping Stored Programs

几个选项控制mysqldump如何处理存储的程序：

```sh
--events: Dump Event Scheduler events
--routines: Dump stored procedures and functions
--triggers: Dump triggers for tables


--skip-events
--skip-routines
--skip-triggers.
```


<br/>


#### 转储表定义和

Dumping Table Definitions and Content Separately

```sh
#--no-data，不转储表数据，导致转储文件只包含用于创建表的语句
#--no-create-info, 从输出中抑制CREATE语句，以便转储文件包含表数据

mysqldump --no-data test > dump-defs.sql
mysqldump --no-create-info test > dump-data.sql

mysqldump --no-data --routines --events test > dump-defs.sql
```


<br/>


#### 使用mysqldump测试升级不兼容性

Using mysqldump to Test for Upgrade Incompatibilities

```sh
#old
mysqldump --all-databases --no-data --routines --events > dump-defs.sql


#new
mysql < dump-defs.sql
```




<br/>
<br/>
<br/>




## 使用二进制日志进行增量恢复

Point-in-Time (Incremental) Recovery Using the Binary Log


时间点恢复是指恢复自**给定时间点**以来所做的数据更改。通常，在还原全量备份之后执行此类恢复。

时间点恢复基于以下原则：

- 时间点恢复的信息源是由全量备份操作之后生成的二进制日志文件表示增量备份集，请开启`--bin-log`选项
要从二进制日志还原数据，你必须知道二进制日志文件的名称和位置，默认情况下，它在数据目录中。

```sql
SHOW BINARY LOGS;


--确定当前binary log file名称
SHOW MASTER STATUS;
```

- `mysqlbinlog`实用程序将二进制日志文件中的事件从二进制格式转换为文本，以便可以执行或查看它们
`mysqlbinlog`具有根据日志中事件时间或事件位置选择二进制日志部分的选项。

- 从二进制日志执行事件会导致重做它们所代表的数据修改，这样可以恢复给定时间段内的数据更改。

```sh
#从二进制日志中执行事件
mysqlbinlog binlog_files | mysql -u root -p
```

- 当需要确定事件时间或位置以在执行事件之前选择部分日志内容时，查看日志内容很有用

```sh
#查看binary log
mysqlbinlog binlog_file | more

#或
mysqlbinlog binlog_file > tmpfile
```

- 将输出保存在文件中非常有用，可以在删除某些事件时执行日志内容

```sh
mysql -u root -p < tmpfile
```

- 如果要在MySQL Server上执行多个二进制日志，安全的方法是使用与Server的单个连接来处理它们

```sh
#unsafe
#可能导致某些问题
mysqlbinlog binlog.000001 | mysql -u root -p # DANGER!!
mysqlbinlog binlog.000002 | mysql -u root -p # DANGER!!


#safe
mysqlbinlog binlog.000001 binlog.000002 | mysql -u root -p


#或
mysqlbinlog --skip-gtids binlog.000001 >  /tmp/statements.sql
mysqlbinlog --skip-gtids binlog.000002 >> /tmp/statements.sql
mysql -u root -p -e "source /tmp/statements.sql"
```



<br/>
<br/>



### 使用事件时间进行时间点恢复

Point-in-Time Recovery Using Event Times


要指示恢复的开始和结束时间，请以`DATATIME`格式指定`mysqlbinlog`的`--start-datetime`和`--stop-datetime`选项。
请先查看binary log的时间区间。

```sh
#恢复数据直到停止时间
mysqlbinlog --stop-datetime="2005-04-20 9:59:59" \
         /var/log/mysql/bin.123456 | mysql -u root -p

#恢复数据从开始时间
mysqlbinlog --start-datetime="2005-04-20 10:01:00" \
         /var/log/mysql/bin.123456 | mysql -u root -p
```



<br/>
<br/>



### 使用事件位置进行时间点恢复

Point-in-Time Recovery Using Event Positions


`mysqlbinlog`的`--start-position`和`--stop-position`选项可用于指定日志位置，它的工作方式与指定时间类似。使用位置可以更准确地了解要恢复的日志部分，尤其是在许多事务与破坏性语句同时发生的情况下。
要确定位置编号，请在执行不需要的事物的时间附近运行`mysqlbinlog`一段时间，但将结果重定向到文本文件以供检查:

```sh
mysqlbinlog --start-datetime="2005-04-20 9:55:00" \
            --stop-datetime="2005-04-20 10:05:00" \
            /var/log/mysql/bin.123456 > /tmp/mysql_restore.sql
```

**位置编号以`log_pos数字`进行标记**，查看并找到相应的位置标号，之后便可使用它们。

```sh
mysqlbinlog --stop-position=368312 /var/log/mysql/bin.123456 \
         | mysql -u root -p


mysqlbinlog --start-position=368315 /var/log/mysql/bin.123456 \
         | mysql -u root -p
```




<br/>
<br/>
<br/>




## MyISAM表维护和崩溃恢复

MyISAM Table Maintenance and Crash Recovery


本节讨论了如何使用`myisamchk`检查或修复`MyISAM`表——具有用于存储数据和索引的`.MYD`和`.MYI`文件。
你可以使用`myisamchk`来检查、修复、优化数据库表。

尽管使用`myisamchk`进行表修复十分安全，但在进行修复或任何可能对表进行大量更改的维护操作之前进行备份总是一个好主意。

影响索引的`myisamchk`操作可能导致使用与MySQL Server使用的值不兼容的全文参数重建`MyISAM FULLTEXT`索引。

<br>

`MyISAM`表也可以使用类似于`myisamchk`操作的SQL语句来完成：

- 检查`MyISAM`表，`CHECK TABLE`
- 修复`MyISAM`表，`REPAIR TABLE`
- 优化`MyISAM`表，`OPTIMIZE TABLE`
- 分析`MyISAM`表，`ANALYZE TABLE`

这些语句可以直接使用，也可通过`mysqlcheck`客户端程序使用。这些语句相对于`myisamchk`的一个优点是Server可以完成所有工作。使用`myisamchk`，你必须确保Server不会同时使用这些表，以便`myisamchk`和Server之间不会发生不需要的交互。


<br/>


### 使用myisamchk进行崩溃恢复

Using myisamchk for Crash Recovery


如果在禁用外部锁定(default)的情况下运行`mysqld`，则当`mysqld`使用同一个表时，你无法可靠地使用`myisamchk`来检查表。如果你可以确定在运行`myisamchk`时没有人会通过mysql访问表，你只需要在开始检查表之前执行`mysqladmin flush-tables`。如果你不能保证这一点，你必须在检查表时停止`mysqld`。如果你运行`myisamchk`来检查`mysqld`同时更新的表，你可能会收到一个警告，即使表没有也如此。

如果Server启用了外部锁定(external locking)，则可以随时使用`myisamchk`检查表。在这种情况下，如果Server尝试更新`myisamchk`正在使用的表，Server将等待`myisamchk`完成后再继续。

如果要使用`myisamchk`来修复或优化表，则必须始终确保`mysqld`Server未使用该表(或外部锁定)。如果没有停止`mysqld`，你应该在运行`myisamchk`之前至少做一个`mysqladmin flush-tables`。如果Server和`myisamchk`同时访问表，你的表可能会损坏。

<br>

执行崩溃恢复时，请务必了解数据库中每个`MyISAM`表`table_name`都对应于下面现实的数据库目录中的三个文件:

```
#数据文件和索引文件最常出问题
tbl_name.frm	Definition (format) file
tbl_name.MYD	Data file
tbl_name.MYI	Index file
```

`myisamchk`的工作原理时逐行创建`.MYD`数据文件的副本。它通过删除旧的`.MYD`文件并将新文件重命名为原始文件名来结束修复阶段。
如果使用`--quick`选项，`myisamchk`不会创建临时的`.MYD`文件，而是假定`.MYD`文件正确并且只生成新的索引文件而不触及`.MYD`文件。这是安全的，因为`myisamchk`会自动检查`.MYD`文件是否已损坏，如果存在则终止修复。



<br/>
<br/>



### 如何检查MyISAM表是否存在错误

How to Check MyISAM Tables for Errors


使用以下命令检查`MyISAM`表：

- `myisamchk tbl_name`
这能发现`99.99%`的错误，它找不到的是仅涉及数据文件的损坏。

- `myisamchk -m tbl_name`
这能发现`99.999%`的错误。它首先检查所有索引条目是否有错，然后读取所有行。它计算行中所有键值的校验和，并验证校验和是否与索引树种键的校验和匹配。

- `myisamchk -e tbl_name`
这可以对所有数据进行全面彻底的检查。

- `myisamchk -e -i tbl_name`
打印更多统计信息



<br/>
<br/>



### 如何修复MyISAM表

How to Repair MyISAM Tables


你同样可使用`CHECK TABLE`和`REPAIR TABLE`语句来检查和修复`MyISAM`表。

损坏的表的症状：

- `tbl_name.frm`被锁定以防止修改
- 找不到文件`tbl_name.MYI`(Errorcode: nnn)
- 意外的文件结束
- 记录文件崩溃
- 从表处理获得`error nnn`

获取更多有关错误的信息：

```sh
#perror nnn
perror 126 127 132 134 135 136 141 144 145
MySQL error code 126 = Index file is crashed
MySQL error code 127 = Record-file is crashed
MySQL error code 132 = Old database file
MySQL error code 134 = Record was already deleted (or record file crashed)
MySQL error code 135 = No more room in record file
MySQL error code 136 = No more room in index file
MySQL error code 141 = Duplicate unique key or constraint on write or update
MySQL error code 144 = Table is crashed and last repair failed
MySQL error code 145 = Table was marked as crashed and should be repaired
```

<br>

如果要从命令行修复表，则必须先停止`mysqld` Server。请注意，当你在远程Server执行`mysqladmin shutdown`时，`mysqld Server`在`mysqladmin`返回后仍然可用一段时间，直到所有语句处理并已停止并且所有索引更改都已刷新到磁盘。

- 步骤1： 检查表

```sh
myisamchk *.MYI
#myisamchk -e *.MYI


#如果mysqld已停止，使用--update-state告诉myisamchk将表标记为已检查
myisamchk --update-state *.MYI
```

- 步骤2： 简易修复表

```sh
#尝试修复索引，而不触及数据
myisamchk -r -q table_name


#数据
1. 数据备份
2. myisamchk -r table_name #这将删除不正确的行和以排除的行，并重构索引文件
3. 如果上一步失败，请使用 myisamchk --safe-recover tbl_name
4. 如果遇到意外错误，查看第3步
```

- 步骤3： 难以修复

```
#只有当索引文件中的第一个16KB块被销毁或包含不正确的信息，或索引文件丢失时，才应该到达此阶段。
#这种情况系，需要创建一个新的索引文件
1. 将数据移动到安全的地方
2. 创建空数据和新索引
mysql db_name
mysql> SET autocommit=1;
mysql> TRUNCATE TABLE table_name;
mysql> QUIT

3. 将旧的数据文件复制回新创建的数据文件
4. 重新执行步骤2
```

- 步骤4： 很难修复

```
#仅当.frm描述文件也崩溃时才应该到达此阶段
#这应该永远不会发生，因为创建表后描述文件不会发生更改
1. 从备份还原描述文件并回到步骤3
2. 如果没有备份，但确切知道如何创建表，请在另一个数据库中创建该表的副本。删除新数据文件，然后将.frm和.MYI移动到崩溃的数据库。返回步骤2尝试重建索引文件
```



<br/>
<br/>



### 优化MyISAM表

MyISAM Table Optimization


要合并碎片行并消除因删除或更新行而导致的浪费空间，请在恢复模式下运行myisamchk: `myisamchk -r table_name`

你也可以通过`OPTIMIZE TABLE`的SQL语句进行表优化。此语句执行表修复和Key 分析，并对索引数进行排序，以便Key查找更快。

`myisamchk`有许多其它选项可用于提高表的性能：

- `--analyze or -a`
执行Key分析。这可通过使连接优化器更好地选择连接表的顺序自己应该使用的索引来提高连接性能。

- `--sort-index or -S`
排序索引块。这可优化搜索并使表扫描更快地使用索引。

- `--sort-records=index_num or -R index_num`
根据给定索引对数据行进行排序。这可使数据更加本地化，并可以加速使用此索引的基于范围的`SELECT`和`ORDER BY`操作



<br/>
<br/>



### 配置MyISAM表维护计划

Setting Up a MyISAM Table Maintenance Schedule


最好定期执行检查表，而不是等着问题发生。
启用自动检查`MyISAM`表也是一个好主意。
还应该在正常系统操作期间定期检查你的表。

```sh
#栗子
35 0 * * 0 /path/to/myisamchk --fast --silent /path/to/datadir/*/*.MYI

myisamchk -r -s --sort-index --myisam_sort_buffer_size=16M */*.MYI
```





<br/>
<br/>

---

<br/>
<br/>




# 优化

Optimization


本章介绍如何优化MySQL性能并提供示例。优化涉及多个级别配置，调整和测量性能。根据你的工作角色(Developer、DBA、both)，你可在**单个SQL语句**、**整个应用程序**、**单个数据库Server**、**多个网络数据库Server**的级别进行优化。
有时你可以主动并提前计划性能，而有时可能会在出现问题后解决配置或代码问题。优化CPU和内存使用还可以提供伸缩性，允许数据库处理更多负载而不会降低速度。


<br/>


## 优化概述

Optimization Overview


数据库性能取决于数据库级别的几个因素，如**表**、**查询**、**配置设置**。这些软件结构导致硬件级别的CPU和I/O操作，你必须尽可能降低这些操作并使其尽可能高效。在处理数据库性能时，首先要了解软件方面的高级规则和指南，并使用**挂钟时间(wall-clock time)**来衡量性能。当你成为专家后，你将了解更多内部发生的信息，并开始测量CPU周期和I/O操作...

典型用户的目标是从现有的软件和硬件配置中获取最佳的数据库性能；高级用户寻找改进MySQL软件本身的机会，或者开发自己的存储引擎或硬件设备来扩展MySQL生态系统。



<br/>
<br/>



### 数据库级别的优化

Optimizing at the Database Level


使数据库应用程序快速运行的最重要的因素是其基本设计：

- **表结构是否合适？**
特别是，列(columns)是否具有正确的数据类型，并且每个表是否具有适合工作类型的列？
例如，更新频繁的应用程序通常具有少量列的许多表；而分析大量数据的应用程序通常具有大量列的少量表。

- **是否有适当的索引来提高查询效率？**

- **是否为每个表使用了适当的存储引擎，并利用使用的每个存储引擎的优势和功能？**
特别是，诸如**InnoDB**之类的事务性(transactional)存储引擎或诸如**MyISAM**之列的非事务性(nontransactional)存储引擎的选择，对于性能和伸缩性非常重要。
`InnoDB`是新表的默认存储引擎。实际上，先进的`InnoDB`性能特征意味着`InnoDB`表通常优于更简单的`MyISAM`表，尤其是对于繁忙的数据库。

- **是否每个表都使用了适当的行格式？**
这取决于表所使用的数据库。
特别是，压缩的表使用较少的磁盘空间，因此需要较少的磁盘I/O来读取和写入数据。压缩适用于`InnoDB`表的各种工作负载，以及只读(read-only)`MyISAM`表。

- **是否应用程序使用了适当的锁定策略？**
例如，通过允许共享访问，以便数据库操作可以并发运行，并在适当时请求独占访问，以便关键操作成为首要任务。
同样，存储引擎的选择也很重要。`InnoDB`存储引擎可以处理大多数锁定问题而无需你的参与，从而在数据库中实现更好的并发性，并减少代码的实验和调优。

- **是否正确使用了用于缓存的所有内存区域？**
也就是说，足够大以容纳频繁访问的数据，但不能太大以至于它们会超载物理内存并导致分页。要配置的主缓存区域是：`InnoDb`缓冲池，`MyISAM` key缓存、MySQL查询缓存。



<br/>
<br/>



### 硬件级别的优化

Optimizing at the Hardware Level


随着数据库变得越来越繁忙，任何数据库应用程序最终都会达到硬件限制。DBA必须评估是否可以调整应用程序或重新配置Server以避免这些瓶颈(bottlenecks)，或者是否需要更多硬件资源？

系统瓶颈通常来自于这些来源：

- **磁盘需求**
磁盘需要一段时间才能找到一段数据。对于现代磁盘，平均时间通常低于10ms。优化搜索时间的方法是将数据分发到多个磁盘上。

- **磁盘读写**
当磁盘位于正确位置时，我们需要读写数据。可以从多个磁盘并行读取。

- **CPU周期**
当数据在主存储器中时，我们必须处理它们以获得我们想要的结果。

- **内存带宽**
当CPU需要的数据量超过了CPU缓存容量时，主存带宽就成了瓶颈。



<br/>
<br/>



### 平衡移植性和性能

Balancing Portability and Performance


要在可移植的MySQL程序中使用面向性能的SQL扩展，你可在语句中包含MySQL特定关键字`/* ... */`评论分隔符(或`--注释`)




<br/>
<br/>
<br/>




## 优化SQL语句

Optimizing SQL Statements


数据库应用程序的核心逻辑是通过SQL语句执行的，无论是直接通过解释器还是通过API在幕后提交。


<br/>


### 优化SELECT语句

Optimizing SELECT Statements


查询以`SELECT`语句的形式执行数据库中的所有查找操作。调整这些语句的首要任务就是缩短响应时间。
除了`SELECT`语句外，查询的调优技术也适用于`DELETE`语句中的`CREATE TABLE ... AS SELECT`, `INSERT INTO ...  SELECT`和`WHERE`等构造子句。这些语句具有额外的性能考虑因素，因为它们将写操作与面向读操作的查询相结合。

优化查询的主要考虑因素有：

- 要使一个慢的`SELECT ... WHERE`查询更快，首先要检查是否可以添加索引。在`WHERE`字句中使用的列上设置索引，以加快评估、过滤和结果的最终检索。为避免浪费磁盘空间，请构建一小组索引，以加速应用程序中使用的许多相关查询。索引对于引用不同表的查询尤其重要，使用连接(joins)和外键(foreign keys)等功能。
- 隔离并调整查询的任何部分，例如函数调用，这会占用过多时间。根据查询的结构，可以为结果集中的每一行调用一次函数，甚至可以为表中的每一行调用一次函数，从而大大减轻任何低效率。
- 最大限度地减少查询中的全表扫描次数，尤其是对于大型表。
- 定期使用`ANALYZE TABLE`语句使表统计信息保持最新，因此优化程序具有构建有效执行计划所需的信息。
- 了解特定于每个表的存储引擎的调优技术，索引技术和配置参数。`InnoDB`和`MyISAM`都有一套指导方针，可以在查询中实现和维持高性能。
- 你可以优化`InnoDB`表的单查询事务。
- 避免以难以理解的方式转换查询。
- 如果其中一个基本准则无法轻松解决性能问题，请通过阅读`EXPLAIN`计划并调整索引、`WHERE`子句、`JOIN`子句等来调查特定查询的内部详细信息。
- 调整MySQL用于缓存区域的大小和属性。通过有效使用**InnoDB buffer pool**、**MyISAM key cache**、**MySQL query cache**，重复查询运行的更快，因为在第二次及以后的时间内都是从内存中检索结果
- 即使对于使用高速缓存存储区快速运行的查询，你仍可以进一步优化，以便它们需要更少的高速缓存，从而使你的应用程序更具可伸缩性。可伸缩性意味着你的应用程序可以处理更多的并发用户，更大的请求...，而不会出现性能大幅下降的情况
- 处理锁定问题，其中查询的速度可能会受到同时访问表的其它回话的影响


<br/>
<br/>


#### WHERE子句优化

WHERE Clause Optimization

你可能想要重写查询以更快地进行过算数运算，同时牺牲可读性。因为MySQL会自动执行类似的优化，所以通常可以避免这种工作，并使查询保持更容易理解和可维护的形式。

- 移除不必要的括号

```sql
((a AND b) AND c OR (((a AND b) AND (c AND d))))
--> (a AND b AND c) OR (a AND b AND c AND d)
```

- 恒量折叠

```sql
(a<b AND b=c) AND a=5
--> b>5 AND b=c AND a=5
```

- 恒量条件去除

```sql
(B>=5 AND B=5) OR (B=6 AND 5=5) OR (B=7 AND 5=6)
--> B=5 OR B=6
```

- 索引使用的常量表达式仅计算一次

- 早期检测无效常量表达式

- 如果不使用`GROUP BY`或聚合函数(`COUNT(), MIN()...`)，`HAVING`将与`WHERE`合并

- 对于连接中的每个表，构造一个更简单的`WHERE`以获得对表的快速平均，并且还尽快跳过行

- 在查询中的任何其它表之前，首先读取所有常量表：
	+ 一个空表或只有一行的表
	+ 与主键或唯一索引上的`WHERE`子句一起使用的表，其中所有索引部分都与常量表达式进行比较并定义为`NOT NULL`

以下所有表都用作常量表：

```sql
SELECT * FROM t WHERE primary_key=1;
SELECT * FROM t1,t2
  WHERE t1.primary_key=1 AND t2.primary_key=t1.id;
```

- 通过尝试所有可能性，可以找到加入表格的最佳连接组合。如果`ORFER BY`和`GROUP BY`子句中的所有列都来自同一个表，则在加入时首先选择该表

- 如果存在`ORDER BY`子句和不同的`GROUP BY`子句，或者`ORDER BY`或`GROUP BY`包含连接队列中第一个表以外的表中的列，则会创建临时表

- 如果使用`SQL_SMALL_RESULT`修饰符，MySQL将使用内存中的临时表

- 查询每个表索引，并使用最佳索引，除非优化程序认为使用表扫描更有效。

- 在某些情况下，MySQL甚至无需查阅数据文件即可从索引中读取行。

- 在输出每一行之前，将跳过与`HAVING`子句不匹配的行。

一些非常快的查询示例：

```sql
SELECT COUNT(*) FROM tbl_name;

SELECT MIN(key_part1),MAX(key_part1) FROM tbl_name;

SELECT MAX(key_part2) FROM tbl_name
  WHERE key_part1=constant;

SELECT ... FROM tbl_name
  ORDER BY key_part1,key_part2,... LIMIT 10;

SELECT ... FROM tbl_name
  ORDER BY key_part1 DESC, key_part2 DESC, ... LIMIT 10;
```

MySQL使用索引数解析一下查询，假设索引列是数字：

```sql
SELECT key_part1,key_part2 FROM tbl_name WHERE key_part1=val;

SELECT COUNT(*) FROM tbl_name
  WHERE key_part1=val1 AND key_part2=val2;

SELECT key_part2 FROM tbl_name GROUP BY key_part1;
```

以下查询使用索引来按排序顺序检索行，而不使用单独的排序传递：

```sql
SELECT ... FROM tbl_name
  ORDER BY key_part1,key_part2,... ;

SELECT ... FROM tbl_name
  ORDER BY key_part1 DESC, key_part2 DESC, ... ;
```


<br/>
<br/>


#### 范围优化

Range Optimization

`range`访问方法使用单个索引来检索包含在一个或多个索引值间隔内的表行的子集。他可用于单部分(single-part)或多部分(multiple-part)索引。


<br/>


##### 单部分索引的范围访问方法

Range Access Method for Single-Part Indexes

对于单部分索引，索引值间隔可以方便地由`WHERE`子句中相应条件表示，表示为范围条件而不是间隔。

单部分索引的范围索引条件的定义如下：

- 对于`BTREE`和`HASH`索引，使用`=, <=>, IN(), IS NULL, IS NOT NULL`运算符时，关键部分与常量值的比较是范围条件
- 另外，对于`BTREE`索引，关键部分与常量值的比较是使用`>, <, >=, <= between, !=, <>`运算符时的范围条件，或者`LIKE`比较时的`LIKE`比较是一个不以通配符开头的常量字符串
- 对于所有索引类型，多个范围条件与`OR`或`AND`组合形成范围条件

常量值表示一下之一：

- 来自查询字符串中的常量
- 来自同一连接(join)的const或system表的列
- 不相关子查询的结果
- 由前面类型的子表达式组成的任何表达式

`WHERE`子句中带有范围条件的查询的栗子：

```sql
SELECT * FROM t1
  WHERE key_col > 1
  AND key_col < 10;

SELECT * FROM t1
  WHERE key_col = 1
  OR key_col IN (15,18,20);

SELECT * FROM t1
  WHERE key_col LIKE 'ab%'
  OR key_col BETWEEN 'bar' AND 'foo';
```

MySQL尝试从每个可能索引的`WHERE`子句中提取范围条件。在提取过程期间，丢弃不能用于构建范围条件的条件，组合产生重叠范围的条件，并且去除产生空范围的条件。

通常，用于范围扫描的条件比`WHERE`子句的限制性更小。MySQL执行额外的检查以过滤掉满足范围条件但不满足完整`WHERE`子句的行。
MySQL不支持合并空间索引的范围方法的多个范围。要解决此限制，可以使用具有相同`SELECT`语句的`UNION`，但将每个空间谓词放在不同的`SELECT`中。


<br/>


##### 多部分索引的范围访问方法

Range Access Method for Multiple-Part Indexes

多部分索引的范围条件是单部分索引范围条件的扩展。多部分索引上的范围条件将索引行限制在一个或多个关键元组上定义关键元组间隔。

例如，考虑定义为`key1(key_part1, key_part2, key_part3)`的多部分索引：

```
key_part1  key_part2  key_part3
  NULL       1          'abc'
  NULL       1          'xyz'
  NULL       2          'foo'
   1         1          'abc'
   1         1          'xyz'
   1         2          'abc'
   2         1          'aaa'
```

条件`key_part1 = 1`定义此间隔：`(1,-inf,-inf) <= (key_part1,key_part2,key_part3) < (1,+inf,+inf)`
此间隔覆盖前面数据集中的第4、第5、第6个元组，并且可以由范围访问方法使用。
相反，条件`key_part3 = 'abc'`不定义单个间隔，并且不能由方位访问方法使用。

以下描述详细地说明了范围条件如何适用于多部分索引：

- 对于`HASH`索引，可使用包含相同值的每个间隔。

```sql
--const1, const2, ...都是常量
---cmp指的是=, <=>, IS NULL比较运算符
--条件涵盖所有索引部分
    key_part1 cmp const1
AND key_part2 cmp const2
AND ...
AND key_partN cmp constN;


--栗子
key_part1 = 1 AND key_part2 IS NULL AND key_part3 = 'foo'
```

- 对于`BTREE`索引，间隔可用于`AND`组合的条件，其中每个条件使用`=, <=>, IS NULL, >, <, >=, <=, !=, <>, BETWEEN, LIKE 'pattern'`来比较关键部分和常量值。只要可以确定包含于条件匹配的所有行的单个key元组，就可以使用间隔。

- 如果覆盖区间中包含的行集的条件与OR组合，则它们形成一个条件，该条件覆盖其间隔的并集中包含的一组行。如果条件与AND组合，则它们形成一个条件，该条件覆盖其间隔交集中包含的一组行。


<br/>


##### 多值比较的等价范围优化

Equality Range Optimization of Many-Valued Comparisons


<br/>


##### 行构造函数表达式的范围优化

Range Optimization of Row Constructor Expressions


<br/>


##### 对范围优化限制内存使用

Limiting Memory Use for Range Optimization

要控制范围优化程序可用的内存，请使用`range_optimizer_max_mem_size`系统变量：

- 0意味着不限制
- 大于零时，优化程序会在考虑范围访问方法时跟踪消耗的内存。如果要超过指定的限制，则放弃范围访问方法，并考虑其它方法。
- 对于`UPDATE`和`DELETE`语句，如果优化程序回退到完整表扫描并启用了`sql_safe_updates`系统变量，则会发生错误而不会警告，因为实际上没有使用任何key来确定要修改的行。
- 对于超出不可用范围优化内存并且优化程序回退到不太理想的计划的单个查询，增加`range_optimizer_max_mem_size`值可以提高性能。




<br/>
<br/>
<br/>




## 优化和索引

Optimization and Indexes


提高`SELECT`操作性能的最佳方法是在查询中测试的一个或多个列上创建索引。索引条目的作用类似于表行的指针，允许查询快速确定哪些行与`WHERE`字句中的条件匹配，并检索这些行的其它列值。
所有MySQL数据类型都可被索引。

尽管为查询中每个可能使用的列创建索引很有诱惑力，但不必要的索引会浪费空间并浪费时间让MySQL确定要使用的索引。索引还会增加`insert, update, delete`的成本，因为必须更新每个索引。
你必须找到适当的平衡，以使用最佳索引集实现快速查询。


<br/>


### MySQL如何使用索引

How MySQL Uses Indexes


索引用于快速查找具有特定列值(column value)的行(row)。如果没有索引，MySQL必须从第一行开始，然后读取整个表以查找相关行。表越大，成本越高。如果表中有相关列的索引，MySQL可以快速确定要在数据文件中间寻找的位置，而无需查看所有数据。这比按顺序读取每一行要快得多。

大多数MySQL索引(PRIMARY KEY, UNIQUE, INDEX, FULLTEXT)都存储在**B树(B-trees)**中。有几个例外：

- 空间数据类型的索引使用**R树(R-trees)**
- MEMORY表同样支持**hash索引**
- InnoDB对FULLTEXT使用**反转列表(inverted lists)**

<br>

MySQL使用索引进行这些操作：

- 快速查找与`WHERE`子句匹配的行
- 出于消除行的考虑。如果在多个索引之间有选择，MySQL通常使用找到最小行数的索引
- 如果表具有多列索引，则优化程序可以使用索引的任何左前缀来查找行
- 在执行join时从其它表中检索行。如果声明它们的类型和大小相同，MySQL可以更有效地使用列上的索引。如`VARCHAR(10)`和`CHAR(10)`的大小相同。
- 对于非二进制字符串之间的比较，两列应使用相同的字符集。
- 查找特定索引列`key_col`的`MIN()`和`MAX()`值，这是由预处理器优化的，它检查是否在索引中`key_col`之前出现的所有关键部分上使用`WHERE key_part_N = 常量`。在这种情况下，MySQL对每个`MIN()`或`MAX()`表达式执行单个键查找，并用常量替换它。如果所有表达式都替换为常量，则查询立即返回。

```sql
SELECT MIN(key_part2),MAX(key_part2)
  FROM tbl_name WHERE key_part1=10;
```

- 如果对可用索引的最左前缀进行排序或分组，则对表进行排序或分组。
- 在某些情况下，可以优化查询以在不咨询数据行的情况下检索值。

<br>

**对于小型表的查询，或大型表所有行的查询，索引不太重要。当查询要访问大多数行时，顺序读取比通过索引更快。顺序读取可以最大限度地减少磁盘搜索，即使查询不需要所有行也是如此。**



<br/>
<br/>



### 主键优化

Primary Key Optimization


表的主键(primary key)表示你在最重要的查询中使用的列或列的集合。它具有关联的索引，以实现快速查询优化。查询性能受益于`NOT NULL`，因为它不能包含任何`NULL`值。使用InnoDB存储引擎，表格数据在物理化上进行组织，以根据主键或列进行超快速查找和排序。

如果你的表又大又重要，但没有明显的列或列的集合作为主键，则可创建一个单独的列，其中包含自动增量值以用作主键。当你使用外键(foreign key)联接(join)表时，这些唯一ID可用于指向其它表中相应行的指针。



<br/>
<br/>



### 外键优化

Foreign Key Optimization


如果一个表有很多列，并且你查询了许多不同的列组合，那么将频率较低的数据拆分为每个都有几列的单独表可能会很有效，并通过从主表中复制数字ID列将它们与主表相关联。这样，每个小表都可以有一个主键来快速查找器数据，并且你可以使用连接操作仅查询所需的列集。根据数据的分布方式，查询可能会执行较少的`I/O`并占用较少的高速缓存，因为相关列在磁盘上打包在一起。
为了最大限度地提高性能，查询尝试从磁盘中读取尽可能少的数据块；只有几列的表可以在每个数据块中容纳更多行。



<br/>
<br/>



### 列索引

Column Indexes


最常见的索引类型涉及单个列，在数据结构中存储该列的值的副本，允许快速查找具有相应列值的行。**B树(B-tree)**数据结构允许索引在`WHERE`子句中快速查找特定值，一组值或一系列值。

- 索引前缀(Index Prefixes)
使用字符串列的索引规范中的`col_name(N)`语法，可以创建使用列的前N个字符的索引。以这种方式仅索引列值的前缀，可以使索引文件更小。当索引`BLOB`或`TEXT`列，必须为索引指定前缀长度。
前缀最长可达1000 Byte，InnoDB表为767Byte(除非你设置了innodb_larger_prefix)

```sql
CREATE TABLE test (blob_col BLOB, INDEX(blob_col(10)))
```

- 全文索引(FULLTEXT Indexes)
全文索引用于全文搜索。只有InnoDB和MyISAM存储引擎支持FULLTEXT索引，并且仅支持`CHAR, VARCHAR, TEXT`列。索引始终发生在整个列上，并且不支持前缀索引。

- 空间索引(Spatial Indexes)
你可以在空间数据类型上创建索引。MyISAM和InnoDB支持空间类型的R树(R-trees)索引。其它存储引擎使用B树来索引空间类型。

- MEMORY存储引擎中的索引
MEMORY存储引擎默认使用HASH索引，但也支持B树索引。



<br/>
<br/>



### 多列索引

Multiple-Column Indexes


MySQL可以创建复合索引(composite indexes)——即多列上的索引，索引最多包含16列。对于某些数据类型，你可以索引列的前缀。

MySQL可以对测试索引中的所有列的查询使用多列索引，或者只测试第一列，前几列等的查询。如果在索引定义中以正确的顺序指定列，则单个复合索引可以加速同一表上的多种查询。
多列索引可被视为排序数组，其行包含通过连接索引列的值创建的值。

假设某表如下：

```sql
CREATE TABLE test (
    id         INT NOT NULL,
    last_name  CHAR(30) NOT NULL,
    first_name CHAR(30) NOT NULL,
    PRIMARY KEY (id),
    INDEX name (last_name,first_name)
);


/*
name索引是一个包含两列的索引，它可用于查询中的查询。
可组合last_name和first_name的值进行查询，还可仅指定该索引的最左前缀last_name的值进行查询
*/


--因此，name索引可用于以下查询
SELECT * FROM test WHERE last_name='Widenius';

SELECT * FROM test
  WHERE last_name='Widenius' AND first_name='Michael';

SELECT * FROM test
  WHERE last_name='Widenius'
  AND (first_name='Michael' OR first_name='Monty');

SELECT * FROM test
  WHERE last_name='Widenius'
  AND first_name >='M' AND first_name < 'N';


--然而，name索引无法用于以下查找
SELECT * FROM test WHERE first_name='Michael';

SELECT * FROM test
  WHERE last_name='Widenius' OR first_name='Michael';
```



<br/>
<br/>



### 验证索引使用

Verifying Index Usage


始终检查所有查询是否确实使用你在表中创建的索引，使用`EXPLAIN`语句。



<br/>
<br/>



### InnoDB和MyISAM索引统计

InnoDB and MyISAM Index Statistics Collection


存储引擎收集有关表的统计信息，供优化器使用。表统计信息基于值组(value group)，其中值组是具有相同键(key)前缀值的一组行。优化器的目的，一个重要统计信息是值组大小的平均值。

MySQL使用值组大小的平均值的方式如下：

- 估计每个`ref`访问必须读取多少行
- 估计一个部分联接将产生多少行，即此表单的操作将产生的行数： `(...) JOIN tbl_name ON tbl_name.key = expr`










<br/>
<br/>



### B树和Hash索引的比较

Comparison of B-Tree and Hash Indexes


了解 **B树(B-Tree)**和**哈希(Hash)**数据结构有助于预测不同查询在索引中使用这些数据结构的不同存储引擎上的执行情况，特别是MEMORY存储引擎。


<br/>


#### B树索引

B-Tree Index Characteristics

B树索引可用于 `=, <, >, <=, >=, BETWEEN`运算符的表达式中的列比较。如果`LIKE`的参数是不以通配符开头的常量字符串，则它也可用于`LIKE`比较。

```sql
# SELECT使用索引的栗子
SELECT * FROM tbl_name WHERE key_col LIKE 'Patrick%';
SELECT * FROM tbl_name WHERE key_col LIKE 'Pat%_ck%';


# SELECT没有使用索引
SELECT * FROM tbl_name WHERE key_col LIKE '%Patrick%';
SELECT * FROM tbl_name WHERE key_col LIKE other_col;


# WHERE使用索引
... WHERE index_part1=1 AND index_part2=2 AND other_column=3

    /* index = 1 OR index = 2 */
... WHERE index=1 OR A=10 AND index=2

    /* optimized like "index_part1='hello'" */
... WHERE index_part1='hello' AND index_part3=5

    /* Can use index on index1 but not on index2 or index3 */
... WHERE index1=1 AND index2=2 OR index1=3 AND index3=3;


# WHERE没有使用索引
    /* index_part1 is not used */
... WHERE index_part2=1 AND index_part3=2

    /*  Index is not used in both parts of the WHERE clause  */
... WHERE index=1 OR A=10

    /* No index spans all rows  */
... WHERE index_part1=1 OR index_part2=10
```

有时MySQL不使用索引，及时有索引也是如此。发生这种情况的一种情况是，优化器估计使用索引将需要MySQL访问表中非常大比例的行。但是，如果此类查询使用LIMIT仅检索某些行，则MySQL仍会使用索引，因为它可以更快地找到要在结果中返回的几行。


<br/>
<br/>


#### 哈希索引

Hash Index Characteristics

哈希索引与B树索引的特征有些不同:

- 仅使用`=,<=>`运算符来比较(速度飞快)，不使用比较符找到一系列值。依赖于这种类型的单值查找的系统被称为**键值存储**。要将MySQL应用于此类应用程序，请尽可能使用哈希索引
- 优化器无法使用哈希索引来加速`ORDER BY`
- MySQL无法确定两个值之间大约有多少行。如果将MyISAM或InnoDB表更改为哈希索引的MEMORY表，则可能会影响某些查询
- 只有整个键可用于搜索行



<br/>
<br/>



### 索引扩展

Use of Index Extensions


InnoDB通过将主键列附加到它来自动扩展每个二级索引。考虑如下表定义:

```sql
CREATE TABLE t1 (
  i1 INT NOT NULL DEFAULT 0,
  i2 INT NOT NULL DEFAULT 0,
  d DATE DEFAULT NULL,
  PRIMARY KEY (i1, i2),
  INDEX k_d (d)
) ENGINE = InnoDB;
```

此表定义了i1, i2两个主键。它还在列(d)上定义了二级索引k_d, 但内部InnoDB扩展了该索引并将其视为列(d, i1, i2)。

在确定如何以及是否使用该索引时，优化程序会考虑扩展二级索引的主键列。这可以带来更高效的查询执行计划和更好的性能。



<br/>
<br/>



### 优化器使用生成的列索引

Optimizer Use of Generated Column Indexes


MySQL支持生成列的索引:

```sql
CREATE TABLE t1 (f1 INT, gc INT AS (f1 + 1) STORED, INDEX (gc));
```

生成的列gc定义为表达式f1 + 1.该列也被索引，优化器可以在执行计划构建期间考虑该索引。




<br/>
<br/>
<br/>
<br/>




## 优化数据库结构

Optimizing Database Structure


作为数据库设计者的角色中，寻找组织 schemas, tables, columns最有效的方法。在调整应用程序代码时，您可以最小化I/O，将相关项目保持在一起，并提前计划，以便在数据量增加时性能保持较高。从高效的数据库设计开始，团队成员可以更轻松地编写高性能的应用程序代码，并使数据库可以随着应用程序的发展和重写而持久。


<br/>


### 优化数据大小

Optimizing Data Size


设计表以最小化磁盘空间。这可以通过 **减少磁盘写入和读取的数据量** 来实现巨大的改进。当在查询执行期间被主动处理时，较小的表通常需要较少的内存。表数据的任何空间缩减也会导致较小的索引可以更快地处理。

MySQL支持许多不同的存储引擎(表类型)和行格式。对于每个表，你可以决定使用哪种存储和索引方法。为你的应用程序选择合适的表格式可以为你带来巨大的性能提升。

通过使用此处列出的技术，你可以获得更好的表性能并最大限度地减少存储空间:

- Table Columns
- Row Format
- Indexes
- Joins
- Normalization


<br/>


#### 列

- 尽可能使用最有效(最小)的数据类型。MySQL有许多专门的类型可以节省磁盘空间和内存。例如，如有可能，请使用较小的整数类型(integer types)来获取较小的表。`MEDIUMINT` 通常是比 `INT` 更好的选择，因为 `MEDIUMINT` 列使用的空间减少了 `25%`。

- 如果可能，将列声明为`NOT NULL`。它通过更好地使用索引并消除测试每个值是否为 `NULL` 的开销，使SQL操作更快。你同样节省了一些存储空间，每列1bit。如果你确实需要表中的 `NULL` 值，请使用它们。只需避免在每列中允许 `NULL` 值得默认设置。


<br/>
<br/>


#### 行格式

- 默认情况下，使用 `DYNAMIC` 行格式创建 `InnoDB` 表。要使用 `DYNAMIC` 以外的行格式，请配置 `innodb_default_row_format`， 或在 `CREATE TABLE` 或 `ALTER TABLE` 语句中显示指定 `ROW_FORMAT` 选项。

- 要通过以压缩格式存储表数据来进一步减少空间，请在创建 `InnoDB` 表时指定 `ROW_FORMAT=COMPRESSED`，或在现有 `MyISAM` 表上运行 `myisampack` 命令。

- 对于 `MyISAM` 表，如果没有任何可变长度列(VARCHAR, TEXT, BLOB列)，则使用固定大小的行格式。这更快，等可能浪费一些空间。


<br/>
<br/>


#### 索引

- 表的主索引应该尽可能短。这使得每行的识别变得简单有效。对于 `InnoDB` 表，主键列在每个辅助索引条目中都是重复的，因此如果你有许多辅助索引，则短主键可以节省大量空间。

- 仅创建你需要提高查询性能的索引。索引适用于检索，但会降低插入和更新操作的速度。如果你主要通过搜索列的组合来访问表，请在它们上创建单个复合索引，而不是为每列创建单独的索引。索引的第一部分应该是最常用的列。如果从表中选择时总是使用多列，则索引中的第一列应该是具有最多重复的列，以获得更好地索引压缩。

- 如果长字符串列很可能在第一个字符数上有唯一的前缀，那么最好只索引此前缀，使用MySQL支持在列的最左边部分创建索引。较短的索引更快，不仅因为它们需要更少的磁盘空间，而且因为它们还会在索引缓存中为你提供更多命中，从而减少磁盘搜索次数。


<br/>
<br/>


#### 联结

- 在某些情况下，分成两个经常扫描的表可能是有益的。如果它是动态格式的表，则尤其如此，并且可以使用较小的静态格式表，该表可用于在扫描表时查找相关行

- 在具有相同数据类型的不同表中声明具有相同信息的列，以基于相应列加速连接。

- 保持列名简单，以便你可以在不同的表中使用相同的名称并简化联结查询。例如，在名为 `customer` 的表中，使用名称 `name` 而不是 `customer_name`。要使你的名称可以知道其它SQL服务器，请考虑将它们保持为小于18个字符。


<br/>
<br/>


#### 规范化

- 通常，尽量保持所有数据不冗余重复(这不是指的高可用的冗余，而是不要重复存储数据)。为了取代重复的名称、地址和长值，为它们分配唯一的ID，在多个较小的表中根据需要重复这些ID，并通过 `join` 子句中的 ID 来联接查询中的表。

- 如果速度比磁盘空间更重要，并且保留多个数据副本的维护成本，你可以放宽规范化规则，复制信息或创建汇总表以获得更快的速度。



<br/>
<br/>



### 优化数据类型

Optimizing MySQL Data Types


<br/>


#### 优化数字数据

Optimizing for Numeric Data

- 对于唯一ID或可以表示为字符串或数字的其它值，首选数字列(prefer numeric columns to string columns)。由于较大的数值可以存储在比相应于字符串更少的字节中，因此传输和比较的速度更快，占用的内存更小。

- 如果你使用数字数据，在许多情况下从数据库访问信息比访问文本文件更快。数据库中的信息可能以比文本文件更紧凑的格式存储，因此访问它涉及更少的磁盘访问。您还可以在应用程序中保存代码，因为您可以避免解析文本文件以查找行和列边界。


<br/>
<br/>


#### 优化字符和字符串类型

Optimizing for Character and String Types

对于 character and string columns，请遵循一下准则:

- 当您不需要特定于语言的整理(collation)规则功能时，请使用二进制排序顺序进行快速比较和排序操作。你可以使用 `BINARY` 运算符在特定查询中使用二进制整理规则。

- 比较不同列的值时，请尽可能声明具有相同字符集和整理规则的列，以避免在运行查询时进行字符串转换。

- 对于小于8KB的列值，请使用二进制 `VARCHAR` 而不是 `BLOB`。`GROUP BY` 和 `ORDER BY` 子句可以生成临时表，如果原始表不包含任何 `BLOB` 列，这些临时表可以使用 MEMORY 存储引擎。

- 如果表中包含的字符串列(如名字和地址)，但许多查询不检索这些列，请考虑将字符串拆分为单独的表，并在必要时使用带有外键的连接查询。当MySQL从一行中检索任何值时，它会都包含改行的所有列的数据块。仅使用最常用的列保持每行较小，允许更多行适合每个数据块。这种紧凑的表减少了常见查询的磁盘I/O和内存使用。

- 当你使用随机生成的值作为 `InnoDB` 表中的主键时，请在其前面加上一个升序(asce)值，如当前的日期和时间。当连续的主值物理存储在彼此附近是，`InnoDB`可以更快地插入和检索它们。


<br/>
<br/>


#### 优化BLOB类型

Optimizing for BLOB Types

- 存储包含文本数据的大型 `BLOB` 时，请考虑先压缩它。当压缩整个表时，请勿使用此技术。

- 对于具有多个列的表，要减少不使用的 `BLOB` 列的查询的内存要求，请考虑将 `BLOB` 列拆分为单独的表，并在需要时使用连接查询它。

- 由于检索和显示 `BLOB` 值得性能要求可能与其它数据类型有很大不同，因此你可以将 特定的BLOB表放在不同的存储设备上，甚至是单独的数据库实例上。

- 可以讲列值的哈希值存储在单独的列中，索引该列，并在查询中测试哈希值，而不是针对非常长的文本字符串测试相等性。



<br/>
<br/>
<br/>



### 优化表

Optimizing for Many Tables


用于快速保持个别查询的一些技术设计在多个表之间拆分数据。当表的数量达到上万，甚至是上百万时，处理所有这些表的开销成为新的性能考虑问题。


<br/>


#### 如何打开和关闭表

[How MySQL Opens and Closes Tables](https://dev.mysql.com/doc/refman/5.7/en/table-cache.html)

当你执行`mysqladmin status`命令时，你应该看到如下内容:

```
Uptime: 426 Running threads: 1 Questions: 11082
Reloads: 1 Open tables: 12
# 如果你的表少于12个，则这个值会有些令人费解
```

MySQL是多线程，因此可能有许多C端同时为给定的表发出查询。为了最大限度地减少同一个表上具有不同状态的多个C端回话的问题，该表由每个并发回话独立打开。对于**MyISAM**表，每个打开表的C端数据文件都需要一个额外的文件描述符。

`table_open_cache`和`max_connections`系统变量会影响Server保持打开的最大文件数。如果增加这些值中的一个或两个，则可能会遇到操作系统对每个进程的打开文件描述符数量施加的限制。许多操作系统允许你增加打开文件限制，该方法因系统而异。
`table_open_cache`与`max_connections`有关。例如，对于200个并发运行的连接，请指定表缓存大小至少为`200 * N`，其中N是执行的任何查询中每个连接的最大表数。你还必须为临时表和文件保留一些额外的文件描述符。
请确保操作系统可以处理`table_open_cache`设置隐含的打开文件描述符的数量。如果`table_open_cache`设置的太高，MySQL可能会用完文件描述符(file descriptors)并出现拒绝连接或无法执行查询等症状。

还有考虑到MyISAM存储引擎需要为每个唯一打开的表提供两个文件描述符。对于分区的MyISAM表，打开的表的每个分区都需要两个文件描述符。 （当MyISAM打开分区表时，它会打开此表的每个分区，无论是否实际使用给定分区。要增加MySQL可用的文件描述符，请设置`open_files_limit`系统变量。

打开表的缓存保持在`table_open_cache`条目的级别上，Server在启动时自动调整缓存大小。要显式设置大小，请在启动时设置`table_open_cache`系统变量。MySQL可能临时打开许多表来执行查询。

在以下情况，MySQL会关闭一个未使用的表并将其从表缓存中删除:
- 当缓存已满并且线程尝试打开不在缓存中的表时
- 当缓存包含多个`table_open_cache`条目并且任何线程都不再使用缓存中的表时
- 当table-flushing操作发生。这有可能在`FLUSH TABLES`语句、执行`mysqladmin flush-tables`或`mysqladmin refresh`命令

当表缓存填满时，Server使用以下过程来定位要使用的缓存条目:
- 从最近最少使用的表开始，发布当前未使用的表
- 如果必须打开新表，但缓存已满且无法释放表，则会根据需要临时扩展缓存。当缓存处于临时扩展状态并且表从已使用状态变为未使用状态时，表将关闭并从缓存中释放。

为每个并发访问打开MyISAM表。这意味着如果两个线程访问同一个表，或一个线程在同一个查询中两次访问该表，则需要打开两次表。每个并发打开都需要表缓存中的条目。在任何MyISAM表的第一次打开都需要两个文件描述符: 一个用于数据文件，一个用于索引文件。对表的每次额外使用仅为数据文件提供一个文件描述符。索引文件描述符在所有线程之间共享。

如果要使用`HANDER tbl_name OPEN`语句打开表，则会为该线程分配专用的表对象。此表对象不由其它线程共享，并且在线程调用`HANDLER tbl_name CLOSE`或线程终止之前不会关闭。发生这种情况时，表将被放回表缓存中（如果缓存未满）。

要确定表缓存是否太小，请检查`Opened_tables`状态变量，该变量指示自Server启动以来的表的打开操作数:

```sql
SHOW GLOBAL STATUS LIKE 'Opened_tables';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Opened_tables | 2741  |
+---------------+-------+
```

如果值非常大或快速增加，及时你没有发出许多`FLUSH TABLES`语句，也请在Server启动时增加`table_open_cache`的值。


<br/>
<br/>


#### 在同一数据库中创建多个表的缺点

[Disadvantages of Creating Many Tables in the Same Database](https://dev.mysql.com/doc/refman/5.7/en/creating-many-tables.html)

如果在同一个数据库目录中有许多MyISAM表，则打开(open)、关闭(close)和创建(create)操作很慢。如果在许多不同的表上执行`SELECT`语句，则表缓存已满时会有一些开销，因为对于每个必须打开的表，必须关闭另一个表。您可以通过增加表缓存中允许的条目数来减少此开销。




<br/>
<br/>
<br/>



### 内部临时表

[Internal Temporary Table Use in MySQL](https://dev.mysql.com/doc/refman/5.7/en/internal-temporary-tables.html)


在某些情况下，Server在处理语句时创建内部临时表。用户无法直接控制何时发生这种情况。

Server在以下条件下创建临时表:
- 评估`UNION`语句，稍后描述一些异常
- 评估某些视图(view)
- 评估派生的表
- 为实现子查询或半连接创建的表
- 评估包含`ORDER BY`和`GROUP BY`子句的语句，或它们包含连接队列中第一个表以外的表中的列的语句
- 评估`DISTINCT`结合`ORDER BY`可能需要临时表
- 对于使用`SQL_SMALL_RESULT`修饰符的查询，MySQL使用内存临时表，除非查询还包含需要磁盘存储的元素
- 为了评估从同一个表中选择和插入的`INSERT ... SELECT`语句，MySQL创建一个临时表来保存SELECT中的行，然后将这些行插入到目标表中
- 评估多表`UPDATE`语句
- 评估`GROUP_CONCAT()`或`COUNT (DISTNCT)`表达式

要确定语句是否需要临时表，请使用`EXPLAIN`并检查`Extra`列以查看是否显示`Using temporary`。
当Server创建内部临时表(无论是内存还是磁盘上)时，它会增加`Created_tmp_tables`状态变量。

<br>

某些查询条件会阻止使用内存中的临时表，在这种情况下，Server会使用磁盘上的表:
- 表中存在`BLOB`或`TEXT`列，这包括具有字符串值的用户定义的变量，因此它们被视为BLOB或TEXT列，具体取决于它们的值分别是二进制字符串还是非二进制
- 如果使用`UNION`或`UNION ALL`，则`SELECT`列表中存在最大长度大于512的字符串列
- `SHOW COLUMNS`和`DESCRIBE`语句使用`BLOB`作为某些列的类型，因此用于结果的临时表是磁盘上的表

<br>

Server不对具有某些限定条件的`UNION`语句使用临时表。相反，它仅从临时表创建中执行结果列类型转换所必须的数据结构。该表未完全实例化，并且没有写入和读取行，行直接发送到C端。结果是减少了内存和磁盘要求，并且在第一行发送到客户端之前的延迟较小，因为Server不需要等到最后一个查询块执行。`EXPLAIN`和优化程序输出反映了此执行策略：`UNION RESULT`查询不存在，因此该块对应于从临时表中读取的部分。

这些条件使`UNION`无需临时表即可进行评估:
- The union is `UNION ALL`, not `UNION` or `UNION DISTINCT`
- 没有全局`ORDER BY`子句
- The union is not the top-level query block of an {INSERT | REPLACE} ... SELECT ... statement


<br/>

**内部临时表存储引擎**
Internal Temporary Table Storage Engine

内部临时表可以保存在内存中(由MEMORY存储引擎处理)，或由InnoDB或MyISAM存储引擎存储在磁盘上。

如果将内部临时表创建在内存中，但变得很大，MySQL会自动将其转换为磁盘表。内存临时表的最大大小由`tmp_table_size`或`max_heap_table_size`的值定义，以较小者为准。这与使用`CREATE TABLE`显式创建的MEMORY表不同。对于此类表，只有`max_heap_table_size`变量确定表可以增长的大小，并且没有转换为磁盘格式。
`internal_tmp_disk_storage_engine`变量定义Server用于管理磁盘内部临时表的存储引擎。允许的值是：INNODB(默认)和MyISAM。

<br/>

**内部临时表存储格式**
Internal Temporary Table Storage Format

内存临时表由MEMORY存储引擎管理，该引擎使用固定长度的行格式。`VARCHAR`和`VARBINARY`列值填充到最大列长度，实际上将它们存储为`CHAR`和`BINARY`列。

磁盘临时表由InnoDB或MyISAM存储引擎管理。两个存储引擎都使用动态宽度行格式存储临时表。与使用固定长度行的磁盘相比，列只占用所需的存储空间，从而减少磁盘I/O，空间要求和处理时间。

对于最初在内存中创建内部临时表的语句，然后将其转换为磁盘表，可以通过跳过转换步骤并在磁盘上创建表开始来实现更好的性能。`big_tables`变量可用于强制内部临时表的磁盘存储。




<br/>
<br/>
<br/>
<br/>




## 优化InnoDB表

[Optimizing for InnoDB Tables](https://dev.mysql.com/doc/refman/5.7/en/optimizing-innodb.html)


**InnoDB**是MySQL客户通常在生产环境中使用的存储引擎，其中可靠性和并发性非常重要。它是默认的MySQL存储引擎。本节介绍如何优化InnoDB表的数据库操作。


<br/>


### 优化InnoDB表的存储布局

[Optimizing Storage Layout for InnoDB Tables](https://dev.mysql.com/doc/refman/5.7/en/optimizing-innodb-storage-layout.html)


- 一旦数据达到稳定大小，或者增长的表增加到上百兆字节(MB)，请考虑使用`OPTIMIZE TABLE`语句重新组织并压缩任何浪费的空间。重组的表需要较少的磁盘I/O来执行全表扫描(full table scan)。这是一个简单的技术，可在其它技术不切实际时提高性能。
`OPTIMIZE TABLE`复制表的数据部分并重建检索。其好处改进了索引中数据的打包，减少了表空间和磁盘上的碎片。好处取决于每个表中的数据。如果表很大或者正在重建的索引不适合缓冲池，则此操作可能很慢。向表中添加大量数据后的第一次运行通常比以后的运行慢得多。

- 在InnoDB中，具有**long PRIMARY KEY**(具有冗长值的单个列或形成长度复合值的多个列)浪费了大量磁盘空间。在指向同一行的所有辅助索引(secondary index)记录中，行的主键值重复。如果主键很长，则创建`AUTO_INCREMENT`列作为主键，或者索引`long VARCHAR`列的前缀而不是整个列。

- 使用`VARCHAR`数据类型而不是`CHAR`来存储可变长度(variable-length)字符串或具有许多`NULL`值的列。即使字符串较短或其值为`NULL`，`CHAR(N)`的列也始终使用N个字符(character)来存储数据。较小的表更适合缓冲池并减少磁盘I/O。
当使用`COMPACT`行格式(默认的InnoDB格式)和可变长度字符串(如utf8)时，`CHAR(N)`列占用可变的空间量，但仍至少占用N个字节。

- 对于大型表或包含大量重复文本(repetitive text)或数字(nemeric)数据的表，情考虑使用`COMPRESSED`行格式。将数据放入缓冲池(buffer pool)或执行全表扫描需要较少的磁盘I/O。在作出永久性决策之前，请使用`COMPRESSED`与`COMPACT`行格式测量可以实现的压缩量。



<br/>
<br/>
<br/>



### 优化InnoDB事务管理

[Optimizing InnoDB Transaction Management](https://dev.mysql.com/doc/refman/5.7/en/optimizing-innodb-transaction-management.html)


要优化InnoDB事务处理，请在事务功能的性能开销(performance overhead)和Server的工作负载(workload)之间找到理想的平衡点。例如，如果应用程序每秒提交数千次，则可能会遇到性能问题，

- 默认的MySQL设置`AUTOCOMMIT=1`可以对繁忙的数据库Server施加性能限制。在可行的情况下，通过发出`SET AUTOCOMMIT=0`或`START TRANSACTION`语句，然后在进行所有更改后发出`COMMIT`语句，将多个相关数据更改操作包装到单个事务中。
如果该事务对数据库进行了修改，InnoDB必须在每次事务提交时将日志刷新到磁盘。当每次更改后都提交时，存储设备的I/O吞吐量会限制每秒潜操作的数量。

- 对于仅包含单个`SELECT`语句的事务，启用`AUTOCOMMIT`可帮助InnoDB识别只读事务并对其进行优化。

- `INSERT`, `DELETE`, `UPDATE`大量行后，避免执行行回滚。如果大型事务正在降低Server性能，则将其回滚可能会使问题变得更糟，可能需要花费几倍的时间来执行原始数据更改操作。杀死数据库进程没有帮助，因为Server启动时会再次启动回滚。
为了尽量减少此问题发生的可能性：
  - 增加缓冲池的大小，以便可以缓存所有数据更改，而不是立即写入磁盘
  - 设置`innodb_change_buffering=all`，以便缓冲除`INSERT`之外的`UPDATE, DELETE`操作
  - 考虑在大数据更改操作期间定期发出`COMMIT`语句，可能会破坏单个删除或更新为较少行数进行操作的多个语句
要在发生失控回滚后摆脱它，请增加缓冲池使回滚变为CPU限制并快速运行，或杀死Server并使用`innodb_force_recovery=3`选项来启动它。
预计此问题很少发生，默认设置`innodb_change_buffering=all`，他允许将`UPDATE`和`DELETE`操作缓存在内存中，从而使它们可在第一时间更快地执行，并且如果需要还可以更快地回滚。确保在处理具有许多`INSERT`, `UPDATE`, `DELETE`操作的长时间运行事务的Server上使用此参数设置。

- 如果发生奔溃时，如果你可以承受丢失一些最新提交的事务，则可以设置 `innodb_flush_log_at_trx_commit=0`。InnoDB无论如何都会尝试每秒刷新(FLUSH)一次日志，尽管无法保证。此外，设置`innodb_support_xa=0`将减少磁盘数据和BINLOG同步而导致到磁盘刷新次数。

- 修改或删除行时，不会立即删除行和关系链的`undo log`，甚至在事务提交后。旧数据将保留，直到先前或同时启动的事务完成，以便这些事务可以访问已修改或已删除的行的先前状态。因此，长时间运行的事务可以阻止InnoDB清除由不同事务更改的数据。

- 在长时间运行的事务中修改或删除行时，使用`READ COMMITTED`和`REPEATABLE READ`隔离级别的其它事务必须执行更多工作，以便在读取相同行时重建旧数据。

- 当长时间运行的事务修改表时，从其它事务对该表的查询不会使用`covering index technique`。通常可从二级索引检索所有结果列的查询，而不是从表数据中查找适当的值。
如果发现二级索引页面的`PAGE_MAX_TRX_ID`太新，或二级索引中的记录被删除标记，则InnoDB可能需要使用`clustered index`来查找记录。



<br/>
<br/>
<br/>



### 优化InnoDB只读事务

[Optimizing InnoDB Read-Only Transactions](https://dev.mysql.com/doc/refman/5.7/en/innodb-performance-ro-txn.html)

InnoDB可以避免与已知为只读的事务设置事务ID(TRX_ID字段)相关的开销。只有可能执行写操作或锁定读取的事务(如SELECT...FOR UPDATE)才需要事务ID。消除不必要的事务ID会减少每次查询或数据更改语句构造读取视图时所咨询的内部数据结构的大小。













































<br/>
<br/>

---

<br/>
<br/>





# 语言结构

Language Structure


本章讨论在使用MySQL时编写SQL语句的以下元素的规则：

- 文字值，如字符串和数字
- 标识符，如数据库、表和列名
- 关键词与保留词
- 用户定义变量和系统变量
- 评论


<br/>



## 文字值

Literal Values


本节描述如何在MySQL中写入文字值。这包含了字符串、数字、十六进制、位值、布尔值、NULL。还将介绍在MySQL中处理这些基本类型时可能遇到的各种细微差别。


<br/>


### 字符串

String Literals

字符串时字节(Byte)或字符(character)序列，包含在单引号(`'`)或双引号(`"`)中。

栗子：

```
'a string'
'a' ' ' 'string'
"another string"
```

> 注意
> 如果启用了`ANSI_QUOTES` SQL模式，字符串只能在单引号中引用，因为双引号的字符串被解释为标识符。

二进制字符串(binary string)是一串字节(Bytes)。每个二进制字符串都有一个名为`binary`的字符集(character set)和排序规则。非二进制字符串是一串字符(characters)，它具有二进制以外的字符集和与字符集兼容的排序规则。

对于两种类型的字符串，比较是基于字符串单元的数值。对于二进制字符串，单位是字节(byte)，使用数字字节值比较；对于非二进制字符串，单位是字符(character)和支持多字节字符的字符集，使用数字字符码值比较。字符码排序是字符串排序的函数。

字符串文字可以有一个可选的字符集导入器和排序字句，将其指定为使用特定字符集和排序的字符串：`[_charset_name]'string' [COLLATE collation_name]`
栗子:

```sql
SELECT _latin1'string';
SELECT _binary'string';
SELECT _utf8'string' COLLATE utf8_danish_ci;


SELECT N'some text';
SELECT n'some text';
SELECT _utf8'some text';
```

<br>

在字符串中，某些序列具有特俗含义，除非启用了`No_BACKSLASH_ESCAPES` SQL模式。如转义字符每个序列都以反斜线(backslash)(`\`)开始。

**特殊字符转义序列：**

| Escape Sequence | Character Represented by Sequence |
| - | - |
| `\0` | An ASCII NUL (X'00') character |
| `\'`	| A single quote (') character |
| `\"`	| A double quote (") character |
| `\b`	| A backspace character |
| `\n`	| A newline (linefeed) character |
| `\r`	| A carriage return character |
| `\t`	| A tab character |
| `\Z`	| ASCII 26 (Control+Z); see note following the table |
| `\\`	| A backslash (\) character |
| `\%`	| A % character; see note following the table |
| `\_`	| A _ character; see note following the table |

在字符串中包含引号字符有几种方法：

- 两个引号
- 引号包含引号
- 转义引号

栗子：

```sql
SELECT 'hello', '"hello"', '""hello""', 'hel''lo', '\'hello';
+-------+---------+-----------+--------+--------+
| hello | "hello" | ""hello"" | hel'lo | 'hello |
+-------+---------+-----------+--------+--------+

SELECT "hello", "'hello'", "''hello''", "hel""lo", "\"hello";
+-------+---------+-----------+--------+--------+
| hello | 'hello' | ''hello'' | hel"lo | "hello |
+-------+---------+-----------+--------+--------+

SELECT 'This\nIs\nFour\nLines';
+--------------------+
| This
Is
Four
Lines |
+--------------------+

SELECT 'disappearing\ backslash';
+------------------------+
| disappearing backslash |
+------------------------+
```

要将二进制数据插入字符串列中，应该使用转义序列表示某些字符。在某些特定的Client环境下，可能还需要转换`NUL`或`Ctrl + Z`。
在编写应用程序时，必须将包含特殊字符正确的转义发送给MySQL。



<br/>
<br/>
<br/>



### 数字

Numeric Literals

数字包括精确值(整数和小数)和近似值(浮点数)。

整数用数字序列表示。数字可能包括`., -, +`，科学表示法`E`等。

```
#精确值，定点数
2.34

#近似值，浮点数
2.34E0
```




### 日期和时间

Date and Time Literals

日期和时间可以用几种格式表示。如`'2015-07-21', '20150721', 20150721`都可解释为日期。

**标准SQL和ODBC日期和时间：**
标准SQL允许使用`type`关键字和字符串指定时态文字。

```sql
--空格可选
DATE 'str'
TIME 'str'
TIMESTAMP 'str'
```

ODBC语法：

```
{ d 'str' }
{ t 'str' }
{ ts 'str' }
```

MySQL使用`type`关键字，这些结构分别包含`DATE, TIME, DATETIME`值，如果指定的话，好包括后面的小数秒部分。`TIMESTAMP`语法在MySQL中生成一个`DATETIME`值，因为`DATETIME`的范围与标准SQL `TIMESTAMP`类型更接近，后者年限为0001-9999。而MySQL的`TIMESTAMP`范围是1970-2038年。

<br>

**日期和时间上下文中的字符串和数字：**

MySQL可以识别以下格式的`DATE`值：

- `'YYYY-MM-DD'`或`'YY-MM-DD'`字符串格式。允许宽松的语法——即任何标点字符都可作为日期之间的分隔符。如`'2012-12-31', '2012/12/31/', '2012@12@31'...`
- `'YYYYMMDD'`或`'YYMMDD'`没有分隔符的字符串格式，前提是该字符作为日期有意义。如`'20121231', '121231'...`
- `YYYYMMDD`或`YYMMDD`数字格式，前置是该数字作为日期有意义。如`20121231, 121231...`

MySQL可以识别以下格式的`DATETIME`和`TIMESTAMP`：

- `'YYYY-MM-DD HH:MM:SS'`或`'YY-MM-DD HH:MM:SS'`字符串格式。允许宽松的语法。如`2012/12/31 00*01*02...`
日期和时间部分可以用`T`分隔，而不是空格。如`2012-12-31 00:01:02, 2012-12-31T01:02:03`

- `'YYYYMMDDHHMMSS'`或`'YYMMDDHHMMSS'`没有分隔符的字符串格式，前提是该字符作为日期有意义
- `YYYYMMDDHHMMSS`或`YYMMDDHHMMSS`数字格式，前提是该数字作为日期有意义

`DATETIME`和`TIMESTAMP`值可以包含一个精度不超多微秒(6位)的小数部分。小数部分应该始终使用小数点`.`与其他部分跟开，无法识别分数秒分隔符。

<br>

MySQL使用以下规则解释两位数的年值：

- 70-99转换为1970-1999
- 00-69转换为2000-2069

<br>

MySQL可以识别以下格式的`TIME`值：

- `'D HH:MM:SS'`字符串格式，`D`表示天数(0-34)。可以使用放松的语法。
- `'HHMMSS'`没有分隔符字符串格式，前提是作为时间有意义。
- `HHMMSS`数字格式，前提是作为时间有意义。

小数秒部分在`'D HH:MM:SS.fraction'`时间格式中识别，其中小数是精度最高可达微秒(6位)的小数部分，小数部分使用小数点`.`与其它部分分隔开，无法识别其它小数秒分隔符。



<br/>
<br/>
<br/>



### 十六进制

Hexadecimal Literals


























<br/>
<br/>

---

<br/>











# 字符集和编码

Character Sets, Collations, Unicode




















<br/>
<br/>

---

<br/>





# 数据类型

Data Type


MySQL支持多种类型的SQL数据类型：

- numeric
- date/time
- string
	+ character
	+ byte
- JSON

<br>

数据类型描述使用如下约定：

- `M`表示整数类型的最大显示宽度
- `D`适用于浮点和定点类型，并指示小数点后面的位数
- `fsp`适用于TIME, DATATIME, TIMESTAMP类型，表示小数点的秒精度
- 方括号`[]`表示类型定义的可选部分



<br/>
<br/>



## 数字

Numberic type


如果为数字列指定`ZEROFILL`，MySQL会自动将`UNSIGNED`属性添加到列中。

数字数据类型允许`UNSIGNED`(无符号)属性，也允许`SIGNED`(符号)。默认情况下，这些数据类型是`SIGNED`，因此`SINGED`属性不起作用。


<br>

- BIT
A bit-value type.(1-64)

- TINYINT
A very small integer.
有符号范围: `-128 to 127`, 无符号范围: `0-255`

- BOOL

- SMALLINT
A small integer.
有符号范围: `-32768 to 32767`, 无符号范围: `0-65535`

- MEDIUMINT
A medium-sized integer.
有符号范围: `-8388608 to 8388607`, 无符号范围: `0-16777215`

- INT
A normal-size integer.
有符号范围: `-2147483648 to 2147483647`, 无符号范围: `0- 4294967295`

- INTERGER
此类型是INT的同义词。

- BIGINT
A large integer.
符号范围: `-9223372036854775808 to 9223372036854775807`, 无符号范围: `0 to 18446744073709551615`
`SERIAL`是`BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE`的别名。

- DECIMAL/DEC

- FLOAT
A small(单精度) floating-point number.
允许的值为: -3.402823466E+38 to -1.175494351E-38, 0, and 1.175494351E-38 to 3.402823466E+38

- DOUBLE
A normal-size(双精度) floating-point number.
允许值为: -1.7976931348623157E+308 to -2.2250738585072014E-308, 0, and 2.2250738585072014E-308 to 1.7976931348623157E+308

- FLOAT
A floating-point number.









<br/>
<br/>


## 日期和时间

Date and Time Type


MySQL允许的TIME, DATETIME, TIMESTAMP值的小数，精度高达微秒(小数点后6位)。

- DATE
A date.
支持范围: `1000-01-01`到`9999-12-31`。
MySQL以`YYYY-MM-DD`格式显示DATE值，但允许使用字符串或数字将值分配给DATE列。

- DATETIME
A date and time combination.
支持范围: `1001-01-01 00:00:00.000000`到`9999-12-31 23:59:59.999999`。
MySQL以`YYYY-MM-DD HH:MM:SS.[fraction]`的格式显示DATETIME值，同样允许字符串或数字将值分配给DATETIME列。

- TIMESTAMP
A timestamp.
支持范围: `1970-01-01 00:00:01.000000`UTC到`2038-01-19 03:14:07.999999`UTC
TIMESTAMP值存储为自纪元`1970-01-01 00:00:01.000000 UTC`以来的秒数，这也叫原子时间。

- TIME
A time.
支持范围: `-838:59:59.000000` to `838:59:59.000000`
MySQL以`HH:MM:SS[.fraction]`的格式显示TIME值，但允许使用字符串或数字将值分配给TIME列。

- YEAR
A year in four-digit format.
MySQL以`YYYY`格式显示YEAR值，但允许使用字符串或数字将值分配给YEAR列。






<br/>
<br/>

---

<br/>


## 字符串

String Type


在某些情况下，MySQL可能会使用`CREATE TABLE`或`ALTER TABLE`语句更改字符串的类型。

**CHARACTER SET/CHARSET**
指定字符集

```sql
CREATE TABLE t
(
    c1 VARCHAR(20) CHARACTER SET utf8,
    c2 TEXT CHARACTER SET latin1 COLLATE latin1_general_cs
);
```

<br>

- CHAR
一个固定长度的字符串，在存储时使用用空格填充指定长度。
VARCHAR的有效最大长度取决于最大行大小(65535字节)和使用的字符集。

- VARCHAR
一个可变长度的字符串。

- BINARY
BINARY类似于CHAR，但存储二进制字节字符串而不是非二进制字符串。

- VARBINARY

- TINYBLOB
A BLOB column with a maximum length of 255 (2^8 − 1) bytes.

- TINYTEXT
A TEXT column with a maximum length of 255 (2^8 − 1) characters.

- BLOB
A BLOB column with a maximum length of 65,535 (2^16 − 1) bytes.

- TEXT
A TEXT column with a maximum length of 65,535 (2^16 − 1) characters.

- MEDIUMBLOB
A BLOB column with a maximum length of 16,777,215 (2^24 − 1) bytes.

- MEDIUMTEXT
A TEXT column with a maximum length of 16,777,215 (2^24 − 1) characters.

- LONGBLOB
A BLOB column with a maximum length of 4,294,967,295 or 4GB (2^32 − 1) bytes.

- LONGTEXT
A TEXT column with a maximum length of 4,294,967,295 or 4GB (2^32 − 1) characters.

- ENUM
An enumeration.

- SET
A set.















































<br/>
<br/>

---

<br/>
<br/>











# 必知必会

[MySQL Crash Course](https://book.douban.com/subject/3354490/)


<br/>


## 了解SQL


<br/>


### 数据库基础

在学习MySQL以及SQL之前，应该对数据库及数据库技术的概念有所了解。


<br/>


#### 什么是数据库

数据库是一种以某种有组织的方式存储的数据集合。

人们通常用数据库这个术语来代表他们使用的数据库软件。这是不正确的，它是引起混淆的根源。确切地说，数据库软件应该成为**DBMS(数据库管理系统)**.


<br/>
<br/>


#### 表

在文件柜中创建文件，然后将相关的资料放入特定的文件中。
在数据库领域，这种文件成为**表(table)**，表是一种结构化的文件，可用来存储某种特定类型的数据。
绝不应该将顾客的清单与订单的订单存储在同一个数据库表中。这样讲使以后的检索和访问很困难。应该创建两个表，每个清单一个表。

**模式(schema)**，关于数据库和表的布局及特征的信息。
有时，模式用作数据库的同义词。


<br/>
<br/>


#### 列和数据类型

表由列组成。列中存储着表中某部分的信息。
**列(column)**，表中的一个字段。
数据库中的每个列都有相应的数据类型。

正确地将数据分解为多个列极为重要。如城市、省、邮编应该是独立的列。通过将它们分隔开，才有可能对其进行组合操作。

<br>

**数据类型(datatype)**，所容许的数据的类型。每个表列都有相应的数据类型。


<br/>
<br/>


#### 行

表中的数据是按行存储的，所保存的每个记录存储在自己的行内。
**行(row)**，表中的一个记录。


<br/>
<br/>


#### 主键

表中每一行都应该有可以表示自己的一列(一组列)。
**主键(primary key)**，一列(一组列)，其值能够唯一区分表中的每个行。

唯一标识表中的每行的这个列(这组列)称为主键，主键用来表示一个特定的行。
应该总是定义主键。虽然并不总是都需要主键，但数据库设计人员都应该保证他们创建的每个表都具有一个主键，以便于以后的数据操作和管理。

表中的任何列都可作为主键，只要它满足一下条件:
- 任意两行都不具有相同的主键值；
- 每行都必须具有一个主键值(主键值不允许NULL值)。

主键通常定义在表的一列上，但这并不是必须的，也可以一起使用多个列作为主键。所有列值的组合必须是唯一的(但单个列的值可以不唯一)。

<br>

**主键的好习惯:**
- 不更新主键列中的值；
- 不重用主键列的值；
- 不在主键列中使用可能会更改的值。



<br/>
<br/>



### 什么是SQL

SQL（发音为字母S-Q-L或sequel）是结构化查询语言（Structured Query Language）的缩写。

SQL有以下优点:
- SQL不是为某个特定数据库供应商专有的语言。几乎所有的DMS都支持SQL。
- SQL简单易学。它的语句全都是由描述性很强的英语单词组成。
- SQL尽管看上去很简单，但它实际上是一种强有力的语言，可进行非常复杂和高级的数据库操作；
- SQL不区分大小写，但建议对SQL关键字使用大写，表列等使用小写，这样方便阅读和调试；
- 在处理SQL语句时，所有空格都将被忽略。



<br/>
<br/>



### SQL注释

- 单行注释
  - `#`
  - `-- `
- 多行注释
  - `/*   */`

```sql
#sql single line comment

-- single line comment
-- 注意，有空格

/*
line1 comment
line2 comment
...
*/
```





<br/>
<br/>
<br/>




## MySQL简介

<br/>

### 什么是MySQL

MySQL是一种DBMS，即它是一种数据库管理软件。

众多开发者和公司使用MySQL的原因:
- 成本，MySQL是开源的；
- 性能，MySQL执行很快；
- 可信赖，大公司也使用它；
- 简单，MySQL易于安装和使用。



<br/>
<br/>



#### C-S

DBMS可分为两类:
- 基于共享文件系统，如Microsoft Access，通常用于桌面用途；
- 基于C-S，如MySQL、Oracle、SQL Server。



<br/>
<br/>



### MySQL工具

工欲善其事必先利其器。有3个工具需要提及:

- msyql命令行使用程序
- MySQL Administrator
- MySQL Query Browser




<br/>
<br/>
<br/>




## 条件判断语句

- `IF`语句
- `CASE`语句


<br/>


### IF

- `IF FUNCTION`
- `IF STATEMENT`

```sql
HELP IF STATEMENT;

IF search_condition THEN statement_list
    [ELSEIF search_condition THEN statement_list] ...
    [ELSE statement_list]
END IF

-- 栗子
CREATE PROCEDURE test1
BEGIN
  IF score >= 90 THEN
    SELECT score, 'A';
  ELSEIF score<90  AND socre>=80 THEN
    SELECT score, 'B'
  ELSEIF socre<80 AND score>=70 THEN
    SELECT score, 'C'
  ELSEIF score<70 AND score>=60 THEN
    SELECT socre, 'D'
  ELSE
    SELECT score, 'E'
  END IF;
END;




HELP IF FUNCTION;
IF(expr1,expr2,expr3)

-- 栗子
SELECT IF(1>2,2,3);
+-------------+
| IF(1>2,2,3) |
+-------------+
|           3 |
+-------------+

```



<br/>
<br/>



### CASE

- `CASE OPERATOR`
- `CASE STATEMENT`

```sql
HELP CASE OPERATOR;

CASE value WHEN [compare_value] THEN result [WHEN [compare_value] THEN
result ...] [ELSE result] END

CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...]
[ELSE result] END

-- 栗子
SELECT CASE 1 WHEN 1 THEN 'one'
              WHEN 2 THEN 'two'
              ELSE 'more'
       END;
-- one



HELP CASE STATEMENT;

CASE case_value
    WHEN when_value THEN statement_list
    [WHEN when_value THEN statement_list] ...
    [ELSE statement_list]
END CASE

-- 或
CASE
    WHEN search_condition THEN statement_list
    [WHEN search_condition THEN statement_list] ...
    [ELSE statement_list]
END CASE

-- 栗子
CREATE PROCEDURE p()
BEGIN
  DECLARE v INT DEFAULT 1;

  CASE v
    WHEN 2 THEN SELECT v;
    WHEN 3 THEN SELECT 0;
    ELSE
      BEGIN
      END;
  END CASE;
END;
```




<br/>
<br/>
<br/>




## 循环语句

- `WHILE`语句
- `REPEAT`语句
- `LOOP`语句


<br/>


### WHILE

```sql
HELP WHILE;

[begin_label:] WHILE search_condition DO
    statement_list
END WHILE [end_label]

-- 栗子
CREATE PROCEDURE dowhile()
BEGIN
  DECLARE v1 INT DEFAULT 5;

  WHILE v1 > 0 DO
    SET v1 = v1 - 1;
  END WHILE;
END;
```



<br/>
<br/>



### REPEAT

```sql
HELP REPEAT;

[begin_label:] REPEAT
    statement_list
UNTIL search_condition
END REPEAT [end_label]

-- 栗子
CREATE PROCEDURE dorepeat(p1 INT)
BEGIN
  SET @x = 0;
  REPEAT
    SET @x = @x + 1;
  UNTIL @x > p1 END REPEAT;
END;
```



<br/>
<br/>



### LOOP

```sql
HELP LOOP;

[begin_label:] LOOP
    statement_list
END LOOP [end_label]

-- 栗子
CREATE PROCEDURE doiterate(p1 INT)
BEGIN
  label1: LOOP
    SET p1 = p1 + 1;
    IF p1 < 10 THEN
      ITERATE label1;
    END IF;
    LEAVE label1;
  END LOOP label1;
  SET @x = p1;
END;
```




<br/>
<br/>
<br/>




## 使用MySQL

<br/>

### 连接

在执行命令之前需要登录到DBMS。为了连接到MySQL，需要以下信息:
- hostname
- port
- username
- passwd(可选)



<br/>
<br/>



### 选择数据库

选择数据库有两种方式:
- 在连接是指定数据库名
- 登录后选择数据库

```
# 1
mysql -h xxx -P xxx -u xxx -p dbName

# 2
mysql> USE dbName;
```



<br/>
<br/>



### 了解数据库和表

数据库、表、列、用户、权限等的信息被存储在数据库和表中。不过，内部表一般不直接访问，可用`SHOW`命令来显示这些信息。

```sql
HELP SHOW;

SHOW DATABASES;

SHOW TABLES;

SHOW COLUMNS FROM tableName;

# 显示服务器信息
SHOW STATUS;

# 显式创建
SHOW CREATE DATABASE xxx;
SHOW CREATE TABLE xxx;

# 查看权限
SHOW GRANTS;

# 显示Server错误或警告信息
SHOW ERRORS;
SHOW WARNINGS;
```




<br/>
<br/>
<br/>



## 检索数据

<br/>

### SELECT语句

SQL语句有简单的英语单词构成，这些单词称为关键字，每个SQL语句由一个或多个关键字构成。
大概，最常用的SQL语句就是`SELECT`语句了，它从一个或多个表中检索信息。



<br/>
<br/>



### 检索单个列

```sql
SELECT columnName from tableName;

```



<br/>
<br/>



### 检索多个列

选择列名时，一定要在列名之间加上逗号，但最后一个列名不加。

```sql
SELECT columnName1, columnName2 FROM tableName;

```



<br/>
<br/>



### 检索所有列

一般，除非你确定表中的每个列，否则最好不要使用`*`通配符。

```sql
SELECT * FROM tableName;

```



<br/>
<br/>



### 检索不同的行

你不想要每个值每次出现，使用`DISTINCT`关键字以指示MySQL只返不同(唯一)的值。
DISTINCT关键字必须放在列名的前面。
DISTINCT关键字应用于所有列，而不仅是前置它的列。

```sql
SELECT DISTINCT columnName FROM tableName;

```



<br/>
<br/>



### 限制结果

SELECT返回所有匹配的行，为了返回一个或多个行，可使用LIMIT子句。

```sql
SELECT columnName FROM tableName LIMIT 10;

# LIMIT a, b指示MySQL返回从行a开始的b行
# 第一个数为开始位置，第二个数为要检索的行数
# 行数是从0开始计算的
SELECT columnName FROM tableName LIMIT 10, 5;
# 行数不够时，MySQL只返回它能返回的行
```



<br/>
<br/>



### 使用完全限定的表名

迄今为止使用的SQL栗子只通过列名引用列。也可能会使用完全限定的名字来引用列(同时使用表名和列名)。

```sql
SELECT tableName.columnName FROM tableName;

# 表名也可以是完全限定的
SELECT tableName.columnName FROM dbName.tableName;

```




<br/>
<br/>
<br/>




## 排序检索数据

本章讲解如何使用`SELECT`语句的`ORDER BY`子句，根据需要排序检索出的数据。


<br/>


### 排序数据

关系数据库设计理论认为，如果不明确规定排序顺序，则不应该假定检索出的数据的顺序有意义。

**子句(clause)**，SQL语句有子句构成，有些子句是必须的，而有的是可选的。

为了明确排序`SELECT`语句检索出的数据，可使用`ORDER BY`子句。

```sql
# 默认为升序
SELECT columnName FROM tableName ORDER BY columnName;

SLECT columnName1, columnName2 FROM tableName ORDER BY columnName2;
```



<br/>
<br/>



### 按多个列排序

经常需要按不止一个列进行排序。多列排序，只要指定列名，列名之间用逗号分隔开即可。

```sql
# 先按列1排序，再按列2排序
SELECT columnName1, columnName2, columnName3 FROM tableName ORDER BY columnName1, columnName2;

```



<br/>
<br/>



### 排序指定方向

- 升序(默认): 从A-Z，关键字`AESC`。因为是默认，所以无需指定。
- 降序: 从Z-A，使用`DESC`关键字。

```sql
SELECT columnName, columnName1 FROM tableName ORDER BY columnName1 DESC;

# 多序列，需要对每个列指定关键字
SELECT columnName, columnName1, columnName2 FROM tableName ORDER BY columnName1 DESC, columnName2i DESC;


# ORDER BY和LIMIT的组合
SELECT columnName
FROM tableName
ORDER BY
columnName DESC
LIMIT 10;
```




<br/>
<br/>
<br/>



## 过滤数据

本章讲解如何使用`SELECT`语句的`WHERE`子句指定搜索条件。


<br/>


### WHERE子句

数据库一般包含大量的数据，很少需要检索表中所有行。通常只会根据特定操作或报告的需要提取表数据的子集。
只检索所需数据需要指定搜索条件(也称为过滤条件)。

在`SELECT`语句中，数据根据`WHERE`子句中指定的搜索条件进行过滤。
在同时使用`ORDER BY`和`WHERE`子句时，应该让`ORDER BY`位于`WHERE`之后，否则将产生错误。

```sql
SELECT columnName
FROM tableName
WHERE id = 1001;
```



<br/>
<br/>



### WHERE子句操作符

MySQL支持如下条件操作符:

| 操作符 | 说明 |
| - | - |
| `=` | 等于 |
| `<>` | 不等于 |
| `!=` | 不等于 |
| `<` | 小于 |
| `<=` | 小于等于 |
| `>` | 大于 |
| `>=` | 大于等于 |
| `BETWEEN` | 在指定的两个值之间 |

<br/>


#### 检查单个值

```sql
SELECT column1, column2
FROM table1
WHERE name = 'zhang';

SELECT column1, column2
FROM table1
WHERE price <= 10;

```


<br/>
<br/>


#### 不匹配检查

```sql
SELECT column1, column2
FROM table1
WHERE id <> 1002;

SELECT column1, column2
FROM table1
WHERE id != 1003;

```


<br/>
<br/>


#### 范围值检查

为了检查某个范围的值，可使用`BETWEEN`操作符。它需要两个值，即范围的开始值和结束值。

```sql
SELECT column1, column2
FROM table1
WHERE price BETWEEN 2 AND 10;

```


<br/>
<br/>


#### 空值检查

在创建表时，可指定列是否可以不包含值。在一个列不包含值时，称其为空值`NULL`。
NULL， 无值(no value)，它与字段包含0、空字符串或仅仅包含空格㓊。

有一个特殊的`WHERE`子句`ISNULL`，用来检测具有NULL值的列。

```sql
SELECT column1
FROM table1
WHERE price IS NULL;
```



<br/>
<br/>



### 组合WHERE子句

使用组合的`WHERE`子句，以`AND`和`OR`子句(逻辑操作符)的方式，进行更强的数据控制。


<br/>


#### AND操作符

```sql
SELECT column1, column2
FROM table1
WHERE name = 'zhang' AND price <= 10;>
```


<br/>
<br/>


#### OR操作符

```sql
SELECT column1, column2
FROM table1
WHERE name = 'zhang' OR name = 'abc';
```


<br/>
<br/>


#### 计算次序

`WHERE`可包含任意数目的`AND`和`OR`操作符，允许两者结合已进行复杂和高级的过滤。
由于逻辑操作符存在优先级，所以请记得使用括号。

```sql
SELECT column1, column2
FROM table1
WHERE (id = 1001 OR id = 1003) AND price >= 10;
```



<br/>
<br/>



### IN操作符

`IN`操作符用来指定条件范围，范围中的每个条件都可以进行匹配。

```sql
SELECT column1, column2
FROM table1
WHERE id IN (1001, 1002)
ORDER BY column2;

# WHERE id = 1001 OR id = 1002
```

其实这里的`IN`和`OR`操作符完成相同的功能。为什么要使用`IN`操作符:
- 在使用长的合法选项清单时，IN操作符的语法更清楚且更直观
- 在使用IN时，计算的次序更容易管理
- IN操作符一般比OR操作符清单执行更快
- IN的最大优点是可以包含其他SELECT语句，使得能够更动态地建立WHERE子句



<br/>
<br/>



### NOT操作符

`WHERE`子句中的`NOT`操作符有且只有一个功能，那就是否定它之后所跟的任何条件。

```sql
SELECT column1, column2
FROM table1
WHERE id NOT IN (1001, 1002)
ORDER BY column2;
```




<br/>
<br/>
<br/>




## 通配符过滤

本章介绍什么是通配符、如何使用通配符以及怎样使用`LIKE`操作符进行通配搜索，以便对数据进行负载过滤。

**通配符(wildcard)**，用来匹配一部分的特殊字符。
**搜索模式(search pattern)**，由字面值、通配符或两者组合构成的搜索条件。

<br/>


### LIKE操作符

通配符本身本身实际是SQL的`WHERE`子句中有特殊含义的字符，SQL支持几种通配符:
- 百分号(`%`)通配符
- 下划线(`_`)通配符

为了在搜索子句中使用通配符，必须使用`LIKE`操作符。`LIKE`只是MySQL，后跟搜索模式利用通配符，而不是直接相等匹配进行比较。



<br/>
<br/>



#### 百分号通配符

- 注意，除了一个或多个字符，`%`还能匹配0个字符(也就是`>=0`);
- `%`通索配符不能匹配NULL;
- 在搜索中，`%`通配符表示任何字符出现任何次数(类似Linux中的星号`*`);
- `%`通配符可在搜索模式的任意位置使用，并且可以使用多个通配符(这点Linux的星号不支持);
- 通配符也可以出现在搜索模式的中间，虽然这样并不太有用; 
- 根据MySQL的配置方式，搜索可以是区分大小写的。

```sql
SELECT id, name
FROM table1
WHERE name LIKE '%zhang%';

SELECT name
FROM table1
WHERE name LIKE 'a%z';
```


<br/>
<br/>


#### 下划线通配符

下划线(`_`)通配符只匹配单个字符

```sql
SELECT id, name
FROM table1
WHERE name LIKE 'zhan_';

```



<br/>
<br/>



### 使用通配符的技巧

如上所见，MySQL通配符很有用。但这种功能是有代价的: **通配符搜索的处理一般要比前面讨论的其它搜索所花时间更长。**
这里给出一些使用通配符要记住的技巧：
- 不要过度使用通配符。如果其它操作符能达到相同的目的，应该使用其它操作符；
- 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处(这样太慢了)；
- 请注意通配符的放置位置，不要乱放。




<br/>
<br/>
<br/>




## 正则表达式搜索

本章介绍如何在MySQL `WHERE`子句内使用正则表达式(RE)来更好地控制数据过滤。


<br/>


### 正则表达式介绍

正则表达式用来匹配文本的特殊的串(字符集)。
所有种类的程序设计语言、文本编辑器、操作系统...都支持正则表达式。
正则表达式用正则表达式语言来建立，它是一种特殊语言。



<br/>
<br/>



### MySQL正则表达式

MySQL用`WHERE`子句中的`REGEXP`关键字对正则表达式提供了初步的支持。
MySQL中的正则表达式默认不缺分大小写；如果要区分，可使用`BINARY`关键字。


<br/>


#### 基本字符匹配

```sql
SELECT name
FROM table1
WHERE score REGEXP '100';

# 这里使用的正则效果还没有LIKE好，因为它并不复杂，这里仅做栗子参考


-- 下面看一个复杂点的
-- 特殊字符.表示匹配任意一个字符
SELECT score
FROM table1
WHERE socre REGEXP '.0'

```


<br/>
<br/>


#### OR匹配

使用`|`进行或匹配。

```sql
SELECT name
FROM table1
WHERE name REGEXP '111|222|333';

```


<br/>
<br/>


#### 匹配多个字符之一

利用一组括号`[]`来完成。

```sql
SELECT name
FROM table1
WHERE name REGEXP '[zhang|li|song]';

```


<br/>
<br/>


#### 匹配范围

集合可用来定义要匹配的一个或多个字符。

```sql
/*
[0123456789]
[0-9]
[a-z]
[a-zA-Z]
*/

SELECT name
FROM table1
WHERE name REGEXP '[a-zA-z]hang';

```


<br/>
<br/>


#### 转义字符

MySQL中的正则表达式使用两个反斜线(`\\`)做转义。

```sql
SELECT score
FROM table1
WHERE score  REGEXP '[8|9]0\\.0';

```


<br/>
<br/>


#### 匹配字符类

为了更方便的工作，可以使用预定义的字符集，称为**字符类(character class)**。

| 类 | 说明 |
| - | - |
| `[:alnum:]`  | 任意字母和数字（同`[a-zA-Z0-9]`） |
| `[:alpha:]` | 任意字符（同[a-zA-Z]） |
| `[:blank:]` | 空格和制表（同[\\t]） |
| `[:cntrl:]` | ASCII控制字符（ASCII 0到31和127） |
| `[:digit:]` | 任意数字（同[0-9]） |
| `[:graph:]` | 与[:print:]相同，但不包括空格 |
| `[:lower:]` | 任意小写字母（同[a-z]） |
| `[:print:]` | 任意可打印字符 |
| `[:punct:]` | 既不在[:alnum:]又不在[:cntrl:]中的任意字符 |
| `[:space:]` | 包括空格在内的任意空白字符（同[\\f\\n\\r\\t\\v]） |
| `[:upper:]` | 任意大写字母（同[A-Z]） |
| `[:xdigit:]` | 任意十六进制数字（同[a-fA-F0-9]） |


<br/>
<br/>


#### 匹配多个实例

重复元字符:

| 元字符 | 说明 |
| - | - |
| `*` | 0个或多个匹配 |
| `+` | 1个或多个匹配（等于{1,}） |
| `?` | 0个或1个匹配（等于{0,1}） |
| `{n}` | 指定数目的匹配 |
| `{n,}` | 不少于指定数目的匹配 |
| `{n,m}` |  匹配数目的范围（m不超过255 |

```sql
SELECT name
FROM table1
WHERE name REGEXP '\\([0-9] zhang?\\)';

```


<br/>
<br/>


#### 定位符

匹配特定位置的文本。

| 元字符 | 说明 |
| - | - |
| `^` | 文本的开始 |
| `$` | 文本的结尾 |
| `[[:<:]]` | 词的开始 |
| `[[:>:]]` | 词的结尾 |

```sql
SELECT name
FROM table1
WHERE name REGEXP '^[0-9\\.]';

```




<br/>
<br/>
<br/>




## 创建计算字段

本章介绍什么是计算字段，如何创建计算字段以及怎样从应用程序中使用别名引用它们。


<br/>


### 计算字段

存储在数据库表中的数据一般不是应用程序所需要的格式，我们需要直接从数据库中检索转换过的数据，然后再在C端重新格式化。



<br/>
<br/>



### 拼接字段

**拼接(concatenate)**，将值联结到一起构成单个值。在MySQL的`SELECT`语句中，可使用`Concat()`函数来拼接两个列。
多数DBMS使用`+`或`||`来实现拼接，MySQL使用`Concat()`函数来实现。当把SQL语句转换为MySQL语句时请一定注意。

```sql
SELECT Concat(name, ', id')
FROM table1;
```


<br/>


#### 使用别名

拼接字段做的很好，但新列并没有名字，而是使用`Concat(name, ', id')`作为列名。这样虽不能说不好，但利于使用。
为了解决这个问题，MySQL支持别名(alias)。可使用`AS`关键字赋予。

```sql
SELECT CONCAT(name, ', id') AS nameId
FROM table1;

```



<br/>
<br/>



### 执行算术计算

计算字段的另一常见用途是对检索出的数据进行算术计算。

MySQL算术操作符:

| 操作符 | 说明 |
| - | - |
| `+` | 加 |
| `-` | 减 |
| `*` | 乘 |
| `/` | 除 |

```sql
SELECT id, quantity, price, quantity*price AS totalPrice
FROM table1;


```




<br/>
<br/>
<br/>




## 数据处理函数

本章介绍什么是函数，MySQL支持何种函数，以及如何使用这些函数。


<br/>
<br/>


### 函数

与其它大多数计算机语言一样，SQL支持利用函数来处理数据。函数一般在数据上执行，它给数据的转换和处理提供了方便。

> 函数没有SQL的可移植性强。几乎每种DBMS的实现都支持其它不支持的函数，而且可能差异还很大。
> 为了代码的可移植性，许多SQL程序员不赞成使用特殊实现的功能。虽然这样有很多好处，但不总是利于应用程序的性能。
> 如果你决定使用函数，应该做好代码注释，以便大家知晓。


<br/>
<br/>


### 使用函数

大多数SQL支持以下类型的函数:
- 用于处理文本串的文本函数
- 用于在数值数据上进行算术操作的数值函数
- 用于处理日期和时间值并从这些值中提取特定成分的日期和时间函数
- 返回DBMS正使用的特殊信息的系统函数


<br/>


#### 文本处理函数

常见文本处理函数:

| 函数 | 说明 |
| - | - |
| `Left()` | 返回串左边的字符 |
| `Length()` | 返回串的长度 |
| `Locate()` | 找出串的子串 |
| `Lower()` | 将串转换为小写 |
| `LTrim()` | 去掉串左边的空格 |
| `Right()` | 返回串右边的字符 |
| `RTrim()` | 去掉串右边的空格 |
| `Soundex()` | 返回串的SOUNDEX值 |
| `SubString()` | 返回子串的字符 |
| `Upper()` | 将串转换为大写 |

SOUNDEX是将任何文本串转换为描述其语音表示的字母数字模式的算法。

<br>

```sql
SELECT name, UPPER(name)  AS name_upcase, LENGTH(name) AS name_length
FROM table1
ORDER BY name;

```


<br/>
<br/>


#### 日期和时间处理函数

日期和时间采用相应的数据类型和特殊的格式存储，以便能快速和有效地排序或过滤，并且节省物理存储空间。
一般，应用程序不使用用来存储日期和时间的格式，因此日期和时间函数总是被用来读取、统计和处理这些值。由于这个原因，日期和时间函数在MySQL语言中具有重要的作用。

常用的日期和时间处理函数:

| 函数 | 说明 |
| - | - |
| `AddDate()` | 增加一个日期 |
| `AddTime()` | 增加一个时间 |
| `CurDate()` | 返回当前时间 |
| `Month()` | 返回日期的月份部分 |
| `Now()` | 返回当前日志和时间 |
| `Second()` | 返回时间的秒部分 |
| `Time()` | 返回日期时间的时间部分 |
| `Year()` | 返回日期的年份部分 |

```sql
SELECT CURDATE(), CURTIME(), Now();


SELECT id, name
FROM table1
WHERE Date(datetime)BETWEEN '2018-12-01' AND '2019-01-31';
```

这是用`WHERE`使用日期和时间对数据进行过滤的一个好时机。请注意日期格式(`yyyy-mm-dd`)，应该总是使用`yyyy`而不是`YY`表示年份，这样更可靠。


<br/>
<br/>


#### 数值处理函数

数值处理函数仅处理数值数据。这些函数一般用于代数、三角或几何运算。

常用数值处理函数:

| 函数 | 说明 |
| - | - |
| `Abc()` | 求绝对值 |
| `Cos()` | 求余弦 |
| `Exp()` | 求指数值 |
| `Mod()` | 求余数 |
| `Pi()` | 求圆周率 |
| `Rand()` | 返回一个随机数 |
| `Sin()` | 求正弦 |
| `Sqrt()` | 求平方根 |
| `Tan()` | 求正切 |




<br/>
<br/>
<br/>




## 汇总数据

本章介绍什么是SQL的聚集函数，以及如何利用它们汇总表的数据。


<br/>


### 聚集函数

我们经常需要汇总熟不而不用把它们实际检索出来，为此MySQL提供了专门的函数。以便分析和报表的生成。

这种类型的检索栗子有以下几种:
- 确定表中行数
- 获得表中行组的和
- 找出表列的最大值、最小值、平均值

返回实际表数据是对时间和处理资源的一种浪费。而实际想要的是汇总信息。

**聚集函数(aggregate function)**，运行在行组上，计算和返回单个值的函数。

SQL聚集函数:

| 函数 | 说明 |
| - | - |
| `AVG()` | 返回某列的平均值 |
| `COUNT()` | 返回某列的行数 |
| `MAX()` | 返回某列的最大值 |
| `MIN()` | 返回 某列的最小值 |
| `SUM()` | 返回某列值之和 |


<br/>
<br/>


#### AVG函数

`AVG()`通过对表中的行数计数并计算特定列值之和，求得该列的平均值。可用来返回所有列的平均值，也可用来返回特定列或行的平均值。
`AVG()`忽略列值为`NULL`的行。

```sql
SELECT AVG(price) AS avg_price
FROM table1;

# 多列
SELECT AVG(price), AVG(age)
FROM table1;
```


<br/>
<br/>


#### COUNT函数

可利用`COUNT()`确定表中行的数据或符合特定条件的行的数目。

`COUNT()`函数有两种使用方式:
- 使用`COUNT(*)`对表中的行的数目进行技术，不管是否为空
- 石宏`COUNT(column)`对特定列的值进行行计数

```sql
SELECT COUNT(*)
FROM table1;


SELECT COUNT(column1)
FROM table1;

SELECT COUNT(column1)
FROM table1
WHERE name = 'xxx';
```


<br/>
<br/>


#### MAX函数

`MAX()`函数用于返回列中的最大值。
它忽略列值为空的行。

对非数值数据使用`MAX()`，虽然它一般用来找出最大的数值或日期，但MySQL允许将它用来返回任意列中的最大值，包括返回文本列中的最大值。在用于文本数据时，如果数据按相应的列排序，则它返回最后一行。

```sql
SELECT MAX(column1), MAX(column2)
FROM table1;

```


<br/>
<br/>


#### MIN函数

`MIN()`函数返回指定列的最小值。
它也会忽略列值为空的行。

对于非数值的使用，它与MAX类似。MySQL允许将它用来返回任意列中的最小值，包括返回文本列中的最小值。在用于文本数据时，如果数据按相应的列排序，则它返回最前面的行。

```sql
SELECT MIN(column1)
FROM table1;

```


<br/>
<br/>


#### SUM函数

`SUM()`用来返回特定列值的和。它也可以用来合计计算值。
`SUM()`函数忽略列值为`NULL`的行。

```sql
SELECT SUM(price) AS total
FROM table1;


SELECT SUM(price*quantity) AS total
FROM table1
WHERE xxx;
```



<br/>
<br/>



### 聚集不同值

对于以上5个聚集函数，都可以使用如下:
- 对所有的行执行计算，指定`ALL`参数或不给参数；
- 只包含不同的值，使用`DISTINCT`参数；
- `ALL`为默认。

```sql
SELECT AVG(DISTINCT price) AS avg_price
FROM table1;

```



<br/>
<br/>



### 组合聚集函数

`SELECT`语句可以根据需要包含多个聚集函数。

```sql
SELECT COUNT(*),
       MIN(price),
       MAX(price),
       AVG(price) AS avg_price
FROM table1;
```




<br/>
<br/>
<br/>




## 分组数据

本章介绍如何分组数据，以便能汇总表内容的自己。这涉及到`GROUP BY`子句和`HAVING`子句。


<br/>


### 数据分组

分组允许把数据分为多个逻辑组，以便能对每个组进行聚集计算。



<br/>
<br/>



### 创建分组

在`SELECT`语句中使用`GROUP BY`子句建立分组。使用`GROUP BY`的一些重要规定:
- `GROUP BY`可以包含任意数目的列。这使得能对分组进行嵌套，分数据分组提供更细致的控制；
- 如果在`GROUP BY`子句中嵌套了分组，数据将在最后规定的分组上进行汇总；
- `GROUP BY`子句中列出的每个列都必须是检索列或有效的表达式；
- 除聚集计算语句外，`SELECT`语句中的每个列都必须在`GROUP BY`子句中给出；
- 如果分组列中具有`NULL`值，则`NULL`将作为一个分组返回。如果列中有多行`NULL`值，它们将分为一组；
- `GROUP BY`子句必须出现在`WHERE`子句之后，`ORDER BY`子句之前；
- 使用`WITH ROLLUP`关键字，可以得到每个分组以及每个分组汇总界别的值。

```sql
SELECT id, SUM(score)
FROM table1
GROUP BY id;

```



<br/>
<br/>



### 过滤分组

除了能用`GROUP BY`分组数据外，MySQL还允许过滤分组。规定包括哪些分组、排除哪些分组。
因为`WHERE`过滤指定的是行而不是分组，所以MySQL为此提供了另外的子句——`HAVING`。它非常类似于`WHERE`，事实上，目前为止所学过的所有类型的`WHERE`子句都可以用`HAVING`来替代。唯一差别是`WHERE`过滤行，而`HAVING`过滤分组。

`HAVING`支持所有`WHERE`操作符；
`WHERE`在数据分组前进行过滤，它排除的行不包括在分组中；
`HAVING`在数据分组后进行过滤。

```sql
SELECT id, SUM(score)
FROM table1
GROUP BY id
HAVING SUM(score) >= 200;


SELECT id, SUM(score)
FROM table1
WHERE id > 10
GROUP BY id
HAVING SUM(score) > 200;
```



<br/>
<br/>



### 分组和排序

虽然`GROUP BY`和`ORDER BY`经常完成相同的工作，但它们非常不同。
一般在使用`GROUP BY`子句时，应该也给出`ORDER BY`子句。这是保证数据正确包旭的唯一方法。

```sql
SELECT id, SUM(score)
FROM table1
WHERE id > 10
GROUP BY id
HAVING SUM(socre) > 200
ORDER BY SUM(score) DESC;
```



<br/>
<br/>



### SELECT子句顺序

| 子句 | 说明 | 是否必须使用 |
| - | - | - |
| `SELECT` | 要返回的列或表达式 | 是 |
| `FROM` | 从中检索数据的表 | 仅在从表选择数据时使用 |
| `WHERE` | 行级过滤 | 否 |
| `GROUP BY` | 分组说明 | 仅在按组计算聚集时使用 |
| `HAVING` | 组级过滤 | 否 |
| `ORDER BY` | 输出排序顺序 | 否 |
| `LIMIT` | 要检索的行数 | 否 |




<br/>
<br/>
<br/>




## 子查询

本章介绍什么是子查询以及如何使用它们。


<br/>


### 子查询

`SELECT`语句是SQL的查询。从单个数据库表中检索数据的单条`SELECT`语句是简单查询。

SQL还允许创建**子查询(subquery)**——即嵌套在其它查询中的查询。



<br/>
<br/>



### 利用子查询进行过滤

把一条`SELECT`语句的返回结果用于另一条`SELECT`语句的`WHERE`子句。

```sql
# 将复杂的子查询分解为多行并进行适当缩进，能极大简化子查询的使用
SELECT id, score
FROM table1
WHERE id IN (SELECT Sid
             FROM table2);


SELECT id, name
FROM table1
WHERE id IN (SELECT Sid
             FROM table2
             WHERE Score IN (SELECT Score
                             FROM table3));
```

在`WHERE`子句中使用子查询能够编写出功能很强并且灵活的SQL语句。对于嵌套的子查询数目没有限制，不过在实际使用时由于性能的限制，不建议嵌套太多。
请注意权衡子查询和性能。



<br/>
<br/>



### 作为计算字段使用子查询

使用子查询的另一方法是创建计算字段。
**相关子查询(correlated subquery)**，涉及外部查询的子查询。
逐渐增加子查询来建立查询，用子查询测试和调试查询很有技巧性，特别是在这些语句的复杂性不断增加的情况下更是如此。


```sql
SELECT name,
       score
       (SELECT COUNT(*)
        FROM table2
        WHERE table2.id = table1.id)
FROM table1
ORDER BY name;
```




<br/>
<br/>
<br/>




## 联结表

本章介绍什么是联结，为什么要使用联结，如果编写使用联结的`SELECT`语句。


<br/>
<br/>


### 联结

SQL最强大的功能之一就是能在数据检索查询的执行中**联结(join)**表。

在能够有效地使用联结之前，必须了解关系表以及关系数据库设计的一些基础知识。


<br/>
<br/>


#### 关系表

**外键(foreign key)**，为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

**可伸缩(scale)**，能够不断适应增加的工作量而不失败。设计良好的数据库或应用程序称之为可伸缩性好(scale well)。

关系数据可以有效地存储和方便处理。因此，关系数据库的可伸缩性比菲关系数据库要好。


<br/>
<br/>


#### 为什么要使用联结

分解数据为多个表能更有效地存储，更方便地处理，并且具有更大的可伸缩性。但这些好处是有代价的。

如果数据存储在多个表中，怎样使用单条`SELECT`语句检索出数据？
答案是使用联结。简单地说，联结是一种机制，用来在同一条`SELECT`语句中关联表，因此称之为联结。使用特殊的语法，可以联结多个表返回一组输出，联结在运行时关联表中正确的行。
联结是由MySQL根据需要建立，它存在于查询的执行当中。



<br/>
<br/>



### 创建联结

```sql
SELECT t1_name, t2_name, t2_price
FROM table1, table2
WHERE t1.id = t2.id
ORDER BY t1_name, t2_name;

```

应该保证所有联结都有`WHERE`子句，否则MySQL经返回比想要的数据多得多的数据。

**叉联结(cross join)**，有时我们会听到返回称为叉联结的笛卡尔积的联结类型。


<br/>
<br/>


#### 内部联结

目前为止所用的联结称为**等值联结(equijoin)**，它基于两个表之间的相等测试。这种联结也称为**内部联结**。
其实，对于这种联结可以使用稍微不同的语法来明确指定联结的类型。
ANSI SQL规范首选INNER JOIN语法。

```sql
SELECT t1_name, t2_name, t2_price
FROM table1 INNER JOIN t2
ON t1.id = t2.id;
```


<br/>
<br/>


#### 联结多个表

SQL对一条`SELECT`语句中可以联结的表的数目没有限制，创建联结的基本规则也相同。

MySQL在运行时关联指定的每个表已处理联结，这种处理可能是非常耗费资源的，因此应该仔细，不要联结不必要的表。联结的表越多，性能下降的越厉害。

```sql
SELECT t2_name, t1_name, t2_price, quantity
FROM table1, table2, table3
WHERE table2.id = table1.id
  AND table3.id = table2.id
  AND num = xxx;
```




<br/>
<br/>
<br/>




## 高级联结

本章介绍如何对被联结的表使用表别名和聚集函数。


<br/>
<br/>


### 表别名

SQL还允许给表名起别名:
- 缩短SQL语句；
- 允许在单条SELECT语句中多次使用相同的表；
- 表别名只在查询中使用，与列别名不同，它不返回到客户机。

```sql
SELECT name, contact
FROM table1 AS t1, table2 AS t2, table3 AS t3
WHERE t1.id = t2.id
  AND t3.num = t2.num
  AND id = 'xxx';
```



<br/>
<br/>



### 不同类型的联结

前面使用的称为**内部联结**或**等值联结**，除此之外，还有3中其它联结:
- **自联结**
- **自然联结**
- **外部联结**


<br/>
<br/>


#### 自联结

使用自联结而不用子查询。
自联结通常作为外部语句用来替代从相同表中检索数据时使用的子查询语句。虽然最终结果是相同的，但有时候处理联结比处理子查询快的多。应该试一下两种方法，以确定哪一种的性能更好。

```sql
# 子查询
SELECT prod_id, prod_name
FROM products
WHERE vend_id = (SELECT vend_id
                 FROM products
                 WHERE prod_id = 'xxx')


# 使用联结
# 此查询中的两个表实际上是相同的表
# 虽然这是合法的，但对products的引用具有二义性，因为MySQL不知道你引用的是products表中的哪个实例
# 为了解决此问题，使用了表别名
SELECT p1.prod_id, p2.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id
  AND p2.prod_id = 'xxx';
```


<br/>
<br/>


#### 自然联结

无论何时对表进行联结，应该至少有一列出现在不止一个表中。标准的联结返回所有数据，甚至相同的列多次出现。**自然联结**排除多次出现，使每个列只返回一次。

```sql
# 通配符只对第一个表使用。
# 所有其它列明确列出，所以没有重复的列被检索出来。
SELECT c.* o.order_num, o.order_date,
       oi.prod_id, oi.quantity, OI.item_price
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
  AND oi.order_num = o.order_num
  AND prod_id = 'xxx';
```

事实上，迄今为止我们建立的每个内部联结都是自然联结，很可能我们永远都不会用到不是自然联结的内部联结。


<br/>
<br/>


#### 外部联结

许多联结将一个表中的行与另一个表中的行相关联。但有时候会需要包含没有关联行的那些行。
联结包含了那些在相关表中没有关联行的行，这种类型的联结称为**外部联结**。

在使用`OUTER JOIN`语法时，必须使用`RIGHT`或`LEFT`关键字指定包括其所有行的表。(（RIGHT指出的是OUTER JOIN右边的表，而LEFT指出的是OUTER JOIN左边的表)
外部联结的类型，它们的唯一差别是所关联的表的顺序不同。
- 左外部联结
- 右外部联结

```sql
# 内部联结栗子
SELECT Student.Id, Score.score
FROM Student INNER JOIN Score
ON Student.Id = Score.Sid;

+----+-------+
| Id | score |
+----+-------+
|  1 |  80.0 |
|  1 |  90.0 |
|  1 |  99.0 |
|  2 |  70.0 |
|  2 |  60.0 |
|  2 |  80.0 |
+----+-------+
6 rows in set (0.00 sec)


# 外部联结
# LEFT
SELECT Student.Id, Score.score
FROM Student  LEFT OUTER JOIN Score
ON Student.Id = Score.Sid;

+----+-------+
| Id | score |
+----+-------+
|  1 |  80.0 |
|  1 |  90.0 |
|  1 |  99.0 |
|  2 |  70.0 |
|  2 |  60.0 |
|  2 |  80.0 |
+----+-------+
6 rows in set (0.00 sec)

# RIGHT
SELECT Student.Id, Score.score
FROM Student  RIGHT OUTER JOIN Score
ON Student.Id = Score.Sid;

+------+-------+
| Id   | score |
+------+-------+
|    1 |  80.0 |
|    1 |  90.0 |
|    1 |  99.0 |
|    2 |  70.0 |
|    2 |  60.0 |
|    2 |  80.0 |
| NULL |  80.0 |
| NULL |  80.0 |
| NULL |  80.0 |
+------+-------+
9 rows in set (0.01 sec)
```

MySQL不支持简化字符`*=`和`=*`的使用，这两种操作在其它DBMS中很流行。



<br/>
<br/>



### 带聚合函数的联结

聚合函数用来汇总数据。

```sql
SELECT customers.cust_name, customers.cust_id,
       COUNT(orders.order_num) AS num_ord
FROM customers INNER JOIN orders
  ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;


SELECT customers.cust_name, customers.cust_id,
       COUNT(orders.order_num) AS num_ord
FROM customers LEFT OUTER JOIN orders
  ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;
```



<br/>
<br/>



### 联结和联结条件

联结及其使用要点:
- 注意所使用的联结类型；
- 保证使用正确的联结条件，否则将返回不正确的数据；
- 应该总是提供联结条件，否则会得出笛卡尔积；
- 在一个联结中可以包含多个表，甚至每个联结可以采用不同的联结类型。虽然这样做合法也很有用，但应该在一起测试它们之前，分别测试每个联结。




<br/>
<br/>
<br/>




## 组合查询

本章讲述如何利用`UNION`操作符将多余`SELECT`语句组合成一个结果集。


<br/>
<br/>


### 组合查询

多数SQL查询都只包含从一个或多个表中返回数据的单条`SELECT`语句。MySQL也允许执行多个查询(多条`SELECT`语句)，并将结果作为单个查询结果集返回。这些组合查询通常称为**并**(union)或**符合查询**(compound query)。

有两种基本情况，需要使用组合查询:
- 在单个查询中从不同的表返回类似结构的数据；
- 对单个表执行多个查询，按单个查询返回数据。

组合查询和多个`WHERE`条件。
多数情况下，组合相同表的两个查询完成的工作与具有多个`WHERE`子句条件的单挑插叙完成的工作相同。换句话说，任何具有多个`WHERE`的`SELECT`语句都可以作为一个组合查询给出。

为了使表述简单，本章栗子的组合查询使用相同的表。但是`UNION`组合查询可以使用不同的表。



<br/>
<br/>



### 创建组合查询

可用`UNION`操作来组合数条SQL查询。


<br/>


#### 使用UNION

在各条`SELECT`语句之间放上关键字`UNION`。

```sql
SELECT Sid, score
FROM Score
WHERE score >= 80;

+-----+-------+
| Sid | score |
+-----+-------+
| 01  |  80.0 |
| 01  |  90.0 |
| 01  |  99.0 |
| 02  |  80.0 |
| 03  |  80.0 |
| 03  |  80.0 |
| 03  |  80.0 |
+-----+-------+


SELECT Sid, score
FROM Score WHERE
score IN (60, 70);

+-----+-------+
| Sid | score |
+-----+-------+
| 02  |  70.0 |
| 02  |  60.0 |
+-----+-------+


# 使用UNION组合这两条语句
SELECT Sid, score
FROM Score
WHERE score >= 80
UNION
SELECT Sid, score
FROM Score WHERE
score IN (60, 70);

+-----+-------+
| Sid | score |
+-----+-------+
| 01  |  80.0 |
| 01  |  90.0 |
| 01  |  99.0 |
| 02  |  80.0 |
| 03  |  80.0 |
| 02  |  70.0 |
| 02  |  60.0 |
+-----+-------+
```


<br/>
<br/>


#### UNION规则

在进行UNION操作是有几条规则需要注意:
- `UNION`必须由两条或两条以上的`SELECT`语句组成，语句之间用关键自`UNINO`分隔；
- `UNION`中的每个查询必须包含先沟通的列、表达式或聚集函数；
- 列数据类型必须兼容，类型不必完全相同，但必须是DBMS可以隐含地转换的类型


<br/>
<br/>


#### 包含或取消重复的行

`UNION`从查询结果中自动去除了重复的行，这是`UNION`的默认行为。如果有需要想返回所有匹配行，可使用`UNION ALL`。

```sql
SELECT Sid, score
FROM Score
WHERE score >= 80
UNION ALL
SELECT Sid, score
FROM Score
WHERE score IN (60, 70);

+-----+-------+
| Sid | score |
+-----+-------+
| 01  |  80.0 |
| 01  |  90.0 |
| 01  |  99.0 |
| 02  |  80.0 |
| 03  |  80.0 |
| 03  |  80.0 |
| 03  |  80.0 |
| 02  |  70.0 |
| 02  |  60.0 |
+-----+-------+
```


<br/>
<br/>


#### 对组合查询结果排序

在使用`UNION`组合查询时，只能使用一条`ORDER BY`子句，并且它必须出现在最后一条`SELECT`语句之后。
对于结果集，不存在用一种方式排序一部分，而又用另一种方式排序另一部分，因此不允许使用多条`ORDER BY`子句。

```sql
SELECT Sid, score
FROM Score
WHERE score >= 80
UNION
SELECT Sid, score
FROM Score
WHERE score IN (60, 70)
ORDER BY Sid, score;

+-----+-------+
| Sid | score |
+-----+-------+
| 01  |  80.0 |
| 01  |  90.0 |
| 01  |  99.0 |
| 02  |  60.0 |
| 02  |  70.0 |
| 02  |  80.0 |
| 03  |  80.0 |
+-----+-------+
```




<br/>
<br/>
<br/>




## 全文本搜索

本章将介绍如何使用MySQL的全文本搜索功能进行高级的数据查询和选择。


<br/>
<br/>


### 理解全文本搜索

并非所有引擎都支持全文本搜索(FULLTEXT SEARCH)。
MySQL最常用的两个引擎为MyISAM何InnoDB，前者支持全文本搜索，而后者不支持。

虽然`LIKE`通配符匹配和`REGX`正常匹配非常有用，但存在几个重要的限制:
- **性能**，通配符和正则表达式通常要求MySQL尝试匹配表中所有行(而这些搜索极少使用表索引)。因此，由于被搜索的行数不断增加，这些搜索可能非常耗时；
- **明确控制**，使用通配符和正则表达式匹配，很难明确地控制什么和不匹配什么；
- **智能化的结果**，虽然基于通配符和正则表达式的搜索提供了非常灵活的搜索，但它们都不能提供一种智能化的选择结果的办法。

所有这些限制以及更多的限制都可用全文本搜索来解决。在使用全文本搜索时，MySQL不需要分别查看每个行，不需要分别分析和处理每个词。MySQL创建指定列中各词的一个索引，搜索可以针对这些词进行。这样，MySQL可以快速有效地决定哪些词匹配，哪些词不匹配，匹配的频率...


<br/>
<br/>


### 使用全文搜索

为了进行全文搜索，必须索引被搜索的列，而且要随着数据的改变不断地重新索引。在对表列进行适当设计后，MySQL会自动进行所有的索引和重新索引。
索引之后，`SELECT`可与`Match()`和`Against()`一起使用以实际执行搜索。


<br/>


#### 启用全文本搜索

```sql
# 创建MyISAM引擎表
CREATE TABLE fulltextSearch(
  note_id int NOT NULL AUTO_INCREMENT,
  prod_id char(10) NOT NULL,
  note_date datetime NOT NULL,
  note_text text NULL,
  PRIMARY KEY(note_id),
  FULLTEXT(note_text)
  ) ENGINE=MyISAM CHARSET=utf8;

```
不要再导入数据时使用`FULLTEXT`。
更新索引要花时间，虽然不是很多，但毕竟要花时间。如果正在导入数据到一个新表，此时不应该启用`FULLTEXT`索引。应该先导入所有数据，再修改表，定义`FULLTEXT`。


<br/>
<br/>

#### 进行全文本搜索

在索引之后，使用两个函数`Match()`(指定搜索的列)，`Against()`(指定要使用的搜索表达式)执行全恩搜索。

```sql
SELECT note_text,
       Match(note_text)
       Against('salah') AS rank
FROM fulltextSearch;
```


<br/>
<br/>


#### 使用查询扩展

查询扩展用来设法放宽所返回的全文本搜索结果的范围。

在使用查询扩展时，MySQL对数据和索引进行两边扫描来完成搜索:
- 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行；
- 其次，MySQL检查这些匹配行并选择所有有用的词；
- 再其次，MySQL再次进行全文本搜索，这次不仅使用原来的条件，而且还是用所有有用的词。

利用查询扩展，能找出可能相关的结果，即使它们并不精确包含所查找的词。

```sql
# 简单栗子，没有扩展查询
SELECT note_text
FROM table1
WHERE Match(note_text) Against('xxx');


# 使用扩展查询
SELECT note_text
FROM table1
WHERE Match(note_text) Against('xxx' WITH QUERY EXPANSION);
```

表中的行越多，使用查询扩展返货的结果越好。


<br/>
<br/>


#### 布尔文本搜搜

MySQL支持全文本搜索的**布尔方式**(bollean mode)。
布尔方式即使某有`FULLTEXT`索引也可以使用。但这是一个非常缓慢的操作。

以布尔方式，可提供关于如下内容的细节:
- 要匹配的词；
- 要排斥的此；·
- 排列提示；
- 表达式分组；
- 另外一些内容。

```sql
SELECT note_text
FROM table1
WHERE Match(note_text) Against('xxx' IN BOOLEAN NODE);
```

<br>

全文本布尔操作符:

| 布尔操作符 | 说明 |
| - | - |
| `+` | 包含，词必须存在 |
| `-` | 排除，词必须不出现 |
| `>` | 包含，而且增加等级值 |
| `<` | 包含，且减少等级值 |
| `()` | 把此排成子表达式 |
| `~` | 取消一个此的排序值 |
| `*` | 词尾的通配符 |
| `""` | 定义一个短语 |

```sql
-- 栗子
-- 搜索包含rabbit和bait的行
SELECT note_text
FROM table1
WHERE Match(note_text) Against('+rabbit +bait' IN BOOLEAN MODE);

-- 没有指定操作符，匹配包含rabbit和bait中的至少一个词的行
SELECT note_text
FROM table1
WHERE Match(note_text) Against('rabbit bait' IN BOOLEAN MODE);

-- 匹配rabbit bait短语而不是两个词
SELECT note_text
FROM table1
WHERE Match(note_text) Against('"rabbit bait"' IN BOOLEAN MODE);
```


<br/>
<br/>


#### 全文本搜索的使用说明

全文本搜索的某些重要说明:
- 在索引全文本数据时，短词(3个或3个以下字符的词)被忽略且从索引中排除；
- MySQL带有一个內建的费用次(stopword)列表，这些词在索引全文本数据时总是被忽略。如果需要，可覆盖这个列表；
- 许多次出现的频率很高，搜索它们没有用处。因此MySQL规定了一条`50%`规则，如果一个词出现在`50%`以上的行中，则将它作为一个非用词忽略。此规则不用于布尔方式；
- 如果表中的函数少于3行，则全文本搜索不反悔结果(因为每个词或者不出现，或者至少出现在50%的行中)；
- 忽略词中的单引号；
- 不具有词分隔符的语言不能恰当地返回全文本搜索结果；
- 仅在MyISAM引擎中支持全文本搜索




<br/>
<br/>
<br/>




## 插入数据

本章介绍如何利用SQL的`INSERT`语句将数据插入表中。


<br/>


### 数据插入

`INSERT`是用来插入(添加)行到数据库表的。插入有几种方式:
- 插入完整的行；
- 插入行的一部分；
- 插入多行；
- 插入某些查询的结果。



<br/>
<br/>



### 插入完整的行

各个列必须以它们在表定义中出现的次序填充。

虽然这种语法很简单，但并不安全，应该尽量避免使用。下面的SQL语句高度依赖于表中列的定义次序，并且还依赖于其次序容易获得的信息。即使可得到这种次序信息，也不能保证下一次表结构变动后各个列完全保持相同的次序，因此，编写依赖于特定列次序的SQL语句是很不安全的。

```sql
INSERT INTO table1
VALUES(NULL,
       'aaaa A, AA',
       'abCD',
       'ChengDu',
       '12345',
       'xxx',
       NULL);

# INSERT语句一般不会产生输出
# 由于第一列id是自增，所以可以不给出值
```

<br>

更安全`INSERT`语句:

```sql
INSERT INTO table1(
  name,
  address,
  city,
  state,
  contact,
  email
)
VALUES(
  'zhang',
  'HighTech',
  'Chengdu',
  'SC',
  '15566668888',
  'xxx@example.com'
);
```

因为提供了列名，VALUES必须以其指定的次序匹配指定的列名，不一定按各个列出现在表中的次序。
其优点是，即使表的结构发生改变，此`INSERT`语句仍然能够正确工作。你会发现id列的`NULL`值是不必要的，id列并没有出现在列表中，所以不需要任何值。
当然，你也可以给出列名，并给出其`NULL`值。

```sql
INSERT INTO table1(
  name,
  contact,
  email,
  address
)
VALUES(
  'ZHANG',
  NULL,
  NULL,
  'HighTect'
);
```

<br>

总使用列的列表。
一般不要使用没有明确给出列的列表的`INSERT`语句。使用列表能使SQL代码继续发挥作用，即使表结构发生了变化。

仔细地给出值。
不管使用哪种`INSERT`语法，都必须给出VALUES的正确数目。如果不提供列名，则必须给每个表列提供一个值。如果提供列名，则必须对每个列出的列给出一个值。否则，将产生行插入不成功的错误消息。

省略列。
如果表的定义允许，则可以在`INSERT`操作中省略某些列。省略的列必须满足以下某个条件:
- 该列定义为允许`NULL`值；
- 在表定义中给出默认值。如果不给出值，将使用默认值；

提高整体性能。
`INSERT`操作可能很耗时(特别是有很多索引需要更新时)，而且它可能降低等待处理的`SELECT`语句的性能。
如果数据检索是最重要的，你可通过在`INSERT`和`INTO`之间添加关键字`LOW_PRIORITY`，指示MySQL降低`INSERT`语句的优先级。这同样也适用于`UPDATE`和`DELETE`语句。



<br/>
<br/>



### 插入多行

`INSERT`可以插入一行或多行到一个表中。
有两种方式:
- 使用多条`INSERT`语句；
- 一条`INSERT`的多个值

```sql
# 多条
INSERT INTO table1(
  name,
  address,
  city,
  state
)
VALUES(
  'name1',
  'hightec',
  'cd',
  'sc'
);
INSERT INTO table1(
  name,
  city,
  state
)
VALUES(
  'name2',
  'cd',
  'sc'
)


# 单条
INSERT INTO table1(
  name,
  address,
  city,
  state,
  country
)
VALUES(
  'NAME1',
  'HIGHTEC',
  'CD',
  'SC',
  'CN'
),
      (
  'NAME2',
  'HIGHTEC',
  'CD',
  'SC',
  'CN'
);
```

提高`INSERT`的性能。
此技术可提高数据库处理的能力，因为MySQL用单条`INSERT`语句处理多个插入比使用多条`INSERT`语句快。




<br/>
<br/>



### 插入检索的数据

`INSERT`可将一条`SELECT`语句的结果插入表中，这称为**INSERT SELECT**，顾名思义，它由一条`INSERT`语句和一条`SELECT`语句组成。

```sql
INSERT INTO table1(
  id,
  contact,
  email,
  name,
  address,
  city,
  state,
  country
)
SELECT id,
       contact,
       email,
       name,
       address,
       city,
       state,
       country
FROM table2
WHERE name = 'zhang21';
```

`INSERT SELECT`中的列名，为了简单起见，此例子中使用了相同的列名。但其实不一定要求列名匹配。




<br/>
<br/>
<br/>




## 更新和删除数据

本章介绍如何利用`UPDATE`语句和`DELETE`语句进一步操控表数据。


<br/>


### 更新数据

有两种方式使用`UPDATE`:
- 更新表中特定行；
- 更新表中所有行。

基本的`UPDATE`语句由3部分组成:
- 要更新的表；
- 列名和它们的新值；
- 确定要更新行的过滤条件。

<br>

不要省略`WHERE`子句。
在使用`UPDATE`时一定要注意细心。因为稍有不注意，就会更新表中的所有行。

`UPDATE`与安全。
可以限制和控制`UPDATE`语句的使用。

<br>

```sql
-- 更新单列
UPDATE table1
SET email = 'beef@meat.com'
WHERE id = 123 AND name = 'beef';


# 更新多列
UPDATE table1
SET email = 'milk@drink.com', address = 'hightec'
WHERE id = 123;


# 为了删除某个列的值，可将其设置为NULL
UPDATE table1
SET email = NULL
WHERE id = 123;
```

<br>

在`UPDATE`语句中使用子查询。
`UPDATE`语句可以使用子查询，使得能用`SELECT`语句检索出的数据更新列数据。

```sql
UPDATE table1
SET zip = (SELECT zip FROM table2 WHERE id = 111)
WHERE id = 111;

UPDATE table1
SET zip = '646100'
WHERE id = (SELECT id FROM table2 WHERE zip = '646100');
```

<br>

`IGNORE`关键字。
如果用`UPDATE`语句更新多行，并且在更新这些行中的一行或多行时出现一个错误，则整个`UPDATE`操作被取消。而使用`IGNORE`关键字，这可以忽略错误，继续执行。

```sql
UPDATE IGNORE table1 ...
```



<br/>
<br/>



### 删除数据

有两种方式使用`DELETE`:
- 从表中删除特定的行；
- 从表中删除所有行。

<br>

不要省略`WHERE`子句。
在使用`DELETE`时一定要注意细心。因为稍不注意，就会错误地删除表中所有行。

`DELETE`与安全。
可以限制和控制`DELETE`语句的使用。

删除表的内容而不是表。
`DELETE`语句从表中删除行，甚至是表中所有行。但是，`DELETE`不删除表本身。

更快地删除。
如果想从表中删除所有行，不要使用`DELETE`。可使用`TRUNCATE TABLE`语句，它完成相同的工作，但速度更快。(它实际是删除原来的表并重新创建一个表，而不是逐行删除表中的数据)。

<br>

```sql
DELETE FROM table
WHERE id = 123 AND name = 'beef';
```

`DELETE`不需要列名或通配符，以为它删除整行而不是删除列。为了删除指定的列，请使用`UPDATE`语句。



<br/>
<br/>



### 更新和删除的指导原则

`UPDATE`语句和`DELETE`语句都使用了`WHERE`子句，这样做的理由很充分。如果省略了`WHERE`子句，则`UPDATE`或`DELETE`操作将应用到表中所有行。

下面是SQL使用者在使用`UPDATE`和`DELETE`语句时应该遵循的习惯:
- 请一定使用带`WHERE`子句的`UPDATE`或`DELETE`语句；
- 保证每个表都有主键，尽可能像`WHERE`子句那样使用它；
- 在对`UPDATE`或`DELETE`语句使用`WHERE`子句前，应先使用`SELECT`进行测试，以保证它过滤的是正确的记录；
- 使用强制实施完整性的数据库，这样MySQL将不允许删除具有与其它表相关联的数据的行；
- 一定要小心操作，MySQL没有撤销按钮。




<br/>
<br/>
<br/>




## 创建和操作表

本章介绍表的创建、更改和删除的基本知识。


<br/>


### 创建表

一般有两种创建表的方法:
- 使用交互式创建和管理表的工具；
- 直接使用MySQL语句操纵。

<br/>

利用`CREATE TABLE`语句创建表，必须给出下列信息:
- 新表的名字；
- 表列的名字和定义。

```sql
-- HELP CREATE TABLE;

# CREATE TABLE tablename IF NOT EXISTS
CREATE TABLE tablename
(
  id      int        NOT NULL AUTO_INCREMENT,
  name    char(50)   NOT NULL ,
  address char(50)   NULL ,
  country char(20)   NULL DEFAULT CN,
  email   char(255)  NULL ,
  texts   text NULL ,
  PRIMARY KEY (id),
  FULLTEXT(texts)
) ENGINE=InnoDB CHARSET=utf8mb4;

```

<br>

关于上面创建表语句的一些解释:
- MySQL忽略空格，所以建议进行适当的缩进(格式化)以方便阅读；
- 在创建新表时，指定的表名必须不存在，否则将出错；
- 如果你仅想创建一个不存在的表， 应该在表后面给出`IF NOT EXISTS`;
- `NULL`值就是没有值或缺值
  - 允许`NULL`值的列也允许在插入行时不给出该列的值；
  - 不允许`NULL`值的列不接受没有值的行，也就是必须要有值。
- 注意理解`NULL`值和空串(`''`，两个单引号，中间没有字符)。空串是一个有效的值，而不是无值；
- 主键必须唯一
  - 如果主键使用单个列，则它的值必须唯一；
  - 如果主键使用多个列，则这些列的组合值必须唯一；
  - 主键中只能使用不允许NULL值的列。允许NULL值的列不能作为唯一标识。
- 自增(`AUTO_INCREMENT`)告诉MySQL，本列每当增加一行 时自动增量。每个表只允许一个自增列，而且它必须被索引。当然，你也可以在插入语句中指定它的值，只要这个值是唯一的即可。后续的增量将开始使用该手工插入的值。让MySQL生成主键的一个缺点就是你不知道这些值是谁；
- 指定默认值。如果在插入行时没有给出值，MySQL允许指定此时使用的默认值。使用`DEFAULT`关键字指定。默认值只支持常量；
- 数据库引擎。如果忽略，则默认为InnoDB
  - InnoDB是一个可靠地事务处理引擎；
  - MEMORY在功能等同于MyISAM，速度很快(特别适合临时表)；
  - MyISAM是一个性能极高的引擎，它支持全文搜索。



<br/>
<br/>



### 更新表

为更新表定义，可使用`ALTER TABLE`语句。但是，理想状态下，当表中存储数据以后，该表就不应该再被更新。在表的设计过程中需要花费大量时间来考虑，以便后期不贵该表进行大的改动。

小心使用`ALTER TABLE`。
应该在进行表结构改动之前做一个完整的备份。数据表的更改不能撤销。如果增加了不需要的列，可能不能删除它们。类似地，如果删除了不应该删除的列，可能会丢失该列中的所有数据。

使用`ALTER TABLE`必须给出以下信息:
- 必须存在的表名；
- 所做更改的列表。

```sql
-- HELP ALTER TABLE;

# 给表添加一个列
ALTER TABLE tablename
ADD add_line1 CHAR(20) NULL ,

# 删除表列
ALTER TABLE tablename
DROP COLUMN add_line1;
```

<br>

ALTER TABLE的一种常见用途是定义外键。

```sql
ALTER TABLE orderitems
ADD CONSTRAINT fk_orderitems_orders
FOREIGN KEY(order_num) REFERENCE orders(order_num);

ALTER TABLE orders
ADD CONSTRAINT fk_orders_customers
FOREIGN KEY(cust_id) REFERENCE customers(cust_id);
```

<br>

复杂的表结构一般需要手动删除过程，它涉及:
- 用新的列布局创建一个新表；
- 使用`INSERT SELECT`语句从旧表复制数据到新表；
- 检验包含所需数据的新表；
- 重命名旧表；
- 用旧表原来的名字重命名新表；
- 根据需要，重新创建触发器、存储过程、索引和外键。



<br/>
<br/>



### 删除表

删除表(删除整个表而不是其内容)，使用`DROP TABLE`语句。
删除表没有确认，也不能撤销，执行这条语句将会永久删除该表。

```sql
-- HELP DROP TABLE;

DROP TABLE tablename;
```



<br/>
<br/>



### 重命名表

使用`RENAME TABLE`语句可以重命名一个表。

```sql
-- HELP RENAME TABLE;

RENAME TABLE oldname TO newname;

# 重命名多个表
RENAME TABLE oldname1 TO newname1,
             oldname2 TO newname2;
```




<br/>
<br/>
<br/>




## 视图

本章将介绍视图是什么，它们怎样工作，何时使用它们。


<br/>


### 视图

**不建议对视图进行更新，因为视图主要用于数据检索，而非更改。**

**视图(view)**是虚拟的表。与包含数据的表不一样，视图只包含使用时动态检索数据的查询。

来看个栗子:
```sql
/* 任何需要这个数据的人都必须理解相关表的结构，
   并且知道如何查询和对表进行联结。 */
SELECT cust_name, cust_contact
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num
  AND prod_id = 'TNT2';

/* 假如把整个查询包装成一个名为productcustomers的虚拟表，则可轻松检索出相同的数据。
   作为视图，它不包含表中应该有的任何列或数据，它包含的是一个SQL查询。 */
SELECT cust_name, cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';
```


<br/>


#### 为什么使用视图

视图的常见应用:
- 重用SQL语句；
- 简化复杂的SQL操作。在编写查询后，可以方便地重用它而不必知道它的基本查询细节；
- 使用表的组成部分而不是整个表；
- 保护数据。可以给用户授予表的特定部分的访问权限而不是整个表的访问权限；
- 更改数据格式和表示。视图可返回与底层表的标识和格式不同的数据。

在视图创建之后，可以用与表基本相同的方式利用它们。可以对视图执行`SELECT`操作，过滤和排序，将视图联结到其它视图或表，甚至还能添加和更新数据。

重要的是知道视图仅仅是用来查看存储在别处的数据的一种设施。视图本身不包含数据，因此它们返回的数据是从其它表中检索出来的。在添加或更改这些表中的数据时，视图将返回改变过的数据。

> 性能问题
> 因为视图不包含数据，所以每次使用视图时，都必须处理查询执行时所需的任一个检索。如果你用多个联结和过滤创建了复杂的视图或嵌套了视图，可能会发现性能下降得很厉害。因此，在部署使用了大量视图的应用前，应该进行测试。


<br/>
<br/>


#### 视图的规则和限制

视图创建和使用的一些最常见的规则和限制:
- 视图必须唯一命名(不能与其它视图或表同名)；
- 对于可以创建的视图数目没有限制；
- 为了创建视图，必须具有足够的访问权限；
- 视图可以嵌套，即可从其它视图中检索数据的查询来构造一个视图；
- `ORDER BY`可用在视图中，但如果该视图检索数据`SELECT`中也含有`ORDER BY`，那么该视图中的`ORDER BY`将被覆盖；
- 视图不能索引，也不能有关联的触发器或默认值；
- 视图可以和表一起使用。



<br/>
<br/>



### 使用视图

- 使用`CREATE VIEW viewName`创建视图；
- 使用`SHOW CREATE VIEW viewName`查看创建视图的语句；
- 使用`DROP VIEW viewName`删除视图；
- 更新视图时，可以先用`DROP`再用`CREATE`，也可以直接使用`CREATE OR REPLACE VIEW`。



<br/>
<br/>



### 利用视图简化复杂的联结

视图最常见的应用之一是隐藏复杂的SQL，这通常会涉及联结。

```sql
-- 这是不使用AS会报错
CREATE VIEW productcustomers AS
SELECT cust_name, cust_contact, prod_id
FROM customers, orders, orderitems
WHERE customers.cust_id = order.cust_id
  AND orderitems.order_num = orders.order_num;


# 查看视图
SHOW VIEW productcustomers;

# 此视图包含上面检索出来的三列内容
DESCRIBE productcustomers;
SHOW COLUMNS FROM productcustomers;


# 检索的结果也是这三列
SELECT * FROM productcustomers;
# 这里注意，如果创建视图中使用的源表数据发生更新或变动，则视图也会相应的发生改变
```


<br/>


#### 用视图重新格式化检索出的数据

视图的另一常见用途是重新格式化检索出的数据。

```sql
-- 例如要经常使用拼接列
SELECT Concat(RTrim(name), '(', RTrim(contry), ')') AS title
FROM table1
ORDER BY name;

-- 不必在每次需要时执行联结，创建一个视图每次需要时使用它即可
CREATE VIEW viewLocation AS
SELECT Concat(RTrim(name), '(', RTrim(country), ')') AS title
FROM table1
ORDER BY name;

-- 检索
SELECT *
FROM viewLocation;
```


<br/>
<br/>


#### 使用视图过滤不想要的数据

视图对于应用普通的`WHERE`子句也很有用。

如果从视图检索数据时使用了`WHERE`子句，则两组`WHERE`子句(传递给视图的和视图自身的)将自动组合。

```sql
-- 栗子
CREATE VIEW customerEmailList AS
SELECT id, name, email
FROM table1
WHERE email IS NOT NULL;


SELECT *
FROM customerEmailList
WHERE id = 111;
```


<br/>
<br/>


#### 使用视图与计算字段

视图对于简化计算字段的使用特别有用。

```sql
-- 栗子
CREATE VIEW viewTotal AS
SELECT id,
       num,
       quantity,
       item_price,
       quantity*item_price AS total_price
FROM table1;

SELECT *
FROM viewTotal
WHERE id = 123;
```


<br/>
<br/>


#### 更新视图

通常，视图是可更新的(即对它使用`INSERT`, `UPDATE`, `DELETE`)。更新一个视图将更新其基表。如果你对视图增加或删除行，实际上是对其基表增加或删除行。

但是，并非所有视图都是可更新的。如果MySQL不能正确地确定被更新的基数据，则不允许更新、插入和删除。如果视图定义中有以下操作:
- 分组(`GROUP BY, HAVING`)；
- 联结；
- 子查询；
- 并；
- 聚集函数(`MIN(), COUNT()...`)；
- `DICTINCT`;
- 导出(计算)列。

**不建议对视图进行更新，因为视图主要用于数据检索，而非更改。**




<br/>
<br/>
<br/>




## 存储过程

本章介绍什么是存储过程，为什么要使用存储过程以及如何使用存储过程...


<br/>


### 存储过程

迄今为止，使用的大多数SQL语句都是以针对一个/多个表的单条语句。并非所有操作都这么简单，经常会有一个完整的操作需要多条语句才能完成。

可以创建**存储过程**。存储过程简答来说，就是为以后的使用而保存的一条或多条MySQL语句的集合。可将其视为批文件，虽然它们的作用不仅限于批处理。



<br/>
<br/>



### 为什么要使用存储过程

使用它的一些主要理由:
- 通过把处理封装在容易使用的单元中，简化复杂的操作；
- 由于不要求反复建立一系列处理步骤，这保证了数据的完整性；
- 简化对变动的管理；
- 提高性能；
- 存在一些职能用在单个请求中的MySQL元素和特性，存储过程可以使用它们来编写功能更强更灵活的代码；
- 一般来说，存储过程的编写比基本SQL语句更复杂，编写存储过程需要更高的技能，更丰富的经验；
- 你可能没有创建存储过程的安全访问权限。

MySQL将编写存储过程与执行存储过程的安全和访问分开来。即使你没有权限编写存储过程，也可在适当的时候执行别的存储过程。



<br/>
<br/>



### 使用存储过程

使用存储过程需要知道如何执行和创建它们。


<br/>


#### 创建存储过程

```sql
-- HELP CREATE PROCEDURE;
CREATE PROCEDURE procedureTest()
BEGIN
  SELECT Avg(price) AS priceAvg
  FROM table1;
END;
# BEGIN, END语句用来限定存储过程体
```

<br>

**注意**
MySQL命令行的分隔符，如果使用MySQL命令行实用程序，请仔细阅读此说明。

默认的MySQL语句分隔符为`;`，MySQL命令行实用程序也是用`;`作为语句分隔符。如果命令行实用程序要解释存储过程自身内的`;`字符，则它们最终不会成为存储过程的成分，这会使存储过程的SQL出现语法错误。
解决办法是临时修改命令行实用程序的语句分隔符。

```sql
-- 除了\(转义字符)，其它都可作为语句分隔符
-- 临时修改MySQL分隔符
DELIMITER //

CREATE PROCEDURE procedureTest()
BEGIN
  SELECT Avg(price) AS priceAvg
  FROM table1;
END //

-- 恢复MySQL默认分隔符
DELIMITER ;
```


<br/>
<br/>


#### 执行存储过程

`CALL`语句接受存储过程以及需要传递给它的参数。

存储过程实际上是一种函数，所以后面需要有括号`()`。

```sql
-- 栗子
CALL procedureTest();

```


<br/>
<br/>


#### 删除存储过程

```sql
-- 如果过程不存在，则删除会产生一个错误
DROP PROCEDURE procedureTest;

```


<br/>
<br/>


#### 使用参数

一般，存储过程并不显示结果，而是把结果返回给你指定的变量。
变量是内存中一个特定的位置，用来临时存储数据。MySQL的变量都必须以`@`开始。

MySQL支持三种类型的参数:
- `IN`，传递给存储过程
- `OUT`，从存储过程传出
- `INOUT`，对存储过程传入和传出

```sql
-- 此存储过程接受三个参数
CREATE PROCEDURE procedureTest2(
  OUT pl DECIMAL(8, 2),
  OUT ph DECIMAL(8, 2),
  OUT pa DECIMAL(8, 2)
)
BEGIN
  SELECT Min(price) INTO pl FROM table1;
  SELECT Max(price) INTO ph FROM table1;
  SELECT Avg(price) INTO pa FROM table1;
END;


-- 执行
CALL procedureTest2(@pricelow,
                    @pricehigh,
                    @priceaverage);
SELECT @pricelow, @pricehigh, @priceaverage;



-- 另一个栗子
CREATE PROCEDURE orderTotal(
  IN onumer INT,
  OUT ototal DECIMA(8, 2)
)
BEGIN
  SELECT Sum(item_price*quantity)
  FROM orderitems
  WHERE order_num = onumber
  INTO ototal;
END;

-- 执行
CALL orderTotal(12345, @total);
SELECT @total;
```


<br/>
<br/>


#### 建立智能存储过程

在存储过程内包含业务规则和智能处理时，它们的威力才真正显现出来。

```sql
# 栗子
/* Name: ordertotal
   Parameter: onumber = order number
              taxable = 0 if not taxable, 1 if taxable
              ototal = order total variable */

CREATE PROCEDURE ordertotal(
  IN onumber INT,
  IN taxable BOOLEAN,  #布尔值1(true), 0(false)
  OUT ototal DECIMAL(8 , 2)
) COMMENT 'Obtain order total, optinally adding tax.'
BEGIN
  -- Declare variable for total
  DECLARE total DECIMAL(8, 2);
  -- Declare tax percentage
  DECLARE taxrate INT DEFAULT 6;

  -- Get the order total
  SELECT Sum(item_price*quantity)
  FROM orderitems
  WHERE order_num = onumber
  INTO total;

  -- Is this taxable?
  IF taxable THEN
    -- Yes, so add taxrate to the total
    SELECT total+(total/100*taxrate) INTO total;
  END IF;

  -- And finally, save to out varible
  SELECT total INTO ototal;
END;


-- 执行
CALL ordertotal(12345, 0, @total);
SELECT @total;
```


<br/>
<br/>


#### 检查存储过程

```sql
-- 显示创建存储过程
SHOW CREATE PROCEDUTE xxx;


-- 获得存储过程详细信息
SHOW PROCEDURE STATUS;
SHOW PROCEDURE STATUS LIKE 'procedureTest1';
```




<br/>
<br/>
<br/>




## 游标

本章将介绍任何是游标，如何使用游标。


<br/>


### 游标

MySQL检索操作返回一组称为结果集的行。但没有办法得到某些行，也不存在每次一行地处理所有行的简单方法。
有时，需要在检索出来的行中前进或后退一行或多行。这就是使用游标(cursor)的原因。

**游标(cursor)**，是一个存储在MySQL服务器上的数据库查询，它不是一条`SELECT`语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。
游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对其数据进行浏览或做出更改。
不像其它DBMS，MySQL的游标只能用于存储过程(和函数)。



<br/>
<br/>



### 使用游标

使用游标的几个明确步骤:
- 在使用游标前，必须声明它(`DECLARE`)。此过程并没有检索数据，它只是定义要使用的`SELECT`语句；
- 一旦声明后，必须打开(`OPEN`)游标以供使用。此过程用前面定义的`SELECT`把数据检索出来；
- 对于填有数据的游标，根据需要取出各行；
- 在结束游标使用时，必须关闭(`CLOSE`)游标。


<br/>


#### 创建游标

`DECLARE`语句创建游标，并定义相应的`SELECT`语句，根据需要带`WHERE`和其它子句。

```sql
-- 游标栗子

CREATE PROCEDURE procedureTest()
BEGIN
  DECLARE numberCursor CURSOR
  FOR
  SELECT number FROM table1;
END;
```


<br/>
<br/>


#### 打开和关闭游标

```sql
-- 打开游标
-- 在处理OPEN语句时执行查询，存储检索出的数据以供浏览和滚动
OPEN numberCursor;

-- 关闭游标
-- CLOSE释放游标使用的所有内部内存和资源，因此在每个游标不再需要时都应该关闭
CLOSE numberCursor;

-- 一个游标关闭后，如果没有重新打开，则不能使用它
```

如果你不明确关闭游标，MySQL将会在到达`END`语句是自动关闭它。

```sql
-- 此存储过程声明、打开和关闭一个游标，但对检索出的数据什么也没做。

CREATE PROCEDURE procedureTest()
BEGIN
  DECLARE numberCursor CURSOR
  FOR
  SELECT number FROM table1;

  -- OPEN
  OPEN numberCursor;

  -- CLOSE
  CLOSE numberCursor;
END;

```


<br/>
<br/>


#### 使用游标数据

游标打开后，可使用`FETCH`语句分别访问它的每一行。它指定检索什么数据，检索出的数据存储在什么地方。它还向前移动游标的内部行指针，是下一条`FETCH`语句检索下一行。

```sql
CREATE PROCEDURE procedureTest()
BEGIN
  DECLARE o INT;
  DECLARE numberCursor CURSOR
  FOR
  SELECT number FROM table1;

  OPEN numberCursor;

  FETCH numberCursor INTO o;

  CLOSE numberCursor;
END;
```




<br/>
<br/>
<br/>




## 触发器

本章介绍什么是触发器，为什么要使用触发器，如何使用触发器。


<br/>


### 介绍

MySQL语句在需要时被执行，存储过程也是如此。但是，如果你想要某条语句(某些语句)在事件发生时自动执行，怎么办？例如:

- 检查每次新增的电话号码格式是否正确
- 检查大小写
- 每卖出一个产品时，都从库存中减去订购的数量
- 无论何时删除一行，都在某个存档表中保留一个副本

这些例子的共同之处是它们都需要在某个表发生更改时自动处理。这确切地说就是**触发器(trigger)**。
触发器是MySQL响应一下任意语句而自动执行的一条MySQL语句(或位于`BEGIN`和`END`语句之间的一组语句):

- `DELETE`
- `INSERT`
- `UPDATE`

其它MySQL语句不支持触发器。

**只有表才支持触发器。**



<br/>
<br/>



### 创建触发器

创建触发器时，需要给出4条信息:

- 唯一的触发器名；
- 触发器关联的表；
- 触发器应该响应的活动(`DELETE`, `INSERT`, `UPDATE`)；
- 触发器何时执行(BEFORE或AFTER)。

```sql
-- HELP CREATE TRIGGER;
CREATE TRIGGER triggerTest   -- 创建新触发器
  AFTER INSERT ON table1     -- 触发器将在INSERT 表table1成功执行后执行
  FOR EACH ROW               -- 对每个插入行执行
  SELECT 'table1 added' INTO @triggerInfo;    -- 对每个插入显示一次，MySQL v5+ 需要使用变量来实现


# 查看
SHOW TRIGGERS;
```



<br/>
<br/>



### 删除触发器

触发器不能更新或覆盖。为了修改一个触发器，必须先删除然后重新创建。

```sql
DROP TRIGGER triggerTest;
```



<br/>
<br/>



### 使用触发器

三种触发器:
- INSERT
- UPDATE
- DELETE


<br/>


#### INSERT触发器

INSERT触发器在`INSERT`语句执行之前(`BEFORE`)或之后(`AFTER`)执行。

- 在INSERT触发器代码内，可引用一个名为NEW的虚拟表，访问被插入的行；
- 在BEFORE INSERT触发器中，NEW中的值也可被更新(允许更改被插入的值)；
- 对于`AUTO_INCREMENT`列，NEW在INSERT执行之前包含0，在`INSERT`执行之后包含新的自动生成值。


<br/>
<br/>


#### UPDATE触发器

UPDATE触发器在`UPDATE`语句执行之前或之后执行。

- 在UPDATE触发器代码中，你可引用一个名为OLD的虚拟表访问以前(`UPDATE`语句前)的值，引用一个名为NEW的虚拟表访问新更新的值；
- 在BEFORE UPDATE触发器中，NEW中的值也可被更新(允许更改将要用于`UPDATE`语句的值)；
- OLD中的值全都是只读的，不能更新。

```sql
-- 以下保证name总为大写
CREATE TRIGGER updateTable1
  BEFORE UPDATE ON table1
  FOR EACH ROW
  SET NEW.name = UPPER(NEW.name)


-- 测试
UPDATE table1
SET name = 'NAme1'
WHER id = 1;

SELECT name
FROM table1
WHERE id = 1;
+-------+
| name  |
+-------+
| NAME1 |
+-------+
```


<br/>
<br/>


#### DELETE触发器

DELETE触发器在`DELETE`语句执行之前或之后执行。

- 在DELETE触发器代码内，你可引用一个名为OLD的虚拟表，访问被删除的行；
- OLD中的值全都是只读的，不能更新。

```sql
-- 使用OLD保存将要被删除的行到一个备份表
-- 你需要创建一个与table1相同列的table1BAK表
DELIMITER //

CREATE TRIGGER deletetable1
  BEFORE DELETE ON table1
  FOR EACH ROW
BEGIN
  INSERT INTO table1BAK(id, name, date)
  VALUES(OLD.id, OLD.name, OLD.date);
END //

DELIMITER ;
```


<br/>
<br/>


#### 小技巧

- 创建触发器可能需要特殊的安全访问权限，但是，触发器的执行是自动的。如果INSERT, UPDATE, DELETE能够执行，则触发器也能执行；
- 应该使用触发器来保证数据的一致性(大小写、格式...)；
- 触发器的一种非常有意义的使用是创建审计跟踪；
- MySQL触发器中不支持`CALL`语句，这表示触发器不能调用存储过程。




<br/>
<br/>
<br/>




## 事务处理

本章介绍什么是事务处理，如何利用`COMMIT`和`ROLLBACK`语句来管理事务处理。


<br/>


### 介绍

并非所有存储引擎都支持事务处理。
MyISAM不支持明确的事务处理管理 ，而InnoDB支持。

**事务处理(transaction processing)**，可以用来维护数据库的完整性，它保证成批的MySQL操作要么完全执行，要么完全不执行。

事务处理的几个术语:

- **事务(transaction)**，指一组SQL语句；
- **回退(rollback)**，指撤销指定SQL语句；
- **提交(commit)**，指将未存储的SQL语句结果写入数据库表；
- **保留点(savepoint)**，指事务处理中设置的临时占位符(placeholder)，你可以对它发布回退(与回退整个事务处理不同)。



<br/>
<br/>



### 控制事务处理

管理事务处理的关键在于将SQL语句组分解为逻辑块，并明确规定数据很是应该回退，何时不应该回退。

```sql
-- 标识事务的开始
START TRANSACTION;
```


<br/>
<br/>


#### 使用ROLLBACK

MySQL的`ROLLBACK`命令用来回退(撤销)MySQL语句。它只能在一个事务处理内使用。
事务处理可用来管理`INSERT`, `UPDATE`和`DELETE`语句。你不能回退`CREATE`或`DROP`操作。

```sql
SELECT * FROM talbe1;  -- 显示该表不为空
START TRANSACTION;     -- 开始事务处理
DELETE FROM table1;    -- 删除整个表内容
SELECT * FROM table1;  -- 检索空表
ROLLBACK;              -- 回退开始事务之后的所有语句
SELECT * FROM table1;  -- 检索表，所有信息又回来了
```

隐含地事务关闭。
当`COMMIT`或`ROLLBACK`语句执行后，事务会自动关闭。


<br/>
<br/>


#### 使用COMMIT

一般的MySQL语句都是直接针对数据库表执行和编写的。这就是所谓的**隐含提交(implicit commit)**，即提交操作是自动进行的。

但在事务处理块中，提交不会隐含地进行。为进行明确的提交，使用`COMMIT`语句。

```sql
START TRANSACTION;
DELETE FROM table1 WHERE id = 2010;
DELETE FROM table2 WHERE id = 2010;
COMMIT;    -- 仅在不出错时写入更改，如果其中一条语句出错，则DELETE不会提交，即它自动撤销。
```


<br/>
<br/>


#### 使用SAVEPOINT

简单的`ROLLBACK`和`COMMIT`语句就可以写入或撤销整个事务处理。更复杂的事务处理可能需要部分提交或回退。

为了支持回退部分事务处理，必须能在事务处理块中何时的位置放置占位符。这样，如果需要回退，则可回退到某个占位符。
这些占位符称为**保留点**。

```sql
-- 创建占位符
-- 使用唯一名字，以便知道回退到何处
SAVEPOINT detele1;
```

保留点越多越好，这样便能进行灵活地回退。
保留点在事务处理完成(COMMIT或ROLLBACK)后自动释放。也可使用`RELEASE SAVEPOINT`明确地释放保留点。


<br/>
<br/>


#### 更改默认提交行为

默认的MySQL行为是自动提交所有更改。

```sql
-- 你可以设置MySQL不自动提交
-- autocommit标志决定是否自动提交更改，不管有没有COMMIT语句
SET autocommit=0;
```




<br/>
<br/>
<br/>




## 字符集和校对

本章介绍MySQL处理不同字符集和语言的基础知识。


<br/>


### 字符集和校对顺序

数据库被用来存储和检索数据。不同的语言和字符集需要以不同的方式存储和检索。因此，MySQL需要适应不同的字符集，适应不同的排序和检索数据的方法。

重要术语:

- **字符集**，为字母和符号的集合；
- **编码**，为某个字符集成员的内部表示；
- **校对**，为规定字符如何比较的指令。



<br/>
<br/>



### 使用字符集和校对顺序

MySQL支持众多的字符集。不同的表、不同的列口可以指定不同的字符集。

```sql
-- 查看完整的字符集
SHOW CHARACTER SET;

SHOW CHARACTER SET LIK 'utf8%';
+---------+---------------+--------------------+--------+
| Charset | Description   | Default collation  | Maxlen |
+---------+---------------+--------------------+--------+
| utf8    | UTF-8 Unicode | utf8_general_ci    |      3 |
| utf8mb4 | UTF-8 Unicode | utf8mb4_general_ci |      4 |
+---------+---------------+--------------------+--------+


-- 默认字符集
SHOW VARIABLES LIKE 'character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+


-- 默认校对
SHOW VARIABLES LIKE 'collation%';
+----------------------+-------------------+
| Variable_name        | Value             |
+----------------------+-------------------+
| collation_connection | utf8_general_ci   |
| collation_database   | latin1_swedish_ci |
| collation_server     | latin1_swedish_ci |
+----------------------+-------------------+
```

<br>

为表和列指定字符集:

```sql
-- 栗子
CREATE TABLE tableTest(
  c1 INT,
  c2 VARCHAR(10),
  C3 VARCHAR(20) CHARACTER SET latin1 COLLATE latin1_general_ci
) DEFAULT CHARACTER SET utf8mb4 COLLATE hebrew_general_ci;
```




<br/>
<br/>
<br/>




## 安全管理

本章将介绍mysql的访问控制和用户管理。


<br/>


### 访问控制

尽量权限最小化。
除非必要，尽量不要使用`root`进行登录。不该在日常的MySQL操作中使用`root`用户。



<br/>
<br/>



### 管理用户

注意理解MySQL中用户和账户的概念，账户为用户加权限。

```sql
-- mysql.user表包含所用账户信息
SELECT user FROM mysql.user;
```


<br/>


#### 创建用户

```sql
-- 新建用户
CREATE USER user1 IDENTIFIED BY 'user1-passwd';

-- 重命名用户
RENAME USER user1 TO User1;

```


<br/>
<br/>


#### 删除用户

```sql
-- 删除用户账号及权限
DROP USER User1;
```



<br/>
<br/>



#### 设置权限

新创建的用户没有分配权限，它们能登录MySQL，但不能看到数据。

`GRANT`至少需要以下信息:

- 要授予的权限；
- 被授予访问权限的库或表；
- 用户名。

<br>

`GRANT`和`REVOKE`可在几个层次上控制访问权限:

- 整个Server，使用`GRANT ALL`和`REVOKE ALL`;
- 整个Database，使用`ON db.*`;
- 特定的表，使用`ON db.tableName`;
- 特定的列;
- 特定的存储过程。

<br>

详细权限:

| 权限 | 描述 |
| - | - |
| `ALL` | 除`GRANT OPTION`外的所有权限 |
| `ALTER` | 修改表 |
| `ALTER ROUNTINE` | 修改和删除存储过程 |
| `CREATE` | 创建表 |
| `CREATE ROUTINE` | 创建存储过程 |
| `CREATE TEMPORARY TABLE` | 创建临时表 |
| `CREATE USER` | 创建、删除、重名用户和解除所有权限 |
| `CREATE VIEW` | 创建视图 |
| `DELETE` | 删除 |
| `DROP` | 删除表 |
| `EXECUTE` | 使用`CALL`和存储过程 |
| `FILE` | 使用`SELECT INTO OUTFILE`和`LOAD DATA INFILE` |
| `GRANT OPTION` | 使用`GRANT`和`REVOKE` |
| `INDEX` | 创建和删除索引 |
| `INSERT` | 插入
| `LOCK TABLES` | 锁表 |
| `PROCESS` | 使用`SHOW FULL PROCESSLIST` |
| `RELOAD` | 使用`FLUSH` |
| `REPLICATION CLIENT` | 服务器位置的访问 |
| `REPLICATION SLAVE` | 由复制从属使用 |
| `SELECT` | 检索权限 |
| `SHOW DATABASES` | 查看数据库 |
| `SHOW VIEW` | 查看视图 |
| `SHUTDOWN` | 关闭MySQL |
| `SUPER` | 使用`CHANGE MASTER, KILL, LOGS, PURGE, MASTER, SET GLOBAL`的超级权限 |
| `UPDATE` | 更新 |
| `USAGE` | 无访问权限 |

<br>

```sql
-- 查看用户权限
SHOW GRANTS FOR User1;
+-----------------------------------+
| Grants for User1@%                |
+-----------------------------------+
| GRANT USAGE ON *.* TO 'User1'@'%' |
+-----------------------------------+


-- 添加权限
-- HELP GRANT;
-- 如果未指定主机，默认为%
GRANT SELECT ON testDB.* TO 'User1'@'localhost';
-- 多个权限
GRANT SELECT, INSERT ON testDB.user1T TO 'User1'@'localhost';


-- 解除权限
-- HELP REVOKE
REVOKE SELECT ON testDB.* FROM 'User1'@'localhost';
```


<br/>
<br/>


#### 修改密码

```sql
-- 修改指定用户密码
SET PASSWORD FOR User1 = Password('User-passwd');

-- 不指定用户名，则修改自己密码
SET PASSWORD = Password('My-passwd');
```




<br/>
<br/>
<br/>




## 数据库维护

本章介绍常见的数据库维护。


<br/>


### 备份数据

可能的解决方案:

- `mysqldump`实用程序来备份；
- 可使用MySQL的`BACKUP TABLE`或`SELECT INTO OUTFILE`转储所有数据到某个外部文件，此外部文件必须不存在。使用`RESTORE TABLE`来还原。

为保证所有数据都被写到磁盘，可能需要在备份前刷新(`FLUSH TABLES`)。



<br/>
<br/>



### 进行数据库维护

应该知道的一些语句:

- `ANALYZE TABLE`，用来检查表键是否正确；
- `CHECK TABLE`，用来针对许多问题对表进行检查:
- 如果MyISAM表访问产生不正确和不一致的结果，可能需要用`REPAIR TABLE`来修复相应的表。这条语句不应该经常使用；
- 如果bong一个表中删除大量数据，应该使用`OPTIMIZE TABLE`来回收所用的空间，从而优化表的性能。

```sql
ANALYZE TABLE mysql.user;
+------------+---------+----------+----------+
| Table      | Op      | Msg_type | Msg_text |
+------------+---------+----------+----------+
| mysql.user | analyze | status   | OK       |
+------------+---------+----------+----------+


CHECK TABLE mysql.user;
+------------+-------+----------+----------------------------------------------------------+
| Table      | Op    | Msg_type | Msg_text                                                 |
+------------+-------+----------+----------------------------------------------------------+
| mysql.user | check | warning  | 2 clients are using or haven't closed the table properly |
| mysql.user | check | status   | OK                                                       |
+------------+-------+----------+----------------------------------------------------------+
```


<br/>
<br/>



### 诊断启动问题

在排除系统启动问题时，首先应该用手动启动Server。使用以下几个重要的mysqld命令行选项:

- `--help`
- `--safe-mode`，装载减去某些最佳配置的Server；
- `--verbose`，显示全文本消息；
- `--version`，显示版本信息



<br/>
<br/>



### 查看日志

主要日志有:

- 错误日志，可用`--log-error`命令行选项更改；
- 查询日志，可用`--log`命令行选项更改；
- 二进制日志， 可用`--log-bin`命令行选项更改；
- 慢查询日志，可用`--log-slow-queries`命令行选项更改。

在使用日志时，可使用`FLUSH LOGS`语句来刷新和重新开始所有日志文件。




<br/>
<br/>
<br/>




## 改善性能

本章将学习与MySQL性能有关的某些要点。


<br/>


与性能相关的一些要点:

- 硬件；
- MySQL配置
  - `SHOW VARIABLES;`
  - `SHOW STATUS;`
- 查看所有MySQL活动进程, `SHOW PROCESSLIST;`
- 可终止某个特定的进程，`KILL $mysql-pid`;
- 使用联结、并、子查询等查询方式，找出最佳的查询方法；
- 使用`EXPLAIN`语句让MySQL解释他将如何执行一条`SELECT`语句；
- 一般来说，存储过程比一条条地执行MySQL语句快；
- 应该总使用正确的数据类型；
- 绝不要检索比需求还有多的数据；
- 有的操作支持一个可选的`DELAYED`关键字；
- 在导入数据时，应该关闭自动提交。你可能还想删除索引，然后在导入完成后再重建索引；
- 必须索引数据库表以改善数据检索的性能；
- 索引改善数据检索的性能，但损害数据插入、删除和更新的性能；
- `LIKE`很慢，一般来说，最好使用`FULLTEXT`而不是`LIKE`；
- 数据库是不断变化的实体。之前优化好了，之后可能也会面目全非了；
- 记得查看MySQL官方文档。





<br/>
<br/>
<br/>




## 数据类型

本章介绍MySQL中不同的数据类型。


<br/>


### 串

最常用的数据类型是串，有两种基本串类型:

- **定长串**：接受长度固定的字符串，其长度是在创建表时指定的。CHAR属于定长串类型；
- **变长串**：存储可变长度的文本。有些具有最大定长，有些则是完全变长的。不管哪种，只有指定的数据得到保存(额外的数据不保存)，TEXT数据变长串类型。

既然变长数据类型这样灵活，为什么还要使用定长数据类型？
因为**性能**。MySQL处理定长列远比处理变长列快的多。此外，MySQL不允许对变长列(或列的可变部分)进行索引。这会极大影响性能。

<br>

不管使用何种形式的串数据类型，请使用引号。


| 数据类型 | 描述 |
| - | - |
| `CHAR` | 1-255个字符的定长串。它的长度必须在创建时指定，否则MySQL假定为`CHAR(1)` |
| `ENUM` | 接受最多6K个串组成的一个预定义集合的某个串 |
| `LONGTEXT` | 与TEXT相同，但最大长度为4GB |
| `MEDIUMTEXT` | 与TEXT相同，但最大长度为16K |
| `SET` | 接受最多64个串组成的一个预定义集合的零个或多个串 |
| `TEXT` | 最大长度为64K的变长文本 |
| `TINYTEXT` | 与TEXT相同，但最大长度为255字节 |
| `VARCHAR` | 长度可变，最大不超过255字节。如果指定`VARCHAR(n)`，则可存储0-n个字符的变长串 |

<br>

你可能会认为电话号码和邮政编码等应该存储在数值字段中，但是，这样做确是不可取的。如果邮政编码为`01234`，则保存的数值为`1234`，实际上丢失了一位数字。
需遵守的基本规则是，如果是数值计算，则应该存储在数值类型中；如果作为字符串使用，则应该保存在串数据类型中。



<br/>
<br/>



### 数值数据类型

数据数据类型存储数值。
注意**有符号**和**无符号(UNSIGNED)**也会影响存储范围。


| 数据类型 | 描述 |
| - | - |
| `BIT` | 位字段，1-64位 |
| `BIGINT` | 整数值，64位。有符号和无符号范围不同 |
| `BOOLEAN` | 布尔值，0或1 |
| `DECIMAL` | 精度可变的浮点数 |
| `DOUBLE` | 双精度浮点数 |
| `FLOAT` | 单精度浮点数 |
| `INT(INTEGER)` | 整数值，32位。有符号和无符号范围不同 |
| `MEDIUMINT` | 整数值，24位。有符合和无符号范围不同 |
| `REAL` | 4字节的浮点值 |
| `SMALLINT` | 整数值，16位。有符号和无符号范围不同 |
| `TINYINT` | 整数值，8位。有符号和无符号范围不同 |

<br>

数值不使用引号。
MySQL中没有专门存储货币的数据类型，一般情况下使用`DECIMA(8, 2)`。



<br/>
<br/>



### 日期和时间类型

MySQL使用专门的数据类型来存存储日期和时间。

| 数据类型 | 描述 |
| - | - |
| `DATE` | 日期格式为`YYYY-MM-DD` |
| `DATETIME` | DATE和TIME的组合 |
| `TIMESTAMP` | 功能呢DATETIME相同，但范围较小 |
| `TIME` | 时间格式为`HH:MM:SS` |
| `YEAR` | 2位数字，表示70(1970)-69(2069)，不推荐 <br> 4位数字，表示1901-2155年，推荐 |



<br/>
<br/>



### 二进制数据类型

二进制数据类型可以存储任何数据，如图像、多媒体、字处理文档...

| 数据类型 | 描述 |
| - | - |
| `BLOB` | 最大长度位64KB |
| `MEDIUMBLOB` | 最大长度为16MB |
| `LONGBLOB` | 最大长度位4GB |
| `TINYBLOB` | 最大长度位255字节 |


























