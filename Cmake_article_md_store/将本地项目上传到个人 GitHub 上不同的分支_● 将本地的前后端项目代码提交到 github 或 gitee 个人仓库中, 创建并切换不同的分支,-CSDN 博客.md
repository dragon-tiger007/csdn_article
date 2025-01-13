> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/u012606565/article/details/83902767)

### 一、前期准备工作

安装 git，具体的[下载地址](https://git-scm.com/downloads)，安装比较简单在此不再详述喽，本次简介是 windows 系统下的步骤，linux 系统下命令类似。

### 二、上传到默认 master branch

0、 登录自己的 [GitHub](https://so.csdn.net/so/search?q=GitHub&spm=1001.2101.3001.7020) 账号，创建一个空的 repository，最好不要添加`README.md`  
![](https://i-blog.csdnimg.cn/blog_migrate/ebf260f49e3ea45b0f844bf1bad546a3.png)

1、进入需要上传的项目文件夹中，选中该文件夹，点击鼠标右键，选择`git bash`，linux 系统下直接打开终端即可  
![](https://i-blog.csdnimg.cn/blog_migrate/938cc3170b31bf5235dd00398ebcfb70.png)  
创建完成之后也会提示怎么将代码加进去，如下图  
![](https://i-blog.csdnimg.cn/blog_migrate/3d54d368f6a93d2f505b792a922b7fba.png)

2、 进入 git 命令窗口，分别输入以下命令（本命令直接将项目添加到`master branch`）

```
 $ git init     //用于创建git仓库
 $ git add .    //表示将该目录下的所有文件都添加到仓库，linux下的命令是:  git add ./
 $ git commit -m "first commit master"    //双引号的内容随意，算是提示信息
 $ git remote add origin https://github.com/xxxxxx/x'x'x'x.git   //最后的地址请自己将第0步创建的repository的地址填入即可，将本地的仓库关联到github上
 $ git pull origin master --allow-unrelated-histories      // 取回远程主机某个分支的更新，再与本地的指定分支合并
 $ git push -u origin master     //上传代码到github远程仓库,此处会需要输入github的用户名和密码，自己根据提示填写上就可以

```

3 、 打开自己的 github 主页，刷新刚刚创建的 repository，你会发现你将自己本地项目已经上传到 github 上了

### 三、将项目上传到不同的分支

1、首先在上一步的基础上，现在将项目添加到新的 branch，假如现在想要上传项目的 2.0 版本，上一步上传的 是 1.0 版本，进入上一步执行的项目目录下，会有`.git 、 .gitattributes、.gitignore`（后面这两个文件可能没有），将这些文件都复制到 2.0 版本的项目目录下即可  
![](https://i-blog.csdnimg.cn/blog_migrate/ce697a1cb66ac766d0360988e2477fd3.png)  
2、进入项目 2.0 版本目录下，执行以下代码

```
git branch   //查看当前branch情况的，此时会出现master
git branch test  //创建新的branch test
git checkout test  //进入branch test
git add ./     
git commit -m  "first commit test"
git remote add origin https://github.com/xxxxxx/x'x'x'x.git   //此处保证github的url与上一步的相同
git push --set-upstream origin kafkaAPI   //最后将项目上传到新的branch上

```

3、刷新 github 的网页，这时候会在 branch 出现 `master、 test` 两个，保证了项目的并行度

### 四、错误总结

1、执行语句`git pull origin master`出现如下错误提示

```
fatal: refusing to merge unrelated histories

```

解决：将命令修改成`git pull origin master --allow-unrelated-histories`即可。  
2、 执行语句`git push -u origin master`出现如下错误提示

```
To https://github.com/xxxx.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/xxxx.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

解决：由于刚开始创建空 repository 的时候带了 README.md 文件，所以出现错误的主要原因是 github 中的 README.md 文件不在本地代码目录中，需要将 github 创建的`README.md`文件复制到本地项目中再次执行命令即可成功。或者刚开始创建 repository 的时候不带`README.md`文件。  
3、执行语句`git remote add origin https://github.com/xxxxxx/x'x'x'x.git`, 出现如下提示  
`fatal: remote origin already exists.`  
出现的原因是：以前可能使用 git 提交过一次该项目，现在使用了新的名字，导致远程的存在，现有的没法提交  
解决：两种办法  
（1）执行如下代码

```
git remote rm origin   //将存在的远程删除
git remote add origin https://github.com/xxxxxx/x'x'x'x.git   //再次执行该命令行即可
git push -u origin master

```

（2）执行如下代码

```
git remote add newname https://github.com/xxxxxx/x'x'x'x.git   //修改成新的名字
git push newname master  //再次提交

```