---
title: C 语言标准库函数
layout: page
date: 2016-06-13 10:56
---

笔记涵盖C语言标准库的15个头文件解释以及相应函数，持续更新。

##1.<assert.h> 断言 ##

头文件<assert.h>提供了宏assert的定义，如果断言不为真，则程序会在标准错误流输出提示信息，并使程序异常中止，调用abort()。

demo：

    #include <assert.h>

    int main()
    {
        int a = 12;
        int b = 24;
        assert(a > b);
        printf("a is smaller than b!\n");
        return 0;
    }

output：

    test-c-function.out: test-c-function.cpp:8: int main(): Assertion ‘a > b' failed.
    已放弃 (核心已转储)

上面的程序中止，printf函数并未执行，且有这样的输出，int main(): Assertion ’a > b' failed.，原因就是因为a其实小于b，导致断言失败，assert输出错误信息，并调用abort()中止了程序的运行。

将 assert(a > b); 修改为assert(a <= b);

output:

    a is smaller than b!

正常输出了printf的内容。

##2.<ctype.h> 字符测试 ##

<ctype.h> 主要提供两类函数：字符测试函数和字符大小转换函数，提供的函数中都是以int类型为参数，并返回一个int类型的值，实参类型应该隐式转换或者显式转换为int类型。

1.islanum

定义：int isalnum(int c)，判断是否是字母或者数字。

2.isalpha

定义：int isalpha(int c),判断是否是字母。

3.iscntrl

定义：int iscntrl(int c),判断是否是控制字符。

4.isdigit

定义：int isdigit(int c)，判断是否是数字。

5.isgraph

定义：int isgraph(int c)，判断是否是可显示字符。

6.islower

定义：int islower(int c)，判断是否是小写字母。

7.isupper

定义：int isupper(int c)，判断是否是大写字母。

8.isprint

定义：int isprint(int c)，判断是否是可显示字符。

9.ispunct

定义：int ispunct(int c)，判断是否是标点字符。

10.issapce

定义：int isspace(int c)，判断是否是空白字符。

11.isxdigit

定义：int isxdigit(int c)，判断字符是否是16进制。

12.tolower

定义：int tolower(int c)，转换为小写字母。

13.toupper

定义：int toupper(int c)，转换为大小字母。


##3.<errno.h> 错误代码 ##

<errno.h> 定义了通过错误码来返回错误信息的宏，errno 宏定义为一个int形态的左值，包含任何函数使用errno功能所产生的上一个错误码。

一些表示错误码，定义为整数值的宏：

EDOM，源自于函数的参数超出范围;

ERANGE，源自于函数的结果超出范围;

EILSEQ，源自于不合法的字符顺序;
