---
title: 编写debian系开机启动脚本
date: 2022-04-25 00:00:00
tags:
  - debian
categories:
  - Linux
---

### 编写debian系开机启动脚本

```bash
#!/bin/bash
### BEGIN INIT INFO
# Default-Start:  2 3 4 5
# Default-Stop: 0 1 6
### END INIT INFO
cd /home/pi/docker/
docker-compose -f docker-compose.yml up -d
```

上传启动脚本到 /etc/init.d 目录下
```bash
cd /etc/init.d
```
```bash
chmod 755 dockerup
```

# 设置开机自启
```bash
sudo update-rc.d dockerup defaults
```

# 删除开机自启
```bash
sudo update-rc.d -f dockerup remove
```

