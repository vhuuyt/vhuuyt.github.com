---
title: "计算机系统基础-课程实验（南大）"
date: "2022-12-31"
tags: ["Linux","瞎折腾"]
summary: "最喜欢蒋炎岩老师了！"
---

# Introduction to Computer System(s)-Programming Assignment

## PA0 2022/12/30

蒋炎岩老师还是保守了，PA0里写了预计平均耗时10小时，也可能我现在才看课程，晚了点，所以在环境搭建上碰了不少壁：（结论：在http://old-releases.ubuntu.com/releases/找到四月份发布的ubuntu-22.04的[torrent](http://old-releases.ubuntu.com/releases/jammy/ubuntu-22.04-desktop-amd64.iso.torrent)文件，然后去迅雷下载好，什么问题也没有。所以说，我还是看课程看晚了，羡慕南大学生啊）。下面是废话：

### gcc生成32位文件

- 开始是在自己的fedora上缺少`libc6-dev-i386`，哎不对，这是[操作系统：设计与实现](https://jyywiki.cn/OS/2022/)那门课上的问题，不过就看了一节课就来听这个前置课程了。用64位的gcc编译32位`gcc -m32`，需要这个库，但fedora上和ubuntu名字不一样，是叫`glibc-devel-i686`。

### fedora上的llvm

- 然后还是在我的fedora上，PA0里，`make menuconfig`之后的`make`出错了

	```bash
	make
	+ CXX src/utils/disasm.cc
	src/utils/disasm.cc: In function ‘void init_disasm(const char*)’:
	src/utils/disasm.cc:67:9: error: invalid use of incomplete type ‘class llvm::MCSubtargetInfo’
	   67 |     gSTI->ApplyFeatureFlag("+m");
	      |         ^~
	In file included from src/utils/disasm.cc:21:
	/usr/include/llvm/MC/MCAsmInfo.h:30:7: note: forward declaration of ‘class llvm::MCSubtargetInfo’
	   30 | class MCSubtargetInfo;
	      |       ^~~~~~~~~~~~~~~
	src/utils/disasm.cc:68:9: error: invalid use of incomplete type ‘class llvm::MCSubtargetInfo’
	   68 |     gSTI->ApplyFeatureFlag("+a");
	      |         ^~
	/usr/include/llvm/MC/MCAsmInfo.h:30:7: note: forward declaration of ‘class llvm::MCSubtargetInfo’
	   30 | class MCSubtargetInfo;
	      |       ^~~~~~~~~~~~~~~
	src/utils/disasm.cc:69:9: error: invalid use of incomplete type ‘class llvm::MCSubtargetInfo’
	   69 |     gSTI->ApplyFeatureFlag("+c");
	      |         ^~
	```

	问了Chatgpt，回答：

	> 这个错误信息表明在编译 src/utils/disasm.cc 文件时出现了问题。根据错误信息，在 src/utils/disasm.cc 第 67、68、69 行中使用了未定义的类 llvm::MCSubtargetInfo。
	>
	> 在 /usr/include/llvm/MC/MCAsmInfo.h 中只有类的前向声明，所以编译器无法在 src/utils/disasm.cc 中使用这个类。
	>
	> 要解决这个问题，可能需要检查 src/utils/disasm.cc 文件是否正确地包含了所需的头文件，以及这些头文件是否正确地包含了 llvm::MCSubtargetInfo 类的定义。
	>
	> 此外，还可能需要检查是否在编译时指定了正确的编译选项，以及是否存在其他与 llvm::MCSubtargetInfo 类相关的问题。

	好吧，我不会C++，也不懂“只有类的向前声明”，干脆放弃了，打算在kvm里装ubuntu（又开始踩坑了）

### 安装`libsdl2-dev`

- 先是在阿里云的服务器上，可以跑，但没GUI界面，放弃。

- ubuntu22.04.1，问题真不少。。。。安装`libsdl2-dev`软件包时出现了依赖问题，系统里有了一个版本为`249.11-0ubuntu3.4`的`libudev1`软件包，但`libudev-dev`需要的是版本为`249.11-0ubuntu3`的`libudev1`软件包。

	```bash
	sudo apt install libsdl2-dev
	Reading package lists... Done
	Building dependency tree... Done
	Reading state information... Done
	Selected version '2.0.20+dfsg-2build1' (Ubuntu:22.04/jammy [amd64]) for 'libsdl2-dev'
	Some packages could not be installed. This may mean that you have
	requested an impossible situation or if you are using the unstable
	distribution that some required packages have not yet been created
	or been moved out of Incoming.
	The following information may help to resolve the situation:
	
	The following packages have unmet dependencies:
	 libudev-dev : Depends: libudev1 (= 249.11-0ubuntu3) but 249.11-0ubuntu3.4 is to be installed
	E: Unable to correct problems, you have held broken packages.
	
	sudo apt remove libudev1
	Reading package lists... Done
	Building dependency tree... Done
	Reading state information... Done
	Some packages could not be installed. This may mean that you have
	requested an impossible situation or if you are using the unstable
	distribution that some required packages have not yet been created
	or been moved out of Incoming.
	The following information may help to resolve the situation:
	
	The following packages have unmet dependencies:
	 apt : Depends: libapt-pkg6.0 (>= 2.4.6) but it is not going to be installed
	 init : PreDepends: systemd-sysv
	 shim-signed : Depends: grub-efi-amd64-signed but it is not going to be installed or
	                        grub-efi-arm64-signed but it is not installable
	               Depends: grub2-common (>= 2.04-1ubuntu24)
	 util-linux : PreDepends: libudev1 (>= 183) but it is not going to be installed
	E: Error, pkgProblemResolver::Resolve generated breaks, this may be caused by held packages.
	```

	> 在安装`libsdl2-dev`软件包时出现了依赖问题。根据错误信息，这个问题是由于系统中已经安装了一个版本为`249.11-0ubuntu3.4`的`libudev1`软件包，而你正在尝试安装的`libudev-dev`软件包需要的是版本为`249.11-0ubuntu3`的`libudev1`软件包。
	>
	> 要解决这个问题，你可以尝试使用以下命令来卸载现有的`libudev1`软件包：
	>
	> sudo apt-get remove libudev1
	>
	> 然后再尝试重新安装`libsdl2-dev`软件包。
	>
	> 注意，这只是一种可能的解决方案，并不能保证对所有情况都适用。如果这个方法不起作用，你可以尝试使用其他方法来解决依赖问题。

	> 这个错误信息表明，当前系统存在一些依赖于`libudev1`软件包的软件包，因此无法卸载`libudev1`软件包。
	>
	> 根据错误信息中给出的信息，在卸载`libudev1`软件包后，系统将无法安装`libapt-pkg6.0`、`systemd-sysv`、`grub-efi-amd64-signed`、`grub-efi-arm64-signed`、`grub2-common`和`util-linux`软件包，这可能会导致一些系统功能无法正常使用。
	>
	> 因此，在卸载`libudev1`软件包之前，你需要考虑到这些软件包的依赖关系，并确保这些软件包不会因为卸载`libudev1`软件包而导致无法使用。
	>
	> 如果你确实希望卸载`libudev1`软件包，你可以尝试使用以下命令来强制卸载：
	>
	> sudo apt-get remove --force-remove-reinstreq libudev1
	>
	> 这会强制卸载`libudev1`软件包，但可能会导致系统的一些功能无法正常使用。因此，在执行此操作之前，你应该充分考虑它的后果，并确保你有足够的知识和技能来解决可能出现的问题。

	到底是我傻，还是Chatgpt傻，remove不成，也没敢听它的去强制卸载，然后去下载了`libudev1_249.11-0ubuntu3_arm64.deb`，然后安装

	```bash
	sudo dpkg -i libudev1_249.11-0ubuntu3_arm64.deb
	dpkg: warning: downgrading libudev1:amd64 from 249.11-0ubuntu3.4 to 249.11-0ubuntu3
	(Reading database ... 184505 files and directories currently installed.)
	Preparing to unpack libudev1_249.11-0ubuntu3_amd64.deb ...
	Unpacking libudev1:amd64 (249.11-0ubuntu3) over (249.11-0ubuntu3.4) ...
	Setting up libudev1:amd64 (249.11-0ubuntu3) ...
	Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
	
	sudo apt install libsdl2-dev
	Reading package lists... Done
	Building dependency tree... Done
	Reading state information... Done
	You might want to run 'apt --fix-broken install' to correct these.
	The following packages have unmet dependencies:
	 libsdl2-dev : Depends: libasound2-dev but it is not going to be installed
	               Depends: libdbus-1-dev but it is not going to be installed
	               Depends: libegl1-mesa-dev but it is not going to be installed
	               Depends: libgl-dev but it is not going to be installed
	               Depends: libgles-dev but it is not going to be installed
	               Depends: libglu1-mesa-dev but it is not going to be installed
	               Depends: libibus-1.0-dev but it is not going to be installed
	               Depends: libpulse-dev but it is not going to be installed
	               Depends: libsdl2-2.0-0 (= 2.0.20+dfsg-2build1) but it is not going to be installed
	               Depends: libsndio-dev but it is not going to be installed
	               Depends: libudev-dev but it is not going to be installed
	               Depends: libwayland-dev but it is not going to be installed
	               Depends: libx11-dev but it is not going to be installed
	               Depends: libxcursor-dev but it is not going to be installed
	               Depends: libxext-dev but it is not going to be installed
	               Depends: libxi-dev but it is not going to be installed
	               Depends: libxinerama-dev but it is not going to be installed
	               Depends: libxkbcommon-dev but it is not going to be installed
	               Depends: libxrandr-dev but it is not going to be installed
	               Depends: libxss-dev but it is not going to be installed
	               Depends: libxt-dev but it is not going to be installed
	               Depends: libxv-dev but it is not going to be installed
	               Depends: libxxf86vm-dev but it is not going to be installed
	 udev : Depends: libudev1 (= 249.11-0ubuntu3.4) but 249.11-0ubuntu3 is to be installed
	E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
	```

	我和机器人一起犯傻。。。。最后，`am-kernels`这个模块是编译成功了，那个按键弹窗是出来了，能用，但第二天就开不了机了，依赖乱成一团了，我猜是降级后依赖乱了，关机前自动修复了也还是坏了。

- 最后还是在http://old-releases.ubuntu.com/releases/找到了四月份发布的ubuntu-22.04的[torrent](http://old-releases.ubuntu.com/releases/jammy/ubuntu-22.04-desktop-amd64.iso.torrent)文件，然后去迅雷下载，直接一步到位安装好了，一路没问题。

## PA1 2022/12/31

### ccache的环境变量配置

它要加在`/usr/bin`之前才会有效。`export PATH=/usr/lib/ccache:$PATH`加入到`.bashrc`，然后`. ~/.bashrc'`，看了[The Linux Command Line](http://billie66.github.io/TLCL/)这本书（具体的shell编程部分还没看，哈哈）还是有好处的，不用跟着老师输入`source ~/.bashrc`了，哈哈。

老师说南大学生在大一下就在“数字逻辑与计算机组成”课程里学习过riscv32，酸了啊。可能riscv32对我又是个新坑，也不知道能不能补回来，好像有本组成原理的黑书就讲了risc-v架构，到时候需要再去看吧。

### 编程模型？

如果没有寄存器, 计算机还可以工作吗? 如果可以, 这会对硬件提供的编程模型有什么影响呢?读ISA手册

Program Counter, PC 在x86中, 它有一个特殊的名字, 叫`EIP`(Extended Instruction Pointer).

### 计算机是个数组逻辑电路

时序逻辑部件：存储器、计数器、寄存器
组合逻辑部件：加法器等

从状态机模型的视角来理解计算机的工作过程: 在每个时钟周期到来的时候, 计算机根据当前时序逻辑部件的状态, 在组合逻辑部件的作用下, 计算出并转移到下一时钟周期的新状态.
