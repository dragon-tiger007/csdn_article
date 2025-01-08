> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/libeiqi1201/article/details/117107099?ops_request_misc=%257B%2522request%255Fid%2522%253A%252290473c940ad96b956dbfce2b2a3741e7%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=90473c940ad96b956dbfce2b2a3741e7&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-117107099-null-null.142^v101^control&utm_term=Permission%20denied%20%28publickey%29&spm=1018.2226.3001.4187)

### Git 在克隆的时候报错、Permission denied (publickey).

#### 报错 Permission denied (publickey) 具体如下：

![](https://i-blog.csdnimg.cn/blog_migrate/3d7d0e36a0289f53278162d846110e1f.png)

#### 原因：没有将自己的电脑的 SSH key 添加到对应的 git 服务器上。

#### 解决：

1、 生成 SSH key

```
> ssh-keygen -t rsa -C “xxxxx@xxxxx.com” 
注意：输入的是自己的邮箱地址
```

![](https://i-blog.csdnimg.cn/blog_migrate/8fa5acbd2b479d424f0911ac9b2ad415.png)

2、 找到生成 Key 值的目录, 前往. ssh 目录、查看对应的公钥

```
> cat ~/.ssh/id_rsa.pub 
注意：内容是（以ssh-rsa 开头，以账号的注册邮箱结尾的）
```

![](https://i-blog.csdnimg.cn/blog_migrate/f2b1b4c65dcc8b87e7d29696f4d7deae.png)

3、 登录对应的 git 服务器、将公钥（id_rsa.pub 中的内容）添加上去

如果是 github

![](https://i-blog.csdnimg.cn/blog_migrate/5990e05469b9edb21fb377a8dce18c6e.png)

![](https://i-blog.csdnimg.cn/blog_migrate/f184b1388932433734ba57b8f2e6bf85.png)

如果是 [gitlab](https://so.csdn.net/so/search?q=gitlab&spm=1001.2101.3001.7020)

![](https://i-blog.csdnimg.cn/blog_migrate/02ec49279a84d2951ea0f884477659f3.png)

![](https://i-blog.csdnimg.cn/blog_migrate/90c58535dec621fedc0ccde069ae48ad.png)

![](https://i-blog.csdnimg.cn/blog_migrate/4fe3812b7791cfec22eb9e5527b9d59e.png)

如果是 gitee

![](https://i-blog.csdnimg.cn/blog_migrate/6f90bc181d827f6ebfabd6cf663fc058.png)

![](https://i-blog.csdnimg.cn/blog_migrate/bd25c53555a20f66b27ae3c988244e6b.png)

完成之后、输入以下命令、查看是否 OK、出现如下内容表示成功

```
ssh -T git@gitee.com

```

![](https://i-blog.csdnimg.cn/blog_migrate/96f84f81944e4340a94a58da9bc3b9ce.png)

再次进行克隆、如果出现如下、则表示克隆成功

![](https://i-blog.csdnimg.cn/blog_migrate/33f52ae6b384c07e69e8616c783ce2d8.png)

原文链接：[Git 报错：Permission denied (publickey) - 啥也不会的程序猿 - 博客园 (cnblogs.com)](https://www.cnblogs.com/laowenBlog/p/11167555.html)