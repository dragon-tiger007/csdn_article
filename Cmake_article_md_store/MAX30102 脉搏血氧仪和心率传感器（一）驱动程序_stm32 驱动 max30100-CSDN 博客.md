> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_50622833/article/details/121040726)

#### 文章目录

*   [前言（文末获取源码）](#_8)
*   [一、PPG 信号简介](#PPG_16)
*   [二、MAX30102 简介](#MAX30102_19)
*   *   [1. 总体结构](#1_20)
    *   [2. 寄存器](#2_24)
*   [三、使用步骤](#_29)
*   *   [1.I2C 通信](#1I2C_30)
    *   [2.MAX30102 初始化](#2MAX30102_52)
    *   [3. 读取数据](#3_91)
    *   [4. 数据分析](#4_109)
*   [四、存在的问题（已修正，详情在下一章）](#_120)
*   [获取工程源码](#_130)

前言（文末获取源码）
----------

Maxim MAX30102 传感器是一款集成脉搏血氧仪和心率监测器模块。MAX30102 包括内部 LED、光电探测器、光学元件以及低噪声电子元件，具有环境光反射特征。该高灵敏度器件由 1.8V 单电源供电，其内部 LED 由独立的 5.0V 电源供电。通过标准的 I2C 兼容接口进行通信。该传感器可通过软件来关断电源，待机模式下的电流消耗量几乎为零。

一、PPG 信号简介
----------

简单来说 PPG 信号就是光照照射在皮肤组织上时，由于**血液流动**造成对**光的吸收率**不同，而其它组织（例如骨骼，肌肉）对光的吸收率基本不变，导致光的**反射率**随血液流动情况改变的信号。如下图所示，**直流信号 DC** 反映的是**组织、骨骼、肌肉、静脉血**等等，**交流信号 AC** 则是反映了**动脉血**流动情况。根据 AC、DC 信号能够计算出**心率、血氧**，_**详细的计算公式在下一章进行介绍**_。  
![](https://i-blog.csdnimg.cn/blog_migrate/9c80c1c836d6b4576144905c4ca25ad4.png)

二、MAX30102 简介
-------------

### 1. 总体结构

![](https://i-blog.csdnimg.cn/blog_migrate/a1a81bf19a564291429704e29bf2ad38.png)  
可以看到，MAX30102 结构包括两个光电二极管（**RED：红光 IR：红外**）、接收器、ADC 通道、数字滤波器、数据寄存器和 I2C 通信模块。

### 2. 寄存器

寄存器包括三大部分：状态寄存器、设置寄存器、温度寄存器。另外还有版本号和设备 ID 的寄存器，如下图。  
![](https://i-blog.csdnimg.cn/blog_migrate/a7bf85545c525d423fe81f6e69ec26ea.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/6d2406a0fa081b42caef09abc9f752ee.png)

三、使用步骤
------

### 1.I2C 通信

和一般的 I2C 通信方式不同，MAX30102 的读写时序如下  
![](https://i-blog.csdnimg.cn/blog_migrate/bbdeed4a02de7b95c9d962020e6f2a55.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/899de851b6ad59b5d0f35d4125083cfd.png)  
程序如下：

```
void max30102_i2c_write(uint8_t reg_adder,uint8_t data)
{
	uint8_t transmit_data[2];
	transmit_data[0] = reg_adder;
	transmit_data[1] = data;
	i2c_transmit(transmit_data,2);
}


void max30102_i2c_read(uint8_t reg_adder,uint8_t *pdata, uint8_t data_size)
{
		uint8_t adder = reg_adder;
		i2c_transmit(&adder,1);
		i2c_receive(pdata,data_size);
}

```

### 2.MAX30102 初始化

对照 datasheet 设置**采样率，工作模式，led 电流**等等参数的设置。下面作为参考，实际使用的效果还是很好的。

```
void max30102_init(void)
{ 
	uint8_t data;
	
	max30102_i2c_write(MODE_CONFIGURATION,0x40);  //reset the device
	
	delay_ms(5);
	
	max30102_i2c_write(INTERRUPT_ENABLE1,0xE0);
	max30102_i2c_write(INTERRUPT_ENABLE2,0x02);  //interrupt enable: FIFO almost full flag, new FIFO Data Ready,
																						 	//                   ambient light cancellation overflow, power ready flag, 
																							//						    		internal temperature ready flag
	
	max30102_i2c_write(FIFO_WR_POINTER,0x00);
	max30102_i2c_write(FIFO_OV_COUNTER,0x00);
	max30102_i2c_write(FIFO_RD_POINTER,0x00);   //clear the pointer
	
	max30102_i2c_write(FIFO_CONFIGURATION,0x0F); //FIFO configuration: sample averaging(1),FIFO rolls on full(0), FIFO almost full value(15 empty data samples when interrupt is issued)  
	
	max30102_i2c_write(MODE_CONFIGURATION,0x03);  //FIFO configuration:SpO2 mode
	
	max30102_i2c_write(SPO2_CONFIGURATION,0x26); //SpO2 configuration:ACD resolution:15.63pA,sample rate control:100Hz, LED pulse width:215 us 
	
	max30102_i2c_write(LED1_PULSE_AMPLITUDE,0x2f);
	max30102_i2c_write(LED2_PULSE_AMPLITUDE,0x2f); //LED current
	
	max30102_i2c_write(TEMPERATURE_CONFIG,0x01);  //temperture
	

	
	max30102_i2c_read(INTERRUPT_STATUS1,&data,1);
	max30102_i2c_read(INTERRUPT_STATUS2,&data,1);//clear the flag
}

```

### 3. 读取数据

采用轮询的方式，未用到引脚中断信号。用过中断引脚的信息，发现读出来的数据波形不太好，用 10-15ms 延时读取是比较好的。

```
void max30102_fifo_read(uint32_t *data)
{
    uint8_t receive_data[6],temp_data=0;
	
		max30102_i2c_read(INTERRUPT_STATUS1,&temp_data,1);
	
		while((temp_data&0x40)!=0x40)
		{
			max30102_i2c_read(INTERRUPT_STATUS1,&temp_data,1);
		}
		max30102_i2c_read(FIFO_DATA,receive_data,6);
    data[0] = ((receive_data[0]<<16 | receive_data[1]<<8 | receive_data[2]) & 0x03ffff);
    data[1] = ((receive_data[3]<<16 | receive_data[4]<<8 | receive_data[5]) & 0x03ffff);
}

```

### 4. 数据分析

max30102 的灵敏度是很高的，**手指未接近和接触后**如下图。  
![](https://i-blog.csdnimg.cn/blog_migrate/220bc3d00099072985e4f683e6bda453.png)

红光和红外光**两路信号**，似乎红外光灵敏度更高一些。  
![](https://i-blog.csdnimg.cn/blog_migrate/140cccc1cfe3d056f938bfea51a48cf3.png)  
PPG 信号波形也是比较漂亮的。  
![](https://i-blog.csdnimg.cn/blog_migrate/77e4a52a2ed12941d0945f055e5e3c03.png)

四、存在的问题（已修正，详情在下一章）
-------------------

*   每过若干个周期会出现莫名的噪声，原因未知，可能是硬件问题，又或者是读取数据方式出现了问题，待解决。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/758e188b30744f1e0ac70d84f8ec6b67.png)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/b0831254b75567e4d0e93cb878497bee.png)

**下一章将讨论心率、血氧的计算**

获取工程源码
------

_关注下方公众号，回复 “**MAX30102**” 获取源码；若有疑问，请在公众号回复 “**交流群**”，进群一起讨论分享！_  
![](https://i-blog.csdnimg.cn/blog_migrate/092249c788bceea9ae1bbff7ffd58137.png)