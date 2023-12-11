---
title: docker指令
date: 2023-12-08T16:53:03+08:00
tags:
  - docker
  - command
categories:
  - Web
---
# docker指令

## docker

显示docker镜像
```bash
docker images
```

显示容器
```bash
docker ps -a
```
停止所有容器
```bash
docker stop $(docker ps -aq) 
```

重启容器
```bash
sudo docker restart CONTAINER（容器名或者id）
```

拉取单个容器

```bash
docker pull mysql:5.7
```

windows下进入容器 (*powershell下无需winpty*)
```bash
winpty docker exec -it a75790238d30 bash 
```

删除全部容器
```bash
docker rm $(docker ps -aq)
```

清理所有无用数据卷
```bash
docker volume prune
```

删除 none 镜像
```bash
docker rmi $(docker images | grep "none" | awk '{print $3}')
```

删除所有镜像
```bash
docker rmi $(docker images -q)
```

批量打包laradock镜像
```bash
docker save -o 1.tgz $(docker images | grep laradock|awk '{print $1}')
```
```bash
docker save -o 1.tgz laradock_workspace laradock_php-fpm laradock_nginx laradock_mysql laradock_redis laradock_portainer laradock_elasticsearch laradock_logstash laradock_kibana
```

导入镜像
```bash
docker load -i 	1.tgz
```
## docker-compose

使用docker-compose启动docker（在docker-compose.yml文件目录下执行）
compose以守护进程模式运行加-d选项

```bash
docker-compose up -d
```
 参数强制重建容器 不加参数的情况下会优先使用已有的容器
```bash
docker-compose up --force-recreate
```
停止所有容器
```bash
docker-compose stop
```
## 常见问题
```text
error getting credentials - err: exit status 1, out: GDBus.Error:org.freedesktop.DBus.Error.ServiceUnknown: The name org.freedesktop.secrets was not provided by any .service files
```

解决方法

```bash
sudo apt install gnupg2 pass
```
参考地址
https://stackoverflow.com/questions/50151833/cannot-login-to-docker-account
### docker pull 无法拉取镜像
docker pull返回以下错误信息

```text
Error response from daemon: Get "https://registry-1.docker.io/v2/": proxyconnect tcp: dial tcp: lookup http.docker.internal on 192.168.65.7:53: read udp 192.168.10.100:43674->192.168.65.7:53: i/o timeout
```


解决方法
清除Docker缓存： 有时候，Docker的缓存可能会导致问题。您可以尝试清除Docker的缓存，然后再次尝试拉取镜像：

```bash
docker system prune -a
```

