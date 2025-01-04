> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/m0_68926749/article/details/135586373?ops_request_misc=%7B%22request%5Fid%22%3A%227522dc2f4d43132fa0295cca3d903b1b%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=7522dc2f4d43132fa0295cca3d903b1b&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-135586373-null-null.142%5Ev101%5Econtrol&utm_term=github%20copilot%E5%AE%89%E8%A3%85&spm=1018.2226.3001.4187)

#### [Github](https://so.csdn.net/so/search?q=Github&spm=1001.2101.3001.7020) Copilot 最全的安装与使用教程

*   [第一章 安装](#__13)
*   [1. 安装 GitHub Copilot](#1_GitHub_Copilot_14)
*   [2. 获取资格](#2_18)
*   [第二章 使用](#__47)
*   *   [1. 产生建议](#1_48)
    *   *   [1.1 键入你想要完成的操作的注释](#11__49)
        *   [1.2 Ctrl+I](#12_CtrlI_51)
    *   [2. 接受建议](#2__56)
    *   [3. 查看下一个建议](#3_60)
    *   [3. 接受部分建议](#3_69)
    *   [4. 在新选项卡接受建议](#4_73)
    *   [5. 完成多项功能](#5_76)
    *   [6. 聊天](#6_81)

![](https://i-blog.csdnimg.cn/blog_migrate/5c40996f5b0ef3340907bc90578826b0.png)

> *   GitHub Copilot 供经过验证的学生、教师和热门开源项目的维护人员免费使用。
> *   如果你不是学生、教师或热门开源项目的维护人员，可以在一次性 30 天试用期中免费试用 GitHub Copilot。
> *   免费试用后，需要付费订阅才能继续使用。
> *   GitHub Copilot 目前为止可以免费试用**一个月**，但是试用的前提是必须要绑定银行卡，因为后续会自动扣费，所以请注意试用结束日期，自己定好闹钟关闭订阅。
> *   订阅价格为每月 **10 美刀**，每年 **100 美刀**，个人觉得还是些许有点贵，如果有需要可以订阅，但请慎重考虑是否订阅（土豪请忽略我这条建议，直接按年订阅拿下）

第一章 安装
------

1. 安装 GitHub [Copilot](https://so.csdn.net/so/search?q=Copilot&spm=1001.2101.3001.7020)
---------------------------------------------------------------------------------------

首先，你需要在 **Visual Studio Code** 中安装 GitHub Copilot 扩展。你可在 [VS Code](https://so.csdn.net/so/search?q=VS%20Code&spm=1001.2101.3001.7020) 的扩展市场中搜索 “GitHub Copilot” 并点击安装。

![](https://i-blog.csdnimg.cn/blog_migrate/4d761764a48fd751549cf3bb28cbf16a.png)

2. 获取资格
-------

首先你肯定要有一个 github 账户  
![](https://i-blog.csdnimg.cn/blog_migrate/95bdb1132dd5eb25fee75894b6917d51.png)  
点击左上角的 sign in，只要输入邮件地址就可以创建账户了

上网搜索一下 Github Copilot，点击官方网址。  
附上官方网址  
[Github Copilot](https://github.com/features/copilot)

![](https://i-blog.csdnimg.cn/blog_migrate/c93d9014a2bd0a2d043d7cd961f84140.png)  
接着点击 Get started with Copilot

![](https://i-blog.csdnimg.cn/blog_migrate/a76b244ff691d12327f572a3606d1e1c.png)  
**划到最下面，点击 start a free trial**(或者各位大佬可以自行付费购买，这部分需要自己探索）

![](https://i-blog.csdnimg.cn/blog_migrate/9856718d77e25760b0dfeff25515f11d.png)  
这就是成功拥有 Github Copilot 的样子。

![](https://i-blog.csdnimg.cn/blog_migrate/b6bda5ea822dc85503d60f580222220f.png)

那么学生版的免费该如何操作呢？  
首先你需要登陆你的 github, 然后点击进校园版的  
![](https://i-blog.csdnimg.cn/blog_migrate/7a668bfcbcfd742f049d71026a85828a.png)  
你需要提供你的学生证照片，**然后耐心地等待几天！！**，等待官方发给你邮件，提示你可以用的时候，你就可以免费地使用了。

> **前提：你可能需要学会科学上网，不然登不上去**

** 到这一步了，你的 VSCODE 就拥有了 Github Copilot！！！！！ **  
![](https://i-blog.csdnimg.cn/blog_migrate/295e2d962e8b83731dda62835e8316ac.png)

第二章 使用
------

### 1. 产生建议

#### 1.1 键入你想要完成的操作的注释

![](https://i-blog.csdnimg.cn/blog_migrate/55986a478582df807497494331ea45e8.png)

#### 1.2 Ctrl+I

![](https://i-blog.csdnimg.cn/blog_migrate/5aad2b633a210a6c6f365bd9c911091e.png)  
点接受就可以了。

### 2. 接受建议

在写代码时候，Github Copilot 会给一些建议，你只需要按 Tab 就可以接受了。  
![](https://i-blog.csdnimg.cn/blog_migrate/ae9a0e634600e440e1aa50db43e2fd0f.png)  
如果说没有建议的话，你可以按**两下 Tab**，就可以获得 Github Copilot 的建议了。

### 3. 查看下一个建议

不同操作系统快捷键不一样  
![](https://i-blog.csdnimg.cn/blog_migrate/46fbe240f70a494ebeea4cdf214c38d9.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/a210005e1f578206fa3f0de231cdc77b.png)  
按下快捷键 Alt+]  
![](https://i-blog.csdnimg.cn/blog_migrate/3184e44483a651eaa9766606527053e3.png)  
就可以有不同的建议了。

### 3. 接受部分建议

![](https://i-blog.csdnimg.cn/blog_migrate/a35bf0b9d3ac0a74ac663380b226cc9a.png)  
按下快捷键 Control±>，可以发现只接受了 main 一个单词。  
![](https://i-blog.csdnimg.cn/blog_migrate/cb429d6e23b74f226208461659b4d2d5.png)

### 4. 在新选项卡接受建议

如果说嫌麻烦，那就在新选项卡接受建议，按下 Ctrl+Enter 即可  
![](https://i-blog.csdnimg.cn/blog_migrate/df1565c87c723e047dd7674930b08eb8.png)

### 5. 完成多项功能

你可以右键看到以下内容：  
![](https://i-blog.csdnimg.cn/blog_migrate/49d489a93cf08944f7a6eec28af1cdf7.png)  
可以完成解释代码任务，修复，生成文档和生成测试等任务。

### 6. 聊天

在你安装完 Github Copilot 后，你会发现 VSCODE 左边多了 Github Copilot 的聊天窗口，你可以像 Chatgpt 一样和 Github Copilot 聊天，

![](https://i-blog.csdnimg.cn/blog_migrate/a545eea9fa8cf6bc6df4209dc5011bbd.png)

Github Copilot 不仅根据你的要求生成代码，而且还会对代码进行说明，最重要的是 Github Copilot 会提示你联想的代码问题，增加你的代码水平。

具体情况可以在以下网址查看：

[Github Copilot 说明文档](https://docs.github.com/zh/copilot/using-github-copilot/getting-started-with-github-copilot)

最近新开了公众号，请大家关注一下。  
![](https://i-blog.csdnimg.cn/blog_migrate/c6e4f6ee239b7770d7acabe0cb976ef2.jpeg)