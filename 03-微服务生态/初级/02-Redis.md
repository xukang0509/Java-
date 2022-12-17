# Redis6

## 一、NoSQL 数据库简介

### 1 技术发展

技术的分类：

1. 解决功能性的问题：Java、Jsp、RDBMS、Tomcat、HTML、Linux、JDBC、SVN
2. 解决扩展性的问题：Struts、Spring、SpringMVC、Hibernate、Mybatis
3. 解决性能的问题：NoSQL、Java线程、Hadoop、Nginx、MQ、ElasticSearch



1. web1.0 时代

Web1.0的时代，数据访问量很有限，用一夫当关的高性能的单点服务器可以解决大部分问题。

![image-20220429171608412](02-Redis.assets/image-20220429171608412.png)



2. web2.0 时代

随着Web2.0的时代的到来，用户访问量大幅度提升，同时产生了大量的用户数据。加上后来的智能移动设备的普及，所有的互联网平台都面临了巨大的性能挑战。

![image-20220429171732062](02-Redis.assets/image-20220429171732062.png)



3. NoSQL能解决CPU及内存压力

![image-20220429171808304](02-Redis.assets/image-20220429171808304.png)



4. NoSQL能解决IO压力

![image-20220429171830249](02-Redis.assets/image-20220429171830249.png)



### 2 NoSQL数据库

#### 2.1 NoSQL数据库概述

NoSQL(NoSQL = **Not Only SQL** )，意即“不仅仅是SQL”，泛指**非关系型的数据库**。 

NoSQL 不依赖业务逻辑方式存储，而以简单的**key-value**模式存储。

因此大大的增加了数据库的扩展能力。

- 不遵循SQL标准。
- 不支持ACID。
- 远超于SQL的性能。



#### 2.2 NoSQL适用场景

- 对数据高并发的读写
- 海量数据的读写
- 对数据高可扩展性的



#### 2.3 NoSQL不适用场景

- 需要事务支持
- 基于sql的结构化查询存储，处理复杂的关系，需要即席查询。
- **（用不着sql的和用了sql也不行的情况，请考虑用NoSql）**



#### 2.4 常见NoSQL数据库

1. Memcache
   - 很早出现的NoSql数据库
   - 数据都在内存中，一般**不持久化**
   - 支持简单的key-value模式，**支持类型单一**
   - 一般是作为**缓存数据库**辅助持久化的数据库



2. Redis
   - 几乎覆盖了Memcached的绝大部分功能
   - 数据都在内存中，**支持持久化**，主要用作备份恢复
   - 除了支持简单的 key-value 模式，还**支持多种数据结构的存储**，比如 **list**、**set**、**hash**、**zset**等
   - 一般是作为**缓存数据库**辅助持久化的数据库



3. MongoDB
   - 高性能、开源、模式自由(schema free)的**文档型数据库**
   - 数据都在内存中， 如果内存不足，把不常用的数据保存到硬盘
   - 虽然是key-value模式，但是对value（尤其是**json**）提供了丰富的查询功能
   - 支持二进制数据及大型对象
   - 可以根据数据的特点**替代RDBMS** ，成为独立的数据库。或者配合RDBMS，存储特定的数据。



### 3 行式存储数据库（大数据时代）

#### 3.1 行式数据库

![image-20220429175101416](02-Redis.assets/image-20220429175101416.png)



#### 3.2 列式数据库

![image-20220429175107822](02-Redis.assets/image-20220429175107822.png)



1. Hbase

HBase是**Hadoop**项目中的数据库。它用于需要对大量的数据进行随机、实时的读写操作的场景中。

HBase的目标就是处理数据量**非常庞大**的表，可以用**普通的计算机**处理超过**10**亿行数据，还可处理有数百万列元素的数据表。



2. Cassandra

Apache Cassandra是一款免费的开源NoSQL数据库，其设计目的在于管理由大量商用服务器构建起来的庞大集群上的**海量数据集(数据量通常达到PB级别)**。在众多显著特性当中，Cassandra最为卓越的长处是对写入及读取操作进行规模调整，而且其不强调主集群的设计思路能够以相对直观的方式简化各集群的创建与扩展流程。



### 4 图关系型数据库

主要应用：社会关系，公共交通网络，地图及网络拓谱(n*(n-1)/2)

![image-20220429175544035](02-Redis.assets/image-20220429175544035.png)

------

## 二、Redis 概述及安装

1. Redis是一个**开源**的 **key-value** 存储系统。
2. 和Memcached类似，它支持存储的value类型相对更多，包括**string**(字符串)、**list**(链表)、**set**(集合)、**zset**(sorted set--有序集合)和**hash**(哈希类型)。
3. 这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是**原子性**的。
4. 在此基础上，Redis**支持各种不同方式的排序**。
5. 与memcached一样，为了保证效率，数据都是**缓存在内存**中。
6. 区别的是Redis会**周期性**的把更新的**数据写入磁盘**或者把修改操作写入追加的记录文件。
7. 并且在此基础上实现了**master-slave(主从)**同步。



### 1 应用场景

#### 1.1 配合关系型数据库做高速缓存

- 高频次，热门访问的数据，降低数据库IO
- 分布式架构，做session共享

![image-20220429180052163](02-Redis.assets/image-20220429180052163.png)



#### 1.2 多样的数据结构存储持久化数据

![image-20220429180128677](02-Redis.assets/image-20220429180128677.png)



### 2 Redis安装

Redis 官方网站：http://redis.io

Redis 中文官方网站：http://redis.cn

![image-20220429200417998](02-Redis.assets/image-20220429200417998.png)



#### 2.1 安装版本

- 6.2.1 for Linux（redis-6.2.1.tar.gz）
- 不用考虑在windows环境下对Redis的支持

#### 2.2 安装步骤

1. **准备工作：下载安装最新版的gcc编译器**

安装 C 语言的编译环境：

```
yum install gcc
```

验证版本：

```
gcc --version
```

![image-20220429200826934](02-Redis.assets/image-20220429200826934.png)



2. 下载redis-6.2.1.tar.gz放/opt目录

3. 解压命令：

   ```
   tar -zxvf redis-6.2.1.tar.gz
   ```

4. 解压完成后进入目录：cd redis-6.2.1

   ![image-20220429201045168](02-Redis.assets/image-20220429201045168.png)

5. 在redis-6.2.1目录下再次执行make命令（只是编译好）

6. 如果没有准备好C语言编译环境，make 会报错—Jemalloc/jemalloc.h：没有那个文件

7. 解决方案：运行make distclean

8. 在redis-6.2.1目录下再次执行make命令（只是编译好）

9. 跳过make test 继续执行: make install



#### 2.3 安装目录：/usr/local/bin

查看默认安装目录：

![image-20220429201311533](02-Redis.assets/image-20220429201311533.png)

- redis-benchmark：性能测试工具，可以在自己本子运行，看看自己本子性能如何
- redis-check-aof：修复有问题的AOF文件
- redis-check-dump：修复有问题的dump.rdb文件
- redis-sentinel：Redis集群使用
- redis-server：Redis服务器启动命令
- redis-cli：客户端，操作入口



#### 2.4 前台启动(不推荐)

前台启动，命令行窗口不能关闭，否则服务器停止

![image-20220429201523416](02-Redis.assets/image-20220429201523416.png)



#### 2.5 后台启动

1. 进入 /opt/redis-6.2.1，复制redis.conf

```
cp /opt/redis-6.2.1/redis.conf /etc/redis.conf
```

2. 编辑 /etc/redis.conf 中 将将里面的daemonize no 改成 yes，让服务在后台启动
3. Redis启动

```
redis-server /etc/redis.conf
```

![image-20220429202038671](02-Redis.assets/image-20220429202038671.png)

4. 用客户端访问：**redis-cli**

![image-20220429202129614](02-Redis.assets/image-20220429202129614.png)

5. 多个端口可以：**redis-cli -p 6379**

6. 测试验证：**ping**

![image-20220429202233045](02-Redis.assets/image-20220429202233045.png)

7. Redis 关闭

单实例关闭：redis-cli shutdown 

多实例关闭，指定端口关闭：redis-cli -p 6379 shutdown



#### 2.6 Redis介绍相关知识

端口**6379**从何而来：Merz

- 默认16个数据库，类似数组下标从0开始，初始默认使用0号库
- 使用命令 `select  dbid` 来切换数据库。如: select 8 
- 统一密码管理，所有库同样密码。
- dbsize：查看当前数据库的key的数量
- flushdb：清空当前库
- flushall：通杀全部库



Redis是**单线程+多路IO复用技术**：

多路复用是指使用一个线程来检查多个文件描述符（Socket）的就绪状态，比如调用 select 和 poll 函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后进行真正的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）



串行  vs  多线程 + 锁（memcached） vs  单线程 + 多路IO复用(Redis)

------

## 三、常用五大数据类型

哪里去获得redis常见数据类型操作命令：http://www.redis.cn/commands.html

### 1 Redis 键(key)

- keys *：查看当前库所有key  (匹配：keys *1)

- exists key：判断某个key是否存在

- type key：查看你的key是什么类型

- del key：删除指定的key数据

- unlink key：根据value选择非阻塞删除

  仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。

- expire key 10：10秒钟：为给定的key设置过期时间

- ttl key：查看还有多少秒过期，-1表示永不过期，-2表示已过期

 

- select 命令切换数据库
- dbsize 查看当前数据库的key的数量
- flushdb 清空当前库
- flushall 通杀全部库



```
[root@localhost /]# /usr/local/bin/redis-cli
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set k1 lucy
OK
127.0.0.1:6379> set k2 mary
OK
127.0.0.1:6379> set k3 jack
OK
127.0.0.1:6379> keys *
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> exists k1
(integer) 1
127.0.0.1:6379> exists k4
(integer) 0
127.0.0.1:6379> type k2
string
127.0.0.1:6379> del k3
(integer) 1
127.0.0.1:6379> exists k3
(integer) 0
127.0.0.1:6379> unlink k2
(integer) 1
127.0.0.1:6379> exists k2
(integer) 0
127.0.0.1:6379> set k3 jack
OK
127.0.0.1:6379> set k2 mary
OK
127.0.0.1:6379> expire k1 10
(integer) 1
127.0.0.1:6379> ttl k1
(integer) 3
127.0.0.1:6379> ttl k1
(integer) 1
127.0.0.1:6379> ttl k1
(integer) -2
127.0.0.1:6379> dbsize
(integer) 2
127.0.0.1:6379> keys *
1) "k2"
2) "k3"
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> select 15
OK
127.0.0.1:6379[15]> select 0
OK
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> 
```



### 2 Redis 字符串(String)

#### 2.1 简介

String是Redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。

String类型是二进制安全的。意味着Redis的string可以包含任何数据。比如jpg图片或者序列化的对象。

String类型是Redis最基本的数据类型，一个Redis中字符串value最多可以是512M。

#### 2.2 常用命令

- set  key  value：添加键值对

  - \*NX：当数据库中key不存在时，可以将key-value添加数据库
  - \*XX：当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
  - \*EX：key的超时秒数
  - \*PX：key的超时毫秒数，与EX互斥

- get  key：查询对应键值

- append  key  value：将给定的 value 追加到原值的末尾

- strlen key：获得值的长度

- setnx  key  value：只有在 key 不存在时，设置 key 的值

- incr  key：将 key 中储存的数字值增1；只能对数字值操作，如果为空，新增值为1

- decr  key：将 key 中储存的数字值减1；只能对数字值操作，如果为空，新增值为-1

- incrby / decrby key  步长：将 key 中储存的数字值增减。自定义步长。

  ```
  原子性：
  所谓原子操作是指不会被线程调度机制打断的操作；
  这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch （切换到另一个线程）。
  （1）在单线程中，能够在单条指令中完成的操作都可以认为是"原子操作"，因为中断只能发生于指令之间。
  （2）在多线程中，不能被其它进程（线程）打断的操作就叫原子操作。
  
  Redis单命令的原子性主要得益于Redis的单线程。
  ```

- mset  key1  value1  key2  value2  .....： 同时设置一个或多个 key-value 对。

- mget  key1  key2  key3  .....：同时获取一个或多个 value 。

- msetnx  key1  value1  key2  value2  .....：同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 

  重点：**原子性，有一个失败则都失败**

- getrange  key  起始位置  结束位置：获得值的范围，类似 java 中的substring，前包，后包

- setrange  key  起始位置  value：用 value 覆写 key 所储存的字符串值，从 起始位置 开始(索引从0开始)。

- setex  key  过期时间  value：设置键值的同时，设置过期时间，单位秒。

- getset  key  value：以新换旧，设置了新值同时获得旧值。



```
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set k1 v100
OK
127.0.0.1:6379> set k2 v200
OK
127.0.0.1:6379> keys *
1) "k2"
2) "k1"
127.0.0.1:6379> get k1
"v100"
127.0.0.1:6379> get k2
"v200"
127.0.0.1:6379> set k1 v1100
OK
127.0.0.1:6379> get k1
"v1100"
127.0.0.1:6379> append k1 abc
(integer) 8
127.0.0.1:6379> get k1
"v1100abc"
127.0.0.1:6379> strlen k1
(integer) 8
127.0.0.1:6379> setnx k1 aaa
(integer) 0
127.0.0.1:6379> setnx k3 v300
(integer) 1
127.0.0.1:6379> get k3
"v300"
127.0.0.1:6379> set k4 100
OK
127.0.0.1:6379> incr k4
(integer) 101
127.0.0.1:6379> get k4
"101"
127.0.0.1:6379> decr k4
(integer) 100
127.0.0.1:6379> incrby k4 100
(integer) 200
127.0.0.1:6379> decrby k4 50
(integer) 150
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
OK
127.0.0.1:6379> mget k1 k2 k3
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k11 v11 k12 v12 k1 v11
(integer) 0
127.0.0.1:6379> keys *
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> msetnx k11 v11 k12 v12
(integer) 1
127.0.0.1:6379> keys *
1) "k11"
2) "k12"
3) "k2"
4) "k1"
5) "k3"
127.0.0.1:6379> set name lucymary
OK
127.0.0.1:6379> getrange name 0 3
"lucy"
127.0.0.1:6379> setrange name 3 abc
(integer) 8
127.0.0.1:6379> get name
"lucabcry"
127.0.0.1:6379> setex age 20 value30
OK
127.0.0.1:6379> ttl age
(integer) 16
127.0.0.1:6379> ttl age
(integer) 13
127.0.0.1:6379> ttl age
(integer) 9
127.0.0.1:6379> ttl age
(integer) -2
127.0.0.1:6379> getset name jack
"lucabcry"
127.0.0.1:6379> get name
"jack"
127.0.0.1:6379> 
```



#### 2.3 数据结构

String的数据结构为简单动态字符串(Simple Dynamic String，缩写SDS)。是可以修改的字符串，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。

![image-20220501110622781](02-Redis.assets/image-20220501110622781.png)

如图中所示，内部为当前字符串实际分配的空间 capacity 一般要高于实际字符串长度len。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。需要注意的是字符串最大长度为512M。



### 3 Redis 列表(List)

#### 3.1 简介

单键多值：

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。

![image-20220501110856749](02-Redis.assets/image-20220501110856749.png)



#### 3.2 常用命令

- lpush/rpush  key  value1  value2  value3  ......： 从左边/右边插入一个或多个值。

  lpush：头插法；rpush：尾插法

- lpop/rpop  key：从左边/右边吐出一个值。值在键在，值光键亡。

  lpop：从头部取数据；rpop：从尾部取数据

- 总结：l，操作头指针；r，操作尾指针

```
127.0.0.1:6379> lpush k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v2"
3) "v1"
127.0.0.1:6379> rpush k2 v1 v2 v3
(integer) 3
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> lpop k1
"v3"
127.0.0.1:6379> rpop k2
"v3"
127.0.0.1:6379> rpop k2
"v2"
127.0.0.1:6379> rpop k2
"v1"
127.0.0.1:6379> exists k2
(integer) 0
127.0.0.1:6379> exists k1
(integer) 1
```



- rpoplpush  key1  key2：从 key1 列表右边吐出一个值，插到 key2 列表左边。

```
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> lpush k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> rpush k2 v11 v12 v13
(integer) 3
127.0.0.1:6379> rpoplpush k1 k2
"v1"
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v11"
3) "v12"
4) "v13"
127.0.0.1:6379> 
```



- lrange  key  start  stop：按照索引下标获得元素(从左到右)

  lrange  key  0  -1：0表示左边第一个，-1表示右边第一个，（0 -1表示获取所有）

- lindex  key  index：按照索引下标获得元素(从左到右)

- llen key：获得列表长度 

- linsert  key  before/after  value  newvalue：在 value 的后面插入 newvalue 插入值

- lrem  key  n  value：从左边删除n个value(从左到右)

- lset  key  index  value：将列表key下标为index的值替换成value

```
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v11"
3) "v12"
4) "v13"
127.0.0.1:6379> lindex k2 0
"v1"
127.0.0.1:6379> lindex k2 2
"v12"
127.0.0.1:6379> llen k2
(integer) 4
127.0.0.1:6379> linsert k2 before "v11" "newv11"
(integer) 5
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "newv11"
3) "v11"
4) "v12"
5) "v13"
127.0.0.1:6379> linsert k2 after "v12" "newv11"
(integer) 6
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "newv11"
3) "v11"
4) "v12"
5) "newv11"
6) "v13"
127.0.0.1:6379> lrem k2 2 "newv11"
(integer) 2
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v11"
3) "v12"
4) "v13"
127.0.0.1:6379> lset k2 1 v111
OK
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v111"
3) "v12"
4) "v13"
127.0.0.1:6379> 
```



#### 3.3 数据结构

List的数据结构为**快速链表quickList**：

首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表。

它将所有的元素紧挨着一起存储，分配的是一块连续的内存。

当数据量比较多的时候才会改成quicklist。

因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next。

![image-20220501113752204](02-Redis.assets/image-20220501113752204.png)

Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。



### 4 Redis 集合(Set)

#### 4.1 简介

Redis Set 对外提供的功能与 list 类似是一个列表的功能，特殊之处在于set是可以**自动排重**的，当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。

Redis 的 Set 是 string类型的无序集合。它底层其实是一个 value 为 null 的 hash 表，所以添加，删除，查找的**复杂度都是O(1)**。

一个算法，随着数据的增加，执行时间的长短，如果是O(1)，数据增加，查找数据的时间不变。



#### 4.2 常用命令

- sadd  key  value1  value2  .....：将一个或多个 member元素加入到集合 key 中，已经存在的 member 元素将被忽略
- smembers  key：取出该集合的所有值。
- sismember  key  value：判断集合 key 是否为含有该 value 值，有的话返回1，没有的话返回0
- scard  key：返回该集合的元素个数。
- srem  key  value1  value2  ......：删除集合中的某个元素。
- spop  key：随机从该集合中吐出一个值。
- srandmember  key  n：随机从该集合中取出n个值。不会从集合中删除 。

```
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> sadd k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> smembers k1
1) "v3"
2) "v2"
3) "v1"
127.0.0.1:6379> sismember k1 v1
(integer) 1
127.0.0.1:6379> sismember k1 v11
(integer) 0
127.0.0.1:6379> scard k1
(integer) 3
127.0.0.1:6379> srem k1 v1 v2
(integer) 2
127.0.0.1:6379> smembers k1
1) "v3"
127.0.0.1:6379> sadd k2 v11 v12 v13 v14
(integer) 4
127.0.0.1:6379> spop k2
"v13"
127.0.0.1:6379> spop k2
"v11"
127.0.0.1:6379> spop k2
"v12"
127.0.0.1:6379> sadd k3 v11 v12 v13 v14
(integer) 4
127.0.0.1:6379> srandmember k3 2
1) "v12"
2) "v13"
127.0.0.1:6379> srandmember k3 2
1) "v12"
2) "v14"
127.0.0.1:6379> srandmember k3 2
1) "v12"
2) "v13"
```



- smove  source  destination  value：把集合中一个值从一个集合移动到另一个集合
- sinter  key1  key2：返回两个集合的交集元素。
- sunion  key1  key2：返回两个集合的并集元素。
- sdiff  key1  key2：返回两个集合的差集元素 (key1中的，不包含key2中的)

```
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> sadd k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> sadd k2 v4 v5 v6
(integer) 3
127.0.0.1:6379> smove k1 k2 v3
(integer) 1
127.0.0.1:6379> smembers k1
1) "v2"
2) "v1"
127.0.0.1:6379> smembers k2
1) "v3"
2) "v4"
3) "v5"
4) "v6"
127.0.0.1:6379> sadd k3 v4 v6 v7
(integer) 3
127.0.0.1:6379> sinter k2 k3
1) "v4"
2) "v6"
127.0.0.1:6379> sunion k2 k3
1) "v6"
2) "v7"
3) "v3"
4) "v4"
5) "v5"
127.0.0.1:6379> sdiff k2 k3
1) "v3"
2) "v5"
127.0.0.1:6379> 
```



#### 4.3 数据结构

Set 数据结构是**Dict字典**，**字典是用哈希表实现的**。

Java 中 HashSet 的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。Redis的set结构也是一样，它的内部也使用hash结构，所有的value都指向同一个内部值。



### 5 Redis 哈希(Hash)

#### 5.1 简介

Redis hash 是一个键值对集合。

Redis hash是一个 string 类型的 **field** 和 **value **的映射表，hash特别适合用于存储对象。

类似Java里面的Map<String, Object>

用户ID为查找的key，存储的value用户对象包含姓名，年龄，生日等信息，如果用普通的key/value结构来存储，主要有以下2种存储方式：

![image-20220501120559927](02-Redis.assets/image-20220501120559927.png)



#### 5.2 常用命令

- hset  key  field  value：给 key 集合中的 field 键赋值 value
- hget  key1  field：从 key1 集合 field 取出 value 
- hmset  key1  field1  value1  field2  value2  .....：批量设置hash的值
- hexists  key1  field：查看哈希表 key 中，给定域 field 是否存在。 
- hkeys  key：列出该hash集合的所有field
- hvals  key：列出该hash集合的所有value
- hincrby  key  field  increment：为哈希表 key 中的域 field 的值加上增量 increment
- hsetnx  key  field  value：将哈希表 key 中的域 field 的值设置为 value，当且仅当域 field 不存在。

```
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> hset user:1001 id 1
(integer) 1
127.0.0.1:6379> hset user:1001 name zhangsan
(integer) 1
127.0.0.1:6379> hget user:1001 id
"1"
127.0.0.1:6379> hget user:1001 name
"zhangsan"
127.0.0.1:6379> hmset user:1002 id 2 name lisi age 20
OK
127.0.0.1:6379> hexists user:1002 id
(integer) 1
127.0.0.1:6379> hexists user:1002 name
(integer) 1
127.0.0.1:6379> hexists user:1002 gender
(integer) 0
127.0.0.1:6379> hkeys user:1002
1) "id"
2) "name"
3) "age"
127.0.0.1:6379> hvals user:1002
1) "2"
2) "lisi"
3) "20"
127.0.0.1:6379> hincrby user:1002 age 2
(integer) 22
127.0.0.1:6379> hsetnx user:1002 age 30
(integer) 0
127.0.0.1:6379> hsetnx user:1002 gender 1
(integer) 1
127.0.0.1:6379> hkeys user:1002
1) "id"
2) "name"
3) "age"
4) "gender"
127.0.0.1:6379> hvals user:1002
1) "2"
2) "lisi"
3) "22"
4) "1"
127.0.0.1:6379> 
```



#### 5.3 数据结构

Hash类型对应的数据结构是两种：

- ziplist（压缩列表）
- hashtable（哈希表）

当 field-value 长度较短且个数较少时，使用ziplist，否则使用hashtable。



### 6 Redis 有序集合(Zset) (sorted set)

#### 6.1 简介

Redis有序集合 zset 与普通集合 set 非常相似，是一个没有重复元素的字符串集合。

不同之处是有序集合的每个成员都关联了一个**评分（score）**，这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了 。

因为元素是有序的, 所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。

访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。



#### 6.2 常用命令

- zadd  key  score1  value1  score2  value2 …...：将一个或多个 member 元素及其 score 值加入到有序集 key 当中；

- zrange  key  start  stop  [WITHSCORES]：返回有序集 key 中，下标在 start ~ stop 之间的元素

  带 WITHSCORES，可以让分数一起和值返回到结果集。

- zrangebyscore  key  min  max  [withscores]  [limit offset count]：返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。 

- zrevrangebyscore  key  max  min  [withscores]  [limit offset count]：同上，改为从大到小排列。 

- zincrby  key  increment  value：为元素的score加上增量

- zrem  key  value1  value2 ...：删除该集合下，指定值的元素

- zcount  key  min  max：统计该集合，分数区间内的元素个数 

- zrank  key  value：返回该值在集合中的排名，从0开始。



```
127.0.0.1:6379> zadd topn 200 java 300 c++ 400 mysql 500 php
(integer) 4
127.0.0.1:6379> zrange topn 0 -1 
1) "java"
2) "c++"
3) "mysql"
4) "php"
127.0.0.1:6379> zrange topn 0 -1 withscores
1) "java"
2) "200"
3) "c++"
4) "300"
5) "mysql"
6) "400"
7) "php"
8) "500"
127.0.0.1:6379> zrangebyscore topn 300 500
1) "c++"
2) "mysql"
3) "php"
127.0.0.1:6379> zrangebyscore topn 300 500 withscores
1) "c++"
2) "300"
3) "mysql"
4) "400"
5) "php"
6) "500"
127.0.0.1:6379> zrevrangebyscore topn 500 200 withscores
1) "php"
2) "500"
3) "mysql"
4) "400"
5) "c++"
6) "300"
7) "java"
8) "200"
127.0.0.1:6379> zincrby topn 50 java
"250"
127.0.0.1:6379> zrem topn mysql php
(integer) 2
127.0.0.1:6379> zrange topn 0 -1 withscores
1) "java"
2) "250"
3) "c++"
4) "300"
127.0.0.1:6379> zcount topn 200 270
(integer) 1
127.0.0.1:6379> zrank topn java
(integer) 0
127.0.0.1:6379> zrank topn c++
(integer) 1
127.0.0.1:6379> 
```



#### 6.3 数据结构

SortedSet (zset) 是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构 Map<String, Double>，可以给每一个元素 value 赋予一个权重 score，另一方面它又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。



zset 底层使用了两个数据结构

1. hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。
2. 跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。



#### 6.4 跳跃表(调表)

简介：

有序集合在生活中比较常见，例如根据成绩对学生排名，根据得分对玩家排名等。对于有序集合的底层实现，可以用数组、平衡树、链表等。数组不便元素的插入、删除；平衡树或红黑树虽然效率高但结构复杂；链表查询需要遍历所有效率低。Redis采用的是跳跃表。跳跃表效率堪比红黑树，实现远比红黑树简单。



实例：对比有序链表和跳跃表，从链表中查询出51

1. 有序列表

![image-20220501204951651](02-Redis.assets/image-20220501204951651.png)

要查找值为51的元素，需要从第一个元素开始依次查找、比较才能找到。共需要6次比较。



2. 跳跃表

![image-20220501205017350](02-Redis.assets/image-20220501205017350.png)

从第2层开始，1节点比51节点小，向后比较。

21节点比51节点小，继续向后比较，后面就是NULL了，所以从21节点向下到第1层

在第1层，41节点比51节点小，继续向后，61节点比51节点大，所以从41向下

在第0层，51节点为要查找的节点，节点被找到，共查找4次。



------

## 四、Redis 配置文件介绍

目录：/etc/redis.conf

### 1 Units：单位

配置大小单位，开头定义了一些基本的度量单位，只支持bytes，不支持bit

大小写不敏感

![image-20220501205800671](02-Redis.assets/image-20220501205800671.png)



### 2 INCLUDES：包含

类似jsp中的include，多实例的情况可以把公用的配置文件提取出来

![image-20220501210116166](02-Redis.assets/image-20220501210116166.png)



### 3 网络相关配置

#### 3.1 bind

默认情况 bind=127.0.0.1；只能接受本机的访问请求

不写的情况下，无限制接受任何ip地址的访问

生产环境肯定要写你应用服务器的地址；服务器是需要远程访问的，所以需要将其注释掉

如果开启了protected-mode，那么在没有设定bind ip且没有设密码的情况下，Redis只允许接受本机的响应

![image-20220501210542058](02-Redis.assets/image-20220501210542058.png)



#### 3.2 protected-mode

将本机访问保护模式设置no

![image-20220501210630559](02-Redis.assets/image-20220501210630559.png)



#### 3.3 port

端口号，默认是6379

![image-20220501210718989](02-Redis.assets/image-20220501210718989.png)



#### 3.4 tcp-bcaklog

设置tcp的backlog，backlog其实是一个连接队列，backlog 队列总和 = 未完成三次握手队列 + 已经完成三次握手队列。

在高并发环境下你需要一个高 backlog 值来避免慢客户端连接问题。

注意Linux内核会将这个值减小到 /proc/sys/net/core/somaxconn 的值（128），所以需要确认增大 /proc/sys/net/core/somaxconn 和 /proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果

![image-20220501210853999](02-Redis.assets/image-20220501210853999.png)



#### 3.5 timeout

一个空闲的客户端维持多少秒会关闭，0表示关闭该功能。即永不关闭。

![image-20220501211000307](02-Redis.assets/image-20220501211000307.png)



#### 3.6 tcp-keepalive

对访问客户端的一种心跳检测，每个n秒检测一次。

单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60 

![image-20220501211120699](02-Redis.assets/image-20220501211120699.png)



### 4 GENERAL：通用

#### 4.1 daemonize

是否为后台进程，设置为yes

守护进程，后台启动

![image-20220501211402442](02-Redis.assets/image-20220501211402442.png)



#### 4.2 pidfile

存放pid文件的位置，每个实例会产生一个不同的pid文件

![image-20220501211501780](02-Redis.assets/image-20220501211501780.png)



#### 4.3 loglevel

指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为**notice**

四个级别根据使用阶段来选择，生产环境选择 notice 或者 warning

![image-20220501211559853](02-Redis.assets/image-20220501211559853.png)



#### 4.4 logfile

日志文件名称

![image-20220501211637420](02-Redis.assets/image-20220501211637420.png)



#### 4.5 databases 16

设定库的数量 默认16，默认数据库为0

可以使用 SELECT  dbid 命令在连接上指定数据库id

![image-20220501211745570](02-Redis.assets/image-20220501211745570.png)



### 5 SECURITY：安全

设置密码：

![image-20220501212210008](02-Redis.assets/image-20220501212210008.png)



访问密码的查看、设置和取消

在命令中设置密码，只是临时的。重启redis服务器，密码就还原了。

永久设置，需要再配置文件中进行设置。

![image-20220501212259725](02-Redis.assets/image-20220501212259725.png)



### 6 LIMITS：限制

#### 6.1 maxclients

- 设置redis同时可以与多少个客户端进行连接。
- 默认情况下为10000个客户端。
- 如果达到了此限制，redis则会拒绝新的连接请求，并且向这些连接请求方发出“max number of clients reached”以作回应。

![image-20220501212641929](02-Redis.assets/image-20220501212641929.png)



#### 6.2 maxmemory

- 建议**必须设置**，否则，将内存占满，造成服务器宕机
- 设置redis可以使用的内存量。一旦到达内存使用上限，redis将会试图移除内部数据，移除规则可以通过 maxmemory-policy 来指定。
- 如果redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么redis则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH等。
- 但是对于无内存申请的指令，仍然会正常响应，比如GET等。如果你的redis是主redis（说明你的redis有从redis），那么在设置内存使用上限时，需要在系统中留出一些内存空间给同步队列缓存，只有在你设置的是“不移除”的情况下，才不用考虑这个因素。

![image-20220501212839939](02-Redis.assets/image-20220501212839939.png)



#### 6.3 maxmemory-policy

- volatile-lru：使用LRU算法移除key，只对设置了过期时间的键；（最近最少使用）
- allkeys-lru：在所有集合key中，使用LRU算法移除key
- volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键
- allkeys-random：在所有集合key中，移除随机的key
- volatile-ttl：移除那些TTL值最小的key，即那些最近要过期的key
- noeviction：不进行移除。针对写操作，只是返回错误信息

![image-20220501213027731](02-Redis.assets/image-20220501213027731.png)



#### 6.4 maxmemory-samples

- 设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个。
- 一般设置3到7的数字，数值越小样本越不准确，但性能消耗越小。

![image-20220501213202266](02-Redis.assets/image-20220501213202266.png)

------

## 五、Redis 的发布和订阅

### 1 什么是发布和订阅

- Redis 发布订阅 (pub/sub) 是一种消息通信模式：
  1. 发送者 (pub) 发送消息
  2. 订阅者 (sub) 接收消息。

- Redis 客户端可以订阅任意数量的频道。



### 2 Redis 的发布和订阅

1. 客户端可以订阅频道如下：

![image-20220501214229569](02-Redis.assets/image-20220501214229569.png)



2. 当给这个频道发布消息后，消息就会发送给订阅的客户端

![image-20220501214330755](02-Redis.assets/image-20220501214330755.png)



### 3 发布订阅命令行实现

1. 打开一个客户端订阅 channel1：

   `SUBSCRIBE channel1`

![image-20220501214519686](02-Redis.assets/image-20220501214519686.png)



2. 打开另一个客户端，给 channel1 发布消息 HelloWorld

   `publish channel1 HelloWorld`

   ![image-20220501214753829](02-Redis.assets/image-20220501214753829.png)



3. 打开第一个客户端可以看到发送的消息

   ![image-20220501214849716](02-Redis.assets/image-20220501214849716.png)



注：发布的消息没有持久化，如果在订阅的客户端收不到 HelloWorld，只能收到订阅后发布的消息。

------

## 六、Redis 新数据类型

### 1 Bitmaps

#### 1.1 简介

现代计算机用二进制（位）作为信息的基础单位，1个字节等于8位，例如 “abc” 字符串是由3个字节组成，但实际在计算机存储时将其用二进制表示，“abc” 分别对应的ASCII码分别是97、98、99，对应的二进制分别是01100001、01100010和01100011，如下图

![image-20220501215233968](02-Redis.assets/image-20220501215233968.png)

合理地使用操作位能够有效地提高内存使用率和开发效率。

Redis提供了 Bitmaps 这个“数据类型”可以实现对位的操作：

1. Bitmaps本身不是一种数据类型，实际上它就是字符串（key-value），但是它可以对字符串的位进行操作。
2. Bitmaps单独提供了一套命令，所以在Redis中使用 Bitmaps 和使用字符串的方法不太相同。可以把Bitmaps想象成一个以位为单位的数组，数组的每个单元只能存储0和1，**数组的下标**在Bitmaps中叫做**偏移量**。

![image-20220501215407889](02-Redis.assets/image-20220501215407889.png)



#### 1.2 命令

##### 1.2.1 setbit

`setbit  key  offset  value`：设置 Bitmaps 中某个偏移量的值value（0 或 1）

offset：偏移量从0开始

实例：

每个独立用户是否访问过网站存放在Bitmaps中，将访问的用户记做1，没有访问的用户记做0，用偏移量作为用户的id。

设置键的第offset个位的值（从0算起），假设现在有20个用户，userid=1，6，11，15，19的用户对网站进行了访问， 那么当前Bitmaps初始化结果如图

![image-20220501220048928](02-Redis.assets/image-20220501220048928.png)

`user:20220501`代表 2022-05-01 这天的独立访问用户的Bitmaps

![image-20220501220337786](02-Redis.assets/image-20220501220337786.png)

注：

- 很多应用的用户id以一个指定数字（例如10000）开头，直接将用户id和Bitmaps的偏移量对应势必会造成一定的浪费，通常的做法是每次做setbit操作时将用户id减去这个指定数字。
- 在第一次初始化Bitmaps时， 假如偏移量非常大， 那么整个初始化过程执行会比较慢， 可能会造成Redis的阻塞。



##### 1.2.2 getbit

`getbit  key  offset`：获取Bitmaps中某个偏移量的值

获取键的第 offset 位的值（从0开始算）

实例：

获取 id=8 的用户是否在2022-05-01这天访问过，返回0说明没有访问过：

![image-20220501220939768](02-Redis.assets/image-20220501220939768.png)

注：因为100根本不存在，所以也是返回0



##### 1.2.3 bitcount

统计**字符串**被设置为1的bit数。一般情况下，给定的整个字符串都会被进行计数，通过指定额外的 start 或 end 参数，可以让计数只在特定的位上进行。

start 和 end 参数的设置，都可以使用负数值：比如 -1 表示最后一个位，而 -2 表示倒数第二个位，start、end 是指bit组的字节的下标数，二者皆包含。

`bitcount  key  [start end]`: 统计字符串从 start 字节到 end 字节比特值为1的数量

实例：

计算2022-05-01这天的独立访问用户数量

![image-20220501221519707](02-Redis.assets/image-20220501221519707.png)

start 和 end 代表起始和结束字节数，下面操作计算用户id在第1个字节到第3个字节之间的独立访问用户数，对应的用户id是11，15，19。

![image-20220501221639472](02-Redis.assets/image-20220501221639472.png)



举例： K1 【01000001 01000000 00000000 00100001】，对应【0，1，2，3】

`bitcount K1 1 2`：统计下标1、2字节组中bit=1的个数，即01000000 00000000 --> 1

`bitcount K1 1 3`：统计下标1~3字节组中bit=1的个数，即01000000 00000000 00100001 --> 3

`bitcount K1 0 -2`：统计下标0到下标倒数第2，字节组中bit=1的个数，即01000001 01000000  00000000  --> 3

  注意：redis的setbit设置或清除的是bit位置，而bitcount计算的是byte位置。



##### 1.2.4 bitop

`bitop  and(or/not/xor)  destkey  key [key...]`

bitop是一个复合操作，它可以做多个 Bitmaps 的and（交集） 、or（并集）、not（非）、xor（异或）操作并将结果保存在destkey中。



实例：

2020-11-04 日访问网站的userid=1，2，5，9。

`setbit unique:users:20201104 1 1`

`setbit unique:users:20201104 2 1`

`setbit unique:users:20201104 5 1`

`setbit unique:users:20201104 9 1`

 

2020-11-03 日访问网站的userid=0，1，4，9。

`setbit unique:users:20201103 0 1`

`setbit unique:users:20201103 1 1`

`setbit unique:users:20201103 4 1`

`setbit unique:users:20201103 9 1`



计算出两天都访问过网站的用户数量

`bitop and unique:users:and:20201104_03 unique:users:20201103 unique:users:20201104`

![image-20220501224114800](02-Redis.assets/image-20220501224114800.png)

![image-20220501223729962](02-Redis.assets/image-20220501223729962.png)



计算出任意一天都访问过网站的用户数量（例如月活跃就是类似这种），可以使用or求并集

`bitop or unique:users:or:20201104_03 unique:users:20201103 unique:users:20201104`

![image-20220501224131250](02-Redis.assets/image-20220501224131250.png)



#### 1.3 Bitmaps 和 set 做对比

假设网站有1亿用户，每天独立访问的用户有5千万，如果每天用集合类型和Bitmaps分别存储活跃用户可以得到表

**Set 和 Bitmaps 存储一天活跃用户对比**

| 数据类型     | 每个用户id占用空间 | 需要存储的用户量 | 全部内存量 |
| ------------ | ------------------ | ---------------- | ---------- |
| Set 集合类型 | 64位               | 5000 0000        | 400MB      |
| Bitmaps      | 1位                | 1 0000 0000      | 12.5MB     |



很明显，这种情况下使用Bitmaps能节省很多的内存空间，尤其是随着时间推移节省的内存还是非常可观的

**Set 和 Bitmaps 存储独立用户空间对比**

| 数据类型     | 一天   | 一月  | 一年  |
| ------------ | ------ | ----- | ----- |
| Set 集合类型 | 400MB  | 12GB  | 144GB |
| Bitmaps      | 12.5MB | 375MB | 4.5GB |



但Bitmaps并不是万金油，假如该网站每天的独立访问用户很少，例如只有10万（大量的僵尸用户），那么两者的对比如下表所示，很显然，这时候使用Bitmaps就不太合适了，因为基本上大部分位都是0。

**Set 和 Bitmaps 存储一天活跃用户对比（独立用户比较少）**

| 数据类型     | 每个 userId 占用空间 | 需要存储的用户量 | 全部内存量 |
| ------------ | -------------------- | ---------------- | ---------- |
| Set 集合类型 | 64位                 | 10 0000          | 800KB      |
| Bitmaps      | 1位                  | 1 0000 0000      | 12.5MB     |



### 2 HyperLogLog

#### 2.1 简介

- 在工作当中，我们经常会遇到与统计相关的功能需求，比如统计网站PV（PageView页面访问量），可以使用Redis的incr、incrby轻松实现。

- 但像UV（UniqueVisitor，独立访客）、独立IP数、搜索记录数等需要**去重和计数**的问题如何解决？

  这种求集合中不重复元素个数的问题称为基数问题。

  解决基数问题有很多种方案：

  1. 数据存储在MySQL表中，使用distinct count 计算不重复个数
  2. 使用Redis提供的hash、set、bitmaps等数据结构来处理

以上的方案结果精确，但随着数据不断增加，导致占用空间越来越大，对于非常大的数据集是不切实际的。

能否能够降低一定的精度来平衡存储空间？Redis推出了HyperLogLog

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

 

什么是基数?

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}，基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。



#### 2.2 命令

##### 2.2.1 pfadd

`pfadd key element [element ...]`

添加指定元素到 HyperLogLog 中

实例：

![image-20220501230539973](02-Redis.assets/image-20220501230539973.png)

将所有元素添加到指定HyperLogLog数据结构中。

如果执行命令后HLL估计的近似基数发生变化，则返回1，否则返回0。



##### 2.2.2 pfcount

`pfcount key [key ...]`

计算HLL的近似基数，可以计算多个HLL，比如用HLL存储每天的UV，计算一周的UV可以使用7天的UV合并计算即可

实例：

![image-20220501231028706](02-Redis.assets/image-20220501231028706.png)



##### 2.2.3 pfmerge

`pfmerge destkey sourcekey [sourcekey ...]`

将一个或多个HLL合并后的结果存储在另一个HLL中，比如每月活跃用户可以使用每天的活跃用户来合并并计算可得

实例：

![image-20220501231539039](02-Redis.assets/image-20220501231539039.png)



### 3 Geospatial

#### 3.1 简介

Redis 3.2 中增加了对GEO类型的支持。

GEO，Geographic，地理信息的缩写。该类型，就是元素的2维坐标，在地图上就是经纬度。

redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见操作。



#### 3.2 命令

##### 3.2.1 geoadd

`geoadd key longitude latitude member [longitude latitude member...] `

添加地理信息（经度、纬度、名称）



实例：

`geoadd china:city 121.47 31.23 shanghai`

`geoadd china:city 106.50 29.53 chongqing 114.05 22.52 shenzhen 116.38 39.90 beijing`

![image-20220502155341978](02-Redis.assets/image-20220502155341978.png)

两极无法直接添加，一般会下载城市数据，直接通过 Java 程序一次性导入。

有效的经度从 -180 度到 180 度。有效的纬度从 -85.05112878 度到 85.05112878 度。

当坐标位置超出指定范围时，该命令将会返回一个错误。

已经添加的数据，是无法再次往里面添加的。



##### 3.2.2 geopos

`geopos key member [member ...]`

获得指定地区的坐标值



实例：

`geopos china:city shanghai`

`geopos china:city beijing`

![image-20220502155528977](02-Redis.assets/image-20220502155528977.png)



##### 3.2.3 geodist

`geodist key member1 member2 [m|km|ft|mi]`

获取两个位置之间的直线距离



实例：

`geodist china:city beijing shanghai`

`geodist china:city beijing chongqing`

![image-20220502155811394](02-Redis.assets/image-20220502155811394.png)



单位：

- m 表示单位为米[默认值]。
- km 表示单位为千米。
- mi 表示单位为英里。
- ft 表示单位为英尺。
- 如果用户没有显式地指定单位参数， 那么 GEODIST 默认使用米作为单位



##### 3.2.4 georadius

`georadius key longitude latitude radius m|km|ft|mi` （经度 纬度 距离 单位）

以给定的经纬度为中心，找出某一半径内的元素



实例：

`georadius china:city 110 30 1000 km`

![image-20220502160156464](02-Redis.assets/image-20220502160156464.png)

------

## 七、Redis_Jedis_测试

### 1 Jedis 所需要的 jar 包

```xml
<dependency>
	<groupId>redis.clients</groupId>
	<artifactId>jedis</artifactId>
	<version>3.2.0</version>
</dependency>
```



### 2 连接 Redis 注意事项

1. 禁用Linux的防火墙：Linux(CentOS7)里执行命令

      `systemctl stop/disable firewalld.service`

2. redis.conf 中注释掉 `bind 127.0.0.1`，然后`protected-mode no`



### 3 Jedis 常用操作

1. 创建无骨架的Maven工程

2. 创建测试程序

```java
package com.xukang.jedis;
import redis.clients.jedis.Jedis;
public class Demo01 {
	public static void main(String[] args) {
		Jedis jedis = new Jedis("192.168.137.3", 6379);
		String pong = jedis.ping();
         // pong = "PONG" 时表示连接成功
		System.out.println("连接成功："+pong);
		jedis.close();
	}
}

```



### 4 测试相关数据类型

#### 4.1 Jedis-API：Key

```java
public class JedisDemo2 {
    public static void main(String[] args) {
        //创建Jedis对象
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        //添加
        jedis.set("k1", "v1");
        jedis.set("k2", "v2");
        jedis.set("k3", "v3");

        Set<String> keys = jedis.keys("*");
        System.out.println(keys.size());//3
        for (String key : keys){
            System.out.println(key);
        }

        //获取
        String k2 = jedis.get("k2");
        System.out.println(k2);//v2

        System.out.println(jedis.exists("k3"));//true
        System.out.println(jedis.ttl("k1"));//-1
    }
}
```



#### 4.2 Jedis-API：String

```java
jedis.mset("str1", "v1", "str2", "v2", "str3", "v3");
System.out.println(jedis.mget("str1", "str2", "str3"));
```



#### 4.3 Jedis-API：List

```java
public class JedisDemo3 {
    public static void main(String[] args) {
        //创建Jedis对象
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        jedis.lpush("key1", "lucy", "mary", "jack");
        List<String> values = jedis.lrange("key1", 0, -1);
        System.out.println(values);//[jack, mary, lucy]
    }
}
```



#### 4.4 Jedis-API：set

```java
public class JedisDemo4 {
    public static void main(String[] args) {
        //创建Jedis对象
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        jedis.sadd("names", "lucy", "jack");
        Set<String> names = jedis.smembers("names");
        System.out.println(names);//[jack, lucy]
    }
}
```



#### 4.5 Jedis-API：hash

```java
public class JedisDemo5 {
    public static void main(String[] args) {
        //创建Jedis对象
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        jedis.hset("user", "age", "20");
        String hget = jedis.hget("user", "age");
        System.out.println(hget);//20

        Map<String, String> map = new HashMap<>();
        map.put("telphone", "13914458899");
        map.put("address", "yancheng");
        map.put("email", "abc@qq.com");
        jedis.hmset("user2", map);
        List<String> res = jedis.hmget("user2", "address", "email");
        for (String ele : res){
            System.out.println(ele);
        }
    }
}
```



#### 4.6 Jedis-API：zset

```java
public class JedisDemo6 {
    public static void main(String[] args) {
        //创建Jedis对象
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        jedis.zadd("china", 100, "sahnghai");
        jedis.zadd("china", 150, "beijing");
        jedis.zadd("china", 200, "nanjing");

        Set<String> china = jedis.zrange("china", 0, -1);
        System.out.println(china);//[sahnghai, beijing, nanjing]
    }
}
```

------

## 八、Redis-Jedis_实例

**完成一个手机验证码功能**

要求：

1. 输入手机号，点击发送后随机生成6位数字码，2分钟有效
2. 输入验证码，点击验证，返回成功或失败
3. 每个手机号每天只能输入3次



![image-20220502170110036](02-Redis.assets/image-20220502170110036.png)



```java
package com.xukang.jedis;

import redis.clients.jedis.Jedis;

import java.util.Random;

public class PhoneCode {
    public static void main(String[] args) {
        //模拟验证码发送
        verifyCode("13914654815");

        //校验
        getRedisCode("13914654815", "606908");


    }

    //1.生成6位数字验证码
    public static String getCode(){
        Random random = new Random();
        StringBuilder sb = new StringBuilder("");
        for (int i = 0; i < 6; i++){
            sb.append(random.nextInt(10));
        }
        return sb.toString();
    }

    //2.每个手机每天只能发送三次，验证码放到redis中，设置过期时间
    public static void verifyCode(String phone){
        //连接redis
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        //拼接key
        //手机发送次数key
        String countKey = "VerifyCode" + phone + ":count";
        //验证码key
        String codeKey = "VerifyCode" + phone + ":code";

        //每个手机每天只能发送三次
        String count = jedis.get(countKey);
        if (count == null){
            //没有发送次数，第一次发送
            //设置发送次数为1
            jedis.setex(countKey, 24*60*60, "1");
        } else if(Integer.parseInt(count) <= 2){
            jedis.incr(codeKey);
        } else if (Integer.parseInt(count) > 2){
            System.out.println("今天发送次数已经超过三次");
            jedis.close();
        }

        //发送的验证码放到redis里面
        String vcode = getCode();
        jedis.setex(codeKey, 120, vcode);
        jedis.close();
    }

    //3.验证码校验
    public static void getRedisCode(String phone, String code){
        //连接redis
        Jedis jedis = new Jedis("192.168.188.100", 6379);

        //验证码key
        String codeKey = "VerifyCode" + phone + ":code";
        //从Redis获取验证码
        String redisCode = jedis.get(codeKey);

        //判断
        if (code.equals(redisCode)){
            System.out.println("成功");
        } else {
            System.out.println("失败");
        }

        jedis.close();
    }
}
```

------

## 九、Redis 与 SpringBoot 整合

**整合步骤：**

1. 新建SpringBoot项目

2. 在 pom.xml 文件中引入 redis 相关依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.7</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.xukang</groupId>
    <artifactId>redis_springboot</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- redis -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <!-- spring2.X集成redis所需common-pool2-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>2.6.2</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.8</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```



3. application.properties 配置 redis 配置

```properties
#Redis服务器地址
spring.redis.host=192.168.188.100
#Redis服务器连接端口
spring.redis.port=6379
#Redis数据库索引（默认为0）
spring.redis.database= 0
#连接超时时间（毫秒）
spring.redis.timeout=1800000
#连接池最大连接数（使用负值表示没有限制）
spring.redis.lettuce.pool.max-active=20
#最大阻塞等待时间(负数表示没限制)
spring.redis.lettuce.pool.max-wait=-1
#连接池中的最大空闲连接
spring.redis.lettuce.pool.max-idle=5
#连接池中的最小空闲连接
spring.redis.lettuce.pool.min-idle=0
```



4. 添加 redis 配置类

```java
package com.xukang.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.RedisSerializationContext;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.time.Duration;

@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
        //key序列化方式
        template.setKeySerializer(redisSerializer);
        //value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
        //value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // 配置序列化（解决乱码的问题）,过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```



5. 测试

```java
package com.xukang.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/redisTest")
public class RedisTestController {
    @Autowired
    private RedisTemplate redisTemplate;

    @GetMapping
    public String testRedis(){
        //设置值到redis
        redisTemplate.opsForValue().set("name", "lucy");
        //从redis获取值
        String name = (String) redisTemplate.opsForValue().get("name");
        return name;
    }
}
```



------

## 十、Redis-事务-锁机制

### 1 Redis 的事务定义

Redis事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。

事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。

Redis事务的主要作用就是**串联多个命令**防止别的命令插队。



### 2 Multi、Exec、discard

从输入Multi命令开始，输入的命令都会依次进入命令队列中，但不会执行；

直到输入Exec后，Redis会将之前的命令队列中的命令依次执行。

组队的过程中可以通过discard来放弃组队。 



![image-20220502180741936](02-Redis.assets/image-20220502180741936.png)

案例：

```shell
#组队成功，提交成功
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 value1
QUEUED
127.0.0.1:6379(TX)> set k2 value2
QUEUED
127.0.0.1:6379(TX)> set k3 value3
QUEUED
127.0.0.1:6379(TX)> exec
1) OK
2) OK
3) OK
```

```shell
#组队中因为discard而放弃组队
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> discard
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> 
```



### 3 事务的错误处理

1. 组队中某个命令出现了报告错误，执行时整个的所有队列都会被取消。

![image-20220502182526687](02-Redis.assets/image-20220502182526687.png)

```shell
#组队阶段报错，提交失败
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set m1 v1
QUEUED
127.0.0.1:6379(TX)> set m2
(error) ERR wrong number of arguments for 'set' command
127.0.0.1:6379(TX)> set m3 v3
QUEUED
127.0.0.1:6379(TX)> exec
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> 
```



2. 如果执行阶段某个命令报出了错误，则只有报错的命令不会被执行，而其他的命令都会执行，不会回滚。

![image-20220502182613476](02-Redis.assets/image-20220502182613476.png)

```shell
#组队成功，提交有成功有失败情况
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set m1 v1
QUEUED
127.0.0.1:6379(TX)> incr m1
QUEUED
127.0.0.1:6379(TX)> set m2 v2
QUEUED
127.0.0.1:6379(TX)> exec
1) OK
2) (error) ERR value is not an integer or out of range
3) OK
127.0.0.1:6379> keys *
1) "m2"
2) "m1"
```



### 4 事务冲突的问题

#### 4.1 例子

为什么要做成事务？

想想一个场景：有很多人有你的账户，同时去参加双十一抢购。



**一个请求想给金额减8000**

**一个请求想给金额减5000**

**一个请求想给金额减1000**

![image-20220502182939150](02-Redis.assets/image-20220502182939150.png)



#### 4.2 悲观锁

![image-20220502183231215](02-Redis.assets/image-20220502183231215.png)

**悲观锁(Pessimistic Lock)**，顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。**传统的关系型数据库里边就用到了很多这种锁机制**，比如**行锁**，**表锁**等，**读锁**，**写锁**等，都是在做操作之前先上锁。



#### 4.3 乐观锁

![image-20220502183511403](02-Redis.assets/image-20220502183511403.png)

**乐观锁(Optimistic Lock)**，顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。**乐观锁适用于多读的应用类型，这样可以提高吞吐量**。Redis就是利用这种 **check-and-set** 机制实现事务的。



#### 4.4 演示乐观锁

`WATCH key [key ...]`

在执行multi之前，先执行`watch key1 [key2]`，可以监视一个(或多个) key ，

如果在事务**执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。**

![image-20220502184517902](02-Redis.assets/image-20220502184517902.png)

![image-20220502184523496](02-Redis.assets/image-20220502184523496.png)



unwatch：

取消 WATCH 命令对所有 key 的监视。

如果在执行 WATCH 命令之后，EXEC 命令或 DISCARD 命令先被执行了的话，那么就不需要再执行UNWATCH 了。



### 5 Redis 事务三特性

1. 单独的隔离操作 

   事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。 

2. 没有隔离级别的概念 

   队列中的命令没有提交之前都不会实际被执行，因为事务提交前任何指令都不会被实际执行

3. 不保证原子性 

   事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚 



------

## 十一、Redis-事务-秒杀案例

### 1 解决计数器和人员记录的事务操作

![image-20220502200104739](02-Redis.assets/image-20220502200104739.png)



### 2 秒杀功能的核心概念代码

index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>iPhone 13 Pro !!!  1元秒杀！！！
</h1>


<form id="msform" action="/doseckill" method="post" enctype="application/x-www-form-urlencoded">
	<input type="hidden" id="prodid" name="prodid" value="0101">
	<input type="button"  id="miaosha_btn" name="seckill_btn" value="秒杀点我"/>
</form>

</body>
<script  type="text/javascript" src="${pageContext.request.contextPath}/script/jquery/jquery-3.6.0.js"></script>
<script  type="text/javascript">
$(function(){
	$("#miaosha_btn").click(function(){	 
		var url=$("#msform").attr("action");
	     $.post(url,$("#msform").serialize(),function(data){
     		if(data=="false"){
    			alert("抢光了" );
    			$("#miaosha_btn").attr("disabled",true);
    		}
		});    
	})
})
</script>
</html>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		  http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
           version="2.5">

    <servlet>
        <description></description>
        <display-name>doseckill</display-name>
        <servlet-name>doseckill</servlet-name>
        <servlet-class>com.xukang.SecKillServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>doseckill</servlet-name>
        <url-pattern>/doseckill</url-pattern>
    </servlet-mapping>
</web-app>
```

SecKillServlet

```java
package com.xukang;

import java.io.IOException;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Random;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.xml.ws.soap.AddressingFeature.Responses;

/**
 * 秒杀案例
 */
public class SecKillServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public SecKillServlet() {
        super();
    }

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String userid = new Random().nextInt(50000) + "";
		String prodid = request.getParameter("prodid");
		
		boolean isSuccess=SecKill_redis.doSecKill(userid,prodid);
		//boolean isSuccess= SecKill_redisByScript.doSecKill(userid, prodid);
		response.getWriter().print(isSuccess);
	}
}
```

SecKill_redis

```java
package com.xukang;

import java.io.IOException;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

import org.apache.commons.pool2.impl.GenericObjectPoolConfig;
import org.slf4j.LoggerFactory;

import ch.qos.logback.core.rolling.helper.IntegerTokenConverter;
import redis.clients.jedis.HostAndPort;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisCluster;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;
import redis.clients.jedis.ShardedJedisPool;
import redis.clients.jedis.Transaction;

public class SecKill_redis {

	//秒杀过程
	public static boolean doSecKill(String uid, String prodid) throws IOException {
		//1 uid和prodid非空判断
		if(uid == null || prodid == null) {
			return false;
		}

		//2 连接redis
		Jedis jedis = new Jedis("192.168.188.100", 6379);

		//3 拼接key
		// 3.1 库存key
		String kcKey = "sk:"+prodid+":qt";
		// 3.2 秒杀成功用户key
		String userKey = "sk:"+prodid+":user";

		//4 获取库存，如果库存null，秒杀还没有开始
		String kc = jedis.get(kcKey);
		if(kc == null) {
			System.out.println("秒杀还没有开始，请等待");
			jedis.close();
			return false;
		}

		// 5 判断用户是否重复秒杀操作
		if(jedis.sismember(userKey, uid)) {
			System.out.println("已经秒杀成功了，不能重复秒杀");
			jedis.close();
			return false;
		}

		//6 判断如果商品数量，库存数量小于1，秒杀结束
		if(Integer.parseInt(kc) <= 0) {
			System.out.println("秒杀已经结束了");
			jedis.close();
			return false;
		}

		//7 秒杀过程
		//7.1 库存-1
		jedis.decr(kcKey);
		//7.2 把秒杀成功用户添加清单里面
		jedis.sadd(userKey,uid);

		System.out.println("秒杀成功了..");
		jedis.close();
		return true;
	}
}
```



### 3 Redis 事务 -- 秒杀并发模拟

使用工具ab模拟测试

CentOS6 默认安装

CentOS7 需要手动安装



#### 3.1 联网安装ab工具

`yum install httpd-tools`

#### 3.2 无网络

1. 进入cd /run/media/root/CentOS 7 x86_64/Packages（路径跟centos6不同）

2. 顺序安装

   apr-1.4.8-3.el7.x86_64.rpm

   apr-util-1.5.2-6.el7.x86_64.rpm

   httpd-tools-2.4.6-67.el7.centos.x86_64.rpm 



#### 3.3 测试及结果

##### 3.3.1 通过ab测试

`vi postfile` 模拟表单提交参数，以&符号结尾；存放当前目录。

内容：`prodid=0101&`



`ab -n 2000 -c 200 -p ~/postfile -T application/x-www-form-urlencoded http://192.168.31.123:8080/doseckill`

```
-n  总请求数
-c  并发数
-p  表单提交文件
-T  context-type
```



##### 3.3.2 超卖问题

![image-20220502203722803](02-Redis.assets/image-20220502203722803.png)

![image-20220502203727639](02-Redis.assets/image-20220502203727639.png)



![image-20220502203829558](02-Redis.assets/image-20220502203829558.png)



##### 3.3.3 利用乐观锁淘汰用户，解决超卖问题。

![image-20220502204942397](02-Redis.assets/image-20220502204942397.png)

```java
package com.xukang;

public class SecKill_redis {
	//秒杀过程
	public static boolean doSecKill(String uid, String prodid) throws IOException {
		//1 uid和prodid非空判断
		if(uid == null || prodid == null) {
			return false;
		}

		//2 连接redis
		Jedis jedis = new Jedis("192.168.188.100", 6379);

		//3 拼接key
		// 3.1 库存key
		String kcKey = "sk:"+prodid+":qt";
		// 3.2 秒杀成功用户key
		String userKey = "sk:"+prodid+":user";

		//监视库存
		jedis.watch(kcKey);

		//4 获取库存，如果库存null，秒杀还没有开始
		String kc = jedis.get(kcKey);
		if(kc == null) {
			System.out.println("秒杀还没有开始，请等待");
			jedis.close();
			return false;
		}

		// 5 判断用户是否重复秒杀操作
		if(jedis.sismember(userKey, uid)) {
			System.out.println("已经秒杀成功了，不能重复秒杀");
			jedis.close();
			return false;
		}

		//6 判断如果商品数量，库存数量小于1，秒杀结束
		if(Integer.parseInt(kc) <= 0) {
			System.out.println("秒杀已经结束了");
			jedis.close();
			return false;
		}

		//7 秒杀过程
		//使用事务
		Transaction multi = jedis.multi();

		//组队操作
		multi.decr(kcKey);
		multi.sadd(userKey, uid);

		//执行
		List<Object> results = multi.exec();

		if(results == null || results.size()==0) {
			System.out.println("秒杀失败了....");
			jedis.close();
			return false;
		}

		//7.1 库存-1
		//jedis.decr(kcKey);
		//7.2 把秒杀成功用户添加清单里面
		//jedis.sadd(userKey,uid);

		System.out.println("秒杀成功了..");
		jedis.close();
		return true;
	}
}
```



#### 3.4 继续增加并发测试

##### 3.4.1 已经秒光，可是还有库存

`ab -n 2000 -c 200 -p ~/postfile -T application/x-www-form-urlencoded http://192.168.31.123:8080/doseckill`

已经秒光，可是还有库存。原因，就是乐观锁导致很多请求都失败。先点的没秒到，后点的可能秒到了。

![image-20220502210535513](02-Redis.assets/image-20220502210535513.png)



##### 3.4.2 连接超时，通过连接池解决

**连接池**:

节省每次连接redis服务带来的消耗，把连接好的实例反复利用。

通过参数管理连接的行为



- 连接池参数
  1. MaxTotal：控制一个pool可分配多少个jedis实例，通过pool.getResource()来获取；如果赋值为-1，则表示不限制；如果pool已经分配了MaxTotal个jedis实例，则此时pool的状态为exhausted。
  2. maxIdle：控制一个pool最多有多少个状态为idle(空闲)的jedis实例；
  3. MaxWaitMillis：表示当borrow一个jedis实例时，最大的等待毫秒数，如果超过等待时间，则直接抛JedisConnectionException；
  4. testOnBorrow：获得一个jedis实例的时候是否检查连接可用性（ping()）；如果为true，则得到的jedis实例均是可用的；



```java
package com.xukang;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

public class JedisPoolUtil {
	private static volatile JedisPool jedisPool = null;

	private JedisPoolUtil() {
	}

	public static JedisPool getJedisPoolInstance() {
		if (null == jedisPool) {
			synchronized (JedisPoolUtil.class) {
				if (null == jedisPool) {
					JedisPoolConfig poolConfig = new JedisPoolConfig();
					poolConfig.setMaxTotal(200);
					poolConfig.setMaxIdle(32);
					poolConfig.setMaxWaitMillis(100*1000);
					poolConfig.setBlockWhenExhausted(true);
					poolConfig.setTestOnBorrow(true);  // ping  PONG
				 
					jedisPool = new JedisPool(poolConfig, "192.168.188.100", 6379, 60000 );
				}
			}
		}
		return jedisPool;
	}

	public static void release(JedisPool jedisPool, Jedis jedis) {
		if (null != jedis) {
			jedisPool.returnResource(jedis);
		}
	}
}
```



```java
public class SecKill_redis {
	//秒杀过程
	public static boolean doSecKill(String uid, String prodid) throws IOException {
		//1 uid和prodid非空判断
		if(uid == null || prodid == null) {
			return false;
		}

		//2 连接redis
		//Jedis jedis = new Jedis("192.168.188.100", 6379);
		//通过连接池得到jedis对象
		JedisPool jedisPoolInstance = JedisPoolUtil.getJedisPoolInstance();
		Jedis jedis = jedisPoolInstance.getResource();

		//3 拼接key
		// 3.1 库存key
		String kcKey = "sk:"+prodid+":qt";
		// 3.2 秒杀成功用户key
		String userKey = "sk:"+prodid+":user";

		//监视库存
		jedis.watch(kcKey);

		//4 获取库存，如果库存null，秒杀还没有开始
		String kc = jedis.get(kcKey);
		if(kc == null) {
			System.out.println("秒杀还没有开始，请等待");
			jedis.close();
			return false;
		}

		// 5 判断用户是否重复秒杀操作
		if(jedis.sismember(userKey, uid)) {
			System.out.println("已经秒杀成功了，不能重复秒杀");
			jedis.close();
			return false;
		}

		//6 判断如果商品数量，库存数量小于1，秒杀结束
		if(Integer.parseInt(kc) <= 0) {
			System.out.println("秒杀已经结束了");
			jedis.close();
			return false;
		}

		//7 秒杀过程
		//使用事务
		Transaction multi = jedis.multi();

		//组队操作
		multi.decr(kcKey);
		multi.sadd(userKey, uid);

		//执行
		List<Object> results = multi.exec();

		if(results == null || results.size()==0) {
			System.out.println("秒杀失败了....");
			jedis.close();
			return false;
		}

		//7.1 库存-1
		//jedis.decr(kcKey);
		//7.2 把秒杀成功用户添加清单里面
		//jedis.sadd(userKey,uid);

		System.out.println("秒杀成功了..");
		jedis.close();
		return true;
	}
}
```



#### 3.5 库存遗留问题

乐观锁造成库存遗留问题：300个并发，只有一个操作完毕后，修改版本号，其余的299个操作执行完毕后发现版本号更新，回滚事务。



##### 3.5.1 LUA脚本

Lua 是一个小巧的[脚本语言](http://baike.baidu.com/item/脚本语言)，Lua脚本可以很容易的被C/C++ 代码调用，也可以反过来调用C/C++ 的函数，Lua并没有提供强大的库，一个完整的Lua解释器不过200k，所以Lua不适合作为开发独立应用程序的语言，而是作为嵌入式脚本语言。

很多应用程序、游戏使用LUA作为自己的嵌入式脚本语言，以此来实现可配置性、可扩展性。

这其中包括魔兽争霸地图、魔兽世界、博德之门、愤怒的小鸟等众多游戏插件或外挂。

https://www.w3cschool.cn/lua/



##### 3.5.2 LUA 脚本在 Redis 中的优势

- 将复杂的或者多步的redis操作，写为一个脚本，一次提交给redis执行，减少反复连接redis的次数。提升性能。
- LUA脚本是类似redis事务，有一定的原子性，不会被其他命令插队，可以完成一些redis事务性的操作。
- 但是注意 redis 的 LUA 脚本功能，只有在Redis 2.6以上的版本才可以使用。
- 利用lua脚本淘汰用户，解决超卖问题。
- redis 2.6版本以后，通过lua脚本解决**争抢问题**，实际上是**redis** **利用其单线程的特性，用任务队列的方式解决多任务并发问题**。

![image-20220503134405529](02-Redis.assets/image-20220503134405529.png)



LUA脚本：

```lua
local userid=KEYS[1]; 
local prodid=KEYS[2];
local qtkey="sk:"..prodid..":qt";
local usersKey="sk:"..prodid..":usr'; 
local userExists=redis.call("sismember",usersKey,userid);
if tonumber(userExists)==1 then 
  return 2;
end
local num= redis.call("get" ,qtkey);
if tonumber(num)<=0 then 
  return 0; 
else 
  redis.call("decr",qtkey);
  redis.call("sadd",usersKey,userid);
end
return 1;
```



```java
package com.xukang;

import java.io.IOException;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

import org.apache.commons.pool2.impl.GenericObjectPoolConfig;
import org.slf4j.LoggerFactory;

import ch.qos.logback.core.joran.conditional.ElseAction;
import redis.clients.jedis.HostAndPort;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisCluster;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;
import redis.clients.jedis.ShardedJedisPool;
import redis.clients.jedis.Transaction;

public class SecKill_redisByScript {
	
	static String secKillScript ="local userid=KEYS[1];\r\n" + 
			"local prodid=KEYS[2];\r\n" + 
			"local qtkey='sk:'..prodid..\":qt\";\r\n" + 
			"local usersKey='sk:'..prodid..\":usr\";\r\n" + 
			"local userExists=redis.call(\"sismember\",usersKey,userid);\r\n" + 
			"if tonumber(userExists)==1 then \r\n" + 
			"   return 2;\r\n" + 
			"end\r\n" + 
			"local num= redis.call(\"get\" ,qtkey);\r\n" + 
			"if tonumber(num)<=0 then \r\n" + 
			"   return 0;\r\n" + 
			"else \r\n" + 
			"   redis.call(\"decr\",qtkey);\r\n" + 
			"   redis.call(\"sadd\",usersKey,userid);\r\n" + 
			"end\r\n" + 
			"return 1" ;
			

	public static boolean doSecKill(String uid,String prodid) throws IOException {

		JedisPool jedispool =  JedisPoolUtil.getJedisPoolInstance();
		Jedis jedis = jedispool.getResource();

		 //String sha1=  .secKillScript;
		String sha1 = jedis.scriptLoad(secKillScript);
		Object result = jedis.evalsha(sha1, 2, uid,prodid);

		String reString=String.valueOf(result);
		if ("0".equals( reString )  ) {
			System.err.println("已抢空！！");
		}else if("1".equals( reString )  )  {
			System.out.println("抢购成功！！！！");
		}else if("2".equals( reString )  )  {
			System.err.println("该用户已抢过！！");
		}else{
			System.err.println("抢购异常！！");
		}
		jedis.close();
		return true;
	}
}
```

------

## 十二、Redis 持久化之 RDB

### 1 简介

1. RDB 是什么？

   在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

2. 备份是如何执行的

   Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到 一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能，如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。**RDB的缺点是最后一次持久化后的数据可能丢失**。

3. Fork

   - Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等） 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程
   - 在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，Linux中引入了“**写时复制技术**”
   - **一般情况父进程和子进程会共用同一段物理内存**，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。



### 2 RDB 持久化流程

![image-20220503141635328](02-Redis.assets/image-20220503141635328.png)



### 3 dump.rdb 文件

在redis.conf中配置文件名称，默认为dump.rdb

![image-20220503142051969](02-Redis.assets/image-20220503142051969.png)

**配置位置**：

rdb文件的保存路径，也可以修改。默认为Redis启动时命令行所在的目录下

可修改为：`dir /myredis/`

![image-20220503142057067](02-Redis.assets/image-20220503142057067.png)



### 4 如何触发RDB快照：保持策略

**配置文件中默认的快照配置**

![image-20220503142422340](02-Redis.assets/image-20220503142422340.png)



**命令 save VS bgsave**

save ：save时只管保存，其它不管，全部阻塞。手动保存。不建议。

bgsave：Redis 会在后台异步进行快照操作，快照同时还可以响应客户端请求。

可以通过 lastsave 命令获取最后一次成功执行快照的时间



**flushall 命令**

执行flushall命令，也会产生dump.rdb文件，但里面是空的，无意义



**save**

格式：save  秒钟  写操作次数

RDB是整个内存的压缩过的Snapshot，RDB的数据结构，可以配置复合的快照触发条件

默认是1分钟内改了1万次，或5分钟内改了10次，或15分钟内改了1次。

禁用

不设置save指令，或者给save传入空字符串



**stop-writes-on-bgsave-error**

当Redis无法写入磁盘的话，直接关掉Redis的写操作。推荐yes。

![image-20220503142929255](02-Redis.assets/image-20220503142929255.png)



**rdbcompression 压缩文件**

对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩。

如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能。推荐yes

![image-20220503143044886](02-Redis.assets/image-20220503143044886.png)



**rdbchecksum 检查完整性**

在存储快照后，还可以让redis使用CRC64算法来进行数据校验，

但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能

推荐yes。

![image-20220503143132744](02-Redis.assets/image-20220503143132744.png)



### 5 RDB 的备份

先通过config get dir 查询rdb文件的目录 

将 *.rdb 的文件拷贝到别的地方



**rdb的恢复**：

1. 关闭Redis
2. 先把备份的文件拷贝到工作目录下 cp dump2.rdb dump.rdb
3. 启动Redis, 备份数据会直接加载



### 6 RDB 的优缺点

优点：

1. 适合大规模的数据恢复
2. 对数据完整性和一致性要求不高更适合使用
3. 节省磁盘空间
4. 恢复速度快

![image-20220503143557053](02-Redis.assets/image-20220503143557053.png)



缺点：

1. Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑
2. 虽然Redis在fork时使用了**写时拷贝技术**，但是如果数据庞大时还是比较消耗性能。
3. 在备份周期在一定间隔时间做一次备份，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。



### 7 如何停止RDB

动态停止RDB：`redis-cli config set save ""#`

save后给空值，表示禁用保存策略



### 8 小总结

- RDB 是一个非常紧凑的文件。
- RDB 在保持RDB文件时父进程唯一要做的就是fork出一个子进程，接下来的工作全部由子进程来做，父进程不需要在做其他IO操作，所以RDB持久化方式可以最大化redis的性能。
- 与 AOF 相比，在恢复大的数据集的时候，RDB 方式会更快一些。



- 数据丢失风险大。
- RDB 需要经常 fork 子进程来保存数据集到硬盘上，当数据集比较大的时候，fork的过程是非常耗时的，可能会导致Redis在一些毫秒级不能相应客户端请求。

------

## 十三、Redis 持久化之 AOF

### 1 简介

1. AOF 是什么？

   以**日志**的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(**读操作不记录**)， **只许追加文件但不可以改写文件**，redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

2. AOF 持久化流程

   1. 客户端的请求写命令会被append追加到AOF缓冲区内；
   2. AOF缓冲区根据AOF持久化策略[always,everysec,no]将操作sync同步到磁盘的AOF文件中；
   3. AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量；
   4. Redis服务重启时，会重新load加载AOF文件中的写操作达到数据恢复的目的；

![image-20220503151627995](02-Redis.assets/image-20220503151627995.png)



### 2 AOF 配置

1. AOF 默认不开启

   可以在redis.conf中配置文件名称，默认为 appendonly.aof

   AOF文件的保存路径，同RDB的路径一致。

   ![image-20220503151745594](02-Redis.assets/image-20220503151745594.png)



2. AOF 和 RDB 同时开启，Redis 听谁的？

   AOF和RDB同时开启，系统默认取AOF的数据（数据不会存在丢失）



3. AOF 启动 / 修复 / 恢复

   - AOF的备份机制和性能虽然和RDB不同，但是备份和恢复的操作同RDB一样，都是拷贝备份文件，需要恢复时再拷贝到Redis工作目录下，启动系统即加载。
   - 正常恢复
     1. 修改默认的 appendonly no，改为yes
     2. 将有数据的aof文件复制一份保存到对应目录(查看目录：`config get dir`)
     3. 恢复：重启redis然后重新加载

   - 异常恢复
     1. 修改默认的appendonly no，改为yes
     2. 如遇到**AOF文件损坏**，通过 `/usr/local/bin/redis-check-aof--fix appendonly.aof`进行恢复
     3. 备份被写坏的AOF文件
     4. 恢复：重启redis，然后重新加载



4. AOF 同步频率设置

   `appendfsync always`

   始终同步，每次Redis的写入都会立刻记入日志；性能较差但数据完整性比较好

   `appendfsync everysec`

   每秒同步，每秒记入日志一次，如果宕机，本秒的数据可能丢失。

   `appendfsync no`

   redis不主动进行同步，把同步时机交给操作系统。



5. Rewrite压缩

   - 是什么？

     AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制，当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集。可以使用命令bgrewriteaof 。

   - 重写原理，如何实现重写

     AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，redis4.0版本后的重写，事实上就是把RDB的快照，以二级制的形式附在新的AOF头部，作为已有的历史数据，替换掉原来的流水账操作。

   - no-appendfsync-on-rewrite

     如果 no-appendfsync-on-rewrite=yes，不写入aof文件只写入缓存，用户请求不会阻塞，但是在这段时间如果宕机会丢失这段时间的缓存数据。（降低数据安全性，提高性能）

     如果 no-appendfsync-on-rewrite=no, 还是会把数据往磁盘里刷，但是遇到重写操作，可能会发生阻塞。（数据安全，但是性能降低）

   - 触发机制，何时重写

     Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发；

     重写虽然可以节约大量磁盘空间，减少恢复时间。但是每次重写还是有一定的负担的，因此设定Redis要满足一定条件才会进行重写；

   - auto-aof-rewrite-percentage

     设置重写的基准值，文件达到100%时开始重写（文件是原来重写后文件的2倍时触发）

     auto-aof-rewrite-min-size

     设置重写的基准值，最小文件64MB。达到这个值开始重写。

   - 例如：文件达到70MB开始重写，降到50MB，下次什么时候开始重写？100MB

   - 系统载入时或者上次重写完毕时，Redis会记录此时AOF大小，设为base_size

     如果Redis的AOF当前大小>= base_size + base_size*100% (默认)且当前大小>=64mb(默认)的情况下，Redis会对AOF进行重写。 

6. 重写流程

   1. bgrewriteaof触发重写，判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。

   2. 主进程fork出子进程执行重写操作，保证主进程不会阻塞。

   3. 子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof_buf缓冲区和aof_rewrite_buf重写缓冲区保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。

   4. 1)子进程写完新的AOF文件后，向主进程发信号，父进程更新统计信息。

      2)主进程把aof_rewrite_buf中的数据写入到新的AOF文件。	

   5. 使用新的AOF文件覆盖旧的AOF文件，完成AOF重写。

   ![image-20220503153258060](02-Redis.assets/image-20220503153258060.png)

   

### 3 AOF优缺点

优点：


![image-20220503153334548](02-Redis.assets/image-20220503153334548.png)

- 备份机制更稳健，丢失数据概率更低。
- 可读的日志文本，通过操作AOF稳健，可以处理误操作。

 

缺点：

- 比起RDB占用更多的磁盘空间。
- 恢复备份速度要慢。
- 每次读写都同步的话，有一定的性能压力。
- 存在个别Bug，造成恢复不能。



### 4 小总结

- AOF 文件是一个只进行追加的日志文件。
- Redis 可以在 AOF 文件体积变得过大时，自动得在后台对 AOF 进行重写。
- AOF 文件有序地保存了对数据库执行的所有写入操作，这些写入操作以 Redis 协议的格式保存，因此AOF文件的内容非常容易被人读懂，对文件经行分析也很轻松。



- 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
- 根据所使用的的 fsync 策略，AOF 的速度可能会慢于 RDB



### 5 大总结

**RDB 和 AOF 用哪个比较好**：

如果对数据不敏感，可以选单独用RDB。

不建议单独用 AOF，因为可能会出现Bug。

如果只是做纯内存缓存，可以都不用。



**官网建议**：

- RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储

- AOF持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以redis协议追加保存每次写的操作到文件末尾

- Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大

- 只做缓存：如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化方式.

- 同时开启两种持久化方式

- 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整

- RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢？ 

- 建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)， 快速重启，而且不会有AOF可能潜在的bug，留着作为一个万一的手段。

- 性能建议：

  因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 900 1这条规则。

  如果使用AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。

  代价，一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。

  只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。

  默认超过原大小100%大小时重写可以改到适当的数值。

------

## 十四、Redis-主从复制

### 1 简介

1. 是什么？

   主机数据更新后根据配置和策略，自动同步到备机的 **master/slaver** 机制，

   Master以写为主，Slave以读为主

2. 优点

   - 读写分离，性能扩展
   - 容灾快速恢复

![image-20220503155243552](02-Redis.assets/image-20220503155243552.png)



### 2 配置主从复制

1. 根目录下创建 myredis 文件夹：`mkdir /myredis`

2. 复制 redis.conf 配置文件到文件夹中：`cp /etc/redis.conf /myredis/redis.conf`

   需关闭AOF

3. 配置一主两从，创建三个配置文件

   `vi redis6379.conf`

   `vi redis6380.conf`

   `vi redis6381.conf`

4. 在三个配置文件中写入内容

   ```
   include /myredis/redis.conf
   pidfile /var/run/redis_6379.pid
   port 6379
   dbfilename dump6379.rdb
   ```

   ```
   include /myredis/redis.conf
   pidfile /var/run/redis_6380.pid
   port 6380
   dbfilename dump6380.rdb
   ```

   ```
   include /myredis/redis.conf
   pidfile /var/run/redis_6381.pid
   port 6381
   dbfilename dump6381.rdb
   ```

   ![image-20220503161041660](02-Redis.assets/image-20220503161041660.png)

5. 启动三台Redis服务器

   ```
   redis-server redis6379.conf 
   redis-server redis6380.conf 
   redis-server redis6381.conf
   ```

   ![image-20220503161427948](02-Redis.assets/image-20220503161427948.png)

6. 查看系统进程，看看三台服务器是否启动

   `ps -ef | grep redis`

   ![image-20220503161847104](02-Redis.assets/image-20220503161847104.png)

7. 查看三台主机运行状况

   `info replication`

   ![image-20220503162440537](02-Redis.assets/image-20220503162440537.png)

   ![image-20220503162450666](02-Redis.assets/image-20220503162450666.png)

   ![image-20220503162459565](02-Redis.assets/image-20220503162459565.png)



8. 配 从库 不配 主库

   `slavof ip port`：成为某个实例的从服务器

   在6380和6381上执行：`slaveof 127.0.0.1 6379`

   ![image-20220503163250000](02-Redis.assets/image-20220503163250000.png)

   ![image-20220503163300816](02-Redis.assets/image-20220503163300816.png)

   ![image-20220503163306473](02-Redis.assets/image-20220503163306473.png)





### 3 一主二仆

1. 主机上写入数据，从机上可以读取数据

![image-20220503163825034](02-Redis.assets/image-20220503163825034.png)

![image-20220503163832491](02-Redis.assets/image-20220503163832491.png)

2. 从机上写数据，报错

![image-20220503163921248](02-Redis.assets/image-20220503163921248.png)

3. 主机挂掉，重启就行，一切如初

   从机重启需重设：slaveof 127.0.0.1 6379

   可以将配置增加到文件中。永久生效。



### 4 薪火相传

上一个Slave可以是下一个slave的Master，Slave同样可以接收其他 slaves的连接和同步请求，那么该slave作为了链条中下一个的master，可以有效减轻master的写压力，去中心化降低风险。

用 `slaveof ip port`

中途变更转向：会清除之前的数据，重新建立拷贝最新的

风险是一旦某个slave宕机，后面的slave都没法备份

主机挂了，从机还是从机，无法写数据了

<img src="02-Redis.assets/image-20220503170347983.png" alt="image-20220503170347983" style="zoom:150%;" />



### 5 反客为主

当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。

用`slaveof no one`将从机变为主机。

![image-20220503170719758](02-Redis.assets/image-20220503170719758.png)

![image-20220503170750362](02-Redis.assets/image-20220503170750362.png)



### 6 复制原理

- Slave 启动成功连接到 master 后会发送一个 sync 命令。
- Master 接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，master 将传送整个数据文件到slave，以完成一次完全同步。
- 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
- 增量复制：Master继续将新的所有收集到的修改命令依次传给slave，完成同步。
- 但是只要是重新连接master，一次完全同步（全量复制）将被自动执行。

![image-20220503165653576](02-Redis.assets/image-20220503165653576.png)



### 7 哨兵模式

1. 什么是哨兵模式？

   **反客为主的自动版**，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库。

   ![image-20220504184331539](02-Redis.assets/image-20220504184331539.png)

2. 配置哨兵模式

   1. 调整为一主二仆模式，6379为主机，6380和6381为从机.

   2. myredis 目录下新建 sentinel.conf 文件，名字绝不能错。

   3. 配置哨兵，在 sentinel.conf 文件中填写内容。

      `sentinel monitor mymaster 127.0.0.1 6379 1`

      其中 mymaster 为监控对象起的服务器名称，1 为至少有多少个哨兵同意迁移的数量。

   4. 启动哨兵

       `/usr/local/bin/redis-sentinel /myredis/sentinel.conf`

      ![image-20220504185949615](02-Redis.assets/image-20220504185949615.png)

   5. 当主机挂掉，从机选举中产生新的主机

      主机挂掉，(大概10秒左右可以看到哨兵窗口日志，切换了新的主机)

      ![image-20220504190702885](02-Redis.assets/image-20220504190702885.png)

      哪个从机会被选举为主机呢？根据优先级别：slave-priority 

      原主机重启后会变为从机。

      ![image-20220504190817753](02-Redis.assets/image-20220504190817753.png)

   6. 复制延迟

      由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。

   7. 故障恢复

      <img src="02-Redis.assets/image-20220504191329662.png" alt="image-20220504191329662" style="zoom:150%;" />

      优先级在redis.conf中默认：slave-priority 100，值越小优先级越高

      偏移量是指获得原主机数据最全的

      每个redis实例启动后都会随机生成一个40位的 runid

   8. 主从复制

      ```java
      private static JedisSentinelPool jedisSentinelPool=null;
      public static Jedis getJedisFromSentinel(){
      	if(jedisSentinelPool==null){
          	Set<String> sentinelSet=new HashSet<>();
          	sentinelSet.add("192.168.188.100:26379");
      
               JedisPoolConfig jedisPoolConfig =new JedisPoolConfig();
               jedisPoolConfig.setMaxTotal(10); //最大可用连接数
      		jedisPoolConfig.setMaxIdle(5); //最大闲置连接数
      		jedisPoolConfig.setMinIdle(5); //最小闲置连接数
      		jedisPoolConfig.setBlockWhenExhausted(true); //连接耗尽是否等待
      		jedisPoolConfig.setMaxWaitMillis(2000); //等待时间
      		jedisPoolConfig.setTestOnBorrow(true); //取连接的时候进行一下测试 ping pong
      
      		jedisSentinelPool= new JedisSentinelPool("mymaster", sentinelSet, jedisPoolConfig);
      		return jedisSentinelPool.getResource();
          }else{
      		return jedisSentinelPool.getResource();
          }
      }
      ```


------

## 十五、Redis-集群

### 1 简介

1. 问题

   容量不够，redis如何进行扩容？

   并发写操作， redis如何分摊？

   另外，主从模式，薪火相传模式，主机宕机，导致ip地址发生变化，应用程序中配置需要修改对应的主机地址、端口等信息。

   之前通过代理主机来解决，但是redis3.0中提供了解决方案。就是**无中心化集群配置**。

2. 什么是集群

   Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。

   Redis 集群通过分区（partition）来提供一定程度的可用性（availability）：即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。



### 2 配置集群

1. 删除持久化数据

   将rdb、aof文件都删除掉。

2. 制作六个实例：6379、6380、6381、6389、6390、6391

3. 基本配置信息

   myredis 目录中编辑 redis6379.conf 文件

   ```
   include /myredis/redis.conf
   pidfile "/var/run/redis_6379.pid"
   port 6379
   dbfilename "dump6379.rdb"
   cluster-enabled yes
   cluster-config-file "nodes-6379.conf"
   cluster-node-timeout 15000
   ```

   cluster-enabled yes  打开集群模式

   cluster-config-file nodes-6379.conf 设定节点配置文件名

   cluster-node-timeout 15000  设定节点失联时间，超过该时间（毫秒），集群自动进行主从切换。

4. 拷贝多个 redis63**.conf 文件 并 编辑内容

   使用查找替换修改另外5个文件：`:%s/6379/6380`

5. 启动六个服务

   ![image-20220504195614644](02-Redis.assets/image-20220504195614644.png)

6. 组合之前，请确保所有redis实例启动后，nodes-xxxx.conf文件都生成正常。

   ![image-20220504195803037](02-Redis.assets/image-20220504195803037.png)

7. 将六个节点合成一个集群

   进入安装目录的src包下：`cd /opt/redis-6.2.1/src/`

   输入命令：`redis-cli --cluster create --cluster-replicas 1 192.168.188.100:6379 192.168.188.100:6380 192.168.188.100:6381 192.168.188.100:6389 192.168.188.100:6390 192.168.188.100:6391`

   此处不要用127.0.0.1， 请用真实IP地址

   --replicas 1：采用最简单的方式配置集群，一台主机，一台从机，正好三组。

   ![image-20220504202126535](02-Redis.assets/image-20220504202126535.png)

   ![image-20220504202131121](02-Redis.assets/image-20220504202131121.png)

8. 普通方式登录

   `redis-cli -p 6379`

   可能直接进入读主机，存储数据时，会出现MOVED重定向操作。所以，应该以集群方式登录。

9. -c 采用集群策略连接，设置数据会自动切换到相应的写主机

   `redis-cli -c -p 6379`

10. 通过`cluster nodes`命令查看集群信息

    ![image-20220504202642763](02-Redis.assets/image-20220504202642763.png)

11. redis cluster 如何分配这六个节点?

    一个集群至少要有三个主节点。

    选项 --cluster-replicas 1 表示我们希望为集群中的每个主节点创建一个从节点。

    分配原则尽量保证每个主数据库运行在不同的IP地址，每个从库和主库不在一个IP地址上。

12. 什么是 slots

    ![image-20220504203218891](02-Redis.assets/image-20220504203218891.png)

    一个 Redis 集群包含 16384 个插槽（hash slot），数据库中的每个键都属于这 16384 个插槽的其中一个。 

    集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。

    集群中的每个节点负责处理一部分插槽。 举个例子， 如果一个集群可以有主节点， 其中：

    节点 A 负责处理 0 号至 5460 号插槽。

    节点 B 负责处理 5461 号至 10922 号插槽。

    节点 C 负责处理 10923 号至 16383 号插槽。



### 3 集群操作

1. 在集群中录入值

   在redis-cli每次录入、查询键值，redis都会计算出该key应该送往的插槽，如果不是该客户端对应服务器的插槽，redis会报错，并告知应前往的redis实例地址和端口。

   redis-cli客户端提供了 –c 参数实现自动重定向。

   如 `redis-cli -c –p 6379` 登入后，再录入、查询键值对可以自动重定向。

   ```
   127.0.0.1:6379> set k1 v1
   -> Redirected to slot [12706] located at 192.168.188.100:6381
   OK
   192.168.188.100:6381> set k2 v2
   -> Redirected to slot [449] located at 192.168.188.100:6379
   OK
   192.168.188.100:6379> set k3 v3
   OK
   ```

   不在一个slot下的键值，是不能使用mget,mset等多键操作。

   ```
   192.168.188.100:6379> mset name lucy age 20 address china
   (error) CROSSSLOT Keys in request don't hash to the same slot
   ```

   可以通过{}来定义组的概念，从而使key中{}内相同内容的键值对放到一个slot中去。

   ```
   192.168.188.100:6379> mset name{user} lucy age{user} 20
   -> Redirected to slot [5474] located at 192.168.188.100:6380
   OK
   192.168.188.100:6380> 
   ```



2. 查询集群中的值

   `cluster getkeysinslot slot count`：返回slot插槽中的键值

   ```
   127.0.0.1:6379> cluster keyslot k1
   (integer) 12706
   127.0.0.1:6379> cluster countkeysinslot 12706
   (integer) 0
   127.0.0.1:6379> cluster countkeysinslot 449
   (integer) 1
   127.0.0.1:6379> cluster getkeysinslot 449 1
   1) "k2"
   127.0.0.1:6379> 
   ```



### 4 恢复故障

如果主节点下线？从节点能否自动升为主节点？注意：**15**秒超时

![image-20220504205141384](02-Redis.assets/image-20220504205141384.png)

主节点恢复后，主从关系会如何？主节点回来变成从机。

![image-20220504205216231](02-Redis.assets/image-20220504205216231.png)

如果所有某一段插槽的主从节点都宕掉，redis服务是否还能继续?

如果某一段插槽的主从都挂掉，而 cluster-require-full-coverage 为 yes ，那么 ，整个集群都挂掉；

如果某一段插槽的主从都挂掉，而 cluster-require-full-coverage 为 no，那么，该插槽数据全都不能使用，也无法存储。

redis.conf 中的参数 cluster-require-full-coverage



### 5 集群的 Jedis 开发

即使连接的不是主机，集群会自动切换主机存储。主机写，从机读。

无中心化主从集群。无论从哪台主机写的数据，其他主机上都能读到数据。

```java
public class JedisClusterTest {
	public static void main(String[] args) { 
		Set<HostAndPort> set = new HashSet<HostAndPort>();
		set.add(new HostAndPort("192.168.188.100", 6379));
		JedisCluster jedisCluster = new JedisCluster(set);
         jedisCluster.set("k1", "v1");
         System.out.println(jedisCluster.get("k1"));
	}
}
```



### 6 集群的优缺点

优点：

1. 实现扩容
2. 分摊压力
3. 无中心配置相对简单



缺点：

1. 多键操作是不被支持的 
2. 多键的Redis事务是不被支持的。lua脚本不被支持
3. 由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。

------

## 十六、Redis-应用问题解决

### 1 缓存穿透

#### 1.1 问题描述

key对应的数据在数据源并不存在，每次针对此key的请求从缓存获取不到，请求都会压到数据源，从而可能压垮数据源。比如用一个不存在的用户id获取用户信息，不论缓存还是数据库都没有，若黑客利用此漏洞进行攻击可能压垮数据库。

![image-20220505155711667](02-Redis.assets/image-20220505155711667.png)



#### 1.2 解决方案

一个一定不存在缓存及查询不到的数据，由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义。

解决方案：

1. **对空值缓存：**如果一个查询返回的数据为空（不管是数据是否不存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟

2. **设置可访问的名单（白名单）：**使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmap里面的id进行比较，如果访问id不在bitmaps里面，进行拦截，不允许访问。

3. **采用布隆过滤器**：(布隆过滤器（Bloom Filter）是1970年由布隆提出的。它实际上是一个很长的二进制向量(位图)和一系列随机映射函数（哈希函数）。布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。)

   将所有可能存在的数据哈希到一个足够大的bitmaps中，一个一定不存在的数据会被这个bitmaps拦截掉，从而避免了对底层存储系统的查询压力。

4. **进行实时监控：**当发现Redis的命中率开始急速降低，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务



### 2 缓存击穿

#### 2.1 问题描述

key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

![image-20220505160448017](02-Redis.assets/image-20220505160448017.png)



#### 2.2 解决方案

key可能会在某些时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题：缓存被“击穿”的问题。

解决问题：

1. **预先设置热门数据**：在redis高峰访问之前，把一些热门数据提前存入到redis里面，加大这些热门数据key的时长
2. **实时调整：**现场监控哪些数据热门，实时调整key的过期时长
3. **使用锁**：
   1. 就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db。
   2. 先使用缓存工具的某些带成功操作返回值的操作（比如Redis的SETNX）去set一个mutex key
   3. 当操作返回成功时，再进行load db的操作，并回设缓存，最后删除mutex key；
   4. 当操作返回失败，证明有线程在load db，当前线程睡眠一段时间再重试整个get缓存的方法。

<img src="02-Redis.assets/image-20220505161043846.png" alt="image-20220505161043846" style="zoom:150%;" />





### 3 缓存雪崩

#### 3.1 问题描述

key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

缓存雪崩与缓存击穿的区别在于缓存雪崩针对很多key缓存，而缓存击穿则是针对某一个key



正常访问：

<img src="02-Redis.assets/image-20220505161336255.png" alt="image-20220505161336255" style="zoom:150%;" />



缓存失效瞬间：

![image-20220505161538372](02-Redis.assets/image-20220505161538372.png)



#### 3.2 解决方案

缓存失效时的雪崩效应对底层系统的冲击非常可怕！

解决方案：

1. **构建多级缓存架构：**nginx缓存 + redis缓存 +其他缓存（ehcache等）

2. **使用锁或队列：**

   用加锁或者队列的方式来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况。

3. **设置过期标志更新缓存：**

   记录缓存数据是否过期（设置提前量），如果过期会触发通知另外的线程在后台去更新实际key的缓存。

4. **将缓存失效时间分散开：**

   比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。



### 4 分布式锁

#### 4.1 问题描述

随着业务发展的需要，原单体单机部署的系统被演化成分布式集群系统后，由于分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效，单纯的Java API并不能提供分布式锁的能力。为了解决这个问题就需要一种跨JVM的互斥机制来控制共享资源的访问，这就是分布式锁要解决的问题！

分布式锁主流的实现方案：

1. 基于数据库实现分布式锁
2. 基于缓存（Redis等）
3. 基于Zookeeper

每一种分布式锁解决方案都有各自的优缺点：

1. 性能：redis最高
2. 可靠性：zookeeper最高

这里，我们就基于redis实现分布式锁。



#### 4.2 解决方案：使用 Redis 实现分布式锁

redis命令：`set sku:1:info "OK" NX PX 10000`：设置key-value，**上锁并设置过期时间**



ex second：设置键的过期时间为 second 秒。 

`set key value ex second` 效果等同于 `setex key second value` 。



px millisecond：设置键的过期时间为 millisecond 毫秒。 

`SET key value PX millisecond` 效果等同于 `PSETEX key millisecond value` 。



NX ：只在键不存在时，才对键进行设置操作。 

`SET key value NX` 效果等同于 `SETNX key value` 。



XX ：只在键已经存在时，才对键进行设置操作。



<img src="02-Redis.assets/image-20220505163507154.png" alt="image-20220505163507154" style="zoom:150%;" />

1. 多个客户端同时获取锁（setnx）
2. 获取成功，执行业务逻辑 {从db获取数据，放入缓存}，执行完成释放锁（del）
3. 其他客户端等待重试



#### 4.3 编写代码

Redis: set num 0

```java
@GetMapping("testLock")
public void testLock(){
    //1获取锁，setnx lock 111
    Boolean lock = redisTemplate.opsForValue().setIfAbsent("lock", "111");
    //2获取锁成功、查询num的值
    if(lock){
        Object value = redisTemplate.opsForValue().get("num");
        //2.1判断num为空return
        if(StringUtils.isEmpty(value)){
            return;
        }
        //2.2有值就转成成int
        int num = Integer.parseInt(value+"");
        //2.3把redis的num加1
        redisTemplate.opsForValue().set("num", ++num);
        //2.4释放锁，del
        redisTemplate.delete("lock");
    }else{
        //3获取锁失败、每隔0.1秒再获取
        try {
            Thread.sleep(100);
            testLock();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

重启，服务集群，通过网关压力测试：

ab -n 1000 -c 100 http://192.168.31.123:8080/test/testLock

![image-20220505164840972](02-Redis.assets/image-20220505164840972.png)

查看redis中num的值：1000



基本实现。

问题：setnx刚好获取到锁，业务逻辑出现异常，导致锁无法释放

解决：设置过期时间，自动释放锁。



#### 4.4 优化代码：设置锁的过期时间

设置过期时间有两种方式：

1. 首先想到通过expire设置过期时间（缺乏原子性：如果在setnx和expire之间出现异常，锁也无法释放）
2. 在set时指定过期时间（推荐）

<img src="02-Redis.assets/image-20220505165053170.png" alt="image-20220505165053170" style="zoom:130%;" />

```java
@GetMapping("testLock")
public void testLock(){
    //1获取锁，setnx lock 111
    Boolean lock = redisTemplate.opsForValue().setIfAbsent("lock", "111", 3, TimeUnit.SECONDS);
    //2获取锁成功、查询num的值
    if(lock){
        Object value = redisTemplate.opsForValue().get("num");
        //2.1判断num为空return
        if(StringUtils.isEmpty(value)){
            return;
        }
        //2.2有值就转成成int
        int num = Integer.parseInt(value+"");
        //2.3把redis的num加1
        redisTemplate.opsForValue().set("num", ++num);
        //2.4释放锁，del
        redisTemplate.delete("lock");
    }else{
        //3获取锁失败、每隔0.1秒再获取
        try {
            Thread.sleep(100);
            testLock();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```



压力测试肯定也没有问题。自行测试

问题：可能会释放其他服务器的锁。

 

场景：如果业务逻辑的执行时间是7s。执行流程如下

1. index1业务逻辑没执行完，3秒后锁被自动释放。
2. index2获取到锁，执行业务逻辑，3秒后锁被自动释放。
3. index3获取到锁，执行业务逻辑
4. index1业务逻辑执行完成，开始调用del释放锁，这时释放的是index3的锁，导致index3的业务只执行1s就被别人释放。

最终等于没锁的情况。

 

解决：setnx获取锁时，设置一个指定的唯一值（例如：uuid）；释放前获取这个值，判断是否自己的锁



#### 4.5 优化代码：UUID防误删

<img src="02-Redis.assets/image-20220505165309861.png" alt="image-20220505165309861" style="zoom:140%;" />



```java
	@GetMapping("testLock")
    public void testLock(){
        String uuid = UUID.randomUUID().toString();
        //1获取锁，setnx lock 111
        Boolean lock = redisTemplate.opsForValue().setIfAbsent("lock", uuid, 3, TimeUnit.SECONDS);
        //2获取锁成功、查询num的值
        if(lock){
            Object value = redisTemplate.opsForValue().get("num");
            //2.1判断num为空return
            if(StringUtils.isEmpty(value)){
                return;
            }
            //2.2有值就转成成int
            int num = Integer.parseInt(value+"");
            //2.3把redis的num加1
            redisTemplate.opsForValue().set("num", ++num);
            //2.4释放锁，del
            String lockUuid = (String) redisTemplate.opsForValue().get("lock");
            if (lockUuid.equals(uuid)){
                redisTemplate.delete("lock");
            }
        }else{
            //3获取锁失败、每隔0.1秒再获取
            try {
                Thread.sleep(100);
                testLock();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
```



问题：删除操作缺乏原子性。

场景：

1. index1执行删除时，查询到的lock值确实和uuid相等

   uuid=v1

   set(lock, uuid);                      

2. index1执行删除前，lock刚好过期时间已到，被redis自动释放

   在redis中没有了lock，没有了锁。

3. index2获取了lock

   index2线程获取到了cpu的资源，开始执行方法

   uuid=v2

   set(lock, uuid)；

4. index1执行删除，此时会把index2的lock删除

   index1 因为已经在方法中了，所以不需要重新上锁。index1有执行的权限。index1已经比较完成了，这个时候，开始执行

删除的index2的锁！



#### 4.6 优化代码：LUA脚本保证删除的原子性

```java
@GetMapping("testLockLua")
public void testLockLua() {
    //1 声明一个uuid ,将做为一个value 放入我们的key所对应的值中
    String uuid = UUID.randomUUID().toString();
    //2 定义一个锁：lua 脚本可以使用同一把锁，来实现删除！
    String skuId = "25"; // 访问skuId 为25号的商品 100008348542
    String locKey = "lock:" + skuId; // 锁住的是每个商品的数据

    // 3 获取锁
    Boolean lock = redisTemplate.opsForValue().setIfAbsent(locKey, uuid, 3, TimeUnit.SECONDS);

    // 第一种： lock 与过期时间中间不写任何的代码。
    // redisTemplate.expire("lock",10, TimeUnit.SECONDS);//设置过期时间
    // 如果true
    if (lock) {
        // 执行的业务逻辑开始
        // 获取缓存中的num 数据
        Object value = redisTemplate.opsForValue().get("num");
        // 如果是空直接返回
        if (StringUtils.isEmpty(value)) {
            return;
        }
        // 不是空 如果说在这出现了异常！ 那么delete 就删除失败！ 也就是说锁永远存在！
        int num = Integer.parseInt(value + "");
        // 使num 每次+1 放入缓存
        redisTemplate.opsForValue().set("num", String.valueOf(++num));
        /*使用lua脚本来锁*/
        // 定义lua 脚本
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
        // 使用redis执行lua执行
        DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
        redisScript.setScriptText(script);
        // 设置一下返回值类型 为Long
        // 因为删除判断的时候，返回的0,给其封装为数据类型。如果不封装那么默认返回String 类型，
        // 那么返回字符串与0 会有发生错误。
        redisScript.setResultType(Long.class);
        // 第一个要是script 脚本 ，第二个需要判断的key，第三个就是key所对应的值。
        redisTemplate.execute(redisScript, Arrays.asList(locKey), uuid);
    } else {
        // 其他线程等待
        try {
            // 睡眠
            Thread.sleep(1000);
            // 睡醒了之后，调用方法。
            testLockLua();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```



Lua脚本详解：

```lua
if redis.call('get', KEYS[1]) == ARGV[1] 
then 
    return redis.call('del', KEYS[1]) 
else 
    return 0 end
```

客户端执行以上的命令：

- 如果服务器返回OK，那么这个客户端获得锁。
- 如果服务器返回null，那么客户端获取锁失败，可以在稍后再重试。

设置的过期时间到达之后，锁将自动释放。

可以通过以下修改，让这个锁的实现更健壮：

- 不使用固定的字符串作为键的值，而是设置一个不可猜测的长期字符串，作为口令串(token)
- 不适应 DEL 命令来释放锁，而是发送一个 Lua 脚本，这个脚本只在客户端传入的值和键的口令串相匹配时，才对键进行删除。

这两个改动可以防止持有过期锁的客户端误删现有锁的情况出现。



![image-20220505173012123](02-Redis.assets/image-20220505173012123.png)



#### 4.7 总结

1. 加锁

   ```java
   // 1. 从redis中获取锁,set k1 v1 px 20000 nx
   String uuid = UUID.randomUUID().toString();
   Boolean lock = this.redisTemplate.opsForValue()
         .setIfAbsent("lock", uuid, 2, TimeUnit.SECONDS);
   ```

2. 使用 Lua 释放锁

   ```java
   // 2. 释放锁 del
   String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
   // 设置lua脚本返回的数据类型
   DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
   // 设置lua脚本返回类型为Long
   redisScript.setResultType(Long.class);
   redisScript.setScriptText(script);
   redisTemplate.execute(redisScript, Arrays.asList("lock"),uuid);
   ```

3. 重试

   ```java
   Thread.sleep(500);
   testLock();
   ```



为了确保分布式锁可用，我们至少要确保锁的实现同时**满足以下四个条件**：

1. 互斥性。在任意时刻，只有一个客户端能持有锁。
2. 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。
3. 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。
4. 加锁和解锁必须具有原子性。

------

## 十七、Redis6 新功能

### 1 ACL

#### 1.1 简介

Redis ACL是Access Control List（访问控制列表）的缩写，该功能允许根据可以执行的命令和可以访问的键来限制某些连接。

在Redis 5版本之前，Redis 安全规则只有密码控制还有通过 rename 来调整高危命令比如 flushdb，KEYS *，shutdown等。Redis 6 则提供ACL的功能对用户进行更细粒度的权限控制 ：

- 接入权限:用户名和密码 
- 可以执行的命令 
- 可以操作的 KEY

参考官网：https://redis.io/topics/acl



#### 1.2 命令

1. 使用 `acl list` 命令展现用户权限列表

   ```
   127.0.0.1:6379> acl list
   1) "user default on nopass ~* &* +@all"
   127.0.0.1:6379> 
   ```

   `user 用户名(default) 是否启用(on/off) 密码(没密码nopass) 可操作的key(~*) 可执行的命令(+@all)`



2. 使用`acl cat`命令

   查看添加权限指令类别

   ![image-20220505175642974](02-Redis.assets/image-20220505175642974.png)

    

   加参数类型名可以查看类型下具体命令

   ![image-20220505175741416](02-Redis.assets/image-20220505175741416.png)



3. 使用`acl whoami`命令查看当前用户

   ![image-20220505175847462](02-Redis.assets/image-20220505175847462.png)



4. 使用`acl setuser`命令创建和编辑用户ACL

1）ACL规则

下面是有效ACL规则的列表。某些规则只是用于激活或删除标志，或对用户ACL执行给定更改的单个单词。其他规则是字符前缀，它们与命令或类别名称、键模式等连接在一起。

![image-20220505180018633](02-Redis.assets/image-20220505180018633.png)



2）通过命令创建新用户默认权限

`acl setuser user1`

![image-20220505180116647](02-Redis.assets/image-20220505180116647.png)

在上面的示例中，我根本没有指定任何规则。如果用户不存在，这将使用just created的默认属性来创建用户。如果用户已经存在，则上面的命令将不执行任何操作。



3）设置有用户名、密码、ACL权限、并启用的用户

`acl setuser user2 on >password ~cached:* +get`

![image-20220505180355622](02-Redis.assets/image-20220505180355622.png)



4)切换用户，验证权限

![image-20220505180533142](02-Redis.assets/image-20220505180533142.png)



### 2 IO 多线程

#### 2.1 简介

Redis6终于支撑多线程了，告别单线程了吗？

IO多线程其实指**客户端交互部分**的**网络IO**交互处理模块**多线程**，而非**执行命令多线程**。Redis6执行命令依然是单线程。



#### 2.2 原理架构

Redis 6 加入多线程，但跟 Memcached 这种从 IO处理到数据访问多线程的实现模式有些差异。Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程。之所以这么设计是不想因为多线程而变得复杂，需要去控制 key、lua、事务，LPUSH/LPOP 等等的并发问题。整体的设计大体如下：

<img src="02-Redis.assets/image-20220505180708547.png" alt="image-20220505180708547" style="zoom:150%;" />



另外，多线程IO默认也是不开启的，需要再配置文件中配置

`io-threads-do-reads yes `

`io-threads 4`



### 3 工具支持 Cluster

之前老版Redis想要搭集群需要单独安装ruby环境，Redis 5 将 redis-trib.rb 的功能集成到 redis-cli 。另外官方 redis-benchmark 工具开始支持 cluster 模式了，通过多线程的方式对多个分片进行压测。

![image-20220505180813282](02-Redis.assets/image-20220505180813282.png)



### 4 Redis 新功能持续关注

Redis6新功能还有：

1. RESP3：新的 Redis 通信协议：优化服务端与客户端之间通信

2. Client side caching客户端缓存：基于 RESP3 协议实现的客户端缓存功能。为了进一步提升缓存的性能，将客户端经常访问的数据cache到客户端。减少TCP网络交互。

3. Proxy集群代理模式：Proxy 功能，让 Cluster 拥有像单实例一样的接入方式，降低大家使用cluster的门槛。不过需要注意的是代理不改变 Cluster 的功能限制，不支持的命令还是不会支持，比如跨 slot 的多Key操作。

4. Modules API

   Redis 6中模块API开发进展非常大，因为Redis Labs为了开发复杂的功能，从一开始就用上Redis模块。Redis可以变成一个框架，利用Modules来构建不同系统，而不需要从头开始写然后还要BSD许可。Redis一开始就是一个向编写各种系统开放的平台。

