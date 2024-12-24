> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_43894537/article/details/143755134)

由于笔者不习惯 [QT Creator](https://so.csdn.net/so/search?q=QT%20Creator&spm=1001.2101.3001.7020) 工具的代码提示功能, 于是搭建 VSCode 环境下编写 QT 代码。本文章主要实现在 VSCode 环境下链接 QT 库, 实现编写代码时能有代码提示。

1. [VSCode 插件](https://so.csdn.net/so/search?q=VSCode%E6%8F%92%E4%BB%B6&spm=1001.2101.3001.7020)，由于只需要在 VSCode 环境下编写代码, 所以只安装 C/C++ 插件

![](https://i-blog.csdnimg.cn/direct/e99638e863514741960e64be0e2ce037.jpeg)

2. 先用 Qt Creator 创建一个简单工程

![](https://i-blog.csdnimg.cn/direct/7a04c3594f714598bac3246cc9f34141.jpeg)

3. 用 VSCode 打开工程文件夹, 打开 main.cpp 文件, 会发现头文件标红, 无法链接 QT 库文件

![](https://i-blog.csdnimg.cn/direct/08738ae82d85406887a2fe9302766815.jpeg)

4. 修改 .vscode/c_cpp_properties.json 内容, 使 VSCode 链接到 QT 库, 先确定 QT Creator 安装位置, 找到库文件路径, 例如：D:/Qt/Qt5.12.9/5.12.9/mingw73_64/include

![](https://i-blog.csdnimg.cn/direct/cd373f9c59d149f685e8070c2b668889.jpeg)

5. 然后再打开 main.cpp 头文件已经不标红, 可以在 VSCode 环境下编写 QT 程序, 并且有代码提示

![](https://i-blog.csdnimg.cn/direct/f6e8fb5196524276bba6c500ef7b38d0.jpeg)

6. 笔者一般在 VSCode 编写 QT 代码,  然后在 QT Creator 中编译运行代码,  如果想在 VSCode 中编译并运行代码，请参考博主: wlkkkkkkkk 的文章 [https://blog.csdn.net/WlkJiangYou/article/details/136262589](https://blog.csdn.net/WlkJiangYou/article/details/136262589 "https://blog.csdn.net/WlkJiangYou/article/details/136262589")