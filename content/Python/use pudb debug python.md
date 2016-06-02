---
title: 使用pudb调试python
layout: page
date: 2016-06-02 23:02
---
本博客主要用于讲解如何使用pudb进行python调试；

## 1.安装 ##

    sudo pip install pudb

pip list查看安装结果：

![](http://i.imgur.com/4Ac3gHe.png)

## 2.使用 ##

测试程序：

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    from pudb import set_trace
    set_trace()

    def min(a,b):
        minValue = 0
        if(a >= b):
            minValue = b
        else:
            minValue = a
        return minValue

    if __name__ == "__main__":
        a = 1
        b = 10
        minValue = 0
        while(a < b):
            minValue = min(a,b)
            print "minValue=",minValue
            a += 1
            print "a=",a

需要在程序前面加上：

    from pudb import set_trace
    set_trace()

或者

    import pudb
    pu.db

## 3.调试 ##

使用pudb test.py，或者python -m pudb.run test.py

进入调试界面，如图

![](http://i.imgur.com/qdGhuNo.jpg)

按照调试命令，即可进行调试，红框为变量和栈的信息；

![](http://i.imgur.com/pajTiFT.jpg)

按下'c'键后，跳出本次循环，进入下次循环，a从3变为了4；

![](http://i.imgur.com/oAt1oj6.jpg)

## 4.调试命令 ##

n : next，执行下一步；

s : step into，进入函数内部；

c : continue，循环中跳出本次循环；

b : break point，断点；

! : python command line，python控制台；

? : help，帮助信息；

## 5.相关链接 ##

[pudb python链接](https://pypi.python.org/pypi/pudb/)

[pudb github链接](https://github.com/inducer/pudb)
