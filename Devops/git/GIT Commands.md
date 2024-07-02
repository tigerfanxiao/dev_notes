
# Install git on centos
```shell
# 在centos下用yum安装git
sudo yum install git -y
```
# 概念
git分为四个地方. 
* 本地 working directory. 其实是当前我们在编程的环境. `git add .` 后放入 stage
* 本地 stage, 只有被追踪的文件才会被放在 stage 中
* 本地 repo, 这两面才存在多个不同的代码版本
* 远程 repo, 这是提交到远端服务器的代码

### 复制仓库

```shell
git fetch # 把远端的仓库复制到本地的 repository
git clone <local/path.git> # 只要clone本地项目的git文件, 就能复制项目
git clone <url>  # 复制远端仓库, 其实也是.git 的文件

```

### fork
fork将 github 上别人的仓库, 复制到我的空间中, 且保留之前仓库的所有commit

文件状态分为
* untracked 没有git观察的
* modified 被修改的

```shell
# 查看仓库文件状态
git status
```

### 代码提交
```shell
git commit -m 'initial commit'
```

###  查看提交历史
```shell
git log -1 # 最近的一次提交
git log --patch -1 # 第一次的提交修改的具体细节
git log --oneline # 一行显示
```

### git diff

```shell
git diff origin master # 对比远端和本地
```

# 分支
本地默认有一个分支, 就是 master, 有些情况下, 会改为 main
```shell
git branch  # list all branches
# create new branch and switch to it
git checkout -b 

# delete branch
git branch -d <branch_name>
```


## checkout

```shell
# show all commit history line by line 
git log --oneline
# check out commit with tag 1a 
git checkout 1a 
# check out branch master 
git checkout master
```


### 区分git add -u, git add -A, git add folder
```shell
git add . # 把当前目录下的文件加入git
git add file01, file02 # 指定几个文件
git add -A # 把working tree里的文件都加入staging area, 这是默认操作
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

```shell
# 先切换到主要的 branch 上
git checkout master
git merge <branch_name> 
```


# gitignore
`.gitignore`只对于 untrack 状态的文件生效. 如果是已经添加进 staging area 的文件. 需要使用下面命令从 staging area 删除, 变成 untrack 状态
```shell
git rm --cache file
```

`.gitignore` 文件样例
```gitignore
fodername 
filename 
*.pyc # all file with pyc extension
```

查看 ignore 文件的列表

```shell
# 查看当前的 .gitignore文件中的规则可以过滤哪些文件? windows 不适用 
git check-ignore
```

# Stash
Stash 默认情况下, 不会关注 untrack 的文件
stash并不依赖于分支, 所以本质上, 你可以将当前分支的修改 stash 后, 切换另一个分支呈现

User case

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
