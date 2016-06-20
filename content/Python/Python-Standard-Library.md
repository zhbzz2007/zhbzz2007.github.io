---
title: Python 常用标准库函数
layout: page
date: 2016-06-18 13:47
---

## OS ##

OS标准库常用函数：

| 模块名称 | 函数名称 |主要作用|
|-------|--------|-------|
|os|abort|立即终止python解释器运行，不返回任何值|
|os|access|测试特定用户对path是否有特定的权限|
|os|chdir|修改当前工作目录到指定路径|
|os|chmod|修改文件的获取权限|
|os|chown|修改目录的用户id和所在组id|
|os|chroot|修改根目录到指定的目录|
|os|chroot|关闭文件描述符|
|os|closerange(fd_low,fd_high)|关闭所有的文件描述符，[fd_low,fd_high]，忽略错误|
|os|confstr(name)|返回字符串形式的系统配置变量|
|os|ctermid|返回当前进程所在的控制终端的名称|
|os|dup|返回一个复制的文件描述符|
|os|dup2|复制文件描述符|
|os|execl|执行参数列表中的可执行文件，替代本进程|
|os|execle|执行参数列表和环境中的可执行文件，替代本进程|
|os|execlp|执行$PATH中搜索的可执行文件，替代本进程|
|os|execlpe|执行$PATH中搜索的和环境中的可执行文件，替代本进程|
|os|execv|执行参数中的可执行路径，替代本进程|
|os|execve|执行参数中和环境中的可执行路径，替代本进程|
|os|execvp|执行参数列表中在$PATH中搜索的可执行文件，替代本进程|
|os|execvpe|执行参数列表和环境中在$PATH中搜索的可执行文件，替代本进程|
|os|fchdir|修改给定文件描述的目录|
|os|fchmod|修改给定文件描述符的获取权限|
|os|fchown|修改给定文件描述符的用户id和所在组id|
|os|fdatasync|强制写入文件到磁盘中给定文件描述符|
|os|fdopen|返回一个链接文件描述符的文件对象|
|os|fork|复制一个子进程|
|os|forkpty|复制一个新进程伴随着一个伪终端|
|os|fpathconf|返回给定文件描述符的限制名称|
|os|fstat|返回stat结果|
|os|fstatvfs|返回statvfs结果|
|os|ftruncate|一个文件截断为指定长度|
|os| **getcwd** |返回当前工作目录的字符串表示形式|
|os|getcwdu|返回当前工作目录的unicode字符串表示形式|
|os|getegid|返回当前进程的有效的群组id|
|os|getenv|返回环境变量，如果不存在，则返回None|
|os|geteuid|返回当前进程的有效的用户id|
|os|getgid|返回当前进程的群组id|
|os|getgroups|返回进程的一组群组id|
|os|getloadavg|返回系统中运行的进程数目，从最后1,5,15分钟，如果不能获得，则返回OSError|

**access(path,mode):**

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

**chdir():** 代码：

    print os.getcwd()
    path = "/home/zhb"
    os.chdir(path)
    print os.getcwd()

结果：

    /home/zhb/Desktop/work
    /home/zhb


**chmod():** 代码：

    fileName = "populations.txt"
    os.chmod(fileName,os.W_OK)

结果：

    -------r-- 1 zhb zhb 525  5月 28 22:29 populations.txt#执行前

    --------w- 1 zhb zhb 525  5月 28 22:29 populations.txt#执行后



## 参考链接 ##

[670个常用的Python库和示例代码](http://www.bkjia.com/ASPjc/1039843.html)

[Python 常用的标准库以及第三方库有哪些？](http://www.zhihu.com/question/20501628)

[Awesome Python](https://github.com/vinta/awesome-python)

[中文版的python常用模块库清单](https://github.com/ziwang-com/zwpy_lst)
