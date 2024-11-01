---
date: 2015-06-13T19:15:00Z
tags:
  - php
title: LMAP搭建
categories:
  - Web
---

## 安装apache

    yum install httpd
	
 Apache配置文件

    vim /etc/httpd/conf/httpd.conf 
添加

        LoadModule php5_module modules/libphp5.so

启动 Apache  mysql
  
     service httpd restart 
     service mysqld restart

This should be changed to whatever you set DocumentRoot to.
<Directory "/var/www/html">

## 安装Mysql

    yum install -y mysql-server mysql mysql-deve

初始化

    service mysqld start

给我们的root账号设置密码为system

    mysqladmin -u root password 'system'



## 安装 PHP组件

    yum install php php-devel php-mysql php-gd libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt php-mbstring 


设置开机启动

    chkconfig httpd on 
    chkconfig mysqld on 

重启 MySql  

    service mysqld restart 

重启Apche  

    service httpd restart 

## 测试
在/var/www/html/ 下

新建文件

##PHPINFO测试
 wim index.php 

<?php 
phpinfo(); 
?> 

 

##连接数据库测试：

mysql.php 

<?php  
$link = mysql_connect("127.0.0.1:3306","root","system"); 
if($link!=false) {  
    echo"成功连接mysql服务器"; 
  } else  {  
    echo"与本地Mysql服务器连接失败"; 
  }  
mysql_close(); 
?>