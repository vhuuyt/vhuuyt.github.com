---
title: "gcc&gdb使用"
date: "2022-04-10"
tags: ["Linux","编程"]
---

# GCC

## 一、文件格式

因为 gcc 也支持 c++，所以.cpp  .hpp  .cp  .cxx  .c++  .h++ 可用

|    说明    | c后缀  |            c++后缀             |
| :--------: | :----: | :----------------------------: |
|  代码文件  |   .c   | .cc  .C  .cpp  .c++  .cp  .cxx |
|   头文件   |   .h   |           .hpp  .h++           |
|  预处理后  |   .i   |               .i               |
|  汇编代码  | .s  .S |             .s  .S             |
|  目标代码  |   .o   |               .o               |
| 静态链接库 |   .a   |               .a               |
| 动态链接库 |  .so   |              .so               |

## 二、示例

```bash
gcc -E hello.c -o hello.i	# 预处理
gcc -S hello.i -o hello.s	# 编译
gcc -c hello.s -o hello.o	# 汇编
gcc hello.o -o hello		# 链接
gcc hello.c -o hello		# 一次完成四阶段，不指定输出格式，会默认 a.out
gcc hello.c -I [要include的头文件的路径] -o hello
gcc hello.c -L [所需要的lib库文件] -o hello
# 助记： ESC iso
```

多文件，只需要编译修改部分即可，也可以使用 make 工具来增加效率

## 三、基本语法

```bash
gcc [选项] [选项 参数]
```

### 常用选项和参数

| 选项和参数                   | 说明                                                         |
| :--------------------------- | ------------------------------------------------------------ |
| -E                           | 预处理，预处理之后的代码送往标准输出                         |
| -S                           | 编译成汇编代码                                               |
| -c                           | 编译成目标文件，不链接库                                     |
| -B [目录]                    | 将 [目录] 添加到编译器的搜索路径中                           |
| -W [warn...]  --Wall         | 设置警告，[可选项很多]，--Wall 开启所有的警告                |
| -U [name]                    | 取消宏定义 name                                              |
| -I [dir...]  (是 i   的大写) | 把 dir 加到**头文件**的搜索路径中，而且 gcc命令<br />会在搜索标准头文件之前先搜索 dir |
| -l library  (是 L 的小写)    | 在链接时候搜索 library 库                                    |
| -L [dir]                     | 把 dir 加到**库文件**的搜索路径中，而且 gcc 命令<br />会在搜索标准库文件之前先搜索 dir |
| --pthread                    | 过 pthread 库加入对多线程的支持，这为预处理和链接<br />设置了标志。pthread 是 POSIX 指定的标准线程库 |
| --std=standard               | 针对 C 语言，--std=c99 表示编译器遵循 C99 标准               |

# GDB

## 一、简单示例

```bash
gcc -g test.c -o test 	# -g 将调试信息添加到程序里
gdb test 				# 进入 GDB，进入后，显示(gdb)。命令在其后输入
(gdb)
### 查看源代码
(gdb)l					# 使用 list(l) 子命令可以一次显示十行源代码
(gdb)l 14,20			# 显示 14 和 20 行
### 设置断点
(gdb)break 13			# 设置断点
(gdb)break func()		# 函数入口处设置断电
(gdb)info break			# 显示断点信息
### 运行调试
(gdb)r					# run(r)
(gdb)n					# next(n) 单步执行下一行
(gdb)c					# continue(c) 继续执行，直到下一个断点或程序结束
(gdb)p 变量名			  # print(p) 查看程序中变量的当前值
### 函数操作
(gdb)bt					# 查看函数的堆栈
(gdb)finish				# 退出函数
(gdb)q					# quit(q) 退出 gdb 工具
```

## 二、基本语法

```bash
gdb [option] --args prog [arguments]
```

### 1.常用的选项和参数

| 选项和参数                  | 说明                                             |
| --------------------------- | ------------------------------------------------ |
| --symbols [file]、-s [file] | 从指定文件中，读取符号表                         |
| --se [file]                 | 从指定文件中读取符号表信息，并用在可执行文件中   |
| --core [file]、-c [file]    | 调试时 core dump 的 core 文件                    |
| --directory [dir]、-d [dir] | 加入一个源文件的搜索路径（默认PATH所定义的路径） |

### 2.启动 gdb 的方法

```bash
gdb [programName]
gdb [programName] core # 用 gdb 工具同时调试一个运行程序和 core 文件，core 是程序非法执行后，core dump 产生的文件
gdb [programName] [PID] # 如果程序是一个服务程序，指定这个服务程序运行时的 PID，gdb 工具会自动 attach 上去，并调试。程序应该存在于 PATH 路径中
```

### 3.gdb 命令的子命令

(gdb)提示符后输入`help`，会列出 **gdb 子命令**的**所有分类**

- aliases：命令别名
- breakpoints：断点定义
- data：数据查看
- files：指定一个文件来查看
- internals：维护命令
- stack：调用栈查看
- status：状态查看
- tracepoints：跟踪程序执行

输入`help`后，再输入`apropos [分类名]`，可查看分类中的子命令用法（apropos /ˌæprəˈpəʊ/ prep.关于，至于）

#### 3.1常用 gdb 子命令如下

1. `file`：**装入**想要调试的可执行文件（仅输`gdb`进入gdb，再装入调试文件） 
2. `kill`：**终止**正在调试的文件
3. `list`：列出可执行文件源码的一部分
4. `next`：执行一行源代码但**不进入**函数内部
5. `step`：执行一行源代码并**进入**函数内部
6. `run`：执行待调试的程序
7. `quit`：终止并退出 gdb 工具
8. `watch`：监视一个变量的值，不管它何时被改变
9. `make`：不退出 gdb 工具就可以重新生成可执行文件
10. `shell`：不离开 gdb 工具就可以执行 UNIX Shell 命令
11. `print [变量名]`：显示变量名保存的值
12. `break [NUM]`：在第 num 行设置断点
13. `bt`：显示所有的调用栈帧。该子命令可用来显示函数的调用顺序
