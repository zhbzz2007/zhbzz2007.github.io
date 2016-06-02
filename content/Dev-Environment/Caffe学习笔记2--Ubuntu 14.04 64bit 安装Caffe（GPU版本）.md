---
title: Caffe学习笔记2--Ubuntu 14.04 64bit 安装Caffe（GPU版本）
layout: page
date: 2016-06-02 23:02
---

## 0.检查配置 ##

**1.**VMWare上运行的Ubuntu，并不能支持真实的GPU（除了特定版本的VMWare和特定的GPU，要求条件严格，所以我在VMWare上搭建好了Caffe环境后，又重新在Windows 7 64bit系统上安装了Ubuntu 14.04 64bit系统，链接[在此](http://www.cnblogs.com/zhbzz2007/p/5493395.html)，以此来搭建Caffe GPU版本）；

**2.**确定GPU支持CUDA

输入：

    lspci | grep -i nvidia

显示结果：


我的是GTX 650，然后到[CUDA GPUs](http://developer.nvidia.com/cuda-gpus)去验证，支持CUDA；

![](http://i.imgur.com/jdsEp5g.png)

**3.**确定Linux版本支持CUDA

输入：

    uname -m && cat /etc/*release

结果显示：

![](http://i.imgur.com/l8caf7X.png)

**4.**确定系统已经安装了GCC

输入：

    gcc --version

结果显示：

![](http://i.imgur.com/INqTjG2.png)

**5.**确定系统已经安装了正确的Kernel Headers和开发包

输入：

    uname -r

结果：4.2.0-35-generic，这个是必须安装的kernel headers和开发包的版本；

安装对应的kernels header和开发包，

    sudo apt-get install linux-headers-$(uname -r)

## 1.Install CUDA ##

下载CUDA，从https://developer.nvidia.com/cuda-downloads，下载对应版本的cuda安装包，我下载的是deb（local）版，

![](http://i.imgur.com/x5ESO9S.png)

安装CUDA，运行如下命令，即可安装CUDA；

    sudo dpkg -i cuda-repo-ubuntu1404_7.5-18_amd64.deb
    sudo apt-get update
    sudo apt-get install cuda

安装完成后，重启一下；

## 2.安装cuDNN ##

下载cuDNN，从https://developer.nvidia.com/rdp/cudnn-download，注册，然后下载，

![](http://i.imgur.com/9mNyhaC.png)

注意事项：
有可能由于最新cuDNN不稳定，导致后续caffe工程编译失败，报出如下错误，详情参考第三个链接，这时可以回退一下，下载较新版本的cuDNN；

    CXX src/caffe/test/test_im2col_layer.cpp
    In file included from ./include/caffe/util/device_alternate.hpp:40:0,
                     from ./include/caffe/common.hpp:19,
                     from ./include/caffe/blob.hpp:8,
                     from src/caffe/test/test_im2col_layer.cpp:5:
    ./include/caffe/util/cudnn.hpp: In function ‘void caffe::cudnn::createPoolingDesc(cudnnPoolingStruct**, caffe::PoolingParameter_PoolMethod, cudnnPoolingMode_t*, int, int, int, int, int, int)’:
    ./include/caffe/util/cudnn.hpp:127:41: error: too few arguments to function ‘cudnnStatus_t cudnnSetPooling2dDescriptor(cudnnPoolingDescriptor_t, cudnnPoolingMode_t, cudnnNanPropagation_t, int, int, int, int, int, int)’
             pad_h, pad_w, stride_h, stride_w));
                                             ^
    ./include/caffe/util/cudnn.hpp:15:28: note: in definition of macro ‘CUDNN_CHECK’
         cudnnStatus_t status = condition; \
                                ^
    In file included from ./include/caffe/util/cudnn.hpp:5:0,
                     from ./include/caffe/util/device_alternate.hpp:40,
                     from ./include/caffe/common.hpp:19,
                     from ./include/caffe/blob.hpp:8,
                     from src/caffe/test/test_im2col_layer.cpp:5:
    /usr/local/cuda/include/cudnn.h:799:27: note: declared here
     cudnnStatus_t CUDNNWINAPI cudnnSetPooling2dDescriptor(
                               ^
    Makefile:572: recipe for target '.build_release/src/caffe/test/test_im2col_layer.o' failed
    make: *** [.build_release/src/caffe/test/test_im2col_layer.o] Error 1

拷贝cuDNN库文件到cuda目录下，

    tar -zxvf cudnn-7.0-linux-x64-v4.0-prod.tgz
    cd cuda
    sudo cp lib64/* /usr/local/cuda/lib64/
    sudo cp include/cudnn.h /usr/local/cuda/include/

设置环境变量，在/etc/profile中添加CUDA环境变量，

    sudo gedit /etc/profile #在打开的文件中加入如下两句话

    export PATH=/usr/local/cuda/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

保存后，使环境变量立即生效，

    source /etc/profile

进入/usr/local/cuda/samples，执行下面的命令来build samples，

    sudo make all -j4

全部编译完成后，进入 samples/bin/x86_64/linux/release，运行deviceQuery，

    ./deviceQuery

如果出现显卡信息，则驱动及显卡安装成功，结果如下：

![](http://images2015.cnblogs.com/blog/668850/201605/668850-20160515201137430-749988136.png)

## 3.编译caffe ##

修改caffe/Makefile.config，

    # cuDNN acceleration switch (uncomment to build with cuDNN).
    USE_CUDNN := 1#去掉这个注释

    # CPU-only switch (uncomment to build without GPU support).
    # CPU_ONLY := 1#加上这个注释

然后输入，

    make clean#第一次编译，不需要执行
    make all
    make test
    make runtest

显示结果：


编译pycaffe，

    make pycaffe

## 4.mnist数据测试 ##

    cd caffe

    # 下载mnist数据
    sh data/mnist/get_mnist.sh

    sh examples/mnist/create_mnist.sh

    ./examples/mnist/train_lenet.sh

执行结果，

![](http://images2015.cnblogs.com/blog/668850/201605/668850-20160515202055195-1470988466.png)

和CPU版本[Caffe学习笔记1--Ubuntu 14.04 64bit caffe安装](http://www.cnblogs.com/zhbzz2007/p/5479181.html)的mnist数据测试相比，速度明显提升；当时我运行脚本后，就开始看书，刚看完一页，抬头发现已经运行完毕，第一次用GPU计算，虽然只是很渣的GTX 650，但依然对GPU的运算能力感到佩服；

## 5.深度学习开源库环境搭建大礼包 ##

昨晚群里一个哥们分享了一个github链接，涵盖了主流深度学习开源库的环境搭建，包括Nvidia驱动、CUDA、cuDNN、TensorFlow、Caffe、Theano、Keras、Torch，链接[在此](https://github.com/saiprashanths/dl-setup)，他的配置是Ubuntu 14.04 64bit + Nvidia Titan X，还是适用于好多朋友的机器的，所以好东西还是要分享给大家；

这条消息对我而言既是好消息，又是坏消息，好消息就是这个大礼包真的很棒，坏消息就是我把环境搭建好之后，才知道这个消息。不过，在搭建环境的过程中，自己踩过一些坑，也算是一个熟悉Linux的过程吧，环境搭建好了，还是很开心的；


## 6.参考链接 ##

[Ubuntu 14.04上安装caffe](http://www.cnblogs.com/wm123/p/5385940.html)

[Ubuntu 14.04 + Caffe + Cuda 7.5 + Opencv 3.0安装教程](http://blog.csdn.net/u013915633/article/details/49867947)

[Caffe error make test](http://stackoverflow.com/questions/36637923/caffe-error-make-test)
