## linux 快学快用





### 目录

在linux世界里面，一切都是文件

映射成文件，用文件来管理

![image-20210626154829843](C:\Users\exbowen\AppData\Roaming\Typora\typora-user-images\image-20210626154829843.png)

重点目录，存放什么内容，要有一个认识

**/bin**  binary的缩写，存放最经常使用的命令

**/home** 普通用户的主目录

**/root**管理员的用户主目录

**/boot** 存放的是启动linux时使用的一些核心文件，包括一些链接文件及镜像文件

**/media** 设备

**/mnt** 外部存储挂载

/usr/local 主机额外安装软件所安装的目录

/var 日志文件和经常被修改的目录放在这个目录下





### vim

3种模式。正常模式，插入模式，命令行模式

正常： 使用快捷键键，在其他模式按esc跳回正常模式

插入模式。i

命令模式，保存退出等  ,按esc   然后  :wq  及输入命令

查找：  : set nu    20    shrift + g

yy 复制    p粘贴

dd 删除



### 基本操作命令

reboot 重启

shutdown 关机    -r重启

syn  把内存数据同步到磁盘

**用户登录注销**

​		logout 注销

​		用户组，家目录概念  /home有各个用户对应的家目录

​		用组管理权限

useradd  [op]  name

userdel 

groupadd 

usermod -g shaolin xxx

用户登录会自动到对应的家目录

pwd 当前路径

切换用户  su  -xx

exit  返回到原来的用户

whoami  查看当前用户

查看用户信息  id  xxx

**etc/passwd** 用户的配置文件，记录用户的各种信息

etc/shadow 口令的配置文件

etc/group 组的信息



### 实用指令

运行级别：  /etc/inittab   initdefault:

 	0 .关机

​	1 .单用户，找回密码

2. 多用户无网络
3. 多用户有网络
4. 保留
5. 图形界面
6. 重启

找回密码：  开机引导的时候输入回车键  看到界面输入e  e  1 b 进入单用户模式修改密码

**帮助指令**

man ls  查看ls的用法和一些配置

help 

**文件目录类**

pwd 

ls 目录或文件    -a显示隐藏的  -l   以列表的方式显示   -lh  human 以人友好阅读的方式来演示

cd 切换目录 

>  cd ~ 回到家目录
>
> cd ..  回到当前目录的上一级目录 
>
> 相对路径和绝对路径

mkdir 创建目录  一次性创建多级目录需要带上-p

redir -rf  强制递归删除   rm-rf   强制删除

touch  创建空文件

**cp指令**   拷贝  -r递归        cp [op]  源文件   dest目录   强制覆盖不提醒   \cp

rm  移除文件或目录

mv  移动文件或目录   还能重命名（当前目录移动）

cat   只读  -n显示行号，常和more结合使用

more    | 管道符做链接     more做分页 

less  和more功能类似，对于大型文件更友好

分页看空格键   enter键一行一行的

**输出重定向和追加**

\> 输出重定向

\>\> 追加

- ls -l > 文件    列表的内容写入到文件中，覆盖
- ls -al >>         追加
- cat  文件1  >  文件2   覆盖
- echo “内容” >> 文件

echo  输出内容到控制台     echo $PATH

head   显示文件的开头部分

tail   输出文件中尾部的内容

>   tail -f   监控文档的所有更新，  工作中很有用

ln 指令， 软链接指令，也叫符号链接     

ln -s  目标文件   软链接

history 查看已经执行过的历史命令，可以查看前同事执行的历史命令来进行学习。你的执行也会被记录起来

- ！号码   可以执行对应编号的命令

clear  清屏

**时间日期类**

date  "+%Y年 %m %d  %H:%M:%S"S

date -s 设置系统当前时间

cal  查看日历指令   cal 2020

**搜索查询类**

find   [范围] [选项]              

> find /home -name  名字     *.txt  也支持通配符
>
> -user  按拥有者来查询
>
> -size  按大小来   +20M 及大于20M的文件

locate  快速  需要用updatedb创建 locate数据库

grep字符和管道符号 | 将前一个命令的处理结果传递给后面的命令处理

grep [-n -i忽略大小写]  查找内容  源文件      常常前面是cat  |  grep 查找

**压缩和解压缩**

gzip gunzip 后面用于解压

当我们使用gzip对文件进行压缩后不会保留源文件

zip unzip   -d  指定解压后文件的存放目录

tar 打包 .tar.gz后缀

-z  打包同时进行压缩	  -zcvf  打包后的文件名   【对那些文件进行打包】

-zxvf   解压   指定解压到某个目录   -C

### 权限

所有者，所在组，其他组

文件的创建者为所有者

ls -ahl 查看文件的所有者

**改变文件的所有者**   chown  用户名 文件名 （change own）

文件所有者、文件所在组

**修改文件所在组**   chgrp (change group)

其他组

**改变用户所在组** 

usermod -g 组名  用户名

usermod -d 目录名 用户名  改变改用户登录的起始目录



文件类型：  -普通文件  d 目录  l 软链接  c字符设备  b块文件，硬盘

rw-r 表示文件所有者权限rw 

r--文件所在组的权限 

r-- 文件的其他组的用户的权限

1 如果是文件表示硬链接数，如果是目录表示子目个数  

最后的的数字显示文件大小

时间最后为文件的修改时间



rwx权限，作用于文件和目录上是有区别的 

w对于文件表示可以修改，删除一个文件的前提是对改文件所在目录有写权限才能删除该目录。

w对于目录则为 创建+删除+重命名

x代表可执行，如果是目录就代表是否可以进去

rwx还可以用数字来表示， rwx 4+2+1=7

.当前目录  ..上一目录



**权限管理   chmod**

可以修改文件或者目录的权限

u g o     a（所有人）   字母方式   u=rwx  g=rw

chmod   751 目录文件名

chown -R tom kkk/ 将kkk目录下的所有文件和目录的所有者

passwd  name 对用户密码进行设置

rx目录才能进去



**crond任务调度**

crontab

-e 编辑 -l查询  -r删除

指系统在某个时间执行的特定的命令或程序

定时的调度我们的脚本或代码

*** 占位符，一些特殊符号的描述 ， “，”连续执行

相关命令  crontab -r  -l  service crond restart

步骤：   

	1. cron -e
 	2. */1 * * * * ls -k /etc >> /tmp/to.txt 表示每一分钟执行一次

几个应用实例：

​	先编写一个文件，然后再给它执行权限，然后再编辑添加进去



**实操linux磁盘分区，挂载**

mbr 

gtp 容量和数量

分区原理 

​	硬盘分区挂载到文件系统中。 mount umount

lsblk -f  查看分区               挂载点





**动态监控进程**

top.  top执行一段时间可以更新正在运行的进程

-d 秒数

-i 不显示任何闲置或僵死进程

-p 指定进程id来监控



**查看系统网络情况**

netstat [选项]



### shell脚本 

.sh后缀

大数据程序员，需要编写shell程序来管理集群

shell 是一个命令行解释器，它为用户提供了一个向Linux内和发送请求以便运行程序的边界级程序，用户可以通过用Shell来启动、挂起、停止甚至是编写一些程序。

#!/bin/bash  开头

常用执行方法。

1. 首先要赋予脚本x权限。

2. 执行脚本

shell 变量   系统变量和用户自定义变量   

```
RESULT = `ls -l /home`   也可以是$()语法
echo  $RESULT   使用，也可以说是引用变量
```

unset     撤销变量

**设置环境变量**   

export  变量名=值

source 配置文件  让修改后的配置信息立即生效

echo $变量名  查询环境变量的值

一般是在 /etc/profile里面定义环境变量

实践，写一个脚本 定时自动备份数据库





### 专题：RPM  & YUM

RPM  RedHat Package Manager 红帽软件包管理工具

rpm -qa 查询所有安装的rpm包

rpm -q 包名：   查询是否安装

rpm -qi    查询软件包信息

rpm  -qf   查询文件所属的软件包

rpm -e 卸载

rpm -ivh 包全路径名称    install  verbose 提示  hash进度条



**yum**  是一个 shell前端软件包管理器，基于rpm.能够从指定的服务器自动下载rpm包并且安装能够自动处理依赖，并且一次安装所有依赖的包

前提是要联网。

1. 查询是否有  yum list | grep  xx
2. yum install xxx

### 

