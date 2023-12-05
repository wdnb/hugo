---
title: EmbedSky交叉编译
date: 2015-06-14 13:50:38
tags:
  - EmbedSky
categories:
  - Embedded
---
1.编译前准备

解压缩	

	tar xfv 文件名

编译交叉编译工具需要以下目录
	
	u-boot-1.1.6 crosstools_3.4.5_softfloat

进入u-boot-1.1.6修改第128行的Makefile 交叉编译工具路径

	CROSS_COMPILE = /home/c/opt/EmbedSky/crosstools_3.4.5_softfloat/gcc-3.4.5-glibc-2.3.6/arm-linux/bin/arm-linux-

设置配置单

	make EmbedSky_config
	make

make时修改目录出现寻找原路径问题使用
	
	make distclean 

2.内核编译

修改第194 行

	export KBUILD_BUILDHOST := $(SUBARCH)
	ARCH            = arm
	CROSS_COMPILE   = /home/lierda/source/opt/EmbedSky/4.3.3/bin/arm-none-linux-gnueabi-

	
	cp config_EmbedSky_W43 .config

3.完成

把usr目录下的mkyaff复制到用户sbin下
	
	sudo cp mkyaffs2image /usr/local/sbin/

编译

	mkyaffs2image root_TQ2440_PDA/ root_TQ2440