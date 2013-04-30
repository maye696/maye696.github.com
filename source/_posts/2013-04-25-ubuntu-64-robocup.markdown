---
layout: post
title: "64位ubuntu下缺少32位类库的一些解决思路"
date: 2013-04-25 13:32
comments: true
categories: Coding 
---


当我试图在64位Ubuntu下搭建robocup的server时，会出现

    error while loading shared libraries: libQtGui.so.4: cannot openshared object file: No such file or directory
    error while loading shared libraries: libXss.so.1: cannot open shared object file: No such file or directory

这个错误很让人恼火，因为server的搭建是完全不可能在进行下去了。经过一番搜索，我发现这是因为64位的linux系统里缺少32位的专有类库的原因。

为了解决这些令人头大的依赖问题，需要手动安装依赖类库，解决方法如下：


<!--more-->


1. 首先启用 multiarch 支持(如果没有启用的话,估计多数 64 位 Ubuntu 用户早已启用了):

        echo foreign-architecture i386 | sudo tee /etc/dpkg/dpkg.cfg.d/multiarch
2. 然后安装官网的 64 位包中没有写出的 32 位依赖库:


        sudo apt-get install libxss1:i386 libqtcore4:i386 libqt4-dbus:i386 libqtgui4:i386



P.S. 分享一下在搜索解决办法的过程中遇到的几个实用命令：
要想察看一个文件在机器上保存的位置，可以使用locate命令，如


    locate libXss.so.1


在我的机器上会输出：

    /usr/lib/i386-linux-gnu/libXss.so.1
    /usr/lib/i386-linux-gnu/libXss.so.1.0.0
    /usr/lib/x86_64-linux-gnu/libXss.so.1
    /usr/lib/x86_64-linux-gnu/libXss.so.1.0.0


如果要搜索一个文件存在于哪些软件包中，可以使用apt-file这款实用工具，如


    apt-file search libXss.so


示例输出：

	ia32-libs: /usr/lib32/libXss.so
	ia32-libs: /usr/lib32/libXss.so.1
	ia32-libs: /usr/lib32/libXss.so.1.0.0
	libxss-dev: /usr/lib/libXss.so
	libxss1: /usr/lib/libXss.so.1
	libxss1: /usr/lib/libXss.so.1.0.0
	libxss1-dbg: /usr/lib/debug/usr/lib/libXss.so.1.0.0