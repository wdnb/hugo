---
date: 2020-07-30 00:25:00
tags:
  - mariadb
title: mariadb安装
categories:
  - Web
---

## 安装MySQL

`sudo apt install mariadb-server`

## 设置数据库

`sudo mysql_secure_installation`

此时系统会询问你：Enter current password for root (enter for none): ,按回车（enter）键，因为第一次登陆是没有密码的。

然后会询问你： Set root password? —— 按 y ，进行root帐号的密码设置

此时，会提示 New password ,在此输入你的MySQL密码，请牢记这个密码，输入完成按回车，会提示re-enter new password此时再重复输入密码，回车即可。

然后，询问你 Remove anonymous users ，按 y 。

然后，询问你 Disallow root login remotely ，按 y 。

然后，询问你 Remove test database and access to it ，按 y 。

然后，询问你 Reload privilege tables now ，按 y 。

最后，您将看到消息 All done! 和 Thanks for using MariaDB! 。表示已经设置完成了。