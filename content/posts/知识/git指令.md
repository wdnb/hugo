---
date: 2015-06-13 19:11:00
status: public
tags:
  - command
title: git指令
categories:
  - Git
---
获取github上的项目

    git clone 项目地址

切换分支
    
    git checkout 'branchName'
从远程更新代码到本地
    
    git pull
添加代码到索引

    git add .
提交

    git commit -m 'update'
更新至远程

    git push origin master    
查看提交纪录
    
    git log
    git log -p xxxxxxxxxxxxxxxxx
回退更改
    
    git reset xxxxxxxxxxxxxxxxx


删除Git仓库所有提交历史记录，成为一个干净的新仓库

Checkout

    git checkout --orphan latest_branch
   Add all the files

    git add -A
Commit the changes

    git commit -am "commit message"
    
Delete the branch

    git branch -D master

Rename the current branch to master

    git branch -m master
Finally, force update your repository

    git push -f origin master

参考地址:
>https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github

git add -A 和 git add . 的区别

Git Version 1.x: 

![](git1.jpg)

Git Version 2.x: 

![](git2.jpg)


## 查看远程分支
```
git branch -a
```
## 回退更改
```
git reset xxxxxxxxxxxxxxxxx
```
## 查看本地分支
```
git branch
```
## 切换分支
```
git checkout  xxx
```
## 检查当前文件状态
```
git status
```
## 把master最新内容合并到我本地分支
```
git merge master
```
回退指定commt（merge）
分2种情况,当revert合并节点的commit的时候 会提示需要-m参数

使用
```
git show 30414f610826486efa8ce8b6303f51817c4e79d9
```
能看到 merge栏有 145ae2613 9bc158141 2个commit id, 

    commit 30414f610826486efa8ce8b6303f51817c4e79d9
    Merge: 145ae2613 9bc158141
    Author: wangyi <wangyi@lanqb.cn>
    Date:   Fri Oct 9 15:54:04 2020 +0800

## 同步私有化项目和原项目代码

参数 -m 就是指定要撤销的那个提价，从左往右，从1开始数

```
git revert 30414f610826486efa8ce8b6303f51817c4e79d9 -m 1 
```
撤销145ae2613分支带来的改动



## 回退到指定commit

```
git reset --hard xxx(
--hard 删除工作空间改动代码，撤销commit，撤销git add . 
注意完成这个操作后，就恢复到了上一次的commit状态
--mixed 不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的
--soft 不删除工作空间改动代码，撤销commit，不撤销git add .
 )
 ```

## 删除未跟踪文件
```
git clean [-d] [-f] [-i] [-n] [-q] [-e <pattern>] [-x | -X] [--] <path>...
```
```
git clean -df
```

    -d   # 删除未跟踪目录以及目录下的文件，如果目录下包含其他git仓库文件，并不会删除（-dff可以删除）。
    -f   # 如果 git cofig 下的 clean.requireForce 为true，那么clean操作需要-f(--force)来强制执行。
    -i   # 进入交互模式
    -n   # 查看将要被删除的文件，并不实际删除文件


## 删除远程分支
```
git push origin --delete LANQB-2974
```
## 删除本地分支 
```
git branch -d <BranchName>
```
## 获取冲突文件列表
```
git diff --name-only --diff-filter=U
```
对比本地分支和线上分支
先更新下本地的远程分支
```
git fetch origin
```
然后可以比对
```
git log feature/LANQB-3394-2..origin/feature/LANQB-3394-2
```
>https://www.zhihu.com/question/53601264


## gitignore文件中添加新过滤文件，但是此文件已经提交到远程库，如何解决？
第一步，为避免冲突需要先同步下远程仓库
```
git pull
```
第二步，在本地项目目录下删除缓存
```
git rm -r --cached .
```
第三步，再次add所有文件
输入以下命令，再次将项目中所有文件添加到本地仓库缓存中
```
git add .
```
第四步，添加commit，提交到远程库
```
git commit -m "filter new files"
```
```
git push
```
## LF will be replaced by CRLF

提交时转换为LF，检出时不转换
```
git config --global core.autocrlf input
```
或者安装换行符切换工具
```
sudo apt-get install dos2unix
```
```
find . -type f | xargs dos2unix
```
or
```
find . -type f -exec dos2unix {} +
```
```
git remote set-url origin git+ssh://git@github.com/username/reponame.git
```

## Git pull 强制覆盖本地文件
拉取所有更新不同步
```
git fetch --all
```
本地代码同步线上最新版本
```
git reset --hard origin/master
```

## 本地分支重命名

如果目标分支不是当前分支，可以使用下面代码：

```
git branch -m 原分支名 新分支名
```

如果是当前分支，那么可以使用加上新名字

```
git branch -m 原分支名称
```

## 关联本地分支到远程分支

```
git branch -u origin/feature/DACHU-act05 feature/DACHU-act05
```

## 切换ssh和http协议
### 查看当前remote
```
git remote -v
```
```
origin  git@gitlab.go-goal.cn:mis/mxxmsg.git (fetch)
```
```
origin  git@gitlab.go-goal.cn:mis/mxxg.git (push)
```
### 从http和ssh互相切换
```
git remote set-url origin url.git
```


