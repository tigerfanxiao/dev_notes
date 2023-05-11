
# 概念


# 编辑代码


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


# 分支
本地默认有一个分支, 就是 master, 有些情况下, 会改为 main
```shell
git branch  # list all branches


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


# .gitignore
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



