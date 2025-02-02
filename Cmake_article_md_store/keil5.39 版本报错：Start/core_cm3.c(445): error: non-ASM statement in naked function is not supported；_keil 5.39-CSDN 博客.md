> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_45899177/article/details/135854420)

初学 STM32，跟着网上的教程开始新建工程，教程编译后 0 Error, 0 Warning， 到我手上 4 Errors，无数个 Warnings。看了一些大佬的文章说是编译器版本的问题，没错确实是编译器版本问题，但是在我的 keil5 上面却缺少需要的编译器版本，于是又接着查资料........

最后也算是解决了，在此记录分享一下

一、版本问题
------

Start/core_cm3.c(445): error: non-ASM statement in naked function is not supported；

针对以上报错信息，结论是编译器版本问题，我的 keil5 使用的是 V6.21 版本的编译器，当把编译器换成 V5 版本时即可正常编译程序

建工程的步骤这里省略哈 ^-^

1、打开 keil5 软件，首先点击 “魔术棒” 图标①，然后在新页面中点击 Target ②，可以看到 ARM Compiler ③ 的版本是 Version 6 ，我们需要更改编译器的版本

![](https://i-blog.csdnimg.cn/blog_migrate/f96e2d2af86ebab9dafee18d0056c8d2.png)

2、此时我天真的选择了 Missing：Compiler Version 5（当时也没多想，看见有个 5 就选上试一试），结果显然是不行的

![](https://i-blog.csdnimg.cn/blog_migrate/96f6a3eded26bf20422121d18bc5ec1b.png)

3、报错：如第二步的操作结果，编译出现如下报错

*** Target 'Target 1' uses ARM-Compiler 'Default Compiler Version 5' which is not available.

二、ARM 编译器 V5 版本下载安装
-------------------

解决该问题的办法就是重新下载 V5 版本的 ARM 编译器，链接如下：

[ARM 编译器 V5.06（提取码：4132）![](https://i-blog.csdnimg.cn/blog_migrate/003a2ce7eb50c2e24a8c624c260c5930.png)https://pan.baidu.com/s/1zRW7sf3_5KGRlyaUzqI1NA](https://pan.baidu.com/s/1zRW7sf3_5KGRlyaUzqI1NA "ARM编译器V5.06（提取码：4132）")1、下载完成后，双击它

![](https://i-blog.csdnimg.cn/blog_migrate/c61b2ccf70bae7f6631e8351479c6cd9.png)

2、点击 Next

![](https://i-blog.csdnimg.cn/blog_migrate/ef5acd6ac441daba7ebc52f672590088.png)

3、勾选 I accept ，再点击 Next

![](https://i-blog.csdnimg.cn/blog_migrate/fcec9d0238834ce7df2b29b30595ba22.png)

4、选择安装目录，默认是 C 盘，这里我安装在 Keil5 安装目录的 ARM->ARMCC 文件夹中（新建的 ARMCC），点击 Next

![](https://i-blog.csdnimg.cn/blog_migrate/0d020ded81a02baff42f89a4bd5fdce9.png)

5、再点击 Next

![](https://i-blog.csdnimg.cn/blog_migrate/9cece3a8fed52612cea1bd79ef421704.png)

6、点击 Install

![](https://i-blog.csdnimg.cn/blog_migrate/8da6f4a43e5ac14eeaa088813c45bb14.png)

7、等待安装完成，点击 Finish。此时去 ARMCC 文件夹查看，发现已经安装成功

![](https://i-blog.csdnimg.cn/blog_migrate/1edb7345bfce62db50566726bff9ee2b.png)

三、Keil5 配置
----------

1、再次进入 keil5 软件界面，点击下图①位置的三个方块图标，在弹出页面中点击 Folders/Extensions，再点击③位置的三个小点

![](https://i-blog.csdnimg.cn/blog_migrate/9c7c62479cc71bd2d2e0ef0dc002bd95.png)

2、点击 Add another ARM Compiler Version to List...

![](https://i-blog.csdnimg.cn/blog_migrate/9d384afd1a89e7b8e591fea248ceba92.png)

3、选择 V5 编译器的安装路径 ARMCC，点击确定

![](https://i-blog.csdnimg.cn/blog_migrate/142e70f1ca2a3eac8001f1133ccddbe5.png)

4、之后可以看到 ARMCC Path 中多了一条

![](https://i-blog.csdnimg.cn/blog_migrate/78b49fe3382dc7d1dd10cfc5e90f2122.png)

5、最后在点击 “魔术棒” 图标，进入 Target 页面，可以看到 ARM Compiler 的选择中多了 Version 5，选择 Version 5，点击 ok

![](https://i-blog.csdnimg.cn/blog_migrate/ecfc1f76b556896bb603c3a8efa32def.png)

6、再重新编译程序，可以看到 0 Error(s), 0 Warning(s).

至此，解决问题 ^-^

![](https://i-blog.csdnimg.cn/blog_migrate/b817dcd675ddac4bc74dd6a7db116b96.png)

以上内容是根据各位前辈大佬的经验总结出自己认为思路比较清晰的解决办法，希望在给自己留下记录的时候也能帮助到其他小伙伴 ^-^