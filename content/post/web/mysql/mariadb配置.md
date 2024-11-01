---
date: 2020-07-30T00:24:00Z
tags:
  - mariadb
title: mariadb配置
categories:
  - Web
---
# mariadb配置

## mariadb远程访问

### 增加远程访问账户

添加远程访问账户

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
```

将当前user和privilige表中的用户信息/权限设置从mysql库(MySQL数据库的内置库)中提取到内存里

```sql 
FLUSH PRIVILEGES
```

看是否多了个%的账号
```sql 
select user,host,password from mysql.user
```

### mariadb设置远程访问

MariaDB为了提高安全性，默认只监听127.0.0.1中的3306端口并且禁止了远程的TCP链接

注释bind-address项以使得所有ip能够访问mariadb

查找
```bash
cd /etc/mysql/
```

```bash
grep -rn "skip-networking" *
```
这里能看到skip-networking这个选项所在配置文件名字是50-server.cnf
编辑文件注释掉bind-address
```bash
vim /etc/mysql/mariadb.conf.d/50-server.cnf
```
重启mariadb
```bash
systemctl restart mysql
```