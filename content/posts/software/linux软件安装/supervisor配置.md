---
date: 2021-03-04 16:32:00
tags:
  - supervisor
categories:
  - Software
title: supervisor配置
---
# supervisor配置
## nodejs安装

> https://github.com/nodejs/help/wiki/Installation

## how-to-install-pip-on-centos-7

> https://linuxize.com/post/how-to-install-pip-on-centos-7/

## 安装
 
> http://supervisord.org/installing.html#installing-to-a-system-with-internet-access

## 配置

> http://supervisord.org/installing.html#creating-a-configuration-file

### supervisor的启动

#### supervisord二进制启动

```bash
supervisord -c /home/g4aaaaz/mywork/go/supervisord.conf
```

#### 检查进程

```bash
ps aux | grep supervisord
```

## 基本操作

### 停止全部进程
```bash
/usr/bin/supervisorctl stop all
```
### 杀死程序
```bash
  ps aux | grep supervisord
```
```bash
  kill PID
```

一、添加好配置文件后

二、更新新的配置到supervisord    
```bash
supervisorctl update
```
三、重新启动配置中的所有程序
```bash
supervisorctl reload
```
四、启动某个进程(program_name=你配置中写的程序名称)
```bash
supervisorctl start program_name
```
五、查看正在守候的进程
```bash
supervisorctl
```
六、停止某一进程 (program_name=你配置中写的程序名称)
```bash
pervisorctl stop program_name
```
七、重启某一进程 (program_name=你配置中写的程序名称)
```bash
supervisorctl restart program_name
```
八、停止全部进程
```bash
supervisorctl stop all
```
注意：显示用stop停止掉的进程，用reload或者update都不会自动重启。
