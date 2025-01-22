> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_55735677/article/details/129612500)

首先说方法，在给 widget 或者 frame 或者其他任何类型的控件添加背景图时，在[样式表](https://so.csdn.net/so/search?q=%E6%A0%B7%E5%BC%8F%E8%A1%A8&spm=1001.2101.3001.7020)中加入如下代码，指定某个控件，设置其背景。

```
类名 # 控件名
{
填充方式：图片路径
}
 
例如：
QWidget#Widget
{
    border-image: url(:/resource/bg2.png);
}
或者
QFrmae#frame
{
    border-image: url(:/resource/bg2.png);
}
```

如果单纯改变样式表，没有指定控件的话，内部的其他控件背景也会改变。

特别提醒：类名 # 控件名，其中控件名要准确，假如你把 widget 的名字改成了其他，那么这里的控件名要一致。

![](https://i-blog.csdnimg.cn/blog_migrate/827af95dac1d4cbde5fdadebcac419bd.png)

错误示范：

![](https://i-blog.csdnimg.cn/blog_migrate/fe37c5ee76de837a46f8aea3213929aa.png)

如图：效果非常杂乱。

![](https://i-blog.csdnimg.cn/blog_migrate/d0a3deaac23299aed0f8454207f57426.png)

正确示范：

![](https://i-blog.csdnimg.cn/blog_migrate/dd54291647ff31f6ef6160eddfa59bbb.png)

效果：只有指定的 widget 背景改变，widget 内部控件背景不变

![](https://i-blog.csdnimg.cn/blog_migrate/56e3fd0b084067c355b319b5b7617344.png)