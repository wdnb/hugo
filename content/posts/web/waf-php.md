---
date: 2016-02-20 16:30:00
status: public
tags:
  - php
title: waf-php
categories:
  - Web
---

centos5.4 x64 编译安装

    yum install libpng-devel
    yum install freetype-devel

    './configure'  '--prefix=/usr/local/php' '--with-config-file-path=/etc' '--with-curl' '--with-gd' '--with-ldap=/usr' '--with-mysql' '--with-mysqli=mysqlnd' '--with-libdir=lib64' '--with-freetype-dir=/usr/lib64' '--with-openssl' '--enable-fpm' '--enable-mbstring' '--enable-gd-native-ttf' '--enable-sockets' '--enable-pcntl' '--enable-sysvmsg' '--enable-sysvsem' '--enable-sysvshm' '--disable-cgi' '--disable-short-tags' '--disable-phar'
    
    make
    make install
    cp php.ini-development /etc/php.ini

添加php至系统变量

     sudo vim/etc/profile

在文件末尾添加 

    PATH=$PATH:/PHP_PATH/bin:
    export PATH

执行指令使得修改生效

    source /etc/profile

查看是否配置成功

    echo $PATH