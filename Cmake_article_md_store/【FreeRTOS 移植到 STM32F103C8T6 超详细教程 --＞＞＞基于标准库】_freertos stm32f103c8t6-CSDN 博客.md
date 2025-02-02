> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_52924633/article/details/139162499?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522317e062332950dda4935bcb4c2ebb1f4%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=317e062332950dda4935bcb4c2ebb1f4&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-139162499-null-null.142^v101^control&utm_term=stm32f103c8t6%E7%A7%BB%E6%A4%8Dfreertos&spm=1018.2226.3001.4187)

#### 移植教程

*   [FreeRTOS 简介](#FreeRTOS_1)
*   *   [FreeRTOS 介绍](#FreeRTOS__3)
    *   [FreeRTOS 优点](#FreeRTOS_7)
*   [FreeRTOS 移植](#FreeRTOS_12)
*   *   [FreeRTOS 下载](#FreeRTOS__13)
    *   [FreeRTOS 目录结构](#FreeRTOS_17)
    *   [移植开始](#_36)
    *   [Keil5 打开工程](#Keil5_74)
    *   [修改 FreeRTOSConfig.h 文件](#FreeRTOSConfigh_92)
    *   *   [修改 stm32f10x_it.c](#stm32f10x_itc_363)
*   [测试](#_396)
*   *   [FreeRTOS 闪烁第一颗小灯](#FreeRTOS_397)

FreeRTOS 简介
-----------

### FreeRTOS 介绍

FreeRTOS 是市场领先的面向微控制器和小型微处理器的实时操作系统 (RTOS)，与世界领先的芯片公司合作开发，现在每 170 秒下载一次。FreeRTOS 通过 MIT 开源许可免费分发，包括一个内核和一组不断丰富的 IoT 库，适用于所有行业领域。FreeRTOS 的构建突出可靠性和易用性。

省却这些繁琐的苦涩难懂的概念，我们来看个简单的：比如你现在用的裸机编程，那么你会用 while(1) 来监测，这样会导致一个现象：你在 while(1) 闪烁一颗灯后，在 while(1) 中又想实现手动控制舵机，那么加入在点亮一颗灯的函数后有很多代码才轮到控制舵机的函数，当你点击舵机旋转按钮，有肯能就会导致舵机按钮按过后，好久才会有反应。这就是弊端，不呢能够及时的响应。而如果使用 FreeRTOS，则可完成实时的操作，把这几个操作看成一个一个任务，人物之间互不打扰的，这样就实现了实时的效果。

### FreeRTOS 优点

*   可移植：对于不同的开发板，均支持移植，提高了通用性。
*   开源 ：开源能够节省很多成本，因为开源免费。
*   轻量级
*   可定制

FreeRTOS 移植
-----------

### FreeRTOS 下载

点击 [FreeRTOS 官网下载连接](https://www.freertos.org/zh-cn-cmn-s/a00104.html)下载 FreeRTOS  
![](https://i-blog.csdnimg.cn/blog_migrate/bdd0af66d53c70ad681c0fab25d604c9.png)  
下载完成后解压至就合适的位置，

### FreeRTOS 目录结构

![](https://i-blog.csdnimg.cn/blog_migrate/4fe57c86b8344bf28326ba2ead9bafd5.png)  
但注意，只有上面的圈中的第一个才有有用，但也不要把删除其他的。

打开圈中的 FreeRTOS 的文件目录  
![](https://i-blog.csdnimg.cn/blog_migrate/95dc3cc32d1b96118c4fedbf0ba650ac.png)  
打开存放内核的 Source，查看有哪些文件即文件夹：  
![](https://i-blog.csdnimg.cn/blog_migrate/b12d666af2fa445810b5d30e636149c1.png)  
在`portable文件夹内`有如下文件夹，选中我们所需的（圈中的即为我们所需的）

![](https://i-blog.csdnimg.cn/blog_migrate/eb95678febe179e854f7304f9f35e453.png)  
在`MenMang文件夹`内有如下文件，我们要用到内存管理算法 4：  
![](https://i-blog.csdnimg.cn/blog_migrate/76ccd3852dba770470eace3974955cbc.png)  
在`RVDS文件夹`选择要使用的开发板系列：  
![](https://i-blog.csdnimg.cn/blog_migrate/4000808c2d087e4d34c41db50c351a0b.png)

`在ARM_CM3文件夹内`有如下文件：  
![](https://i-blog.csdnimg.cn/blog_migrate/f2db4b3e547667855c3148ec05870564.png)

### 移植开始

在之前 STM32 的模板中新建：没有模板的，可参考我的前篇文章：[【STM32 标准库开发 工程模板建立最强最详细总结，详细到炸裂，一顿狂输出，看完立刻起飞】](https://blog.csdn.net/weixin_52924633/article/details/136669654?spm=1001.2014.3001.5501) 建立模板：我这里提供一个建立好的模板：  
[工程模板下载](https://pan.baidu.com/s/1nlPnzKGz5fD26aGIuDSusQ?pwd=8888)  
工程模板下载完成后目录结构如下图：并将该文件夹改名为`FreeRTOS工程模板`  
原始：![](https://i-blog.csdnimg.cn/blog_migrate/2749369f128a99f7bc9b3fb29be7bb6e.png)  
改名后：  
![](https://i-blog.csdnimg.cn/blog_migrate/a699c4ab88d05a49a8a99c4c94635015.png)

我们在这个模板中`新建文件夹FreeRTOS`  
![](https://i-blog.csdnimg.cn/blog_migrate/4fabf48895d1e8a1cee8d8bc0571d044.png)

在`FreeRTOS文件夹内`建立三个文件夹存放上述的头文件夹和内存管理文件夹以及. c 文件：  
![](https://i-blog.csdnimg.cn/blog_migrate/3c5714ffcc9f9318d7c30c37b3021e9e.png)

`source文件夹`存放. c 文件  
![](https://i-blog.csdnimg.cn/blog_migrate/63a04e33017ac1036a1ddce54af19867.png)  
完成后

![](https://i-blog.csdnimg.cn/blog_migrate/99e52e759f6ddebecdb1a2a464e938b2.png)

`portable文件夹`存放内存管理的文件夹  
d.png)

![](https://i-blog.csdnimg.cn/blog_migrate/5c2d319bf79da87811a4fadea3b96105.png)  
include 文件夹添加头文件

![](https://i-blog.csdnimg.cn/blog_migrate/5e353f276f40f9d4896bc7853936dc60.png)

找到配置文件`FreeRTOSConfig.h`：  
1. 找到目录  
![](https://i-blog.csdnimg.cn/blog_migrate/c371ff99bb205a74590be3ea37cbb349.png)  
2. 打开`CORTEX_STM32F103_Keil 文件夹可查看到该FreeRTOSConfig.h`复制该文件至 User  
![](https://i-blog.csdnimg.cn/blog_migrate/e15c8ff32393a516e965286a05059de4.png)

### Keil5 打开工程

![](https://i-blog.csdnimg.cn/blog_migrate/c461599f65f7f5d929a126564904ddbe.png)  
点击`“品”`添加必要文件  
![](https://i-blog.csdnimg.cn/blog_migrate/f818ac043f479e15591df8056a560b16.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/702fa6f654d37b33e0dbc9b9dd30e03c.png)  
`添加FreeRTOSConfig.h`：  
![](https://i-blog.csdnimg.cn/blog_migrate/ad36ca24e6121759ae27280b7cf7f066.png)

完成后：  
![](https://i-blog.csdnimg.cn/blog_migrate/e688882807132b482a48b30bcae9559d.png)

`将头文件目录添加至编译器，让编译器知道.h文件路径在哪`, 不要忘记点 OK 确定按钮：  
![](https://i-blog.csdnimg.cn/blog_migrate/f1ac24fbf5fe60584f70e47ef5c410be.png)

### 修改 FreeRTOSConfig.h 文件

把 FreeRTOSConfig.h 里面的内容替换成我下面已经裁剪好的这个：

![](https://i-blog.csdnimg.cn/blog_migrate/a72862271f6d7e85431e7a8d1116ed67.png)  
替换里面的内容

```
/*
 * FreeRTOS V202212.01
 * Copyright (C) 2020 Amazon.com, Inc. or its affiliates.  All Rights Reserved.
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy of
 * this software and associated documentation files (the "Software"), to deal in
 * the Software without restriction, including without limitation the rights to
 * use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
 * the Software, and to permit persons to whom the Software is furnished to do so,
 * subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in all
 * copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
 * FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
 * COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
 * IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
 * CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 *
 * https://www.FreeRTOS.org
 * https://github.com/FreeRTOS
 *
 */

#ifndef FREERTOS_CONFIG_H
#define FREERTOS_CONFIG_H

/*-----------------------------------------------------------
 * Application specific definitions.
 *
 * These definitions should be adjusted for your particular hardware and
 * application requirements.
 *
 * THESE PARAMETERS ARE DESCRIBED WITHIN THE 'CONFIGURATION' SECTION OF THE
 * FreeRTOS API DOCUMENTATION AVAILABLE ON THE FreeRTOS.org WEB SITE.
 *
 * See http://www.freertos.org/a00110.html
 *----------------------------------------------------------*/

#define configUSE_PREEMPTION		1


/***************************************************************
             FreeRTOS与钩子函数有关的配置选项                                            
**************************************************************/
/* 置1：使用空闲钩子（Idle Hook类似于回调函数）；置0：忽略空闲钩子
 * 
 * 空闲任务钩子是一个函数，这个函数由用户来实现，
 * FreeRTOS规定了函数的名字和参数：void vApplicationIdleHook(void )，
 * 这个函数在每个空闲任务周期都会被调用
 * 对于已经删除的RTOS任务，空闲任务可以释放分配给它们的堆栈内存。
 * 因此必须保证空闲任务可以被CPU执行
 * 使用空闲钩子函数设置CPU进入省电模式是很常见的
 * 不可以调用会引起空闲任务阻塞的API函数
 */
#define configUSE_IDLE_HOOK			0  //空闲时 钩子函数 回调函数

/* 置1：使用时间片钩子（Tick Hook）；置0：忽略时间片钩子
 * 
 * 
 * 时间片钩子是一个函数，这个函数由用户来实现，
 * FreeRTOS规定了函数的名字和参数：void vApplicationTickHook(void )
 * 时间片中断可以周期性的调用
 * 函数必须非常短小，不能大量使用堆栈，
 * 不能调用以”FromISR" 或 "FROM_ISR”结尾的API函数
 */
 /*xTaskIncrementTick函数是在xPortSysTickHandler中断函数中被调用的。因此，vApplicationTickHook()函数执行的时间必须很短才行*/
#define configUSE_TICK_HOOK			0

//使用内存申请失败钩子函数
#define configUSE_MALLOC_FAILED_HOOK			0 


#define configCPU_CLOCK_HZ			( ( unsigned long ) 72000000 )
#define configTICK_RATE_HZ			( ( TickType_t ) 1000 )
#define configMAX_PRIORITIES		( 32 ) //可使用的最大优先级
#define configMINIMAL_STACK_SIZE	( ( unsigned short ) 128 )   //是128字，并非字节 即128*4个字节，堆栈大小以字为单位
#define configTOTAL_HEAP_SIZE		( ( size_t ) ( 17 * 1024 ) )
#define configMAX_TASK_NAME_LEN		( 16 )
#define configUSE_TRACE_FACILITY	0
#define configUSE_16_BIT_TICKS		0 //系统节拍计数器变量数据类型，1表示为16位无符号整形，0表示为32位无符号整形
#define configIDLE_SHOULD_YIELD		1 //1:空闲任务放弃CPU使用权,给其他同优先级的用户任务  2:空闲优先级和其他优先级相同。避免2，多使用1

#define configUSE_TIME_SLICING      1 //时间片调度，当优先级相同时执行


#define configUSE_QUEUE_SETS        0 //队列 为1开启，为0关闭



#define configUSE_TASK_NOTIFICATIONS 1 //开启任务通知功能，默认开启


#define configUSE_MUTEXES           0  //互斥信号量开关


                                           
#define configUSE_RECURSIVE_MUTEXES			0   //使用递归互斥信号量 


#define configUSE_COUNTING_SEMAPHORES		0  //为1时使用计数信号量

/* 设置可以注册的信号量和消息队列个数 */
#define configQUEUE_REGISTRY_SIZE			10                                 
                                                                       
#define configUSE_APPLICATION_TASK_TAG		 0         


/*****************************************************************
              FreeRTOS与内存申请有关配置选项                                               
*****************************************************************/
//支持动态内存申请
#define configSUPPORT_DYNAMIC_ALLOCATION     1    
//支持静态内存
#define configSUPPORT_STATIC_ALLOCATION		 0					


/*
 * 大于0时启用堆栈溢出检测功能，如果使用此功能 
 * 用户必须提供一个栈溢出钩子函数，如果使用的话
 * 此值可以为1或者2，因为有两种栈溢出检测方法 */
#define configCHECK_FOR_STACK_OVERFLOW		 0   


/********************************************************************
          FreeRTOS与运行时间和任务状态收集有关的配置选项   
**********************************************************************/
//启用运行时间统计功能
#define configGENERATE_RUN_TIME_STATS	     0             
 //启用可视化跟踪调试
#define configUSE_TRACE_FACILITY			 0    
/* 与宏configUSE_TRACE_FACILITY同时为1时会编译下面3个函数
 * prvWriteNameToBuffer()
 * vTaskList(),
 * vTaskGetRunTimeStats()
*/

                                                                        
/********************************************************************
                FreeRTOS与协程有关的配置选项                                                
*********************************************************************/
//启用协程，启用协程以后必须添加文件croutine.c
#define configUSE_CO_ROUTINES 			        0                 
//协程的有效优先级数目
#define configMAX_CO_ROUTINE_PRIORITIES       ( 2 )                   


/***********************************************************************
                FreeRTOS与软件定时器有关的配置选项      
**********************************************************************/
 //启用软件定时器
#define configUSE_TIMERS				        0                              
//软件定时器优先级
#define configTIMER_TASK_PRIORITY		      (configMAX_PRIORITIES-1)        
//软件定时器队列长度
#define configTIMER_QUEUE_LENGTH		        10                               
//软件定时器任务堆栈大小
#define configTIMER_TASK_STACK_DEPTH	      (configMINIMAL_STACK_SIZE*2)    




/*
 * 某些运行FreeRTOs的硬件有两种方法选择下一个要执行的任务:
 *
 * 通用方法和特定于硬件的方法（以下简称"特殊方法")。
 * 通用方法:
 * 1.configUSE_PORT_OPTIMISED_TASK_SELECTION为О或者硬件不支持这种特殊方法。
 * 2.可以用于所有FreeRTOS支持的硬件
 * 3.完全用c实现,效率略低于特殊方法。
 * 4.不强制要求限制最大可用优先级数目

 * 特殊方法:
 * 1.必须将configUSE_PORT_OPTIMISED_TASK_SELECTION设置为1。
 * 2.依赖一个或多个特定架构的汇编指令（一般是类似计算前导零[CLZ]指令）。
 * 3.比通用方法更高效
 * 4.一般强制限定最大可用优先级数目为32
 * 一般是硬件计算前导零指令，如果所使用的，MCU没有这些硬件指令的话此宏应该设置为0 !
*/
#define configUSE_PORT_OPTIMISED_TASK SELECTION  1

/*
 * configUSE_TICKLESS_IDLE  

 * 置1:使能低功耗tickless模式;置0:保持系统节拍(tick）中断一直运行
 * 假设开启低功耗的话可能会导致下载出现问题，因为程序在睡眠中,可用以下办法解决
 * 下载方法:
 * 1.将开发版正常连接好
 * 2.按住复位按键,点击下载瞬间松开复位按键
 * 

 * 1.通过跳线帽将BO0T 0接高电平(3.3v)
 * 2.重新上电,下载
 * 1.使用FlyMcu擦除一下芯片,然后进行下载STMISP ->清除芯片(z)
 * 
 */
#define configUSE_TICKLESS_IDLE   0





/*配置必要的声明*/
#define xPortPendSVHandler              PendSV_Handler 
#define vPortSVCHandler                 SVC_Handler
#define INCLUDE_xTaskGetSchedulerState  1



/************************************************************
            FreeRTOS可选函数配置选项                                                     
************************************************************/
#define INCLUDE_xTaskGetSchedulerState       1                       
#define INCLUDE_vTaskPrioritySet		     1
#define INCLUDE_uxTaskPriorityGet		     1
#define INCLUDE_vTaskDelete				     1
#define INCLUDE_vTaskCleanUpResources	     1
#define INCLUDE_vTaskSuspend			     1
#define INCLUDE_vTaskDelayUntil			     1
#define INCLUDE_vTaskDelay				     1
#define INCLUDE_eTaskGetState			     1
#define INCLUDE_xTimerPendFunctionCall	     0
//#define INCLUDE_xTaskGetCurrentTaskHandle       1
//#define INCLUDE_uxTaskGetStackHighWaterMark     0
//#define INCLUDE_xTaskGetIdleTaskHandle          0



/******************************************************************
            FreeRTOS与中断有关的配置选项                                                 
******************************************************************/
#ifdef __NVIC_PRIO_BITS
	#define configPRIO_BITS       		__NVIC_PRIO_BITS
#else
	#define configPRIO_BITS       		4                  
#endif
//中断最低优先级
#define configLIBRARY_LOWEST_INTERRUPT_PRIORITY			15     

//系统可管理的最高中断优先级，
#define configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY	5 //5指 中断优先级  0~5不被管控，中断5~15被freertos管控

#define configKERNEL_INTERRUPT_PRIORITY 		( configLIBRARY_LOWEST_INTERRUPT_PRIORITY << (8 - configPRIO_BITS) )	/* 240 */

#define configMAX_SYSCALL_INTERRUPT_PRIORITY 	( configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY << (8 - configPRIO_BITS) )




/* This is the value being used as per the ST library which permits 16
priority values, 0 to 15.  This must correspond to the
configKERNEL_INTERRUPT_PRIORITY setting.  Here 15 corresponds to the lowest
NVIC value of 255. */
#define configLIBRARY_KERNEL_INTERRUPT_PRIORITY	15

#endif /* FREERTOS_CONFIG_H */



```

#### 修改 stm32f10x_it.c

1. 添加头文件  
![](https://i-blog.csdnimg.cn/blog_migrate/90f2e4b88d18e56ad72c52d8fc67de19.png)

```
/*引入FreeRTOS文件*/
#include "FreeRTOS.h"//引入FreeRTOS文件
#include "task.h"//引入task.h文件

```

2. 注释：因为 FreeRTOS.h 已经有定义，要用 FreeRTOS 里的了，如果不替换，会报重复定义的错。  
![](https://i-blog.csdnimg.cn/blog_migrate/15296048f0b0f4e4552ae4a7bdd1ae7a.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/19f801405e82722a45ae9eda34b1bcfa.png)

3. 添加内容：在 SysTick_Handler(void) 函数中添加内容  
![](https://i-blog.csdnimg.cn/blog_migrate/0a9c7a93738251dac8aa327893cea621.png)

```
/**
  * @brief  This function handles SysTick Handler.
  * @param  None
  * @retval None
  */
void SysTick_Handler(void)
{
	if(xTaskGetSchedulerState()!=taskSCHEDULER_NOT_STARTED)//如果开启了调度
	{
		xPortSysTickHandler();
	}
}


```

编译：无警告，无问题  
![](https://i-blog.csdnimg.cn/blog_migrate/fba29cac6ea173bed0cc4c808619819c.png)

测试
--

### FreeRTOS 闪烁第一颗小灯

`main.c`

```
#include "stm32f10x.h"                  // Device header

#include "LED.h"

#include "FreeRTOS.h"
#include "task.h"


//任务优先级
#define START_TASK_PRIO		1
//任务堆栈大小	
#define START_STK_SIZE 		128  
//任务句柄
TaskHandle_t StartTask_Handler;
//任务函数
void start_task(void *pvParameters);

//任务优先级
#define LED1_TASK_PRIO		2
//任务堆栈大小	
#define LED1_STK_SIZE 		50  
//任务句柄
TaskHandle_t LED1Task_Handler;
//任务函数
void led1_task(void *pvParameters);


/*******************************************************************************
* 函 数 名         : main
* 函数功能		   : 主函数
* 输    入         : 无
* 输    出         : 无
*******************************************************************************/
int main()
{
	LED_Init();
	
	//创建开始任务
    xTaskCreate((TaskFunction_t )start_task,            //任务函数
                (const char*    )"start_task",          //任务名称
                (uint16_t       )START_STK_SIZE,        //任务堆栈大小
                (void*          )NULL,                  //传递给任务函数的参数
                (UBaseType_t    )START_TASK_PRIO,       //任务优先级
                (TaskHandle_t*  )&StartTask_Handler);   //任务句柄              
    vTaskStartScheduler();          //开启任务调度
}

//开始任务任务函数
void start_task(void *pvParameters)
{
    taskENTER_CRITICAL();           //进入临界区
      
    //创建LED1任务
    xTaskCreate((TaskFunction_t )led1_task,     
                (const char*    )"led1_task",   
                (uint16_t       )LED1_STK_SIZE, 
                (void*          )NULL,
                (UBaseType_t    )LED1_TASK_PRIO,
                (TaskHandle_t*  )&LED1Task_Handler); 
				
    vTaskDelete(StartTask_Handler); //删除开始任务
    taskEXIT_CRITICAL();            //退出临界区
} 

//LED1任务函数
void led1_task(void *pvParameters)
{
    while(1)
    {
        LED1_ON();
        vTaskDelay(1000);
        LED1_OFF();
        vTaskDelay(1000);
    }
}


```

现象便是小灯 1s 一次的闪烁。  
注意：测试小灯闪烁仅仅是在上面创建好的 FreeRTOS 模板中的 main.c 代码中加了代码而已。  
完整 FreeRTOS 模板及小灯闪烁代码链接：  
[小灯闪烁及 FreeRTOS 动态及静态创建任务工程模板以及点灯源码](https://pan.baidu.com/s/1PFGNdWIwhMU4pQxaW4tuVw?pwd=6666)

完结 ------------------------------------------------------------------ 撒花

声明：仅供学习使用