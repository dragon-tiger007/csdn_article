> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/feiyangqingyun/article/details/112986837)

### 一、前言

从 Qt5.14 开始，官方的在线安装提供了 qt for webassembly 构建套件，这对很多小白来说绝对是个好消息，也绝对是个好东西，好消息是不用再去[交叉编译](https://so.csdn.net/so/search?q=%E4%BA%A4%E5%8F%89%E7%BC%96%E8%AF%91&spm=1001.2101.3001.7020)自己生成 qt for webassembly 的 qt 库，在线安装版本直接就给你安装好，很多小白就困在如何交叉编译 qt for webassembly 的 qt 库上了，环境简直是弄哭了，望而却步；好东西是你可以直接将你现有的 qt 程序直接编译成 wasm 文件然后直接网页运行，注意这里不是说 activex 的形式在 IE 中运行，而是直接各种支持 wasm 的浏览器上直接运行，比如谷歌浏览器、[火狐浏览器](https://so.csdn.net/so/search?q=%E7%81%AB%E7%8B%90%E6%B5%8F%E8%A7%88%E5%99%A8&spm=1001.2101.3001.7020)、edge 浏览器等，反正主流的浏览器都支持，是不是很牛逼，大致的原理就是借助 emsdk 中的 emscripten 编译器将 qt 的程序直接静态编译生成 wasm 文件，然后同时生成对应的 js 文件和 html 文件，js 文件负责加载 wasm 文件进行编译使用 canvs 绘制程序。理论上 c++ 程序执行效率要比 js 高，个人体验下来也是效率蛮高，最激动的就是一行代码不用修改，直接就可以编译成网页程序。

WebAssembly 介绍：

*   WebAssembly 是一种可以使用非 JavaScript 编程语言编写代码并且能在浏览器上运行的技术方案。
*   WebAssembly 有一套完整的语义，实际上 wasm 是体积小且加载快的二进制格式，其目标就是充分发挥硬件能力以达到原生执行效率。
*   WebAssembly 运行在一个沙箱化的执行环境中，甚至可以在现有的 JavaScript 虚拟机中实现。在 web 环境中，WebAssembly 将会严格遵守同源策略以及浏览器安全策略。
*   WebAssembly 设计了一个非常规整的文本格式用来、调试、测试、实验、优化、学习、教学或者编写程序。可以以这种文本格式在 web 页面上查看 wasm 模块的源码。
*   WebAssembly 在 web 中被设计成无版本、特性可测试、向后兼容的。WebAssembly 可以被 JavaScript 调用，进入 JavaScript 上下文，也可以像 WebAPI 一样调用浏览器的功能。当然，WebAssembly 不仅可以运行在浏览器上，也可以运行在非 web 环境下。

1.  qt+widget 编译的程序网页地址：  
    [https://feiyangqingyun.gitee.io/qwidgetdemo/](https://feiyangqingyun.gitee.io/qwidgetdemo/)
2.  qt+quick 编译的程序网页地址：  
    [https://feiyangqingyun.gitee.io/qwidgetdemo/gallery.html](https://feiyangqingyun.gitee.io/qwidgetdemo/gallery.html)
3.  WebAssembly 中文网  
    [https://www.wasm.com.cn/](https://www.wasm.com.cn/)
4.  qt for webassembly 官网介绍  
    [https://doc.qt.io/qt-5/wasm.html](https://doc.qt.io/qt-5/wasm.html)

**2022-02-20 补充：**  
新版的 qtcreator 只要指定下 emsdk 的目录即可，更简单。

1.  下载好 https://github.com/emscripten-core/emsdk
2.  解压出目录 emsdk-main，进入 cmd 执行  
    emsdk install 1.39.8 或者 2.0.14 或者 latest  
    emsdk activate 1.39.8  
    emsdk activate --embedded 1.39.8 或者对应版本  
    执行 em++ --version 查看版本。
3.  qtcreator 设备 webassembly 选择 emsdk-main 目录，自动识别。如果有多个 Qt 版本不同需要，动态切换这个目录即可。比如 Qt5.15 对应选择 1.39.8，Qt6.2 选择 2.0.14。
4.  切换到构建套件看下是否正常。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/c8ff0970027de9631f9f769b973facbf.jpeg#pic_center)

**公众号：Qt 实战，各种开源作品、经验整理、项目实战技巧，专注 Qt/C++ 软件开发，视频监控、物联网、工业控制、嵌入式软件、国产化系统应用软件开发。**

**公众号：Qt 入门和进阶，专门介绍 Qt/C++ 相关知识点学习，帮助 Qt 开发者更好的深入学习 Qt。多位 Qt 元婴期大神，一步步带你从入门到进阶，走上财务自由之路。**

**官方店：[https://shop114595942.taobao.com//](https://shop114595942.taobao.com/)**

### 二、搭建步骤

#### （一）、安装 Qt 集成开发环境

从 Qt5.15 开始官方不再提供离线安装包，只提供源码包自行编译或者在线安装，在线安装的时候需要输入账号信息登录才能在线下载选择的 Qt 版本和构建套件及其他工具，慢慢的各位 Qt 开发者要习惯这种方式，要么自己熟悉编译流程自行编译，对应大部分初学者来说一个是没有这个必要还一个是太难了，建议放弃这种方式，所以从现在开始就慢慢的要习惯在线安装方式，官方提供了在线安装的程序，双击即可运行，相信 90% 的 Qt 开发者都使用过，这里直接略过，只需要在选择安装的构建套件的时候记得勾选 WebAssembly 构建套件就行，这样已经很方便了，之前都是需要自己编译呢。  
![](https://i-blog.csdnimg.cn/blog_migrate/35ef26733713300472c3c45204b1f590.png#pic_center)

安装好以后如果勾选了 mingw 版本的 Qt 构建套件，则可以自行测试 hello 跑起来，同时你也会发现 qt for webassembly 这个构建条件是不可用的，别急，那是因为现在你只安装了 qt for webassembly 的 qt 的库，而并没有找到需要的编译器 emscripten。  
![](https://i-blog.csdnimg.cn/blog_migrate/e235e61b093de3ec1574c0995cbbf3cf.png#pic_center)

#### （二）、安装 emsdk 编译器

任何编程语言开发环境，都离不开编译器，需要用对应的编译器将代码编译成对应的可执行文件，Qt 是一个兼容了 N 种编译器的通用代码库，你使用何种编译器则调用对应的 Qt 库然后再编译生成对应的程序，qt for webassembly 就需要借助 emsdk 中的编译器 emscripten 来编译，而不是使用 msvc、mingw、gcc 等，所以需要单独安装 emsdk 编译器。

##### 1、常规安装办法

*   前提：电脑安装有 git 环境，能从 github 下载项目，安装有 python 环境，比如 python3.7.4，如果不会玩 git 命令行请自行百度。
*   第一步：双击 python-3.7.4-amd64.exe，安装 python 开发环境，记得勾选添加环境变量。
*   第二步：获取源码，打开 git 命令行工具，输入 git clone https://github.com/emscripten-core/emsdk.git ，等待下载完成，一般 1-2 分钟就下载完成。
*   第三步：打开 cmd 工具，进入到 emsdk 目录，执行 emsdk install 1.39.7 安装 emsdk 编译器（Qt5.15 对应的是 1.39.7 版本，而不是 1.39.8，之前下载的 1.39.8 用下来每次编译都有警告提示版本不一致说是要 1.39.7）。这个下载需要点时间请耐心等待，我电脑大概 13 分钟，具体看网速。
*   第四步：安装完成后继续在当前的 cmd 命令行窗口执行 emsdk activate --embedded 1.39.7 激活 sdk。
*   第五步：激活成功以后，将 emsdk 目录下的. emscripten 文件复制到 C:\Users\Administrator 目录下（即用户目录），Qt for webass 构建套件编译的时候会去这里找编译器和各种编译需要的变量。
*   第六步：用记事本打开. emscripten，将 emsdk_path = os.path.dirname(os.environ.get(‘EM_CONFIG’)).replace(‘\’, ‘/’) 改成 emsdk 目录的绝对路径，比如 emsdk_path = ‘H:/github/emsdk’，如果运行有问题则全部改成绝对路径。
*   第七步：打开 QtCreator，配置 Qt for WebAssembly 构建套件，此时可以看到编译器中能够识别到所需的 em 编译器。
*   第八步：编译好以后如果弹出的是 IE 浏览器则复制地址拷贝到谷歌浏览器或者 edge 或者火狐浏览器运行，目前 IE 浏览器不支持 WebAssembly。
*   第九步：默认采用的是静态编译，意味着可以脱离 Qt 环境运行，.wasm 文件比较大因为静态集成了 Qt 的运行库。除了编译运行以外，还可以直接发布到有 ngix 或者 apche 环境的站点，直接可以运行。他就类似于 PHP 需要站点环境支持才能运行。

##### 2、小白懒人办法

常规的办法是万能的，包括选用其他版本的编译器等，但是大部分的初学者其实还没有 git 环境和 python 环境呢，怎破，此时非常想体验一把将 qt 程序编译到网页运行的想法超级强烈，马上安排懒人办法，注意此办法针对的是 Qt5.15.2 版本，本人特意将下载好的编译器整个文件夹中各种无关的文件全部删除。  
emsdk 地址：[https://pan.baidu.com/s/1ZxG-oyUKe286LPMPxOrO2A](https://pan.baidu.com/s/1ZxG-oyUKe286LPMPxOrO2A) 提取码：o05q 名称：emsdk.zip

*   第一步：将下载好的 emsdk 压缩包解压到目录，为了方便统一管理，我这里放在 C:/Qt。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/5755f01d8751bb9750f10410950d1a5c.png#pic_center)
    
*   第二步：将 emsdk 目录下的. emscripten 文件复制到 C:\Users\Administrator 目录下（即用户目录），Qt for webass 构建套件编译的时候会去这里找编译器和各种编译需要的变量。
    
*   第三步：默认. emscripten 文件中填写的是我这边安装的目录，你可以用记事本打开进行编辑改成你的目录。
    
*   第四步：重新打开 QtCreator，切换到工具 - 选项 - kits，重新设置 Qt5.15.2 webassemly 的编译器，下拉选择 Emscripten Compiler。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/a6f0c7e2fac038cf879212dff3061797.png#pic_center)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/f6b10cf131bccc52a8f41c08fc6cf713.png#pic_center)
    
*   第五步：新建个项目，拖几个控件放界面，编译大概一分钟左右，由于是静态编译时间会久一点，此时会生成五个文件，其中 qtloader.js 和 qtlogo.svg 每个项目是一样的，不同的文件是 untitled.js、untitled.html、untitled.wasm。需要发布的话只需要将这 5 个文件拷贝到网站的 WWW 目录下就行。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/df777a989bfbed1bb76b4f15a5c9db41.png#pic_center)
    
*   第六步：编译完成以后会自动打开电脑默认浏览器比如 IE 浏览器，因为 IE 浏览器不支持 wasm，所以你需要将地址复制拷贝到 edge 或者谷歌火狐等浏览器运行。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/c3dae974121ca1b7d2475730a8b562f6.png#pic_center)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/3704bb133f3134698fc4be9eb0122f9b.png#pic_center)
    
*   第七步：如果要支持中文则需要将中文字体文件添加到项目的资源文件一起编译。
    
*   其他说明：首次加载比较慢，后面由于有缓存的原因重新加载非常快，建议发布的时候可以放一个带宽很好的服务器。
    

#### （三）、支持的模块

目前 qt for webassembly 套件不是支持所有的模块，比如常见的 sql 数据库模块就不支持，估计现在 wasm 还是定位在客户端的原因吧，network 中的 tcp udp 也不支持，好消息是 websocket client 是支持的，也就意味着你可以写个 websocket 的 server 端负责监听和解析，web 端直接 websocket 通信交互，比如传输视频数据，这不就是网页中显示实时视频了！亲测无误。

*   Qt5Charts
*   Qt5Core
*   Qt5Gui
*   Qt5Quick
*   Qt5Svg
*   Qt5WebSockets
*   Qt5Widgets
*   Qt5QuickControls2
*   其他部分模块

### 三、效果图

![](https://i-blog.csdnimg.cn/blog_migrate/fae11ead2805993a5af92e7eec059125.gif#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/4cb0cc0ead48fd74b379e06e6d0bb594.gif#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/0214abded752d4e10a7a63763562a34b.gif#pic_center)