---
date: 2015-06-13 19:15:00
status: public
tags:
  - mysql
title: mysql建数据库
categories:
  - Web
---

## 新建用户
 
登录MYSQL

    mysql -u root -p

创建用户

    insert into mysql.user(Host,User,Password) values('localhost','waf_alarm',password('waf_alarm'));

刷新系统权限表

	flush privileges;

这样就创建了一个名为：waf_alarm 密码为：waf_alarm 的用户。
 
为用户授权
 
登录MYSQL（有ROOT权限）。我里我以ROOT身份登录.

    mysql -u root -p

首先为用户创建一个数据库(waf_alarm_db)

    create database waf_alarm_db;

授权用户拥有waf_alarm_db数据库的所有权限

    grant all privileges on waf_alarm_db.* to 'waf_alarm'@'localhost' identified by 'waf_alarm';
    
刷新系统权限表

    flush privileges;

 
如果想指定部分权限给一用户，可以这样来写:

	grant select,update on jeecnDB.* to wordpress@localhost identified by 'wordpress';

刷新系统权限表。

	flush privileges;
    grant 权限1,权限2,…权限n on 数据库名称.表名称 to 用户名@用户地址 identified by ‘连接口令’;

 
权限1,权限2,…权限n代表select,insert,update,delete,create,drop,index,alter,grant,references,reload,shutdown,process,file等14个权限。
当权限1,权限2,…权限n被all privileges或者all代替，表示赋予用户全部权限。
当数据库名称.表名称被*.*代替，表示赋予用户操作服务器上所有数据库所有表的权限。
用户地址可以是localhost，也可以是ip地址、机器名字、域名。也可以用’%’表示从任何地址连接。
‘连接口令’不能为空，否则创建失败。