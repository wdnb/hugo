---
title: centos iptables firewalld 开放端口
date: 2017-03-03 18:57:22
tags:
  - linux
categories:
  - Command
---

# centos6.X
```
iptables -I INPUT -p udp --dport 137 -j ACCEPT 
iptables -I INPUT -p udp --dport 138 -j ACCEPT 
iptables -I INPUT -p tcp --dport 139 -j ACCEPT    
iptables -I INPUT -p tcp --dport 445 -j ACCEPT 
iptables-save
service iptables save
chkconfig iptables on 
```
# centos7.x
centos7使用firewalld代替了原来的iptables
开启端口

    firewall-cmd --zone=public --add-port=80/tcp --permanent
命令含义：
 --zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
 --permanent   #永久生效，没有此参数重启后失效
重启防火墙

    firewall-cmd --reload