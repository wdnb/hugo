---
title: debian开机启动脚本编写
date: 2022-04-25T00:00:00+08:00
tags:
  - debian
categories:
  - Linux
---
# debian开机启动脚本编写

## 将所需要开启启动的指令填入脚本

```bash
#!/bin/bash
### BEGIN INIT INFO
# Default-Start:  2 3 4 5
# Default-Stop: 0 1 6
### END INIT INFO
cd /home/pi/docker/
docker-compose -f docker-compose.yml up -d
```

### 上传启动脚本到 /etc/init.d 目录下
```bash
cd /etc/init.d
```
```bash
chmod 755 dockerup
```

## 设置开机自启
```bash
sudo update-rc.d dockerup defaults
```

## 删除开机自启
```bash
sudo update-rc.d -f dockerup remove
```

