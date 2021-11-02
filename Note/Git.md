# Git常见命令汇总

## 前言

Git（读音为/gɪt/）是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理

<!-- more -->

## 流程图

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1d538d63559402fbcfd82d68b08061c~tplv-k3u1fbpfcp-watermark.awebp)

## 流程

1. 在工作区开发，添加，修改文件
2. 将修改后的文件放入暂存区
3. 将暂存区域的文件提交到本地仓库
4. 将本地仓库的修改推送到远程仓库

## 工作区域

- Workspace：工作区
  - 就是平时进行开发改动的地方，是当前看到最新的内容，在开发的过程也就是对工作区的操作
- Index：暂存区
  - 当执行 `git add` 的命令后，工作区的文件就会被移入暂存区，暂存区标记了当前工作区中那些内容是被 Git 管理的，当完成某个需求或者功能后需要提交代码，第一步就是通过 `git add` 先提交到暂存区
- Repository：本地仓库
  - 位于自己的电脑上，通过 `git commit` 提交暂存区的内容，会进入本地仓库
- Remote：远程仓库
  - 用来托管代码的服务器，远程仓库的内容能够被分布在多个地点的处于协作关系的本地仓库修改，本地仓库修改完代码后通过 `git push` 命令同步代码到远程仓库

## 基本操作

- 创建本地仓库 
  - git init
- 链接本地仓库与远端仓库
  - git remote add  origin url
  - origin默认是远端仓库别名  url 可以是**可以使用https或者ssh的方式新建**
- 检查配置信息
  - git config --list
  - git config --global user.name "yourname"
  - git config --global user.email  "your_email"
- 生成SSH密钥
  - ssh-keygen -t rsa -C "这里换上你的邮箱"
  - cd ~/.ssh 里面有一个文件名为id_rsa.pub,把里面的内容复制到git库的我的SSHKEYs中
- 常看远端仓库信息
  - git remote -v
- 远端仓库重新命名
  - git remote rename old new
- 显示工作目录和暂存区的状态
  -  git status
    - Changes to be committed
      - 已经在stage区, 等待添加到HEAD中的文件
    -  Changes not staged for commit
      - 有修改, 但是没有被添加到stage区的文件
    - Untracked files
      - 没有tracked过的文件, 即从没有add过的文件
- 提交到缓存区
  - git add .  全部上传到缓存区
  - git add   指定文件
- 提交到本地仓库
  - git commit -m 'some message'
- 提交远程仓库
  - git push <远程主机名> <本地分支名>:<远程分支名>
    - git push origin master
      - 将本地的`master`分支推送到`origin`主机的`master`分支。如果`master`不存在，则会被新建
    - git push origin
      - 将当前分支推送到`origin`主机的对应分支。如果当前分支只有一个追踪分支，那么主机名都可以省略
- 查看分支
  - git  branch
- 创建新分支
  - git branch 
- 切换分支
  - git checkout 
- 创建分支并切换
  - git checkout -b 
- 删除分支
  - git branch -d 
- 删除远程分支
  - git branch -d
- 切换分支
  - git checkout
- 将远程存储库中的更改合并到当前分支中
  - git pull [options] [<repository> [<refspec>…]]
  - git pull origin next:master
    - 取回`origin`主机的`next`分支，与本地的`master`分支合并
  - git pull origin next
    - 远程分支(`next`)要与当前分支合并，则冒号后面的部分可以省略

