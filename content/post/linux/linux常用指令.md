---
title: linux常用指令
date: 2015-06-13 19:08:42
tags:
  - command
categories:
  - Linux
---
# 通用

## linux检测远程端口是否打开

```bash
nc -v ip port，如nc -v 172.17.193.18 5902
```
根据显示的Connected信息确定端口是否打开。

若显示：Ncat:Connected to 172.17.193.18:5902.
则表示远程端口已打开。
若显示：Ncat:Connection refused.
则表示远程端口未打开。

## 查看io信息

```bash
iotop
```

## 查看网络流量

```bash 
iftop
```
```bash
iptraf-ng
```

## 建立软链接
其中的 a 就是源文件，b是链接文件名,其作用是当进入b目录，实际上是链接进入了a目录
```bash
ln -s a b
```


## iptables

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
## Linux关闭SELinux

重启后永久性生效：
`修改/etc/selinux/config文件中设置SELINUX=disabled ，然后重启服务器。`

即时生效，重启后失效：
使用命令

```bash 
setenforce 0
```

附：
setenforce 1 设置SELinux 成为enforcing模式   
setenforce 0 设置SELinux 成为permissive模式

## 关闭蜂鸣声
```bash 
rmmod pcspkr
```
### 写入配置文件

```bash
echo "alias pcspkr off">>/etc/modprobe.conf
```

## grep通过文件名查找文件所在位置
-r 选项表示递归(recursive)遍历所有子目录
-l 选项表示只列出文件名
./ 是目录名, 表示当前文件夹   
```bash
grep -rl 'filename' ./
```

# centos

## centos添加EPEL
```bash
yum install epel-release
```

## 安装本地rpm包

```bash
yum localinstall -y xxx.rpm
```

## 查看所有安装的软件包
```bash
rpm -qa  
```
