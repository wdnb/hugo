---
date: 2015-06-13 19:16:00
status: public
tags:
  - mysql
title: mysql基本操作
categories:
  - Web
---

当向表中插入字符串值（以及一些数据值时）。必须使用引号，否则MySQL认为它们是字段名：
自增主键的值设为NULL或者‘’；
包含引用标志的值需要在前面加上反斜线（转义符），不过数值不需要加引号：
## 显示命令 
1、显示数据库列表。 

    show databases; 

2、显示库中的数据表： 
选择数据库

    use mysql;
    show tables; 

3、显示数据表的结构： 

    describe 表名; 

4:查询表中的记录： 

    select * from 表名
##修改操作
1:修改表字段名称

    alter table T_VMD_VERSION change logid log_id int UNSIGNED NOT NULL

2:修改表字段的数据类型

    alter table T_USER_MESSAGE modify addtime INT;
    ALTER TABLE T_USER_MESSAGE  modify content longtext binary;

3:修改字段默认值

    alter table表名alter column字段名drop default; (若本身存在默认值，则先删除)
    alter table 表名 alter column 字段名 set default 默认值;(若本身不存在则可以直接设定)

4:修改表名字

    ALTER  TABLE table_name RENAME TO new_table_name； 

5:添加表字段

    ALTER TABLE TABLE_NAME ADD FIELDNAME TINYINT(1) UNSIGNED NOT NULL;
    // ALTER TABLE T_USER_MESSAGE ADD message_id int UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY;

##新建操作
1:建库： 

    create database 库名; 
   指定字符集建库

     CREATE DATABASE `test2` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
2:建表： 

    use 库名； 
    create table 表名 (字段设定列表)
##删除操作
1:删库和删表: 

    drop database 库名; 
    drop table 表名； 

   2:清空(删除)表记录： 
   
    DELETE FROM table1
    TRUNCATE TABLE table1
如果DELETE不加WHERE子句，那么它和TRUNCATE TABLE是一样的，但它们有一点不同，那就是DELETE可以返回被删除的记录数，而TRUNCATE TABLE返回的是0。
##数据导入导出
1、导入导出数据库表：
    登陆mysql，选择数据库后选择表导出 
    导出表
    
    select * from alarms into outfile '/home/mysql/bak.txt';
导入备份    
    
    load data local infile '/home/mysql/bak.txt' into table alarms;
2、导入导出数据库：
    登陆mysql，选择数据库后选择表导出 
    导出数据库
    
    mysqldump -u用户名 -p密码 数据库名 > 数据库名.sql
导入数据库 (需要先建好数据库,选中数据库(use database) )

    source /home/abc/abc.sql;
##用户操作
1:创建用户

    insert into mysql.user(Host,User,Password) values('localhost','waf_alarm',password('waf_alarm'));

刷新系统权限表

	flush privileges;

这样就创建了一个名为：waf_alarm 密码为：waf_alarm 的用户。
2: 删除用户

        use mysql;
        select host,user,password from user;
        DELETE FROM user WHERE User="waf_alarm" and Host="localhost";
        flush privileges;
3:修改密码

    update mysql.user set password=password(‘新密码’) where User=waf_alarm and Host=”localhost”;
    flush privileges;
## 修改默认字符集

    vim/etc/my.cnf

在[client]下添加

    default-character-set=utf8

在[mysqld]下添加

    default-character-set=utf8
    
9.使得允许远程连接mysql数据库
允许mysql的3006端口

    iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
    iptables-save
    service iptables save
    
登陆mysql数据库 

    mysql -u root -p
    use mysql;
    select host,user,password from user;
    grant all privileges on waf_alarm_db.* to 'waf_alarm'@'%' identified by 'waf_alarm';
    flush privileges;