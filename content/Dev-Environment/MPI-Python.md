---
title: MPI Python环境搭建
layout: page
date: 2016-08-04 16:50
---

本博客主要用于介绍MPI Python环境搭建；

## 0.Python安装 ##

## 1.openmpi安装 ##

shell中执行，

    wget http://www.open-mpi.org/software/ompi/v1.4/downloads/openmpi-1.4.1.tar.gz
    tar xzvf openmpi-1.4.1.tar.gz
    mkdir openmpi
    ./configure --prefix = /home/zhb/install/openmpi
    cd openmpi-1.4.1
    make all install

然后把bin目录和lib目录添加到环境变量里面，

    sudo vim .profile

文本中输入，

    export PATH=/home/zhb/install/openmpi/bin:$PATH
    export LD_LIBRARY=/home/zhb/install/openmpi/lib:$LD_LIBRARY

然后执行，

    source .profile

## 2.安装mpi4py ##

在[mpi4py](https://pypi.python.org/pypi/mpi4py)网址下载mpi4py源码，然后在shell中执行，

    tar -xgvf mpi4py-2.0.0.tar.gz
    cd mpi4py-2.0.0
    vim mpi.cfg

在[openmpi]下面，修改为刚才已经安装好的openmpi的目录；

    # Open MPI example
    # ----------------
    [openmpi]
    #mpi_dir              = /home/devel/mpi/openmpi-1.8.6
    mpi_dir              = /home/zhb/install/openmpi/
    mpicc                = %(mpi_dir)s/bin/mpicc
    mpicxx               = %(mpi_dir)s/bin/mpicxx
    #include_dirs         = %(mpi_dir)s/include
    #libraries            = mpi
    library_dirs         = %(mpi_dir)s/lib
    runtime_library_dirs = %(library_dirs)s

然后在shell中执行，

    python setup.py install

## 3.测试mpi4py ##

打开python，输入，

     import mpi4py.MPI as MPI
     dir(MPI)

就可以看到mpi4py.MPI的各个方法。
