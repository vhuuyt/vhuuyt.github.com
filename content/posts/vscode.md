---
title: "win vscode中搭建c/c++环境"
date: "2022-04-19"
tags: ["编辑器","编程"]
---

# 下载准备

[7z](https://sparanoid.com/lab/7z/download.html)是一个开源免费无广告的解压软件。
[vscode&MinGW64的安装包](https://wwc.lanzoul.com/b0218oj0f)(密码：9p2f，MinGW64是我从[小熊猫c++](https://royqh1979.gitee.io/redpandacpp/)里面拿来的)

> 啊啊啊，吹爆了，[Cygwin](https://cygwin.com/install.html)能安装好多Linux下的工具，Gnome居然也可以！make也可以!用这个集成工具安装MinGW64明显简单了许多啊，这么好的工具才发现！是在看一个[用C语言实现一个编辑器](https://viewsourcecode.org/snaptoken/kilo/index.html)的项目里看到的。

安装vscode，并将MinGW64**解压**到一个位置（建议C盘根目录），复制MinGW64里面的bin文件夹的**路径**。

> 解压这里，解压完会嵌套一层文件夹，把里面那个MinGW64剪切出来替换掉外面那个就好啦。

# 配置系统变量

进入**设置**，搜索进入->**查看高级系统设置**，**高级**->(右下角)**环境变量**->选中**系统变量**栏的*Path*->点击**编辑**->**新建**->粘贴刚才复制的**bin文件夹目录**->确定返回

`win + R`键输入`cmd`进入小黑窗，输入`gcc -v`查看是否设置成功，返回 gcc 版本信息则成功。

# vscode 配置

### 新建 hello.c

按提示**建立工作文件夹**后，新建`hello.c`文件，输入测试程序

```c
#include <stdio.h>
int main(void)
{
    puts("hello");
    return 0;
}
```

### 编译配置

1. 左侧边栏进入 `Extensions`，搜索安装 C/C++ 扩展（**紫圈白条的那个**）
2. *Terminal* -> *Confingure Default Build Task* -> 选择`C/C++:gcc.exe 生成活动文件` 即编译器路径是 `C:/MinGW64/bin/gcc.exe` 的一项，之后将自动生成 `tasks.json`。

### 调试配置

1. 打开 `hello.c` 文件，按左侧边栏 `Run and Debug` 按钮，弹出窗口选择`GCC(GDB/LLDB)`，将自动生成 `launch.json`。
2. 点击右下角的 `Add configure` 选择 `c/c++:(gdb)启动`。
3. 之后将`"program"`参数中的汉字去掉，使其变成 `${fileDirname}\\${fileBasenameNoExtension}.exe` 即可
4. `miDebuggerPath` 的参数改为 `C:/MinGW64/bin/gdb.exe`（即你的MinGW64中的gdb.exe路径）

# 快捷键

`Ctrl Shift B` 生成`.exe`可执行文件
`F9` 标记断点（鼠标点代码行号前的空白也可）
`F5` 进行 Debug
`F11` step into 下一步（会**进入函数内部**）
`F10` step over 则**不会进入**调用库函数及自定义函数的内部

> 前天，在笔记本上用vscode的时候，`Ctrl+Shift+b`不能生成可执行文件，但是用 PowerShell 和 cmd 就都可以，今天（22.4.26）终于破案啦，原来是我安的users版的原因，换成系统全局安装版就没问题啦，开心。

