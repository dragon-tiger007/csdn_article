> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/TM007_/article/details/140250158)

![](https://i-blog.csdnimg.cn/direct/c42956c00ae74599a84d521384dddf79.webp)

**目录**

[👋前言](#t0)

[👀一、内网穿透](#t1)

[🌱二、内网穿透工具](#t2)

[💞️三、本地测试](#t3)

 [3.1 环境准备](#%C2%A0%20%C2%A0%20%C2%A0%20%C2%A0%203.1%20%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)

 [3.2 nginx 修改启动页面](#%C2%A0%20%C2%A0%20%C2%A0%20%C2%A0%20%C2%A03.2%20nginx%20%E4%BF%AE%E6%94%B9%E5%90%AF%E5%8A%A8%E9%A1%B5%E9%9D%A2)

 [3.3 神卓互联注册，创建映射规则](#%C2%A0%20%C2%A0%20%C2%A0%20%C2%A0%203.3%20%E7%A5%9E%E5%8D%93%E4%BA%92%E8%81%94%E6%B3%A8%E5%86%8C%EF%BC%8C%E5%88%9B%E5%BB%BA%E6%98%A0%E5%B0%84%E8%A7%84%E5%88%99)

[📫四、章末](#t4)

#### 👋前言

        小伙伴们大家好，一直有一个想法，就是搭建自己的网站，并且可以通过公网访问；中间也去看了很多搭建服务的步骤，了解到有两种常见的实现方式，一个就是购买商业服务器，把项目部署到上面，另外就是自己的电脑做服务器，通过内网穿透实现，这篇文章来看下如何将自己的电脑开发成一个服务器，并且可以供外网访问

#### 👀一、内网穿透

        内网穿透是一种网络技术，允许通过公共网络（比如互联网）访问处于私有局域网（内网）内部的计算机或服务。简单来讲比如我们日常自己开发的 SpringBoot 项目，只能我们自己本地访问 “ http://localhost:8080/...", 通过内网穿透操作后，不仅可以自己本地访问，在别的设备上也可以访问本地的 8080 端口

#### 🌱二、内网穿透工具

        本地测试使用的是 神卓互联，官网如下：

[巴比达_企业级内网穿透](https://www.shenzhuohl.com/chuantou.html?bd_vid=12686741513710243326 "巴比达_企业级内网穿透")

        目前市面上有很多供应商，具体介绍可以参考这篇博主大大的文章，介绍了很多可以实现的工具（但是注意甄别，有的方法已经不可用了）

[十多种 五种永久免费 内网穿透傻瓜式使用_免费内网穿透 - CSDN 博客](https://blog.csdn.net/qq_40739917/article/details/106001561?ops_request_misc=&request_id=&biz_id=102&utm_term=%E9%A3%9E%E9%B8%BD%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F%E5%85%8D%E8%B4%B9&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-106001561.142%5Ev100%5Epc_search_result_base9&spm=1018.2226.3001.4187 "十多种 五种永久免费 内网穿透傻瓜式使用_免费内网穿透-CSDN博客")

#### 💞️三、本地测试

#####         3.1 环境准备

        既然要测试访问本地项目，首先要有一个可以启动的项目，这里想到了之前用的 Nginx, 把 nginx 下载到本地后可以一键启动，也可以算是一个可以运行的项目，访问 nginx 监听的端口号即可访问一个页面，nginx 的下载安装使用可以看之前的文章，链接如下：

[【Nginx ＜一＞⭐️】Nginx 的初步了解以及安装使用_nginx 1.26.0 安装 - CSDN 博客](https://blog.csdn.net/TM007_/article/details/138791806?spm=1001.2014.3001.5501 "【Nginx ＜一＞⭐️】Nginx 的初步了解以及安装使用_nginx 1.26.0 安装-CSDN博客")

#####          3.2 nginx 修改启动页面

                3.2.1 修改 nginx 默认访问页面，index2.html 是一个简单的登录页面，html 文件也可以简单通过创建 .txt 文件修改好之后再改为 .html 文件即可

                注：修改默认访问页面之后需要重启 nginx 项目，访问监听端口页面如下![](https://i-blog.csdnimg.cn/direct/e1a84dda3c294c6ca9bb7f049c5a657e.png)

![](https://i-blog.csdnimg.cn/direct/3f8701e1876248d2bb9bbc8a2c30f69b.png)![](https://i-blog.csdnimg.cn/direct/a76687786d7b4c2d9f806aed3bf7add1.png) 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta >
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        html {
            height: 100%;
        }
        body {
            height: 100%;
        }
        .container {
            height: 100%;
            background-image: linear-gradient(to right, #fbc2eb, #a6c1ee);
        }
        .login-wrapper {
            background-color: #fff;
            width: 358px;
            height: 588px;
            border-radius: 15px;
            padding: 0 50px;
            position: relative;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
        .header {
            font-size: 38px;
            font-weight: bold;
            text-align: center;
            line-height: 200px;
        }
        .input-item {
            display: block;
            width: 100%;
            margin-bottom: 20px;
            border: 0;
            padding: 10px;
            border-bottom: 1px solid rgb(128, 125, 125);
            font-size: 15px;
            outline: none;
        }
        .input-item:placeholder {
            text-transform: uppercase;
        }
        .btn {
            text-align: center;
            padding: 10px;
            width: 100%;
            margin-top: 40px;
            background-image: linear-gradient(to right, #a6c1ee, #fbc2eb);
            color: #fff;
        }
        .msg {
            text-align: center;
            line-height: 88px;
        }
        a {
            text-decoration-line: none;
            color: #abc1ee;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="login-wrapper">
            <div class="header">Login</div>
            <div class="form-wrapper">
                <input type="text" >
                <input type="password" >
                <div class="btn">Login</div>
            </div>
            <div class="msg">
                Don't have account?
                <a href="#">Sign up</a>
            </div>
        </div>
    </div>
</body>
</html>
```

#####         3.3 神卓互联注册，创建映射规则

        官网如下，注册登录不做赘述：

[神卓互联内网穿透 | 端口映射工具 - 神卓互联官网](https://www.shenzhuohl.com/ "神卓互联内网穿透|端口映射工具-神卓互联官网")

                3.3.1 映射规则创建

![](https://i-blog.csdnimg.cn/direct/c253c27e51d34e3db123fb86f832fb02.png)

![](https://i-blog.csdnimg.cn/direct/225abdf774c3433b957ac29a0dadab75.png)                 3.3.2 创建完成之后，会提示需要下载桌面版然后重启映射规则

                按照提示下载安装包，安装完成之后使用自己的账号登录，然后重启映射规则

![](https://i-blog.csdnimg.cn/direct/5b567e7ba2204ee2b35150a7a13a0811.png)

                之后，就可以通过映射的公网地址，访问我们本地启动的项目了！ 

![](https://i-blog.csdnimg.cn/direct/291031b017514cce92c78702f3b68557.png)

#### 📫四、章末

        用自己的电脑[搭建服务器](https://so.csdn.net/so/search?q=%E6%90%AD%E5%BB%BA%E6%9C%8D%E5%8A%A1%E5%99%A8&spm=1001.2101.3001.7020)，整体比较简单，但是缺陷也很明显，对电脑的配置要求比较高，另外就是电脑要一直启动，并且项目也不能停止；总的来说，用来部署个人项目还可以！

        文章到这里就结束了~

![](https://i-blog.csdnimg.cn/direct/a0a11bbcbf014bf2828024c18105a1b0.png)