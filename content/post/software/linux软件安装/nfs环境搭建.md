---
title: nfs环境搭建
date: 2015-06-14T14:20:59Z
tags:
  - nfs
categories:
  - Software
---
 
## 方式一： 

	sudo ifconfig eth2 192.168.7.54 netmask 255.255.255.0 

说明：该种方式可以使改变即时生效，重启后会恢复为原来的IP 

## 方式二： 

说明：该方式要重启后生效，且是永久的 

UBUNTU

	sudo vim /etc/network/interfaces

CENTOS

	vim /etc/sysconfig/network-scripts/ifcfg-eth0

	 1 # Advanced Micro Devices [AMD] 79c970 [PCnet32 LANCE]
	  2 DEVICE=eth0
	  3 BOOTPROTO=none
	  4 HWADDR=00:0c:29:7e:b0:34
	  5 ONBOOT=yes
	  6 TYPE=Ethernet
	  7 USERCTL=no
	  8 IPV6INIT=no
	  9 PEERDNS=yes
	 10 NETMASK=255.255.255.0
	 11 IPADDR=192.168.7.51
	 12 GATEWAY=192.168.7.1

--------------------------------------------------------


	mount -t nfs -o nolock 192.168.7.54:/home/lierda/env/nfs /mnt

在用户目录下新建ENV文件夹，在ENV里新建用于放网络
文件系统的文件夹huzl_nfs

在终端里执行`sudo apt-get install portmap nfs-kernel-server`

安装好后再安装`sudo apt-get install portmap nfs-common`

然后`sudo gedit /etc/exports`

在文件的最后加上`/home/huzl/env/huzl_nfs/my_wlw_fs`

	*(rw,sync,no_root_squash)

保存退出。

重启nfs服务器

	sudo /etc/init.d/portmap restart
	sudo /etc/init.d/nfs-kernel-server restart

测试nfs服务器

	showmount –e