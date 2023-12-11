---
title: docker配置
date: 2020-07-25 16:01:22
tags:
  - docker
categories:
  - Web
---
# linux安装docker

## 官方安装文档

官方安装文档很详细了建议直接参考官方文档安装

>https://docs.docker.com/engine/install/

## docker 无root模式安装
>https://docs.docker.com/engine/security/rootless/

```bash
dockerd-rootless-setuptool.sh install
```


[INFO] 要控制 docker.service，请运行：`systemctl --user (start|stop|restart) docker.service`

[INFO] 要在系统启动时运行 docker.service，请运行：`sudo loginctl enable-linger pi`

[INFO] 确保设置了以下环境变量（或将它们添加到 ~/.bashrc）：

```bash
export PATH=/usr/bin:$PATH
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```


### docker配置镜像加速器,阿里云的需要登录账号获取

阿里云docker镜像加速器 个人比较喜欢用这个 速度比较快

>https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
	"https://hub-mirror.c.163.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## docker-compose
### 树莓派安装docker-compose
树莓派没有官方的 docker-compose 可以用linuxserver封装的docker-compose(*截至2020-07-25*)

[linuxserver官方地址](https://hub.docker.com/r/linuxserver/docker-compose)

```bash
sudo curl -L --fail https://raw.githubusercontent.com/linuxserver/docker-docker-compose/master/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```