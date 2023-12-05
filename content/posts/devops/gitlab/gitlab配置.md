---
title: gitlab配置
date: 2020-07-19 18:37:22
tags:
  - gitlab
  - 树莓派
categories:
  - DevOps
---
## gitlab配置 

#### 编辑gitlab配置gitlab.rb指定external_url
如果不配置external_url的话gitlab的仓库地址会是localhost,这样就失去使用的意义了,得事先准备一个解析到gitlab所在服务器ip的域名，如果是内网的话还得做ddns解析,然后端口映射出去，外网域名就直接解析到固定ip，开放对应端口就行
官方配置文档
>https://docs.gitlab.com/omnibus/settings/configuration.html

编辑gitlab.rb添加external_url,我这里是docker安装的gitlab,所以文件位置在dockerfile指定的目录

`sudo vim /home/pi/docker/data/gitlab/config/gitlab.rb`

`external_url 'http://URL:8989'`

重新读取配置使配置生效

`docker exec -it mydock_gitlab_1 gitlab-ctl reconfigure`

这时候项目的git地址已经变成external_url指定的域名了

### gitlab 配置docker镜像加速

#### config.toml增加runners.machine配置
官方配置文档
>https://docs.gitlab.com/runner/configuration/autoscale.html#distributed-container-registry-mirroring

注意缩进

`sudo vim /home/pi/docker/data/gitlab/runner/config.toml`
```
[runners.machine]
  MachineOptions = [
      "engine-registry-mirror=https://URL"
  ]
```
