##  1.Docker概述

### 1.1说明

> 一款产品从开发到上线，从操作系统，到运行环境，再到应用配置。作为开发+运维之间的协作我们需要关心很多东西，这也是很多互联网公司都不得不面对的问题，特别是各种版本的迭代之后，不同版本环境的兼容，对运维人员是极大的考验！
> 环境配置如此麻烦，换一台机器，就要重来一次，费力费时。很多人想到，能不能从根本上解决问题，**软件可以带环境安装？**也就是说，安装的时候，把原始环境一模一样地复制过来。解决开发人员说的“ 在我的机器上可正常工作”的问题。
> 之前在服务器配置一个应用的运行环境，要安装各种软件，就拿一个基本的工程项目的环境来说吧，Java/Tomcat/MySQL/JDBC驱动包等。安装和配置这些东西有多麻烦就不说了，它还不能跨平台。假如我们是在 Windows 上安装的这些环境，到了 Linux 又得重新装。况且就算不跨操作系统，换另一台同样操作系统的服务器，要移植应用也是非常麻烦的。
> 传统上认为，软件编码开发/测试结束后，所产出的成果即是程序或是能够编译执行的二进制字节码文件等（Java为例）。而为了让这些程序可以顺利执行，开发团队也得准备完整的部署文件，让维运团队得以部署应用程式，开发需要清楚的告诉运维部署团队，用的全部配置文件+所有软件环境。不过，即便如此，仍然常常发生部署失败的状况。

![image-20211012211039706](https://gitee.com/code0002/blog-img/raw/master/img/image-20211012211039706.png)

Docker之所以发展如此迅速，也是因为它对此给出了一个标准化的解决方案。

![image-20211012203516424](https://gitee.com/code0002/blog-img/raw/master/img/image-20211012203516424.png)

**Docker镜像的设计，使得Docker得以打破过去「程序即应用」的观念。通过Docker镜像 ( images ) 将应用程序所需要的系统环境，由下而上打包，达到应用程序跨平台间的无缝接轨运作。**

Docker的思想来自于集装箱，集装箱解决了什么问题？在一艘大船上，可以把货物规整的摆放起来。并且各种各样的货物被集装箱标准化了，集装箱和集装箱之间不会互相影响。那么我就不需要专门运送水果的船和专门运送化学品的船了。只要这些货物在集装箱里封装的好好的，那我就可以用一艘大船把他们都运走。
docker就是类似的理念。



### 1.2历史

Docker和容器技术为什么会这么火爆？说白了，就是因为它**“轻”**。

> 在容器技术之前，业界的网红是虚拟机。虚拟机技术的代表，是**VMWare和OpenStack**。
>
> 相信很多人都用过虚拟机。虚拟机，就是在你的操作系统里面，装一个软件，然后通过这个软件，再模拟一台甚至多台“子电脑”出来。
> 在“子电脑”里，你可以和正常电脑一样运行程序，例如开QQ。如果你愿意，你可以变出好几个“子电脑”，里面都开上QQ。“子电脑”和“子电脑”之间，是相互隔离的，互不影响。

**虚拟机属于虚拟化技术。而Docker这样的容器技术，也是虚拟化技术，属于轻量级的虚拟化。**

> 虚拟机虽然可以隔离出很多“子电脑”，但占用空间更大，启动更慢，虚拟机软件可能还要花钱（例如VMWare）。
> 而容器技术恰好没有这些缺点。它不需要虚拟出整个操作系统，只需要虚拟一个小规模的环境（类似“沙 箱”）。
> 它启动时间很快，几秒钟就能完成。而且，它对资源的利用率很高（一台主机可以同时运行几千个Docker容器）。此外，它占的空间很小，虚拟机一般要几GB到几十GB的空间，而容器只需要MB级甚至KB级。
> 正因为如此，容器技术受到了热烈的欢迎和追捧，发展迅速。



### 1.3docker 理念

Docker是**基于Go语言实现**的云开源项目。
Docker的主要目标是**“Build，Ship and Run Any App , Anywhere”**，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到**“一次封装，到处运行”**。
Linux 容器技术的出现就解决了这样一个问题，而 Docker 就是在它的基础上发展过来的。将应用运行在Docker 容器上面，**而 Docker 容器在任何操作系统上都是一致的**，这就实现了跨平台、跨服务器。只需要一次配置好环境，换到别的机子上就可以一键部署好，大大简化了操作。

### 1.4对比

#### 以前的虚拟机技术

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211012214207166.png" alt="image-20211012214207166" style="zoom: 33%;" />

虚拟机（virtual machine）就是带环境安装的一种解决方案。
它可以在一种操作系统里面运行另一种操作系统，比如在Windows 系统里面运行Linux 系统。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，**而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。**这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。

虚拟机缺点：1、资源占用多  2、冗余步骤多  3 、启动慢

#### 容器虚拟化技术

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211012214334845.png" alt="image-20211012214334845" style="zoom:33%;" />

由于前面虚拟机存在这些缺点，Linux发展出了另一种虚拟化技术：**Linux容器（LinuxContainers，缩写为LXC）**。
Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，**只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。**

#### 开发/运维（DevOps）



### 1.5学习途径

**Docker官网：http://www.docker.com**

![image-20211012213816695](https://gitee.com/code0002/blog-img/raw/master/img/image-20211012213816695.png)

**Docker中文网站：https://www.docker-cn.com  文档地址：https://docs.docker.com/desktop/mac/  非常详细！！**
**Docker Hub官网：https://hub.docker.com （仓库）**



## 2.Docker安装

### 2.1 mac安装

https://blog.csdn.net/lxh_worldpeace/article/details/105454114

### 2.2 基本组成

**架构图**

![image-20211012214949600](https://gitee.com/code0002/blog-img/raw/master/img/image-20211012214949600.png)

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211012215515554.png" alt="image-20211012215515554" style="zoom: 50%;" />



#### 2.2.1 镜像（image）：

Docker 镜像（Image）就是一个只读的模板。**镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。** 就好似 Java 中的**类和对象，类就是镜像，容器就是对象！**

#### 2.2.2 容器（container）：

Docker 利用容器（Container）独立运行一个或一组应用。**容器是用镜像创建的运行实例。 它可以被启动、开始、停止、删除。**每个容器都是相互隔离的，保证安全的平台。 可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等） 和运行在其中的应用程序。容器的定义和镜像几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。

#### 2.2.3 仓库（repository）：

**仓库（Repository）是集中存放镜像文件的场所。** 

> 仓库(Repository)和仓库注册服务器（Registry）是有区别的。仓库注册服务器上往往存放着多个仓 库，每个仓库中又包含了多个镜像，每个镜像有不同的标签（tag）。 
>
> 仓库分为公开仓库（Public）和私有仓库（Private）两种形式。 最大的公开仓库是 Docker Hub(https://hub.docker.com/)，存放了数量庞大的镜像供用户下载。 国内的公开仓库包括阿里云 、网易云 等 12345678

### 2.3 阿里云镜像加速



### 2.4底层原理







## 3.Docker常用命令

### 3.1镜像命令





### 3.2容器命令



### 3.3操作命令



## 4.镜像

### 4.1

## 5.容器数据卷



## 6.DockerFile：微服务项目构建成镜像

## 7.Docker网络原理

## 8.IDEA整合Docker

## Compose

## Swarm

