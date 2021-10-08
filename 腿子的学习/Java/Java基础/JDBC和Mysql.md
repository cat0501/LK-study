## JDBC

> SUN公司为了简化、统一对数据库的操作，定义了一套Java操作数据库的规范（接口），称之为JDBC。
> 这套接口由数据库厂商去实现，这样，开发人员只需要学习jdbc接口，并通过jdbc加载具体的驱动，就
> 可以操作数据库。

JDBC全称为：**Java Data Base Connectivity**（java数据库连接），它主要由**接口**组成。
组成JDBC的２个包：**java.sql、javax.sql**
开发JDBC应用需要以上2个包的支持外，还需要导入相应JDBC的数据库实现(即数据库驱动)。



### 一个JDBC程序

实验环境

```java
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci; USE jdbcStudy; CREATE TABLE users( id INT PRIMARY KEY, NAME VARCHAR(40), PASSWORD VARCHAR(40), email VARCHAR(60), birthday DATE );INSERT INTO users(id,NAME,PASSWORD,email,birthday) VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'), (2,'lisi','123456','lisi@sina.com','1981-12-04'), (3,'wangwu','123456','wangwu@sina.com','1979-12-04');
```





