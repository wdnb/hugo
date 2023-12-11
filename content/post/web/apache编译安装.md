---
title: apache编译安装
date: 2015-09-26 15:01:23
tags:
  - apache
categories:
  - Web
---
**编译安装apr**

    ./configure --prefix=/opt/apr-1.5.2

**编译安装 apr-util**

    ./configure --prefix=/opt/apr-util-1.5.4 --with-apr=/opt/apr-1.5.2

**编译安装 pcre2**

    ./configure --prefix=/opt/pcre-8.37

**编译安装apache**

    ./configure --prefix=/opt/httpd-2.4.16 --with-apr=/opt/apr-1.5.2 --with-apr-util=/opt/apr-util-1.5.4 --with-pcre=/opt/pcre-8.37 --enable-shared=max --enable-module=rewirte --enable-module=so --enable-vhost-alias --enable-mods-shared=all --libdir=/usr/lib64

make时如提示以下错误，请创建文件夹复制libapr-1.la(eg./opt/apr-1.5.2/lib/libapr-1)到它指定的目录（eg./opt/lib/）
*//libtool: link: cannot find the library `/opt/lib/libapr-1.la' or unhandled argument `/opt/lib/libapr-1.la'*

修改配置文件

    vim /opt/httpd-2.4.16/conf/httpd.conf

去掉注释

    ServerName www.example.com:80

增加

    LoadModule php5_module modules/libphp5.so
    AddType application/x-httpd-php .php

重启

    /opt/httpd-2.4.16/bin/apachectl restart


将apache添加到开机启动项

    sudo cp /opt/httpd-2.4.16/bin/apachectl /etc/init.d/
    sudo mv /etc/init.d/apachectl /etc/init.d/httpd

这样就能通过

    sudo service httpd start

来启动apache了

	sudo vim /etc/init.d/httpd
添加以下两行注释 使得chkconfig 能够识别脚本

    #!/bin/bash
    #chkconfig:345 61 61
    #description:Apache httpd

chkconfig 指令调用的是init.d目录下的脚本

所以现在chkonfig httpd on 就能配置开机启动了