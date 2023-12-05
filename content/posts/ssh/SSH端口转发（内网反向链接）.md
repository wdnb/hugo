---
title: SSH端口转发（内网反向链接）
date: 2015-06-13 19:13:26
tags:
  - ssh
categories:
  - Manual
---
## 开启端口

linux系统下，81端口一般情况下是关闭的。
开启81端口：

    iptables -I INPUT -i eth0 -p tcp --dport 81 -j ACCEPT
    iptables -I OUTPUT -o eth0 -p tcp --sport 81 -j ACCEPT

关闭81端口：

    iptables -I INPUT -i eth0 -p tcp --dport 81 -j DROP
    iptables -I OUTPUT -o eth0 -p tcp --sport 81 -j DROP
 
然后保存： 

    #/etc/rc.d/init.d/iptables save 

再查看是否已经有了： 

    [root@vcentos ~]# /etc/init.d/iptables status

## 在内网B主机上生产公钥和私钥

    ssh-keygen
    ...(一直按Enter，最后在~/.ssh/下生成密钥)
    ls ~/.ssh/
    id_rsa id_rsa.pub known_hosts

 
## 复制B主机上生成的id_rsa.pub公钥到外网A主机上，并将内容加入到~/.ssh/authorized_keys中
把 id_rsa 私钥复制到路由器中 /root/.ssh 文件夹下

    cat id_rsa.pub >> ~/.ssh/authorized_keys

运行

    autossh -M 5678 -NR 19999:localhost:22 user@23.244.180.54

如果提示错误
Agent admitted failure to sign using the key. 
解决方法：
在当前用户下执行命令：

    ssh-add

Centos需要安装autossh
http://www.harding.motd.ca/autossh/

## 在内网机器输入

    autossh -M 5678 -NR 19999:localhost:22 root@myserver_ip
    //autossh -M 5678 -NR 19999:localhost:22 user@23.244.180.54
    ///bin/su -c '/usr/bin/autossh -M 5678 -NR 19999:localhost:22 root@23.244.180.54 -p2221' - root

1 修改服务器ssh配置文件，在/etc/ssh/sshd_config中加入下面一句话，为了能够实现全局访问，ssh中 -g参数不知道为什么没有效果，所以只好这么改了。

    GatewayPorts yes  
    
    /etc/init.d/sshd restart