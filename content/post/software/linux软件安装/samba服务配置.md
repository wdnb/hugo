---
date: 2015-06-13T19:15:00Z
tags:
  - sabma
title: samba服务配置
categories:
  - Software
---

## 安装

    yum install samba

## 将root用户添加到samba用户当中

    smbpasswd -a root    

## 开机启动 

    chkconfig smb on

## 防火墙开放端口（或者是直接关闭 /etc/init.d/iptables stop） 
开放端口 
    
    iptables -I INPUT -p udp --dport 137 -j ACCEPT 
    iptables -I INPUT -p udp --dport 138 -j ACCEPT 
    iptables -I INPUT -p tcp --dport 139 -j ACCEPT    
    iptables -I INPUT -p tcp --dport 445 -j ACCEPT 
    iptables-save
    service iptables save
    chkconfig iptables on 

## 配置sabam服务访问软链接权限

    [global] 
    unix extensions  = No

## 共享目录

    vim /etc/samba/smb.conf
      
    [coding]
    path = /html
    public = yes
    writable = yes
    printable = no
##重启启服务

    service smb restart
##在windows下连接samba服务器
映射网络驱动器
地址类似
 
    \\192.168.25.168\H3C
前半段为IP,后半段为smb.conf里所配置的目录名称

## Linux下连接指令

smbclient  `//209.141.51.244/root -U root`

## win下清除网络认证
 首先通过开始---运行---cmd输入：“net use”命令查看现有的连接，然后执行“net use \\Samba服务器IP地址或者netbios名称\ipc$  /del”，删除Samba服务器已经建立的连接。或者执行“net use * /del”将现在所有的连接全部删除。最后，再次执行“\\ip地址”时，就可以切换用户了。

## win10无法映射网络驱动器解决办法

将计算机账户设置为本地账户，如使用微软账户则无法登陆
打开注册表 Win+R输入regedit,定位到如下位置 HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters 创建一个 DWORD 项，命名为 ‘AllowInsecureGuestAuth’ ，值设置为“1”.


 最后重启计算机再次访问查看结果。

参考地址
>http://einverne.github.io/2015/07/12/windows-10-cannot-connect-openwrt-samba.html