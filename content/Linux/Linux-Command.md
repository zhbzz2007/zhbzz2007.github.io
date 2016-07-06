---
title: Linux 常用命令
layout: page
date: 2016-06-07 17:12
---

本博客主要用于整理Linux操作系统下常用的命令，因此会持续更新。

###1.文件管理 ###

1. cat

    查看文件内容；

    输入：

        cat 1.txt

    输出：

        Hello,Linux
        I like Ubuntu


2. chgrp

    修改文件或者目录所属的群组；

3. chmod

    修改文件或者目录的读写执行权限；

4. chown

    修改文件或者目录的所有者；

5. cksum

    检查文件爱呢的CRC是否正确，确保文件从一个系统传输到另一个系统的过程中不被损坏；

6. cmp

    比较两个文件是否相同，如果相同，则不显示任何信息，如果不相同，则显示不相同的行数；

    输入：

        cmp 1.txt 3.txt

    输出：

        1.txt 3.txt 不同：第 20 字节，第 2 行

7. diff

    比较两个文件的差异，如果没有差异，则不显示任何信息，如果有差异，则显示出有差异的地方；

    输入：

        diff 1.txt 3.txt

    输出：

        2c2,3
        < I like Ubuntu
        ---
        > I like CentOS
        >

8. diffstat

    读取diff的比较结果，显示统计数字；

9. file

    辨识文件文件的类型；

    输入：

        file test-c-function.out

    输出：

        test-c-function.out: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=6caf20f3985526c5562abdd51d45a2a006536ba0, not stripped

10. **find** (参数超级多，需要仔细研究)

    查找；

11. git,gitview

12. indent

    可辨识C原始代码文件，并加以格式化，以方便用户阅读；

13. cut

    负责剪切数据，以每一行为一个处理对象，这种机制和sed一样；

    输入：

        cat 1.txt | cut -c 5-10

    输出：

        o,Linu
        ke Ubu

14. ln

    为一个文件在另一个位置建立一个同步的链接，最常用的参数是-s；

15. less

   对文件或者其他输出进行分页显示 ，PageUp键向上翻页，PageDown向下翻页，退出按Q键；

###2.磁盘管理 ###

1. ls

    用于显示当前目录下文件与目录。

2. ll

    用于显示当前目录下所有文件与目录。

3. cd

    切换到指定目录。

4. mkdir

    创建目录。

5. pwd

    显示当前绝对路径。

6. mount

    挂载分区。

7. umount

    卸载分区。

8. df

    查看各个分区占用空间。

###3.文档编辑 ###

1. grep

    搜索。

###4.文件传输 ###


###5.磁盘维护 ###

1. bleachbit

    删除临时文件。（可以通过apt-get安装）

2. baobab

    可视化分析各个磁盘及各个子目录的使用情况。（可以通过apt-get安装）

3. synaptic

    查看各个应用所占用的空间，也可以删除旧的内核。（可以通过apt-get安装）

###6.网络通讯 ###

1. ping

    检测网络连通。

###7.系统管理 ###

1. top

    查看当前系统所起进程。


###8.系统设置 ###

1. clear

    清除当前命令行窗口。

###9.备份压缩 ###

1. tar

    文件解压缩。

###10.设备管理 ###















参考链接：

[Linux常用命令全集](http://itlab.idcquan.com/linux/special/linuxcom/)

[Linux教程](http://www.runoob.com/linux/linux-tutorial.html)
