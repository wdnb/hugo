---
date: 2016-07-26 11:16:00
tags:
  - sendmail
title: linux下sendmail邮件发送提示stat=Deferred
categories:
  - Linux
---


邮件发送日志所在位置:`/var/log/maillog`以及`/var/spool/mail/*`
自建邮箱sendmail无法发送邮件,日志提示
stat=Deferred,
550 5.1.1 user unknown,
503 5.0.0 Need RCPT (recipient),
邮件被拒收,没有成功发送.
这里分析是由于SMTP协议设计上的缺陷,发邮件的时候随便填写一下别人的域名，就可以冒充别的网站来发送邮件了,自建邮箱没有配置SPF信息就被收件方服务提供商当作垃圾邮件直接拒收处理了.

首先检查邮件服务是否启动,这里使用的是sendmail, `service sendmail start`

1:修改本地主机名 `hostname -v DOMAIN.com`
2:在域名控制处(上面的域名DOMAIN.com)添加SPF记录 主机记录@,记录类型TXT,记录值 `v=spf1 ip4:1.1.1.1 ~all`,保存. 这里ip4后面的ip是你邮件发送服务器的固定ip

等待域名解析生效.