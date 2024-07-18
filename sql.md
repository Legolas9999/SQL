# 五、SQL语句

## 1、SQL语句分类

### ☆ DDL

数据定义语言：简称DDL(Data Definition Language)
用来定义数据库对象：数据库，表，列等。
关键字：create，alter，drop等

### ☆ DML

数据操作语言：简称DML(Data Manipulation Language)
用来对数据库中表的记录进行更新。
关键字：insert，delete，update等

### ☆ DQL

数据查询语言：简称DQL(Data Query Language)
用来查询数据库中表的记录。
关键字：select，from，where等

### ☆ DCL

数据控制语言：简称DCL(Data Control Language)
用来定义数据库的访问权限和安全级别，及创建用户。


# DDL数据库操作

## 创建数据库


基本语法：

```sql
create database [if not exists] 数据库名称 [编码：default character set utf8];
```

## 查询数据库 显示所有数据库


基本语法：显示所有数据库

```sql
show databases;
```

## 删除数据库


基本语法：

```sql
drop database 数据库名称;
```


## 选择数据库

```sql
use 数据库名称;
```

## 查看正在使用的数据库

（8.0以后版本需要基于select查询来获取当前数据库）

```sql
select database();
```

# DDL数据表操作

## 数据表的创建

基本语法：

```sql
create table 数据表名称(
	字段1名称 字段类型 [字段约束],
	字段2名称 字段类型 [字段约束],
	...
)[engine=innodb default charset=utf8]; 
```

## 查询已创建数据表

显示所有数据表（当前数据库）

```sql
use 数据库名称;
show tables;
```

## 显示数据表的信息（编码格式、字段等信息）

```sql
desc 表名;
```

## 输出建表语句

```sql
show create table 表名;
```

## 数据表字段添加

```sql
alter table 数据表名称 add 新字段名称 字段类型 [ first | (after 字段名称)] ;

选项说明：
first：把新添加字段放在第一位
after 字段名称：把新添加字段放在指定字段的后面
```
## 修改数据表名称

```sql
rename table 旧名称 to 新名称;
# 或者
alter table 旧名称  rename  新名称;
```


## 修改字段名称与类型

修改字段名称与字段类型
```sql
alter table 表名 change 旧字段名 新字段名 数据类型;
```

## 仅修改字段的类型

```sql
alter table 表名 modify 字段名 数据类型;
```

## 删除某个字段

```sql
alter table 表名 drop 字段名称;
```



## 删除数据表


```sql
drop table 数据表名称;
```

## 字段类型详解

① 整数类型

| **分类**         | **类型名称**   | **说明**                 |
| ---------------- | -------------- | ------------------------ |
| tinyint     | 很小的整数     | -128 ~ 127               |
| smallint         | 小的整数       | -32768 ~ 32767           |
| mediumint        | 中等大小的整数 | -8388608 ~ 8388607       |
| int(integer) | 普通大小的整数 | -2147483648 ~ 2147483647 |

> 整数类型的选择，看数字范围！

> unsigned无符号型，只有正数，没有负数情况，tinyint unsigned，表示范围0-255

② 浮点类型（带小数点数据类型）

浮点类型（精度失真情况）和定点类型（推荐使用定点类型）

| 分类             | 类型名称              |
| ---------------- | --------------------- |
| float            | 单精度浮点数          |
| double           | 双精度浮点数          |
| ==decimal(m,d)== | 定点数，decimal(10,2) |

> decimal(10,2) ：代表这个数的总长度为10 = 整数长度 + 小数长度，2代表保留2位小数

③ 日期类型

| 名称       | 说明                                                     |
| ------------ | ------------------------------------------------------------ |
| year         | YYYY 1901~2155                                               |
| ==time==     | HH:MM:SS -838:59:59~838:59:59                                |
| ==date==     | YYYY-MM-DD  1000-01-01~9999-12-3                             |
| ==datetime== | YYYY-MM-DD  HH:MM:SS 1000-01-01 00:00:00~ 9999-12-31 23:59:59 |
| timestamp    | YYYY-MM-DD  HH:MM:SS 1970~01~01 00:00:01  UTC~2038-01-19 03:14:07UTC |

> 日期时间类型的选择主要看格式！

④ 文本类型

| **类型名称**   | **说明**                             |
| -------------- | ------------------------------------ |
| ==char(m)==    | m为0~255之间的整数定长（固定长度）   |
| ==varchar(m)== | m为0~65535之间的整数变长（变化长度） |
| ==text==       | 允许长度0~65535字节                  |
| mediumtext     | 允许长度0~167772150字节              |
| longtext       | 允许长度0~4294967295字节             |

> 255个字符以内，固定长度使用char(字符长度)，变化长度使用varchar(字符长度)
>
> 假设：有一个数据，一共有5个字符，char(11)，实际占用11个字符，相同长度数据，char类型查询要比varchar要快一些；变化长度varchar(11)，实际占用5个字符长度。
>
> 超过255个字符，选择text文本类型！

| **类型名称**   | **说明**                             |
| -------------- | ------------------------------------ |
| enum    | 枚举类型，只能从给定的值中选择一个，gender enum('男','女','保密')   |



## 数据的增加操作

基本语法：

```sql
insert into 数据表名称[(字段1,字段2,字段3...)] values (字段1的值,字段2的值,字段3的值...);
```

> 特别注意：在SQL语句中，除了数字，其他类型的值，都需要使用引号引起来，否则插入时会报错。

> unsigned代表无符号型，只有0到正数。tinyint unsigned无符号型，范围0 ~ 255

> enum枚举类型，多选一。只能从给定的值中选择一个

- 使用insert语句插入单条数据

```sql
insert into 表名 values (字段1的值,字段2的值,字段3的值...);
insert into 表名(字段1,字段2,字段3...) values (字段1的值,字段2的值,字段3的值...);
```

- 批量插入多条数据

```sql
insert into 表名 values (字段1的值,字段2的值,字段3的值...),
(字段1的值,字段2的值,字段3的值...),
(字段1的值,字段2的值,字段3的值...);
```

## 数据的修改操作

基本语法：

```sql
update 数据表名称 set 字段1=更新后的值,字段2=更新后的值,... where 更新条件;
```

> 特别说明：如果在更新数据时，不指定更新条件，则其会把这个数据表的所有记录全部更新一遍。


## 数据的删除操作

基本语法：

```sql
delete from 数据表名称 [where 删除条件];
```


delete from与truncate清空数据表操作

```sql
delete from 数据表;
或
truncate 数据表;
```



delete from与truncate区别在哪里？

- delete：删除==数据记录==
  - 数据操作语言（DML）
  - 删除==大量==记录速度慢，==只删除数据，主键自增序列不清零== (例如：删除最后一条数据，其id为5，再添加一条数据时，想让新数据id还是5，但实际是6。-->12346)
  - 可以==带条件==删除
- truncate：删除==所有数据记录==
  - 数据定义语言（DDL）
  - 清里大量数据==速度快==，==主键自增序列清零==
  - ==不能带条件删除==

# SQL约束

## 1、主键约束

1、PRIMARY KEY 约束唯一标识数据库表中的每条记录。<br>
2、主键必须包含`唯一`的值。<br>
3、主键列`不能包含 NULL` 值。<br>
4、每个表都应该有一个主键，并且每个表只能有一个主键。<br>

遵循原则：<br>
1）主键应当是对用户没有意义的<br>
2）永远也不要更新主键。<br>
3）主键不应包含动态变化的数据，如时间戳、创建时间列、修改时间列等。<br>
4） 主键应当由计算机自动生成。<br>

- 创建主键约束：创建表时，在字段描述处，声明指定字段为主键
```sql
create table person(
  id int primary key,       #主键约束
  first_name varchar(255),
  last_name varchar(255)
);

# 或者

create table person(
  id int,       
  first_name varchar(255),
  last_name varchar(255),
  primary key(id)           #主键约束
);

# 或者

alter table 表名 add primary key(字段名);
```


- 删除主键约束：如需撤销 PRIMARY KEY 约束，请使用下面的 SQL

```sql
alter table 表名 drop primary key;
```



> 补充：自动增长

我们通常希望在每次插入新记录时，数据库自动生成字段的值。

我们可以在表中使用 `auto_increment`（自动增长列）关键字，自动增长列类型必须是`整型`，自动增长列必须为键(一般是`主键`)。

**下列 SQL 语句把 "Persons" 表中的 "Id" 列定义为** **auto_increment** **主键**

```sql
create table persons3(
	id int auto_increment primary key,
	first_name varchar(255),
	last_name varchar(255),
	address varchar(255),
	city varchar(255)
) default charset=utf8;
```

向persons添加数据时，可以不为Id字段设置值，也可以设置成null，数据库将自动维护主键值：

```sql
insert into persons3(first_name,last_name) values('Bill','Gates');
insert into persons3(id,first_name,last_name) values(null,'Bill','Gates');
```


## 2、非空约束

NOT NULL 约束强制列不接受 NULL 值。
NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。
下面的 SQL 语句强制 "id" 列和 "last_name" 列不接受 NULL 值：

```sql
create table person(
  id int primary key,       #主键约束
  first_name varchar(255) not null,  #非空约束
  last_name varchar(255)
);
```


## 3、唯一约束

UNIQUE 约束`唯一标识`数据库表中的每条记录。<br>
UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。<br>
PRIMARY KEY 拥有自动定义的 UNIQUE 约束。<br>
UNIQUE 约束可以为null


请注意：
每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。

```sql
create table person(
  id int primary key,       #主键约束
  first_name varchar(255) not null,  #非空约束
  last_name varchar(255) unique      # 唯一约束
);
```

## 4、默认值约束

default 默认值

```sql
create table person(
  id int primary key,       #主键约束
  first_name varchar(255) not null,  #非空约束
  last_name varchar(255) unique,      # 唯一约束
  gender enum('男','女') default '男'  # 默认值
);

# 对已有列设置默认值
alter table 表名 alter column 字段名 set default 默认值;
```

## 5、外键约束(了解)

外键约束（==多表==关联使用）

比如：有两张数据表，这两个数据表之间==有联系==，通过了==某个字段==可以建立连接，这个字段在其中一个表中是==主键==，在另外一张表中，我们就把其称之为==外键==。

当主键表的数据发生变化，则与之关联的表也发生变化

![](https://github.com/user-attachments/assets/34c7a75f-d050-4fe2-acde-156eaad511a9)
## 6、小结

主键约束：唯一标示，不能重复，不能为空。
1）主键应当是对用户没有意义的
2）永远也不要更新主键。
3）主键不应包含动态变化的数据，如时间戳、创建时间列、修改时间列等。
4） 主键应当由计算机自动生成。

自动增长：
我们可以在表中使用 auto_increment（自动增长列）关键字，自动增长列类型必须是整型，自动增长列必须为键(一般是主键)。

非空约束：
NOT NULL 约束强制列不接受 NULL 值。

唯一约束：
UNIQUE 约束唯一标识数据库表中的每条记录。
UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。
PRIMARY KEY 拥有自动定义的 UNIQUE 约束。

默认值约束：

DEFAULT  默认值

外键约束：

适合应用在多表关联中，如果关联字段在一个表中是主键，在另外一张表就称之为外键。

外键约束：当主键表中的数据有变化，则与之关联的数据表也会随之发生变化！

# 十、DQL数据查询语言

五子句查询

```sql
select * from 数据表 [where子句] [group by分组子句] [having子句] [order by子句] [limit子句];
① where子句
② group by子句
③ having子句
④ order by子句
⑤ limit子句
注意：在以上5个子句中，五子句的顺序不能颠倒！
```

## 1、数据集准备

```mysql
CREATE TABLE product
(
    pid         INT PRIMARY KEY,
    pname       VARCHAR(20),
    price       DOUBLE,
    category_id VARCHAR(32)
);
```

插入数据：

```mysql
INSERT INTO product VALUES (1,'联想',5000,'c001');
INSERT INTO product VALUES (2,'海尔',3000,'c001');
INSERT INTO product VALUES (3,'雷神',5000,'c001');
INSERT INTO product VALUES (4,'杰克琼斯',800,'c002');
INSERT INTO product VALUES (5,'真维斯',200,'c002');
INSERT INTO product VALUES (6,'花花公子',440,'c002');
INSERT INTO product VALUES (7,'劲霸',2000,'c002');
INSERT INTO product VALUES (8,'香奈儿',800,'c003');
INSERT INTO product VALUES (9,'相宜本草',200,'c003');
INSERT INTO product VALUES (10,'面霸',5,'c003');
INSERT INTO product VALUES (11,'好想你枣',56,'c004');
INSERT INTO product VALUES (12,'香飘飘奶茶',1,'c005');
INSERT INTO product VALUES (13,'海澜之家',1,'c002');
```

## 2、select查询

```sql
# 根据某些条件从某个表中查询指定字段的内容
格式：select [distinct]*| 列名,列名 from 表where 条件
```

## 3、简单查询

```sql
# 1.查询所有的商品.  
select * from product;
# 2.查询商品名和商品价格. 
select pname,price from product;
# 3.查询结果是表达式（运算查询）：将所有商品的价格+10元进行显示.
select pname,price+10 from product;
```

## 4、条件查询

![image-20210906185658519](media/image-20210906185658519.png)

### ☆ 比较查询

```sql
# 查询商品名称为“花花公子”的商品所有信息：
SELECT * FROM product WHERE pname = '花花公子';
# 查询价格为800商品
SELECT * FROM product WHERE price = 800;
# 查询价格不是800的所有商品
SELECT * FROM product WHERE price != 800;
SELECT * FROM product WHERE price <> 800;
# 查询商品价格大于60元的所有商品信息
SELECT * FROM product WHERE price > 60;
# 查询商品价格小于等于800元的所有商品信息
SELECT * FROM product WHERE price <= 800;
```

### ☆ 范围查询

```sql
# 查询商品价格在200到1000之间所有商品
SELECT * FROM product WHERE price BETWEEN 200 AND 1000;
# 查询商品价格是200或800的所有商品
SELECT * FROM product WHERE price IN (200,800);
```

### ☆ 逻辑查询

```sql
# 查询商品价格在200到1000之间所有商品
SELECT * FROM product WHERE price >= 200 AND price <=1000;
# 查询商品价格是200或800的所有商品
SELECT * FROM product WHERE price = 200 OR price = 800;
# 查询价格不是800的所有商品
SELECT * FROM product WHERE NOT(price = 800);
```

### ☆ 模糊查询

```sql
# 查询以'香'开头的所有商品
SELECT * FROM product WHERE pname LIKE '香%';
# 查询第二个字为'想'的所有商品
SELECT * FROM product WHERE pname LIKE '_想%';
```

### ☆ 非空查询

```sql
# 查询没有分类的商品
SELECT * FROM product WHERE category_id IS NULL;
# 查询有分类的商品
SELECT * FROM product WHERE category_id IS NOT NULL;
```

## 5、排序查询

```sql
# 通过order by语句，可以将查询出的结果进行排序。暂时放置在select语句的最后。
格式：SELECT * FROM 表名 ORDER BY 排序字段 ASC|DESC;
ASC 升序 (默认)
DESC 降序

# 1.使用价格排序(降序)
SELECT * FROM product ORDER BY price DESC;
# 2.在价格排序(降序)的基础上，以分类排序(降序)
SELECT * FROM product ORDER BY price DESC,category_id DESC;
```

## 6、聚合查询

之前我们做的查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询，它是对一列的值进行计算，然后返回一个单一的值；==另外聚合函数会忽略空值==。

今天我们学习如下五个聚合函数：

| **聚合函数** | **作用**                                                     |
| ------------ | ------------------------------------------------------------ |
| count()      | 统计指定列不为NULL的记录行数；                               |
| sum()        | 计算指定列的数值和，如果指定列类型不是数值类型，则计算结果为0 |
| max()        | 计算指定列的最大值，如果指定列是字符串类型，使用字符串排序运算； |
| min()        | 计算指定列的最小值，如果指定列是字符串类型，使用字符串排序运算； |
| avg()        | 计算指定列的平均值，如果指定列类型不是数值类型，则计算结果为0 |

案例演示：

```sql
# 1、查询商品的总条数
SELECT COUNT(*) FROM product;
# 2、查询价格大于200商品的总条数
SELECT COUNT(*) FROM product WHERE price > 200;
# 3、查询分类为'c001'的所有商品的总和
SELECT SUM(price) FROM product WHERE category_id = 'c001';
# 4、查询分类为'c002'所有商品的平均价格
SELECT AVG(price) FROM product WHERE categ ory_id = 'c002';
# 5、查询商品的最大价格和最小价格
SELECT MAX(price),MIN(price) FROM product;
```

## 7、分组查询与having子句

### ☆ 分组查询介绍

分组查询就是将查询结果按照指定字段进行分组，字段中数据相等的分为一组。

分组的目的就是为了聚合！

**分组查询基本的语法格式如下：**

GROUP BY 列名 [HAVING 条件表达式] [WITH ROLLUP]

**说明:**

- 列名: 是指按照指定字段的值进行分组。
- HAVING 条件表达式: 用来过滤分组后的数据。
- WITH ROLLUP：在所有记录的最后加上一条记录，显示select查询时聚合函数的统计和计算结果!!! 回溯统计

### ☆ group by的使用

group by可用于单个字段分组，也可用于多个字段分组

```sql
-- 根据gender字段来分组
select gender from students group by gender;
-- 根据name和gender字段进行分组
select name, gender from students group by name, gender;
```

① group by可以实现去重操作

② group by的作用是为了实现分组统计（group by + 聚合函数）

### ☆ group by + 聚合函数的使用

```sql
-- 统计不同性别的人的平均年龄
select gender,avg(age) from students group by gender;
-- 统计不同性别的人的个数
select gender,count(*) from students group by gender;
```

![image-20210330145259415](https://github.com/user-attachments/assets/0989bcb7-461a-4668-8ce6-8007836fa1bf)

### ☆ group by + having的使用

having作用和where类似都是过滤数据的，但having是过滤分组数据的，只能用于group by

- where 用于分组前

- having 用于分组后

```sql
-- 根据gender字段进行分组，统计分组条数大于2的
select gender,count(*) from students group by gender having count(*)>2;
```

案例演示：

```sql
#1 统计各个分类商品的个数
SELECT category_id ,COUNT(*) FROM product GROUP BY category_id ;

#2 统计各个分类商品的个数,且只显示个数大于1的信息
SELECT category_id ,COUNT(*) FROM product GROUP BY category_id HAVING COUNT(*) > 1;
```

## 8、limit分页查询

分页查询在项目开发中常见，由于数据量很大，显示屏长度有限，因此对数据需要采取分页显示方式。例如数据共有30条，每页显示5条，第一页显示1-5条，第二页显示6-10条。

 格式：

```sql
SELECT 字段1，字段2... FROM 表名 LIMIT M,N
M: 整数，表示从第几条索引开始，计算方式 （当前页-1）*每页显示条数
N: 整数，表示查询多少条数据
SELECT 字段1，字段2... FROM 表明 LIMIT 0,5
SELECT 字段1，字段2... FROM 表明 LIMIT 5,5
```

## 9、小结

```sql
条件查询：select *|字段名 form 表名 where 条件；
排序查询：SELECT * FROM 表名 ORDER BY 排序字段 ASC|DESC;
聚合查询函数：count()，sum()，max()，min()，avg()。
分组查询：SELECT 字段1,字段2… FROM 表名 GROUP BY 分组字段 HAVING 分组条件;
分页查询：
SELECT 字段1，字段2... FROM 表名 LIMIT M,N
M: 整数，表示从第几条索引开始，计算方式 （当前页-1）*每页显示条数
N: 整数，表示查询多少条数据
```

# 十一、多表查询

## 交叉连接(了解)

没有意思，但是它是所有连接的基础。其功能就是将表1和表2中的每一条数据进行连接。

结果：

字段数 = 表1字段 + 表2的字段

记录数 = 表1中的总数量 * 表2中的总数量（笛卡尔积）

```sql
select * from students cross join classes;
或
select * from students, classes;
```

![image-20210330160813460](https://github.com/user-attachments/assets/5ebe287a-624c-4b08-91a7-f388b399da7c)

## 1、内连接

### ☆ 连接查询的介绍

连接查询可以实现多个表的查询，当查询的字段数据来自不同的表就可以使用连接查询来完成。

连接查询可以分为:

1. 内连接查询
2. 左外连接查询
3. 右外连接查询

### ☆ 内连接查询

查询两个表中符合条件的共有记录

![image-20210329231722765](https://github.com/user-attachments/assets/a3c969a9-ac4e-4091-bdee-9d1196a972e9)

**内连接查询语法格式:**

```sql
select 字段 from 表1 [inner] join 表2 on 表1.字段1 = 表2.字段2;

# 或者

select 字段 from 表1, 表2 where 表1.字段1 = 表2.字段2;
```

**说明:**

- inner join 就是内连接查询关键字
- on 就是连接查询条件

**例1：使用内连接查询学生表与班级表:**

```sql
select * from students as s inner join classes as c on s.cls_id = c.id;
```

### ☆ 小结

- 内连接使用inner join .. on .., on 表示两个表的连接查询条件
- 内连接根据连接查询条件取出两个表的 “交集”

## 2、左外连接

### ☆ 左连接查询

`以左表为主`根据条件查询右表数据，如果根据条件查询右表数据不存在使用`null`值填充

![image-20210329232043956](https://github.com/user-attachments/assets/09bc2f6b-d5b9-479b-a3f8-0a6ea8ffcef5)

**左连接查询语法格式:**

```sql
select 字段 from 表1 left [outer] join 表2 on 表1.字段1 = 表2.字段2
```

**说明:**

- left join 就是左连接查询关键字
- on 就是连接查询条件
- 表1 是左表
- 表2 是右表

**例1：使用左连接查询学生表与班级表:**

```sql
select * from students as s left join classes as c on s.cls_id = c.id;
```

### ☆ 小结

- 左连接使用left join .. on .., on 表示两个表的连接查询条件
- 左连接以左表为主根据条件查询右表数据，右表数据不存在使用null值填充。

## 3、右外连接

### ☆ 右连接查询

以右表为主根据条件查询左表数据，如果根据条件查询左表数据不存在使用null值填充

![image-20210329232137674](media/image-20210329232137674.png)

**右连接查询语法格式:**

```sql
select 字段 from 表1 right [outer] join 表2 on 表1.字段1 = 表2.字段2
```

**说明:**

- right join 就是右连接查询关键字
- on 就是连接查询条件
- 表1 是左表
- 表2 是右表

**例1：使用右连接查询学生表与班级表:**

```sql
select * from students as s right join classes as c on s.cls_id = c.id;
```
## 4、自连接
自己和自己连接，数据之间存在层级关系，记得要取别名

## 5、表与表的关系
①一对一
![](https://github.com/user-attachments/assets/48f5669a-951f-4589-b38c-c16848320fee)
②多对一
![](https://github.com/user-attachments/assets/c2479af5-b57a-4130-84f0-50a278400581)

③多对多 需要建立一张临时表
![](https://github.com/user-attachments/assets/53c92408-e409-4dbe-a44f-a746f58feeba)
### ☆ 小结

- 右连接使用right join .. on .., on 表示两个表的连接查询条件
- 右连接以右表为主根据条件查询左表数据，左表数据不存在使用null值填充。

# 十二、外键约束（扩展）

主键：primary key

外键：foreign key（应用场景：在两表或多表关联的时候设置的，用于标志两个表之间的关联关系）

```powershell
create  table 数据表名称(
	字段名称  字段类型  字段约束[5种情况]
) default charset=utf8;
```

① 主键约束primary key

② 默认值约束default

③ 非空约束not null

④ 唯一约束unique key

⑤ 外键约束foreign key

原则：在一张表中，其是主键。但是在另外一张表中，其是从键（非主键），但是这个字段是两张表的关联字段，我们把这个字段就称之为外键。

## 1、外键约束作用

外键约束:对外键字段的值进行更新和插入时会和引用表中字段的数据进行验证，数据如果不合法则更新和插入会失败，保证数据的有效性。如果合法就会建立关联关系，当我们在外键所在表中进行操作时，另外一个表中的数据也会受到关联。

dage表：

| id编号（主键） | name姓名 |
| -------------- | -------- |
| 1              | 陈浩南   |
| 2              | 乌鸦哥   |

xiaodi表：

| id编号（主键） | name姓名   | dage_id（`外键`） |
| -------------- | ---------- | --------------- |
| 1              | 山鸡       | 1               |
| 2              | 大天二     | 1               |
| 3              | 乌鸦的小弟 | 2               |

> 外键设计原则：保证两张表的关联关系，保证数据的一致性。在选择时，一般在一个表中时关联字段，在另外一个表中是主键，则这个字段建议设置为外键。

## 2、对于已经存在的字段添加外键约束

```sql
-- 为cls_id字段添加外键约束
alter table 数据表 add foreign key(外键字段) references 数据表(主键) 
[on delete cascade| set null] [on update cascade | set null];

# 可以设置 当主键删除或更新时，外键的动作
```
![](https://github.com/user-attachments/assets/fbb2a55a-41fc-4d98-92ad-0ef6d3ffb86e)

## 3、在创建数据表时设置外键约束

```sql
-- 创建一个大哥表
create table dage(
  id int not null auto_increment,
  name varchar(20),
  primary key(id)
) default charset=utf8;
-- 添加测试数据
insert into dage values (null, '陈浩南');
insert into dage values (null, '乌鸦');


-- 创建一个小弟表
create table xiaodi(
  id int not null auto_increment,
  name varchar(20),
  dage_id int,
  primary key(id)
) default charset=utf8;
-- 把dage_id设置为主键
alter table xiaodi add foreign key(dage_id) references dage(id) on delete cascade;

-- 插入测试数据
insert into xiaodi values (null, '山鸡', 1);
insert into xiaodi values (null, '大天二', 1);
insert into xiaodi values (null, '乌鸦的小弟', 2);


-- 测试外键
delete from dage where id = 2;
select * from xiaodi;  -- 看看乌鸦的小弟是否还存在

-- 删除外键
show create table xiaodi; -- 查看外键名称(如xiaodi_ibfk_1)
alter table xiaodi drop foreign key xiaodi_ibfk_1;

```

## 4、删除外键约束

```sql
-- 需要先获取外键约束名称,该名称系统会自动生成,可以通过查看表创建语句来获取名称
show create table 数据表;

-- 获取名称之后就可以根据名称来删除外键约束
alter table 数据表 drop foreign key 外键名;
```

# 十三、子查询(三步走)

## 1、子查询（嵌套查询）的介绍

在一个 select 语句中,嵌入了另外一个 select 语句, 那么被嵌入的 select 语句称之为子查询语句，外部那个select语句则称为主查询.

**主查询和子查询的关系:**

1. 子查询是嵌入到主查询中
2. 子查询是辅助主查询的,要么充当条件,要么充当数据源(数据表)
3. 子查询是可以独立存在的语句,是一条完整的 select 语句

## 2、子查询的使用

**例1. 查询学生表中大于平均年龄的所有学生:**

需求：查询年龄 > 平均年龄的所有学生

前提：① 获取班级的平均年龄值

​			  ② 查询表中的所有记录，判断哪个同学 > 平均年龄值

第一步：写子查询

```sql
select avg(age) from students;
```

第二步：写主查询

```sql
select * from students where age > (平均值);
```

第三步：第一步和第二步进行合并

```sql
select * from students where age > (select avg(age) from students);
```



**例2. 查询学生在班的所有班级名字:**

需求：显示所有有学生的班级名称

前提：① 先获取所有学员都属于那些班级

​	         ② 查询班级表中的所有记录，判断是否出现在①结果中，如果在，则显示，不在，则忽略。

第一步：编写子查询

```sql
select distinct cls_id from students is not null;
```

第二步：编写主查询

```sql
select * from classes where cls_id in (1, 2, 3);
```

第三步：把主查询和子查询合并

```sql
select * from classes where cls_id in (select distinct cls_id from students where cls_id is not null);
```



**例3. 查找年龄最小,成绩最低的学生:**

第一步：获取年龄最小值和成绩最小值

```sql
select min(age), min(score) from student;
```

第二步：查询所有学员信息（主查询）

```sql
select * from students where (age, score) = (最小年龄, 最少成绩);
```

第三步：把第一步和第二步合并

```sql
select * from students where (age, score) = (select min(age), min(score) from students);
```

# 十五、开窗函数(mysql 8.0)

## 1、数据准备

```sql
create table employee (
    empid int,
    ename varchar(20) ,
    deptid int,
    salary decimal(10,2)
) default charset=utf8;

insert into employee values(1,'刘备',10,5500.00);
insert into employee values(2,'赵云',10,4500.00);
insert into employee values(2,'张飞',10,3500.00);
insert into employee values(2,'关羽',10,4500.00);

insert into employee values(3,'曹操',20,1900.00);
insert into employee values(4,'许褚',20,4800.00);
insert into employee values(5,'张辽',20,6500.00);
insert into employee values(6,'徐晃',20,14500.00);

insert into employee values(7,'孙权',30,44500.00);
insert into employee values(8,'周瑜',30,6500.00);
insert into employee values(9,'陆逊',30,7500.00);
```

## 2、开窗函数使用

在不改变原有数据的情况下，增加一列保存聚合之后的结果

* 格式:

```sql
select 
  empid,   #原有的列
  ename,
  deptid,
  开窗函数()/排序函数() over(partition by 分组字段 order by 排序字段) as 别名
from 表名;
where 条件
```

  * partition by ：相当于分组group by  
  * order by ：相当于前面的order by
  * 注意，where在开窗函数之前执行

* 使用:

```sql
select *,
row_number() over (partition by deptid order by salary) as row_n
from employee;
  
  # select *,
  #        row_number() over (order by salary) as row_n
  # from employee;
  
  # 1. 查询每一个部门的薪资排名
  select *,
         row_number() over (partition by deptid order by salary) as row_n,
         rank() over (partition by deptid order by salary) as rank_n,
         dense_rank() over (partition by deptid order by salary) as drank_n
  from employee;
  
  # 开窗函数:
  # row_number：显示排序后的行数
  # rank: 显示名次，可以并列排名，下一个排名会跳跃并列个数
  # dense_rank: 显示名次，可以并列排名，下一个排名不会跳跃
  
  # 2. 查询每个部门薪资排名第2的员工;
  select * from
  (select
      *,
      dense_rank() over (partition by deptid order by salary desc) as row_n
  from employee) c where c.row_n=2;
```