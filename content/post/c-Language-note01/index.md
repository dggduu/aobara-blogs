+++
author = "aobara"
title = "C语言笔记01"
description = ""
date = "2024-10-29"
categories = [
    "编程",
    "笔记",
]
+++
## 类型
### 浮点值的上溢：
当计算导致数字过大，超过当前类型能表达的范围时， 
就会发生上溢。这种行为在过去是未定义的，不过现在C语言规定，在这种情况下会给toobig赋一个表示无穷大的特定值，而且printf ()显示该值为inf或infinity (或者具有无穷含义的其他内容)。  
e.g.  
```c
#include <stdio.h>
int main(){
    float toobig=3.4E38*100.0f;
    printf("%E\n",toobig);
    return 0;
}
```
溢出，结果为无穷大：INF  

### 特殊的浮点值NaN (not a number的缩写)
例如，给asin ()函数传递一个值，该函数将 
返回一个角度，该角度的正弦就是传入函数的值。但是正弦值不能大于1，因此，如果传入的参数大于1，
该函数的行为是未定义的。在这种情况下，该函数将返回NaN值，printf ()函数可将其显示为nan、NaN
或其他类似的内容。

### 复数和虚数类型
C 语言有 3 种复数类型：float_Complex、double_Complex 和 long double—Complex  

如果包含 complex. h 头文件，便可用 complex 代替Complex，用 imaginary 代替Imaginary，还可以用I代替-1的平方根。



## 可移植类型：stdint.h 和 inttypes.h
### 精确宽度整数类型(exact-width integer type)

int32_t
表示32位的有符号整数类型。在使用32位i nt的系统中，头文件会把int32_t作为int的别名。
**不同的系统也可以定义相同的类型名。例如，int为16位、l ong为32位的系统会把int32_t作为long的别名。**

| 类型名 | printf说明符 | scanf说明符 | 最小值       | 最大值       |
|--------|--------------|-------------|--------------|--------------|
| int8_t | PRId8        | SCNd8       | INT8_MIN     | INT8_MAX     |
| int16_t| PRIu16       | SCNud16     | INTI6_MIN    | INT16_MAX    |
| int32_t| PRIu32       | SCNud32     | INT32_MIN    | INT32_MAX    |
| int64_t| PRIu64       | SCNud64     | INT64_MIN    | INT64_MAX    |
| uint8_t| PRIu8        | SCNuu8      | 0            | UINT8_MAX    |
| uint16_t| PRIu16      | SCNuu16     | 0            | UINT16_MAX   |
| uint32_t| PRIu32      | SCNuu32     | 0            | UINT32_MAX   |
| uint64_t| PRIu64      | SCNuu64     | 0            | UINT64_MAX   |

### 最小宽度类型minimum width type)
一些类型名保证所表示的类型一定是至少有指定宽度的最小整数类型.  
如果某系统的最小整数类型是16位，可能不会定义int8_t类型。尽管如此，该系统仍可使用int_least8_t类型，但可能把该类型实现为16位的整数类型。

| 类型名         | printf()说明符          | scanf()说明符           | 最小值               | 最大值               |
|----------------|-------------------------|-------------------------|----------------------|----------------------|
| int_least8_t   | PRILEASTd8              | SCNLEASTd8              | INT_LEAST8_MIN       | INT_LEAST8_MAX       |
| int_least16_t  | PRILEASTd16             | SCNLEASTd16             | INT_LEAST16_MIN      | INT_LEAST16_MAX      |
| int_least32_t  | PRILEASTd32             | SCNLEASTd32             | INT_LEAST32_MIN      | INT_LEAST32_MAX      |
| int_least64_t  | PRILEASTd64_z           | SCNLEASTd64_z           | INT_LEAST64_MIN      | INT_LEAST64_MAX      |
| uint_least8_t  | PRILEASTu8              | SCNLEASTu8              | 0                    | UINT_LEAST8_MAX      |
| uint_least16_t | PRILEASTU16             | SCNLEASTu16             | 0                    | UINT_LEAST16_MAX     |
| uint_least32_t | PRILEASTu32             | SCNLEASTu32             | 0                    | UINT_LEAST32_MAX     |
| uint_least64_t | PRILEASTu64             | SCNLEASTu64             | 0                    | UINT_LEAST64_MAX     |


### 最快最小宽度类型(.fastst minimum width type)
int_fast8_t被定义为系统中对8位有符号值而言运算最快的整数类型的别名。

| 类型名       | printf ()说明符 | scanf ()说明符 | 最小值                | 最大值                |
|--------------|-----------------|----------------|-----------------------|-----------------------|
| int_fast8_t  | PRIFASTd8       | SCNFFASTd8     | INT_FAST8_MIN         | INT_FAST8_MAX         |
| int_fast16_t | PRIFASTd16      | SCNFFASTd16    | INT_FAST16_MIN        | INT_FAST16_MAX        |
| int_fast32_t | PRIFASTd32      | SCNFFASTd32    | INT_FAST32_MIN        | INT_FAST32_MAX        |
| int_fast64_t | PRIFASTd64      | SCNFFASTd64    | INT_FAST64_MIN        | INT_FAST64_MAX        |
| uint_fast8_t | PRIFASTu8       | SCNFFASTu8     | 0                     | UINT_FAST8_MAX        |
| uint_fast16_t| PRIFASTul6      | SCNFFASTul6    | 0                     | UINT_FAST16_MAX       |
| uint_fast32_t| PRIFASTu32      | SCNFFASTu32    | 0                     | UINT_FAST32_MAX       |
| uint_fast64_t| PRIFASTu64      | SCNFFASTu64    | 0                     | UINT_FAST64_MAX       |

### 最大整数类型
C99定义了最大的有符号整数类型intmax_t，可储存任何有效的有符号整数值。类似地，unitmax_t表示最大的无符号整数类型。顺带一提，这些类型有可能比long long和unsigned long类型更大

| 类型名         | printf()说明符          | scanf()说明符           | 最小值               | 最大值               |
|----------------|-------------------------|-------------------------|----------------------|----------------------|
| intmax_t       | PRIdMAX                 | SCNdMAX                 | INTMAX_MIN           | INTMAX_MAX           |
| uintmax_t      | PRIUMAX                 | SCNuMAX                 | 0                    | UINTMAX_MAX          |



## ctype.h头文件

### 字符测试函数
| 表7.1 ctype.h头文件中的字符测试函数 |
| --- | --- |
| 函数名 | 如果是下列参数时，返回值为真 |
| isalnum() | 字母数字（字母或数字） |
| isalpha() | 字母 |
| isblank() | 标准的空白字符（空格、水平制表符或换行符）或其他本地化指定为空白的字符 |
| iscntrl() | 控制字符，如Ctrl+B |
| isdigit() | 数字 |
| isgraph() | 除空格外的任意可打印字符 |
| islower() | 小写字母 |
| ispunct() | 可打印字符 |
| isspace() | 空白字符（空格、换行符、回车符、垂直制表符、水平制表符或其他本地化定义的字符） |
| isupper() | 大写字母 |
| isxdigit() | 十六进制数字符 |

| 表7.2 ctype.h头文件中的字符映射函数 |
| --- | --- |
| 函数名 | 行为 |
| tolower() | 如果参数是大写字符，该函数返回小写字符；否则，返回原始参数 |
| toupper() | 如果参数是小写字符，该函数返回大写字符；否则，返回原始参数 |

### 字符映射函数
| 函数名       | 行为                                                         |
|--------------|-------------------------------------------------------------|
| `tolower()`  | 如果参数是大写字符，该函数返回小写字符；否则，返回原始参数   |
| `toupper()`  | 如果参数是小写字符，该函数返回大写字符；否则，返回原始参数   | 



## 位操作
![1](/1.png "Optional title")
### 位运算符
1. 二进制反码或按位取反：~
2. 按位与：&
3. 按位或：丨
4. 按位异或：^
 1^1=0;    1^0=1;   0^0=0；

5.移位运算符
```<!DOCTYPE html>
>> 左位移 =/2
<< 右位移 =*2
```
 ### 例子：掩码（flags = flags & MASK）
 #### 提取前4位二进制
假设有一个8位的二进制数 10101010，我们想要提取前4位（高位）、
> 原始数据：10101010
> 掩码：11110000
> 结果：10100000
### 内存对齐
```c
_Alignas(align-value) type variable;
//假设我们有一个 char 类型的变量，但我们希望它在内存中按4字节对齐：
_Alignas(4) char c;
```

## C 标准库 - <limits.h>
| 宏             | 描述                                       | 值                                      |
|----------------|--------------------------------------------|-----------------------------------------|
| **字符类型**   |                                            |                                         |
| CHAR_BIT       | char 类型的位数                            | 通常为 8                                |
| CHAR_MIN       | char 类型的最小值（有符号或无符号）        | -128 或 0                               |
| CHAR_MAX       | char 类型的最大值（有符号或无符号）        | 127 或 255                              |
| SCHAR_MIN      | signed char 类型的最小值                   | -128                                    |
| SCHAR_MAX      | signed char 类型的最大值                   | 127                                     |
| UCHAR_MAX      | unsigned char 类型的最大值                 | 255                                     |
| **短整数类型** |                                            |                                         |
| SHRT_MIN       | short 类型的最小值                         | -32768                                  |
| SHRT_MAX       | short 类型的最大值                         | 32767                                   |
| USHRT_MAX      | unsigned short 类型的最大值                | 65535                                   |
| **整数类型**   |                                            |                                         |
| INT_MIN        | int 类型的最小值                           | -2147483648                             |
| INT_MAX        | int 类型的最大值                           | 2147483647                              |
| UINT_MAX       | unsigned int 类型的最大值                  | 4294967295                              |
| **长整数类型** |                                            |                                         |
| LONG_MIN       | long 类型的最小值                          | -9223372036854775808L                   |
| LONG_MAX       | long 类型的最大值                          | 9223372036854775807L                    |
| ULONG_MAX      | unsigned long 类型的最大值                 | 18446744073709551615UL                  |
| **长长整数类型** |                                          |                                         |
| LLONG_MIN      | long long 类型的最小值                     | -9223372036854775808LL                  |
| LLONG_MAX      | long long 类型的最大值                     | 9223372036854775807LL                   |
| ULLONG_MAX     | unsigned long long 类型的最大值            | 18446744073709551615ULL                 |

## 断言库（assert.h）
assert(BOOL);
like this:  
>Assertion failed: a, file D:\promgmme\1.c, line 5

