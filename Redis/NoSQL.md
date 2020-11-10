# 什么是NoSQL

1.NoSQL指的不是没有SQL，而是说不仅仅是SQL

2.泛指非关系型数据库

3.数据存储不需要一个固定式格式

4.不需要多余的操作就可以横向扩展

# 为什么要用NoSQL

因为当今时代，互联网用户量大，每天各种数据的爆发式增长，传统的关系型数据库已经无法解决一些问题，尤其是WEB2.0时代，尤其是超大规模的高并发社区，暴露出许多难以克服的问题，NoSQL在当今大数据时代发展十分迅速，是我们当今时代必须要掌握的一个技术

# NoSQL 特点

1.方便扩展

2.大数据量高性能（Redis 一秒写8万次，读11万次，是一种细粒度的缓存，性能比较高）

3.数据库类型是多样的（不需要事先设计数据库，随取随用）

4.键值对储存，列储存，文档储存，图形数据库

5.最终一致性



# NoSQL的四大分类

## 一  K-V键值对

1. Redis
2. Tair
3. memcache

## 二 文档型数据库

​	1. MongoDB

   			2. ConthDB

 ## 三 列储存数据库

1. HBase

   			2. 分布式文件系统

## 四 图数据库

1. Neo4j

   			2. InfoGrid







# 历史发展

一.单机时代

![image-20200830155830851](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830155830851.png)

缺点：

1.

二.缓存时代

缓存解决大部分读的问题

![image-20200830160349065](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830160349065.png)



三.分库分表+水平拆分+数据库集群

数据库本质 : 读和写

以前用MyISAM：表锁，十分影响效率，高并发下会出现严重的锁问题

现在用Innodb：行锁

分库分表：来解决写的压力

![image-20200830161247775](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830161247775.png)

四.当今年代

问题：

1.世界已经发生了大变化，数据爆炸时代

2.关系型数据库不够用，数据多，变化快

3.用户群体大，访问量增大

4.大数据压力下数据库表基本上无法更改

![image-20200830165709546](C:\Users\Z-X-L\AppData\Roaming\Typora\typora-user-images\image-20200830165709546.png)



