- Git 是分布式的版本管理工具. 分布式说的是每个终端都有一个完整的版本
- Git 使用保存不大的文件, 所以大文件上传git有问题, 应该用其他数据库的方式, 比如面向对象的存储来保存大的二进制文件, 图像, 视频这些信息
# Installment
```shell
# under centos 
sudo yum install git -y
git --version # show git install well
```
# Concepts
git分为四个地方. 
* 本地 working directory. 其实是当前我们在编程的环境. `git add .` 后放入 staging area. 此时为untrack状态
```shell
git restore <filename> # discard untracked change in working directory
```
* 本地 staging area, 只有被追踪的文件才会被放在 stage 中
```shell
git add . # 将文件从working area 保存到 staging area, 看做是暂存
git add file01, file02 # 指定几个文件
git add -A # 把repo主目录下的所有文件都加入staging area, 这是默认操作
git add -u # 只是把deleted, modified 文件加入staging area, 对于新的.,   文件不做操作
git add folder # 只是把folder中的文件都加入staing area

git restore --staged <file>  # 把文件从staging area 撤回working directory, 为untrack状态. 文件本身没有进入index
git restore -s <source> <file> # 取回某一次commit中文件的状态
git rm --cache <file> # 如果文件已经在git 的index中, 把文件从staging area 撤回working directory, 为untrack状态

# 对于已经被track的文件, 如果要修改文件名
git mv <oldfile> <newfile> # 使用mv命令是不能修改缓存区和版本库的内容的
```
- 使用`.gitignore` 文件来忽略某些临时文件. 注意 `.gitignore` 本身需要提交到仓库
```shell
*.swap # 所有vim的temp文件
*.class # java
temp # 目录
```
* 本地 repository, 这两面才存在多个不同的代码版本
```shell
git commit -m "notes" # 把staging中暂存的变动, 持久的保存到repo中. 在.git中保存着每次提交的生成的对象
git log # 查询提交记录. 这里 HEAD是提交的名称和链接, master也是. HEAD指向master
commit 878dee88d64a85a041fd07cda51591e6827299f1 (HEAD -> master)
Author: xiao <tigerfanxiao@gmail.com>
Date:   Thu Dec 4 13:18:05 2025 +0000
    init commit
git log --oneline
git log --stats # show staticstic information
git log <filename> # 只显示和某个文件有关的提交
git log --author="xiao" # 只显示和某个用户有关的提交, 比如自己
git log -p <commit-id> # 显示变动的详细信息

git show # show the detail of lastest commit
git show <commit-id> # show detail of single commit

```
* remote repo 
	* upstream or original upstream 指的是最原始的别人的代码库
	* origin 一般是指你fork 的代码库

- 本地目录下的`.git`目录中的存放这staging area和本地文件仓库
- tag 是某些重大事件, 比如发布某个版本. commit一般是作为每天工作的结束, 或者某个功能的完结
```shell
version-number.release-number.modification-number
git tag -a v1.1 -m "v1.1 Released." <commit-id>
git tag -l # list all tags
git tag -d <tag_name> # delete tag
git show <tag_name> # show tag object
git checkout <tag_name> # 会造成分离头指针
git push origin <tag_name> 
git push --tags # push all tags
git push --delete origin <tag_name> # delete remote tag
```
### Main and HEAD
HEAD in Git, **HEAD** is simply a **pointer** that tells you **where you currently are** in the repository.
main is default branch, and normally point to the latest commit. By default HEAD point to main
```shell
git log --oneline
878dee8 (HEAD -> master) init commit

HEAD~ # 表示HEAD的前一次提交
HEAD~2 # 表示HEAD前两次提交
```
### Git Object
- blob: 文件
- tree: 目录
- commit: 提交
- tag: 标签
```shell
git log --pretty=raw # show log history with git object 

git cat-file blob <blob-obj-id>
git ls-tree <tree-obj-id>
git log --pretty=raw <commit id>
git cat-file tag <tagname>
```

### Git Reference


# git config
- 配置区分为三个层级, 配置的格式`ini`
	- system 系统配置, `/etc/gitconfig`
	- global全局配置(对当前用户生效), `$HOME/.gitconfig`
	- local 配置(对当前repo 生效) `.git/config`
- 第一次使用都需要全局用户和邮箱. 也可以配置author或者committer
```shell
git config -l # 只有做了 git init . 的目录下才有返回
git config --global user.name <name>
git config --global user.email <email>
git config --global core.editor vim # 也可以通过 GIT_EDITOR 环境变量来配置, 且优先级更高
git config --global color.ui true # 启动语法着色功能

```
- 把默认的 git pull 配置成 git rebase
```shell
# 配置 git pull 模式使用 rebase
git config pull.rebase true
# rollback git rebase configuration
git config pull.rebase false
```


# Git Branches
- 一般情况下, 分支分为长期分支和短期分支. 
- 长期分支有:
	- main 分支保留发布历史
	- develop 分支保留开发历史
- 短期分支有:
	- feature 分支
	- fix-bug 分支

```shell
# 创建分支
git branch <branch name>
git branch -l # show all the branches
git checkout <branch name> # 切换分支
```
# merge
- merge 分为快进式合并和三路合并
- 快进式合并, 至分叉之后, 主分支不变, 新的分支修改完成后, 合并入主分支. 此时主分支的HEAD只要移动指针到最新的提交就行
- 三路合并主分支和新分支各自做了改变
- 每一次merge操作也会生成一次 commit

conflict 文件样式

```shell
<<<<<<<HEAD
# 这里是本地仓库的代码
=======
# 这里是远端仓库的代码
>>>>>>> # 本次的提交ID。

```
如果要把 branch B 合并到 Brach A 中, 就要先切换到 Branch A 来操作 merge
```shell
# 先切换到主要的 branch 上
git checkout master
git merge <branch_B> # 将brach_B 合并到master上

# 同理如果要删除 branch B, 则登录 main branch
git branch -d <branch_B>
```

### rebase
当两个人修改同一个分支. 别人在这个分支上先提交了, 当你提交的时候就会被 reject. 然后你需要先下载的她的新内容. 再把自己的 commit 放在她的提交后面

```shell
# 使用 rebase 可以规避 git pull必然会多出一个没有意义的 merge commit
git pull origin main <url> --rebase
# not the first time 
git pull --rebase
```


### fork
fork将 github 上别人的仓库, 复制到我的空间中, 且保留之前仓库的所有commit

文件状态分为
* untracked 没有git观察的
* modified 被修改的

```shell
# 查看仓库文件状态
git status
git commit -m 'initial commit'
```
### git diff

```shell

git diff #比较工作区和暂存区的变动
git diff --cached # 显示暂存尚未提交的文件变动, 比较暂存区与前一次提交的变动
git diff <commit id> # 显示制定的提交和工作区
git diff --cached <commit id> # 比较暂存区与制定的提交
 
```

- 比较上一次提交后, 到本地未暂存的变动

```shell
git diff origin master # 对比远端和本地master
```
### Restore the previous state
```shell
# show all commit history line by line 
git log --oneline
git log --stats # show staticstic information
# go back to previous commit
git checkout 1a 
# restore state for specific file
git checkout a1b2c3d -- app.ts
```


# Stash
Stash 默认情况下, 不会关注 untrack 的文件
stash并不依赖于分支, 所以本质上, 你可以将当前分支的修改 stash 后, 切换另一个分支呈现

Use case

-   当你在修改代码时, 并不想要做commit, 然后突然想去其他的branch上查看一些信息.
-   你发现本应该在其他branch上完成的修改, 结果在master branch上做了, 你想把这部分修改移动到其他branch上

```shell
git stash # stash 当前修改
git stash save -m "worked on add function" # 指定名字 stash
git stash -u # include also untracked file
# 应用修改
git stash pop # 应用最近一次修改, 并删除
git stash apply <stash@{1}>  # 指定名字应用 stash

# show your stash list 
git stash list # bring back your last stash and drop it 
# drop the specific stash 
git stash drop <stash@{1}> 
# drop all the stash 
git stash clear
```


# Best Practice

-   merge the update locally
-   Only commit the changes belong to one topic in one commit

# rebase
- 为了解决merge时, 在合并分支上提交历史丢失的问题
- 本质上是把合并分支的commit 作为新的commit 直接复制到被合并分支的commit后面. 可能造成冲突, 但是可以尽最大可能保留合并分支上的历史commit信息
- rebase的问题是, 如果存在大量conflict, 会造成merge hell
- 注意rebase操作和merge相反. 需要切换到合并分支上, 制定rebase的commit到被合并分支的commit. 然后在做一次快进式合并
- Do not use rebase on commits that you're already pushed/shared on a remote repository. Instead, use it for cleaning up your local commit history before merge into a shared team branch.

```shell
git checkout feature_branch
git rebase master # rebase the feature_branch to master branch latest commit
git checkout master
git merge feature_branch
```

# Large File Error
[参考文章](https://medium.com/analytics-vidhya/tutorial-removing-large-files-from-git-78dbf4cf83a)

### 不要做
不要直接删除文件，然后重新commit， 这样远程仓库还是会不断的提示你要上传那个巨大的文件

### 最近一次commit上传大文件
```shell
git rm --cached csv_building_damage_assessment.csv  
git commit --amend -C HEAD
```


### 在之前的commit中上传了大文件
```shell
git log --oneline
git rebase -i 8464da4 # 找到上传大文件那次commit之前的commit
# git rebase 执行后， 会有3个步骤
```

把上传文件的commit中的pick操作改成edit
```shell
edit d1bfae6 download data CSV  
pick de69e51 preliminary exploratory data analysis  
pick 099e6e4 update gitignore to ignore large data file
```

此时， 删除文件
```shell
git rm --cached csv_building_damage_assessment.csv  
git commit --amend --allow-empty -C HEAD
git rebase --continue
# 如果操作成功返回
# Successfully rebased and updated refs/heads/master.
```


# rebase -i
可以用交互的方式修改之前的commit


# Gitbash
windows上的git bash
如果要配置开机启动
打开gitbash时, 直接到指定笔记的目录

1. 首先在用户目录下创建`~/.bash_profile`
```shell
# generated by Git for Windows

cd /c/AllDoc/dev_notes
```
# reset vs revert
- reset命令会修改history, revert 不会修改历史, 而是新增一个commit
- reset 有三种模式 soft, mixed, hard

```shell
git reset --soft HEAD~ # 当前已经staged的变化不变. HEAD退回前一个commit, 历史记录消失

git reset HEAD~ # 默认是mixed, staged的变动会被移动到working direcotory, HEAD退还前一个commit, 历史记录消失

# 删除 working directory 和 staging area 中的变化, 完全恢复到最后一次 commit 的状态
git reset --hard HEAD

git revert HEAD --no-edit # HEAD 表示最近的一次 commit
```

# Best Practice

### Go to the previous commit
```shell
git log --oneline
git checkout <commit id>
```

# Remote repo

正常情况下, 我想要fork一个标准库到自己的github空间
origin 表示我们remote的仓库. 比如我们从别人的仓库fork到自己的空间的库
### push local code to remote repo
- origin 表示我自己的 fork
- upstream 表示原作者的仓库
```shell
# 添加一个remote 仓库
git remote add <repo_name> <url>
# 查看所有的 remote 仓库
git remote -v # 一般可以看到两行. push一个url, pull一个url是可以分开的
# 删除连接的仓库
git remote rm <repo_name> 

# 如果远端没有这个分支,则会创建. 如果有,则添加一个新的 commit
git push -u origin main # 或者main以外的分支
```
当我提交了本地的修改到本地库后, 本地的HEAD和master都指向了commit-5, 但是远程的仓库origin/master, origin/HEAD 还是指向commit-4, 因为我们还没有push到远程仓库
![[Pasted image 20251205173417.png]]
可以看到本地库 HEAD -> master, 远程库 origin/master origin/HEAD 多指向了同一个commit-5
![[Pasted image 20251205173259.png]]
pull code
```shell
git fetch # 只是把变动拉下来, 但是不会自动 merge, 需要手动 merge

# git pull = git fetch + git merge
git pull # 会自动 merge 到 branch

git branch -r # list all branch in remote
git branch -vv # show the mapping relationship fo local and remote

git merge origin/<branch> # 通常合并的都是同名的分支

git push remote --all # 推送所有分支
git push remote --tag # 推送所有 tag
```

# git 协作

不在同一个办公室, 甚至在不同的国家, 进行代码推送的时候会遇到的问题
- git-flow 分支模型
master/develop/feature/release/hotfix


# Git Sec
- 一个新程序员将自己的代码推送到了master或者develop 分支上?
- 