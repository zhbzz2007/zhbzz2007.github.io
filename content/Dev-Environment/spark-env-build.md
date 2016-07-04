---
title: Ubuntu 14.04 64bit搭建spark单机版
layout: page
date: 2016-06-22 23:04
---

主要用来记录Ubuntu 14.04 64bit操作系统下单机版本spark环境搭建。

## 1.安装java ##

下载Linux版本的JDK压缩包，我下载的是jdk-7u79-linux-x64.tar.gz。

输入如下命令：

    chmod 0755 jdk-7u79-linux-x64.tar.gz#修改jdk压缩包的权限
    tar -xvf jdk-7u79-linux-x64.tar.gz#解压缩
    sudo mkdir /usr/lib/jvm#在/usr/lib下新建jvm目录
    sudo cp -rf jdk-7u79-linux-x64 /usr/lib/jvm#将jdk文件拷贝到jvm目录

    sudo gedit ~/.profile

在文件末尾加上，

    export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_79/

保存关闭后，输入，

    source ~/.profile

使用env命令查看JAVA_HOME的值，

    env

如果JAVA_HOME=/usr/lib/jvm/jdk1.7.0_79/，说明配置成功。

将系统默认的jdk修改过来，

    sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_79//bin/java 300

    sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_79//bin/javac 300

    sudo update-alternatives --config java
    sudo update-alternatives --config javac

输入java -version，看到如下信息，则说明安装了sun的jdk。

	java version "1.7.0_79"
	Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
	Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)


## 2.安装scala ##

下载地址：http://scala-lang.org/download/

选择最新的版本下载，我下载的是scala-2.11.8.tgz，

    mv scala-2.11.8.tgz ~/install
    chmod 0755 scala-2.11.8.tgz
    tar -xvf scala-2.11.8.tgz

配置环境，

    sudo gedit ~/.profile

添加如下配置，

    export SCALA_HOME=$HOME/install/scala-2.11.8
    export PATH=$PATH:SCALA_HOME/bin:$HOME/bin

然后输入scala，进入scala命令行说明scala安装正确，如下图所示。

![](http://i.imgur.com/6K9UQq0.png)

## 3.安装spark ##

下载地址：http://spark.apache.org/downloads.html

选择最新的版本下载，我下载的是spark-1.6.1-bin-hadoop2.6.tgz，

    mv spark-1.6.1-bin-hadoop2.6.tgz ~/install
    chmod 0755 spark-1.6.1-bin-hadoop2.6.tgz
    tar -xvf spark-1.6.1-bin-hadoop2.6.tgz

配置环境，

    sudo gedit ~/.profile

添加如下配置，

    export SPARK_HOME=$HOME/install/spark-1.6.1-bin-hadoop2.6


**运行spark shell**

    cd  ~/install/spark-1.6.1-bin-hadoop2.6/conf
    cp spark-env.sh.template spark-env.sh
    sudo gedit spark-env.sh

添加如下配置：

    export SPARK_MASTER_IP=127.0.0.1
    export SPARK_LOCAL_IP=127.0.0.1

启动spark shell，

    cd ~/install/spark-1.6.1-bin-hadoop2.6/bin
    ./spark-shell#进入scala命令行
    ./pyspark#进入python命令行

就会看到spark启动的过程，如下图所示。

![](http://i.imgur.com/UbSmSyB.jpg)

##4.运行IPYTHON##

**启动IPython** , 在SPARK主目录下运行如下命令,

    IPYTHON=1 IPYTHON_OPTS="-pylab" ./bin/pyspark

如图所示，

![](http://images2015.cnblogs.com/blog/668850/201607/668850-20160703213345343-2121532560.png)

**启动IPython Notebook** ,在SPARK主目录下运行如下命令，

    IPYTHON=1 IPYTHON_OPTS=notebook ./bin/pyspark

如图所示，

![](http://images2015.cnblogs.com/blog/668850/201607/668850-20160703213351484-2007425375.png)


## 5.参考链接 ##

[Ubuntu 12.04 中安装和配置 Java JDK](http://www.cnblogs.com/bluestorm/archive/2012/05/10/2493592.html)

《深入理解Spark-核心思想与源码剖析》耿嘉安著。
