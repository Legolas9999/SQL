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
desc 数据表名称;
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


## 修改字段名称

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
| ==tinyint==      | 很小的整数     | -128 ~ 127               |
| smallint         | 小的整数       | -32768 ~ 32767           |
| mediumint        | 中等大小的整数 | -8388608 ~ 8388607       |
| ==int(integer)== | 普通大小的整数 | -2147483648 ~ 2147483647 |

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

| 份额里       | 类型名称                                                     |
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

# 八、DML数据操作语言

## 1、DML包括哪些SQL语句

insert插入、update更新、delete删除

## 2、数据的增删改（重点）

英语小课堂：

增加：insert

删除：delete

修改：update

### ☆ 数据的增加操作

基本语法：

```sql
mysql> insert into 数据表名称([字段1,字段2,字段3...]) values (字段1的值,字段2的值,字段3的值...);
```

> 特别注意：在SQL语句中，除了数字，其他类型的值，都需要使用引号引起来，否则插入时会报错。

第一步：准备一个数据表

```sql
mysql> use db_itheima;
mysql> create table tb_user(
	id int,
	username varchar(20),
	age tinyint unsigned,
	gender enum('男','女','保密'),
	address varchar(255)
) engine=innodb default charset=utf8;
```

> unsigned代表无符号型，只有0到正数。tinyint unsigned无符号型，范围0 ~ 255

> enum枚举类型，多选一。只能从给定的值中选择一个

第二步：使用insert语句插入数据

```sql
mysql> insert into tb_user values (1,'刘备',34,'男','广州市天河区');
mysql> insert into tb_user(id,username,age) values (2,'关羽',33);
```

第三步：批量插入多条数据

```sql
mysql> insert into tb_user values (3,'大乔',19,'女','上海市浦东新区'),(4,'小乔',18,'女','上海市浦东新区'),(5,'马超',26,'男','北京市昌平区');
```

### ☆ 数据的修改操作

基本语法：

```sql
mysql> update 数据表名称 set 字段1=更新后的值,字段2=更新后的值,... where 更新条件;
```

> 特别说明：如果在更新数据时，不指定更新条件，则其会把这个数据表的所有记录全部更新一遍。

案例：修改username='马鹏'这条记录，将其性别更新为男，家庭住址更新为广东省深圳市

```sql
mysql> update tb_user set gender='男',address='广东省深圳市' where username='马鹏';
```

案例：今年是2020年，假设到了2021年，现在存储的学员年龄都差1岁，整体进行一次更新

```sql
mysql> update tb_user set age=age+1;
```

###  ☆ 数据的删除操作

基本语法：

```sql
mysql> delete from 数据表名称 [where 删除条件];
```

案例：删除tb_user表中，id=1的用户信息

```sql
mysql> delete from tb_user where id=1;
```



delete from与truncate清空数据表操作

```sql
mysql> delete from 数据表;
或
mysql> truncate 数据表;
```



delete from与truncate区别在哪里？

- delete：删除==数据记录==
  - 数据操作语言（DML）
  - 删除==大量==记录速度慢，==只删除数据，主键自增序列不清零==
  - 可以==带条件==删除
- truncate：删除==所有数据记录==
  - 数据定义语言（DDL）
  - 清里大量数据==速度快==，==主键自增序列清零==
  - ==不能带条件删除==

# 九、SQL约束

## 1、主键约束

1、PRIMARY KEY 约束唯一标识数据库表中的每条记录。
2、主键必须包含唯一的值。
3、主键列不能包含 NULL 值。
4、每个表都应该有一个主键，并且每个表只能有一个主键。

遵循原则：

1）主键应当是对用户没有意义的
2）永远也不要更新主键。
3）主键不应包含动态变化的数据，如时间戳、创建时间列、修改时间列等。
4） 主键应当由计算机自动生成。

创建主键约束：创建表时，在字段描述处，声明指定字段为主键

![image-20210906183745166](media/image-20210906183745166.png)

删除主键约束：如需撤销 PRIMARY KEY 约束，请使用下面的 SQL

```sql
alter table persons2 drop primary key;
```

![image-20210906183908865](media/image-20210906183908865.png)



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

运行效果：

![image-20210906184220825](media/image-20210906184220825.png)

## 2、非空约束

NOT NULL 约束强制列不接受 NULL 值。
NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。
下面的 SQL 语句强制 "id" 列和 "last_name" 列不接受 NULL 值：

![image-20210906184938237](media/image-20210906184938237.png)

## 3、唯一约束

UNIQUE 约束唯一标识数据库表中的每条记录。
UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。
PRIMARY KEY 拥有自动定义的 UNIQUE 约束。

请注意：
每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。

![image-20210906185040009](media/image-20210906185040009.png)

## 4、默认值约束

default 默认值

## 5、外键约束(了解)

外键约束（==多表==关联使用）

比如：有两张数据表，这两个数据表之间==有联系==，通过了==某个字段==可以建立连接，这个字段在其中一个表中是==主键==，在另外一张表中，我们就把其称之为==外键==。

foreign key

手机分类（iPhone 13 Pro，Mi MAX，HuaWei）

分类表

手机



产品表

iPhone 13 Pro

Mi MAX

HuaWei

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

```mysql
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

**分组查询基本的语法格式如下：**

GROUP BY 列名 [HAVING 条件表达式] [WITH ROLLUP]

**说明:**

- 列名: 是指按照指定字段的值进行分组。
- HAVING 条件表达式: 用来过滤分组后的数据。
- WITH ROLLUP：在所有记录的最后加上一条记录，显示select查询时聚合函数的统计和计算结果

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

![image-20210330145259415](media/image-20210330145259415.png)

### ☆ group by + having的使用

having作用和where类似都是过滤数据的，但having是过滤分组数据的，只能用于group by

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

![image-20210330160813460](media/image-20210330160813460.png)

## 1、内连接

### ☆ 连接查询的介绍

连接查询可以实现多个表的查询，当查询的字段数据来自不同的表就可以使用连接查询来完成。

连接查询可以分为:

1. 内连接查询
2. 左外连接查询
3. 右外连接查询

### ☆ 内连接查询

查询两个表中符合条件的共有记录

![image-20210329231722765](media/image-20210329231722765.png)

**内连接查询语法格式:**

```sql
select 字段 from 表1 inner join 表2 on 表1.字段1 = 表2.字段2
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

以左表为主根据条件查询右表数据，如果根据条件查询右表数据不存在使用null值填充

![image-20210329232043956](media/image-20210329232043956.png)

**左连接查询语法格式:**

```sql
select 字段 from 表1 left join 表2 on 表1.字段1 = 表2.字段2
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
select 字段 from 表1 right join 表2 on 表1.字段1 = 表2.字段2
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

### ☆ 小结

- 右连接使用right join .. on .., on 表示两个表的连接查询条件
- 右连接以右表为主根据条件查询左表数据，左表数据不存在使用null值填充。

# 十二、子查询(三步走)

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

## 3、小结

子查询是一个完整的SQL语句，子查询被嵌入到一对小括号里面

掌握子查询编写三步走