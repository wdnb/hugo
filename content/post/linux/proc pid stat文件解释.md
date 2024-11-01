---
date: 2016-09-01T17:47:00Z
tags:
  - command
title: " proc pid stat文件解释"
categories:
  - Linux
---

 /proc/PID/stat
包含了所有CPU活跃的信息，该文件中的所有值都是从系统启动开始累计到当前时刻。

```bash
cat /proc/6873/stat
```

	6873 (a.out) R 6723 6873 6723 34819 6873 8388608 77 0 0 0 41958 31 0 0 25 0 3 0 5882654 1409024 56 4294967295 134512640 134513720 3215579040 0 2097798 0 0 0 0 0 0 0 17 0 0 0

参数解释
|   |   |
|---|---|
|参数|解释|
|pid|进程(包括轻量级进程，即线程)号|
|comm|应用程序或命令的名字|
|task_state|任务的状态，R:runnign, S:sleeping (TASK_INTERRUPTIBLE), D:disk sleep (TASK_UNINTERRUPTIBLE), T: stopped, T:tracing stop,Z:zombie, X:dead|
|ppid|父进程ID|
|pgid|线程组号|
|sid|该任务所在的会话组ID|
|tty_nr|该任务的tty终端的设备号|
|tty_pgrp|终端的进程组号|
|task->flags|进程标志位，查看该任务的特性|
|min_flt|该任务不需要从硬盘拷数据而发生的缺页（次缺页）的次数|
|cmin_flt|累计的该任务的所有的waited-for进程曾经发生的次缺页的次数目|
|maj_flt|该任务需要从硬盘拷数据而发生的缺页（主缺页）的次数|
|cmaj_flt|累计的该任务的所有的waited-for进程曾经发生的主缺页的次数目|
|utime|该任务在用户态运行的时间，单位为jiffies|
|stime|该任务在核心态运行的时间，单位为jiffies|
|cutime|累计的该任务的所有的waited-for进程曾经在用户态运行的时间，单位为jiffies|
|cstime|累计的该任务的所有的waited-for进程曾经在核心态运行的时间，单位为jiffies|
|priority|任务的动态优先级|
|nice|任务的静态优先级|
|num_threads|该任务所在的线程组里线程的个数|
|it_real_value|由于计时间隔导致的下一个 SIGALRM 发送进程的时延，以 jiffy 为单位.|
|start_time|该任务启动的时间，单位为jiffies|
|vsize|该任务的虚拟地址空间大小|
|rss|该任务当前驻留物理地址空间的大小|
|rlim|该任务能驻留物理地址空间的最大值|
|start_code|该任务在虚拟地址空间的代码段的起始地址|
|end_code|该任务在虚拟地址空间的代码段的结束地址|
|start_stack|该任务在虚拟地址空间的栈的结束地址|
|kstkesp|esp(32 位堆栈指针) 的当前值, 与在进程的内核堆栈页得到的一致.|
|kstkeip|指向将要执行的指令的指针, EIP(32 位指令指针)的当前值.|
|pendingsig|待处理信号的位图，记录发送给进程的普通信号|
|block_sig|阻塞信号的位图|
|sigign|忽略的信号的位图|
|sigcatch|被俘获的信号的位图|
|wchan|如果该进程是睡眠状态，该值给出调度的调用点|
|nswap|被swapped的页数，当前没用|
|cnswap|所有子进程被swapped的页数的和，当前没用|
|exit_signal|该进程结束时，向父进程所发送的信号|
|task_cpu(task)|运行在哪个CPU上|
|task_rt_priority|实时进程的相对优先级别|
|task_policy|进程的调度策略，0=非实时进程，1=FIFO实时进程；2=RR实时进程|