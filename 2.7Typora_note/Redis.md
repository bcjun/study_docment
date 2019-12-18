## Redis用途

~~~mermaid
graph TD
redis的用途-->缓存热数据;
redis的用途-->计数器;
redis的用途-->充当队列/栈;
redis的用途-->位操作;
redis的用途-->分布式锁与单线程机制;
redis的用途-->最新列表;
redis的用途-->排行榜;
最新列表-->商品浏览记录;
分布式锁与单线程机制-->验证码避免重复发;
排行榜-->新闻最新列表;
位操作-->用户是否在线;
计数器-->用户点击次数;
缓存热数据-->收获地址;
充当队列/栈-->celery/延时队列;
~~~

> **热数据存储**
>
> ~~~
> 
> ~~~
>
> **计数器**
>
> ~~~
> 
> ~~~
>
> **队列/盏**
>
> ~~~
> 
> ~~~
>
> **分布式锁**
>
> > string
> >
> > 60秒内同一手机号只能发送一个验证码
>
> ~~~python
> # 使用管道(也可以不使用)
> pl = conn.pipeline()
> # 设置验证码过期时间为300秒
> pl.setex(mobile,300,SmsCode)
> # 标记60秒,避免60秒重复发送验证码
> pl.setex("sms_mobile%s"%mobile,60,SmsCode)
> # 执行管道
> pl.execute()
> 
> PS:
>     上面只是一部分
>     还有一部分是去判断验证码是否过期,或者六十秒内是否重复发送过短信
> ~~~
>
> **最新列表**
>
> > list
> >
> > 商品浏览记录
>
> ~~~python
> # 连接数据库
> conn = get_redis_connection("history")
> # 使用管道(也可以不使用)
> pl = conn.pipeline()
> # 去重
> pl.lrem('history_%s' % user_id, 0, sku_id)
> # 添加数据
> pl.lpush('history_%s' % user_id,sku_id)
> # 截取前四条数据(截取之外的数据会被删除掉)
> pl.ltrim('history_%s' % user_id, 0, 4)
> # 执行管道
> pl.execute()
> ~~~
>
> **排行榜**
>
> ~~~
> 
> ~~~
>





## Redis介绍



+ **NoSQL跟SQL的区别**

  | NoSQL                         | SQL                         |
  | ----------------------------- | --------------------------- |
  | nosql语法通用性低             | sql语法通用性较高           |
  | 数据之间关系比较复杂          | 数据之间以Key-Value形式存在 |
  | 不支持事务                    | 支持事务                    |
  | 读写速度极快                  | 读写速度较慢                |
  | 数据读取在内存中              | 数据从磁盘中读写            |
  | [redis官网](http://redis.cn/) |                             |

+ redis特点

  + 读写速度快

  + 基于内存

  + 键值形式

  + 支持二进制存储,所以可以存储各种数据

  + 默认有16个库,无需建库

  + redis是单线程的，所以也是线程安全的。

    > ~~~
    > 在Redis中字符串类型的Value最多可以容纳的数据长度是512M。
    > ~~~

+ redis应用场景

  + 用来做缓存(首页)
  + 社交类
  + session共享


## Redis配置

 > ~~~shell
  > # 安装Redis服务器后 将/usr/local/redis/redis.conf 复制到下面的路径
  > # 因为redis默认是从这里加载配置文件的.
  > /etc/redis/redis.conf 
  > ~~~
  >

## Redis服务端和客户端命令

> ~~~shell
> 1.启动redis服务器命令： sudo redis-server 配置文件 (可以不填，默认配置读取/etc/redis/redis.conf)
> 2.连接redis客户端命令： sudo redis-cli -h ip地址 -p 端口   -c:连接集群
> 3.关闭redis服务器:sudo service redis stop
> ~~~

## Redis数据操作

+ **数据类型**

  + string类型
  + hash类型
  + list类型
  + set类型
  + zset类型

+ **redis数据类型用Python表示**

  ~~~python
  # string类型
  {"key":"value"}
  # hash类型
  {"key":[{"field":"value"},{"field":"value"},{"field":"value"}]}
  # list类型
  {"key":["value","value1","value2"]}
  # zset类型
  {"key":{"value","value1","value2"}
  ~~~

  

> 键一定都是字符串,值可以是其他类型,但是最终值也是字符串

| redis库操作          | 含义                                                  | 例子                                                         |          |
| -------------------- | ----------------------------------------------------- | ------------------------------------------------------------ | -------- |
| select               | 选择第几个库                                          | select 1                                                     |          |
| **redis键操作**      | **含义**                                              | **例子**                                                     |          |
| keys                 | 查找键，参数⽀持正则表达式                            | keys pattern例:keys *                                        |          |
| exists               | 判断键是否存在，如果存在返回1，不存在返回0            | exists key                                                   |          |
| type                 | 查看键对应的value的类型                               | type key                                                     |          |
| del                  | 删除键及对应的值                                      | del key1 key2 ...                                            |          |
| expire               | 设置过期时间，以秒为单位                              | expire key seconds                                           |          |
| ttl                  | 查看有效时间，以秒为单位                              | ttl key                                                      |          |
| **String类型操作**   | **含义**                                              | **例子**                                                     |          |
| set                  | (**增**)创建                                          | set key value                                                |          |
| mset                 | (**增**)创建多个                                      | mset key1 value1 key2 value2 ..                              |          |
| setex                | (**增**)创建并设置过期时间                            | setex key seconds value                                      |          |
| append               | (**增**)追加值                                        | append key value                                             |          |
| get                  | (**查**)获取值                                        | get key                                                      |          |
| mget                 | (**查**)获取多个值                                    | mget key1 key2 ....                                          |          |
| del                  | (**删**)删除键                                        | del key                                                      |          |
| **hash类型操作**     | **含义**                                              | **例子**                                                     |          |
| hset                 | (**增**)设置单个属性                                  | hset key field value                                         |          |
| hmset                | (**增**)设置多个属性                                  | hmset key field1 value1 field2 value2 ...                    |          |
| hkeys                | (**查**)获取指定键所有的属性                          | hkeys field                                                  |          |
| hget                 | (**查**)获取某个属性的值                              | hget key field                                               |          |
| hmget                | (**查**)获取多个属性的值                              | hmget key field1 field2 ...                                  |          |
| hvals                | (**查**)获取所有属性的值                              | hvals key                                                    |          |
| hdel                 | (**删**)删除属性的值                                  | hdel key field                                               |          |
| **list类型操作**     | **含义**                                              | **例子**                                                     |          |
| lpush                | (**增**)在左侧插⼊数据                                | lpush key value1 value2 ...                                  |          |
| rpush                | (**增**)在右侧插⼊数据                                | rpush key value1 value2 ...                                  |          |
| linsert              | (**增**)在指定元素的前或后插⼊新元素                  | linsert key before或after 现有元素 新元素                    |          |
| lset                 | (**改**)修改键为key的列表中下标为index的元素值为value | lset key index value                                         |          |
| lrange               | (**查**)返回列表⾥指定范围内的元素                    | lrange key start stop                                        |          |
| lrem                 | (**删**)将列表中前count次出现的值为value的元素移除    | lrem key count value                                         |          |
|                      |                                                       | count > 0: 从头往尾移除 <br />count < 0: 从尾往头移除 count = 0: 移除所有 |          |
| ltrim                | (截取下标0到2数据)                                    | ltrim key 0 2                                                | 浏览记录 |
| **set无序集合操作**  | **含义**                                              | **例子**                                                     |          |
| sadd                 | (**增**)添加元素                                      | sadd key member1 member2 ...                                 |          |
| smembers             | (**查**)返回所有的元素                                | smembers key                                                 |          |
| srem                 | **(删**)删除                                          | srem key                                                     |          |
| **zset有序集合操作** | **含义**                                              | **例子**                                                     |          |
| zadd                 | (**增**)添加                                          | zadd key score1 member1 score2 member2 ...                   |          |
| zrange               | (**查**) 返回指定范围内的元素                         | zrange key start stop                                        |          |
| zincrby              |                                                       |                                                              |          |
| zscore               | 返回成员member的score值                               | zscore key member                                            |          |
| zrem                 | 删除指定元素                                          | zrem key member1 member2 ...                                 |          |
| zremrangebyscore     | 删除集合'a4'中权限在5、6之间的元素                    | zremrangebyscore a4 5 6                                      |          |



## 与Python交互

+ **Redis交互基本操作**

~~~python
# 导入模块
from redis import StrictRedis
# 创建连接对象
sr = StrictRedis(host='localhost', port=6379, db=0，decode_responses=True) #redis默认是bytes类型。decode_response=True 使得返回的数据解码成字符串
# 增加字符串
result=sr.set('name','itheima')

~~~

+ **语法说明**

~~~python
# 所有的操作都一样,除了删除,删除在终端是del 但是在交互时需要写成delete
# 查询字符串
result = sr.get('name')
# 修改
result = sr.set('name','itcast')
#获取所有的键
result=sr.keys()

~~~



## 主从搭建

+ **作用**

  > 进行读写分离
  >
  > 网站的读写比率是10:1

+ **主从配置的关键**

  > 修改etc/redis/redis.conf文件

+ **主从配置搭建步骤**

  + 1.先停掉redis-server

    > ~~~shell
    > # 开机自启动
    > sudo service redis stop
    > # 非开机自启动的可以kill掉服务
    > ps -aux | grep redis #配合 sudo kill -9 进程id
    > ~~~

  + 2.修改配置文件

    > ~~~shell
    > #复制redis的配置文件并修改成从服务器配置文件
    > sudo vim slave.conf
    > # 在文件里主要修改
    > # 1.bind 从属数据库ip地址 
    > # 2.port 从属数据库端口号
    > # 3.slaveof 主数据库ip地址 主数据库端口号
    > ~~~

  + 3.重启redis服务

    > ~~~shell
    > #启动主服务器
    > sudo redis-server redis.conf
    > #启动从服务器
    > sudo redis-server redis_slave.conf
    > # 连接主服务器客户端
    > redis-cli -h 192.168.26.128 -p 6379
    > # 连接从服务器客户端
    > redis-cli -h 从服务器IP -p 从服务器端口号
    > ~~~

  + 4.查看是否配置成功

    > ~~~shell
    > redis-cli -h 主ip地址 info Replication
    > ~~~

+ **[redis.conf常用配置信息](https://blog.csdn.net/ljphilp/article/details/52934933)**

  - 绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip，

    > bind 127.0.0.1

  - 端⼝，默认为6379

    > port 6379

  - 是否以守护进程运⾏

    > daemonize yes

  - 数据⽂件, RDB

    > dbfilename dump.rdb

  - 数据⽂件存储路径

    > dir /var/lib/redis

  - ⽇志⽂件

    > logfile "/var/log/redis/redis-server.log"

  - 数据库，默认有16个

    > database 16

  - 主从复制，类似于双机备份。

    > slaveof



## [搭建集群]( http://www.cnblogs.com/wuxl360/p/5920330.html)

+ **集群的有点**

  + 可以对读写数据进行负载均衡

  + 集群之间又彼此相互独立,课提高数据安全性

  + 一个集群至少需要六台服务器

    ~~~
    默认配置三台主服务器三台从服务器
    ~~~

    

+ **集群搭建**

  > ~~~shell
  > # 跟主从搭建一样,需要先修改配置文件
  > # redis的安装包中包含了redis-trib.rb，⽤于创建集群
  > # 将该文件复制到/usr/local/bin/下  这样可以在任何⽬录下调⽤此命令
  > sudo cp /usr/share/doc/redis-tools/examples/redis-trib.rb /usr/local/bin/
  > ~~~

+ 修改配置文件

  > ~~~shell
  > port 7003			# 如果端口被占用改成其他的
  > bind 172.16.179.131  # 需要修改成本地IP
  > daemonize yes
  > pidfile 7003.pid	# 改成跟端口号同名
  > cluster-enabled yes
  > cluster-config-file 7003_node.conf	# 改成跟端口号一样
  > cluster-node-timeout 15000
  > appendonly yes
  > ~~~

+ 启动

  ~~~
  redis-server 7000.conf
  redis-server 7001.conf
  redis-server 7002.conf
  redis-server 7003.conf
  redis-server 7004.conf
  redis-server 7005.conf
  ~~~

+ 复制集群文件目录

  ~~~shell
  # 复制到usr/local/bin/下 这样可以在任何⽬录下调⽤此命令
  sudo cp /usr/share/doc/redis-tools/examples/redis-trib.rb /usr/local/bin/
  ~~~

+ 安装ruby环境，因为redis-trib.rb是⽤ruby开发的

  ~~~
  sudo apt-get install ruby
  PS:
  	 源的地址有可能变动了，尾缀不是.org而是.com了，所以要执行一下命令。
  	 gem sources --add https://gems.ruby-china.com --remove https://rubygems.org/
  ~~~

+ 运⾏命令创建集群

  ~~~shell
  redis-trib.rb create --replicas 1 172.16.179.130:7000 172.16.179.130:7001 172.16.179.130:7002 172.16.179.131:7003 172.16.179.131:7004 172.16.179.131:7005
  # 执⾏这个指令报错,主要原因是由于安装的 ruby 不是最 新版本!
  # 注意要修改IP和端口号
  ~~~

+ redis客户端登录服务器

  ~~~shell
  redis-cli -h 172.16.179.131 -c -p 7002
  # redis登录服务器后,具体写入数据到那个服务器不是由用户决定的,而且是由CRC16算法决定的
  ~~~

+ 拓展

  > - redis cluster在设计的时候，就考虑到了去中⼼化，去中间件，也就是说，集群中 的每个节点都是平等的关系，都是对等的，每个节点都保存各⾃的数据和整个集 群的状态。每个节点都和其他所有节点连接，⽽且这些连接保持活跃，这样就保 证了我们只需要连接集群中的任意⼀个节点，就可以获取到其他节点的数据
  > - Redis集群没有并使⽤传统的⼀致性哈希来分配数据，⽽是采⽤另外⼀种叫做哈希 槽 (hash slot)的⽅式来分配的。redis cluster 默认分配了 16384 个slot，当我们 set⼀个key 时，会⽤CRC16算法来取模得到所属的slot，然后将这个key 分到哈 希槽区间的节点上，具体算法就是：CRC16(key) % 16384。所以我们在测试的 时候看到set 和 get 的时候，直接跳转到了7000端⼝的节点
  > - Redis 集群会把数据存在⼀个 master 节点，然后在这个 master 和其对应的salve 之间进⾏数据同步。当读取数据时，也根据⼀致性哈希算法到对应的 master 节 点获取数据。只有当⼀个master 挂掉之后，才会启动⼀个对应的 salve 节点，充 当 master
  > - 需要注意的是：必须要3个或以上的主节点，否则在创建集群时会失败，并且当存 活的主节点数⼩于总节点数的⼀半时，整个集群就⽆法提供服务了

  

## Redis集群与Python交互

+ 安装包如下

  > pip install redis-py-cluster

+ **Redis集群与Python交互基本操作**

~~~Python
from rediscluster import *
# 构建所有的节点，Redis会使⽤CRC16算法，将键和值写到某个节点上 
startup_nodes = [
    {'host': '192.168.26.128', 'port': '7000'},
    {'host': '192.168.26.130', 'port': '7001'},
    {'host': '192.168.26.128', 'port': '7002'},
]
# 构建StrictRedisCluster对象
src=StrictRedisCluster(startup_nodes=startup_nodes,decode_responses=True)
# 设置键为name、值为itheima的数据
result=src.set('name','itheima')
~~~

## Redis事务

> ~~~
> Redis提供了一定的事务支持，可以保证一组操作原子执行不被打断，但是如果执行中出现错误，事务不能回滚，Redis未提供回滚支持。
> 
> multi 开启事务
> exec 执行事务
> ~~~
>
> **shell执行**
>
> ~~~
> 127.0.0.1:6379> multi
> OK
> 127.0.0.1:6379> set c 300
> QUEUED
> 127.0.0.1:6379> hgetall a
> QUEUED
> 127.0.0.1:6379> set d 400
> QUEUED
> 127.0.0.1:6379> get d
> QUEUED
> 127.0.0.1:6379> exec
> 1) OK
> 2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
> 3) OK
> 4) "400"
> 127.0.0.1:6379>
> 如果事务中出现了错误，事务并不会终止执行，而是只会记录下这条错误的信息，并继续执行后面的指令。所以事务中出错不会影响后续指令的执行。
> ~~~
>
> #### Python客户端操作
>
> ~~~
> from redis import StrictRedis
> 
> # 1.创建redis客户端对象
> # decode_responses=True 将返回的bytes类型数据转换成字符串
> redis_client = StrictRedis(port=6381, decode_responses=True)
> 
> # 2.通过客户端对象构建管道对象
> pipeline = redis_client.pipeline()
> 
> # 3.通过管道对象设置数据
> pipeline.set("name", "kobe")
> pipeline.get("name")
> 
> # 4.执行管道中存储的命令，获取结果
> ret = pipeline.execute()
> 
> print(ret)
> ~~~
>
> **Redis Cluster 集群不支持事务**

## Redis持久化

**RDB 快照持久化**

> redis可以将内存中的数据写入磁盘进行持久化。在进行持久化时，redis会创建子进程来执行
>
> **redis默认开启了快照持久化机制。**
>
> ~~~bash
>  # redis的配置文件
>  触发快照的三种条件
>  save 900 1		900秒内，如果修改了一个键，则拍快照
>  save 300 10	300秒内，如果修改了10个键，则拍快照
>  save 60 10000	60秒内，如果修改了10000个键，则拍快照
> ~~~
>
> **其他**
>
> ~~~
> BGSAVE
> 
> 执行BGSAVE命令，手动触发RDB持久化
> 
> SHUTDOWN
> 
> 关闭redis时触发
> 
> redis 默认快照存储位置为当前目录的dump.rdb文件中,通过dir指定目录,通过dbfilename指定文件名。
> ~~~

**AOF 追加文件持久化**

> redis可以将执行的所有指令追加记录到文件中持久化存储，这是redis的另一种持久化机制。
>
> **redis默认未开启AOF机制。**
>
> ```shell
> # 在redis.conf中修改
> appendonly yes  # 是否开启AOF
> appendfilename "appendonly.aof"  # AOF文件
> ```
>
> AOF机制记录操作的时机
>
> ```shell
> # appendfsync always  # 每个操作都写到磁盘中
> appendfsync everysec  # 每秒写一次磁盘，默认
> # appendfsync no  # 由操作系统决定写入磁盘的时机
> ```
>
> 使用AOF机制的缺点是随着时间的流逝，AOF文件会变得很大。但redis可以压缩AOF文件。
>
> #### 结合使用
>
> redis允许我们同时使用两种机制，通常情况下我们会设置AOF机制为everysec 每秒写入，则最坏仅会丢失一秒内的数据。

## 哨兵

> 哨兵程序是redis中自带的
>
> 只有集群中默认是开启了哨兵机制的
>
> 哨兵跟redis—server是相互独立的。既一主一从也可以创建三个哨兵。
>
> ~~~
> # 在redis安装后，会自带sentinel哨兵程序，修改sentinel.conf配置文件
> 
> bind 127.0.0.1
> port 26380
> daemonize yes
> logfile /var/log/redis-sentinel.log
> sentinel monitor mymaster 127.0.0.1 6380 2
> sentinel down-after-milliseconds mymaster 30000
> sentinel parallel-syncs mymaster 1
> sentinel failover-timeout mymaster 180000
> ~~~
>
> sentinel monitor mymaster 127.0.0.1 6380 2 说明
>
> - mymaster 为sentinel监护的redis主从集群起名
> - 127.0.0.1 6300 为主从中任一台机器地址
> - 2 表示有两台以的sentinel认为某一台redis宕机后，才会进行自动故障转移。
>
> **启动方式**
>
> ~~~
> redis-sentinel sentinel.conf
> 至少三个sentinel以上
> sentinel要分散运行在不同的机器上
> ~~~
>
> 

**Python客户端使用哨兵**

> ~~~
> # redis 哨兵
> REDIS_SENTINELS = [
>     ('127.0.0.1', '26380'),
>     ('127.0.0.1', '26381'),
>     ('127.0.0.1', '26382'),
> ]
> REDIS_SENTINEL_SERVICE_NAME = 'mymaster'
> 
> from redis.sentinel import Sentinel
> _sentinel = Sentinel(REDIS_SENTINELS)
> redis_master = _sentinel.master_for(REDIS_SENTINEL_SERVICE_NAME)
> redis_slave = _sentinel.slave_for(REDIS_SENTINEL_SERVICE_NAME)
> ~~~
>
> **示例**
>
> ~~~
> # 读数据，master读不到去slave读
> try:
>     real_code = redis_master.get(key)
> except ConnectionError as e:
>     real_code = redis_slave.get(key)
> 
> # 写数据，只能在master里写
> try:
>     current_app.redis_master.delete(key)
> except ConnectionError as e:
>     logger.error(e)
> ~~~

