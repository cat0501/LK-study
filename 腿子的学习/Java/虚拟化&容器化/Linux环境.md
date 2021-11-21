> **环境配置记录：**
> **192.168.1.7 宿主机			172.20.10.10**
>
> **192.168.1.8 CentOS-7_64_2		172.20.10.14**
>
> **192.168.1.9 CentOS-7_64_1		172.20.10.12**
>
> **39.101.189.62阿里云服务器**



> **部分安装包存放于百度网盘中：**
>
> **链接: https://pan.baidu.com/s/1GVHczo0gkAVTs8WkD01PwQ** 
>
> **提取码: bd4q** 



## 0.Linux环境

**网络配置**

```shell
# 第一步：切换到root权限
su root

# 第二步：分配ip
dhclient

# 第三步：ifconfig拿到分配的ip
inet 172.20.10.14

# 第四步：网卡配置(参照如下)
vim /etc/sysconfig/network-scripts/ifcfg-ens33
# 第五步：重启网卡
systemctl restart network.service

# 第六步：测试与外网和宿主机的连通性,宿主机ping虚拟机
ping www.baidu.com
ping 主机ip   			 ping 172.20.10.10

ping 另一台虚拟机ip 	ping 171.20.10.15





dhclient(7353) is already running - exiting.问题解决：杀死进程
ps -ef | grep dhclient
kill -9 7587
```



```shell
# 网卡配置

TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="f14aacb1-441e-4bbb-8018-014e4a84b842"
DEVICE="ens33"
ONBOOT="yes"
IPADDR=172.20.10.14
NETMASK=255.255.255.0
GATEWAY=172.20.10.1
DNS1=119.29.29.29
```



## 1.Git安装

**安装包**

```shell
https://mirrors.edge.kernel.org/pub/software/scm/git/
```

**安装可能需要的依赖**

```shell
yum install curl-devel expat-devel gettext-devel openssl-devel zlibdevel gcc-c++ perl-ExtUtils-MakeMaker

# 报错解决：https://blog.csdn.net/weicuidi/article/details/52935100
vi /etc/resolv.conf
service network restart
# 试一下yum update命令，应该就可以了。如果还不可以，可以再进行下一步:
# 进入 /etc/yum.repos.d ，编辑vi CentOS-Base.repo，找到如下的黑色加粗部分，注释掉第一行(mirrorlist)，取消注释第二行(baseurl)就可以了

# Another app is currently holding the yum lock解决方法
rm -f /var/run/yum.pid
```

**编译安装**

```shell
[root@localhost ~]# cd git-2.26.2/
[root@localhost git-2.26.2]# make configure
[root@localhost git-2.26.2]# ./configure --prefix=/usr/local/git
[root@localhost git-2.26.2]# make profix=/usr/local/git
[root@localhost git-2.26.2]# make install
```

make: *** [configure] Error 127解决：https://blog.csdn.net/weixin_42890739/article/details/107316049

**加入环境变量**

```shell
export GIT_HOME=/usr/local/git
export PATH=$PATH:$GIT_HOME/bin

source /etc/profile
```

**查看结果**

```shell
[root@localhost git-2.26.2]# git --version
git version 1.8.3.1
```

## 2.JDK安装

**安装包**

```shell
jdk-8u161-linux-x64.tar.gz
https://www.cnblogs.com/aijiao/p/14344582.html
```

**卸载已有的OPENJDK（如果有）**

```shell
# 查找已经安装的 OpenJDK 包：
rpm -qa | grep java
# 将 java 开头的安装包均卸载即可：
yum -y remove java-1.8.0-openjdk-1.8.0.131-11.b12.el7.x86_64
yum -y remove java-1.7.0-openjdk-1.7.0.141-2.6.10.5.el7.x86_64
yum -y remove java-1.7.0-openjdk-headless-1.7.0.141-2.6.10.5.el7.x86_64
yum -y remove java-1.8.0-openjdk-headless-1.8.0.131-11.b12.el7.x86_64
```

**创建⽬录并解压**

```shell
cd /usr/local/
mkdir java
cd java

tar -zxvf /root/jdk-8u161-linux-x64.tar.gz -C ./
```

**配置JDK环境变量**

```shell
# 编辑 /etc/profile ⽂件，在⽂件尾部加⼊如下 JDK 环境配置即可
JAVA_HOME=/usr/local/java/jdk1.8.0_161
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
# 执⾏如下命令让环境变量生效
source /etc/profile
```

**验证JDK安装结果**

```shell
java -version
javac

java version "1.8.0_161"
```

## 3.Node安装

**安装包**

```shell
node-v14.18.1-linux-x64.tar.xz
```

**创建目录并解压**

```shell
cd /usr/local/
mkdir node
cd node

# 将 Node 的安装包解压到 /usr/local/node 中即可
tar -xJvf /root/node-v14.18.1-linux-x64.tar.xz -C ./
```

**配置NODE系统环境变量**

```shell
# 编辑 ~/.bash_profile ⽂件，在⽂件末尾追加如下信息:
# Nodejs
export PATH=/usr/local/node/node-v14.18.1-linux-x64/bin:$PATH
# 刷新环境变量，使之⽣效
source ~/.bash_profile
```

**检查安装结果**

```shell
node -v
v14.18.1

npm version
{
  npm: '6.14.15',
  ares: '1.17.2',
  brotli: '1.0.9',
  cldr: '39.0',
  icu: '69.1',
  llhttp: '2.1.4',
  modules: '83',
  napi: '8',
  nghttp2: '1.42.0',
  node: '14.18.1',
  openssl: '1.1.1l',
  tz: '2021a',
  unicode: '13.0',
  uv: '1.42.0',
  v8: '8.4.371.23-node.84',
  zlib: '1.2.11'
}

npx -v
6.14.15
```

## 4.Python安装

CentOS 7.4 默认⾃带了⼀个 Python2.7 环境，然⽽现在主流都是 Python3 ，所以接下来再装⼀个 Python3 ，打造⼀个共存的环境。

**准备PYTHON3安装包并解压**

```shell
Python-3.8.3.tgz

tar zxvf Python-3.8.3.tgz
```

**安装相关预备环境**

```shell
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlitedevel readline-devel tk-devel gcc make
```

**编译并安装**

```shell
cd Python-3.8.3/
./configure prefix=/usr/local/python3
make && make install
```

**添加软链接**

我们还需要将刚刚安装⽣成的⽬录 /usr/local/python3 ⾥的 python3 可执⾏⽂件做⼀份软链接，链接到 /usr/bin 下，⽅便后续使⽤python3

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

**验证安装**

命令⾏输⼊ python3 ，即可查看 Python3 版本的安装结果

```shell
Python 3.8.3 (default, Oct 22 2021, 22:05:23)
```

⽽输⼊ python ，依然还是 python2.7.5 环境

## 5.Maven安装

**准备MAVEN安装包并解压**

```shell
apache-maven-3.6.3-bin.tar.gz 安装包，并将其放置于提前创建好的 /opt/maven⽬录下。

tar zxvf apache-maven-3.6.3-bin.tar.gz
```

**配置MAVEN加速镜像源**

配置的是阿⾥云的maven镜像源。

```shell
# 编辑修改 /opt/maven/apache-maven-3.6.3/conf/settings.xml⽂件，在 <mirrors></mirrors> 标签对⾥添加如下内容即可：

<mirror>
		<id>alimaven</id> 
		<name>aliyun maven</name> 
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
		<mirrorOf>central</mirrorOf>
</mirror>
```

**配置环境变量**

因为下载的是⼆进制版安装包，所以解压完，配置好环境变量即可使⽤了。

```shell
# 编辑修改 /etc/profile ⽂件，在⽂件尾部添加如下内容，配置 maven 的安装路径:
export MAVEN_HOME=/opt/maven/apache-maven-3.6.3
export PATH=$MAVEN_HOME/bin:$PATH

# 接下来执⾏ source /etc/profile 来刷新环境变量，让 maven 环境的路径配置⽣效
source /etc/profile
```

**检验安装结果**

```shell
# mvn -v
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /opt/maven/apache-maven-3.6.3
```



## 6.MySQL安装

**⾸先准备安装包**

```shell
mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
```

**卸载系统⾃带的MARIADB（如果有)**

```shell
# ⾸先查询已安装的 Mariadb 安装包,并卸载：
rpm -qa|grep mariadb
yum -y remove mariadb-libs-5.5.56-2.el7.x86_64
```

**解压MYSQL安装包**

```shell
# 将上⾯准备好的 MySQL 安装包解压到 /usr/local/ ⽬录，并重命名为 mysql
tar -zxvf /root/mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
mv mysql-5.7.30-linux-glibc2.12-x86_64 mysql
```

**创建MYSQL⽤户和⽤户组**

```shell
groupadd mysql
useradd -g mysql mysql

# 新建 /usr/local/mysql/data ⽬录，后续备⽤
mkdir data
```

**修改MYSQL⽬录的归属⽤户**	

```shell
chown -R mysql:mysql ./
```

**准备MYSQL的配置⽂件**

```shell
# 在 /etc ⽬录下新建 my.cnf ⽂件,写⼊如下简化配置：

[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock
[mysqld]
skip-name-resolve
#设置3306端⼝
port = 3306
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装⽬录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放⽬录
datadir=/usr/local/mysql/data
# 允许最⼤连接数
max_connections=200
# 服务端使⽤的字符集默认为8⽐特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使⽤的默认存储引擎
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M

# 同时使⽤如下命令创建 /var/lib/mysql ⽬录，并修改权限：
mkdir /var/lib/mysql
chmod 777 /var/lib/mysql
```



**正式开始安装MYSQL**

```shell
cd /usr/local/mysql
./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```

```shell
2021-10-23T05:44:11.126582Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2021-10-23T05:44:11.304715Z 0 [Warning] InnoDB: New log files created, LSN=45790
2021-10-23T05:44:11.327973Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2021-10-23T05:44:11.409104Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 3f87734f-33c4-11ec-afb2-000c29ec8a06.
2021-10-23T05:44:11.411183Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2021-10-23T05:44:11.411892Z 1 [Note] A temporary password is generated for root@localhost: oF&_dewD-7Gd
```

```shell
# 上面得到的root密码，首次登录使用
oF&_dewD-7Gd
```

**复制启动脚本到资源⽬录**

```shell
# 复制
cp ./support-files/mysql.server /etc/init.d/mysqld

# 并修改 /etc/init.d/mysqld ，修改其 basedir 和 datadir 为实际对应⽬录：
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
```

**设置MYSQL系统服务并开启⾃启**

```shell
# ⾸先增加 mysqld 服务控制脚本执⾏权限：
chmod +x /etc/init.d/mysqld
```

```shell
# 同时将 mysqld 服务加⼊到系统服务：
chkconfig --add mysqld
```

```shell
# 最后检查 mysqld 服务是否已经⽣效即可：
chkconfig --list mysqld

# mysqld         	0:off	1:off	2:on	3:on	4:on	5:on	6:off
这样就表明 mysqld 服务已经⽣效了，在2、3、4、5运⾏级别随系统启动⽽⾃动启动，以后可以直接使⽤ service 命令控制 mysql 的启停。
```

**启动MYSQLD**

```shell
service mysqld start
```

**将 MYSQL 的 BIN ⽬录加⼊ PATH 环境变量**

```shell
# 这样⽅便以后在任意⽬录上都可以使⽤ mysql 提供的命令。编辑 ~/.bash_profile ⽂件，在⽂件末尾处追加如下信息:
export PATH=$PATH:/usr/local/mysql/bin

source ~/.bash_profile
```

**⾸次登陆MYSQL**

```shell
# 解决bash: mysql: command not found... 的方法
https://blog.csdn.net/qq_38695182/article/details/79953474

# root 账户登录 mysql ，使⽤上⽂安装完成提示的密码进⾏登⼊
mysql -u root -p
```

**接下来修改ROOT账户密码**

```shell
mysql>alter user user() identified by "111111";
mysql>flush privileges;
```

**设置远程主机登录**

```shell
mysql> use mysql;
mysql> update user set user.Host='%' where user.User='root';
mysql> flush privileges;
```

**最后利⽤NAVICAT等⼯具进⾏测试即可**

```shell
172.20.10.12
```



## 7.Redis安装

**⾸先准备REDIS安装包**	

```shell
# redis-6.2.6.tar.gz,直接放在了 root ⽬录下
```

**解压安装包**

```shell
# 在 /usr/local/ 下创建 redis ⽂件夹并进⼊
cd /usr/local/
mkdir redis
cd redis	

# 将 Redis 安装包解压到 /usr/local/redis 中即可
tar zxvf /root/redis-6.2.4.tar.gz -C ./
```

**编译并安装**

```shell
cd redis-6.2.4/
make && make install
```

**将 REDIS 安装为系统服务并后台启动**

```shell
# 进⼊ utils ⽬录，并执⾏如下脚本即可：
cd utils/

# ./install_server.sh报错
https://blog.csdn.net/weixin_45949073/article/details/109213758
# 此处我全部选择的默认配置即可，有需要可以按需定制
./install_server.sh
```

**查看REDIS服务启动情况**

```shell
systemctl status redis_6379.service
```

**启动REDIS客户端并测试**

```shell
# 启动⾃带的 redis-cli 客户端，测试通过：
[root@localhost utils]# redis-cli
127.0.0.1:6379> 
127.0.0.1:6379> set foo bar
OK
127.0.0.1:6379>  get foo
"bar"

# 但是此时只能在本地访问，⽆法远程连接，因此还需要做部分设置
```

**设置允许远程连接**

```shell
# 编辑 redis 配置⽂件
vim /etc/redis/6379.conf
# 将 bind 127.0.0.1 修改为 0.0.0.0
# 然后重启 Redis 服务即可：
systemctl restart redis_6379.service
```

**设置访问密码**

```shell
# 编辑 redis 配置⽂件
vim /etc/redis/6379.conf
# 找到如下内容,去掉注释，将 foobared 修改为⾃⼰想要的密码，保存即可。
#requirepass foobared
# 保存，重启 Redis 服务
systemctl restart redis_6379.service

# 这样后续的访问需要先输⼊密码认证通过⽅可：
[root@localhost utils]# redis-cli
127.0.0.1:6379> get foo
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 1
OK 
127.0.0.1:6379> get foo
"bar"
```

## 8.消息队列RabbitMQ安装

**⾸先安装ERLANG环境，安装RABBITMQ**

```shell
# 详细过程（可用）
https://blog.csdn.net/qq_39135287/article/details/95725385
----------------------------------------------------------

# RabbitMQ-rabbitmq-sever启动成功，但是无法访问web管理模块页面：防火墙未关闭（原因）
https://blog.csdn.net/qq_41358151/article/details/115717666
----------------------------------------------------------
1.查看web管理模块是否启动
./rabbitmq-plugins list

原因：防火墙未关闭
解决：执行如下指令即可

systemctl status firewalld

systemctl stop firewalld
```

```shell
# centos7报错Loading mirror speeds from cached hostfile解决方法
https://www.cnblogs.com/huifeidezhuzai/p/15047976.html

# 接下来执⾏如下命令正式安装 erlang 环境：
yum install erlang-22.3.3-1.el7.x86_64
```



**设置RABBITMQ开机启动**

**启动RABBITMQ服务**

**开启WEB可视化管理插件：**

```shell
cd /usr/local/software/rabbitmq_software/rabbitmq_server-3.7.16/sbin

./rabbitmq-plugins enable rabbitmq_management   //开启web管理界面插件 
```



**访问可视化管理界⾯：**



## 9.应用服务器Tomcat安装

**准备安装包**

```shell
apache-tomcat-8.5.55.tar.gz
```

**解压并安装**

```shell
# 创建 tomcat ⽂件夹并进⼊
cd /usr/local/
mkdir tomcat
cd tomcat

# 解压到 /usr/local/tomcat 中即可
tar -zxvf /root/apache-tomcat-8.5.55.tar.gz -C./
```

**启动TOMCAT**

```shell
# 直接进 apache-tomcat-8.5.55 ⽬录，执⾏其中 bin ⽬录下的启动脚本即可
[root@localhost apache-tomcat-8.5.55]# cd bin/
[root@localhost bin]# ./startup.sh

# 这时候浏览器访问： 你的主机IP:8080 ，得到如下画⾯说明成功启动了
```

**配置快捷操作和开机启动**

```shell
# ⾸先进⼊ /etc/rc.d/init.d ⽬录，创建⼀个名为 tomcat 的⽂件，并赋予执⾏权限
[root@localhost ~]# cd /etc/rc.d/init.d/
[root@localhost init.d]# touch tomcat
[root@localhost init.d]# chmod +x tomcat

# 接下来编辑 tomcat ⽂件，并在其中加⼊如下内容：
#!/bin/bash
#chkconfig:- 20 90
#description:tomcat
#processname:tomcat
TOMCAT_HOME=/usr/local/tomcat/apache-tomcat-8.5.55
case $1 in
start) su root $TOMCAT_HOME/bin/startup.sh;;
stop) su root $TOMCAT_HOME/bin/shutdown.sh;;
 *) echo "require start|stop" ;;
esac

# 这样后续对于Tomcat的开启和关闭只需要执⾏如下命令即可：
service tomcat start
service tomcat stop

# 最后加⼊开机启动即可：
chkconfig --add tomcat
chkconfig tomcat on
```

## 10.Web服务器Nginx安装

**⾸先安装包并解压**

```shell
# root目录下
nginx-1.17.10.tar.gz

cd /usr/local/
mkdir nginx
cd nginx

# 解压
tar zxvf /root/nginx-1.17.10.tar.gz -C ./
```

**预先安装额外的依赖**

```shell
yum -y install pcre-devel
yum -y install openssl openssl-devel
```

**编译安装NGINX**

```shell
cd nginx-1.17.10
./configure
make && make install

# 可执行文件位置
/usr/local/nginx/sbin/nginx
```

**启动NGINX**

```shell
[root@localhost sbin]# /usr/local/nginx/sbin/nginx

# 停止
/usr/local/nginx/sbin/nginx -s stop
# 修改配置文件后重新加载
/usr/local/nginx/sbin/nginx -s reload
# 配置文件位置
/usr/local/nginx/conf/nginx.conf

# 默认80端口被占用改为81，修改配置文件后重新加载
https://blog.csdn.net/yufeng_lai/article/details/88819981
```

**浏览器验证启动情况**

```shell
ip:port
```

## 11.Docker环境安装

**安装DOCKER**

```shell
yum install -y docker
```

**开启Docker服务**

```shell
systemctl start docker.service
```

**查看安装结果**

```shell
# docker version

Client:
 Version:         1.13.1
 API version:     1.26
 Package version: docker-1.13.1-208.git7d71120.el7_9.x86_64
 Go version:      go1.10.3
 Git commit:      7d71120/1.13.1
 Built:           Mon Jun  7 15:36:09 2021
 OS/Arch:         linux/amd64

Server:
 Version:         1.13.1
 API version:     1.26 (minimum version 1.12)
 Package version: docker-1.13.1-208.git7d71120.el7_9.x86_64
 Go version:      go1.10.3
 Git commit:      7d71120/1.13.1
 Built:           Mon Jun  7 15:36:09 2021
 OS/Arch:         linux/amd64
 Experimental:    false
```

**设置开机启动**

```shell
systemctl enable docker.service
```

**配置DOCKER镜像下载加速**

```shell
# 默认安装后的 Docker 环境在拉取 Docker 镜像时速度很慢：因此我们需要⼿动配置镜像加速源，提升获取 Docker 镜像的速度。
# 编辑配置文件
vim /etc/docker/daemon.json
# 加⼊加速镜像源地址即可：⽐如这⾥使⽤的是 ⽹易 的加速源，其他像 阿⾥云 、 DaoCloud 这些也都提供加速源，按需选择即可。
{
"registry-mirrors": ["http://hub-mirror.c.163.com"] 
}
# 加完加速地址后，重新加载配置⽂件，重启 docker 服务即可：
systemctl daemon-reload
systemctl restart docker.service
# 这样镜像加速器就配置完成了，后续下载 docker 镜像速度将⼤增。
```

## 12.ZooKeeper

**准备安装包，解压并安装**

```shell
# root目录下
apache-zookeeper-3.6.1-bin.tar.gz

cd /usr/local/
mkdir zookeeper
cd zookeeper

# 解压
tar -zxvf /root/apache-zookeeper-3.6.3-bin.tar.gz -C ./
```

**创建data目录**

```shell
# 在 /usr/local/zookeeper/apache-zookeeper-3.6.3-bin ⽬录中创建⼀个 data ⽬录,等下该 data ⽬录地址要配到 ZooKeeper 的配置⽂件中
```

**创建配置⽂件并修改**

```shell
# 进⼊到 zookeeper 的 conf ⽬录，复制 zoo_sample.cfg 得到 zoo.cfg ：修改配置⽂件 zoo.cfg ，将其中的 dataDir 修改为上⾯刚创建的 data ⽬录，其他选项可以按需配置

[root@localhost apache-zookeeper-3.6.3-bin]# cd conf/
[root@localhost conf]# cp zoo_sample.cfg zoo.cfg
```

**启动ZOOKEEPER**

```shell
[root@localhost apache-zookeeper-3.6.3-bin]# ./bin/zkServer.sh start

# 启动后可以通过如下命令来检查启动后的状态：
[root@localhost apache-zookeeper-3.6.3-bin]# ./bin/zkServer.sh status

ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper/apache-zookeeper-3.6.3-bin/bin/../conf/zoo.cfg
Client port found: 2181. Client address: localhost. Client SSL: false.
Mode: standalone

# 可以看出默认会绑定端⼝ 2181
```

**配置环境变量**

```shell
vim /etc/profile

export ZOOKEEPER_HOME=/usr/local/zookeeper/apache-zookeeper-3.6.1-bin
export PATH=$PATH:$ZOOKEEPER_HOME/bin

source /etc/profile
```

**设置开机⾃启**

```shell
# ⾸先进⼊ /etc/rc.d/init.d ⽬录，创建⼀个名为 zookeeper 的⽂件，并赋予执⾏权限

[root@localhost ~]# cd /etc/rc.d/init.d/
[root@localhost init.d]# touch zookeeper
[root@localhost init.d]# chmod +x zookeeper
```

```shell
# 编辑 zookeeper ⽂件，并在其中加⼊如下内容：

#!/bin/bash
#chkconfig:- 20 90
#description:zookeeper
#processname:zookeeper
ZOOKEEPER_HOME=/usr/local/zookeeper/apache-zookeeper-3.6.1-bin
export JAVA_HOME=/usr/local/java/jdk1.8.0_161 # 此处根据你的实际情况更换对
应
case $1 in
start) su root $ZOOKEEPER_HOME/bin/zkServer.sh start;;
stop) su root $ZOOKEEPER_HOME/bin/zkServer.sh stop;;
 status) su root $ZOOKEEPER_HOME/bin/zkServer.sh status;;
restart) su root $ZOOKEEPER_HOME/bin/zkServer.sh restart;;
 *) echo "require start|stop|status|restart" ;;
esac

# 最后加⼊开机启动即可：
chkconfig --add zookeeper
chkconfig zookeeper on
```







