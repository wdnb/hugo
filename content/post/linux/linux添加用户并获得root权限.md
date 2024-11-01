---
date: 2016-09-03T15:23:00Z
tags:
  - command
title: linux添加用户并获得root权限
categories:
  - Linux
---

# 

## 添加一个名为vagrant的用户
```bash
adduser vagrant
```
## 修改密码
```bash
passwd vagrant
```
    
## 修改 /etc/sudoers 文件
```bash
vim /etc/sudoers
```
找到下面一行，在root下面添加一行，如下所示：
```bash
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
vagrant   ALL=(ALL)     ALL
```

vim强制保存并退出
```bash
:x
```