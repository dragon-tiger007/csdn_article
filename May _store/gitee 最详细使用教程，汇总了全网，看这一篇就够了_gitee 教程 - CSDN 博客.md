> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/maxle/article/details/124867297)

**目录**

[1、gitee 是什么？](#t0)

[2、git 网站上的注册登录](#t1)

[3、准备工作](#t2)

[4、上传文件到 gitee](#t3)

[5、下载自己的仓库和别人的](#t4)

[*6、基本命令汇总：](#t5)

### 1、gitee 是什么？

基于 git 的代码托管协助平台

### 2、git 网站上的注册登录

打开 gitee 官网 [Gitee - 基于 Git 的代码托管和研发协作平台](https://gitee.com/ "Gitee - 基于 Git 的代码托管和研发协作平台")打开注册登录即可。邮箱注册最好，非邮箱在个人 - 设置里添加自己的邮箱。

新手请**公开自己的邮箱**，如图：

![](https://i-blog.csdnimg.cn/blog_migrate/f6cc370836fcfbf616873f61bb822498.png)

![](https://i-blog.csdnimg.cn/blog_migrate/6cc2a62bf853bd3b2d121480f1ba9b95.png)

### 3、准备工作

1、工具一：git-bit 的安装，[Git![](https://i-blog.csdnimg.cn/blog_migrate/a40ae67961626623519cb1442f42c371.png)https://git-scm.com/](https://git-scm.com/ "Git") [安装教程](https://so.csdn.net/so/search?q=%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B&spm=1001.2101.3001.7020)看这个。

2、工具二：[TortoiseGit](https://so.csdn.net/so/search?q=TortoiseGit&spm=1001.2101.3001.7020).msi 小乌龟（可选软件）

    这个软件是为了图形化的方式。安装有先后顺序。

3、配置 RSA 公钥

1）打开 git bash，在哪里鼠标右键都行，因为目前还在配置。

![](https://i-blog.csdnimg.cn/blog_migrate/3090b2aa23d8d4dca588b897a95f984c.png)

 2）输入代码来实现 git 账户和本地的关联。

```
ssh-keygen -t rsa -C "你的邮箱"

```

一直回车，一共三次，虽然出现了冒号，但是不用填。 

3）结束后输入来查看自己的密钥：

```
cat ~/.ssh/id_rsa.pub

```

![](https://i-blog.csdnimg.cn/blog_migrate/35eb37580793003e8b1dcfeb0f6cfde7.png)

4） 将下面的密钥全部复制到网站上去：

在官网 --- 个人 --- 设置 ---ssh 公钥 --- 下面的公钥文本域（大的输入框）复制进去 --- 上面的标题是随意改的，给自己看的 --- 确定。

5）测试是否连接到远程自己的账号。

```
 ssh -T git@gitee.com


```

6）创建远程仓库

打开官网，新建仓库。

创建成功跳转过后，点击克隆下载，然后复制 ssh 的地址来进行上传下载（后面会用到地址）

### 4、上传文件到 gitee

1）新建文件夹

2）进入刚刚新建的文件夹，即进入 “gitspace”，点击鼠标右键，选择 "Git Bash Here", 如下图：

![](https://i-blog.csdnimg.cn/blog_migrate/7ef246a6dd28663f4bca68ace28300b4.png)

3）进行基础配置，也叫全局设置，作为 git 的基础配置，作用是告诉 git 你是谁，你输入的信息将出现在你创建的提交中，使用下面两条命令：

```
  git config --global user.name "你的名字或昵称"
  git config --global user.email "你的邮箱"
```

看下我运行的界面：

![](https://i-blog.csdnimg.cn/blog_migrate/ca02145dccf64ec084eb3cd076e392b4.png) 

中间的过程： 

![](https://i-blog.csdnimg.cn/blog_migrate/80900e7268fee3174d8dc32828161ae0.png) 

粘贴后会出现直接运行，使用上键修改代码，没出现不用管，双引号可以不加。

名字网站首页。邮箱是刚才设置的。

![](https://i-blog.csdnimg.cn/blog_migrate/38acb321958f2a08bb216c67b31cf99e.png)

 4）输入初始化命令 git init  回车，文件夹出现了隐藏文件夹。这步是将本地文件初始化为本地仓库。

5）输入要链接到码云的地址，（前面我们复制的地址）  回车。

```
git remote add origin 地址

```

6）在这个新建文件夹里随便放个文件。

插入一下，输入可以查看这个文件夹的文件装填

```
git status 命令用于查看在你上次提交之后是否有对文件进行再次修改

```

7）输入命令：

```
git add .

```

8）添加注释，来说明自己为什么要上传，方便以后自己查阅 例如：

```
 git commit -m "第一次上传" 

```

9）提交到码云上面，git push origin master

因为是第一次提交，要更改为：

```
git push -u origin master

```

  第二次提交就按照上面的写法即可，不在需要加  -u 。

注意：如果最后一步报错，可以使用 `git push -f origin master`，来强制覆盖。

```
 
   git push origin master //（正常提交）和
   git push origin master -f //（强制提交，强制提交可能会把之前的commit注释信息，不会改变修改的代码，慎用），都是提交到master分支
```

执行结果如图： 

![](https://i-blog.csdnimg.cn/blog_migrate/5d7406fb6dadc1431ba46b098fcf1c5b.png)  

刷新 gitee 网站就有了。-f  的位置似乎不影响，眼尖的发现了，很好。还有的在这一步需要输入

![](https://i-blog.csdnimg.cn/blog_migrate/8956e1bec1933914e45d1219a15a515b.png)

输入一下网站的用户名（邮箱）和密码就行。

### 5、下载自己的仓库和别人的

      新建个文件夹方便看，进入到这个文件夹，鼠标右键 - 打开 git bash 命令窗口 -- 复制网站上的 ssh 链接 - 在刚才的 Git 窗口中输入命令 git clone 然后右键即可。

```
git clone url

```

### *6、基本命令汇总：

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。他有四个位置：

*   workspace：工作区
*   staging area：暂存区 / 缓存区
*   local repository：版本库或本地仓库
*   remote repository：远程仓库

<table><tbody><tr><td><code><a href="https://www.runoob.com/git/git-init.html" rel="nofollow" title="git init">git init</a></code></td><td>初始化仓库</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-clone.html" rel="nofollow" title="git clone">git clone</a></code></td><td>拷贝一份远程仓库，也就是下载一个项目。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-add.html" rel="nofollow" title="git add">git add</a></code></td><td>添加文件到暂存区</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-status.html" rel="nofollow" title="git status">git status</a></code></td><td>查看仓库当前的状态，显示有变更的文件。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-diff.html" rel="nofollow" title="git diff">git diff</a></code></td><td>比较文件的不同，即暂存区和工作区的差异。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-commit.html" rel="nofollow" title="git commit">git commit</a></code></td><td>提交暂存区到本地仓库。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-reset.html" rel="nofollow" title="git reset">git reset</a></code></td><td>回退版本。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-rm.html" rel="nofollow" title="git rm">git rm</a></code></td><td>删除工作区文件。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-mv.html" rel="nofollow" title="git mv">git mv</a></code></td><td>移动或重命名工作区文件。</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-commit-history.html#git-log" rel="nofollow" title="git log">git log</a></code></td><td>查看历史提交记录</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-commit-history.html#git-blame" rel="nofollow" title="git blame <file>">git blame &lt;file&gt;</a></code></td><td>以列表形式查看指定文件的历史修改记录</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-remote.html" rel="nofollow" title="git remote">git remote</a></code></td><td>远程仓库操作</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-fetch.html" rel="nofollow" title="git fetch">git fetch</a></code></td><td>从远程获取代码库</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-pull.html" rel="nofollow" title="git pull">git pull</a></code></td><td>下载远程代码并合并</td></tr><tr><td><code><a href="https://www.runoob.com/git/git-push.html" rel="nofollow" title="git push">git push</a></code></td><td>上传远程代码并合并</td></tr></tbody></table>

其他常见 git 命令

查看所有分支 ：git branch -a

切换到某一分支：git checkout 分支名称

合并分支：git merge 原分支 目标分支

4. 更新代码到本地  
git status（查看本地分支文件信息，确保更新时不产生冲突）

git checkout -- [file name] （若文件有修改，可以还原到最初状态; 若文件需要更新到服务器上，应该先 merge 到服务器，再更新到本地）

git branch（查看当前分支情况）

git checkout remote branch

git pull

若命令执行成功，则更新代码成功！

可以直接使用： git pull 命令一步更新代码

> 创建于 2021 年 12 月 19 日的草稿，更新于 2022 年 5 月 19 日