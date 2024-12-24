> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_45100742/article/details/139024078)

### 1、简介

*   LLVM（Low Level Virtual Machine）是一个**开源的编译器基础设施项目**，最初由克里斯 · 拉特纳（Chris Lattner）在 2000 年开发，后来在苹果公司、Google、Mozilla、Intel 等多家公司和社区的贡献下不断发展壮大。LLVM 提供了一系列可重用的编译器和工具链技术组件，用于开发编译器前端和后端。
*   **组成部分：**
    *   **LLVM Core Libraries**：核心库，包括中间表示（IR）、类型系统、指令定义等。
    *   **Clang**：基于 LLVM 的 C、C++ 和 Objective-C 编译器前端。
    *   **LLD**：基于 LLVM 的链接器。
    *   **LLVM IR**：中间表示语言，用于描述程序的高级抽象，既可以用作前端和后端的接口，又可用于中间优化。
    *   **LLVM Passes**：优化和转换代码的各种 passes，包括循环优化、数据流分析、寄存器分配等。
    *   **LLDB**：基于 LLVM 的调试器。
    *   **MLIR（Multi-Level Intermediate Representation）**：支持多层次中间表示，用于更复杂和异构的计算模型。

### 2、安装 clang

*   下载 LLVM 并安装。下载地址：[Releases · llvm/llvm-project (github.com)](https://github.com/llvm/llvm-project/releases "Releases · llvm/llvm-project (github.com)")
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/29ed958b001e97a117ba9cdb05de38cd.png)
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/e509bcae84a1d636c2b1b3b97b24e9e2.png)
*   验证：
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/de909d60cb81e3411ddfea70671b0472.png)
*   下载 mingw64**（可选）**。下载地址：[Downloads - MinGW-w64](https://www.mingw-w64.org/downloads/#llvm-mingw "Downloads - MinGW-w64")
    *   **【注】安装 MinGW-w64 并不是必须的，但可以提供一些有用的功能和支持。**
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/37974851f5f39e2bd43295825d3f3911.png)
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/306f5ec5f7fe7a93dd4c7c2672b06a87.png)
    *   解压后，配置 path。
        *   ![](https://i-blog.csdnimg.cn/blog_migrate/1e115d82941e5cd47220cd868ecc584e.png)
    *   验证：
        *   ![](https://i-blog.csdnimg.cn/blog_migrate/55a0c3d46f562247c92fa26a35d322f8.png)

### 3、测试

*   准备一个 hello.c 文件。
    *   ```
        #include <stdio.h>
         
        int main() {
            printf("Hello, World!\n");
            return 0;
        }
        ```
        
*   ```
    clang hello.c -o hello.exe
    
    ```
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/cedaf456291b8c5f141251784d5398db.png)