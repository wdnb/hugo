---
title: pdo_oci oci8 pdo_pgsql扩展编译安装
date: 2015-09-26T15:13:34Z
tags:
  - php
categories:
  - Web
---

公司用的zend框架开发只能用pdo连接数据库,分别是PGSQL和ORACLE,ORACLE  PDO_OCI的安装比较蛋疼,以下为我自己的安装摘记,免得哪一天我又得干这种恐怖的事情
**PDO_PGSQL**

> http://pecl.php.net/package/PDO_PGSQL

checking for pg_config… not found  configure: error: Cannot find libpq-fe.h. Please specify correct PostgreSQL  installation path

解决方法:`yum install  postgresql-devel`

**安装oracle的底层协议支持**

    rpm -ivh oracle-instantclient11.2-basic-11.2.0.2.0.i386.rpm
    
    rpm -ivh oracle-instantclient11.2-devel-11.2.0.2.0.i386.rpm

oracle-instantclient可以到oracle的官方下载。我的系统是32位的，如果是64位系统请下载

oracle-instantclient11.2-basic-11.2.0.3.0-1.x86_64.rpmoracle-instantclient11.2-devel-11.2.0.3.0-1.x86_64.rpm

**安装oci8的php扩展**

    oci8-1.4.10.tgz  
    tar -zxvf oci8-1.4.10.tgz    
    cd oci8-1.4.10

    /usr/local/php/bin/phpize CFLAGS="-I/usr/include/oracle/11.2/client/" CXXFLAGS="-I/usr/include/oracle/11.2/client/"(注意：如果是64位的系统，client改成client64)
    
        ./configure --with-php-config=/usr/local/php/bin/php-config --with-oci8=instantclient,/usr/lib/oracle/11.2/client64/lib/

**pdo_oci**

    ./configure --with-php-config=/usr/local/php/bin/php-config --with-pdo-oci=instantclient,/usr,11.2



 1. 设置oracle的相关参数

    declare -x ORACLE_HOME="/data/oracle/product"
    declare -x ORACLE_SID="product"

其他参考

> http://segmentfault.com/a/1190000002722201

    ./configure --with-php-config=/usr/local/php/bin/php-config --with-pdo-oci=instantclient,/usr/lib/oracle/11.2/client64/lib/
     ./configure --with-php-config=/usr/local/php/bin/php-config --with-pdo-oci=instantclient,/usr,11.2