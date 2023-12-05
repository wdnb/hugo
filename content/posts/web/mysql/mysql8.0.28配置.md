---
date: 2022-04-25 00:00:00
tags:
  - mysql
title: mysql 8.0.28配置
categories:
  - Web
---

## mysql 8.0.28配置

### 登录的用户名不可用这个ip登录错误
一般网上都推荐直接开放远程登录 添加一个%的root账户，但是我个人不是很喜欢权限彻底开放，个人是docker里的php访问mysql报以下错误 只要把所有的局域网网段加入登录授权就行了 `10.*，127.*，192.168.*`

`SQLSTATE[HY000] [1130] Host '192.168.16.2' is not allowed to connect to this MySQL server`

#### 登录mysql

`mysql -uroot -p`

#### 选择数据库

```show databases;```
```use mysql;```

#### 创建用户使用192网段

```create user 'root'@'192.168.%'IDENTIFIED BY '123456';```

#### 授予该用户所有权限

8.0以上版本用以下指令进行授权

```GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.%' WITH GRANT OPTION;```

#### 将当前user和privilige表中的用户信息/权限设置从mysql库(MySQL数据库的内置库)中提取到内存里

```FLUSH PRIVILEGES;```

#### 检测自己添加的账号是否添加成功

```select user,host from mysql.user;```
