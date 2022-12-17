# Docker

+++

## 一、基础篇

### 1 Docker简介

#### 1.1 docker是什么

> 问题：为什么会有docker出现？

假定您在开发一个尚硅谷的谷粒商城，您使用的是一台笔记本电脑而且您的开发环境具有特定的配置。其他开发人员身处的环境配置也各有不同。您正在开发的应用依赖于您当前的配置且还要依赖于某些配置文件。此外，您的企业还拥有标准化的测试和生产环境，且具有自身的配置和一系列支持文件。您希望尽可能多在本地模拟这些环境而不产生重新创建服务器环境的开销。请问？

您要如何确保应用能够在这些环境中运行和通过质量检测？并且在部署过程中不出现令人头疼的版本、配置问题，也无需重新编写代码和进行故障修复？

答案就是使用容器。Docker之所以发展如此迅速，也是因为它对此给出了一个标准化的解决方案-----**系统平滑移植，容器虚拟化技术**。

环境配置相当麻烦，换一台机器，就要重来一次，费力费时。很多人想到，能不能从根本上解决问题，**软件可以带环境安装？**也就是说，**安装的时候，把原始环境一模一样地复制过来。开发人员利用 Docker 可以消除协作编码时“在我的机器上可正常工作”的问题**。

![image-20221203193827010](07-docker.assets/image-20221203193827010.png)

之前在服务器配置一个应用的运行环境，要安装各种软件，就拿尚硅谷电商项目的环境来说，Java/RabbitMQ/MySQL/JDBC驱动包等。安装和配置这些东西有多麻烦就不说了，它还不能跨平台。假如我们是在 Windows 上安装的这些环境，到了 Linux 又得重新装。况且就算不跨操作系统，换另一台同样操作系统的服务器，要**移植**应用也是非常麻烦的。

传统上认为，软件编码开发/测试结束后，所产出的成果即是程序或是能够编译执行的二进制字节码等(java为例)。而为了让这些程序可以顺利执行，开发团队也得准备完整的部署文件，让维运团队得以部署应用程式，<font color="gree">开发需要清楚的告诉运维部署团队，用的全部配置文件+所有软件环境。不过，即便如此，仍然常常发生部署失败的状况</font>。Docker的出现<font color="red">使得Docker得以打破过去「程序即应用」的观念。透过镜像(images)将作业系统核心除外，运作应用程式所需要的系统环境，由下而上打包，达到应用程式跨平台间的无缝接轨运作</font>。

> docker理念

**Docker是基于Go语言实现的云开源项目**。

Docker的主要目标是“Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到“**一次镜像，处处运行**”。

![image-20221203194010893](07-docker.assets/image-20221203194010893.png)

<font color="gree">Linux容器技术的出现就解决了这样一个问题，而 Docker 就是在它的基础上发展过来的</font>。将应用打成镜像，通过镜像成为运行在Docker容器上面的实例，而 Docker 容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。<font color="red">只需要一次配置好环境，换到别的机子上就可以一键部署好，大大简化了操作</font>。

> 总结：
>
> 解决了**运行环境和配置问题**的**软件容器**，方便做持续集成并有助于整体发布的容器虚拟化技术。



#### 1.2 容器与虚拟机比较

> 传统虚拟机技术

虚拟机（virtual machine）就是带环境安装的一种解决方案。

它可以在一种操作系统里面运行另一种操作系统，比如在Windows10系统里面运行Linux系统CentOS7。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。  

| Win10 | VMWare | Centos7 | 各种cpu、内存网络额配置+各种软件 | 虚拟机实例 |
| ----- | ------ | ------- | -------------------------------- | ---------- |

传统虚拟机技术基于安装在主操作系统上的虚拟机管理系统(如: VirtualBox 和
VMWare等)，创建虚拟机(虚拟出各种硬件)，在虚拟机上安装从操作系统，在从操作系统中安装部署各种应用。

![image-20221203194945475](07-docker.assets/image-20221203194945475.png)

虚拟机的缺点：

1. 资源占用多
2. 冗余步骤多
3. 启动慢

> 容器虚拟化技术

由于前面虚拟机存在某些缺点，Linux发展出了另一种虚拟化技术：<font color="gree">Linux容器(Linux Containers，缩写为 LXC)</font>。

Linux容器是与系统其他部分隔离开的一系列进程，从另一个镜像运行，并由该镜像提供支持进程所需的全部文件。容器提供的镜像包含了应用的所有依赖项，因而在从开发到测试再到生产的整个过程中，它都具有可移植性和一致性。

**Linux 容器不是模拟一个完整的操作系统**而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。<font color="gree">容器与虚拟机不同，不需要捆绑一整套操作系统</font>，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

![image-20221203195326305](07-docker.assets/image-20221203195326305.png)

Docker容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统虚拟机则是在硬件层面实现虚拟化。与传统的虚拟机相比，Docker 优势体现为启动速度快、占用体积小。

> 对比

![image-20221203195605687](07-docker.assets/image-20221203195605687.png)

比较了 Docker 和传统虚拟化方式的不同之处：

* 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；
* 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

* 每个容器之间互相隔离，每个容器有自己的文件系统，容器之间进程不会相互影响，能区分计算资源。

![img](07-docker.assets/1652093324480-073374a2-4ce3-4a56-932b-5a73329736a2.jpeg)

对比：

| **特性**         | **容器**           | **虚拟机** |
| ---------------- | ------------------ | ---------- |
| **启动**         | 秒级               | 分钟级     |
| **大小**         | 一般为Mb           | 一般为Gb   |
| **速度**         | 接近原生           | 比较慢     |
| **系统支持数量** | 单机支持上千个容器 | 一般几十个 |



#### 1.3 docker能干嘛？

> 技术职级变化

- coder
- programmer
- software engineer
- DevOps engineer

> 开发/运维（DevOps）新一代开发工程师

一次构建，随处运行：

1. 更快速的应用交付和部署

   传统的应用开发完成后，需要提供一堆安装程序和配置说明文档，安装部署后需根据配置文档进行繁杂的配置才能正常运行。Docker化之后只需要交付少量容器镜像文件，在正式生产环境加载镜像并运行即可，应用安装配置在镜像里已经内置好，大大节省部署配置和测试验证时间。

2. 更便捷的升级和扩缩容

   随着微服务架构和Docker的发展，大量的应用会通过微服务方式架构，应用的开发构建将变成搭乐高积木一样，每个Docker容器将变成一块“积木”，应用的升级将变得非常容易。当现有的容器不足以支撑业务处理时，可通过镜像运行新的容器进行快速扩容，使应用系统的扩容从原先的天级变成分钟级甚至秒级。

3. 更简单的系统运维

   应用容器化运行后，生产环境运行的应用可与开发、测试环境的应用高度一致，容器会将应用程序相关的环境和状态完全封装起来，不会因为底层基础架构和操作系统的不一致性给应用带来影响，产生新的BUG。当出现程序异常时，也可以通过测试环境的相同容器进行快速定位和修复。

4. 更高效的计算资源利用

   Docker是*内核级虚拟化*，其不像传统的虚拟化技术一样需要额外的Hypervisor支持，所以在一台物理机上可以运行很多个容器实例，可大大提升物理服务器的CPU和内存的利用率。

> Docker应用场景

![image-20221203200316193](07-docker.assets/image-20221203200316193.png)



#### 1.4 下载地址

docker官网：http://www.docker.com

Docker Hub官网: https://hub.docker.com/



### 2 Docker安装

#### 2.1 前提说明

> CentOS Docker 安装

![image-20221203200809223](07-docker.assets/image-20221203200809223.png)

> 前提条件

目前，CentOS 仅发行版本中的内核支持 Docker。Docker 运行在CentOS 7 (64-bit)上，

要求系统为64位、Linux系统内核版本为 3.8以上，这里选用Centos7.x

> 查看自己的内核

uname命令用于打印当前系统相关信息（内核版本号、硬件架构、主机名称和操作系统类型等）。

![image-20221203201101183](07-docker.assets/image-20221203201101183.png)



#### 2.2 docker的基本组成

> docker三剑客
>
> 1. 镜像(image)
> 2. 容器(container)
> 3. 仓库(repository)

详细说明：

1. 镜像(image)

   Docker镜像（Image）就是一个**只读**的模板。镜像可以用来创建Docker容器，**一个镜像可以创建很多容器**。

   它也相当于是一个root文件系统。比如官方镜像centos:7就包含了完整的一套centos:7最小系统的root文件系统。

   相当于容器的“源代码”，**docker镜像文件类似于Java的类模板，而docker容器实例类似于java中new出来的实例对象**。

   | **Docker** | **面向对象** |
   | ---------- | ------------ |
   | 容器       | 对象         |
   | 镜像       | 类           |

2. 容器(container)

   - 从面向对象角度

     Docker 利用容器（Container）独立运行的一个或一组应用，应用程序或服务运行在容器里面，容器就类似于一个虚拟化的运行环境，**容器是用镜像创建的运行实例**。就像是Java中的类和实例对象一样，镜像是静态的定义，容器是镜像运行时的实体。容器为镜像提供了一个标准的和隔离的运行环境，它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台

   - 从镜像容器角度

     **可以把容器看做是一个简易版的 Linux 环境**（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。

3. 仓库(repository)

   仓库（Repository）是**集中存放镜像文件**的场所。

   类似于

   Maven仓库，存放各种jar包的地方；

   github仓库，存放各种git项目的地方；

   Docker公司提供的官方registry被称为Docker Hub，存放各种镜像模板的地方。

   仓库分为公开仓库（Public）和私有仓库（Private）两种形式。

   最大的公开仓库是Docker Hub(https://hub.docker.com/)，存放了数量庞大的镜像供用户下载。国内的公开仓库包括阿里云 、网易云等

> 小总结：

<font color="gree">需要正确的理解仓库/镜像/容器这几个概念</font>:

Docker本身是一个容器运行载体或称之为管理引擎。我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就是image镜像文件。只有通过这个镜像文件才能生成Docker容器实例(类似Java中new出来一个对象)。

image文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

- 镜像文件

  image 文件生成的容器实例，本身也是一个文件，称为镜像文件。

- 容器实例

  一个容器运行一种服务，当我们需要的时候，就可以通过docker客户端创建一个对应的运行实例，也就是我们的容器

- 仓库

  就是放一堆镜像的地方，我们可以把镜像发布到仓库中，需要的时候再从仓库中拉下来就可以了



#### 2.3 Docker平台架构图解

> Docker平台架构图解(入门版)

![architecture.svg](https://cdn.nlark.com/yuque/0/2022/svg/12911942/1652093339897-20255a0a-e981-43e3-9e9e-654b8da3b2c8.svg)

Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上，然后通过Socket连接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。<font color="gree">容器，是一个运行时环境，就是我们前面说到的集装箱。可以对比mysql演示对比讲解</font>。

![image-20221203202656631](07-docker.assets/image-20221203202656631.png)

> Docker平台架构图解(架构版)

首次懵逼正常，后续深入，先有大概轮廓，混个眼熟

整体架构及底层通信原理简述：

Docker 是一个 C/S 模式的架构，后端是一个松耦合架构，众多模块各司其职。

Docker运行的基本流程为：

1. 用户是使用 Docker Client 与 Docker Daemon 建立通信，并发送请求给后者
2. Docker Daemon 作为 Docker 架构的主体部分，首先提供 Docker Server 的功能使其可以接收 Docker Client 的请求
3. Docker Engine 执行 Docker 内部的一系列工作，每一项工作都是以一个 Job 的形式存在
4. Job 的运行过程中，当需要容器镜像时，则从 Docker Registry 中下载镜像，并通过镜像管理驱动 Graph Driver 将下载镜像以 Graph 的形式存储
5. 当需要为 Docker 创建网络环境时，通过网络管理驱动 Network driver 创建并配置 Docker 容器网络环境
6. 当需要限制 Docker 容器运行资源或执行用户指令等操作时，则通过 Exec driver 来完成
7. Libcontainer 是一项独立的容器管理包，Network driver 以及 Exec driver 都是通过 Libcontainer 来实现具体对容器进行的操作

![img](07-docker.assets/1652093347909-4fcf65d1-da12-47cb-9a2f-0c4528d7e4c9.png)



#### 2.4 docker安装步骤

> CentOS7安装Docker
>
> https://docs.docker.com/engine/install/centos/

1. 确定你是CentOS7及以上版本

2. 卸载旧版本

   ```shell
   yum remove docker \
              docker-client \
              docker-client-latest \
              docker-common \
              docker-latest \
              docker-latest-logrotate \
              docker-logrotate \
              docker-engine
   ```

   旧版本的Docker引擎包可能叫做：`docker`、`docker-engine`。

   新版本的Docker引擎包叫做：`docker-ce`

   ![image-20221203204008457](07-docker.assets/image-20221203204008457.png)

3. yum安装gcc相关

   要求CentOS7能上外网

   ```bash
   yum -y install gcc
   
   yum -y install gcc-c++
   ```

   以上两行代码依次执行。

4. 安装需要的软件包

   ![image-20221203204333492](07-docker.assets/image-20221203204333492.png)

   ```bash
   yum install -y yum-utils
   ```

5. 设置stable镜像仓库

   - 大坑

     ```bash
     yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     ```

     连接的是外网服务器仓库，往往会出现连接慢或连接超时的问题。

   - 推荐

     推荐连接的是国内的阿里云服务器仓库。

     ```bash
     yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
     ```

     ![image-20221203204954167](07-docker.assets/image-20221203204954167.png)

6. 更新yum软件包索引

   ```bash
   yum makecache fast
   ```

7. 安装 DOCKER CE

   ```bash
   yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```

8. 启动docker

   ```bash
   systemctl start docker
   ```

9. 测试

   ```bash
   [root@linux-docker ~]# docker version
   Client: Docker Engine - Community
    Version:           20.10.21
    API version:       1.41
    Go version:        go1.18.7
    Git commit:        baeda1f
    Built:             Tue Oct 25 18:04:24 2022
    OS/Arch:           linux/amd64
    Context:           default
    Experimental:      true
   
   Server: Docker Engine - Community
    Engine:
     Version:          20.10.21
     API version:      1.41 (minimum version 1.12)
     Go version:       go1.18.7
     Git commit:       3056208
     Built:            Tue Oct 25 18:02:38 2022
     OS/Arch:          linux/amd64
     Experimental:     false
    containerd:
     Version:          1.6.10
     GitCommit:        770bd0108c32f3fb5c73ae1264f7e503fe7b2661
    runc:
     Version:          1.1.4
     GitCommit:        v1.1.4-0-g5fd4c4d
    docker-init:
     Version:          0.19.0
     GitCommit:        de40ad0
   ```

   ```bash
   [root@linux-docker ~]# docker run hello-world
   
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   
   To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
       (amd64)
    3. The Docker daemon created a new container from that image which runs the
       executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
       to your terminal.
   
   To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash
   
   Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/
   
   For more examples and ideas, visit:
    https://docs.docker.com/get-started/
   ```

10. 卸载

    ```bash
    systemctl stop docker 
    
    yum remove docker-ce docker-ce-cli containerd.io
    
    rm -rf /var/lib/docker
    
    rm -rf /var/lib/containerd
    ```



#### 2.5 阿里云镜像加速

1. 阿里云官网：https://promotion.aliyun.com/ntms/act/kubernetes.html

2. 注册一个属于自己的阿里云账户(可复用淘宝账号)

3. 获得加速器地址连接

   - 登陆阿里云开发者平台

     ![image-20221203205746912](07-docker.assets/image-20221203205746912.png)

   - 点击控制台

     ![image-20221203205804407](07-docker.assets/image-20221203205804407.png)

   - 选择容器镜像服务

     ![image-20221203205824040](07-docker.assets/image-20221203205824040.png)

   - 获取加速器地址

     ![image-20221203205845440](07-docker.assets/image-20221203205845440.png)

4. 粘贴脚本直接执行

   ![image-20221203205939509](07-docker.assets/image-20221203205939509.png)

5. 重启服务器

   ```bash
   systemctl daemon-reload
   
   systemctl restart docker
   ```



#### 2.6 永远的HelloWorld

> 启动Docker后台容器(测试运行 hello-world)

![image-20221203210112752](07-docker.assets/image-20221203210112752.png)

输出这段提示以后，hello world就会停止运行，容器自动终止。

> run干了什么

![image-20221203210203319](07-docker.assets/image-20221203210203319.png)



#### 2.7 底层原理

> 为什么Docker会比VM虚拟机快？

1. docker有着比虚拟机更少的抽象层

   由于docker不需要Hypervisor(虚拟机)实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。

2. docker利用的是宿主机的内核，而不需要加载操作系统OS内核

   当新建一个容器时，docker不需要和虚拟机一样重新加载一个操作系统内核。进而避免引寻、加载操作系统内核返回等比较费时费资源的过程，当新建一个虚拟机时，虚拟机软件需要加载OS，返回新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统，则省略了返回过程，因此新建一个docker容器只需要几秒钟。

![img](07-docker.assets/1652093324480-073374a2-4ce3-4a56-932b-5a73329736a2.jpeg)

|                | **Docker容器**          | **虚拟机(VM)**              |
| -------------- | ----------------------- | --------------------------- |
| **操作系统**   | 与宿主机共享OS          | 宿主机OS上运行虚拟机OS      |
| **存储大小**   | 镜像小，便于存储和运输  | 镜像庞大（vmdk、vdi等）     |
| **运行性能**   | 几乎无额外性能损失      | 操作系统额外的CPU、内存消耗 |
| **移植性**     | 轻便、灵活，适应于Linux | 笨重，与虚拟化技术耦合度高  |
| **硬件亲和性** | 面向软件开发者          | 面向硬件运维者              |
| **部署速度**   | 快速、秒级              | 较慢，10s以上               |



### 3 Docker常用命令

#### 3.1 帮助启动类命令

- 启动docker：systemctl start docker
- 停止docker：systemctl stop docker
- 重启docker：systemctl restart docker
- 查看docker状态：systemctl status docker
- 开机启动：systemctl enable docker
- 查看docker概要信息：docker info
- 查看docker总体帮助文档：docker --help
- 查看docker命令帮助文档：docker 具体命令 --help



#### 3.2 镜像命令

> docker images

- 列出本地主机上的镜像

- options说明：

  -a：列出本地所有的镜像（含历史映像层）

  -q：只显示镜像ID。

- 例：docker images -a -q (显示所有的镜像ID)

> docker search 某个XXX镜像名字

- 网站：https://hub.docker.com

- 命令：docker search [OPTIONS] 镜像名字

- options说明：

  --limit：只列出N个镜像，默认25个

- 例：docker search --limit 5 redis

  ![image-20221204171601527](07-docker.assets/image-20221204171601527.png)

  | **参数**    | **说明**         |
  | ----------- | ---------------- |
  | NAME        | 镜像名称         |
  | DESCRIPTION | 镜像说明         |
  | STARS       | 点赞数量         |
  | OFFICIAL    | 是否是官方认证的 |
  | AUTOMATED   | 是否是自动构建的 |

> docker pull 某个XXX镜像名字

- 下载镜像

- docker pull 镜像名字[:TAG]

- docker pull 镜像名字

  - 没有TAG就是最新版，等价于docker pull 镜像名字:latest

- 例：docker pull ubuntu

  ![image-20221204172028724](07-docker.assets/image-20221204172028724.png)

> docker system df (查看镜像/容器/数据卷所占的空间)

![image-20221204172244485](07-docker.assets/image-20221204172244485.png)

> docker rmi 某个XXX镜像名字ID

- 删除镜像
- 删除单个：docker rmi -f 镜像ID
- 删除多个：docker rmi -f 镜像名1:TAG 镜像名2:TAG
- 删除全部：docker rmi -f $(docker images -qa)

> 面试题：谈谈docker虚悬镜像是什么？

- 是什么：仓库名、标签都是\<none\>的镜像，俗称虚悬镜像dangling image

  ![image-20221204172631983](07-docker.assets/image-20221204172631983.png)

- 后续Dockerfile章节再介绍



#### 3.3 容器命令

> 前提说明

有镜像才能创建容器，这是根本前提(下载一个CentOS或者ubuntu镜像演示)

![image-20221204172843668](07-docker.assets/image-20221204172843668.png)

- docker pull centos

- docker pull ubuntu

  ![image-20221204172933864](07-docker.assets/image-20221204172933864.png)

- 本次演示用ubuntu演示

> 新建+启动容器

- docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

- OPTIONS说明（常用）：有些是一个减号，有些是两个减号

  - --name="容器新名字"：为容器指定一个名称；

  - -d：后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；

  - -i：以交互模式运行容器，通常与 -t 同时使用；

  - -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；也即启动交互式容器(前台有伪终端，等待交互)；

  - -P: 随机端口映射，大写P

  - -p: 指定端口映射，小写p

    ![image-20221204173448578](07-docker.assets/image-20221204173448578.png)

- 启动交互式容器(前台命令行)

  docker run -it centos /bin/bash

  ![image-20221204173614661](07-docker.assets/image-20221204173614661.png)

> 列出当前所有正在运行的容器

- docker ps [OPTIONS]
- OPTIONS说明（常用）：
  1. -a：列出当前所有正在运行的容器+历史上运行过的
  2. -l：显示最近创建的容器。
  3. -n：显示最近n个创建的容器。
  4. -q：静默模式，只显示容器编号。

> 退出容器

两种退出方式

1. exit：run进去容器，exit退出，容器停止
2. ctrl+p+q：run进去容器，ctrl+p+q退出，容器不停止

> 启动已停止运行的容器

- docker start 容器ID或者容器名

> 重启容器

- docker restart 容器ID或者容器名

> 停止容器、强制停止容器

- 停止容器：docker stop 容器ID或者容器名
- 强制停止容器：docker kill 容器ID或容器名

> 删除已停止的容器

- docker rm 容器ID
- 一次性删除多个容器实例
  1. docker rm -f $(docker ps -a -q)
  2. docker ps -a -q | xargs docker rm



> 下面是守护式容器(后台服务器管理)
>
> 下载一个Redis6.0.8镜像演示

> 启动守护式容器(后台服务器)

- 在大部分的场景下，我们希望 docker 的服务是在后台运行的，我们可以过 -d 指定容器的后台运行模式。

- docker run -d 容器名

  ```bash
  #使用镜像 centos:latest 以后台模式启动一个容器
  docker run -d centos
  ```

  问题：然后docker ps -a 进行查看，**会发现容器已经退出**。

  很重要的要说明的一点：*Docker容器后台运行，就必须有一个前台进程*。

  容器运行的命令如果不是那些*一直挂起的命令*（比如运行top，tail），就是会自动退出的。

  这个是docker的机制问题，比如你的web容器，我们以nginx为例，正常情况下，我们配置启动服务只需要启动响应的service即可。例如service nginx start

  但是，这样做，nginx为后台进程模式运行，就导致docker前台没有运行的应用，这样的容器后台启动后，会立即自杀因为他觉得他没事可做了。

  所以，最佳的解决方案是，将你要运行的程序以前台进程的形式运行，常见就是命令行模式，表示我还有交互操作，别中断，O(∩_∩)O哈哈~

- redis 前后台启动演示case

  1. 前台交互式启动：docker run -it redis:6.0.8

     ![image-20221204175731219](07-docker.assets/image-20221204175731219.png)

  2. 后台守护式启动：docker run -d redis:6.0.8

     ![image-20221204175511613](07-docker.assets/image-20221204175511613.png)

> 查看容器日志

- docker logs 容器ID

  ![image-20221204180516595](07-docker.assets/image-20221204180516595.png)

> 查看容器内运行的进程

- docker top 容器ID

  ![image-20221204180411705](07-docker.assets/image-20221204180411705.png)

> 查看容器内部细节

- docker inspect 容器ID

> 进入正在运行的容器并以命令行交互

- docker exec -it 容器ID bashShell

  ![image-20221204180748278](07-docker.assets/image-20221204180748278.png)

  ![image-20221204180754358](07-docker.assets/image-20221204180754358.png)

- 重新进入：docker attach 容器ID

- 上述两个区别

  1. attach 直接进入容器启动命令的终端，不会启动新的进程，用exit退出，会导致容器的停止。
  2. exec 是在容器中打开新的终端，并且可以启动新的进程，用exit退出，不会导致容器的停止。

- 推荐大家使用 docker exec 命令，因为退出容器终端，不会导致容器的停止。

- 用之前的redis容器实例进入试试

  进入redis服务

  1. docker exec -it 容器ID /bin/bash
  2. docker exec -it 容器ID redis-cli
  3. 一般用-d后台启动的程序，再用exec进入对应容器实例

> 从容器内拷贝文件到主机上

- 容器 → 主机

- docker cp 容器ID:容器内路径 目的主机路径

  ![image-20221204181440561](07-docker.assets/image-20221204181440561.png)

> 导入和导出容器

- export 导出容器的内容留作为一个tar归档文件[对应import命令]

- import 从tar包中的内容创建一个新的文件系统再导入为镜像[对应export]

- 案例

  1. docker export 容器ID > 文件名.tar

     ![image-20221204182153737](07-docker.assets/image-20221204182153737.png)

  2. cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号

     ![image-20221204182605288](07-docker.assets/image-20221204182605288.png)

#### 3.4 总结

![Docker-Command-Diagram.png](07-docker.assets/1652093386636-41aec8f5-bd52-4a99-8c3c-daa61c1d78a5.png)

- attach	Attach to a running container	# 当前 shell 下 attach 连接指定运行镜像
- build    Build an image from a Dockerfile    # 通过 Dockerfile 定制镜像
- commit    Create a new image from a container changes    # 提交当前容器为新的镜像
- cp    Copy files/folders from the containers filesystem to the host path    #从容器中拷贝指定文件或者目录到宿主机中
- create    Create a new container    # 创建一个新的容器，同 run，但不启动容器
- diff    Inspect changes on a container's filesystem    # 查看 docker 容器变化
- events    Get real time events from the server    # 从 docker 服务获取容器实时事件
- exec    Run a command in an existing container    # 在已存在的容器上运行命令
- export    Stream the contents of a container as a tar archive    # 导出容器的内容流作为一个 tar 归档文件[对应 import]
- history    Show the history of an image    # 展示一个镜像形成历史
- images    List images    # 列出系统当前镜像
- import    Create a new filesystem image from the contents of a tarball    # 从tar包中的内容创建一个新的文件系统映像[对应export]
- info    Display system-wide information    # 显示系统相关信息
- inspect    Return low-level information on a container    # 查看容器详细信息
- kill    Kill a running container    # kill 指定 docker 容器
- load    Load an image from a tar archive    # 从一个 tar 包中加载一个镜像[对应 save]
- login    Register or Login to the docker registry server    # 注册或者登陆一个 docker 源服务器
- logout    Log out from a Docker registry server    # 从当前 Docker registry 退出
- logs    Fetch the logs of a container    # 输出当前容器日志信息
- port    Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # 查看映射端口对应的容器内部源端口
- pause    Pause all processes within a container    # 暂停容器
- ps    List containers    # 列出容器列表
- pull    Pull an image or a repository from the docker registry server    # 从docker镜像源服务器拉取指定镜像或者库镜像
- push    Push an image or a repository to the docker registry server    # 推送指定镜像或者库镜像至docker源服务器
- restart    Restart a running container    # 重启运行的容器
- rm    Remove one or more containers    # 移除一个或者多个容器
- rmi    Remove one or more images    # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]
- run    Run a command in a new container    # 创建一个新的容器并运行一个命令
- save    Save an image to a tar archive    # 保存一个镜像为一个 tar 包[对应 load]
- search    Search for an image on the Docker Hub    # 在 docker hub 中搜索镜像
- start    Start a stopped containers    # 启动容器
- stop    Stop a running containers    # 停止容器
- tag    Tag an image into a repository    # 给源中镜像打标签
- top    Lookup the running processes of a container    # 查看容器中运行的进程信息
- unpause    Unpause a paused container    # 取消暂停容器
- version    Show the docker version information    # 查看 docker 版本号
- wait    Block until a container stops, then print its exit code    # 截取容器停止时的退出状态值



### 4 Docker镜像

#### 4.1 Docker镜像原理

> 是什么？

*镜像*

是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境(包括代码、运行时需要的库、环境变量和配置文件等)，这个打包好的运行环境就是image镜像文件。

只有通过这个镜像文件才能生成Docker容器实例(类似Java中new出来一个对象)。

> 分层的镜像

以我们的pull为例，在下载的过程中我们可以看到docker的镜像好像是在一层一层的在下载

![image-20221205191434256](07-docker.assets/image-20221205191434256.png)

> UnionFS（联合文件系统）

*UnionFS（联合文件系统）*：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

*特性*：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

> Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是引导文件系统bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs(root file system)，在bootfs之上。包含的就是典型 Linux 系统中的/dev、/proc、/bin、/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。 

![layer01.jpg](07-docker.assets/1652093445038-27095471-f01f-4978-a28f-d8e2df893dab.jpeg)

**平时我们安装进虚拟机的CentOS都是好几个G，为什么docker这里才200M？？**

![image-20221205191900199](07-docker.assets/image-20221205191900199.png)

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供 rootfs 就行了。由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

> 为什么 Docker 镜像要采用这种分层结构呢

镜像分层最大的一个好处就是共享资源，方便复制迁移，就是为了复用。

比如说有多个镜像都从相同的 base 镜像构建而来，那么 Docker Host 只需在磁盘上保存一份 base 镜像；

同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。



#### 4.2 重点理解

*Docker镜像层都是只读的，容器层是可写的*。

当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

所有对容器的改动————无论添加、删除、还是修改文件都只会发生在容器层中。只有容器层是可写的，容器层下面的所有镜像层都是只读的。

![layer04.jpg](07-docker.assets/1652093466220-a1c70673-7ca7-4025-9432-d89152439000.jpeg)



#### 4.3 Docker镜像commit操作案例

- `docker commit`：提交容器副本使之成为一个新的镜像
- `docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]`

> 案例演示ubuntu安装vim

1. 从Hub上下载ubuntu镜像到本地并成功运行

2. 原始的默认Ubuntu镜像是不带着vim命令的

   ![image-20221205202429481](07-docker.assets/image-20221205202429481.png)

3. 外网连通的情况下，安装vim

   docker容器内执行下面两条命令：

   - `apt-get update`：先更新我们的包管理工具
   - `apt-get -y install vim`：然后安装我们需要的vim

   ![image-20221205202719952](07-docker.assets/image-20221205202719952.png)

   ![image-20221205202834762](07-docker.assets/image-20221205202834762.png)

4. 安装完成后，commit我们自己的新镜像

   - `docker commit -m="add vim bash" -a="xk" 80c78608cc60 xk/myubuntu:1.1`

   ![image-20221205203337814](07-docker.assets/image-20221205203337814.png)

5. 启动我们的新镜像并和原来的对比

   ![image-20221205203632831](07-docker.assets/image-20221205203632831.png)



#### 4.4 总结

Docker中的镜像分层，**支持通过扩展现有镜像，创建新的镜像**。类似Java继承于一个Base基础类，自己再按需扩展。

新镜像是从 base 镜像一层一层叠加生成的。每安装一个软件，就在现有镜像的基础上增加一层

![layer03.png](07-docker.assets/1652093459380-335e0f0e-1213-47d9-a17b-f49734699ef2.png)



### 5 本地镜像发布到阿里云

#### 5.1 本地镜像发布到阿里云流程

![image-20221205210411668](07-docker.assets/image-20221205210411668.png)



#### 5.2 将本地镜像推送到阿里云

1. 本地镜像素材原型

   ![image-20221205210517186](07-docker.assets/image-20221205210517186.png)

2. 阿里云开发者平台：https://promotion.aliyun.com/ntms/act/kubernetes.html

3. 选择控制台，进入容器镜像服务

   ![image-20221205210600831](07-docker.assets/image-20221205210600831.png)

4. 选择个人实例

   ![image-20221205210702159](07-docker.assets/image-20221205210702159.png)

5. 命名空间

   ![image-20221205210718299](07-docker.assets/image-20221205210718299.png)

   ![image-20221205210726891](07-docker.assets/image-20221205210726891.png)

6. 仓库名称

   ![image-20221205210830349](07-docker.assets/image-20221205210830349.png)

   ![image-20221205210846926](07-docker.assets/image-20221205210846926.png)

   ![image-20221205210857532](07-docker.assets/image-20221205210857532.png)

7. 进入管理界面获得脚本

   ![image-20221205210913396](07-docker.assets/image-20221205210913396.png)

8. 将镜像推送到阿里云registry

   管理界面脚本

   ![image-20221205211143538](07-docker.assets/image-20221205211143538.png)

   脚本实例

   ```bash
   docker login --username=山海kang registry.cn-hangzhou.aliyuncs.com
   docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/shanhai-xk/myubuntu1.1:[镜像版本号]
   docker push registry.cn-hangzhou.aliyuncs.com/shanhai-xk/myubuntu1.1:[镜像版本号]
   ```

   ![image-20221205211954061](07-docker.assets/image-20221205211954061.png)



#### 5.3 将阿里云上的镜像下载到本地

- `docker pull registry.cn-hangzhou.aliyuncs.com/shanhai-xk/myubuntu1.1:[镜像版本号]`

![image-20221205212437184](07-docker.assets/image-20221205212437184.png)



### 6 本地镜像发布到私有库

#### 6.1 本地镜像发布到私有库流程

![image-20221205214406815](07-docker.assets/image-20221205214406815.png)



#### 6.2 私有库是什么

1. 官方Docker Hub地址：https://hub.docker.com/，中国大陆访问太慢了且准备被阿里云取代的趋势，不太主流。
2. Dockerhub、阿里云这样的公共镜像仓库可能不太方便，涉及机密的公司不可能提供镜像给公网，所以需要创建一个本地私人仓库供给团队使用，基于公司内部项目构建镜像。
3. Docker Registry是官方提供的工具，可以用于构建私有镜像仓库



#### 6.3 将本地镜像推送到私有库

1. 下载镜像Docker Registry

   ![image-20221205214905806](07-docker.assets/image-20221205214905806.png)

2. 运行私有库Registry，相当于本地有个私有Docker hub

   docker run -d -p 5000:5000 -v /shanhai/myregistry/:/tmp/registry --privileged=true registry

   默认情况，仓库被创建在容器的/var/lib/registry目录下，建议自行用容器卷映射，方便于宿主机联调

   ![image-20221205215316799](07-docker.assets/image-20221205215316799.png)

3. 案例演示创建一个新镜像，ubuntu安装ifconfig命令

   - 从Hub上下载ubuntu镜像到本地并成功运行

   - 原始的Ubuntu镜像是不带着ifconfig命令的

     ![image-20221205215841045](07-docker.assets/image-20221205215841045.png)

   - 外网连通的情况下，安装ifconfig命令并测试通过

     `apt-get update`

     `apt-get install net-tools`

     ![image-20221205220114635](07-docker.assets/image-20221205220114635.png)

   - 安装完成后，commit我们自己的新镜像

     命令：在容器外执行，记得

     `docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]`

     `docker commit -m="ifconfig cmd add" -a="zzyy" a69d7c825c4f zzyyubuntu:1.2`

     ![image-20221205220430566](07-docker.assets/image-20221205220430566.png)

   - 启动我们的新镜像并和原来的对比

     官网是默认下载的Ubuntu没有ifconfig命令

     我们自己commit构建的新镜像，新增加了ifconfig功能，可以成功使用。

4. curl验证私服库上有什么镜像

   `curl -XGET http://192.168.88.110:5000/v2/_catalog`

   可以看到，目前私服库没有任何镜像上传过。。。。。。

   ![image-20221205220919775](07-docker.assets/image-20221205220919775.png)

5. 将新镜像xxkkubuntu:1.2修改符合私服规范的Tag

   按照公式：`docker tag 镜像:Tag Host:Port/Repository:Tag`

   **自己host主机IP地址，填写同学你们自己的，不要粘贴错误**，O(∩_∩)O

   使用命令 docker tag 将 xxkkubuntu:1.2 这个镜像修改为 192.168.88.110:5000/xxkkubuntu:1.2

   `docker tag xxkkubuntu:1.2  192.168.88.110:5000/xxkkubuntu:1.2`

   ![image-20221205221310090](07-docker.assets/image-20221205221310090.png)

6. 修改配置文件使之支持http

   ![image-20221205221502714](07-docker.assets/image-20221205221502714.png)

   > 别无脑照着复制，registry-mirrors 配置的是国内阿里提供的镜像加速地址，不用加速的话访问官网的会很慢。
   >
   > *2个配置中间有个逗号','别漏了*，这个配置是json格式的。

   `vim /etc/docker/daemon.json`

   添加：`"insecure-registries": ["192.168.88.110:5000"]`  这一行信息

   ![image-20221205221830677](07-docker.assets/image-20221205221830677.png)

   上述理由：docker默认不允许http方式推送镜像，通过配置选项来取消这个限制。====> *修改完后如果不生效，建议重启docker*。

7. push推送到私服库

   `docker push 192.168.88.110:5000/xxkkubuntu:1.2`

   ![image-20221205222316706](07-docker.assets/image-20221205222316706.png)

8. curl验证私服库上有什么镜像

   `curl -XGET http://192.168.88.110:5000/v2/_catalog`

   ![image-20221205222436142](07-docker.assets/image-20221205222436142.png)

9. pull到本地并运行

   `docker pull 192.168.88.110:5000/xxkkubuntu:1.2`

   ![image-20221205222802483](07-docker.assets/image-20221205222802483.png)

   `docker run -it 镜像ID /bin/bash`

   ![image-20221205222926039](07-docker.assets/image-20221205222926039.png)



### 7 Docker容器数据卷

#### 7.1 前提注意事项

*坑：开启容器时容器卷记得加入*：--privileged=true

> why

Docker挂载主机目录访问**如果出现cannot open directory .: Permission denied**。

解决办法：在挂载目录后多加一个--privileged=true参数即可

如果是CentOS7，安全模块会比之前系统版本加强，不安全的会先禁止，所以目录挂载的情况被默认为不安全的行为，
在SELinux里面挂载目录被禁止掉了额，如果要开启，我们一般使用`--privileged=true`命令，扩大容器的权限解决挂载目录没有权限的问题，也即使用该参数，container内的root拥有真正的root权限，否则，container内的root只是外部的一个普通用户权限。



回忆：

docker run -d -p 5000:5000 `-v /shanhai/myregistry/:/tmp/registry --privileged=true` registry



#### 7.2 容器数据卷简介

> 是什么？

- 一句话：有点类似我们Redis里面的rdb和aof文件

- 将docker容器内的数据保存进宿主机的磁盘中

- 运行一个带有容器卷存储功能的容器实例

  ` docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录      镜像名`

> 能干嘛？

将运用与运行的环境打包镜像，run后形成容器实例运行，但是我们对数据的要求希望是*持久化*的。

Docker容器产生的数据，如果不备份，那么当容器实例删除后，容器内的数据自然也就没有了。

为了能保存数据在docker中我们使用卷。

特点：

1. 数据卷可在容器之间共享或重用数据
2. 卷中的更改可以直接实时生效，爽
3. 数据卷中的更改不会包含在镜像的更新中
4. 数据卷的生命周期一直持续到没有容器使用它为止



#### 7.3 数据卷案例

##### 7.3.1 宿主vs容器之间映射添加容器卷

> 直接命令添加：
>
> `docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录 镜像名`

`docker run -it --privileged=true --name xk1 -v /tmp/hostdata:/tmp/dockerdata ubuntu`

![image-20221205231418848](07-docker.assets/image-20221205231418848.png)

![image-20221205231420328](07-docker.assets/image-20221205231420328.png)

> 查看数据卷是否挂载成功
>
> `docker inspect 容器ID`

![image-20221205231608131](07-docker.assets/image-20221205231608131.png)

> 容器和宿主机之间数据共享

1. docker修改，主机同步获得
2. 主机修改，docker同步获得
3. docker容器stop，主机修改，docker容器重启后同步主机数据。



##### 7.3.2 读写规则映射添加说明

> 读写(默认)：
>
> `docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:rw 镜像名`

默认同上案例，默认就是rw

> 只读
>
> `docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:ro 镜像名`

容器实例内部被限制，只能读取不能写

 /容器目录:ro 镜像名  (就能完成功能，此时容器自己只能读取不能写)

此时如果宿主机写入内容，可以同步给容器内，容器可以读取到。

![image-20221205232531612](07-docker.assets/image-20221205232531612.png)



##### 7.3.3 卷的继承和共享

> 容器1完成和宿主机的映射

`docker run -it --privileged=true -v /xkdata/u:/temps --name sh-xk1 ubuntu`

![image-20221205234018374](07-docker.assets/image-20221205234018374.png)

![image-20221205234020601](07-docker.assets/image-20221205234020601.png)

> 容器2继承容器1的卷规则
>
> `docker run -it --privileged=true --volumes-from 父类 --name u2 ubuntu`

`docker run -it --privileged=true --volumes-from sh-xk1 --name sh-xk2 ubuntu`

![image-20221205234358438](07-docker.assets/image-20221205234358438.png)



### 8 Docker常规安装简介

总体步骤：

1. 搜索镜像
2. 拉取镜像
3. 查看镜像
4. 启动镜像---服务端口映射
5. 停止容器
6. 移除容器



#### 8.1 安装tomcat

1. docker hub上面查找tomcat镜像

   `docker search tomcat --limit 5`

   ![image-20221206000138402](07-docker.assets/image-20221206000138402.png)

2. 从docker hub上拉取tomcat镜像到本地

   `docker pull tomcat`

   ![image-20221206000347142](07-docker.assets/image-20221206000347142.png)

3. docker images查看是否有拉取到的tomcat

   `docker images tomcat`

4. 使用tomcat镜像创建容器实例(也叫运行镜像)

   `docker run -d -p 8080:8080 tomcat`

   - -p 小写，主机端口:docker容器端口

   - -P 大写，随机分配端口

     ![image-20221206000524086](07-docker.assets/image-20221206000524086.png)

   - i：交互

   - t：终端

   - d：后台

5. 访问猫首页

   虚拟机中浏览器访问：http://localhost:8080/

   > 问题：

   ![image-20221206001041631](07-docker.assets/image-20221206001041631.png)

   > 解决：

   - 可能没有映射端口或者没有关闭防火墙

   - 把webapps.dist目录换成webapps

     先成功启动tomcat

     ![image-20221206001416325](07-docker.assets/image-20221206001416325.png)

     查看 webapps 文件夹查看为空

     若 webapps 文件夹中为空，则删除 webapps 文件夹，将 webapps.dist 文件夹重命名为 webapps 文件夹名

     之后再次访问猫首页：http://localhost:8080/

     ![image-20221206002053248](07-docker.assets/image-20221206002053248.png)

     ![image-20221206002114778](07-docker.assets/image-20221206002114778.png)

6. 免修改版说明

   `docker pull billygoo/tomcat8-jdk8`

   `docker run -d -p 8080:8080 --name mytomcat8 billygoo/tomcat8-jdk8`



#### 8.2 安装mysql

##### 8.2.1 mysql5.7 安装

1. docker hub上面查找mysql镜像

   ![image-20221206004807837](07-docker.assets/image-20221206004807837.png)

2. 从docker hub上(阿里云加速器)拉取mysql镜像到本地标签为5.7

   ![image-20221206005113149](07-docker.assets/image-20221206005113149.png)

##### 8.2.2 实例

使用 mysql5.7 镜像创建容器(也叫运行镜像)

命令出处，哪里来的？

`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag`

![](07-docker.assets/image-20221206005236357.png)

> 简单版：

1. 使用mysql镜像

   `docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7`

   `docker ps`

   `docker exec -it 容器ID /bin/bash`

   `mysql -uroot -p`

   ![image-20221206005921328](07-docker.assets/image-20221206005921328.png)

2. 建库建表插入数据

   ![image-20221206010248867](07-docker.assets/image-20221206010248867.png)

3. 外部Win10也来连接运行在dokcer上的mysql容器实例服务

   ![image-20221206010447453](07-docker.assets/image-20221206010447453.png)

4. 问题

   - 插入中文数据试试

     ![image-20221206010730872](07-docker.assets/image-20221206010730872.png)

     为什么报错？

     docker上默认字符集编码隐患

     docker里面的mysql容器实例查看，内容如下：`SHOW VARIABLES LIKE 'character%'`。

     ![image-20221206010853753](07-docker.assets/image-20221206010853753.png)

   - 删除容器后，里面的mysql数据如何办，会永远丢失。

> 实战版

1. 新建mysql容器实例

   ```
   docker run -d -p 3306:3306 --privileged=true -v /zzyyuse/mysql/log:/var/log/mysql -v /zzyyuse/mysql/data:/var/lib/mysql -v /zzyyuse/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456  --name mysql mysql:5.7
   
   docker run -d -p 3306:3306 --privileged=true 
   -v /zzyyuse/mysql/log:/var/log/mysql 
   -v /zzyyuse/mysql/data:/var/lib/mysql 
   -v /zzyyuse/mysql/conf:/etc/mysql/conf.d 
   -e MYSQL_ROOT_PASSWORD=123456  
   --name mysql mysql:5.7
   ```

2. 在宿主机的 /zzyyuse/mysql/conf 目录下新建 my.cnf

   通过容器卷同步给mysql容器实例

   my.cnf 里面输入：

   ```properties
   [client]
   default_character_set=utf8
   [mysqld]
   collation_server = utf8_general_ci
   character_set_server = utf8
   ```

   ![image-20221206011638205](07-docker.assets/image-20221206011638205.png)

3. 重新启动mysql容器实例再重新进入并查看字符编码

   ![image-20221206012038111](07-docker.assets/image-20221206012038111.png)

4. 再新建库新建表再插入中文测试

   ![image-20221206012433925](07-docker.assets/image-20221206012433925.png)

   ![image-20221206012632795](07-docker.assets/image-20221206012632795.png)

> 总结

之前的DB：无效

修改字符集操作+重启mysql容器实例

之后的DB：有效，需要新建

结论：*docker安装完MySQL并run出容器后，建议请先修改完字符集编码后再新建mysql库-表-插数据*。

> 假如将当前容器实例删除，再重新来一次，之前建的db01实例还有吗？

有的。



#### 8.3 安装redis

1. 从docker hub上(阿里云加速器)拉取redis镜像到本地标签为6.0.8

   `docker pull redis:6.0.8`

   ![image-20221206014455409](07-docker.assets/image-20221206014455409.png)

2. 入门命令

   `docker run -d -p 6379:6379 redis:6.0.8`

   ![image-20221206014834780](07-docker.assets/image-20221206014834780.png)

3. 命令提醒：容器卷记得加入`--privileged=true`

   Docker挂载主机目录Docker访问出现cannot open directory .: Permission denied

   解决办法：在挂载目录后多加一个`--privileged=true`参数即可

4. 在CentOS宿主机下新建目录/app/redis

   `mkdir -p /app/redis`

   ![image-20221206015111382](07-docker.assets/image-20221206015111382.png)

5. 将一个redis.conf文件模板拷贝进/app/redis目录下

   /app/redis目录下修改redis.conf文件

   - 开启redis验证 [可选]

     requirepass 123

   - 允许redis外地连接，必须注释掉 # bind 127.0.0.1

     ![image-20221206015706799](07-docker.assets/image-20221206015706799.png)

   - daemonize no

     将 `daemonize yes` 注释起来或者 `daemonize no` 设置，因为该配置和`docker run中-d`参数冲突，会导致容器一直启动失败。

     ![image-20221206015831411](07-docker.assets/image-20221206015831411.png)

   - 开启redis数据持久化：`appendonly yes` [可选]

6. 使用redis6.0.8镜像创建容器(也叫运行镜像)

   ```
   docker run  -p 6379:6379 --name myr3 --privileged=true -v /app/redis/redis.conf:/etc/redis/redis.conf -v /app/redis/data:/data -d redis:6.0.8 redis-server /etc/redis/redis.conf
   
   
   docker run -p 6379:6379 --name myr3 --privileged=true 
   -v /app/redis/redis.conf:/etc/redis/redis.conf 
   -v /app/redis/data:/data 
   -d redis:6.0.8 redis-server /etc/redis/redis.conf
   ```

   ![image-20221206020401801](07-docker.assets/image-20221206020401801.png)

7. 测试redis-cli连接上来

   `docker exec -it 运行着Rediis服务的容器ID redis-cli`

   ![image-20221206020551224](07-docker.assets/image-20221206020551224.png)

8. 证明docker启动使用了我们自己指定的配置文件

   - 修改配置文件前

     ![image-20221206020711952](07-docker.assets/image-20221206020711952.png)

   - 修改配置文件后

     记得重启服务：`docker restart 容器名或容器ID`

     ![image-20221206020905188](07-docker.assets/image-20221206020905188.png)

     ![image-20221206021145768](07-docker.assets/image-20221206021145768.png)

+++

## 二、高级篇

### 1 Docker复杂安装详说

#### 1.1 安装mysql主从复制

##### 1.1.1 新建主服务器容器实例3307

1. 新建主服务器容器实例3307

   ```bash
   docker run -p 3307:3306 --name mysql-master -v /mydata/mysql-master/log:/var/log/mysql -v /mydata/mysql-master/data:/var/lib/mysql -v /mydata/mysql-master/conf:/etc/mysql -e MYSQL_ROOT_PASSWORD=root  -d mysql:5.7
   # OR
   docker run -p 3307:3306 --name mysql-master \
   -v /mydata/mysql-master/log:/var/log/mysql \
   -v /mydata/mysql-master/data:/var/lib/mysql \
   -v /mydata/mysql-master/conf:/etc/mysql \
   -e MYSQL_ROOT_PASSWORD=root \
   -d mysql:5.7
   ```

   ![image-20221206190240979](07-docker.assets/image-20221206190240979.png)

2. 进入 /mydata/mysql-master/conf 目录下新建 my.cnf

   `cd /mydata/mysql-master/conf`

   `vim my.cnf`

   ```ini
   [mysqld]
   ## 设置server_id，同一局域网中需要唯一
   server_id=101
   ## 指定不需要同步的数据库名称
   binlog-ignore-db=mysql
   ## 开启二进制日志功能
   log-bin=mall-mysql-bin
   ## 设置二进制日志使用内存大小（事务）
   binlog_cache_size=1M
   ## 设置使用的二进制日志格式（mixed,statement,row）
   binlog_format=mixed
   ## 二进制日志过期清理时间。默认值为0，表示不自动清理。
   expire_logs_days=7
   ## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
   ## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
   slave_skip_errors=1062
   ```

3. 修改完配置后重启master实例

   `docker restart mysql-master`

   ![image-20221206190640516](07-docker.assets/image-20221206190640516.png)

4. 进入mysql-master容器

   `docker exec -it mysql-master /bin/bash`

   `mysql -uroot -proot`

   ![image-20221206190833433](07-docker.assets/image-20221206190833433.png)

5. master容器实例内创建数据同步用户

   `CREATE USER 'slave'@'%' IDENTIFIED BY '123456';`

   `GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';`



##### 1.1.2 新建从服务器容器实例3308

1. 新建从服务器容器实例3308

   ```bash
   docker run -p 3308:3306 --name mysql-slave \
   -v /mydata/mysql-slave/log:/var/log/mysql \
   -v /mydata/mysql-slave/data:/var/lib/mysql \
   -v /mydata/mysql-slave/conf:/etc/mysql \
   -e MYSQL_ROOT_PASSWORD=root  \
   -d mysql:5.7
   ```

   ![image-20221206191216555](07-docker.assets/image-20221206191216555.png)

2. 进入 /mydata/mysql-slave/conf 目录下新建 my.cnf

   `cd /mydata/mysql-slave/conf`

   `vim my.cnf`

   ```ini
   [mysqld]
   ## 设置server_id，同一局域网中需要唯一
   server_id=102
   ## 指定不需要同步的数据库名称
   binlog-ignore-db=mysql  
   ## 开启二进制日志功能，以备Slave作为其它数据库实例的Master时使用
   log-bin=mall-mysql-slave1-bin  
   ## 设置二进制日志使用内存大小（事务）
   binlog_cache_size=1M  
   ## 设置使用的二进制日志格式（mixed,statement,row）
   binlog_format=mixed  
   ## 二进制日志过期清理时间。默认值为0，表示不自动清理。
   expire_logs_days=7  
   ## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
   ## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
   slave_skip_errors=1062  
   ## relay_log配置中继日志
   relay_log=mall-mysql-relay-bin  
   ## log_slave_updates表示slave将复制事件写进自己的二进制日志
   log_slave_updates=1  
   ## slave设置为只读（具有super权限的用户除外）
   read_only=1
   ```

3. 修改完配置后重启slave实例

   `docker restart mysql-slave`

   ![image-20221206191644748](07-docker.assets/image-20221206191644748.png)

4. 在主数据库中查看主从同步状态

   `show master status;`

   ![image-20221206191813322](07-docker.assets/image-20221206191813322.png)

5. 进入mysql-slave容器

   `docker exec -it mysql-slave /bin/bash`

   `mysql -uroot -proot`

   ![image-20221206192028739](07-docker.assets/image-20221206192028739.png)



##### 1.1.3 配置主从复制

1. 在从数据库中配置主从复制

   ```mysql
   change master to master_host='192.168.88.110', master_user='slave', master_password='123456', master_port=3307, master_log_file='mall-mysql-bin.000001', master_log_pos=617, master_connect_retry=30;
   
   # master_host：主数据库的IP地址；
   # master_port：主数据库的运行端口；
   # master_user：在主数据库创建的用于同步数据的用户账号；
   # master_password：在主数据库创建的用于同步数据的用户密码；
   # master_log_file：指定从数据库要复制数据的日志文件，通过查看主数据的状态，获取File参数；
   # master_log_pos：指定从数据库从哪个位置开始复制数据，通过查看主数据的状态，获取Position参数；
   # master_connect_retry：连接失败重试的时间间隔，单位为秒。
   ```

   ![image-20221206192539565](07-docker.assets/image-20221206192539565.png)

2. 在从数据库中查看主从同步状态

   `show slave status \G;`

   ![image-20221206192649912](07-docker.assets/image-20221206192649912.png)

3. 在从数据库中开启主从同步

   `start slave;`

   ![image-20221206192755219](07-docker.assets/image-20221206192755219.png)

4. 查看从数据库状态发现已经同步

   `show slave status \G;`

   ![image-20221206192838350](07-docker.assets/image-20221206192838350.png)



##### 1.1.4 测试

1. 主机新建库-使用库-新建表-插入数据

   ![image-20221206193219013](07-docker.assets/image-20221206193219013.png)

2. 从机使用库-查看记录

   ![image-20221206193350294](07-docker.assets/image-20221206193350294.png)

#### 1.2 安装redis集群

##### 1.2.1 集群存储算法

> 面试题：1~2亿条数据需要缓存，请问如何设计这个存储案例
>
> 回答：单机单台100%不可能，肯定是分布式存储，用redis如何落地？
>
> 上述问题阿里P6~P7工程案例和场景设计类必考题目，一般业界有3种解决方案：

###### 1.2.1.1 哈希取余算法分区

![image-20221206204538342](07-docker.assets/image-20221206204538342.png)

2亿条记录就是2亿个`k-v`，我们单机不行必须要分布式多机，假设有3台机器构成一个集群，用户每次读写操作都是根据公式：`hash(key) % N`个机器台数，计算出哈希值，用来决定数据映射到哪一个节点上。

> *优点*：

简单粗暴，直接有效，只需要预估好数据规划好节点，例如3台、8台、10台，就能保证一段时间的数据支撑。使用Hash算法让固定的一部分请求落到同一台服务器上，这样每台服务器固定处理一部分请求（并维护这些请求的信息），起到`负载均衡+分而治之`的作用。

> *缺点*：

原来规划好的节点，进行扩容或者缩容就比较麻烦了额，不管扩缩，每次数据变动导致节点有变动，映射关系需要重新进行计算，在服务器个数固定不变时没有问题，如果需要弹性扩容或故障停机的情况下，原来的取模公式就会发生变化：`Hash(key)/3`会变成`Hash(key)/?`。此时地址经过取余运算的结果将发生很大变化，根据公式获取的服务器也会变得不可控。

某个redis机器宕机了，由于台数数量变化，会导致hash取余全部数据重新洗牌。



###### 1.2.1.2 一致性哈希算法分区

> *是什么*：

一致性Hash算法背景

一致性哈希算法在1997年由麻省理工学院中提出的，设计目标是为了解决<font color="gree">分布式缓存数据<font color="red">变动和映射问题</font>，某个机器宕机了，分母数量改变了，自然取余数不OK了</font>。

> *能干嘛*：

提出一致性Hash解决方案。目的是当服务器个数发生变动时，尽量减少影响客户端到服务器的映射关系。

> *3大步骤*：

1. 算法构建一致性哈希环

   *一致性哈希环*：

   一致性哈希算法必然有个hash函数并按照算法产生hash值，这个算法的所有可能哈希值会构成一个全量集，这个集合可以成为一个hash空间[0,2^32-1]，这个是一个线性空间，但是在算法中，我们通过适当的逻辑控制将它首尾相连(0 = 2^32)，这样让它逻辑上形成了一个环形空间。

   它也是按照使用取模的方法，前面笔记介绍的节点取模法是对节点（服务器）的数量进行取模。而一致性Hash算法是对2\^32取模，简单来说，**一致性Hash算法将整个哈希值空间组织成一个虚拟的圆环**，如假设某哈希函数H的值空间为0-2\^32-1（即哈希值是一个32位无符号整形），整个哈希环如下图：整个空间**按顺时针方向组织**，圆环的正上方的点代表0，0点右侧的第一个点代表1，以此类推，2、3、4、…… 直到2\^32-1，也就是说0点左侧的第一个点代表2\^32-1，0和2\^32-1在零点中方向重合，我们把这个由2\^32个点组成的圆环称为Hash环。

   ![image-20221206205742062](07-docker.assets/image-20221206205742062.png)

2. 服务器IP节点映射

   *节点映射*：

   将集群中各个IP节点映射到环上的某一个位置。

   将各个服务器使用Hash进行一个哈希，具体可以选择服务器的`IP`或`主机名`作为关键字进行哈希，这样每台机器就能确定其在哈希环上的位置。假如4个节点NodeA、B、C、D，经过IP地址的哈希函数计算(hash(ip))，使用IP地址哈希后在环空间的位置如下：

   ![image-20221206205940636](07-docker.assets/image-20221206205940636.png)

3. key落到服务器的落键规则

   当我们需要存储一个k-v键值对时，首先计算key的hash值，hash(key)，将这个key使用相同的函数Hash计算出哈希值并确定此数据在环上的位置，*从此位置沿环顺时针“行走”*，第一台遇到的服务器就是其应该定位到的服务器，并将该键值对存储在该节点上。

   如我们有Object A、Object B、Object C、Object D四个数据对象，经过哈希计算后，在环空间上的位置如下：根据一致性Hash算法，数据A会被定为到Node A上，B被定为到Node B上，C被定为到Node C上，D被定为到Node D上。

   ![image-20221206210106756](07-docker.assets/image-20221206210106756.png)

> *优点*：

1. 一致性哈希算法的*容错性*

   容错性

   假设Node C宕机，可以看到此时对象A、B、D不会受到影响，只有C对象被重定位到Node D。一般的，在一致性Hash算法中，如果一台服务器不可用，则**受影响的数据仅仅是此服务器到其环空间中前一台服务器（即沿着逆时针方向行走遇到的第一台服务器）之间数据**，其它不会受到影响。简单说，就是C挂了，受到影响的只是B、C之间的数据，并且这些数据会转移到D进行存储。

   ![image-20221206210359879](07-docker.assets/image-20221206210359879.png)

2. 一致性哈希算法的*扩展性*

   扩展性

   数据量增加了，需要增加一台节点NodeX，X的位置在A和B之间，那收到影响的也就是A到X之间的数据，重新把A到X的数据录入到X上即可，不会导致hash取余全部数据重新洗牌。

> *缺点*：

一致性哈希算法的*数据倾斜*问题

Hash环的数据倾斜问题

一致性Hash算法在服务节点太少时，容易因为节点分布不均匀而造成数据倾斜（被缓存的对象大部分集中缓存在某一台服务器上）问题，

例如系统中只有两台服务器：

![image-20221206210833034](07-docker.assets/image-20221206210833034.png)



###### 1.2.1.3 哈希槽算法分区

> 为什么出现

解决一致性哈希算法的*数据倾斜*问题：

哈希槽实质就是一个数组，数组[0, 2^14 -1]形成hash slot空间。

> 能干什么

解决均匀分配的问题，*在数据和节点之间又加入了一层，把这层称为哈希槽（slot），用于管理数据和节点之间的关系*，现在就相当于节点上放的是槽，槽里放的是数据。

![image-20221206211757890](07-docker.assets/image-20221206211757890.png)

槽解决的是粒度问题，相当于把粒度变大了，这样便于数据移动。

哈希解决的是映射问题，使用key的哈希值来计算所在的槽，便于数据分配。

> 多少个hash槽

一个集群只能有16384个槽，编号0-16383（0-2^14-1）。这些槽会分配给集群中的所有主节点，分配策略没有要求。可以指定哪些编号的槽分配给哪个主节点。集群会记录节点和槽的对应关系。解决了节点和槽的关系后，接下来就需要对key求哈希值，然后对16384取余，余数是几key就落入对应的槽里。`slot = CRC16(key) % 16384`。以槽为单位移动数据，因为槽的数目是固定的，处理起来比较容易，这样数据移动问题就解决了。

> 哈希槽计算

Redis 集群中内置了 16384 个哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。当需要在 Redis 集群中放置一个 key-value时，redis 先对 key 使用 crc16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，也就是映射到某个节点上。如下代码，key之A、B在Node2，key之C落在Node3上

![image-20221206212044095](07-docker.assets/image-20221206212044095.png)



##### 1.2.2 配置3主3从redis集群

1. 关闭防火墙+启动docker后台服务

   ```bash
   systemctl disable firewalld.service
   systemctl start docker
   ```

2. 新建6个docker容器redis实例

   ```bash
   docker run -d --name redis-node-1 --net host --privileged=true -v /data/redis/share/redis-node-1:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6381
   
   docker run -d --name redis-node-2 --net host --privileged=true -v /data/redis/share/redis-node-2:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6382
    
   docker run -d --name redis-node-3 --net host --privileged=true -v /data/redis/share/redis-node-3:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6383
    
   docker run -d --name redis-node-4 --net host --privileged=true -v /data/redis/share/redis-node-4:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6384
    
   docker run -d --name redis-node-5 --net host --privileged=true -v /data/redis/share/redis-node-5:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6385
    
   docker run -d --name redis-node-6 --net host --privileged=true -v /data/redis/share/redis-node-6:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6386
   
   # 参数说明：
   # docker run：创建并运行docker容器实例
   # --name redis-node-6：容器名字
   # --net host：使用宿主机的IP和端口，默认
   # --privileged=true：获取宿主机root用户权限
   # -v /data/redis/share/redis-node-6:/data：容器卷，宿主机地址:docker内部地址
   # redis:6.0.8：redis镜像和版本号
   # --cluster-enabled yes：开启redis集群
   # --appendonly yes：开启持久化
   # --port 6386：redis端口号
   ```

   如果运行成功，效果如下：

   ![image-20221206213244750](07-docker.assets/image-20221206213244750.png)

3. 进入容器redis-node-1并为6台机器构建集群关系

   - 进入容器

     `docker exec -it redis-node-1 /bin/bash`

     ![image-20221206213759650](07-docker.assets/image-20221206213759650.png)

   - 构建主从关系

     ```bash
     # 注意，进入docker容器后才能执行一下命令，且注意自己的真实IP地址
     redis-cli --cluster create 192.168.88.110:6381 192.168.88.110:6382 192.168.88.110:6383 192.168.88.110:6384 192.168.88.110:6385 192.168.88.110:6386 --cluster-replicas 1
     
     # --cluster-replicas 1 表示为每个master创建一个slave节点
     ```

     ![image-20221206214213148](07-docker.assets/image-20221206214213148.png)

     ![image-20221206214324559](07-docker.assets/image-20221206214324559.png)

   - 一切OK的话，3主3从搞定

4. 链接进入6381作为切入点，*查看集群状态*

   `redis-cli -p 6381`

   `cluster info`

   `cluster nodes`

   ![image-20221206215029717](07-docker.assets/image-20221206215029717.png)

   ![image-20221206215240934](07-docker.assets/image-20221206215240934.png)

   ```
   主机		从机
   6381	  6385
   6382	  6386
   6383	  6384
   ```



##### 1.2.3 主从容错切换迁移案例

> 数据读写存储

1. 启动6机构成的集群并通过exec进入

2. 对6381新增两个key

   `redis-cli -p 6381`

   ![image-20221206215728151](07-docker.assets/image-20221206215728151.png)

3. 防止路由失效加参数-c并新增两个key

   加入参数-c，优化路由：`redis-cli -p 6381 -c`

   ![image-20221206215855638](07-docker.assets/image-20221206215855638.png)

4. 查看集群信息

   `redis-cli --cluster check 192.168.88.110:6381`

   ![image-20221206220320572](07-docker.assets/image-20221206220320572.png)![image-20221206220437905](07-docker.assets/image-20221206220437905.png)

> 容错切换迁移

1. 主6381和从机切换，先停止主机6381

   6381主机停了，对应的真实从机上位

   6381作为1号主机分配的从机以实际情况为准，具体是几号机器就是几号

   ![image-20221206220721860](07-docker.assets/image-20221206220721860.png)

2. 再次查看集群信息

   6381宕机了，6385上位成为了新的master。

   备注：本次6381为主下面挂从6385。每次案例下面挂的从机以实际情况为准，具体是几号机器就是几号

   ![image-20221206220946925](07-docker.assets/image-20221206220946925.png)

3. 先还原之前的3主3从

   - 先启6381

     `docker start redis-node-1`

     ![image-20221206221540313](07-docker.assets/image-20221206221540313.png)

   - 再停6385

     `docker stop redis-node-5`

     ![image-20221206221814880](07-docker.assets/image-20221206221814880.png)

   - 再启6385

     `docker start redis-node-5`

     ![image-20221206221953850](07-docker.assets/image-20221206221953850.png)

   - 主从机器分配情况以实际情况为准

4. 查看集群状态

   `redis-cli --cluster check 自己IP:6381`

   ![image-20221206222408656](07-docker.assets/image-20221206222408656.png)

##### 1.2.4 主从扩容案例

1. 新建6387、6388两个节点+新建后启动+查看是否8节点

   ```bash
   docker run -d --name redis-node-7 --net host --privileged=true -v /data/redis/share/redis-node-7:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6387
   
   docker run -d --name redis-node-8 --net host --privileged=true -v /data/redis/share/redis-node-8:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6388
   ```

   如果运行成功，效果如下：

   ![image-20221206224521171](07-docker.assets/image-20221206224521171.png)

2. 进入6387容器实例内部

   `docker exec -it redis-node-7 /bin/bash`

3. 将新增的6387节点(空槽号)作为master节点加入原集群

   `redis-cli --cluster add-node 自己实际IP地址:6387 自己实际IP地址:6381`

   `redis-cli --cluster add-node 192.168.88.110:6387 192.168.88.110:6381`

   6387 就是将要作为master新增节点

   6381 就是原来集群节点里面的领路人，相当于6387拜拜6381的码头从而找到组织加入集群

   ![image-20221206225000781](07-docker.assets/image-20221206225000781.png)

4. 检查集群情况第1次

   `redis-cli --cluster check 192.168.88.110:6381`

   ![image-20221206225250225](07-docker.assets/image-20221206225250225.png)

5. 重新分派槽号

   命令：redis-cli --cluster reshard IP地址:端口号

   `redis-cli --cluster reshard 192.168.88.110:6381`

   ![image-20221206225554935](07-docker.assets/image-20221206225554935.png)

   ![image-20221206225822737](07-docker.assets/image-20221206225822737.png)

6. 检查集群情况第2次

   `redis-cli --cluster check 192.168.88.110:6381`

   > 为什么6387是3个新的区间，以前的还是连续？
   >
   > 重新分配成本太高，所以前3家各自匀出来一部分，从6381/6382/6383三个旧节点分别匀出1364个坑位给新节点6387

   ![image-20221206230103657](07-docker.assets/image-20221206230103657.png)

7. 为主节点6387分配从节点6388

   命令：`redis-cli --cluster add-node ip:新slave端口 ip:新master端口 --cluster-slave --cluster-master-id 新主机节点ID`

   `redis-cli --cluster add-node 192.168.88.110:6388 192.168.88.110:6387 --cluster-slave --cluster-master-id 81925b5c7d6b13d51b3749b9808c29216f5d035f`

   81925b5c7d6b13d51b3749b9808c29216f5d035f-------这个是6387的编号，按照自己实际情况

   ![image-20221206230848711](07-docker.assets/image-20221206230848711.png)

8. 检查集群情况第3次

   `redis-cli --cluster check 192.168.88.110:6381`

   ![image-20221206231140661](07-docker.assets/image-20221206231140661.png)

##### 1.2.5 主从缩容案例

1. 目的：6387和6388下线

2. 检查集群情况1：获得6388的节点ID

   `redis-cli --cluster check 192.168.88.110:6382`

   5490630a118a6c65625f93df8e711f4667a4cc40

   ![image-20221206232517670](07-docker.assets/image-20221206232517670.png)

3. 从集群中将4号从节点6388删除

   命令：redis-cli --cluster del-node ip:从机端口 从机6388节点ID

   `redis-cli --cluster del-node 192.168.88.110:6388 5490630a118a6c65625f93df8e711f4667a4cc40`

   ![image-20221206232829932](07-docker.assets/image-20221206232829932.png)

   `redis-cli --cluster check 192.168.88.110:6382`

   检查一下发现，6388被删除了，只剩下7台机器了。

   ![image-20221206233002790](07-docker.assets/image-20221206233002790.png)

4. 将6387的槽号清空，重新分配，本例将清出来的槽号都给6381

   `redis-cli --cluster reshard 192.168.88.110:6381`

   ![image-20221206233547401](07-docker.assets/image-20221206233547401.png)

5. 检查集群情况第二次

   `redis-cli --cluster check 192.168.88.110:6382`

   4096个槽位都指给6381，它变成了8192个槽位，相当于全部都给6381了，不然要输入3次，一锅端

   ![image-20221206234029511](07-docker.assets/image-20221206234029511.png)

6. 将6387删除

   命令：redis-cli --cluster del-node ip:端口 6387节点ID

   `redis-cli --cluster del-node 192.168.88.110:6387 1c182c3beadbf9f2a963455f2f9ee8ff1388b3f3`

   ![image-20221206234454182](07-docker.assets/image-20221206234454182.png)

7. 检查集群情况第三次

   `redis-cli --cluster check 192.168.88.110:6382`

   ![image-20221206234633015](07-docker.assets/image-20221206234633015.png)



### 2 DockerFile解析

#### 2.1 DockerFile简介

Dockerfile是用来*构建Docker镜像*的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。

官网：https://docs.docker.com/engine/reference/builder/

构建三步骤

1. 编写Dockerfile文件
2. `docker build`命令构建镜像
3. `docker run`依镜像运行容器实例

![image-20221207001017406](07-docker.assets/image-20221207001017406.png)

#### 2.2 DockerFile构建过程解析

> Dockerfile内容基础知识

- 每条保留字指令都**必须为大写字母**且后面要跟随至少一个参数
- 指令按照从上到下，顺序执行
- \# 表示注释
- 每条指令都会创建一个新的镜像层并对镜像进行提交

> Docker执行Dockerfile的大致流程

1. docker从基础镜像运行一个容器
2. 执行一条指令并对容器作出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行一个新容器
5. 执行dockerfile中的下一条指令直到所有指令都执行完成

> 小总结：

从应用软件的角度来看，Dockerfile、Docker镜像与Docker容器分别代表软件的三个不同阶段：
*  Dockerfile是软件的原材料
*  Docker镜像是软件的交付品
*  Docker容器则可以认为是软件镜像的运行态，也即依照镜像运行的容器实例

Dockerfile面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可，合力充当Docker体系的基石。

![image-20221207001730959](07-docker.assets/image-20221207001730959.png)

1. Dockerfile，需要定义一个Dockerfile，Dockerfile定义了进程需要的一切东西。Dockerfile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程(当应用进程需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制)等等；
2. Docker镜像，在用Dockerfile定义一个文件之后，`docker build`时会产生一个Docker镜像，当运行Docker镜像时会真正开始提供服务；
3. Docker容器，容器是直接提供服务的。



#### 2.3 DockerFile常用保留字指令

参考tomcat8的dockerfile入门：https://github.com/docker-library/tomcat

1. *FROM*

   基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条必须是from

2. *MAINTAINER*

   镜像维护者的姓名和邮箱地址

3. *RUN*

   容器构建时需要运行的命令

   RUN 是在 docker build 时运行

   两种格式

   - shell格式：`RUN yum -y install vim`

     ![image-20221207224009253](07-docker.assets/image-20221207224009253.png)

   - exec格式

     ![image-20221207224039860](07-docker.assets/image-20221207224039860.png)

4. *EXPOSE*

   当前容器对外暴露出的端口

5. *WORKDIR*

   指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

6. *USER*

   指定该镜像以什么样的用户去执行，如果都不指定，默认是root

7. *ENV*

   用来在构建镜像过程中设置环境变量

   `ENV MY_PATH /usr/mytest` 

   这个环境变量可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样；

   也可以在其它指令中直接使用这些环境变量，

   比如：`WORKDIR $MY_PATH`

8. *ADD*

   将宿主机目录下的文件拷贝进镜像且会自动处理URL和解压tar压缩包

9. *COPY*

   类似ADD，拷贝文件和目录到镜像中。

   将从构建上下文目录中 `<源路径>的文件/目录` 复制到新的一层的镜像内的 <目标路径> 位置

   - `COPY src dest`
   - `COPY ["src", "dest"]`
   - <src源路径>：源文件或者源目录
   - <dest目标路径>：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。

10. *VOLUME*

    容器数据卷，用于数据保存和持久化工作

11. *CMD*

    指定容器启动后的要干的事情

    ![image-20221207225315503](07-docker.assets/image-20221207225315503.png)

    > 注意：
    >
    > Dockerfile 中可以有多个 CMD 指令，**但只有最后一个生效，CMD 会被 docker run 之后的参数替换**。
    >
    > 参考官网Tomcat的dockerfile演示讲解
    >
    > 官网最后一行命令
    >
    > ![image-20221207224905747](07-docker.assets/image-20221207224905747.png)
    >
    > 我们演示自己的覆盖操作
    >
    > ![image-20221207225136934](07-docker.assets/image-20221207225136934.png)

    它和前面RUN命令的区别

    - CMD是在 `docker run` 时运行。
    - RUN是在 `docker build` 时运行。

12. *ENTRYPOINT*

    也是用来指定一个容器启动时要运行的命令

    类似于 CMD 指令，*但是ENTRYPOINT不会被docker run后面的命令覆盖*，而且这些命令行参数*会被当作参数送给 ENTRYPOINT 指令指定的程序*。

    > 命令格式和案例说明

    命令格式：

    `ENTRYPOINT ["<executeable>", "<param1>", "<param2>", ...]`

    ENTRYPOINT 可以和 CMD 一起用，一般是变参才会使用 CMD，这里的 CMD 等于是在给 ENTRYPOINT 传参。

    当指定了ENTRYPOINT后，CMD的含义就发生了变化，不再是直接运行其命令而是将CMD的内容作为参数传递给ENTRYPOINT指令，他两个组合会变成`<ENTRYPOINT> "<CMD>"`

    案例如下：假设已通过 Dockerfile 构建了 `nginx:test镜像`：

    ![image-20221207225945199](07-docker.assets/image-20221207225945199.png)

    | **是否传参**     | **按照dockerfile编写执行**     | **传参运行**                                   |
    | ---------------- | ------------------------------ | ---------------------------------------------- |
    | Docker命令       | docker run nginx:test          | docker run nginx:test -c /etc/nginx/*new.conf* |
    | 衍生出的实际命令 | nginx -c /etc/nginx/nginx.conf | nginx -c /etc/nginx/*new.conf*                 |

    > 优点：在执行docker run的时候可以指定 ENTRYPOINT 运行所需的参数。
    >
    > 注意：如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。

13. 小结

    ![image-20221207230306036](07-docker.assets/image-20221207230306036.png)

#### 2.4 自定义镜像mycentosjava8

> 要求

Centos7镜像具备 vim + ifconfig + jdk8

JDK的下载镜像地址：https://mirrors.yangxingzhen.com/jdk/

![image-20221207232449497](07-docker.assets/image-20221207232449497.png)

centos镜像下载：

![image-20221207234826664](07-docker.assets/image-20221207234826664.png)

> 编写

准备编写Dockerfile文件，文件名称必须为Dockerfile

![image-20221207232813728](07-docker.assets/image-20221207232813728.png)

`vim Dockerfile`

```dockerfile
FROM centos:7.6.1810
MAINTAINER shanhai<sh@126.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

#安装vim编辑器
RUN yum -y install vim
#安装ifconfig命令查看网络IP
RUN yum -y install net-tools
#安装java8及lib库
RUN yum -y install glibc.i686
RUN mkdir /usr/local/java
#ADD 是相对路径jar,把jdk-8u171-linux-x64.tar.gz添加到容器中,安装包必须要和Dockerfile文件在同一位置
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java/
#配置java环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH

EXPOSE 80

CMD echo $MYPATH
CMD echo "success--------------ok"
CMD /bin/bash
```

> 构建

`docker build -t 新镜像名字:TAG .`

注意，上面TAG后面有个空格，有个点

`docker build -t mycentosjava8:1.5 .`

![image-20221207235218267](07-docker.assets/image-20221207235218267.png)

构建出一个新的镜像：

![image-20221207235246629](07-docker.assets/image-20221207235246629.png)

> 运行

`docker run -it 新镜像名字:TAG`

`docker run -it mycentosjava8:1.5 /bin/bash`

![image-20221207235616556](07-docker.assets/image-20221207235616556.png)



#### 2.5 虚悬镜像

> 虚悬镜像是什么？

仓库名、标签都是\<none\>的镜像，俗称dangling image

Dockerfile写一个：

1. `vim Dockerfile`

   ```dockerfile
   FROM ubuntu
   CMD echo 'action is success'
   ```

2. `docker build .`

   ![image-20221208001100895](07-docker.assets/image-20221208001100895.png)

3. `docker images`

   ![image-20221208001146380](07-docker.assets/image-20221208001146380.png)

> 查看虚悬镜像：`docker image ls -f dangling=true`

![image-20221208001240283](07-docker.assets/image-20221208001240283.png)

> 删除虚悬镜像：`docker image prune`

虚悬镜像已经失去存在价值，可以删除

![image-20221208001739543](07-docker.assets/image-20221208001739543.png)



### 3 Docker微服务实战

#### 3.1 通过IDEA新建一个普通微服务模块

1. 建Module--->SpringBoot项目--->docker-book

2. 改POM文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.5.13</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
   
       <groupId>com.shanhai</groupId>
       <artifactId>docker-boot</artifactId>
       <version>0.0.1-SNAPSHOT</version>
   
       <properties>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
           <maven.compiler.source>1.8</maven.compiler.source>
           <maven.compiler.target>1.8</maven.compiler.target>
           <junit.version>4.12</junit.version>
           <log4j.version>1.2.17</log4j.version>
           <lombok.version>1.16.18</lombok.version>
           <mysql.version>5.1.47</mysql.version>
           <druid.version>1.1.16</druid.version>
           <mapper.version>4.1.5</mapper.version>
           <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
       </properties>
   
       <dependencies>
           <!--SpringBoot通用依赖模块-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--test-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
       
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-resources-plugin</artifactId>
                   <version>3.1.0</version>
               </plugin>
           </plugins>
       </build>
   </project>
   ```

3. 写application.yml配置文件

   ```yaml
   server:
     port: 6001
   ```

4. 主启动类

   ```java
   package com.shanhai;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class DockerBootApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(DockerBootApplication.class, args);
       }
   
   }
   ```

5. 业务类

   ```java
   package com.shanhai.controller;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   
   import java.util.UUID;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/8 0:39
    */
   @RestController
   public class OrderController {
       @Value("${server.port}")
       private String port;
   
       @RequestMapping("/order/docker")
       public String helloDocker() {
           return "hello docker\t"+port+"\t"+ UUID.randomUUID().toString();
       }
   
       @RequestMapping(value ="/order/index", method = RequestMethod.GET)
       public String index() {
           return "服务端口号: "+"\t"+port+"\t"+UUID.randomUUID().toString();
       }
   }
   ```

6. 本机测试

   ![image-20221208004815566](07-docker.assets/image-20221208004815566.png)

   ![image-20221208004829612](07-docker.assets/image-20221208004829612.png)

 

#### 3.2 通过dockerfile发布微服务部署到docker容器

1. IDEA工具里面搞定微服务jar包

   docker-boot-0.0.1-SNAPSHOT.jar

   ![image-20221208005059720](07-docker.assets/image-20221208005059720.png)

2. 编写Dockerfile

   注：将微服务jar包和Dockerfile文件上传到同一个目录下/mydocker

   ```dockerfile
   # 基础镜像使用java
   FROM java:8
   # 作者
   MAINTAINER shanhai
   # VOLUME 指定临时文件目录为/tmp，在主机/var/lib/docker目录下创建了一个临时文件并链接到容器的/tmp
   VOLUME /tmp
   # 将jar包添加到容器中并更名为zzyy_docker.jar
   ADD docker-boot-0.0.1-SNAPSHOT.jar sh_docker.jar
   # 运行jar包
   RUN bash -c 'touch /sh_docker.jar'
   ENTRYPOINT ["java","-jar","/sh_docker.jar"]
   #暴露6001端口作为微服务
   EXPOSE 6001
   ```

   ![image-20221208005621135](07-docker.assets/image-20221208005621135.png)

3. 构建镜像

   `docker build -t sh_docker:1.6 .`

   ![image-20221208005904092](07-docker.assets/image-20221208005904092.png)

4. 运行容器

   `docker run -d -p 6001:6001 sh_docker:1.6`

   ![image-20221208010329557](07-docker.assets/image-20221208010329557.png)

5. 访问测试

   ![image-20221208010453842](07-docker.assets/image-20221208010453842.png)

   ![image-20221208010455418](07-docker.assets/image-20221208010455418.png)



### 4 Docker网络

#### 4.1 Docker网络是什么

> docker不启动，默认网络情况

- ens33

- lo

- virbr0

  在CentOS7的安装过程中如果有**选择相关虚拟化的的服务安装系统后**，启动网卡时会发现有一个以网桥连接的私网地址的virbr0网卡(virbr0网卡：它还有一个固定的默认IP地址192.168.122.1)，是做虚拟机网桥的使用的，其作用是为连接其上的虚机网卡提供NAT访问外网的功能。

  我们之前学习Linux安装，勾选安装系统的时候附带了libvirt服务才会生成的一个东西，如果不需要可以直接将libvirtd服务卸载，`yum remove libvirt-libs.x86_64`。

![image-20221208074539171](07-docker.assets/image-20221208074539171.png)

> docker启动后，网络情况：**会产生一个名为docker0的虚拟网桥**。

![image-20221208074811399](07-docker.assets/image-20221208074811399.png)

查看docker网络模式命令：`docker network ls`

![image-20221208074958834](07-docker.assets/image-20221208074958834.png)



#### 4.2 常用基本命令

1. All命令：`docker network --help`

   ![image-20221208075239300](07-docker.assets/image-20221208075239300.png)

2. 查看网络：`docker network ls`

3. 查看网络源数据：`docker network inspect XXX网络名字`

4. 删除网络：`docker network rm XXX网络名字`

5. 新增网络：`docker network create XXX网络名字`

> 案例实操：

![image-20221208075919999](07-docker.assets/image-20221208075919999.png)



#### 4.3 Docker网络能干嘛

- 容器间的互联和通信以及端口映射
- 容器IP变动时候可以通过服务名直接网络通信而不受到影响



#### 4.4 Docker网络模式

##### 4.4.1 总体介绍

| **网络模式** | **简介**                                                     | **使用方式**                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------ |
| bridge       | 为每一个容器分配、设置IP等，并将容器连接到一个`docker0`虚拟网桥，默认为该模式 | `--network bridge`                   |
| host         | 容器将不会虚拟出自己的网卡、配置自己的IP等，而是使用宿主机的IP和端口 | `--network host`                     |
| none         | 容器有独立的 Network namespace，但并没有对齐进行任何网络设置，如分配 `veth pari` 和 网桥连接、IP等 | `--network none`                     |
| container    | 新创建的容器不会创建自己的网卡和配置自己的IP，而是和一个指定的容器共享IP、端口范围等 | `--network container:NAME或者容器ID` |

> 容器实例内默认网络IP生产规则

1. 先启动两个ubuntu容器实例

   `docker run -it --name u1 ubuntu bash`

   `docker run -it --name u2 ubuntu bash`

   ![image-20221208081057915](07-docker.assets/image-20221208081057915.png)

2. 查看网络源数据

   `docker inspect 容器ID or 容器名字`

   `docker inspect u1 | tail -n 20`

   `docker inspect u2 | tail -n 20`

   ![image-20221208081421567](07-docker.assets/image-20221208081421567.png)

   ![image-20221208081423499](07-docker.assets/image-20221208081423499.png)

3. 关闭u2实例，新建u3，查看ip变化

   ![image-20221208081814664](07-docker.assets/image-20221208081814664.png)

4. 结论：*docker容器内部的ip是有可能会发生改变的*。



##### 4.4.2 bridge模式

> bridge是什么？

Docker 服务默认会创建一个 docker0 网桥（其上有一个 docker0 内部接口），该桥接网络的名称为docker0，它在**内核层**连通了其他的物理或虚拟网卡，这就将所有容器和本地主机都放到**同一个物理网络**。Docker 默认指定了 docker0 接口 的 IP 地址和子网掩码，**让主机和容器之间可以通过网桥相互通信**。

查看 bridge 网络的详细信息，并通过 grep 获取名称项：`docker network inspect bridge | grep name`

![image-20221208212049147](07-docker.assets/image-20221208212049147.png)

`ifconfig | grep docker`

![image-20221208212143494](07-docker.assets/image-20221208212143494.png)

> 说明

1. Docker使用Linux桥接，在宿主机虚拟一个Docker容器网桥(docker0)，Docker启动一个容器时会根据Docker网桥的网段分配给容器一个IP地址，称为Container-IP，同时Docker网桥是每个容器的默认网关。因为在同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的Container-IP直接通信。

2. *docker run 的时候，没有指定network的话默认使用的网桥模式就是bridge，使用的就是docker0*。在宿主机ifconfig，就可以看到docker0和自己create的network(后面讲)eth0，eth1，eth2 …… 代表网卡一，网卡二，网卡三 …… ，lo代表127.0.0.1，即localhost，inet addr用来表示网卡的IP地址

3. 网桥docker0创建一对对等虚拟设备接口一个叫veth，另一个叫eth0，成对匹配。

   1. 整个宿主机的网桥模式都是docker0，类似一个交换机有一堆接口，每个接口叫veth，在本地主机和容器内分别创建一个虚拟接口，并让他们彼此联通（这样一对接口叫veth pair）；
   2. 每个容器实例内部也有一块网卡，每个接口叫eth0；
   3. docker0上面的每个veth匹配某个容器实例内部的eth0，两两配对，一一匹配。

   ![bridge.webp](07-docker.assets/1652093671572-83d0901c-5052-482e-9dde-4a2804b35bc9.webp)

> 案例实操

`docker run -d -p 8081:8080   --name tomcat81 billygoo/tomcat8-jdk8`

`docker run -d -p 8082:8080   --name tomcat82 billygoo/tomcat8-jdk8`

![image-20221208213409939](07-docker.assets/image-20221208213409939.png)

两两匹配验证

![image-20221208213607944](07-docker.assets/image-20221208213607944.png)

![image-20221208213816325](07-docker.assets/image-20221208213816325.png)

![image-20221208213903115](07-docker.assets/image-20221208213903115.png)



##### 4.4.3 host模式

*是什么*：直接使用宿主机的 IP 地址与外界进行通信，不再需要额外进行 NAT 转换。

*说明*：容器将**不会获得**一个独立的Network Namespace，而是和宿主机共用一个Network Namespace。**容器将不会虚拟出自己的网卡而是使用宿主机的IP和端口**。

![host.webp](07-docker.assets/1652093680434-8180b11d-89f2-4bb6-a485-b3c22bd28688.webp)

> 案例实操

*警告*：`docker run -d -p 8083:8080 --network host --name tomcat83 billygoo/tomcat8-jdk8`

![image-20221208214835549](07-docker.assets/image-20221208214835549.png)

问题：docke启动时总是遇见标题中的警告

原因：docker启动时指定`--network=host`或`-net=host`，如果还指定了`-p 映射端口`，那这个时候就会有此警告，并且通过-p设置的参数将不会起到任何作用，端口号会以主机端口号为主，重复时则递增。

解决：解决的办法就是使用docker的其他网络模式，例如`--network=bridge`，这样就可以解决问题，或者直接无视。



*正确*：`docker run -d --network host --name tomcat83 billygoo/tomcat8-jdk8`

![image-20221208215418176](07-docker.assets/image-20221208215418176.png)

无之前的配对显示了，看容器实例内部：`docker inspect tomcat83 | tail -n 20`

![image-20221208215537836](07-docker.assets/image-20221208215537836.png)

没有设置-p的端口映射了，如何访问启动的tomcat83？？

`http://宿主机IP:8080/`

在CentOS里面用默认的火狐浏览器访问容器内的tomcat83看到访问成功，因为此时容器的IP借用主机的，所以容器共享宿主机网络IP，这样的好处是外部主机与容器可以直接通信。



##### 4.4.4 none模式

*是什么*：禁用网络功能，只有lo标识(就是127.0.0.1表示本地回环)

> 案例实操

`docker run -d -p 8084:8080 --network none --name tomcat84 billygoo/tomcat8-jdk8`

![image-20221208220009129](07-docker.assets/image-20221208220009129.png)

进入容器内部查看

![image-20221208220415117](07-docker.assets/image-20221208220415117.png)

在容器外部查看

![image-20221208220520447](07-docker.assets/image-20221208220520447.png)



##### 4.4.5 container模式

> container是什么

新建的容器和已经存在的一个容器共享一个网络ip配置而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。

![container.webp](07-docker.assets/1652093688335-b6e6329d-5292-4901-ad0b-9e57bf46a1cf.webp)

> 案例实操1：

`docker run -d -p 8085:8080 --name tomcat85 billygoo/tomcat8-jdk8`

`docker run -d -p 8086:8080 --network container:tomcat85 --name tomcat86 billygoo/tomcat8-jdk8`

运行结果

![image-20221208221228946](07-docker.assets/image-20221208221228946.png)

相当于tomcat86和tomcat85公用同一个ip同一个端口，导致端口冲突

本案例用tomcat演示不合适。。。演示坑。。。

换一个镜像给大家演示，

> 案例实操2：

Alpine操作系统是一个面向安全的轻型 Linux发行版：

Alpine Linux 是一款独立的、非商业的通用 Linux 发行版，专为追求安全性、简单性和资源效率的用户而设计。可能很多人没听说过这个 Linux 发行版本，但是经常用 Docker 的朋友可能都用过，因为他小，简单，安全而著称，所以作为基础镜像是非常好的一个选择，可谓是麻雀虽小但五脏俱全，镜像非常小巧，不到 6M 的大小，所以特别适合容器打包。



`docker run -it --name alpine1  alpine /bin/sh`

`docker run -it --network container:alpine1 --name alpine2  alpine /bin/sh`

运行结果，验证共用搭桥

![image-20221208221948567](07-docker.assets/image-20221208221948567.png)

![image-20221208222026845](07-docker.assets/image-20221208222026845.png)

假如此时关闭alpine1，再看看alpine2

![image-20221208222311659](07-docker.assets/image-20221208222311659.png)

![image-20221208222354385](07-docker.assets/image-20221208222354385.png)



##### 4.4.6 自定义网络

> 过时的link

![image-20221208223327972](07-docker.assets/image-20221208223327972.png)

> 案例实操：before

`docker run -d -p 8081:8080 --name tomcat-81 billygoo/tomcat8-jdk8`

`docker run -d -p 8082:8080 --name tomcat-82 billygoo/tomcat8-jdk8`

上述成功启动并用docker exec进入各自容器实例内部

*按照IP地址ping是OK的*；

![image-20221208224132912](07-docker.assets/image-20221208224132912.png)

![image-20221208224301336](07-docker.assets/image-20221208224301336.png)

*按照服务名ping结果是行不通的*。

![image-20221208224807925](07-docker.assets/image-20221208224807925.png)

![image-20221208224809871](07-docker.assets/image-20221208224809871.png)

> 案例实操：after

自定义桥接网络，自定义网络默认使用的是桥接网络bridge

*新建自定义网络*：`docker network create shan-network`

![image-20221208225215449](07-docker.assets/image-20221208225215449.png)

*新建容器加入上一步新建的自定义网络*：

`docker run -d -p 8081:8080 --network shan-network --name tomcat-81 billygoo/tomcat8-jdk8`

`docker run -d -p 8082:8080 --network shan-network --name tomcat-82 billygoo/tomcat8-jdk8`

![image-20221208225940615](07-docker.assets/image-20221208225940615.png)

![image-20221208225942208](07-docker.assets/image-20221208225942208.png)

*结论*：**自定义网络本身就维护好了主机名和ip的对应关系（ip和域名都能通）**。



### 5 Docker-compose容器编排

#### 5.1 简介及安装卸载

> Docker-compose容器编排是什么？

Docker-Compose是Docker官方的开源项目，负责实现对Docker容器集群的快速编排。

Compose 是 Docker 公司推出的一个工具软件，可以管理多个 Docker 容器组成一个应用。你需要定义一个 YAML 格式的配置文件`docker-compose.yml`，*写好多个容器之间的调用关系*。然后，只要一个命令，就能同时启动/关闭这些容器。



> Docker-compose容器编排能干嘛？

- docker建议我们每一个容器中只运行一个服务，因为docker容器本身占用资源极少，所以最好是将每个服务单独的分割开来；但是这样我们又面临了一个问题？

- 如果我需要同时部署好多个服务，难道要每个服务单独写Dockerfile然后再构建镜像，构建容器，这样累都累死了，所以docker官方给我们提供了docker-compose多服务部署的工具。

- 例如要实现一个Web微服务项目，除了Web服务容器本身，往往还需要再加上后端的数据库mysql服务容器，redis服务器，注册中心eureka，甚至还包括负载均衡容器等等。。。。。。

- Compose允许用户通过一个单独的`docker-compose.yml`模板文件（YAML格式）来定义一组相关联的应用容器为一个项目（project）。

- 可以很容易地用一个配置文件定义一个多容器的应用，然后使用一条指令安装这个应用的所有依赖，完成构建。Docker-Compose 解决了容器与容器之间如何管理编排的问题。



> Docker-compose容器编排的下载安装

官网：https://docs.docker.com/compose/compose-file/compose-file-v3/

官网下载：https://docs.docker.com/compose/install/

安装步骤：

```bash
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

![image-20221209000838545](07-docker.assets/image-20221209000838545.png)

卸载：`rm /usr/local/bin/docker-compose`

![image-20221208234808548](07-docker.assets/image-20221208234808548.png)



#### 5.2 Compose核心概念

- 一文件：`docker-compose.yml`

- 两要素：

  1. 服务（service）

     一个个应用容器实例，比如订单微服务、库存微服务、mysql容器、nginx容器或者redis容器

  2. 工程（project）

     由一组关联的应用容器组成的一个*完整业务单元*，在`docker-compose.yml`文件中定义。



#### 5.3 Compose使用的三个步骤及常用命令

> Compose使用的三个步骤

1. 编写`Dockerfile`定义各个微服务应用并构建出对应的镜像文件
2. 使用`docker-compose.yml`定义一个完整业务单元，安排好整体应用中的各个容器服务。
3. 最后，执行`docker-compose up`命令来启动并运行整个应用程序，完成一键部署上线。

> Compose常用命令

| **命令**                            | **说明**                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| docker-compose -h                   | 查看帮助                                                     |
| docker-compose up                   | 启动所有docker-compose服务                                   |
| *docker-compose up -d*              | *启动所有docker-compose服务并后台运行*                       |
| *docker-compose down*               | *停止并删除容器、网络、卷、镜像*                             |
| docker-compose exec yml里面的服务id | 进入容器实例内部<br />docker-compose exec<br />*docker-compose.yml文件中写的服务id* /bin/bash |
| docker-compose ps                   | 展示当前docker-compose编排过的运行的所有容器                 |
| docker-compose top                  | 展示当前docker-compose编排过的容器进程                       |
| docker-compose logs yml里面的服务id | 查看容器输出日志                                             |
| *docker-compose config*             | *检查配置*                                                   |
| *docker-compose config -q*          | *检查配置，有问题才有输出*                                   |
| docker-compose restart              | 重启服务                                                     |
| docker-compose start                | 启动服务                                                     |
| docker-compose stop                 | 停止服务                                                     |



#### 5.4 Compose编排微服务

##### 5.4.1 改造升级微服务工程docker-boot

###### 5.4.1.1 SQL建表建库

```mysql
CREATE DATABASE db2022;
USE db2022;
CREATE TABLE `t_user` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(50) NOT NULL DEFAULT '' COMMENT '用户名',
  `password` varchar(50) NOT NULL DEFAULT '' COMMENT '密码',
  `sex` tinyint(4) NOT NULL DEFAULT '0' COMMENT '性别 0=女 1=男 ',
  `deleted` tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '删除标志，默认0不删除，1删除',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户表';
```



###### 5.4.1.2 改POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.13</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.shanhai</groupId>
    <artifactId>docker-boot</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <mysql.version>5.1.47</mysql.version>
        <druid.version>1.1.16</druid.version>
        <mapper.version>4.1.5</mapper.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
    </properties>

    <dependencies>
        <!--guava Google 开源的 Guava 中自带的布隆过滤器-->
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>23.0</version>
        </dependency>
        <!-- redisson -->
        <dependency>
            <groupId>org.redisson</groupId>
            <artifactId>redisson</artifactId>
            <version>3.13.4</version>
        </dependency>
        <!--SpringBoot通用依赖模块-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--swagger2-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
        <!--SpringBoot与Redis整合依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!--springCache-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
        <!--springCache连接池依赖包-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
        <!-- jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.1.0</version>
        </dependency>
        <!--Mysql数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--SpringBoot集成druid连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>${druid.version}</version>
        </dependency>
        <!--mybatis和springboot整合-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>${mybatis.spring.boot.version}</version>
        </dependency>
        <!--通用基础配置junit/devtools/test/log4j/lombok/hutool-->
        <!--hutool-->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.2.3</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
            <optional>true</optional>
        </dependency>
        <!--persistence-->
        <dependency>
            <groupId>javax.persistence</groupId>
            <artifactId>persistence-api</artifactId>
            <version>1.0.2</version>
        </dependency>
        <!--通用Mapper-->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper</artifactId>
            <version>${mapper.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>3.1.0</version>
            </plugin>
        </plugins>
    </build>
</project>
```



###### 5.4.1.3 写YML

```yaml
server:
  port: 6001
# ========================alibaba.druid相关配置=====================
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://192.168.88.110:3306/db2022?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
    druid:
      test-while-idle: false
# ========================redis相关配置=====================
  redis:
    database: 0
    host: 192.168.88.110
    port: 6379
    password:
    lettuce:
      pool:
        max-active: 8
        max-wait: -1ms
        max-idle: 8
        min-idle: 0
# ========================swagger=====================
  swagger2:
    enabled: true
# ========================mybatis相关配置===================
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.shanhai.entities
```



###### 5.4.1.4 主启动

```java
package com.shanhai;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import tk.mybatis.spring.annotation.MapperScan;

@SpringBootApplication
@MapperScan("com.shanhai.mapper")
public class DockerBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(DockerBootApplication.class, args);
    }

}
```



###### 5.4.1.5 业务类

1. config配置类

   -  RedisConfig

     ```java
     package com.shanhai.config;
     
     import lombok.extern.slf4j.Slf4j;
     import org.springframework.context.annotation.Bean;
     import org.springframework.context.annotation.Configuration;
     import org.springframework.data.redis.connection.lettuce.LettuceConnectionFactory;
     import org.springframework.data.redis.core.RedisTemplate;
     import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
     import org.springframework.data.redis.serializer.StringRedisSerializer;
     
     import java.io.Serializable;
     
     @Configuration
     @Slf4j
     public class RedisConfig {
         /**
          * @param lettuceConnectionFactory
          * @return
          *
          * redis序列化的工具配置类，下面这个请一定开启配置
          * 127.0.0.1:6379> keys *
          * 1) "ord:102"  序列化过
          * 2) "\xac\xed\x00\x05t\x00\aord:102"   野生，没有序列化过
          */
         @Bean
         public RedisTemplate<String, Serializable> redisTemplate(LettuceConnectionFactory lettuceConnectionFactory) {
             RedisTemplate<String,Serializable> redisTemplate = new RedisTemplate<>();
     
             redisTemplate.setConnectionFactory(lettuceConnectionFactory);
             //设置key序列化方式string
             redisTemplate.setKeySerializer(new StringRedisSerializer());
             //设置value的序列化方式json
             redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
     
             redisTemplate.setHashKeySerializer(new StringRedisSerializer());
             redisTemplate.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());
     
             redisTemplate.afterPropertiesSet();
     
             return redisTemplate;
         }
     
     }
     ```

   - SwaggerConfig

     ```java
     package com.shanhai.config;
     
     import org.springframework.beans.factory.annotation.Value;
     import org.springframework.context.annotation.Bean;
     import org.springframework.context.annotation.Configuration;
     import springfox.documentation.builders.ApiInfoBuilder;
     import springfox.documentation.builders.PathSelectors;
     import springfox.documentation.builders.RequestHandlerSelectors;
     import springfox.documentation.service.ApiInfo;
     import springfox.documentation.spi.DocumentationType;
     import springfox.documentation.spring.web.plugins.Docket;
     import springfox.documentation.swagger2.annotations.EnableSwagger2;
     
     import java.text.SimpleDateFormat;
     import java.util.Date;
     
     @Configuration
     @EnableSwagger2
     public class SwaggerConfig {
         @Value("${spring.swagger2.enabled}")
         private Boolean enabled;
     
         @Bean
         public Docket createRestApi() {
             return new Docket(DocumentationType.SWAGGER_2)
                     .apiInfo(apiInfo())
                     .enable(enabled)
                     .select()
                     .apis(RequestHandlerSelectors.basePackage("com.atguigu.docker")) //你自己的package
                     .paths(PathSelectors.any())
                     .build();
         }
     
         public ApiInfo apiInfo() {
             return new ApiInfoBuilder()
                     .title("山海Java学习"+"\t"+new SimpleDateFormat("yyyy-MM-dd").format(new Date()))
                     .description("docker-compose")
                     .version("1.0")
                     .termsOfServiceUrl("https://www.atguigu.com/")
                     .build();
         }
     }
     ```

2. 新建entity

   - User

     ```java
     package com.shanhai.entities;
     
     import javax.persistence.Column;
     import javax.persistence.GeneratedValue;
     import javax.persistence.Id;
     import javax.persistence.Table;
     import java.util.Date;
     
     @Table(name = "t_user")
     public class User {
         @Id
         @GeneratedValue(generator = "JDBC")
         private Integer id;
     
         /**
          * 用户名
          */
         private String username;
     
         /**
          * 密码
          */
         private String password;
     
         /**
          * 性别 0=女 1=男 
          */
         private Byte sex;
     
         /**
          * 删除标志，默认0不删除，1删除
          */
         private Byte deleted;
     
         /**
          * 更新时间
          */
         @Column(name = "update_time")
         private Date updateTime;
     
         /**
          * 创建时间
          */
         @Column(name = "create_time")
         private Date createTime;
     
         /**
          * @return id
          */
         public Integer getId() {
             return id;
         }
     
         /**
          * @param id
          */
         public void setId(Integer id) {
             this.id = id;
         }
     
         /**
          * 获取用户名
          *
          * @return username - 用户名
          */
         public String getUsername() {
             return username;
         }
     
         /**
          * 设置用户名
          *
          * @param username 用户名
          */
         public void setUsername(String username) {
             this.username = username;
         }
     
         /**
          * 获取密码
          *
          * @return password - 密码
          */
         public String getPassword() {
             return password;
         }
     
         /**
          * 设置密码
          *
          * @param password 密码
          */
         public void setPassword(String password) {
             this.password = password;
         }
     
         /**
          * 获取性别 0=女 1=男 
          *
          * @return sex - 性别 0=女 1=男 
          */
         public Byte getSex() {
             return sex;
         }
     
         /**
          * 设置性别 0=女 1=男 
          *
          * @param sex 性别 0=女 1=男 
          */
         public void setSex(Byte sex) {
             this.sex = sex;
         }
     
         /**
          * 获取删除标志，默认0不删除，1删除
          *
          * @return deleted - 删除标志，默认0不删除，1删除
          */
         public Byte getDeleted() {
             return deleted;
         }
     
         /**
          * 设置删除标志，默认0不删除，1删除
          *
          * @param deleted 删除标志，默认0不删除，1删除
          */
         public void setDeleted(Byte deleted) {
             this.deleted = deleted;
         }
     
         /**
          * 获取更新时间
          *
          * @return update_time - 更新时间
          */
         public Date getUpdateTime() {
             return updateTime;
         }
     
         /**
          * 设置更新时间
          *
          * @param updateTime 更新时间
          */
         public void setUpdateTime(Date updateTime) {
             this.updateTime = updateTime;
         }
     
         /**
          * 获取创建时间
          *
          * @return create_time - 创建时间
          */
         public Date getCreateTime() {
             return createTime;
         }
     
         /**
          * 设置创建时间
          *
          * @param createTime 创建时间
          */
         public void setCreateTime(Date createTime) {
             this.createTime = createTime;
         }
     }
     ```

   - UserDTO

     ```java
     package com.shanhai.entities;
     
     import io.swagger.annotations.ApiModel;
     import io.swagger.annotations.ApiModelProperty;
     import lombok.AllArgsConstructor;
     import lombok.Data;
     import lombok.NoArgsConstructor;
     
     import java.io.Serializable;
     import java.util.Date;
     
     @NoArgsConstructor
     @AllArgsConstructor
     @Data
     @ApiModel(value = "用户信息")
     public class UserDTO implements Serializable {
         @ApiModelProperty(value = "用户ID")
         private Integer id;
     
         @ApiModelProperty(value = "用户名")
         private String username;
     
         @ApiModelProperty(value = "密码")
         private String password;
     
         @ApiModelProperty(value = "性别 0=女 1=男 ")
         private Byte sex;
     
         @ApiModelProperty(value = "删除标志，默认0不删除，1删除")
         private Byte deleted;
     
         @ApiModelProperty(value = "更新时间")
         private Date updateTime;
     
         @ApiModelProperty(value = "创建时间")
         private Date createTime;
     
         /**
          * @return id
          */
         public Integer getId() {
             return id;
         }
     
         /**
          * @param id
          */
         public void setId(Integer id) {
             this.id = id;
         }
     
         /**
          * 获取用户名
          *
          * @return username - 用户名
          */
         public String getUsername() {
             return username;
         }
     
         /**
          * 设置用户名
          *
          * @param username 用户名
          */
         public void setUsername(String username) {
             this.username = username;
         }
     
         /**
          * 获取密码
          *
          * @return password - 密码
          */
         public String getPassword() {
             return password;
         }
     
         /**
          * 设置密码
          *
          * @param password 密码
          */
         public void setPassword(String password) {
             this.password = password;
         }
     
         /**
          * 获取性别 0=女 1=男 
          *
          * @return sex - 性别 0=女 1=男 
          */
         public Byte getSex() {
             return sex;
         }
     
         /**
          * 设置性别 0=女 1=男 
          *
          * @param sex 性别 0=女 1=男 
          */
         public void setSex(Byte sex) {
             this.sex = sex;
         }
     
         /**
          * 获取删除标志，默认0不删除，1删除
          *
          * @return deleted - 删除标志，默认0不删除，1删除
          */
         public Byte getDeleted() {
             return deleted;
         }
     
         /**
          * 设置删除标志，默认0不删除，1删除
          *
          * @param deleted 删除标志，默认0不删除，1删除
          */
         public void setDeleted(Byte deleted) {
             this.deleted = deleted;
         }
     
         /**
          * 获取更新时间
          *
          * @return update_time - 更新时间
          */
         public Date getUpdateTime() {
             return updateTime;
         }
     
         /**
          * 设置更新时间
          *
          * @param updateTime 更新时间
          */
         public void setUpdateTime(Date updateTime) {
             this.updateTime = updateTime;
         }
     
         /**
          * 获取创建时间
          *
          * @return create_time - 创建时间
          */
         public Date getCreateTime() {
             return createTime;
         }
     
         /**
          * 设置创建时间
          *
          * @param createTime 创建时间
          */
         public void setCreateTime(Date createTime) {
             this.createTime = createTime;
         }
     
         @Override
         public String toString() {
             return "User{" +
                     "id=" + id +
                     ", username='" + username + '\'' +
                     ", password='" + password + '\'' +
                     ", sex=" + sex +
                     '}';
         }
     }
     ```

3. 新建mapper

   - 新建接口UserMapper 

     ```java
     package com.shanhai.mapper;
     
     import com.shanhai.entities.User;
     import tk.mybatis.mapper.common.Mapper;
     
     public interface UserMapper extends Mapper<User> {
     }
     ```

   - src\main\resources路径下新建mapper文件夹并新增UserMapper.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.shanhai.mapper.UserMapper">
         <resultMap id="BaseResultMap" type="com.shanhai.entities.User">
             <!--
               WARNING - @mbg.generated
             -->
             <id column="id" jdbcType="INTEGER" property="id" />
             <result column="username" jdbcType="VARCHAR" property="username" />
             <result column="password" jdbcType="VARCHAR" property="password" />
             <result column="sex" jdbcType="TINYINT" property="sex" />
             <result column="deleted" jdbcType="TINYINT" property="deleted" />
             <result column="update_time" jdbcType="TIMESTAMP" property="updateTime" />
             <result column="create_time" jdbcType="TIMESTAMP" property="createTime" />
         </resultMap>
     </mapper>
     ```

4. 新建service

   ```java
   package com.shanhai.service;
   
   import com.shanhai.entities.User;
   import com.shanhai.mapper.UserMapper;
   import lombok.extern.slf4j.Slf4j;
   import org.springframework.data.redis.core.RedisTemplate;
   import org.springframework.stereotype.Service;
   
   import javax.annotation.Resource;
   
   @Service
   @Slf4j
   public class UserService {
   
       public static final String CACHE_KEY_USER = "user:";
   
       @Resource
       private UserMapper userMapper;
       @Resource
       private RedisTemplate redisTemplate;
   
       /**
        * addUser
        * @param user
        */
       public void addUser(User user) {
           //1 先插入mysql并成功
           int i = userMapper.insertSelective(user);
   
           if(i > 0)
           {
               //2 需要再次查询一下mysql将数据捞回来并ok
               user = userMapper.selectByPrimaryKey(user.getId());
               //3 将捞出来的user存进redis，完成新增功能的数据一致性。
               String key = CACHE_KEY_USER+user.getId();
               redisTemplate.opsForValue().set(key,user);
           }
       }
   
       /**
        * findUserById
        * @param id
        * @return
        */
       public User findUserById(Integer id) {
           User user = null;
           String key = CACHE_KEY_USER+id;
   
           //1 先从redis里面查询，如果有直接返回结果，如果没有再去查询mysql
           user = (User) redisTemplate.opsForValue().get(key);
   
           if(user == null)
           {
               //2 redis里面无，继续查询mysql
               user = userMapper.selectByPrimaryKey(id);
               if(user == null)
               {
                   //3.1 redis+mysql 都无数据
                   //你具体细化，防止多次穿透，我们规定，记录下导致穿透的这个key回写redis
                   return user;
               }else{
                   //3.2 mysql有，需要将数据写回redis，保证下一次的缓存命中率
                   redisTemplate.opsForValue().set(key,user);
               }
           }
           return user;
       }
   }
   ```

5. 新建controller

   ```java
   package com.shanhai.controller;
   
   import cn.hutool.core.util.IdUtil;
   import com.shanhai.entities.User;
   import com.shanhai.service.UserService;
   import io.swagger.annotations.Api;
   import io.swagger.annotations.ApiOperation;
   import lombok.extern.slf4j.Slf4j;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.bind.annotation.RestController;
   
   import javax.annotation.Resource;
   import java.util.Random;
   
   @Api(description = "用户User接口")
   @RestController
   @Slf4j
   public class UserController {
       @Resource
       private UserService userService;
   
       @ApiOperation("数据库新增3条记录")
       @RequestMapping(value = "/user/add",method = RequestMethod.POST)
       public void addUser() {
           for (int i = 1; i <=3; i++) {
               User user = new User();
   
               user.setUsername("zzyy"+i);
               user.setPassword(IdUtil.simpleUUID().substring(0,6));
               user.setSex((byte) new Random().nextInt(2));
   
               userService.addUser(user);
           }
       }
   
       @ApiOperation("查询1条记录")
       @RequestMapping(value = "/user/find/{id}",method = RequestMethod.GET)
       public User findUserById(@PathVariable Integer id) {
           return userService.findUserById(id);
       }
   }
   ```



###### 5.4.1.6 打包-Dockerfile

> `mvn package`命令将微服务形成新的jar包，并上传到Linux服务器/mydocker目录下

![image-20221209144409014](07-docker.assets/image-20221209144409014.png)

> 编写Dockerfile

```dockerfile
# 基础镜像使用java
FROM java:8
# 作者
MAINTAINER shanhai
# VOLUME 指定临时文件目录为/tmp，在主机/var/lib/docker目录下创建了一个临时文件并链接到容器的/tmp
VOLUME /tmp
# 将jar包添加到容器中并更名为zzyy_docker.jar
ADD docker-boot-0.0.1-SNAPSHOT.jar sh_docker.jar
# 运行jar包
RUN bash -c 'touch /sh_docker.jar'
ENTRYPOINT ["java","-jar","/sh_docker.jar"]
# 暴露6001端口作为微服务
EXPOSE 6001
```

![image-20221209144659240](07-docker.assets/image-20221209144659240.png)

> 构建镜像：`docker build -t sh_docker1:1.8 .`

![image-20221209145104092](07-docker.assets/image-20221209145104092.png)

![image-20221209145201273](07-docker.assets/image-20221209145201273.png)



##### 5.4.2 不用Compose

1. 单独的mysql容器实例

   **新建mysql容器实例**：

   ```bash
   docker run -d -p 3306:3306 --privileged=true \ 
   -v /shanhai/mysql/log:/var/log/mysql \
   -v /shanhai/mysql/data:/var/lib/mysql \
   -v /shanhai/mysql/conf:/etc/mysql/conf.d \
   -e MYSQL_ROOT_PASSWORD=123456 \
   --name mysql mysql:5.7
   ```

   进入mysql容器实例并新建库db2022+新建表t_user

   `docker exec -it mysql /bin/bash`

   `mysql -uroot -p123456`

   ```mysql
   create database db2022;
   use db2022;
   
   CREATE TABLE `t_user` (
     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
     `username` varchar(50) NOT NULL DEFAULT '' COMMENT '用户名',
     `password` varchar(50) NOT NULL DEFAULT '' COMMENT '密码',
     `sex` tinyint(4) NOT NULL DEFAULT '0' COMMENT '性别 0=女 1=男 ',
     `deleted` tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '删除标志，默认0不删除，1删除',
     `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
     `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户表';
   ```

2. 单独的redis容器实例

   ```bash
   docker run -p 6379:6379 --name myr3 \
   --privileged=true \
   -v /app/redis/redis.conf:/etc/redis/redis.conf \
   -v /app/redis/data:/data \
   -d redis:6.0.8 redis-server /etc/redis/redis.conf
   ```

3. 启动微服务工程

   ```bash
   docker run -d -p 6001:6001 sh_docker1:1.8
   ```

4. 上面三个容器实例依次顺序启动成功

   ![image-20221209150306852](07-docker.assets/image-20221209150306852.png)

5. swagger测试

   `http://192.168.88.110:6001//swagger-ui.html#`

   `http://linux-docker:6001//swagger-ui.html#`

   ![image-20221209152931541](07-docker.assets/image-20221209152931541.png)

6. 上面成功了，有哪些问题？

   - 先后顺序要求固定，先mysql+redis才能微服务访问成功
   - 需要执行多个run命令......
   - 容器间的启停或宕机，有可能导致IP地址对应的容器实例变化，映射出错，要么生产IP写死(可以但是不推荐)，要么通过服务调用



##### 5.4.3 使用Compose

服务编排，一套带走，安排。

1. 在Linux服务器/mydocker目录下编写`docker-compose.yml`文件

   ```yaml
   version: "3"
   
   services:
     microService:
       image: sh_docker1:1.8
       container_name: sh01
       ports:
         - "6001:6001"
       volumes:
         - /app/microService:/data
       networks: 
         - sh_net
       depends_on: 
         - redis
         - mysql
    
     redis:
       image: redis:6.0.8
       ports:
         - "6379:6379"
       volumes:
         - /app/redis/redis.conf:/etc/redis/redis.conf
         - /app/redis/data:/data
       networks: 
         - sh_net
       command: redis-server /etc/redis/redis.conf
    
     mysql:
       image: mysql:5.7
       environment:
         MYSQL_ROOT_PASSWORD: '123456'
         MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
         MYSQL_DATABASE: 'db2022'
         MYSQL_USER: 'shanhai'
         MYSQL_PASSWORD: '123123'
       ports:
          - "3306:3306"
       volumes:
          - /app/mysql/db:/var/lib/mysql
          - /app/mysql/conf/my.cnf:/etc/my.cnf
          - /app/mysql/init:/docker-entrypoint-initdb.d
       networks:
         - sh_net
       command: --default-authentication-plugin=mysql_native_password #解决外部无法访问
   
   networks: 
      sh_net: 
   ```

2. 第二次修改微服务工程docker-boot

   - 写YML：通过服务名访问，IP无关

     ```yaml
     server:
       port: 6001
     # ========================alibaba.druid相关配置=====================
     spring:
       datasource:
         type: com.alibaba.druid.pool.DruidDataSource
         driver-class-name: com.mysql.jdbc.Driver
         # url: jdbc:mysql://192.168.88.110:3306/db2022?useUnicode=true&characterEncoding=utf-8&useSSL=false
         url: jdbc:mysql://mysql:3306/db2022?useUnicode=true&characterEncoding=utf-8&useSSL=false
         username: root
         password: 123456
         druid:
           test-while-idle: false
     # ========================redis相关配置=====================
       redis:
         database: 0
         # host: 192.168.88.110
         host: redis
         port: 6379
         password:
         lettuce:
           pool:
             max-active: 8
             max-wait: -1ms
             max-idle: 8
             min-idle: 0
     # ========================swagger=====================
       swagger2:
         enabled: true
     # ========================mybatis相关配置===================
     mybatis:
       mapper-locations: classpath:mapper/*.xml
       type-aliases-package: com.shanhai.entities
     ```

   - `mvn package`命令将微服务形成新的jar包，并上传到Linux服务器/mydocker目录下

   - 编写Dockerfile

     ```dockerfile
     # 基础镜像使用java
     FROM java:8
     # 作者
     MAINTAINER shanhai
     # VOLUME 指定临时文件目录为/tmp，在主机/var/lib/docker目录下创建了一个临时文件并链接到容器的/tmp
     VOLUME /tmp
     # 将jar包添加到容器中并更名为zzyy_docker.jar
     ADD docker-boot-0.0.1-SNAPSHOT.jar sh_docker.jar
     # 运行jar包
     RUN bash -c 'touch /sh_docker.jar'
     ENTRYPOINT ["java","-jar","/sh_docker.jar"]
     # 暴露6001端口作为微服务
     EXPOSE 6001
     ```

   - 构建镜像

     ```bash
     docker build -t sh_docker1:1.8 .
     ```

     ![image-20221209214953199](07-docker.assets/image-20221209214953199.png)

     ![image-20221209215046260](07-docker.assets/image-20221209215046260.png)

3. 执行`docker-compose up`或者执行`docker-compose up -d`

   ![image-20221209215348888](07-docker.assets/image-20221209215348888.png)

4. 进入mysql容器实例并新建库db2022+新建表t_user

   `docker exec -it mysql /bin/bash`

   `mysql -uroot -p123456`

   ```sql
   use db2022;
   
   CREATE TABLE `t_user` (
     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
     `username` varchar(50) NOT NULL DEFAULT '' COMMENT '用户名',
     `password` varchar(50) NOT NULL DEFAULT '' COMMENT '密码',
     `sex` tinyint(4) NOT NULL DEFAULT '0' COMMENT '性别 0=女 1=男 ',
     `deleted` tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '删除标志，默认0不删除，1删除',
     `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
     `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户表';
   ```

5. 测试通过

6. 关停：`docker-compose stop`

   ![image-20221209220335178](07-docker.assets/image-20221209220335178.png)



### 6 Docker轻量级可视化工具Portainer

> 简介

Portainer是一款轻量级的应用，它提供了图形化界面，用于方便地管理Docker环境，包括单机环境和集群环境。

Portainer分为开源社区版（CE版）和商用版（BE版/EE版）。

> 安装Portainer

官网：

- https://www.portainer.io/
- https://docs.portainer.io/start/install/server/docker/linux

步骤：

1. 创建数据卷

   ```bash
   docker volume create portainer_data
   
   docker volume ls
    
   docker volume inspect portainer_data
   ```

   ![image-20221209230816199](07-docker.assets/image-20221209230816199.png)

2. docker命令安装

   ```bash
   # 拉取镜像
   docker pull portainer/portainer-ce
   
   # 运行容器
   # --restart=always 如果Docker引擎重启了，那么这个容器实例也会在Docker引擎重启后重启，类似开机自启
   docker run -d -p 8000:8000 -p 9000:9000 \
   --name portainer --restart always --privileged=true \
   -v /var/run/docker.sock:/var/run/docker.sock \
   -v portainer_data:/data \
   portainer/portainer-ce
   ```

   ![image-20221209235321532](07-docker.assets/image-20221209235321532.png)

3. 第一次登录需创建admin，访问地址：`LinuxIP地址:9000`

   ![image-20221209223007682](07-docker.assets/image-20221209223007682.png)

4. 设置admin用户和密码后首次登陆

   用户名默认admin，密码8位：12345678

   ![image-20221209224150017](07-docker.assets/image-20221209224150017.png)

5. 选择local选项卡后本地docker详细信息展示

   ![image-20221210012459358](07-docker.assets/image-20221210012459358.png)

6. 上一步的图形展示，能想得起对应命令吗？

   `docker system df`

   ![image-20221209225042179](07-docker.assets/image-20221209225042179.png)

7. 安装Nginx

   添加新的容器

   ![image-20221210012741986](07-docker.assets/image-20221210012741986.png)

   设置容器：名字，映射端口，镜像

   ![image-20221210013037627](07-docker.assets/image-20221210013037627.png)

   ![image-20221210013253090](07-docker.assets/image-20221210013253090.png)

   ![image-20221210013419524](07-docker.assets/image-20221210013419524.png)

   ![image-20221210013517509](07-docker.assets/image-20221210013517509.png)



### 7 Docker容器监控之CAdvisor+InfluxDB+Granfana

#### 7.1 Before

原生命令

- 操作

  ```bash
  docker stats
  ```

  ![image-20221210014929944](07-docker.assets/image-20221210014929944.png)

- 问题

  通过`docker stats`命令可以很方便的看到当前宿主机上所有容器的CPU，内存以及网络流量等数据，一般小公司够用了。。。。

  但是，`docker stats`统计结果只能是当前宿主机的全部容器，数据资料是实时的，没有地方存储、没有健康指标过线预警等功能。



#### 7.2 容器监控3剑客简介

*CAdvisor*监控收集 + *InfluxDB*存储数据 + *Granfana*展示图表

1. CAdvisor

   CAdvisor是一个容器资源监控工具，包括容器的内存、CPU、网络IO、磁盘IO等监控，同时提供了一个Web页面用于查看容器的实时运行状态。

   CAdvisor默认存储2分钟的数据，而且只是针对单物理机。不过CAdvisor提供了很多数据集成接口，支持InfluxDB、Redis、Kafka、Elasticsearch等集成，可以加上对应配置将监控数据发往这些数据库存储起来。

   CAdvisor主要功能：

   - 展示Host和容器两个层次的监控数据
   - 展示历史变化数据

2. InfluxDB

   InfluxDB是用Go语言编写的一个开源分布式时序、事件和指标数据库，无需外部依赖。

   CAdvisor默认只在本机保存2分钟的数据，为了持久化存储数据和统一收集展示监控数据，需要将数据存储到InfluxDB中。InfluxDB是一个时序数据库，专门用于存储时序相关数据，很适合存储CAdvisor的数据。而且CAdvisor本身已经提供了InfluxDB的集成方法，在启动容器时指定配置即可。

   InfluxDB主要功能：

   - 基于时间序列，支持与时间有关的相关函数（如最大、最小、求和等）
   - 可度量性：可以实时对大量数据进行计算
   - 基于事件：支持任意的事件数据

3. Granfana

   Grafana是一个开源的数据监控分析可视化平台，支持多种数据源配置（支持的数据源包括InfluxDB、MySQL、Elasticsearch、OpenTSDB、Graphite等）和丰富的插件及模板功能，支持图表权限控制和报警。

   Granfana主要功能：

   - 灵活丰富的图形化选项
   - 可以混合多种风格
   - 支持白天和夜间模式
   - 多个数据源

![image-20221210015600643](07-docker.assets/image-20221210015600643.png)



#### 7.3 CIG安装部署

compose容器编排，一套带走

1. 新建目录：`mkdir /mydocker/cig`

   ![image-20221210015831051](07-docker.assets/image-20221210015831051.png)

2. 新建3件套组合的`docker-compose.yml`文件

   ```yaml
   version: '3.1'
    
   volumes:
     grafana_data: {}
    
   services:
    influxdb:
     image: tutum/influxdb:0.9
     restart: always
     environment:
       - PRE_CREATE_DB=cadvisor
     ports:
       - "8083:8083"
       - "8086:8086"
     volumes:
       - ./data/influxdb:/data
    
    cadvisor:
     image: google/cadvisor
     links:
       - influxdb:influxsrv
     command: -storage_driver=influxdb -storage_driver_db=cadvisor -storage_driver_host=influxsrv:8086
     restart: always
     ports:
       - "8080:8080"
     volumes:
       - /:/rootfs:ro
       - /var/run:/var/run:rw
       - /sys:/sys:ro
       - /var/lib/docker/:/var/lib/docker:ro
    
    grafana:
     user: "104"
     image: grafana/grafana
     user: "104"
     restart: always
     links:
       - influxdb:influxsrv
     ports:
       - "3000:3000"
     volumes:
       - grafana_data:/var/lib/grafana
     environment:
       - HTTP_USER=admin
       - HTTP_PASS=admin
       - INFLUXDB_HOST=influxsrv
       - INFLUXDB_PORT=8086
       - INFLUXDB_NAME=cadvisor
       - INFLUXDB_USER=root
       - INFLUXDB_PASS=root
   ```

3. 启动docker-compose文件

   ```bash
   docker-compose up -d
   ```

   ![image-20221210020804409](07-docker.assets/image-20221210020804409.png)

4. 查看三个服务容器是否启动

   ![image-20221210020836499](07-docker.assets/image-20221210020836499.png)



#### 7.4 测试

1. 浏览cAdvisor*收集*服务，http://ip:8080/

   第一次访问慢，请稍等。

   cadvisor也有基础的图形展现功能，这里主要用它来作数据采集

   ![image-20221210021344852](07-docker.assets/image-20221210021344852.png)

2. 浏览influxdb*存储*服务，http://ip:8083/

   ![image-20221210021451904](07-docker.assets/image-20221210021451904.png)

3. 浏览grafana*展现*服务，http://ip:3000

   ip+3000端口的方式访问,默认帐户密码（admin/admin）(新密码：12345678)

   ![image-20221210021647339](07-docker.assets/image-20221210021647339.png)

   > 配置步骤

   - 配置数据源

     ![image-20221210023704781](07-docker.assets/image-20221210023704781.png)

   - 选择influxdb数据源

     ![image-20221210023712768](07-docker.assets/image-20221210023712768.png)

   - 配置细节

     ![image-20221210023749609](07-docker.assets/image-20221210023749609.png)

     ![image-20221210023802513](07-docker.assets/image-20221210023802513.png)

     ![image-20221210023808442](07-docker.assets/image-20221210023808442.png)

   - 配置面板panel

     ![image-20221210023835546](07-docker.assets/image-20221210023835546.png)

     ![image-20221210023844186](07-docker.assets/image-20221210023844186.png)

     ![image-20221210023850277](07-docker.assets/image-20221210023850277.png)

     ![image-20221210023859897](07-docker.assets/image-20221210023859897.png)

     ![image-20221210023910584](07-docker.assets/image-20221210023910584.png)

     ![image-20221210023919511](07-docker.assets/image-20221210023919511.png)

4. 到这里cAdvisor+InfluxDB+Grafana容器监控系统就部署完成了
