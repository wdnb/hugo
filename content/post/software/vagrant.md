---
date: 2016-09-03 15:20:00
tags:
  - vagrant
title: vagrant
categories:
  - Software
---

制作自己的vagrant box

事先工作:
1.安装VirtualBox
2.安装Vagrant
3.在VirtualBox中安装操作系统，例如 CentOS

1.创建vagrant用户和用户目录，密码为vagrant
2.添加vagrant用户的公共密钥，文件为/home/vagrant/.ssh/authorized_keys(权限要为600)
3.密钥生成和authorized_keys配置可参考:  [ssh密钥生成](http://blog.jbface.com/post/linux/sshmi-yao)
4.在宿主机中执行vagrant package --base 虚拟机名称，这样会创建指定虚拟机的box
5.将制作好的Box添加到Vagrant环境中，vagrant box add name package.box
6.初始化运行环境,vagrant init name
7.运行Vagrant虚拟机，vagrant up
8.将上面生成的私钥私钥放置在宿主机目录下
在配置文件Vagrantfile新增以下2行配置(此文件在vagrant init后生成)

    config.ssh.private_key_path = './id_rsa'
    config.ssh.forward_agent = true

[vagrant使用教程](https://github.com/astaxie/Go-in-Action/blob/master/ebook/zh/01.3.md)

## vagrant错误处理
### No guest additions
    default: No guest additions were detected on the base box for this VM! Guest
    default: additions are required for forwarded ports, shared folders, host only
    default: networking, and more. If SSH fails on this machine, please install
    default: the guest additions and repackage the box to continue.
宿主机下执行,(可能会等待很长时间)

    vagrant plugin install vagrant-vbguest
