
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
git diff HEAD..origin/main # 对比本地最细的commit和remote最新的commit 
git merge FETCH_HEAD #将拉取下来的最新内容合并到当前所在的分支中

```

### Fork
作用是复制远端的开源仓库到自己的远端仓库中. 这样你就可以在自己的仓库中进行修改和创建新的的分支. 一般通过点击 fork 按钮来实现


# pull request
如果你想要把自己的修改, 提交到最原始的代码库, 使用自己仓库页面上的 pull request
原始 repo -> fork 后放我自己的 repo -> clone 到本地我的仓库 -> commit / push 到我自己的 repo -> pull request 合并到原始的 repo

```shell
git request-pull
```
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


# Local Repo

## branch
By default, you have only one branch **master**, which you can renamed as **main**
```shell
# list all local branches
git branch 
# list all remote branches
git branch -r 
# create new branch locally and switch to it
git checkout -b <branch_name>
# download the remote branch which is not in local
git checkout -b <branch_name> origin/<branch_name>
# switch branch
git checkout <branch_name>
# delete branch
git branch -d <branch_name>

# 如果我现在在分支 shipping_calculator_fixes 上
# 我同样把本地的 branch 推到 origin 上
# 在之后的 pull 和 push 时, 在这个 branch 上就只要用 push 和 pull 了, 不用指明 branch
git push --set-upstream origin shipping_calculator_fixes

```
你会遇到本地repo分支和remote分支不同的情况, 即本地只有一个分支, remote有多个分支

### revert
如果你对最近的一次 commit 不满意, 用 git revert 可以撤销最近 commit 的改动, 并把撤销作为一个新的 commit 提交. 不是很推荐, 会造成 commit 记录太多
```shell
# HEAD 是当前 branch 中最近的一次 commit
# --no-edit 表示
git revert HEAD --no-edit 
git checkout main
git merge <branch_b>
git log --oneline # 可以看到在 branch_b 上提交的两次 commit
```

```shell
git diff <modified_file>
```
# Roles

- developer
	- git clone, git pull, git push, and git request-pull
- integrator
	- Reviews and integrate changes
	- Use git pull, git revert, and git push
- repository administrator
	- Structure repository organization and interaction
	- Configure server
	- Define email and index setting
	- Manage the look and feel of the application

# ssh

```shell
ssh-keygen -t rsa -b 4096 -C "<your email address>"
cd ~/.ssh
ls
# 此时在 id_rsa 私钥, id_rsa.pub 公钥产生
# 将公钥复制到 github 的 setting 中
# 在本地目录下, 将文件上传到仓库
git remote add origin git@github.com:tigerfanxiao/profiles-rest-api.git
git branch -M main
git push -u origin main


# 打开 ssh-agent
eval "$(ssh-agent -s)"
# 把私钥加入 ssh-agent
ssh-add ~/.ssh/id_rsa
# 复制公钥, 复制到github 页面上
cat ~/.ssh/id_rsa.pub | clip
```