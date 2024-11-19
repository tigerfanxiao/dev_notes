

在本地生成多个私钥，参数是账户邮箱
```shell
ssh-keygen -t rsa -C "your_email@youremail.com"
```
生成的过程中会产生对话框，输入私钥的文件名。比如为github账户 `id_rsa_github`
完成后，会在`.ssh/` 生成了公钥和私钥两个文件。
公钥需要上传到远端的repo上， 私钥需要在本地做配置关联


将私钥添加到ssh中, 这是临时的， 关闭shell之后会消失
所以把这部分代码写道`.profile` 中
在 `~/`下创建 `.bashrc` `.profile`, `.bash_profile`
```shell
# .profile
eval $(ssh-agent)  # only start ssh agent, could you do some adding 
# Agent pid 1955 
ssh-add ~/.ssh/id_rsa_github
ssh-add ~/.ssh/id_rsa_huawei
# Check all the key you saved in ssh
ssh-add -l
```

如果遇到 `Could not open a connection to your authentication agent.` 问题, 则启动ssh-agent
在 `.ssh/`下创建`config`文件，输入一下内容，用于持久化账户对应的私钥

```
#github account
Host github.com-tigerfanxiao
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_github

#huawei account
Host codehub-g.huawei.com-xWX1087747
	HostName codehub-g.huawei.com
	User git
	IdentityFile ~/.ssh/id_rsa_huawei

```

此时，如果公钥已经配置在远端仓库后，复制远端仓库到本地当前文件夹
```shell
git clone git@github.com:tigerfanxiao/dev_notes.git .

# config local user info
git config user.name "tigerfanxiao"
git config user.email "tigerfanxiao@gmail.com" 
```
推送本地文件到远端
```shell
# 默认情况下, remote_repo 是 origin
git remote add origin git@github.com:tigerfanxiao/dev_notes.git
# 原始代码库作为 upstream
git remote add upstream git@github.com:tigerfanxiao/dev_notes.git

git remote -v # 结果如下
origin git@github.com:tigerfanxiao/dev_notes.git (fetch)
origin git@github.com:tigerfanxiao/dev_notes.git (push)
upstream git@github.com:tigerfanxiao/dev_notes.git (fetch)
upstream git@github.com:tigerfanxiao/dev_notes.git (push)

git fetch upstream # 从 upstream 拿到最新的变化
git merge upstream/master # 把 upstream 的变化合并到本地的 master 分支上
# 直接 fetch 并 merge
git pull upstream 
```

```shell
git branch -M main
git push -u <remote_repo> main
```


# 回退

```shell
# 本地完全删除当前的 commit, 回到上一个 commit 的状态. history 会改变
git reset --hard HEAD~1
```