---
date: 2015-06-13 19:11:00
tags:
  - command
title: git指令
categories:
  - Git
---
[GIT COMMANDS](https://git-scm.com/docs/git#_git_commands)
# 分支操作

## reset commit
建议reset只用于本地还没提交的commit reset后会将工作空间重置回目标commit 用于线上容易翻车
```bash
git reset HEAD
 ```
## 丢弃本地改动 同步为线上最新代码

### 拉取线上最新代码
```bash
git fetch --all
```
### 本地代码reset到线上最新版本
```bash
git reset --hard origin/<BranchName>
```
 
- --hard 删除工作空间改动代码，撤销commit，撤销git add . 
- 注意完成这个操作后，就恢复到了上一次的commit状态
- --mixed 不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
- 这个为默认参数,git reset --mixed HEAD 和 git reset HEAD 效果是一样的
- --soft 不删除工作空间改动代码，撤销commit，不撤销git add .

## revert commit（merge）
分常规commit和merge commit两种情况
当revert merge commit的时候 会提示需要-m参数

### revert merge commit
使用
```bash
git show 30414f610826486efa8ce8b6303f51817c4e79d9
```
能看到 merge栏有 145ae2613 9bc158141 2个commit id, 
```bash
    commit 30414f610826486efa8ce8b6303f51817c4e79d9
    Merge: 145ae2613 9bc158141
    Author: xx <x@x.com>
    Date:   Fri Oct 9 15:54:04 2020 +0800
```
参数 -m 就是指定要撤销的那个提交，从左往右，从1开始数

```bash
git revert 30414f610826486efa8ce8b6303f51817c4e79d9 -m 1 
```
撤销145ae2613分支带来的改动

## 查看远程分支
```bash
git branch -a
```
## 查看本地分支
```bash
git branch
```
## 切换本地分支
```bash
git checkout 'branchName'
```
## 合并本地分支
```bash
git merge 'branchName'
```
## 本地分支重命名

如果目标分支不是当前分支，可以使用下面代码：

```bash
git branch -m 原分支名 新分支名
```

如果是当前分支，那么可以使用加上新名字

```bash
git branch -m 原分支名称
```

## 关联本地分支到远程分支

```bash
git branch -u origin/feature/DACHU-act05 feature/DACHU-act05
```

## 获取冲突文件列表（对比2个分支差别）
```bash
git diff --name-only --diff-filter=U
```
对比本地分支和线上分支
先更新下本地的远程分支
```bash
git fetch origin
```
然后可以比对
```bash
git log feature/LANQB-3394-2..origin/feature/LANQB-3394-2
```
参考地址：
>https://www.zhihu.com/question/53601264

## 删除远程分支
```bash
git push origin --delete LANQB-2974
```
## 删除本地分支 
```bash
git branch -d <BranchName>
```
# 代码操作

## 获取git上的项目
```bash
git clone 项目地址
```
## 从远程更新代码到本地
git pull = git fetch + git merge
```bash
git pull
```

## 添加代码到索引
```bash
git add .
```

### git add -A 和 git add . 的区别

Git Version 1.x: 

![](img/git指令/git1.jpg)

Git Version 2.x: 

![](img/git指令/git2.jpg)

## 检查当前文件状态
```bash
git status
```

## 写入暂存区
```bash
git commit -m 'update'
```
## 推送至远程
```bash
git push origin <BranchName>    
```
## 查看提交纪录
```bash
git log
```
```bash	
git log -p xxxxxxxxxxxxxxxxx
```	

## 删除未跟踪文件
```bash
git clean [-d] [-f] [-i] [-n] [-q] [-e <pattern>] [-x | -X] [--] <path>...
```
```bash
git clean -df
```

    -d   # 删除未跟踪目录以及目录下的文件，如果目录下包含其他git仓库文件，并不会删除（-dff可以删除）。
    -f   # 如果 git cofig 下的 clean.requireForce 为true，那么clean操作需要-f(--force)来强制执行。
    -i   # 进入交互模式
    -n   # 查看将要被删除的文件，并不实际删除文件


# 常见问题

## LF will be replaced by CRLF

提交时转换为LF，检出时不转换
```bash
git config --global core.autocrlf input
```
或者安装换行符切换工具
```bash
sudo apt-get install dos2unix
```
```bash
find . -type f | xargs dos2unix
```
or
```bash
find . -type f -exec dos2unix {} +
```

```bash
git remote set-url origin git+ssh://git@github.com/username/reponame.git
```

## 切换ssh和http协议

### 查看当前remote
```bash
git remote -v
```
```text
origin  git@gitlab.go-goal.cn:mis/mxxmsg.git (fetch)
origin  git@gitlab.go-goal.cn:mis/mxxg.git (push)
```
### 从http和ssh互相切换
```bash
git remote set-url origin url.git
```

## 删除Git仓库所有提交历史记录，成为一个干净的新仓库
参考地址:
>https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github
- Checkout
```bash
git checkout --orphan latest_branch
```	
- Add all the files
```bash
git add -A
```	
- Commit the changes
```bash
git commit -am "commit message"
```   
- Delete the branch
```bash
git branch -D master
```
- Rename the current branch to master
```bash
git branch -m master
```
- Finally, force update your repository
```bash
git push -f origin master
```

## .gitignore文件中添加新过滤文件，但是此文件已经提交到远程库，如何解决？
第一步，为避免冲突需要先同步下远程仓库
```bash
git pull
```
第二步，在本地项目目录下删除缓存
```bash
git rm -r --cached .
```
第三步，再次add所有文件
输入以下命令，再次将项目中所有文件添加到本地仓库缓存中
```bash
git add .
```
第四步，添加commit，提交到远程库
```bash
git commit -m "filter new files"
```
```bash
git push
```
