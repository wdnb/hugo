---
date: 2016-02-20 16:30:00
tags:
  - nginx
title: waf-nginx
categories:
  - Web
---

centos5.4 x64 编译安装
    
    yum install pcre-devel


    ./configure --prefix=/usr/local/nginx --without-http_ssi_module --without-http_geo_module --without-http_map_module --without-http_uwsgi_module --without-http_scgi_module --without-http_proxy_module --without-http_memcached_module --without-http_limit_conn_module --without-http_limit_req_module --without-http_empty_gif_module --with-http_ssl_module --with-ipv6

    make
    make install