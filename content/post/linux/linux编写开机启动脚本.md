---
date: 2016-09-14 15:23:00
tags:
  - command
title: linux编写开机启动脚本
categories:
  - Linux
---

编写shell脚本	
前面3行必需

        #!/bin/bash
        # chkconfig: 2345 20 80   [这个就是默认在2345运行级别是开启的，20为启动顺序，80为停止顺序]
        # description: Saves and restores system entropy pool for \
    
chmod 755 wafinfo
放到目录/etc/profile.d/下
(chkconfig指令对应的服务要和文件名称完全对应)
注册服务

    chkconfig --add wafinfo
    chkconfig --list wafinfo