---
layout: post
title: "fedora下搭建robocup模拟运行环境 : simspark"
date: 2013-04-25 09:24
comments: true
categories: Coding
---

大二下学期加入了学校的robocup模拟组，本想学习后参加比赛的，没想到模拟组后来的比赛都取消了。。。。学校也取消了模拟组的活动。。。这就是我的命运吗？。。。。等于学到的一点东西都没用了。还是把一些总结发在这里吧，有个纪念。




获得超级管理员权限，即先在终端输入su，再输入密码，此时命令开头符号由$变为#。


下载安装simspark 所需的库文件。终端输入

    yum –y install gcc-c++ doxygen
    boost boost-devel freetype freetype-devel freeglut freeglut-devel ruby rubydevel
    SDL* DevIL DevIL-devel ode ode-devel





（以下步骤与Ubuntu下安装步骤相同）

<!--more-->

将simspark-0.2.2和rcssserver3d0.6.5解压到主文件夹下。


使用终端打开到simspark-0.2.2文件下


         # mkdir build
         # cd build
         # cmake ..
         # make
         # make install


同样的操作在rcssserver3d0.6.5下进行

上面步骤结束后server 便安装完毕了，终端运行rcsoccersim3d 即可，平台
simspark出现。

 卸载server 时，在两个文件下的build 文件中分别运行make uninstall，但顺序要
与安装相反，即先卸载rcssserver3d，再卸载simspark。














![](http://f-1.tuzhan.com/893f001cfacb/p-2/l/2013/04/25/09/24d8c16ff177401a9420e707ce959ae1.jpg)