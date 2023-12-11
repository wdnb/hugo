---
title: cas服务端配置
date: 2016-10-13 18:57:22
tags:
  - cas
categories:
  - Software
---
用tomcat7配置ssl完成后提示"使用了不受支持的协议"无法访问,后更换tomcat6后解决
操作系统:
centos5.4
使用的软件版本 
jdk:1.7.0_111
tomcat:apache-tomcat-6.0.45
cas client:php CAS-1.3.4
cas server:cas-server-4.0.0-release	
一:配置初始环境		
1:安装openjdk
yum install java-1.7.0-openjdk-devel.x86_64
2:默认目录在/usr/lib/jvm/java(这是一个软链接)
3:配置环境变量

    vim /etc/profile

最末尾添加以下内容
JAVA_HOME=/usr/lib/jvm/java
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export PATH
export CLASSPATH

    source /etc/profile

二:配置tomcat ssl
1:生成证书
    
    keytool -genkey -alias tomcat -keyalg RSA -keystore /root/keystore.jks
填入的内容随意,最后输入yes回车,最后提示Enter key password for <tomcat>回车即可

2:修改%TOMCAT_HOME%/conf/server.xml文件，添加以下内容：
keystoreFile和keystorePass为证书存放路径和密码

    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
    maxThreads="150" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS"
    keystoreFile="/root/keystore.jks"
    keystorePass="system" />
	
3:
使用%TOMCAT_HOME%/bin/startup.sh启动tomcat
访问https://192.168.25.163:8443/
如不能访问请访问8080端口查看tomcat服务是否正常

三:cas服务端安装
拷贝cas-server-4.0.0/modules/cas-server-webapp-4.0.0.war
到%TOMCAT_HOME%/webapps/目录下
重命名为cas.war
启动tomcat服务
访问https://192.168.25.163:8443/cas
默认用户名和密码为 casuser:Mellon