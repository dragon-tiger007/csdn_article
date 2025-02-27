> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/Na0H_/article/details/140360343)

**目录**

[前言](#t0)

[一、为什么要移植 OLED？](#t1)

[二、对移植过程的思考](#t2)

[2.1 江协代码的粗略分析](#t3)

[2.1.1 对第一部分的江协代码进行分析](#t4)

[2.1.2 对第二部分的江协代码进行分析](#t5)

[2.2 将江协代码与 HAL 的区别](#t6)

[三、进行移植](#t7)

[3.1 CubeMx 配置](#t8)

[3.1.1 选择芯片型号](#t9)

[3.1.2 System Core 配置](#t10)

[3.1.3 时钟树设置（Clock Configuration）](#t11)

[3.1.4 Project Manager 设置](#t12)

[3.2 代码编写](#t13)

[3.2.1 将程序设置为自启动模式](#t14)

[3.2.2 创建自己的文件夹](#t15)

[3.2.3 开始移植，修改江协代码](#t16)

[四、移植后的效果图](#t17)

[总结](#t18)

[参考文献](#t19)

前言
--

文章为记录个人学习 [HAL 库](https://so.csdn.net/so/search?q=HAL%E5%BA%93&spm=1001.2101.3001.7020)的成果，使用的芯片型号为：**STM32F103C8T6**，使用辅助软件：**CubeMx**，这篇文章是对江协标准库 OLED 进行移植，所用到的 OLED 文件来源于江协。参考的视频来源于 B 站 up 主野生绿波电龙，视频的 BV 号为：BV1jr421L7kU

以及 B 站 up 主江协科技，视频的 BV 号为：BV1th411z7sn

以及 B 站 up 主成电应电科协，视频 BV 号为：BV1y7411m7gg

一、为什么要移植 OLED？
--------------

OLED 作为单片机数据显示的一个外设，我们可以把各类参数显示在 OLED 上面，就比如可以把

STM32F103C8T6 与 OpenMv 通信过程的关键信息显示在 OLED 上，这样我们可以通过单片机接收到的数据对其进行下一步的判断、处理；也可以作为检验代码是否出错的排查工具。因此，OLED 对于单片机而言，其功能十分重要。

二、对移植过程的思考
----------

### 2.1 江协代码的粗略分析

打开江协的代码，我们可以发现关于 OLED 有三个函数，分别为：OLED.c，OLED.h，OLED_Font.h

![](https://i-blog.csdnimg.cn/direct/312c55e196084cc295ad69c6b5fce95f.png)

查看江协的 OLED.c 的代码，我们会发现他是用软件模拟 IIC 通信来进行的，我们可以将要修改代码分为两个部分：

![](https://i-blog.csdnimg.cn/direct/88f5790aeb264521b4f48595713f1523.png)

第一部分是模拟 IIC 中定义时钟总线和数据总线，第二部分是引脚的初始化操作。

#### 2.1.1 对第一部分的江协代码进行分析

这是一个 define 的宏定义，是对 GPIO 口进行写入的操作。这时候我看到了 “**BitAction**”，这是啥玩意？字面意思是 bit 行动？我进行了如下操作。

![](https://i-blog.csdnimg.cn/direct/854bb03b871b457e8d75b9794c3ab930.png)

对 “BitAction” 进行选中，找到它定义的地方，可以发现它是一个结构体：

![](https://i-blog.csdnimg.cn/direct/45d6075aab7e48679d753c1f9431fdf0.png)

因此我们就可以知道它的作用了，它的作用是对端口进行使能或不使能。而 BitAction(x) 则可以通过 x 的取值来选择其为使能或不使能。

#### 2.1.2 对第二部分的江协代码进行分析

第二部分倒是更方便理解，由于其是对 GPIO 口的初始化，我们可以在之后的 CubeMx 中直接对其进行配置。值得注意的是 “GPIO_Speed” 的部分，江协将单片机端口的速度设置成了 50MHZ，对应 HAL 库的速度选值我们可以参考下表（来源于成电应电科协视频 P43 的 4 分 15 秒处）

![](https://i-blog.csdnimg.cn/direct/018e726431bd4ea6b1b3d3c367838178.png)

因此在配置引脚输出速度时可以选用 “MEDIUM”

### 2.2 江协代码与 HAL 库的区别

在江协代码的第一部分中，我们会发现 “**GPIO_WriteBit(GPIOB, GPIO_Pin_8, (BitAction)(x))**” 的这种格式与 HAL 库的格式不相符，HAL 对于 GPIO 口写入的格式应该为 **“HAL_GPIO_WritePin(GPIOx,GPIO_PIN_x,GPIO_PIN_SET（或者是 GPIO_PIN_RESET）)**”，因此我们在移植的时候首先要对其进行改写。

那么在 HAL 库中有没有像标准库那样 “BitAction” 的结构体对 GPIO 端口进行使能或不使能操作呢？在标准库中，我们发现 “BitAction” 所存放的位置是 “stm32f10x_gpio.h” 中。

![](https://i-blog.csdnimg.cn/direct/f322eb1ed64845bba1dcffffe8ffdda1.png)

那么在 HAL 库中有没有类似的文件呢？我去翻了翻 HAL 的 gpio 口的文件，发现有和 “BitAction” 类似功能的函数，它在 HAL 库中名字为 “GPIO_PinState”，被存放在“stm32f1xx_hal_gpio.h” 中。

![](https://i-blog.csdnimg.cn/direct/2a6476c2ee3c4d6d955e9fbdef08ec34.png)

因此我们在 HAL 库中要把 “BitAction” 替换为“GPIO_PinState”。

三、进行移植
------

### 3.1 CubeMx 配置

#### 3.1.1 选择芯片型号

打开 [CubeMx 软件](https://so.csdn.net/so/search?q=CubeMx%E8%BD%AF%E4%BB%B6&spm=1001.2101.3001.7020)：

![](https://i-blog.csdnimg.cn/direct/0a8324a8f6084c2fb445a9ad96442254.png)​

在搜索框中输入芯片型号 “STM32F103C8”，并选择该型号：

![](https://i-blog.csdnimg.cn/direct/b7b5a668a426459d86ea238eafdb56e7.png)​

#### 3.1.2 System Core 配置

点击 “SYS”，将 Debug 选择为 “Serial Wire”，其余地方保持默认设置即可，不用更改。

![](https://i-blog.csdnimg.cn/direct/6342c1ed85ff43e78d4109cc4e32df8a.png)​

点击 “RCC”，将“High Speed Clock（HSE）” 选择为“Crystal/Ceramic Resonator”，其余地方保持默认设置即可，不用更改。

![](https://i-blog.csdnimg.cn/direct/a6eb7ce6d76f4b998d5c0b095aa9e68d.png)​

点击芯片的 “PB8” 引脚，选择“GPIO_Output”（PB9 同理）

![](https://i-blog.csdnimg.cn/direct/a35fed919efb4d93ad2c6722b4a62b8d.png)

点击 "GPIO"，再点击 "PB8"，将 “GPIO mode” 设置为开漏输出模式（Output Open Drain），速度选择 “Medium”，“PB9” 的设置与 “PB8” 的设置一样，这里就不重复赘述了。

![](https://i-blog.csdnimg.cn/direct/06a93621106b46bf9bc24d8c84582e00.png)

#### 3.1.3 时钟树设置（Clock Configuration）

点击 “Clock Configuration”，将框中的“8” 修改为“72”，然后**按下回车键**，

![](https://i-blog.csdnimg.cn/direct/652cb9319f414fc0aeb9e0832fc75323.png)​

在弹出来的框中选择 “OK”

![](https://i-blog.csdnimg.cn/direct/a1d7952d899d4e209caf6177d2f2211e.png)​

#### 3.1.4 Project Manager 设置

输入项目名称 “OLED”，将“EWARM” 改为“MDK-ARM”

![](https://i-blog.csdnimg.cn/direct/3b44ae0bc5364d8fbfaf3aa6f43b692e.png)​

版本更改为 V5

![](https://i-blog.csdnimg.cn/direct/a6dfd982927e4df083740d80efe1f801.png)​

点击 “Code Generator”，勾选图片第二个框中的内容（这是用于生成对应的. c 和. h 文件的），然后点击“GENERATE CODE” 生成代码![](https://i-blog.csdnimg.cn/direct/737a40544a044bd4b49fbbc1f17bf8da.png)​

点击 “Open Project” 打开 keil5

![](https://i-blog.csdnimg.cn/direct/3411ea529d28420b98e655dd55e375cd.png)​

### 3.2 代码编写

#### 3.2.1 将程序设置为自启动模式

点击菜单栏中的 “魔术棒”

![](https://i-blog.csdnimg.cn/direct/92133563c8c24c45b6684efde2650438.png)​

点击 “Debug”，然后再点击 “Settings”

![](https://i-blog.csdnimg.cn/direct/8ce1a9b999894d9baef9b621ed28cbe0.png)​

点击 “Flash Download”，勾选选项 “Reset and Run”，再点击 “确定”

![](https://i-blog.csdnimg.cn/direct/d7943873b41f438ba7c0526e4554c156.png)​

#### 3.2.2 创建自己的文件夹

创建自己的文件夹可以区别于 CubeMx 生成代码时创建的文件夹，更为方便管理。

打开文件路径，创建一个新文件夹 “MyUsed”

![](https://i-blog.csdnimg.cn/direct/470eb27cca65425e9e419b8431684070.png)

文件路径位置每个人不一定相同，可以根据自己的 CubeMx 中的这个位置来定位（这里我的文件路径的位置是在 F 盘，STM32 文件夹下的 CubeMx 文件夹，CubeMx 文件夹下的 test 文件夹，test 文件夹下的 OLED 文件夹，请根据自己的位置来定位文件夹路径）：

![](https://i-blog.csdnimg.cn/direct/c62ffc44c1dd4a2dbc2215d3d1e674ac.png)

把江协的三个关于 OLED 的代码复制到 “MyUsed” 文件夹下

![](https://i-blog.csdnimg.cn/direct/eef91024792f40cb8c4129bab2675b60.png)

打开 keil5，按照下面四步进行操作

![](https://i-blog.csdnimg.cn/direct/5512bc1803a34310962f98ada73a59b9.png)

在刚刚创建好的 “MyUsed” 文件夹，右击鼠标

![](https://i-blog.csdnimg.cn/direct/18264c7f8c714b32aebbf7f8087daacd.png)

点击 “向上一级”

![](https://i-blog.csdnimg.cn/direct/312622e060044024a3ed7e72443b1123.png)

把文件类型改为 “All files”，之后打开 “MyUsed”

![](https://i-blog.csdnimg.cn/direct/de1fd6477a8140608415a1166709c84d.png)

按住鼠标左键，将三个 OLED 代码全部选中后点击 “Add”，添加完成后点击“Close” 关闭标签页

![](https://i-blog.csdnimg.cn/direct/73cf701aefc644d98fefef8394593618.png)

此时三个文件就被加入到 Keil5 所创建的文件夹 “MyUsed” 中了，但它的路径还未添加，keil5 无法识别，接下来是添加路径：

点击魔术棒，再点击 “C/C++”，然后点击三个小点

![](https://i-blog.csdnimg.cn/direct/b6c14ccff0b04766903240d89104261c.png)

点击文件夹图标，再点击三个小点

![](https://i-blog.csdnimg.cn/direct/f9a16bddcb6c4c658b909d8bc2b87722.png)

返回文件的上一级

![](https://i-blog.csdnimg.cn/direct/281456978c9c4ae7afdd50a8e455a74d.png)

点击 “MyUsed”，选中后点击 “选择文件夹”

![](https://i-blog.csdnimg.cn/direct/89a8d22b4217454599b4db3b883d97d3.png)

点击 “OK”，再点击 “OK”

![](https://i-blog.csdnimg.cn/direct/1f81ae4f6fe7434e926d44ed82491d95.png)

自此，创建自己的文件夹就完成了。

#### 3.2.3 开始移植，修改江协代码

打开 keil5 左侧的 “MyUsed” 文件夹中的 OLED.c，将头文件处 “#include "stm32f10x.h" 改为“#include gpio.h”；将“GPIO_WriteBit” 替换为 “HAL_GPIO_WritePin”；将“GPIO_Pin” 改为“GPIO_PIN”；将“BitAction" 替换为“GPIO_PinState”。结果如下所示：

```
#include "gpio.h"
#include "OLED_Font.h"
 
/*引脚配置*/
#define OLED_W_SCL(x)		HAL_GPIO_WritePin(GPIOB,GPIO_PIN_8,(GPIO_PinState)(x))
#define OLED_W_SDA(x)		HAL_GPIO_WritePin(GPIOB,GPIO_PIN_9,(GPIO_PinState)(x))
```

对引脚初始化函数 “void OLED_I2C_Init(void)” 进行修改，将标准库对引脚的初始化改为 CubeMx 自动生成的引脚初始化，即：

```
/*引脚初始化*/
void OLED_I2C_Init(void)
{
	MX_GPIO_Init();
	
	OLED_W_SCL(1);
	OLED_W_SDA(1);
}
```

添加 OLED 的头文件，打开 main.c 文件，在 “/* USER CODE BEGIN Includes */” 和“ /* USER CODE END Includes */ ”之间添加头文件 #include "OLED.h"

```
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "gpio.h"
 
/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "OLED.h"
/* USER CODE END Includes */
```

初始化 OLED，在 “/* USER CODE BEGIN SysInit */” 和“ /* USER CODE END SysInit */ ”之间添加“OLED_Init();”

```
/* Configure the system clock */
    SystemClock_Config();
 
/* USER CODE BEGIN SysInit */
    OLED_Init();
/* USER CODE END SysInit */
```

开始测试在 OLED 上显示数据，在 “/* USER CODE BEGIN 2 */” 和“ /* USER CODE END 2 */ ”之间写入代码：

```
/* Configure the system clock */
    SystemClock_Config();
 
/* USER CODE BEGIN SysInit */
    OLED_Init();
/* USER CODE END SysInit */
 
/* Initialize all configured peripherals */
    MX_GPIO_Init();
 
/* USER CODE BEGIN 2 */
    OLED_ShowChar(1, 1, 'A');
    OLED_ShowString(1, 3, "HelloWorld!");
    OLED_ShowNum(2, 1, 12345, 5);
    OLED_ShowString(3, 1, "NaOH");
/* USER CODE END 2 */
```

至此，OLED 的移植就结束了。

四、移植后的效果图
---------

![](https://i-blog.csdnimg.cn/direct/6020a8ee6b694b2fbb1b7b6f5d4816bf.jpeg)  
 

总结
--

移植完 OLED 后可以用于之后的串口通信部分，来检查数据的收发是否有问题，对我而言还是挺有帮助的，这也正是我移植 OLED 的目的之一。OLED 相较于单片机就好似 printf 相较于 C 语言，如果没有 printf，C 语言的学习将会很困难，你无法对代码进行测试，OLED 的作用就在于此。

参考文献
----

[1] 成电应电科协. (2020, March 24). 【STM32 教程】入门教程 (基于 HAL 库 + CubeMX+MDK-ARM) [Video]. Retrieved from [https://www.bilibili.com/video/BV1y7411m7gg?p=43](https://www.bilibili.com/video/BV1y7411m7gg?p=43 "https://www.bilibili.com/video/BV1y7411m7gg?p=43")

[2] breeze0321. (2022, April 4). STM32F103c8t6 - CubeMX 快速实现时钟配置 - 最大 72M 时钟的设定及实验测试. CSDN 博客. <[https://blog.csdn.net/weixin_43604457/article/details/123262730](https://blog.csdn.net/weixin_43604457/article/details/123262730 "https://blog.csdn.net/weixin_43604457/article/details/123262730")>

[3] 野生绿波电龙. (2024, May 19). P3. 移植江协 OLED 显示屏【HAL 库复现江协全部 STM32 例子合集】 [Video]. Retrieved from [https://www.bilibili.com/video/BV1jr421L7kU/?spm_id_from=333.788](https://www.bilibili.com/video/BV1jr421L7kU/?spm_id_from=333.788 "https://www.bilibili.com/video/BV1jr421L7kU/?spm_id_from=333.788")

[4] 江协科技. (2021, July 29). STM32 入门教程 - 2023 版 细致讲解 中文字幕 [Video]. Retrieved from

[https://www.bilibili.com/video/BV1th411z7sn?p=10](https://www.bilibili.com/video/BV1th411z7sn?p=10 "https://www.bilibili.com/video/BV1th411z7sn?p=10")