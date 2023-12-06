---
date: 2015-06-13 20:59:00
tags:
  - ssh
title: ssh密钥
categories:
  - Manual
---
## 密钥生成

	ssh-keygen -t rsa

回车即可
SSH 秘钥生成结束后，你可以在用户目录 (~/.ssh/) 下看到私钥 id_rsa 和公钥 id_rsa.pub 这两个文件

## 配置authorized_keys实现免密自动登陆
复制公钥重命名为authorized_keys,并修改权限为600
    
    cat id_dsa.pub >> ~/.ssh/authorized_keys
    chmod 600 authorized_keys
将私钥导出到需要采用免密登陆的地方进行配置即可
