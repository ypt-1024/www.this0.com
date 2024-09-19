---
title: 微服务项目架构搭建
tags:
  - sss
categories:
  - null
abbrlink: 40c5a151
date: 2024-02-21 02:23:58
description:
---

# 第一章 NoSQL数据库

## 学习目标

1 了解什么是NoSQL 数据库及常见的NoSQL数据库

2 了解其他类型数据库

## 第一节 NoSQL数据库概述

### 1.1 什么是NoSQL数据库

> NoSQL(NoSQL = Not Only SQL )，意即“不仅仅是SQL”，泛指非关系型的数据库。

-   不遵循SQL标准。
-   不支持ACID。
-   远超于SQL的性能。

### 1.2 NoSQL适用的场景

-   对数据高并发的读写
-   海量数据的读写
-   对数据高可扩展性的

### 1.3 NoSQL不适用的场景

- 需要事务支持

- 基于sql的结构化查询存储，处理复杂的关系查询

  //TODO,不支持事务，不支持ACID,不适用处理复杂关系的查询，但是redis每个都是原子操作

### 1.4 常见NoSQL数据库

#### 1.4.1 Memcached

1 很早出现的NoSql数据库

2 **数据都在内存中，一般不持久化**

3 支持简单的key-value模式，**支持类型单一**

4 一般是作为**缓存数据库**辅助持久化的数据库

//TODO,Memcached不支持持久化，仅支持key-value

#### 1.4.2 Redis

![](https://blog-resources.this0.com/image/202403281329197.png?x-oss-process=style/this0-blog)

1 几乎覆盖了Memcached的绝大部分功能

2 数据都在内存中，**支持持久化，主要用作备份恢复**

3 除了支持简单的key-value模式，还支持多种数据结构的存储，比如 list、set、hash、zset等。

4 一般是作为**缓存数据库**辅助持久化的数据库

//TODO,支持持久化，更多数据结构

#### 1.4.3 MongoDB

![](https://blog-resources.this0.com/image/202403281329198.png?x-oss-process=style/this0-blog)

1 高性能、开源、模式自由(schema free)的**文档型数据库**

2 数据都在内存中， 如果内存不足，把不常用的数据保存到硬盘

3 虽然是key-value模式，但是对value（尤其是**json**）提供了丰富的查询功能

4 支持二进制数据及大型对象

5 可以根据数据的特点**替代RDBMS** ，成为独立的数据库。或者配合RDBMS，存储特定的数据。

//TODO,文档型数据库

## 第二节 DB-Engines数据库排名

查看连接[**http://db-engines.com/en/ranking**](http://db-engines.com/en/ranking "http://db-engines.com/en/ranking")

![image-20230627115200495](https://blog-resources.this0.com/image/202403281329199.png?x-oss-process=style/this0-blog)



# 第二章 Redis简介和安装

## 学习目标

1 能够独立完成redis数据库的安装和启动方式调试

2 能够简单操作redis数据库

## 第一节 Redis简介和适用场景

* Redis是key-value存储系统（区别于MySQL的二维表格的形式存储。）

* Redis数据都是缓存在计算机内存中，但是Redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，实现数据的持久化。

* Redis读写速度快，Redis读取的速度是110000次/s，写的速度是81000次/s；

* Redis的所有操作都是原子性的。

* Redis支持多种数据结构：string（字符串），list（列表），hash（哈希），set（集合），zset(有序集合)

* Redis支持集群部署

* 支持过期时间，支持事务，消息订阅

### 1.1 配合关系型数据库做高速缓存

1 高频次，热门访问的数据，降低数据库IO

![](https://blog-resources.this0.com/image/202403281329200.png?x-oss-process=style/this0-blog)

### 1.2 多样的数据结构存储持久化数据

![](https://blog-resources.this0.com/image/202403281329201.png?x-oss-process=style/this0-blog)



## 第二节 Redis的安装和基本操作

### 2.1 安装

//TODO，archlinux安装redis见站内文章：

### 2.2 Redis的启动和停止

#### 2.2.1 查看安装目录

进入自己的安装路径，然后执行

```
ls |grep redis
```

- redis-benchmark:性能测试工具，可以在自己本子运行，看看自己本子性能如何

  //TODO,后面演示

- redis-check-aof：修复有问题的AOF文件，rdb和aof后面讲

- redis-check-dump：修复有问题的dump.rdb文件

- redis-sentinel：Redis集群使用

- redis-server：Redis服务器启动命令

-   redis-cli：客户端，操作入口

#### 2.2.2 默认配置文件拷贝到自定义路径

养成好习惯，拷贝到自定义路径能留个备份

```
cp /etc/redis/redis.conf /home/ypt/SDE/myredis/redis7.conf
```

配置文件修改4处：

1 后端启动

daemonize yes

2 保护模式

protected-mode no

3 远程连接

注释掉

bind 127.0.0.1 -::1

4 redis访问密码

requirepass redis

#### 2.2.3 前台启动方式

```纯文本
redis-server
```

不推荐原因: 窗口不能关闭,关闭则服务停止

#### 2.2.4 后台启动方式

启动redis时,使用我们自己修改之后的配置文件

```纯文本
redis-server /home/ypt/SDE/myredis/redis7.conf
```

-   查看服务启动状态

```纯文本
ps -ef | grep redis
```

#### 2.2.5 通过客户端连接redis

-   通过客户端指令连接redis

```text
redis-cli
```

-   如果想退出客户端可以 按 Ctrl+c  ,退出客户端不会关闭redis服务
-   通过客户端连接指定端口下的redis (默认6379)

```text
redis-cli -p 6379
```

-   连接后,测试与redis的连通性

```text
ping
```

带密码连接

```
redis-cli -a 密码
```

#### 2.3.6 停止redis服务

-   单实例`非客户端连接模式`下关闭服务

```text
redis-cli shutdown
```

-   在`客户端连接模式`下,直接使用shutdown关闭当前连接的redis服务

```text
shutdown
```

-   `多实例`关闭指定端口的redis服务

```text
redis-cli -p 6379 shutdown
```

#### 2.3.7 Redis端口号 6379 由来

Alessia **Merz**

----

//TODO，跳过了，回头看

# //TODO

##### （2）数据库操作

```纯文本
默认16个数据库，类似数组下标从0开始，初始默认使用0号库
使用命令 select <dbid>来切换数据库。如: select 8
统一密码管理，所有库同样密码。
dbsize查看当前数据库的key的数量
flushdb清空当前库
flushall通杀全部库
```

##### （3）Redis单线程+多路复用

多路复用是指使用一个线程来检查多个文件描述符（Socket）的就绪状态，比如调用select和poll函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后进行真正的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）

* 多路：指的是多个网络连接客户端
* 复用：指的是复用同一个线程
* I/O 多路复用：其实是使用一个线程来检查多个 Socket 的就绪状态，在单个线程中通过记录跟踪每一个 socket（I/O流）的状态来管理处理多个 I/O 流。

![image-20230706100628816](https://blog-resources.this0.com/image/202403281329212.png?x-oss-process=style/this0-blog)

1、一个 socket 客户端与服务端连接时，会生成对应一个套接字描述符(套接字描述符是文件描述符的一种)，每一个 socket 网络连接其实都对应一个文件描述符。

* 文件描述符(file descriptor)： Linux 系统中，把一切都看做是文件，当进程打开现有文件或创建新文件时，内核向进程返回一个文件描述符。可以理解文件描述符是一个索引，这样，要操作文件的时候，我们直接找到索引就可以对其进行操作了。我们将这个索引叫做文件描述符（file descriptor），简称fd。

2、多个客户端与服务端连接时，Redis 使用 「I/O 多路复用程序」 将客户端 socket 对应的 FD 注册到监听列表(一个队列)中。当客服端执行 read、write 等操作命令时，I/O 多路复用程序会将命令封装成一个事件，并绑定到对应的 FD 上。

3、「文件事件处理器」使用 I/O 多路复用模块同时监控多个文件描述符（fd）的读写情况，当 accept、read、write 和 close 文件事件产生时，文件事件处理器就会回调 FD 绑定的事件处理器进行处理相关命令操作。

4、文件事件分派器接收到I/O多路复用程序传来的套接字fd后，并根据套接字产生的事件类型，将套接字派发给相应的事件处理器来进行处理相关命令操作。

5、整个文件事件处理器是在单线程上运行的，但是通过 I/O 多路复用模块的引入，实现了同时对多个 FD 读写的监控，当其中一个 client 端达到写或读的状态，文件事件处理器就马上执行，从而就不会出现 I/O 堵塞的问题，提高了网络通信的性能。



# 第三章 Redis常用数据类型和命令

## 学习目标

1 Redis的数据类型

redis的存储时 key-value形式的,这里的五大类型指的是 value的五种数据类型

//TODO

- String

- List
- Hash (key-value键值对)
- Set （无序集合）
- Zset

----

不常用的5个

- GEO （经纬度）
- HyperLogLog （基数统计）
- bitmap（位图）
- bitfield (位域)
- Stream （流）

2 相关命令

```纯文本
1 如何对键进行一些操作
2 String类型的value值如何进行操作
3 List 类型的value如何进行操作
4 Set类型的value如何进行操作
5 Hash类型的value如何进行操作
6 Zset类型的value如何进行操作
```

## 第一节 key操作的相关命令

//TODO命令，记

| 语法          | 功能                                                         |
| ------------- | ------------------------------------------------------------ |
| keys \*       | 查看当前库所有key (匹配：keys \*1)                           |
| exists key    | 判断某个key是否存在                                          |
| type key      | 查看你的key是什么类型                                        |
| del key       | 删除指定的key数据                                            |
| `unlink key`  | 非阻塞删除,仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作 |
| expire key 10 | 10秒钟：为给定的key设置过期时间                              |
| ttl key       | 查看还有多少秒过期，-1表示永不过期，-2表示已过期             |
| select        | 命令切换数据库                                               |
| dbsize        | 查看当前数据库的key的数量                                    |
| flushdb       | 清空当前库                                                   |
| flushall      | 清空全部库                                                   |

//TODO,unlink key，非阻塞删除

help命令 help @数据类型

## 第二节 字符串类型(String)

### 2.1 简介

1 String是Redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。

2 String类型是二进制安全的。意味着Redis的string可以包含任何数据。比如jpg图片或者序列化的对象。

3 String类型是Redis最基本的数据类型，一个Redis中字符串value最多可以是512M

### 2.2 常用命令

//TODO背

```
set key value [NX|XX] [GET] [EX seconds|PX milliseconds|EXAT unix-time-seconds|PXAT unix-time- milliseconds|KEEPTTL
```

//TODO,EXAT unix-time-seconds 是时间戳，秒，后面是毫秒

| 语法                                          | 解释                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| set \<key>\<value>                            | 添加键值对                                                   |
|                                               | NX：当数据库中key不存在时，可以将key-value添加数据库         |
|                                               | XX：当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥 |
|                                               | EX：key的超时秒数                                            |
|                                               | PX：key的超时毫秒数，与EX互斥                                |
| get \<key>                                    | 查询对应键值                                                 |
| append \<key>\<value>                         | 将给定的\<value> 追加到原值的末尾                            |
| strlen \<key>                                 | 获得值的长度                                                 |
| setnx \<key>\<value>                          | 只有在 key 不存在时 设置 key 的值                            |
| incr \<key>                                   | 将 key 中储存的数字值增1,只能对数字值操作，如果为空，新增值为1 |
| decr \<key>                                   | 将 key 中储存的数字值减1,只能对数字值操作，如果为空，新增值为-1 |
| incrby / decrby \<key><步长>                  | 将 key 中储存的数字值增减。自定义步长                        |
| mset \<key1>\<value1>\<key2>\<value2> .....   | 同时设置一个或多个 key-value对                               |
| mget \<key1>\<key2>\<key3> .....              | 同时获取一个或多个 value                                     |
| msetnx \<key1>\<value1>\<key2>\<value2> ..... | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。有一个失败则都失败(原子性) |
| getrange \<key><起始位置><结束位置>           | 获得值的范围，类似java中的substring，**前包，后包**          |
| setrange \<key><起始位置>\<value>             | 用 \<value> 覆写\<key>所储存的字符串值，从<起始位置>开始(**索引从0**开始)。 |
| setex \<key> <过期时间> \<value>              | 设置键值的同时，设置过期时间，单位秒。                       |
| getset \<key>\<value>                         | 以新换旧，设置了新值同时获得旧值。                           |

//TODO

set默认setxx,

set k1 v1 get

取出老的值，设置新的值v1覆盖他

```
127.0.0.1:6379> get name
"zhangsan"
127.0.0.1:6379> set name lisi get
"zhangsan"
127.0.0.1:6379> get name
"lisi"
```

,整理一下顺序



保留原来的过期时间

keepttl



mset ，一次设置多个

```
127.0.0.1:6379> mset  name1 zhangsan name2 lisi
OK
127.0.0.1:6379> keys *
1) "name1"
2) "name2"
3) "name"
127.0.0.1:6379> mget  name  name1 name2
1) "lisi"
2) "zhangsan"
3) "lisi"
127.0.0.1:6379> 
```



getrange \<key><起始位置><结束位置>  获得值的范围，类似java中的substring，**前包，后包**

127.0.0.1:6379> GETRANGE name 0 -1

0 到 -1 就是取全部



setrange设置指定区间范围内的值，格式是setrange key值 第几个字符开始 ，具体值

![下载](https://blog-resources.this0.com/image/202403282048509.png?x-oss-process=style/this0-blog)



![image-20240328214346543](https://blog-resources.this0.com/image/202403282143643.png?x-oss-process=style/this0-blog)

incr key

incrby key increment

decr key

decrby key decrement



获取字符串长度和内容追加

//TODO，





> redis指令运行的原子性

-   所谓原子操作是指不会被线程调度机制打断的操作；这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch （切换到另一个线程）。
    -   （1）在单线程中， 能够在单条指令中完成的操作都可以认为是"原子操作"，因为中断只能发生于指令之间。
    -   （2）在多线程中，不能被其它进程（线程）打断的操作就叫原子操作。
    -   （3）Redis单命令的原子性主要得益于的单线程。

> 问题 JAVA中的 a++ 是否具有原子性

**原子性：**即不可分割性。比如 a=0；（a非long和double类型） 这个操作是不可分割的，那么我们说这个操作是原子操作。再比如：a++； 这个操作实际是a = a + 1；是可分割的，所以他不是一个原子操作。非原子操作都会存在线程安全问题，需要**使用同步技术（sychronized）或者锁（Lock）来让它变成一个原子操作**。一个操作是原子操作，那么我们称它具有原子性。

### 2.3 数据结构

//TODO

String的数据结构为简单动态字符串(Simple Dynamic String,缩写SDS)。是可以修改的字符串，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配.

![](https://blog-resources.this0.com/image/202403281329213.png?x-oss-process=style/this0-blog)

如图中所示，内部为当前字符串实际分配的空间，capacity一般要高于实际字符串长度len。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。需要注意的是字符串最大长度为512M。



## 第三节 Redis 列表(List)

### 3.1 简介

单键多值, 一个键下的value是一个List.Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。

![](https://blog-resources.this0.com/image/202403281329214.png?x-oss-process=style/this0-blog)

### 3.2 常用命令

| 语法                                               | 功能                                             |
| -------------------------------------------------- | ------------------------------------------------ |
| lpush/rpush \<key>\<value1>\<value2>\<value3> .... | 从左边/右边插入一个或多个值。                    |
| lpop/rpop \<key>                                   | 从左边/右边吐出一个值。值在键在，值光键亡。      |
| rpoplpush \<key1>\<key2>                           | 从\<key1>列表右边吐出一个值，插到\<key2>列表左边 |
| lrange \<key>\<start>\<stop>                       | 按照索引下标获得元素(从左到右)                   |
|                                                    | 0左边第一个，-1右边第一个，（0-1表示获取所有）   |
| lindex \<key>\<index>                              | 按照索引下标获得元素(从左到右)                   |
| llen \<key>                                        | 获得列表长度                                     |
| linsert \<key> before \<value>\<newvalue>          | 在\<value>的前面插入\<newvalue>插入值            |
| linsert \<key> after \<value>\<newvalue>           | 在\<value>的后面插入\<newvalue>插入值            |
| lrem \<key>\<n>\<value>                            | 从左边删除n个value(从左到右)                     |
| lset\<key>\<index>\<value>                         | 将列表key下标为index的值替换成value              |

list 一键多值

```
127.0.0.1:6379> LPUSH list1 1 2 3 4 5
(integer) 5
127.0.0.1:6379> RPUSH list2 11 22 33 44 55
(integer) 5
127.0.0.1:6379> LRANGE list1
(error) ERR wrong number of arguments for 'lrange' command
127.0.0.1:6379> LRANGE list1 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
127.0.0.1:6379> LRANGE list2 0 -1
1) "11"
2) "22"
3) "33"
4) "44"
5) "55"

```

没有rrange操作

```
127.0.0.1:6379> LPOP list2
"22"
127.0.0.1:6379> LRANGE list2 0 -1
1) "33"
2) "44"
3) "55"

```

lpop

最左边的取值并删除，弹出

rpop

最右边的取值并删除，弹出

lindex

llen

lrem ，在key的值里删x个y元素：

lrem key x y

ltrim key 开始index 结束index 截取指定范围的值后再赋值给key

rpoplpush list1 list2左边弹出一个，放到右边：图：

![屏幕截图_20240328_234058](https://blog-resources.this0.com/image/202403282341076.png?x-oss-process=style/this0-blog)

lest key 下标 元素

linsert key before/after

### 3.3 数据结构

//TODO,五大数据解构都没看

List的数据结构为快速链表quickList。首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表。它将所有的元素紧挨着一起存储，分配的是一块连续的内存。当数据量比较多的时候才会改成quicklist。因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next。

![](https://blog-resources.this0.com/image/202403281329215.png?x-oss-process=style/this0-blog)

Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。

## 第四节Redis 集合(Set)

java里面的hashset

添加

sadd

遍历

smembers key

是否存在，有返回1,没有返回0

sismember set1 1

删除

srem

统计

scard

随机取数

127.0.0.1:6379> SRANDMEMBER set1 3

迁移

smove

集合运算

1 sdiff set1 set2 属于set1 不属于set2的元素

```
127.0.0.1:6379> SMEMBERS set1
1) "1"
2) "2"
3) "a"
4) "b"
5) "c"
127.0.0.1:6379> SMEMBERS set2
1) "1"
2) "2"
3) "3"
4) "a"
5) "x"
127.0.0.1:6379> sdiff set1 set2
1) "b"
2) "c"
127.0.0.1:6379> 
```

2 合并：SUNION set1 set2 

1) ```
   127.0.0.1:6379> SUNION set1 set2 
   1) "1" 
   2) "2" 
   3) "a" 
   4) "b" 
   5) "c" 
   6) "3" 
   7) "x"
   ```

3 交集

sinter set1 set2

4 sintercard redis7新出的，返回交集的个数

```
127.0.0.1:6379> sintercard 2 set1 set2 
(integer) 3
```



### 4.1 简介

&#x20;   Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以**自动排重**的，当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。

&#x20;   Redis的Set是string类型的无序集合。它底层其实是一个value为null的hash表，所以添加，删除，查找的**复杂度都是O(1)**。一个算法，随着数据的增加，执行时间的长短，如果是O(1)，数据增加，查找数据的时间不变  &#x20;

### 4.2 常用命令

| 语法                                  | 功能                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| sadd \<key>\<value1>\<value2> .....   | 将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略 |
| smembers \<key>                       | 取出该集合的所有值。                                         |
| sismember \<key>\<value>              | 判断集合\<key>是否为含有该\<value>值，有1，没有0             |
| scard\<key>                           | 返回该集合的元素个数。                                       |
| srem \<key>\<value1>\<value2> ....    | 删除集合中的某个元素。                                       |
| spop \<key>                           | 随机从该集合中吐出一个值                                     |
| spop \<key>\<N>                       | 随机从该集合中吐出N个值。                                    |
| srandmember \<key>\<n>                | 随机从该集合中取出n个值。不会从集合中删除 。                 |
| smove \<source>\<destination>\<value> | 把集合中一个值从一个集合移动到另一个集合                     |
| sinter \<key1>\<key2>                 | 返回两个集合的交集元素。                                     |
| sunion \<key1>\<key2>                 | 返回两个集合的并集元素。                                     |
| sdiff \<key1>\<key2>                  | 返回两个集合的**差集**元素(key1中的，不包含key2中的)         |

### 4.2 数据结构

Set数据结构是dict字典，字典是用哈希表实现的。Java中HashSet的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。Redis的set结构也是一样，它内部也使用hash结构，所有value都指向同一个内部值。



## 第五节 Redis 哈希(Hash)

kv模式不变，但V是一个键值对

```
127.0.0.1:6379> hset user id 1 name zhangsan age 25
(integer) 3
127.0.0.1:6379> hget user id
"1"
127.0.0.1:6379> hget user name
"zhangsan"
127.0.0.1:6379> hget user age
"25"

```

要hget key key进行取值

//TODO

在很早以前，3.0版本，就和hmset没区别？

### 5.1 简介

Redis hash 是一个键值对集合。Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。类似Java里面的Map\<String,Object>用户ID为查找的key，存储的value用户对象包含姓名，年龄，生日等信息

- 方式1  单key+序列化 .问题:每次修改用户的某个属性需要，先反序列化改好后再序列化回去。开销较大。

  ![image-20230706110321231](https://blog-resources.this0.com/image/202403281329216.png?x-oss-process=style/this0-blog)

- 方式2 多key-value .问题:用户ID数据冗余 &#x20;

  ![image-20230706110341117](https://blog-resources.this0.com/image/202403281329217.png?x-oss-process=style/this0-blog)

- 方式3 单key + 多(field+value)

![image-20230706110256007](https://blog-resources.this0.com/image/202403281329218.png?x-oss-process=style/this0-blog)

* **通过 key(用户ID) + field(属性标签) 就可以操作对应属性数据了，既不需要重复存储数据，也不会带来序列化和并发修改控制的问题**&#x20;

### 5.2 常用命令

| 语法                                                 | 功能                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| hset \<key>\<field>\<value>                          | 给\<key>集合中的 \<field>键赋值\<value>                      |
| hget \<key1>\<field>                                 | 从\<key1>集合\<field>取出 value                              |
| hmset \<key1>\<field1>\<value1>\<field2>\<value2>... | 批量设置hash的值                                             |
| hexists\<key1>\<field>                               | 查看哈希表 key 中，给定域 field 是否存在。                   |
| hkeys \<key>                                         | 列出该hash集合的所有field                                    |
| hvals \<key>                                         | 列出该hash集合的所有value                                    |
| hincrby \<key>\<field>\<increment>                   | 为哈希表 key 中的域 field 的值加上增量 1 -1                  |
| hsetnx \<key>\<field>\<value>                        | 将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 . |

### 5.3 数据结构

Hash类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）。当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable。



## 第六节 Redis 有序集合Zset

### 6.1 简介

Redis有序集合zset与普通集合set非常相似，是一个没有重复元素的字符串集合。不同之处是有序集合的每个成员都关联了一个评分（score）,这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了 。因为元素是有序的, 所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。

### 6.2 常用命令



| 语法                                                         | 功能                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| zadd \<key>\<score1>\<value1>\<score2>\<value2>…             | 将一个或多个 member 元素及其 score 值加入到有序集 key 当中。 |
| zrange\<key>\<start>\<stop>  \[WITHSCORES]                   | 升序返回有序集 key 中，下标在\<start>\<stop>之间的元素,0代表第一个元素索引,-1代表最后一个元素索引.带WITHSCORES，可以让分数一起和值返回到结果集。 |
| zrevrange \<key>\<start>\<stop> \[WITHSCORES]                | 降序返回有序集 key 中，下标在\<start>\<stop>之间的元素,0代表第一个元素索引,-1代表最后一个元素索引.带WITHSCORES，可以让分数一起和值返回到结果集 |
| zrangebyscore \<key> \<min> \<max> \[withscores] \[limit offset count] | 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。 |
| zrevrangebyscore \<key> \<max> \<min> \[withscores] \[limit offset count] | 同上，改为从大到小排列。                                     |
| zincrby \<key>\<increment>\<value>                           | 为元素的score加上增量                                        |
| zrem \<key>\<value>                                          | 删除该集合下，指定值的元素                                   |
| zcount \<key>\<min>\<max>                                    | 统计该集合，分数区间内的元素个数                             |
| zrank \<key>\<value>                                         | 返回该值在集合中的排名，从0开始。                            |

案例：如何利用zset实现一个文章访问量的排行榜？

![](https://blog-resources.this0.com/image/202403281329219.png?x-oss-process=style/this0-blog)

小括号不包含

ZRANGEbyscore zset1 (70 500  withscores

zmpop，7.0的 要加count 数量

```
127.0.0.1:6379> ZMPOP 1 myzset max count
(error) ERR syntax error
127.0.0.1:6379> ZMPOP 1 myzset max count
(error) ERR syntax error
127.0.0.1:6379> ZMPOP 1 myzset max count 1
1) "myzset"
2) 1) 1) "three"
      2) "3"
127.0.0.1:6379> ZMPOP 1 myzset max count 1
1) "myzset"
2) 1) 1) "two"
      2) "2"
127.0.0.1:6379> ZMPOP 1 myzset max count 1
1) "myzset"
2) 1) 1) "one"
      2) "1"
127.0.0.1:6379> ZMPOP 1 myzset max count 1
(nil)
```

//TODO,我理解zmpop 后面的可选参数count 是执行几次，

逆序

127.0.0.1:6379> ZrevRANGE myzset 0 -1  

1) "three" 
2) "two" 
3) "one" 

127.0.0.1:6379> ZRANGE myzset 0 -1  

1) "one" 
2) "two" 
3) "three"

 

### 6.3 数据结构

SortedSet(zset)是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构Map\<String, Double>，可以给每一个元素value赋予一个权重score，另一方面它又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。

//TODO,单线程和多路复用

//TODO,后五种没看



# //TODO第四章 Jedis客户端程序

## 学习目标

1 了解Jedis

2 能够独立搭建Jedis的环境

3 熟练操作key操作相关API

4 熟练操作String操作相关API

5 熟练操作List操作相关API

6 熟练操作Set操作相关API

7 熟练操作Hash操作相关API

8 熟练操作Zset操作相关API

## 第一节 Jedis简介

```纯文本
 Redis不仅是使用命令来操作，现在基本上主流的语言都有客户端支持，比如java、C、C#、C++、php、Node.js、Go等。在官方网站里列一些Java的客户端，有Jedis、Redisson、Jredis、JDBC-Redis、等其中官方推荐使用Jedis和Redisson。 在企业中用的最多的就是Jedis。Jedis提供了完整Redis命令，而Redisson有更多分布式的容器实现。
```

## 第二节 环境准备

> 1 创建maven普通项目,导入如下依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.8.1</version>
    <scope>test</scope>
</dependency>
```

> 2 虚拟机和Redis设置

-   禁用Linux的防火墙：Linux(CentOS7)里执行命令
-   systemctl stop/disable firewalld.service
-   redis.conf中注释掉bind 127.0.0.1 ,然后 protected-mode 的值设置为no

> 3 测试JAVA程序和Redis之间的通信

```java
package com.atguigu.jedis;
import redis.clients.jedis.Jedis;
public class Demo01 {
    @Test
    public void TestPing() {
        Jedis jedis = new Jedis("192.168.6.101",6379);
        String pong = jedis.ping();
        System.out.println("连接成功："+pong);
        jedis.close();
    }
}

```

## 第三节 key相关API

```java
@Test
public void testKeyAPI(){
    jedis.set("k1", "v1");
    // 添加 键值对并设置过期时间
    jedis.setex("k2",100, "v2");
    jedis.set("k3", "v3");
    // 获取所有的键
    Set<String> keys = jedis.keys("*");
    System.out.println(keys.size());
    for (String key : keys) {
        System.out.println(key);
    }
    // 判断某个键是否存在
    System.out.println(jedis.exists("k1"));
    // 查看键剩余过期时间
    System.out.println(jedis.ttl("k2"));
    // 根据键获取值
    System.out.println(jedis.get("k1"));
}
```

## 第四节 String相关API

```java
// 添加String
System.out.println(jedis.set("k1", "v1"));
// 一次添加多个
System.out.println(jedis.mset("ka","aaa","kb","bbb"));
//  获取
System.out.println(jedis.get("k1"));
// 一次获取多个
System.out.println(jedis.mget("k1","ka","kb"));
// 追加
System.out.println(jedis.append("k1", "vvvvv"));
// 获取长度
System.out.println(jedis.strlen("k1"));
// 不存在时进行设置
System.out.println(jedis.setnx("k1","xxxxx"));
System.out.println(jedis.setnx("k2","10"));
// 增长/减少
System.out.println(jedis.incr("k2"));
System.out.println(jedis.decr("k2"));
System.out.println(jedis.incrBy("k2", 10));
System.out.println(jedis.decrBy("k2", 10));
```

## 第五节 List相关API

```java
    @Test
    public void testList(){
        // 放入List
        Long lpush = jedis.lpush("klist", "a", "b", "c", "d", "d");
        System.out.println(lpush);
        // 获取List
        List<String> kList = jedis.lrange("klist", 0, -1);
        kList.forEach(System.out::println);
        // 取值
        String klist = jedis.lpop("klist");

    }
```

## 第六节 Set相关API

```java
   @Test
    public void testSet(){
        // 添加一个set集合
        jedis.sadd("skey","a","b","c","d","e");
        // 获取制定的set集合
        Set<String> skey = jedis.smembers("skey");
        skey.forEach(System.out::println);
        //判断是否包含
        System.out.println(jedis.sismember("skey","a"));
        //删除元素
        jedis.srem("skey","a","b");
        //弹出一个元素
        System.out.println(jedis.spop("skey"));
        //弹出N个元素
        System.out.println(jedis.spop("skey",2));
        //从一个set向另一个set移动元素
        jedis.smove("skey","bkey","X");
        // ……

    }
```

## 第七节 Hash相关API

```java
// 添加值
jedis.hset("player1","pname","宇智波赵四儿");
jedis.hset("player1","page","14");
jedis.hset("player1","gender","boy");
// 获取值
System.out.println(jedis.hget("player1","pname"));

//  批量添加值
Map<String,String> player2=new HashMap<String,String>();
player2.put("pname","旋涡刘能");
player2.put("page","13");
player2.put("gender","boy");
jedis.hmset("player2",player2);

// 查看filed是否存在
System.out.println(jedis.hexists("player1", "pname"));
// 查看集合中所有的field
Set<String> player1fields = jedis.hkeys("player1");
player1fields.forEach(System.out::println);
// 查看集合中所有的value
List<String> player1vals = jedis.hvals("player1");
player1vals.forEach(System.out::println);
// 给制定属性+1
jedis.hincrBy("player1","page",5);
// 如不存在,添加某个属性
jedis.hsetnx("player1","height","156");
System.out.println(jedis.hget("player1","page"));
System.out.println(jedis.hget("player1","height"));
```

## 第八节 ZSet相关API

```java
// 准备数据
Map<String ,Double> map=new HashMap<>();
map.put("李四",11d);
map.put("王五",8d);
map.put("赵六",20d);
map.put("刘七",3d);
//  添加元素
jedis.zadd("zkey",10,"张三");
jedis.zadd("zkey",map);
 // 升序返回有序
 Set<String> zkeys = jedis.zrange("zkey", 0, -1);
 zkeys.forEach(System.out::println);
 //  降序返回元素
 Set<String> zkeys2 = jedis.zrevrange("zkey", 0, -1);
 zkeys2.forEach(System.out::println);

 System.out.println("===========");
 Set<String> zkeys3 = jedis.zrangeByScore("zkey", 10, 20);
 zkeys3.forEach(System.out::println);
 System.out.println("===========");
 Set<String> zkeys4 = jedis.zrevrangeByScore("zkey", 20, 10);
 zkeys4.forEach(System.out::println);
 // 增加分数
 jedis.zincrby("zkey",5,"张三");
 jedis.zincrby("zkey",-5,"赵六");
 //  删除 元素
 jedis.zrem("zkey","张三");
 System.out.println(jedis.zcount("zkey",10,20));
 System.out.println(jedis.zrank("zkey","李四"));
```



# //TODO第五章 SpringBoot整合Redis

## 5.1 创建工程

![image-20230627195300172](https://blog-resources.this0.com/image/202403281329220.png?x-oss-process=style/this0-blog)



## 5.2 添加依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.0.5</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
</dependencies>
```



## 5.3 创建配置文件

application.properties

```properties
spring.data.redis.host=192.168.6.131
spring.data.redis.port=6379
#spring.data.redis.client-type=lettuce

#设置lettuce的底层参数
#spring.data.redis.lettuce.pool.enabled=true
#spring.data.redis.lettuce.pool.max-active=8

#切换jedis
spring.data.redis.client-type=jedis
spring.data.redis.jedis.pool.enabled=true
spring.data.redis.jedis.pool.max-active=8
```



RedisTemplate、StringRedisTemplate： 操作redis的工具类

- 要从redis的连接工厂获取链接才能操作redis
- **Redis客户端**

- - Lettuce： 默认
  - Jedis：可以使用以下切换

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <exclusions>
        <exclusion>
            <groupId>io.lettuce</groupId>
            <artifactId>lettuce-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!--        切换 jedis 作为操作redis的底层客户端-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```



## 5.4 创建启动类

```java
package com.atguigu;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```



## 5.5 序列化定制

```java
@Configuration
public class AppRedisConfiguration {

    /**
     * 允许Object类型的key-value，都可以被转为json进行存储。
     * @param redisConnectionFactory 自动配置好了连接工厂
     * @return
     */
    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        //把对象转为json字符串的序列化工具
        template.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }
}
```



## 5.6 编写测试方法

创建测试类，编写测试方法

```java
@Autowired
RedisTemplate redisTemplate;

@Test
void redisTest(){
    //测试String
    redisTemplate.opsForValue().set("a","1234");
    System.out.println(redisTemplate.opsForValue().get("a"));
    
    //测试list集合
    List list = new ArrayList<>();
    list.add("lucy");
    list.add("mary");
    redisTemplate.opsForValue().set("abc",list);
    System.out.println(redisTemplate.opsForValue().get("abc"));
}
```



# //TODO第六章 Redis配置文件解读

## 学习目标

1 了解网络相关的配置

2 了解GENERAL通用配置

3 了解SECURITY安全配置

4 了解LIMIT限制

## 第一节 网络配置相关&#x20;

### （1） bind绑定连接IP

```纯文本
默认情况bind=127.0.0.1只能接受本机的访问请求,不写的情况下，无限制接受任何ip地址的访问,生产环境肯定要写你应用服务器的地址；服务器是需要远程访问的，所以需要将其注释掉.如果开启了protected-mode，那么在没有设定bind ip且没有设密码的情况下，Redis只允许接受本机的响应
```

![image-20230706115102403](https://blog-resources.this0.com/image/202403281329221.png?x-oss-process=style/this0-blog)

```纯文本
保存配置，停止服务，重启启动查看进程，不再限制是本机访问了。
```

![image-20230706115212609](https://blog-resources.this0.com/image/202403281329224.png?x-oss-process=style/this0-blog)

```纯文本
这里完成之后,就可以在window上安装一个redis客户端,连接虚拟机上的redis服务了
```



### （2） 端口号： 6379

![image-20230706115349569](https://blog-resources.this0.com/image/202403281329225.png?x-oss-process=style/this0-blog)



### （3）tcp-backlog 连接队列

```纯文本
  设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。

​  注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值（128），所以需要确认增大/proc/sys/net/core/somaxconn和/proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果
```

![image-20230706115554212](https://blog-resources.this0.com/image/202403281329227.png?x-oss-process=style/this0-blog)



### （4）timeout连接超时

```纯文本
一个空闲的客户端维持多少秒会关闭，0表示关闭该功能。即永不关闭。
```

![image-20230706115650664](https://blog-resources.this0.com/image/202403281329228.png?x-oss-process=style/this0-blog)



### （5）tcp-keepalive  连接心跳检测

```纯文本
对访问客户端的一种心跳检测，每个n秒检测一次。
单位为秒，如果设置为0 则不会进行Keepalive检测，建议设置成60  
```

![image-20230706115754570](https://blog-resources.this0.com/image/202403281329229.png?x-oss-process=style/this0-blog)



## 第二节 GENREAL通用配置

### （1）UNITS单位

```纯文本
 配置大小单位,开头定义了一些基本的度量单位，只支持bytes，不支持bit,大小写不敏感
```

![image-20230706120047530](https://blog-resources.this0.com/image/202403281329230.png?x-oss-process=style/this0-blog)



### （2）INCLUDES包含

```纯文本
在当前配置文件中引入其他配置文件中的内容,一般都是引入一些公共配置
```

![image-20230706120131180](https://blog-resources.this0.com/image/202403281329231.png?x-oss-process=style/this0-blog)



### （3）daemonize 后台进程

```纯文本
是否为后台进程，设置为yes ,守护进程，后台启动
```

![image-20230706120319825](https://blog-resources.this0.com/image/202403281329232.png?x-oss-process=style/this0-blog)



### （4）pidfile 进程ID文件

```纯文本
存放pid文件的位置，每个实例会产生一个不同的pid文件
```

![image-20230706120451425](https://blog-resources.this0.com/image/202403281329233.png?x-oss-process=style/this0-blog)



### （5）databases 16&#x20;

```纯文本
设定库的数量 默认16，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
```

![image-20230706120627340](https://blog-resources.this0.com/image/202403281329234.png?x-oss-process=style/this0-blog)



## 第三节 SECURITY安全配置

> 1 设置密码

![image-20230706120811624](https://blog-resources.this0.com/image/202403281329235.png?x-oss-process=style/this0-blog)

```纯文本
访问密码的查看、设置和取消
在命令中设置密码，只是临时的。重启redis服务器，密码就还原了。
永久设置，需要再配置文件中进行设置。
```

![image-20230706121200103](https://blog-resources.this0.com/image/202403281329236.png?x-oss-process=style/this0-blog)

**重新连接客户端测试**

![image-20230706121253744](https://blog-resources.this0.com/image/202403281329237.png?x-oss-process=style/this0-blog)



## 第四节 LIMIT限制

### （1）maxclients 客户端最大连接数

-   设置redis同时可以与多少个客户端进行连接。
-   默认情况下为10000个客户端。
-   如果达到了此限制，redis则会拒绝新的连接请求，并且向这些连接请求方发出“max number of clients reached”以作回应。

![image-20230706121546036](https://blog-resources.this0.com/image/202403281329238.png?x-oss-process=style/this0-blog)



### （2）maxmemory 最大占用内存

-   建议**必须设置**，否则，将内存占满，造成服务器宕机
-   设置redis可以使用的内存量。一旦到达内存使用上限，redis将会试图移除内部数据，移除规则可以通过maxmemory-policy来指定。
-   如果redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么redis则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH等。
-   但是对于无内存申请的指令，仍然会正常响应，比如GET等。如果你的redis是主redis（说明你的redis有从redis），那么在设置内存使用上限时，需要在系统中留出一些内存空间给同步队列缓存，只有在你设置的是“不移除”的情况下，才不用考虑这个因素。

![image-20230706121729584](https://blog-resources.this0.com/image/202403281329239.png?x-oss-process=style/this0-blog)



### （3）maxmemory-policy  置换策略

-   volatile-lru：使用LRU算法移除key，只对设置了过期时间的键；（最近最少使用）
-   allkeys-lru：在所有集合key中，使用LRU算法移除key
-   volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键
-   allkeys-random：在所有集合key中，移除随机的key
-   volatile-ttl：移除那些TTL值最小的key，即那些最近要过期的key
-   noeviction：不进行移除。针对写操作，只是返回错误信息

![image-20230706121843454](https://blog-resources.this0.com/image/202403281329240.png?x-oss-process=style/this0-blog)



### （4）maxmemory-samples

-   设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个。
-   一般设置3到7的数字，数值越小样本越不准确，但性能消耗越小。

![image-20230706122145032](https://blog-resources.this0.com/image/202403281329241.png?x-oss-process=style/this0-blog)

-   设置为 10，那么 Redis 将会增加额外的 CPU 开销以保证接近真正的 LRU 性能



# //TODO第七章 Redis事务锁机制及案例

## 学习目标

1 熟悉Redis事务的定义和特点

2 熟练Redis事务控制的相关命令

3 熟练使用Redis的锁对数据进行监视

4 熟练编写Redis调用LUA脚本代码

## 第一节 Redis事务和锁机制

### 1.1 Redis事务的定义

```text
  Redis事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。Redis事务的主要作用就是串联多个命令防止别的命令插队。
```



### 1.2 Redis事务控制命令

| 命令    | 功能             |
| ------- | ---------------- |
| multi   | 开始组队         |
| exec    | 执行队列中的命令 |
| discard | 取消组队         |

```纯文本
从输入Multi命令开始，输入的命令都会依次进入命令队列中，但不会执行，直到输入Exec后，Redis会将之前的命令队列中的命令依次执行。组队的过程中可以通过discard取消组队
```

![](https://blog-resources.this0.com/image/202403281329242.png?x-oss-process=style/this0-blog)

> 情况1 ,组队成功,提交成功

![image-20230706135508396](https://blog-resources.this0.com/image/202403281329243.png?x-oss-process=style/this0-blog)



### 1.3 Redis事务错误处理

> 情况2,组队报错,提交失败：提交失败组队中某个命令出现了报告错误，执行时整个的所有队列都会被取消

![](https://blog-resources.this0.com/image/202403281329244.png?x-oss-process=style/this0-blog)

![image-20230706135937567](https://blog-resources.this0.com/image/202403281329245.png?x-oss-process=style/this0-blog)



> 情况3, 组队成功,提交时有成功有失败。如果执行阶段某个命令报出了错误，则只有报错的命令不会被执行，其他的命令都会执行，不会回滚。

![](https://blog-resources.this0.com/image/202403281329246.png?x-oss-process=style/this0-blog)

![image-20230706140338715](https://blog-resources.this0.com/image/202403281329247.png?x-oss-process=style/this0-blog)



### 1.4 Redis事务场景案例

> 场景说明

```纯文本
想想一个场景：有很多人有你的账户,同时去参加双十一抢购  
一个请求想给金额减8000
一个请求想给金额减5000
一个请求想给金额减1000
```

![](https://blog-resources.this0.com/image/202403281329248.png?x-oss-process=style/this0-blog)

> **悲观锁**

```纯文本
悲观锁(Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。
```

![](https://blog-resources.this0.com/image/202403281329249.png?x-oss-process=style/this0-blog)

> **乐观锁**

```纯文本
乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量。Redis就是利用这种check-and-set机制实现事务的。
```

![](https://blog-resources.this0.com/image/202403281329250.png?x-oss-process=style/this0-blog)

> **监视和取消监视key**

```纯文本
在执行multi之前，先执行watch key1 [key2],可以监视一个(或多个) key ，如果在事务**执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。**
```

![image-20230706141601678](https://blog-resources.this0.com/image/202403281329252.png?x-oss-process=style/this0-blog)

```纯文本
取消 WATCH 命令对所有 key 的监视。如果在执行 WATCH 命令之后，EXEC 命令或DISCARD 命令先被执行了的话，那么就不需要再执行UNWATCH 了。 
```



### 1.5 Redis事务的三个特性

-   单独的隔离操作
    -   事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
-   没有隔离级别的概念
    -   队列中的命令没有提交之前都不会实际被执行，因为事务提交前任何指令都不会被实际执行
-   不保证原子性
    -   事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚



## 第二节 Redis Lua 脚本

### 2.1 什么是LUA

> 什么是LUA脚本

![](https://blog-resources.this0.com/image/202403281329253.png?x-oss-process=style/this0-blog)

```text
Lua 是一个小巧的[脚本语言](http://baike.baidu.com/item/脚本语言)，Lua脚本可以很容易的被C/C++ 代码调用，也可以反过来调用C/C++的函数，Lua并没有提供强大的库，一个完整的Lua解释器不过200k，所以Lua不适合作为开发独立应用程序的语言，而是作为嵌入式脚本语言。很多应用程序、游戏使用LUA作为自己的嵌入式脚本语言，以此来实现可配置性、可扩展性。这其中包括魔兽争霸地图、魔兽世界、博德之门、愤怒的小鸟等众多游戏插件或外挂。
```

> LUA脚本的优势

```纯文本
将复杂的或者多步的redis操作，写为一个脚本，一次提交给redis执行，减少反复连接redis的次数。提升性能。
LUA脚本是类似redis事务，有一定的原子性，不会被其他命令插队，可以完成一些redis事务性的操作。但是注意redis的lua脚本功能，只有在Redis 2.6以上的版本才可以使用。利用lua脚本淘汰用户，解决超卖问题。redis 2.6版本以后，通过lua脚本解决争抢问题，实际上是redis利用其单线程的特性，用任务队列的方式解决多任务并发问题。
```

![](https://blog-resources.this0.com/image/202403281329254.png?x-oss-process=style/this0-blog)



### 2.2 创建SpringBoot工程

![image-20230706154943584](https://blog-resources.this0.com/image/202403281329255.png?x-oss-process=style/this0-blog)



### 2.3 引入相关依赖

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.5</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
    </dependencies>
```



### 2.4 创建配置文件

```properties
spring.data.redis.host=192.168.6.131
spring.data.redis.port=6379
```



### 2.5 创建LUA脚本

创建文件夹lua，创建脚本文件test.lua

![image-20230706155329622](https://blog-resources.this0.com/image/202403281329256.png?x-oss-process=style/this0-blog)

> LUA脚本

```bash
local current = redis.call('GET', KEYS[1])
if current == ARGV[1]
  then redis.call('SET', KEYS[1], ARGV[2])
  return true
end
return false
```



### 2.6 创建配置类

```java
package com.atguigu;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.script.RedisScript;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class AppRedisConfiguration  {

    //简单序列化
    @Bean
    public RedisTemplate<String,String> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String,String> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(factory);
        // 设置键序列化方式
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        // 设置简单类型值的序列化方式
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        // 设置默认序列化方式
        redisTemplate.setDefaultSerializer(new StringRedisSerializer());
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }

    //加载lua脚本，设置返回值类型
    @Bean
    public RedisScript<Boolean> script() {
        Resource scriptSource = new ClassPathResource("lua/test.lua");
        return RedisScript.of(scriptSource, Boolean.class);
    }
}
```



### 2.7 创建测试类

```java
package com.atguigu;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.script.RedisScript;

import java.util.Collections;
import java.util.List;

@SpringBootTest
public class TestLua {

    @Autowired
    private RedisScript<Boolean> script;

    @Autowired
    private RedisTemplate<String,String> redisTemplate;

    @Test
    public void test() {
        boolean flag = checkAndSet("hello","helloworld");
        System.out.println(flag ? "修改成功" : "修改失败");

        // 手工添加一个值，再试试
        redisTemplate.opsForValue().set("key", "hello");
        boolean flag1 = checkAndSet("world","hello");
        System.out.println(flag1 ? "修改成功" : "修改失败");
    }

    private boolean checkAndSet(String value1,String value2) {
        List<String> keyList = Collections.singletonList("key");
        return redisTemplate.execute(script, keyList, value1,value2);
    }
}
```

* RedisTemplate.execute说明

![image-20230706160027980](https://blog-resources.this0.com/image/202403281329257.png?x-oss-process=style/this0-blog)

**RedisTemplate.execute需要传入三个值**

![image-20230706160201481](https://blog-resources.this0.com/image/202403281329258.png?x-oss-process=style/this0-blog)

* **第一个参数 RedisScript script：** Lua脚本

- **第二个参数 List keys：**集合
  - 如果是单个参数，使用这个可以转换为**单元素集合**
    - Collections.singletonList(参数)；
  - 多参数
    - `List<String> keys = Arrays.asList(key1, key2, key3);`
- **第三个参数 args：**ARGV，也就是其他类型参数

# 第八章 Redis的持久化

//TODO,顺序跳过了

## 学习目标

1 熟悉Redis持久化的概念

2 熟悉RDB持久化方式特点以及相关操作

3 熟悉AOF持久化方式特点以及相关操作

## 第一节 持久化总体介绍

//TODO,rdb是将数据集整体存下来存在磁盘，aof是存操作命令，后面全部执行一次

> 我们知道Redis是一个内存型数据库,内存的特性是掉电或者程序退出则不保存数据,但是经过实测我们发现,Redis重启服务后,之前存储的数据仍然在,那么这就是通过持久化的方式实现的.

![](https://blog-resources.this0.com/image/202403281329259.png?x-oss-process=style/this0-blog)

> Redis 提供了2个不同形式的持久化方式。

-   RDB（Redis DataBase）定时数据快照 默认方式
-   AOF（Append Of File） 指令日志文件 手动开启

## 第二节 RDB持久化

### 2.1 RDB简介

> 在指定的时间间隔内将内存中的数据集快照写入磁盘,也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里.

![](https://blog-resources.this0.com/image/202403281329260.png?x-oss-process=style/this0-blog)

### 2.2 RDB持久化流程

> 执行流程

&#x20;   Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到 一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。

> Fork子进程

-   Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等） 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程
-   在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，Linux中引入了“写时复制技术”
-   一般情况父进程和子进程会共用同一段物理内存，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。

> RDB持计划流程图

![](https://blog-resources.this0.com/image/202403281329261.png?x-oss-process=style/this0-blog)

### 2.3 RDB相关配置与操作

1 自动触发

> RDB文件名配置

-   &#x20;在redis.conf中配置文件名称，默认为dump.rdb

![image-20230706162436439](https://blog-resources.this0.com/image/202403281329262.png?x-oss-process=style/this0-blog)



我是archlinux开发，改成

```
dbfilename dumparch.rdb
```

单机版可以不改。



redis能使用config命令获取配置

```
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "redis"
127.0.0.1:6379> 
```



> RDB文件位置配置

-   rdb文件的保存路径，也可以修改。默认为Redis启动时命令行所在的目录下.
-   可以通过修改该配置,将RDB文件存到系统的指定目录下

![image-20230706162540821](https://blog-resources.this0.com/image/202403281329263.png?x-oss-process=style/this0-blog)

我就改成了

```
dir /home/ypt/SDE/myredis/dumpfiles/
```



> RDB自动执行快照策略

![image-20230706162725781](https://blog-resources.this0.com/image/202403281329264.png?x-oss-process=style/this0-blog)

-   save命令临时这只快照执行策略
    - 格式：save 秒钟 写操作次数 RDB是整个内存的压缩过的Snapshot，RDB的数据结构，可以配置复合的快照触发条件， 默认是1分钟至少1万个key发生变化，或5分钟至少100个key发生变化，或1个小时至少1个key发生变化。
    
    - 禁用 不设置save指令，或者给save传入空字符串
    



比如，配置

```
save 5 2
```

`对变化的计数是累计的`

间隔 >= 5秒,修改次数 >= 2个,自动触发



> RDB手动执行快照命令

-   save VS bgsave
    -   save ：使用主进行进行持久化指令,save时只管保存，其它不管，全部阻塞。手动保存。不建议。
    -   bgsave：Redis会在后台异步进行快照操作，快照同时还可以响应客户端请求。
        可以通过lastsave 命令获取最后一次成功执行快照的时间
        ![](https://blog-resources.this0.com/image/202403281329265.png?x-oss-process=style/this0-blog)
    
-   flushall命令
    -   执行flushall命令，也会产生dump.rdb文件，但里面是空的，无意义
    
-   shutdown命令
    - shutdown命令在关系服务的时候也会进行自动的持久化,创建rdb文件
    

> RDB备份异常策略

-   stop-writes-on-bgsave-error 配置
    -   当Redis无法写入磁盘的话，直接关掉Redis的写操作。推荐yes.

![image-20230706162910097](https://blog-resources.this0.com/image/202403281329266.png?x-oss-process=style/this0-blog)



> RDB 文件压缩配置

-   rdbcompression配置
    -   对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩。如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能。推荐yes.
        ![image-20230706163019916](https://blog-resources.this0.com/image/202403281329267.png?x-oss-process=style/this0-blog)



> RDB文件检查完整性配置

-   rdbchecksum配置
    -   在存储快照后，还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能.推荐yes.

![image-20230706163056568](https://blog-resources.this0.com/image/202403281329268.png?x-oss-process=style/this0-blog)



> RDB手动备份操作

- 查询rdb文件的目录 将 \*.rdb的文件拷贝到别的地方

-   rdb的恢复
    - 关闭Redis
    
    - 先把备份的文件拷贝到工作目录下 cp dump2.rdb dump.rdb
    
    - 启动Redis, 备份数据会直接加载
    
      

> RDB禁用操作

-   修改配置文件永久禁用

![](https://blog-resources.this0.com/image/202403281329269.png?x-oss-process=style/this0-blog)

-   通过指令临时禁用

```纯文本
动态停止RDB：redis-cli config set save ""  save后给空值，表示禁用保存策略(不建议)
```



### 2.4 RDB的优势和劣势

#### 2.4.1 优势

-   适合大规模的数据恢复
-   对数据完整性和一致性要求不高更适合使用
-   **节省磁盘空间**
-   **恢复速度快**

![](https://blog-resources.this0.com/image/202403281329270.png?x-oss-process=style/this0-blog)

#### 2.4.2 劣势

-   Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑
-   虽然Redis在fork时使用了**写时拷贝技术**,但是如果数据庞大时还是比较消耗性能。
-   在备份周期在一定间隔时间做一次备份，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。

### 2.5 RDB小总结

![](https://blog-resources.this0.com/image/202403281329271.png?x-oss-process=style/this0-blog)



## 第三节 AOF持久化

### 3.1 AOF简介

&#x20;      Append Only File 以日志的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(读操作不记录)， 只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

### 3.2 AOF持计划流程

（1）客户端的请求写命令会被append追加到AOF缓冲区内；

（2）AOF缓冲区根据AOF持久化策略\[always,everysec,no]将操作sync同步到磁盘的AOF文件中；

（3）AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量；

（4）Redis服务重启时，会重新load加载AOF文件中的写操作达到数据恢复的目的；

![](https://blog-resources.this0.com/image/202403281329272.png?x-oss-process=style/this0-blog)

### 3.3 AOF相关配置与操作

> AOF文件名配置

-   可以在redis.conf中配置文件名称，默认为 appendonly.aof

![image-20230706163701368](https://blog-resources.this0.com/image/202403281329273.png?x-oss-process=style/this0-blog)



> AOF文件位置路径

-   Redis6中，AOF文件的保存路径，同RDB的路径一致。
-   Redis7有变化：

base：基本文件

incr：增量文件

manifest：清单文件

![image-20230706164947931](https://blog-resources.this0.com/image/202403281329274.png?x-oss-process=style/this0-blog)



> AOF开启-修复-恢复操作

```纯文本
AOF的备份机制和性能虽然和RDB不同, 但是备份和恢复的操作同RDB一样，都是拷贝备份文件，需要恢复时再拷贝到Redis工作目录下，启动系统即加载。
```

-   正常恢复数据
    - 修改默认的appendonly no，改为yes,开启AOP方式
    
      ![image-20230706163851658](https://blog-resources.this0.com/image/202403281329275.png?x-oss-process=style/this0-blog)
    
    - 将有数据的aof文件复制一份保存到对应目录(查看目录：config get dir)
    
    - 恢复：重启redis然后重新加载
    
      
    
-   异常修复数据
    - 修改默认的appendonly no，改为yes
    
    - 如遇到AOF文件损坏，通过/usr/local/bin/redis-check-aof --fix appendonly.aof.1.incr.aof进行恢复
    
    - 备份被写坏的AOF文件
    
    - 恢复：重启redis，然后重新加载
    
      

> AOF同步频率设置

![image-20230706164003278](https://blog-resources.this0.com/image/202403281329277.png?x-oss-process=style/this0-blog)

-   appendfsync always
    -   始终同步，每次Redis的写入都会立刻记入日志；性能较差但数据完整性比较好
    
-   appendfsync everysec
    -   每秒同步，每秒记入日志一次，如果宕机，本秒的数据可能丢失。
    
-   appendfsync no
    - redis不主动进行同步，把同步时机交给操作系统。
    
      

> AOF压缩配置

- 什么是文件压缩 rewrite重写?

  ```纯文本
  AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制, 当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof
  
  ```

  ![](https://blog-resources.this0.com/image/202403281329278.png?x-oss-process=style/this0-blog)

- 如何实现重写?

  -   AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，redis4.0版本后的重写，是指上就是把rdb 的快照，以二级制的形式附在新的aof头部，作为已有的历史数据，替换掉原来的流水账操作。
  -   no-appendfsync-on-rewrite 设置重写策略
  -   如果 no-appendfsync-on-rewrite=yes ,不写入aof文件只写入缓存，用户请求不会阻塞，但是在这段时间如果宕机会丢失这段时间的缓存数据。（降低数据安全性，提高性能）
  -   如果 no-appendfsync-on-rewrite=no, 还是会把数据往磁盘里刷，但是遇到重写操作，可能会发生阻塞。（数据安全，但是性能降低）

- 何时触发重写?

  ```纯文本
  Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发,重写虽然可以节约大量磁盘空间，减少恢复时间。但是每次重写还是有一定的负担的，因此设定Redis要满足一定条件才会进行重写。
  ```

  -   auto-aof-rewrite-percentage 设置重写基准值
      -   文件达到100%时开始重写（文件是原来重写后文件的2倍时触发）
  -   auto-aof-rewrite-min-size 设置重写基准值
      -   最小文件64MB。达到这个值开始重写。

  ```纯文本
  例如：文件达到70MB开始重写，降到50MB，下次什么时候开始重写？100MB
  系统载入时或者上次重写完毕时，Redis会记录此时AOF大小，设为base_size,如果Redis的AOF当前大小>= base_size +base_size*100% (默认)且当前大小>=64mb(默认)的情况下，Redis会对AOF进行重写。 
  ```

- 重写的流程是?

  -   （1）bgrewriteaof触发重写，判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。
  -   （2）主进程fork出子进程执行重写操作，保证主进程不会阻塞。
  -   （3）子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof\_buf缓冲区和aof\_rewrite\_buf重写缓冲区,保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。
  -   （4）
      -   1\).子进程写完新的AOF文件后，向主进程发信号，父进程更新统计信息。&#x20;
      -   2\).主进程把aof\_rewrite\_buf中的数据写入到新的AOF文件。
  -   （5）使用新的AOF文件覆盖旧的AOF文件，完成AOF重写。

![](https://blog-resources.this0.com/image/202403281329279.png?x-oss-process=style/this0-blog)

### 3.4 AOF的优势

![](https://blog-resources.this0.com/image/202403281329280.png?x-oss-process=style/this0-blog)

-   备份机制更稳健，丢失数据概率更低。
-   可读的日志文本，通过操作AOF稳健，可以处理误操作。

### 3.5 AOF的劣势

-   比起RDB占用更多的磁盘空间。
-   恢复备份速度要慢。
-   每次读写都同步的话，有一定的性能压力。
-   存在个别Bug，造成无法恢复。

### 3.6 AOF小总结

![](https://blog-resources.this0.com/image/202403281329281.png?x-oss-process=style/this0-blog)

### 3.7 持久化方案选择

> RDB和AOP用哪个好?

-   官方推荐两个都启用。
-   如果对数据不敏感，可以选单独用RDB。
-   不建议单独用 AOF，因为可能会出现Bug。
-   如果只是做纯内存缓存，可以都不用。
-   AOF和RDB如果同时开启,系统默认取AOF中的持久化数据

> 官网建议

![](https://blog-resources.this0.com/image/202403281329282.png?x-oss-process=style/this0-blog)

-   RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储
-   AOF持久化方式记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据,AOF命令以redis协议追加保存每次写的操作到文件末尾.
-   Redis还能对AOF文件进行后台重写,使得AOF文件的体积不至于过大
-   只做缓存：如果你只希望你的数据在服务器运行的时候存在,你也可以不使用任何持久化方式.
-   同时开启两种持久化方式
-   在这种情况下,当redis重启的时候会优先载入AOF文件来恢复原始的数据, 因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整.
-   RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢？
-   建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)， 快速重启，而且不会有AOF可能潜在的bug，留着作为一个万一的手段。
-   性能建议

```纯文本
因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 900 1这条规则。
 
如果使用AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。
代价,一是带来了持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。
只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。
默认超过原大小100%大小时重写可以改到适当的数值。
```



# 第九章 Redis主从复制

## 学习目标

1 熟悉Redis主从复制的特点

2 能搭建Redis的 一主二仆和哨兵模式

## 第一节 什么是主从复制

> 主机数据更新后根据配置和策略， 自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主



## 第二节 主从复制的作用

-   读写分离，性能扩展
-   容灾快速恢复

![](https://blog-resources.this0.com/image/202403281329283.png?x-oss-process=style/this0-blog)



## 第三节 主从复制具体操作

### （1）实现思路

- 1 一个redis服务作为主机,主要负责写操作

- 2 两个redis服务作为从机,主要负责读操作

- 3 从机自动从主机同步数据下来

- 4 从机主动找主机,而主机不会找从机

- 5 正常来说主机和从机应该在不同的IP上开启redis服务,我们为了快速模拟,可以在一台机器上模拟出三个redis服务即可

  

### （2） 一台机器上启动多个redis服务

-   使用redis-server启动服务时,要以来redis.conf配置文件.那么我们可以准备三个redis.conf文件,用来配置三个不同的服务,启动三次分别以来三个不同的服务即可



### （3）新建三个redis配置文件

**用于定义每个服务的专属配置**

- 新建redis6379.conf

  关闭aof功能
  
  ![image-20230706184344365](https://blog-resources.this0.com/image/202403281329284.png?x-oss-process=style/this0-blog)
  
  ```纯文本
  include /root/myredis/redis.conf
  pidfile /var/run/redis_6379.pid
  port 6379
  dbfilename dump6379.rdb
  ```
  
  ![](https://blog-resources.this0.com/image/202403281329286.png?x-oss-process=style/this0-blog)
  
  -   含义介绍
  
  ```纯文本
  include /root/myredis/redis.conf # 引入共同的配置
  pidfile /var/run/redis_6379.pid # 使用独立的进程文件
  port 6379 # 设置当前服务的端口号
  dbfilename dump6379.rdb # 使用独立的RDB持久化文件  暂时不适用AOP持久化
  ```



- 新建redis6380.conf

  ![](https://blog-resources.this0.com/image/202403281329287.png?x-oss-process=style/this0-blog)

- 新建redis6381.conf

  ![](https://blog-resources.this0.com/image/202403281329288.png?x-oss-process=style/this0-blog)

  -   在redis6381中多添加一个配置,设置从机的优先级，值越小，优先级越高，用于选举主机时使用。默认100

  ```纯文本
  replica-priority 10 
  ```



### （4）启动三个服务

![image-20230706185404340](https://blog-resources.this0.com/image/202403281329289.png?x-oss-process=style/this0-blog)



### （5）查看启动服务进程

![image-20230706185335546](https://blog-resources.this0.com/image/202403281329290.png?x-oss-process=style/this0-blog)



### （6）使用info replication查看主从相关信息

* 连接redis，使用：redis-cli -p 端口号
* 执行 info replication查看信息

![image-20230706185249563](https://blog-resources.this0.com/image/202403281329291.png?x-oss-process=style/this0-blog)



### （7） 配置主从机器

- 配从不配主,是让从机主动去找主机

- 在6380 和6381的机器上执行如下命令

  ```纯文本
  slaveof 127.0.0.1 6379
  ```

- 执行完毕再次查看主从配置信息

  ![image-20230706185658801](https://blog-resources.this0.com/image/202403281329292.png?x-oss-process=style/this0-blog)



### （8）测试主从读写操作

- 主机上写入数据OK

- 在从机上写数据报错

  ![](https://blog-resources.this0.com/image/202403281329293.png?x-oss-process=style/this0-blog)

- 主机宕机,重启即可恢复主从状态,无需其他操作

- 从机宕机,重启后需要重新执行 slaveof 127.0.0.1 6379 才能恢复

- 从机可以在配置文件中写入slaveof 127.0.0.1 6379 ,这样重启无需手动输入slaveof 127.0.0.1 6379就可以恢复从机状态

  

## 第四节 主从复制原理

-   Slave启动成功连接到master后会发送一个sync命令
-   Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步
-   全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
-   增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步
-   但是只要是重新连接master,一次完全同步（全量复制)将被自动执行

![](https://blog-resources.this0.com/image/202403281329294.png?x-oss-process=style/this0-blog)



## 第五节 主动复制三种模式

> 第一种 一主二仆

-   问题1: 切入点问题,slave1、slave2是从头开始复制还是从切入点开始复制?比如从k4进来，那之前的k1,k2,k3是否也可以复制？
-   问题2 :从机是否可以写？set可否？
-   问题3:主机shutdown后情况如何？从机是上位还是原地待命？
-   问题4:主机又回来了后，主机新增记录，从机还能否顺利复制？
-   问题5:其中一台从机down后情况如何？依照原有它能跟上大部队吗(还会自动变为从机吗?)？

![](https://blog-resources.this0.com/image/202403281329295.png?x-oss-process=style/this0-blog)

> 第二种 薪火相传

```纯文本
上一个Slave可以是下一个slave的Master，Slave同样可以接收其他 slaves的连接和同步请求，那么该slave作为了链条中下一个的master, 可以有效减轻master的写压力,去中心化降低风险。用 slaveof  <ip><port>
​中途变更转向:会清除之前的数据，重新建立拷贝最新的,风险是一旦某个slave宕机，后面的slave都没法备份,主机挂了，从机还是从机，无法写数据了
```

![](https://blog-resources.this0.com/image/202403281329297.png?x-oss-process=style/this0-blog)

> 第三种 反客为主

-   当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。用 slaveof no one  将从机变为主机。

![](https://blog-resources.this0.com/image/202403281329298.png?x-oss-process=style/this0-blog)

## 第六节 哨兵模式

### 6.1 哨兵模式简介

> 反客为主的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库



### 6.2 哨兵模式的使用步骤

#### （1）第一步: 设置简单的一主二仆

![](https://blog-resources.this0.com/image/202403281329299.png?x-oss-process=style/this0-blog)



#### （2）第二步: 为哨兵模式准备配置文件

- 在/root/myredis 目录下新建sentinel.conf 配置文件中放入如下内容

  ```text
  sentinel monitor mymaster 127.0.0.1 6379 1
  ```

- 其中mymaster为监控对象起的服务器名称， 1 为至少有多少个哨兵同意迁移的数量。 &#x20;



#### （3）第三步: 启动哨兵

- 运行/usr/local/bin 下 redis-sentinel 命令,执行/root/myredis/sentinel.conf配置文件

  ```text
  redis-sentinel /root/myredis/sentinel.conf
  ```

- redis做压测可以用自带的redis-benchmark工具 &#x20;



### 6.3 哨兵模式的操作演示

#### （1）主机宕机演示

-   当主机宕机,会从从机中选择一个作为新的主机,根据优先级slave-properity, 原主机重启后会成为从机

![image-20230706190854461](https://blog-resources.this0.com/image/202403281329300.png?x-oss-process=style/this0-blog)



#### （2）复制延时

```纯文本
由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。
```



#### （3）故障恢复

-   优先级在redis.conf中默认：replica-priority 100，值越小优先级越高
-   偏移量是指获得原主机数据最全的
-   每个redis实例启动后都会随机生成一个40位的runid

![](https://blog-resources.this0.com/image/202403281329301.png?x-oss-process=style/this0-blog)



# 第十章 Redis集群操作

## 学习目标

1 熟悉Redis中的集群模式特点

2 能够独立搭建Redis的集群

3 能够熟练操作集群

## 9.1 目前面临问题分析

-   容量不够，redis如何进行扩容？
-   并发写操作， redis如何分摊？
-   另外，主从模式，薪火相传模式，主机宕机，导致ip地址发生变化，应用程序中配置需要修改对应的主机地址、端口等信息。
-   之前通过代理主机来解决，但是redis3.0中提供了解决方案。就是无中心化集群配置。



## 9.2 什么是集群

> Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。

> Redis 集群通过分区（partition）来提供一定程度的可用性（availability ）：即使集群中有一部分节点失效或者无法进行通讯，集群也可以继续处理命令请求。



## 9.3 集群的搭建

### （1）第一步,搭建前的准备

-   之前操作产生的rdb和aof文件删除
-   appendonly 修改回 no
-   清空主从复制和哨兵模式留下的一些文件
-   开启daemonize yes
-   protected-mode no
-   注释掉bind



### （2）第二步,制作六个实例的配置文件

- 配置文件的内容

  ```text
  include /root/myredis/redis.conf
  port 6379
  pidfile "/var/run/redis_6379.pid"
  dbfilename "dump6379.rdb"
  cluster-enabled yes
  cluster-config-file nodes-6379.conf
  cluster-node-timeout 15000
  ```
  
- 内容解释

  ```text
  include /root/myredis/redis.conf #引用公共的配置文件
  port 6379 # 设置端口号
  pidfile "/var/run/redis_6379.pid" # 设置pid进程文件
  dbfilename "dump6379.rdb" # 设置rdb持久化问价名
  cluster-enabled yes # 开启集群
  cluster-config-file nodes-6379.conf # 设置集群使用的结点文件名
  cluster-node-timeout 15000 # 设置结点失联时间
  ```
  
- 创建6379 6380 6381  6389 6390 6391 六个结点的配置文件

  ```text
  创建一个配置文件后,进行复制即可,然后再vim下,通过 :%s/6379/目标端口 来批量替换每个配置文件中的端口号
  ```

  ![image-20230706200218897](https://blog-resources.this0.com/image/202403281329302.png?x-oss-process=style/this0-blog)



### （3）第三步,启动六个服务

![image-20230706200143345](https://blog-resources.this0.com/image/202403281329303.png?x-oss-process=style/this0-blog)

-   组合之前，请确保所有redis实例启动后，nodes-xxxx.conf文件都生成正常。

![image-20230706200315643](https://blog-resources.this0.com/image/202403281329304.png?x-oss-process=style/this0-blog)



### （4）第四步 ,将六个服务合并为一个集群

- 切换目录到redis的src下

  ```text
  cd /opt/redis-7.0.10/src
  ```

- 运行如下指令

  ```text
  redis-cli --cluster create --cluster-replicas 1 192.168.6.131:6379 192.168.6.131:6380 192.168.6.131:6381 192.168.6.131:6382 192.168.6.131:6383 192.168.6.131:6384
  ```

  ```text
  
  ```

  **此处不要用127.0.0.1， 请用真实IP地址    --replicas 1 采用最简单的方式配置集群，一台主机，一台从机，正好三组。**

  ![image-20230706200816752](https://blog-resources.this0.com/image/202403281329305.png?x-oss-process=style/this0-blog)

  输入 yes 继续

  ![image-20230706200954694](https://blog-resources.this0.com/image/202403281329306.png?x-oss-process=style/this0-blog)



## 9.4集群的登录

### （1）集群登录方式

![](https://blog-resources.this0.com/image/202403281329308.png?x-oss-process=style/this0-blog)

-   登录指令添加 -c 代表以集群方式登录



### （2）登录后查看集群信息

- 一个集群至少要有三个主节点。选项 --cluster-replicas 1 表示我们希望为集群中的每个主节点创建一个从节点。

- 分配原则尽量保证每个主数据库运行在不同的IP,每个从库和主库不在一个IP地址上。

  ```text
  cluster nodes
  ```


![image-20230706201216761](https://blog-resources.this0.com/image/202403281329309.png?x-oss-process=style/this0-blog)



## 9.5 集群的slots

![image-20230706201426691](https://blog-resources.this0.com/image/202403281329310.png?x-oss-process=style/this0-blog)

-   一个 Redis 集群包含 16384 个插槽（hash slot）， 数据库中的每个键都属于这 16384 个插槽的其中一个，
-   集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。
-   集群中的每个节点负责处理一部分插槽。 举个例子， 如果一个集群可以有主节点， 其中：
    -   节点 A 负责处理 0 号至 5460 号插槽。
    -   节点 B 负责处理 5461 号至 10922 号插槽。
    -   节点 C 负责处理 10923 号至 16383 号插槽。



## 9.6 集群中录入值

-   在redis-cli每次录入、查询键值，redis都会计算出该key应该送往的插槽，如果不是该客户端对应服务器的插槽，redis会报错，并告知应前往的redis实例地址和端口。
-   redis-cli客户端提供了 –c 参数实现自动重定向。
-   如 redis-cli -c –p 6379 登入后，再录入、查询键值对可以自动重定向。

![image-20230706201647379](https://blog-resources.this0.com/image/202403281329311.png?x-oss-process=style/this0-blog)



-   不在一个slot下的键值，是不能使用mget,mset等多键操作。

-   可以通过{}来定义组的概念，从而使key中{}内相同内容的键值对放到一个slot中去。

![image-20230706201756159](https://blog-resources.this0.com/image/202403281329312.png?x-oss-process=style/this0-blog)



## 9.7 集群中查找值

-   cluster keyslot key 计算key应该保存在那个插槽
-   cluster countkeysinslot slot的值 计算某个插槽中保存的key的数量
-   CLUSTER GETKEYSINSLOT \<slot>\<count> 返回 count 个 slot 槽中的键。

![image-20230706202007674](https://blog-resources.this0.com/image/202403281329313.png?x-oss-process=style/this0-blog)



## 9.8 集群故障恢复

-   如果主节点下线？从节点能否自动升为主节点？注意：**15秒超时**

![image-20230706202459470](https://blog-resources.this0.com/image/202403281329314.png?x-oss-process=style/this0-blog)

-   主节点恢复后，主从关系会如何？主节点回来变成从机。

![image-20230706202640635](https://blog-resources.this0.com/image/202403281329315.png?x-oss-process=style/this0-blog)

-   如果所有某一段插槽的主从节点都宕掉，redis服务是否还能继续?
    -   redis.conf中cluster-require-full-coverage 为yes 那么 ，整个集群都挂掉
    -   redis.conf中cluster-require-full-coverage 为no 那么，只有该插槽数据全都不能使用。



## 9.9 集群提供的好处

-   实现扩容
-   分摊压力
-   无中心配置相对简单



## 9.10 集群的不足

-   多键操作是不被支持的
-   多键的Redis事务是不被支持的。lua脚本不被支持
-   由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。

