---
title: php安装扩展_phpize方式
date: 2015-09-26T15:07:58Z
tags:
  - php
categories:
  - Web
---

 1. 进入php编译目录下的ext/插件目录/；
 2. 根据php安装路径执行/usr/local/php/bin/phpize ；
 3. 进入目录后执行

     ./configure --with-php-config=/usr/local/php/bin/php-config
     
    make&&make install

 4. 修改php配置文件/usr/local/php/etc/php.ini，添加XXXX.so模块。
 
	extension=pdo_oci.so
	extension=pdo_pgsql.so

	--with-php-config=/usr/local/php/bin/php-config
以及

	/usr/local/php/etc/php.ini

	为你PHP的安装路径和配置路径