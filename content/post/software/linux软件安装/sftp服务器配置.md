---
date: 2015-06-13 19:16:00
tags:
  - ftp
title: sftp服务器配置
categories:
  - Software
---

## 安装vsftpd

    yum install vsftpd

## 配置

     vi /etc/vsftpd.conf

## 注销掉，关闭匿名访问

    #anonymous_enable=YES

## 去掉#让本地账号可以访问，比如root，等系统登录账号

    local_enable=YES 
    write_enable=YES

	你需要在`/etc/vsftpd/user_list`文件中把root那一行删除或者注释掉
	同理，`/etc/vsftpd/ftpusers`文件中的root也注释掉

## 设置开机启动

    chkconfig vsftpd on