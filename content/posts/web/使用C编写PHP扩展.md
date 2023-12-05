---
date: 2016-02-27 11:27:00
status: public
tags:
  - php
title: 使用C编写PHP扩展
categories:
  - Web
---

###环境:
php7.0.2-fpm
Linux localhost 2.6.18-164.el5 #1 SMP Thu Sep 3 03:28:30 EDT 2009 x86_64 x86_64 x86_64 GNU/Linux

C编译出PHP扩展有两种形式
•	作为一个可装载模块或者DSO（动态共享对象）
•	静态编译到PHP
##作为一个可装载模块或者DSO（动态共享对象）
通过扩展骨架生成器生成配置文件(位于php源码目录下的ext目录中)，
执行
`./ext_skel --extname=myext`
会在ext目录下自动建立扩展目录myext
修改myext目录下的config.m4
`vim config.m4`
去掉这3行首的注释标签"dnl"

    PHP_ARG_WITH(myext, for myext support,
    Make sure that the comment is aligned:
    [  --with-myext             Include myext support])
    

然后在myext目录下 执行

    phpize
    ./configure
    make
    make install

配置 php.ini 加入
`extension=myext.so`
重启php-fpm进程
`ps aux | grep php-fpm`
`kill -USR2 23949`（为上一个指令得到的主进程ID号）
执行 `php -i | grep myext `
有如下输出信息代表扩展安装成功
`myext support => enabled`
当然，这样得到的是一个没有任何功能的空扩展，要实现功能的话需要修改myext目录下的myext.c，把逻辑功能写进去。
##静态编译到PHP
现在，先让我们执行一下PHP源码根目录下的./configure --help命令。会发现输出信息并没有包含我们的扩展，这是因为这个configure脚本生成的时候，我们的扩展还没有编写呢。(这个configure是PHP官方分发的。)，所以首先我们需要使用buildconf命令生成新的configure脚本。 $ ./buildconf --force
现在当我们再执行./configure --help的时候，便会发现myext扩展的信息已经出现了。现在我们只需要重新走一遍PHP的编译过程，便可以把我们的扩展以静态编译的方式加入到PHP主程序中了。哦，千万不要忘记使用--enable-myext参数开启我们的扩展。

参考资料:
>https://github.com/walu/phpbook