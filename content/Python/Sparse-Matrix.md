---
title: Python 稀疏矩阵表示
layout: page
date: 2016-06-02 22:47
---

本博客主要用于记录python稀疏矩阵表示方法;

##方法1： ##

用dict以二进制字符串形式存储，参考链接：

http://www.jb51.net/article/51609.htm

##方法2: ##

使用scipy中的sparse进行存储，sparse库中提供多种表示稀疏矩阵的格式，

###1.稀疏矩阵表示 ##

1) dok_matrix，继承于dict，以（行，列）作为key，对应值为矩阵中位于（行，列）中的元素值，适合单个元素的添加，删除和存取操作，通常用来逐渐添加非零元素，然后转换成其它支持快速运算的格式。

输入：

    from scipy import sparse
    a = sparse.dok_matrix((3,4))
    a[0,1] = 2.0
    a[1,3] = 3.0
    print a.keys()
    print a.values()

输出：

    [(0, 1), (1, 3)]
    [2.0, 3.0]

设置对角矩阵，

输入：

    from scipy import sparse
    a = sparse.dok_matrix((4,4))
    a.setdiag(2)#设置值到对角元素上
    print a.keys()
    print a.values()

输出：

    [(3, 3), (0, 0), (1, 1), (2, 2)]
    [2.0, 2.0, 2.0, 2.0]



2）lil_matrix,使用两个列表保存非零元素，data保存每行中的非零元素，rows保存非零元素所在的列，适合逐个添加元素，并且能够快速获取行相关的数据。

输入：

    from scipy import sparse
    b = sparse.lil_matrix((10,5))
    b[2,3] = 1.0
    b[3,4] = 2.0
    b[3,2] = 3.0
    print b.data
    print b.rows

输出：

    array([[], [], [1.0], [3.0, 2.0], [], [], [], [], [], []], dtype=object)
    array([[], [], [3], [2, 4], [], [], [], [], [], []], dtype=object)

3）coo_matrix，采用三个数组row、col和data保存非零元素的信息，三个数组的长度相同，row保存元素的行，col保存元素的列，data保存元素的值，coo_matrix不支持元素的存取和增删，一旦创建后，除了将它转换成其它格式的矩阵，几乎无法对其做任何操作和矩阵运算。

coo_matrix支持重复元素，即同一行列坐标可以出现多次，当转换成其它格式的矩阵时，将对同一行列坐标对应的多个值进行求和。

输入：

    from scipy import sparse
    row = [2,3,3,2]
    col = [3,4,2,3]
    data = [1,2,3,10]
    c = sparse.coo_matrix((data,(row,col)),shape = (5,6))
    print c.col, c.row,c.data
    print c.toarray()

输出：

    [3 4 2 3] [2 3 3 2] [ 1  2  3 10]
    [[ 0  0  0  0  0  0]
     [ 0  0  0  0  0  0]
     [ 0  0  0 11  0  0]
     [ 0  0  3  0  2  0]
     [ 0  0  0  0  0  0]]

###2.稀疏矩阵存取 ###

有时候需要将稀疏矩阵保存至本地，以后直接读取即可。

1）savetxt/loadtxt,保存为文本文件，会将所有的元素保存，本地文件较大。

2）mmwrite/mmread，保存为mtx文件，只保存非零元素，本地文件小。

其中mmread/mmwrite，是在scipy.io库中。

参考链接：

http://blog.csdn.net/pipisorry/article/details/41762945

http://hyry.dip.jp/tech/book/page/scipynew/scipy-810-sparse.html#sparse

http://blog.csdn.net/stereohomology/article/details/37657777

http://scipy.github.io/devdocs/generated/scipy.sparse.dok_matrix.html#scipy.sparse.dok_matrix

## 序列化 ##

import pickle

pickle.dump:将数据保存至本地;

pickle.load：从本地加载数据，和原来的数据一样，不需要重新解析;


参考链接：

http://www.cnblogs.com/linyawen/archive/2012/03/22/2411381.html
