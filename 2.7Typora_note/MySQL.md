[TOC]

# 数据库及表操作

###  [了解]数据库

- 什么四数据库？
  - 储存和管理数据的仓库

- 分类
  - 关系型数据库(sql)
- 非关系型数据库(nosql)
- 作用
  - 储存和管理数据
- 特点
  - 持久化存储
  - 读写速度快
  - 可以保证数据的有效性

### 关系型数据库管理系统

- 关系型数据库管理系统：RDBMS

  - 概念：是一个用于关系型数据库的软件
  - 组成：
    - 客户端
    - 服务端

- sql介绍：

  - 主要分成：
    - DQL：数据查询语言，主要用于数据库查询; select
    - DML:   数据库操作语言，主要用于数据的增，删，改；insert,delete,update
    - DDL：数据定义语言，主要用于数据库和表的操作，create,drop

  ### 3.0   MySQL数据库

  > ```
  > 首先登录MySQL。  
  > 格式：mysql> set password for 用户名@localhost = password('新密码');  
  > 例子：mysql> set password for root@localhost = password('123');  
  > 
  > 
  > 方法2：用mysqladmin   
  > 格式：mysqladmin -u用户名 -p旧密码 password 新密码  
  > 例子：mysqladmin -uroot -p123456 password 123  
  > ```

- 介绍：MySQL数据库关系系统

- 服务端使用

  - 安装：

    ```shell
    # 安装前注意要先更新，不然容易挂掉的。
    sudo apt-get install mysql-server
    ```

  - 停止：

    ```
    sudo service mysql stop
    ```

  - 重启

    ```shell
    sudo service mysql start
    ```

  - 查看状态

    ```shell
    service mysql status
    ```

  - 配置文件位置

    > /etc/mysql/mysql.conf.d/mysqld.cnf	

- 客户端使用

  - 安装：

    ```shell
    sudo apt-get mysql-client
    ```

  - 登录

    ```mysql
    -- 登录本地mysql服务器
    sudo mysql -u root -p password
    -- 登录远程mysql服务器
    sudo mysql -h 192.168.43.88 -u root -p password
    -- 登录远程服务器前需要修改两处
    	-- /etc/mysql/mysql.conf.d/mysqld.cnf  注释掉127.0.0.1  
    	-- 进到mysql里面；在mysql下面的user里面，把需要远程登录的user的host改成%	
    
    ```

  - 修改root账号密码

    ```shell
    # 获取超级管理的账号密码 sudo cat /etc/mysql/debian.cnf 
    host     = localhost
    user     = debian-sys-maint  # 用户名
    password = 0gN5MHQbWSmMVyPo  # 密码
    socket   = /var/run/mysqld/mysqld.sock 
    #使用超级管理 用户名 和 密码 登录mysql
    mysql -u debian-sys-maint -p 0gN5MHQbWSmMVyPo
    ```

    ```mysql
    -- 使用超级用户修改密码  
    use mysql
    update user set authentication_string=password('mysql') where user='root';
    update user set plugin='mysql_native_password' where user = 'root';
    -- 修改后要重启mysql 
    sudo service mysql restart 
    
    ```

  - **权限设置**
  
    ~~~mysql
    -- 1.新建MySQL用户
    
    $ create user username identified by '123456';
    -- 2.授权itcast用户访问meiduo_mall数据库
    
    $ grant all on sec.* to 'web'@'%';
    -- 3.授权结束后刷新特权
    
    $ flush privileges;
    ~~~
  
    

### 数据类型和约束

- 作用：

  > 保证数据的准确性和完整性

- 数据类型：

  - 整数：int 4B(字节)，bit 1b(位)      #  int unsigned 无负号整型
  - 小数：decimal(总位数，小数位数)
  - 字符串：varchar(可变)，char(固定)
  - 日期：date,time,datetime
  - 枚举：enum 枚举

- 数据约束

  - 主键：primary key 			默认 :非空，唯一
  - 非空:not null
  - 唯一：nuique
  - 默认值：default
  - 外键：foreign key
  - 自动增长：auto_increment

| CAHR(Length)              | Length字节                         | 定长字段，长度为0~255个字符                                  |
| ------------------------- | ---------------------------------- | ------------------------------------------------------------ |
| VARCHAR(Length)           | String长度+1字节或String长度+2字节 | 变长字段，长度为0~65 535个字符                               |
| TINYTEXT                  | String长度+1字节                   | 字符串，最大长度为255个字符                                  |
| TEXT                      | String长度+2字节                   | 字符串，最大长度为65 535个字符                               |
| MEDIUMINT                 | String长度+3字节                   | 字符串，最大长度为16 777 215个字符                           |
| LONGTEXT                  | String长度+4字节                   | 字符串，最大长度为4 294 967 295个字符                        |
| TINYINT(Length)           | 1字节                              | 范围：-128~127，或者0~255（无符号）                          |
| SMALLINT(Length)          | 2字节                              | 范围：-32 768~32 767，或者0~65 535（无符号）                 |
| MEDIUMINT(Length)         | 3字节                              | 范围：-8 388 608~8 388 607，或者0~16 777 215（无符号）       |
| INT(Length)               | 4字节                              | 范围：-2 147 483 648~2 147 483 647，或者0~4 294 967 295（无符号） |
| BIGINT(Length)            | 8字节                              | 范围：-9 223 372 036 854 775 808~9 223 372 036 854 775 807，或者0~18 446 744 073 709 551 615（无符号） |
| FLOAT(Length, Decimals)   | 4字节                              | 具有浮动小数点的较小的数                                     |
| DOUBLE(Length, Decimals)  | 8字节                              | 具有浮动小数点的较大的数                                     |
| DECIMAL(Length, Decimals) | Length+1字节或Length+2字节         | 存储为字符串的DOUBLE，允许固定的小数点                       |
| DATE                      | 3字节                              | 采用YYYY-MM-DD格式                                           |
| DATETIME                  | 8字节                              | 采用YYYY-MM-DD HH:MM:SS格式                                  |
| TIMESTAMP                 | 4字节                              | 采用YYYYMMDDHHMMSS格式；可接受的范围终止于2037年             |
| TIME                      | 3字节                              | 采用HH:MM:SS格式                                             |
| ENUM                      | 1或2字节                           | Enumeration(枚举)的简写，这意味着每一列都可以具有多个可能的值之一 |
| SET                       | 1、2、3、4或8字节                  | 与ENUM一样，只不过每一列都可以具有多个可能的值               |

### MySQL数据库操作sql语句  

- MySQL函数

  ```mysql
  -- 显示当前时间
  select now()
  -- 显示当前所在数据库
  select databases()
  
  ```

- MySQL数据库常用操作

  ```mysql
  -- 数据库创建
  create database 数据库名 charset=utf8;
  -- 删除数据库
  drop database 数据库名；  
  -- 使用数据库
  use 数据库名;
  
  ```

### MySQL数据表操作sql语句 

- 创建表

  ```mysql
  -- 创建
  create table 表名(字段1 类型 约束，字段2 类型 约束)；
  -- 例子
  create table students(
                      id int unsigned primary key auto_increment,
                      name varchar(20) not null,
                      age int default 10,
                      height decimal(3,2),
                      sex enum('男', '女')
                          );
  
  ```

- 修改表

  ```mysql
  -- 添加字段  alter table 表名 add 列名 类型;
  alter table students add birthday datetime not null;
  
  -- 修改类型  alter table 表名 modify 列名 类型及约束;
  alter table students modify birthday date;
  
  -- 修改字段  alter table 表名 change 原名 新名 类型及约束;
  alter table students change birthday birth date not null;
  
  -- 删除字段  alter table 表名 drop 列名;
  alter table students drop birth;
  
  -- 删除表    drop table 表名; 		
  drop table test;  -- 跟删库类似
  
  ```

- 查看表

  ```mysql
  -- 查看表的创建语句
  show create table 表名字;
  -- 查看数据表的名字;
  desc students
  
  ```

### 表中数据操作

- **增**

  ```mysql
  -- 全列插入  insert into 表名 values(...)；
  insert into students values(0, '张三', null, 1.80, '男'); -- 数据一个不能少
  -- 部分插入
  insert into students(name, height) values('张三',1.80)；
  -- 多行插入
  insert into 表名 values(...),(...);
  insert into students(name, height) values('张三',1.80)("李四"，1.80)；
  
  ```

- **删**

  ```mysql
  -- 物理删除
  delete from students where id = 5;
  -- 逻辑删除
  	-- 给students表添加一个 is_delete 字段 bit 类型  默认为0
  	alter table students add is_delete bit default 0 not null;
  	-- bit 类型，智能保存 0 或者 1
  	-- is_delete = 1  逻辑删除  
  	update students set is_delete = 1 where id = 3;
  
  ```

  

- **改**

  ```mysql
  -- 全部修改
  update students set age = 15;
  -- 按条件部分修改
  update students set age = 16 where id = 3;
  
  ```

### 建立索引

> **查看表中已有索引**
>
> ```sql
> show index from 表名;
> -- 主键列会自动创建索引
> ```
>
> **索引的创建:**
>
> ```sql
> -- 创建索引的语法格式
> -- alter table 表名 add index 索引名[可选](列名, ..)
> -- 给name字段添加索引
> alter table classes add index my_name (name);
> ```
>
> **索引的删除:**
>
> ```sql
> -- 删除索引的语法格式
> -- alter table 表名 drop index 索引名
> -- 如果不知道索引名，可以查看创表sql语句
> show create table classes;
> alter table classes drop index my_name;
> ```
>
> ### 联合索引
>
> ```mysql
> # 联合索引又叫复合索引，即一个索引覆盖表中两个或者多个字段，一般用在多个字段一起查询的时候。
> -- 创建teacher表
> create table teacher
> (
>  id int not null primary key auto_increment,
>  name varchar(10),
>  age int
> );
> 
> -- 创建联合索引
> alter table teacher add index (name,age);
> ```
>
> **联合索引的好处:**
>
> - 减少磁盘空间开销，因为每创建一个索引，其实就是创建了一个索引文件，那么会增加磁盘空间的开销。
> - 联合索引又叫复合索引，即一个索引覆盖表中两个或者多个字段，一般用在多个字段一起查询的时候。
>
> **联合索引的最左原则**
>
> 在使用联合索引的时候，我们要遵守一个最左原则,即index(name,age)支持 name 、name 和 age 组合查询,而不支持单独 age 查询，因为没有用到创建的联合索引。
>
> ```
> -- 下面的查询使用到了联合索引
> select * from stu where name='张三' -- 这里使用了联合索引的name部分
> select * from stu where name='李四' and age=10 -- 这里完整的使用联合索引，包括 name 和 age 部分 
> -- 下面的查询没有使用到联合索引
> select * from stu where age=10 -- 因为联合索引里面没有这个组合，只有 name | name age 这两种组合
> ```

### 数据库三大范式

+ 第一范式:

  > 每一列元素都是不可再分割的

+ 第二范式:

  > 所有元素必须完全依赖主键

+ 第三范式:

  > 数据不能存在传递依赖

### 语法总结

- 关键字之间一般用空格分开

- 数据之间一般用逗号分开

- 了删除数据用delete外,其他的都是用drop

- 对字段进行增删改时需要用到alter

- 数据的增删改主要是三个关键字  delete insert update

- 数据导入

  - ```bash
    mysql -h127.0.0.1 -uroot -pmysql meiduo_mall < areas.sql
    
    ```

  - source  数据.sql

# Mysql原理

### Mysql引擎

> 不同的存储引擎都有各自的特点
>
> ![mysql存储引擎](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/mysql%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E.png)
>
> ```
> 四种引擎中只有InnoDB支持事务和外键,其他都不支持
> MyISAM查询插入速度快,但是不支持事务
> 
> 一个数据库中多个表可以使用不同引擎以满足各种性能和实际需求，
> 
> ```

**char与varchar**

- char 不可变，查询效率高，可能造成存储浪费
- varchar 可变，查询效率不如char，节省空间

**外键**

- 保持数据完整性

- 提供便捷查询

  ```
  CASCADE		删除主表时自动删除从表,更新主表时自动更新从表
  SET NULL	删除或更新主表时自动更新从表值为NULL
  NO ACTION	从表记录不存在时，主表才可以删除,从表记录不存在时，主表才可以更新
  
  ```

**原理**

> ```
> Innodb用的是b+树，
> 为什么它没有myisam快
> 
> ```



### 数据库优化

**索引**

> 索引是采用平衡搜索二叉树实现的
>
> ```
> 特点：
> 	树的右子节点比上级节点小，树的左节点比树的上级节点大。
> 
> ```
>
> 搜索的时候只要数据比父节点大就可以排除所有的右节点了，一下砍掉一般。所以搜索速度快。
>
> 但是对新增修改数据的效率有影响，以为每次新增或修改，可能都需要重构树结构

![平衡搜索二叉树](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/%E5%B9%B3%E8%A1%A1%E6%90%9C%E7%B4%A2%E4%BA%8C%E5%8F%89%E6%A0%91.png)

**SQL查询优化**

> 避免全表扫描`若要提高效率，可以考虑全文检索；`
>
> ```
> # 1.模糊查询  如下。都会导致全表查询
> SELECT id FROM t WHERE name LIKE ‘%abc%’
> SELECT id FROM t WHERE name LIKE ‘_abc’
> 
> ```
>
> 避免使用`select *`的操作
>
> ```
> 查询时使用select明确指明所要查询的字段，避免使用`select *`的操作，
> 
> ```
>
> SQL语句尽量大写
>
> ```
> 对于小写的sql语句，通常数据库在解析sql语句时，通常会先转换成大写再执行。
> 
> ```
>
> 尽量避免在 where 子句中使用!=或<>操作符
>
> ```
> MySQL只有对以下操作符才使用索引：<，<=，=，>，>=，BETWEEN，IN，以及某些时候的LIKE；
> 
> ```
>
> 遵循最左原则
>
> ```
> # 遵循最左原则，在where子句中写查询条件时把索引字段放在前面，如
> mobile为索引字段，name为非索引字段
> 推荐
> SELECT ... FROM t WHERE mobile='13911111111' AND name='python'
> 不推荐
> SELECT ... FROM t WHERE name='python' AND mobile='13911111111' 
> 
> 建立了复合索引 key(a, b, c)
> 推荐
> SELECT ... FROM t WHERE a=... AND b=... AND c= ...
> SELECT ... FROM t WHERE a=... AND b=...
> SELECT ... FROM t WHERE a=...
> 不推荐 (字段出现顺序不符合索引建立的顺序)
> SELECT ... FROM t WHERE b=... AND c=...
> SELECT ... FROM t WHERE b=... AND a=... AND c=...
> 
> ```
>
> 尽量不要使用子查询
>
> ```
> # 能使用关联查询解决的,尽量不要使用子查询
> 子查询
> SELECT article_id, title FROM t_article WHERE user_id IN (SELECT user_id FROM t_user  WHERE user_name IN ('itcast', 'itheima', 'python'))
> 
> 关联查询(推荐)
> SELECT b.article_id, b.title From t_user AS a INNER JOIN t_article AS b ON a.user_id=b.user_id WHERE a.user_name IN ('itcast', 'itheima', 'python');
> 
> ```
>
> 不需要获取全表数据的时候，不要查询全表数据，使用LIMIT来限制数据。

**其他**

> ```
> 在进行表设计时，可适度增加冗余字段(反范式设计)，减少JOIN操作；
> 多字段表可以进行垂直分表优化，多数据表可以进行水平分表优化；
> 选择恰当的数据类型，如整型的选择；
> 对于强调快速读取的操作，可以考虑使用MyISAM数据库引擎；
> 对较频繁的作为查询条件的字段创建索引；唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件；更新非常频繁的字段不适合创建索引；
> 编写SQL时使用上面的方式对SQL语句进行优化；
> 使用慢查询工具找出效率低下的SQL语句进行优化；
> 构建缓存，减少数据库磁盘操作；
> 可以考虑结合使用内在型数据库，如Redis，进行混合存储
> 
> ```

### Mysql主从

![mysql-master-slave](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/mysql-master-slave.jpg)

> **复制分成三步：**
>
> 1. master将改变记录到二进制日志(binary log)中（这些记录叫做二进制日志事件，binary log events）；
> 2. slave将master的binary log events拷贝到它的中继日志(relay log)；
> 3. slave重做中继日志中的事件，将改变反映它自己的数据。
>
> ```
> 该过程的第一部分就是master记录二进制日志。在每个事务更新数据完成之前，master在二日志记录这些改变。MySQL将事务串行的写入二进制日志，即使事务中的语句都是交叉执行的。在事件写入二进制日志完成后，master通知存储引擎提交事务。
> 
> 下一步就是slave将master的binary log拷贝到它自己的中继日志。首先，slave开始一个工作线程——I/O线程。I/O线程在master上打开一个普通的连接，然后开始binlog dump process。Binlog dump process从master的二进制日志中读取事件，如果已经跟上master，它会睡眠并等待master产生新的事件。I/O线程将这些事件写入中继日志。
> 
> SQL slave thread处理该过程的最后一步。SQL线程从中继日志读取事件，更新slave的数据，使其与master中的数据一致。只要该线程与I/O线程保持一致，中继日志通常会位于OS的缓存中，所以中继日志的开销很小。
> 
> 此外，在master中也有一个工作线程：和其它MySQL的连接一样，slave在master中打开一个连接也会使得master开始一个线程。
> 
> 利用主从在达到高可用的同时，也可以通过读写分离提供吞吐量。
> 
> ```
>
> **主从具体搭建**
>
> ```
> file:///E:/%E9%BB%91%E9%A9%AC%E8%B5%84%E6%96%992/%E8%AF%BE%E4%BB%B6/%E7%BE%8E%E5%A4%9A%E5%95%86%E5%9F%8E%E9%A1%B9%E7%9B%AE-%E8%AE%B2%E4%B9%89/wpo/mysql-read-write/master-slave.html
> 
> ```
>
> 

### [事务的隔离级别](https://www.cnblogs.com/shihaiming/p/11044740.html)

> ```
> MySQL数据库默认使用可重复读（ Repeatable read）。
> ```

**隔离级别越高，越能保证数据的完整性和一致性，但是对并发性能的影响也越大。**

| 事务隔离级别                 | 脏读 | 不可重复读 | 幻读 |
| ---------------------------- | ---- | ---------- | ---- |
| 读未提交（read-uncommitted） | 是   | 是         | 是   |
| 不可重复读（read-committed） | 否   | 是         | 是   |
| 可重复读（repeatable-read）  | 否   | 否         | 是   |
| 串行化（serializable）       | 否   | 否         | 否   |

> **脏读**
>
> ```
> 事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据
> 
> ```
>
> **不可重复读**
>
> ```
> 事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果 不一致。
> 
> ```
>
> **幻读**
>
> ```
> 系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读。
> 
> ```
>
> **PS**:
>
> ```
> 不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读侧重于新增或删除。解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表
> 
> ```
>
> **修改事务隔离级别**
>
> ```
> # 修改成	不可重复读（read-committed）
> transaction-isolation=READ-COMMITTED
> 
> ```
>
> ![03修改事务隔离级别1](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/03%E4%BF%AE%E6%94%B9%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB1.png)
>
> ![03修改事务隔离级别2](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/03%E4%BF%AE%E6%94%B9%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB2.png)





# Mysql数据操作

### as 和 distinct 关键字

+ as：起别名

  ~~~mysql
  -- 可以通过as给表以及字段起别名
  select name as 姓名, age as 年龄 from students as s;
  -- 简写的时候可以省略 as
  select name  姓名, age  年龄 from students s;
  
  -- PS 一旦表起了别名, 就不能使用原表名来访问字段了. 
  ~~~

+ distinct：去重

  ~~~mysql
  -- distinct 主要用于去重
  select distinct sex from students;  -- 去重之后 sex 就不会有重复的了
  
  -- 注意: 多列去重, 数据放在一起不重复, 但是每一列可能会重复. 
  select distinct age,sex from students;
  
  ~~~

  

### where 和 比较运算符

+ where和运算符

+ 大于(>)、小于(<)、等于(=)、大于等于(>=)、小于等于(<=)、不等于(!=或者<>)

  ~~~mysql
  -- 运算符：> 、 < 、= 、 >= 、 <= 、 <> 、 !=
  -- 逻辑运算符使用上面跟Python运算符一样
  -- 例子
  select * from students where age > 18; 		-- 年龄大于18的学生
  select * from students where age <= 18; 	-- 年龄小于等于18的学生
  select * from students where age = 18;		-- 年龄等于18的学生
  select * from students where age != 18;		-- 年龄不等于18的学生
  ~~~

###   where 和 比较运算符

+ where 和 比较运算符

  ~~~mysql
  -- and、or、not
  -- 例子
  select * from students where age > 18 and age < 28; 
  select * from students where age>18 and gender = '女';
  select * from students where age > 58 or height > 180;
  select * from students where not (age>18 and gender='女');
  ~~~

### where之模糊查询

+ like 表示模糊查询

  + %：表示任意多个字符
  + _  ：表示任意一个字符

  ~~~mysql
  select * from students where name like '小%';   -- 查询姓名中 以 "小" 开始的名字
  select * from students where name like '%小%';  -- 查询姓名中 有 "小" 所有的名字
  select * from students where name like '__';    -- 查询有2个字的名字
  select * from students where name like '__%';   -- 查询至少有2个字的名字
  ~~~

### where之范围查询

+ in 用于非连续的范围的查询

  + 判断是否某个非连续范围（类似or）

+ between 起始 and 终值

  + 判断是否属于起始所在的连续范围

  ~~~mysql
  -- in
  select name from students where age in (18, 34);    -- 查询年龄为18、34的姓名
  select name from students where age not in (18, 34);-- 年龄不是 18、34岁的信息
  -- between
  select * from students where age between 18 and 34;  -- 查询年龄18到34之间的的信息 包含18 34
  select * from students where age not between 18 and 34;
  -- PS
  -- not或者可以在where 后面加
   select name from students where not age in (18, 34);
   select * from students where not age between 18 and 34;
  ~~~


### where之空值判断

+ 表示空判断

  > is null

+ 表示非空判断

  > is not null 

  ~~~mysql
  -- 判空is null
   select * from students where height is null
   -- 判非空is not null
   select * from students where height is not null;
  ~~~

### order排序

+ 排序：order by 列1, 列2, ....

+ 排序规则：

  + 升序

    > asc（默认） ps: 因为默认是升序，可以不写

  + 降序

    > desc

  ~~~mysql
  -- 查询年龄在18到34岁之间的男性，按照年龄从小到大到排序(默认是asc升序)
  select * from students where (age between 18 and 34) and gender='男' order by age；
  -- 查询年龄18到34岁之间的女性，身高从高到矮排序, 如果身高相同的情况下按照年龄从大到小排序
  select * from students where (age between 18 and 34) and gender = '女' order by height desc, age desc;
  -- 查询年龄在18到34岁之间的女性，身高从高到矮排序, 如果身高相同的情况下按照年龄从大到小排序,如果年龄也相同那么按照id从大到小排序
  select * from students where (age between 18 and 34) and gender = '女' order by height desc, age desc, id desc;
  ~~~

### limit限制记录

+ limit用途

  + 数据很多时，用来显示指定数量的数据

+ limit 使用格式

  > limit start ,count 

  + 表中的数据，位置默认从 0 开始

+ limit 一定要写到 SQL 语句的最后面

  ~~~mysql
  -- n: 第n页
  	-- m: 每页显示m个数据
  	-- limit (n-1)*m, m
  -- 每页显示2个，第1个页面 
  select * from students limit 0,2; -- ps： 从0开始 显示两条数据
  -- 每页显示2个，第4个页面
  select * from students limit 6,2;
  -- 每页显示2个，显示第6页的信息, 按照年龄从小到大排序
  select * from students order by age limit 10, 2;
  ~~~

### 聚合函数

+ 聚合函数：组函数，对一组数据进行运算

+ 常见的聚合函数：默认聚合函数会忽略null，如需替换，需要使用ifnull判断

  + count(*)   计数

  + max(age)  取最大值

  + min(age)   取最小值

  + sum(age)  求和

  + avg(age)    求平均值

    + round(12.3434,2)

      > 将 12.3434进行四舍五入，只保留两位小数

    + ifnull(age,0)

      > 进行非空判断，如果有null，则用0替换

  ~~~mysql
  -- 聚合函数:组函数
  
  -- 计算班级学生的总数   count(*)
  select count(height) from students;
  -- 查询最大的年龄		 max(*)
  select max(age) from students;
  -- 查询女性的最矮身高    min(*)
  select min(height) from students where gender = '女';
  -- 计算所有人的年龄总和  sum(*)
  select sum(age) from students;
  -- 计算平均年龄		  avg(*)
  select avg(age) from students;
  -- 计算所有人的平均年龄，保留2位小数  round(avg(age),2)
  select round(avg(age), 2) from students;
  -- 如果身高为null, 0计算, 计算平均身高. 
  select round(avg(ifnull(height, 0)),2) from students where gender='男';
  ~~~

### group分组(重点)

+ 用于分组(按照某个特定字段进行分类)

  > group by gender

+ 分组统计/计算

  > group by + count(*)/max(age)/min(age)/avg(age)/sum(age)

+ 分组 + 分组内容以`,`链接为一个字符串

  > group by + group_concat(name)

+ 分组+过滤

  > group by gender having gender in (1,2)

+ 分组 + 小计:

  > group by + with rollup

  ~~~mysql
  -- group by 列名
  -- 按照性别分组,查询所有的性别
  select gender from students group by gender;
  
  -- group by + 聚合函数
  -- 统计每种性别中的人数
  select gender,count(*) from students group by gender;
  
  -- group by + 分组内容以`,`链接为一个字符串
  -- 查询同种性别中的姓名
  select gender,group_concat(name) from students group by gender;
  
  -- group by+过滤
  -- 查询平均年龄超过30岁的性别，以及姓名 having avg(age) > 30(重点)
  select gender, avg(age), group_concat(name) from students group by gender having avg(age) > 30;
  
  -- group by + 小计:
  -- 查询同种性别中的姓名, 并汇总所有类别的姓名
   select gender, group_concat(name) from students group by gender with rollup;
  ~~~

+ 注意

  ~~~mysql
  --  分组查询的时候, 直接查询列只能group by后面的列, 其他列只能通过聚合函数统计. 
  --  分组前过滤使用 where, 分组后过滤使用 having. 
  --  where 后面不能有聚合函数, having后面可以有聚合函数
  ~~~

### 内连接

+ 内连接：查询的结果为两个表匹配到的数据，默认是笛卡尔积算法（所有组合）

+ 关键字

  > 表A inner join 表二 on 表A.cls_id = 表B.id

+ 格式

  > select * from students inner join classes on students.cls_id=classes.id

  ~~~mysql
  -- 连接查询
  -- select ... from 表A inner join 表B on 表A.cls_id = 表B.id;
  select * from students inner join classes on cls_id = classes.id;
  
  ~~~

### 左连接/右连接

+ 右连接 : 以右表为主查询左表内容； 

  > 表1 right join 表2 on ....

  + 右外连接一个表，在从表中没有找到匹配，左侧补 NULL

+ 左连接：

  > 表1 left join 表2  on .... 

  + 左连接一个表，在从表中没有找到的匹配，左侧补 NULL

  ~~~mysql
  -- 右连接
  select * from students s right join classes c on s.cls_id = c.id;
  -- 左连接
  select * from students s left join classes c on s.cls_id = c.id;
  ~~~

+ PS

  ~~~mysql
  -- 左连接性能会比有链接性能好
  ~~~

### 自连接

+ 自连接： 特殊的内连接; 左表和右表是同一个表；通过设置别名区分

  ~~~mysql
  -- 自连接查询
  -- 一般用于查询收获地址
  -- 1、查询所有的省和直辖市
  select * from areas where pid is null;
  -- 2、查询省的名称为“广东省”的所有城市
  select c.title  from areas c inner join areas p on c.pid = p.id where p.title='广东省';
  ~~~

### 子查询

+ 定义：
  
+ 在一个 select 语句中,嵌入了另外一个 select 语句, 那么被嵌入的 select 语句称之为子查询语句,外部那个select语句则称为主查询.
  
+ 主查询跟子查询的关系

  + 子查询嵌入主查询中
  + 充当主查询的条件或者数据源
  + 子查询也是一条完整的查询语句

  ~~~mysql
  -- 标量子查询:
  -- 子查询返回的结果是一个数据(一行一列); 可以把这个子查询当成  一个数据来用就可以
  -- 1 查出平均身高
  select avg(height) from students;
  
  -- 列子查询: 
  -- 返回的结果是一列(一列多行)  可以把这个子查询当成一个  列表来用
  -- 查询学生的学号和班级编号对应的学生信息
  select * from students where cls_id in (select id from classes);
  
  -- 行子查询: 
  -- 返回的结果是一行(一行多列)	可以把这个子查询当成一个 元组使用
  -- 查找年龄最大,身高最高的学生:
  select * from students where (age, height) = (select max(age), max(height) from students);
  
  -- 表子查询: 
  -- 返回的结果是表 可以把这个子查询当一张表来用. 
  ~~~

### 数据库DDL规范

+ 三大范式：
  + 1NF ：列的原子性, 列不能再分. 
  + 2NF ：满足1NF,  表中必须有主键, 并且非主键列完全依赖主键, 不存在部分依赖.
  + 3NF :   满足2NF,  表非主键字段必须直接依赖于主键, 不存在传递依赖. +

+ E-R模型
  + E：实体
  + R：关系
+ 表间关系
  + 一对一：外键存储在任意一个表中都可以
  + 一对多：外键存储在多的一方
  + 多对多：需要建立第三张表，存储外键

### 外键约束使用(重点)

+ 外键的概念：

  > 外键字段数据是另一张表中的主键

+ 作用：在插入和更新外键字段的时候，会去关联表中验证。

+ 创建外键：

  + 已经存在的表建立外键：

    ~~~mysql
    alter table students add foreign key(cls_id) references classes(id);
    ~~~

  + 创建表的时候建立外键

    ~~~mysql
    -- 表一
    create table school(
    	id int unsigned primary key auto_increment,
    	name varchar(30)
    );
    -- 表二
    create table teacher(
    	id int unsigned primary key auto_increment,
    	name varchar(20),
    	age int default 20,
    	s_id int unsigned,
    	foreign key(s_id) references school(id)
    );
    ~~~

+ 删除外键

  + 查看外键

    ~~~mysql
    show create table teacher
    ~~~

  + 删除外键:

    ~~~mysql
    alter table teacher drop foreign key teacher_ibfk_1;
    ~~~

# 实战应用

+ 查询结果插入到其他表中

  + 创建新表

    ~~~mysql
    create table goods_cates(
    	id int unsigned primary key auto_increment,
    name varchar(40) not null
    );
    ~~~

  + 吧查询出来的数据插入新表中

        ~~~mysql
    -- 查询去重类别信息
    select cate_name from goods group by cate_name;
    -- 把查询出来的类别信息插入类别表中
    insert into goods_cates(name) select cate_name from goods group by cate_name;
        ~~~

+ 使用连接更新表中某个字段数据

  + 更新 goods 表 cate_name 为 goods_cates.id

    ~~~mysql
    -- 商品表和类别表关联查询
    select * from goods g inner join goods_cates c on g.cate_name = c.name;  
    
    -- 我们可以把 from 后面当成一张表
    update goods g inner join goods_cates c on g.cate_name = c.name set g.cate_name = c.id;
    ~~~

+ 修改goods表结构

  + 修改 cate_name 为 cate_id

    ~~~mysql
    -- 修改字段名称和类型, 使用change; 只修改类型使用modify
    alter table goods change cate_name cate_id int unsigned not null, change brand_name brand_id int unsigned not null;
    
    -- 三张表关联查询(内连接)
    select * from goods g, goods_cates c, goods_brands b where g.cate_id = c.id and g.brand_id = b.id;
    ~~~


>  sql语句中“ ||” 符号表示，连接符。 
>
> ~~~sql
> -- last_name和first_namel以空格连接
> select last_name || " " || first_name AS NAME from employees
> ~~~
>
>  对于表actor批量插入如下数据,如果数据已经存在，请忽略，不使用替换操作 
>
> ~~~sql
> insert or ignore into actor values(3,'ED','CHASE','2006-02-15 12:34:33')
> -- mysql 去掉or就可以
> ~~~
>
> 





### 数据库练习

+ ### 资料准备

  + sql准备

    + [学生](E:\黑马资料2\sql)

    + [收货地址](E:\黑马资料2\sql)

      ~~~mysql
      -- 创建 "京东" 数据库
      create database jing_dong charset=utf8;
      
      -- 使用 "京东" 数据库
      use jing_dong;
      
      -- 创建一个商品goods数据表
      create table goods(
          id int unsigned primary key auto_increment not null,
          name varchar(150) not null,
          cate_name varchar(40) not null,
          brand_name varchar(40) not null,
          price decimal(10,3) not null default 0,
          is_show bit not null default 1,
          is_saleoff bit not null default 0
      );
      
      -- 向goods表中插入数据
      
      insert into goods values(0,'r510vc 15.6英寸笔记本','笔记本','华硕','3399',default,default); 
      insert into goods values(0,'y400n 14.0英寸笔记本电脑','笔记本','联想','4999',default,default);
      insert into goods values(0,'g150th 15.6英寸游戏本','游戏本','雷神','8499',default,default); 
      insert into goods values(0,'x550cc 15.6英寸笔记本','笔记本','华硕','2799',default,default); 
      insert into goods values(0,'x240 超极本','超级本','联想','4880',default,default); 
      insert into goods values(0,'u330p 13.3英寸超极本','超级本','联想','4299',default,default); 
      insert into goods values(0,'svp13226scb 触控超极本','超级本','索尼','7999',default,default); 
      insert into goods values(0,'ipad mini 7.9英寸平板电脑','平板电脑','苹果','1998',default,default);
      insert into goods values(0,'ipad air 9.7英寸平板电脑','平板电脑','苹果','3388',default,default); 
      insert into goods values(0,'ipad mini 配备 retina 显示屏','平板电脑','苹果','2788',default,default); 
      insert into goods values(0,'ideacentre c340 20英寸一体电脑 ','台式机','联想','3499',default,default); 
      insert into goods values(0,'vostro 3800-r1206 台式电脑','台式机','戴尔','2899',default,default); 
      insert into goods values(0,'imac me086ch/a 21.5英寸一体电脑','台式机','苹果','9188',default,default); 
      insert into goods values(0,'at7-7414lp 台式电脑 linux ）','台式机','宏碁','3699',default,default); 
      insert into goods values(0,'z220sff f4f06pa工作站','服务器/工作站','惠普','4288',default,default); 
      insert into goods values(0,'poweredge ii服务器','服务器/工作站','戴尔','5388',default,default); 
      insert into goods values(0,'mac pro专业级台式电脑','服务器/工作站','苹果','28888',default,default); 
      insert into goods values(0,'hmz-t3w 头戴显示设备','笔记本配件','索尼','6999',default,default); 
      insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default); 
      insert into goods values(0,'x3250 m4机架式服务器','服务器/工作站','ibm','6888',default,default); 
      insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default);
      ~~~

      

> 
>
> ****

+ ### sql练题

  ~~~mysql
  -- 1.查询类型cate_name为 '超级本' 的商品名称、价格
  -- 2.显示商品的分类
  -- 3.求所有电脑产品的平均价格,并且保留两位小数
  -- 4.显示每种商品的平均价格
  -- 5.查询每种类型的商品中 最贵、最便宜、平均价、数量
  -- 6.查询所有价格大于平均价格的商品，并且按价格降序排序
  ~~~

# Python跟Mysql交互

### Python连接MySQL

- 作用：使用python操作mysql数据库

- 安装：

  ```shell
  sudo pip3 install pymysql
  ```

- 使用

  ```python
  # 1.导入pymysql
  import pymysql
  
  # 2.跟pymysql建立连接
  conn = pymysql.connect( host='localhost', port=3306,user='root', password='mysql',
          				database='python_29',charset='utf8')
  #3. 获取游标
  cursor = conn.curser
  #4. 使用游标执行sql语句
  cursor.execute("select * from students")
  #5. 获取数据
  datas = cursor.fetchall() # 数据为一个二维元祖
  #6. 关闭游标
  cursor.close()
  #7. 关闭连接
  conn.close()
  ```

- 补充：配置mysql支持外网链接

  1.修改配置文件: sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf 

  ```shell
  # 不注释的话，表示只支持本地IP连接
  # bind-address      = 127.0.0.1 
  ```

  2.到mysql客户端中，修改user表中的host, 把host 改为 % 

  ```mysql
  -- 如果有有两个root; 一个host, `%` 一个host, localhost
  delete from user where user='root' and host='localhost';
  -- 如果只有一个root; 并且host是localhost
  Update user set host='%' where user='root';
  -- PS 让服务器能够被外网客户端连，让客户端的user能够连接外网的服务器
  ```

  3.远程客户端连接数据库

  ```shell
  mysql -h192.168.42.135 -uroot -p
  ```

### Python操作数据库CURD

- 操作步骤

  - 导入模块

  - 创建连接对象

  - 创建游标对象

  - 使用游标对象执行SQL

    > ```python
    > row_num = cursor.execute(sql)  #返回执行后影响的行数，如果不需要也可不接收返回,sql里面可以写sql语句
    > ```

  - 提交(执行成功)

    > ```mysql
    > conn.commit() #如果不提交，所有的数据只是暂时缓存在内存中。
    > ```

  - 回滚(执行失败)

    > ```mysql
    > conn.rollback()
    > ```

  - 获取执行的结果（影响的行数）并打印执行的结果

  - 关闭游标

  - 关闭连接

###  SQL防注入

- 防注入的思路：

  - sql中需要变化的地方，可以占位符 %s %d...

    > sql中需要变化的地方，可以占位符 %s %d...
    > 注意：'%s' 可以表示任意类型的数据的占位符

  - 把参数封装到 列表中 

    > ```python
    > params = ['黄蓉']
    > ```

  - 把列表传递给 execute(sql, 列表)

    > ```python
    > cursor.execute(sql, params)
    > ```

- 例子

  > ```python
  > import  pymysql
  > # 创建对象
  > conn=pymysql.connect(host="localhost",user="haitang",password="password",database="test",charset="utf8")
  > # 获取游标
  > cursors = conn.cursor()
  > #sql防注入
  > name = input("请输入要查询的名字：")
  > age = input("请输入要查询人的年龄：")
  > param_list = [name,age]
  > sql_data = 'select * from students where name = %s and age = %s'
  > cursors.execute(sql_data,param_list)
  > # 数据打印
  > data = cursors.fetchall()
  > for i in data:
  >  print(i)
  >  # 关闭游标
  > cursors.close()
  > # 关闭链接
  > conn.close()
  > ```

###  事务

- 事务概念：一批SQL操作封装在一起, 要么全部执行, 要么都不执行。

  > mysql默认开启事务，并且在敲下回车时，自动提交

- 作用：保证数据一致性和完整性。

- 事务的特征 ACID：

  - 原子性: 一批SQL操作封装在一起, 要么全部执行, 要么都不执行
  - 一致性: 事务执行前后状态是一致的. 
  - 隔离性: 一个事务在没有提交之前, 对其他用户是不可见的
  - 持久性: 一旦提交了, 修改的数据就被持久化到数据库中. 

**事务的使用**

- 步骤

  - 开启事务

    > begin;
    > 或者
    > start transaction;

  - 操作数据库

    > insert/update/delete 

  - 确认修改(二选一)

    > ```mysql
    > -- 把操作结果写入数据库中
    > commit;
    > 
    > ```

  - 回滚(二选一)

    > ```mysql
    > -- 取消事务中所有操作, 回到没有操作的之前的状态
    > rollback;
    > 
    > ```

### 索引

- 索引作用：提高查询效率

- 索引的使用：

  - 查看索引：

    > ```mysql
    > show index from students(表名)
    > 
    > ```

  - 创建索引:

    > ```mysql
    > alter table students add index name_index(name)
    > 
    > ```

  - 删除索引:

    > ```mysql
    > alter table students drop index name_index;
    > 
    > ```

- 索引优点和缺点:

  - 优点:

    - 提高查询速度

  - 缺点:

    - 会占用更多的内存空间 是一种以空间换时间的一种方式

    > 占用更多存储空间; 由于在进行添加, 删除, 修改的时候 需要维护索引, 会降低添加, 删除, 修改的效率.

# 读写分离

### Mysql主从配置

**1.作用和好处**

- 通过增加从服务器来**提高数据库的性能**，在主服务器上执行写入和更新，在从服务器上向外提供读功能，可以动态地调整从服务器的数量，从而调整整个数据库的性能。

- **提高数据安全**，因为数据已复制到从服务器，从服务器可以终止复制进程，所以，可以在从服务器上备份而不破坏主服务器相应数据

- 在主服务器上生成实时数据，而在从服务器上分析这些数据，从而**提高主服务器的性能**

**2.主从同步机制**

- 1.冷备份

  - **主机需要停机，再将数据备份给从机**

- 2.热备份

  - **主机不停机，实时将数据备份给从机**

- 3.同步机制

  - 先执行冷备份

    - 把主机中原有的数据全部导出来

      - 再导入到从机中，实现冷备份

      - 保证从机和主机在同一起跑线

        - 在同一起跑线才能做热备份

  - 再执行热备份

    - 在主机中创建从机账号，并授权可以拷贝主机所有数据库的所有表

    - 配置主机告诉从机在哪个文件的哪个地方开始实时同步主机的数据

    - 实时同步的数据其实是主机中产生的二进制日志

      - 同步二进制速度最快

### 主从同步实现

- 0.说明

  - 我们电脑中的ubuntu里面的MySQL作为主机

  - 使用Dokcer安装的MySQL作为从机

- 1.安装配置Docker中MySQL从机

  - 1.获取Docker注册中心的MySQL镜像

    - 此版本的镜像，尽量和ubuntu中MySQL主机版本一致

    - docker官方镜像

      - docker image pull mysql:5.7.22  
        或  
        docker load -i mysql_docker_5722.tar

  - 2.指定从机配置文件
    - 运行mysql docker镜像，需要在宿主机中建立文件目录用于mysql容器保存数据和读取配置文件。

    - 创建从机存储目录

      - cd ~  
        mkdir mysql_slave  
        cd mysql_slave  
        mkdir data  
        cp -r /etc/mysql/mysql.conf.d ./

      - 最后一行命令是将主机的配置文件拷贝到从机  
        以免从机要从新生成一个

    - 效果

      - 

        - 

  - 3.配置从机
    - sudo vim ~/mysql_slave/mysql.conf.d/mysqld.cnf

    - port  =  8306  
      general_log  = 0  
      server-id  = 2

  - 4.创建Docker容器运行MySQL从机

    - 1.目的

      - 运行MySQL从机

    - 2.命令

      - sudo docker run --name mysql-slave -e MYSQL_ROOT_PASSWORD=mysql -d --network=host -v /home/python/mysql_slave/data:/var/lib/mysql -v /home/python/mysql_slave/mysql.conf.d:/etc/mysql/mysql.conf.d  mysql:5.7.22

    - 3.测试

      - mysql -uroot -pmysql -h 127.0.0.1 --port=8306

      - mysql -uroot -pmysql -h 192.168.103.210 --port=8306

- 2.配置ubuntu中MySQL主机

  - sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf

  - 

  - 重启主机

    - sudo service mysql restart

- 3.冷备份

  - 1.登录到ubuntu中MySQL主机，收集数据

    - mysqldump -uroot -pmysql --all-databases --lock-all-tables > ~/master_db.sql

  - 2.登录到Docker中MySQL从机，同步数据

    - mysql -uroot -pmysql -h127.0.0.1 --port=8306 < ~/master_db.sql

    - mysql -uroot -pmysql -h192.168.103.210 --port=8306 < ~/master_db.sql

- 4.热备份

  - 1.登入ubuntu中MySQL主机，创建用于从服务器同步数据使用的帐号

    - mysql –uroot –pmysql  

      GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%' identified by 'slave';  

      FLUSH PRIVILEGES;

  - 2.获取ubuntu中MySQL主机的二进制日志信息

    - SHOW MASTER STATUS;

    - 

    - 说明

      - File

        - 从机热备份时读取数据的文件

      - Position

        - 从机热备份时读取数据的位置、节点

  - 3.Docker中MySQL从机连接ubuntu中MySQL主机

    - 1.进入docker中的mysql

      - mysql -uroot -pmysql -h 127.0.0.1 --port=8306

      - mysql -uroot -pmysql -h 192.168.103.210 --port=8306

    - 2.从机连接到主机

      - change master to master_host='127.0.0.1', master_user='slave', master_password='slave',master_log_file='mysql-bin.000027', master_log_pos=730;

      - •	master_host：主服务器Ubuntu的ip地址  
        •	master_log_file: 前面查询到的主服务器日志文件名  
        •	master_log_pos: 前面查询到的主服务器日志文件位置

    - 3.启动slave服务器，并查看同步状态

      - start slave;  
        show slave status \G

    - 4.成功效果

      - 

  - 4.测试热备份

    - 在主机中新建一个数据库后，直接在从机查看是否存在

### 配置Django实现数据库读写分离

**1. 在配置文件中增加slave数据库的配置**

- DATABASES = {  
      '**default**': {  
          '**ENGINE': 'django.db.backends.mysql**',  
          '**HOST': '192.168.103.210**’,  # 数据库主机  
          '**PORT**': 3306,  # 数据库端口  
          '**USER': 'root**',  # 数据库用户名  
          '**PASSWORD': 'mysql**',  # 数据库用户密码  
          '**NAME': 'meiduo_mall**'  # 数据库名字  
  *    *},  
       '**slave**': {  
           '**ENGINE': 'django.db.backends.mysql**',  
           '**HOST': '192.168.103.210**’,  # 数据库主机  
           '**PORT**': 8306,  # 数据库端口  
           '**USER': 'root**',  # 数据库用户名  
           '**PASSWORD': 'mysql**',  # 数据库用户密码  
           '**NAME': 'meiduo_mall**'  # 数据库名字  
  *    *}  
       }

**2. 创建数据库操作的路由分发类**

- **meiduo_mall.utils.db_router.py**

- **class** MasterSlaveDBRouter(object):  
      """数据库主从读写分离路由"""  

      **def** db_for_read(self, model, **hints):  
          """读数据库"""  
          **return "slave**"  
        
      **def** db_for_write(self, model, **hints):  
          """写数据库"""  
          **return "default**"  
        
      **def** allow_relation(self, obj1, obj2, **hints):  
          """是否运行关联操作"""  
          **return True**

**3. 配置读写分离路由**

- #* *配置读写分离  
  DATABASE_ROUTERS = ['**meiduo_mall.utils.db_router.MasterSlaveDBRouter**']



# 数据库优化

~~~mermaid
graph LR
数据库优化-->软件;
数据库优化-->策略;
策略-->主从搭建;
策略-->cookie;
策略-->多级缓存;
软件-->数据库表优化;
软件-->sql优化;
数据库表优化-->合理的设计表;
数据库表优化-->常用字段建索引;
sql优化-->最左原则;
sql优化-->避免全表扫描;
sql优化-->按需查询;
按需查询-->不要用select*;
按需查询-->页数要用limit限制;
最左原则-->left-join;
最左原则-->索引最左靠;
避免全表扫描-->避免使用like;
避免全表扫描-->不用使用不等于;
避免全表扫描-->不要使用子查询;
cookie-->每条能存4kb,一个域名能存20条;
cookie-->jwt;
多级缓存-->count等会引起全表扫描的数据存内存上;
多级缓存-->热数据存redis中;
热数据存redis中-->定时过期;
热数据存redis中-->惰性过期;

linkStyle 0 stroke:#0ff,stroke-width:4px;
linkStyle 1 stroke:#0ff,stroke-width:4px;
linkStyle 2 stroke:#0ff,stroke-width:4px;
linkStyle 3 stroke:#0ff,stroke-width:4px;
linkStyle 4 stroke:#0ff,stroke-width:4px;
linkStyle 5 stroke:#0ff,stroke-width:4px;
linkStyle 6 stroke:#0ff,stroke-width:4px;
linkStyle 7 stroke:#0ff,stroke-width:4px;
linkStyle 8 stroke:#0ff,stroke-width:4px;
linkStyle 9 stroke:#0ff,stroke-width:4px;
linkStyle 10 stroke:#0ff,stroke-width:4px;
linkStyle 11 stroke:#0ff,stroke-width:4px;


~~~

并发量

​		一台mysql的并发量大概是400-500左右

​		一个Django框架并发量大概1000左右

实现高并发

​		数据库优化

​		动态页面静态化

​		redis 