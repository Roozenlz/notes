# MySQL基础

## 数据库（DataBase 简称 ： DB）

>  用于存储和管理数据的仓库。

### 数据库的特点：

1. 持久化存储数据的。其实数据库就是一个文件系统
2. 方便存储和管理数据
3. 使用了统一的方式操作数据库 —— SQL

## MySQL服务的基本操作

> mysqld是MySQL的主程序，服务器端。mysql是MySQL的命令行工具，客户端。

### 服务启动

- net start mysql : 启动mysql的服务

- net stop mysql:关闭mysql服务

### 登录

- mysql -uroot -p密码

- mysql -hip -uroot -p连接目标的密码

- mysql --host=ip --user=root --password=连接目标的密码

### 退出

- exit
- quit

### 更改root密码

```
第一步：修改配置文件免密码登录mysql

1、进入文件：vi /etc/my.cnf

2、按i键表示可以编辑；添加skip-grant-tables；按esc键，输入:wq保存退出

3、重启mysql:sudo service mysqld restart

第二步免密码登录mysql

1、登录：mysql -u root -p

2、提示输入密码按回车进入

3、进入数据库，输入：use mysql；

4、查看root用户信息：select host, user, authentication_string, plugin from user;

5、更新root用户信息，把密码设置为空字符串：update user set authentication_string='' where user='root';

第三步、退出mysql；注释掉/etc/my.cnf文件最后的 skip-grant-tables ；重启：sudo service mysqld restart

第四步：设置密码

1、重新开启一个客户端；

2、登录mysql(这时候还是不用输入密码，因为上面已经把密码设置为空字符串了);

3、修改root用户密码:ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';  

4、退出mysql后就可以用密码登录了
```



## SQL

> Structured Query Language：结构化查询语言，其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。

### SQL通用语法

- SQL 语句可以单行或多行书写，以分号结尾。

- 可使用空格和缩进来增强语句的可读性。

- MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。

- 3 种注释

  - 单行注释: -- 注释内容(--后要有空格) 或 # 注释内容(mysql 特有)

  - 多行注释: /* 注释 */

### SQL分类

- **DDL**(Data Definition Language)数据定义语言用来定义数据库对象：数据库，表，列等。关键字：`create`,`drop`,`alter` 等
- **DML**(Data Manipulation Language)数据操作语言用来对数据库中表的数据进行增删改。关键字：`insert`, `delete`,` update` 等
- **DQL**(Data Query Language)数据查询语言用来查询数据库中表的记录(数据)。关键字：`select`, `where` 等
- **DCL**(Data Control Language)数据控制语言(了解)用来定义数据库的访问权限和安全级别，及创建用户。关键字：`GRANT`， `REVOKE` 等

![SQL分类](http://tc.roozen.xyz/image/SQL%E5%88%86%E7%B1%BB.bmp)

### DDL:操作数据库、表

> create / alter / drop

#### 操作数据库

##### C(Create):创建

```mysql
#创建数据库：
create database 数据库名称;
#创建数据库，判断不存在，再创建：
create database if not exists 数据库名称;
#创建数据库，并指定字符集
create database 数据库名称 character set 字符集名;
#练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
create database if not exists db4 character set gbk;
```



##### R(Retrieve)：查询

```mysql
#查询所有数据库的名称:
show databases;
#查询某个数据库的字符集:查询某个数据库的创建语句
show create database 数据库名称;
```



##### U(Update):修改

```mysql
#修改数据库的字符集
alter database 数据库名称 character set 字符集名称;
```



##### D(Delete):删除

```mysql
#删除数据库
drop database 数据库名称;
#判断数据库存在，存在再删除
drop database if exists 数据库名称;
```



##### 使用数据库

```mysql
#查询当前正在使用的数据库名称
select database();
#使用数据库
use 数据库名称;
```



#### 操作表

##### C(Create):创建

```mysql
create table 表名(
	列名1 数据类型1,
	列名2 数据类型2,
	....
	列名n 数据类型n
);
#注意：最后一列，不需要加逗号（,）
```



###### 数据库类型

```mysql
#最常用的类型
int：整数类型
age int,
double:小数类型
score double(5,2)
date:日期，只包含年月日，yyyy-MM-dd
datetime:日期，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
timestamp:时间错类型	包含年月日时分秒，yyyy-MM-dd HH:mm:ss，如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
varchar：字符串
#姓名最大20个字符，zhangsan 8个字符 ，张三 2个字符
name varchar(20)
```

![数据库数据类型](http://tc.roozen.xyz/image/image-20220627093348478.png)



###### 创建表示例：

```mysql
create table student(
	id int,
	name varchar(32),
	age int ,
	score double(4,1),
	birthday date,
	insert_time timestamp
);
```



###### 复制表

```mysql
create table 表名 like 被复制的表名;	  	
```



#### R(Retrieve)：查询

```mysql
#查询某个数据库中所有的表名称
show tables;
#查看创建表的SQL语句
SHOW CREATE TABLE 表名;
#查询表结构
desc 表名;
```



#### U(Update):修改

```mysql
#修改表名
alter table 表名 rename to 新的表名;
#修改表的字符集
alter table 表名 character set 字符集名称;
#添加一列
alter table 表名 add 列名 数据类型;
#修改列名称 类型
alter table 表名 change 列名 新列名 新数据类型;
alter table 表名 modify 列名 新数据类型;
#删除列
alter table 表名 drop 列名;
```



#### D(Delete):删除

```mysql
#直接删除表
drop table 表名;
#判断表是否存在，如果存在则删除表
drop table  if exists 表名 ;
```



#### 使用表

```mysql
use 表名
```



### DML：增删改表中数据

> insert /update/delete

#### 添加数据

`insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);`

- 列名和值要一一对应。

- 如果表名后，不定义列名，则默认给所有列添加值

  `insert into 表名 values(值1,值2,...值n);`

- 除了数字类型，其他类型需要使用引号(单双都可以)引起来

- 没有添加数据的字段会使用NULL填充

```mysql
insert into student (id,name,age,sex) values (1, '孙悟空', 20, '男');
```





#### 删除数据：

`delete from 表名 [where 字段名=值]`

- ==如果不加条件，则删除表中所有记录。==

- 如果要删除所有记录
  - `delete from 表名;` 不推荐。有多少条记录就会执行多少次删除操作
  - `TRUNCATE TABLE 表名;` 推荐，效率更高，先删除表，然后再创建一张一样的表。

```mysql
-- 带条件删除数据，删除id为1的记录
delete from student where id=1;
-- 不带条件删除数据,删除表中的所有数据
delete from student;
```



#### 修改数据：

`update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];`

- 如果不加任何条件，则会将表中所有记录全部修改。

```mysql
-- 不带条件修改数据，将所有的性别改成女
update student set sex = '女';
-- 带条件修改数据，将id号为2的学生性别改成男
update student set sex='男' where id=2;
-- 一次修改多个列，把id为3的学生，年龄改成26岁，address改成北京
update student set age=26, address='北京' where id=3;
```



### DQL：查询表中的记录

> select /show

#### 查询整张表

`select * from 表名;`



#### 查询的完整语法

```mysql
select 字段列表 from 表名列表
where 条件列表
group by 分组字段
having 分组之后的条件
order by 排序
limit 分页限定
```



#### 基础查询

##### 多个字段的查询

`select 字段名1，字段名2... from 表名;`

##### 去除重复

`SELECT DISTINCT 字段名 FROM 表名;`

```mysql
-- 查询学生来至于哪些地方
select address from student;
-- 去掉重复的记录
select distinct address from student;
```

##### 查询结果参与运算

- 某列数据和固定值运算:`SELECT 列名1 + 固定值 FROM 表名;`

- 某列数据和其他列数据参与运算:`SELECT 列名1 + 列名2 FROM 表名;`

- 计算列一般可以使用四则运算计算一些列的值。

  ```mysql
  select * from student;
  -- 给所有的数学加5分
  select math+5 from student;
  -- 查询math + english的和
  select * from student;
  select *,(math+english) as 总成绩 from student;
  -- as可以省略
  select *,(math+english) 总成绩 from student;
  ```

- 一般只会进行数值型的计算。null参与的运算，计算结果都为null。

##### ifnull(表达式1,表达式2)：

- 表达式1：哪个字段需要判断是否为null
- 表达式2：如果表达式1为null后的替换值。

##### 起别名：as(as也可以省略)

- 对列指定别名:`SELECT 字段名1 AS 别名, 字段名2 AS 别名... FROM 表名;`

- 对列和表同时指定别名:

  `SELECT 字段名1 AS 别名, 字段名2 AS 别名... FROM 表名 AS 表别名;`

  ```mysql
  select st.name as 姓名,age as 年龄 from student as st
  ```

  

#### 条件查询

语法：`SELECT 字段名 FROM 表名 WHERE 条件;`

流程：取出表中的每条数据，满足条件的记录就返回，不满足条件的记录不返回

| 比较运算符                | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| \> 、< 、<= 、>= 、= 、<> | <>在SQL中表示不等于，在mysql中也可以使用!=没有==             |
| BETWEEN...AND             | 在一个范围之内，如：between 100 and 200<br/>相当于条件在100到200之间，包头又包尾 |
| IN( 集合)                 | 集合表示多个值，使用逗号分隔                                 |
| LIKE(模糊查询)            | _:单个任意字符<br/>%：多个任意字符                           |
| IS NULL                   | 查询某一列为NULL的值，注：不能写=NULL                        |

```mysql
-- 查询math分数大于80分的学生
select * from student3 where math>80;
-- 查询english分数小于或等于80分的学生
select * from student3 where english <=80;
-- 查询age等于20岁的学生
select * from student3 where age = 20;
-- 查询age不等于20岁的学生，注：不等于有两种写法
select * from student3 where age <> 20;
select * from student3 where age != 20;
-- 查询id是1或3或5的学生
select * from student3 where id in(1,3,5);
-- 查询id不是1或3或5的学生
select * from student3 where id not in(1,3,5);
-- 查询english成绩大于等于75，且小于等于90的学生
select * from student3 where english between 75 and 90;
-- 查询姓马的学生
select * from student3 where name like '马%';
select * from student3 where name like '马';
-- 查询姓名中包含'德'字的学生
select * from student3 where name like '%德%';
-- 查询姓马，且姓名有两个字的学生
select * from student3 where name like '马_';
```

| 逻辑运算符  | 说明                                  |
| :---------- | ------------------------------------- |
| and  或 &&  | 与，SQL中建议使用前者，后者并不通用。 |
| or  或 \|\| | 或                                    |
| not  或 !   | 非                                    |

```mysql
-- 查询age大于35且性别为男的学生(两个条件同时满足)
select * from student3 where age>35 and sex='男';
-- 查询age大于35或性别为男的学生(两个条件其中一个满足)
select * from student3 where age>35 or sex='男';
-- 查询id是1或3或5的学生
select * from student3 where id=1 or id=3 or id=5;
```

实际操作：

```mysql
-- 查询年龄大于20岁
SELECT * FROM student WHERE age > 20;
SELECT * FROM student WHERE age >= 20;
-- 查询年龄等于20岁
SELECT * FROM student WHERE age = 20;
-- 查询年龄不等于20岁
SELECT * FROM student WHERE age != 20;
SELECT * FROM student WHERE age <> 20;
-- 查询年龄大于等于20 小于等于30
SELECT * FROM student WHERE age >= 20 &&  age <=30;
SELECT * FROM student WHERE age >= 20 AND  age <=30;
SELECT * FROM student WHERE age BETWEEN 20 AND 30;
-- 查询年龄22岁，18岁，25岁的信息
SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
SELECT * FROM student WHERE age IN (22,18,25);
-- 查询英语成绩为null
-- 不对的。null值不能使用 = （!=） 判断
SELECT * FROM student WHERE english = NULL; 
SELECT * FROM student WHERE english IS NULL;
-- 查询英语成绩不为null
SELECT * FROM student WHERE english  IS NOT NULL;
-- 查询姓马的有哪些？ like
SELECT * FROM student WHERE NAME LIKE '马%';
-- 查询姓名第二个字是化的人
SELECT * FROM student WHERE NAME LIKE "_化%";
-- 查询姓名是3个字的人
SELECT * FROM student WHERE NAME LIKE '___';
-- 查询姓名中包含德的人
SELECT * FROM student WHERE NAME LIKE '%德%';
```



#### 排序查询

> 通过ORDER BY子句，可以将查询出的结果进行排序(排序只是显示方式，不会影响数据库中数据的顺序)

`SELECT 字段名 FROM 表名 WHERE 字段=值 ORDER BY 字段名 [ASC|DESC];`

- ASC: 升序，默认值

- DESC: 降序

如果有多个排序条件，则当前一个条件的值一样时，才会判断第二条件。

```mysql
-- 查询所有数据,在年龄降序排序的基础上，如果年龄相同再以数学成绩升序排序
select * from student order by age desc, math asc;
```



##### 聚合函数

>  将一列数据作为一个整体，进行纵向的计算。

`SELECT 聚合函数(列名) FROM 表名;`

- count：计算个数

  一般选择非空的列：主键

  count(*)

- max：计算最大值
- min：计算最小值
- sum：计算和
- avg：计算平均值

==聚合函数的计算，排除null值。==



```mysql
-- 查询学生总数
select count(id) as 总人数 from student;
select count(*) as 总人数 from student;
```



##### IFNULL(列名，默认值)

> 如果列名不为空，返回这列的值。如果为NULL，则返回默认值。

```mysql
#查询id字段，如果为null，则使用0代替
select ifnull(id,0) from student;
#我们可以利用IFNULL()函数，如果记录为NULL，给个默认值，这样统计的数据就不会遗漏
select count(ifnull(id,0)) from student;
```



示例：

```mysql
-- 查询年龄大于20的总数
select count(*) from student where age>20;
-- 查询数学成绩总分
select sum(math) 总分 from student;
-- 查询数学成绩平均分
select avg(math) 平均分 from student;
-- 查询数学成绩最高分
select max(math) 最高分 from student;
-- 查询数学成绩最低分
select min(math) 最低分 from student;
```



#### 分组查询

> 分组查询是指使用 GROUP BY语句对查询信息进行分组，相同数据作为一组

`SELECT 字段1,字段2... FROM 表名 GROUP BY 分组字段 [HAVING 条件];`

![image-20220627135430450](http://tc.roozen.xyz/image/image-20220627135430450.png)

GROUP BY将分组字段结果中相同内容作为一组，并且返回每组的第一条数据，所以单独分组没什么用处。分组的目的就是为了统计，**分组一般会跟聚合函数一起使用**。

```mysql
-- 按性别进行分组，求男生和女生数学的平均分
select sex, avg(math) from student3 group by sex;
```

当我们使用某个字段分组,在查询的时候也需要将这个字段查询出来,否则看不到数据属于哪组的。

**having与where的区别：**

| 子句       | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| **where**  | 1.对查询结果进行分组前，将不符合where条件的行去掉，即在**分组之前**过滤数据，即**先过滤再分组**。<br/>2.where后面**不可以**使用聚合函数 |
| **having** | 1.having 子句的作用是筛选满足条件的组，即在**分组之后**过滤数据，即**先分组再过滤**。<br/>2.having后面**可以**使用聚合函数 |

```mysql
-- 对分组查询的结果再进行过滤
-- 查询年龄大于25岁的人，按性别分组，统计每组的人数，并只显示性别人数大于2的数据
SELECT sex, COUNT(*) as 人数 FROM student3 WHERE age > 25 GROUP BY sex having 人数 >2;
```



示例：

```mysql
-- 按照性别分组。分别查询男、女同学的平均分
SELECT sex , AVG(math) FROM student GROUP BY sex;	
-- 按照性别分组。分别查询男、女同学的平均分,人数	
SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex;	
--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;
SELECT sex , AVG(math),COUNT(id) 人数 FROM student WHERE math > 70 GROUP BY sex HAVING 人数 > 2;
```



#### 分页查询

LIMIT是限制的意思，所以LIMIT的作用就是限制查询记录的条数。

`SELECT *|字段列表 [as 别名] FROM 表名 [WHERE子句] \[GROUP BY子句\]\[HAVING子句\]\[ORDER BY子句\]\[LIMIT子句\];`

语法：limit 开始的索引,每页查询的条数;

公式：开始的索引 = （当前的页码 - 1） * 每页显示的条数

limit 是一个MySQL"方言"

```mysql
-- 每页显示3条记录 
SELECT * FROM student LIMIT 0,3; -- 第1页
SELECT * FROM student LIMIT 3,3; -- 第2页
SELECT * FROM student LIMIT 6,3; -- 第3页
```



### DCL：管理用户，授权

> grant /revoke

#### 用户管理

##### 添加用户

`CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`

主机名：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%。

创建的用户名都在mysql数据库中的user表中可以查看到，密码经过了加密。

```mysql
-- 创建user1用户，只能在localhost这个服务器登录mysql服务器，密码为123
create user 'user1'@'localhost' identified by '123';
-- 创建user2用户可以在任何电脑上登录mysql服务器，密码为123
create user 'user2'@'%' identified by '123';
```



##### 删除用户

`DROP USER '用户名'@'主机名';`



##### 修改密码

###### 修稿root用户密码

`mysqladmin -uroot -p password 新密码`

需要在未登陆MySQL的情况下操作，新密码不需要加上引号。



###### 修改普通用户密码

`set password for '用户名'@'主机名' = password('新密码');`

需要在登陆MySQL的情况下操作，新密码要加单引号。

`UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';`

```mysql
UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');
```



##### 查询用户

```mysql
-- 1. 切换到mysql数据库
USE myql;
-- 2. 查询user表
SELECT * FROM USER;
```



#### 权限管理

> 用户创建之后，没有任何权限！需要给用户授权

##### 查询权限

`SHOW GRANTS FOR '用户名'@'主机名';`

```mysql
SHOW GRANTS FOR 'lisi'@'%';
```

usage是指连接（登陆）权限，建立一个用户，就会自动授予其usage权限（默认授予）。



##### 授予权限

`grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';`

授予用户的权限，如CREATE、ALTER、SELECT、INSERT、UPDATE等。如果要授予所有的权限则使用ALL；如果要授予该用户对所有数据库和表的相应操作权限则可用\*表示，如\*.\*

```mysql
-- 给张三用户授予所有权限，在任意数据库任意表上
GRANT ALL ON *.* TO 'zhangsan'@'localhost';
-- 给user1用户分配对test这个数据库操作的权限：创建表，修改表，插入记录，更新记录，查询
grant create,alter,insert,update,select on test.* to 'user1'@'localhost';
```



##### 撤销权限

`revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';`

```mysql
REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
-- 撤销user1用户对test数据库所有表的操作的权限
revoke all on test.* from 'user1'@'localhost';
```



## 备份与还原

### 备份

DOS下，未登录的时候执行`mysqldump -u用户名 -p密码 数据库 > 文件的路径`

数据库中的所有表和数据都会导出成SQL语句

```mysql
-- 备份db数据库中的数据到d:\db.sql文件中
mysqldump -uroot -proot db > d:/db.sql
```

### 还原

还原的时候需要先登录MySQL,并选中对应的数据库。

```mysql
use db;
source d:/db.sql;
```



## 约束

> 对表中的数据进行限制，保证数据的正确性、有效性和完整性。一个表如果添加了约束，不正确的数据将无法插入到表中。约束在创建表的时候添加比较合适。

### 约束种类

| 约束名   | 约束关键字  |
| -------- | ----------- |
| 主键     | primary key |
| 唯一     | unique      |
| 非空     | not null    |
| 外键     | foreign key |
| 检查约束 | mysql不支持 |

### 主键约束

> 通常不用业务字段作为主键，单独给每张表设计一个id的字段，把id作为主键。
>
> 主键是给数据库和程序使用的，不是给最终的客户使用的。
>
> 所以主键有没有含义没有关系，只要**唯一**，**非空**就行。
>
> 一张表只能有一个字段为主键。
>
> 如：身份证，学号不建议做成主键

#### 创建表时添加主键约束

```mysql
create table st5 (
	id int primary key, -- id为主键
	name varchar(20),
	age int
)
```

#### 删除主键

```mysql
alter table st5 drop primary key;
```

#### 创建完表后添加主键

```mysql
alter table st5 add primary key(id);
```

#### 主键自增

> 如果某一列是整数类型的，使用 `auto_increment` 可以来完成值得自动增长

##### 在创建表时，添加主键约束，并且完成主键自增长

```mysql
create table stu(
	id int primary key auto_increment,-- 给id添加主键约束
	name varchar(20)
);
```

##### 删除自动增长

```mysql
ALTER TABLE stu MODIFY id INT;
```

##### 添加自动增长

```mysql
ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
```

##### 自动增长的使用

```mysql
-- 插入数据
insert into st6 (name,age) values ('小乔',18);
insert into st6 (name,age) values ('大乔',20);
-- 另一种写法
insert into st6 values(null,'周瑜',35);
```

##### 修改自增长的默认值起始值

> AUTO_INCREMENT 的默认起始值是1

- 创建表时指定起始值

  `CREATE TABLE 表名(
  列名 int primary key AUTO_INCREMENT
  ) AUTO_INCREMENT=起始值;`

  ```mysql
  -- 指定起始值为1000
  create table st4 (
  id int primary key auto_increment,
  name varchar(20)
  ) auto_increment = 1000;
  ```

- 创建好以后修改起始值

  `ALTER TABLE 表名 AUTO_INCREMENT=起始值;`

  ```mysql
  alter table st4 auto_increment = 2000;
  ```

- DELETE和TRUNCATE删除记录对自增长的影响

  - DELETE：删除所有的记录之后，自增长没有影响，接着之前的数字增长。
  - TRUNCATE：删除以后，自增长又重新从起始值开始。



### 唯一约束

> 表中某一列不能出现重复的值，唯一约束可以有多个NULL值，null表示没有数据，不存在重复的问题

#### 创建表时添加唯一约束

```mysql
CREATE TABLE stu(
	id INT,
	phone_number VARCHAR(20) UNIQUE -- 手机号
);
```

#### 删除唯一约束

```mysql
ALTER TABLE stu DROP INDEX phone_number;
```

#### 表创建完后添加唯一约束

```mysql
ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
```



### 非空约束

> 某一列不能为null。

#### 创建表时添加约束

```mysql
CREATE TABLE stu(
	id INT,
	NAME VARCHAR(20) NOT NULL -- name为非空
);
```

#### 表创建完后添加非空约束

```mysql
ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;
```

#### 删除非空约束

```mysql
ALTER TABLE stu MODIFY NAME VARCHAR(20);
```



### 非空且唯一约束，与主键约束的区别

1. 主键在一个表中，只能有一个
2. 主键可以单列，也可以是多列
3. 自增长只能用在主键上



### 外键约束

外键：在从表中与主表主键对应的那一列

主表： 一方，用来约束别人的表

从表： 多方，被别人约束的表

![image-20220627151802859](http://tc.roozen.xyz/image/image-20220627151802859.png)

#### 创建表时添加外键

```mysql
create table 表名(
	....
	外键列
	constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
);
```

#### 删除外键

```mysql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
-- 删除employee表的emp_depid_fk外键
alter table employee drop foreign key emp_depid_fk;
```

#### 创建表后添加外键

```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
-- 在employee表存在的情况下添加外键
alter table employee add constraint emp_depid_fk
foreign key (dep_id) references department(id);
```

#### 级联操作

> 在修改和删除主表的主键时，同时更新或删除副表的外键值

##### 添加级联操作

```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
	FOREIGN KEY (外键字段名称) 
	REFERENCES 主表名称(主表列名称) 
	ON UPDATE CASCADE
	ON DELETE CASCADE;
```

##### 分类

- 级联更新：ON UPDATE CASCADE

  级联更新，只能是创建表的时候创建级联关系。更新主表中的主键，从表中的外键列也自动同步更新

- 级联删除：ON DELETE CASCADE



实例：

```mysql
-- 创建部门表(id,dep_name,dep_location)
-- 一方，主表
create table department(
id int primary key auto_increment,
dep_name varchar(20),
dep_location varchar(20)
);

-- 多方，从表
create table employee(
id int primary key auto_increment,
name varchar(20),
age int,
dep_id int, -- 外键对应主表的主键
-- 创建外键约束
constraint emp_depid_fk foreign key (dep_id) references department(id)
)
```



### 默认约束

`字段名 字段类型 DEFAULT 默认值`

```mysql
-- 创建一个学生表 st9，包含字段(id,name,address)， 地址默认值是广州
create table st9 (
id int,
name varchar(20),
address varchar(20) default '广州'
)
-- 添加一条记录,使用默认地址
insert into st9 values (1, '李四', default);
select * from st9;
insert into st9 (id,name) values (2, '李白');
-- 添加一条记录,不使用默认地址
insert into st9 values (3, '李四光', '深圳');
```

==注意：默认约束只有在不给值时才会采用默认值。如果给了null，那值就是null值。==

## 数据库的设计

### 多表之间的关系

- 一对一(了解)

  > 一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能

  如：人和身份证

  分析：一个人只有一个身份证，一个身份证只能对应一个人
  
  实现方式：一对一关系实现，可以在任意一方添加唯一外键指向另一方的主键。


- 一对多(多对一)：

  如：部门和员工

  分析：一个部门有多个员工，一个员工只能对应一个部门

  实现方式：在多的一方建立外键，指向一的一方的主键。

- 多对多

  如：学生和课程

  分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择

  实现方式：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键。

```mysql
-- 创建旅游线路分类表 tab_category
-- cid 旅游线路分类主键，自动增长
-- cname 旅游线路分类名称非空，唯一，字符串 100
CREATE TABLE tab_category (
cid INT PRIMARY KEY AUTO_INCREMENT,
cname VARCHAR(100) NOT NULL UNIQUE
);

-- 创建旅游线路表 tab_route
/*
rid 旅游线路主键，自动增长
rname 旅游线路名称非空，唯一，字符串 100
price 价格
rdate 上架时间，日期类型
cid 外键，所属分类
*/
CREATE TABLE tab_route(
rid INT PRIMARY KEY AUTO_INCREMENT,
rname VARCHAR(100) NOT NULL UNIQUE,
price DOUBLE,
rdate DATE,
cid INT,
FOREIGN KEY (cid) REFERENCES tab_category(cid)
);

/*创建用户表 tab_user
uid 用户主键，自增长
username 用户名长度 100，唯一，非空
password 密码长度 30，非空
name 真实姓名长度 100
birthday 生日
sex 性别，定长字符串 1
telephone 手机号，字符串 11
email 邮箱，字符串长度 100
*/
CREATE TABLE tab_user (
uid INT PRIMARY KEY AUTO_INCREMENT,
username VARCHAR(100) UNIQUE NOT NULL,
PASSWORD VARCHAR(30) NOT NULL,
NAME VARCHAR(100),
birthday DATE,
sex CHAR(1) DEFAULT '男',
telephone VARCHAR(11),
email VARCHAR(100)
);

/*
创建收藏表 tab_favorite
rid 旅游线路 id，外键
date 收藏时间
uid 用户 id，外键
rid 和 uid 不能重复，设置复合主键，同一个用户不能收藏同一个线路两次
*/
CREATE TABLE tab_favorite (
rid INT, -- 线路id
DATE DATETIME,
uid INT, -- 用户id
-- 创建复合主键
PRIMARY KEY(rid,uid), -- 联合主键
FOREIGN KEY (rid) REFERENCES tab_route(rid),
FOREIGN KEY(uid) REFERENCES tab_user(uid)
);
```

### 数据库设计的范式

> 设计数据库时，需要遵循的一些规范。
>
> 要遵循后边的范式要求，必须先遵循前边的所有范式要求

**设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。**

#### 分类

1. 第一范式（1NF）：每一列都是不可分割的原子数据项

   原子性：表中每列不可再拆分。

2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）

   不产生局部依赖，一张表只描述一件事情。表中的每一列都完全依赖于主键。

3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

   不产生传递依赖，表中每一列都直接依赖于主键。而不是通过其它列间接依赖于主键。

#### 相关概念

**函数依赖**：A-->B，如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A
例如：学号-->姓名。  （学号，课程名称） --> 分数

**完全函数依赖**：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。例如：（学号，课程名称） --> 分数

**部分函数依赖**：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。例如：（学号，课程名称） -- > 姓名

**传递函数依赖**：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A，例如：学号-->系名，系名-->系主任

**码**：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码，例如：该表中码为：（学号，课程名称）

**主属性**：码属性组中的所有属性

**非主属性**：除过码属性组的属性



## 多表查询

### 笛卡尔积

> 有两个集合A,B .取这两个集合的所有组成情况。要完成多表查询，需要消除无用的数据

### 分类

#### 内连接查询

##### 内连接查询步骤:

1. 确定查询哪些表
2. 确定表连接的条件
3. 确定查询的条件
4. 确定查询的字段

##### 隐式内连接

> 看不到join关键字,使用where条件消除无用数据

`SELECT 字段名 FROM 左表, 右表 WHERE 条件`

例子：

```mysql
-- 查询所有员工信息和对应的部门信息
SELECT * FROM emp,dept WHERE emp.`dept_id` = dept.`id`;

-- 查询员工表的名称，性别。部门表的名称
SELECT emp.name,emp.gender,dept.name FROM emp,dept WHERE emp.`dept_id` = dept.`id`;
-- 最规范的写法
SELECT 
t1.name, -- 员工表的姓名
t1.gender,-- 员工表的性别
t2.name -- 部门表的名称
FROM
emp t1,
dept t2
WHERE 
t1.`dept_id` = t2.`id`;
```



##### 显式内连接

> 使用INNER JOIN ... ON语句, 可以省略INNER

`select 字段列表 from 表名1 [inner] join 表名2 on 条件`

例如：

```mysql
SELECT * FROM emp INNER JOIN dept ON emp.`dept_id` = dept.`id`;	
SELECT * FROM emp JOIN dept ON emp.`dept_id` = dept.`id`;
```



#### 外链接查询

##### 左外连接

> 用左边表的记录去匹配右边表的记录，如果符合条件的则显示；否则，显示NULL
> 可以理解为：在内连接的基础上保证左表的数据全部显示。查询的是左表所有数据以及其交集部分。

<img src="http://tc.roozen.xyz/image/image-20220628095844441.png" alt="image-20220628095844441" style="zoom:200%;" />

`select 字段列表 from 左表 left [outer] join 右表 on 条件;`

例子：

```mysql
-- 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
SELECT 	t1.*,t2.`name` FROM emp t1 LEFT JOIN dept t2 ON t1.`dept_id` = t2.`id`;
```

##### 右外连接

> 用右边表的记录去匹配左边表的记录，如果符合条件的则显示；否则，显示NULL
> 可以理解为：在内连接的基础上保证右表的数据全部显示。查询的是右表所有数据以及其交集部分。

<img src="http://tc.roozen.xyz/image/image-20220628100227739.png" alt="image-20220628100227739" style="zoom:200%;" />

`select 字段列表 from 左表 right [outer] join 右表 on 条件;`

例子：

```mysql
SELECT 	* FROM dept t2 RIGHT JOIN emp t1 ON t1.`dept_id` = t2.`id`;
```

#### 子查询

> 查询中嵌套查询，称嵌套查询为子查询。一个查询的结果做为另一个查询的条件。子查询要使用括号。

例如：

```mysql
-- 查询工资最高的员工信息
-- 1 查询最高的工资是多少 9000
SELECT MAX(salary) FROM emp;
			
-- 2 查询员工信息，并且工资等于9000的
SELECT * FROM emp WHERE emp.`salary` = 9000;
			
-- 一条sql就完成这个操作。子查询
SELECT * FROM emp WHERE emp.`salary` = (SELECT MAX(salary) FROM emp);
```



##### 单行单列

> 子查询结果只要是单行单列，肯定在WHERE后面作为条件，父查询使用：比较运算符，如：> 、<、<>、= 等

`SELECT 查询字段 FROM 表 WHERE 字段=（子查询）;`

```mysql
-- 查询员工工资小于平均工资的人
SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp);
-- 查询工资最高的员工
select * from emp where salary = (select max(salary) from emp);
```



##### 多行单列

> 子查询结果是单例多行，结果集类似于一个数组，父查询使用IN运算符

`SELECT 查询字段 FROM 表 WHERE 字段 IN （子查询）;`

```mysql
-- 查询'财务部'和'市场部'所有的员工信息
SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部';
SELECT * FROM emp WHERE dept_id = 3 OR dept_id = 2;
-- 子查询
SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部');
```



##### 多行多列的

> 子查询结果只要是多列，肯定在FROM后面作为虚拟表参与查询

`SELECT 查询字段 FROM （子查询） 表别名 WHERE 条件;`

```mysql
-- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
-- 子查询
SELECT * FROM 
dept t1 ,
(SELECT * FROM emp WHERE emp.`join_date` > '2011-11-11') t2
WHERE t1.id = t2.dept_id;
-- 普通内连接
SELECT * FROM emp t1,dept t2 WHERE t1.`dept_id` = t2.`id` AND t1.`join_date` >  '2011-11-11'
```



##### 小结

- 子查询结果只要是单列，则在WHERE后面作为条件
- 子查询结果只要是多列，则在FROM后面作为表进行二次查询



## 事务

> 在实际的开发过程中，一个业务操作如：转账，往往是要多次访问数据库才能完成的。转账是一个用户扣钱，另一个用户加钱。如果其中有一条SQL语句出现异常，这条SQL就可能执行失败。事务执行是一个整体，所有的SQL语句都必须执行成功。如果其中有1条SQL语句出现异常，则所有的SQL语句都要回滚，整个业务执行失败。

### 事务提交的两种方式

- 自动提交：一条DML(增删改)语句会自动提交一次事务。

  mysql 数据库默认是自动提交事务

- 手动提交：需要先开启事务，再提交。

  Oracle 数据库默认是手动提交事务

查看事务的默认提交方式：`SELECT @@autocommit;`

1 代表自动提交  0 代表手动提交

修改默认提交方式： `set @@autocommit = 0;`

@@表示全局变量。



### 手动提交事务的SQL语句

开启事务： `start transaction;`或者`BEGIN;`

回滚：`rollback;`

提交：`commit;`



### 事务的四大特征

1. 原子性（Atomicity）：每个事务都是一个整体，不可再拆分，事务中所有的SQL语句要么都执行成功，要么都失败。
2. 持久性（Durability）：一旦事务执行成功，对数据库的修改是持久的。就算关机，也是保存下来的。
3. 隔离性（Isolation）：事务与事务之间不应该相互影响，执行时保持隔离的状态。
4. 一致性（Consistency）：事务在执行前数据库的状态与执行后数据库的状态保持一致。如：转账前2个人的总金额是2000，转账后2个人总金额也是2000



### 事务原理

> 事务开启之后, 所有的操作都会临时保存到事务日志中, 事务日志只有在得到commit命令才会同步到数据表中，其他任何情况都会清空事务日志(rollback，断开连接)

![image-20220628103126208](http://tc.roozen.xyz/image/image-20220628103126208.png)



### 回滚点

> 在某些成功的操作完成之后，后续的操作有可能成功有可能失败，但是不管成功还是失败，前面操作都已经成功，可以在当前成功的位置设置一个回滚点。可以供后续失败操作返回到该位置，而不是返回所有操作，这个点称之为回滚点。

#### 回滚点的操作语句

设置回滚点：`savepoint 名字`

回到回滚点：`rollback to 名字`

**设置回滚点可以让我们在失败的时候回到回滚点，而不是回到事务开启的时候。**



### 事务的隔离级别

事务在操作时的理想状态： 所有的事务之间保持隔离，互不影响。因为并发操作，多个用户同时访问同一个数据。可能引发并发访问的问题：

1. 脏读：一个事务读取到了另一个事务中尚未提交的数据
2. 不可重复读：一个事务中两次读取的数据内容不一致，要求的是一个事务中多次读取时数据是一致的，这是事务update时引发的问题
3. 幻读：一个事务中两次读取的数据的数量不一致，要求在一个事务多次读取的数据的数量是一致的，这是insert或delete时引发的问题



#### 数据库有四种隔离级别

1. **read uncommitted**：读未提交

   产生的问题：脏读、不可重复读、幻读

2. **read committed**：读已提交 （Oracle）

   产生的问题：不可重复读、幻读

3. **repeatable read**：可重复读 （MySQL默认）

   产生的问题：幻读

4. **serializable**：串行化

   一个事务没有执行完，其他事务的SQL执行不了，可以挡住幻读

**隔离级别从小到大安全性越来越高，但是效率越来越低**



#### 事务隔离级别相关的命令

查询隔离级别：`select @@tx_isolation;`

设置隔离级别：`set global transaction isolation level 级别字符串;`

设置事务隔离级别，需要退出MySQL再重新登录才能看到隔离级别的变化



