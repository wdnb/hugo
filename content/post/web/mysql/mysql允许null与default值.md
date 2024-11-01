---
title: mysql允许null与default值
date: 2015-06-13T19:14:49Z
tags:
  - mysql
categories:
  - Web
---
# Mysql 允许null 与 default值

分为下面4种情况：
- 允许null， 指定default值。
- 允许null， 不指定default，这个时候可认为default值就是null
- 不允许null，指定default值，不能指定default值为null，否者报错 Invalid default value for xxx
- 不允许null，不指定default值。这种情况，Insert的时候，必须指定值。否者报错 Field xxx doesn't have a default value
