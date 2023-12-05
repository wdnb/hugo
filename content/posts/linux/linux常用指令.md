---
title: linux常用指令
date: 2015-06-13 19:08:42
tags:
  - command
categories:
  - Linux
---
## 查看所有安装的软件包

	rpm -qa  

## 建立软链接
`ln -s a b` 中的 a 就是源文件，b是链接文件名,其作用是当进入b目录，实际上是链接进入了a目录

## 安装本地rpm包

    yum localinstall -y xxx.rpm
	
## 关闭防火墙

	重启后生效 
	开启： chkconfig iptables on 
	关闭： chkconfig iptables off 

	即时生效，重启后失效 
	开启： service iptables start 
	关闭： service iptables stop 
## Linux关闭SELinux
     重启后永久性生效：
    修改/etc/selinux/config文件中设置SELINUX=disabled ，然后重启服务器。

    即时生效，重启后失效：
    使用命令`setenforce 0`

附：

    setenforce 1 设置SELinux 成为enforcing模式
    
    setenforce 0 设置SELinux 成为permissive模式
## 关闭蜂鸣声
	
	rmmod pcspkr

## 写入配置文件

	echo "alias pcspkr off">>/etc/modprobe.conf

## grep通过文件名查找文件所在位置

	    grep -rl 'filename' ./

-r 选项表示递归(recursive)遍历所有子目录
-l 选项表示只列出文件名
./ 是目录名, 表示当前文件夹   

##centos添加EPEL
    yum install epel-release