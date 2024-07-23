---
layout : post
title : "GDB调试工具学习笔记"
author : "法厄异"
header-style: text
catalog : true
tags:
     -GBD
---

# GDB学习笔记

## 一、首先编译的时候要加上-g来告诉编译器加上调试信息

1、用g++编译器的时候：

```
**g++  test.cpp  -g**
```

2如果是make编译的话，在CMakeLists.txt中加上：

```c++
**\# 只在Debug模式下添加-g**

**if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")**

 **set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")**

**endif()**
```

然后在cmake时用这条语句

```sh
cmake -DCMAKE_BUILD_TYPE=Debug ..
```



或者直接在CMakeLists.txt中加上：

```c++
**set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")**
```

然后直接 **cmake ..** 就行了



## 二、执行有以下命令（仅展示本人常用的命令）

### 1、首先是进入gdb，gdb+（已编译的可执行文件）

```c++
gdb a.out
```



### 2、断点设置

（1）通过行号设置断点

```c++
b(breakpoint) + 行号 或者  b(breakpoint) + 源文件：行号
```



（2）通过函数名断点

```shell
b(breakpoint) + 源文件：函数名
```



（3）删除断点

删除单个断点

```shell
d（delete）+断点编号
```

或删除全部断点

```shell
d + breakpoints
```



（4）使所有断点无效

```shell
disable  b
```

使所有断点有效

```shell
enable b(breakpoints)
```



（5）查看设置的断点信息

```
info b
```



3、运行

```
r（run）
```



4、逐过程（不会进入函数内部）

```
n（next）
```



5、逐语句（按着程序一步步走下去，例如遇到printf会进入函数内部）

```
s(step)
```



6、修改变量的值

```
set var 变量名=值
```



7、打印变量

```
p（printf）
```



8、跟随查看变量，每次停下来都会显示变量的值

```
display 变量名
```



9、取消变量跟随查看

```
undisplay 变量名
```



10、从一个断点处，直接运行至下一个断点处

```
c(continue)
```



（不输入命令按enter，会执行上一次执行的命令）







# 以下由ai生成

GDB（GNU Debugger）是一个强大的命令行调试工具，用于调试C、C++等语言编写的程序。下面是一些GDB的基本命令，可以帮助你在调试过程中定位问题和理解程序的运行状态：

1. **启动与配置**
   - `file <program>`: 加载要调试的程序文件。
   - `run [arguments]`: 运行程序，可以传入参数。
   - `set args <arguments>`: 设置程序的启动参数。
   - `set environment <variable> <value>`: 设置环境变量。
   - `set confirm off`: 关闭确认提示，使某些操作不需要确认直接执行。
2. **断点管理**
   - `break <function>` 或 `b <function>`: 在指定的函数开始处设置断点。
   - `break <filename>:<line>` 或 `b <filename>:<line>`: 在指定文件的某一行设置断点。
   - `info breakpoints` 或 `i b`: 显示所有已设置的断点。
   - `delete <number>` 或 `d <number>`: 删除指定编号的断点。
   - `clear <location>`: 清除在指定位置的断点。
3. **控制流程**
   - `continue` 或 `c`: 继续执行直到下一个断点或程序结束。
   - `step` 或 `s`: 单步执行，进入函数调用。
   - `next` 或 `n`: 单步执行，但不进入函数调用。
   - `finish`: 执行直到当前函数返回。
   - `until <line>`: 执行直到到达指定行。
4. **检查状态**
   - `list` 或 `l`: 显示源代码。
   - `backtrace` 或 `bt`: 显示当前调用堆栈。
   - `info registers` 或 `i r`: 查看寄存器的值。
   - `print <expression>` 或 `p <expression>`: 计算表达式并打印结果。
   - `x/<format> <address>`: 检查内存地址的内容。
5. **修改状态**
   - `set variable <var> = <value>`: 修改变量的值。
   - `set $<register> = <value>`: 修改寄存器的值。
6. **查看和设置断点条件**
   - `condition <num> <expr>`: 为断点设置条件。
   - `commands <num>`: 设置断点触发后的命令列表。
7. **其他常用命令**
   - `quit` 或 `q`: 退出GDB。
   - `help <command>` 或 `h <command>`: 获取关于某个命令的帮助信息。
