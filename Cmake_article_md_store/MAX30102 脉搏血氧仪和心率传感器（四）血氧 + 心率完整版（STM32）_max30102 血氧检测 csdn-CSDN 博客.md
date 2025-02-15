> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_50622833/article/details/121951942?spm=1001.2014.3001.5502)

#### 文章目录

*   [前言](#_7)
*   [一、先上测试结果](#_15)
*   *   [1. 测试步骤](#1_16)
    *   [2. 测试结果](#2_21)
    *   [3. 后续处理方法](#3_23)
*   [二、血氧饱和度](#_30)
*   *   [1. 原理](#1_31)
    *   [2. 计算方法](#2_33)
*   [三、源码获取（STM32 例程）](#STM32_75)

前言
--

相较于上一章，增加和改进的地方有：①增加了血氧饱和度测量；②改进了心率的代码。即中断采集完一段数据后才 “扔进 “函数进行处理，期间处理器可以做其它事情，但算法原理与上一章基本相同；③减少了代码量，较为简洁高效。

一、先上测试结果
--------

### 1. 测试步骤

*   手指接触到传感器，**等待 1-2s 后**串口输出信息；
*   前 2-3 个数据是**不稳定**的数据，**因为采集的是刚刚接触到传感器的数据**（**如下图红框部分**），**可以丢弃**；
*   输出的第 3 个数据以后**是比较稳定的数据**了；
*   手指**离开**传感器以后，**串口不再输出信息。再次接触传感器后重新开始测量**。

### 2. 测试结果

![](https://i-blog.csdnimg.cn/blog_migrate/ab2a2da4fc6f9140d75e76d5687a6c7e.png)![](https://i-blog.csdnimg.cn/blog_migrate/1a5bd7749a671f79e37245f65e081bdc.png)![](https://i-blog.csdnimg.cn/blog_migrate/707cca3cf792a8d3e83a6139aab78736.png)

### 3. 后续处理方法

可以看到就算是稳定的数据也有一定的起伏，可以来个滤波处理。

*   均值滤波：例如采集 10 组数据，去掉 2 个最大、最小值，剩下的取均值；
*   取中值，这个很简单就不说了。

还有其它很多的滤波方法。总之，可以**根据需要**进行数据处理。

二、血氧饱和度
-------

### 1. 原理

MAX30102 传感器上具有红光（660nm）和红外光（880nm）两个 LED，人体氧合血氧蛋白和非氧合血氧蛋白对这两个不同波长的光**吸收率**的差异较为明显。可以据此得出血氧饱和度。

### 2. 计算方法

官方提供的方法如下：

![](https://i-blog.csdnimg.cn/blog_migrate/dde31d092a61b089c08132dbdd03432f.png)

```
float max30102_getSpO2(float *ir_input_data,float *red_input_data,uint16_t cache_nums)
{
			float ir_max=*ir_input_data,ir_min=*ir_input_data;
			float red_max=*red_input_data,red_min=*red_input_data;
			float R;
			uint16_t i;
			for(i=1;i<cache_nums;i++)
			{
				if(ir_max<*(ir_input_data+i))
				{
					ir_max=*(ir_input_data+i);
				}
				if(ir_min>*(ir_input_data+i))
				{
					ir_min=*(ir_input_data+i);
				}
				if(red_max<*(red_input_data+i))
				{
					red_max=*(red_input_data+i);
				}
				if(red_min>*(red_input_data+i))
				{
					red_min=*(red_input_data+i);
				}
			}
			 R=((ir_max+ir_min)*(red_max-red_min))/((red_max+red_min)*(ir_max-ir_min));
			return ((-45.060)*R*R + 30.354*R + 94.845);
}

```

三、[源码](https://so.csdn.net/so/search?q=%E6%BA%90%E7%A0%81&spm=1001.2101.3001.7020)获取（STM32 例程）
----------------------------------------------------------------------------------------------

_关注下方公众号，回复 “**MAX30102V4**” 获取源码；若有疑问，请在公众号回复 “**交流群**”，进群一起讨论分享！_  
![](https://i-blog.csdnimg.cn/blog_migrate/1100c2018211def786e494e5c2786733.png)