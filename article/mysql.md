---
title: MySql
date: 2021-12-01 10:12:33
categories:
  - database
tags:
  - 数据库
---

# MySql

> 关系型数据库：建立在关系模型上，由多张相互连接的二维表组成的数据库

## 安装

- MySQL 官网下载最新版<https://dev.mysql.com/downloads/installer/>
- 然后在 cmd 里面 cd 到`D:\mysql-8.0.11`安装目录
- 在该文件夹下创建 my.ini 配置文件编辑 my.ini 配置以下基本信息

```ini
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

1. 以管理员身份打开 cmd 命令行工具切换目录`cd D:\mysql-8.0.11\bin`
2. 初始化数据库`mysqld --initialize --console`执行完成后会输出 root 用户的初始默认密码!!!!
3. 安装`mysqld install`
4. 启动输入以下命令`net start mysql`
5. 停止`net stop mysql`
6. 登录 MySQL`mysql -u root -p`
7. 修改初始密码`ALTER USER USER() IDENTIFIED BY '123456';`

## mysql 数据类型

| 类型           | 名称                                                    |
| -------------- | ------------------------------------------------------- |
| 整数类型       | tinyint\smallint\mediumint\int\bigint                   |
| 浮点小数类型   | float\double                                            |
| 定点小数类型   | decimal                                                 |
| 时间、日期类型 | year\time\date\datetime\timestamp                       |
| 文本字符串     | char\varchar\tinytext\text\mediumtext\longtext\enum\set |
| 二进制字符串   | bit\binary\varbinary\tinyblob\blog\mediumblob\longblob  |

> char(n)和 varchar(n)中括号中 n 代表字符的个数并不代表字节个数比如 CHAR(30)就可以存储 30 个字符。

## sql 语句

语法:

- 单行或多行书写，已分号结尾
- 可使用空格缩进
- 不区分大小写，关键词推荐大写
- 注释：两横杆加空格`-- 注释...`或`/* ... */`或 mysql 特有的`#`

分类：

- DDL（data definition language）数据定义语句，建表、字段、数据库
- DML（data manipulation language）数据操作语句，增删改
- DQL（data query language）数据查询语句
- DCL（data control language）数据控制语句，创建数据库用户，权限访问控制

## sql 常用语句

- DDL
  | 语句 | 说明 |
  | -| - |
  | show databases; | 显示数据库 |
  | show create database 数据库名; | 查看数据库信息 |
  | select databases(); | 查询当前数据库 |
  | use 数据库名; | 选择数据库 |
  | create database [ IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]; | 创建数据库 |
  | drop database [ IF EXISTS] 数据库名; | 删除数据库 |
  | alter database 库名 选项信息; | 修改库信息 |
  | show tables; | 查看所有表 |
  | show tables from 数据库名; | 查看所有表 |
  | show create table 表名; | 查询指定表建表语句 |
  | desc 表名; | 表结构 |
  | show 列 from 表名; | 获取表的一个列 |
  | create table 表名(字段名 字段类型[COMMENT 注释],....)\[COMMENT 注释];| 创建表，最后一个字段后面没有逗号 |
  | RENAME TABLE 原表名 TO 新表名 | 对表进行重命名 |
  | RENAME TABLE 原表名 TO 库名.表名 | 可将表移动到另一个数据库 |
  | DROP TABLE[ IF EXISTS] 表名 | 删除表 |
  | TRUNCATE [TABLE] 表名 | 清空表 |
  | ALTER TABLE 表名 操作名 | 修改表信息，操作如下 |
  操作名
  `ADD[ COLUMN] 字段名`-- 增加字段;
  `AFTER 字段名`-- 表示增加在该字段名后面;
  `FIRST`-- 表示增加在第一个;
  `ADD PRIMARY KEY(字段名)`-- 创建主键;
  `ADD UNIQUE [索引名] (字段名)`-- 创建唯一索引;
  `ADD INDEX [索引名] (字段名)`-- 创建普通索引;
  `DROP[ COLUMN] 字段名`-- 删除字段;
  `MODIFY[ COLUMN] 字段名 字段属性`-- 支持对字段属性进行修改不能修改字段名(所有原有属性也需写上);
  `CHANGE[ COLUMN] 原字段名 新字段名 字段属性`-- 支持对字段名修改;
  `DROP PRIMARY KEY`-- 删除主键(删除主键前需删除其 AUTO_INCREMENT 属性);
  `DROP INDEX 索引名`-- 删除索引;
  `DROP FOREIGN KEY 外键` -- 删除外键;

  数据类型

  - 整型
    |整型|占用字节|范围|范围|
    |-|-|-|-|
    |tinyint|1|-2^7^~2^7-1^|-128~127|
    |smallint|2|-2^15^~2^15-1^|-32768~32767|
    |mediumint|3|-2^23^~2^23-1^|-8388608~8388607|
    |int|4|-2^31^~2^31-1^ |-2147483648~2147483647|
    |bigint|8|-2^63^~2^63-1^||
    一个字节=8 位
    无符号数（unsigned）:就是没有负数，无符号数的正数的范围是有符号数正数范围的 2 倍。
  - 浮点数
    |浮点数|占用字节|范围|
    |-|-|-|
    |float(单精度)|4|-3.4E+38~3.4E+38|
    |double(双精度)|8|-1.8E+308~1.8E+308|
    浮点数的声明：float(M,D)，double(M,D)
    M：总位数 D：小数位数
    没有指定小数位数，默认小数位数是 0 位
    MySQL 浮点数支持科学计数法
    浮点数的精度会丢失
  - 定点数
    语法：decimal(M,D)
    M 的最大值是 65，D 的最大值是 30，默认是(10,0)
    decimal 是变长的，大致每 9 个数字用 4 个字节存储，整数和小数分开存储。定点数占用的资源可能会比浮点数大很多。
  - 字符型
    |数据类型|描述|
    |-|-|
    |char(L)|定长字符|
    |varchar(L)|可变长度字符|
    |tinytext|大段文本（大块数据） 28-1=255 个字符|
    |text|大段文本（大块数据） 216-1=65535 个字符|
    |mediumtext|2^24^-1|
    |longtext|2^32^-1|
    char
    1、 char(L)：MySQL 不回收多余的空间
    2、 L 的最大长度是 255
    varchar
    1、 varchar(L):MySQL 回收多余的空间
    2、 L 的理论最大长度是 65535，但事实上达不到，因为有的字符是多字节字符，比如一个 utf8 的字符占用 3 个字节，65535/3 大约保存 2 万多字符；如果是 gbk,一个字符占两个字节，65535/2 大约保存 3 万多个字符。
    一个记录的所有字段（不包含大数据）的总长度不能超过 65535 个字节
  - 枚举（enum）
    从集合中选择一个值作为数据（单选）,MySQL 管理枚举值是通过整型的数字来管理的，第一个值是 1，第二个值是 2，以此类推。枚举优点
    1、 限制值
    2、 节省空间
    3、 运行速度快（整数比字符串运行速度快）
    思考题：已知枚举占用 2 个字节，请问最多可以有多个枚举值。
    答：2 个字节 16 位，可以保存 216 方个值（65536，0-65535），因为枚举值从 1 开始，所以最多可以有 65535 个枚举值
  - 集合（set）
    从集合中选择一些值作为数据（多选）,集合和枚举一样，在 MySQL 内部也是通过数字来管理的。MySQL 为每个集合元素分配一个固定的值。分配方式从前往后依次是 20,21,22，…。如果有多个选项，值是单个选项的和
    集合是一个按位或的关系
    > 按位或和按位与
    >
    > > 按位与：所有的位都是 1 结果才是 1
    > > 按位或：只要有一位是 1 结果就是 1
    > > 思考题：一直集合占用 8 个字节，可以表示多少个选项？
    > > 答：8 个字节是 64 个位，可以表示 64 个选项
  - 日期时间型
    |数据类型|描述|
    |-|-|
    |datetime|日期时间 占 8 个字节|
    |date|日期 占四个字节|
    |time|时间|
    |year|年份，占用 1 个字节|
    |timestamp|时间戳，占用 4 个字节|
    datetime (日期时间)
    格式：年-月-日 小时:分钟:秒
    timestamp （时间戳）
    datetime 和 timestamp 在表现上是一样的，他们的区别在于：Datetime 从 1 到 9999，而 timestamp 从 1970 年到 2038 年（原因是 timestamp 只占用了 4 个字节），2038 年 01 月 19 日 11:14:07 秒后就超过了 4 个字节的长度。
    year
    因为 year 占用 1 个字节，所以只能保存 255 个年份，范围是 1901~2155
    time
    可以表示时间，还可以表示时间间隔，范围是-838:59:59~838:59:59，time 也支持以天的方式表示时间间隔
  - boolean
    MySQL 不支持 boolean 型，true 和 false 在数据库中对应的是 1 和 0

### 对于字段的定义

字段名 数据类型 `[NOT NULL | NULL] [DEFAULT default_value] [AUTO_INCREMENT] [UNIQUE [KEY] | [PRIMARY] KEY] [COMMENT 'string']`

### 表选项

```sql
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
```

### 列属性列约束

1. 主键约束
   - 主键的作用 1、 保证数据完整性 2、 加快查询速度
   - 能唯一标识记录的字段可以作为主键。
   - 一个表只能有一个主键。
   - 主键具有唯一性。
   - 声明字段时用 primary key 标识。
     也可以在字段列表之后声明，例 `create table tab ( id int, stu varchar(10), primary key (id));`
   - 主键字段的值不能为 null。
   - 选择主键的原则 1、 最少性：尽量选择单个键做主键 2、 稳定性：尽量选择更新少的列做主键。3、 能用数字做主键的就不要用字符串
   - 主键可以由多个字段共同组成。此时需要在字段列表后声明的方法。
     例 create table tab ( id int, stu varchar(10), age int, primary key (stu, age));
2. unique 唯一约束
   - 不能重复可以为空
   - 一个表可以有多个唯一键
3. null 约束
   - null 不是数据类型是列的一个属性。
     表示当前列是否可以为 null 表示什么都没有。
   - null, 允许为空。默认。
   - not null, 不允许为空。
     例如： `insert into tab values (null, 'val');`
     此时表示将第一个字段的值设为 null, 取决于该字段是否允许为 null
4. default 默认值属性
   - 当前字段的默认值。
   - `insert into tab values (default, 'val');` -- 此时表示强制使用默认值。
   - `create table tab ( add_time timestamp default current_timestamp );`-- 表示将当前时间的时间戳设为默认值。`current_date, current_time`
5. auto_increment 自动增长约束
   - 自动增长必须为`索引主键`或`unique`
   - 只能存在一个字段为自动增长。
   - 默认为 1 开始自动增长。可以通过表属性`auto_increment = x` 进行设置或 `alter table tbl auto_increment = x;`，一般和 unsigned 一起使用（使用无符号整数）
   - 自动增长列上的数据被删除，默认不再重复使用。
   - delete from 清空数据后编号继续增长，truncate table 清空表后编号从 1 开始增长.因为 truncate table 是删除原来的表创建一个新表。
6. comment 注释
   - 例：`create table tab ( id int ) comment '注释内容';`
7. foreign key 外键约束
   - 用于限制主表与从表数据完整性。
   - `create table 表名(字段名 类型)[constraint] [外键名称] foreign key(外键字段名) references 主表(主表列名)`
   - `alter table 表名 add constraint 外键名称 foreign key (外键字段名) references 主表(主表列名);`将表的外键关联到主表的字段。每个外键都有一个名字可以通过 constraint 指定
   - 存在外键的表称之为从表子表外键指向的表称之为主表父表。
   - 作用保持数据一致性完整性主要目的是控制存储在外键表从表中的数据。
   - `alter table 表名 drop foreign key 外键名称`删除外键
     删除、更新外键行为
     |行为|说明|
     |-|-|
     |no action/restrict|父表操作时，检查是否有对应外键，若有则不允许删除、更新|
     |cascade|级联，若有，则也删除、更新外键在子表中的记录|
     |set null|若有，则设置子表中该外键值为 null，外键值必须允许取 null|
     |set default|若有，则设置子表中该外键值为默认值，innodb 不支持|
     `create table 表名(字段名 类型)[constraint] [外键名称] foreign key(外键字段名) references 主表(主表列名) on update cascade on delete cascade`

### MySQL 中可以对 InnoDB 引擎使用外键约束

语法：
`foreign key (外键字段 references 主表名 (关联字段) [主表记录删除时的动作] [主表记录更新时的动作]`

此时需要检测一个从表的外键需要约束为主表的已存在的值。外键在没有关联的情况下可以设置为 `null`，前提是该外键列没有 `not null`。

可以不指定主表记录更改或更新时的动作那么此时主表的操作被拒绝。

如果指定了 `on update` 或 `on delete` 在删除或更新时有如下几个操作可以选择

1. cascade 级联操作。主表数据被更新主键值更新从表也被更新外键值更新。主表记录被删除从表相关记录也被删除。

2. set null 设置为 null。主表数据被更新主键值更新从表的外键被设置为 null。主表记录被删除从表相关记录外键被设置成 null。但注意要求该外键列没有 not null 属性约束。

3. restrict 拒绝父表删除和更新。

注意外键只被 InnoDB 存储引擎所支持。其他引擎是不支持的。

### DML（增删改）

- 增

```sql
INSERT [INTO] 表名 [(字段列表)] VALUES (值列表)[, (值列表), ...]
        -- 如果要插入的值列表包含所有字段并且顺序一致，则可以省略字段列表。
        -- 可同时插入多条数据记录！
        -- 字符串和日期数据应包含在引号中
        REPLACE 与 INSERT 完全一样，可互换。
INSERT [INTO] 表名 SET 字段名=值[, 字段名=值, ...]


```

- 删

```sql
DELETE FROM 表名[ 删除条件子句]

没有条件子句则会删除全部
```

- 改

```sql
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]

mysql> SELECT * from runoob_tbl WHERE runoob_id=3;
```

### DQL

> 执行顺序`form`>`where`>`group by`>`having`>`select`>`order by`>`limit`

#### 单表查询

```sql
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

- 去除重复记录
  `select distinct 字段列表 from 表名`

- 设置别名 AS

`SELECT 字段1[AS 别名], 字段2[AS 别名] FROM 表名;-- AS可省略`

- WHERE 子句

```sql
   SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

> MySQL 的 WHERE 子句的字符串比较是不区分大小写的。 你可以使用 BINARY 关键字来设定 WHERE 子句的字符串比较是区分大小写的。
> 字符串用单引号

条件判断：`列 操作符 值`

操作符：
`= < > <= >=  !=`
`BETWEEN...AND...` 在某个范围（含最小最大值）
`IN(...)`在 in 之后的列表中的值，多选一
`IS NULL` 是 null

逻辑运算符
`AND`或`&&` 多个条件同时成立
`OR`或`||` 多个条件成立一个
`NOT`或`!` 非

LIKE 子句

LIKE 子句中使用百分号 `%`字符来表示任意个字符，类似于 `UNIX` 或`正则表达式中`的星号`*`。如果没有使用百分号 `%`, LIKE 子句与等号`=`的效果是一样的。`_`下划线匹配单个字符

`mysql> SELECT * from runoob_tbl WHERE runoob_author LIKE '%COM';`

- 分组聚合
  `select 列,聚合函数 from 表 [where条件] group by 列 [having 分组后的过滤条件]`

聚合函数（将一列数据作为一个整体进行纵向计算）:

> null 值不参与聚合函数计算

```txt
sum(列)-- 求和
avg(列)--平均
min(列)
max(列)
count(列|*)-- 数量
```

where 与 having:
where 是分组前过滤，不满足 where 条件不参与分组。having 是分组后对结果进行过滤。where 不能对聚合函数进行判断，而 having 可以

分组后，查询字段一般为聚合函数和分组的字段，查询其他字段只会显示第一个值

- 排序

`select 字段, from 表  order by 关键字 [asc|desc],关键词2 [asc|desc]`

如果是多字段排序，当第一个字段值相同的时候，才会根据第二个字段进行排序

- 分页（mysql)

`select * from table limit n[,m]`
n：起始索引,跳过前 n 条，m：取出 m 条

#### 多表查询

> 多表关系
>
> > 一对一(用户和用户详情)：多用于单表拆分，将用户和用户详情拆分成两个表，提升操作效率。任意一方加入外键，并设置外键为唯一
> > 一对多(一个部门对于多个员工，一个员工对应一个部门)：多的一方建立外键，指向一的一方
> > 多对多(一个学生对于多个课程，一个课程对应多个学生)：只能建立第三张表，包含两个外键，分别关联两方主键

- 多表查询语法(它会让参与的表先产生笛卡尔积【如两个集合 a，b 的所有组合情况】，然后在 where 过滤，性能差)

  `select  字段1,字段2... from 表1,表2... [where 条件]`

as 别名
`select  字段1,字段2... from 表1 as 别名,表2 as 别名... [where 条件]`

- 多表链接查询

```sql
SELECT 字段列表 FROM 表1  [,INNER]|[LEFT|RIGHT,[OUTER]] JOIN  表2 ON 表1.字段 = 表2.字段;
```

1. 内关联是求交集，必须双向匹配才能查出
2. 外关联：左外连接查询 (左边表中的数据优先全部显示，不用匹配右边),右外连接查询 (右边表中的数据优先全部显示，不用匹配左边)
3. 全连接查询(联合查询)，联合查询不会使索引失效，而 or 会

   ```sql
    -- 全连接查询：是在内连接的基础上增加 左右两边没有显示的数据
    -- 注意: mysql并不支持全连接 full JOIN 关键字
    -- 注意: 但是mysql 提供了 UNION 关键字.使用 UNION 可以间接实现 full JOIN 功能
    -- 联合查询多张表的列数，类型必须保持一致
    select * from student left join sc on student.sid = sc.sid
    UNION
    select * from student right join sc on student.sid = sc.sid;
    -- 注意: UNION 和 UNION ALL 的区别:UNION 会去掉重复的数据,而 UNION ALL 则直接显示结果/
   ```

4. 自连接，必须起别名，可以内联也可以外联
   `select 字段 from 表1 别名1 join 表1 别名2 on 条件...`

- 三表查询

`select 表1.字段,表2.字段,表3.字段 from 表1 join 表2 on 表1.关联字段 = 表2.关联字段 join 表3 on 表2.关联字段 = 表3.关联字段 where....;`

- 子查询（嵌套查询）
  - 标量子查询（子查询结果返回的结果为单个值）
  - 列子查询（返回的一列）
    常用操作：`in`在集合范围内多选一,`not in`不在集合范围,`any`任意一个满足即可,`some`等同 any,`all`必须全部满足
  - 行子查询（返回的一行）
    常用操作：`in`在集合范围内多选一,`not in`不在集合范围,`=`等于,`!=`不等于
  - 表子查询(多行多列)
    常用操作：`in`在集合范围内多选一,

### DCL 管理用户管理权限

- 查询用户

  ```sql
  use mysql;
  select * from user;
  ```

- 创建用户
  `create user '用户名'@'主机名' identified by '密码'`
  主机名指在哪个主机上才能登陆访问数据库，可改为`%`，即可在任何主机登陆
- 修改用户密码
  `alter user '用户名'@'主机名' identified width mysql_nativev_password by '密码'`

- 删除用户
  `drop user '用户名'@'主机名'`
- 权限控制
  常用权限：
  |权限|说明|
  |-|-|
  |ALL,ALLPRIVILEGES|所有权限|
  |SELECT|查询|
  |INSERT|插入数据|
  |UPDATE|修改数据|
  |DELETE|删除数据|
  |ALTER|修改表|
  |DROP|删除表，数据库，视图|
  |CREATE|创建表，数据库|
  |INDEX|授予用户可以在表上定义索引的权限|
  |super|允许执行一系列数据库管理命令|
  |file|file 权限指的是对服务器主机上文件的访问，数据库用户拥有 file 权限才可以执行 select ..into outfile，load data infile…操作.|
  但是不要把 file, process, super 权限授予管理员以外的账号，这样存在严重的安全隐患。

  查询权限
  `show grants for '用户名'@'主机名'`

  授权
  `grant 权限,权限2 on 数据库.表名 to'用户名'@'主机名'`
  数据库名和表名可以通配符`*`代表所有

  撤销权限
  `revoke 权限,权限2 on 数据库.表名 from  '用户名'@'主机名'`

## mysql 函数

### 字符串函数

| 函数                      | 功能                       |
| ------------------------- | -------------------------- |
| concat(s1,s2...)          | 字符串拼接                 |
| lower(str)                | 转小写                     |
| upper(str)                | 转大写                     |
| trim(str)                 | 去除首尾空格               |
| substring(str,strart,len) | 返回 start 起 len 个字符串 |

### 数值函数

| 函数       | 功能                        |
| ---------- | --------------------------- |
| ceil(x)    | 向上取整                    |
| floor(x)   | 向下取整                    |
| mod(x,y)   | 返回 x/y 的模               |
| rand()     | 0-1 随机数                  |
| round(x,y) | x 的四舍五入，暴露 y 位小数 |

### 日期函数

| 函数                              | 功能                                                                   |
| --------------------------------- | ---------------------------------------------------------------------- |
| curdate()                         | 当前日期                                                               |
| curtime()                         | 当前时间                                                               |
| now()                             | 当前日期时间                                                           |
| year(date)                        | 获取指定 date 的年                                                     |
| month(date)                       | 获取指定 date 的月                                                     |
| day(date)                         | 获取指定 date 的日                                                     |
| date_add(date,interval expr type) | 返回 date 加上时间间隔 expr 后的时间值 ,type 为单位(day,month,year...) |
| date_sub(date,interval expr type) | 返回 date 减去时间间隔 expr 后的时间值 ,type 为单位(day,month,year...) |
| datediff(date1,date2)             | 返回 date1 和 date2 之间的天数                                         |

### 流程函数

| 函数                                                                     | 功能                                                    |
| ------------------------------------------------------------------------ | ------------------------------------------------------- |
| if(value,t,f)                                                            | 如果 value 为真返回 t，否则返回 f                       |
| ifnull(val1,val2)                                                        | 如果 val1 为不为空，返回 val1，否则返回 val2            |
| case when [val1] then [res1]when [val2] then [res2]...else [default] end | 如果 val1 为真，返回 res1，...否则返回 default 值       |
| case [expr] when [val1] then [res1]...else [default] end                 | 如果 expr 等于 val1 ，返回 res1，...否则返回 default 值 |

## 事务

### 特性

- 原子性：事务是不可分割的最小操作单元，要么全部成功，要么去全部失败；
- 一致性：事务完成时，必须使所有的数据都保持一致状态。
- 隔离性：数据库系统提供的隔离机制，保证事务在不受外部并发操作的独立环境下运行
- 持久性：事务一旦提交或回滚，它对数据库的数据的改变是永久的

### 操作

- 查看设置事务提交方式

```sql
SELECT @@autocommit;
Set @@autocommit=0;
```

- 开启事务

```sql
START TRANSACTION;
或
BEGIN;
```

- 提交事务

```sql
COMMIT;
```

- 回滚事务

```sql
ROLLBACK;
```

### 并发事务问题

- 脏读：一个事务读到另一个事务还没有提交的数据
- 不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同
- 幻读：一个事务按照条件查询数据时，没有对应的数据行，但是插入数据时，又提示该行数据已经存在。

- 事务隔离级别
  |隔离级别|脏读|不可重复读|幻读|
  |-|-|-|-|
  |Read uncommitted|出现|出现|出现|
  |Read committed|不出现|出现|出现|
  |Repeatable Read(mysql 默认)|不出现|不出现|出现|
  |Serializable|不出现|不出现|不出现|

- 查看事务隔离级别

```sql
select @@TRANSACTION_ISOLATION;
```

- 设置事务隔离级别

```sql
SET [SEEESION|GLOBAL] TRANSACTION ISOLATION LEVEL{READ UNCOMMITTED|READ COMMITTED|REPEATABLE READ|SERIALIZABLE}
```

## 存储引擎

> 就是存储数据、建立索引、更新查找数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可以被称为表类型

- innoDB
  > 兼顾高可靠性和高性能的通用存储引擎，在 mysql5.5 之后为默认引擎
  > 特点：DM 操作遵循 ACID 模型，支持`事务`支持`级锁`提高并发访问性能；支持`外键`，保证数据的完整性和正确性;最大 64TB，空间和内存使用高，批量插入慢，适用除读操作和插入操作外，还有大量的更新、删除操作的应用。
  > 文件：xxx.idb:存储该表的表结构和数据和索引
- MyISAM
  > 早期 mysql 默认引擎
  > 特点：不支持事务、外键、行锁；支持表锁，访问速度快;空间和内存使用低，批量插入快，适用于读操作和插入操作为主的应用。
- Memory

  > 表数据存在内存中，常用于临时表、缓存使用
  > 特点：内存存放、hash 索引
  > 文件：xxx.sdi:存储表结构信息

- 创建表时，指定存储引擎

```sql
create table 表名(字段1 类型 [comment 注释],...)engine=innoDB [comment 表注释];
```

- 查看当前数据库支持的存储引擎

```sql
show engines;
```

## 索引

> 帮助 mysql 高效获取数据的数据结构（有序）。在数据之外，数据库还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用数据，这样就可以在这些数据结构上实现高级的查找算法，这种数据结构就是索引
> 优势：提高数据检索效率，降低数据库的 io 成本。对数据进行排序时，降低 cpu 消耗。
> 缺点：索引需要额外的磁盘空间。降低表的更新速度，效率降低。

### 　类型

| 索引结构           | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| B+Tree 索引        | 最常见的索引，大部分引擎都支持                               |
| Hash 索引          | 利用哈希表实现，只有精确匹配索引的查询才有效，不支持范围查找 |
| R-tree 空间索引    | MyISAM 引擎的一个特殊索引类型，常用于地理空间数据类型        |
| Full-text 全文索引 | 通过建立到排索引，快速匹配文档的方式，类似 ES                |

### 分类

| 分类     | 说明                                                 | 特点                     | 关键词   |
| -------- | ---------------------------------------------------- | ------------------------ | -------- |
| 主键索引 | 针对表中的主键创建的索引                             | 默认自动创建，只能有一个 | primary  |
| 唯一索引 | 避免同一个表中数据列的值重复                         | 可以多个                 | unique   |
| 常规索引 | 快速定位特定数据                                     | 可以多个                 |          |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 | 可以多个                 | fulltext |

innoDB 根据索引的存储类型又可以分为下面两种

- 聚集索引 clustered Index：将数据存储和索引放到一块，索引结构的叶子节点保存了行数据（必须有，且只有一个）
  - 如果主键存在，主键索引就是聚集索引
  - 若无主键，将使用第一个唯一索引作为聚集索引
  - 若表既无主键也无唯一索引，innoDB 将自动生成 rowid 作为隐藏的聚集索引
- 二级索引 Secondary Index：将数据和索引分开存储，索引结构的叶子节点关联的是对应的主键（可以存在多个）

### 语法

- 创建索引
  `create [unique|fulltext] INDEX 索引名 ON 表名(字段名,...)`
- 查看索引
  `show index from 表名`
- 删除索引
  `drop idnex 索引名 on 表名`

### 使用

- 最左前缀法则：如果索引了多列，查询从索引的最左列开始，并且不跳过索引中列，若跳过某一列，索引将部分失效即后面的字段索引失效
- 范围查询：如果索引了多列，出现范围查询（`>`，`<`），范围查询右侧的列索引失效。可使用`>-`，`<=`避免出现这种情况
- 索引列运算：索引列进行运算操作，索引将失效
- 字符串不加引号：字符串不加引号，索引将失效
- 模糊查询：尾部模糊匹配，索引不失效，头部模糊索引失效。
- or 连接：or 前后都有索引，索引才不会失效
- 数据分布影响：若 mysql 评估索引查询比全表查询慢，将不会使用索引
- 覆盖索引：尽量使用覆盖索引（查询使用了索引，兵器需要返回的列，在该索引中已经全部能够找到），减少 select \*
- 单列索引与联合索引：若存在多个查询条件，建议建立联合索引。使用单列索引时，mysql 会根据评估，自动选择，并不会全用上索引
- sql 提示：

  ```sql
  select * from 表 use index(索引名) where...
  select * from 表 ignore index(索引名) where...
  select * from 表 force index(索引名) where...
  ```

- 前缀索引：字段类型为 varchar 或 text，建立的索引会变得非常大。通过截取字符串前一部分建立索引提交索引效率

  ```sql
  #n为截取的个数
  create index 索引名 on 表(列名(n));
  ```

## 性能优化

- 查看操作次数
  `show [global|session] status link 'com_______';`七个下划线

### 慢查询日志

> 记录所有执行超过指定参数（long_query_time,单位：秒，默认 10s）的所有 sql 语句日志，默认没有开启

- 查看是否开启：`show variables like 'slow_query_log';`
- 开启：编辑/etc/my.cng

```conf
show_query_log=1
long_query_time=2
```

- 日志文件：`/var/lib/mysql/localhost-slow.log`

### profile 详情

> 查看 sql 语句具体详情

- 查看是否支持：`select @@have_profiling;`
- 启动：`set profiling=1;`
- 查看：

```sql
#查看全部
show profiles;
#指定id
show profile for query sql语句id;
#查看cup情况
show profile cpu for query sql语句id;
```

### explain 执行计划

> 获取 mysql 如何执行 select 语句信息。

- 使用

```sql
explain select * from 表
或
desc select * from 表
```

- 结果字段说明:
  - id：操作顺序，id 相同，从上到下，否则值也大，越先执行
  - select_type：查询类型
  - type：连接类型，由好到差->null、system、cons、eq_ref、ref、range、index、all
  - prossible_key：显示可能应用在这张表上的索引
  - key：实际使用的索引
  - key_len：索引中使用的字节数，该值为最大可能长达，并非实际使用长度
  - rows：必须要执行查询的行数，是个估值
  - filterd：表示返回结果的行数占需读取行数的百分百，越大越好

### insert

- 主键顺序插入
- 多条插入尽可以使用批量插入`insert into 表 values(1,'1'),(2,'2')`
- 手动提交事务

  ```sql
  start transaction;
  insert to 表 values(..)
  insert to 表 values(..)
  insert to 表 values(..)
  commit;
  ```

- 大批量插入数据：使用 mysql 的 load 指令进行插入操作

```bash
mysql --local-infile -u root -p
set global local_infile=1;
load data local infile '/home/sql.log' into table '表名' fields terminated by '字段间的分割符' lines teminated by '每一行的分隔符'
```

### 主键优化

- 尽量降低主键长度--减少 b+tree 空间
- 插入数据时尽量顺序插入--避免页分裂
- 尽量不要使用 uuid 做主键--避免页分裂
- 避免对主键修改--重新构建 b+tree

## MySQL 分页查询的 5 种方法

方式 1：直接使用数据库提供的 SQL 语句

适应场景: 适用于数据量较少的情况(元组百/千级)。很简单，该语句的意思就是查询 m+n 条记录，去掉前 m 条，返回后 n 条。无疑该查询能够实现分页，但 m 越大，查询性能就越低，因为 MySQL 需要扫描全部 m+n 条记录。

方式 2：基于索引再排序

`select \* from table order by id limit m, n;`

很简单，该语句的意思就是查询 m+n 条记录，去掉前 m 条，返回后 n 条。无疑该查询能够实现分页，但 m 越大，查询性能就越低，因为 MySQL 需要扫描全部 m+n 条记录。

方式 2：

`select \* from table where id > #max_id# order by id limit n;`

该查询同样会返回后 n 条记录，却无需像方式 1 扫描前 m 条记录，但必须在每次查询时拿到上一次查询（上一页）的最大 id（或最小 id），是比较常用的方式。

当然该查询的问题也在于我们不一定能拿到这个 id，比如当前在第 3 页，需要查询第 5 页的数据，就不行了。

方式 3：

方式 3：基于索引再排序
为了避免方式 2 不能实现的跨页查询，就需要结合方式 1。

性能需要，m 得尽量小。比如当前在第 3 页，需要查询第 5 页，每页 10 条数据，且当前第 3 页的最大 id 为#max_id#，则：

`select \* from table where id > #max_id# order by id limit 10, 10;`

该方式就部分解决了方式 2 的问题，但如果当前在第 2 页，要查第 1000 页，性能仍然较差。

方式 4：

`select \* from table as a inner join (select id from table order by id limit m, n) as b on a.id = b.id order by a.id;`

该查询同方式 1 一样，m 的值可能很大，但由于内部的子查询只扫描了 id 字段，而非全表，所以性能要强于方式 1，并且能够解决跨页查询问题。

方式 5：

`select \* from table where id > (select id from table order by id limit m, 1) limit n;`

该查询同样是通过子查询扫描字段 id，效果同方式 4。但方式 5 的性能会略好于方式 4，因为它不需要进行表的关联，而是一个简单的比较，在不知道上一页最大 id 的情况下，是比较推荐的用法。

方法 6: 基于索引使用 prepare,
语句样式: MySQL 中,可用如下方法:
代码如下:
`PREPARE stmt*name FROM SELECT * FROM 表名称 WHERE id*pk > (？* ？) ORDER BY id_pk ASC LIMIT M`
（第一个问号表示 pageNum，第二个？表示每页元组数）
适应场景: 大数据量。
原因: 索引扫描,速度会很快. prepare 语句又比一般的查询语句快一点。

方法 7:利用 MySQL 支持 ORDER 操作可以利用索引快速定位部分元组,避免全表扫描
比如: 读第 1000 到 1019 行元组(pk 是主键/唯一键)。
代码如下:
`SELECT \* FROM your_table WHERE pk>=1000 ORDER BY pk ASC LIMIT 0,20`

## 插入表情

1. MySQL 的版本
   `utf8mb4` 的最低 mysql 版本支持版本为 `5.5.3+`，若不是，请升级到较新版本。
2. MySQL 驱动
   `5.1.34` 可用,最低不能低于 `5.1.13`
3. 修改 MySQL 配置文件
   修改 mysql 配置文件 `my.cnf`，`my.cnf` 一般在 `etc/mysql/my.cnf` 位置。找到后请在以下三部分里添加如下内容：

```ini
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

```bash
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

当数据库中涉及编码类型改变的时候，需要统一数据库，表，字段的编码类型，少一个都不行。
修改数据库字符集：
`ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;`
修改表的字符集：
`ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
修改字段的字符集：
`ALTER TABLE table_name CHANGE column_name column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;` 4. 查看是否成功修改
使用`SHOW VARIABLES WHERE Variable_name LIKE 'character_set_%' OR Variable_name LIKE 'collation%';`查看当前编码,结果中的 `collation_connection` 、`collation_database` 、`collation_server` 是什么没关系。但必须保证系统变量 描述

```txt
character_set_client (客户端来源数据使用的字符集)
character_set_connection (连接层字符集)
character_set_database (当前选中数据库的默认字符集)
character_set_results (查询结果字符集)
character_set_server (默认的内部操作字符集)
```

这几个变量必须是 utf8mb4,才能插入表情。
查看表编码
`mysql> show create table dictionary; # dictionary 是表名` 5. 数据库连接的配置
数据库连接参数中也要设置为 utf8bm4 格式，否则是无法插入的 `charset=utf8mb4`

## 数据库备份与还原

1. 备份原文件
   MySQL 中的每一个数据库和数据表分别对应文件系统中的目录和其下的文件。在 Linux 下数据库文件的存放目录一般为/var/lib/mysql。在 Windows 下这个目录视 MySQL 的安装路径而定。备份文件前，需要将 MySQL 服务停止，然后将数据库目录拷贝即可。恢复数据数据库时，需要先创建好一个数据库(不一定同名)，然后将备份出来的文件(注意，不是目录)复制到对应的 MySQL 数据库目录中。使用这一方法备份和恢复数据库时，需要新旧的 MySQL 版本一致，否则可能会出现错误。

2. 使用命令

备份数据库(不需要进入 mysql)：
`mysqldump –user=root –password=root密码 –lock-all-tables 数据库名 > 备份文件.sql`
恢复数据库：
`mysql -u root –password=root密码 数据库名 < 备份文件.sql`

## nodejs 连接数据库

```sql
client does not support authentication protocol requested by server
//出现连接失败的原因mysql8之前的版本中加密规则是mysql_native_password,而在mysql8之后,加密规则是caching_sha2_password。并提供了两种解决方案
//把用户密码登录的加密规则还原成mysql_native_password这种加密方式
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;这里的password是你正在使用的密码
```
