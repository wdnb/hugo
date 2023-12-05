---
title: 树莓派部署k3s
date: 2020-07-28 23:43:22
tags:
  - 树莓派
  - k3s
categories:
  - DevOps
---
硬件设备： 树莓派4b 8g,树莓派3B 1g

k3s版本： k3s version v1.18.6+k3s1 (6f56fa1d)

操作系统：

Linux raspberrypi 5.4.51-v7l+ #1326 SMP Fri Jul 17 10:51:18 BST 2020 armv7l GNU/Linux

helm: v3.2.4

## 前言
主要以提供的官方文档为准，指令记录只有关键的流程

## 配置
配置文件会被写到 /etc/systemd/system 下
`sudo vim /etc/systemd/system/k3s.service`

## 查看日志

`tail -f /var/log/syslog`

## 前期工作和错误处理

>https://blog.jbface.com/posts/752b168e.html

## k3s部署官方文档
>https://rancher2.docs.rancher.cn/docs/installation/k8s-install/_index

### 安装 Kubernetes 并配置 K3s Server

### 安装并且指定外部存储
官方文档
>https://docs.rancher.cn/k3s/installation/datastore.html#_2-2-mysql

```
curl -sfL https://docs.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -s - server \
--datastore-endpoint="mysql://root:123456@tcp(127.0.0.1:3306)/kubernetes"
```
如果只有mysql://的话 使用root用户名连接到MySQL套接字/var/run/mysqld/mysqld.sock，无需密码并且试图创建一个名为kubernetes的数据库

## 跨vps组建集群 

### 添加master节点配置外网ip和证书ip和数据库

```
curl -sfL https://docs.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -s - server --node-external-ip=xxx.xxx.xxx.xxx \
--tls-san=xxx.xxx.xxx.xxx \
--datastore-endpoint="mysql://root:password@tcp(xxx.xxx.xxx.xxx:3306)/kubernetes"
```

### 检测是否有EXTERNAL-IP

`kubectl get nodes -o wide`

### 获取NODE_TOKEN

`cat /var/lib/rancher/k3s/server/node-token`

### 添加slave节点并且连接到外网master

```
curl -sfL https://docs.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -s - agent \
--server https://xxx.xxx.xxx.xxx:6443 --token NODE_TOKEN
```



