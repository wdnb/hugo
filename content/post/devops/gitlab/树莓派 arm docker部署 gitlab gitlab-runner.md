---
title: 树莓派 arm docker部署 gitlab gitlab-runner
date: 2020-07-18 22:37:22
tags:
  - gitlab
  - 树莓派
categories:
  - DevOps
---
# 前言

想学习了解一下ci/cd，但是windows下的gitlab和postgresql的volumes没法直接映射到宿主机，干脆就弄了个树莓派玩玩

## 准备工作

### 安装Raspberry
树莓派4b 8g内存版，安装Raspberry Pi OS
官方下载地址，因为当服务器不需要桌面，我这里下载的是 Raspberry Pi OS (32-bit) Lite


>https://www.raspberrypi.org/downloads/raspberry-pi-os/

用rufus将系统烧入sd卡

>https://rufus.ie/

### 安装docker和docker-composer
>http://blog.jbface.com/posts/f61bf40.html

### 获取gitlab,runner镜像
gitlab,gitlab-runner官方docker镜像没有arm版这里用的是第三方编译的arm docker镜像
>https://github.com/ulm0/gitlab
>https://github.com/ulm0/gitlab-runner

## 开始

这里参考的是lardock的dockfile写出来的配置
只需要gitlab,gitlab-runner,redis,postgres

>https://github.com/wdnb/dnmp

## 配置

### 启动
```bash
docker-composer up
```


### 获取Runner的URL和token
这里需要先进入gitlab的管理中心获取Runner的URL和token

>http://URL:8989/admin/runners

默认用户是root,初始密码是配置在dnmp项目的env里的GITLAB_ROOT_PASSWORD,可以进入gitlab后修改

### 注册runner
gitlab-runner运行会提示：
`ERROR: Failed to load config stat /etc/gitlab-runner/config.toml: no such fi`
这是因为还没注册注册runner
官方runner注册文档
>https://docs.gitlab.com/runner/register/

根据镜像volume挂载方式的不同 local system volume mounts和Docker volume mounts 注册指令有一点点区别，我这里用的是local system volume mounts.
PATH为docker映射到宿主机的gitlab-runner配置目录（/home/pi/...），指令执行完成后生成的config.toml会存放到这个目录下，填入上面获取的url和registration-token进行注册
```bash
  docker run --rm -it -v ~/PATH/gitlab/runner:/etc/gitlab-runner  gitlab/gitlab-runner register \
  --non-interactive \
  --executor "docker" \
  --docker-image alpine:latest \
  --url "http://URL:8989" \
  --registration-token "PROJECT_REGISTRATION_TOKEN" \
  --description "docker-runner" \
  --tag-list "docker,aws" \
  --run-untagged="true" \
  --locked="false" \
  --access-level="not_protected"
```
## 查看注册信息
```bash
sudo cat /home/pi/docker/data/gitlab/runner/config.toml
```


## gitlab配置
安装完毕后还有一些配置的调优参考这里

[gitlab配置](/posts/devops/gitlab/gitlab配置)

