---
date: 2016-09-02 14:52:00
tags:
  - cpu
  - memory
title: CPU和内存的使用率算法
categories:
  - Linux
---
[Linux平台Cpu使用率的计算](http://www.blogjava.net/fjzag/articles/317773.html)
## 总的Cpu使用率计算

计算方法：
 cpu使用信息
       
    cat /proc/stat

第一行的数值表示的是CPU总的使用情况，所以我们只要用第一行的数字计算就可以
1、  采样两个足够短的时间间隔的Cpu快照，分别记作t1,t2，其中t1、t2的结构均为：
(user、nice、system、idle、iowait、irq、softirq、stealstolen、guest)为CPU使用情况顺序排列的9元组 ;
2、  计算总的Cpu时间片totalCpuTime
a)         把第一次的所有cpu使用情况求和，得到s1;
b)         把第二次的所有cpu使用情况求和，得到s2;
c)         s2 - s1得到这个时间间隔内的所有时间片，即totalCpuTime = j2 - j1 ;
3、计算空闲时间idle
idle对应第四列的数据，用第二次的第四列 - 第一次的第四列即可
idle=第二次的第四列 - 第一次的第四列
6、计算cpu使用率
pcpu =100* (total-idle)/total

## 计算某个进程cpu和内存使用率

### 内存使用率计算：

总内存量：totalmem = meminfo中获取内存的总量MemTotal对应的值

进程实际占用内存大小：processmem = <pid>/status文件中获取VmRSS对应的值

Ps: (实际也可以在<pid>/statm中获取，但里面是页数，要乘以每页的大小字节数，一般4k)

内存使用率：pmem = processmem/totalmem * 100%;

### cpu使用率计算：
[proc/pid/stat文件解析](http://blog.jbface.com/post/linux/proc-pid-statwen-jian-jie-shi)
cpu总的使用时长: totalcpu1 = stat文件第一行数字总和，里面是各种时间，user + nice + system + idle + iowait + irq + softirq + stealstolen + guest

进程使用cpu时长：processcpu1 = 读取<pid>/stat文件，按照空格区分，第14位到17位数字，分别表示为utime，stime，cutime，cstime，时长为utime+stime

ps:(有些文章写需要四个时间加和，但我测试后结果和top,ps不一致，后看top源文件，发现并没有使用cutime，cstime，因此去掉)

隔一段时间后，在同样方法取一次，标记为totalcpu2,processcpu2

ps:(间隔时间不能过短，因为文件中记录的时间单位为1/100秒，即至少要大于10毫秒才有意义)

cpu核数：cpunum = /proc/stat中，可以根据cpu[0-9]计算cpu核数

cpu使用率：pcpu = (processcpu2-processcpu1)/(totalcpu2-totalcpu1)*cpunum*100%
