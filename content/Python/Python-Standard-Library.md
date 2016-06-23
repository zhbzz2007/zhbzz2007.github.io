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
|os|getlogin|返回实际登录名|
|os|getpgid|调用系统getgpid()|
|os|getpgrp|返回当前进程群组id|
|os|**getpid**|返回当前进程id|
|os|**getppid**|返回父亲进程id|
|os|getresgid|获得当前进程真实、有效、已保存的群组id|
|os|getresuid|获得当前进程真实、有效、已保存的用户id|
|os|getsid|调用系统getsid()|
|os|**getuid**|返回当前进程的用户id|
|os|initgroups|调用系统initgroups()初始化群组获取列表，包括指定用户是一个成员，还有指定的群组id|
|os|isatty|如果文件描述符fd是一个连接到副终端的打开的文件描述符，就返回True|
|os|**kill**|通过信号，杀死一个进程|
|os|**killgp**|通过信号，杀死一个进程群组|
|os|lchown|修改路径的所有者和群组id为指定的数字uid和gid，不会返回相应的符号链接（区别于硬链接）|
|os|link|产生到一个文件的硬链接|
|os|**listdir**|以列表形式返回一个目录下所有的入口名称，列表中元素顺序是随机的|
|os|**lseek**|设置一个文件描述符当前的位置，返回新的光标字节位置，从文件开始处开始|
|os|lstat|类似于stat()，但是不会产生符号链接|
|os|major|从原始设备数中提取中设备主数|
|os|makedev|从主、从设备数中压缩为原始设备数|
|os|**makedirs**|超级mkdir，创建叶子目录，以及所有中间的目录|
|os|minor|从原始设备数中提取从设备数|
|os|**mkdir**|创建一个目录|
|os|mkfifo|创建一个FIFO，一个POSIX命名管道，用于进程间通信|
|os|mknode|创建一个文件系统节点|
|os|nice|降低进程的优先级，并返回新的优先级|
|os|open|打开文件|
|os|openty|打开一个伪终端|
|os|**path**|包含路径相关方法的模块|
|os|pathconf|返回文件或者目录的配置有限名称|
|os|pathsep|路径分割符|
|os|pipe|创建一个pipe|
|os|popen|通过一个命令，打开一个pipe|
|os|popen2|在子进程中执行shell命令|
|os|popen3|在子进程中执行shell命令|
|os|popen4|在子进程中执行shell命令|
|os|putenv|修改或者添加环境变量|
|os|read|读一个文件描述符|
|os|readlink|读一个字符串表示的路径，路径指向符号链接|
|os|remove|删除一个文件，类似于unlink|
|os|removedirs|超级rmdir，删除叶子目录和所有空的中间目录|
|os|rename|重新对文件或者目录进行命名|
|os|renames|超级rename，如果必须的话，创建目录，删除所有目录|
|os|rmdir|删除一个目录|
|os|sep|目录分隔符，Linux下为'/'，Windows下为'\\'|
|os|setegid|设置当前进程有效的群组id|
|os|seteuid|设置当前进程有效的用户id|
|os|setgid|设置当前进程群组id|
|os|setgroups|设置当前进程的群组|
|os|setpgid|调用setpgid|
|os|setpgrp|使得这个进程成为进程群组的第一个|
|os|setregid|设置当前进程为真实且有效的群组id|
|os|setresgid|设置当前进程为真实、有效、已保存的群组id|
|os|setresuid|设置当前进程为真实、有效、已保存的用户id|
|os|setreuid|设置的当前进程为真实、有效的用户id|
|os|setsid|调用setsid|
|os|setuid|设置当前进程用户id|


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
