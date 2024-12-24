> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/pandamohist/p/14531209.html)

概述[#](#350416337)
-----------------

*   本文将介绍 **VScode** + **cmake** 在 **windows10** 上编译 c++ 代码
*   前提： 我之前已经安装过 VS2017， 故 编译将采用 cl.exe。

开始之前[#](#1921593250)
--------------------

本文演示环境基于 windows10。cmake 和 VScode 版本如下。  
VS code 版本: **1.54.1**  
cmake 版本: **3.18**

VSCode 插件安装[#](#1780945158)
---------------------------

我的插件安装的比较多，你瞧  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210313235112313-523511022.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210313235112313-523511022.png)  
还有  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210313235147620-1949480418.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210313235147620-1949480418.png)

编译本文演示的代码需要打开 VScode 插件商店或者离线安装如下插件：

### c/c++[#](#558805250)

下载地址： [cmake](https://marketplace.visualstudio.com/items?item>点我直达官网</a><br>
点击 <strong>install</strong> 将启动 vscode 在线安装 或者右侧的 <strong>download extension</strong> 再离线安装</p><p></p><h3 id=)[#](#724028812)

下载地址： [cmake tools](https://marketplace.visualstudio.com/items?item>点我直达官网</a></p><p></p><h3 id=)[#](#2523530217)

下载地址： [cmake](https://marketplace.visualstudio.com/items?item>点我直达官网</a><br>
插件安装后，下面开始准备安装 cmake</p><p></p><h2 id=)[#](#2402554638)

*   下载地址： [https://cmake.org/download/](https://cmake.org/download/)
    
*   下载适合自己的版本， 安装后，将其 **cmake.exe** 所在目录添加到系统环境变量（或者打开命令行转到 cmake.exe 所在目录），测试 cmake 是否安装成功。  
    [![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000232733-929125967.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000232733-929125967.png)
    
*   测试键入命令 **cmake --version**. 如果弹出类似下面的输出，则说明成功  
    [![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000447407-9837792.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000447407-9837792.png)
    

使用 VScode 打开文件夹[#](#2815969611)
-------------------------------

方便测试，可在桌面创建文件夹， 这里取名为 **udp_socket** 为例。创建成功后，使用 VScode 打开该文件夹， 方式有两种：

*   1. 先打开 VScode, VS code 首页会提示你选择打开文件夹  
    [![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000832490-969075732.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000832490-969075732.png)  
    VScode 菜单也可以打开文件夹  
    [![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000920291-1653052529.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314000920291-1653052529.png)
*   2. 打开创建的 **udp_socket** 文件夹，此时，点击鼠标右键菜单中选择 **通过 VScode 打开** ， 即可。

演示代码[#](#3464291670)
--------------------

基于打开的 VScode，创建 名为 **main.cc** 的文件  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314001252198-358522818.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314001252198-358522818.png)  
创建结束后是这样的  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314001326763-1200549382.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314001326763-1200549382.png)  
文件已经准备好，准备一段测试代码，如下

```
#include <iostream>

/// 增加函数调试使用
void hello_vs_code_()
{
    using namespace std;
    int x = 11 + 22 + 33;
    cout << "\n x = " << x;
}

int main(int argc, char* argv[], char* en[])
{
    using namespace std;
    cout << "hello vscode";
    hello_vs_code_();

    return 0;   
}


```

点击保存。

准备 cmakelists.txt 文件[#](#3008193398)
------------------------------------

*   cmakelists.txt 文件放在创建的 **udp_socket** 文件夹下。  
    可以创建默认的 CMakeLists.txt 文件，不过，文件内容不是我想要的，我选择了更加通俗易懂的 **modern cmake**。 cmakelists.txt 文件内容如下

```
cmake_minimum_required(VERSION 3.18)

# ---------------------------------------------------------------------------------------------------
# 1. set name
project(lib_udp)

# ---------------------------------------------------------------------------------------------------
# 2. to get all source files
# set source files
# -------------------------------------------------------------------------------------
file(GLOB_RECURSE udp_src ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cc)

# build a library for udp
function(lib_udp_on_win)
	# dynamic library
	# ---------------------------------------------------------------------------------------------------

	# compiler is vs
	if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
		# build program
		add_executable( ${PROJECT_NAME} ${lib_udp_type} ${udp_src} )

		# .h and .cxx files
		target_sources(${PROJECT_NAME} PRIVATE ${udp_src} )

		# use c++11
		target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_11)

	endif("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
endfunction(lib_udp_on_win)


# build  
# ---------------------------------------------------------------------------------------------
if (CMAKE_SYSTEM_NAME MATCHES "Windows")
	lib_udp_on_win()
endif()



```

这段配置代码 仅配置一个可执行程项目。cmake 语法不是本文的重点。

cmake tools 插件配置[#](#3526955335)
--------------------------------

*   一定要理解， VScode 仅仅是一个软件，没有 Visual studio 2017 这样的 IDE 集成度高，什么都配置好了。
*   打开 VScode 的设置， 键入： cmake  
    [![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314002418384-1557173994.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314002418384-1557173994.png)  
    设置 cmake.exe 所在路径，如图，我的 cmake 安装在 **C:\major\development\tools\cmake_64\bin** 目录下，同时将 cmake.exe 添加到目录后面，指定 cmake 的绝对路径。

开始编译代码[#](#3435514908)
----------------------

下面的操作都是基于： ctrl + shift + p 快捷键。

### 1. select a kit[#](#2658524838)

按下快捷键 ctrl + shift + p ， 键入： **cmake:select a kit**, 回车选择适合自己的工具包。  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003020474-1680974525.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003020474-1680974525.png)  
我这里演示的是 x86  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003056102-780893235.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003056102-780893235.png)

### 2. select variant[#](#2013272721)

按下快捷键 ctrl + shift + p ， 键入： **cmake:select variant**  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003158007-1940781175.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003158007-1940781175.png)  
因为要演示调试，这里选择 debug.  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003320956-524555957.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003320956-524555957.png)

### build[#](#1153065813)

按下快捷键 ctrl + shift + p ， 键入： **cmake:build**, 选择 `cmake:build**. ![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003520456-1189206038.png) 观察`输出 ` 窗口， 可以看到已经编译成功  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003621982-1472359560.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314003621982-1472359560.png)

### debug 调试[#](#4184791441)

设置好断点，按下快捷键 ctrl + shift + p ， 键入： **cmake:debug** , 程序将执行，并停在断点所在位置，  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314004007022-1587597936.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314004007022-1587597936.png)  
调试快捷键和 VS 开发 IDE 一致。  
左侧可以观察变量的值。  
[![](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314004102061-1966862805.png)](https://img2020.cnblogs.com/blog/1630599/202103/1630599-20210314004102061-1966862805.png)

### 继续运行 F5[#](#709098892)

F5， 程序将运行结束。

补充 (可有可无）[#](#1660024086)
-------------------------

如果你更改了 cmakelists.txt 文件，可以使用命令 **cmake:configure** 实现项目配置，再执行 build 就 OK 了