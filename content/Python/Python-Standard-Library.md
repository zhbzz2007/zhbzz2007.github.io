---
title: Python 常用标准库函数
layout: page
date: 2016-06-18 13:47
---

## OS ##

OS标准库常用函数：

**abort()：** 立即终止python解释器运行，不返回任何值;

**access(path,mode):** 测试特定用户对path是否有特定的权限。

文件的信息为：

-rw-rw-r--  1 zhb zhb      525  5月 28 22:29 populations.txt

代码：

    fileName = "populations.txt"
    print os.access(fileName,os.F_OK)#测试文件是否存在
    print os.access(fileName,os.R_OK)#测试文件是否具有读权限
    print os.access(fileName,os.W_OK)#测试文件是否具有写权限
    print os.access(fileName,os.X_OK)#测试文件是否具有执行权限

结果：

    True
    True
    True
    False

**chdir():** 修改当前工作目录到指定路径;

代码：

    print os.getcwd()
    path = "/home/zhb"
    os.chdir(path)
    print os.getcwd()

结果：

    /home/zhb/Desktop/work
    /home/zhb


**chmod():** 修改文件的获取权限;

代码：

    fileName = "populations.txt"
    os.chmod(fileName,os.W_OK)

结果：

    -------r-- 1 zhb zhb 525  5月 28 22:29 populations.txt#执行前

    --------w- 1 zhb zhb 525  5月 28 22:29 populations.txt#执行后

**chown():** 修改目录的用户id和所在组id;



## 参考链接 ##

[670个常用的Python库和示例代码](http://www.bkjia.com/ASPjc/1039843.html)

[Python 常用的标准库以及第三方库有哪些？](http://www.zhihu.com/question/20501628)

[Awesome Python](https://github.com/vinta/awesome-python)

[中文版的python常用模块库清单](https://github.com/ziwang-com/zwpy_lst)
