---
date: 2016-09-03 15:23:00
status: public
tags:
  - command
title: linux添加一个用户并且让用户获得root权限
categories:
  - Linux
---

linux添加一个用户并且让用户获得root权限

    #adduser vagrant  //添加一个名为vagrant的用户
    #passwd vagrant   //修改密码
    
修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：

    ## Allow root to run any commands anywhere
    root    ALL=(ALL)     ALL
    vagrant   ALL=(ALL)     ALL

wq!强制保存并退出