> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_54165863/article/details/142812311)

使用 VSCode 进行 [Qt 开发](https://so.csdn.net/so/search?q=Qt%E5%BC%80%E5%8F%91&spm=1001.2101.3001.7020) 插件 Qt Support
----------------------------------------------------------------------------------------------------------------

使用 VSCode 进行 Qt 开发一般都是使用的官方插件 Qt tools，使用起来并不是太方便，所以我选择 Qt Support 插件。

### 一、Qt Support 功能

1.  可以创建项目
2.  导入基于 CMake 的 qt 项目
3.  可以添加 Qt 项目文件
    *   Designer Form Class
    *   Designer Form
    *   C++ class
    *   Translation
    *   Resource
4.  Shift + F1 快捷键查看 Qt 文档
5.  qrc 资源编辑器
6.  使用 Qt Designer 打开 ui 文件
7.  使用 Qt Linguist 打开 ts 文件
8.  使用 Qt Creator 打开项目
9.  使用 Visual Studio 打开项目

### 二、配置环境变量

#### 1. 需要安装的软件

*   VSCode [VSCode 官网下载](https://code.visualstudio.com/Download)
*   Qt [Qt 官网下载](https://download.qt.io/official_releases/online_installers/)
*   cmake 跨平台编译工具 [CMake 官网下载](https://cmake.org/)
*   llvm 编译器 [llvmorg-18.1.8 下载地址](https://github.com/llvm/llvm-project/releases/tag/llvmorg-18.1.8)

Qt 需要安装 mingw 编译器，mvs[c 编译器](https://so.csdn.net/so/search?q=c%E7%BC%96%E8%AF%91%E5%99%A8&spm=1001.2101.3001.7020)这个插件暂时还不支持。

Qt Support 需要使用 llvm 里面 clangd。

下载 llvm 进入链接选择 win32 或者 win64 都可以。  
![](https://i-blog.csdnimg.cn/direct/889537d98fdd4abbb9c0899d0be60eed.png#pic_center)

#### 2. 设置环境变量

需要设置 cmake 和 LLVM 环境变量  
![](https://i-blog.csdnimg.cn/direct/11414911a72f492797d797f1a1be3be1.png#pic_center)

启动 cmd，输入 **cmake --version** 和 **clangd --version** 命令，显示版本表示配置环境成功。

![](https://i-blog.csdnimg.cn/direct/f6bdc2a8ace640a6ba1d67d736f9eec1.png#pic_center)

### 三、VSCode 配置

#### 1. 安装插件

在扩展商店下载搜索 Qt Support 插件，安装好插件自动安装其他配套插件

![](https://i-blog.csdnimg.cn/direct/3674f6c9074a4444b9752b97a227fe15.png#pic_center)

#### 2. 配置插件

##### 1. 配置 Qt Support 插件

添加 qt 安装目录

![](https://i-blog.csdnimg.cn/direct/d7265c064aa94fc2b9aefdece15a3747.png#pic_center)

##### 2. 配置 CMake Tools 插件

添加 cmake 可执行文件路径，我们添加了 cmake 的环境变量，直接用 **camke** 也可以  
![](https://i-blog.csdnimg.cn/direct/251ed95f24064006a71ba8a718de4db4.png#pic_center)

##### 3. 配置 Kylin Clangd 插件

在. vscode 文件下打开 settings.json 文件，添加指定编译器程序，添加这个是为了防止 qt 头文件报错

![](https://i-blog.csdnimg.cn/direct/2a341963825c4fc68d5a426229b34c0d.png#pic_center)  
按 F1 输入 **restart the clangd language server** 选择重新启动 clangd 语言服务

##### 4. 配置 cmake

按 F1 输入 **open user settings** 选择打开用户设置（json）, 添加用于指定构建系统生成器

```
"cmake.generator": "MinGW Makefiles"

```

![](https://i-blog.csdnimg.cn/direct/5068ebf9e47040bfb2896f3b688fa98f.png#pic_center)

### 四、开始创建 qt 项目

1.  点击创建 qt 项目

![](https://i-blog.csdnimg.cn/direct/0b9a3ab60fe446648678df437a53280f.png#pic_center)  
2. 选择项目模板，一般选择带 ui 的，点击下一步

![](https://i-blog.csdnimg.cn/direct/e4fa6bca67134848be378fe04429d16d.png#pic_center)

4.  设置项目名称，选择创建位置选择，点击下一步  
    ![](https://i-blog.csdnimg.cn/direct/9896ca557a9840fb8d553184e03b49a1.png#pic_center)
    
5.  可以设置主窗口基类，点击完成  
    ![](https://i-blog.csdnimg.cn/direct/487acd1540be426a939d374e63800fc0.png#pic_center)
    
6.  qt 项目创建成功，在左下角点击 kit 选编译器，再点击下面的运行按钮就可以了
    

> 你再 Kylin Clangd 插件设置的编译器程序是 64 位，在编译项目也要用 64 位的，不然 qt 的文件会报错，但是也能运行。

![](https://i-blog.csdnimg.cn/direct/ce3a6a0d9461411cb1dbe3604178cc7b.png#pic_center)

### 五. 运行 qDebug 不打印

在 CMakelists.txt 中将 **WIN32_EXECUTABLE** 改为 **false**, 就会打印

```
set_target_properties(Qtwidgetapplication PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE true
    WIN32_EXECUTABLE false
)

```

### 六. qDebug 打印乱码

在 main 文件使用 Qt 框架中的 QTextCodec 类，设置应用程序的文本编码方式为 GBK，这种方法就可以解决

```
int main(int argc, char *argv[])
{
    QTextCodec *codec = QTextCodec::codecForName("GBK");
    QTextCodec::setCodecForLocale(codec);
    QApplication a(argc, argv);
    Qtwidgetapplication w;
    w.show();
    return a.exec();
}

```