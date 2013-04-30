---
layout: post
title: "利用Openwrt实现远程音频播放过程记录"
date: 2013-04-23 22:26
comments: true
categories: Coding 
---
因为我采用的Tp-link wr703n路由器可用rom只有4M，并且openwrt系统会占用3.1M左右， 因此需要外接u盘安装软件包，外挂移动硬盘来存储需要远程传输的音频和视频文件。

一、挂载U盘和移动硬盘
 
具体方法：

 1.安装包

    block-mount
    kmod-usb-storage
    kmod-fs-ext4
    e2fsprogs




<!--more--> 




2.运行如下命令
 
    
    mke2fs -j /dev/sda1                           //格式化分区为ext4 
   
    mount /dev/sda /mnt                          //挂载U盘分区2到 /mnt 
    
    mkdir /tmp/root                              //在tmp下创建root目录 
    
    mount -o bind / /tmp/root                    //挂载并同步系统跟目录到/tmp/root 
    
    cp /tmp/root/* /mnt –a                       //把/tmp/root下的所有文件考到/mnt 即u盘 
    
    umount /tmp/root                              //卸载 /tmp/root
    






  3.按下图配置即可


![](\images\11.jpg)




二、搭建DLNA网络 构建流媒体播放平台

1.选择DLNA解决方案原因：

实现多平台流媒体播放目前有几种方法，一是http ftp，二是samba，三是dlna/upnp。一就放弃了，浏览起来太麻烦，而且很多特性不支持，samba呢太过消耗资源了，播放较大的视频就有点卡，dlna/upnp则是最理想的选择。目前linux有两个dlna/upnp server的实现，ushare 、minidlna。ushare已经停止开发，而且使用过程中经常出现段错误。因此我最终选择了minidlna来实现。





2.Minidlna配置


简单来说就是安装minidlna 然后修改/etc/config/minidlna 配置文件 按图配置即可

 
![](\images\12.jpg)
 

3.安卓设备测试


![](http://f-1.tuzhan.com/85c214dbb5b4/p-2/m/2013/04/24/14/61c44fbfd6984d44b8c7cd2284477e6e.jpg)
![](http://f-1.tuzhan.com/cbbdc4ee343e/p-2/m/2013/04/24/14/8948124d4d7348bb9e167dc1fd286345.jpg)
![](http://f-1.tuzhan.com/431b24e51f70/p-2/m/2013/04/24/14/1c4723cfd41e46d98a791843fa74521c.jpg)


4.iOS设备运行截图


![](http://f-1.tuzhan.com/f13a7530ce7b/p-2/m/2013/04/24/14/5158ae2e58cd4e12a909180ae1575acd.jpg)
![](http://f-1.tuzhan.com/627b178ac400/p-2/m/2013/04/24/14/7147d3cf12424cf8ae76ba67f59d9085.jpg)
![](http://f-1.tuzhan.com/e3db4efcef09/p-2/m/2013/04/24/14/dc0ba720baf5459a8e44eb46ff54f64c.jpg)


从运行截图中可以看到对音乐文件支持封面、流派显示等常用功能
音乐播放支持实时快进，拖动进度条即可。


利用这个系统可以实现远程传输老师上课的音频录音或者视频录像，在多种平台的移动设备上实时观看。

 
三、挂载usb声卡实现伪airplay效果


AirPlay无线技术允许用户在扬声器底座、影音接收器和立体声系统等设备上无线同步播放音乐。


利用openwrt加外挂usb声卡并外接音箱可以实现伪airplay效果，可以在手机等移动设备上控制音箱播放音频文件。


1.配置USB声卡

安装声卡内核模块

    opkg install kmod-input-core 
    opkg install kmod-soundcore 
    opkg install kmod-usb-audio


2.测试声卡是否工作正常
声卡与音箱连接并且插入含有MP3文件的USB存储设备
这里假设usb设备挂载路径为/mnt/sdb1

    opkg install madplay
    madplay /mnt/sba1/*.mp3


如正常发声则表示声卡驱动成功

3.安装配置MPD

    opkg install mdnsresponder
    opkg install libspeex
    opkg install mpd




4.配置mpd


![](\images\19.png)



5.启动mpd进程

    /etc/init.d/mpd restart


6.用手机控制播放

手机连接路由器，在android电子市场里搜索 MPDroid 并安装运行MPDroid，选择第二个默认连接 ，不要选通过wlan连接。在Host选项里填入路由器的IP地址，其它则默认，然后返回到主界面
此时即可用手机无线控制路由器的播放、暂停以及其它操作
 
**手机控制USB声卡播放运行截图**


 ![](http://f-1.tuzhan.com/dd89a5267ea9/p-2/m/2013/04/24/14/1a7a589a9a4742a1b4830469e11fb2e0.jpg)

 
**路由器实际运行图片**


![](http://f-1.tuzhan.com/c060a284cb73/p-2/m/2013/04/24/15/1df1f1a3e70b476b94e99597118fa365.jpg)


 