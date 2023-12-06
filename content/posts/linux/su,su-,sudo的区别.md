---
date: 2016-02-03 16:06:00
tags:
  - command
title: su,su -,sudo的区别
categories:
  - Linux
---

解析su,su -,sudo的区别
作者：Warm Color
肯定有人不知道下面两个命令的区别,
```
	[warmcolor@PC ~]$ su
	[warmcolor@PC ~]$ su - ##(有个减号)
```

那下面两个命令的区别呢?

```
	[warmcolor@PC ~]$ su
	[warmcolor@PC ~]$ sudo su
```
 
首先,su,su -这两个命令都能获得root权限,
但root的密码是不能随便交给别人的,这时就需要sudo命令了,
使用用户自己的密码,临时赋予一般用户root权限,
sudo的运行过程是这样的:

1.	检查用户是否在/etc/sudoers的列表中,
2.	如果在,以root权限执行命令,
3.	取消用户的root
 
接着说说这三个命令的区别:
下面是su的过程:
```

[warmcolor@PC ~]$ su
 
密码：
 
[root@PC warmcolor]# pwd
 
/home/warmcolor
 
[root@PC ~]# echo $PATH
 
/usr/lib/qt-3.3/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/warmcolor/bin
```
下面是su -的过程:
```
[warmcolor@PC ~]$ su -
密码：
 
[root@PC ~]# pwd
 
 /root
 
[root@PC ~]# echo $PATH
 
/usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```
下面是sudo的过程:
```
[warmcolor@PC ~]$ sudo pwd
 
[sudo] password for warmcolor:
 
/home/warmcolor
 
[warmcolor@PC ~]$ sudo echo $PATH
 
/usr/lib/qt-3.3/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/warmcolor/bin
```
可以看出su和sudo没有切换工作目录和环境变量,只是赋予用户权限,
而su -是真正切换到root登录,工作目录切换到/root,环境变量也同时改变.
而网上还有一个说法,sudo 默认将原有的环境变量 reset,只保留一些对安全没有影响设定.
 
至于上面第二个问题,答案其实很简单,
同样切换到root登录,
su使用root的密码,而sudo su使用用户密码.
 
上述命令更为具体的描述请参见man手册.
  原文链接: 
  >http://blog.warmcolor.net/?p=3542