> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/a137748099/article/details/117336959)

上一篇说到 Qt Creator 配置 [WebAssembly](https://so.csdn.net/so/search?q=WebAssembly&spm=1001.2101.3001.7020)，通过 Qt Creator 编译 Qt 程序并运行

#### 部署

编译会生成以下几个文件

<table><thead><tr><th>文件名</th><th>文件说明</th></tr></thead><tbody><tr><td>app.html</td><td>HTML 页面</td></tr><tr><td>qtloader.js</td><td>js 文件，调用 wasm 加载 Qt 程序的内容</td></tr><tr><td>app.js</td><td>js 文件，加载 Qt 程序</td></tr><tr><td>app.wasm</td><td>emscripten 编译生成的二进制文件</td></tr><tr><td>qtlogo.svg</td><td>网页图标</td></tr></tbody></table>

打包部署其实只需要这几个文件即可

app.html 为程序显示入口，app.html 通过 qtloader.js 和 app.js 加载 Qt 应用的内容进行显示，通过查看 app.html、qtloader.js、app.js 的内容，很容易看到他们之间的调用关系

我们使用浏览器打开 app.html 并不能直接运行，会提示 **“TypeError: Failed to fetch”**

![](https://i-blog.csdnimg.cn/blog_migrate/033ad15990cafe6a21970111f8696bde.png)

需要通过 web 服务器访问 app.html 才能正常启动

我们可以通过 python 快速启动一个 web 服务器进行显示，

1. 打开命令行工具，进入项目所在的目录，即 app.html 所在的目录

```
cd F:\PROJECT\build-TestSnowPage-Qt_5_15_2_WebAssembly-Release

```

2.python 开启 web 服务器

```
#端口号为8000
C:\Users\Mango\AppData\Local\Programs\Python\Python39\python.exe -m http.server 8000
```

![](https://i-blog.csdnimg.cn/blog_migrate/1e8666d7e4bded99fa7f3171b4f876d8.png)

3. 浏览器启动 app.html

```
E:/emsdk-main/upstream/emscripten/emrun.py --browser chrome --port 8000 F:\PROJECT\build-TestSnowPage-Qt_5_15_2_WebAssembly-Release\TestSnowPage.html

```

通过 Qt Creator 运行就是通过这个方式进行启动的

![](https://i-blog.csdnimg.cn/blog_migrate/a37bc503f2825ed58b1d026894e68f90.png)

Qt Creator 依赖 python 环境来启动的，如果事先没安装 python，在配置 emscripten 环境的时候安装有 python，可以按上面的方式用命令行启动

![](https://i-blog.csdnimg.cn/blog_migrate/a3138cb7ebf98de9d5bb0e847a28ee8a.png)

我们也可以通过部署其他 web 服务器，如 IIS、Apache、Nginx，只需要打包上面所说的五个文件进行部署，设置 app.html 为启动页面即可

#### IIS 部署 Qt for WebAssembly 应用

**1. 首先把编译生成的那五个文件拷贝出来放到一个单独的文件夹**

![](https://i-blog.csdnimg.cn/blog_migrate/1cb82070dae4060fadd437d971aabd3e.png)

**2.win10 下开启 IIS，百度出来很多，贴一个地址** [https://jingyan.baidu.com/article/ea24bc39c43d72da62b331ce.html](https://jingyan.baidu.com/article/ea24bc39c43d72da62b331ce.html)

**3. 打开 IIS 管理器，右键网站 > 添加网站**

![](https://i-blog.csdnimg.cn/blog_migrate/7a4bf67339caf5122710645d0d946e69.png)

**4. 物理路径选择第一步创建的目录，端口号换一个，80 已经被占用了**

![](https://i-blog.csdnimg.cn/blog_migrate/57c7b20b50a9d69e0fcef88e3978a56c.png)

**5. 设置默认文档，选择项目对应的 app.html**

![](https://i-blog.csdnimg.cn/blog_migrate/312be30e36367e7e8b8084d32e002b14.png)

![](https://i-blog.csdnimg.cn/blog_migrate/f0d82e74f0d500b0988b63ba89727de8.png)

![](https://i-blog.csdnimg.cn/blog_migrate/7a98cdfe1c984227ea5376e22b4e2404.png)

**6. 启动服务器**

![](https://i-blog.csdnimg.cn/blog_migrate/adebfc44f16617cf81fd239ddb69eb34.png)

**7. 在浏览器打开 [http://localhost:8000/](http://localhost:8100/) 就可以运行了**

![](https://i-blog.csdnimg.cn/blog_migrate/45b476354aad0bb6ad4822c9e5f442bd.png)

这样一个 Qt for WebAssembly 应用就部署起来了，其他网络互通的设备也能通过浏览器访问该站点

到这里，你是不是想着 Qt 是不是也能来写 web 应用了，哈哈哈哈

现在 web 前端技术这么成熟，通过 Qt 来写前端没有任何优势的啦，拿来耍耍可以，或者特殊情况使用

#### 中文不显示问题

运行一个带有中文的 Qt 程序我们会发现中文无法显示，变成一个个方框

![](https://i-blog.csdnimg.cn/blog_migrate/fd488e9b33f83d34acccc85e0866e1f2.png)

Qt for WebAssembly 目前还无法显示中文，这是因为无法使用浏览器或者说客户宿主机的字体，缺少相关的中文字体库，需要程序本身就带有中文的字体库才能显示

提供一个字体库下载，文泉驿字体库，不算大，5m 不到

网盘下载：[https://pan.baidu.com/s/1OYYxTWlE2r0VxBrJ1QvkNg](https://pan.baidu.com/s/1OYYxTWlE2r0VxBrJ1QvkNg)，提取码：cr63

加载中文字体库

```
QFontDatabase::addApplicationFont(":/ttf/WenQuanWeiMiHei-1.ttf");

```

加载之后就能正常显示中文啦

![](https://i-blog.csdnimg.cn/blog_migrate/f4d5ec84bd5ac8379aa2e34b364c68ad.png)

网上有的文章会介绍中文字体库的一个裁剪，因为中文字体库一般会比较大，静态链接会使程序变得很大，有需要的可以自行了解

#### 结语

Qt for WebAssembly 的介绍就靠一段落了，WebAssembly 作为一门新技术，已经被官方 Web 标准接纳，继 HTML、CSS 与 JavaScript 这 3 种语言后，WebAssembly 成为了第 4 种 Web 语言，相信以后会有更大的发展，让我们拭目以待