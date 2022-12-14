# MySQL进阶

## 1. 外键约束和表关联关系

### 外键约束

foreign key 功能 : 建立表与表之间的某种约束的关系，由于这种关系的存在，能够让表与表之间的数据，更加的完整，关连性更强。

```mysql
# 创建部门表
CREATE TABLE dept (
    id int PRIMARY KEY AUTO_INCREMENT,
    dname VARCHAR(50) not null
);

# 创建人员表
CREATE TABLE person (
  id int PRIMARY KEY AUTO_INCREMENT,
  name varchar(32) NOT NULL,
  age tinyint DEFAULT 0,
  sex enum('m','w','o') DEFAULT 'o',
  salary decimal(8,2) DEFAULT 250.00,
  hire_date date NOT NULL,
  dept_id int
);
```

上面两个表中每个人员都应该有指定的部门，但是实际上在没有约束的情况下人员是可以没有部门的或者也可以添加一个不存在的部门，这显然是不合理的。

foreign key 外键的定义语法：

```mysql
[CONSTRAINT symbol] FOREIGN KEY（外键字段） 

REFERENCES tbl_name (主表主键)

[ON DELETE {RESTRICT | CASCADE | SET NULL | NO ACTION}]

[ON UPDATE {RESTRICT | CASCADE | SET NULL | NO ACTION}]
```

该语法可以在 CREATE TABLE时使用

```mysql
CREATE TABLE person (
  id int PRIMARY KEY AUTO_INCREMENT,
  name varchar(32) NOT NULL,
  age tinyint DEFAULT 0,
  sex enum('m','w','o') DEFAULT 'o',
  salary decimal(10,2) DEFAULT 250.00,
  hire_date date NOT NULL,
  dept_id int ,
  constraint dept_fk foreign key(dept_id) references dept(id)
);
```

级联动作

* restrict(默认)  :  on delete restrict  on update restrict
    * 当主表删除记录时，如果从表中有相关联记录则不允许主表删除
    * 当主表更改主键字段值时，如果从表有相关记录则不允许更改
* cascade ：数据级联更新  on delete cascade   on update cascade
    * 当主表删除记录或更改被参照字段的值时,从表会级联更新
* set null  :   on delete set null    on update set null
    * 当主表删除记录时，从表外键字段值变为null
    * 当主表更改主键字段值时，从表外键字段值变为null

### 表关联设计

当我们应对复杂的数据关系的时候，数据表的设计就显得尤为重要，认识数据之间的依赖关系是更加合理创建数据表关联性的前提。常见的数据关系如下：

- 一对一关系：一张表的一条记录一定只能与另外一张表的一条记录进行对应，反之亦然。
- 一对多关系：一张表中有一条记录可以对应另外一张表中的多条记录；但是反过来，另外一张表的一条记录。只能对应第一张表的一条记录，这种关系就是一对多或多对一
- 多对多关系：一对表中（A）的一条记录能够对应另外一张表（B）中的多条记录；同时B表中的一条记录也能对应A表中的多条记录

### 表连接

如果多个表存在一定关联关系，可以多表在一起进行查询操作，其实表的关联整理与外键约束之间并没有必然联系，但是基于外键约束设计的具有关联性的表往往会更多使用关联查询查找数据。

- 多个表数据可以联合查询，语法格式如下

    ```mysql
    select  字段1,字段2... from 表1,表2... [where 条件]
    ```

- 笛卡尔积就是将A表的每一条记录与B表的每一条记录强行拼在一起。所以，如果A表有n条记录，B表有m条记录，笛卡尔积产生的结果就会产生n*m条记录。

- 内连接：内连接查询只会查找到符合条件的记录，其实结果和表关联查询是一样的,官方更推荐使用内连接查询。

    ```mysql
    SELECT 字段列表
        FROM 表1  INNER JOIN  表2
    ON 表1.字段 = 表2.字段;
    ```

- 左连接  : 左表为主表，显示右表中与左表匹配的项

    ```mysql
    SELECT 字段列表
        FROM 表1  LEFT JOIN  表2
    ON 表1.字段 = 表2.字段;
    ```

- 右连接 ：右表为主表，显示左表中与右表匹配的项

    ```mysql
    SELECT 字段列表
        FROM 表1  RIGHT JOIN  表2
    ON 表1.字段 = 表2.字段;
    ```

    

## 2. 视图

MySQL 视图（View）是一种虚拟存在的表，同真实表一样，视图也由列和行构成，但视图并不实际存在于数据库中。行和列的数据来自于定义视图的查询中所使用的表，并且还是在使用视图时动态生成的。

数据库中只存放了视图的定义，并没有存放视图中的数据，这些数据都存放在定义视图查询所引用的真实表中。使用视图查询数据时，数据库会从真实表中取出对应的数据。因此，视图中的数据是依赖于真实表中的数据的。一旦真实表中的数据发生改变，显示在视图中的数据也会发生改变。

视图可以从原有的表上选取对用户有用的信息，那些对用户没用，或者用户没有权限了解的信息，都可以直接屏蔽掉，作用类似于筛选。这样做既使应用简单化，也保证了系统的安全。

- 创建视图

    ```mysql
    CREATE VIEW 视图名 AS SELECT语句
    ```

    - 视图名：指定视图的名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。

    - SELECT语句：指定创建视图的 SELECT 语句，可用于查询多个基础表或源视图。

- 视图表的增删改查操作

    - 视图的增删改查操作与一般表的操作相同，使用insert update delete select即可，但是原数据表的约束条件仍然对视图产生作用。

- 查看现有视图

    ```mysql
    show full tables in 数据库名 where table_type like 'VIEW';
    ```

- 删除视图

    ```mysql
    drop view [IF EXISTS] 视图名；
    ```

    - IF EXISTS 表示如果存在，这样即使没有指定视图也不会报错。

- 修改视图

    - 参考创建视图，将create关键字改为alter



## 3. 函数和存储过程

函数和存储过程一样，都是在数据库中定义一些 SQL 语句的集合。存储函数可以通过 return 语句返回函数值，主要用于计算并返回一个值。而存储过程没有直接返回值，主要用于执行操作。

```mysql
delimiter 自定义符号
　　create function 函数名(形参列表) returns 返回类型
　　begin
　　　　函数体
　　　　return val
　　end  自定义符号
delimiter ;
```

调用存储函数

```mysql
select 存储函数名字()
```

创建存储过程语法与创建函数基本相同，但是没有返回值。

```mysql
delimiter 自定义符号　
　　create procedure 存储过程名(形参列表)
　　begin
　　　　存储过程
　　end  自定义符号
delimiter ;
```

存储过程三个参数的区别

* IN 类型参数可以接收变量也可以接收常量，传入的参数在存储过程内部使用即可，但是在存储过程内部的修改无法传递到外部。
* OUT 类型参数只能接收一个变量，接收的变量不能够在存储过程内部使用（内部为NULL），但是可以在存储过程内对这个变量进行修改。因为定义的变量是全局的，所以外部可以获取这个修改后的值。
* INOUT类型参数同样只能接收一个变量，但是这个变量可以在存储过程内部使用。在存储过程内部的修改也会传递到外部。

调用存储过程

```mysql
call 存储过程名字()
```

```mysql
# 使用show status语句查看存储过程和函数的信息
show {procedure|function} status [like’存储过程或存储函数的名称’]

# 使用show create语句查看存储过程和函数的定义
show create  {procedure|function}  存储过程或存储函数的名称

# 查看所有函数或者存储过程
select name from mysql.proc where db='数据库名' and type='[procedure/function]';

# 删除存储过程或存储函数
DROP {PROCEDURE | FUNCTION} [IF EXISTS] sp_name
```

### 函数和存储过程区别

1. 函数有且只有一个返回值，而存储过程不能有返回值。
2. 函数只能有输入参数，而存储过程可以有in,out,inout多个类型参数。
3. 存储过程中的语句功能更丰富，实现更复杂的业务逻辑，可以理解为一个按照预定步骤调用的执行过程，而函数中不能展示查询结果集语句，只是完成查询的工作后返回一个结果，功能针对性比较强。
4. 存储过程一般是作为一个独立的部分来执行(call调用)。而函数可以作为查询语句的一个部分来调用。



## 4. 事物

数据库的事务（Transaction）是一种机制、一个操作序列，包含了一组数据库操作命令。事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么都执行，要么都不执行，因此事务是一个不可分割的工作逻辑单元。

在数据库系统上执行并发操作时，事务是作为最小的控制单元来使用的，特别适用于多用户同时操作的数据库系统。例如，航空公司的订票系统、银行、保险公司以及证券交易系统等。

事务具有 4 个特性

- 原子性：事务是一个完整的操作。事务的各元素是不可分的（原子的）。事务中的所有元素必须作为一个整体提交或回滚。如果事务中的任何元素失败，则整个事务将失败。
- 一致性：在事务开始之前，数据库中存储的数据处于一致状态。在正在进行的事务中. 数据可能处于不一致的状态，如数据可能有部分被修改。然而，当事务成功完成时，数据必须再次回到已知的一致状态。通过事务对数据所做的修改不能损坏数据，或者说事务不能使数据存储处于不稳定的状态。
- 隔离性：对数据进行修改的所有并发事务是彼此隔离的，这表明事务必须是独立的，它不应以任何方式依赖于或影响其他事务。修改数据的事务可以在另一个使用相同数据的事务开始之前访问这些数据，或者在另一个使用相同数据的事务结束之后访问这些数据。
- 持久性：一个事务成功完成之后，它对数据库所作的改变是永久性的，即使系统出现故障也是如此。也就是说，一旦事务被提交，事务对数据所做的任何变动都会被永久地保留在数据库中。

**事务操作**

- 开启事务

    ```mysql
    begin;
    ```

- 开始执行事务中的若干条SQL命令（增删改）

- 终止事务，若begin之后使用commit提交事务或者使用rollback进行事务回滚。

    ```mysql
    commit; # 事务中SQL命令都执行成功,提交到数据库,结束!
    rollback; # 有SQL命令执行失败,回滚到初始状态,结束!
    ```

事务四大特性中的隔离性是在使用事务时最为需要注意的特性，因为隔离级别不同带来的操作现象也有区别

隔离级别

- 读未提交：read uncommitted

    > 事物A和事物B，事物A未提交的数据，事物B可以读取到
    > 这里读取到的数据叫做“脏数据”
    > 这种隔离级别最低，这种级别一般是在理论上存在，数据库隔离级别一般都高于该级别

- 读已提交：read committed

    > 事物A和事物B，事物A提交的数据，事物B才能读取到
    > 这种隔离级别高于读未提交
    > 换句话说，对方事物提交之后的数据，我当前事物才能读取到
    > 这种级别可以避免“脏数据”
    > 这种隔离级别会导致“不可重复读取”

- 可重复读：repeatable read

    > 事务A和事务B，事务A提交之后的数据，事务B读取不到
    > 事务B是可重复读取数据
    > 这种隔离级别高于读已提交
    > MySQL默认级别
    > 虽然可以达到可重复读取，但是会导致“幻像读”

- 串行化：serializable

    > 事务A和事务B，事务A在操作数据库时，事务B只能排队等待
    > 这种隔离级别很少使用，吞吐量太低，用户体验差
    > 这种级别可以避免“幻像读”，每一次读取的都是数据库中真实存在数据，事务A与事务B串行，而不并发



## 5. 数据库优化

### 数据库设计范式

设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式。

> 目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。

各种范式呈递次规范，越高的范式数据库冗余越小。但是范式越高也意味着表的划分更细，一个数据库中需要的表也就越多，此时多个表联接在一起的花费是巨大的，尤其是当需要连接的两张或者多张表数据非常庞大的时候，表连接操作几乎是一个噩梦，这严重地降低了系统运行性能。所以通常数据库设计遵循第一第二第三范式，以避免数据操作异常,又不至于表关系过于复杂。


范式简介：

- 第一范式： 数据库表的每一列都是不可分割的原子数据项，而不能是集合，数组，记录等组合的数据项。简单来说要求数据库中的表示二维表，每个数据元素不可再分。

    例如： 在国内的话通常理解都是姓名是一个不可再拆分的单位，这时候就符合第一范式；但是在国外的话还要分为FIRST NAME和LAST NAME，这时候姓名这个字段就是还可以拆分为更小的单位的字段，就不符合第一范式了。

- 第二范式： 第二范式（2NF）要求数据库表中的每个实例或记录必须可以被唯一地区分，所有属性依赖于主属性。即选取一个能区分每个实体的属性或属性组，作为实体的唯一标识，每个属性都能被主属性筛选。其实简单理解要设置一个区分各个记录的主键就好了。

- 第三范式： 在第二范式的基础上属性不传递依赖，即每个属性不依赖其他非主属性。要求一个表中不包含已在其它表中包含的非主关键字信息。其实简单来说就是合理使用外键，使不同的表中不要有重复的字段就好了。

### MySQL存储引擎

数据库存储引擎是数据库底层软件组件，数据库管理系统使用数据引擎进行创建、查询、更新和删除数据操作。简而言之，存储引擎就是指表的类型。数据库的存储引擎决定了表在计算机中的存储方式。不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能，使用不同的存储引擎还可以获得特定的功能。现在许多数据库管理系统都支持多种不同的存储引擎。MySQL 的核心就是存储引擎。

| 存储引擎  |                             描述                             |
| :-------: | :----------------------------------------------------------: |
|  ARCHIVE  | 用于数据存档的引擎，数据被插入后就不能在修改了，且不支持索引。 |
|    CSV    |        在存储数据时，会以逗号作为数据项之间的分隔符。        |
| BLACKHOLE |              会丢弃写操作，该操作会返回空内容。              |
| FEDERATED |     将数据存储在远程数据库中，用来访问远程表的存储引擎。     |
|  InnoDB   |                具备外键支持功能的事务处理引擎                |
|  MEMORY   |                         置于内存的表                         |
|   MERGE   |             用来管理由多个 MyISAM 表构成的表集合             |
|  MyISAM   |                   主要的非事务处理存储引擎                   |
|    NDB    |                    MySQL 集群专用存储引擎                    |

```mysql
# 1、查看所有存储引擎
show engines;
# 2、查看已有表的存储引擎
show create table 表名;
# 3、创建表指定
create table 表名(...)engine=MyISAM;
# 4、已有表指定
alter table 表名 engine=InnoDB;
```

**InnoDB**		

```mysql
1. 支持行级锁,仅对指定的记录进行加锁，这样其它进程还是可以对同一个表中的其它记录进		行操作。
2. 支持外键、事务、事务回滚
3. 表字段和索引同存储在一个文件中
		 1. 表名.frm ：表结构
 		 2. 表名.ibd : 表记录及索引文件
```

**MyISAM**

```mysql
1. 支持表级锁,在锁定期间，其它进程无法对该表进行写操作。如果你是写锁，则其它进程则		读也不允许
2.  表字段和索引分开存储
      	 1. 表名.frm ：表结构
         2. 表名.MYI : 索引文件(my index)
         3. 表名.MYD : 表记录(my data)
```

- 执行查操作多的表用 MyISAM
- 执行写操作多的表用 InnoDB

### 字段数据类型选择

- 优先程度   数字 >  时间日期 > 字符串
- 同一级别   占用空间小的 > 占用空间多的
- 对数据存储精确不要求 float > decimel
- 如果很少被查询可以用 TIMESTAMP（时间戳实际是整形存储）

### 键的设置

- Innodb如果不设置主键也会自己设置隐含的主键，所以最好自己设置
- 尽量设置占用空间小的字段为主键
- 外键的设置用于保持数据完整性，但是会降低数据导入和操作效率，特别是高并发情况下，而且会增加维护成本
- 虽然高并发下不建议使用外键约束，但是在表关联时建议在关联键上建立索引，以提高查找速度

### SQL优化

- 尽量选择数据类型占空间少，在where ，group by，order by中出现的频率高的字段建立索引

- 尽量避免使用 select * ...;用具体字段代替 * ,不要返回用不到的任何字段 

- 少使用like %查询，否则会全表扫描

- 控制使用自定义函数

- 单条查询最后添加 LIMIT 1，停止全表扫描

- where子句中不使用 != ,否则放弃索引全表扫描

- 尽量避免 NULL 值判断,否则放弃索引全表扫描

    优化前：select number from t1 where number is null;

    优化后：select number from t1 where number=0;

    * 在number列上设置默认值0,确保number列无NULL值

- 尽量避免 or 连接条件,否则会放弃索引进行全表扫描，可以用union代替

    优化前：select id from t1 where id=10 or id=20;

    优化后： select id from t1 where id=10 union all  select id from t1 where id=20;

- 尽量避免使用 in 和 not in,否则会全表扫描

    优化前：select id from t1 where id in(1,2,3,4);

    优化后：select id from t1 where id between 1 and 4;

### 表的拆分

垂直拆分 ： 表中列太多，分为多个表，每个表是其中的几个列。将常查询的放到一起，blob或者text类型字段放到另一个表

水平拆分 ： 减少每个表的数据量，通过关键字进行划分然后拆成多个表



## 6. pymysql模块

pymysql是一个第三方库，如果自己的计算机上没有可以在终端使用命令进行安装。

```
pip install pymysql
```

1. 建立数据库连接(db = pymysql.connect(...))
    1. 参数host：连接的mysql主机，如果本机是'127.0.0.1'
    2. 参数port：连接的mysql主机的端口，默认是3306
    3. 参数database：数据库的名称
    4. 参数user：连接的用户名
    5. 参数password：连接的密码
    6. 参数charset：通信采用的编码方式，推荐使用utf8

2. 创建游标对象(cur = db.cursor())
3. 游标方法: cur.execute("insert ....")
4. 提交到数据库或者获取数据 : db.commit()/cur.fetchall()
5. 关闭游标对象 ：cur.close()
6. 断开数据库连接 ：db.close()

