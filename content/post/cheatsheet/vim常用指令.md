---
title: vim常用指令
date: 2015-06-13T19:19:15Z
tags:
  - vim
categories:
  - CheatSheet
---
vim 批量替换
```bash
:from,tos/old/new/g
```

from是起始行

to是终止行用$表示到文件最后一行

s是替换的意思

old是想被替换的文本

new是你的新文本

g表示全局

例如:
```bash
:1,$s/yanyan/amy/g
```
就是将一个文件的第一行到最后一行，也就是整个文件的yanyan这个字串替换成amy

设置行号
```bash
:set number
```
