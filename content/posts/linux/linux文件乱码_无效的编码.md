---
title: linux文件乱码_无效的编码
date: 2015-06-13 19:18:52
tags:
  - command
categories:
  - Linux
---
    yum install convmv

## convmv 使用方法：

    convmv -f 源编码 -t 新编码 [选项] 文件名

## 常用参数：

    -r 递归处理子文件夹
    
    –notest 真正进行操作，默认情况下是不对文件进行真实操作
    
    –list 显示所有支持的编码
    
    –unescap 可以做一下转义，比如把%20变成空格


## 应用举例：

    convmv -f GBK -t UTF-8 --notest  -r *

这句命令会对当前目录及子目录下所有的文件进行处理。