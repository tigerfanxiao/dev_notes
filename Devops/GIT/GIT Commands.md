
# Linux Git Installment
```shell
# 在centos下用yum安装git
sudo yum install git -y
```
# Concepts
git分为四个地方. 
* 本地 working directory. 其实是当前我们在编程的环境. `git add .` 后放入 stage
* 本地 stage, 只有被追踪的文件才会被放在 stage 中
* 本地 repo, 这两面才存在多个不同的代码版本
* remote repo 
	* upstream or original upstream 指的是最原始的别人的代码库
	* origin 一般是指你fork 的代码库
	

### get changes
```shell
git fetch # 只是把变动拉下来, 但是不会自动 merge, 需要手动 merge
# git pull = git fetch + git merge
git pull # 会自动 merge 到 branch
```

### git rebase
当两个人修改同一个分支. 别人在这个分支上先提交了, 当你提交的时候就会被 reject. 然后你需要先下载的她的新内容. 再把自己的 commit 放在她的提交后面

```shell
# 使用 rebase 可以规避 git pull必然会多出一个没有意义的 merge commit
git pull origin main <url> --rebase
# not the first time 
git pull --rebase
```

把默认的 git pull 配置成 git rebase
```shell
# 配置 git pull 模式使用 rebase
git config pull.rebase true
# rollback git rebase configuration
git config pull.rebase false
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
git diff origin master # 对比远端和本地master
```
### git history

```shell
# show all commit history line by line 
git log --oneline
# go back to previous commit
git checkout 1a 
```

# Staging Area
```shell
git add . # 把当前目录下的文件加入 staging area
git add file01, file02 # 指定几个文件
git add -A # 把repo主目录下的所有文件都加入staging area, 这是默认操作
git add -u # 只是把deleted, modified 文件加入staging area, 对于untracked的文件不做操作
git add folder # 只是把folder中的文件都加入staing area
```


# merge
merge操作也会生成一次 commit

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
git merge <branch_B>

# 同理如果要删除 branch B, 则登录 main branch
git branch -d <branch_B>
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

# git rebase

-   Do not use rebase on commits that you're already pushed/shared on a remote repository. Instead, use it for cleaning up your local commit history before merge into a shared team branch.



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


# git bash
windows上的git bash
如果要配置开机启动


# reset

```shell
# 删除 working directory 和 staging area 中的变化, 完全恢复到最后一次 commit 的状态
git reset --hard HEAD

git revert HEAD --no-edit # HEAD 表示最近的一次 commit
```


# Use Case
### Go to the previous commit
```shell
git log --oneline
git checkout 
```