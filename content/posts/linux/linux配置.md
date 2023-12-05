---
title: linux配置
date: 2020-07-28 23:40:22
tags:
  - linux
categories:
  - Command
---
## linux检测远程端口是否打开

`nc -v ip port，如nc -v 172.17.193.18 5902`
根据显示的Connected信息确定端口是否打开。

若显示：Ncat:Connected to 172.17.193.18:5902.
则表示远程端口已打开。
若显示：Ncat:Connection refused.
则表示远程端口未打开。

## 查看io信息

`iotop`

## 查看网络流量

`iftop`
`iptraf-ng`