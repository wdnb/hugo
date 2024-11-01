---
date: 2016-02-20T16:28:00Z
tags:
  - php
title: 使用php-fpm socket方式连接nginx
categories:
  - Web
---

环境：
php7.0.2
nginx-1.9.9
centos5.4 x64

在/dev/shm/目录下建一个文件

     touch /dev/shm/php-fcgi.sock

为其他用户添加写入能力
    
    sudo    chmod o+w /dev/shm/php-fcgi.sock

修改php配置文件

    cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
    cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
    
修改 www.conf（`vim /usr/local/php/etc/php-fpm.d/www.conf`）
  
    ;listen =127.0.0.1:9000
    listen =/dev/shm/php-fcgi.sock
    
在nginx配置文件server新增php配置，将http的方式改为socket方式（`vim /usr/local/nginx/conf/nginx.conf`）

        location ~ .*\.php?$ {
                #fastcgi_pass  127.0.0.1:9000;
                fastcgi_pass   unix:/dev/shm/php-fcgi.sock;
                fastcgi_index index.php;
                include fastcgi.conf;}
                
避免权限问题修改运行用户

        #user  nobody;
        user  root;        

重启php-fpm与nginx
------------------------------
检测nginx配置文件

    sudo /usr/local/nginx/sbin/nginx -t

重启

    sudo /usr/local/nginx/sbin/nginx -s reload

------------------------------
vim /usr/local/php/etc/php-fpm.conf 去掉里面那个 pid = run/php-fpm.pid 前面的分号生成pid文件(没有的话自行添加)

    [global]
    ; Pid file
    ; Note: the default prefix is /usr/local/php/var
    ; Default Value: none
    pid = run/php-fpm.pid


php-fpm 启动：
    
    /usr/local/php/sbin/php-fpm
php-fpm 关闭：

    kill -INT `cat /usr/local/php/var/run/php-fpm.pid`
    
php-fpm 重启：

    kill -USR2 `cat /usr/local/php/var/run/php-fpm.pid`
    
------------------------------

    ls -al /dev/shm

可以看到php-cgi.sock文件unix套接字类型


#配置中出现的问题：
##nginx: [error] open() ＂/usr/local/nginx/logs/nginx.pid＂ failed错误

解决方法：（使用-c参数指定配置文件）
启动nginx

    /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

##本地无法访问虚拟机的nginx服务器

解决方法：
1：虚拟机本地使用links浏览器能打开页面
2：lsof -i :80也有进程

3：能ping通虚拟机Ip的
以上三条排除后的原因
可能是防火墙端口没打开 

    iptables -I INPUT -p tcp --dport 80 -j ACCEPT 
    iptables-save
    service iptables save