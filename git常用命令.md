---
title: git常用命令
date: 2018-08-17 14:08:16
tags:
---

## git 常用命令


- git config --global user.name "tangchunxin"
- git config --global user.email "tangchunxin@p2peye.com"
- git config --global credential.helper store

#### git强制覆盖：
-   git fetch --all
-     git reset --hard origin/master
-     git pull
 
    
- git stash  //将本地修改临时暂存起来
- git stash pop //从暂存区还原
- git stash list  //暂存区列表
- git stash pop stash@{0} //从暂存区还原

- git status
- git add a.php   //提交到本地暂存区
- git commit -m "提交a.php"  //提交到本地仓库
- git push origin   //推送到远程
- git branch -a  //查看远程分支状态
- git branch dev  //创建分支
- git checkout dev  //切分支
- git checkout -b  dev  //创建并切分支
- 
- git push origin dev  将本地新建的分支 推到远程
- git merge dev  把dev的代码 合并到当前分支
- git log --oneline
- git diff  //工作区 和暂存区
- git diff --cached  //暂存区和历史版本
- git diff master   //工作去和历史版本
- 
- git diff  946de  go.txt
- 


- git checkout -- go.txt  撤销本地的修改
- git reset HEAD a.php   从暂存区撤回到 本地


假设远端库名是 origin，你要比较的本地分支为 test，远端分支就是 xxx



本地分支的话，直接git diff branchA branchB，而远程分支的话，git diff branchA remoteB/branchB，区别就是远程分支前面要加上remote名称。


- git reset --hard HEAD^  从本地库回退版本,本地修改都会被撤销
- git reset --mixed HEAD^  从本地仓库撤回代码 ,本地修改保存
- git reset --soft HEAD^  从本地仓库撤回代码,本地修改保存,暂存区也保存

#### 问题1.在dev3分支  pull master分支代码后,貌似dev3分支,失去了当前分支的追踪信息,因为重新找到追踪信息 git branch --set-upstream-to origin dev3 
```
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev3

```

- git push -u  加了参数-u后，以后即可直接用git push 代替git push origin master
- git push --set-upstream origin dev3


- git push origin dev3 --force
- 

```
Unable to create '.git/index.lock': File exists
2014年03月03日 14:10:27
阅读数：5159
Git – fatal: Unable to create ‘/.git/index.lock’: File exists.
fatal: Unable to create ‘/path/my_proj/.git/index.lock’: File exists.
If no other git process is currently running, this probably means a
git process crashed in this repository earlier. Make sure no other git
process is running and remove the file manually to continue.
可以试着删除 index.lock
rm -f ./.git/index.lock
```

