---
title: 使用gitlab部署hexo文章到github
date: 2020-07-18T22:37:22Z
tags:
  - gitlab
  - 树莓派
  - hexo
categories:
  - DevOps
---
# **!!!Warning!!!**
**12/8/2023：不再推荐此方法！如果只要部署静态博客到GitHub 请使用**
[GitHub Actions  workflows](https://docs.github.com/en/actions/using-workflows)

# 使用gitlab部署hexo文章到github

## 前记
使用GitLab 自动部署到github 省去了在自己电脑上装node 写好文章直接push就行，体验接近动态博客

为啥不直接发布到gitlab pages呢？我的gitlab是部署在树莓派里的，博客发布到gitlab的pages指不定哪天就凉了,还有xss的问题，所以还是发布到github吧

### 在gitlab中创建一个私有仓库

### 向项目目录下添加私钥

### 将公共ssh密钥添加到您的github

### 将.gitlab-ci.yml添加到您的项目

private_key是私钥
,hexo-generator-searchdb,用于生成网站搜索功能
,hexo-abbrlink --save这个是生成唯一永久文章链接用的,这些都需要相应的在hexo中配置才能生效
推送您的更改并查看结果:)
 
## .gitlab-ci.yml

```bash
image: node:lts-alpine
cache:
  paths:
    - node_modules/

before_script:
  - npm config set registry https://registry.npm.taobao.org/
  - npm install hexo-cli -g
  - npm install hexo-generator-searchdb --save
  - npm install hexo-abbrlink --save
  - npm install hexo-deployer-git --save
  - npm install
  - sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
  - apk --update add openssh-client
  - apk --update add git
  - eval $(ssh-agent -s)
  - chmod 700 private_key
  - ssh-add private_key
  - git config --global user.email "xxxx@xx.com"
  - git config --global user.name "xxxx"
  - echo StrictHostKeyChecking no >> /etc/ssh/ssh_config

pages:
  script:
    - hexo generate --deploy
  artifacts:
    expire_in: 1 day
    paths:
      - public
  only:
    - master
```
