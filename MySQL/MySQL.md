# 初识MySQL

> 1.1 基本知识

​	MySQL 是一个开源数据库

​    前世：瑞典MySQL AB公司

​	今生：属于Oracle 旗下

​	体积小，速度快，总体成本低，招人成本低

​	中小型网站，大型网站，集群





 	PS:原始数据库: TXT文本，Excel，Word

> 1.2 为什么学习数据库

1. 岗位需求
2. 现在世界是一个大数据时代，得数据者得天下
3. 存数据
4. 数据库是所有软件体系中最核心的存在 DBA(专门研究数据库)

> 1.3 什么是数据库

​	概念：数据仓库，软件，安装在操作系统之上的

​	作用：存储数据，管理数据

> 1.4 数据库分类

关系型数据库（SQL）

* Excel，MySQL，Sql Server，DB2 ，SQLlite
* 通过表和表之间，行和列之间的关系进行数据的储存

非关系型数据库(NoSQL)

* Redis MongDB
* 非关系型数据库 ， 对象的储存 通过对象的自身的属性来决定

DBMS(数据库管理软件)

* 数据库的管理软件，科学有效的管理我们的数据，维护和获取数据

> 1.5 安装MySQL

 安装建议：

1. 尽量不要用EXE安装
2. 尽可能使用压缩

> 1.6 MySQL 基本命令

* `mysql -u[username] -p[password]`连接数据库
* `UPDATE mysql.user SET  authentication_string = password('[password]') WHERE user = 'root' AND Host = 'localhost;'` 修改数据库密码
* ` SHOW databases; ` 显示数据库虽有的表
* `SHOW tables;` 显示数据库所有的表
* `DESCRIBE [databasename];` 显示数据库表的信息
* `exit`退出连接



​	注意:所有的语句都用 ; 结尾

> 1.7 操作数据库

1. 创建数据库

   ```SQL
   CREATE DATABASE IF NOT EXISTS [dataname] -- 创建数据库
   ```

2. 删除数据库

   ```sql
   DROP DATABASE IF EXISTS [dataname] -- 删除数据库
   ```

3. 使用数据库

   ```sql
   USE [dataname] -- 使用数据库
   ```
   
4. 查看创建SQL

   ```sql
   SHOW CREATE DATABASE [库名]
   SHOW CREATE TABLE [表名]
   ```

> 1.8 数据库类型

* 数值
  1. tinyint 十分小的数据 1个字节
  2. smalint 较小的数据 2个字节
  3. int 标准的整数 4个字节
  4. mediumint 中等大小的数据 3个字节
  5. bigint 较大的数据  8个字节
  6. float 单精度浮点数 4个字节
  7. double 双精度浮点数 8个字节
  8. decimal 字符串形式的浮点数  金融计算的时候 一般使用decimal

* 字符串
  1. char		 比较小的字符串 0-255
  2. varchar  可变字符串 0-65535
  3. tingtext  较小的文本 2^8 - 1
  4. text          文本串    2^16 -1

* 时间日期
  1. date YYYY-MM-DD
  2. time HH:mm:ss
  3. datetime  YYYY-MM-DD HH:mm:ss
  4. timestamp 时间戳 1970-1-1 到现在的毫秒数
  5. year 年份表示

* Null
  1. null 为没有值
  2. 不要使用null进行运算
  
  

> 1.9 数据库的字段属性









> 2.0 表操作

1. 创建表

   ```sql
   -- 注意使用英文括号
   -- AUTO_INCREMENT 表示自增
   -- DEFAULT 设置默认值
   -- PRIMARY KEY 主键
   CREATE TABLE IF NOT EXISTS `[tablename]` (
       `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
       `name` VARCHAR(50) NOT NULL DEFAULT '匿名' COMMENT '姓名',
       `pwd` VARCHAR(20) NOT NULL COMMENT '密码',
        PRIMARY KEY(`id`)
   ) ENGINE=INNODB DEFAULT CHARSET=utf8
   ```

2. 修改表

   ```sql
   ALTER TABLE [oldname] RENAME AS [newname] --修改表名
   ALTER TABLE [tablename] ADD COLUMN [fieldname] [type] [constraint] [comment] --添加字段
   ALTER TABLE [tablename] MODIFY [fieldname] [type] --修改表字段（修改约束）
   ALTER TABLE [tablename] CHANGE [oldfieldname] [newfieldname] [type] --字段重命名
   ALTER TABLE [tablename] DROP [fieldname] --删除字段
   DROP TABLE IF EXISTS [tablename] -- 删除表
   ```

   

> 2.1 数据库引擎

* INNODB 默认使用
  1. 安全性高
  2. 事务处理
  3. 多表用户操作
* MYISAM  早些年使用的
  1. 节约看空间，速度较快

|            | MYISAM | INNODB |
| ---------- | ------ | ------ |
| 事务支持   | 不支持 | 支持   |
| 数据行锁定 | 不支持 | 支持   |
| 外键       | 不支持 | 支持   |
| 全文索引   | 支持   | 不支持 |
| 表空间大小 | 较小   | 较大   |

> 2.2 数据库的得物理存储

所有的数据库文件都在data文件夹下，

本质还是文件储存



MySQL 引擎在物理文件上的区别

* INNODB 在数据表中只有一个 *.frm 文件，以及上级目录下的ibdata1文件
* MYISAM 对应的文件
  1. *.frm   表结构的定义文件
  2.  *.MYD 数据文件(data)
  3. *.MYI   索引文件

> 2.3 设置数据库表的字符集编码

`CHARSET = utf8`

不设置话，会是mysql的默认字符集编码（不支持中文）

在 my.ini 设置中添加配置（但不建议）

`character-set-server=utf8`

> 2.4 DML 语言

数据库的意义：数据储存，数据管理

DML语言：数据库操作语言

1. INSERT

2. UPDATE
3. DELETE

* 添加

  ```sql
  INSERT INTO [tablename](fieild1,fieild2,fieild3...) VALUES (val1,val2,val3...) --插入一条
  INSERT INTO [tablename](fieild1,fieild2,fieild3...) VALUES (val1,val2,val3...),(val1,val2,val3...)... -- 插入多条
  ```

* 修改

  ```sql
  UPDATE [tablename] SET `[fieldname]` = `[val]` WHERE [conditions] --修改操作
  ```

* 删除

  ```sql
  DELETE FROM [tablename] WHERE [conditions] --删除数据
  
  TRUNCATE [tablename] --清空表
  
  /*
  相同点：都能删除数据
  */
  
  /*
  不同点：
  	TRUNCATE 重新设置自增列
  	TRUNCATE 不会影响事务
  */
  
  /*
  了解即可：DELETE删除的问题，重启数据库
   INNODB 自增列会从1开始（存在内存中 断电即失）
   MyISAM 继续上一个自增量的开始 （存在文件中 不会丢失）
  */
  ```



> 2.5 DQL(Data Query Language) 语言 

1. 所有的查询操作都用它 Select
2. 简单查询 复杂的查询它都能做
3. 数据库最核心的语言

4. 使用频率最高



* 查询全部字段

  ```sql
  SELECT * FROM [tablename]
  ```

* 查询指定字段

  ```sql
  SELECT [field],[field] FROM [tablename]
  ```

* 查询指定别名

  ```sql
  SELECT [field] AS [newname],[field] AS [newname] FROM [tablename]
  ```

* 查询去重

  ```sql
  SELECT DISTINCT [field]  FROM [tablename]
  ```

* 查询家条件

  ```sql
  SELECT DISTINCT [field]  FROM [tablename] WHERE [conditions] 
  ```

* 模糊查询

  ```sql
  -- %(代表0到任意一个字符) _(一个字符)
  SELECT * FROM [tablename] WHERE [field] LIKE '刘%' 
  ```

* IN语句

  ```sql 
  SELECT * FROM  [tablename] WHERE [field] IN (a1,a2,a3)
  ```

* NULL语句

  ```sql
  --为NULL查询
  SELECT * FROM  [tablename] WHERE [field] IS NULL
  --不为BULL查询
  SELECT * FROM  [tablename] WHERE [field] IS NOT NULL
  ```

* 联表查询

  ```SQL
  --内联（查询出两表的交集）
  SELECT * FROM [tablename1]
  INNER JOIN [tablename2] ON [tablename1].[field] = [tablename2].[field] 
  
  --右联(以右边的表为主)
  SELECT * FROM [tablename1]
  RIGHT JOIN [tablename2] ON [tablename1].[field] = [tablename2].[field] 
  
  --左联(以左边的表为准)
  SELECT * FROM [tablename1]
  LEFT JOIN [tablename2] ON [tablename1].[field] = [tablename2].[field] 
  
  ```

* 自联查询

  ```sql
  SELECT T1.[field],T2.[field] FROM [tablename] AS T1,[tablename] AS T2 WHERE 1.[field] = T2.[field]
  ```

* 排序

  ```SQL
  -- ASC 为升序  DESC 为降序
  SELECT * FROM  [tablename] ORDER BY [field] [(ASC)/(DESC)]
  ```

* 分页

  ```SQL
  -- 跳过n条 取第m条
  SELECT * FROM  [tablename]  [field] IS NULL ORDER BY [field] [(ASC)/(DESC)] LIMIT n,m
  ```

  



条件：WHERE 字句运算符 

|               操作符 | 含义           | 示例         | 结果  |
| -------------------: | -------------- | ------------ | ----- |
|                    = | 等于           | 5=6          | false |
|               <>或!= | 不等于         | 5!=6,5<>6    | true  |
|                    > | 大于           | 5>6          | false |
|                    < | 小于           | 5<6          | true  |
|                   >= | 大于等于       | 5>=6         | false |
|                   <= | 小于等于       | 5<=6         | true  |
| BETWEEN  ... AND ... | 在某个范围类   | [5,6]        |       |
|                  AND | 连接多个条件   | 5>1 AND 1>2  | false |
|                   OR | 连接多个条件   | 5>1 OR 1>2   | true  |
|              IS NULL | 值为null       |              |       |
|          IS NOT NULL | 值不为null     |              |       |
|                 LIKE | 匹配           |              |       |
|                   IN | 在a1或a2或a3中 | IN(a1,a2,a3) |       |



> 2.6 MySQL 函数

* 数学类

  ```sql
  SELECT ABS(-8) --绝对值
  SELECT CEILING(9.4) --向上取整
  SELECT FLOOR(9.4) --向下取整
  SELECT RAND() --返回一个 0-1 之间的数
  SELECT SING(-1) --判断一个数的符号 0-0  小于0返回-1 大于0返回 1  
  ```

* 字符串函数

  ```sql
  SELECT CHAR_LENGTH('即使再小的帆，也能远航') --字符串的长度
  SELECT CONCAT('我','爱','你们') --拼接字符串
  SELECT INSERT('我爱你',1,1,'恨') --插入/替换字符串
  SELECT LOWER('AD') -- 转小写
  SELECT UPPER('ad') --转大写
  SELECT INSTR('ZXL','X') --返回第一次出现的子串索引
  SELECT REPLACE('坚持就能成功','坚持','努力') --替换字符串
  SELECT SUBSTR('坚持就能成功',4,6) --从第4个开始截取6个 返回截取后的
  SELECT REVERSE('坚持就能成功') --反转字符串
  ```

* 时间

  ```sql
  SELECT CURRRENT_DATE() --获取当前日期
  SELECT CURDATE() --获取当期日期
  SELECT NOW() --获取当前的时间
  SELECT LOCALTIME() --获取本地时间
  SELECT SYSDATE() --获取系统时间
  SELECT YEAR(NOW()) --获取年
  SELECT MONTH(NOW()) --获取月
  SELECT DAY(NOW()) --获取日
  SELECT HOUR(NOW()) --获取小时
  SELECT MINUTE(NOW()) --获取分钟
  SELECT SECOND(NOW()) --获取秒数
  ```

* 系统

  ```sql
  SELECT SYSTEM_USER()
  SELECT USER()
  SELECT VERSION()
  ```

  

