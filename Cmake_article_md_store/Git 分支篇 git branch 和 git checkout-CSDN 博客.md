> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/xwh3165037789/article/details/130244379)

### 分支作用

在开发过程中，项目往往由多人协同开发，那么将多人编写的代码汇总到一起就成了一个困难且复杂的工作，另外项目也需要备份和版本迭代，因此不能只有一个版本。因此分支就成为了优秀的解决方案。

分支相互独立，不同部门在不同分支开发，分支由主分支构建，分支代码独立运行且无误后融合到主分支，保证主分支都是稳定可部署的代码。分支的存在极大的提高了开发效率。

分支一般分为主分支和其他分支，不同程序员在分支上编写代码，无误后融合到主分支（一般为 master 分支）是各个程序员的代码都融合到主分支上。

分支也可以进行版本迭代，开发的第一版融合到 master1 主分支上，作为第一版，二次开发时融合到 master2 上作为第二个版本，代码也不会丢失。

### Git 分支管理

初始化仓库  
分支依赖于仓库，因此分支需要在 git 仓库构建。[git 命令](https://so.csdn.net/so/search?q=git%E5%91%BD%E4%BB%A4&spm=1001.2101.3001.7020)支持使用`git init`初始化一个空的 git 仓库。

```
git init

```

![](https://i-blog.csdnimg.cn/blog_migrate/7b13514a9839d1fd69e06703936c5f5c.png)

git branch

`git branch`命令是分支管理命令， 有如下功能：

*   查看分支
*   创建分支
*   删除分支
*   重命名分支
*   设置上游分支
*   将分支推送到远程仓库

```
# 查看本地分支
git branch

```

**默认只列出本地分支，不显示远程分支，并且在当前分支前面使用 * 标记**

![](https://i-blog.csdnimg.cn/blog_migrate/089131933f43d5b2dc308e718b60f1a6.png)

刚初始化的仓库是没有分支的，如果复制项目到当前目录，并绑定远程仓库，最后执行`git push origin master`命令就会自动在本地和远程仓库生成 master 分支。（推荐）

当然也可以直接在本地创建分支，git 提供了`git branch [branchName]`来兴建一个分支。

![](https://i-blog.csdnimg.cn/blog_migrate/47ff71df78b8682ea82b4f460828e3da.png)  
出现上面错误的原因是 "库是空的，无法创建主分支"，需要添加内容并提交到工作区，也就是执行如下命令：

```
git add .

git commit -m "xxx"

```

执行命令后 git 仓库就有内容了，并且 git 系统自动以此内容创建一个`master`主分支

![](https://i-blog.csdnimg.cn/blog_migrate/8ebf584e4230693c3d65d8ae5dcf2aab.png)

```
# 查看远程分支
git branch -r

git branch --remotes

```

![](https://i-blog.csdnimg.cn/blog_migrate/bd49c436691b5d4caf39f4c429d85aee.png)

```
# 查看所有分支包含远程和本地
git branch -a

git branch --all

```

![](https://i-blog.csdnimg.cn/blog_migrate/7b0ba9d9c83e44d4825cf4d2d8ed744e.png)

```
# 查看分支提交的详细信息
git branch -v

git branch --verbose

```

![](https://i-blog.csdnimg.cn/blog_migrate/25788cf68df0a66d57ff329840d9ad98.png)

> 在查看远程仓库的分支时注意绑定远程分支仓库。

```
# 创建本地分支
git branch help

```

![](https://i-blog.csdnimg.cn/blog_migrate/671c1794fc03c5b8845e4c0572dadc85.png)

```
#git checkout -b 创建并切换到新的分支
git checkout -b <branch>

```

```
# 切换到指定分支
git checkout <branch>

```

![](https://i-blog.csdnimg.cn/blog_migrate/9282880e7a5b99a610602462b0648c33.png)

> `git checkout -b <branch>`=`git branch <branch>`+`git checkout <branch>`

```
# 将本地分支推送到远程仓库（创建远程仓库分支）
git push origin <local_branch>:<remote_branch>

# 简写
git push -u origin <local_branch>

```

```
# 删除一个名字为branchName的分支,如果该分支有提交未进行合并，则会删除失败。
git branch -d <branchName>

# 强制删除一个名字为branchName 的分支
git branch -D <branchName>


# 删除远程分支
git push origin -d <branch>

git push origin :<branch>

```

```
# 重命名当前分支
git branch -m <branch>

# 重命名指定分支
git branch -m <old-branch> <new-branch>

```

git checkout

`git checkout`[切换分支](https://so.csdn.net/so/search?q=%E5%88%87%E6%8D%A2%E5%88%86%E6%94%AF&spm=1001.2101.3001.7020)和创建分支的命令。git checkout 命令可以切换通过 git branch 命令创建的分支。每个分支都是一个独立的项目空间。

checkout 一个分支，会更新当前的工作空间中的文件，使其与检出分支的 commit 版本状况保持一致。这之后工作区中的所有变更都会被记录在 checkout 出来的那个分支上。这一操作可以认为是在挑选你希望修改的工作分支。

git checkout 命令有时候会跟 git clone 命令相混淆。两个命令中最为显著的差别在于，git clone 用于从远程仓库获取代码，而 git checkout 则用来在本地系统中业已存在的代码库中切换不同的版本。

```
# 切换本地分支
git checkout <branch>

# 切换远程分支
git checkout -t <origin/xxx>

```

![](https://i-blog.csdnimg.cn/blog_migrate/e1020710b643ffd9903d8359ca398a96.png)

在不同分支下改变目录下的文件，提交到工作区的内容时不一样的。也就是说当切换分支后，就是一个独立的空间，这个空间工作区的内容由`git add`和`git commit`决定，最后`git push`推送该分支的代码。

分支项目拉取

在 git 管理的项目中提供了两种方法拉取远程项目`git clone`和`git pull`两个命令。在分支中`git clone`和`git pull`是不一样的，前者是面向公开项目的，g 后者就是用户本地和远程仓库传输的。

`git clone`是作用于主分支，将主分支克隆到本地，这个过程无需密码验证，任何开发者都可以将远程仓库的主分支地址拉取到本地（只作用于主分支）。

`git pull`可以在任意分支上从远程的任何分支拉取项目，此过程需要密码验证。在管理本地分支项目与远程分支项目时都是使用该命令。

远程分支也是可以直接拉取到本地的，通过`git fetch`命令。

```
# 在本地新建一个xiaoxu分支，并将远程origin仓库的master分支代码下载到本地xiaoxu分支
git fetch origin master:xiaoxu

```

```
# 取回origin主机的master分支
git fetch origin master

```

```
# 将某个远程主机的更新，全部取回本地
git fetch <远程主机名>

```

```
# 取回特定分支的更新
git fetch <远程主机名> <分支名>

```