# Git常见命令汇总

## 前言

Git（读音为/gɪt/）是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理

<!-- more -->

## 流程图

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1d538d63559402fbcfd82d68b08061c~tplv-k3u1fbpfcp-watermark.awebp)

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

