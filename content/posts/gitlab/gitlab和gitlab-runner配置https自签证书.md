---
title: gitlab和gitlab-runner配置 https 自签证书
date: 2020-08-17 0:22:22
tags:
  - gitlab
  - 树莓派
categories:
  - DevOps
---

## gitlab配置https

### 生成自签证书

```
#秘钥脚本，将以下内容保存为shell脚本，然后运行
#出现提示输入信息的地方输入信息，先输入域名然后4次证书密码，任意密码，四次保持一致。

#!/bin/sh

# create self-signed server certificate:

read -p "Enter your domain [139.199.125.93]: " DOMAIN

echo "Create server key..."

openssl genrsa -des3 -out $DOMAIN.key 2048

echo "Create server certificate signing request..."

SUBJECT="/C=US/ST=Mars/L=iTranswarp/O=iTranswarp/OU=iTranswarp/CN=$DOMAIN"

openssl req -new -subj $SUBJECT -key $DOMAIN.key -out $DOMAIN.csr

echo "Remove password..."

mv $DOMAIN.key $DOMAIN.origin.key
openssl rsa -in $DOMAIN.origin.key -out $DOMAIN.key

echo "Sign SSL certificate..."

openssl x509 -req -days 3650 -in $DOMAIN.csr -signkey $DOMAIN.key -out $DOMAIN.crt

echo "TODO:"
echo "Copy $DOMAIN.crt to /etc/nginx/ssl/$DOMAIN.crt"
echo "Copy $DOMAIN.key to /etc/nginx/ssl/$DOMAIN.key"
echo "Add configuration in nginx:"
echo "server {"
echo "    ..."
echo "    listen 443 ssl;"
echo "    ssl_certificate     /etc/nginx/ssl/$DOMAIN.crt;"
echo "    ssl_certificate_key /etc/nginx/ssl/$DOMAIN.key;"
echo "}"
```
### 生成证书后拷贝到宿主机配置目录

```
cp domain.crt domain.key ~/PATH/data/gitlab/config/ssl

```
### gitlab https配置

以下是与https有关的配置，添加在gitlab.rb里后运行`sudo docker exec -it 4fac53125f59 gitlab-ctl reconfigure`生效
如果gitlab是运行在docker里的，也可以将配置放在docker-compose.yml里，重启容器生效。

```
external_url 'https://DOMAIN:9898/'
nginx['listen_https'] = true
nginx['listen_port'] = 443
nginx['redirect_http_to_https'] = true
nginx['ssl_certificate'] = "/etc/gitlab/ssl/DOMAIN.crt"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/DOMAIN.key"
```

### 配置可信证书

将DOMAIN.crt证书添加到~/PATH/data/gitlab/config/gitlab/trusted-certs/目录并运行`sudo docker exec -it 4fac53125f59 gitlab-ctl reconfigure`。

## gitlab-runner配置https

### 给runner添加证书
#### 注意证书rsa秘钥长度最低2048位
一开始因为rsa的位数只有1024导致注册runner一直提示/api/v4/runners: x509: certificate signed by unknown authority,和没添加证书的提示一样
这时候用curl请求https://DOMAIN:9898/后发现错误信息为curl: (60) SSL certificate problem: EE certificate key too weak发现是因为生成的rsa私钥长度不够
使用2048位的rsa生成的自签证书请求https://DOMAIN:9898/这时错误信息变成curl: (60) SSL certificate problem: self signed certificate 现在再执行https runner注册就成功了
### 注册https gitlab runner
关键点就是加了个`--tls-ca-file`参数,指向容器内部的证书路径
修改一下DOMAIN和TOKEN为自己的信息
我用的是树莓派，所以用的是klud/gitlab-runner镜像，官方的是gitlab/gitlab-runner，视情况替换

[x509: certificate signed by unknown authority官方解决办法](https://docs.gitlab.com/runner/configuration/tls-self-signed.html)

```
docker run --rm -it -v ~/docker/data/gitlab/runner:/etc/gitlab-runner  gitlab/gitlab-runner register \
--non-interactive \
--tls-ca-file=/etc/gitlab-runner/DOMAIN.crt \
--executor "docker" \
--docker-image alpine:latest \
--url "https://DOMAIN:9898/" \
--registration-token "TOKEN" \
--description "docker-runner" \
--tag-list "docker,raspberry" \
--run-untagged="true" \
--locked="false" \
--access-level="not_protected"
```
