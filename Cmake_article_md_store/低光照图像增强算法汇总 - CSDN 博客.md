> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/WZZ18191171661/article/details/104325353)

#### 目录

*   *   [1、场景需求](#1_1)
    *   [2、Retinex 算法](#2Retinex_4)
    *   *   *   [2.1 Retinex 算法简介](#21_Retinex_6)
            *   [2.2 Retinex 核心代码实现](#22_Retinex_8)
            *   [2.3 Retinex 算法效果展示与分析](#23_Retinex_121)
    *   [3、LIME 算法](#3LIME_126)
    *   *   *   [3.1、LIME 算法简介](#31LIME_128)
            *   [3.2、LIME 核心代码实现](#32LIME_130)
            *   [3.3、LIME 算法效果展示与分析](#33LIME_256)
    *   [4、RetinexNet 算法](#4RetinexNet_260)
    *   *   *   [4.1 RetinexNet 算法简介](#41_RetinexNet_262)
            *   [4.2 RetinexNet 网络详解](#42_RetinexNet_264)
            *   [4.3 RetinexNet 核心代码实现](#43_RetinexNet_266)
            *   [4.4 RetinexNet 算法效果展示与分析](#44_RetinexNet_324)
    *   [5、MBLLEN 算法](#5MBLLEN_329)
    *   *   *   [5.1 MBLLEN 算法简介](#51_MBLLEN_331)
            *   [5.2 MBLLEN 网络详解](#52_MBLLEN_333)
            *   [5.3 MBLLEN 核心代码实现](#53_MBLLEN_337)
            *   [5.4 MBLLEN 算法效果展示与分析](#54_MBLLEN_377)
    *   [6、KinD 算法](#6KinD_382)
    *   *   *   [6.1 KinD 算法简介](#61_KinD_384)
            *   [6.2 KinD 网络详解](#62_KinD_387)
            *   [6.3 KinD 核心代码实现](#63_KinD_390)
            *   [6.4 KinD 算法效果展示与分析](#64_KinD_496)
    *   [7、EnlightenGAN 算法](#7EnlightenGAN_501)
    *   *   *   [7.1 EnlightenGAN 算法简介](#71_EnlightenGAN_503)
            *   [7.2 EnlightenGAN 网络简介](#72_EnlightenGAN_505)
            *   [7.3 EnlightenGAN 核心代码实现](#73_EnlightenGAN_508)
            *   [7.4 EnlightenGAN 算法效果展示与分析](#74_EnlightenGAN_575)
    *   [8、总结](#8_580)
    *   [参考资料](#_583)
    *   [注意事项](#_591)

### 1、场景需求

  在现实场景中，由于光线、视角等问题会导致我们拍摄出来的照片比较阴暗，具体的图片如下图中的 1、3、5 列所示，然后这些阴暗的图片不仅会影响我们的观察，而且会极大的影响计算机视觉处理算法的效果，2、4、6 列表示的是使用了低光照图像增强算法之后的效果。**本文主要针对低光照的图片展开论述，对经典的一些低光照图像增强算法进行了总结和初略的分析。**  
![](https://i-blog.csdnimg.cn/blog_migrate/855ea2f7ab6b22f3b3c16ec97ed139fa.jpeg#pic_center)

### 2、Retinex 算法

[论文链接](https://www.researchgate.net/publication/316744775_Multi-scale_retinex_with_color_restoration_image_enhancement_based_on_Gaussian_filtering_and_guided_filtering) -[Github 链接](https://github.com/dongb5/Retinex)

##### 2.1 Retinex 算法简介

  Retinex 是一种常用的建立在科学实验和科学分析基础上的图像增强方法，它是由 Edwin.H. Land 于 1963 年提出的。Retinex 的基础理论是物体的颜色是由物体对长波（红色）、中波（绿色）、短波（蓝色）光线的反射能力来决定的，而不是由反射光强度的绝对值来决定的，物体的色彩不受光照非均匀性的影响，具有一致性，即 retinex 是以色感一致性（颜色恒常性）为基础的。** 不同于传统的线性、非线性的只能增强图像某一类特征的方法，Retinex 可以在动态范围压缩、边缘增强和颜色恒定三个方面做到平衡，因此可以对各种不同类型的图像进行自适应的增强。** 具体的原理请参考[该博客](https://blog.csdn.net/qq_26898461/article/details/47258897)。

##### 2.2 Retinex 核心代码实现

```
# coding=utf-8
import numpy as np
import cv2

def singleScaleRetinex(img, sigma):
    '''单尺度Retinex函数'''
    retinex = np.log10(img) - np.log10(cv2.GaussianBlur(img, (0, 0), sigma))
    return retinex

def multiScaleRetinex(img, sigma_list):
    '''多尺度Retinex函数'''
    # 提前分配空间
    retinex = np.zeros_like(img)
    # 遍历所有的尺度
    for sigma in sigma_list:
    	# 对计算的结果进行叠加
        retinex += singleScaleRetinex(img, sigma)
    # 计算多个尺度的平均值
    retinex = retinex / len(sigma_list)
    return retinex

def colorRestoration(img, alpha, beta):
	'''颜色灰度函数'''
    img_sum = np.sum(img, axis=2, keepdims=True)
    color_restoration = beta * (np.log10(alpha * img) - np.log10(img_sum))
    return color_restoration

def simplestColorBalance(img, low_clip, high_clip):    
    '''最简单的颜色均衡函数'''
    total = img.shape[0] * img.shape[1]
    for i in range(img.shape[2]):
        unique, counts = np.unique(img[:, :, i], return_counts=True)
        current = 0
        for u, c in zip(unique, counts):            
            if float(current) / total < low_clip:
                low_val = u
            if float(current) / total < high_clip:
                high_val = u
            current += c    
        img[:, :, i] = np.maximum(np.minimum(img[:, :, i], high_val), low_val)
    return img    

def MSRCR(img, sigma_list, G, b, alpha, beta, low_clip, high_clip):
	'''MSRCR函数'''
    img = np.float64(img) + 1.0
    # 对原图先做多尺度的Retinex
    img_retinex = multiScaleRetinex(img, sigma_list)    
    # 对原图做颜色恢复
    img_color = colorRestoration(img, alpha, beta)   
    # 进行图像融合 
    img_msrcr = G * (img_retinex * img_color + b)
    
    for i in range(img_msrcr.shape[2]):
        img_msrcr[:, :, i] = (img_msrcr[:, :, i] - np.min(img_msrcr[:, :, i])) / \
                             (np.max(img_msrcr[:, :, i]) - np.min(img_msrcr[:, :, i])) * \
                             255
    # 将图像调整到[0,255]范围内          
    img_msrcr = np.uint8(np.minimum(np.maximum(img_msrcr, 0), 255))
    # 做简单的颜色均衡
    img_msrcr = simplestColorBalance(img_msrcr, low_clip, high_clip)       
    return img_msrcr

def automatedMSRCR(img, sigma_list):
	'''automatedMSRCR函数'''
    img = np.float64(img) + 1.0
    img_retinex = multiScaleRetinex(img, sigma_list)
    for i in range(img_retinex.shape[2]):
        unique, count = np.unique(np.int32(img_retinex[:, :, i] * 100), return_counts=True)
        for u, c in zip(unique, count):
            if u == 0:
                zero_count = c
                break
            
        low_val = unique[0] / 100.0
        high_val = unique[-1] / 100.0
        for u, c in zip(unique, count):
            if u < 0 and c < zero_count * 0.1:
                low_val = u / 100.0
            if u > 0 and c < zero_count * 0.1:
                high_val = u / 100.0
                break 
        img_retinex[:, :, i] = np.maximum(np.minimum(img_retinex[:, :, i], high_val), low_val)
        img_retinex[:, :, i] = (img_retinex[:, :, i] - np.min(img_retinex[:, :, i])) / \
                               (np.max(img_retinex[:, :, i]) - np.min(img_retinex[:, :, i])) \
                               * 255
    img_retinex = np.uint8(img_retinex)
    return img_retinex

def MSRCP(img, sigma_list, low_clip, high_clip):
	'''MSRCP函数'''
    img = np.float64(img) + 1.0
    intensity = np.sum(img, axis=2) / img.shape[2]    
    retinex = multiScaleRetinex(intensity, sigma_list)
    intensity = np.expand_dims(intensity, 2)
    retinex = np.expand_dims(retinex, 2)
    intensity1 = simplestColorBalance(retinex, low_clip, high_clip)
    intensity1 = (intensity1 - np.min(intensity1)) / \
                 (np.max(intensity1) - np.min(intensity1)) * \
                 255.0 + 1.0
    img_msrcp = np.zeros_like(img)
    
    for y in range(img_msrcp.shape[0]):
        for x in range(img_msrcp.shape[1]):
            B = np.max(img[y, x])
            A = np.minimum(256.0 / B, intensity1[y, x, 0] / intensity[y, x, 0])
            img_msrcp[y, x, 0] = A * img[y, x, 0]
            img_msrcp[y, x, 1] = A * img[y, x, 1]
            img_msrcp[y, x, 2] = A * img[y, x, 2]
    img_msrcp = np.uint8(img_msrcp - 1.0)
    return img_msrcp

```

##### 2.3 Retinex 算法效果展示与分析

![](https://i-blog.csdnimg.cn/blog_migrate/80ff9b8d271a5e27372c046a0b6c1eee.jpeg#pic_center)  
  上图展示了 Retinex 算法在一些真实数据上面的效果。上面主要展示了该算法在一些文档上面的增强效果，第一行的 5 张图片表示的是原始的输入图片，第二行的 5 张图片表示的是使用 Retinex 算法增强之后的效果。**通过观察我们可以发现，该算法不仅能够增强图像的亮度信息，同时可以去除图片中的部分阴影信息；但是该算法的运算速度比较慢，不能应用到一些实时的场景中。**

### 3、LIME 算法

[论文链接](http://www.dabi.temple.edu/~hbling/publication/LIME-tip.pdf) -[LIME 主页](https://sites.google.com/view/xjguo/lime) -[Github 链接](https://github.com/zj611/LIME_Processing)

##### 3.1、LIME 算法简介

  LIME 算法是一个简单而高效的低光照图像增强算法。**该算法首先通过在 R、G 和 B 通道中找到最大值来单独估计每个像素的照明；然后我们通过在初始光照图上施加一个结构先验来细化它，作为最终的光照映射；最后根据光照映射就可以生成最终的增强图像**。

##### 3.2、LIME 核心代码实现

```
// 导入头文件
#include "lime.h"
#include <vector>
#include <iostream>

// 定义命名空间
namespace feature
{
    // 获取原图像的通道
    lime::lime(cv::Mat src)
    {
        channel = src.channels();
    }
    
    cv::Mat lime::lime_enhance(cv::Mat &src)
    {
        cv::Mat img_norm;
        // 图像归一化
        src.convertTo(img_norm, CV_32F, 1 / 255.0, 0);
        // 提前分配空间
        cv::Size sz(img_norm.size());
        cv::Mat out(sz, CV_32F, cv::Scalar::all(0.0));

        auto gammT = out.clone();
        // 根据通道做不同的处理
        if (channel == 3)
        {
            Illumination(img_norm, out);
            Illumination_filter(out, gammT);

            //lime
            std::vector<cv::Mat> img_norm_rgb;
            cv::Mat img_norm_b, img_norm_g, img_norm_r;

            cv::split(img_norm, img_norm_rgb);

            img_norm_g = img_norm_rgb.at(0);
            img_norm_b = img_norm_rgb.at(1);
            img_norm_r = img_norm_rgb.at(2);

            cv::Mat one = cv::Mat::ones(sz, CV_32F);

            float nameta = 0.9;
            auto g = 1 - ((one - img_norm_g) - (nameta * (one - gammT))) / gammT;
            auto b = 1 - ((one - img_norm_b) - (nameta * (one - gammT))) / gammT;
            auto r = 1 - ((one - img_norm_r) - (nameta * (one - gammT))) / gammT;

            cv::Mat g1, b1, r1;

            //TODO <=1
            threshold(g, g1, 0.0, 0.0, 3);
            threshold(b, b1, 0.0, 0.0, 3);
            threshold(r, r1, 0.0, 0.0, 3);

            img_norm_rgb.clear();
            img_norm_rgb.push_back(g1);
            img_norm_rgb.push_back(b1);
            img_norm_rgb.push_back(r1);

            cv::merge(img_norm_rgb,out_lime);
            out_lime.convertTo(out_lime,CV_8U,255);

        }
        else if(channel == 1)
        {
            Illumination_filter(img_norm, gammT);
            cv::Mat one = cv::Mat::ones(sz, CV_32F);
            float nameta = 0.9;
            //std::cout<<img_norm.at<float>(1,1)<<std::endl;
            auto out = 1 - ((one - img_norm) - (nameta * (one - gammT))) / gammT;

            threshold(out, out_lime, 0.0, 0.0, 3);

            out_lime.convertTo(out_lime,CV_8UC1,255);

        }
        else
        {
            std::cout<<"There is a problem with the channels"<<std::endl;
            exit(-1);
        }
        return out_lime.clone();
    }

    void lime::Illumination_filter(cv::Mat& img_in,cv::Mat& img_out)
    {
        int ksize = 5;
        //mean filter
        blur(img_in,img_out,cv::Size(ksize,ksize));
        //GaussianBlur(img_in,img_mean_filter,Size(ksize,ksize),0,0);

        //gamma
        int row = img_out.rows;
        int col = img_out.cols;
        float tem;
        float gamma = 0.8;
        for(int i=0;i<row;i++)
        {
            for(int j=0;j<col;j++)
            {
                tem = pow(img_out.at<float>(i,j),gamma);
                tem = tem <= 0 ? 0.0001 : tem;  //  double epsolon = 0.0001;
                tem = tem > 1 ? 1 : tem;

                img_out.at<float>(i,j) = tem;
            }
        }
    }
    void lime::Illumination(cv::Mat& src,cv::Mat& out)
    {
        int row = src.rows, col = src.cols;
        for(int i=0;i<row;i++)
        {
            for(int j=0;j<col;j++)
            {
                out.at<float>(i,j) = lime::compare(src.at<cv::Vec3f>(i,j)[0],
                                                   src.at<cv::Vec3f>(i,j)[1],
                                                   src.at<cv::Vec3f>(i,j)[2]);
            }
        }
    }
}

```

##### 3.3、LIME 算法效果展示与分析

![](https://i-blog.csdnimg.cn/blog_migrate/d38afab29d8a8d29ed068845f8d72fd2.jpeg#pic_center)  
  上图展示了 LIME 算法在一些图片上面的效果，第一行表示的是原始的输入图片，第二行表示的是由 LIME 算法输出的光照映射，第三行表示的是经过 LIME 算法增强之后的效果。**通过观察我们可以得出，LIME 算法能够较好的处理一些低光照图片，美中不足的是经过 LIME 算法增强之后的图片的色彩好像不太理想。**

### 4、RetinexNet 算法

[论文链接](https://arxiv.org/pdf/1808.04560.pdf) -[Github 链接](https://github.com/weichen582/RetinexNet) - [项目主页](https://daooshee.github.io/BMVC2018website/)

##### 4.1 RetinexNet 算法简介

  RetinexNet 是 Retinex 算法的加强版，主要的思路是通过在收集到的低光照数据块上面训练一个神经网络。**整个网路分为两部分，Decom-Net 网络用来实现图像分解，Enhance-Net 网络用来实现光照调节。在训练 Decom-Net 的过程中，没有 GT，学习网络时只需满足以下关键约束条件包括由成对的低 / 正常光图像共享的一致反射率以及光照的一致性，由 Enhance-Net 来实现光照的增强。RetinexNet 是一个端到端的低光照增强网络，大量的实验表明，该方法不仅可以获得令人满意的低光增强效果，而且可以很好地表示图像分解**。

##### 4.2 RetinexNet 网络详解

![](https://i-blog.csdnimg.cn/blog_migrate/21276154ada8eee763d76e8203118476.jpeg#pic_center)  上图展示了 RetinexNet 网络的实现细节，整个图像增强的过程分为三步，具体包括图像分解、亮度调整，图像重建。**在图像分解阶段，Decom-Net 子网络将输入的图像分解为反射率图像和光照图像两部分，两个部分之间共享权重，如上图中的 Snormal 和 Slow 所示，输入的两个图像都被分解成两个部分；在光照调节阶段，一个基于编解码架构的 Enhance-Net 网络用来对光照图像进行光照增强，该子网络中包含着一些残差连接用来解决梯度消失问题，同时对反射率图像执行去噪操作；在图像重建阶段，将经过光照增强的光照图像和经过去噪的反射率图像重新组合起来形成最终的增强图片**。

##### 4.3 RetinexNet 核心代码实现

```
# 导入相应的python包
from __future__ import print_function

import os
import time
import random

from PIL import Image
import tensorflow as tf
import numpy as np

from utils import *

# 定义concat操作
def concat(layers):
    return tf.concat(layers, axis=3)

def DecomNet(input_im, layer_num, channel=64, kernel_size=3):
	'''分解子网络函数'''
    input_max = tf.reduce_max(input_im, axis=3, keepdims=True)
    input_im = concat([input_max, input_im])
    with tf.variable_scope('DecomNet', reuse=tf.AUTO_REUSE):
        conv = tf.layers.conv2d(input_im, channel, kernel_size * 3, padding='same', activation=None, )
        for idx in range(layer_num):
            conv = tf.layers.conv2d(conv, channel, kernel_size, padding='same', activation=tf.nn.relu, name='activated_layer_%d' % idx)
        conv = tf.layers.conv2d(conv, 4, kernel_size, padding='same', activation=None, name='recon_layer')

    R = tf.sigmoid(conv[:,:,:,0:3])
    L = tf.sigmoid(conv[:,:,:,3:4])

    return R, L

def RelightNet(input_L, input_R, channel=64, kernel_size=3):
	'''光照增强子网络函数'''
    input_im = concat([input_R, input_L])
    with tf.variable_scope('RelightNet'):
        conv0 = tf.layers.conv2d(input_im, channel, kernel_size, padding='same', activation=None)
        conv1 = tf.layers.conv2d(conv0, channel, kernel_size, strides=2, padding='same', activation=tf.nn.relu)
        conv2 = tf.layers.conv2d(conv1, channel, kernel_size, strides=2, padding='same', activation=tf.nn.relu)
        conv3 = tf.layers.conv2d(conv2, channel, kernel_size, strides=2, padding='same', activation=tf.nn.relu)
        
        up1 = tf.image.resize_nearest_neighbor(conv3, (tf.shape(conv2)[1], tf.shape(conv2)[2]))
        deconv1 = tf.layers.conv2d(up1, channel, kernel_size, padding='same', activation=tf.nn.relu) + conv2
        up2 = tf.image.resize_nearest_neighbor(deconv1, (tf.shape(conv1)[1], tf.shape(conv1)[2]))
        deconv2= tf.layers.conv2d(up2, channel, kernel_size, padding='same', activation=tf.nn.relu) + conv1
        up3 = tf.image.resize_nearest_neighbor(deconv2, (tf.shape(conv0)[1], tf.shape(conv0)[2]))
        deconv3 = tf.layers.conv2d(up3, channel, kernel_size, padding='same', activation=tf.nn.relu) + conv0
        
        deconv1_resize = tf.image.resize_nearest_neighbor(deconv1, (tf.shape(deconv3)[1], tf.shape(deconv3)[2]))
        deconv2_resize = tf.image.resize_nearest_neighbor(deconv2, (tf.shape(deconv3)[1], tf.shape(deconv3)[2]))
        feature_gather = concat([deconv1_resize, deconv2_resize, deconv3])
        feature_fusion = tf.layers.conv2d(feature_gather, channel, 1, padding='same', activation=None)
        output = tf.layers.conv2d(feature_fusion, 1, 3, padding='same', activation=None)
    return output

```

##### 4.4 RetinexNet 算法效果展示与分析

![](https://i-blog.csdnimg.cn/blog_migrate/9cf43f170ab7ce2b4d2c9a47ae0b57f1.jpeg#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/97374ee0ba746f84d73556b118b33ea3.jpeg#pic_center)  
  上面第一张展示了 Retinex-Net 算法在一些室内和室外场景下和其它不同增强算法的比较结果，第一列表示原始的输入图片，最后一列表示使用 Retinex-Net 增强之后的结果。与其它的算法相比，Retinex-Net 算法能够获得更清晰的增强效果，图像的细节更加丰富。第二张展示了 Retinex-Net 算法在真实的文档图片上面的增强效果，第一行表示原始的输入图片，第二行表示增强后的图片，增强后的图像看起来更加清晰，便于后续算法的处理。

### 5、MBLLEN 算法

[论文链接](http://bmvc2018.org/contents/papers/0700.pdf) -[Github 链接](https://github.com/Lvfeifan/MBLLEN) - [项目主页](http://phi-ai.org/project/MBLLEN/default.htm)

##### 5.1 MBLLEN 算法简介

  MBLLEN 算法是一个多分支低光照图像增强网络。**该算法的核心思想是在不同等级中提取出丰富的图像特征，因此我们可以通过多个子网络做图像增强，最后通过多分支融合产生输出图像。图像的质量从不同的方向得到了提升。该算法不仅仅能用来进行图像增强，而且可以用来进行视频增强**。

##### 5.2 MBLLEN 网络详解

![](https://i-blog.csdnimg.cn/blog_migrate/49b9c0b325f2904f1b5ba17a6fb469da.jpeg#pic_center)  
  上图展示了 MBLLEN 的网络架构，**该网络主要包含三个主要的模块，具体包括 FEM 模块、EM 模块和 FM 模块。FEM 模块（feature extraction module ），即特征检测模块，其主要作用是从图像中获取关键的特征，如图中所示，主要有一些 WXHX32 的卷积层构成；EM 模块（enhancement module），即增强模块，其主要的作用是通过编解码架构对图像的特征进行增强，具体的细节如图中所示；FM 模块（fusion module），即融合模块，其主要的作用是将不同等级输出的结果融合起来，从而形成最终的结果**。

##### 5.3 MBLLEN 核心代码实现

```
# 导入相应的python包
from keras.layers import Input, Conv2D, Conv2DTranspose, Concatenate
from keras.applications.vgg19 import VGG19
from keras.models import Model

def build_vgg():
	''''搭建VGG基准网络函数'''
	# 使用预训练的VGG网络
    vgg_model = VGG19(include_top=False, weights='imagenet')
    vgg_model.trainable = False
    return Model(inputs=vgg_model.input, outputs=vgg_model.get_layer('block3_conv4').output)

def build_mbllen(input_shape):
    def EM(input, kernal_size, channel):
    	'''搭建EM子网络函数'''
        conv_1 = Conv2D(channel, (3, 3), activation='relu', padding='same', data_format='channels_last')(input)
        conv_2 = Conv2D(channel, (kernal_size, kernal_size), activation='relu', padding='valid', data_format='channels_last')(conv_1)
        conv_3 = Conv2D(channel*2, (kernal_size, kernal_size), activation='relu', padding='valid', data_format='channels_last')(conv_2)
        conv_4 = Conv2D(channel*4, (kernal_size, kernal_size), activation='relu', padding='valid', data_format='channels_last')(conv_3)
        conv_5 = Conv2DTranspose(channel*2, (kernal_size, kernal_size), activation='relu', padding='valid', data_format='channels_last')(conv_4)
        conv_6 = Conv2DTranspose(channel, (kernal_size, kernal_size), activation='relu', padding='valid', data_format='channels_last')(conv_5)
        res = Conv2DTranspose(3, (kernal_size, kernal_size), activation='relu', padding='valid', data_format='channels_last')(conv_6)
        return res

    inputs = Input(shape=input_shape)
    FEM = Conv2D(32, (3, 3), activation='relu', padding='same', data_format='channels_last')(inputs)
    EM_com = EM(FEM, 5, 8)

    for j in range(3):
        for i in range(0, 3):
            FEM = Conv2D(32, (3, 3), activation='relu', padding='same', data_format='channels_last')(FEM)
            EM1 = EM(FEM, 5, 8)
            EM_com = Concatenate(axis=3)([EM_com, EM1])

    outputs = Conv2D(3, (1, 1), activation='relu', padding='same', data_format='channels_last')(EM_com)
    return Model(inputs, outputs)

```

##### 5.4 MBLLEN 算法效果展示与分析

![](https://i-blog.csdnimg.cn/blog_migrate/e29b1ac67b1092d608cdb4c0383ab39a.jpeg#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/b561e185acfd690d843317244a468f72.jpeg#pic_center)  
  上图展示了 MBLLEN 算法的增强效果。第一张图表示的是 MBLLEN 算法在一些公有数据集上面和其它算法的比较结果，**通过观察可以得知，该算法能够获得更加自然的增强效果，增强后的图像的整体效果看起来比较舒服，增强之后的细节更加清晰**。第二张图表示的是该算法在几张真实的文档图像上面的增强效果，增强之后的图片中仿佛进入了阳光，整个图片都变得亮堂了很多，但是在有些情况下，该算法输出的结果有点过曝光的效果，幸运的是该算法还可以输出一个较低亮度的结果，具体的结果留给你自己去做测试。

### 6、KinD 算法

[论文链接](https://arxiv.org/pdf/1905.04161.pdf) -[Github 链接](https://github.com/zhangyhuaee/KinD)

##### 6.1 KinD 算法简介

  在暗光条件下拍摄的图像经常（部分）能见度低。除了不满意的灯光，多种类型的退化，如由于相机质量的限制而产生的噪声和颜色失真，隐藏在黑暗中。换句话说，  
仅仅提高暗区的亮度必然会放大隐藏的伪影。**KinD 算法是一个简单高效的网络，它是受到 Retinex 的启发，将原始的图像分解为两个部分。KinD 将原始的图像空间分解为两个比较相似的子空间，该算法使用在不同曝光程度的图片块来进行训练，该算法对严重的视觉缺陷具有很强的鲁棒性，并且用户友好地任意调整光照水平。另外，我们的模型在 2080ti GPU 上处理一个 VGA 分辨率处理图像的时间不到 50ms**。

##### 6.2 KinD 网络详解

![](https://i-blog.csdnimg.cn/blog_migrate/03e46e90d0c3e34fb6abf4b3ea9d27d5.jpeg#pic_center)  
  上图展示了 KinD 算法的整体网络架构。整个网络的架构和 RetinexNet 很相似，**整个网络包括 Decomposition-Net、Adjustment-Net 和 Restoration-Net，Decomposition-Net 子网络的作用是对输入的图像进行分解，网络结构方面和 RetinexNet 稍微有一些不同，但是主要的作用是相同的；Adjustment-Net 用来调节光照，该子网络由几个卷积层组成，该网络不执行去噪操作；Restoration-Net 子网络的作用是通过组合反射率图像和光照图像形成最终的增强图像，该子网络是一个带有残差连接的编解码网络**，具体的细节如上图所示。

##### 6.3 KinD 核心代码实现

```
# 导入相应的Python包
import tensorflow as tf
import tensorflow.contrib.slim as slim
from tensorflow.contrib.layers.python.layers import initializers

def lrelu(x, trainbable=None):
    return tf.maximum(x*0.2,x)

def upsample_and_concat(x1, x2, output_channels, in_channels, scope_name, trainable=True):
    with tf.variable_scope(scope_name, reuse=tf.AUTO_REUSE) as scope:
        pool_size = 2
        deconv_filter = tf.get_variable('weights', [pool_size, pool_size, output_channels, in_channels], trainable= True)
        deconv = tf.nn.conv2d_transpose(x1, deconv_filter, tf.shape(x2) , strides=[1, pool_size, pool_size, 1], name=scope_name)

        deconv_output =  tf.concat([deconv, x2],3)
        deconv_output.set_shape([None, None, None, output_channels*2])

        return deconv_output

def DecomNet_simple(input):
	'''分解网络实现函数'''
    with tf.variable_scope('DecomNet', reuse=tf.AUTO_REUSE):
        conv1=slim.conv2d(input,32,[3,3], rate=1, activation_fn=lrelu,scope='g_conv1_1')
        pool1=slim.max_pool2d(conv1, [2, 2], stride = 2, padding='SAME' )
        conv2=slim.conv2d(pool1,64,[3,3], rate=1, activation_fn=lrelu,scope='g_conv2_1')
        pool2=slim.max_pool2d(conv2, [2, 2], stride = 2, padding='SAME' )
        conv3=slim.conv2d(pool2,128,[3,3], rate=1, activation_fn=lrelu,scope='g_conv3_1')
        up8 =  upsample_and_concat( conv3, conv2, 64, 128 , 'g_up_1')
        conv8=slim.conv2d(up8,  64,[3,3], rate=1, activation_fn=lrelu,scope='g_conv8_1')
        up9 =  upsample_and_concat( conv8, conv1, 32, 64 , 'g_up_2')
        conv9=slim.conv2d(up9,  32,[3,3], rate=1, activation_fn=lrelu,scope='g_conv9_1')
        # Here, we use 1*1 kernel to replace the 3*3 ones in the paper to get better results.
        conv10=slim.conv2d(conv9,3,[1,1], rate=1, activation_fn=None, scope='g_conv10')
        R_out = tf.sigmoid(conv10)

        l_conv2=slim.conv2d(conv1,32,[3,3], rate=1, activation_fn=lrelu,scope='l_conv1_2')
        l_conv3=tf.concat([l_conv2, conv9],3)
        # Here, we use 1*1 kernel to replace the 3*3 ones in the paper to get better results.
        l_conv4=slim.conv2d(l_conv3,1,[1,1], rate=1, activation_fn=None,scope='l_conv1_4')
        L_out = tf.sigmoid(l_conv4)

    return R_out, L_out

def Restoration_net(input_r, input_i):
	'''重建网络实现函数'''
    with tf.variable_scope('Restoration_net', reuse=tf.AUTO_REUSE):
        input_all = tf.concat([input_r,input_i], 3)
        
        conv1=slim.conv2d(input_all,32,[3,3], rate=1, activation_fn=lrelu,scope='de_conv1_1')
        conv1=slim.conv2d(conv1,32,[3,3], rate=1, activation_fn=lrelu,scope='de_conv1_2')
        pool1=slim.max_pool2d(conv1, [2, 2], padding='SAME' )

        conv2=slim.conv2d(pool1,64,[3,3], rate=1, activation_fn=lrelu,scope='de_conv2_1')
        conv2=slim.conv2d(conv2,64,[3,3], rate=1, activation_fn=lrelu,scope='de_conv2_2')
        pool2=slim.max_pool2d(conv2, [2, 2], padding='SAME' )

        conv3=slim.conv2d(pool2,128,[3,3], rate=1, activation_fn=lrelu,scope='de_conv3_1')
        conv3=slim.conv2d(conv3,128,[3,3], rate=1, activation_fn=lrelu,scope='de_conv3_2')
        pool3=slim.max_pool2d(conv3, [2, 2], padding='SAME' )

        conv4=slim.conv2d(pool3,256,[3,3], rate=1, activation_fn=lrelu,scope='de_conv4_1')
        conv4=slim.conv2d(conv4,256,[3,3], rate=1, activation_fn=lrelu,scope='de_conv4_2')
        pool4=slim.max_pool2d(conv4, [2, 2], padding='SAME' )

        conv5=slim.conv2d(pool4,512,[3,3], rate=1, activation_fn=lrelu,scope='de_conv5_1')
        conv5=slim.conv2d(conv5,512,[3,3], rate=1, activation_fn=lrelu,scope='de_conv5_2')

        up6 =  upsample_and_concat( conv5, conv4, 256, 512, 'up_6')

        conv6=slim.conv2d(up6,  256,[3,3], rate=1, activation_fn=lrelu,scope='de_conv6_1')
        conv6=slim.conv2d(conv6,256,[3,3], rate=1, activation_fn=lrelu,scope='de_conv6_2')

        up7 =  upsample_and_concat( conv6, conv3, 128, 256, 'up_7'  )
        conv7=slim.conv2d(up7,  128,[3,3], rate=1, activation_fn=lrelu,scope='de_conv7_1')
        conv7=slim.conv2d(conv7,128,[3,3], rate=1, activation_fn=lrelu,scope='de_conv7_2')

        up8 =  upsample_and_concat( conv7, conv2, 64, 128, 'up_8' )
        conv8=slim.conv2d(up8,  64,[3,3], rate=1, activation_fn=lrelu,scope='de_conv8_1')
        conv8=slim.conv2d(conv8,64,[3,3], rate=1, activation_fn=lrelu,scope='de_conv8_2')

        up9 =  upsample_and_concat( conv8, conv1, 32, 64, 'up_9' )
        conv9=slim.conv2d(up9,  32,[3,3], rate=1, activation_fn=lrelu,scope='de_conv9_1')
        conv9=slim.conv2d(conv9,32,[3,3], rate=1, activation_fn=lrelu,scope='de_conv9_2')

        conv10=slim.conv2d(conv9,3,[3,3], rate=1, activation_fn=None, scope='de_conv10')
    
        out = tf.sigmoid(conv10)
        return out

def Illumination_adjust_net(input_i, input_ratio):
	'''光照调节网络实现函数'''
    with tf.variable_scope('Illumination_adjust_net', reuse=tf.AUTO_REUSE):
        input_all = tf.concat([input_i, input_ratio], 3)
        
        conv1=slim.conv2d(input_all,32,[3,3], rate=1, activation_fn=lrelu,scope='en_conv_1')
        conv2=slim.conv2d(conv1,32,[3,3], rate=1, activation_fn=lrelu,scope='en_conv_2')
        conv3=slim.conv2d(conv2,32,[3,3], rate=1, activation_fn=lrelu,scope='en_conv_3')
        conv4=slim.conv2d(conv3,1,[3,3], rate=1, activation_fn=lrelu,scope='en_conv_4')

        L_enhance = tf.sigmoid(conv4)
    return L_enhance

```

##### 6.4 KinD 算法效果展示与分析

![](https://i-blog.csdnimg.cn/blog_migrate/dbbc5ffbf9a01a419793a2ef9425b494.jpeg#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/cc1380ef899389114d566e5a00f918f7.jpeg#pic_center)  
  上图展示了 KinD 算法的增强效果。第一张图像表示的是该算法和其它经典的增强算法之间的比较结果，**通过观察我们可以发现，通过该算法增强后的图像的更加明亮，看起来更加逼真，色彩基本上都正常的复原了。第二张图像表示的是该算法在几张真实的文档图片上面的增强效果，除了在第四列图像上面有一点问题，在其它图像上面都展现出来较好的增强效果。除此之外，需要提到的是该算法的运行速度并不快，2080Ti 上面都需要 50ms，都达不到实时，速度还待改进，作者出了一个改进版的 KinD++，具体的细节的效果请看**[该链接](https://github.com/zhangyhuaee/KinD_plus)。

### 7、EnlightenGAN 算法

[论文链接](https://arxiv.org/pdf/1906.06972.pdf) -[Github 链接](https://github.com/TAMU-VITA/EnlightenGAN)

##### 7.1 EnlightenGAN 算法简介

  EnlightenGAN 算法是一个高效的无监督生成对抗网络，它不需要使用低光照 / 正常图像块训练，大量实验结果表明该算法可以在很多测试图片中取得较好的泛化效果。**该算法使用从输入本身提取的信息来正则化非配对训练，论文中提出了一个全局 - 局部鉴别器结构、一个自正则感知损失融合与注意力机制。总而言之，这个算法开启了无监督图像增强的先河，在训练使用非配对的训练块，而且具有很好的泛化效果，它推动了 GAN 在图像增强问题上面的应用**。

##### 7.2 EnlightenGAN 网络简介

![](https://i-blog.csdnimg.cn/blog_migrate/b39eba94ebfc60e4f305cfdc6de172c5.jpeg#pic_center)  
  上图展示了 EnlightenGAN 网络的整体架构。**整个网络包括一个生成器和一个判别器，该生成器是一个带有残差连接的编解码网络，每一个卷积块包含两个 3x3 的卷积、一个 BN 和一个 LeakyRelu，每一个注意力模块是有特征映射和注意力映射相乘获得的；该网络包含一个全局判别器和一个局部判别器，全局判别器的输入是整张图片，它用来判别输入的图片是没有增强的还是增强过的，局部判别器的输入是一些 patch 块，用来判断这些 patch 是增强之后还是没有增强的。**

##### 7.3 EnlightenGAN 核心代码实现

```
def define_G(input_nc, output_nc, ngf, which_model_netG, norm='batch', use_dropout=False, gpu_ids=[], skip=False, opt=None):
	'''生成器网络代码实现'''
    netG = None
    use_gpu = len(gpu_ids) > 0
    norm_layer = get_norm_layer(norm_type=norm)

    if use_gpu:
        assert(torch.cuda.is_available())

    if which_model_netG == 'resnet_9blocks':
        netG = ResnetGenerator(input_nc, output_nc, ngf, norm_layer=norm_layer, use_dropout=use_dropout, n_blocks=9, gpu_ids=gpu_ids)
    elif which_model_netG == 'resnet_6blocks':
        netG = ResnetGenerator(input_nc, output_nc, ngf, norm_layer=norm_layer, use_dropout=use_dropout, n_blocks=6, gpu_ids=gpu_ids)
    elif which_model_netG == 'unet_128':
        netG = UnetGenerator(input_nc, output_nc, 7, ngf, norm_layer=norm_layer, use_dropout=use_dropout, gpu_ids=gpu_ids)
    elif which_model_netG == 'unet_256':
        netG = UnetGenerator(input_nc, output_nc, 8, ngf, norm_layer=norm_layer, use_dropout=use_dropout, gpu_ids=gpu_ids, skip=skip, opt=opt)
    elif which_model_netG == 'unet_512':
        netG = UnetGenerator(input_nc, output_nc, 9, ngf, norm_layer=norm_layer, use_dropout=use_dropout, gpu_ids=gpu_ids, skip=skip, opt=opt)
    elif which_model_netG == 'sid_unet':
        netG = Unet(opt, skip)
    elif which_model_netG == 'sid_unet_shuffle':
        netG = Unet_pixelshuffle(opt, skip)
    elif which_model_netG == 'sid_unet_resize':
        netG = Unet_resize_conv(opt, skip)
    elif which_model_netG == 'DnCNN':
        netG = DnCNN(opt, depth=17, n_channels=64, image_channels=1, use_bnorm=True, kernel_size=3)
    else:
        raise NotImplementedError('Generator model name [%s] is not recognized' % which_model_netG)
    if len(gpu_ids) >= 0:
        netG.cuda(device=gpu_ids[0])
        netG = torch.nn.DataParallel(netG, gpu_ids)
    netG.apply(weights_init)
    return netG


def define_D(input_nc, ndf, which_model_netD,
             n_layers_D=3, norm='batch', use_sigmoid=False, gpu_ids=[], patch=False):
    '''判别器网络代码实现'''
    netD = None
    use_gpu = len(gpu_ids) > 0
    norm_layer = get_norm_layer(norm_type=norm)

    if use_gpu:
        assert(torch.cuda.is_available())
    if which_model_netD == 'basic':
        netD = NLayerDiscriminator(input_nc, ndf, n_layers=3, norm_layer=norm_layer, use_sigmoid=use_sigmoid, gpu_ids=gpu_ids)
    elif which_model_netD == 'n_layers':
        netD = NLayerDiscriminator(input_nc, ndf, n_layers_D, norm_layer=norm_layer, use_sigmoid=use_sigmoid, gpu_ids=gpu_ids)
    elif which_model_netD == 'no_norm':
        netD = NoNormDiscriminator(input_nc, ndf, n_layers_D, use_sigmoid=use_sigmoid, gpu_ids=gpu_ids)
    elif which_model_netD == 'no_norm_4':
        netD = NoNormDiscriminator(input_nc, ndf, n_layers_D, use_sigmoid=use_sigmoid, gpu_ids=gpu_ids)
    elif which_model_netD == 'no_patchgan':
        netD = FCDiscriminator(input_nc, ndf, n_layers_D, use_sigmoid=use_sigmoid, gpu_ids=gpu_ids, patch=patch)
    else:
        raise NotImplementedError('Discriminator model name [%s] is not recognized' %
                                  which_model_netD)
    if use_gpu:
        netD.cuda(device=gpu_ids[0])
        netD = torch.nn.DataParallel(netD, gpu_ids)
    netD.apply(weights_init)
    return netD

```

##### 7.4 EnlightenGAN 算法效果展示与分析

![](https://i-blog.csdnimg.cn/blog_migrate/6c4982765588f0b1c3e4a4071a633acd.jpeg#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/f04be927f67f6c113fd00f664bc08027.jpeg#pic_center)  
  上图展示了 EnlightenGAN 算法的增强效果。**第一张证明了 EnlightenGAN 论文中提出的局部判别器和注意力模块的有效性，同时也体现出了 EnlightenGAN 算法的增强效果。第二张展示了该算法在真实场景中的一些图片上面的展示效果，整体看来该算法取得了较好的增强效果，但是在有些图像上面增强的结果不是很均匀，仍然存在着一些阴影区域，不过越来越多的基于 GAN 的图像增强算法会慢慢的解决这些问题**，敬请期待。

### 8、总结

  本文主要对几种经典的低光照图像算法进行了简单的介绍，但是并不代表当前只有这么多算法，我介绍的可能只是冰山一角，如果你对低光照图像增强感兴趣，请参考参考文献中的一些论文和资料。**总而言之，低光照图像增强算法在现实场景中会有很多的应用，比如低光照条件下面的检测、跟踪、行人重识别；通过图像增强来提升文本检测和文本识别的精度，更多的场景还需要你自己去挖掘和应用**。

### 参考资料

1、Learning-to-See-in-the-Dark- [论文链接](http://cchen156.web.engr.illinois.edu/paper/18CVPR_SID.pdf) -[Github 链接](https://github.com/cchen156/Learning-to-See-in-the-Dark) - [项目主页](http://cchen156.web.engr.illinois.edu/SID.html)  
2、DPED- [论文链接](https://arxiv.org/pdf/1704.02470.pdf) -[Github 链接](https://github.com/aiff22/DPED) - [项目主页](http://people.ee.ethz.ch/~ihnatova/)  
3、noteshrink-[Github 链接](https://github.com/mzucker/noteshrink)  
4、SICE- [论文链接](http://www4.comp.polyu.edu.hk/~cslzhang/paper/SICE.pdf) [Github 链接](https://github.com/csjcai/SICE)  
5、Attention-guided Low-light Image Enhancement- [论文链接](https://arxiv.org/pdf/1908.00682v2.pdf)  
6、部分低光照资料汇总 -[Github 链接](https://github.com/rockeyben/Low-Light)

### 注意事项

[1] 如果您对 AI、自动驾驶、AR、ChatGPT 等技术感兴趣，欢迎关注我的微信公众号 “**AI 产品汇**”，有问题可以在公众号中私聊我！  
[2] 该博客是本人原创博客，如果您对该博客感兴趣，想要转载该博客，请与我联系（qq 邮箱：1575262785@qq.com）, 我会在第一时间回复大家，谢谢大家的关注.  
[3] 由于个人能力有限，该博客可能存在很多的问题，希望大家能够提出改进意见。  
[4] 如果您在阅读本博客时遇到不理解的地方，希望您可以联系我，我会及时的回复您，和您交流想法和意见，谢谢。  
[5] 本文测试的图片可以通过该链接进行下载。[网盘链接](https://pan.baidu.com/s/1mlf_2zudoSvnMtQaElgVBA) - 提取码：x7ta。  
[5] **本人业余时间承接各种本科毕设设计和各种小项目，包括图像处理（数据挖掘、机器学习、深度学习等）、matlab 仿真、python 算法及仿真等，有需要的请加 QQ：1575262785 详聊，备注 “项目”！！！**