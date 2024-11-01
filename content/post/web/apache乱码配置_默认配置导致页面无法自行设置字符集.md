---
title: apache乱码配置_默认配置导致页面无法自行设置字符集
date: 2015-06-13T19:17:46Z
tags:
  - apache
categories:
  - Web
---
## 修改默认字符集

    vim /etc/httpd/conf/httpd.conf

修改

    AddDefaultCharset UTF8
为

    AddDefaultCharset Off