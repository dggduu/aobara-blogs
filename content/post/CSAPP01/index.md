+++
author = "aobara"
title = "CSAPP笔记01"
description = ""
date = "2024-11-04"
categories = [
    "编程",
    "笔记",
]
image = ""
+++


## 第一章：计算机系统漫游
### 编译系统的构成
![编译系统](1.png "Optional title")  
#### 预处理阶段
预处理器
(cpp)
根据以字符＃开头的命令，修改原始的C程序。结果就得到了另一个C程序，通常
是以.i作为文件扩展名。
#### 编译阶段
编译器(eel)将文本文件hello.i翻译成文本文件hello.s,它包含一个汇编语言程序.该程序包含函数main的定义(明文)
#### 汇编阶段
汇编器(as)hello.s翻译成机器语言指令，把这些指令打包成一种叫做可重定位目标程序(rlocatableobject program)的格式。

**hello.o文件是一个二进制文件，它包含的17节是函数main的指令编码如果我们在文本编辑器中打开hello.o文件，将看到一堆乱码**

#### 链接阶段
printf函数存在于一个名为printf.o的单独的预编译好了的目标文件中，而这个文件必须以某种方式合并到我们的hello.o。程序中链接器(Id)就负责处理这种合并