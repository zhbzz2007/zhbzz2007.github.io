---
title: tf配置
layout: page
date: 2016-06-01 23:29
---

本博客主要用于在Ubuntu14.04 64bit 操作系统上搭建google开源的深度学习框架tensorflow。

## 0.安装CUDA和cuDNN ##

如果要安装GPU版本的tensorflow，就必须先安装CUDA和cuDNN，请参考[Caffe学习笔记2--Ubuntu 14.04 64bit 安装Caffe（GPU版本）](http://www.cnblogs.com/zhbzz2007/p/5499180.html)。

## 1.安装tensorflow ##

github上下载已经编译好的.whl文件。

![](http://images2015.cnblogs.com/blog/668850/201605/668850-20160528220012022-1342690038.png)

输入如下，

    sudo pip install tensorflow-0.8.0-cp27-none-linux_x86_64.whl

即可自动安装。

安装完成后，进入python，输入，

    import tensorflow

显示如下，表明GPU版本的tensorflow安装成功，版本微0.8.0。

![](http://images2015.cnblogs.com/blog/668850/201605/668850-20160528220441006-1023444395.png)
