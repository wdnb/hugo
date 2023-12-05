---
title: linux配置ip及路由表
date: 2019-03-02 22:14:46
tags:
  - linux
categories:
  - Command
---

centos7配置ip：  

    ip addr add 192.168.10.68/24  br + dev enp1s0
    ip ro add default via 192.168.10.1 dev enp1s0

配置静态ip

    cd /etc/sysconfig/network-scripts

BOOTPROTO修改为static
ONBOOT=yes
IPADDR=192.168.10.99
GATEWAY=192.168.10.1
NETMASK=255.255.255.0


    TYPE=Ethernet
    BOOTPROTO=static
    DEFROUTE=yes
    PEERDNS=yes
    PEERROUTES=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_PEERDNS=yes
    IPV6_PEERROUTES=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=stable-privacy
    NAME=enp12s0
    UUID=bac373f2-baf4-4353-b3f5-646b0a4f0e6c
    DEVICE=enp12s0
    ONBOOT=yes
    IPADDR=192.168.10.99
    GATEWAY=192.168.10.1
    NETMASK=255.255.255.0


操作路由表

    route
    route delete default
    ip route add default via 192.168.26.1