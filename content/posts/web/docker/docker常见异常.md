---
title: docker常见异常
date: 2023-12-08T15:45:00
tags:
  - docker
categories:
  - Web
---
## docker pull 无法拉取镜像
docker pull返回以下错误信息

`Error response from daemon: Get "https://registry-1.docker.io/v2/": proxyconnect tcp: dial tcp: lookup http.docker.internal on 192.168.65.7:53: read udp 192.168.10.100:43674->192.168.65.7:53: i/o timeout`

### 解决方法
清除Docker缓存： 有时候，Docker的缓存可能会导致问题。您可以尝试清除Docker的缓存，然后再次尝试拉取镜像：
`
```bash
docker system prune -a
```

`