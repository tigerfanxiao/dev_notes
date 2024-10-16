

### clone
- 复制本地代码仓库
```shell
git clone <local/path.git> # 只要clone本地项目的git文件, 就能复制项目
```
- 将远端的代码下载到本地
```shell
git clone https://github.com/miguelgrinberg/flasky.git 

```


### pull

git pull 是面向远程仓库的命令. 作用是将 remote repo 中的代码拉下来并合并
git pull =  git fetch + git merge
* fetch只是拉取, 不会做合并, 在本地会出现一个 FETCH_HEAD 分支
* merge之后才会出现 conflict
```shell
git pull 

# 下面两个操作等价于 git pull
git fetch origin master #从远程主机的master分支拉取最新内容 
git merge FETCH_HEAD #将拉取下来的最新内容合并到当前所在的分支中

```

### Fork
作用是复制远端的开源仓库到自己的远端仓库中. 这样你就可以在自己的仓库中进行修改和创建新的的分支. 一般通过点击 fork 按钮来实现

### push
将本地的代码推送到远端的仓库
- origin 表示我自己的 fork
- upstream 表示原作者的仓库
```shell
# 如果远端没有这个分支,则会创建. 如果有,则添加一个新的 commit
git push -u origin <local branch> 
# 查看所有的 remote 仓库
git remote -v
git remote add <repo_name> <url>
git remote rm <repo_name> # 删除连接的仓库

```


# pull request

原始 repo -> fork 后放我自己的 repo -> clone 到本地我的仓库 -> commit / push 到我自己的 repo -> pull request 合并到原始的 repo

### .gitignore

在 git repo目录下创建 `.gitignore` 文件
```shell

# 忽略任何目录下的指定文件夹
**/__pycache__
```


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
