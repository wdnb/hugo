---
title: centos安装nodejs
date: 2015-06-13 17:30:51
tags:
  - node
categories:
  - Software
---
## 编译安装
    # yum install openssl-devel
    # cd /usr/local/src
    # wget http://nodejs.org/dist/node-latest.tar.gz
    # tar zxvf node-latest.tar.gz
    (cd into extracted folder: ex "cd node-v0.10.3")
    # ./configure
    # make
    # make install

Note that this requires Python 2.6+ to use ./configure above. You can modify the "configure" file to point to python2.7 in line 1 if necessary.
    
To create an RPM package, you can use FPM:
    
    # wget http://nodejs.org/dist/node-latest.tar.gz
    # tar zxvf node-latest.tar.gz
    (cd into extracted folder: ex "cd node-v0.10.3")
    # ./configure --prefix=/usr/
    # make
    # mkdir /tmp/nodejs
    # make install DESTDIR=/tmp/nodejs/
    # tree -L 3 /tmp/nodejs/
    /tmp/nodejs/
    └── usr
        ├── bin
        │   ├── node
        │   ├── node-waf
        │   └── npm -> ../lib/node_modules/npm/bin/npm-cli.js
        ├── include
        │   └── node
        ├── lib
        │   ├── dtrace
        │   ├── node
        │   └── node_modules
        └── share
            └── man
Now make the nodejs package:
    
    # fpm -s dir -t rpm -n nodejs -v 0.8.18 -C /tmp/nodejs/ usr/bin usr/lib
    Then install and check the version:
    
    # rpm -ivh nodejs-0.8.18-1.x86_64.rpm 
    Preparing...                ########################################### [100%]
       1:nodejs                 ########################################### [100%]
    
    # /usr/bin/node --version
    v0.8.18
    Source: https://github.com/jordansissel/fpm/wiki/PackageMakeInstall