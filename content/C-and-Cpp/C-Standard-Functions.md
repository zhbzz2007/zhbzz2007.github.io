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

## 5.<limits.h>取值范围 ##

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

## 7.<math.h>数学函数 ##

<math.h>是C语言中的数学函数库。

**三角函数：**

double sin(double x)：正弦

double cos(double x)：余弦

double tan(double x)：正切

**反三角函数：**

double asin(double x)：反正弦，结果介于[-PI/2,PI/2]

double acos(double x)：反余弦，结果介于[0,PI]

double atan(double x)：反正切（主值），结果介于[-PI/2,PI/2]

double atan2(double y,double)：反正切（整圆值），结果介于[-PI，PI]

**双曲三角函数：**

double sinh(double x)：计算双曲正弦

double cosh(double x)：计算双曲余弦

double tanh(double x)：计算双曲正切

**指数和对数：**

double exp(double x)：求取自然数e的幂

double sqrt(double x)：开平方

double log(double x)：以e为底的对数

double log10(double x)：以10为底的对数

double pow(double x,double y)：计算以x为底的y次幂

float powf(float x,float y)：与pow一致，输入与输出皆为浮点数

**取整：**

double ceil(double x)：向上取整

double floor(double x)：向下取整

**标准化浮点数：**

double frexq(double f,int \*p)：标准化浮点数，f = x * 2^p，已知f求x，p（x介于[0.5,1]）

double ldexp(double x,int p)：与frexp相反，已知x，p求f

**取整与取余：**

double modf(double x,double \*y)：将参数的整数部分通过指针回传，返回小数部分

double fmod(double x,double y)：返回两个参数相除的余数

## 8.<setjmp.h> ##

<setjmp.h>中定义了一种特别的函数调用和函数返回顺序的方式，允许程序流程立即从一个深层嵌套的函数中返回。

int setjmp(jmp_buf env)：设置跳转点，将当前程序的状态保存在结构env，为调用宏longjmp设置一个跳转点。

longjmp(jmp_buf env,int retval)：跳转，利用setjmp设置的env变量进行跳转，程序会自动跳转到setjmp宏的返回语句处，返回值为retval。

一般，宏setjmp和longjmp是成对使用，这样程序流程可以从一个深层嵌套的函数中返回。

## 9.<signal.h> ##

<signal.h>头文件中，提供了一些函数用以处理执行过程中所产生的信号。

typedef sig_atomic_t

sig_atomic_t：类型是int类型，用于接受signal函数的返回值

**宏：**

以SIG_开头的宏用于定义信号处理函数，

SIG_DFL：默认信号处理函数

SIG_ERR：表示一个错误信号，当signal函数调用失败时的返回值

SIG_IGN：信号处理函数，表示忽略该信号

SIG开头的宏是用来在下列情况下，用于表示一个信号代码，

SIGABRT：异常中止（abort函数产生）

SIGFPE：浮点错误（0作为除数产生的错误，非法的操作）

SIGILL：非法错误（指令）

SIGINT：交互式操作产生的信号（如CRTL+C）

SIGSEGV：无效访问存储（片段的非法访问，内存非法访问）

SIGTERM：终止请求

**函数：**

void (\*signal(int sig,void (\*func)(int))) (int):sig表示一个信号代码，即上面所定义的SIG开头的宏，当有信号出现的时候，参数func所定义的函数就会被调用。

如果func等于SIG_DFL，则表示调用默认的处理函数;

如果等于SIG_IGN，则表示这个信号被忽略（不做处理）;

如果func是用户自定义的函数，则会先调用默认的处理函数，然后再调用用户自定义的函数。自定义函数，有意额参数，参数类型为int，用来表示信号代码，同时，函数必须以return,abort,exit或longjmp等语句结束。当自定义函数运行结束，程序会继续从被终止的地方继续运行。（除非信号是SIGFPE导致结果未定义，则可能无法继续运行）

如果调用signal函数成功，则会返回一个指针，该指针指向为所指定的信号类别的所预先定义的信号处理器。

如果调用失败，则返回一个SIG_ERR，同时errno的值也会被相应的改变。

int raise(int sig)：发送一个sig，信号参数为SIG开头的宏。

如果调用成功，返回0,否则返回一个非零值。
