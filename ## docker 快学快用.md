## docker  快学





### 第一印象

一个容器

不是传统的硬件虚拟化，直接运行在宿主机内核上。



Docker 由**镜像**(Image)、**容器**(Container)、**仓库**(Repository) 三部分组成。

docker的镜像就像是它的文件系统

用dockerfile文件文件来构建镜像，可以让镜像具备可重复性，同时保证启动脚本和运行程序的标准化。、

编写好文件之后，通过docker build命令进行构建



DevOps。开发/运维, 开发自己运维

 瀑布-> 敏捷开发



### 为什么要学docker

云计算时代，docker,go。5G时代





### 镜像

理解，类似操作系统的启动文件，镜像都是静态文件，构建后不再改变。分层存储架构，每一层都是在下面层次的基础上构建。

类比为模板，类。容器就是实例对象。容器是用镜像创建的运行实例

可以把容器看作是一个简易版的linux环境和运行在其中的应用程序，容器的最上面一层是可读可写的

仓库，放了一堆镜像的地方，类比github 



### 容器

镜像（`Image`）和容器（`Container`）的关系，就像是面向对象程序设计中的 `类` 和 `实例` 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化

直接对宿主（或网络存储）发生读写，其性能和稳定性更高

### 仓库

需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，[Docker Registry]() 就是这样的服务





### 方向:

区块链  go语言 

Docker  Go   Swarm/Componse/Machine/mesos/k8s ---CI/CD jenkins整合



docker。集群

打破过去程序及应用的观念，透过镜像将作业系统核心除外的运行程序所需的系统的环境，**由上而下**打包，达到应用程序式跨平台间的无缝衔接。一次构建处处运行

解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术





docker的优点：基于容器的虚拟化，仅包含业务运行所需的runtime环境，centos基础镜像仅170M，宿主机可部署100-1000个容器。分层的存储和包管理，devops理念





### **安装**

yum  install -y  epel-release 

-y自动选择，不用再进行一次确认，全自动

安装  docker-io

/etc/docker/daemon.json 修改配置来使用阿里云加速器

检查是否生效

ps -ef | grep docker

e内容   f全格式

docker run imgxx  本地没有会去hub上找一个镜像



### 工作原理

 docker是一个client-server 结构的系统，docker守护进程运行在主机上，然后通过socket链接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。容器是一个运行时环境，就是我们前面说的集装箱

快：更少的抽象层，跟操作系统内核直接通信，不需要和向虚拟机一样重新加载一个操作系统内核。和宿主机共享cpu





### linux

useradd yaoyue  创建用户  

passwd yaoyue 

groupadd docker 创建docker组

usermod -aG docker yaoyue    把用户加入到docker组





### 镜像加速

[阿里云加速器(点击管理控制台 -> 登录账号(淘宝账号) -> 右侧镜像中心 -> 镜像加速器 -> 复制地址)](https://www.aliyun.com/product/acr?source=5176.11533457&userCode=8lx5zmtu)

查看配置是否生效： docker info

> 或者用  ps -ef | grep docker 可以查看得到

服务命令  systemctl   （control系统控制)

1. 在/etc/docker/daemon.json  写入镜像
2. systemctl daemon-reload  重载配置
3. systemctl restart docker 重启doceker服务



### docker run 干了什么

客户端会去访问 docker_host ，其中的docker daemon 会去找镜像生成容器实例，如果镜像没有就会去仓库里拉

daemon 守护进程。 英文意思： 古希腊神话中的半神半人精灵 ，有个虚拟光驱工具也叫这个名

虚拟机的  Hypervisor+Guest OS  ==>  替换成Docker Engine



### 理解docker

鲸鱼背上有集装箱

蓝色的大海  -----  宿主机系统

鲸鱼   ----docker

集装箱    -----容器实例    form  镜像模板

镜像千层饼， 里面一层套一层

可以把容器看作一个精简版的linux环境



### 常用命令

docker info 

docker version 

docker --help

**镜像**

docker images          列出本地镜像的 

>  options 说明     -a 列出所有，包括中间印象层 。  -q只显示镜像id    --digests摘要    --no-trunc：显示完整的镜像信息

docker search  镜像的名字

>  options 说明   --no-trunc   -s收藏数   --automated：只列出antomated build类型的镜像

docker pull 镜像的名字：版本号

docker  rmi 某个镜像名字ID   *进行镜像删除*    -f强制删除，不确认

>  删除全部   docker rmi -f  $(docker images -qa) 
>
> ---$(docker images -qa) 插出全部的id,然后进行删除

**容器**

1. 新建并启动容器    

   > docker run [opt] IMAGE [command] [args...]
   >
   > options 说明：  
   >
   > --name 容器指定名称   
   >
   > -d后台运行容器,即启动守护式容器
   >
   > -i 以交互模式运行容器，通常与-t同时使用    -t 为容器重新分配一个伪输入终端     -it 就进入了docker运行的容器, 比如run centos 就像是登录进去   exit  退出
   >
   > -P 随端口映射
   >
   > -p: 指定端口映射  4种形式   ip:hostPort:containerPort ip::containerPort **hostPort:containerPort** containerPort   

2. 列出当前所有正在运行的容器

   >  docker ps [opt]
   >
   > options说明：  
   >
   > ​	-l   上一个运行的容器
   >
   > ​	-n   上3次
   >
   >  	-q  quite 静默形式，  只显示容器编号
   >
   > ​	-no-trunc

3. 退出容器

   exit 容器停止退出 <br/>

   ctrl + p + q 容器不停止退出

4. 启动容器

   docker start

5. 重启容器

   docker restart

6. 停止容器

   docker stop   容器慢慢停止

7. 强制停止容器

   docker kill   id           拔电源

8. 删除已停止的容器

   docker rm id 

   *docker rmi 是删除镜像的* 

   一次性删除多个容器    docker rm -f  $(docker ps -a -q)

9. 重要

   **以后台模式启动一个容器**，启动守护式容器

   docker run -d centos 

   docker容器后台运行，必须有一个前台进程，容器运行的命令如果部署那些一直挂起的命令，会自动自杀。

   方法让有一个前台进程运行 加上 /bin/bash后缀  

   后面的/bin/bash的作用是表示载入容器后运行bash，*表示启动容器后启动bash*

   **查看容器日志**

   docker logs -f -t --tail 容器ID    

   >  options :   -t 加入时间戳  -f跟随最新  --tail数   显示最后多少条
   >
   >  docker run -d centos /bin/sh -c "while true;do echo hello zzyy;sleep 2; done"

   **查看容器内运行的进程**

   docker top 容器 id

   **查看容器内部细节**  

   docker inspect id

   **进入正在运行的容器 并以交互模式**

   docker attch -it 容器id   直接进入容器启动命令的终端，不会启动新的进程

   docker exec  -it 是在容器中打开新的终端，并且可以启动新的进程，更强大一些

   **从容器内拷贝文件到主机上**

    docker cp  容器id:/对应文件目录    主机文件目录

   还有别的命令可以去搜索常用命令加强学习



### docker 镜像

#### 是什么

1. UnionFS联合文件系统

   > 支持对文件系统的修改作为一次次的提交来一层层叠加，可以将不同目录挂载到同一个虚拟文件系统下
   >
   > Union文件系统是Docker镜像的基础，镜像可以通过分层来进行继承，基于基础镜像，可以制作出各种具体的应用镜像
   >
   > **特性**： 一次加载多个文件系统，但从外面看来只能看到一个文件系统，联合加载会把各层文件系统叠加起来，最终的文件系统会包含所有底层的文件和目录
   >
   > 多层，洋葱？

2. docker镜像加载原理

   bootfs  bootloader + kernel  bootloader引导加载kernel刚启动时会加载bootfs文件系统。

   镜像的最底层就是bootfs，rootfs在bootfs之上，就是各种不同的操作系统发行版

   > 为什么docker中的centos那么小，因为对于一个精简的os，rootfs可以很小，只需要包括最基本的命令，工具和程序库，底层直接用Host的kernel。

3. 分层的镜像

4. 为什么docker镜像要采用这种分层结构呢

   共享资源，如果多个镜像都从相同的base镜像构建而来，宿主机上只需要保存一份base镜像，同时内存中也只需要加载一份base镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享

#### 特点

都是只读的

当容器启动时，一个新的可写层被加载到镜像的顶部，这一层通常被称为容器层，容器层之下的都叫镜像层

#### Docker镜像commit操作补充

docker commit  提交容器副本使之成为一个新的镜像

docker commit -m ="提交描述的信息" -a="作者"  容器id  命名空间/要创建的目标镜像名：[标签名]

 docker run -it -p **8888:8080** (解释：前面的是主机端口号，后面的是里面的服务端口号，也就是dockers容器端口号)

-P  随机分配端口





### 容器数据卷

v    volume卷  让宿主机和docker能够共享数据，进行一定通信

**是什么**

​	有点类似Redis里面的rdb和aof文件

**能干吗**

​	做数据**持久化**

​	容器之间希望有可能**共享**数据 ，继承数据

**数据卷**

​	特点:  卷中的更改不会包含在镜像的更新中，数据卷的生命周期也一直持续到没有容器使用它为止

​	容器内添加： 

  1. 直接命令添加 （需要容器内对应目录）

     > docker run -it -v  /宿主机绝对路径目录:/容器内目录  镜像名
     >
     > 容器停止退出后，主机修改后数据是否同步： 完全同步
     >
     > 命令带权限：(ro   readonly)
     >
     > docker run -it -v  /宿主机绝对路径目录:/容器内目录:ro  镜像名   

  2. dockerFile添加

     docker cp 从容器内拷贝文件到主机

     dockerfile是什么     

     类比javeEE   hello.java  --->  hello.class

     ​		Docker images ===>   DockerFile(一种描述)

     小例子： 

      1. 根目录下新建mydocker文件夹并进入

      2. 可在Dockerfile中使用VOLUME指令来给镜像添加一个或多个数据卷

         > VOLUME ["/目录1","/目录2"]

      3. File构建

     ```
     FROM centos 
     VOLUME ["/目录1","/目录2"]
     CMD echo "finished, ----success1"
     CMD /bin/bash
     ```

     4. build后生成镜像    build -f ，执行命令生成镜像.获得一个自定义的镜像

     5. run 容器

     Docker挂载主机目录Docker访问出现cannot open directory : Permission denied 

     解决办法: 在挂载目录后多加一个--privileged=true参数

**数据卷容器**

 1. 是什么

    名名的容器挂载数据卷，其他容器通过挂载这个父容器实现数据共享，挂载数据卷的容器，称之为数据卷容器

	2. 总体介绍

	3. 容器间传递共享

​	结论： 容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止





### dockerfile 重点

#### 是什么

dockerfile是用来构建 docker镜像的构建文件，是由一系列命令和参数构成的脚本

构建三步骤，   编写dockerfile   ->      docker build   ->  docker run

文件是怎么样的。

```
FROM scratch //基础镜像
MAINTAINER   //作者和邮箱

```



#### dockerfile构建过程解析

手动编写一个Dockerfile文件，然后docker build执行，获得一个自定义的镜像

​	**内容基础知识**： 

	1. 没体哦啊保留字指令都必须为大写字母。且后面需要跟随参数
	2. 指令从上到下顺序执行
	3. #表示注释
	4. 每条指令都会创建一个新的镜像层，并对镜像进行提交

**大致流程**

	1. 从基础镜像运行一个容器，
	2. 执行一条指令对容器进行修改
	3. 执行类似docker commit的操作提交一个新的镜像层
	4. docker再基于刚提交的镜像运行一个新容器
	5. 执行dockerfile中的下一条指令知道所有指令都执行完成

#### DockderFile体系结构 

1. FROM        基础镜像 （base镜像：scratch）

2. MAINTAINER     镜像维护者的姓名和邮箱

3. RUN              容器构建时需要运行的命令

4. EXPOSE       当前容器对外暴露的端口号

5. WORKDIR     知道在创建容器后，终端默认登录的进行工作目录，一个落脚点

6. ENV    用来构建镜像过程中设置环境变量，类似于全局变量

7. ADD    拷贝加解压，add命令会自动处理url和解压tar压缩包

8. COPY      将从构建上下文目录中源路径文件/目录复制到新的一层的镜像内的目录路径位置

9. VOLUME

10. CMD     

    1. 指定一个容器启动时要运行的命令，dockerfile中可以有多个cmd命令，但只有最后一个生效
    2. cmd会被docker run 之后的参数替换    比如   docker  run -it  xxx    **ls -l**  会覆盖掉dockerfile里面的cmd

11. ENTEYPOINT  和cmd的功能一样，但是是每一个都会生效。不是替换而是追加

12. ONBUILD   当构建一个被继承的dockerfile时运行命令，父镜像在被子继承后父镜像的onbuild会被触发

    

#### 案例

**案例1**

```
FROM  centos    //继承本地的centos
MAINTAINER yaoyue<2316570512@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim 
RUN yum -y install net-tools

EXPOSE 8000

CMD echo $MYPATH
CMD echo "SUCCESS ---------OK"
CMD /bin/bash
```

构建：   docker build -f /mydocker/Dockerfile2   -t 新镜像名:tag  .            最后面的.表示当前目录

docker history 镜像名  ：列出镜像变更历史

**案例2** 

```
FROM centos 
RUN yum intsall -y curl 
ENTEYPOINT ["curl","-s","http://ip.cn"]
CMD -i
```

**案例3**

```
ONBUILD RUN echo "father onbuild  ----- 666"   //如果是被继承的话父镜像的这个就会被运行

需要子Dockerfile
FROM father_images
```

**案例4**

```
FROM  centos 
MAINTAINER yaoyue<2316570512@qq.com>
#把宿主机当前上下文的c.txt拷贝到容器usr/local路径下
#把java与tomcat添加到容器中
#安装vim编辑器
#设置工作访问时候的WORKDIR路径，登录落脚点
#配置java与tomcat环境变量
#容器运行时监听的端口
#启动时运行tomcat
COPY c.txt /usr/local/cincontainer.txt
ADD jdk-8u171-linux-x64.tar.gz /usr/loacl/
ADD apache-tomcat-9.0.8.tar.gz /usr/local/
RUN yum -y install vim 
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_HOME /..
ENV CLASSPATH /...
ENV CATALINA_HOME /..
ENV CATALINA_BASE /..
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/LIB:$CATALINA_HOME/bin
EXPOSE 8080
# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.8/bin/startup/sh"]
# CMD ["/usr/local/apache-tomcat-9.0.8/bin/catalina.sh, "run"]
CMD /usr/local/apache-tomcat-9.0.8/bin/startup/sh && tail -F /usr/local/apache-tomcat-9.0.8/bin/logs catalina.out
```

构建  =》 run =》 验证 =》 结合前述的容器卷将测试的web服务test发布



#### 小总结





### 镜像推送到阿里云





### 疑问与解决

勿在浮沙上筑高台

--daemon=false 开起后台运行

**交互式容器，守护式容器**

>  交互 -it    后台-d    
>
> i交互  t终端

**top 命令** 用来监控linux的系统状况，是常用的性能分析工具，能够实时显示系统中各进程的资源占用情况

**ps命令**  process status的缩写，列出系统中正在运行的那些进程，执行ps命令时进程的快照

**kernel 是什么**  翻译：坚果仁，核心要点，核心，中心要点

**curl  命令**   命令行工具发出请求，如果用得好可以取代postman这种图形化工具



