---
title: MySql
date: 2021-12-01 10:12:33
categories:
  - web
---

# MySql

## 安装

- MySQL 官网下载最新版`https://dev.mysql.com/downloads/installer/`
- 然后在 cmd 里面 cd 到`D:\mysql-8.0.11`安装目录
- 在该文件夹下创建 my.ini 配置文件编辑 my.ini 配置以下基本信息

```
[client]
# 设置mysql客户端默认字符集
default-character-set=utf8

[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=C:\\web\\mysql-8.0.11
# 设置 mysql数据库的数据的存放目录MySQL 8+ 不需要以下配置，系统自己生成即可否则有可能报错
# datadir=C:\\web\\sqldata
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

> MySQL 数据库进行初始化命令操作时提示 my.ini 配置文件错误的解决方法
> 通过修改 MySQL 数据库中 my.ini 配置文件编码格式的方法解决此问题。
> 步骤：用记事本打开 my.ini 文件——》点击“文件”选择“另存为”——》将编码选为 ANSI——》保存覆盖——》重启 MySQL 。

- 以管理员身份打开 cmd 命令行工具切换目录`cd D:\mysql-8.0.11\bin`
- 初始化数据库`mysqld --initialize --console`执行完成后会输出 root 用户的初始默认密码!!!!
- 安装`mysqld install`
- 启动输入以下命令`net start mysql`
- 停止`net stop mysql`
- 登录 MySQL`mysql -u root -p`
- 修改初始密码`ALTER USER USER() IDENTIFIED BY '123456';`

### mysql 数据类型

|                |                                                         |
| -------------- | ------------------------------------------------------- |
| 整数类型       | tinyint\smallint\mediumint\int\bigint                   |
| 浮点小数类型   | float\double                                            |
| 定点小数类型   | decimal                                                 |
| 时间、日期类型 | year\time\date\datetime\timestamp                       |
| 文本字符串     | char\varchar\tinytext\text\mediumtext\longtext\enum\set |
| 二进制字符串   | bit\binary\varbinary\tinyblob\blog\mediumblob\longblob  |

> char(n)和 varchar(n)中括号中 n 代表字符的个数并不代表字节个数比如 CHAR(30)就可以存储 30 个字符。

### mysql 常用语句

> 不区分大小写

| 语句                                                    | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| use xxx;                                                | 选择数据库                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| show databases;                                         | 显示数据库                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| show columns from customers;                            | 获取 customers 表的一个列                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| CREATE DATABASE xxx;                                    | 创建数据库                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| drop database xxxx;                                     | 删除数据库                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| show create database xxxx;                              | 查看数据库信息                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| alter database 库名 选项信息;                           | 修改库信息                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| CREATE TABLE table_name (column_name column_type,....); | 创建表                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| show tables;                                            | 查看所有表                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| show tables from xxx;                                   | 查看所有表                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ALTER TABLE 表名 表的选项                               | 修改表信息                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| RENAME TABLE 原表名 TO 新表名                           | 对表进行重命名                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| RENAME TABLE 原表名 TO 库名.表名                        | 可将表移动到另一个数据库                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ALTER TABLE 表名 操作名                                 | 操作名:`ADD[ COLUMN] 字段名`-- 增加字段;`AFTER 字段名`-- 表示增加在该字段名后面;`FIRST`-- 表示增加在第一个;`ADD PRIMARY KEY(字段名)`-- 创建主键; `ADD UNIQUE [索引名] (字段名)`-- 创建唯一索引;`ADD INDEX [索引名] (字段名)`-- 创建普通索引;`DROP[ COLUMN] 字段名`-- 删除字段;`MODIFY[ COLUMN] 字段名 字段属性`-- 支持对字段属性进行修改不能修改字段名(所有原有属性也需写上);`CHANGE[ COLUMN] 原字段名 新字段名 字段属性`-- 支持对字段名修改;`DROP PRIMARY KEY`-- 删除主键(删除主键前需删除其 AUTO_INCREMENT 属性);`DROP INDEX 索引名`-- 删除索引;`DROP FOREIGN KEY 外键` -- 删除外键; |
| DROP TABLE[ IF EXISTS] 表名                             | 删除表                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| TRUNCATE [TABLE] 表名                                   | 清空表                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

> 对于字段的定义
> 字段名 数据类型 [NOT NULL | NULL] [DEFAULT default_value] [AUTO_INCREMENT] [UNIQUE [KEY] | [PRIMARY] KEY] [COMMENT 'string']
> -- 表选项

    -- 字符集
        CHARSET = charset_name
        如果表没有设定则使用数据库字符集
    -- 存储引擎
        ENGINE = engine_name
        表在管理数据时采用的不同的数据结构结构不同会导致处理方式、提供的特性操作等不同
        常见的引擎InnoDB MyISAM Memory/Heap BDB Merge Example CSV MaxDB Archive
        不同的引擎在保存表的结构和数据时采用不同的方式
        MyISAM表文件含义.frm表定义.MYD表数据.MYI表索引
        InnoDB表文件含义.frm表定义表空间数据和日志文件
        SHOW ENGINES -- 显示存储引擎的状态信息
        SHOW ENGINE 引擎名 {LOGS|STATUS} -- 显示存储引擎的日志或状态信息
    -- 数据文件目录
        DATA DIRECTORY = '目录'
    -- 索引文件目录
        INDEX DIRECTORY = '目录'
    -- 表注释
        COMMENT = 'string'
    -- 分区选项
        PARTITION BY ... (详细见手册)

/_ 列属性列约束 _/

1. 主键

- 能唯一标识记录的字段可以作为主键。
- 一个表只能有一个主键。
- 主键具有唯一性。
- 声明字段时用 primary key 标识。
  也可以在字段列表之后声明

例 create table tab ( id int, stu varchar(10), primary key (id));

- 主键字段的值不能为 null。
- 主键可以由多个字段共同组成。此时需要在字段列表后声明的方法。

例 create table tab ( id int, stu varchar(10), age int, primary key (stu, age));

2. unique 唯一索引唯一约束

使得某字段的值也不能重复。

3. null 约束

   null 不是数据类型是列的一个属性。
   表示当前列是否可以为 null 表示什么都没有。
   null, 允许为空。默认。
   not null, 不允许为空。
   insert into tab values (null, 'val');
   -- 此时表示将第一个字段的值设为 null, 取决于该字段是否允许为 null

4. default 默认值属性

当前字段的默认值。
insert into tab values (default, 'val'); -- 此时表示强制使用默认值。
create table tab ( add_time timestamp default current_timestamp );
-- 表示将当前时间的时间戳设为默认值。
current_date, current_time

5. auto_increment 自动增长约束

自动增长必须为索引主键或 unique

只能存在一个字段为自动增长。

默认为 1 开始自动增长。可以通过表属性 auto_increment = x 进行设置或 alter table tbl auto_increment = x;

6. comment 注释

例：create table tab ( id int ) comment '注释内容';

7. foreign key 外键约束

用于限制主表与从表数据完整性。

alter table t1 add constraint `t1_t2_fk` foreign key (t1_id) references t2(id);
-- 将表 t1 的 t1_id 外键关联到表 t2 的 id 字段。
-- 每个外键都有一个名字可以通过 constraint 指定

存在外键的表称之为从表子表外键指向的表称之为主表父表。

作用保持数据一致性完整性主要目的是控制存储在外键表从表中的数据。

MySQL 中可以对 InnoDB 引擎使用外键约束

语法

foreign key (外键字段 references 主表名 (关联字段) [主表记录删除时的动作] [主表记录更新时的动作]

此时需要检测一个从表的外键需要约束为主表的已存在的值。外键在没有关联的情况下可以设置为 null.前提是该外键列没有 not null。

可以不指定主表记录更改或更新时的动作那么此时主表的操作被拒绝。

如果指定了 on update 或 on delete 在删除或更新时有如下几个操作可以选择

1. cascade 级联操作。主表数据被更新主键值更新从表也被更新外键值更新。主表记录被删除从表相关记录也被删除。

2. set null 设置为 null。主表数据被更新主键值更新从表的外键被设置为 null。主表记录被删除从表相关记录外键被设置成 null。但注意要求该外键列没有 not null 属性约束。

3. restrict 拒绝父表删除和更新。

注意外键只被 InnoDB 存储引擎所支持。其他引擎是不支持的。

### 增删查改

- 创建表

```
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

- 增

```
INSERT [INTO] 表名 [(字段列表)] VALUES (值列表)[, (值列表), ...]
        -- 如果要插入的值列表包含所有字段并且顺序一致，则可以省略字段列表。
        -- 可同时插入多条数据记录！
        REPLACE 与 INSERT 完全一样，可互换。
INSERT [INTO] 表名 SET 字段名=值[, 字段名=值, ...]
```

- 查

```
SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
        -- 可来自多个表的多个字段
        -- 其他子句可以不使用
        -- 字段列表可以用*代替表示所有字段
        你可以使用 LIMIT 属性来设定返回的记录数。
你可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。
```

- 删

```
DELETE FROM 表名[ 删除条件子句]

没有条件子句则会删除全部
```

- 改

```
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]

mysql> SELECT * from runoob_tbl WHERE runoob_id=3;
```

- WHERE 子句

```
   SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

> MySQL 的 WHERE 子句的字符串比较是不区分大小写的。 你可以使用 BINARY 关键字来设定 WHERE 子句的字符串比较是区分大小写的。

`mysql> SELECT * from runoob_tbl WHERE BINARY runoob_author='RUNOOB.COM';`

- LIKE 子句

> LIKE 子句中使用百分号 \%字符来表示任意字符类似于 UNIX 或正则表达式中的星号 \*。如果没有使用百分号 \%, LIKE 子句与等号\= 的效果是一样的。

`mysql> SELECT * from runoob_tbl WHERE runoob_author LIKE '%COM';`

### nodejs 连接数据库

```
client does not support authentication protocol requested by server
//出现连接失败的原因mysql8之前的版本中加密规则是mysql_native_password,而在mysql8之后,加密规则是caching_sha2_password。并提供了两种解决方案
//把用户密码登录的加密规则还原成mysql_native_password这种加密方式
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;这里的password是你正在使用的密码
```

<<<<<<< HEAD
方式1：直接使用数据库提供的SQL语句
=======
### MySQL 分页查询的 5 种方法
>>>>>>> 3c5cfeb51abb6cea4e5e0de711a8fed595408289

方式 1：

<<<<<<< HEAD
适应场景: 适用于数据量较少的情况(元组百/千级)。很简单，该语句的意思就是查询m+n条记录，去掉前m条，返回后n条。无疑该查询能够实现分页，但m越大，查询性能就越低，因为MySQL需要扫描全部m+n条记录。

方式2：基于索引再排序
=======
select \* from table order by id limit m, n;

很简单，该语句的意思就是查询 m+n 条记录，去掉前 m 条，返回后 n 条。无疑该查询能够实现分页，但 m 越大，查询性能就越低，因为 MySQL 需要扫描全部 m+n 条记录。
>>>>>>> 3c5cfeb51abb6cea4e5e0de711a8fed595408289

方式 2：

select \* from table where id > #max_id# order by id limit n;

该查询同样会返回后 n 条记录，却无需像方式 1 扫描前 m 条记录，但必须在每次查询时拿到上一次查询（上一页）的最大 id（或最小 id），是比较常用的方式。

当然该查询的问题也在于我们不一定能拿到这个 id，比如当前在第 3 页，需要查询第 5 页的数据，就不行了。

方式 3：

为了避免方式 2 不能实现的跨页查询，就需要结合方式 1。

性能需要，m 得尽量小。比如当前在第 3 页，需要查询第 5 页，每页 10 条数据，且当前第 3 页的最大 id 为#max_id#，则：

select \* from table where id > #max_id# order by id limit 10, 10;

该方式就部分解决了方式 2 的问题，但如果当前在第 2 页，要查第 1000 页，性能仍然较差。

方式 4：

select \* from table as a inner join (select id from table order by id limit m, n) as b on a.id = b.id order by a.id;

该查询同方式 1 一样，m 的值可能很大，但由于内部的子查询只扫描了 id 字段，而非全表，所以性能要强于方式 1，并且能够解决跨页查询问题。

方式 5：

select \* from table where id > (select id from table order by id limit m, 1) limit n;

该查询同样是通过子查询扫描字段 id，效果同方式 4。但方式 5 的性能会略好于方式 4，因为它不需要进行表的关联，而是一个简单的比较，在不知道上一页最大 id 的情况下，是比较推荐的用法。

## 插入表情

1. MySQL 的版本

utf8mb4 的最低 mysql 版本支持版本为 5.5.3+，若不是，请升级到较新版本。

2. MySQL 驱动

5.1.34 可用,最低不能低于 5.1.13

3. 修改 MySQL 配置文件

修改 mysql 配置文件 my.cnf

my.cnf 一般在 etc/mysql/my.cnf 位置。找到后请在以下三部分里添加如下内容：

```
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'
```

重启 mysql:

```
一、MySQL启动方式
1、使用 service 启动：service mysqld start
2、使用 mysqld 脚本启动：/etc/init.d/mysqld start
3、使用 safe_mysqld 启动：safe_mysqld&
二、MySQL停止
1、使用 service 启动： service mysqld stop
2、使用 mysqld 脚本启动：/etc/init.d/mysqld stop
3、mysqladmin shutdown
三、MySQL重启
1、使用 service 启动：service mysqld restart
2、使用 mysqld 脚本启动：/etc/init.d/mysqld restart
四、强制关闭
以上方法都无效的时候，可以通过强行命令：“killall mysql”来关闭MySQL，但是不建议用这样的方式，
据库服务，有可能导致表损坏……所以自己掂量着用。
Windows下重启MySQL服务,对于没装mysql图形管理端的用户来说启动和停止mysql服务：
…\…\bin>net stop mysql
…\…\bin>net start mysql
```

> 当数据库中涉及编码类型改变的时候，需要统一数据库，表，字段的编码类型，少一个都不行。

 修改数据库字符集：
 `ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;`
 修改表的字符集：
 `ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
 修改字段的字符集：
 `ALTER TABLE table_name CHANGE column_name column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`

4. 查看是否成功修改
   使用`SHOW VARIABLES WHERE Variable_name LIKE 'character_set_%' OR Variable_name LIKE 'collation%';`查看当前编码,结果中的 collation_connection 、collation_database 、collation_server 是什么没关系。但必须保证系统变量 描述

```
character_set_client (客户端来源数据使用的字符集)
character_set_connection (连接层字符集)
character_set_database (当前选中数据库的默认字符集)
character_set_results (查询结果字符集)
character_set_server (默认的内部操作字符集)
```

这几个变量必须是 utf8mb4,才能插入表情。

查看表编码
`mysql> show create table dictionary; # dictionary 是表名`

5. 数据库连接的配置
   数据库连接参数中也要设置为 utf8bm4 格式，否则是无法插入的
   `charset=utf8mb4`

## 多表查询

- 多表查询语法
`select  字段1,字段2... from 表1,表2... [where 条件]`
- 多表链接查询
```
多表连接查询语法(重点)
SELECT 字段列表
    FROM 表1  INNER|LEFT|RIGHT JOIN  表2
ON 表1.字段 = 表2.字段;
```
内连接查询与多表联合查询的效果是一样的,左外连接查询 (左边表中的数据优先全部显示),右外连接查询 (右边表中的数据优先全部显示)

全连接查询(显示左右表中全部数据)
```
//全连接查询：是在内连接的基础上增加 左右两边没有显示的数据
//注意: mysql并不支持全连接 full JOIN 关键字
//注意: 但是mysql 提供了 UNION 关键字.使用 UNION 可以间接实现 full JOIN 功能
select * from student left join sc on student.sid = sc.sid
UNION
select * from student right join sc on student.sid = sc.sid;
/注意: UNION 和 UNION ALL 的区别:UNION 会去掉重复的数据,而 UNION ALL 则直接显示结果/
```
- 三表查询

`select 表1.字段,表2.字段,表3.字段 from 表1 join 表2 on 表1.关联字段 = 表2.关联字段 join 表3 on 表2.关联字段 = 表3.关联字段 where....;`



<<<<<<< HEAD
方式3：基于索引再排序
=======
## 数据库备份与还原
>>>>>>> 3c5cfeb51abb6cea4e5e0de711a8fed595408289

1. 备份原文件
MySQL中的每一个数据库和数据表分别对应文件系统中的目录和其下的文件。在Linux下数据库文件的存放目录一般为/var/lib/mysql。在Windows下这个目录视MySQL的安装路径而定。备份文件前，需要将MySQL服务停止，然后将数据库目录拷贝即可。恢复数据数据库时，需要先创建好一个数据库(不一定同名)，然后将备份出来的文件(注意，不是目录)复制到对应的MySQL数据库目录中。使用这一方法备份和恢复数据库时，需要新旧的MySQL版本一致，否则可能会出现错误。

2. 使用命令

备份数据库：
`mysqldump –user=root –password=root密码 –lock-all-tables 数据库名 > 备份文件.sql`

<<<<<<< HEAD
该方式就部分解决了方式2的问题，但如果当前在第2页，要查第1000页，性能仍然较差。



方式4：

select * from table as a inner join (select id from table order by id limit m, n) as b on a.id = b.id order by a.id;

该查询同方式1一样，m的值可能很大，但由于内部的子查询只扫描了id字段，而非全表，所以性能要强于方式1，并且能够解决跨页查询问题。



方式5：

select * from table where id > (select id from table order by id limit m, 1) limit n;

该查询同样是通过子查询扫描字段id，效果同方式4。但方式5的性能会略好于方式4，因为它不需要进行表的关联，而是一个简单的比较，在不知道上一页最大id的情况下，是比较推荐的用法。

方法6: 基于索引使用prepare
（第一个问号表示pageNum，第二个？表示每页元组数）

—语句样式: MySQL中,可用如下方法:

代码如下:

PREPARE stmt_name FROM SELECT * FROM 表名称 WHERE id_pk > (？* ？) ORDER BY id_pk
ASC LIMIT M
—适应场景: 大数据量。

—原因: 索引扫描,速度会很快. prepare语句又比一般的查询语句快一点。

方法7:利用MySQL支持ORDER操作可以利用索引快速定位部分元组,避免全表扫描
—比如: 读第1000到1019行元组(pk是主键/唯一键)。

代码如下:

—SELECT * FROM your_table WHERE pk>=1000 ORDER BY pk ASC LIMIT 0,20
=======
恢复数据库：
`mysql -u root –password=root密码 数据库名 < 备份文件.sql`
>>>>>>> 3c5cfeb51abb6cea4e5e0de711a8fed595408289
