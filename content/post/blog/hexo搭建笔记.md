---
date: 2015-06-13T20:12:00Z
tags:
  - hexo
title: hexo搭建笔记
categories:
  - Blog
---

请先参考官方网站指导
>http://hexo.io/zh-cn/docs/index.html 
还有这篇博文
>http://www.songchubai.com/2015/08/12/github-gitcafe/

安装前需要先安装**node.js**以及**git**
(记得将node的bin加入PATH /home/vagrant/.nvm/versions/node/v4.1.1/bin)
node.js安装成功后运行指令安装hexo

	npm install -g hexo-cli

安装完成后建立目录运行命令

    hexo init <folder>
    cd <folder>
    npm install

_config.yml配置

	deploy:
		type: git
		repository: git@github.com:wdnb/blog.git
		branch: gh-pages (主pages模式为master)

## 同时部署到多个代码托管网站(需要使用同一个密钥)

	deploy:
	  type: git
	  message: ""
	  repo: 
		gicafe: git@gitcafe.com:wdnb/wdnb.gitcafe.io.git,gitcafe-pages
		github: git@github.com:wdnb/wdnb.github.io.git,master

在github上
1.创建项目
2.建立分支gh-pages

指令 
生成静态文件并推送到服务器

	hexo d -g 

清除public目录下的文件
	
	hexo -clean

有ERROR Deployer not found : github问题尝试执行

	npm install hexo-deployer-git --save 
	
再运行

在github上配置你的域名

1：在hexo本地文件目录新建CNAME文件，内容为所用域名 如：blog.jbface.com
2：将域名解析到你的github主页网址 如:
主机记录blog,记录类型CNAME记录,记录值wdnb.github.io.
