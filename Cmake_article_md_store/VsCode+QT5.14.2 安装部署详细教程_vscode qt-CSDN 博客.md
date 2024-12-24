> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_44002418/article/details/131724795)

#### 文章目录

*   *   [一、下载](#_1)
    *   *   [1、下载 [QT](https://download.qt.io/archive/qt/5.14)](#1QThttpsdownloadqtioarchiveqt514_2)
        *   [2、下载 [VsCode](https://code.visualstudio.com/)](#2VsCodehttpscodevisualstudiocom_28)
        *   [3、下载 [Cmake](https://cmake.org/)](#3Cmakehttpscmakeorg_38)
    *   [二、配置环境变量](#_44)
    *   *   [1、打开环境变量设置](#1_46)
        *   [2、QT 环境变量设置](#2QT_48)
        *   [3、Cmak 环境变量设置](#3Cmak_54)
    *   [三、Vscode 配置](#Vscode_59)
    *   *   [1、安装插件](#1_61)
        *   [2、配置](#2_67)
    *   [四、使用](#_77)
    *   *   [1、新建项目](#1_78)
        *   [2、编译运行](#2_96)
        *   [3、问题](#3_109)

### 一、下载

#### 1、下载 [QT](https://download.qt.io/archive/qt/5.14)

**注意事项**：

1.  最好不要选择在线安装包，我安装了两天！！！快安装完成了结果是一个什么签名还是密码提示不识别。
2.  从 Qt 5.15 开始，开源离线安装程序不再可用。官网原文如下：  
      Due to The Qt Company offering changes, open source offline installers are not available any more since Qt 5.15. Read more about offering changes in the https://www.qt.io/blog/qt-offering-changes-2020 blog.  
    翻译：  
      由于 Qt 公司提供的更改，从 Qt 5.15 开始，开源离线安装程序不再可用。在 https://www.qt.io/blog/qt-offering-changes-2020 博客中阅读有关提供更改的更多信息。

点击链接进入下载点，详细安装步骤如下：

![](https://i-blog.csdnimg.cn/blog_migrate/b0256d4e43bca6dbd1cd25b142f6fd94.png)  
选择 qt  
![](https://i-blog.csdnimg.cn/blog_migrate/5b66ccf65a87bcb78ab46a23e0180a11.png)  
由于 2 中的原因，这里我选择了离线安装，没有选择在线安装，所以选择 5.14 版本  
![](https://i-blog.csdnimg.cn/blog_migrate/1d7e364a94870a81fb78f0b22d49ab70.png)  
这里我选择 5.14.2  
![](https://i-blog.csdnimg.cn/blog_migrate/232c9562dd8cc749ebbbe56f6febad3d.png)  
由于我是 [windows10](https://so.csdn.net/so/search?q=windows10&spm=1001.2101.3001.7020)，所以选择 windows 的安装包  
![](https://i-blog.csdnimg.cn/blog_migrate/61d88026e933c4f6f0aab35ea10c3c33.png)  
安装就不详细介绍了，可以参考 [nanke_yh](https://blog.csdn.net/nanke_yh/article/details/127132527) 兄弟的博客，很简单，下一步就可以了，挑几个重要的，安装目录不要放在系统盘，安装大概 20/30G 左右。  
![](https://i-blog.csdnimg.cn/blog_migrate/c39393ea485af69547420050fb27b241.png)  
组件没必要全部安装，看个人选择，下面的部分足够了，后面如果需要别的，可以在安装组件，参考 [CodingPioneer](https://blog.csdn.net/zlbdmm/article/details/129240178) 兄弟的操作即可。  
![](https://i-blog.csdnimg.cn/blog_migrate/7758d8a935d0909dc14af83b0abc25d6.png)  
其余都可以下一步，等待安装完成即可。

#### 2、下载 [VsCode](https://code.visualstudio.com/)

点击链接进入官网，如果是 [windows 系统](https://so.csdn.net/so/search?q=windows%E7%B3%BB%E7%BB%9F&spm=1001.2101.3001.7020)，默认是就是 windows，如下图：  
![](https://i-blog.csdnimg.cn/blog_migrate/601494cc9d0b1c2e26944863f098bfda.png)  
如果 linux 系统，那么就会是下图，mac 电脑没用过，应该默认是 mac 系统对应的版本  
![](https://i-blog.csdnimg.cn/blog_migrate/f90bb60346c695c8cd80c0487e10d3cb.png)  
下载完成后，一路 next，但是友情提示三点：

*   2.1 软件安装目录选择到非系统目录，这是安装过程中可以改的。
*   2.2 插件安装目录选择到非系统目录，建议后期手动修改
*   2.3 缓存目录选择到非系统目录，建议后期手动修改

#### 3、下载 [Cmake](https://cmake.org/)

点击链接进入官网，点击右上角下载按钮  
![](https://i-blog.csdnimg.cn/blog_migrate/6c6877d9736575ed15e5c79b636c0849.png)  
按照描述选择对应的安装方式  
![](https://i-blog.csdnimg.cn/blog_migrate/e4e6d48e5f7f43319b5e109a0a52ecb7.png)

### 二、配置环境变量

#### 1、打开环境变量设置

在 Windows 操作系统中，按下 Win + R 键，然后输入 “sysdm.cpl”，然后依次选择高级 -> 环境变量 -> 系统变量 -> 双击 path 变量

#### 2、QT 环境变量设置

选择新建，前两个路径是安装时选择的版本套件的目录和 bin 目录 (参考的别人的，可能是以防 vscode 找不到套件吧)；第三个是 QT 的 GCC 和 G++ 的执行文件的路径；最后一个可以不添加，点击确定保存设置。  
![](https://i-blog.csdnimg.cn/blog_migrate/2724422656c36c41cb6dcb74d98368b3.png)  
之后启动 cmd，输入 gcc -v，如果显示版本，则代表设置成功，注意最上方显示的 gcc 的路径  
![](https://i-blog.csdnimg.cn/blog_migrate/17de4c381a1c156febd17d3cab853398.png)

#### 3、Cmak 环境变量设置

按照 1 中的方法，打开环境变量，然后新建，将安装 cmake 的路径中的 bin 目录添加进去后，点击确定。  
![](https://i-blog.csdnimg.cn/blog_migrate/ad999a95e1129f78df7c34f044a7575e.png)  
配置完成在 cmd 中输入 cmake --version，显示版本则代表安装成功  
![](https://i-blog.csdnimg.cn/blog_migrate/78cf6b41768607d1c387d133b008460e.png)

### 三、Vscode 配置

#### 1、安装插件

C/C++：使用 C/C 语言编写代码，没什么好说的  
Chinese：VsCode 汉化，安装完成，重启 VsCode 即可  
Cmake 和 Cmake Tool：作用可以参考[博客园 - 可可西](https://www.cnblogs.com/kekec/p/14223504.html)  
QT Configure：具体描述可以查看该插件的 "细节" 部分  
Qt tools：具体描述可以查看该插件的 "细节" 部分

#### 2、配置

2.1 Cmake Tool 配置  
这里需要执行 Cmake 的可执行文件的路径，一定要包含文件名  
![](https://i-blog.csdnimg.cn/blog_migrate/e559ef48a4c3c079183460902ef207ba.png)

2.2 QT Configure 配置：  
  这里需要设置下图三个地方，Mingw PAth 设置了环境变量的话应该是自动有的，如果没有，手动添加一下；Qt Dir 是 QT 的安装目录；Qt Kit Dir 是套件的路径，找到自己对应的套件的路径，复制到这里。  
![](https://i-blog.csdnimg.cn/blog_migrate/84444760c46eae58bc147b74c42e63db.png)  
配置完成重启 VsCode，以防设置不生效。

### 四、使用

#### 1、新建项目

1.1 新建项目空文件夹，这里是在 share/qt / 目录下建了一个 hello 文件夹  
![](https://i-blog.csdnimg.cn/blog_migrate/21ba5538a69d8edc3154d8f495878351.png)

1.2 使用右键，通过 VsCode 打开，也可以通过 VsCode 左上角，文件 -> 打开文件夹，选中刚刚新建的文件夹，如下图  
![](https://i-blog.csdnimg.cn/blog_migrate/4b4eefe3568904f4b6dacff0bc8ee792.png)  
1.3 按 F1，输入 qt，选择新建工程 new project  
![](https://i-blog.csdnimg.cn/blog_migrate/011afa9afb4854c6a5484d6ba1afa7ae.png)  
1.4 输入项目名称，我这里是 hello，按回车  
![](https://i-blog.csdnimg.cn/blog_migrate/7bf48251655e8007919b54b58b19753c.png)  
1.5 选择套件，我这里选择 64 位的，因为我环境变量配置的也是 64 的，注意，这里一定要保证全部匹配  
![](https://i-blog.csdnimg.cn/blog_migrate/e6f54402ca5af57f1ee834d4fde60630.png)  
1.6 因为部署的是 Cmake，所以这里选择 Cmake  
![](https://i-blog.csdnimg.cn/blog_migrate/5d5b002a3d55de1aa039f71c29a1d3da.png)  
1.7 选择带 UI 文件的，我这里需要 UI 框，看个人选择  
![](https://i-blog.csdnimg.cn/blog_migrate/c72f0b648fcf85189df9b16454390772.png)  
1.8 完成后就建立了一个 QT 项目，如下图  
![](https://i-blog.csdnimg.cn/blog_migrate/c6c6bd0868112e189fa939ed9f7a5fc7.png)

#### 2、编译运行

2.1 检查编译工具  
![](https://i-blog.csdnimg.cn/blog_migrate/42ee7f134f06d9436ecdc75feea8660a.png)  
如果最下方没有选项卡，参考 **3 问题 3**  
如果有上图中工具及版本，那么接着下面的操作。如果没有，那么参考 **3 问题中的 1**

2.2 点击 build 选项卡编译 ，会构建项目，最左侧会有进度，构建完成未报错的话，会在左侧 build 目录生成 hello.exe 的可以行文件，如下图：![](https://i-blog.csdnimg.cn/blog_migrate/7c296b5a5bd1e09acff114b4a0db4819.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/65602338edd3d2e47164155a3dec64ec.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/941518db1e70ac5eedee36d1c007df1f.png)  
2.3 编译完成，接下来就是运行，试试看效果，点击最下方选项卡中的右三角箭头 launch 选项卡运行，如下图：  
![](https://i-blog.csdnimg.cn/blog_migrate/9c1e6c7bab26b06f87a76d727a5b72d1.png)  
2.4 出现此 UI 框代表配置成功，如果没有此项，参考 **3 问题 4**  
![](https://i-blog.csdnimg.cn/blog_migrate/367e087ccbbe8f2673316ae1b9da8155.png)

#### 3、问题

1、下方选项卡没有 kit 的问题  
![](https://i-blog.csdnimg.cn/blog_migrate/e0faaadfe92c74e697ce11c3390bf363.png)  
按 F1，输入 cmake，选择 " **扫描工具包** "，如果扫描到了，再次按 F1，输入 cmake，选择" **选择工具包** "，选择设置环境变量路径下的 GCC 和版本即可。如果下方的图中没有，但是环境变量又设置好了，那么参考 **3 问题中的 2**  
![](https://i-blog.csdnimg.cn/blog_migrate/3165a758995e6c68a114e8fd37d7a0ef.png)  
2、选择 " **选择工具包** " 时没有设置环境变量时的 GCC 工具的问题  
2.1 按 F1，输入 cmake，选择配置本地 cmake 工具包  
![](https://i-blog.csdnimg.cn/blog_migrate/98f4ceb268f30598b7ff9aaae1de8e4b.png)  
2.2 复制原有的配置信息，进行更改，将 QT 的 GCC 和 G++ 路径复制保存即可  
![](https://i-blog.csdnimg.cn/blog_migrate/c5e9882678bb4d60697f69273aeaaaa5.png)  
再次点击最下面选项卡的 **build** 编译工具，就会出现正确的编译工具了  
![](https://i-blog.csdnimg.cn/blog_migrate/d65aa116e9fceeb9c3f355dc3409dc70.png)  
3、创建完项目没有下方选项卡，cmake 没有 " **选择套件** " 选项的问题  
3.1 按 F1 输入 Cmake，选择配置，这里会自动配置该项目，如果你没有其他的 gcc 工具，那么 kit 选项卡应该是可以识别的。  
![](https://i-blog.csdnimg.cn/blog_migrate/bc759528fd33250ef0f155ede222b586.png)  
3.2 由于我这里安装了 mingw 的编译包，看下图输出台，如果有和我一样的问题，那么按下面的继续操作，如果没有可以跳过。  
![](https://i-blog.csdnimg.cn/blog_migrate/ed78ec5ac93f26fd6eb63f4053160627.png)

4、运行后发现，没有弹窗，但是终端显示执行了且没有报错，如下图：  
![](https://i-blog.csdnimg.cn/blog_migrate/448017fd1bd8e837c6f0047499d4e17f.png)  
原因是由于可执行程序根目录下没有 QT 相关 dll 文件，找到对应套件的 windeployqt.exe 文件，然后使用该文件去执行构建好的 build 目录下的可执行文件 hello.exe，会自动在 build 目录下生成所需要的 dll 文件，如下图：![](https://i-blog.csdnimg.cn/blog_migrate/7450fbfc25ad505cdd15fa021056b4aa.png)  
这个时候，我们再去点击运行，就会发现弹出了我们所需要的窗口了，如下图：  
![](https://i-blog.csdnimg.cn/blog_migrate/4d96c5bec67d7b8f54995efe841f76e1.png)