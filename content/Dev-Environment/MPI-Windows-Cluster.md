---
title: MPI Windows集群环境搭建
layout: page
date: 2016-08-04 16:50
---

## 1 .Net FrameWork 3.5 安装 ##

Windows8及以上系统，默认不安装.Net FrameWork 3.5 and 2.0，而MPICH2需要.Net FrameWork 2.0，由于服务器没有联网，因此无法在线安装，可以通过系统自带的dism提取系统镜像中的内容；

1.系统镜像如下所示，

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815152508281-257861931.png)

2.以管理员身份运行cmd，在shell中输入以下内容，注意修改source即可，本机为E盘；

    dism.exe /online /enable-feature /featurename:NetFX3 /Source:E:\sources\sxs

上述命令执行后可能会失败，如图所示，

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815152731281-868017276.png)

使用/enable-feature /all即可，

    dism.exe /online /enable-feature /all /featurename:NetFX3 /Source:E:\sources\sxs

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815152736046-1142029200.png)

.Net FrameWork 3.5 and 2.0安装成功；

Win + Q中搜索"启用或关闭Windows功能"，然后在"功能"中可以看见.Net FrameWork 3.5功能已经安装ok，其中包括.Net 2.0和3.0；

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815153126921-1596167345.png)

## 2 安装MPICH ##

在每台机器上安装好mpi的安装包，安装的路径任意选择，安装过程中在使用者那选择Everyone，将MPICH2的bin目录添加进系统环境变量Path中；

检查计算机的服务里是否有MPICH2 Process Manager,Argonne National Lab这项服务，具体步骤：右击任务栏->启动任务管理器->点到服务那一列->点击右下角服务按钮，就会出现服务列表，从上面找看看有没有这个服务，如果没有则运行下面的命令；

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815153825000-1374736988.png)

Win + Q搜索"cmd"，然后以管理员身份运行cmd，进入MPICH2的bin目录，在shell中输入，

    smpd -install -phrase behappy(为安装时输入的phrase)

## 3 添加用户账户 ##

为了使计算机集群能使用MPI，应该给每个安装了MPI的计算机添加一个相同的账户，账户密码也必须相同，账户要求是管理员身份。这里的账户为mpi，密码可以随便设置；

## 4 注册账户 ##

运行wmpiregister.exe
，将mpi账户和密码添加进去，注册，然后点击确定；

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815154406265-687613405.png)

## 5 配置 ##

测试前需要在每台计算机上常见一个路径相同的文件下，比如在F盘根目录下创建一个mpi的文件夹，将编译成功需要执行的.exe文件放入该目录；

运行wmpiconfig.exe，注意将各计算机的防火墙关掉，个计算机在同一个项目组内，这里我使用的计算机都是内网机器，都在WORKGROUP工作组内；

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815154942531-874358996.png)


## 6 测试 ##

**1.单机单核版**

在本机直接运行cpi.exe，输入一个比较大的数值，80000000，如下

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815155206765-1334655303.png)

**2.单机多核版**

运行wmpiexec.exe，处理器选择2,然后执行，如下，

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815155548843-543875995.png)

执行结果如下，

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815155554250-1408894194.png)

**3.集群版**

在cmd中输入如下命令，

    mpiexec -hosts 3 ip1 ip2 ip3 F:/mpi/cpi.exe

执行结果如下，2个计算机节点，可以看到与使用双核的效果差不多；

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815155938375-953737032.png)

使用3个计算机节点，使用的时间是单机单核的1/3，可以说集群中计算机节点越多，处理越快，效率几乎是线性的；

![](http://images2015.cnblogs.com/blog/668850/201608/668850-20160815155944093-98642656.png)


## 7 参考链接 ##

1.[Windows下MPI的环境搭建及机群测试](http://www.cnblogs.com/Romi/archive/2012/05/28/2522401.html)

2.[Win8/8.1 下映像管理和恢复环境的配置](http://www.cnblogs.com/jcf94/p/4157994.html)

3.[Windows 8系统默认开启的.Net Framework版本是4.0，而部分用户可能需要使用到3.5或以下版本，简单添加方法](http://www.cnblogs.com/taoweiji/p/3659892.html)

4.[windows系统lammps安装MPICH2的问题](http://blog.sina.cn/dpool/blog/s/blog_8af243c30101lnma.html)

5.[Win7/8下提示OpenSCManager failed 拒绝访问](http://jingyan.baidu.com/article/f0062228dc4fa6fbd3f0c80f.html)
