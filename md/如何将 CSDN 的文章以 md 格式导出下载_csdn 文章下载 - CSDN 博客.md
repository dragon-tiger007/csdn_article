> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_56326415/article/details/132651235?ops_request_misc=&request_id=&biz_id=102&utm_term=csdn%E6%80%8E%E4%B9%88%E4%B8%8B%E8%BD%BD%E6%96%87%E7%AB%A0md%E6%A0%BC%E5%BC%8F&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-132651235.142^v100^control&spm=1018.2226.3001.4187)

1. 准备工作
-------

##### 1.1 下载插件

在浏览器中下载插件简悦

##### ![](https://i-blog.csdnimg.cn/blog_migrate/9204e1e1c373dbbb8d29f1c012ed02e1.png)1.2 准备一个 [github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020) 仓库

###### 1.2.1 为什么需要一个 github 仓库？

虽然这个插件自带了一个以 md 格式导出，我试了一下，typora 根本打不开他下载的文件（**可能是我不会用）**。所以我们先保存到 github 上，在从 github 下载到本地。

###### 1.2.2 创建仓库

打开 github，点击右上角加号，然后点击 New repository

![](https://i-blog.csdnimg.cn/blog_migrate/e47e04986fbd091310c43e9c5e6578d5.png)

给仓库起一个名字**（记住这个名字一会有用）**之后，选择创建一个 README file 文件，然后点击创建即可

![](https://i-blog.csdnimg.cn/blog_migrate/58b9a3904ccab78c2924ab9b04e3c300.png)

##### 1.3 创建 github 的 token

打开网址 [Sign in to GitHub · GitHub](https://github.com/settings/tokens/new "Sign in to GitHub · GitHub")

**Note**：随便起一个

勾选 **repo**

然后点击最下面的 **Generate token**，

![](https://i-blog.csdnimg.cn/blog_migrate/5c9fccbe2aa33719d379ab64db606515.png)

保存生成的 token 信息

![](https://i-blog.csdnimg.cn/blog_migrate/4d65e35d0c19fdc6b9737eb45e9020b2.png)

2. 保存文章为 md 格式到 github
----------------------

##### 2.1 打开文章

打开我们想要保存的文章

##### 2.2 打开插件

点击红色的插件图标

![](https://i-blog.csdnimg.cn/blog_migrate/920d863eb67db9dec5d8acaf20bca299.png)

##### 2.3 给插件进行 github 授权

依次点击，点击 **保存到 GitHub** 后会出现一个授权窗口

![](https://i-blog.csdnimg.cn/blog_migrate/41b8a2d4896cb67e2fb1dee878833572.png)

##### 2.4  绑定 github 信息

 **第一个框**中输入我们生成的 **github token**

      **第二个框中**输入  **你的 github 用户名 / 仓库名称。**你也可以直接打开刚创建的 github 仓库，然后复制地址栏的 github.com/ 的后面的内容

      **第三个框**输入 **md**

 点击验证并绑定 Github 的信息

![](https://i-blog.csdnimg.cn/blog_migrate/22beb985a96f06b90e5b5eb9eff70c2e.png)

##### 2.5 文章保存到仓库

重新做 2.3 步就可以成功保存到 github 仓库中了。

3. 将仓库中的 md 文章下载到本地
-------------------

##### 3.1 打开仓库

我们所有文章都保存在了 [md 文件](https://so.csdn.net/so/search?q=md%E6%96%87%E4%BB%B6&spm=1001.2101.3001.7020)夹中

![](https://i-blog.csdnimg.cn/blog_migrate/1e6229f099f7962c7a79b960469859fd.png)

##### 3.2 不 clone 仓库，直接下载压缩包

![](https://i-blog.csdnimg.cn/blog_migrate/b8429427d0c38f1088c3c926afdee0e5.png)

##### 3.3 使用 clone 仓库的方式

**clone 需要下载 git，如果没有请先安装**

1. 复制这个链接

![](https://i-blog.csdnimg.cn/blog_migrate/413568f7db6394076996fb7674873a46.png)

2. 打开 git bash 输入

> git clone 复制的网址

![](https://i-blog.csdnimg.cn/blog_migrate/1b0535bdeb87e9c1efd4bfcd798baca1.png)

4. 查看文章
-------

可以看到图片、文章格式都非常好的保存了下来！

![](https://i-blog.csdnimg.cn/blog_migrate/1815c0ccc028c0ed3b6060ad06b0b794.png)

参考博客：[将 CSDN 或一般博客导出为 markdown 文件的通用方法_csdn 转 markdown_瑞哥 - RealWang 的博客 - CSDN 博客](https://blog.csdn.net/wangrui1573/article/details/124662720 "将CSDN或一般博客导出为markdown文件的通用方法_csdn转markdown_瑞哥-RealWang的博客-CSDN博客")