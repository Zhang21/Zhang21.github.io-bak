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

null的行为有一些特别。

```Javascript
// 仅匹配值为null的文档
db.c.find({"z": {"$eq": null, "$exists": true}})
```

<br/>

`$regex`可在查询中为字符串的模式匹配提供正则表达式功能。

```JavaScript
db.users.find({"name": {"$regex": /joey?/i}})
```

<br/>

查询数组元素的方式与查询标量值相同。

```Javascript
db.food.insertOne({"fruit": ["apple", "banana", "peach"])
// 查询其中一个内容也可以成功匹配到文档
db.food.find({"fruit": "banana"})

// 如果需要通过多个元素来匹配数组，使用 $all
db.food.find({"fruit": {"$all": ["apple", "banana"]}})

// 可以使用数组下标来查询
db.food.find({"fruit.2": "peach"})

// $size 查询特定长度的数组
db.food.find({"fruit": {"$size": 3}})

// $slice 返回数组中特定子集
db.blog.posts.findOne(criteria, {"comments": {"$slice": -1}})

// 返回查询条件匹配的任意数组元素
db.blog.posts.find({"comments.name": "Bob"}, {"comments.$": 1})
```

<br/>

要正确指定一组条件而无须指定每个键，请使用`$elemMatch`。


<br/>
<br/>


## $where查询

`$where`字句允许在查询中执行任意的JavaScript代码。为了安全起见，应该严格限制或消除`$where`的使用。


<br/>
<br/>


## 游标

数据库会使用游标返回`find`的执行结果。游标的客户端实现通常能够在很大程度上对查询的最终输出进行控制。

<br/>

最常用的查询选项是限制(limit)返回结果的数量、略过(skip)一定数量的结果以及排序(sort)。所有这些选项必须在查询被发送到数据库之前指定。

```Javascript
db.c.find().limit(3)

db.c.find().skip(3)

// 1升序， -1降序
db.c.find().sort({"username": 1, "age": -1})
```

<br/>

略过大量的结果会导致性能问题。因为需要找到被略过的结果，然后再丢弃这些数据。

<br/>

游标包括两个部分：面向客户端的游标，和由客户端游标所表示的数据库游标。

在服务器端，游标会占用内存和资源，一旦游标遍历完结果之后，或者客户端发送一条消息要求终止，数据库就可以释放它正在使用的资源。释放这些资源可以让数据库将其用于其他用途，这是非常有益的，因此要确保可以尽快释放游标。

如果10分钟没有被使用的话，数据库游标也将自动销毁。

有时可能需要一个游标维持很长时间。这种情况下，许多驱动程序实现了一个称为`immportal`的函数，或者类似的机制。它告诉数据库不要让游标超时。如果关闭了游标超时，则必须遍历完所有结果或主动将其销毁以确保游标被关闭。否则，它会一直占用数据库的资源，知道服务器重新启动。




<br/>

---

<br/>




# 索引

索引使你能够高效地执行查询。为集合选择正确的索引对性能至关重要。主要内容：

- 什么是索引？为什么要使用索引？
- 如何选择要创建索引的字段？
- 如何强制使用索引？如何对索引的使用进行评估？
- 创建及删除索引的管理细节。


<br/>
<br/>


## 索引简介

数据库索引类似于图书索引。有了索引便不需要浏览整本书，而是可以采取一种快捷方式，只查看一个有内容引用的有序列表。这使得MongoDB的查找速度提高了好几个数量级。

不适用索引的查询称为**集合扫描(全表扫描)**，这意味着服务器端必须浏览整本书才能得到查询的结果。

栗子，创建一个包含百万文档的集合:

```Javascript
for (i=0; i<1000000; 1++) {
	db.users.insertOne(
    	{
        	"i": i,
            "username": "user" + i,
            "age": Math.floor(Math.random()*120),
            "created": new Date()
        }
    );
}
```

通过不使用索引和使用索引，研究这个集合中查询的性能差异。

可以在查询中使用`explain`命令来产看MongoDB在执行查询时所需要做的事情。

```Javascript
db.users.find({"usename": "user101"}).explain("executionStats")

// 省略部分结果
{
    ...
    "executionStats" : {
            "executionSuccess" : true,
            "nReturned" : 1,
            "executionTimeMillis" : 419,
            "totalKeysExamined" : 0,
            "totalDocsExamined" : 1000000,
            "executionStages" : {
                    "stage" : "COLLSCAN",
                    "filter" : {
                            "username" : {
                                    "$eq" : "user101"
                            }
                    },
                    "nReturned" : 1,
                    "executionTimeMillisEstimate" : 375,
                    "works" : 100002,
                    "advanced" : 1,
                    "needTime" : 1000000,
                    "needYield" : 0,
                    "saveState" : 7822,
                    "restoreState" : 7822,
                    "isEOF" : 1,
                    "invalidates" : 0,
                    "direction" : "forward",
                    "docsExamined" : 1000000
            }
    },
}
```

介绍下`executionStats`字段下的内嵌文档。

`totalDocsExamined`是MongoDB在试图查询时查看的文档总数，可以看到，集合中的每个文档都被扫描过了。也就是说，MongoDB必须查看每个文档中的每个字段。

`executionTimeMills`字段显示了执行查询所用的毫秒数。

`nTeturned`字段显示返回的结果数是1，因为只有一个用户名为user101的用户。注意，MongoDB必须在集合的每个文档中查找匹配项，因为它不知道用户名是唯一的。

为了使MongoDB高效地响应查询，应用程序中的所有查询模式都应该有索引支持。


<br/>
<br/>


### 创建索引

使用`createIndex`方法在`username`字段上创建索引:

```JavaScript
db.users.createIndex({"username": 1})

// 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引
db.users.createIndex({"username": 1}, {"background": true})

// 查看索引
db.users.getIndexes()
```

索引的创建时间和集合大小有关。可以使用`db.currentOp()`查看索引创建进度。

在此执行之前的查询：

```Javascript
db.users.find({"username": "user101"}).explain("executionStats")

{
		...
        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 1,
                "executionTimeMillis" : 1,
                "totalKeysExamined" : 1,
                "totalDocsExamined" : 1,
                "executionStages" : {
                        "stage" : "FETCH",
                        "nReturned" : 1,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 2,
                        "advanced" : 1,
                        "needTime" : 0,
                        "needYield" : 0,
                        "saveState" : 0,
                        "restoreState" : 0,
                        "isEOF" : 1,
                        "invalidates" : 0,
                        "docsExamined" : 1,
                        "alreadyHasObj" : 0,
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "nReturned" : 1,
                                "executionTimeMillisEstimate" : 0,
                                "works" : 2,
                                "advanced" : 1,
                                "needTime" : 0,
                                "needYield" : 0,
                                "saveState" : 0,
                                "restoreState" : 0,
                                "isEOF" : 1,
                                "invalidates" : 0,
                                "keyPattern" : {
                                        "username" : 1
                                },
                                "indexName" : "username_1",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "username" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "username" : [
                                                "[\"user101\", \"user101\"]"
                                        ]
                                },
                                "keysExamined" : 1,
                                "seeks" : 1,
                                "dupsTested" : 0,
                                "dupsDropped" : 0,
                                "seenInvalidated" : 0
                        }
                }
        },
    	...
}
```

可以看到，查询几乎是一瞬间完成的。而且查询任何用户名所花费的时间基本上是一致的。

<br/>

索引可以显著缩短查询时间。然而，使用索引是有代价的：修改索引字段的写操作（插入、更新和删除）会花费更长的时间。这是因为在更改数据时，除了更新字段，还必须更新索引。通常来说，这个代价是值得的。关键是要找出要索引的字段。

要选择为哪些字段创建索引，可以查看常用的查询以及哪些需要快速执行的查询，并尝试从中找到一组通用的键。

如果这是一个很少用到的查询或是由管理员执行的不太关心时间消耗的查询，那么就不应该在此上面创建索引。


<br/>
<br/>


### 复合索引

对于很多查询模式来说，在两个或更多的键上创建索引是必要的。这称为**复合索引**(compound index)。如果查询中有多个排序方向或者查询条件中有多个键，那么这个索引会很有用。复合索引是创建在多个字段上的索引。

```Javascript
db.users.createIndex({"age": 1, "username": 1}, {"background": true})
```

上面的索引会是下面这个样子:

```
[0, "user100010"] -> 8623513776
[0, "user1002"] -> 8599246768
...
[0, "user100414"] -> 8623564208
[1, "user100113"] -> 8623525680
...
[1, "user100626"] -> 8623591344
[2, "user100191"] -> 8623535664
...
```

<br/>

```Javascript
db.users.find({"age": {"$gte": 21, "$lte": 30}})
```

这是范围查询，用于查找与多个值相匹配的文档（上例指21岁到30岁）。MongoDB会使用索引中的第一个键，即`age`，以返回匹配的文档。如下所示：

```
[21, "user100154"] -> 8623530928
...
[22, "user100017"] -> 8623513392
...
[30, "user100098"] -> 8623532720
...
```

通常来说，如果MongoDB使用索引进行查询，那么它会按照索引顺序返回结果文档。

在设计复合索引时，将排序键放在第一位通常是一个好策略。


<br/>
<br/>


### 如何选择索引

MongoDB是如何选择索引来满足查询的？

假设有5个索引。当有查询进来时，MongoDB会查看这个查询的**形状**。这个形状与要搜索的字段和一些附加信息（比如是否有排序）有关。基于这些信息，系统会识别出一组可能用于满足查询的候选索引。

假设有一个查询进入，5个索引中的3个被标识为改查询的候选索引。然后，MongoDB会创建3个查询计划，分别为每个索引创建1个，并在3个并行线程中运行此查询，每个线程使用不同的索引。
这样做的目的是看哪一个能够最快地返回结果。到达目标状态的第一个查询计划成为赢家。更重要的是，以后会选择它作为索引，用于具有相同形状的其他查询。
每个计划会相互竞争一段时间（成为试用期），之后每一次的结果都会用来在总体上计算出一个获胜的计划。

让多个查询计划相互竞争的真正价值在于，对于具有相同形状的后续查询，MongoDB会知道要选择哪个索引。服务器端维护了查询计划的缓存。一个获胜的计划存储在缓存中，以备在将来用于进行该形状的查询。随着时间的推移以及集合和索引的变化，查询计划可能会从缓存中被淘汰。而MongoDB会再次进行尝试，以找到最适合当前集合和索引集的查询计划。

其他会导致计划从缓存中被淘汰的事件有：重建特定的索引、添加或删除索引，显式清楚计划缓存。重启mongod进程也会导致查询计划缓存丢失。


<br/>
<br/>


### 使用复合索引

复合索引要比单键索引要复杂一些，但也更强大。设计复合索引时需要进行各种思考，目标是使读写操作尽可能高效。

首先需要考虑的是索引的选择性。对于给定的查询模式，索引将在多大程度上减少扫描的记录数。

<br/>

示例，包含一百万条记录的学生数据集，内容就像下面这样:

```
{
	"_id": Object("585d817db4743f74e2da067c"),
    "student_id": 0,
    "scores": [
    {
    	"type": "exam",
        "score": 38.05
    },
    {
    	"type": "quiz",
        "score": 79.45
    },
    {
    	"type": "homework",
        "score" 74.50
    }
    ],
    "class_id:: 127
}
```

我们将从两个索引开始，看看MongoDB如何使用（或不使用）这些索引来满足查询。

```mongo
db.students.createIndex({"class_id": 1})
db.students.createIndex({"student_id": 1, class_id: 1})
```

会围绕以下查询，因为这个查询可以说明在设计索引时必须考虑的几个问题。

```mongo
db.students.find({student_id: {$gt: 500000}, class_id: 54})
.sort({student_id: 1})
.explain("executionStats")
```

此查询对`student_id`大于500000的所有记录进行了请求，这会有一半的记录。我们还将搜索限制在了`class_id`为54的记录中。最后，根据`student_id`按升序进行排序。
通过查看explain方法提供的执行统计信息，来说明MongoDB如何使用索引来完成这个查询。

```json
{
  ...
  "executionStats": {
    "executionSuccess": true,
    "nReturned": 9903,
    "executionTimeMillis": 4325,
    "totalKeysExamined": 850477,
    "totalDocsExamined": 9903,
    "executionStages": {
    }
  },
  ...
}
```

在`executionStats`中，先看一下`totalKeysExamined`。这个字段描述的是，为了生成结果集，MongoDB在索引中遍历了多少个键。
可以将`totalKeysExamined`与`nReturned`进行比较，以了解MongoDB必须遍历多少索引才能找到与查询匹配的文档。在本例中，为了定位9903个索引，一共检查了850477个索引键。这表示用于完成此查询的索引选择性比较低。

explain输出的前面部分是获胜的查询计划（参见`winningPlan`字段）。查询计划描述了MongoDB用来满足查询的步骤。我们尤其对使用了哪个索引以及MongoDB必须在内存中进行排序感兴趣。

```json
"winningPlan": {
  "stage": "FETCH",
  "inputStage": {
  	"stage": "IXSCAN",
    "keyPattern": {
      "student_id": 1,
      "class_id": 1
    },
    "indexName": "student_id_1_class_id_1",
    ...
  },
  ...
}
```

expalin的输出查询计划显示为一颗包含各个阶段的树。一个阶段可以有一个或多个输入阶段，这取决于它有多少个子阶段。输入阶段向其父阶段提供文档或索引键。
本例中有一个输入阶段，即索引扫描。该扫描阶段向其父阶段`FETCH`提供了哪些匹配查询的文档的记录ID。然后，`FETCH`阶段会获取文档本身，并将其分批返回给发出请求的客户端。

<br/>

失败的查询计划（本例只有一个）则会使用基于`class_id`的索引，但之后它必须进行内存排序。当在查询计划中看到`SORT`阶段时，意味着MongoDB将无法使用索引对数据库中的结果集进行排序，而必须执行内存排序。

```
"rejectedPlans": [
  {
    "stage": "SORT",
    "sortPattern": {
      "student_id": 1
    },
  }
]
```

对于这个查询，获胜的索引是能够返回排序输出的索引。要获胜，只需要获取测试数量的已排序结果文档。而对于另一个计划，这个查询线程必须要返回整个结果集（将近10000个文档），因为需要将这些结果在内存中进行排序。

<br/>

上面运行的这个查询，同时包含了多值部分和等值部分。`class_id`再植行查询时更具选择性，它经结果集限制在了10000条以下，而不是多值部分所定位到的850000多条。

换句话说，在当前情况下，如果可以使用基于`calss_id`的索引则会比较好。

<br/>

游标的`hint`方法能够通过索引的形状或名称来指定要使用的特定索引。

如下所示，如果使用`hint`稍微更改以下查询，那么`explain`的输出结果会完全不同。

```
db.students.find({student_id: {$gt: 500000}, class_id: 54})
.sort({{student_id: 1}})
.hint({class_id: 1})
.explain("executionStats")
```

```
"executionStats": [
  "nReturned": 9903,
  "executionTimeMillis": 272,
  "totalKeysExamined": 20076,
  "totalDocsExamined": 20076,
  ...
]
```

结果显示，为了得到略少于一万条的结果集，从扫描大约850000个索引键降低到了大约20000个。此外，执行时间仅为272毫秒，而不是之前使用另一个索引时那个查询的4.3秒。

然而，我们真正希望看到的是`nReturned`与`totalKeysExamined`非常接近。此外，为了更有效地执行此查询，我们希望可以不使用`hint`。解决这两个问题的方法是设计一个更好的索引。

对于这里所讨论的查询模式，更好的索引应该基于`class_id`和`student_id`，两个键的顺序不能变。以`class_id`作为前缀，在查询中使用等值过滤来限制索引需要考虑的键。这是查询中最具选择性的部分，从而有效限制了MongoDB完成此查询所需考虑的键的数量。

创建如下索引：

```
db.students.createIndex({class_id: 1, student_id: 1})
```

<br/>

虽然不是所有数据集都这样，但通常在设计复合索引时，应将等值过滤字段排在多值过滤字段之前。

有了新索引之后，重新执行查询时就不需要提示了。可从explain的输出结果中的`executionStats`字段看到，查询速度非常快（37毫秒），其中返回的结果数(`nReturned`)等于索引所扫描的键数(`totalKeysExamined`)。还可以看到，这个结果是因为`executionStages`所对应的获胜的查询计划包含了一个索引扫描，它使用了新创建的索引。

```
"executionStats": {
  "executionSucess": true,
  "nReturned": 9903,
  "executionTimeMills": 37,
  "totalKeysExamined": 9903,
  "totalDocsExamined": 9903,
  ...
}
```

<br/>

如果思考以下创建索引的原理，就能明白为什么会有这样的结果。`[class_id, student_id]`索引由如下一对对键组成。由于学生ID在其中是有序的，因此为了满足排序要求，MongoDB只需从`class_id`为54的第一对键开始全部进行遍历。

```
...
[53, 999617]
[53, 999916]
[54, 500001],
[54, 500048]
...
```

在考虑复合索引的设计时，需要知道对于利用索引的通用查询模式，如何处理其**等值过滤**、**多值过滤**以及**排序**这些部分。对于所有复合索引都必须考虑这3个因素，而且如果在设计索引时可以正确地平衡这些关注点，那么你的查询就会从MongoDB中获得最佳的性能。

<br/>

为了消除这个例子中的特殊情况，我们改为按照最终成绩进行排序，更改后的查询如下。

```
db.statudents.find({student_id: {$gt: 500000}, class_id: 54})
.sort({final_grade: 1})
.explain("executionStats")
```

运行这个查询并查看explain输出，就会发现这里使用了内存排序。虽然查询速度仍然很快（136毫秒），但由于使用了内存排序，因此比在`student_id`上排序慢了一个数量级。可以看到，在进行内存排序时，获胜的查询计划包含一个`SORT`阶段。

```
...
"executionStats": {
  "executionSuccess": true,
  "nReturned": 9903,
  "executionTimeMillis": 136,
  "totalKeysExamined": 9903,
  "totalDocsExamined": 9903,
  "executionStages": {
    "stage": "SORT",
    ...
  }
}
```

如果可以的话，应该用更好的索引设计来避免内存排序。这样便能从数据集大小和系统负载两个方面更容易地进行扩展。

但要做到这一点，必须做出权衡。这在设计复合索引时是很常见的情况。

为了避免内存排序，需要检查比返回的文档数量更多的键，这对于复合索引来说往往是必需的。为了使用索引进行排序，MongoDB应该能够按顺序遍历索引键。这意味着需要在复合索引键中包含排序字段。

新的复合索引中的键应该按照如下顺序排列：`[class_id, final_grade, student_id]`。注意，我们在等值过滤之后立即包含了排序部分，但这是在多值过滤之前。这个索引在缩小此查询所涉及的键的集合时具有非常多的选择性。之后，通过遍历与等值过滤匹配的那些索引，MongoDB可以识别出与多值过滤部分匹配的纪律。并且这些记录将正确地按照最终成绩升序排序。

<br/>

这个复合索引会迫使MongoDB检查比结果集中文档数量更多的键。不过，通过使用索引来确保对文档排序的方式节省了执行时间。

创建新索引：

```
db.students.find({student_id: {$gt: 500000}, class_id: 54})
.sort({final_grade: 1})
.explain("executionStats")
```

在explain的输出中查看`executionStats`，具体细节和硬件以及系统的其它因素有关，但可以看到获胜的计划中不再包含内存排序，而是使用刚刚创建的索引来满足查询，其中也包括了排序的部分。

```
"executionStats": {
  "executionSuccess": true,
  "nReturnd": 9903,
  "executionTimeMillis": 42,
  "totalKeysExamined": 9905,
  "totalDocsExamined": 9903,
  "executionStages": {
    "stage": "FETCH",
    ...
  }
}
```


<br/>
<br/>


### 复合索引的最佳实践

概括来说，在设计复合索引时：

- 等值过滤的键应该在最前面
- 用于排序的键应该在多值字段之前
- 多值过滤的键应该在最后面

在设计复合索引时应遵循这些准则，然后在实际的工作负载下进行测试，这样就可以确定索引所支持的查询模式都有哪些。


<br/>
<br/>


#### 选择键的方向

到目前为止，我们的所有索引都是升序的。

注意，相互反转的索引是等价的，如`{"age" 1, "username": -1}`使用的查询与`{"age": -1, "username": 1}`完全一样。

只有基于多个查询条件进行排序时，索引方向才是重要的。如果只是基于一个键进行排序，那么MongoDB可以简单地从反方向读取索引。只有在基于多建排序时，方向才重要。


<br/>
<br/>


#### 使用覆盖查询

在上面的例子中，索引都是用来查找正确的文档，然后跟随指针去获取实际的文档。然而，如果查询只需要查询索引中包含的字段，那么就没哟u必要去获取实际的文档。

当一个索引包含用户请求的所有字段时，这个索引就**覆盖**了本次查询。只要切实可行，就应该优先使用覆盖查询，而不是去获取实际的文档，这样可以使工作集大幅减少。

为了确保查询只是用索引就可以完成，应该使用**投射**（只返回查询中指定的字段）来避免返回`_id`字段（除非它是索引的一部分）。

```
db.users.find({"username": "user1010"}, {"_id": 0, "username": 1, "email": 1})
```

如果对一个被覆盖的查询运行explain，那么结果中会有一个并不处于`FETCH`阶段之下的`IXSCAN`阶段，并且在`executionStats`中，`totalDocsExamined`的值是0。


<br/>
<br/>


#### 隐式索引

复合索引具有双重功能，而且针对不同的查询可以充当不同的功索引。

如果有一个在`{age: 1, username: 1}`上的索引，那么`age`字段的排序方式就和在`{age: 1}`上的索引相同。因此，这个复合索引就可以当作`{age: 1}`索引一样使用。

这可以推广到所需的任意多个键：如果一个拥有N个键的索引，那么你同时免费得到了所有这些键的前缀所组成的索引。如果有一个类似`{a: 1, b: 1, c: 1, ..., z: 1}`的索引，那么实际上也等于有了`{a: 1}`, `{a: 1, b:1}`, `{a: 1, b: 1, c: 1}`等一系列索引。

注意，这一点并不适用与这些键的任意子集。如`{b: 1}`, `{a: 1, c: 1}`作为索引的查询是不是被优化的。只有能够使用索引前缀的查询才能从中受益。


<br/>
<br/>


### $运算符如何使用索引








































