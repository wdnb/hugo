---
date: 2016-09-01T14:36:00Z
tags:
  - passwd
title: etc passwd文件解释
categories:
  - Linux
---

`cat /etc/passwd`

        root:x:0:0:root:/root:/bin/bash 
    
    　　bin:x:1:1:bin:/bin:/sbin/nologin 
    
    　　daemon:x:2:2:daemon:/sbin:/sbin/nologin 
    
    　　desktop:x:80:80:desktop:/var/lib/menu/kde:/sbin/nologin 
    
    　　mengqc:x:500:500:mengqc:/home/mengqc:/bin/bash 
    
　　在该文件中，每一行用户记录的各个数据段用“：”分隔，分别定义了用户的各方面属性。各个字段的顺序和含义如下： 

　　注册名：口令：用户标识号：组标识号：用户名：用户主目录：命令解释程序 

　　(1)注册名(login_name)：用于区分不同的用户。在同一系统中注册名是惟一的。在很多系统上，该字段被限制在8个字符(字母或数字)的长度之内；并且要注意，通常在Linux系统中对字母大小写是敏感的。这与MSDOS/Windows是不一样的。 

　　(2)口令(passwd)：系统用口令来验证用户的合法性。超级用户root或某些高级用户可以使用系统命令passwd来更改系统中所有用户的口令，普通用户也可以在登录系统后使用passwd命令来更改自己的口令。 

　　现在的Unix/Linux系统中，口令不再直接保存在passwd文件中，通常将passwd文件中的口令字段使用一个“x”来代替，将/etc /shadow作为真正的口令文件，用于保存包括个人口令在内的数据。当然shadow文件是不能被普通用户读取的，只有超级用户才有权读取。 

　　此外，需要注意的是，如果passwd字段中的第一个字符是“*”的话，那么，就表示该账号被查封了，系统不允许持有该账号的用户登录。 

　　(3)用户标识号(UID)：UID是一个数值，是Linux系统中惟一的用户标识，用于区别不同的用户。在系统内部管理进程和文件保护时使用 UID字段。在Linux系统中，注册名和UID都可以用于标识用户，只不过对于系统来说UID更为重要；而对于用户来说注册名使用起来更方便。在某些特 定目的下，系统中可以存在多个拥有不同注册名、但UID相同的用户，事实上，这些使用不同注册名的用户实际上是同一个用户。 

　　(4)组标识号(GID)：这是当前用户的缺省工作组标识。具有相似属性的多个用户可以被分配到同一个组内，每个组都有自己的组名，且以自己的组标 识号相区分。像UID一样，用户的组标识号也存放在passwd文件中。在现代的Unix/Linux中，每个用户可以同时属于多个组。除了在 passwd文件中指定其归属的基本组之外，还在/etc/group文件中指明一个组所包含用户。 

　　(5)用户名(user_name)：包含有关用户的一些信息，如用户的真实姓名、办公室地址、联系电话等。在Linux系统中，mail和finger等程序利用这些信息来标识系统的用户。 

　　(6)用户主目录(home_directory)：该字段定义了个人用户的主目录，当用户登录后，他的Shell将把该目录作为用户的工作目录。 在Unix/Linux系统中，超级用户root的工作目录为/root；而其它个人用户在/home目录下均有自己独立的工作环境，系统在该目录下为每 个用户配置了自己的主目录。个人用户的文件都放置在各自的 

　　主目录下。 

　　(7)命令解释程序(Shell)：Shell是当用户登录系统时运行的程序名称，通常是一个Shell程序的全路径名， 

　　如/bin/bash。 

　　需要注意的是，系统管理员通常没有必要直接修改passwd文件，Linux提供一些账号管理工具帮助系统管理员来创建和维护用户账号。 

　　Linux口令管理之/etc/passwd文件 

　　/etc/passwd文件是Linux/UNIX安全的关键文件之一.该文件用于用户登录时校验 用户的口令,当然应当仅对root可写.文件中每行的一般格式为: 

　　LOGNAME:PASSWORD:UID:GID:USERINFO:HOME:SHELL 

　　每行的头两项是登录名和加密后的口令,后面的两个数是UID和GID,接着的 一项是系统管理员想写入的有关该用户的任何信息,最后两项是两个路径名: 一个是分配给用户的HOME目录,第二个是用户登录后将执行的shell(若为空格则 缺省为/bin/sh). 

　　(1)口令时效 

　　/etc/passwd文件的格式使系统管理员能要求用户定期地改变他们的口令. 在口令文件中可以看到,有些加密后的口令有逗号,逗号后有几个字符和一个 冒号.如: 

　　steve:xyDfccTrt180x,M.y8:0:0:admin:/:/bin/sh 

　　restrict:pomJk109Jky41,.1:0:0:admin:/:/bin/sh 

　　pat:xmotTVoyumjls:0:0:admin:/:/bin/sh 

　　可以看到,steve的口令逗号后有4个字符,restrict有2个,pat没有逗号. 

　　逗号后第一个字符是口令有效期的最大周数,第二个字符决定了用户再次 修改口信之前,原口令应使用的最小周数(这就防止了用户改了新口令后立刻 又改回成老口令).其余字符表明口令最新修改时间. 

　　要能读懂口令中逗号后的信息,必须首先知道如何用passwd_esc计数,计 数的方法是: 

　　.=0 /=1 0-9=2-11 A-Z=12-37 a-z=38-63 

　　系统管理员必须将前两个字符放进/etc/passwd文件,以要求用户定期的 修改口令,另外两个字符当用户修改口令时,由passwd命令填入. 

　　注意:若想让用户修改口令,可在最后一次口令被修改时,放两个".",则下 一次用户登录时将被要求修改自己的口令. 

　　有两种特殊情况: 

　　. 最大周数(第一个字符)小于最小周数(第二个字符),则不允许用户修改 口令,仅超级用户可以修改用户的口令. 

　　. 第一个字符和第二个字符都是".",这时用户下次登录时被要求修改口 令,修改口令后,passwd命令将"."删除,此后再不会要求用户修改口令. 

　　(2)UID和GID 

　　/etc/passwd中UID信息很重要,系统使用UID而不是登录名区别用户.一般 来说,用户的UID应当是独一无二的,其他用户不应当有相同的UID数值.根据惯 例,从0到99的UID保留用作系统用户的UID(root,bin,uucp等). 

　　如果在/etc/passwd文件中有两个不同的入口项有相同的UID,则这两个用 户对相互的文件具有相同的存取权限.

　　/etc /group文件含有关于小组的信息,/etc/passwd中的每个GID在本文件中 应当有相应的入口项,入口项中列出了小组名和小组中的用户.这样可方便地了 解每个小组的用户,否则必须根据GID在/etc/passwd文件中从头至尾地寻找同组 用户. 

　　/etc/group文件对小组的许可权限的控制并不是必要的,因为系统用UID,GID (取自/etc/passwd)决定文件存取权限,即使/etc/group文件不存在于系统中,具 有相同的GID用户也可以小组的存取许可权限共享文件. 

　　小组就像登录用户一样可以有口令.如果/etc/group文件入口项的第二个域 为非空,则将被认为是加密口令,newgrp命令将要求用户给出口令,然后将口令加 密,再与该域的加密口令比较. 

　　给 小组建立口令一般不是个好作法.第一,如果小组内共享文件,若有某人猜 着小组口令,则该组的所有用户的文件就可能泄漏;其次,管理小组口令很费事, 因为对于小组没有类似的passwd命令.可用/usr/lib/makekey生成一个口令写入 /etc/group. 

　　以下情况必须建立新组: 

　　(1)可能要增加新用户,该用户不属于任何一个现有的小组. 

　　(2)有的用户可能时常需要独自为一个小组. 

　　(3)有的用户可能有一个SGID程序,需要独自为一个小组. 

　　(4)有时可能要安装运行SGID的软件系统,该软件系统需要建立一个新组. 

　　要 增加一个新组,必须编辑该文件,为新组加一个入口项. 由于用户登录时,系统从/etc/passwd文件中取GID,而不是从/etc/group中 取GID,所以group文件和口令文件应当具有一致性.对于一个用户的小组,UID和 GID应当是相同的.多用户小组的GID应当不同于任何用户的UID,一般为5位数,这 样在查看/etc/passwd文件时,就可根据5位数据的GID识别多用户小组,这将减少 增加新组,新用户时可能产生的混淆.
