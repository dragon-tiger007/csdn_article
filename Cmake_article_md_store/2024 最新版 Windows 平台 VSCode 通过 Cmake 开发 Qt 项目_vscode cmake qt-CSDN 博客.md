> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_74027669/article/details/142960155)

        VSCode 配合 Cmake 可以非常[有效的](https://so.csdn.net/so/search?q=%E6%9C%89%E6%95%88%E7%9A%84&spm=1001.2101.3001.7020)开发 Qt 项目，因为它提供的 Qt 插件和 Cmake 插件具有较好的集成性。**(本博客使用的是 Qt6 和 Cmake3)。**

  首先创建一个空的文件夹。

![](https://i-blog.csdnimg.cn/direct/bd9984b4a55346239632812842f5f02b.png)

        将下面的插件都安装好

![](https://i-blog.csdnimg.cn/direct/9da7ab88a1b648a9bda6a5ded845f291.png)

![](https://i-blog.csdnimg.cn/direct/b18fab53bdb94b0cb687d8e8eecfc4c4.png)

接着 ctrl+shift+p，选择 QConfigure:New Project。

![](https://i-blog.csdnimg.cn/direct/10ac53fa4ec04c908d9c054ead2f722d.png)

指定项目名称

![](https://i-blog.csdnimg.cn/direct/1281460317d544f3b7102fffd23ee46c.png)

        选择 Qt 的工具链，这里必须使用 Qt 自带的 mingw 或者 msvc 等编译器，不能使用自己安装的 mingw 或者 msvc 编译器。

 **不然后面 cmake 编译的时候会报错提示：**

 **cannot find -ld3d12**

        因为自己的 mingw 找不到这个库，Qt 的 mingw 有找到这个库的功能，这个库是 windows 自带的一个库，基本 windows 系统都具备，但不一定找得到。

![](https://i-blog.csdnimg.cn/direct/fe89b66075554168bebae9235bd5b31d.png)

构建工具使用 Cmake

![](https://i-blog.csdnimg.cn/direct/ab9799c2a51a4e6186d97e7556827f99.png)

把 UI 文件带上

![](https://i-blog.csdnimg.cn/direct/205cd767906c49b6a94e9599427825ae.png)

接着就得到了源码文件了。

![](https://i-blog.csdnimg.cn/direct/edad88e8d41b440ab6c81aeda9c01e96.png)

由于 [VSCode 插件](https://so.csdn.net/so/search?q=VSCode%E6%8F%92%E4%BB%B6&spm=1001.2101.3001.7020)生成的 Cmake 文件是针对 Qt5 的，所以对于 Qt6 我们需要进行一定的更改。

![](https://i-blog.csdnimg.cn/direct/b3c8cafa47cc4713aa354a5c68df1b9b.png)

将多余的代码去掉，并将模块改为 Qt6。

![](https://i-blog.csdnimg.cn/direct/774cf406f9a441c7b47cc55d216d5d74.png)

```
cmake_minimum_required(VERSION 3.5)
project(qttest2 LANGUAGES CXX)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_PREFIX_PATH "d:/c.app/QT6/6.6.2/mingw_64")
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
find_package(Qt6 COMPONENTS Widgets REQUIRED)
aux_source_directory(./src srcs)
add_executable(${PROJECT_NAME}
    ${srcs} 
) 
target_link_libraries(${PROJECT_NAME} PRIVATE Qt6::Widgets)
```

再次 ctrl+shift+p，点击 Cmake 配置

![](https://i-blog.csdnimg.cn/direct/2076a7771d8b41fcb3b5c07d3c548eef.png)

这里的工具链一定要选择 QT 自带的。

![](https://i-blog.csdnimg.cn/direct/6cb202c67a5a49558cac81d480b7b2fd.png)

        如果没有显示出来，那你就点击扫描工具包，可以扫描出来，当然前提是你配置了 PATH 环境变量，这个安装 Qt 的时候一般都会告诉你配置。

        选择完工具链之后就开始构建了。

![](https://i-blog.csdnimg.cn/direct/a58d52044c514b278fa2489d9aaea035.png)

发现我们的目录多出了 build 目录，这时候我们就可以将. vscode 删掉了，因为. vscode 是 VSCode 自带的 C++ 项目构建工具，但是我们已经有 CMake 来构建了，所以就不需要了。

![](https://i-blog.csdnimg.cn/direct/fc2b249af470441eada64c2a6f923ec2.png)

        你的 build 目录和我的 build 目录里面的文件可能是不一样的，因为我用的 Cmake 生成器是 Ninja，所以产生的是 build.ninja，ninja 就是通过 build.ninja 产生可执行文件的，你的可能是 MSVC 所以可能会产生. sln 文件，或者你是使用最原始的 Makefile 生成器，那么产生的就是 Makefile 文件。

![](https://i-blog.csdnimg.cn/direct/6487b382842f424d8a2a3120844b5e20.png)

接着打开终端

![](https://i-blog.csdnimg.cn/direct/b36d861493df4dc2ab0d83e2b90d2b99.png)

进入到 build 目录中，输入 Ninja -C ./

代表在当前目录，也就是 build 目录产生可执行文件。

![](https://i-blog.csdnimg.cn/direct/70cad1c1c5ee4cc090a7d7d978cce4fc.png)

接着直接执行可执行文件就可以了，.\qttest2.exe。

![](https://i-blog.csdnimg.cn/direct/739f3b386bf14d24a06828b504e94fd1.png)

        至此 VSCode 配置 Qt 开发就完成了，我比较喜欢这种构建方式，因为这种方式可以完全掌控项目的文件组成，不想 QtCreator 将项目封装起来了，你都不知道怎么分包编译. cpp 文件，如果我想要将. ui 文件放到 view 文件夹下面，.cpp 文件放到 src 文件夹下面，所有. h 文件放到 include 文件夹下面，这时候 Cmake 构建这种项目就特别清晰了。