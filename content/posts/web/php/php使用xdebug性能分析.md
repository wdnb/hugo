---
title: php使用xdebug性能分析
date: 2017-03-02 15:13:35
tags:
  - xdebug
categories:
  - Web
---
    phpize
    ./configure --enable-xdebug --with-php-config=/usr/local/php/bin/php-config
    make
    make install

```
 vim /usr/local/php/etc/php.ini
```
``` 
[Xdebug]
zend_extension=xdebug.so
xdebug.profiler_enable=on
xdebug.trace_output_dir="/root/tmp"
xdebug.profiler_output_dir="/root/tmp"
```

    kill -USR2 `cat /usr/local/php/var/run/php-fpm.pid`