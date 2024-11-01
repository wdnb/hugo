---
title: rsync配置
date: 2015-06-13T19:09:47Z
tags:
  - rsync
categories:
  - Software
---
## rsyncd.conf

    # Minimal configuration file for rsync daemon
    # See rsync(1) and rsyncd.conf(5) man pages for help
     
    # This line is required by the /etc/init.d/rsyncd script
    pid file = /var/run/rsyncd.pid   
    port = 873 
    address = 209.141.51.244
    #uid = nobody
    #gid = nobody   
    uid = root   
    gid = root   
     
    use chroot = yes 
    read only = yes 
     
     
    #limit access to private LANs
    #hosts allow=192.168.1.0/255.255.255.0 10.0.1.0/255.255.255.0 
    #hosts deny=*
    hosts allow=23.244.180.54
    max connections = 3
    motd file = /etc/rsyncd/rsyncd.motd
     
    #This will give you a separate log file
    log file = /home/wwwlogs/rsync.log
     
    #This will log every file transferred - up to 85,000+ per user, per sync
    #transfer logging = yes
     
    log format = %t %a %m %f %b
    syslog facility = local3
    timeout = 300
     
    [backup]   
    path = /root
    list=yes
    ignore errors
    auth users = rsync
    secrets file = /etc/rsyncd/rsyncd.secrets

## rs.sh

    rsync -avzP --delete  --password-file=./pass rsync@209.141.51.244::backup /backup
## rsyncd.secrets

    rsync:system

## pass

    system

## rsync服务重启

    209.141.51.244
    
    /usr/bin/rsync -vzrtopg --delete  --progress root@209.141.51.244::backup 
    ./backup
    
    //开机启动  把下面的代码放到/etc/rc.local
    rsync --daemon --config=/etc/rsyncd/rsyncd.conf 
    ps -ef |grep rsync
    kill -9 0000

在前面"一句话SSH命令利用rsync同步备份VPS网站目录文件"文章中仅仅是简单RSYNC功能应用，强大的RSYNC不仅仅这么简单的功能。鉴于老左的能力和技术有限，搜寻强大的网络资源库看看有没有对RSYNC升级的应用功能加以学习应用。看到小夜的关于RSYNC增量备份的文章，于是老左这边也一起学习和整理看看是否完整能够实现定时同步增量备份。
第一、RSYNC同步备份准备工作
我们需要先下载2个文件包：RSYNC服务器端配置文件（VPS数据部分） /RSYNC客户端配置文件（VPS备份主机）
客户端：http://soft.laozuo.org/backup/rsync-root.zip
服务器端：http://soft.laozuo.org/backup/rsync-server.zip
第二、配置服务器端VPS
我们把rsync-server.zip下载的服务器端RSYNC配置文件上传至/etc目录，在上传之前，需要修改几个位置：
A - rsyncd.conf第7行的address = 11.11.11.11把IP地址修改成我们的服务器端IP地址
B -rsyncd.conf 第20行修改hosts allow=22.22.22.22修改成我们客户端的IP地址
C -rsyncd.conf第35行修改path = /home/wwwroot/ 修改成我们需要同步的目录
D - 修改rsyncd.secrets文件中的用户名和密码（用户名需要与下面的E一致，密码随意），然后在SSH中授予600权限
chmod 600 /etc/rsyncd/rsyncd.secrets
E -rsyncd.conf第38行，auth users = loong ，后面的loong用户名需要与上面D中设置的用户名一致
第三、设置服务器端运行rsync
/usr/bin/rsync --daemon --config=/etc/rsyncd/rsyncd.conf
第四、设置客户端配置文件
下载rsync-root.zip文件，修改文件后上传至ROOT目录中
A - 设置/root/pass文件中的密码为客户端ROOT密码，并且也需要授权600权限
chmod 600 /root/root/pass
B - 设置/root/rs.sh中的脚本路径，需要保持与服务器端一致
rsync -avzP --delete --password-file=/root/pass laozuoserver@111.111.111.111::vpsmmhome /home/wwwroot/laozuo.org/
#laozuoserver为服务器端/etc/rsyncd/rsyncd.secrets的用户名一致
#111.111.111.111代表服务器端的IP地址
#vpsmmhome为/etc/rsyncd/rsyncd.conf中自定义用户名
#/home/wwwroot/laozuo.org/为需要同步备份的网站目录
C - 设置rs.sh权限
chmod +x /root/rs.sh
第五、测试备份以及定时备份
执行sh rs.sh可以实现测试现在就手工备份，执行的时候需要我们输入/root/pass的密码，然后才可以执行
 
我们肯定不是需要手工备份，我们需要定时执行备份脚本（输入 crontab -e 然后添加下面一行）。
30 */1 * * * /root/rs.sh
备注：每小时的30分钟自动同步一次，这个时间我们可以自己设置，你也可以设置一天备份一次。
老左在写教程的时候已经测试成功一次，且定时设置，可以确保这篇文章是完整的。RSYNC这篇的备份是定时增备份，如果文件没有变化是不会变动，会变动有变动的文件，保持与客户端一致，同样的我们也可以设置数据库的备份，这样保证文件的同步，一旦客户端VPS出现问题，我们只要切换解析就可以保证网站可以不受影响。


> http://www.laozuo.org/4480.html | 老左博客