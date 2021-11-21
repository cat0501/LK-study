## 一、Mysql

### 1 说明

#### 1.1 SQL&&NOSQL

**关系型数据库 ( SQL )**
**MySQL , Oracle , SQL Server , SQLite , DB2 , ...**
关系型数据库通过外键关联来建立表与表之间的关系

**非关系型数据库 ( NOSQL )**
**Redis , MongoDB , ...**
非关系型数据库通常指数据以对象的形式存储在数据库中，而对象之间的关系通过每个对象自身的属性来决定



### 2 安装Mysql

#### mac

https://blog.csdn.net/hechenhongbo/article/details/105224653

```bash
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

#### win



### 3 navicate 15

https://xclient.info/s/navicat-premium.html



### 4 基础

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211012014602515.png" alt="image-20211012014602515" style="zoom: 50%;" />

#### 4.1 库操作

创建数据库

删除数据库

查看数据库

使用数据库

修改数据库

#### 4.2 建表

#### 4.3 数据类型

#### 4.4  字段属性

#### 4.5 表类型

> **MySQL的数据表的类型 : MyISAM , InnoDB , HEAP , BOB , CSV等**

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211012015211205.png" alt="image-20211012015211205" style="zoom:50%;" />

- **适用 MyISAM : 节约空间及相应速度**
- **适用 InnoDB : 安全性 , 事务处理及多用户操作数据表**



### 5 DML增删改



### 6  DQL查

### 7 函数

### 8 事务

### 9 索引

### 10 权限管理

### 11 数据库设计



## 二、JDBC

> SUN公司为了简化、统一对数据库的操作，定义了一套Java操作数据库的规范（接口），称之为JDBC。
> 这套接口由数据库厂商去实现，这样，开发人员只需要学习jdbc接口，并通过jdbc加载具体的驱动，就
> 可以操作数据库。

JDBC全称为：**Java Data Base Connectivity**（java数据库连接），它主要由**接口**组成。
组成JDBC的２个包：**java.sql、javax.sql**
开发JDBC应用需要以上2个包的支持外，还需要导入相应JDBC的数据库实现(即数据库驱动)。



### 1 一个JDBC程序

实验环境

```java
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci; USE jdbcStudy; CREATE TABLE users( id INT PRIMARY KEY, NAME VARCHAR(40), PASSWORD VARCHAR(40), email VARCHAR(60), birthday DATE );INSERT INTO users(id,NAME,PASSWORD,email,birthday) VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'), (2,'lisi','123456','lisi@sina.com','1981-12-04'), (3,'wangwu','123456','wangwu@sina.com','1979-12-04');
```



### 2 JDBC介绍



### 3 数据库驱动

###  4 对象说明







