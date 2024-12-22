> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/Forever_change/article/details/134006968?ops_request_misc=&request_id=&biz_id=102&utm_term=ubuntu%E6%80%8E%E4%B9%88%E4%B8%8B%E8%BD%BD%E9%85%8D%E7%BD%AEvscode&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-6-134006968.142%5Ev100%5Epc_search_result_base5&spm=1018.2226.3001.4187)

### 一、下载 vscode

进入 vscode 官网，因为使用的是 ubuntu 系统所以**下载. deb 文件**。vscode 官网链接如下所示：

[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/ "Visual Studio Code - Code Editing. Redefined")  
![](https://i-blog.csdnimg.cn/blog_migrate/928b166615042e1a038d3f4190051580.png)

将安装包**下载在 Download（下载）目录**下

![](https://i-blog.csdnimg.cn/blog_migrate/7b844ac7c228cde69c87d6250b1f8e19.png)

###  二、安装 vscode

#### 1. 打开终端进入 Download（下载）目录

![](https://i-blog.csdnimg.cn/blog_migrate/d5de48519af34d2c20338de593e37def.png)

#### 2. 执行本地安装命令

**sudo dpkg -i** code_1.83.1-1696982868_amd64.deb

![](https://i-blog.csdnimg.cn/blog_migrate/d00caf20de392aeeab11fd0496c1e10c.png)

####  3. 打开 vscode 软件

 **a. 在终端执行 code 命令打开软件**

 **b. 在显示应用程序中的 vscode 右健添加到收藏夹。**

### **三、安装 vscode 插件（适合 C/C++）**

1. **C/C++：必须安装的插件**。可对 C 和 C++ 代码进行调试支持、智能提示、代码重构、代码格式化等重要基础功能

2. Code Runner：支持代码运行的插件。

3. Include AutoComplete：自动头文件包含 。

4. GBKtoUTF8：将代码 GBK 格式转换为 UTF8 的格式。

5. **C/C++ Snippets**：即 C/C++ 重用代码块。提供了许多常见的代码片段，例如 if/else 语句、循环语句、函数定义、数组操作等等。

6. Rainbow Brackets：彩虹花括号，有助于阅读代码。

7. Code Autocomplete，自动补全插件。

8. **C/C++ Advanced** [Lint](https://so.csdn.net/so/search?q=Lint&spm=1001.2101.3001.7020 "Lint") ： C/C++ 静态检测 插件。可以在编译前检查代码中的潜在错误，并提供错误提示和建议。它还支持多种编译器，包括 GCC、Clang 和 MSVC。

9. DeviceTree：设备树语法插件。

![](https://i-blog.csdnimg.cn/blog_migrate/5f5259c67d9b91d60edb9eefedbf50a5.png)![](https://i-blog.csdnimg.cn/blog_migrate/aa97cd83c9b71138246f66bfb166d02b.png)

### **四、设置 vscode 编译环境**

对于刚下载好的 vscode 中的代码格式不一定是适合自己平时的阅读习惯和编码习惯所以需要进行相应的设置。初始化的代码样式如下图所示：

![](https://i-blog.csdnimg.cn/blog_migrate/ada54e544a7fb007b9163fd53c0b9af5.png)

#### 1. 设置字体的样式

        在 VSCode 中，可以通过两种方式打开用户设置，一种是通过命令面板（**Ctrl+Shift+P**）输入 "Preferences: Open User Settings"。

        另一种方式是通过 "File" 菜单中的 "Preferences" 子菜单打开 "Settings" 面板，点击右上角的 "**Open Settings (JSON)**" 进入用户设置编辑界面。

##### a. 设置字体大小

```
{
    "editor.fontSize": 18
}
```

##### b. 设置字体系列（可选）

```
{
    "editor.fontFamily": "Consolas, 'Courier New', monospace"
}
```

###### "editor.fontFamily" 被设置为 Consolas、Courier New 和系统默认 monospace 字体的联合使用

##### c. 设置字体加粗（可选）

```
{
    "editor.fontWeight": "bold"
}
```

#### 2. 改变 tab 键的空格数

##### a. 单次修改

![](https://i-blog.csdnimg.cn/blog_migrate/59afc69165ea4e63a23c60e7098e214f.png)

在窗口顶部出现操作面板，在其中选择使用 “Tab” 缩进之后会接着出现从 1-8 这几个数字的选项。只需要选择 4 就可以设置 tab 键输入的空格数为 4 个了，但是该方法只是用于当前文件。

##### b. 永久修改

点击顶部状态栏之中的文件选项，然后在其下拉列表内依次**选择首选项 - 设置将编辑器设置窗口**打开。

 ![](https://i-blog.csdnimg.cn/blog_migrate/ece3b5b5b03e856baa512dd90fbf55af.png)

只需要在该输入框之中将值改成 4 就可以修改制表符空格数量为 4 个了，但是输入浮点数或者不是数字的字符就是会出现报错提示。

如果想要看更加细致的文章可以参考下面作者写的文章，内容非常详细：

[【精选】Linux 之 Ubuntu 代码开发工具 Visual Studio Code(VSCode) 的安装与一些常用插件配置的简单整理_visual studio ubuntu_仙魁 XAN 的博客 - CSDN 博客](https://blog.csdn.net/u014361280/article/details/127986092 "【精选】Linux 之 Ubuntu 代码开发工具 Visual Studio Code(VSCode) 的安装与一些常用插件配置的简单整理_visual studio ubuntu_仙魁XAN的博客-CSDN博客")