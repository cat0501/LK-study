windows常用cmd命令：

清空屏幕内容：`cls`



[TOC]





# 一、数据库MySQL详解

## 1.安装

### 1.1 Windows



### 1.2 mac os

https://blog.csdn.net/hechenhongbo/article/details/105224653

```shell
安装mysql

cat@zhangjilindembp ~ % brew install mysql
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/li
######################################################################## 100.0%
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/lz
######################################################################## 100.0%
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/pr
######################################################################## 100.0%
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/zs
######################################################################## 100.0%
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/my
######################################################################## 100.0%
==> Installing dependencies for mysql: libevent, lz4, protobuf and zstd
==> Installing mysql dependency: libevent
==> Pouring libevent-2.1.12.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/libevent/2.1.12: 57 files, 2MB
==> Installing mysql dependency: lz4
==> Pouring lz4-1.9.3.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/lz4/1.9.3: 22 files, 657.8KB
==> Installing mysql dependency: protobuf
==> Pouring protobuf-3.17.3.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/protobuf/3.17.3: 210 files, 18.0MB
==> Installing mysql dependency: zstd
==> Pouring zstd-1.5.0.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/zstd/1.5.0: 31 files, 3.5MB
==> Installing mysql
==> Pouring mysql-8.0.26.big_sur.bottle.1.tar.gz
==> /usr/local/Cellar/mysql/8.0.26/bin/mysqld --initialize-insecure --user=cat -
==> Caveats
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -uroot

To start mysql:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/mysql/bin/mysqld_safe --datadir=/usr/local/var/mysql
==> Summary
🍺  /usr/local/Cellar/mysql/8.0.26: 304 files, 296MB
==> Caveats
==> mysql
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -uroot

To start mysql:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/mysql/bin/mysqld_safe --datadir=/usr/local/var/mysql
cat@zhangjilindembp ~ % brew services start mysql
==> Successfully started `mysql` (label: homebrew.mxcl.mysql)
cat@zhangjilindembp ~ % mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: N
Please set the password for root here.

New password: 

Re-enter new password: 
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : n

 ... skipping.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done! 
cat@zhangjilindembp ~ % 
```



### 1.3 Linux

**1.准备安装包：**

mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz安装包，放于root目录下。

**2.卸载系统⾃带的MARIADB（如果有）**

**3.解压MySQL安装包**



带来的问题解决（测试可用）：

https://www.cnblogs.com/wanglle/p/11416987.html#!comments

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021130921603.png" alt="image-20211021130921603" style="zoom: 67%;" />

## 2.基本概念

## 3.MySQL数据类型、运算符、常用函数

### 3.1数据类型

#### 3.1.1数值类型

**MySQL 支持所有标准的 SQL 数据类型**



包括严格数据类型的**`严格数值类型`**，这些数据类型有：

- INTEGER
- SMALLINT
- DECIMAL
- NUMERIC

**`近似数值数据类型`** 并不用严格按照指定的数据类型进行存储，这些有：

- FLOAT
- REAL
- DOUBLE PRECISION

还有经过扩展之后的数据类型，它们是：

- TINYINT
- MEDIUMINT
- BIGINT
- BIT

> 其中 INT 是 INTEGER 的缩写，DEC 是 DECIMAL 的缩写。



**所有数据类型汇总如下：**

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021131911371.png" alt="image-20211021131911371" style="zoom:67%;" />

##### 整数

我们一般会在 SQL 语句的数据类型后面加上指定长度来表示数据类型许可的范围，例如：

```sql
int(7)
```

表示 int 类型的数据最大长度为 7，如果填充不满的话会自动填满，如果不指定 int 数据类型的长度的话，默认是 `int(11)`。



```sql
create table test1(aId int, bId int(5));

/* 然后我们查看一下表结构 */
desc test1;
```

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021132736925.png" alt="image-20211021132736925" style="zoom:80%;" />



##### 小数

有两种类型；一种是**`浮点数`**类型，一种是**`定点数`**类型；



##### 位类型







#### 3.1.2日期时间类型

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133022948.png" alt="image-20211021133022948" style="zoom:80%;" />





#### 3.1.3字符串类型







### 3.2运算符

- 算术运算符
- 比较运算符
- 逻辑运算符
- 位运算符

##### 算术运算符

![image-20211021133231576](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133231576.png)

##### 比较运算符

![image-20211021133344138](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133344138.png)

##### 逻辑运算符

![image-20211021133410448](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133410448.png)

##### 位运算符

![image-20211021133433671](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133433671.png)

### 3.3常用函数

#### 3.3.1字符串函数

![image-20211021133759175](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133759175.png)

#### 3.3.2数值函数

![image-20211021133738555](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133738555.png)

#### 3.3.3日期和时间函数

![image-20211021133721794](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133721794.png)

#### 3.3.4流程函数

#### 3.3.5其他函数

![image-20211021133639264](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021133639264.png)







# 二、9道MySQL面试题

MySQL 涉及到数据存储、锁、磁盘寻道、分页等操作系统概念，而且互联网对 MySQL 的注重程度是不言而喻的，后面要加紧对 MySQL 的研究。

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211020114005715.png" alt="image-20211020114005715" style="zoom:80%;" />







## 1.非关系型数据库与关系型数据库的对比

### 1.非关系型数据库NoSQL

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021125106316.png" alt="image-20211021125106316" style="zoom:80%;" />

非关系型数据库（感觉翻译不是很准确）称为 **NoSQL**，也就是 Not Only SQL，不仅仅是 SQL。非关系型数据库不需要写一些复杂的 SQL 语句，其内部存储方式是以 **key-value** 的形式存在可以把它想象成电话本的形式，每个人名（key）对应电话（value）。常见的非关系型数据库主要有 **Hbase、Redis、MongoDB** 等。

非关系型数据库不需要经过 SQL 的重重解析，所以性能很高；非关系型数据库的可扩展性比较强，数据之间没有耦合性，遇见需要新加字段的需求，就直接增加一个 key-value 键值对即可。



<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021125046584.png" alt="image-20211021125046584" style="zoom:80%;" />

关系型数据库以**表格**的形式存在，以**行和列**的形式存取数据，关系型数据库这一系列的行和列被称为表，无数张表组成了数据库，常见的关系型数据库有 **Oracle、MySQL、Microsoft SQL Server、DB2**等。

关系型数据库能够支持复杂的 SQL 查询，能够体现出数据之间、表之间的关联关系；关系型数据库也支持事务，便于提交或者回滚。


- Oracle ，这个数据库是目前使用最广泛的数据库，是国内的政府机关以及金融机构使用的最多的一个数据库
- Mysql，现在这个数据库也有社区版和收费版，现在也是属于Oracle旗下的产品，是国内中小企业使用的最多的一个数据库
- Sql Server 是微软开发的一个数据库
- MariaDB，也是有Mysql的作者来开发的一个开源数据库，当然目前在国内使用的不多
- DB2，这个是IBM开发出来的产品
- SQLite，是一个微型的数据库，一般用在手机的操作系统中，用来存储手机中的通讯录、短信之类的内容





## 2.MySQL事务的四大特性

### 四大特性ACID：`原子性`、`一致性`、`隔离性`、`持久性`

- **`原子性(Atomicity)`**: 原子性指的就是 MySQL 中的包含事务的操作要么全部成功、要么全部失败回滚，因此事务的操作如果成功就必须要全部应用到数据库，如果操作失败则不能对数据库有任何影响。

> 这里涉及到一个概念，什么是 MySQL 中的事务？
>
> 事务是一组操作，组成这组操作的各个单元，要不全都成功要不全都失败，这个特性就是事务。
>
> 在 MySQL 中，事务是在引擎层实现的，只有使用 innodb 引擎的数据库或表才支持事务。
>

- **`一致性(Consistency)`**：一致性指的是一个事务在执行前后其状态一致。比如 A 和 B 加起来的钱一共是 1000 元，那么不管 A 和 B 之间如何转账，转多少次，事务结束后两个用户的钱加起来还得是 1000，这就是事务的一致性。

- **`持久性(Durability)`**: 持久性指的是一旦事务提交，那么发生的改变就是永久性的，即使数据库遇到特殊情况比如故障的时候也不会产生干扰。

- **`隔离性(Isolation)`**：隔离性需要重点说一下，当多个事务同时进行时，就有可能出现脏读(dirty read)、不可重复读(non-repeatable read)、幻读(phantom read) 的情况，为了解决这些并发问题，提出了隔离性的概念。

### 三大并发问题：脏读、不可重复读、虚幻读

> **脏读：事务 A 读取了事务 B 更新后的数据，但是事务 B 没有提交，然后事务 B 执行回滚操作，那么事务 A 读到的数据就是脏数据**
>
> **不可重复读：事务 A 进行多次读取操作，事务 B 在事务 A 多次读取的过程中执行更新操作并提交，提交后事务 A 读到的数据不一致。**
>
> **幻读：事务 A 将数据库中所有学生的成绩由 A -> B，此时事务 B 手动插入了一条成绩为 A 的记录，在事务 A 更改完毕后，发现还有一条记录没有修改，那么这种情况就叫做出现了幻读。**

### 四种隔离级别：读未提交、读已提交（Oracle）、可重复读（MySQL）、串行化

SQL的隔离级别有四种，它们分别是**`读未提交(read uncommitted)`、`读已提交(read committed)`、`可重复读(repetable read) `和 `串行化(serializable)`**。下面分别来解释一下。

读未提交：读未提交指的是一个事务在提交之前，它所做的修改就能够被其他事务所看到。

读已提交：读已提交指的是一个事务在提交之后，它所做的变更才能够让其他事务看到。

可重复读：可重复读指的是一个事务在执行的过程中，看到的数据是和启动时看到的数据是一致的。未提交的变更对其他事务不可见。

串行化：顾名思义是对于同一行记录，`写`会加`写锁`，`读`会加`读锁`。当出现读写锁冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行。



这四个隔离级别可以解决脏读、不可重复读、幻象读这三类问题。总结如下：

![image-20211021102355249](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021102355249.png)

其中隔离级别由低到高是：读未提交 < 读已提交 < 可重复读 < 串行化

隔离级别越高，越能够保证数据的完整性和一致性，但是对并发的性能影响越大。大多数数据库的默认级别是`读已提交(Read committed)`，比如 Sql Server、Oracle ，但是 MySQL 的默认隔离级别是 `可重复读(repeatable-read)`。

## 3.MySQL常见存储引擎的区别

```shell
# 列出所有的存储引擎
SHOW ENGINES
```

![image-20211021104514039](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021104514039.png)



可以看到，InnoDB 是 MySQL 默认支持的存储引擎，支持**事务、行级锁定和外键**。

### MyISAM存储引擎的特点

在 5.1 版本之前，MyISAM 是 MySQL 的默认存储引擎，MyISAM 并发性比较差，使用的场景比较少，主要特点是





### InnoDB存储引擎的特点

自从 MySQL 5.1 之后，默认的存储引擎变成了 InnoDB 存储引擎，相对于 MyISAM，InnoDB 存储引擎有了较大的改变，它的主要特点是

- 支持事务操作，具有事务 ACID 隔离特性，默认的隔离级别是**可重复读(repetable-read)**、通过MVCC（并发版本控制）来实现的。能够解决**脏读**和**不可重复读**的问题。
- InnoDB 支持外键操作。
- InnoDB 默认的锁粒度**行级锁**，并发性能比较好，会发生死锁的情况。
- 和 MyISAM 一样的是，InnoDB 存储引擎也有 **.frm文件存储表结构** 定义，但是不同的是，InnoDB 的表数据与索引数据是存储在一起的，都位于 B+ 数的叶子节点上，而 MyISAM 的表数据和索引数据是分开的。
- InnoDB 有安全的日志文件，这个日志文件用于恢复因数据库崩溃或其他情况导致的数据丢失问题，保证数据的一致性。
- InnoDB 和 MyISAM 支持的索引类型相同，但具体实现因为文件结构的不同有很大差异。
- 增删改查性能方面，如果执行大量的增删改操作，推荐使用 InnoDB 存储引擎，它在删除操作时是对行删除，不会重建表。
  

### 对比

- **锁粒度方面**：由于锁粒度不同，InnoDB 比 MyISAM 支持更高的并发；InnoDB 的锁粒度为行锁、MyISAM 的锁粒度为表锁、行锁需要**对每一行进行加锁，所以锁的开销更大，但是能解决脏读和不可重复读的问题，相对来说也更容易发生死锁**
- **可恢复性上**：由于 InnoDB 是**有事务日志的**，所以在产生由于数据库崩溃等条件后，可以根据日志文件进行恢复。而 MyISAM 则没有事务日志。
- **查询性能上**：MyISAM 要优于 InnoDB，因为 InnoDB 在查询过程中，是需要维护数据缓存，而且查询过程是先定位到行所在的数据块，然后在从数据块中定位到要查找的行；而 MyISAM 可以直接定位到数据所在的内存地址，可以直接找到数据。
- **表结构文件上**： MyISAM 的表结构文件包括：.frm(表结构定义),.MYI(索引),.MYD(数据)；而 InnoDB 的表数据文件为:.ibd和.frm(表结构定义)；

## 4.MySQL基础架构是怎样的

我们可以把 MySQL 拆解成几个零件，如下图：

![image-20211021105615386](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021105615386.png)

大致上来说，MySQL 可以分为 **Server层**和 **存储引擎层**。

Server 层包括连接器、查询缓存、分析器、优化器、执行器，包括大多数 MySQL 中的核心功能，所有跨存储引擎的功能也在这一层实现，包括 存储过程、触发器、视图等。

存储引擎层包括 MySQL 常见的存储引擎，包括 MyISAM、InnoDB 和 Memory 等，最常用的是 InnoDB，也是现在 MySQL 的默认存储引擎。存储引擎也可以在创建表的时候手动指定，比如下面

```sql
CREATE TABLE t (i INT) ENGINE = <Storage Engine>; 
```



**MySQL 的执行过程如下：**

### 4.1 连接器

需要一个`连接器`来连接用户和 MySQL 数据库，我们一般使用

```sql
mysql -u 用户名 -p 密码
```

来进行 MySQL 登陆，和服务端建立连接。在完成 `TCP 握手` 后，连接器会根据你输入的用户名和密码验证你的登录身份。如果用户名或者密码错误，MySQL 就会提示 Access denied for user，来结束执行。如果登录成功后，MySQL 会根据权限表中的记录来判定你的权限。


![image-20211021111007268](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021111007268.png)



### 4.2 查询缓存

> MySQL 在得到一个执行请求后，会首先去 查询缓存 中查找，是否执行过这条 SQL 语句，**之前执行过的语句以及结果会以 key-value 对的形式，被直接放在内存中**。key 是查询语句，value 是查询的结果。如果通过 key 能够查找到这条 SQL 语句，就直接返回 SQL 的执行结果。
>
> 如果语句不在查询缓存中，就会继续后面的执行阶段。执行完成后，执行结果就会被放入查询缓存中。可以看到，如果查询命中缓存，MySQL 不需要执行后面的复杂操作，就可以直接返回结果，效率会很高。

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021111130838.png" alt="image-20211021111130838" style="zoom:80%;" />



### 4.3 分析器

如果没有命中查询，就开始执行真正的 SQL 语句。

- 首先，MySQL 会根据你写的 SQL 语句进行解析，分析器会先做 **词法分析**，你写的 SQL 就是由多个字符串和空格组成的一条 SQL 语句，MySQL 需要识别出里面的字符串是什么，代表什么。
- 然后进行 **语法分析**，根据词法分析的结果， 语法分析器会根据语法规则，判断你输入的这个 SQL 语句是否满足 MySQL 语法。如果 SQL 语句不正确，就会提示 `You have an error in your SQL syntax`

### 4.4 优化器

经过分析器的词法分析和语法分析后，你这条 SQL 就**合法**了，MySQL 就知道你要做什么了。

但是在执行前，还需要进行优化器的处理，优化器会判断你使用了哪种索引，使用了何种连接，优化器的作用就是**确定效率最高的执行方案**。

### 4.5 执行器

MySQL 通过分析器知道了你的 SQL 语句是否合法，你想要做什么操作，通过优化器知道了该怎么做效率最高，然后就进入了执行阶段，开始执行这条 SQL 语句。

在执行阶段，MySQL 首先会判断你有没有执行这条语句的权限，没有权限的话，就会返回没有权限的错误。如果有权限，就打开表继续执行。打开表的时候，执行器就会根据表的引擎定义，去使用这个引擎提供的接口。对于有索引的表，执行的逻辑也差不多。

至此，MySQL 对于一条语句的执行过程也就完成了。



## 5.SQL的执行顺序是怎样的

我们在编写一个查询语句的时候，它的执行顺序你知道吗？

```sql
SELECT DISTINCT
    < select_list >
FROM
    < left_table > < join_type >
JOIN < right_table > ON < join_condition >
WHERE
    < where_condition >
GROUP BY
    < group_by_list >
HAVING
    < having_condition >
ORDER BY
    < order_by_condition >
LIMIT < limit_number >
```

### 5.1FROM连接

首先，对 SELECT 语句执行查询时，对`FROM` 关键字两边的表执行连接，会形成`笛卡尔积`，这时候会产生一个`虚表VT1(virtual table)`



> 在 MySQL 中，有三种类型的表：
> 一种是**永久表**，永久表就是创建以后用来长期保存数据的表
> 一种是**临时表**，临时表也有两类，一种是和永久表一样，只保存临时数据，但是能够长久存在的；还有一种是临时创建的，SQL 语句执行完成就会删除。
> 一种是**虚表**，虚表其实就是视图，数据可能会来自多张表的执行结果。



### 5.2ON过滤

然后对 FROM 连接的结果进行 ON 筛选，创建 VT2，把符合记录的条件存在 VT2 中。

### 5.3JOIN连接

第三步，如果是 OUTER JOIN(left join、right join) ，那么这一步就将添加外部行，如果是 left join 就把 ON 过滤条件的左表添加进来，如果是 right join ，就把右表添加进来，从而生成新的虚拟表 VT3。

### 5.4WHERE过滤

第四步，是执行 WHERE 过滤器，对上一步生产的虚拟表引用 WHERE 筛选，生成虚拟表 VT4。

### 5.5GROUP BY：分组操作

根据 group by 字句中的列，会对 VT4 中的记录进行分组操作，产生虚拟机表 VT5。果应用了group by，那么后面的所有步骤都只能得到的 VT5 的列或者是聚合函数（count、sum、avg等）。

### 5.6HAVING

紧跟着 GROUP BY 字句后面的是 HAVING，使用 HAVING 过滤，会把符合条件的放在 VT6

### 5.7SELECT

第七步才会执行 SELECT 语句，将 VT6 中的结果按照 SELECT 进行刷选，生成 VT7

### 5.8DISTINCT：去重操作

在第八步中，会对 TV7 生成的记录进行去重操作，生成 VT8。事实上如果应用了 group by 子句那么 distinct 是多余的，原因同样在于，分组的时候是将列中唯一的值分成一组，同时只为每一组返回一行记录，那么所有的记录都将是不相同的。

### 5.9ORDER BY：排序操作

应用 order by 子句。按照 order_by_condition 排序 VT8，此时返回的一个游标，而不是虚拟表。sql 是基于集合的理论的，集合不会预先对他的行排序，它只是成员的逻辑集合，成员的顺序是无关紧要的。



<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021112735381.png" alt="image-20211021112735381" style="zoom:67%;" />



## 6.什么是临时表，如何删除临时表

**什么是临时表？**MySQL 在执行 SQL 语句的过程中，通常会临时创建一些`存储中间结果集`的表，临时表只对当前连接可见，在连接关闭时，临时表会被删除并释放所有表空间。

**临时表分为两种：**一种是`内存临时表`，一种是`磁盘临时表`，什么区别呢？内存临时表使用的是 MEMORY 存储引擎，而临时表采用的是 MyISAM 存储引擎。

**MySQL 会在下面这几种情况产生临时表：**

- 使用 UNION 查询：UNION 有两种，一种是**UNION** ，一种是 **UNION ALL** ，它们都用于联合查询；区别是 使用 UNION 会去掉两个表中的重复数据，相当于对结果集做了一下**去重(distinct)**。使用 UNION ALL，则不会排重，返回所有的行。使用 UNION 查询会产生临时表。
- 使用 **TEMPTABLE 算法**或者是 UNION 查询中的视图。TEMPTABLE 算法是一种创建临时表的算法，它是将结果放置到临时表中，意味这要 MySQL 要先创建好一个临时表，然后将结果放到临时表中去，然后再使用这个临时表进行相应的查询。
- ORDER BY 和 GROUP BY 的子句不一样时也会产生临时表。
- DISTINCT 查询并且加上 ORDER BY 时；
- SQL中用到 SQL_SMALL_RESULT 选项时；如果查询结果比较小的时候，可以加上 SQL_SMALL_RESULT 来优化，产生临时表
- FROM 中的子查询；
- EXPLAIN 查看执行计划结果的 Extra 列中，如果使用 **Using Temporary** 就表示会用到临时表。



## 7.MySQL常见索引类型

索引是存储在一张表中特定列上的**`数据结构`**，索引是在列上创建的。并且，索引是一种数据结构。

**在 MySQL 中，主要有下面这几种索引：**

- **全局索引(FULLTEXT)**：全局索引，目前只有 MyISAM 引擎支持全局索引，它的出现是为了解决针对文本的模糊查询效率较低的问题。
- **哈希索引(HASH)**：哈希索引是 MySQL 中用到的唯一 key-value 键值对的数据结构，很适合作为索引。HASH 索引具有一次定位的好处，不需要像树那样逐个节点查找，但是这种查找适合应用于查找单个键的情况，对于范围查找，HASH 索引的性能就会很低。
- **B-Tree 索引**：B 就是 Balance 的意思，BTree 是一种平衡树，它有很多变种，最常见的就是 B+ Tree，它被 MySQL 广泛使用。
- **R-Tree 索引**：R-Tree 在 MySQL 很少使用，仅支持 geometry 数据类型，支持该类型的存储引擎只有MyISAM、BDb、InnoDb、NDb、Archive几种，相对于 B-Tree 来说，R-Tree 的优势在于范围查找。



## 8.varchar和char的区别

**`char`** ：表示的是`定长`的字符串，当你输入小于指定的数目，比如你指定的数目是 `char(6)`，当你输入小于 6 个字符的时候，char 会在你最后一个字符后面补空值。当你输入超过指定允许最大长度后，MySQL 会报错

**`varchar`**： varchar 指的是长度为 n 个字节的可变长度，并且是`非Unicode`的字符数据。n 的值是介于 1 - 8000 之间的数值。存储大小为实际大小。



使用 char 存储定长的数据非常方便、char 检索效率高，无论你存储的数据是否到了 10 个字节，都要去占用 10 字节的空间

使用 varchar 可以存储变长的数据，但存储效率没有 char 高。

## 9.什么是内连接、外连接、交叉连接、【笛卡尔积】

**外连接(OUTER JOIN)**：外连接分为三种，分别是**左外连接(LEFT OUTER JOIN 或 LEFT JOIN)** 、**右外连接(RIGHT OUTER JOIN 或 RIGHT JOIN)** 、**全外连接(FULL OUTER JOIN 或 FULL JOIN)**

### 9.1 左外连接：

又称为左连接，这种连接方式**会显示左表不符合条件的数据行**，右边不符合条件的数据行直接显示 NULL

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211021114209458.png" alt="image-20211021114209458" style="zoom:80%;" />

### 9.2 右外连接：

也被称为右连接，他与左连接相对，这种连接方式会显示右表不符合条件的数据行，左表不符合条件的数据行直接显示 NULL

**MySQL 暂不支持全外连接**

### 9.3 内连接(INNER JOIN)：结合两个表中相同的字段，返回关联字段相符的记录。

![image-20211021114358483](https://gitee.com/code0002/blog-img/raw/master/img/image-20211021114358483.png)



## 10.谈谈SQL优化的经验



查询语句无论是使用哪种判断条件 等于、小于、大于， **WHERE** 左侧的条件查询字段不要使用函数或者表达式
使用 **EXPLAIN** 命令优化你的 SELECT 查询，对于复杂、效率低的 sql 语句，我们通常是使用 explain sql 来分析这条 sql 语句，这样方便我们分析，进行优化。
当你的 SELECT 查询语句只需要使用一条记录时，要使用 **LIMIT 1**
不要直接使用 **SELECT ***，而应该使用具体需要查询的表字段，因为使用 EXPLAIN 进行分析时，SELECT * 使用的是全表扫描，也就是 type = all。
为每一张表设置一个 ID 属性
避免在 **WHERE** 字句中对字段进行 **NULL** 判断
避免在 **WHERE** 中使用 **!=** 或 **<>** 操作符
使用 **BETWEEN AND** 替代 **IN**
为搜索字段创建索引
选择正确的存储引擎，InnoDB 、MyISAM 、MEMORY 等
使用 **LIKE %abc%** 不会走索引，而使用 **LIKE abc%** 会走索引
对于枚举类型的字段(即有固定罗列值的字段)，建议使用**ENUM**而不是**VARCHAR**，如性别、星期、类型、类别等
拆分大的 DELETE 或 INSERT 语句
选择合适的字段类型，选择标准是 尽可能小、尽可能定长、尽可能使用整数。
字段设计尽可能使用 **NOT NULL**
进行水平切割或者垂直分割

> 水平分割：通过建立结构相同的几张表分别存储数据
> 垂直分割：将经常一起使用的字段放在一个单独的表中，分割后的表记录之间是一一对应关系。



文章参考：

https://cxuan.blog.csdn.net/article/details/117730660

https://cxuan.blog.csdn.net/article/details/105594307

https://www.cnblogs.com/sharpest/p/10390035.html

https://blog.csdn.net/yl2isoft/article/details/17205413

https://www.cnblogs.com/jinianjun/archive/2011/11/08/2240525.html

https://www.cnblogs.com/huihuixi/p/12155165.html

https://www.php.cn/faq/418056.html

https://blog.csdn.net/w516162189/article/details/78914035

https://baike.baidu.com/item/聚集索引/11041381?fr=aladdin

https://blog.csdn.net/riemann_/article/details/90324846

https://blog.csdn.net/qq_39101581/article/details/82461076

https://blog.csdn.net/csdn_hklm/article/details/78394412

https://zhidao.baidu.com/question/307471035920165604.html

https://www.zhihu.com/question/24225007

https://baike.baidu.com/item/索引/5716853

https://www.cnblogs.com/ghostwu/p/8544333.html

https://www.cnblogs.com/yuxiuyan/p/6511837.html

https://www.jb51.net/article/147261.htm

https://www.cnblogs.com/zhangchaocoming/p/11380724.html

https://baike.baidu.com/item/myisam/8970102?fr=aladdin

https://segmentfault.com/a/1190000019400925

https://www.csdn.net/gather_2e/MtTaEg4sNDk5MC1ibG9n.html

《极客时间》- MySQL实战45讲

https://www.cnblogs.com/wyaokai/p/10921323.html

https://www.cnblogs.com/hhhhuanzi/p/12296776.html

https://zhidao.baidu.com/question/55127810.html

https://www.cnblogs.com/limuzi1994/p/9684083.html



# 三、MySQL优化

## 1.SQL优化步骤

### 1.1通过 show status 命令了解 SQL 执行次数

### 1.2定位执行效率较低的 SQL

### 1.3通过 EXPLAIN 命令分析 SQL 的执行计划



## 2.索引

### 2.1索引介绍

### 2.2索引分类

### 2.3索引使用

### 2.4查看索引的使用情况





## 3.MySQL分析表、检查表和优化表

MySQL分析表

MySQL检查表

MySQL优化表



## 4.常用SQL优化

尽量，建议，不建议



### 导入的优化



### insert 的优化

#### 如果向同一张表插入多条数据的话，最好一次性插入，这样可以减少数据库建立连接 -> 断开连接的时间，如下所示

### group by 的优化

### order by 的优化

### 优化嵌套查询

### count 的优化

### limit 分页的优化

### SQL 中 IN 包含的值不应该太多

### 只需要一条数据的情况

### 尽量用 union all 来代替 union

### where 条件优化

#### 不建议使用 % 前缀模糊查询，例如 LIKE “%name”或者LIKE “%name%”，这种查询会导致索引失效而进行全表扫描。但是可以使用LIKE “name%”。

### 查询时，尽量指定查询的字段名

#### 尽量使用 select 字段名 这种方式，避免直接 **select***





https://cxuan.blog.csdn.net/article/details/119102296



