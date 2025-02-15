> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_50622833/article/details/121582101?spm=1001.2014.3001.5502)

#### 文章目录

*   [前言](#_7)
*   [一、修正上一章产生的错误](#_12)
*   [二、FIR 滤波器设计](#FIR_17)
*   *   [1. 对采集的信号进行频谱分析](#1_18)
    *   [2. 滤波器设计](#2_21)
    *   [3. 滤波器仿真](#3_24)
*   [三、ARM_MATH 库实现（以 STM32 为例）](#ARM_MATHSTM32_27)
*   *   [实际效果测试](#_35)
    *   [滤波前](#_39)
    *   [滤波后](#_42)
*   [四、获取工程源码](#_45)

前言
--

数据经过采集之后，还会包含很多噪声，和一些不必要的成分。为了方便后续处理和更加精确地计算结果，需要对采集的信号进行滤波。数字信号处理属于较难学科，博主才疏学浅，如有不足之处敬请指正。

一、修正上一章产生的错误
------------

在上一章中，读取的 PPG 信号每个若干个周期会出现噪声，原因是读取时序和数据采集的时序对不上。使用中断管脚信号后错误消失。  
![](https://i-blog.csdnimg.cn/blog_migrate/954e21bafa80065cc1149e1bd3704edc.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/cd5d378df0322e5e7a50c55a0ca2ac8c.png)

二、FIR [滤波器](https://so.csdn.net/so/search?q=%E6%BB%A4%E6%B3%A2%E5%99%A8&spm=1001.2101.3001.7020)设计
--------------------------------------------------------------------------------------------------

### 1. 对采集的信号进行频谱分析

![](https://i-blog.csdnimg.cn/blog_migrate/1ce0b03bef370212364e7d83e2e949fe.png)  
可以看到 PPG 信号成分的频率主要集中在 0.5-2Hz 之间。为了消除个体差异，应该保留的频率成分 **0.5-3Hz**（也就是心率 30 次 / min - 180 次 / min）。

### 2. 滤波器设计

为了易实现，设计一个低通滤波器。参数如下：  
![](https://i-blog.csdnimg.cn/blog_migrate/fc618fd5253269a2f4149e641c138e71.png)

### 3. 滤波器仿真

可以看到，保留了 3Hz 以下的频率成分，滤除了 3Hz 以上的频率成分。  
![](https://i-blog.csdnimg.cn/blog_migrate/2407c6388abc040864421b7715014fcb.png)

三、ARM_MATH 库实现（以 STM32 为例）
--------------------------

将 ARM_MATH 库移植到工程中，上文设计的滤波器参数生成头文件导入工程中。关键的两个函数如下：

```
	arm_fir_init_f32(&S, NUM_TAPS,(float32_t *)&firCoeffs32LP[0], &firStateF32[0], blockSize);
	arm_fir_f32(&S,&input,&output,  blockSize);

```

这里不多介绍，需要了解的，可以参考**安富莱**的 DSP 教程。

### 实际效果测试

串口实时打印输出，红色的曲线为原始信号，蓝色的曲线为滤波后的波形。  
![](https://i-blog.csdnimg.cn/blog_migrate/62cd1d81df2924ab397f9a8780c5e697.png)

### 滤波前

![](https://i-blog.csdnimg.cn/blog_migrate/43f3759dfd109df650ed816e590e0830.png)

### 滤波后

![](https://i-blog.csdnimg.cn/blog_migrate/bec05c93ff4c9ec89366d0b5e0314064.png)

四、获取工程源码
--------

_关注下方公众号，回复 “**MAX30102V2**” 获取源码；若有疑问，请在公众号回复 “**交流群**”，进群一起讨论分享！_  
![](https://i-blog.csdnimg.cn/blog_migrate/91f7aecffb3ef1995e6d6254c8ee1ed8.png)