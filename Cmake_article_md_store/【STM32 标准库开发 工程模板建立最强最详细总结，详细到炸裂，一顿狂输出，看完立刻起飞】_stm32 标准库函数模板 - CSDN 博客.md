> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_52924633/article/details/136669654?spm=1001.2014.3001.5501)

#### 如何创建 STM32 工程模板创建与调试

*   [STM32 开发的三种模式](#STM32_1)
*   *   [寄存器模式](#_2)
    *   [标准库模式](#_5)
    *   [HAL 库模式](#HAL_7)
*   [如何创建标准库工程模板](#_10)
*   *   [创建 Start 目录](#Start_11)
    *   [创建 Library 目录](#Library_33)
    *   [创建 User 目录](#User_51)
    *   [创建 Hardware 文件夹](#Hardware_68)
    *   [创建 System 文件夹](#System_73)
*   [Keil5 配置](#Keil5_78)
*   *   [新建 Start 分组](#Start_89)
    *   [新建 Library 分组](#Library_110)
    *   [添加 User 分组](#User_116)
    *   *   [创建 main](#main_120)
    *   [Hardware 和 System 分组添加和说明](#HardwareSystem_127)
*   [添加宏定义头文件路径](#_131)
*   *   [代码测试](#_151)
*   [调试配置](#_160)
*   [总结](#_165)

STM32 开发的三种模式
-------------

### [寄存器](https://so.csdn.net/so/search?q=%E5%AF%84%E5%AD%98%E5%99%A8&spm=1001.2101.3001.7020)模式

寄存器模式：最底层的开发，运行速度最快。实际上也是使用了固件库，但是不是使用固件库的函数，而是使用了固件库的定义，包括[宏定义](https://so.csdn.net/so/search?q=%E5%AE%8F%E5%AE%9A%E4%B9%89&spm=1001.2101.3001.7020)，结构体定义。和 51 的开发差不多，但因为 32 的寄存器太多，实际开发手动配置大量寄存器极其耗费时间，同时在没有注释的情况下可读性差，所以较少使用。

### 标准库模式

标准库模式：基于寄存器进行了函数的封装，而由于函数封装以及内部大量的检查参数有效性的代码，运行速度相对于寄存器模式较慢。封装之后可以根据函数名字就能明白代码作用，容易记忆，使用方便，所以较多人使用。

### HAL 库模式

HAL 库模式：全称是 Hardware Abstraction Layer（抽象印象层），相比于标准库更加深入的封装，有句柄、回调函数等概念（ps: 有点类似 Windows 开发），因此相对于标准库模式有更好的可移植性（可在不同芯片的移植），但代价就是更多的性能损失。

如何创建标准库工程模板
-----------

### 创建 Start 目录

`Start`文件夹内存放的是`启动文件`，也即是 STM32 的程序就是从启动文件开始执行的。  
在项目中新建文件夹 Start

![](https://i-blog.csdnimg.cn/blog_migrate/d023bd55b9d2d3cf035aec8eab99cd7b.png)  
在固件库里找到并且复制固件库中的启动文件  
![](https://i-blog.csdnimg.cn/blog_migrate/5a6ab78c0118d26bf4e9e910294e5486.png)  
将文件粘贴到 Start 文件夹内  
![](https://i-blog.csdnimg.cn/blog_migrate/a2154f9e2047cc5655ff7fbbfc729180.png)  
再将固件库中`stm32f10x.h`和`system_stm32f10x.h`和`system_stm32f10x.c`也复制到 Start 文件夹内  
![](https://i-blog.csdnimg.cn/blog_migrate/f31cca64ceee3064ebd79d120bac97e3.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/dd48620818fa860bc6fb2ccf41f3038d.png)  
其中`stm32f10x.h`存放的是外设寄存器描述文件，他的作用就好比 51 单片机里的`#include <REGX52.h>`的作用一样是用来描述 stm32 有哪些寄存器和它相应的地址的。  
`system_stm32f10x.c`的作用是用来配置时钟，stm32 主频 72MHz 就是`system`文件里的函数配置的。

因为 STM32 是内核和内核外围设备组成的，而且内核设备寄存器描述和外围设备的描述文件设备不是在一起的，所以还需要在`Start`文件夹内添加一个内核寄存器描述文件`core_cm3.c和core_cm3.h`。  
![](https://i-blog.csdnimg.cn/blog_migrate/6414428405104d0d6016eabbbed11bd3.png)  
粘贴到 Start 文件夹内  
![](https://i-blog.csdnimg.cn/blog_migrate/7a6bffe8e7112c11a73e0a37ef3977cb.png)  
`到这里，工程Start必要文件就已经完成了`

### 创建 Library 目录

`Library库文件存放的是库函数`

新建 Library 文件夹  
![](https://i-blog.csdnimg.cn/blog_migrate/652ca31f096c1277887fd8c34f7c0d12.png)  
打开固件库的文件夹，打开 Library，STM32 标准外设驱动, src 路径下的. c 文件。  
![](https://i-blog.csdnimg.cn/blog_migrate/b7f42f3adb9ea613c4ba255c5dedb61b.png)  
打开并且复制  
![](https://i-blog.csdnimg.cn/blog_migrate/9eeb232ff23f3497a6d2f740c6864cf0.png)  
粘贴到`Library文件夹内`  
![](https://i-blog.csdnimg.cn/blog_migrate/0f24394640e934902fa131e6c17359dc.png)  
紧接着打开固件库的`inc文件夹`，**里面存放的是上面. c 声明，也即是. h 文件。**  
![](https://i-blog.csdnimg.cn/blog_migrate/6f16b65bf144cbce1a7a7f9b1725788c.png)  
全选复制  
![](https://i-blog.csdnimg.cn/blog_migrate/30fd61aa6412f9db40d9325950b49634.png)  
`同样粘贴到Library文件夹`  
![](https://i-blog.csdnimg.cn/blog_migrate/99a11f6a73ddff096728697fd34d9673.png)  
到这里，Library 文件夹内容已经结束

### 创建 User 目录

新建`User`文件夹  
![](https://i-blog.csdnimg.cn/blog_migrate/7e8eaf444e628424aa6e9d486a0095fd.png)

`User目录是用来存放main函数`  
但是一般还要再添加三个文件  
`stm32f10x_conf.h`和`stm32f10x_it.h`和`stm32f10x_it.c`

其中  
`stm32f10x_conf.h`是用来配置库函数头文件的包含关系的，另外还有用来参数检查的函数定义，这里是所有库函数都需要的。

`stm32f10x_it.h`和`stm32f10x_it.c`是用来存放中断函数的。

将三个文件复制到`User`文件夹内  
![](https://i-blog.csdnimg.cn/blog_migrate/5954c2f303e530faf061bae7b6acb08b.png)  
粘贴到 User 文件夹  
![](https://i-blog.csdnimg.cn/blog_migrate/6b0c4e877e2d5cfb1f9c60f575bfc976.png)

### 创建 Hardware 文件夹

新建`Hardware`文件夹  
`此文件夹用来写硬件模块的封装函数，模块化编程，便于管理使用。例如蜂鸣器函数的封装、LED函数的封装、光敏电阻模块的封装等等。`  
![](https://i-blog.csdnimg.cn/blog_migrate/6ed28289a605a26573e67fbea4501ced.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/f583afcbf423fed7e3e22fcac4545453.png)

### 创建 System 文件夹

该文件夹用来存储系统内部资源，例如延时函数等等。  
![](https://i-blog.csdnimg.cn/blog_migrate/f5a4fa76af11dd94628fbd38948baacf.png)  
至此，文件夹创立完成。

Keil5 配置
--------

接下来是 keil5 创建项目和添加上述的文件啦。  
打开 keil5, 选择新建工程  
![](https://i-blog.csdnimg.cn/blog_migrate/b52d8fdd5651fd9e4024dfd5854240fb.png)  
给工程起名字，点击确认

![](https://i-blog.csdnimg.cn/blog_migrate/01fafa2f4ca7280f58f1cd8e9a81eb98.png)  
选择开发板型号  
![](https://i-blog.csdnimg.cn/blog_migrate/c934ddac4743411a8fd76b1deb2d3fbf.png)  
点击确认，出现以下弹框，`x`掉即可  
![](https://i-blog.csdnimg.cn/blog_migrate/d0c63f4c9c04b6d7870a7297ce87d03e.png)

### 新建 Start 分组

![](https://i-blog.csdnimg.cn/blog_migrate/c683f06f44bbc4ef63a486cf8ba8eaa0.png)

![](https://i-blog.csdnimg.cn/blog_migrate/7aed9bb38928465f0a2861d56bed4302.png)  
添加分组后，会显示 New Group, 将其重命名为上面几个文件夹的内容，先点下 New Group, 过三秒再点击下它就能重新命名了。  
![](https://i-blog.csdnimg.cn/blog_migrate/30291e2934ff56f35d8c879e64974748.png)  
继续创建剩下的分组  
![](https://i-blog.csdnimg.cn/blog_migrate/997a879b0f18782b6f99c8ed7945eac7.png)  
右键`Start`选择 Add Existing Files to …  
![](https://i-blog.csdnimg.cn/blog_migrate/1854316f83c965a6041a1196dbf1b3a0.png)  
路径选择刚才上面创建的 Start 文件夹，并且选择添加文件夹的文件。  
![](https://i-blog.csdnimg.cn/blog_migrate/e8e638c8e56995c0acc89eb3e833e756.png)  
添加文件到 Start

![](https://i-blog.csdnimg.cn/blog_migrate/16faa7d43d3d06e3529e3902493dce08.png)  
添加后 Start 里面内容如下：  
![](https://i-blog.csdnimg.cn/blog_migrate/ce3d25925323b4b54181de46cc74820e.png)  
`特别注意：`**对于里面添加的 startp_stm32f10x_md.s，这里有时候不同的开发板选择不同后缀启动文件，这里给出选择的图，供大家参考选择**  
![](https://i-blog.csdnimg.cn/blog_migrate/823521e0683280feeda9db87afd882a7.png)  
**如果您的开发板是 256~512k，那在右键`Start`选择 Add Existing Files to … 这一步添加文件时就要添加`hd`后缀的文件了，也即是：**  
![](https://i-blog.csdnimg.cn/blog_migrate/dd73a8c69209d23a5b2490733ed21e06.png)

### 新建 Library 分组

这里添加分组已经能在上步添加完成，这里只需要添加已有的文件到此分组中即可。和上步相同。  
右键`Library`点击`Add Existing Files to ...`添加 Library 文件夹里`所有的`文件。  
![](https://i-blog.csdnimg.cn/blog_migrate/671c96a9ccb20eabb7501f7e21d6e633.png)  
此时 Library 分组内成功添加：  
![](https://i-blog.csdnimg.cn/blog_migrate/90c5f68dccdb54e31a0f87ef23b91909.png)

### 添加 User 分组

User 分组已经在上面添加完成，这里重复上面操作，`全选`，添加  
![](https://i-blog.csdnimg.cn/blog_migrate/ed9808741483b481d61627c0af7d5f1d.png)

#### 创建 main

仍在 User 分组内创建，右键`Add New Item To Group 'User'..`  
![](https://i-blog.csdnimg.cn/blog_migrate/00bff09b84255bf840dfd289c80c5158.png)  
`创建main.c文件`  
![](https://i-blog.csdnimg.cn/blog_migrate/8ec0a3ef6e943d6141a2923948d7ec86.png)  
此时看到 User 分组内便有了 main 函数  
![](https://i-blog.csdnimg.cn/blog_migrate/09ad97a4195bafcb7126fe557b05fb56.png)

### Hardware 和 System 分组添加和说明

分组添加在上方添加 Start 分组时已经叙述，这里不做过多解释。

这里的 Hardware 和 System 并不是 stm32 开发板必须的，其实添加到上方 User 配置好后，就已经完成模板的建立了。Hardware 是用来放硬件模块函数的的封装的。System 是用来放系统所需要的软件资源的。

添加宏定义头文件路径
----------

如果我们不添加头文件路径，那么系统便不知道头文件到底在那个分组，哪个位置。  
宏定义：`USE_STDPERIPH_DRIVER`  
![](https://i-blog.csdnimg.cn/blog_migrate/7308249f5f17320ff3d0a8f3c3ba2813.png)

添加所有分组，  
![](https://i-blog.csdnimg.cn/blog_migrate/c0cbba806fdbd1e1ddcc670c63661bf9.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/d3c98e6746ad512a9ad64f40fb76e812.png)

添加进来  
![](https://i-blog.csdnimg.cn/blog_migrate/270efad951210f011292fdc219b5eff0.png)  
然后重复上述操作再添加其他的 User、Library、System、Hardware 文件夹。  
![](https://i-blog.csdnimg.cn/blog_migrate/c8165090bf275a2b1065b5fbe36a4cd4.png)  
至此，头文件路径添加成功。

**特别注意：** _**`创建一个目录一定要像上述一样添加到魔术棒的c/c++的Includepath路径里面。`**_  
来到 main.c 右键空白处，引入头文件

![](https://i-blog.csdnimg.cn/blog_migrate/7774d60807191f8622869029ec0a4bb0.png)

### 代码测试

![](https://i-blog.csdnimg.cn/blog_migrate/9dd35c4d4a166122fc1f9e65c67d860c.png)  
满怀期待想看看是否成功却在编译后会发现很多错误  
![](https://i-blog.csdnimg.cn/blog_migrate/eb358dbe778649ee3151fb7dc4fd1e94.png)  
这使能因为版本库版本不对应，我们再次点击 “魔术棒”->Target->verson5  
![](https://i-blog.csdnimg.cn/blog_migrate/23fc67f20acb6210dce8b6ac0eccff8b.png)

`再次编译：`  
![](https://i-blog.csdnimg.cn/blog_migrate/fc7fa67ec3fc34421eee9461db9fc9ec.png)

调试配置
----

![](https://i-blog.csdnimg.cn/blog_migrate/98f44c7acf933556aba1ee3a5010adba.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/2c73369d5ce8e2e7f02ecda4cee0207d.png)  
最后一步：  
![](https://i-blog.csdnimg.cn/blog_migrate/334ef525f16573a87c246d9cdbd4543b.png)更改完记得点`确定`才能生效。

总结
--

![](https://i-blog.csdnimg.cn/blog_migrate/e80ec268a4dc719661643ce6b2f177fb.png)

下次想开发其他模块时不用再上述操作了，只需要复制 Start 文件夹、Library 文件夹、User 文件夹到新工程即可使用。一劳永逸啊！！！

**ok 了，写了这么多，小舞就问你能不能给个支持鸭？真是太不容易了。  
最强模板建立过程，耗费了大量精力。**