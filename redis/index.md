# Redis



参考:

- 《Redis官方文档》: <http://www.redis.cn/documentation.html>
- 《Redis命令大全》: <http://www.redis.cn/commands.html>


<br>


环境:

- CentOS7x86_64
- Redis 3.2


<!--more-->

<br/>

---

<br>



# 简介


<br>


## Redis是什么


Redis是一个使用ANSI C编写的开源、支持网络、基于内存、可选持久性的、非关系型,键值对存储数据库。
Redis是一个开源(BSD许可)的,内存中的数据结构存储系统,它可以用作数据库、缓存和消息中间件。

毫无疑问,Redis开创了一种新的数据存储思路,使用Redis,我们不用在面对功能单调的数据库时,把精力放在如何把大象放进冰箱这样的问题上,而是利用Redis灵活多变的数据结构和数据操作,为不同的大象构建不同的冰箱。希望你喜欢这个比喻。

Remote Dictionary Server(Redis)是由一个Salvatore Sanfilippo写的key-value储存系统。Redis提供了一些丰富的数据结构,包括lists,sets,ordered sets,hashes,当然还有和Memcached一样的string结构,所以常被称为是一款数据结构服务器(data structure server)。Redis当然还包括了对这些数据结构的丰富操作。

你可以在这些类型上面运行原子操作,例如,追加字符串,增加哈希中的值,加入一个元素到列表,计算集合的交集、并集和差集,或者是从有序集合中获取最高排名的元素。



<br/>
<br>

## Redis的优点

为了满足性能,Redis采用内存(in-memory)数据集(dataset)。根据你的使用场景,你可以通过每隔一段时间转储数据集到磁盘,或者追加每条命令到日志来持久化。持久化也可以被禁用,如果你只是需要一个功能丰富,网络化的内存缓存。

- 性能极高,Redis能支持超过100K+每秒的读写频率
- 丰富的数据类型,Redis支持二进制案例的Strings,Lists,Hashes,Sets及Ordered Sets数据类型操作
- 原子,Redis的所有操作都是原子性的,同时Redis还支持对几个操作全并后的原子性执行
- 丰富的特性,Redis还支持publish/sucscribe,通知,key过期等特性
- Redis还支持主从异步复制,非常快的非阻塞初次同步、网络断开时自动重连局部重同步



<br>

---

<br/>



# 安装

直接通过`yum`安装:

```sh
yum install -y redis
```

<br>

启动redis-server的两种方式:

- redis-server: standalone模式
- systemctl redis start: daemon模式
	+ 需要在配置文件中开启daemonize

启动redis-cli:

```
redis-cli
redis-cli -a passwd
```



<br>

---

<br>



# 配置


redis配置文件(/etc/redis.conf)常用参数:

参数 | 说明
- | -
daemonize | 以守护进程启动,放置于后台
bind | 监听地址,建议只对本地127.0.0.1开放
protect-mode | redis的保护模式
requirepass | 设置密码
timeout | 超时
tcp-keepalive | 在Linux上,指定值(秒)用于发送ACKs的时间,关闭连接需要双倍的时间,默认为0
loglevle | 指定日志记录的级别。有四个级别:debug(记录很多信息,用于开发测试)、notice(常用于生产环境)、warning(严重的信息)、verbose(有用的信息)
logfile | 日志文件,默认为stdout
databases | 可用数据库,范围在0-(database-1)
save | 保存数据到磁盘(.rdb)
stop-writes-on-bgsave-error | 后台储存错误停止写
rdbcompression | 储存到本地数据库时(持久化到rdb文件)是否压缩
dbfilename | 本地持久化数据库文件名,默认dump.rdb
dir | 数据库文件路径,是目录
salveof | 设置从库
masterauth | 设置主库认证的密码
slave-read-only | 设置slave是否只读
slave-serve-stale-data | 从库同主库失去连接或复制正在进行时,从库是否继续响应客户端请求
repl-disable-tcp-nodelay | tcp-nodelay
slave-priority | slave优先级,master不能工作后,从众多slave中选出优先值最小的slave提升为master,优先值为0表示不能为master
appendonly | 是否开启AOF数据备份,redis会把所接收到的每一次写操作请求都追加到appendonly.aof文件,当此文件很大
appendsync | AOF文件同步策略,后台会进行大量I/O
no-appendfsync-on-rewrite | -
auto-aof-rewrite-percentage | aof自动重写
auto-aof-rewrite-min-size | 指定最小大小用于aof重写
slowlog-log-slower-than | 慢日志,记录超过特定执行时间的命令,不包括I/o
slowlog-max-len | 慢日志记录的长度,超过大小,最先进入队列的记录会被踢出
hash-max-zipmap-entries | hash将以一种特殊的编码方式(大大减少内存使用)来储存,这是其中一个临界值
hash-max-zipmap-value | 另一个临界值
list-max-ziplist-entries | 多个list以特定的方式编码来节省空间
activerehashing | Redis将在每100ms时使用1ms的CPU时间来对redis的hash表进行重新hash,可降低内存的使用
hz | 不是所有任务都以相同的频率执行,但redis按照指定的“hz”值执行检查任务
aof-rewrite-incremental-fsync | 当一个子节点重写AOF文件时,则文件每生产32m数据进行同步


<br>

官方文档对VM的使用建议:

- 当KEY很小而VALUE很大时,使用VM的效果会比较好,因为这样节约内存比较大
- 当key不小时,可以考虑使用一些非常方法将很大的key变成value,比如将key,value组合成一个新的value




<br>

---

<br/>



# 数据类型



Redis不仅仅是简单的key-value存储器,同时也是一种data structure server。传统的key-value是指支持使用一个key字符串来索引value字符串的储存。而Redis中,value不仅仅支持字符串,还支持更多的复杂结构,包括列表、集合、哈希表等。Redis采用二进制安全,这就意味着你可以使用任何二进制序列作为重点。


<br>


## 字符串(strings)


字符串 是一种最基本的Redis值类型。Redis字符串是二进制安全的,这意味着一个Redis字符串能包含任意类型的数据。

只关心二进制化的字符串,不关心具体格式。只会严格的按照二进制的数据存取。不会妄图已某种特殊格式解析数据。



<br>


## 列表(lists)


Redis列表是简单的字符串列表,按照插入顺序序列,你可以添加一个或多个元素到列表的头部或者尾部。



<br>


## 散列(hash)


Redis Hashes是字符串字段和字符串值之间的映射,因此他们是展现对象的完美数据类型。如一个有姓、名、年龄等属性的用户。一个带有一些字段的hash仅仅需要一块很小的空间储存,因此你可以储存数以百万计的对象在一个小的Redis实例中。

哈希主要用来表现对象,他们有能力储存很多对象,因此你可以将哈希用于许多其他的任务。


<br>


## 无序集合(unorder set)


Redis集合(Set)是一个无序的字符串集合。可以用O(1)的时间复杂度(无论集合中有多少元素时间复杂度都是常量)完成添加、删除、测试元素是否存在。

Redis集合拥有令人满意的不允许包含相同成员的属性。多次添加相同的元素,最终在集合里只会有一个元素。实际上就是添加元素时无序检测元素是否存在。

一个Redis集合有趣的事情是它支持一些服务端的命令从现有的集合出发去进行集合运算,因此你可以在非常短的时间内进行合并(unions)、交集(intersections)、找出不同的元素(difference of sets)。


<br>


## 有序集合(order set)


Redis有序集合与普通集合非常相似,也是一个没有重复项的字符串集合。不同之处是有序集合的每一个成员都关联了一个评分,这个评分被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的,但是评分可以是重复了。

使用有序集合可以以非常快的速度(O(log(N)))添加,删除和更新元素。可以很快根据评分(score)或者次序(position)来获取一个范围的元素。访问有序集合的中间元素也是很快的,因此能够使用有序集合作为一个没有重复成员的智能列表。在有序集合中,你可以很快捷的访问一切你需要的东西。

简而言之,使用有序的集合你可以做完许多对性能有极端要求的任务,而那些任务使用其他类型的数据库真的是很难完成。



<br>

---

<br/>



# 命令


<br>


## 常用命令


```sh
exists key    #判断一个key是否存在

del key    #删除某个或一系列key

type key    #返回某个key元素的数据类型,key不存在返回空

keys key-pattern    #返回匹配的key列表

randomkey    #随机获取一个已经存在的key

rename oldname newname	#改key的名字,如果存在将会覆盖

dbsize    #返回当前数据库的key的总和

expire key time   #设置某个key的过期时间(秒),到期后自动删除

ttl    #查询key剩余存活时间

flushdb    #清空当前数据库中的所有键

flushall    #清空所有数据库中的键
```


<br>
<br>


## 设置相关


```sh
config get    #用来读取Redis服务器的配置参数

config set    #用于更改运行Redis服务器的配置参数

config resetstat    #重置数据统计报告,通常返回OK
```


<br>
<br>


## 连接操作


```sh
quit	#关闭连接

auth	#密码认证

help command	#帮助
```


<br>
<br>


## 持久化


```sh
save		#将数据同步保存到磁盘


bgsave		#将数据异步保存到磁盘


lastsave	#返回上次成功将数据保存到磁盘的Unix时戳
```


<br>
<br/>


## 远程服务


```sh
info		#服务器信息统计,基本所有信息

monitor		#实时转储收到的请求

slaveof		#改变复制策略

shutdown		#将数据同步保存到磁盘,然后关闭服务

server		#Redis server的常规信息

clients		#Client的连接选项

memory		#存储占用相关信息

persistence		#RDB and AOF 相关信息

stats		#常规统计

replication		#Master/slave请求信息

cpu		#CPU占用信息统计

cluster		#Redis 集群信息

keyspace		#数据库信息统计

all			#返回所有信息

default		#返回常规设置信息
```


<br>
<br/>



## 值(value)操作


```sh
exists key		#判断一个key是否存在

del key			#删除一个key

type key		#返回值的类型

keys pattern	#返回满足给定模式的所有key

randomkey		#随机返回key空间的一个

rename oldname newname	#改key的名字,如果存在将会覆盖

dbsize			#返回当前数据库中key的数目

expire			#设定一个key的活动时间(s)

ttl				#获得一个key的活动时间

select index	#按索引查询

move key dbindex	#移动当前数据库中的key到dbindex数据库

flushdb			#删除当前选择的数据库中的所有key

flushall		#删除所有数据库中的所有key
```



<br>
<br/>



## 字符串(string)操作


```
set key value           #给数据库中名称为key的string赋值value

get key                         #返回数据库中名为key的string的value

getset key value                #给名称为key的string赋予上一次的value

mget key1 key2 ... key N                #返回库中多个string的value

setnx key value         #添加string 名称为key 值为value

setex key time value            #向库中添加string 设定过期时间time

mset key 1 value 1 ... key N value N            #批量设置多个string的值

msetnx key 1 value 1 ... key N value N          #如果所有名称为 key N的string都不存在 则向库中添加string 名称为 key N赋值value N

incr key                #名称为key的string加 1 操作

incrby key integer              #名称为key的string增减integer

decr key                #名称为key的string减1操作

decrby key integer              #名称为key的string的值附加value

append key value                #名称为key的值附加value

substr key start end            #返回名称为key的string的value的子串
```



<br/>
<br>



## 列表(list)操作


```
rpush key value                 #在名称为key的list尾部添加一个值为value的元素

lpush key value                 #在名称为key的list首部添加一个值为value的元素

llen key                #返回名称为key的list的长度

lrange key start end            #返回名称为key的list中start至end之间的元素 下表从0开始

ltrim key start end             #截取名称为key的list 保留start至end之间的元素

lindex key index                #返回名称为key的list中index位置的元素

lset key index value            #给名称为key的list中index位置的元素赋值value

lrem key count value            #删除count个名称为key的list中值为value的元素

brpop key1 key2 ... keyN               #rpop的block版本

rpoplpush srckey dstkey                 #返回并删除名为srckey的list尾元素 并将该元素添加到名为dstkey的list的头部
```



<br>
<br/>



## 集合(set)操作


```
sadd key member                 #向名为key的set中添加元素member

srem key member                 #删除名为key的set中元素的member

spop key                #随机返回并删除名为key的set中的一个元素

smove srckey dstkey member              #将member元素从名为srckey的集合移动到名为dstkey的集合

scard key               #返回名为key的set的基数

sismember key member            #测试member是否是名称为key的set的集合

sinter key1 key2 ... key N              #求交集

sinterstore dstkey key1 ... key N               #求交集并将交集保存到dstkey的集合

sunion key1 ... key N           #求并集

sunionstore dstkey key 1 ... key N              #求并集并将并集保存到dstkey的集合

sdiff key1 ... key N            #求差集

sdiffstore dstkey key 1 ... key N               #求差集并将差集保存到dstkey的集合

smembers key            #返回名为key的set的所有元素

srandmember key                 #随机返回名为key的set的一个元素
```



<br>
<br/>



## 有序集合(sorted set)操作


```
zadd key score member           #向名为key的zset中添加元素member score用于排序 如果该元素已经存在 则根据
score更新该元素的顺序

zrem key member                 #删除名为key的zset中的元素member

zincrby key increment member            #如果在名为key的zset中已经存在元素member 则该元素的score增加increment 否则向集合中添加该元素 其score的值为increment

zrank key member                #返回名为key的zset 顺序

zrevrank key member             #返回名为key的zset 倒序

zrange key start end            #返回名为key的zset score顺序按index从start到end返回所有元素

zrevrange key start end                 #返回名为key的zset score倒序按index从start到end返回所有元素

zrangebyscore key min max               #返回名为key的zset中score大于等于min 小于等于max的所有元
```



<br>
<br/>



## hash操作


```
hset key field value            #向名为key的hash中添加元素filed----value

hget key field          #返回名为key的hash中field对应的value

hmset key field1 value1 ... field N value N             #向名为key的hash中添加元素field----value

hmget key field1 ... field N            #返回名为key的hash中filed对应的value

hincrby key field integer               #将名为key的hash中field的value增加integer

hexists key field               #名为key的hash中是否存在键为field的域

hdel key field          #删除名为key的hash中键为field的域

hlen key                #返回名为key的hash中元素个数

hkeys key               #返回名为key的hash中所有键

hvals key               #返回名为key的hash中所有键对应的value

hgetall key     #返回名为key的hash中所有的键 field 及其对应的value

```




<br>

---

<br/>



# 高级应用



Redis高级应用包括**安全性设置、主从复制、事务处理、持久化机制和虚拟内存的使用**。


<br>
<br/>


## 安全性


由于redis速度相当快，一秒钟可以150K次密码尝试，所以需要设置一个密码强度很强大的密码。

设置密码的两种方法：

- `config set requirepass "passwd"`，通过命令设置密码
- 直接在配置文件中`requirepass`属性后加上密码

认证登录的两种方式：

- `redis-cli -a passwd`
- `redi-cli` --> `auth passwd`


<br>
<br>


## 主从复制


Redis的主从复制的配置和使用都比较简单。

- master server
- slave server


Redis主从复制特点：

- 一主多从
- 当master宕机后，优先级值小的那台slave server自动转变为master
- 主从复制不同阻塞master，在同步数据时master可以继续处理client的请求
- 提高了系统的可伸缩性


Redis主从复制过程：

- slave与master建立连接，发送sync同步命令
- master会启动一个后台进程，将数据库快照保存到文件中，同时master主进程会开始收集新的写命令并缓存
- 后台完成保存后，就将此文件发送给slave
- slave将文件保存在磁盘上


<br>

### 主从复制栗子

Redis主从配置，一主多从。
注意：由于redis吃内存，可能会由于内存过小而无法正常启动redis，可查看`/var/log/message`。

<br>

**配置master**：

```
vim /etc/redis_master.conf


daemon yes
bind 127.0.0.1 ip1
port 6379
requirepass fuza_mima
protect-mode yes
datebases 100
logfile /var/log/redis/redis_master.log
dir /var/lib/redis_master


mkdir /var/lib/redis_master
chown redis:redis /var/lib/redis_master


systemctl start redis
```


**配置slave**：

```
vim /etc/redis_slave.conf


daemon yes
bind 127.0.0.1
port 6379
protect-mode yes
logfile /var/log/redis/redis_slave.log
dir /var/lib/redis_slave


slaveof <master-ip> <master-port>
masterauth <master-passwd>
slave-read-only yes
slave-priority 100

#master挂掉后，从slave中选出优先级最小的作为master

······
#其他具体主从参数自己配置




mkdir /var/lib/redis_slave
chown redis:redis /var/lib/redis_slave



systemctl start redis
```

<br/>

**测试master**：

```
redis-cli -a xxx

set name zhang
get zhang
```


**测试slave**：

```
redis-cli
auth('passwd')


key *
get zhang
```

<br/>

**注意**：

由于Redis只是主从，并不像MongoDB的集群功能。当Redis master挂掉以后，虽然优先级较小的slave成为了master，但从库是无法更新数据的。这点也可以从Redis从的配置文件中看出，连接到Redis主的IP：PORT，并通过主的密码来认证。


<br>
<br/>


## 高可用


Redis的主从模式，并不支持高可用。
不过，Redis引进了哨兵模式(sentinel)，提供Redis实时监控和故障检测恢复的功能。
Redis Sentinel 是 Redis 的官方高可用解决方案，是设计用来帮助管理 Redis 实例的系统。

运行 Sentinel 强制使用配置文件，这个文件被系统用来保存当前状态，在重启时能重新加载。如果没有指定配置文件，或者配置文件的路径不可写，Sentinel 将拒绝启动。
Sentinel 运行时默认监听 TCP 端口 26379，所以为了让 Sentinel 正常运行，你的服务器必须开放 26379 端口，以接受从其他 Sentinel 实例 IP 地址的连接。否则，Sentinel 间就没法通信，没法协调，也不会执行故障转移。

Redis Sentinel 是一个分布式系统，这意味着，你通常想要在你的基础设施中运行多个 Sentinel 进程，这些进程使用 gossip 协议来判断一台主服务器是否下线(down)，使用 agreement 协议来获得授权以执行故障转移，并更新相关配置。

```
The redis-sentinel command is a symbolic link to the redis-server command which imply the --sentionel option.

redis-server [ configuration_file ] [ options ] --sentinel
redis-sentinel [ configuration_file ] [ options ]
```

<br>

Redis Sentinel用于完成如下4个任务：

- 监控(Monitoring)
Sentinel 不断检查你的主从实例是否运转正常。

- 通知(Notification)
Sentinel 可以通过 API 来通知系统管理员，或者其他计算机程序，被监控的Redis实例出了问题。

- 自动故障转移(Automatic failover)
如果一台主服务器运行不正常，Sentinel 会开始一个故障转移过程，将从服务器提升为主服务器，配置其他的从服务器使用新的主服务器，使用 Redis 服务器的应用程序在连接时会收到新的服务器地址通知。

- 配置提供者(Configuration provider)
Sentinel 充当客户端服务发现的权威来源：客户端连接到 Sentinel 来询问某个服务的当前 Redis 主服务器的地址。当故障转移发生时，Sentinel 会报告新地址。

<br>

### 配置文件

Redis Sentinel示例配置文件：

只需要指定需要监控的主服务器，并给主服务器去一个名字；
没有必要指定从服务器，因为它们会被自动发现；
每一次故障转移时，将一台从服务器提升为主服务器都会重写配置文件；
无论你指定多少个同意来检测实例是否正常工作，Sentinel 需要系统中已知的大多数 Sentinel 的投票才能开始故障转移，并且在故障转移之后获取一个新的配置纪元(configuration Epoch) 赋予新的配置；



```
#默认26379端口
#sentinel  <option_name>  <master_name>  <option_value>


#仲裁数为2
sentinel  monitor  mymaster  127.0.0.1  6379  2

#哨兵认为实例不可达的毫秒数
sentinel  down-after-milliseconds  mymaster  60000

#
sentinel  failover-timeout  mymaster  180000

#在一次故障转移之后，被配置为同时使用新主服务器的从服务器数量
sentinel  parallel-syncs  mymaster  1

# master 有密码就要使用,
#sentinel auth-pass mymaster ****

sentinel  monitor  resque  192.168.1.3  6380  4
sentinel  down-after-milliseconds  resque  10000
sentinel  failover-timeout  resque  180000
sentinel  parallel-syncs  resque  5
```



<br>
<br/>


## 事务处理


Redis的事务处理比较简单。只能保证client发起的事务中的命令可以连续的执行，而且不会插入其他的client命令。

当一个client在连接中发出multi命令时，这个连接就进入一个事务的上下文，该连接后续的命令不会执行，而是存放在一个队列中，当执行exec命令时，redis会顺序的执行队列中的所有命令。如果其中执行出现错误，执行正确的不会回滚，不同于关系型数据库的事务。


<br>
<br>


## 持久化机制


持久化就是把数据从内存保存到硬盘。

Redis是一个支持持久化的内存数据库，Redis需要经常将内存中的数据同步到磁盘来保证持久化。

Redis支持两种持久化方式：

1. snapshotting(快照)
	+ 将数据存放到文件里，默认方式。默认写入dump.rdb二进制文件中
	+ 可配置redis在n秒内超过m个key被修改就自动做快照
	+ save 500 10 --> 500s内超过10个key被修改，则保存快照

由于快照方式在一定间隔时间做一次保存， 如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。
AOF比快照方式有更好的持久化性，是由于使用aof时，redis会将每一个收到的写命令都通过write函数写入到文件中当redis启动时会通过重新执行文件中保存的写命令在内存中重新建立整个数据库的内容。

2. appendonly file(AOF)
	+ aof方式redis会将每一次的函数都追加到文件中，当redis重启时会重新执行文件中保存的命令

<br>

配置文件参数：

```sh
#启用aof持久化方式
appendonly yes

#每秒写入磁盘一次，在性能和持久化方面做了很好的折中
appendonly everysc


#将数据写入磁盘
save 900 1
save 300 10
save 60 10000
```


<br>
<br/>


## 虚拟内存


Redis的虚拟内存是暂时把不经常访问的数据从内存交换到磁盘中，从而腾出内存空间用于其它的访问数据。
对于redis这样的内存数据库，内存总是不够用的。

在配置文件(/etc/redis.conf)中配置VM:

```sh
#开启vm功能
vm-enableyes


#交换出来的value保存的文件路径
vm-swap-file    /tmp/redis.swap

#redis使用的最大内存上线
vm-max-memory 10000000

#每个页面的大小32字节
vm-page-size 32

#最多使用多少个页面
vm-pages 123217729

#用于执行value对象换入的工作线程数量
vm-max-threads 4
```


<br/>
<br/>



## 批量删除

```
#删除库中所有Key
SELECT 0
FLUSHDB


#删除所有库中Key
FLUSHALL


#默认为db0
#批量删除keys
redis-cli KEYS "test" | xargs redis-cli DEL

#通配符
redis-cli KEYS "test*" | xargs redis-cli DEL

#指定数据库
redis-cli -n 1 KEYS "test*" | xargs redis-cli -n 1 DEL

#指定主机，密码
redis-cli -h xxx -a xx KEYS "test*" | xargs redis-cli -h xxx -a xx DEL
```


<br/>
<br/>



## bigkeys

```
#对redis中的key进行采样，寻找较大的Key
redis-cli --bigkeys

#之后对结果进行分析
```


<br>
<br/>

---

<br>



# 注意



- Redis监听地址`bind： x.x.x.x`，强烈建议只对本地`127.0.0.1`开放。不建议对外网开放，有安全隐患
- 防火墙，最简单就是关闭防火墙，另一个就是开放redis的监听端口
- 开启守护进程，让redis可以在后台运行而不必通过`redis-server`的方式来启动，将配置文件里的`deamonize no`改为`yes`
- 关闭redis的保护模式(protect-mode)，这里的保护模式是指是否允许其他IP的设备访问redis。如果开启的话就只能允许本机访问。如果是生产开发的实际运行环境，请一定开启保护模式
- 设置redis数据库密码！不仅仅是redis，任何数据库都应该设置密码，否则对外网开放的数据库就成了活靶子。



<br>
<br/>

---

<br>


# 多数据库


- Redis支持多个数据库
- 类似于其它数据库，不同的数据存储在不同的数据库中
- Redis下，数据库是由一个整数索引标识，而不是数据库名称。默认情况下，客户端连接到`数据库0`
- Redis不支持自定义数据库名称，所以需要开发者记录那些数据库存储了哪些数据
- Redis不支持为每个数据库设置不同的访问密码，因为密码是在配置文件中设置的。所以一个用户可对所有数据库进行访问
- Redis默认支持16个数据库，但可在配置文件中修改
- 使用`SELECT`命令切换数据库
- `FLUSHALL`命令或清除所有数据库，请注意

<br>

```
cat /etc/redis.conf

# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16

```









































