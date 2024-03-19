---
date: ‎2020‎-08-17T17:38:09+08:00
tags:
  - hexo
title: hexo搭建笔记
categories:
  - Blog
---

# hexo自动化部署配置

`.gitlab-ci.yml`文件

```
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
  - git config --global user.name "xx"
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