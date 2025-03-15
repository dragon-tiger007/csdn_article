> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_34139994/article/details/135363950)

作者：billy  
版权声明：著作权归作者所有，商业转载请联系作者获得授权，非商业转载请注明出处

前言
--

今天来和大家聊一下 Qt Design Studio 这个软件。这个软件的主要功能是用来快速完成 [UI 界面](https://so.csdn.net/so/search?q=UI%20%E7%95%8C%E9%9D%A2&spm=1001.2101.3001.7020)，就和 widget 中的 designer 设计器一样，可以把控件直接拖到界面上，通过修改属性快速完成一个界面，通过预览非常直观的看到界面效果，效率可以说是非常高的。

[不同的](https://so.csdn.net/so/search?q=%E4%B8%8D%E5%90%8C%E7%9A%84&spm=1001.2101.3001.7020)是，Qt Design Studio 生成的是 .ui.qml 文件，看后缀我们就知道这是用在 Qt Quick 项目中的。相当于原来的 qt quick designer，目前 qt quick designer 已经移除了，独立出来变成了 Qt Design Studio 这个软件。博主和 Qt 的技术人员沟通了解，他们如此设计的初衷是想把界面设计和逻辑开发完全分割开来，通过 Qt Design Studio 生成界面这部分的工作完全可以交给美工来完成，工程师只需要完成逻辑处理绑定到相应的事件上即可。

很多小伙伴会说原来的 qt quick designer 不好用但是还会用，现在的 Qt Design Studio 很方便但是不会用，不知道如何和 Qt Quick 项目结合来使用，这里博主整了一下大致的使用流程，希望对大家有用。

版本选择
----

目前 Qt Design Studio 4.2 不支持 Qt5  
1）使用 Qt 6.x 版本的可以使用最新的 Qt Design Studio 4.2  
2）Qt 5.x 版本的可以使用 Qt Design Studio 4.1.1

我目前的环境是 Qt 5.15.2 + Qt Design Studio 4.1.1

操作流程
----

##### 1. 新建一个 Qt Quick 工程，取名叫 StudioUIToPro

![](https://i-blog.csdnimg.cn/blog_migrate/9a2864330b9052ee8daaa9472931b9f7.png)

##### 2. 删除 main.qml 文件

到根目录下我们可以看到有一个资源文件，资源文件中有一个 main.qml 文件，也就是运行软件时显示的 UI 界面，我们现在的目的是在项目中显示 Qt Design Studio 中生成的 UI 界面，所以不需要 main.qml 了，直接删除  
![](https://i-blog.csdnimg.cn/blog_migrate/4000eb786f22b6ad31a96a08a7a656ab.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/7e98956a2f230a46dcedeb0e29af6f33.png)

##### 3. 使用 Qt Design Studio 在 StudioUIToPro 工程下新建 StudioUI 工程

![](https://i-blog.csdnimg.cn/blog_migrate/790e83c3b04931c816f43d0e8650e631.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/338ef8572c2ed34ec0549cf08856e59c.png)

##### 4. 在 StudioUIToPro 中的资源文件中添加 StudioUI 生成的 qml 文件

StudioUI 生成的 qml 文件在 content 和 imports 两个文件夹下，content 下是 UI 界面文件，imports 下是自定义的模块，作为依赖也需要添加进来  
![](https://i-blog.csdnimg.cn/blog_migrate/453f7f41853963cea47032a8f1948901.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/652a29d001e3adb6da21bfd43ed7c4ad.png)

##### 5. 修改 main.cpp 中启动文件的路径

软件运行显示的 UI 界面是 App.qml，修改路径后运行会报错，提示没有找到模块 StudioUI  
![](https://i-blog.csdnimg.cn/blog_migrate/5ac88c6250aa36443dd3d927998b9b37.png)

模块的定义在 imports\StudioUI\qmldir 文件里，所以需要把模块路径添加到程序中  
![](https://i-blog.csdnimg.cn/blog_migrate/71a4c74d309a710ad9f4fac70b301b93.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/da1a7502786c728d855fd761a6ec06b8.png)

##### 6. 修复小问题

上述设置完成之后，已经可以正常运行了，点击运行显示和 Qt Design Studio 中一样的 UI 界面。这里还有两个小问题可以改善一下。  
1）打开 App.qml 和 Screen01.ui.qml 文件仍然会显示 StudioUI 模块无法识别![](https://i-blog.csdnimg.cn/blog_migrate/82a382508c4c1dfebd139303aa003809.png)  
因为 qml 文件中使用的模块默认只会在 Qt 的安装路径下去查找，自己添加新的 module 之后，还需要把新模块的路径添加到查找路径中，在 pro 中添加路径如下：  
`QML_IMPORT_PATH += StudioUI/imports`

2）有一个信号槽连接方式的错误，Qt Design Studio 中生成的信号槽连接方式用的是 Qt5 的方式，而 Qt 5.15 和 Qt 6 中连接方式略有差异，需要把槽函数改为 function 的形式  
![](https://i-blog.csdnimg.cn/blog_migrate/a2d1b7346ba914f342928ae11cbb4ccc.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/8c0e71337d36fbf155e7f40242ea857b.png)

##### 7. 测试

至此已完成所有修改，可以进行快乐的开发测试了，我们在 Qt Design Studio 中随意的拖放控件到界面上，如下所示  
![](https://i-blog.csdnimg.cn/blog_migrate/87723c1e4a03af24b585ec0c96de384f.png)

在 Qt Creator 中运行即可得到最新的 UI 界面，这样就可以和 widget 中的 designer 一样使用了，对于不熟悉 QML 语法的小伙伴也可以非常快速的完成一个界面  
![](https://i-blog.csdnimg.cn/blog_migrate/1f67146cc4f68651092b279dfd18d7da.png)