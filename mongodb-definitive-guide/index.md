# MongoDB权威指南


参考:
- [MongoDB权威指南](https://book.douban.com/subject/35688800/)

<br/>

环境:

- centos7
- mongodb4.2+


<br/>
<br/>

<!--more-->

<br/>
<br/>




# 简介

MongoDB中没有预定义模式，文档键值的类型和大小不是固定的，因此按需添加或删除字段变得很容易。

MongoDB的设计采用了横向扩展。面向文档的数据模型使跨多台服务器拆分数据更加容易。MongoDB会自动平衡跨集群的数据和负载，自动重新分配文档，并将读写操作路由到正确的机器上。

MongoDB包含了索引、聚合、特殊的集合和索引类型、文件存储等功能。

MongoDB在其WiredTiger存储引擎中使用了机会锁，以最大限度地提高并发和吞吐量。它会使用尽可能多的RAM作为缓存，并尝试为查询自动选择正确的索引。




<br/>

---

<br/>




# 入门指南

一些基本概念:

- 文档是基本数据单元，可理解为行。
- 集合，可理解为表。
- 数据库
- 每个文档都有一个特殊的键`_id`，其在所属的集合中是唯一的。
- mongo shell是一个功能强大的javascript解释器。


<br/>
<br/>


## 文档

文档是一组有序键值的集合，不能包含重复的键。


<br/>
<br/>


## 集合

集合就是一组文档。集合具有**动态模式**的特性，这意味着一个集合中的文档可以具有任意数量的不同形状。但创建模式并将相关类型的文档放在一起非常合理。通过将单一类型的文档放入集合，可以更高效地对集合进行索引。

使用子集合来组织数据在很多场景中是一个好方法。


<br/>
<br/>


## 库

数据库对集合进行分组。库名称长度限制为64字节。一个推荐的做法是将单个应用程序的所有数据都存储在一个库里。

将库和集合名称连接起来，可以获得一个完全限定的集合名称，称位**命名空间**。命名空间长度限制为120字节，实际应该小于100字节。

一些库名是保留的，有特殊含义：

- `admin`: 身份认证和授权。
- `local`: 特定于单个服务器的数据会存储在此数据库中。在副本集中，用于存储复制过程中所使用的数据，而local库不会被复制。
- `config`: 分片集群使用它来存储分片的信息。


<br/>
<br/>


## 数据类型

MongoDB中文档可以被认为是类似于JSON的形式。

常见的类型有以下几种:

- null: 空值或不存在的值
- 布尔
- 数值: 默认使用64为浮点数
- 字符串: 任何utf8字符串
- 日期: 将日期存储为64为整数，表示自Unix纪元以来的毫秒数。
- 正则表达式: 与js的正则语法相同
- 数组
- 内嵌文档
- Object ID: 12字节的ID，是文档的唯一标识。`_id`默认为此类型。
- 二进制数据: 如果要将非utf8字符串存入数据库，使用二进制数据是唯一的办法。
- 代码: 任意js代码


<br/>
<br/>


## mongo shell

因为mongo shell就是一个js shell，所以可通过js的在线文档获得大量帮助。可以输入`help`命令进行访问。

可以使用mongo shell执行js脚本。

<br/>

mongo shell辅助函数对应的js函数:

| 辅助函数 | 等价函数 |
| - | - |
| use db1 | db.getSisterDB("db1") |
| show dbs | db.getMongo().getDBs() |
| show collections | db.getCollectionNames() |

<br/>

如果你有一些需要频繁被加载的脚本，那么可将它们添加到`.mongorc.js`文件中。此文件会在启动mongo shell时自动执行。

最常见的用途是移除哪些比较问现的mongo shell命令，调用下面这些命令会打印出错误消息。

```javascript
var no = function() {
	print("Not on my watch.")
}

// 禁止删除数据库
db.dropDatabase = DB.prototype.dropDatabase = no;

// 禁止删除集合
DBCollection.prototype.drop = no;

// 禁止删除单个索引
DBCollection.prototype.dropIndex = no;
// 禁止删除多个索引
DBCollection.prototype.dropIndexes = no;
```




<br/>

---

<br/>




# 增删查改

数据库的CRUD操作。

MongoDB会对插入的数据进行最基本的检查，检查文档的基本结构，如果不存在`_id`字段，则自动添加一个。其中一项最基本的结构检查是文档大小，所有文档都必须小于16MB。可使用`Object.bsonsize(doc)`来查看文档的bson大小。

为了让你对16MB的数据量有一个概念，以`《战争与和平》`为例，整部著作也只有3.14MB。

不建议使用`insert()`，应该使用`insertOne()`或`insertMany()`。

不建议使用`remove()`，应该使用`deleteOne()`或`deleteMany()`。

使用`updateOnt()`或`updateMany()`。

`replaceOne()`会用新文档完全替换匹配的文档，这对于大规模模式迁移的场景非常有用。

`upsert()`是一种特殊类型的更新。如果找不到与筛选条件相匹配的文档，则会以这个条件和更新文档为基础来创建一个新文档；如果找到了匹配的文档，则进行正常的更新。

<br/>

通常文档只会有一部分需要更新，可以使用原子的更新运算符更新文档中的特定字段。应该始终使用修饰符`$`来增加、修改或删除键。

- `$inc`
- `$set`, `$unset`
- `$push`, `$pop`
- `$sort`
- `$slice`
- `$ne`
- ...




<br/>

---

<br/>




# 查询

涵盖以下方面:

- 使用`$`条件进行范围查询、数据集合查询、不等式查询和其它查询；
- 查询会返回一个数据库游标，其只会在需要时才惰性地进行批量返回；
- 有很多可以针对游标执行的元操作，包括跳过一定数量的结果、限定返回结果的数量，以及对结果进行排序。


<br/>


## find

传递给数据库的查询文档的值必须是常量。

```js
// 空查询条件会匹配所有内容
db.c.find()

// age 27的所有文档
db.users.find({"age": 27})

// 返回指定的键
db.users.find({"name": "joe", "age": 27}, {"username": 1, "email": 1, "_id": 0})
```


<br/>
<br/>


## 查询条件

更加复杂的条件。

```javascript
// $lt, $lte, $gt, $gte, $ne
db.users.find({"aget": {"$gte": 18, "$lte": 30}})

start = new Date("01/01/2007")
db.users.find({"registered": {"$lt": start}})

// $in, $nin, $or, $not, $mod
db.users.find({"user_id": {"$in": [123, 456]}})
db.users.find({"$or": [{"name": {"$in": ["a", "b"]}}, {"winner": true}]})
```


<br/>
<br/>


## 特定类型的查询



































