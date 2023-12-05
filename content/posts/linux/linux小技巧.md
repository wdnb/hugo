---
date: 2016-09-06 11:16:00
status: public
tags:
  - command
title: linux小技巧
categories:
  - Linux
---

## ubuntu下unzip中文乱码
使用`unzip -O cp936`解压缩即可
## 执行shell出现bad interpreter 

    :set ff
可以看到dos或unix的字样. 如果的确是dos格式的, 那么你可以用set ff=unix把它强制为unix格式的, 然后存盘退出. 再运行一遍看.
## Linux修改字符集
修改Linux服务器的配置文件：

    vim /etc/sysconfig/i18n
如果安装系统的时候选择了中文系统，则把LANG字段改为：
LANG="zh_CN.UTF-8"
如果安装系统的时候选择的英文系统，则把LANG字段改为：
LANG="en_US.UTF-8"

##centos6.5yum安装软件出现[Errno 256] No more mirrors to try

    yum clean metadata
    yum clean all

## linux文件权限说明

0 – no permission
1 – execute
2 – write
3 – write and execute
4 – read
5 – read and execute
6 – read and write
7 – read, write, and execute

## linux系统时间修改
1.调整年月日
    
    date -s 05/10/2009 
2.调整时分秒
    
    date -s 10:18:00