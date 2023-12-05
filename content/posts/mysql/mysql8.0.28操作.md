---
date: 2022-04-25 00:00:00
tags:
  - mysql
  - docker
title: mysql 8.0.28操作
categories:
  - Web
---

## mysql 8.0.28操作

### docker mysql 导入sql

拷贝sql文件到docker中的mysql容器中
以下指令将test.sql文件拷贝到mysql容器里的/目录下

```docker cp test.sql mysql:/test.sql```

### 进入mysql容器

```docker exec -it mysql  /bin/bash```

然后使用`mysql -uroot -p` 登录mysql
如果是表已经存在的话 先删除你要导入的目标表

```DROP TABLE table_name ;```

### 导入数据

```source /test.sql```
