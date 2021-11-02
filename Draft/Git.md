# Git

## 工作区域

- Workspace：工作区
  - 就是平时进行开发改动的地方，是当前看到最新的内容，在开发的过程也就是对工作区的操作
- Index：暂存区
  - 当执行 `git add` 的命令后，工作区的文件就会被移入暂存区，暂存区标记了当前工作区中那些内容是被 Git 管理的，当完成某个需求或者功能后需要提交代码，第一步就是通过 `git add` 先提交到暂存区
- Repository：本地仓库
  - 位于自己的电脑上，通过 `git commit` 提交暂存区的内容，会进入本地仓库
- Remote：远程仓库
  - 用来托管代码的服务器，远程仓库的内容能够被分布在多个地点的处于协作关系的本地仓库修改，本地仓库修改完代码后通过 `git push` 命令同步代码到远程仓库

## 流程

1. 在工作区开发，添加，修改文件
2. 将修改后的文件放入暂存区
3. 将暂存区域的文件提交到本地仓库
4. 将本地仓库的修改推送到远程仓库





## 廖雪峰

- 创建版本库`git init`

- 把文件添加到版本库`git add readme.txt`

- 文件提交到仓库`git commit`，`-m`后面输入的是本次提交的说明

- 为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

  ```
  $ git add file1.txt
  $ git add file2.txt file3.txt
  $ git commit -m "add 3 files."
  ```

- ### 小结

  现在总结一下今天学的两点内容：

  初始化一个Git仓库，使用`git init`命令。

  添加文件到Git仓库，分两步：

  1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
  2. 使用命令`git commit -m <message>`，完成



- `git status`命令可以让我们时刻掌握仓库当前的状态

- `git diff`这个命令看看具体修改了什么内容

- ### 小结

  - 要随时掌握工作区的状态，使用`git status`命令。
  - 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。



- ```
  git log --pretty=oneline
  ```

- 在Git中，用`HEAD`表示当前版本

- 上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`

- Git提供了一个命令`git reflog`用来记录你的每一次命令

- Git在内部有个指向当前版本的`HEAD`指针

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ wrote a readme file
```

- ### 小结

  现在总结一下：

  - `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
  - 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
  - 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本



- Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

  ![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

- 创建Git版本库时，Git自动为我们创建了唯一一个`master`分支

- 需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改

- Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`

  - ```
    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
    	modified:   readme.txt
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
    	LICENSE
    
    no changes added to commit (use "git add" and/or "git commit -a")
    ```

- git diff：

  上面已经总结了。

  1. git diff：是查看working tree与index的差别的。
  2. git diff --cached：是查看index与repository的差别的。
  3. git diff HEAD：是查看working tree和repository的差别的。其中：HEAD代表的是最近的一次commit的信息。



- `git checkout -- file`可以丢弃工作区的修改，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况
  
  - `readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态
  - `readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态
  
- 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态

- `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令

- `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

- `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本

- 从暂存区恢复工作区，

  git resotre --worktree readme.txt

  从master恢复暂存区 

  git restore --staged readme.txt

  从master同时恢复工作区和暂存区

  git restore --source=HEAD --staged --worktree readme.txt

  

- 从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`

- 删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本git checkout -- test.txt

- `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”















## git配置命令

- 配置用户信息
  1. 配置用户名：`git config --global user.name "your name"`
  2. 配置用户邮箱：`git config --global user.email "youremail@github.com"`
- 查询配置信息
  1. 列出当前配置：`git config --list`
  2. 列出repository配置：`git config --local --list`
  3. 列出全局配置：`git config --global --list`
  4. 列出系统配置：`git config --system --list`
- 其他配置
  1. 配置解决冲突时使用哪种差异分析工具，比如要使用vimdiff：`git config --global merge.tool vimdiff`
  2. 配置git命令输出为彩色的：`git config --global color.ui auto`
  3. 配置git使用的文本编辑器：`git config --global core.editor vi`

## 工作区上的操作命令

- 新建仓库
  1. 创建一个新的本地仓库：`git init`
  2. 从远程git仓库复制项目：`git clone <url>`
  3. 可以在clone命令后指定新的项目名：`git clone git://github.com/wasd/example.git mygit`
- 提交
  1. 提交工作区所有文件到暂存区：`git add .`
  2. 提交工作区中指定文件到暂存区：`git add <file1> <file2> ...`
  3. 提交工作区中某个文件夹中所有文件到暂存区：`git add [dir]`
- 撤销
  1. 删除工作区文件，并且也从暂存区删除对应文件的记录：`git rm <file1> <file2>`
  2. 从暂存区中删除文件，但是工作区依然还有该文件:`git rm --cached <file>`
  3. 取消暂存区已经暂存的文件：`git reset HEAD <file>...`
  4. 撤销上一次对文件的操作：`git checkout --<file>`
  5. 隐藏当前变更，以便能够切换分支：`git stash`
  6. 查看当前所有的储藏：`git stash list`
  7. 应用最新的储藏：`git stash apply`，如果想应用更早的储藏：`git stash apply stash@{2}`；重新应用被暂存的变更，需要加上`--index`参数：`git stash apply --index`
  8. 使用apply命令只是应用储藏，而内容仍然还在栈上，需要移除指定的储藏：`git stash drop stash{0}`；如果使用pop命令不仅可以重新应用储藏，还可以立刻从堆栈中清除：`git stash pop`
  9. 在某些情况下，你可能想应用储藏的修改，在进行了一些其他的修改后，又要取消之前所应用储藏的修改。Git没有提供类似于 stash unapply 的命令，但是可以通过取消该储藏的补丁达到同样的效果：`git stash show -p stash@{0} | git apply -R`；同样的，如果你沒有指定具体的某个储藏，Git 会选择最近的储藏：`git stash show -p | git apply -R`





## Git基本操作

- git add
  - ```
    # 添加某个文件到暂存区，后面可以跟多个文件，以空格区分
    git add xxx
    # 添加当前更改的所有文件到暂存区。
    git add .
    ```

- git commit

  - ```
    # 提交暂存的更改，会新开编辑器进行编辑
    git commit 
    # 提交暂存的更改，并记录下备注
    git commit -m "you message"
    # 等同于 git add . && git commit -m
    git commit -am
    # 对最近一次的提交的信息进行修改,此操作会修改commit的hash值
    git commit --amend
    ```

- git pull

  - ```
    # 从远程仓库拉取代码并合并到本地，可简写为 git pull 等同于 git fetch && git merge 
    git pull <远程主机名> <远程分支名>:<本地分支名>
    # 使用rebase的模式进行合并
    git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
    ```

- git fetch

  - ```
    # 获取远程仓库特定分支的更新
    git fetch <远程主机名> <分支名>
    # 获取远程仓库所有分支的更新
    git fetch --all
    ```

  - 与 `git pull` 不同的是 `git fetch` 操作仅仅只会拉取远程的更改，不会自动进行 merge 操作。对你当前的代码没有影响

- git branch

  - ```
    # 新建本地分支，但不切换
    git branch <branch-name> 
    # 查看本地分支
    git branch
    # 查看远程分支
    git branch -r
    # 查看本地和远程分支
    git branch -a
    # 删除本地分支
    git branch -D <branch-nane>
    # 重新命名分支
    git branch -m <old-branch-name> <new-branch-name>
    ```

## git rebase

- rebase 翻译为变基，他的作用和 merge 很相似，用于把一个分支的修改合并到当前分支上









## 基本概念

- 版本库.git文件
  - 使用git管理文件时，git文件
- 工作区
  - 本地项目存放文件的位置
- 暂存区
  - 通过add命令将工作区的文件添加到缓冲区
- 本地仓库
  - commit命令将暂存区的文件添加到本地仓库
  - 通常而言，HEAD指针指向的就是master分支
- 远程仓库
  - GitHub

## Git文件状态

- git status
  - Changes not staged for commit
    - 缓存区没有，需要我们`git add`
  - Changes to be committed
    - 文件放在缓存区了，我们需要`git commit`
  - nothing to commit, working tree clean
    - 这个时候，我们将本地的代码推送到远端即可

## 常见命令

### git配置命令

- ```
  git config --global user.name "your name"
  ```

  - 配置用户名

- ```
  git config --global user.email "youremail@github.com"
  ```

  - 配置用户邮箱

- ```
  git config --list	
  ```

  - 列出当前配置

- ```
  git config --local --list
  ```

  - 列出Repository配置

- ```
  git config --global --list
  ```

  - 列出全局配置

- ```
  git config --system --list
  ```

  - 列出系统配置

