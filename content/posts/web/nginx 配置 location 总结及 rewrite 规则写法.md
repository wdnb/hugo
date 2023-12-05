---
date: 2016-02-03 15:54:00
status: public
tags:
  - nginx
title: nginx 配置 location 总结及 rewrite 规则写法
categories:
  - Web
---

以=开头表示精确匹配
如 A 中只匹配根目录结尾的请求，后面不能带任何字符串。
^~ 开头表示uri以某个常规字符串开头，不是正则匹配
~ 开头表示区分大小写的正则匹配;
~* 开头表示不区分大小写的正则匹配
/ 通用匹配, 如果没有其它匹配,任何请求都会匹配到

参考地址:
>http://seanlook.com/2015/05/17/nginx-location-rewrite/