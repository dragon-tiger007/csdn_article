> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/pengrunxin/article/details/121431619?spm=1001.2014.3001.5506)

#### QSlider 的样式表的各项设置含义

*   [qslider 的构成（三部分）](#qslider_1)
*   [qslider 每部分构造要素](#qslider_19)
*   [仿照 QQ 音乐播放器的滑条样式表](#QQ_30)
*   [QSlider 样式表使用](#QSlider_101)
*   [QT 官方 QSlider 样式表例子](#QTQSlider_106)

qslider 的构成（三部分）
----------------

首先，我们要明白 qslider 的构成有哪些：通常为三种：

*   滑动过的槽；
    
*   未滑动过槽；
    
*   中间的滑块（分为两种）-》手动拉动时显示的滑块，平时滑动的滑块；
    
    对应样式表中的：  
    /_1. 滑动过的槽_ /  
    QSlider::sub-page:horizontal  
    /_2. 未滑动过槽_ /  
    QSlider::add-page:horizontal  
    /_3. 平时滑动的滑块_ /  
    QSlider::handle:horizontal  
    /_4. 手动拉动时显示的滑块_ /  
    QSlider::handle:horizontal:hover
    

图片:  
![](https://i-blog.csdnimg.cn/blog_migrate/db1238bde5f1786422d85ffaa92133a7.png#pic_center)

qslider 每部分构造要素
---------------

那每部分构造怎么设计？其实 QSlider 三部分的构成要素是类似的，这就极度方便我们开发，每部分构造主要分为 5 部分。

*   background: / _背景（颜色）_/
*   margin-top: / _上区域（遮住高度）_/
*   margin-bottom: / _下区域（遮住高度）_
*   border: / _外围环（颜色，厚度）_/
*   border-radius: / _外围环圆角角度_ /
*   width: / _宽度（滑环有效）（滑槽设置无效）_/

图片：  
![](https://i-blog.csdnimg.cn/blog_migrate/3c8e0a4ab0665875e91360a86a66c1a8.png#pic_center)![](https://i-blog.csdnimg.cn/blog_migrate/e8f1d1453516945387ffc3d9ee60a09e.png#pic_center)

仿照 QQ 音乐播放器的滑条样式表
-----------------

效果：  
![](https://i-blog.csdnimg.cn/blog_migrate/7c28a8cef52ee56f8ba564ee16bc914b.gif#pic_center)代码：

```
/*horizontal ：水平QSlider*/
QSlider::groove:horizontal {
border: 0px solid #bbb;
}

/*1.滑动过的槽设计参数*/
QSlider::sub-page:horizontal {
 /*槽颜色*/
background: rgb(90,49,255);
 /*外环区域倒圆角度*/
border-radius: 2px;
 /*上遮住区域高度*/
margin-top:8px;
 /*下遮住区域高度*/
margin-bottom:8px;
/*width在这里无效，不写即可*/
}

/*2.未滑动过的槽设计参数*/
QSlider::add-page:horizontal {
/*槽颜色*/
background: rgb(255,255, 255);
/*外环大小0px就是不显示，默认也是0*/
border: 0px solid #777;
/*外环区域倒圆角度*/
border-radius: 2px;
 /*上遮住区域高度*/
margin-top:9px;
 /*下遮住区域高度*/
margin-bottom:9px;
}
 
/*3.平时滑动的滑块设计参数*/
QSlider::handle:horizontal {
/*滑块颜色*/
background: rgb(193,204,208);
/*滑块的宽度*/
width: 5px;
/*滑块外环为1px，再加颜色*/
border: 1px solid rgb(193,204,208);
 /*滑块外环倒圆角度*/
border-radius: 2px; 
 /*上遮住区域高度*/
margin-top:6px;
 /*下遮住区域高度*/
margin-bottom:6px;
}

/*4.手动拉动时显示的滑块设计参数*/
QSlider::handle:horizontal:hover {
/*滑块颜色*/
background: rgb(193,204,208);
/*滑块的宽度*/
width: 10px;
/*滑块外环为1px，再加颜色*/
border: 1px solid rgb(193,204,208);
 /*滑块外环倒圆角度*/
border-radius: 5px; 
 /*上遮住区域高度*/
margin-top:4px;
 /*下遮住区域高度*/
margin-bottom:4px;
}

```

自己看一遍代码，看看每一步骤是干什么的，自己动手码码试试，超简单，一遍就会，什么样式都可以自己设计了。

QSlider 样式表使用
-------------

ui 界面设置样式表：点击 qslider 控件 + 鼠标右键打开 + 改变样式表 + 复制代码到里面就 OK 了  
![](https://i-blog.csdnimg.cn/blog_migrate/50f47dbf4e092d0424f073afb5d8d0f7.png#pic_center)直接设置（比较麻烦，容易出错，请注意使用格式！）  
![](https://i-blog.csdnimg.cn/blog_migrate/c0905d2e8cfa81974ad40f4243a93ac3.png#pic_center)

QT 官方 QSlider 样式表例子
-------------------

```
qthelp://org.qt-project.qtwidgets.5111/qtwidgets/stylesheet-examples.html#customizing-qslider

```

我介绍都是按照自己理解翻译过来，有些土白，你们可以自己去上面 QT 官方 QSlider 的例子中，自己加强理解。

都看到这里，来都来了，不点赞关注下，有什么问题可以在评论区提问