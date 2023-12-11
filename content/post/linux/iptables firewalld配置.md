---
title: iptables firewalld配置
date: 2017-03-03 18:57:22
tags:
  - iptables
  - firewalld
  - command
categories:
  - Linux
---
## iptables
### 开放端口
```bash
iptables -I INPUT -p udp --dport 137 -j ACCEPT 
iptables -I INPUT -p udp --dport 138 -j ACCEPT 
iptables -I INPUT -p tcp --dport 139 -j ACCEPT    
iptables -I INPUT -p tcp --dport 445 -j ACCEPT 
```
```bash
iptables-save
```
```bash
service iptables save
```
```bash
chkconfig iptables on 
```
### 关闭iptables
**非本地环境不推荐关闭防火墙**

重启后生效 
```bash
chkconfig iptables on 
chkconfig iptables off 
```
即时生效，重启后失效 
```bash
service iptables start 
service iptables stop 
```

## firewalld

### 开放端口

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
命令含义：
```bash
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent   #永久生效，没有此参数重启后失效
```

重启防火墙
```bash
firewall-cmd --reload
```
