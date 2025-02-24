> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_54784198/article/details/126732377)

前言
--

为了方便自己，于是方便了大家。

一、获取 stm32Cube 包
----------------

1 ——打开下面的链接  
[ST 官网链接](https://www.st.com/zh/embedded-software/stm32cube-mcu-mpu-packages.html#)  
![](https://i-blog.csdnimg.cn/blog_migrate/0100eee7ae4582f37e0ca9c58ac62563.png)  
2——下载 stm32 标准外设库  
我要用 STMCubeG413rbt6，所以我选择 STM32CubeG4 系列  
![](https://i-blog.csdnimg.cn/blog_migrate/6727ee363553f970e8a2ec294619b393.png)  
点击![](https://i-blog.csdnimg.cn/blog_migrate/2beafc5c39e74f8abf9bee4b84c662b9.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/c5b25c931fd6e8a33f60023fe3eff91e.png)  
点击![](https://i-blog.csdnimg.cn/blog_migrate/e8b2326ae09ba00b9264c32ccd07686f.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/837fc2f0df8ec0de42f45dab5d365a4e.png)  
点击【1.5.0】 后会弹出一个协议  
![](https://i-blog.csdnimg.cn/blog_migrate/aeb69a53ee88d955e05c84f789931a1c.png)  
点击【接受】，下载成功  
当你接受后，如果你是第一次的话，看下面  
![](https://i-blog.csdnimg.cn/blog_migrate/005b51c90d87e83c1ba8b38368c4ce80.png)  
注意当你接受后，如果你是第一次的话，不会直接下载。它会弹出一个框，你只需要把邮箱号给输入了，然后再打开邮箱验证一哈  
![](https://i-blog.csdnimg.cn/blog_migrate/11f3d86bdc0aeb73a7bfba4d7705400b.png)

二、安装固件库
-------

这里有三种方法，经过尝试，有两种可用。

### 第一种：简单快洁，直接在 stm32CubeMX 上安装。

![](https://i-blog.csdnimg.cn/blog_migrate/849b44cf48c99909106b11c1ac34a752.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/dffd51fda91d25fbc2a199078b690b9d.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/820f7e7b46d49262d7c03911d384030a.png)  
然后客官请稍等片刻，![](https://i-blog.csdnimg.cn/blog_migrate/2bf4ffdf1401998eac4dba461e3c540d.png)

### 第二种：方便直接。原理上突破。

通过上面的第一种方法后，我发现个奇妙的事。添加成功？那么添加的东西在哪儿？  
![](https://i-blog.csdnimg.cn/blog_migrate/fa518c7aa1a055332d11096526b92428.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/9915f10188ce352e6dd25217684e34ee.png)  
我们看看里面放了什么  
![](https://i-blog.csdnimg.cn/blog_migrate/16202743f8bc625272504151a728649b.png)  
同理，方法二就是直接解压到这里，注意文件夹名字。  
![](https://i-blog.csdnimg.cn/blog_migrate/b68e965b33d320dd2d93705f616bcbd8.png)  
好了，验证哈成功没  
![](https://i-blog.csdnimg.cn/blog_migrate/2bf4ffdf1401998eac4dba461e3c540d.png)