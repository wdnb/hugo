---
status: public
tags:
  - gcc
title: 自行编译GCC
date: 2016-02-03 16:32:00
categories:
  - Software
---

载并解压GCC源码后执行：
>https://ftp.gnu.org/gnu/gcc/
```
$ cd GCC_SOURCE # GCC_SOURCE代表你解压源代码的目录
$ ./contrib/download_prerequisites # 由脚本下载并解压必备库
$ cd #回到家目录
$ mkdir gcc-build
$ cd gcc-build
$ ../GCC_SOURCE/configure --prefix=$HOME --enable-languages=c,c++ --disable-multilib 
$ make -j4
$ make install
```
检验结果
由于我们指定了--prefix=$HOME，所以GCC的可执行文件将被安装到$HOME/bin，请确保该目录位于$PATH中，然后执行$ gcc --version确认GCC版本。
引用地址
>https://segmentfault.com/a/1190000002982589

##切换gcc版本

execute in terminal :
```
gcc -v
g++ -v
```
Okay, so that part is fairly simple. The tricky part is that when you issue the command GCC it is actually a sybolic link to which ever version of GCC you are using. What this means is we can create a symbolic link from GCC to whichever version of GCC we want.
•	You can see the symbolic link :
```
ls -la /usr/bin | grep gcc-4.4
ls -la /usr/bin | grep g++-4.4
```
•	So what we need to do is remove the GCC symlink and the G++ symlink and then recreate them linked to GCC 4.3 and G++ 4.3:
```
rm /usr/bin/gcc
rm /usr/bin/g++

ln -s /usr/bin/gcc-4.3 /usr/bin/gcc
ln -s /usr/bin/g++-4.3 /usr/bin/g++
```
•	Now if we check the symbolic links again we will see GCC & G++ are now linked to GCC 4.3 and G++ 4.3:
```
ls -la /usr/bin/ | grep gcc
ls -la /usr/bin/ | grep g++
```
•	Finally we can check our GCC -v again and make sure we are using the correct version:
```
gcc -v
g++ -v
```