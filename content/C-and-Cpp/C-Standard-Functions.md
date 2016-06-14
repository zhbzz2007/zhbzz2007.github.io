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
    #include <stdio.h>

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

## 4.<float.h>浮点数运算 ##

<float.h>定义了浮点数值的最大最小限，浮点型数值以下面的方式定义：符号-value E 指数  符号是正负，value是数字的值。

下面的值是用#define定义，所有实例里FLT指的是float，DBL是double，LDBL指的是long double。

FLT_ROUNDS：定义浮点型数值四舍五入的方式，-1是不确定，0是向0,1是向最近，2是向正无穷大，3是负无穷大。

FLT_RADIX 2：定义指数的基本表示（比如base-2是二进制，base-10是十进制，base-16是十六进制）。

FLT_MANT_DIG，DBL_MANT_DIG，LDBL_MANT_DIG：定义数值里数字的个数。

FLT_DIG 6，DBL_DIG 10,LDBL_DIG 10：再四舍五入之后能不更改表示的最大小数位。

FLT_MIN_EXP，DBL_MIN_EXP，LDBL_MIN_EXP：FLT_RADIX的指数的最小负整数值。

FLT_MIN_10_EXP -37,DBL_MIN_10_EXP -37,LDBL_MIN_10_EXP -37：10进制表示法的指数的最小负整数值。

FLT_MAX_EXP，DBL_MAX_EXP，LDBL_MAX_EXP：FLT_RADIX的指数的最大整数值。

FLT_MAX_10_EXP +37,DBL_MAX_10_EXP +37,LDBL_MAX_10_EXP +37：10进制表示法的指数的最大整数值。

FLT_MAX 1E+37,DBL_MAX 1E+37，LDBL_MAX 1E+37：浮点型的最大限。

FLT_EPSILON 1E-5,DBL_EPSILON 1E-9,LDBL_EPSILON 1E-9：能表示的最小有符号数。

## 5.<limits.h> ##

CHAR_BIT：一个ASCII字符长度。

SCHAR_MIN：字符型最小值。

SCHAR_MAX：字符型最大值。

UCHAR_MAX：无符号字符型最大值。

CHAR_MIN，CHAR_MAX：char字符的最小最大值，如果char字符表示有符号整数，那么它们的值就和有符号整数一样，否则char字符的最小值就是0,最大值就是无符号char字符的最大值。

MB_LEN_MAX：一个祖父所占最大字节数。

SHRT_MIN：最小短整型。

SHRT_MAX：最大短整型。

USHRT_MAX：最大无符号短整型。

INT_MIN：最小整型。

INT_MAX：最大整型。

UINT_MIN：最大无符号整型。

LONG_MIN：最小长整型。

LONG_MAX：最大长整型。

ULONG_MAX：无符号长整型。

## 6.<locale.h>本地化 ##

<locale.h>定义了区域设置相关的函数。

setlocale函数：用于设置或返回当前的区域特性。

定义:setlocale(constant,location)

constant：指定设置的场景信息。

location：指定需要进行场景设置的国家或区域。

localeconv函数：用于返回当前区域中的数字和货币信息（保存在struct lconv结构实例中）。

定义:struct lconv * localeconv(void)
