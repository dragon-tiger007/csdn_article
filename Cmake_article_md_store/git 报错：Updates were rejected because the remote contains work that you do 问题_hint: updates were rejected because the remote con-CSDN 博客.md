> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/tjh1998/article/details/127325330)

刚开始学习使用 git，通过 pu[sh 命令](https://so.csdn.net/so/search?q=sh%E5%91%BD%E4%BB%A4&spm=1001.2101.3001.7020)：打算将本地仓库中的文件上传到远端仓库时，报了以下错误：

```
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., ‘git pull …’) before pushing again.
hint: See the ‘Note about fast-forwards’ in ‘git push --help’ for details.

```

原因：远程仓库不为空：自己在创建远程仓库的时候，添加了如下两个文件，本地仓库中并没有这两个文件。  
![](https://i-blog.csdnimg.cn/blog_migrate/e4dd406b8d77d50fa3641cd4fbc20038.png)  
解决方法：添加:–allow-unrelated-histories

```
 git pull origin master --allow-unrelated-histories

```

![](https://i-blog.csdnimg.cn/blog_migrate/e8068570927bdb1949d0c1ff219b4f27.png)  
将远端的本地文件首先拉取到本地来，因为 commit 的时间不一致，所以直接拉去的时候就会出现问题，必须添加忽略不相干的历史选项，这样本地文件中也出现了 read.md 和 readen.md 两个文件了。  
之后再正常推送即可。

```
git push origin master

```

引用：

> 首先它的出现是因为在你上传的时候，远程仓库中有着本地仓库没有的文件，及导致本地仓库和远程有不同的开始点，也就是两个仓库没有共同的 commit 出现的无法提交。

以后还是先创建远端仓库，然后 clone 到本地吧，不会出现什么问题；或者远端仓库默认不要添加初始化文件，设置为空即可。  
本质上是将分支进行了合并。  
![](https://i-blog.csdnimg.cn/blog_migrate/b079f27e2cd253140ee738b1a02ea202.png)