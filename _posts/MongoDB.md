---
title: "MongoDB"
date: "2017-12-11 10:49:52"
tags: "MongoDB"
categories: "Database"

---

参考：
- [MongoDB官方文档](https://docs.mongodb.com/)
- [MongoDB中文文档](http://www.mongoing.com/docs)
- <https://zh.wikipedia.org/wiki/MongoDB>
- <http://www.ywnds.com/?p=5635>
- <https://www.centos.bz/2017/08/mongodb-secure-intro-user-auth/>
- <http://www.03sec.com/3176.shtml>
- <http://www.ywnds.com/?p=6502>
- <http://wiki.jikexueyuan.com/project/the-little-mongodb-book/>

环境：
- CentOS7_x64；
- MongoDB3.4；

<!--more-->



<br/>

---

<br/>


# NoSQL

NoSQL(Not Only SQL)是对不同于传统的**关系型数据库**的**数据库管理系统(DBMS)**的统称。
NoSQL不使用SQL作为查询语言，其数据结构可以不需要固定的表格模式，有横向可扩展性的特征。
NoSQL用于超大规模数据的存储，这些类型的数据存储不需要固定的模式，无序多余操作就可以横向扩展。


<br>

关系型数据库的典型实现主要被调整用于执行规模小而读写频繁，或大批量极少写访问的事务。
当代典型的关系型数据库在一些数据敏感的应用中表现了糟糕的性能。例如：

- 为巨量文档创建索引
- 高流量网站的网页服务
- 发送流媒体


<br>

NoSQL数据库分类：

类型 | 栗子 | 特点
- | - | -
文档存储 | MongoDB | 用类似json的格式存储，存储的内容是文档型的。这样就有机会对某些字段建立索引，实现关系数据库的某些功能
图形关系存储 | Neo4j | 图形关系的最佳存储
键-值(key-value)存储 | 最终一致性的键-值存储 <br> 架构性键-值存储 | xxx
主机式服务 | key-value硬盘存储 <br> key-value RAM存储 | MemcacheDB <br> Redis
多数据库 | OpenQM | xxx
时序型数据库 | Graphite | xxx
对象数据库 | ObjecStore | 通过类似面向对象语言的语法操作数据库，通过对象的方式存取数据
列存储 | HBase | 顾名思义，按列存储数据。方便存储结构化和半结构化数据，方便做数据压缩，针对某一列或某几列的查询有很大的IO优势。






<br/>
<br>


---

<br/>

# MongoDB简介

<br/>

![MongoDB](/images/MongoDB/MongoDB.jpeg)

MongoDB(<https://www.mongodb.com/>)，是一种文档导向的数据库管理系统，由C++撰写而成，以此来解决应用程序开发社区中的大量现实问题。它是一种NoSQL。
MongoDB支持的数据结构非常松散，是类似于json的bson格式，因此可以存储比较复杂的数据类型。
MongoDB是一个开源文档数据库，提供高性能，高可用性和自动扩展。

<br>

**预备知识：**
1. MongoDB中的`database`有和数据库一样的概念。一个MongoDB实例中，可以有零个或多个数据库，每个都作为一个高等容器，用于存储数据；
2. MongoDB数据库中有零个或多个`collections`(集合)。集合类似于传统意义上的`table`(表)；
3. MongoDB的集合是由零个或多个`documents`(文档)组成。文档类似于`row`(行)；
4. MongoDB的文档由零个或多个`fields`(字段)组成。字段类似于`columns`(列)；
5. MongoDB中`Indexes`(索引)扮演的角色与RDMS中一样；
6. MongoDB中的`Cursors`(游标)很重要，当你向MongoDB取数据的时候，它会给你返回一个结果集的指针而不是真正的数据，这个指针我们叫它游标。我们可以用游标做任何事情，比如计数或跨行之类。



<br>
## MongoDB特点

不如这样认为，MongoDB是关系型数据库的一个代替案。比如用`Lucene`作为关系型数据库的全文检索索引的加强，或者是`Redis`作为持久性`key-value`存储。

**无模式(Flexible Schema)**：
它不需要一个固定的模式，这使得他们比传统的数据库表要灵活更多。

<br>
**写操作(Writes)**：
MongoDB可以胜任的一个特殊角色是在日志领域。有两点使得MongoDB的写操作非常快：
1. 可以选择发送了写操作之后立刻返回，而无需等到操作完成；
2. 可以控制数据持久性的写行为。

<br/>
**高性能(High Performance)**：
MongoDB提供了高性能的数据持久性。尤其是：
- 对嵌入式数据模型的支持减少了数据库系统上的I/O活动；
- 索引支持更快的查询，并且可以包含来自嵌入式文档和数组的键。

<br>
**高可用(High Availability)**：
MongoDB的复制工具，称为副本集。提供：自动故障转移和数据冗余。

<br/>
**持久性(Durability)**：
在MongoDB中，日志(Journaling)是默认启动的，该功能允许快速恢复服务器，比如遭遇到了服务器奔溃或停电的问题。

<br>
**丰富的查询语言(Rich Query Language)**：
MongoDB支持丰富的查询语言来支持读写操作(CRUD)，数据聚合(Data Aggregation)，全文搜索(Text Search)。

<br/>
**水平可伸缩性(Horizontal Scalability)**：
MongoDB提供了横向可伸缩性。

<br>
**支持多个存储引擎(Support for Multiple Storage Engines)**：
在MongoDB3.2以后默认引擎为: WiredTiger Storage Engine，允许第三方为MongoDB开发存储引擎。



<br>
## database和collection

MongoDB stores BSON documents.

![collection](/images/MongoDB/collection.png)


<br>
### databases

In MongoDB,databases hold collections of documents.
如果一个数据库不存在，当你第一次存储数据时，MongoDB会自动创建数据库。这意味着可以切换到不存在的数据库。

默认情况下，集合不要求其文档具有相同的模式；文档不要求具有相同的字段集；字段的数据类型在集合的文档间可以有所不同。

```sh
#select a db
use <db>

#create a db
use newdb
db.newcoll.insert({name:'zhang'})
db.newcoll.insert({filed01:'filed01', filed02:'filed02', filed03:'filed03', filed04:'filed04'})
db.newcoll.insert({groups: ['A', 'B', 'C']})
db.newcoll.find().pretty()
```


<br/>
### collection

MongoDB stores documents in collections.
collection类似于关系型数据库中的table。

```sh
db.coll02.insert({x:1})
db.coll03.createIndex({y:1})
```


<br>
### 显式创建(explicit creation)

MongoDB提供了`db.createCollection()`方法来显式创建一个附带各种选项的集合。如设置document最大大小，文件验证规则等选项。
如果不需要指定这些选项，就不需要使用显式创建集合，而直接向集合中插入数据即可。
修改collection选项，使用`collMod`方法。



<br/>
### 视图(View)

视图的定义是公开的，视图的解释操作将包括定义视图的管道。因此，避免直接引用视图定义中的敏感字段和值。

**创建/删除视图：**

```sh
db.runCommand(
{
	crete: <view>,
	viewOn: <source>,
	pipeline: <pipeline>
})

db.createView(<view>, <source>, <pipeline>, <collation>)

db.collection.drop()
```

<br>
**视图行为：**

视图存在以下行为：
- 视图只读，视图上的写操作将会出错；
- 视图使用底层集合的索引；
- 如果视图的基础集合被分割，视图也被认为可分割；
- 不能重命名视图；
- 视图上的字符串使用视图的默认排序规则。



<br>
### 限制集

限制集是固定大小的集合支持基于文档插入顺序的高吞吐率的插入、检索、删除操作。
限制集工作在某种程度上类似于循环缓冲区：一旦一个文档填满分配给它的空间，它将通过在限制集中重写老文档来给新文档让出空间。

<br>
#### 行为

**插入顺序**
限制集合能够保留插入顺序。因此，查询并不需要索引来保证以插入顺序来返回文档。减少了索引的消耗，限制集可以支持更高的插入吞吐量。

**最旧文档的自动删除**
为了给新文档腾出空间，再不需要脚本或显示删除操作的前提下，限制集自动删除集合中最旧的文档。

例如replication set中的oplog.rs集合。考虑潜在用于集合封顶的用例：
- 存储高容量系统生成的日志信息。没有索引的情况下向限制集中插入文档的速度接近于直接在文件系统中写日志的速度；
- 在限制集中缓存少量的数据。


**_id索引**
限制集合有一个_id字段并且默认在_id字段上创建索引。


<br/>
#### 限制和建议

**更新**
更新限制集中的文档，创建一个索引保证这些更新操作不需要进行集合扫描。

**文档大小**
一个更新或替换操作改变了文档大小，操作将会失败。

**文档删除**
不能从一个限制集中删除文档！
为了从一个集合中删除所有文档，使用`drop()`方法来删除集合然后重新创建限制集。

**分片**
不能对限制集分片。

**查询效率**
用自然顺序监视限制集中大部分最近插入的文档。


<br>
#### 程序

**创建一个限制集**
必须使用`db.createCollection()`方法创建限制集。且必须指定**以字节为单位**的最大集合大小。MongoDB将会预先分配集合。
另外，可为限制集指定最大文档数据，用`max`字段。

大小参数是必须的。MongoDB会在达到最大限制前删除旧的文件。

```sh
use <db>

#限制集大小
db.createCollection("log", {capped: true, size: 1000000})

#限制集和文档大小
db.createCollection("log", {capped: true, size: 5242880, max: 5000})
```

**查询一个限制集**
如果没有对限制集指定排序，则MongoDB的结果顺序和插入顺序相同。

**检查一个集合是否是限制集**
`isCapped()`方法

```sh
db.collection.isCapped()
#db.coll01.isCapped()
#false
```

**将集合转换为限制集**
`convertToCapped()`方法

```sh
db.runCommand({"covertToCapped": "coll01", size: 1000000});
#db.coll01.isCapped()
#true
```

**在规定的时间周期之后将自动移除数据**
通过设置MongoDB的TTL时集合中的数据过期。
TTL collection与限制集不兼容。

**Tailable游标**
类似于Unix中的`taif -f`



<br>
<br>
## document

MongoDB存储数据记录为BSON文档。
BSON是JSON文档的二进制表示，因此它包含比JSON更多的数据类型。


<br>
### document structure

MongoDB字段由key-value对组成。
字段值可以是任一BSON数据类型，包括其他文档，数组，阵列。

```sh
{
	filed1: value1;
	filed2: value2;
    ...
    filedN: valueN
}


#data type
var mydoc ={
		_id: ObjectId("5099803df3f4948bd2f98391"),
		name: { first: "Alan", last: "Turing" },
		birth: new Date('Jun 23, 1912'),
		death: new Date('Jun 07, 1954'),
		contribs: [ "Turing machine", "Turing test", "Turingery" ],
		views : NumberLong(1250000)
}

_id是ObjectID；
name是嵌入式文档；
birth是日期类型；
contribs是字符串数组；
view是NumberLong类型。
```

**字段名(field name)**
字段名是字符串。
document对field name有以下限制:
- 字段名称`_id`保留用作主键(primary key)，它的值在collection中必须唯一，不可变。它的类型可以是数组外的任何类型；
- 字段名称不能以`$`字符开头；
- 字段名称不能包含`.`字符；
- 字段名称不能包含`null`字符。

BSON documents 可能有多个字段名称相同的字段。然而，大多数的MongoDB Interface，MongoDB结构（如hash表），并不支持重复字段名称。
如果需要操作具有多个相同名称字段的文档，请参考 mongo driver。

一些由内部MongoDB进程创建的documents可能会有重复的字段，但是没有MongoDB进程会向一个已经存在的user document中添加重复字段。


**字段值限制(field value limit)**
For `indexed collections`，indexed fields的值有一个最大索引值长度限制(maximum index key length)。


<br>
### 圆点表示法(dot notation)

MongoDB使用圆点表示法来访问数组中的元素，访问嵌套文档中的字段。


**数组(array)**
通过基于0的索引位置来指定或访问数组中的元素。

```sh
<array>.<index>

{
	contribs: [ 'Turing machine', 'Turing test', 'Turingery' ]
}

#contribs.0 == 'Turing machine'
#contribs.1 == 'Turing test'
#contribs.2 == 'Turingery'
```


**嵌套文档(embedded documents)**
通过圆点表示法来指定或访问嵌套文档中的字段。

```sh
<embedded document>.<field>

{
	name: { first: 'AAA', last: 'ZZZ'},
    contact: { phone: { type: 'cell', number: '1-22-333' }}
}

#name.first == 'AAA'
#contact.phone.number == '1-22-333'
```


<br>
### 文档限制(document limitation)

**文档大小限制(size limit)**
BSON document最大size为：16MB。

最大document size确保一个单一document不能使用过量的RAM，或是传输期间的过量带宽。
MongoDB提供了`GridFS API`，用来保存超过最大size的文档。


**文档字段序列(field order)**
MongoDB用write operation来作为document的序列，除了一下情况：
- `_id`字段总是document中的第一个field；
- 包含重命名的`update`操作，会导致document中的field重新排序。


**_id字段**
在MongoDB中，每个保存在collection中的document都要求一个唯一的`_id`，用以担任主键(primary key)。
如果向document中`insert`数据是忽略的`_id`字段，则MongoDB driver会为`_id字段`自动生成一个ObjectID。

```sh
#默认生成_id
db.coll01.insert({name: 'zhang', sex: 'man', hobby: 'woman'})
# "_id" : ObjectId("5a32166ebf2c986e8106f891")

#自定义_id
db.coll01.insert({_id:'ZhangCustomDefine', name:'zhang', sex: 'man', arr: [0, 1, 2, 3], emmdoc: {emm01:'Emm01', emm02: 'Emm02', emm03: 'Emmo3'}})
#"_id" : "ZhangCustomDefine"
```

`_id字段`有以下行为和约束：
- 默认情况下，MongoDB在collection创建document时，会创建一个唯一的`_id`作为索引；
- `_id字段`总是document中的第一个字段。如果server接受的document中`_id`不在第一个字段，那么Server会移动`_id`到第一个字段；
- `_id`字段的数据类型除了数组外的任意BSON 数据类型；
- 不要存储BSON正则表达式的类型在`_id`字段中。


`_id`字段值的常用选项：
- 使用ObjectId；
- 使用了自然唯一的标识符，节省了空间并避免了额外的索引；
- 生成一个自动递增的数字；
- 在应用程序代码中生成UUID；


<br>
### 文档结构的其他用途

**查询过滤文档(query filter)**
使用<key>:<value>表达式来指定条件。

```sh
{
	<field1>: <value1>
    <field2>: <value2>
    ...
}
```

**更新特定文档(update)**
使用`db.collection.update()`操作更新数据。




<br>
<br>
## BSON类型

BSON是一个用来存储document和MongoDB进行远程调用的二进制序列化格式。
BSON支持以下数据类型作为文档中的值。每个数据类型都有一个相应的数字和字符串别名，可与`$type`操作符一起使用，以便按照bson类型查询文档。

Type | Number | Alias
- | - |
double | 1 | "double"
字符串 | 2 | "string"
对象 | 3 | "object"
数组 | 4 | "array"
二进制数据 | 5 | "binData"
未定义 | 6 | "undefined"
ObjectId | 7 | "objectId"
Boolean | 8 | "bool"
日期 | 9 | "date"
空 | 10 | "null"
正则表达式 | 11 | "regex"
DBPointer | 12 | "dbPointer"
JavaScript | 13 | "javascript"
符号 | 14 | "symbol"
JavaScript(带范围) | 15 | "javascriptWithScope"
32位整数 | 16 | "int"
时间戳 | 17 | "timestamp"
64位整数 | 18 | "long"
Decimal128 | 19 | "decimal"
Min key | -1 | "minKey"
Max key | 127 | ---

如果你想要将BSON转换为JSON，参考Extended JSON。


<br>
### ObjectId

ObjcetIds are small, likely unique, fast to generate, and ordered.
ObjectIds由12个字节组成，其中前4个字节是反映ObjectId创建的时间戳(timestamp)。
- 一个4字节的值，代表从Unix纪元开始的秒数；
- 一个3字节的机器标识符；
- 日期对象排在时间戳对象之前；
- MongoDB在比较过程中，会把一些类型看成相等。

栗子：`{ "_id" : ObjectId("5a33354068b6c5e5fb6f213f"), "name" : "ZHANG" }`。

在mongo shell中，可以访问ObjectId的创建时间，使用`ObjectId.getTimestamp()`方法。
在`_id`字段中存储的ObjectId值的排序，大致相当于按其创建时间排序。
ObjectId的值顺序与生成时间之间并不严格。


<br>
### 字符串
BSON字符串都是UTF-8编码。一般来说，每种编程语言的驱动程序在序列化和反序列化BSON的时候，都会从语言的字符串形式转化为UTF-8。这就使得使用BSON字符串简单存储大多数国际字符变为可能。


<br>
### 时间戳

BSON有一个特殊的时间戳类型用于MongoDB内部使用，与普通的日期类型无关。而在应用开发中可使用BSON日期类型。
时间戳值是一个64位的值： 前32位是与Unix纪元相差的秒数，后32位是在某秒总操作的一个递增的序列数。

在MongoDB复制集中，`oplog`有一个ts字段。这个字段的值使用BSON时间戳表示了操作时间。

```sh
db.coll02.insert( { ts: new Timestamp() } )

db.coll02.find()
#{ "_id" : ObjectId("5a333e3f68b6c5e5fb6f2141"), "ts" : Timestamp(1513307711, 1) }
```


<br>
### 日期

BSON日期是一个64位整数，表示利当前Unix新纪元(1970.01.01)的毫秒数，可到未来的2.9亿年。
BSON日期类型是有符号的，负数表示1970年之前的时间。

```
var date1 = new Date()
var date2 = ISODate()

#date1
#date2
#ISODate("2017-12-15T03:28:08.227Z")
```



<br>
<br>
## MongoDB Extended JSON

JSON只能表示BSON类型的一个子集。为了保留类型信息，MongoDB对JSON格式添加了如下扩展性：

- Strict mode： Any JSON parser can parse these strict mode representations as key/value pairs;
- mongo shell mode： The MongoDB internal JSON parser and the mongo shell can parse this mode.

多种数据类型的表示取决于JSON解析的上下文！



<br/>
### 解析器(parser)和支持的格式(format)

**Input in Strict mode**
如下可在严格模式下被解析并识别类型信息。

- REST Interface;
- mongo import;
- --query;
- MongoDB Compass.


<br>
**Input in mongo shell mode**
如下可在严格模式下被解析并识别类型信息。

- REST Interface;
- mongo import;
- `--query`;
- MongoDB Compass


<br>
**Output in Strict mode**
`mongoexport`, REST, HTTP Interfaces.


<br>
**Output in mongo shell mode**
`bsondump`


<br>
### BSON数据类型和关联表示

**Binary**

Strict mode | mongo shell mode
- | -
{ "$binary": "<bindata>", "$type": "<t>" } | BinData ( <t>, <bindata> )

```
<bindata>是二进制base64表示；
<t>是由单字节的数据类型表示。
```


<br>
**Date**

Strict mode | mongo shell mode
- | -
{ "$date": "<date>" } | new Date ( <date> )

```
In Strict mode, <date>是 ISO-8601的日期格式的时区字段，类型如**YYYY-MM-DDTHH:mm:ss.mm<+/-offset>;
MongoDb JSON解析器目前暂不支持载入ISO-8601日期类型。
```


<br>
**Timestamp**

Strict mode | mongo shell mode
- | -
{ "$timestamp" ; { "t": <t>, "i" } } | Timestamp( <t>, <i> )

```
<t>是32位无符号整数的JSON表现形式；
<i>是增量的32位无符号整数。
```


<br>
**Regular Expression**

Strict mode | mongo shell mode
- | -
{ "$regex": <sRegex>, "$options": "<sOptions>" } | /<jRegex>/<jOptions>

```
<sRegex>是有效地JSON字符串；
<jRegex>是一个可能包含有效的JSON字符和未转义的双引号(")，但可能不包括未转义的斜杠(/)字符；
<sOptions>是一个正则表达式选项；
<jOptions>是一个只能包含字符"g", "i", "m", "s"的字符串。
```


<br>
**OID**

Strict mode | mongo shell mode
- | -
{ "$oid": "<id>" } | ObjectId( "<id>" )

`<id>是一个24字符的十六进制(hexadecimal)字符串`


<br>
**DB Reference**

strict mode | mongo shell mode
- | -
{ "$ref": "<name>", "$id": "<id>" } | DBRef("<name>", "<id>")


```
<name>是一个有效的JSON字符；
<id>是任一extended JSON type。
```


<br>
**Undefined Type**

strict mode | mongo shell mode
- | -
{  "$undefined": true } | undefined


<br>
**MinKey/MaxKey**

strict mode | mongo shell mode
- | -
{ "$minkey": 1 } | MinKey
{ "$maxkey": 1 } | MaxKey


<br>
**NumberLong**

strict mode | mongo shell mode
- | -
{ "$numberLong": "<number>" } | NumberLong( "<number>" )

```
Number是一个64位有符号整数。必须使用"，否则它将被解释为浮点数，从而导致损失精度；
db.json.insert{{ longquoted: NumberLong("12345678901234345") })
```






<br>
<br/>

---

# MongoDB安装

参考:
* <https://docs.mongodb.com/manual/administration/install-on-linux/>;
* <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/>;

MongoDB有社区版(Community)和企业版(Enterprise)。社区版免费，企业版在商业方面收费。

<br>
MongoDB在仓库中提供官方支持的包，包含以下软件包：

Package | Description
- | -
monogdb-org | 将自动安装下面四个组件包
mongodb-org-server | 包含`mongod`守护进程和相关配置和`init`脚本
mongodb-org-mongos | 包含`mongos`守护进程
mongodb-org-shell | 包含`mongo-shell`
mongodb-org-tools | 包含相关MongoDB工具，如`mongoimport`,`mongoexport`,`mongodump`,`mongorestore`...

`mongodb-org-server`包提供了一个`/etc/mongod.conf`配置文件来开始和初始化`mongod`。默认配置文件默认bind_ip为 `127.0.0.1`，当你有需要和副本集时请修改它。



<br>
## 自建mongodb.repo仓库安装

仓库地址：<https://repo.mongodb.org>

```sh
vim /etc/yum.repos.d/mongodb34.repo
#编辑仓库


[mongodb34]
name=MongoDB34 Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=0
enabled=1


#安装mongodb
yum install -y mongodb-org
```


<br/>
## 下载rpm包安装

```sh
cd /root/mongodb
wget https://repo.mongodb.org/yum/redhat/7/mongodb-org/3.4/x86_64/RPMS/mongodb-org-3.4.10-1.el7.x86_64.rpm
wget https://repo.mongodb.org/yum/redhat/7/mongodb-org/3.4/x86_64/RPMS/mongodb-xxx-3.4.10-1.el7.x86_64.rpm
#共五个包


yum ./mongo-org*
```


<br>
## 源码安装

```sh
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-3.6.0.tgz

tar -axvf mongodb-linux-x86_64-rhel70-3.6.0.tgz -C ./


#默认路径/usr/local
make && make install
```


<br>
## 开启mongodb

```sh
#默认启动方式
systemctl start mongod


#指定配置文件启动
#注意修改配置文件里面的某些路径和名称，不然会和默认配置文件冲突
mongod -f /etc/mongo_27018.conf
mongod -f /etc/mongo_27019.conf
``


<br>
## 卸载mongodb

```sh
systemctl stop mongod

yum remove $(rpm -qa | grep mongodb-org)

rm -rf /var/log/mongodb
rm -rf /var/dbpath/mongo
```


## mongodb异常关闭后

```sh
#首先查看日志文件
tail /var/log/mongodb/mongod.log

#删除
rm /var/run/mongodb/mongod.pid /var/db/mongodb/mongod.lock
```





<br>
<br/>

---

# MongoDB配置文件

MongoDB的配置文件格式使用了YAML格式。
[YAML维基百科](https://zh.wikipedia.org/wiki/YAML)，Yet Another Markup Language。强调以数据为中心，而不是标记语言为重点，用方向缩略语重命名。

<br>
默认配置文件`/etc/mongod.conf` 的几个大块：

```
systemLog:        #日志

storage:          #存储

processManagement:        #进程管理

net:        #网络

security:        #安全

operationProfiling:        #性能分析器

replication:        #主从复制

sharding:        #架构

setParameter:        #自定义变量

auditLog:        #检测日志

snmp:        #简单网络管理协议

```



<br>
## systemLog

日志相关参数：

```
systemLog:
  verbosity: <int>                #日志级别，默认0,1-5均会包含debug信息
  quiet: <boolean>                #安静，true时mongod将会减少日志的输出量
  traceAllExceptions: <boolean>        #打印异常详细信息
  syslogFacility:  <string>                #指定用于登录时信息到syslog Facility水平，前提是启用syslog
  path:  <string>          #日志路径，默认情况下，MongoDB将覆盖现有的日志文件
  logAppend: <boolean>        #mongod重启后，在现有日志后继续添加日志，否则备份当前日志，然后创建新日志
  logRotate: rename|reopen        #日志轮询，防止一个日志文件特别大。rename重命名日志文件，默认值；reopen使用Linuxrotate特性，关闭并重新打开日志文件，前提为logAppend: true
  destination: <string>        #日志输出目的地，可为file或syslog，若不指定，则会输出到 std out
  timeStampFormat: <string>        #指定日志格式的时间戳，有 ctime, Iso869-utc, iso8691-local
  component:            #为不同的组件指定各自的日志信息级别
      accessControl:
          verbosity: <int>
      command:
          verbosity: <int>
```



<br>
## storage

存储引擎相关参数:

```
storage:
  dbPath: <string>        #mongodb进程存储数据目录，此配置进队此mongod进程有效，你使用配置文件开启的mongod就可以指定额外的数据目录
  indexBuildRetry:  <boolean>        #当构件索引时mongod意外关闭，那么在此启动是否重建索引，默认true
  repairPath: <string>        #在repair期间使用此目录存储临时数据，repair结束后此目录下数据将被删除
  journal:
      enabled: <boolean>        #journal日志持久存储，journal日志用来数据恢复，通常用于故障恢复，建议开启
      commitIntervalMs: <num>        #mongod日志刷新值，范围1-500毫秒，默认100，不建议修改
  directoryPerDB:  <boolean>        #是否将不同的数据存储在不同的目录中，dbPath子目录
  syncPeriodSecs:  <int>        #fsync操作将数据flush到磁盘的时间间隔，默认为60秒，不建议修改
  engine:  <string>        #存储引擎

  mmapv1:    #mmapv1存储引擎，3.2前默认
      preallocDataFiles:  <boolean>
      nsSize: <int>
      quota:
          enforced: <boolean>
          maxFilesPerDB: <int>
      smallFiles: <boolean>
      journal:
          debugFlags: <int>
          commitIntervalMs: <num>
  wiredTiger:    #WiredTiger存储引擎，3.2后默认
      engineConfig:
          cacheSizeGB: <number>    #最大缓存大小
          journalCompressor: <string>    #日志压缩算法，可选值有 none，snappy(默认)，zlib
          directoryForIndexes: <boolean>    #是否将索引和collections数据分别存储在dbPath单独的目录中
      collectionConfig:
          blockCompressor: <string>    #collection数据压缩算法，可选none, snappy，zlib
      indexConfig:
          prefixCompression: <boolean>    #是否对索引数据使用前缀压缩。对那些经过排序的值存储有很大帮助，可有效减少索引数据的内存使用量。
  inMemory:    #inMemory内存存储引擎，bate版
      engineConfig:
          inMemorySizeGB: <number>
```


<br>
## processManagement

进程相关参数:

```
processManagement:
  fork: <boolean>        #是否以fork模式运行mongod进程，默认情况下，mongod不作为守护进程运行
  pidFilePath: <string>        #将mongod进程ID写入指定文件，如未指定，将不会创建PID文件
```


<br>
## net

网络相关参数:

```
net:
  prot: <int>    #监听端口，默认27017
  bindIp: <string>    #绑定IP，如果此值是“0.0.0.0”则绑定所有接口
  maxIncomingConnections: <int>    #mongod进程允许的最大连接数，如果此值超过系统配置的连接数阈值，将不会生效(ulimit)
  wireObjectCheck: <boolean>    #当客户端写入数据时，检查数据的有效性（BSON）。如果数据格式不良，update,insert等操作将会被拒绝
  ipv6: <boolean>    #是否支持多实例之间使用ipv6
  unixDomainSocker:    #适用于Unix系统
      enabled: <boolean>
      pathPrefix: <string>
      filePermissions: <int>
  http:    #
      enabled: <boolean>
      JSONEnabled: <boolean>
      RESTInterfaceEnabled: <boolean>
  ssl:
      sslOnNormalPorts: <boolean>
      mode: <string>
      PEMKeyFile: <string>
      PEMKeyPassword: <string>
      clusterFile: <string>
      clusterPassword: <string>
      CAFile: <string>
      CRLFile: <string>
      allowConnectionsWithoutCertificates: <boolean>
      allowInvalidCertificates: <boolean>
      allowInvalidHostnames: <boolean>
      disabledProtocols: <string>
      FIPSMode: <boolean>
  compression:
      compressors: <string>
```


<br>
## security

安全相关参数:

```
security:
  authorization: enabled    #MondoDB认证功能
  keyFile: /path/mongo.key    #MongoDB副本集节点身份验证密钥文件
  clusterAuthMode: <string>    #集群members间的认证模式
  transitionToAuth: <boolean>
   javascriptEnabled:  <boolean>    #是否允许执行JavaScript脚本
   redactClientLogData: <boolean>
   sasl:
      hostName: <string>
      serviceName: <string>
      saslauthdSocketPath: <string>
   enableEncryption: <boolean>
   encryptionCipherMode: <string>
   encryptionKeyFile: <string>
   kmip:
      keyIdentifier: <string>
      rotateMasterKey: <boolean>
      serverName: <string>
      port: <string>
      clientCertificateFile: <string>
      clientCertificatePassword: <string>
      serverCAFile: <string>
   ldap:
      servers: <string>
      bind:
         method: <string>
         saslMechanism: <string>
         queryUser: <string>
         queryPassword: <string>
         useOSDefaults: <boolean>
      transportSecurity: <string>
      timeoutMS: <int>
      userToDNMapping: <string>
      authz:
         queryTemplate: <string>
```


<br>
## operationProfiling

慢查询相关参数：

```
operationProfiling:
  slowOpThresholdMs: <int>    #数据库profiler判定一个操作是“慢查询”的时间阈值，单位毫秒。mongod会把慢查询记录到日志中，默认100ms
  mode: <string>    #数据库profiler级别，操作的性能信息将会被写入日志文件中，可选值“off”--关闭profiling，“slowOp”--只包包含慢操作，“all”--记录所有操作
  #数据库profiling会影响性能，建议只在性能调试阶段开启
```


<br>
## replication

副本集：

```
replication:
  oplogSizeMB: <int>    #replication操作日志的最大尺寸，如果太小，secondary将不能通过oplog来同步数据，只能全量同步
  replSetName: <string>    #副本集名称，副本集中所有的mongod实例都必须有相同的名字，Sharding分布式下，不同的sharding应该使用不同的repSetName
  secondaryIndexPrefetch: <string>    #副本集中的secondary，从oplog中应用变更操作之前，将会先把索引加载到内存
  enalbeMajorityReadConcern: <boolean>    #允许readConcern的级别为“majority”
```


<br>
## sharding
分片相关参数：

```
sharding:
  clusterRole: <string>    #在sharding集群中，此mongod实例可选的角色。configsvr,默认监听27019端口 和 shardsvr,默认监听27018端口
  archiveMovedChunks: <boolean>    #当chunks因为“负载均衡”而迁移到其他节点时，mongod是否将这些chunks归档，并保存在dbPath/movechunk目录下，mongod不会删除moveChunk下的文件
```


<br>
## setParameter

自定义变量：

```
setParameter:
  <parameter1>: <value1>
  <parameter2>: <value2>
  enableLocalhostAuthBypass: false    #栗子
```


<br>
## auditLog

审计相关参数：

```
auditLog:
  destination: <string>    #指定审计记录的输出方式，有syslog, console, file
  format: <string>    #输出格式，有JSON 和 BSON
  path: <string>    #如果审计时间输入为文件，那么就需要指定文件完整路径及文件名
  filter: <string>    #过滤器，可限制审计系统记录的操作类型，该选项需要一个表单的查询文档的字符串表示形式
```






<br>
<br/>

---

# Mongo Shell

**mongo shell**是一个交互式的JavaScript结构的MongoDB。使用mongo shell来查询和更新数据以及执行管理操作。


## mongo shell基础知识


### 启动monso shell

启动mongo shell前确保MongoDB实例正在运行。

```sh
mongo [option] [db address] [.js]

#以默认配置启动
mongo

#以特定配置启动
mongo --port 27018

#连接远程mongo shell
mongo --host $host --port $port -u $user -p $passwd

mongo <db>
mongo <host>/<db>
mongo <hsot:port>/<db>
```


<br>
**.mongorc.js文件**
mongo shell开始运行时，mongo将在用户主目录下检查`.mongorc.js`的js文件。如果找到，mongo将在首次命令行之前解释执行.mongorc.js的内容。
如果你使用mongo shell执行一个js或表达式，无论是通过`mongo --eval`，或指定一个.js文件，mongo都将在js处理完成之后读取.mongorc.js文件。可使用 `--norc`选项禁止加载.mongorc.js。

```sh
ll /root/.mongorc.js
# -rw------- 1 root root 0 Dec 27  2016 /root/.mongorc.js
```


<br>
### 使用mongo shell

可能在启动mongo shell的时候会警告:
- WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'. We suggest setting it to 'never'.
- WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'. We suggest setting it to 'never'
- WARNING: Access control is not enabled for the database.

hugepage(大内存页面)，是Linux操作系统一种管理内存的方式。和通常方式相比，hugepage模式下内存分配管理会有所差异。
MongoDB显然不希望这个特定被启用。
新版MongoDB增加了安全性设计，推荐用户创建使用数据库时进行验证。所以我们需要创建用户认证。

关闭hugepage:

```sh
vim /etc/rc.d/rc.local

echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag

chmox a+x /etc/rc.d/rc.local
```

创建用户认证:

```sh
>use admin
>db.createUser(
{
	user: "zhang",
    pwd: "zhang",
    roles: [{ role: "root", db: "admin"}]
}
)

mongo -u zhang -p zhang --authenticationDatabase admin
#或
mongo
use admin
db.auth("zhang", "1314520")
```



```sh
mongo

#显示当前使用数据库
>db

#切换数据库
>use <database>

#查看所有数据库
>show dbs
```

<br>
你可以切换到一个并不存在的数据库。当你第一次向数据库存储数据，如创建一个集合，MongoDB将自动创建数据库。

```sh
use nodb

db.nocollestion.insert({x:1});
```

<br>
### 格式化打印结果

`db.collection.find()`方法返回一个`cursor`(游标)。如果返回的游标未使用var关键字指定变量，则游标将自动迭代最多20次，以打印出与查询匹配的前20个`documents`

```sh
#在操作中添加`.pretty()`，以格式化打印结果
#使用.pretty显示结果很舒服
db.collection.find().pretty()


print()	#无格式打印
printjson() #用JSON打印
```


<br>
### mongo shell中的多行操作

mongo shell中如果你以`( , { , [`开始，那么知道你输入了对应的`) , } , ]`才算结束命令。


<br/>
### Tab命令补全和键盘快捷键

mongo shell支持键盘快捷键，例如：

- 使用 `上/下箭头` 进行历史命令切换；
- 使用 `Tab键` 自动补全命令。


<br>
### mongo shell批量操作

```sh
mongo -u xxx -p xxx --authenticationDatabase=xxx << EOF
show dbs
use zhang
db.coll01.drop()
db.coll02.update( { _id: "xxx" }, { name: "zhang" })
EOF
```




<br>
### 退出mongo shell

```sh
quit()

exit

Ctrl+c
```




<br>
## 配置mongo shell

可在mongo shell中设置变量`prompt`的值来修改提示符内容。
prompt变量可以存储字符串以及JavaScript代码。

也可以在`.mongorc.js`文件中增加提示符的逻辑操作来设置每次启动mongo shell的提示符。


<br>
### 自定义提示符

**自定义提示符展示操作符：**

在mongo shell中定义一下变量。

```sh
cmdCount = 1;
prompt = function() {
			return (cmdCount++) + '> ';
}


#效果
1>
2>
...
```


<br>
**自定义提示符显示数据库和主机名：**

形式为：<database>@<hostname>$

```sh
host = db.serverStatus().host;
prompt = function() {
			return db+'@'+host+'$'
}


#效果
test@localhost$
```


<br>
**自定义提示符展示服务器启动时间和文档数：**

```sh
prompt = function() {
			return 'Uptime:' + db.serverStatus().uptime + 'Documents:' + db.stats().objects + '> ';
}


#效果
Uptime:1234 Documents:5 >
```

<br>
**注意：**
在mongo shell里面定义的`prompt`变量知识临时生效的，退出shell后便没有。
如果想要当前用户永久生效，可写入`~/.mongorc.js`文件。则此用户每次启动mongo shell前都会执行这个文件。

```sh
vim ~/.mongorc.js

host = db.serverStatus().host;
prompt = function() {
             return db+"@"+host+"> ";
         }
```




<br>
### 在mongo shell中使用外部编辑器

可在启动mongo shell之前设置`EDITOR`环境变量来在mongo shell中使用自己的编辑器。

```sh
export EDITOR=vim
mongo

#edit <variable>|<function>
function myfunc(){}

edit myfunc
#此时是edit使用vim编辑myfunc
function myfunc(){
	print("It was edited by vim!")
}

myfunc()

```



<br>
### 修改mongo shell批处理大小

`db.collection.find()`是一种JavaScript方法，返回一个`cursor`(游标)。如果返回的游标未使用var关键字指定变量，则游标将自动迭代最多20次，以打印出与查询匹配的前20个`documents`。
可以设置`DBQuery.shellBatchSize`属性来修改默认20篇文档。

```sh
DBQuery.shellBatchSize = 10;
```





<br>
<br>
## 获取mongo shell帮助

合理运用`Tab键`补全命令！

```sh
###命令行帮助
mongo --help


###mongo shell里查看帮助列表
help


###数据库帮助
#db.<method>
show dbs
db.help()


###集合帮助
#db.<collection>.<method>
show collections
db.collections.help()


###游标帮助
db.collection.find().help()


###封装对象帮助
help misc
```



<br>
<br/>
## 给mongo shell写脚本

可使用JavaScript为mongo shell编写脚本，用于处理MongoDB中的数据或执行管理操作。


<br>
### 打开新连接

在mongo shell或JavaScript文件中，可使用`Mongo()`构造函数来实例化数据库连接：

```sh
new Mongo()
new Mongo(<host>)
new Mongo(<host:port>)


#栗子
conn = new Mongo();
db = conn.getDB('mydb');	#将全局db变量设置为mydb

#连接
db = connect('localhost:27017/mydb');

#认证
db.auth(<user>, <passwd>)

db.auth({
	user: <user>,
    pwd: <passed>
})
```


<br>
### 交互式和脚本化mongo的区别

mongo shell中的帮助与JavaScript中帮助不一样！

mongo shell帮助 | JavaScript等量
- | -
show dbs | db.adminCommand('listDatabases')
use <db> | db = db.getSiblingDB('<db>')
show collections | db.getCollectionNames()
show users | db.getUsers()
show log <logname> | db.adminCommand({'getLog' : '<logname>'})



<br>
### 脚本

使用mongo shell来计算JavaScript的值。

<br>
**--eval <javascript>**
`mongo`执行 `--eval`后的js命令

```
mongo test --eval "printjson(db.getCollectionNames())"
```

<br>
**执行JavaScript文件**

```sh
mongo localhost:27017/test myjs.js

#在shell中执行.js
>load("myjs.js")
>loca("/root/mongo/myjs.js")
```



<br>
<br/>
## mongo shell中的数据类型

MongoDB BSON提供了除JSON之外的其它数据类型的支持。Driver提供了对这些数据类型在主机语言的本地化支持，mongo shell也提供了一些帮助类来支持这些数据类型在mongo javascript shell中的使用。


<br>
### 日期

mongo shell提供了多种方法返回日期:

- `Date()` 方法返回当前日期为一个字符串；
- `new Date()` 构造函数返回一个使用ISODate()包装返回的Date对象；
- `ISODate()` 构造函数返回一个使用ISODate()包装返回的Date对象。

<br>
返回一个日期为字符串：

```
var myDateString = Date();
#查看变量值
myDateString
```

<br>
验证类型：

```
typeof myDateString()
#string
```

<br>
返回Date：

```
var myDate = new Date();
myDate
#ISODate("2017-12-12T08:43:31.405Z")

#验证
myDate instanceof Date
```


<br>
### ObjectId

mongo shell对objectid数据类型提供objectId()包装类。

`new ObjectId`


<br/>
### NumberLong

mongo shell默认将所有数字处理为浮点值。

用`numberlong()`包装来处理64位整数。

```sh
NumberLong("2090845886852")
```


<br>
### NumberInt

用`NumberInt()`构造函数来显式指定32位整数。


<br>
### NumberDecimal

`mongo shell`默认将所有的数字处理为64位浮点的`double`值。`mongo shell`提供了`NumberDecimal()`构造函数限制指定128位基于十进制的浮点值，能够以精确的精度仿效十进制近似值。
这个功能在金融、税务以及科学计算等方面应用。

```sh
>NumberDecimal('1000.55')
#强烈建议加上引号，没加引号可能会存在精度丢失的情况
```


<br>
###　在mongo shell中检查类型

`instanceof`返回一个bool值来验证一个值是否为某些类型的实例。

```sh
mydoc._id instanceof ObjectId
#true
```

`typeof`返回一个字段的类型。

```sh
typeof mydoc._id
#object
```




<br>
<br>
## mongo shell快速参考

### mongo shell 历史命令

mongo shell历史命令保存在`~/.dbshell`文件中，`cat ~/.dbshell`。也可以使用**上/下键**切换历史命令。


<br>
### 命令行选项

option | description
- | -
`--help` | 显示命令行选项
`--nodb` | 启动mongo shell而不连接到数据库
`--shell` | 执行文件后运行mongo shell


<br/>
### mongo shell命令助手

help methods and commands | description
- | -
`help` | 显示帮助
`db.help` | 显示数据库方法的帮助
`db.collection.help()` | 显示集合方法的帮助
`show dbs` | 打印服务器上的所有数据库列表
`show databases` | 打印所有可获取的数据库列表
`use <db>` | 切换数据库
`show collections` | 打印当前数据库上的所有集合列表
`show users` | 打印当前数据库的用户列表
`show roles` | 打印当前数据库的所有角色(user-define and built-in)列表
`show profile` | 打印花费1ms或更多时间的五个最近的操作
`load()` | 在shell中执行一个JavaScript文件，建议使用绝对路径


<br/>
### mongo shell的基本JavaScript操作

mongo shell为数据库操作提供了一个JavaScript API。
db引用当的是前数据库的变量。

JavaScript db-operation | description
- | -
`db.auth()` | 在安全模式下认证用户
`coll = db.<collection>` | 将当前db中的特定collection设置为coll，可在此变量上执行操作，如`coll.find();`
`db.collection.find()` | 查找集合中的所有文档，并返回一个游标
`db.collection.insert()` | 插入一个新文档到集合中
`db.collection.update()` | 更新集合中一个存在的文档
`db.collection.save()` | 插入或更新 集合中的文档
`db.collection.remove()` | 从集合中删除文档
`db.collection.drop()` | 删除整个集合
`db.collection.createIndex()` | 在集合中创建索引
`db.getSiblingDB()` | 跨数据库查询


<br>
### 键盘快捷键

keysrtoke | function
- | -
Up/Down arrow | 前/后 历史命令
Left/Right arrow | 左右移动
Home/End | 行首/行尾
Tab | 自动补全
ctrl+c | 退出
ctrl+L | 清屏


<br>
### mongo shell查询方法

在mongo shell中，使用`find()`和`findOne()`方法执行读操作。

read-operations | description
- | -
`db.collection.find(<query>)` | 查找集合中与<query>匹配的文档，如果未指定<query>或为空，则读取操作会选择集合中的所有文档
`db.collection.find(<query>, <projection>)` | 查找与<query>匹配的文档，返回<projection>特定字段
`db.collection.find().sort(<sort order>)` | 返回<sort order>排序结果
`db.collection.find(<query>).sort(<sort order>)` | 返回匹配<query>和<sort order>排序结果
`db.collection.find(...).limit(<n>)` | 限制输出结果为<n>行
`db.collection.find().pretty().limit()` | 匹配，格式化，限制输出
`db.collection.find().limit().pretty()` | 同上
`db.collection.find(...).skip(<n>)` | 跳过前<n>行
`db.collection.count()` | 返回集合中文档总数
`db.collection.find().count()` | 返回匹配文档总数
`db.collection.findOne(<query>)` | 查找并返回单一的文档，null表示未找到


<br>
### 管理命令助手

js db-administrative-methods | description
- | -
`db.cloneDatabase(<host>)` | 从指定主机克隆当前数据库，noauth mode
`db.copyDatabase(<from>, <to>, <host>)` | copy <from>db to <to>db
`db.fromColl.renameCollection(<toColl>)` | rename collection
`db.repairDatabase()` | 修复当前db
`db.dropDatabases()` | 删除当前数据库


<br/>
### 打开附加连接

可以在mongo shell中创建一个新连接。

```js
>db = connect("<host>:<port>/<db>")
#db = connect("192.168.1.11/admin")

>conn = new Mongo()
>db = conn.getDB("dbname")
```







<br>
<br/>

---

# MongoDB CRUD操作

`CRUD`操作就是*创建(create)，读取(read)，更新(update)，删除(delete)*文档(document)!

<br>
**创建(create)操作**
创建或插入， 即是向 collection 添加新的 document。如果插入时集合不存在，插入操作会创建该集合。

```sh
db.collection.insert()
db.collection.insertOne()
db.collection.insertMany()
```


<br>
**读取(read)操作**
读操作，获取 collection 中的 document。

```sh
db.collection.find()
```


<br>
**更新(update)操作**
更新操作，修改 collection 中已经存在的 document。

```sh
db.collection.update()
db.collection.updateOne()
db.collection.updateMany()
db.collection.replaceOne()
```


<br>
**删除(delete)操作**
删除操作，是从一个 collection 中删除 document 的操作。

```sh
db.collection.remove()
db.collection.deleteOne()
db.collection.deleteMany()
```



<br/>
<br>
## 插入文档(Insert)


<br>
### 插入方法

MongoDB提供了如下插入方法向collection中插入document：

- `db.collection.insert()`, 向集合中插入一个或多个文档;
- `db.collection.insertOne()`, 向集合中插入一个文档;
- `db.collection.insertMany()`, 向集合中插入多个文档.


<br>

#### db.collection.insert()

`db.collection.insert()`,向collection中插入一个或多个document。
要想插入一个document，传递**一个文档**给该方法；要想插入多个documents，传递**文档数组**给该方法。

```sh
#插入一个文档
db.user.insert(
	{
    	_id: "ZhangTest",
    	name: "zhang",
        age: 2017,
        sex: "man"
    }
)


#插入多个文档
db.user.insert(
	[
    	{ name: "AAA", age: 20, status: "A" },
        { name: "BBB", age: 21, status: "B" },
        { name: "CCC", age: 22, status: "C" }
    ]
)
```


<br>
#### db.collection.insertOne()

`db.collection.insertOne()`,向collection中插入单个document。

```sh
db.user.insertOne(
	{
		name: "zhang",
        age: "2017",
        sex: "man",
        education: "bachelor"
	}
)

#此处并未自定义_id字段，因此它会自动添加_id字段
```


<br>

#### db.collection.insertMany()

`db.collection.insertMany()`,向collection插入多个documents。

```sh
db.user.insertMany(
	[
    	{ name: "AAA", age: "20", status: "A" },
        { name: "BBB", age: "21", status: "B" },
        { name: "CCC", age: "22", status: "C" }
    ]
)

#自动生成3个document的_id字段
```



<br>

### 插入操作的行为表现

**创建集合**
插入的时候如果collection不存在，那么插入操作会创建collection。

**_id字段**
在MongoDB中，存储于collection中的每一个document都需要一个唯一的`_id`字段作为`primary_key`。如果一个插入的document操作遗漏了`_id`字段，则`MongoDB driver`会自动生成一个`ObjectId`。

**原子性**
MongoDB中所有的写操作在单一文档层级上是原子的。





<br>
<br>

## 查询文档(Read)

MongoDB提供了`db.collection.find()`方法从collection中读取document。

```sh
db.collection.find( <query filter>, <projection> )

#<query filter>指明返回哪些document
#<projection>指明返回匹配document的那些filed
```


<br>

### 示例

```sh
db.user.insertMany(
  [
  	{
		_id: 1,
		name: "A",
		favorites: { artist: "Picasso", food: "pizza" },
        finished: [ 11, "AA" ],
        points: [ { points: 85, bonus: 30 }, { points: 85, bonus: 10 } ]
    },
    {
    	_id: 2,
        name: "B",
    	favorites: { artist: "Miro", food: "merigue" },
        finished: [ 22, "BB" ],
        points: [ { points: 85, bonus: 20 }, { points: 64, bonus: 12 } ]
    },
    {
    	_id: 3,
        name: "C",
        favorites: { artist: "Gaogeng", food: "cake" },
        finished: [ 33, "CC" ],
        points: [ { points: 67, bonus: 8 }, { points: 55, bonus: 21 } ]
    }
  ]
)
```


<br>

### 查询和规划操作符

**Comparison:**

```mongo
$eq
$gt
$gte
$lt
$ne
$in
$nin
```

**Logical：**

```mongo
$or
$and
$not
$nor
```

**Element:**

```mongo
$exists
$type
```


**Evaluation:**

```mongo
$mod
$regex
$text
$where
```


**Geospatial:**

```mongo
$geoWithin
$geoIntersects
$near
$nearSphere
```


**Array:**

```mongo
$all
$eleMatch
$size
```


**Bitwise:**

```mongo
$bitsAllSet
$bitsAnySet
$bitsAllClear
$bitsAnyClear
```


**Comments:**

```
$comment
```


**Projection Operators:**

```
$
$eleMatch
$meta
$slice
```



<br>

#### 选择collectino中所有document

一个空的`query filter`会选择集合汇总所有文档。

```mongo
db.users.find({})
db.user.find()
```

<br>

#### 指定查询过滤条件

**1. 指定等于条件**

```mongo
{ <field1>: <value1>, ...}

#栗子
db.user.find( { name: "C" } )
```


**2. 使用查询操作符指定条件**

```mongo
{ <field1>: { <operator1>: <value1> }, ... }

#栗子
db.user.find( { name: { $in: [ "A", "B" ] } } )
```


**3. 指定逻辑查询条件条件**
逻辑查询(AND, OR, NOT)。符合查询可以在集合文档的多个字段上指定条件。

```mongo
#AND
db.user.find( { name: "A", age: { $lt: 30} } )


#OR
db.user.find( {
	$or: [ { name: "A" }, { age: { $lt: 30 } } ]
} )


#AND和OR
db.user.find( {
	name: "A",
    $or: [ {age: { $lt: 30 } }, { type: 1 } ]
} )
```


<br>

#### 嵌入式文档的查询

当字段中包含嵌入文档时，查询可以指定嵌入文档中的精确匹配或使用`圆点(.)表示法`对嵌入文档的单个字段指定匹配。

```mongo
#精确匹配
db.user.find({
	favorites: { artist: "Picasso", food: "pizza" }
})


#圆点.表示法，记得加引号
db.user.find( { "favorites.artist": "Picasso" } )
```


<br>

#### 数组上的查询

当字段包含数组，可查询精确的匹配数组或数组中特定的值。如果数组包含嵌入文档，可使用`圆点表示法`查询内嵌文档中特定的字段。

```mongo
#精确匹配
db.user.find({ finished: [ 11, "AA" ] })


#匹配一个数组元素，会显示整个文档
db.user.find({ finished: "BB" })


#匹配数组中指定元素，会返回整个文档
db.user.find({ "finished.1": "CC" })


#指定数组中的多个查询条件
db.user.find({ finished: { $elemMatch: {$gte: 11, $lt: 33} } })

db.user.find({ finished: { $gt: 11, $lt: 33 } })


#嵌入文档数组
db.user.find({ 'points.points': {$lte: 80 } })
db.user.find({ "points.0.points": {$lte: 80} })


#元素组合满足查询条件
db.user.find({
	"points.points": {$lte: 80}, "points.bouns": 20
})

```



<br/>

### 返回查询的映射字段

默认地，MongoDB中的查询返回匹配文档中的所有字段。为了限制MongoDB发送给应用的数据量，我们可以在查询操作中包括一个`projection`文档。


<br/>

#### 映射文档

映射文档限制了返回所有匹配文档的字段。映射文档可以致命包括哪些字段或排除哪些字段。
这个就很不错了，可以过滤掉我们不需要的信息。

```mongo
db.users.find( {name: "AAA"} ,{_id: 0, name: 1, age: ture} )
db.user.find( { name: "BBB"}, {_id: false} )

1或true，表示在返回的文档中包含字段；
0或false，排除该字段；
```





<br>
<br/>

## 更新文档(Update)

**更新方法：**

- `db.collection.updateOne()`, 更新一个文档
- `db.collection.updateMany()`, 更新多个文档
- `db.replaceOne()`, 替换一个文档
- `db.collection.update()`, 更新或替换一个文档


<br/>

### 更新的行为表现

- 原子性：
MongoDB中所有的写操作在单一文档层级上是原子的。

- _id字段：
不能更新`_id`字段的值，也不能用不同`_id`字段值的替换文档来替换已存在的文档。

- 文档大小：
当执行更新操作增加的文档大小超过了为该文档分配的空间时，更新操作会在磁盘上重定位该文档。

- 字段顺序：
MongoDB按照文档写入的顺序整理文档字段。但`_id`字段始终是文档中第一个字段；`renaming`操作可能会导致文档中的字段重新排序。


<br/>

### Update Operator

**Fields**

name | description
- | -
$currentDate | 将字段值设置为当前日期(date or timestamp)
$inc | 按指定的数字递增字段的值
$min | 指定的值小于字段的值时才更新
$max | 指定的值大于字段的值时才更新
$mul | 将字段的值乘以指定的数字
$rename | 重命名一个字段
$set | 设置文档中字段的值
$setOnInsert | 如果更新导致文档插入，则设置字段的值
$unset | 从文档中删除指定的字段，

<br/>

**Array**

name | description
- | -
$ | 用作更新与查询条件匹配的第一个元素的占位符
$[] | 用作更新与查询条件匹配的文档的数组的所有元素的占位符
$[<identifier>] | xxx
$addToSet | 在集合中不存在元素时添加元素到数组
$pop | 移除数组中的第一项或最后一项
$pull | 删除所有匹配指定查询的数组元素
$push | 向数组中添加项
$pullAll | 从数组中删除所有匹配的值

<br/>

**Modifiers**

name | description
- | -
$each | 修饰$push and $addToSet， 向数组中添加多个项
$position | 修饰$push，在数组中指定位置添加元素
$slice | 修饰$push，限制更新数组的大小
$sort | 修饰$push，重新排列存储在数组中的文档

<br>

**BitWise**

```
$bit	执行按位AND,OR,XOR更新
```



<br>

### 更新文档字段中指定字段

为了修改文档中的字段，MongoDB提供了`update operators`，如用来修改值的`$set`。


```mongo
{
	<update operator>: { <field>: <value>, ...}
}


#更改指定字段的值
db.user.update(
	{ _id: 1 },
    { $set: {name: "SET"} }
)

#删除指定字段，文档中其他字段还在
db.user.update(
	{ _id: 1 },
    { $unset: {name: "SET"} }
)


#
db.user.updateMany(
	{ _id: 2},
    { $set: {name: "AAA", age: 222} }
)
```


<br>

### 文档替换(Replace)

当替换文档时，替换的文档必须仅仅有 `<field>: <value>`组成。
替换文档可以有不同于源文档的字段，但`_id`字段是不变的。

**建议使用`_id`作为过滤条件，因为它是唯一的。

```mongo
db.collection.replaceOne()


db.user.replaceOne(
	{ name: "AAA" },
    { name: "A", age: 2, sex: "man", favorites: { artist: "Dali", food: "banana" } }
)

db.user.update(
	{ _id: 1},
    { name: "A", age: 2, sex: "man", favorites: { artist: "Dali", food: "banana" } }
)
```




<br>
<br/>

## 删除文档(Delete)

**方法：**

- `db.collection.remove()`, 删除一个文档，或所有满足匹配的文档;
- `db.collection.deleteOne()`, 删除匹配最多条件的单个文档，即使可能有多个文档可能与指定过滤条件匹配;
- `db.collection.deleteMany()`, 删除所有匹配指定过滤条件的文档。


<br>

### 删除的行为表现

- Indexes
删除操作不会删除索引，即使从集合中删除了所有的文档。

- 原子性
MongoDB中所有的写操作在单一文档层级上是原子的。


<br>

### 删除

```mongo
#删除所有文档
db.collectin.deleteMany({})
db.collection.remove({})


#删除所有满足条件的文档
db.user.remove( { name: "A" } )
db.user.deleteMany( { name: "A: } )


#仅删除一个满足条件最多的文档
db.user.deleteOne( { name: "A" } )

db.users.remove( { name: "A"}, 1)
```



<br>

----

<br/>

# 聚合(Agrregation)

聚合操作处理数据记录并返回计算的结果。聚合操作将多个文档中的值(value)分组，并对分组的数据进行各类操作以返回单个结果。

MongoDB提供了三种方式进行聚合：

- aggregation pipeline(聚合管道);
- map-reduce function(映射化简);
- single aggregation methods(聚合指南)

<br/>

**Aggregation Pipeline(聚合管道)**

MongoDB的聚合框架(aggregation framework)是仿照数据处理管道的概念(concept)。Document输入多级管道，它将Document转换为聚合结果。

最基本的pipeline stage提供了：类似查询(query)操作的过滤器(filter)和类似修改(modify)输出文档格式的文档转换。

其他pipeline operation提供了按特定字段对文档进行分组和排序的工具，以及聚合数组内容(包括文档数组)的字段或工具。此外，pipeline stage可以使用运算符(operators)来处理任务。(如计算平均值和连接等...)

pipeline通过在MongoDB中使用本地操作，从而提供了高效的数据聚合。所以也是MongoDB中数据聚合的首选方法。

aggregation pipeline能够在一个共享的集合上操作。

aggregation pipeline可以使用索引来提高某些阶段的性能(performance)。另外，管道聚合还有一个内部优化阶段(optimization phase)。

![aggregation-pipeline](/images/MongoDB/aggregation-pipeline.png)

<br/>

**Map-Reduce(映射化简)**

一般来说，map-reduce操作有两个阶段：

- map stage: 处理每个文档并未每个输入文档发出一个或多个对象(object)；
- reduce stage: 结合映射操作的输出。

可选地，map-reduce有一个对结果做最后修改的最后阶段。与aggregation-operation类似，map-reduce可以指定查询条件来选择一个输入文档，以及对结果进行排序和限制。

map-reduce使用自定义的JavaScript函数执行映射和化简操作，以及可选的最终操作。与聚合管线相比，自定义的JavaScript提供了很大的灵活性。一般来说，map-reduce比aggregation pipeline效率更低，更复杂。

map-reduce能够在一个共享的集合上操作，同样也可以输出到共享集合。

![map-reduce](/images/MongoDB/map-reduce.png)

<br/>

**Single Purpose Aggregation Operations(聚合指南)**

MongoDB同样提供了`db.collection.count()`和`db.collection.distinct()`。

所有这些操作都从单个集合中聚合文档，虽然这些操作提供了对常见聚合过程的简单访问，但它们缺少aggregation pipeline和map-reduce的灵活性和功能。

![distinct](/images/MongoDB/distinct.png)



<br>
<br>

## Aggregation Pipeline(聚合管道)

MongoDB的聚合框架是仿照数据处理管道的概念。文档输入多级管道，它将文档转换为聚合结果。

当map-reduce的复杂性可能是没有保证的，aggregation pipeline为map-reduce提供了一个可选也可能是聚合任务的首选解决方案。
aggregation pipeline对key value和result size有一些限制。




<br/>
<br/>

## 映射化简



<br/>
<br>

## 聚合指南




<br>

---

<br>

# MongoDB文本索引



MongoDB支持在字符串内容上执行文本检索(text search)的查询操作。视图不支持文本检索。
为了执行文本检索，MongoDB使用`text index`和`$text`操作符。text索引可以包括任何值为字符串或字符串元素数组的字段。

栗子：

```sh
db.sample.insert(
	[
		{ _id: 1, name: "A", description: "AAA" },
        { _id: 2, name: "B", description: "BBB" },
        { _id: 3, name: "C", description: "CCC" }
	]
)
```

为了执行文本检索查询，你必须在集合有一个text索引，一个集合只能有**一个**文本检索索引，但是这个索引可以覆盖多个字段。

启动在`name`和`description`字段上的文本检索：

```sh
db.sample.createIndex(
	{ name: "text", description: "text" }
)
```

使用`$text`查询操作符在一个有`text index`的集合上执行文本检索

```
db.sample.find({
	$text: { $search: "A B" }
})


#精确检索
db.sample.find({
	$text: { $search: "A \"B\"" }
})


#词语排除
db.sample.find({
	$text: { $search: "A B -AAA" }
})
```

MongoDB默认返回没排序的结果。然而文本检索将会对每个文档计算一个相关性分数，表明该文档与查询的匹配程度。
为了使用相关性分数进行排序，你必须使用 `$meta textScore`字段进行映射然后基于该字段进行排序。

```
db.sample.find(
	{ $text: { $search: "A AAA B" } },
    { score: { $meta: "textScore" } }
).sort( { score: { $meta: "textScore" } })
```

文本检索可以在聚合管道中使用。



<br/>
<br/>

## 文本索引





<br/>
<br>

## 文本检索操作符




<br/>
<br>

## 在管道聚合中使用文本索引




<br>
<br/>

## 使用基本技术Rosette语义平台的文本索引




<br>
<br/>

## 文本检索语言




<br>

---

<br/>

# MongoDB数据模型



MongoDB的数据具有灵活的模式，集合本身没有对文档结构的规则性校验。


<br>

## 数据模型设计介绍

关系型数据库要求你再插入数据之前必须先定义好一个表的模式结构，而MongoDB的集合并不限制文档结构。
这种灵活性让对象和数据库文档之间的映射变得很容易。即使数据记录之间有很大的变化，每个文档也可以很好的映射到各条不同的记录。
当然，在实际使用中，同一个集合中的文档往往都有一个比较类似的结构。

数据模型设计中最具挑战性的是在应用程序需求，数据库引擎性能要求和数据读写模式之间的权衡考量。


<br/>

### 文档结构

**引用(reference)**
引用方式通过**存储链接**或**引用信息**来实现两个不同文档之间的关联。
应用程序可以通过解析这些数据库引用来访问相关数据。简单来讲，这就是规范化的数据模型。

![规范化数据模型](/images/MongoDB/data-model-normalized.png)


**内嵌(embedded data)**
内嵌方式指把相关联的数据保存在同一个文档之内。
MongoDB的文档结构允许一个字段或一个数组内的值为一个嵌套的文档。这种**冗余**的数据模型可以让应用程序在一个数据库内完成对相关数据的读取或修改。

![内嵌式数据模型](/images/MongoDB/data-model-denormalized.png)


<br/>

### 写操作的原子性

在MongoDB中，写操作在文档级别是原子的(atomic)，没有一个单独的写操作可以原子地影响多个文档或多个集合。但，对原子性写操作利好的内嵌数据模型会限制应用程序对数据的使用场景。

- 嵌入(embdded)数据的非规格化(denormalized)数据模型将单个文档所表示的实体(entity)的所有相关数据组合在一起。这有利于原子写操作，因为单个写操作可以插入或更新实体的数据；
- 规格化(normalizing)数据通过多个集合拆分数据，并需要多个不是原子集合的写操作。


<br/>

### 文档的增长

如果文档的大小超出分配给文档的原空间大小，那么MongoDB就需要把文档从磁盘上的现有位置移动到一个新的位置以存放更多的数据。这种数据增长的情况也会影响到是否要使用规范化或非规范化。



<br/>

### 数据的使用和性能

设计文档模型时，一定要考虑应用程序会如何使用你的数据。

例如：

- 假如应用程序通常只会使用最近插入的文档，那么可以考虑使用限制集；
- 假如应用会做大量的读操作，那么可以加多一些索引的方法来提升常见查询的性能。



<br>
<br/>

## 文档验证

MongoDB提供了在更新和插入期间验证(validate)文档的功能(capability)。验证规则是在每个集合中指定使用验证符(validator)选项，利用一个文档指定验证堆栈或表达式。

- 通过`collMod`命令附带验证符选项向一个已经存在的集合添加文档验证；
- 利用`db.createCollection()`命令附带验证符选项来创建文档验证规则。

```sh
db.createCollection( "contacts",
   { validator: { $or:
      [
         { phone: { $type: "string" } },
         { email: { $regex: /@mongodb\.com$/ } },
         { status: { $in: [ "Unknown", "Incomplete" ] } }
      ]
   }
} )
```

MongoDb同样提供了`validationLevel`选项，它确定了MongoDb在更新期间如何将验证规则应用到已有文档，以及验证操作选项。它确定MongoDB是否错误并**拒绝**违反验证规则的文档，或者**警告**日志中的违规，但允许无效的文档。


<br/>

### 行为

验证发生在更新和插入期间。当向一个文档添加验证，在修改之前，现有文档不会进行验证检查。

<br/>

**现有文档**

可使用`validationLevel`选项来控制MongoDB怎样处理现有文档。

默认情况下，`MongoDB`是严格的，并且将验证规则应用于所有插入和更新操作。

```sh
#moderate level
#在中等级别下，对不符合验证标准的现有文档更新将不会检查有效性

db.runCommand({
	collMod: "contacts",
    validator: { $or: [ { phone: { $exists: true } }, { email: { $exists: true}} ] },
    validationLevel: "moderate"
})
```

设置`validationLevel`为`off`以禁用验证功能。

<br/>

**接受或拒绝无效文档**

`validationAction`选项决定了MongoDB如何处理违反(violate)验证规则的文档。

默认情况下，`validationAction`是错误的，并且拒绝任何违反验证条件的插入和更新操作。

```sh
#当validationAction为warn时，MongoDB记录所有违反行为，但允许插入或更新操作。

db.createCollection( "contacts",
   {
      validator: { $or:
         [
            { phone: { $type: "string" } },
            { email: { $regex: /@mongodb\.com$/ } },
            { status: { $in: [ "Unknown", "Incomplete" ] } }
         ]
      },
      validationAction: "warn"
   }
)


#如下违规操作将会报警，并由于是warn，所以写入成功
db.contacts.insert( { name: "Amanda", status: "Updated" } )
```

<br/>

**约束(restriction)**

无法在`admin`,`local`,`config`数据库的集合 和 `system.*`集合 里面指定验证符(validator)。

<br/>

**绕过文档验证**

通过`bypassDocumentValidation`选项来绕过文档验证。



<br>
<br/>

## 数据建模理论

### 数据模型设计

一个高效的数据模型能够很好的满足应用程序的需求。设计一个文档数据结构最关键的考量就是决定是使用**嵌套(embdded)**还是**引用(reference)**。

<br>

#### 内嵌式数据模型(非规范化)

在MongoDB里面，可以把相关的数据包括在一个单个的结构或者文档下面。这样的数据模型也叫作**非规范化**模式。

内嵌数据可以让应用程序把相关的数据保存在同一条数据记录里面，这样，应用程序就可以发送较少的请求给MongoDB来完成常用的查询和更新请求。

<br>

一般来说，下述情况建议使用内嵌数据模型：

- 数据对象之间有包含(contain)关系；
- 数据对象间有一对多的关系。

通常情况下，内嵌数据会对读操作有比较好的性能提高，可以使应用程序在一个单个操作就可以完成对数据的读取。同时，内嵌数据也对更新相关数据提供了一个原子性写操作。

<br/>

#### 规范化数据模型

一般来说，下述情况可以使用规范化模型：

- 内嵌数据会导致很多数据的重复，并且读性能的优势又不足与盖过数据重复的弊端时；
- 需要表达比较复杂的多对多关系时；
- 大型多层次结构数据集。


<br>

### MongoDB特性和数据模型的关系

MongoDB的数据建模不仅仅取决于应用程序的数据需求，也要考虑MongoDB本身的一些特性。

<br/>

#### 文档增长性(increase)

如果更新操作导致文档大小增加，那么可能需要重新设计数据模型，在不同文档之间使用引用的方式而非内嵌、冗余的数据结构。
MongoDB会自动调整空白填充的大小以尽可能的减小文档迁移。你也可以使用一个**预分配策略**来防止文档的增长。

<br/>

#### 原子性(atomic)

在MongoDB中，所有在文档级别的操作都具有原子性。一个单个写操作最多只可以修改一个文档。即使是一个会改变同一个集合中多个文档的命令，在同一时间也只会操作一个文档。即便是涉及多个子文档的多个操作，只要是在同一文档之内，这些操作仍旧是有原子性的。

尽可能保证那些需要在一个原子操作内进行修改的字段定义在同一个文档里面。如果你的应用程序允许对两个数据的非原子性更新操作，那么可把这些数据定义在不同的文档内。

把相关数据定义到同一个文档里的内嵌方式有利于这种原子性操作。对于那些使用引用来关联相关数据的数据模型，应用程序必须再用额外的读和写操作去取回和修改相关的数据。

<br/>

#### 分片(sharding)

MongoDB使用分片来实现水平扩展。使用分片的集群可以支持海量的数据和高并发读写。使用分片技术把一个数据库内的某一个集合的数据进行分区，从而达到把数据分到多个mongod实例(或分片上)的目的。

MongoDB依据**分片键**分发数据和应用程序的事务请求。选择一个合适的分片键对性能有很大的影响，也会促进或阻碍MongoDB的定向分片查询和增强的写性能。所以在选择分片键的时候要仔细考量分片键所用的字段。

<br/>

#### 索引(index)

对常用操作可以使用索引来提高性能。对查询条件中常见的字段，以及需要排序的字段创建索引。
MongoDB会对`_id`自动创建唯一索引。

创建索引时，需要考虑索引的下述特征：

- 每个索引要求至少8KB的数据空间；
- 每增加一个索引，就会对写操作性能有一些影响。对于一个写多读少的集合，索引会变得很费时。因为每个插入必须要更新所有索引；
- 每个索引都会占一定的硬盘空间和内存(对于活跃的索引)。索引可能会用到很多这样的资源，因此对这些资源要进行管理和规划，特别是在计算热点数据大小的时候。

<br/>

#### 集合的数量

某些情况下，可能需要把相关的数据保存到多个集合里面。比如：

```
{ log: "dev", ts:..., info: ... }
{ log: "debug", ts:..., info: ... }
```

一般来说，很大的集合数量对性能没有什么影响，反而在某些场景下有不错的性能。使用不同的集合在高并发批处理场景下会有很好的帮助。

当使用有大量集合的数据模型时，请注意：

- 每个集合有几KB的额外开销；
- 每个索引(包含`_id`)，需要至少8KB的数据空间；
- 每个MongoDB的数据库有且仅有一个命名文件(namespace file)(<db>.ns)。这个命名文件保存了数据库的所有元数据，每个索引和集合在这个文件里都有一条记录；
- MongoDB的命名文件有大小的限制(默认16MB)。利用`db.system.namespaces.count()`查看。

<br/>

#### 包含大量小文档的集合

如果你有一个包含大量小文档的集合，则应该考虑为了性能而嵌入。如果你可以通过一些逻辑关系将这些小文档分组，并且你经常通过这个分组来检索文档，那么你应该考虑将小文档"卷起来"成为包含一系列嵌入式文档的大文档。

将这些小文档“卷起来”成为逻辑分组，意味着检索一组文档的查询设计顺序读取和较少的随机磁盘访问。此外，将文档“卷起”并将公共字段移动到较大的文档会使字段上的索引受益。公共字段的副本将会减少，并且相应索引中的关联键条目也会减少。

然而，如果你通常只需要检索分组中的一个文档的子集，那么“滚动”文档可能无法提供更好的性能。此外，如果晓得，独立的文档代表数据的自然模型，那你应该维护改模型。

<br/>

#### 小文档的存储优化(storage optimization)

每个MongoDB文档都包含一定的开销(overhead)，这些开销通常是无关紧要的。但如果文档只有几个字节，那就相当重要了。

考虑以下有关优化这些集合的存储利用率的建议：

- 显示地使用`_id`字段；
- 使用较短的字段名称；
- 嵌套文档。


<br>

#### 数据生命周期管理

数据模型决策应考虑数据生命周期管理。

集合的**TTL功能*在一段时间后标识文档到期。如果应用程序需要一些数据才能在数据库中持久化一段有限的时间，请考虑使用TTL特性。

此外，你的应用程序仅使用最近插入的文档，请考虑限制集。



<br>
<br>

## 数据模型例子与范式

### 文档关系建模

#### 一对一关系建模：内嵌文档模型

用内嵌文档方式实现一对一关系。

#### 一对多关系建模：内嵌文档模型

用内嵌文档方式实现一对多关系。

#### 一对多关系建模：文档引用模式

用文档引用实现一对多关系。


<br/>

### 树结构建模


#### 父文档引用

父文档引用模式用一个文档来表示树的一个节点。每一个文档除了存储节点的信息，同时也保存该节点父节点文档的id值。

![data-model-tree](/images/MongoDB/data-model-tree.png)

```sh
db.test.insert({ _id: "MongoDB", parent: "Databases" })
db.test.insert({ _id: "Databases", parent: "Programming" })
db.test.insert({ _id: "Programming", parent: "Books" })
db.test.insert({ _id: "Books", parent: null })


#查询父节点
db.test.findOne({ _id: "MongoDB" }).parent

#对parent字段创建索引，这样可以快速的按照父节点查找
db.test.createIndex({ parent: 1 })

#查询一个父节点的所有子节点
db.test.find({ parent: "Databases" })
```
<br/>

#### 子文档引用

子文档引用模式用一个文档来表示树的一个节点。每一个文档除了存储节点信息外，同时也用一个数组来保存该节点的所有子节点的id值。

![data-model-tree](/images/MongoDB/data-model-tree.png)

```sh
db.test.insert({ _id: "MongoDB", children: [] })
db.test.insert({ _id: "Databases", children: [ "MongoDB", "dbm" ]})
db.test.insert({ _id: "Programming", children: [ "Languages", "Databases" ]})
db.test.insert({ _id: "Books", children: [ "Programming" ]})


#查询子节点
db.test.findOne({ _id: "Databases"}).children

#对children字段创建索引，这样就可以快速按照子节点查找
db.test.createIndex({ children: 1 })

#查找一个子节点的父节点和同级节点
db.test.find({ children: "MongoDB" })
```

<br/>

#### 祖先数组(ancestors array)

祖先数组模式用一个文档来表示树的一个节点。每一个文档除了存储节点的信息，同时也存储了对父文档及祖先文档的id值。

![data-model-tree](/images/MongoDB/data-model-tree.png)

```sh
db.test.insert({ _id: "MongoDB", ancestors: [ "Books", "Programming", "Databases" ], parent: "Databases" })
db.test.insert({ _id: "Databases", ancestors: [ "Books", Programming" ], parent: [ "MongoDB", "dbm" ]})
db.test.insert({ _id: "Programming", ancestors: [ "Books" ], parent: "Books" })
db.test.insert({ _id: "Books", ancestors: [ ], parent: null })


#查询一个节点的祖先节点
db.test.findOne({ _id: "MongoDB" }).ancestors

#对ancestors创建索引
db.test.createIndex({ ancestors: 1 })

#利用ancestors字段来查找某个节点的所有子代节点
db.test.find({ ancetors: "Programmming" })
```

<br/>

#### 物化路径(materialized path)

物化路径模式将每个树节点存储在文档中。除了存储节点信息外，同时也存储了祖先文档或路径的id值。

![data-model-tree](/images/MongoDB/data-model-tree.png)

```sh
db.test.insert({ _id: "Books", path: null })
db.test.insert({ _id: "Programming", path: ",Books," })
db.test.insert({ _id: "Databases", path: ",Books,Programming," })
db.test.insert({ _id: "MongoDB", path: ",Books,Programming,Databases," })


#查询整个树的所有节点并按path排序
db.test.find().sort({ path: 1 })

#可以在path字段上使用re来查询
db.test.find({ path: /,Programming,/ })
db.test.find({ path: /^,Books,/ })

#在path字段上创建索引
db.test.createIndex({ path: 1 })
```

<br/>

#### 嵌套集合(nested set)

嵌套集合模式对整个树结构进行一次深度优先的遍历。遍历时候对每个节点的压栈和出栈作为两个不同的步骤记录下来。每一个节点就是一个文档，除了节点信息外，文档还保存父节点的id以及遍历的两个步骤编号。压栈是的步骤保存到**left字段**里，而出栈时的步骤编号则保存到**right字段**里。

![data-model-example-nested-set](/images/MongoDB/data-model-example-nested-set.png)

```sh
db.test.insert({ _id: "Books", parent: 0, left: 1, right: 12 })
db.test.insert({ _id: "Programming", parent: "Books", left: 2, right: 11 })
db.test.insert({ _id: "Databases", parent: "Programming", left: 5, right: 10 })
db.test.insert({ _id: "MongoDB", parent: "Databases", left: 6, right: 7 })


#查询摸个节点的子代节点
db.test.find({ left: { $gt: db.test.findOne({ _id: "Databases" }), right: { $lt: db.test.findOne({"_id: "Databases"}) } })
```


<br>

### 具体应用模型举例


#### 原子性事务建模

如何使用内嵌技术来保证同一文档内相关字段更新操作的原子性。

举例来说，假设你在设计一个图书馆的借书系统，你需要管理书的库存量以及出借记录。一本书的可借数量加上借出数量的和必须等于总的保有量，那么对这两个字段的更新必须是原子性的。

<br/>

#### 关键词搜索建模

描述了一种把关键词保存在数组里并使用多键索引来实现关键词搜索功能的方法。

为实现关键词搜索，在文档内增加一个数组字段并把每一个关键词加到数组里。然后你可以对该字段建一个 多键索引。这样你就可以对数组里面的关键词进行查询了。

<br/>

#### 货币数据建模

处理货币数据的应用程序通常需要捕获小数(franctional)货币单位，并在执行算术时需要精确地模拟十进制四舍五入。许多现代系统(float,double)使用的基于二级制的浮点运算不能精确地表示小数，而且需要某种程度的近似，因而不适合于货币运算。因此，在货币数据建模时，这一约束是一个重要的考虑因素。

- 数字模型
如果需要查询数据库中精确、数学书有效匹配或需要执行Server端算术，则数字模型可能是适合的。

- 非数字模型
如果需要在Server端做一些对货币数值的数学计算，那么严格精度可能会更合适一些。


<br/>

#### 时间数据模型

MongoDB默认存储UTC时间，并将任何本地时间转换成这种形式。




<br>

---

<br/>

# MongoDB管理


administration

<br>

The administration 文档说明了MongoDB实例和部署正在进行的操作和维护。本文档包括这些问题的高级概述，以及涵盖操作MongoDB的特定过程的教程。


<br/>
<br>

## 操作清单(operation checklist)

如下清单，提供了帮助你避免在MongoDB部署中出现问题的建议。

<br/>

### 文件系统(file system)

- 将磁盘分区与RAID配置对齐；
- 避免对`dbpath`使用NFS。使用NFS会导致性能下降和不稳定；
- 针对Linux/Unix的文件格式，建议使用XFS或EXT4。如果可能的话，对MongoDB使用XFS性能会更好；
- 对于WiredTiger存储引擎，强烈建议使用XFS来避免使用EXT4时发现的性能问题；
- 针对Windows，不要使用FAT(FAT16/32/exFAT)文件系统，请使用NTFS文件系统。

<br/>

### 复制(replication)

- 验证所有非隐藏副本集成员的RAM, CPU, 磁盘, 网络设置, 配置等方面是否相同；
- 配置`oplog`的大小来适合你的用例；
- 确保副本集包好至少3个以journaling方式运行的数据承载节点；
- 在配置副本集成员时使用主机名(hostname)，而不是IP地址；
- 确保所有的mongod实例之间使用全双工网络；
- 确保每台主机都能解析它自己；
- 确保副本集包含奇数个投票的成员(voting members)，确保票数不会相等则一定会有主被选举出来；
- 确保mongod实例有0或1票；
- 为了高可用(high availability)，副本集集群最少部署3台数据中心。

<br/>

### 分片(sharding)

- 将配置服务器放置于专用硬件，以便在大型集群中实现最佳性能。确保硬件有足够的RAM来讲数据文件完全存储到内存中，并且有专门的存储；
- 使用NTP同步分片集群上所有组件的时钟；
- 确保Mongod, mongos和配置服务器之间的全双工网络连接；
- 使用CNAME将配置服务器标识到集群中，以便可以在不停机的情况下重命名和重新编号配置服务器。

<br/>

### Journaling

- 确保所有实例都使用journaling；
- 将journal放置于低延迟(low-latency)磁盘上，用于编写密集的工作负载。注意，这将影响快照式备份(snapshot)，因为构成数据库状态的文件将驻留在单独的volume上。

<br/>

### 硬件(hardware)

- 使用RAID10和SSD能够获得最佳性能；
- 确保每个mongod为它的dbpath提供了IOPS；
- 在虚拟环境中运行时，避免动态内存功能；
- 避免将所有副本集成员放置于相同的SAN(存储区网络)中。

<br/>

### 部署到云上

- AWS;
- Azure;
- Aliyun;
- Tencent.

<br>

### 操作系统配置

**Linux**

- 关闭`hugepages`和`defrag`；
- 调整存储数据库文件设备上的readahead设置，以适应用例；
- 在虚拟环境中的RHEL7/CENTOS7上禁用优化工具；
- 为SSD驱动使用`noop`或`deadline`磁盘调度；
- 禁用`NUMA`或将`vm.zone_reclaim_mode`设置为0，并运行`node interleaving`的mongod实例；
- 调整硬件的`ulimit`值以适应实例；
- 对dbpath挂载点使用`noatime`；
- 对你的部署配置足够的文件句柄(fs.file-max value of 98000)，内核pid限制(kernel.pid_max value of 64000)，每个进程的最大线程数(kernel.threads-max value 0f 64000)；
- 确保你的系统配置有swap交换分区；
- 确保系统默认TCP keepalived设置正确。

**Windows**

- 考虑禁用NTFS的最后访问时间更新。这类似与在Unix-like系统上禁用atime。

<br>

### 备份(backup)

- 安排备份和恢复过程的定期测试，以便手头有时间估计，并恢复其功能。

<br>

### 监控(monitor)

- 监视Server的硬件统计信息(磁盘使用，CPU，可用磁盘空间...)
- 监视mongodb的状态。

<br>

### 负载均衡(load balance)

- 配置负载均衡启用"sticky session"或“client affinity”，对现有连接有足够的超时时间；
- 避免放置负载均衡器在MongoDb集群或副本集组件。



<br>
<br/>


## 开发清单(development checklist)

如下清单，提供了帮助你避免在MongoDB部署期间出现的问题的建议。

<br>

### 数据持久性(data durability)

- 确保副本集至少包含3个(带有w:majority)数据承载节点，这3个数据承载节点需要为副本集的高数据持久性；
- 确保所有实例都是用了journaling。

<br>

### 架构设计(schema design)

MongoDB中的数据具有动态结构。collection并不要求document结构。这有助于迭代开发和多态性。然而，集合中的文档通常具有高度的同类结构。

- 确保你需要的集合集中的索引(indexes)支持你的查询(query)。除了`_id`索引，你必须显式的创建所有索引；
- 确保你的架构设计支持你的开发类型；
- 确保你的架构设计不依赖于长度不受绑定的索引数组；
- 再架构设计时考虑文档大小限制。

<br/>

### 复制(replication)

- 使用奇数个副本集成员以确保选举顺利进行。如果有偶数个成员，请使用仲裁者(arbiter)以确保级数的选票；
- 确保使用监控工具和适当的写关注来保持从库数据最新；
- 不要使用从库读取来扩展整体读取吞吐量。

<br/>

### 分片(sharding)

- 确保你的`sharded key`将负载均匀地分配到分片上；
- 对需要按分片数进行缩放的工作负载(workload)使用有针对性的操作；
- 对非目标(non-targeted)查询，总是从主节点读取可能对陈旧或孤立的数据很敏感；
- 当向新的非散列(hash)分片集合中插入大数据集时，Pre-split and manually balance chunks。

<br>

### 驱动(drivers)

- 使用连接池(connection pooling)；
- 确保你的应用程序在复制集选举期间还能够处理瞬时写入(transient write)和错误读取；
- 确保你的应用程序处理失败的请求并适时地重试它们；
- 使用指数退避逻辑重试数据库请求；
- 如果需要计算数据库操作的编译执行时间，对读操作使用`cursor.maxTimeMS()`，对写操作使用`wtimeout`。




<br>
<br/>


## 性能(MongoDB Perfomance)

- 当遇到性能下降时，通常与数据库的访问策略、硬件可用性和开放的数据库连接数相关；
- 一些用户可能由于不适当的索引策略或结果不足而经历性能限制，或由于架构设计模式差；
- 性能问题可能表明数据库正以最大限度运行，是时候给数据库添加额外的容量(capacity)了。尤其是，应用程序的工作集应该有足够的物理内存。

<br/>

### 锁紧性能(lock performance)

- MongoDB使用锁系统来确保数据集的一致性。如果某些操作需要长时间运行(long-running)，或队列窗体，随着请求和操作等待`lock`，性能将会下降；
- 锁相关的减速是可以间歇的，可查看`lock`部分是否影响了性能；
- `locak.deadlockCount`提供了遭遇死锁(deadlocks)的次数；
- 如果`globalLock.currentQueue.total`很高，则可能有大量的请求在等待`lock`。这表明并发问题(concurrency issue)可能影响性能；
- 如果`globalLock.totalTime`时间比`uptime`高，那么数据库在锁定状态中存在了大量时间；
- 长查询(long query)可能会导致索引无效使用、非最佳(non-optimal)建构设计、差的查询结构、系统体系结构问题、RAM不足导致页面错误(page fault)和磁盘读取。

<br/>

### 连接数(number of connections)

在某些情况下，应用程序和数据库之间的连接数量可能超出服务器处理请求的能力。`serverStatus`文档中的以下字段可以提供观察：

- `globalLock.activeClients`包含正在进行或排队的活动操作的客户端总数；
- `connnections`由以下两个字段组成： 1，`connections.current`连接到数据库实例的当前客户端总数； 2，`connections.available`可用的连接总数。

如果有大量的并发程序请求，则数据库可能无法满足需求。那么就需要增加部署的容量。

对于读操作巨大(read-heavy)的应用程序，增加你的副本集大小并将读操作分发给SECONDARY。
对于写操作巨大(write-heavy)的应用程序，部署分片并将一个或多个分片添加到分片集群中，以便在mongod实例之间分配负载。

连接数到达峰值也可能是应用程序或驱动错误所导致的结果。

除非收到系统范围的限制，否则MongoDB对传入连接没有限制。在基于Unix系统上，可使用`ulimit`命令或修改`/etc/sysctl`系统文件来修改系统限制。

<br>

### 数据库性能分析(database profiling)

MongoDB的**profiler**是一种数据库分析系统，可以帮助识别低效的查询和操作。

有如下分析级别(profiling-level)可用：

Level | Settiing
- | -
0 | Off.No profiling
1 | On.Only includes "slow" operations
2 | On.Includes all operations

在mongo shell中运行如下命令来配置性能分析器：

```sh
#dbsetProfilingLever()

db.setProfilingLevel(1)
```

`slowOpThresholdMs`的设置定义了什么是一个`slow`操作，要设置一个慢操作的阈值(threshold)，可以在运行时作为`db.setProfilingLevel()`操作的一个参数来配置`slowOpThresholdMs`。

默认情况下，mongod将会把所有的慢查询(slow query)记录到日志，这是由`slowOpThresholdMs`定义的。

通过在mongo shell中使用`show profile`，你可以在数据库中的`system.profile`集合中查看性能分析器的输出。
或者执行如下操作：

```sh
#返回超过100ms的所有操作，这个值请高于阈值`slowOpThresholdMs`

db.system.profile.find(
	{ millis: { $gt: 100 } }
)
```

你必须使用查询操作符去访问`system.profile`文档中的查询字段。


<br>

### 数据库性能分析器(databases profiler)

数据库性能分析器(db profiler)收集有关MongoDB的**写操作、游标和运行在mongod实例上的命令**的细微数据，你可以在每个数据库或每个实例基础上启用性能分析(profiling)。默认情况系，分析器是关闭的。启用profiling的时候需要配置`profiling leverl`。

The `database profiler`将所有的数据收集到`system.profile`集合中，它是一个限制集(capped collection)。

<br/>

#### 分析等级(Profiling levels)

- 0， 关闭分析器，不收集任何数据。`mongod`总是将操作时间长于`slowOpThresholdMs`的值写入日志。这是默认分析器级别；
- 1， 只收集慢操作的分析数据。默认是以100ms；
- 2， 收集所有数据库操作的分析数据。

<br/>

#### 启用分析器(profiling)和设置分析级别(profiling level)

当启用`profiling`，也要设置`profiling level`，分析器将数据记录到`system.profile`集合。当你在数据库中启用profiling后，MongoDB会在数据库中创建`system.profile`集合。

使用`db.setProfilingLevel()`来设置`profiling level`和启用profiling。

```sh
db.setProfilingLevel(1)
```

<br>

**指定慢操作的阈值(the Threshold for slow operations)**

慢操作的阈值(threshold)应用于整个mongod实例。当你修改了阈值，那你就对所有的数据库实例进行了修改。修改了数据库慢操作的阈值同样也会影响整个mongod实例性能分析子系统的慢操作阈值。
默认情况下，慢操作的阈值为100ms。性能分析`level-1`将会记录长于阈值的慢操作到日志。

要更改阈值，请将两个参数(parameter)在`mongo shell`传递给`db.setProfilingLevel()`。第一个参数是为当前的数据库设置`profiling level`，第二个参数是为整个mongod实例设置默认的慢操作阈值。

栗子：

```sh
mongo

>use zhang
>db.serProfilingLevel(1,100)

#会在zhang数据库下生产system.profile集合
```

<br/>

**检查分析等级(check profiling level)**

```
db.getProfilingStatus()
#default
#{ "was" : 0, "slowms" : 100 }


db.getProfilingLevel()
#0
```

<br/>

**为一个完整的mongod实例启用profiling**

在测试环境中，处于开发目的，你可以为一个完整的mongod实例启用profiling功能。性能分析等级应用于mongod实例中的所有数据库。

```
#设置level：1，slowOpThresholdMs: 50
mongod --profile 1 --slowms 50
```

<br/>

**数据库分析和分片**

无法对mongos实例启用profiling。要对分片集群启用profiling功能，你必须对分片集群中的每个mongod实例启用profiling功能才行。

<br>

#### 查看性能分析器的数据(profiler data)

数据库性能分析器关于数据库操作的日志信息放置于`system.profile`集合中。如需查看性能信息，请查询该集合。

栗子：

```
db.system.profile.find()

db.system.profile.find().limit(10).sort({ ts: -1 }).pretty()

#指定时间
db.system.profile.find( { millis: { $gt: 5 } } ).pretty()


#除了某个命令外
db.system.profile.find({ op: { $ne: 'cmd' } }).pretty

#某个特定集合
db.system.profile.find( { ns: 'db.collection' } ).pretty()


#显示最近的事件
show profile
```

<br/>

#### 分析器开销(profiler overhead)

分析器对性能影响很小。`system.profile`集合是一个默认大小为1MB的限制集。这样大小的集合通常可以存储上千份分析文档，但一些应用程序可能在每次操作中只使用或多或少的分析数据。

<br/>

**在Primary上面修改`system.profile`集合的大小**

1. 停止profiling；
2. 删除(drop)`system.profile`集合；
3. 新建一个`system.profile`集合；
4. 重启profiling。

```
use db
db.serProfilingLevel(0)
db.system.profile.drop()
db.createCollection( "system.profile", { capped: true, size: 4000000 } )
db.setProfilingLevel(1)
```

<br>

**在Secondary上修改`system.profile`集合的大小**

在Secondary上修改`system.profile`集合的大小，你必须停止Secondary，然后以standalone模式运行它，之后执行修改步骤。当做完上述步骤之后，以一个副本集成员的方式使用standalone模式重启它。


<br>
<br/>

### 禁用显见的大页面(Disable Transparent Huge Pages)

Transpatent Huge Pages(THP)是一个Linux的内存管理系统，通过使用更大的内存页，减少了在具有大量内存的机器上进行Translation Lookaside Buffer(TLB)查找的开销。

然而，数据库工作负载(workload)在THP中的性能往往很差，因为它们往往具有稀疏的(sparse)而不是连续的(contiguous)内存访问模式。你应该在Linux机器上禁用THP来确保MongoDB获得最佳的性能。

<br>


**1. 创建`init.d`脚本**

```sh
#!/bin/bash
### BEGIN INIT INFO
# Provides:          disable-transparent-hugepages
# Required-Start:    $local_fs
# Required-Stop:
# X-Start-Before:    mongod mongodb-mms-automation-agent
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Disable Linux transparent huge pages
# Description:       Disable Linux transparent huge pages, to improve
#                    database performance.
### END INIT INFO

case $1 in
  start)
    if [ -d /sys/kernel/mm/transparent_hugepage ]; then
      thp_path=/sys/kernel/mm/transparent_hugepage
    elif [ -d /sys/kernel/mm/redhat_transparent_hugepage ]; then
      thp_path=/sys/kernel/mm/redhat_transparent_hugepage
    else
      return 0
    fi

    echo 'never' > ${thp_path}/enabled
    echo 'never' > ${thp_path}/defrag

    re='^[0-1]+$'
    if [[ $(cat ${thp_path}/khugepaged/defrag) =~ $re ]]
    then
      # RHEL 7
      echo 0  > ${thp_path}/khugepaged/defrag
    else
      # RHEL 6
      echo 'no' > ${thp_path}/khugepaged/defrag
    fi

    unset re
    unset thp_path
    ;;
esac
```


**2. 使之可执行**

```sh
chmod 755 /etc/init.d/disable-transparent-hugepages
```


**3. 配置操作系统以在开机的时候运行它**

```sh
#Debian系列
update-rc.d disable-transparent-hugepages defaults


#RedHat系列
chkconfig --add disable-transparent-hugepages


#SUSE
insserv /etc/init.d/disable-transparent-hugepages
```


**4. 如果适用，覆盖(override)tuned和ktune**

```sh
#RedHat/CentOS7
mkdir /etc/tuned/no-thp


vim /etc/tuned/no-thp/tuned.conf

[main]
include=virtual-guest
[vm]
transparent_hugepages=never


tuned-adm profile no-thp
```


**5. 测试你做的改变**

```sh
cat /sys/kernel/mm/redhat_transparent_hugepage/enabled
cat /sys/kernel/mm/redhat_transparent_hugepage/defrag

#always madvise [never]
```

<br>

**另一种简便的方式来禁用THP**

```sh
vim /etc/rc.d/rc.local

echo 'never' > /sys/kernel/mm/transparent_hugepage/enabled
echo 'never' > /sys/kernel/mm/transparent_hugepage/defrag

chmod u+x /etc/rc.d/rc.local
```


<br/>
<br>

### Unix系统下的`ulimit`的设置

大多Unix-Like系统，都提供了限制每个进程和每个基本用户使用线程，文件和网络连接等系统资源的一些方法。
**ulimits**防止单个用户使用太多的系统资源。有时，这些限制的默认值太小，这会导致MongoDB操作过程中出现一系列问题。

```sh
#限制文件
#/etc/security/limits.conf
#/etc/security/limits.d/
```

<br/>

#### 资源利用

mongod和mongos每次使用线程和文件描述符来跟踪连接和管理内部操作。

通常情况下，所有的mongod和mongos实例：

- 利用每一个文件描述符和线程来跟踪每个即将到来的连接；
- 将每个内部线程或pthread作为一个系统进程来跟踪。

<br>

**mongod**

- mongod实例使用的每个数据文件都有一个文件描述符；
- 当`storage.journal.enabled`为true是，mongod进程实例使用的每个日志文件都有一个文件描述符；
- 在复制集中，每个mongod保持一个连接复制集中所有其他集合成员的连接。

**mongos**

- mongos实例与每个分片都保持一个连接池，所有mongos可以重用连接，这样因为不用建立新连接，从而能快速的满足请求；
- 通过限制连接数，可以防止mongos因在mongod实例上创建太多连接而产生级联效应。


<br>

#### 资源限制的设置

`ulimit`是指每个user使用各种资源的限制值。因此，无论你的mongod实例是以单个用户多进程执行还是以多mongod进程执行，都可以看到对这些资源的连接。

ulimits有**hard**和**soft**两个方式。

- hard：是指用户在任何时候都可以活动的进程的最大数量，这是上限。没有任何non-root进程能够增加hard ulimit；
- soft：是对会话或进程实际执行的限制，但任何进程都可以将其增加到hard ulimit的最大值。

较低的soft limit可能无法创建新线程(thread)，如果连接数太高，则关闭错误连接。因此，将soft和hard的值都设置为推荐值是非常重要的。

修改`ulimit`设置之后，要重启程序修改值才会有效。可通过`/proc`文件系统查看运行进程当前的限制值。

使用`ulimit`对系统限制的改变在系统重启后都会恢复到默认值。需要修改其它文件来确保修改一直生效。

**ulimit**

```sh
ulimit -a

core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 7170
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 7170
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

<br>

**修改ulimit**

```sh
#-f (文件大小)
#-t (cpu 时间)
#-v (虚拟内存)
#-n (单个进程文件打开数)
#-m (memory size)
#-u (可打开的进程/线程)

ulimit -t unlimited
ulimit -u 64000
```


<br>
<br>

## 配置和维护(maintenance)


### Run-time databases configuration

command line和configuration file interfaces为MongoDB管理员提供了控制数据库系统操作的大量选项和设置。

使用配置文件启动MongoDB实例：

```sh
mongod --config /etc/mongod.conf
mongod -f /etc/mongod.conf
```

<br>

#### 配置数据库

mongodb的配置文件从MongoDB3.0以后使用YAML格式。

```sh
vim /etc/mongod.conf


processManagement:
   fork: true
net:
   bindIp: 127.0.0.1
   port: 27017
storage:
   dbPath: /var/lib/mongodb
systemLog:
   destination: file
   path: "/var/log/mongodb/mongod.log"
   logAppend: true
storage:
   journal:
      enabled: true
```

对于大多数以standalone模式运行的servers，以上是一个足够的基本配置。
Unix-Like操作系统需要以超级用户(root)权限才能运行端口小于1024的程序。

<br>

#### 安全考虑(security consideration)

下面的配置选项集合对于限制对于mongod实例的访问非常有用。

```sh
net:
   port: 27017
   bindIp: 127.0.0.1,192.168.1.11

security:
   authorization: enabled
```

<br>

#### 复制集和分片配置(replication and sharding configuration)

复制集的配置非常简单，只需要replSetName在集合中的所有成员具有一致的副本集名字。

```sh
replication:
  replSetName: zhang
```

开启副本集认证：

```sh
#利用openssl生成keyFile
openssl rand -base64 256 > /dir/path/mongodb/keyFile


security:
  replSetName: zhang
  keyFile: /dir/path/mongodb/keyfile


chown -R mongod:mongod /dir/path/mongodb
```

设置`keyFile`启用身份认证，并为复制集成员在相互身份认证时使用的认证文件指定一个密钥文件。密钥文件的内容是任意的，但在复制集中的所有成员和连接到该集合的mongos实例之间必须相同。不然怎么能认证通过呢。
秘钥文件的大小必须**小于1KB**，并且只能包含**base64集**中的字符，并且此密钥文件在Unix系统上必须**not have group**或**not have world permissions**。


<br>

#### 分片配置(sharding configuration)

分片要求配置服务器和分片服务器的Mongod实例具有不同的mongod配置文件。配置服务器存储集群的元数据(metadata)，而分片服务器存储数据(data)。

在配置文件中给mongod实例配置**配置服务器(config-server)**，给`sharding.clusterRole`指定配置服务器。

```sh
#配置config-server

net:
  bindIp: 192.168.1.11
  port:27001
replication:
  replSetName: zhang
sharding:
  clusterRole: configserver
#configserver必须要是一个部署的副本集
```

<br>

#### 在同一个系统上运行多个数据库实例(multiple database instances)

**在许多情况下，在单个系统(single system)上运行多个数据库实例是不推荐的。**

但可能由于一些部署或者测试的目的，你需要在单个系统上运行多个mongod实例。在这些情况下，请为每一个mongod实例使用一个基本的配置文件，但要额外配置如下值：

- dbpath(必须);
- pidFilePath(必须);
- systemLog(非必须，但建议开启);

栗子：

```sh
#mongod_27017实例
vim /etc/mongod_27017.conf

systemLog:
  path: /var/log/mongod_27017.log
storage:
  dbPath: /var/lib/mongodb27017
processManagement:
  pidFilePath: /var/lib/mongodb27017/mongod_27017.pid



#mongod_27018实例
vim /etc/mongod_27018.conf

systemLog:
  path: /var/log/mongod_27018.log
storage:
  dbPath: /var/lib/mongodb27018
processManagement:
  pidFilePath: /var/lib/mongodb27018/mongod_27018.pid


##启动实例
mongod -f /etc/mongod_27017.conf
mongod -f /etc/mongod_27018.conf
```

<br/>

#### 诊断配置(diagnostic configuration)

以下配置选项可控制各种mongod行为，用以诊断的目的：

- `operationProfiling.mode`设置`database profiler level`。profiler在默认情况下不处于活动状态，因为它本身可能会影响性能。除非启用它，否则不会对查询进行分析；
- `operationProfiling.slowOpThresholdMs`配置慢操作的阈值以确定查询是否**慢**，用以作为分析器记录日志的目的。默认阈值是100ms；
- `systemLog.verbosity`控制mongod写入日志的日志输出量。只有在遇到未在正常日志记录级别中反映的问题是才启用此选项。



<br>
<br>

### 升级(upgrade)到最新的MongoDB

修订(revisions)提供了security patches、bug fixes以及不包含任何反向破坏更改的新的或更改的功能。但是，最新版本也可能存在一些兼容性问题，请注意。

<br/>

#### 升级之前(before upgrading)

- 确保备份了最新的数据集；
- 有关特定MongoDb版本的特殊事项和兼容性问题，请注意查看；
- 如果你的安装包包括了复制集，在升级期间预定维护窗口(maintanence window)。

<br/>

#### 升级程序(upgrade procedure)

**在升级之前请一定要备份所有数据！**

按照如下步骤升级：

1. 对于使用认证的部署，首先升级所有的MongoDB drivers；
2. 升级分片集群；
3. 升级任一standalone实例；
4. 升级不属于分片集群的任一副本集。

<br/>

#### 升级一个MongoDB实例

要升级mongod或mongos实例，使用如下方法之一：

- 使用操作系统的包管理工具和官方MongoDB包进行升级(推荐的方法)；
- 使用新二进制文件替换现有二进制文件来升级实例。

<br/>

#### 替换现有二级制文件(replace the existing binaries)

**在升级MongoDB前请一定备份你的所有数据！**

首选的升级方式是使用包管理工具和官方的MongoDB包。

通过替换现有二进制文件来升级mongod或mongos实例，执行如下操作：

1. 下载最新MongoDB二进制文件到本地，并解压缩到MongoDB安装目录；
2. 关闭实例；
3. 替换二进制文件；
4. 重启实例。

<br/>

#### 升级分片集群

1. 禁用分片集群的平衡器(blancer)；
2. 升级配置服务器(config-server)；
3. 升级每个分片；
4. 升级每个mongos实例；
5. 重新启用平衡器。

<br/>

#### 升级复制集

若要升级复制集，请单独升级每个副本集成员。从Secondary开始，最后以Primary结束。

<br>

**升级SECONDARY**

1. 升级SECONDARY的mongod实例；
2. 升级一个Secondary之后，在升级下一个实例之前，请等待Secondary恢复(recover)到SECONDARY state。使用`rs.status()`命令来检查复制集成员的状态。

**升级PRIMARY**

1. 使用`rs.stepDown`命令来退出primary，以启动正常的故障转移过程；
2. 查看是否有另外的SECONDARY节点成为了PRIMARY节点；
3. 关闭并升级实例。



<br>
<br/>

### 管理mongod进程

#### 开启mongod进程

```sh
mongod


#指定数据目录
mongod --dbpath /dir/mongodb/


#指定TCP端口
mondod --port 12345


#将mongod以守护进程的方式启动
mongod --fork --logpath /var/log/mongod.log


#其他选项
mongod --help
```

<br/>

#### 停止mongod进程

```sh
#使用shutdownServer()
use admin
db.shutdownServer()


#使用--shutdown
mongod --shutdown


#使用ctrl+c
ctrl+c


#使用kill
#千万不要使用kill -9(SIGKILL)来终止mongod
kill mongod_pid
kill -2 mongod_pid
```

<br/>

#### 停止一个复制集

步骤：

1. 检查SECONDARY的oplog的时间戳；
2. 如果从节点的时间戳落后于主节点10s内，mongod将会返回不会被关闭的消息。你可以传递一个`timeoutSecs`参数给shutdown命令来等待从节点追上主节点；
3. 一旦从节点追上进度或60s后，主节点将会关闭。

强制关闭复制集：`db.adminCommand( { shutdown: 1, force: true } )`

如果没有节点能立刻更新到最新的数据，发送`shutdown`加上`timeoutSecs`参数来在指定的时间内保持对从节点的检查。如果在分配的时间内有任意的一个从节点追上，主节点将会关闭。反之，主节点不会关闭。

```js
db.adminCommand({ shutdown: 1, timeoutSecs: 5 })

#或
db.shutdownServer({ timeoutSecs: 5})
```


<br>
<br/>

### 终止(Terminate)运行的操作

MongoDB提供了两种方法来终止正在运行的操作。

- `maxTimeMS()`
- `db.killOp()`

<br>

#### maxTimeMS()

`maxTimeMS()`方法给一个操作(operation)设置了时间限制(time limit)。这个时间单位默认是毫秒(ms)。当这个操作达到了指定的时间限制时，MongoDB将在下一个中断点(interrupt point)中断这个操作。

栗子：

```js
db.location.find(
  {
	"town": { "$regex": "(Pine Lumber)",
    			"$options": 'i' }
  } ).maxTimeMS(30)



db.runCommand(
  {
	distinct: "collection",
    key: "city",
    maxTimeMS: 45
  }
)
```

<br/>

#### killOp

`killOp()`方法将在下一个中断节点中断正在运行的操作。`killOp()`方法通过**操作ID**(operation ID)来标识目标操作。

栗子：

```js
db.killOp(<opID>)


#查看正在运行的操作
db.currentOp()
```

**注意：**
终止正在运行的操作时一定要谨慎！只使用`db.killOp()`方法来终止由客户端发起的操作，而不要终止内部数据库(internal database)的操作。


<br>
<br/>

### 轮询(rotate)日志文件

当使用`--logpath`选项或`systemLog.path`设置时，mongod或mongos实例会将所有活动和操作的实时账户报告给日志文件。默认情况下，只有当使用了`logRotate`命令，或者mongod或mongos进程从操作系统接收到一个`SIGUSR1`信号时，才会进行日志轮询响应。

MongoDB的标准日志轮询方法会存档当前日志文件并启动一个新的日志文件。为此，mongod或mongos实例将通过ISODate日期格式的UTC时间戳来重命名当前日志文件。然后它会打开一个新的日志文件，关闭旧的日志文件，并将所有新的日志发送到新的日志文件。

你也可以通过配置MongoDB的`systemLog.logRatate`或`--logRotate`选项，来支持Unix/Linux的日志轮询功能。
最后，你可以使用`--syslog`选项来配置mongod发送日志数据到系统日志。在这种情况下，你可以选用其他的日志轮询工具。

<br>

#### 默认日志轮询行为

**在mongo shell中轮询日志：**

```sh
#开启一个实例
mongod -v --logpath /var/log/mongodb/test01.log

#列出日志文件
ls /var/log/mongodb/test01.log*

#轮询日志文件
mongo
>use admin
>db.runCommand({ logRotate: 1 })

#查看新的日志文件
ls /var/log/mongodb/test01.log*
#new: test01.log
#old: test01.log-2018-01-11T08-22-50
```

**使用`--logRotate reopen`选项轮询日志：**

```sh
mongod -v --logpath /var/log/mongodb/test01.log --logRotate reopen --logapend

ls /var/log/mongodb/test01.log*

mongo
>use admin
>db.runCommand({ logRotate: 1 })
```

<br/>

#### 系统日志轮询(Syslog log rotate)

```sh
mongod --syslog
```

<br/>

#### 使用SIGUSR1强制日志轮询

对于基于Unix/Linux的系统，可以使用**SIGUSR1**信号来轮询单个进程的日志。

```sh
kill -SIGUSR1 <mongod-pid>
```



<br/>
<br>

## 数据中心意识(data center awareness)


### MongoDB部署中的分离(segregation)操作

MongoDB拥有许多特性，包括允许数据库管理员和开发者在部署数据库的过程中通过一些功能或地理组群对数据库应用进行分割操作。

MongoDB支持跨越不同维度的操作的分段，这可能包括了在单个数据中心(single data center)的部署中的多数据中心(multi-date center)部署、机架、网络或电源电路的多个数据中心和地理区域。

MongoDB还支持基于功能或操作参数的数据库分离操作，以确保某些mongod实例仅用于报告工作负载，或只在特定的分片上分离集合的某些高频部分。

<br>

特别是在MongoDB中你可以：

- 确保写操作传播到复制集的特定成员；
- 确保复制集中的特定成员响应了查询操作；
- 确保分片键在具体范围上的平衡，并且驻留在特定的分片上。


<br>

### 区域(zone)

#### 管理分片区域(manage shard zones)

<br>

#### 按位置分割数据(segementing data by location)

<br/>

#### 为SLA或SLO改变分层硬件

<br>

#### 按应用程序或客户分割数据

<br/>

#### Distributed Local Writes for Insert Only Workloads

<br/>

#### 管理分片区域




<br>
<br/>

## MongoDB备份方案(backup methods)

**在生存中部署MongoDB时，如果发生数据丢失的事件，你应该指定一个捕获和恢复备份的策略(strategy)。**

<br/>

**back up with MongoDB cloud manager or Ops manager**

- MongoDB Cloud Manager
- Ops Manager

<br>

**复制底层数据文件进行备份(back up by copying underlying data files)**

- 使用文件系统快照备份(back up with filesystem snapshots)

你可以通过复制MongoDB的底层数据文件来创建MongoDB部署的备份。
如果MongoDB储存其数据文件的卷(volume)支持时间点快照(point-in-time snapshots)，则可以使用这些快照在某个时刻创建MongoDB系统的备份。
文件系统的快照是一个操作系统的卷管理器的功能，并没有具体到MongoDB。通过文件系统快照，操作系统将卷的快照用作数据备份的基准。快照的机制取决于底层的存储系统。
例如，在Linux上，逻辑卷管理器(LVM)可以创建快照。

要获得运行中的MongoDB进程的正确快照，必须启用日志记录(jorunaling)，并且日志必须与其它MongoDB数据文件存储在相同的逻辑卷上。如果没有启用日志记录，则无法保证快照将是一致有效地。

为了获得分片集群一致的快照，你必须禁用平衡器(balancer)和捕捉每一个分片的快照以及大约在同一时刻的配置服务器。


- 使用`cp`或`scp`备份

如果你的系统不支持快照功能，则可以使用`cp`，`rsync`或类似的工具直接复制文件。
由于复制多个文件不是原子操作，因此你必须在复制文件之前停止对mongod的所有写入。否则，你将复制处于无效状态的文件。

复制底层数据而产生的备份不支持复制集的时间恢复节点，并且难以管理更大的共享集群。此外，这些备份很大。因为它们包括索引和复制底层存储填充和分片。
相反，`mongodump`会创建较小的备份。

- 使用`mongodump`备份

如果在`mongodump`创建备份的同时，应用程序对数据进行修改，那么`mongodump`将会与这些应用竞争资源。

`mongodump`从一个MongoDB数据库中读取数据，并创建高保真度(high fidelity)的BSON文件。`mongorestore`工具可使用这个文件来进行MongoDB数据库恢复。
`mongodump`和`mongorestore`是用于备份和恢复小型MongoDB部署的简单和高效的工具，但对于捕获较大的系统并不理想。

`mongodump`和`mongorestore`针对正在运行的mongod进程进行操作，可以直接操纵底层的数据文件。默认情况下，`mongodump`不会捕获`local database`数据库的内容。

`mongodump`只捕获数据库中的文档(documents)，用以给备份节省空间，但`mongorestore`或mongod必须在恢复数据之后重建索引。

当连接到MongoDB实例时，`mongodump`可能会对MongoDB的性能产生不利影响。如果你的数据大小大于系统内存，查询可能会将工作单元从内存中推开，从而导致页面错误。

当`mongodump`在捕获输出时，应用程序可以继续修改数据。对于复制集来说，`mongodump`提供了`--oplog`选项来用以在`mongodump`操作期间包含数据的oplog条目。这允许相应的`mongorestore`操作去还原所捕获的oplog。

然而，对于复制集来说，请考虑使用MongoDB Cloud Manager 或 Ops Manager来备份。



<br/>
<br>

### 使用文件系统快照进行备份和恢复(back up and restore with filesystem snapshots)

使用系统工具创建MongoDB系统的备份，诸如**LVM**，或**block-level**备份方法。使用系统工具来创建MongoDB数据文件的设备的副本。这些方法完成迅速、工作可靠，但是需要在MongoDB之外进行额外的系统配置。


<br/>


#### 快照综述(snapshots overview)

快照的工作方式是在实时数据(live data)和一个特定快照卷之间创建指针(pointer)。这个指针在理论上等同于**硬链接(hard link)**。作为工作数据偏离的快照，快照过程使用**写时复制**(copy-on-write)策略。结果，快照又只存储修改的数据。

创建快照后，在文件系统上挂载(mount)快照镜像，并从中复制数据。生成的备份包含所有数据的完整副本。


<br/>

**Valid database at the time of snapshot**

当快照生成时数据库必须有效。这就意味着数据库所接收的所有写入(write)都需要完整的写入磁盘————无论是`journal`还是数据文件。
如果备份发生时磁盘上没有写入(write)，备份将不反映这些更改。

对于WiredTiger storage engine，数据文件反映了最后一个检查点(last checkpoint)的一致状态。每2GB的数据或每分钟就会出现检查点。

<br/>

**Entire disk image**

快照创建一个整个磁盘镜像的镜像。除非你需要备份你的整个系统，否则考虑隔离(isolate)你的MongoDB数据文件、journal，并配置一个不包含任何其他数据的逻辑磁盘。
或者，将所有的MongoDB数据文件保存在一个专用的设备上，这样你就可以在没有重复(duplicating)和无关(extraneous)数据的情况下进行备份。

<br/>

**Site failure precaution**

确保将数据从快照复制到其他系统。这确保了在站点故障(site failure)的时候数据是安全的。

<br>

**No incremental backups**

本教程不包含增量备份(incremental backups)的过程。虽然不同的快照方法提供了不同的功能，但下面列出的LVM方法不提供捕获增量备份的任何容量。

<br>

**Snapshots with journaling**

如果你的mongod实例启用了journaling，则可以使用任何类型的文件系统和volume/block level快照工具来创建备份。

如果你在基于Linux的系统上管理你自己的基础架构，请使用LVM配置你的系统以提供磁盘包并提供快照功能。



<br>


#### 在Linux上使用LVM进行备份和还原

生产备份系统必须考虑一些特定环境的应用程序特定需求和因素。

<br/>

**Crete a snapshot**

- 确保你创建的快照具有足够的空间来考虑数据的增长；
- 如果快照超出了空间，快照镜像将无法使用。请放弃这个逻辑卷并创建另外一个；
- 命令执行完毕时快照将存在。你可以随时直接从快照进行还原，也可以创建新的逻辑卷并从此快照还原到备用镜像；
- 虽然快照对于快速创建高质量的备份非常好，但它们并不是理想的作为存储备份数据的格式；
- 快照通常取决于并位于与原始磁盘镜像相同的存储基础架构上。因此，将这些快照存档并将其存储在别处至关重要。

```sh
#下面的这个vg-name指卷组名，这个卷组首先需要建立
#系统卷组和设备的位置和路径可能因LVM的配置二略有不同
#此大小不反映数据大小

lvcreate --size 1G --snapshot --name mongodb-snap20180111 /dev/vg-name/mongodb
```

<br>

**Archive a snapshot**

创建好snapshot之后，挂载`mount`快照并将数据复制到单独的存储中。

压缩快照：

```sh
umount /dev/vg-name/mongodb-snap01
dd if=/dev/vg-name/mongodb-snap01 | gzip > mongodb-snap01.gz
```

<br/>

**Restore a snapshot**

同样适用LVM进行还原。

```sh
#lv-mongodb, vg0-vgname

lvcreate --size 1G --name mongodb vg0
gzip -d -c mongodb-snap01.gz | dd of=/dev/vg0/mongodb
mount /dev/bg0/mongodb /dir/path
```

还原的快照中有一个陈旧的`mongo.lock`文件，如果你没有从快照中删除此文件，那么MongoDB可能会认为锁文件指示的是不正常的关闭。如果你开启了`storage.journal.enabled`，但没有使用`db.fsyncLock()`的话，那不需要删除`mongo.lock`文件，反之，删除它。

<br/>

**Restore directly form a snapshot**

不使用gz压缩文件下还原备份。

```sh
umount /dev/vg-name/mongodb-snap01
lvcreate --size 1G --name mongodb vg0
dd if=/dev/vg0/mongodb-snap01 of=/dev/vg0/mongodb
mount /dev/vg0/mongodb /dir/path
```

<br/>

**Remote backup storage**

可以使用组合的进程和SSH实施离线备份。

```sh
umount /dev/vg-name/mongodb-snap01
dd if=/dev/vg0/mongodb-snap01 | ssh user@host gzip > /dir/path/mongodb-snap01.gz
lvcreate --size 1G --name mongodb vg0
ssh user@host gzip -d -c /dir/path/mongodb-snap-01.gz | dd of =/dev/vg0/mongodb
mount /dev/vg0/mongodb /dir/path
```


<br/>


#### 使用单独卷上的Journal日志文件或没有Journal日志文件进行备份实例

从MongoDB3.2开始，为了使用WiredTiger对MongoDB实例进行volume-level备份，数据文件和Journal日志文件不再要求驻留在一个卷上。

如果你的mongod实例没有使用Journal，或者启用了将Journal志文件放置于一个单独的卷上，则必须刷新(flush)对磁盘的所有写入，并在备份期间锁住数据库用以阻止写操作。
如果有复制集(replica set)配置，那么你可以在SECONDARY上不接收读取用以备份数据。

<br>

**1. 刷新写入磁盘并锁定数据库以防止进一步的写入：**

```
#锁住数据库
db.fsyncLock();
```

**2. 使用快照备份数据库：**

**3. 解锁数据库：**

```
#解锁数据库
db.fsyncUnlock();
```



<br>
<br/>



### 使用MongoDB工具进行备份和恢复(back up and restore with MongoDB tools)

使用MongoDB提供的备份还原工具——`mongodump`和`mongorestore`来处理BSON data，对于创建小型部署的备份是很有用的。
对于弹性(resilient)备份和非破坏性(non-disruptive)备份，使用文件系统或块级磁盘快照。

因为`mongodump`和`mongorestore`操作通过与正在运行中的`mongod`实例进行交互(interacting)，它们会影响正在运行的数据库的性能(performance)。这些工具不仅会为正在运行的数据库实例创建流量，还会强制数据库通过内存读取所有的数据。当MongoDB读取不经常(infrequently)使用的数据时，它会驱逐(evict)频繁(frequently)访问的数据，导致数据库正常工作负载的性能下降。

<br>

当使用MongoDB's tools 来备份你的数据时，考虑如下建议：

- 标签文件(label file)，以便你可以识别备份的内容以及备份所反映的时间点
- 如果对你来说，`mongodump`和`mongorestore`对性能的影响是不可接受的，请使用替代备份策略——filesystem snapshot或MongoDB CloudManager
- 使用`--oplog`去捕获在`mongodump`期间的传入写(write)操作，以确保备份一致性的数据状态
- 通过将备份文件还原到测试环境中，以确认备份是可用的


<br>


#### MongoDB tools

**MongoDB工具介绍及区别：**

- mongoexport
`mongoexport` is a utility that produces a **JSON or CSV** export of data stored in a MongoDB instance.

- mongoimport
The `mongoimport` tool imports content from an **Extended JSON, CSV, or TSV** export created by mongoexport, or potentially, another third-party export tool.

- mongodump
`mongodump` is a utility for creating a **binary export of the contents of a database**. mongodump can export data from either mongod or mongos instances.
mongodump excludes the content of the local database in its output.
The mongodump utility backs up data by connecting to a running mongod or mongos instance.

- mongorestore
The `mongorestore` program writes data from a **binary database dump** created by mongodump to a MongoDB instance.


<br>


#### 步骤(Procedures)


##### 使用mongodump备份

- `mongodump·备份数据库，如果数据库启用了访问控制，则必须拥有每个备份的数据库查询的权限。内置的备份角色提供了执行任何和数据库备份有关所需的权限。
- 这就意味着你使用`mongodump`的user必须要对所备份的数据库有读取权限。
- `mongodump`能够为**整个服务器、数据库或集合**创建备份，或者使用查询仅备份集合的一部分。
- `mongodump`默认排除`local`数据库。
- `mongodump`必须要能够连接到正在运行的mongod或mongos实例。默认连接为127.0.0.1:27017。
- `mongodump`默认创建在当前目录下创建./dump备份文件。
- 如果`mongodump`备份目录中已经存在备份数据目录，那么`mongodump`将会覆盖它们。
- 指定认证库来认证你的用户名和密码。

<br/>

**使用oplog进行时间点操作**

- 在`mongodump`中使用`--oplog`选项来收集oplog条目，用以在副本集中构建数据库的实时快照。
- 使用`--oplog`，`mongodump`会从源数据库复制所有的数据，包括备份开始到结束这段时间所有的oplog记录。
- 在`mongorestore`还原时使用`--oplogReplay`选项，允许你还原特定时间节点的备份。这就对应在`mongodump`期间oplog的记录。


```sh
#127.0.0.1:27017 ./dump
mongodump


#--host,-h  --port
mongodump -h mongodb.example.net --port 27107
mongudump -h 127.0.0.1 --port 27018


#-o, --out
mongoodump -o /var/mongodb_backup/
mongodump --host 127.0.0.1 --port 27017 --out /var/mongodb_backup/


#--collection, --db
mongodump --db zhang --out /var/mongodb_backup/zhang
mongodump --db zhang --collection test


#--authenticationDatabase
mongodump --port 27018 -u zhang -p "passwd"  --authenticationDatabase admin -d zhang -o /var/mongodb_backup/zhang
```



<br>


#### 使用mongorestore还原

若要将数据还原到启用了访问控制的MongoDB部署，如果备份数据不包括`system.profile`集合数据，则restore角色提供了对数据库的访问权限。

如果备份数据包含了`system.profile`集合并且目标数据库不包含`system.profile`集合，那么`mongorestore`会去创建这个集合即使`mongorestore`并没有还原`system.profile`文档。因此，用户就需要额外的权限才能在`system.profile`集合中上执行`createCollection`和`convertToCapped`。

如果使用`--oplogReplay`，这个restore角色还不足以重放oplog。所以如果需要重放oplog，请使用一个能够重放oplog的角色。


```sh
mongorestore /var/mnogodb_backup


mongorestore /var/mnogodb_backup --oplogReplay


mongorestore --port 27018 -u zhang -p "passwd" --authecticationDatabase admin -d zhang /var/mongodb_back/zhang
```


<br/>

#### 批量化操作mongo shell(`EOF`)

```sh
for coll in {collection1,collection2,...}
do
	mongo host:port/db -u x -p xx << EOF
    use db
    db.$coll.drop()
    EOF
done
```




<br>
<br/>


### 从MongoDB备份中还原副本集

你不能将单个数据集(data set)还原为三个新的mongod实例，然后为此创建一个副本集(replication set)。
如果你将数据集复制到每个mongod实例，然后创建副本集，则MongoDB将强制SECONDARY执行`initial sync`。


<br>

#### 向一个单一副本集节点中还原数据(Restore Database into a Single Node Replica Set)

1. 获取备份数据库文件

2. 使用备份数据库文件作为数据库路径启动一个mongod实例

```sh
#方法1，直接启动
mongod --dbpath /dir/path/mongodump --replSet <replName>


#方法2，使用配置文件启动，推荐
vim /etc/mongod.conf

storage:
  dbPath: /dir/path/mongodump
replication:
  replSetName: zhang

```

3. 连接到mongo shell

4. 初始化这个新的副本集

```
#对于有且仅有一个成员的副本集使用rs.initiate()
rs.initiate()
```

<br/>

#### 向副本集中添加成员(Add Members to the Replica Set)

MongoDB对于还原副本集SECONDARY节点提供了两种选择：

1. 手动复制数据库文件到数据目录
2. 允许`initial sync`

**建议：**

> 如果备份的数据库文件很大，那么`initial sync`可能需要很长的时间才能完成。对于大型数据库，最好将数据库文件复制到每台主机上。

<br>

##### Copy Database File and Restart mongod Instance

1. Shut down the mongod instance that you restored

> 使用 `--shutdown` 或 `db.shutdownServer()`来确保一个正常干净的关闭

2. 复制Primary的数据目录到每个从节点

3. Start the mongod instance that you restorerd

4. Add the secondaries to the replica set

```sh
PRIMARY>rs.add()
```

<br/>

##### Update Secondaries using Initial Sync

1. 确保副本集成员的数据目录为空
2. 将每个潜在成员添加到副本集



<br>
<br/>


### 备份和还原分片集群(sharded cluster)


<br>

#### 通过文件系统快照(fs snapshots)备份一个分片集群


<br>


#### 通过Database Dumps备份一个分片集群


<br>


#### Schedule Backup Window for Sharded Clusters


<br/>


#### 还原一个分片集群



<br/>
<br>


### 从意外关闭中恢复(Recover a standalone after an unexpected shutdow)

当一个standalone模式的mongod实例关闭了journaling功能后，一个unclean的shutdown可能会导致数据处于不一致的状态。
当unclean shutdown之后，如果在dbPath下存在一个非空的`mongod.lock`文件，则mongod实例会记录如下信息：

> Dectected unclean shutdown - mongod.lock is not empty

这样的话你必须要修复你的数据库，才能正常的启动mongod。

> **警告：**
> 不要用如下方法处理副本集 unclean shutdown。相反，你应该从备份或者从另一个副本集的成员恢复。

默认情况下，MongoDB在启用journaling的情况下运行，以防止发生unclean shutdown时数据不一致的问题。


使用运行mongod实例的那个用户来进行修复，避免由权限不一致而导致的新问题。

1. Create a backup of the data files
2. Start mongod with --repair




<br>
<br/>


## 监控(Monitoring)MongoDB

监控是数据库管理的重要组成部分，充分了解MongoDB的运行状态，并在没有危机的情况下维护和部署。此外，了解MongoDB的正常操作参数将允许你在问题升级成为故障前诊断他们。


<br>


### Monitoring for MongoDB

<br/>

#### Monitoring Strategies(策略)

有三种方法可以从运行中的MongoDB实例中收集状态信息：

1. MongoDB提供的一组实时上报程序，提供数据库活动的实时报告；
2. 数据库命令以更大的保真度返回有关当前数据库状态的统计信息；
3. MongoDB Atlas，MongoDB Cloud Manager；

每个策略在不同的情况下都是很有用的，所以它们能够很好地进行互补。

<br>

#### MongoDB Reporting Tools

**Utilities**

MongoDB提供了许多可以返回活动统计信息的实用工具，这对于诊断问题和评估操作非常有用。

- `mongostat`
	`mongostat`按类型捕获并返回数据库操作的计数(insert,query,update,delete...)

![mongostat](/images/MongoDB/mongostat.png)

- `mongotop`
	`mongotop`通过类型捕获和返回数据库操作(insert,query,update,delete)

![mongotop](/images/MongoDB/mongotop.png)

<br/>

#### Commands

MongoDB包含了许多上报数据库状态的命令。这些命令可以提供比上面的实用程序更精细的粒度级别。考虑在脚本和程序中使用它们的输出来开发自定义警报。
`db.currentOp`方法是一个识别当前数据库实例正在进行的操作。

- `db.serverStatus()`
	`db.serverStatus()`，返回数据库状态的一般概述，详细的磁盘使用，内存使用，连接，journaling日志和索引访问。它返回快速并不影响MongoDB性能。

- `db.stats()`
	`db.stats()`，提供了database上的统计信息。返回使用的存储量，数据库包含的数据量及对象，collection和索引计数器。

![dbStats](/images/MongoDB/dbstats.png)

- `db.collection.stats()`
	`db.collection.stats()`，提供了collection上的统计信息。包含集合中的对象数量，结合大小，集合磁盘空间用量，索引信息。

- `rs.status()`
	`rs.status()`，返回一个复制集状态的概述。


<br>

#### 第三方工具

许多第三方(third party)工具支持对MongoDB的监控。

- Nagios
- Zabbix
- Ganglia
- Motop
- ...


<br>
<br/>


### Monitor MongoDB with SNMP on Linux

**SNMP is only available in MongoDB Enterprise**


<br>


### Monitor MongoDB Windows with SNMP





<br>

---

<br>


# MongoDB索引


Indexes

<br/>

索引支持在MongoDB中高效地(effecient)执行查询。没有索引，MongoDB就必须采取collection scan。扫描每个集合中的每个文档，用以匹配查询。如果查询存在适当的索引，则MongoDB可以使用该索引来限制它必须检查的文档数量。

索引是特殊的数据结构，将集合数据集中的一小部分以易于遍历(traverse)的形式存储。索引存储特定字段或字段集的值，按字段值排序。索引条目的排序支持高效的相等匹配和基于范围的查询操作。除此之外，MongoDB可以使用索引中的排序返回排序后的结果。


![栗子](/images/MongoDB/index_sort.png)

<br>

从根本上来说(fundamentally)，MongoDB中的索引类似于其他数据库的索引。MongoDB在collection级别定义索引，并支持集合的文档的任何字段或子字段上的索引。

<br>


**默认`_id`索引**
	在创建一个collection期间，MongoDB在`_id`字段上创建一个唯一的索引。你也可以自定义`_id`的值。你不能在`_id`字段上删除此索引。

**创建一个索引**
	`db.collection.createIndex`方法只有在同一规范不存在时才创建索引。

```
db.collection.createIndex(<key and index type>, option)
```

**索引类型**
MongoDB提供了许多不同的索引类型来支持特定类型的数据和查询。

- Single Field
	除了MongoDB定义的`_id`索引，MongoDB还支持在文档的单个字段上创建用户自定义的升序(ascending)/降序(descending)索引。
    对于单字段索引和排序操作，MongoDB可以在任何方向遍历索引。
![单字段索引](/images/MongoDB/SingleField.png)

- Compound(复合) Index
	MongoDB也支持多个字段的用户自定义索引。
![复合索引](/images/MongoDB/CompundIndex.png)

- Multikey Index
	MongoDB使用多键索引来索引存储在数组中的内容。
![多键索引](/images/MongoDB/MultikeyIndex.png)

- Geospatial(地理空间) Index
	为了支持对地理空间坐标数据的有效查询，MongoDB提供了两个特殊的索引：`2d index`返回平面几何的2D索引；`2dsphere index`返回球形几何结果。

- Text Index
	MongoDB提供了一个文本(text)类型索引，用以支持搜索集合中的字符串内容(string content)。

- Hashed(散列) Index
	为了支持基于散列的分片，MongoDB提供了散列索引类型，它索引字段值的散列值。但只支持相等的匹配，而不能支持基于范围的查询。


<br>


**Index Properties(特性)**

- Unique Index
	索引的唯一性是MongoDB拒绝索引字段的重复值。

- Partial Index
	部分索引仅索引复合指定过滤器表达式的集合中的文档。

- Sparse(稀疏) Index
	索引的稀疏属性确保索引仅包含具有索引字段的文档的条目，跳过没有索引字段的文档。

- TTL Index
	TTL索引是MongoDB可以用来在一定时间后自动从集合中删除文档的特殊索引。对于某些类型的消息，如机器生成的事件数据，日志和会话信息等，只需在数据库库中保存有限的时间，这是非常理想的。

<br>

**Index Use**
索引能够提高读操作的效率。

<br/>

**Index and Collation**
要使用索引进行字符串比较，操作还必须指定相同的排序规则。

<br>
**Coverd Query**
当查询条件和查询投影仅包含索引字段时，MongoDB将直接从索引返回结果，而不扫描任何文档或将文档带入内存。




<br>

## Single Filed Index



<br>
<br/>

## Compound Index



<br>
<br/>

## Multikey Index



<br>
<br/>

## Text Index



<br>

## 2dsphere Index



<br>
<br/>

## 2d Index



<br>
<br/>

## geoHaystack Index



<br>
<br/>



## Hashed Index




<br>
<br/>

## Index Property




<br>
<br/>

## Index Build Operation on a Populated Collection



<br>
<br/>

## Index Intersection



<br>
<br/>

## Manage Index



<br>
<br/>

## Measure Index Use



<br>
<br/>

## Indexing Strategy



<br>
<br/>

## Index Reference





<br>

---

<br/>


# MongoDB存储


Storage

<br/>

FAQ: MongoDB Storage: <https://docs.mongodb.com/v3.4/faq/storage/>

<br>

存储引擎(storage engine)是MongoDB管理数据库主要的组件。

journal日志，用于数据库不正常关闭时修复数据库。有几种可选的配置项，用以平衡数据库的性能和可用性。

**GridFS**是一个适合处理大文件的多功能的存储系统，例如那些超过16MB文档大小限制的文件。



<br/>
<br>

## Storage Engine

存储引擎是数据库的组件，负责管理数据库在内存(in-memory)和磁盘中(on-disk)两种存储方式。
由于不同的存储引擎在特定的工作负载下有更好的性能，所以，为你的应用程序选择一个适当的存储引擎会提高性能。

<br>

**[WiredTiger](#WiredTiger存储引擎)**是从MongoDB3.2开始的默认存储引擎。它非常适合大多数工作负载，并推荐使用它来进行部署。WiredTiger提供了文档级并发模型，检查点和要说等特性。

**[MMAPv1](#MMAPv1存储引擎)**是一个原始的MongoDB存储引擎，它是MongoDB3.2以前的默认存储引擎。它在大量读取和写入以及更新方面的工作负载表现良好。

**[In-Memory](#In-Memory存储引擎)**要在MongoDB Enterprise中才能获取。它不是将文档保存在磁盘上，而是将它们保留在内存中，以获得可预测的数据延迟。



<br/>
<br>

### WiredTiger存储引擎

MongoDB3.2以后使用WiredTiger存储引擎作为默认存储引擎。

```
mongod --storageEngine wiredTiger


#或
vim /etc/mongod.conf

storage:
  engine:  wriedTiger
```


<br>

#### 文档级别并发(currency)

WiredTiger使用文档级并发来控制写操作。因此，多个客户端可以同时修改一个集合中的不同文档。

对于大多数读写操作，WiredTiger使用乐观的并发控制。WiredTiger仅在global、database和collection-levels使用intent lock。
当存储引擎检测到两个操作之间的冲突时，其中一个操作将引发写冲突，从而导致MongoDB透明地重试该操作。


<br>

#### 快照和检查点

WiredTiger users multiVersion Concurrency Control(MVCC).在操作开始时，WiredTiger向事务提供数据的实时快照。快照显示内存中数据的一致性视图。

当写入磁盘时，WiredTiger将快照中的所有数据以一致性的方式跨越所有数据文件写入磁盘。持久(durable)的数据充当数据文件中的检查点。检查点确保数据文件与最后一个检查点保持一致性，并包括最后一个检查点。

MongoDB配置WiredTiger来创建检查点(即将快照数据写入磁盘)，间隔时间为60s，或2G日志数据。

在写入新检查点期间，前一个检查点仍然有效。

当WiredTiger的元数据表被原子地更新以引用新的检查点，新的检查点将变得可访问和永久。一旦新检查点可以访问，WiredTiger就会从旧的检查点这种释放页面(free page)。

<br>

#### Journal

WiredTiger采用预写事务日志联合检查点，用以确保数据的持久性(durability)。
你也可以关闭journal功能来减少维护日志的开销。

WiredTiger日志坚持在检查点之间修改所有数据。如果MongoDB在检查点之间退出，它将使用日志重放自上一个检查点以来修改的所有数据。

WiredTiger journal使用snappy compression Library来进行压缩。

**WiredTiger最小日志记录的大小是128Byte，如果日志记录小于等于128Byte，则WiredTiger不会压缩日志文件。**

对于以standalone模式运行的mongo实例，关闭journal日志功能意味着当MongoDB意外地在检查点之前退出时，你将丢失一些数据修改。对于复制集成员，复制过程和恒提供足够的持久性保证。

<br>

#### Compression

使用WiredTiger，MongoDB支持压缩所有的collections和indexes。通过使用CPU进行压缩，减少了储存空间的使用。

默认地，WiredTiger使用snappy compression library对所有的collections进行block压缩，对所有索引进行前缀(prefix)压缩。

对于collection，也可以使用zlib进行block压缩。可通过`storage.wiredTiger.collectionConfig.blockCompressor`设置压缩方法。
对于index，使用`storage.wiredTiger.indexConfig.prefixCompression`关闭prefix压缩。

对于大多数工作负载，默认压缩设置平衡了存储效率和处理要求。

<br>

#### Memory Use

对于WiredTiger，MongodB使用WiredTiger内部缓存和文件缓存。

从MongoDB3.4开始，WiredTiger内部缓存将使用一下两种类型中更大的一种：

- 50% of RAM minus 1GB
- 256MB

<br>

WiredTiger内部缓存中的数据与磁盘上格式的数据使用不同的表现形式：

- 文件系统缓存的数据与磁盘上的格式相同，包括了对数据文件进行压缩的好处。操作系统使用文件系统缓存来减少磁盘I/O
- 指标加载在WiredTiger内部缓存有一个不同的磁盘上的数据表示格式，但仍然可利用 prefix index compression来减少内存使用。索引前缀压缩重复数据删除常用前缀的索引字段。
- WiredTiger内存缓存的collection数据是未压缩的，并使用与磁盘格式不同的表现形式。block compression能够节省大量磁盘空间，但必须解压缩数据后服务器才能操作。

**通过文件系统缓存，MongoDB自动使用 (WiredTiger缓存或其他进程不使用)空闲内存。**

调整WiredTiger内部缓存的大小，避免将WiredTiger的内初缓存值增加到默认值之上。

```
#命令行
--wiredTigerCacheSizeGB



#配置文件
storage.wiredTiger.engineConfig.cacheSizeGB
```

<br>

#### Change Standalone to wiredTiger

MongoDB version 3.0 or later in order to use wiredTiger storage engine!

过程：

1. mongod is running
2. export data using mongodump
3. create a data directory for the new mongod running with wiredTiger
4. start mongod with wiredTiger
5. upload the dumpdata using mongorestore


<br>

#### Change Replica Set to wiredTiger

Replica sets can have members with different storage engines.
因此，你可以把所有成员的存储引擎更换为WiredTiger。
MongoDB version 3.0 or later in order to use wiredTiger storage engine!

过程：

1. shutdown the secondary member.--`db.shutdownServer`
2. prepare a data directory for the new mongod running with wiredTiger
3. start mongod with wiredTiger
4. repeat the procedure for other replica set secodaries you wish to upgrade


<br>

#### Change Sharded Cluster to wiredTiger

if the shard is a standalone, see [Change Standalone to wiredTiger](#Change Standalone to wiredTiger);
if the shard is a replica set, see [Change Replica Set to wiredTiger](#Change Replica Set to wiredTiger).

<br/>

##### Change config server to wriedTiger

如果你打算更新config server使用WiredTiger，那么必须全部更新！

过程：

1. disable the balancer--`sh.disableBalancer()`
2. shutdown the third config server to ensure read-only metadata.--`db.shutdownServer()`
3. export the data of the second config server with mongodump
4. For the second config server, create a new data directory for use with WiredTiger.
5. Stop the second config server.--`db.shutdownServer()`
6. Start the second config server mongod with the WiredTiger storage engine option.
7. Upload the exported data using mongorestore to the second config server.
8. Shut down the second config server to ensure read-only metadata.--`db.shutdownServer()`
9. Restart the third config server to prepare for its upgrade.
10. Export the data of the third config server with mongodump
11. For the third config server, create a new data directory for use with WiredTiger.
12. Stop the third config server.
13. Start the third config server with the WiredTiger storage engine option.
14. Upload the exported data using mongorestore to the third config server.
15. Export data of the first config server with mongodump.
16. For the first config server, create a new data directory for use with WiredTiger.
17. Stop the first config server.
18. Start the first config server with the WiredTiger storage engine option.
19. Upload the exported data using mongorestore to the first config server.
20. Restart the second config server to enable writes to the sharded cluster’s metadata
21. Re-enable the balancer.--`sh.startBalancer()`



<br>
<br/>

### MMAPv1存储引擎




<br>
<br/>

### In-Memory存储引擎




<br>
<br/>


## Journaling

**为了在发生故障时提供持久性，MongoDB使用了县写日志记录到磁盘的日志文件。**
To provide durability in the event of a failure, MongoDB uses write ahead logging to on-disk journal files.


<br>

### journaling and the wiredTiger storage engine

本节所指的log指的是WiredTiger的 write-ahead log(journal)，而不是MongoDB日志文件。


WiredTiger使用checkpoints在磁盘上提供一致的数据视图，并允许MongoDB从上一个checkpoint修复。然而，如果MongoDB在检查点之间以外退出，则需要使用journaling来修复上次检查点之后发生的信息。

使用journaling的修复过程：

1. 在数据文件中查找上一个检查点的标识符(identifier)
2. 在journaling文件中搜索与上一个检查点标识符匹配的记录
3. 应用自上一个检查节点依赖journal文件中的操作

<br/>

#### journal process

通过journaling，WiredTiger为每个客户端启动的写操作创建一个journal记录。journal record包括有初始写入引起的任何内部写入操作。

例如，集合中文档的更新可能导致对index的修改，WiredTiger创建一个包含update操作及其相关index修改的单独的journal record。

MongoDB将WiredTiger配置为in-memory的buffering来存储日志记录。线程坐标来分配和复制到他们的缓冲区的一部分。所有日志记录高达128KB是缓存的。
WiredTiger根据如下条件将journal record同步到磁盘。

- 每50ms
- MongoDB在WiredTiger中设置60s为间隔的用户数据检查点或2GBjournal数据已被写入，以先发生为准。
- 如果写操作包含有`j:true`的写关注点，则WiredTiger强制对journal文件进行同步。
- MongoDB限制了journal文件大小为100MB，因此WiredTiger每100MB就会创建一个新的journal文件。当创建了一个新的journal文件时，WiredTiger会同步上一个journal文件。


**在写操作之间，虽然日志记录保留在WiredTiger缓冲区中，但在mongod实例hard shutdown之后可能会丢失更新。**

![Journal大小](/images/Zabbix/journalSize.png)

<br>

#### Journal File

MongoDB在数据库目录下创建一个`journal`子目录存放journal文件。名字为`WiredTigerLog.<sequence>`，从`0000000001`开始。如上图所示。

Journal文件包含对每一个写操作的记录。每个记录都有唯一的标识符。

MongoDB将WiredTiger配置为对journal数据使用快速压缩。最小日志大小为128KB，如果小于此，WiredTiger不会压缩此记录。最大大小为100MB，超过此，WiredTiger会创建一个新的journal文件。

MongoDB自动删除旧日志文件，以维护从上一个检查点恢复所需的文件。


<br>
<br/>

### Journaling and the MMAPv1 Storage Engine



<br>
<br/>

### Journaling and the In-Memory Storage Engine



<br/>
<br/>


## Manage Journaling

MongoDB uses write ahead logging to an on-disk journal to guarantee write operation durability.

启用journal后，如果MongodB意外退出，则程序可以恢复写入了journal日志文件的所有内容。MongoDB将在重启时重新应用写操作，并保持一致性。


### 过程

<br>

#### Enable journaling

```
mongod --jouranl


##或
vim /etc/mongod.conf

storage:
  journal:
    enabled: true
```

<br>

#### Disable journaling

```sh
mongod --noJournal


###或
修改配置文件
```

> **警告**
> 不要在生产系统上禁用日记功能。
> 如果在一个副本集上使用`--noJournal`关闭了journal日志，则还应该修改副本集配置文件。

<br>

#### Monitor journal status

- `serverStatus`

<br>

#### Recover data after unexpected shutdown

在奔溃后重启时，MongoDB会在服务器可用之前replay journal日志记录中的所有日志文件。



<br>
<br/>


## GridFS


GridFS是一种用于存储和检索超过BSON文档大小限制值16MB的文件规范。

GridFS没有将单个文件存储到单个的文档中，而是将文件分割成部分(parts)或块(chunks)，并将每个块存储到单独的文档中。默认情况下，GridFS的块大小为255KB。也就是说，GridFS将文件分成255KB的块，最后一块大小就不确定了。

GridFS使用两个集合来存储文件。一个存储文件块(chunks)，另一个存储文件元数据(metadata)。

当你查询(query)GridFS文件时，驱动程序会根据需要重新组装这些块。你可对通过GridFS存储的文件执行范围查询。还可以从任意文件部分访问信息。

GridFS不仅可用于存储超过16MB的文件，还可用于存储需要访问的任何文件，而不必将整个文件加载到内存中。


<br>

### 何时使用GridFS

在MongoDB中，使用GridFS存储大于16MB的文件。

某些情况下，在MongoDB数据库中存储大文件可能比在系统级文件系统上更有效。

- 如果文件系统限制了一个目录中的文件数量，则可使用GridFS存储所需的文件
- 当你想要访问大文件的部分信息时而不想将整个文件加载到内存中时，可使用GridFS收回文件的各个部分，而不必将整个文件读入内存
- 当你希望文件和元数据自动同步并部署在多个系统和设施中时，可使用GridFS

如果需要原子地(atomically)更新整个文件的内容，请不要使用GridFS。作为一种选择，你可以为每个文件存储多个版本，并在元数据中指定该文件的当前版本。

此外，如果文件都是小于16MB的BSON文件大小限制，则考虑手动存储在一个单文档中，而不必使用GridFS。


<br>
<br/>

### 使用GridFS

使用GridFS存储和检索文件，请使用如下任何一项：

- A MongoDB Driver
- The mongofile cmd-line tool


<br>
<br/>


### GridFS集合

GridFS把文件存储在两个集合里：

1. chunks collection
	- stores the binary chunks
2. files collection
	- stores the file's metadata

GridFS将这些集合放在一个普通的存储区(bucket)中，每个存储区前面加上名称。默认地，GridFS使用两个名为`fs`的存储区集合：

1. fs.files
2. fs.chunks

币可以选择一个不同的存储区名字，也可以在一个数据库中创建多个存储区。

<br>

#### The chunks collection

块集合中的每个文档都表示一个独立的文件块。格式如下：

```
{
	"_id": <ObjectId>,
    "files_id": <ObjectId>,
    "n": <num>,
    "data": <binary>
}
```

块集合中的文档包含如下字段：

- chunks._id
	+ The unique ObjectId of the chunk
- chunks.files_id
	+ The _id of the “parent” document
- chunks.n
	+ The sequence number of the chunk，GridFS从0开始标号所有块
- chunks.data
	+ BSON Binary type

<br/>

#### file集合

GridFS的file集合，格式如下：

```
{
	"_id": <ObjectId>,
    "length": <num>,
    "chunkSize": <num>,
    "uploadData": <timestamp>,
    "md5": <hash>,
    "filename": <string>,
    "contentType": <string>,
    "aliases": <string array>,
    "metadata": <any>
}
```

- files._id
	+ The unique identifier for this document
- files.length
	+ The size of the document in bytes
- files.chunSize
	+ The size of each chunk in bytes
- files.uploadDate
	+ The date the document was first stored by GridFS
- files.md5
	+ An MD5 hash of the complete file
- file.filename
	+ Optional. A human-readable name for the GridFS file
- file.contentType
	+ Optional. A valid MIME type for the GridFS file
- files.aliases
	+ Optional. An array of alias strings
- files.metadata
	+ Optional. The metadata field may be of any data type and can hold any additional information you want to store


<br>

### GridFS索引

为了提高效率，GridFS在每个chunks and files collections上使用索引。

<br/>

#### chunks索引

GridFS使用一个唯一的、混合的索引。在chunks集合上使用`files_id`和`n`字段。

```
db.fs.chunks.find( { files_id: myFileID } ).sort( { n:1 })


#创建索引
db.fs.chunks.createIndex({ files_id: 1, n:1 }, { unique: true });
```

<br>

#### files索引

GridFS使用索引，在files集合上使用`filename`和`uploadDate`字段。

```
db.fs.files.find({ filename: myFileName }).sort({ uploadDate: 1 })


#创建索引
db.fs.files.createIndex({ filename:1, uploadDate: 1 });
```

<br/>

### 分片GridFS

如果需要分片GridFS数据存储，使用chunks集合设置: `{ files_id: 1, n:1}` or `{ files_id: 1 }`作为分片key索引。

不能对chunks集合使用hash分片。

files_id是一个ObjectId。



<br>
<br/>

---

<br>



# MongoDB安全



Security

<br/>

MongoDB提供了各种特性(features)，如身份认证(authentication)、访问控制(access control)、加密(encryption)，以保护MongoDB部署。


<br/>
<br>


## Security Checklist


- 启用访问控制和强制认证
	+ Enable Access Control and Enforce Authentication
	+ 可使用默认的MongoDB认证机制或现有的外部框架
- 配置基于角色的访问控制
	+ Configure Role-Based Access Control
	+ 首先创建administrator，接着在创建其他用户
	+ 创建角色，定义一组用户所需的确切访问权限
- 加密通信
	+ Encrypt Communication
	+ 配置MongoDB使用TLS/SSL加密连接
- 加密和保护数据
	+ Encrypt and Protect Data
- 限制网络曝光
	+ Limit Network Exposure
	+ 确保MongoDB运行在一个受信任的网络环境上，并限制MongoDB的监听接口
- 审计系统活动
	+ Audit System Activity
	+ 跟踪对数据库配置和数据的访问和更改
- 使用专用用户运行MongoDB
	+ Run MongoDB with a Dedicated User
	+ 使用专用的操作系统用户账户运行MongoDB进程
- 使用安全配置选项运行MongoDB
	+ Run MongoDB with Secure Configuration Options
	+ MongoDB为了支持某些服务端操作执行：`mapReduce`,`group`,`$where`
	+ 如果你不使用这些操作，请关闭服务器端脚本执行`--noscripting`
- 请求一个安全技术执行指南
	+ Request a Security Technical Implementation Guide
- 考虑安全标准合格性
	+ Consider Security Standards Compliance



<br>
<br>


## 认证


Authentication

<br>

要作为用户进行身份认证，必须提供用户名(username)，密码(password)和与用户关联的身份验证数据库(authentication database)。

```
mongo --host --username --password --authenticationDatabase


#Or
mongo
>use <authenticationDatabase>
>db.auth('username','password')
```

<br/>

- 认证机制
	+ Authentication Mechanisms
	+ MongoDB支持多种认证机制
		* SCRAM-SHA-1
		* MongoDB Challenge and Response (MONGODB-CR)
		* x.509 Certificate Authentication
		* LDAP proxy authentication(MongoDB Enterprise)
		* Kerberos authentication(MongoDB Enterprise)
- 内部认证
	+ Internal Authentication
	+ 除了验证客户端的身份外。MongoDB还可以要求副本集和分片集的成员对其各自的成员进行认证


<br/>

### 用户

Users

<br/>

要在MongoDB中验证客户端，必须向MongoDB添加相应的用户。

- 用户管理接口
	+ User Management Interface
	+ 使用`db.createUser()`方法创建用户
	+ 添加用户时，可为用户分配角色以授予权限
	+ 在数据库管理中创建的第一个用户应该是具有管理其他用户权限的administrator
	+ 也可以更新/删除一个已经存在的用户的权限
- 认证数据库
	+ Authentication Database
	+ 在特定的数据库中创建用户，这个数据库是用户的认证库
	+ 用户名和认证库充当该用户的唯一标识符。如果两个用户具有相同的用户名，但是在不同的数据库中创建，则它们是两个单独的用户
	+ 用户可拥有不同数据库的权限，而不限于认证库
	+ 通过数据库角色给用户分配相应的权限
- 认证一个用户
	+ Authentication Database
	+ 使用用户名、密码、认证库验证一个用户
- 集中的用户数据
	+ Centralized User Data
	+ MongoDB将所有的用户名、密码和认证库信息，保存到admin库的syste.users集合中
	+ 使用用户管理命令而不要直接访问这个集合
- 分片集群用户
	+ Sharded Cluster Users


<br/>

#### 添加用户

Add Users

<br>

MongoDB使用基于角色的访问控制(RBAC)来确定用户的访问权限。用户被授予一个或多个角色，这些角色确定用户对MongoDB资源的访问或权限，以及用户可以执行的操作。
用户应该只具有确保系统最小权限所需要的最小权限。

<br/>

**前提(Prerequisites)**

对于用户创建，你必须拥有以下权限

- 在数据库中创建一个新用户，必须在数据库资源上有`createUser`操作
- 对一个用户授权角色，必须在角色数据库中有`grantRole`操作

<br/>

**栗子**

```
use admin
db.createUser(
	{
		user: 'zhang',
        pwd: 'passwd123',
        roles: [
        	{ role: 'root' },
            { db: 'admin' }
        ]
	}
)


#在配置文件中开启用户认证
vim /etc/mongod.conf

security:
    authorization: enabled
```


<br>
<br>


### 认证机制

Authentication Mechanisms

<br/>

#### SCRAM-SHA-1


<br>

#### MONGODB-CR


<br>

#### x.509

MongoDB对于客户端身份认证和副本集、分片集成员的内部认证支持x.509证书认证。

x.509证书认证需要安全的TLS/SSL连接。

<br/>

**证书授权(Certificate Authority)**

在生产使用中，MongoDB的部署应该使用由认证机构签名和生成的有效证书。

<br/>

**Client x.509 Certificates**

要想服务器验证身份，客户端可以使用x.509证书而不是用户名和密码。

Client Certificate Requirements：

- 单个证书颁发机构(CA)必须同时为客户端和服务器颁发证书
- 客户端证书必须包含如下字段：
```
keyUsage = digitalSignature
extendedKeyUsage = clientAuth
```
- 每个唯一的MongoDB用户必须有一个唯一的证书
- 一个客户端x.509证书的主题，包含了可辨识名称(DN)。必须不同于成员x.509证书

<br/>

MongoDB user and $external database

若要使用客户端证书进行认证，必须先将客户端证书中的subject值添加为MongoDB用户。每个唯一的x.509客户端证书对因孤独一个MongoDB用户。

在$external database中添加用户，认证库便是外部数据库。

<br/>

Authenticate

使用x.509客户端进行身份验证，请通过TLS/SSL连接到MongoDB。`--ssl` and `--sslPEMKeyFile`

<br/>

**Member x.509 Certificates**

对于内部认证，分片集和副本集的成员可以使用x.509证书来代替使用SCRAM-SHA-1认证机制的keyfile。

**Member Certificate Requirements**

- CA必须为所有分片集，副本集成员颁发x.509证书
- 成员证书的主题中找到Distinguished Name(DN)必须为以下至少一个属性指定非空值：Organization(O)，Organization Unit(OU)，Domain Component(DC)
- 组织属性，组织单元属性和域组件必须与其他集群成员的证书相匹配。
```
CN=host1,OU=Dept1,O=MongoDB,ST=NY,C=US
C=US, ST=CA, O=MongoDB, OU=Dept1, CN=host2
```

<br/>

**MongoDB Configuration**

- 配置文件：security.clusterAuthMode and net.ssl.clusterFile
- cmd-line options: --clusterAuthMode and --sslClusterFile

<br/>

**Member Certificate and PEMKeyFile**

- 配置文件： net.ssl.PEMKeyFile
- cmd-line option: --sslPEMKeyFile



<br>
<br>

### Enterprise Authentication Mechanisms



<br/>
<br/>

### MongoDB认证和角色

要想了解MongoDB的权限必须先了解如下一些关键字：

- user
	+ 用户，用于提供客户端连接MongoDB的认证账户
- role
	+ 角色，数据权限的集合，创建用户的时候必须要指定对应的角色，否则用户无法操作数据库
- resource
	+ 资源，包括database或collection 也可以是database和collection的组合
- actions
	+ 权限操作，定义了 user 能够对 resource document 执行的操作。如 增、删、改、查
- privilege
	+ 权限，privilege 是一组 resource 和 action的组合，对资源拥有什么操作称为权限
- authenticationDatabase
	+ 认证库，即创建角色或用户时所在的库

<br/>

#### 角色管理

MondoDB支持基于角色的访问控制（RBAC）来管理对MongoDB系统的访问。一个用户可以被授权一个或多个角色以决定该用户对数据库资源和操作的访问权限。在权限以外，用户是无法访问系统的。

数据库角色在创建用户的role参数中设置。角色分为內建角色和自定义角色。

<br/>

**内建角色**

- 数据库用户角色
	+ read：允许用户读取指定数据库
	+ readWrite：允许用户读写指定数据库
- 数据库管理员角色
	+ dbAdmin：允许用户进行索引创建、删除，查看统计或访问system.profile，但没有角色和用户管理的权限
	+ userAdmin：提供了在当前数据库中创建和修改角色和用户的能力
	+ dbOwner：提供对数据库执行任何操作的能力。这个角色组合了readWrite、dbAdmin和userAdmin角色授权的特权
- 集群管理角色
	+ hostManager：提供监视和管理服务器的能力
	+ clusterManager：在集群上提供管理和监视操作。可以访问配置和本地数据库，这些数据库分别用于分片和复制
	+ clusterMonitor：提供对监控工具的只读访问
	+ clusterAdmin：提供最强大的集群管理访问(副本集、分片、主从等)。组合了clusterManager、clusterMonitor和hostManager角色的能力，还提供了dropDatabase操作
- 备份恢复角色
	+ backup：提供备份数据所需的能力
	+ restore： 提供使用mongorestore恢复数据的能力
- 所有数据库角色
	+ readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
	+ readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
	+ userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
	+ dbAdminAnyDataBase：只在admin数据库中可用，赋予用户所有数据库的adAdmin权限
- 超级用户角色
	+ root：超级权限，只能针对admin库
-  内部角色
	+ __system：提供对数据库中任何对象的任何操作的特权

<br/>

**自定义角色**

MongoDB内置角色一般来说都是够用的，但当内置角色不满足需求时就可以自定义角色了。使用 db.createRole() 方法来自定义角色。

只能在admin库中创建角色：

```
use admin
db.createRole(
    {
        role:<role_name>,	#定义角色名称
        privilege:[    #权限集
            { resource:{cluster:true, actions:[<action_name>] },
            { resource: {db:<db_name>, collection:<coll_name> },
        	{ actions:[<action_name>] }    #定义对这个库或集合可进行的权限操作，这是一个数组
        ],
        roles:[ { role:<role_name>, db:<db_name> } ]    #是否继承其他的角色
    }
)
```

角色创建完毕后MongoDB会在系统库admin下创建一个collection名叫 system.roles，里面存储的即是角色相关的信息。

```
db.system.roles.find()
```

<br/>

#### 操作角色

```
#查看角色
db.getRole()


#角色继承

#角色授权
db.grantRolesToRole()

#角色移权
db.revokeRolesfromRole()
```

<br/>

#### 用户管理

**创建用户**：

```
db.createUser({
    user:"xxx", pwd:"xxxx", customDate:"xxx",
    roles:[{    #指定角色名称以及认证库
        role:"xxx", db:"xxxx"
    }]
})
```

<br/>

**开启认证**：

```
vim /etc/mongo.conf

security:
    authorization：enabled



db.auth("user","passwd")    #在use db后
或
mongo -u user -p passwd --authenticationDatabase xxx
#在哪个库创建的用户就需要使用哪个库进行认证
```

<br/>

**查看用户**：

```
db.getUser("user")
db.system.users.find()
```

<br/>

**删除用户**：

```
db.dropUser("user")
db.dropAllUsers()
```

<br>

**添加用权限**：

```
db.grantRolesToUser()
```

<br/>

**修改用户密码**：

```
db.changeUserPassword("user","new_passwd")
```

<br/>

在MongoDB中删除库和集合并不会级联删除对应的角色和用户。因此如果想彻底删除对应的业务应该先删除库与其对应的角色和用户。

如果既想实现精细化权限控制又想简化用户管理，原则上建议只给开发创建一个账户，并且使用admin做认证库，这样可以避免清理过期业务库而导致无法登陆的问题。




<br>
<br>


### 内部认证


Internal Authentication

<br>

可以对副本集和分片集成员进行验证。
对于成员的内部认证，MongoDB可以使用keyfile或x.509证书。

<br>

**KeyFile**

keyfiles的内容作为成员的共享密码，其长度必须在6-1024个字符之间，只能包含base64 set中的字符。

```
openssl rand -base64 512 > /etc/mongodb.keyfile
chmod 600 /etc/mongodb.keyfile
chown mongod:mongod /etc/mongodb.keyfile


#配置文件：security.keyFile
#cmd-line option: --keyFile

vim /etc/mongod.conf

security:
  authorization: enabled
  keyFile: "/etc/mongodb.keyfile"
  clusterAuthMode: "keyFile"
```

<br/>

**x.509**

内部认证使用x.509进行验证。

- CA必须为所有分片集，副本集成员颁发x.509证书
- 成员证书的主题中找到Distinguished Name(DN)必须为以下至少一个属性指定非空值：Organization(O)，Organization Unit(OU)，Domain Component(DC)
- 组织属性，组织单元属性和域组件必须与其他集群成员的证书相匹配。
```
CN=host1,OU=Dept1,O=MongoDB,ST=NY,C=US
C=US, ST=CA, O=MongoDB, OU=Dept1, CN=host2
```

<br/>

**MongoDB Configuration**

- 配置文件：security.clusterAuthMode and net.ssl.clusterFile
- cmd-line options: --clusterAuthMode and --sslClusterFile


<br>

#### 在副本集中强制秘钥文件访问控制

Enforce Keyfile Access Control in a Replica Set

<br/>

对副本集执行访问控制需要配置：

- 使用内部身份验证副本集成员之间的安全性
- 使用用户访问控制连接客户端和副本集间的安全性

步骤：

1. 创建一个密钥文件

Create a keyfile

通过密钥文件进行身份验证，副本集中的每个mongod实例都使用密钥文件的内容作为共享密码，用于验证部署中的其它成员。

```
#yum install -y openssl

openssl rand -base64 756 > <path-to-keyfile>
chmod 400 <path-to-keyfile>
chown <owner>:<owner>
```

<br>

2. 复制密钥文件到每个副本集成员

Copy the keyfile to each replica set member

将密钥文件复制到每一台主机的副本集成员中。
确保运行mongod实例的用户就是keyfile的所有者，并可以访问密钥文件。

<br/>

3. 关闭所有的副本集成员

Shut down all members of the replica set

关闭每个副本集中的mongod，从Secondary开始。知道所有的成员都脱机为止，包括任何仲裁者(Arbiter)。Primary是最后一个关闭的成员。

```
use admin
db.shutdownServer()
```

<br/>

4. 启动访问控制并重启副本集成员

```
vim /etc/mongod.conf

security:
  keyFile: <path-to-keyfile>
  clusterAuthMode: keyfile
replication:
  replSetName: <replcaSetName>



#cmd-line
mongod --keyFile <path-to-keyfile> --clusterAuthMode keyfile --replSet <replicaSetName>

```

<br/>

5. 连接到mongo shell

在Primary上使用`rs.status()`来标识副本集成员。

<br/>

6. 创建一个administrator

Create the user administrator

必须在Primary上创建用户。

```
admin = db.getSiblingDB("admin")
admin.createUser(
  {
	user: 'zhang',
    pwd: 'password',
    roles: [{ role: 'userAdminAnyDatabase', db: 'admin' }]
  }
)
```

<br/>

7. 开启用户认证

```
vim /etc/mongod.conf

security:
  authorization: enabled
  keyFile: <path-to-keyfile>
  clusterAuthMode: keyfile
replication:
  replSetName: <replcaSetName>
```


<br/>

8. 以管理员身份进行认证

Authenticate as the User Administrator

```
mogno
>db.getSiblingDB("admin").auth('zhang','password')


#or
mongo -u 'zhang' -p 'password' --authenticationDatabase 'admin'
```

<br/>

9. 创建集群管理员(可选)

Create the cluster administrator (Optional)

```
db.getSiblingDB("admin").createUser(
  {
    "user" : "ravi",
    "pwd" : "changeme2",
    roles: [ { "role" : "clusterAdmin", "db" : "admin" } ]
  }
)
```


<br>

#### 在不停机的副本集中强制实施keyfile访问控制





























































