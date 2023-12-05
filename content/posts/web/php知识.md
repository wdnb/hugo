---
title: php知识
date: 2016-09-03 15:13:36
tags:
  - php
categories:
  - Web
---
##php开启代码错误提示centos环境下

	vim /etc/php.ini

把 `display_errors = Off` 改为 `display_errors = On`
把 `error_reporting = xxx` 改为 `error_reporting = E_ALL | E_STRICT`

##PHP 0 和null和false的区别
PHP 0 和null和false值相等类型不等！
注意：
NULL是一种特殊的类型.
两种情况下为NULL
1. $var = NULL;
2. $var;
3.0、"0"、NULL以及没有任何属性的对象都将被认为是空的。
举例如下：
```
    <?php
    $test=0;
    if($test==''){
     echo '<br />在php中，0即为空'; //被输出
    }
    if($test===''){
     echo '<br />在php中，0即为空'; //不被输出
    }
    if($test==NULL){
     echo '<br />在php中，0即为空'; //被输出
    }
    if($test===NULL){
     echo '<br />在php中，0即为空'; //不被输出
    }
    if($test==false){
     echo '<br />在php中，0即为空'; //被输出
    }
    if($test===false){
     echo '<br />在php中，0即为空'; //不被输出
    }
    ?>
    ```