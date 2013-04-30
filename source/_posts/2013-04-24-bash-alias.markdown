---
layout: post
title: "利用bash的alias解决长难命令记忆问题"
date: 2013-04-24 14:03
comments: true
categories: Coding
---


智商比较低，有些较长较复杂的bash命令记不住。 突然想起来可以用alias创建别名，妈妈再也不担心我记不住bash命令啦！ 哦也！


Windows在个人文件夹下的.bash_profile文件里定义就可以。 如果没有这个文件用vim自己创建。
我自己的例子：


    alias fb='rake gen_deploy'
    alias ks='cd mayecn.com'
    alias xb='rake new_post'

