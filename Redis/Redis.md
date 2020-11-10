# Redis入门

## Redis 是什么

Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，免费开源，也被人们称为结构化数据库，Redis是很快的，是用C语言单线程实现的，CPU不是Redis性能瓶颈，Resdis的瓶颈是根据机器的内存和网络宽带决定的

核心：Redis是将数据放在内存中的，用单线程操作效率是最高的，多线程存在CPU上下文的切换耗时操作，而对于内存系统来说，是不存在上下文切换，多次读写都是在一个CPU上面的，在内存的情况下这个是最佳方案



PS:Windows在GitHub上下载（停更很久，官方建议使用Linux版本）

![image-20200830192631329](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830192631329.png)



## Redis 能干嘛

1. 内存储存，持久化（RDB,AOF）
2. 效率高，用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器，计数器

## Redis 特性

1. 多样的数据类型
2. 持久化
3. 集群
4. 事务

## Linux 环境安装Redis (远程安装Redis)

1. 下载Redis(http://www.redis.cn/download.html)

2. 使用XShell将文件传出到Linux系统中的/opt目录下(opt目录一般放安装软件)

3. 使用 tar -zxvf  命令解压压缩包

4. 使用命令 yum install gcc-c++ 安装gcc环境(没网络的话需要配置网络)

5. 使用 make DESTDIR=/usr/local/bin install 命令 (如果出现error错误，需要升级gcc版本)

6. 使用 make install 检查安装

7. redis 的默认安装路径 `usr/lcoal/bin`(如下图)

   ![image-20200830202404010](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830202404010.png)

8. 创建自己的配置文件文件夹（有助于以后自己维护）

9. 拷贝 `opt/redis/redis-conf` 到 自己创建的配置文件夹（如我的:myconfig）

10. 配置自己的 redis-config 文件（如绑定端口号，IP地址，是否后台运行）

11. 进入 bin 目录，执行命令 redis-server myconfig/redis.conf  用自己配置好的配置文件启动redis

12. 执行 ps-ef|grep redis 查看redis服务是否启动

    ![image-20200830203131438](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830203131438.png)

13. 执行命令 redis-cli -p 6379 测试redis是否运行正常

14. 关闭redis命令 shurdown 

    ![image-20200830203354003](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830203354003.png)

## Redis 性能测试

```bash
# Host:127.0.0.1 端口:6379 客户端数量:100 并发量：100000  
redis-benchmark -h 127.0.0.1 -p 6379 -c 100 -n 100000
```

![image-20200831212420195](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200831212420195.png)



> 配置详解（Redis.config）

启动的是就需要它，启动的时候需要指定启动配置，

* 单位 大小写不敏感

  ![image-20200920105005117](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200920105005117.png)

* 包含其他配置文件（INCLUDES）

  ![image-20200920105223591](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200920105223591.png)

* 网络
  1. `bind 127.0.0.1` 绑定Ip
  2. `protexted-mode yes` 保护模式
  3. `port 6379` 端口
* 通用
  1. `daemonize yes` 以守护进程启动，默认是No 需要我们改为yes
  2. `pidfile [local]` 如果以后台的一个方式启动 我们就需要制定一个PID文件
  3. `loglevel notice` 日志等级
  4. `logfile [local]` 日志文件地址
  5. `databases [num]` 数据库的数量 默认是16个数据库
  6. `always-show-logo [y/n]` 是否总是显示Logo 
* 快照
  1. `save 900 1` 如果900秒内至少1个key进行了修改整个时候就进行持久化操作
  2. `save 300 10` 如果300秒内至少10个key进行了修改整个时候就进行持久化操作
  3. `save 60 10000` 如果60秒内至少10000个key进行了修改整个时候就进行持久化操作
  4. `stop-writes-on-bgsave-error yes`  持久化出错是否进行正常工作
  5. `rdbcompression yes` 是否压缩rdb文件，需要消耗CPU资源
  6. `rdbchecksum yes` 保存rdb文件时，进行错误的检查校验
  7. `dir ./` rdb 保存目录
* REPLICATION 主从复制
* SECURITY 安全
  1. `requirepass [password]` 配置密码，默认是没有密码的
* CLIENTS 最大客户端数量
  1. `maxclients [num]` 设置客户端数量
* MEMORY MANAGEMENT 内存管理
  1. `maxmemory <bytes>` 设置内存最大大小
  2. `maxmemory-policy [rule]` 设置内存达到上限后处理策略
* APPEND ONLY MODE 模式 aof配置
  1. `appendonly [y/n]` 模式是不开启aof模式的，模式是使用rdb方式持久化的
  2. `appendfilename [filename]` 持久化文件的名字
  3. `appendfsync [rule]` everysec  每秒都同步一下，可能会丢失最后一秒的数据 ；always每次修改都会同步；no 不同步 





> 基本操作

  #### 基本数据类型

* 字符串（string）
  1. value 除了是我们的字符串还可以为数字
  2. 可用来做计数器
  3. 统计多单位的数量
  4. 对象缓存储存
* 列表 （lists）
  1. 可做栈，队列，阻塞队列，消息队列
  2. 实际上是一个链表
  3. 在两边插入或者改动值，效率最高，中间元素，相对来说效率会低一点
* 散列（hashes）
  1. 用户信息保存
  2. 经常变动信息保存
  3. 更加适合储存对象
* 集合（set）
  	1. set的值不能重复
   2. 可用作抽随机
   3. 微博，B站 共同关注，二度好友（六度分割）
* 有序集合（sorted sets）
  1. 排行榜应用实现
  2. 班级成绩排序
  3. 工资排序



#### Redis（string）命令

* `set [key] [val]`  添加内容
* `keys *` 查看所有key
* `exists [key]` 查看Key 是否存在
* `move [key]` 移除key
* `expire [key] [time(S)]` 设置过期时间
* `ttl [key]` 查看key的过期时间
* `type [key]` 查看key的类型
*  `append [key] [val]` 追加新的内容（若Key不存在,就相当于添加内容）
* `strlen [key]` 获取key内容的长度
* `incr [key]` 累加1
*  `decr [key]` 累减1
* `incrby [key] [n]` 累加 n
* `decrby [key] [n]` 累减 n
* `GETRANGE [key] [start] [end]` 截取字符串的值
* `SETRANGE [key] [offset] [val]` 替换字符串
* `SETEX [key] [seconds] [val]` 设置指定过期时间的内容
* `SETNX [key] [val] ` 添加内容 若key存在则返回0  若key不存在则设置成功 (常用于分布式锁)
* `MSET [key] [val] [key1] [val1] ...` 批量添加内容
* `MGET [key] [key2] ...` 批量获取内容
* `MSETNX [key] [key1] [key2]...` 批量添加（是一个原子性操作）
* `GETSET [key] [val]` 如果key不存在值，则返回nil ，如果key存在获取原来的值并设置新的值

#### Redis (list) 命令

* `LPUSH [key] [val]` 将一个值或者多个值放到列表的头部（左）

* `RPUSH [key] [val]` 将一个值放到列表的尾部（右）

* `LRANGE [key] [start] [end] ` 通过区间获取具体的值

* `LPOP [key] ` 弹出值（移除list的第一个值）

* `RPOP [key] `移除list的最后一个值

* `LINDEX [key] [index]` 通过下标获取值

* `LLEN [key]` 查看list长度

* `LREM [key] [count] [val]` 移除list中指定个数的值

* `LTRIM [key] [start] [end]` 截取list指定的长度，这个list也已经改了，截断了只剩下的截取元素

* `RPOPLPUSH [key1] [key2]` 将list最后一个只用到另一个list

* `LSET [key] [index] [val]` 设置list中索引处的值 （若key不存在则报错）

* `LINSERT [key] [BEFORE|AFTER] [pivot] [val]`  将一个值插入到list中某个值的前面或者后面


#### Redis (set) 命令

* `sadd [key] [val]` 添加一个值
* `SREM [key] [val]` 移除某一个元素
* `SMEMBERS [key]` 显示set里面的值
* `SMEMBERS [key] [val]` 查看这个key里面值是否存在
* `SCARD [key]` 获取set中的个数
* `SRANDMEMBER [key] [count]` 随机抽取key中值 count个指定的值
* ` SPOP [key]` 随机取出并删除一个值
* `SMOVE [key1] [key2]` 将key1中的值移动到key2
* `SDIFF [key1] [key2]` 差集
* `SINTER [key1]  [key2]`  并集
* `SUNION [key1] [key2]` 交集

#### Redis （Hash）命令

* `HSET [key] [f] [v]`  添加值
* `HMSET [key1] [val1] [key2] [val2]` 添加多个值
* `HGET [key] [f]` 获取值
* `HGETALL [key]` 获取所有值
* `HDEL [key] [f]` 删除hash指定的key 对应的Value也消失了
* `HLEN [key]` 产看hash表的字段数量
* `HEXISTS [key] [f]` 判断hash中指定的字段是否存在
* `HKEYS [key]` 只获得所有的field
* `HVALS [key]` 获取所有的值
* `HINCRBY [key] [f] [n]` 累计n
* `HSETNX [key] [f] [v]` 如果不存在则可以设置 不存在则不能设置

#### Redis (Zset) 命令

* `ZADD [key] [sort] [v]` 添加一个值 
* `ZADD [key] [sort] [v] [sort1] v1` 添加多个值
* `ZREM [key] [val]` 移除一个值 
* `ZRANGE [key] [start] [end]` 获取所有值
* `ZCARD [key] `  获取元素个数
*  `ZRANGEBYSCORE [key] [min] [max]`  升序排序输出
* `ZRANGEBYSCORE [key] -inf +inf withscores`  升序分数和字段一起输出
* `ZRANGEBYSCORE [key] -inf 2500 withscores`  升序分数(2500以下)和字段一起输出
* `ZREVRANGE [key] [start] [end] ` 降序输出
* `ZREVRANGE [key] [start] [end] withscores`  降序输出分数和字段
* `ZCOUNT [key] [start] [end]` 获取区间的值



####  三种特殊类型

* geospatial 地理位置
  1. 朋友定位
  2. 附近的人
  3. 打车距离
  
* Hyperloglog
  1. UV统计（UV:用户访问次数）
  2. 内存占用固定，2^64不同的元素技术
  
* Bitmaps 位储存（只有 0 和 1）

  1. 统计疫情感染人数
  2. 记录状态(登录，打卡等等状态)

  

#### Redis (geospatial ) 命令

* `GEOADD [key] [lo] [ln] [mem] [lo1] [ln1] [mem1]` 添加经纬度
* `GEOPOS [key] [mem]` 获取经纬度
* `GEODIST [key] [mem1] [mem2] [u]` 算出两地的直线距离 [u] 有 m(米)km(千米)mi(英里)ft(英尺)
* `GEORADIUS [key] [lo] [ln] [radius] [u]  `取出附近的人   [u] 有 m(米)km(千米)mi(英里)ft(英尺)
* `GEORADIUS [key] [lo] [ln] [radius] [u] withdist withcoord count [n]` 取出附近的n个人 
* ` GEORADIUSBYMEMBER [key] [mem] [radius] [u]` 找出[mem]附近的人
* `ZRANGE [key] [start] [end]` 查看信息
* `ZREM [key] [mem]` 移除指定元素

#### Redis (Hyperloglog)  命令

* `PFadd [key] [val1] [val2] [val3] .....` 添加元素
* `PFCOUNT [key] ` 统计key下面元素不重复的个数
* `PFMERGE [newkey] [key1] [key2]` 合并key1和key2的元素并组成newkey 

#### Redis(Bitmaps) 命令

* `SETBIT [key] [offset] [val]` 添加一个数据 
* `GETBIT [key] [offset]` 获取一个数据
* `BITCOUNT [key] ` 统计值为1的数量





#### Redis 其他命令

* Redis 默认 16 个数据库，默认使用的是第 0 个，可以使用 `select` 切换数据库

* `keys *` 查看当前胡数据库所有的key

* 清除当前数据库命令 `flushdb`，清空所有的数据库 `flushall`

* 端口6379来源是因为粉丝效应，可知乎

  





> 事务

Redis 事务本质：一组命令的集合！一个事务中的所有，命令都会被序列化，会按照顺序执行，一次性，顺序性，排他性，没有原子性

Redis 事务没有隔离级别的概念

所有的命令在十五中弘，并没有直接被执行，只有发起执行命令的时候才会执行

Redis 单条命令式保存原子性，但是事务不保证原子性

Redis 事务使用

* `MULTI`开启事务
* 命令插入
* `EXEC` 执行事务

* `DISCARD` 取消事务

编译型异常，事务中所有的命令都不会被执行

运行时异常，如果事务队列中存在语法性，那么执行命令的时候，其他的命令是可以正常执行的，错误命令抛出异常

> 监控 （Watch）

**悲观锁**

* 很悲观，认为什么时候都会出问题，无论做什么都要加锁

**乐观锁**

* 很乐观，认为什么时候都不会出问题，所以不会上锁，更新数据的时候去判断一下，在此期间是否有人修改过数据

* 获取Version
* 更新的时候比较Version

Watch 监控

* `WACTCH [key]` 监视某一个key
* `UNWATCH ` 取消监控

> 持久化

持久化，在规定时间内执行了多少次操作，则会持久化到文件 .rdb.aof

Redis 是内存数据库，断电即时，所以就需要持久化操作

* RDB（Redis Database）

  在指定的时间内将内存的数据集快照写入磁盘，恢复时将快照文件直接读到内存中

  Redis会单独创建一个子进程来进行持久化，会先将数据写入到一个临时文件中，持久化过程结束了，在用这个临时文件替换上次的文件，整个过程时不进行任何IO操作的，祝贺就确保了极高的性能，如果需要进行大规模的数据恢复，且对于回复的完整性不是非常敏感，那RDB方式要比AOF高效，RDB的缺点是最后一次持久化的数据可能丢失。

  * 触发机制
    1. save的规则满足的情况下，会自动触发rdb规则
    2. 执行flushall命令，也会触发我们的rdb规则
    3. 退出redis，也会产生rdb文件
  * 恢复RDB文件
    1. 只要将rdb文件放在我们的redis启动的目录就可以了，redis启动的时候后会自动检查恢复其中的数据
  * 优点
    1. 适合大规模的数据恢复
    2. 对数据完整性要求不高
  * 缺点
    1. 需要一定的时间间隔进行操作
    2. fork进程的时候，会占用一定的内存空间

* AOF （Append Only File）

  我们将所有的操作记录下来

  以日志的形式来记录每个操作，将Redis执行过程中的所有指令记录下来（读操作不记录），之追加文件但不可以修改文件，Redis启动之初会读取该文件重新构建数据，换言之Redis重启的话就更具日志文件的内容将写指令从前到后执行一次完成数据恢复，模式是不开启的

  * 触发机制
    1. 手动开启
  * 恢复RDB文件
    1. redis启动的时候后会自动检查恢复其中的数据
  * 优点
    1. 每一次修改都同步，文件的完整性更好
    2. 每秒一次同步，可以根据具体情况配置规则
  * 缺点
    1. 备份文件较大
    2. 恢复速度也比较慢









































