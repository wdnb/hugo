---
title: linux常用指令
date: 2015-06-13 19:08:42
tags:
  - command
categories:
  - Linux
---
# linux常用指令

## 通用

### linux检测远程端口是否打开

```bash
nc -v ip port，如nc -v 172.17.193.18 5902
```
根据显示的Connected信息确定端口是否打开。

若显示：Ncat:Connected to 172.17.193.18:5902.
则表示远程端口已打开。
若显示：Ncat:Connection refused.
则表示远程端口未打开。

### 查看io信息

```bash
iotop
```

### 查看网络流量

```bash 
iftop
```

```bash
iptraf-ng
```

### 建立软链接
其中的 a 就是源文件，b是链接文件名,其作用是当进入b目录，实际上是链接进入了a目录
```bash
ln -s a b
```

### linux文件权限说明
```bash
0 – no permission
1 – execute
2 – write
3 – write and execute
4 – read
5 – read and execute
6 – read and write
7 – read, write, and execute
```
### linux系统时间修改
1.调整年月日
```bash
date -s 05/10/2009
```
2.调整时分秒
```bash
date -s 10:18:00
```
### Linux关闭SELinux

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

### 关闭蜂鸣声
```bash 
rmmod pcspkr
```

#### 写入配置文件

```bash
echo "alias pcspkr off">>/etc/modprobe.conf
```

### grep通过文件名查找文件所在位置
-r 选项表示递归(recursive)遍历所有子目录
-l 选项表示只列出文件名
./ 是目录名, 表示当前文件夹   
```bash
grep -rl 'filename' ./
```
### 常见问题

#### 执行shell出现bad interpreter 
```vim
:set ff
```
可以看到dos或unix的字样. 如果的确是dos格式的, 那么你可以用set ff=unix把它强制为unix格式的, 然后存盘退出. 再运行一遍看.
#### linux修改字符集
修改Linux服务器的配置文件：
```bash
vim /etc/sysconfig/i18n
```	
如果安装系统的时候选择了中文系统，则把LANG字段改为：
LANG="zh_CN.UTF-8"
如果安装系统的时候选择的英文系统，则把LANG字段改为：
LANG="en_US.UTF-8"

## ubuntu
### 常见问题
#### ubuntu下unzip中文乱码
使用以下指令解压缩即可
```bash
unzip -O cp936
```

## centos

### centos添加EPEL
```bash
yum install epel-release
```

### 安装本地rpm包

```bash
yum localinstall -y xxx.rpm
```

### 查看所有安装的软件包
```bash
rpm -qa  
```

### 常见问题

#### centos6.5yum安装软件出现[Errno 256] No more mirrors to try
```bash
    yum clean metadata
```
```bash
    yum clean all
```
#### 文件乱码 无效的编码
```bash
yum install convmv
```
##### convmv 使用方法：
```bash
convmv -f 源编码 -t 新编码 [选项] 文件名
```
##### 常用参数：

    -r 递归处理子文件夹
    
    –notest 真正进行操作，默认情况下是不对文件进行真实操作
    
    –list 显示所有支持的编码
    
    –unescap 可以做一下转义，比如把%20变成空格


##### 应用举例：
```bash
convmv -f GBK -t UTF-8 --notest  -r *
```
这句命令会对当前目录及子目录下所有的文件进行处理。