> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/iriczhao/article/details/126712968)

在 [QtCreator](https://so.csdn.net/so/search?q=QtCreator&spm=1001.2101.3001.7020) 中，对于 xxx.qml、xxx.ui.qml 文件都是可以通过『设计』打开的，只是 xxx.qml 很多时候是没有任何 2D 或者 3D 显示效果的。但是如果在新安装 QtCreator 的时候没有安装对应插件，xxx.qml、xxx.ui.qml 则无法打开。

解决方法：

依次打开：帮助 -> 关于插件。找到如下图所示的地方，勾选上：

![](https://i-blog.csdnimg.cn/blog_migrate/4717b0b90faf1a8e583255928884b4a9.png)

然后重新启动 QtCreator 即可。