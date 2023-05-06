
# github
github 是一种远程代码托管平台. 

### clone
将远端的代码下载到本地
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
```shell
# 如果远端没有这个分支,则会创建. 如果有,则添加一个新的 commit
git push -u origin <local branch> 
```


