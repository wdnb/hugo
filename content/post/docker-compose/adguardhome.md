---
title: adguardhome
categories:
  - Docker-Compose
date: 2024-11-01T17:38:24+0800
tags:
---




```go
version: "3.8"
services:

  adguardhome:
    container_name: adguardhome
    restart: unless-stopped
    volumes:
      - ./data/own/workdir:/opt/adguardhome/work
      - ./data/own/confdir:/opt/adguardhome/conf
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "68:68/udp"
      - "80:80/tcp"
      - "443:443/tcp"
      - "443:443/udp"
      - "3000:3000/tcp"
      - "853:853/tcp"
      - "784:784/udp"
      - "853:853/udp"
      - "8853:8853/udp"
      - "5443:5443/tcp"
      - "5443:5443/udp"
    image: adguard/adguardhome
    
networks:
  default:
    driver: bridge
    ipam:
      driver: default
      # 解除下面的注释可以设置网段，用于nginx等容器固定容器IP
      #config:
      #  - subnet: 10.0.0.0/24
```