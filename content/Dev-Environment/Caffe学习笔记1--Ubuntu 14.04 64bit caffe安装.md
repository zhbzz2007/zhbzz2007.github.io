---
title: Caffe学习笔记1--Ubuntu 14.04 64bit caffe安装
layout: page
date: 2016-06-02 23:02
---

本篇博客主要用于记录Ubuntu 14.04 64bit操作系统搭建caffe环境，目前针对的的是CPU版本；

1.安装依赖库

    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
    sudo apt-get install --no-install-recommends libboost-all-dev

CUDA：如果仅仅是cpu安装，忽略这一步；

BLAS：安装ATLAS

    sudo apt-get install libatlas-base-dev

Python：可选

2.安装python依赖的依赖库：

安装gfortran,后面编译过程中会用到

    sudo apt-get install gfortran

安装blas,Ubuntu下对应的是libopenblas，其它操作系统可能需要安装其它版本的blas——这是个OS相关的。

    sudo apt-get install libopenblas-dev

安装lapack，Ubuntu下对应的是liblapack-dev，和OS相关。

    sudo apt-get install liblapack-dev

安装atlas，Ubuntu下对应的是libatlas-base-dev，和OS相关。

    sudo apt-get install libatlas-base-dev

安装pip

    sudo apt-get install python-pip
    sudo apt-get install python-dev
    sudo apt-get install python-nose
    sudo apt-get install g++
    sudo apt-get install git

3.Ubuntu14.04安装相关依赖库

    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

4.通过git，拉取源码

    git clone https://github.com/BVLC/caffe.git

5.安装python依赖

    cd caffe/python
    for req in $(cat requirements.txt); do sudo pip install $req; done

6.编辑caffe所需的Makefile文件

    cd caffe
    cp Makefile.config.example Makefile.config
    vim Makefile.config

主要修改这两处

    #去掉注释
    CPU_ONLY := 1

    BLAS_INCLUDE := /usr/include/atlas/

7.编译caffe

    make -j4
测试编译结果：

    make test
    make runtest

测试结果如下：

![](http://i.imgur.com/oEwskUe.png)

8.配置python

    # 编译pycaffe
    make pycaffe -j4
    make distribute

进入python后，直接import caffe会失败，

![](http://i.imgur.com/3NdsB5k.png)

在.bashrc中配置，

    sudo gedit ~/.bashrc
    # 我的caffe/python目录 /home/zhb/Desktop/Work/caffe/python
    export PYTHONPATH=$PYTHONPATH:/your/path/caffe/python
    source ~/.bashrc

![](http://i.imgur.com/sG2jIcL.png)

9.mnist数据测试

    cd caffe

    #下载mnist数据
    sh data/mnist/get_mnist.sh

    sh examples/mnist/create_mnist.sh

    # 修改 solver_mode 为 CPU
    vim examples/mnist/lenet_solver.prototxt

    ./examples/mnist/train_lenet.sh

执行结果如下:

![](http://i.imgur.com/XRilUTR.png)

10.参考链接

[Ubuntu Installation](http://caffe.berkeleyvision.org/install_apt.html)

[linux(ubuntu)下的caffe编译安装](https://www.zybuluo.com/hanxiaoyang/note/364737)

[Ubuntu-安装-theano+caffe-超详细教程](http://www.cnblogs.com/anyview/p/5025704.html)

[Caffe + Ubuntu 14.04 64bit + CUDA 6.5 配置说明](http://www.cnblogs.com/platero/p/3993877.html)

[ Caffe研究实践 一 ------环境搭建](http://blog.csdn.net/forest_world/article/details/51371560)

[GIthub Issues 263](https://github.com/BVLC/caffe/issues/263)
