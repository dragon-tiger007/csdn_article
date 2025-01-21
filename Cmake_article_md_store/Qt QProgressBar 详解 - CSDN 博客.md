> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/wzz953200463/article/details/125530997?ops_request_misc=%257B%2522request%255Fid%2522%253A%25220f88ce8e399f330e5bb36b0bb0499760%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=0f88ce8e399f330e5bb36b0bb0499760&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-125530997-null-null.142^v101^pc_search_result_base2&utm_term=qprocessbar%E6%A0%B7%E5%BC%8F%E8%A1%A8&spm=1018.2226.3001.4187)

#### 1.QProgressBar 简述

QProgressBar 提供了一个水平或垂直的进度条，可以使用 setMinimum() 和 setMaximum 指定最小和最大步数。当前的步数是用 setValue() 设置的。进度条可以用 reset() 重绕到开头。

#### 2. 常用方法

<table border="1" cellpadding="1" cellspacing="1"><tbody><tr><td>void&nbsp;setMaximum(int maximum)</td><td>设置最大值</td></tr><tr><td>void&nbsp;setMinimum(int minimum)</td><td>设置最小值</td></tr><tr><td>void&nbsp;setRange(int minimum, int maximum)</td><td>设置范围，最大、最小值</td></tr><tr><td>void&nbsp;setValue(int value)</td><td>设置当前值</td></tr><tr><td>void&nbsp;reset()</td><td>重置</td></tr><tr><td>void&nbsp;setOrientation(Qt::Orientation)</td><td>设置方向，垂直，水平</td></tr><tr><td>void&nbsp;setAlignment(Qt::Alignment alignment)</td><td>设置对齐方式，居中，左、右</td></tr><tr><td>void&nbsp;setTextVisible(bool visible)</td><td>设置进度条文本是否显示</td></tr><tr><td>void&nbsp;setInvertedAppearance(bool invert)</td><td>设置正、反</td></tr><tr><td>void&nbsp;setFormat(const QString &amp;format)</td><td>设置文本显示格式</td></tr></tbody></table>

#### 3. 示例，比较进度条

![](https://i-blog.csdnimg.cn/blog_migrate/02c9700e605a8d29633ccb0cf150f3c8.gif)

**p1 设置如下，正常设置。**

```
    ui->progressBar1->setMinimum(0);
    ui->progressBar1->setMaximum(100);
    ui->progressBar1->setValue(50);
    ui->progressBar1->setOrientation(Qt::Horizontal);
```

**p2 设置如下，设置了文字对齐方式，进度条方向等。**

```
    ui->progressBar2->setMinimum(0);
    ui->progressBar2->setMaximum(100);
    ui->progressBar2->setValue(50);
    ui->progressBar2->setOrientation(Qt::Horizontal);
    ui->progressBar2->setInvertedAppearance(true);//设置反方向
    ui->progressBar2->setFormat("%v");
    ui->progressBar2->setAlignment(Qt::AlignLeft | Qt::AlignVCenter);  // 对齐方式
```

setFormat()，有如下几种方式

<table border="1" cellpadding="1" cellspacing="1"><tbody><tr><td><p>%p%&nbsp;&nbsp;</p></td><td>百分比，这是默认的显示方式</td></tr><tr><td>%v&nbsp; &nbsp; &nbsp;</td><td>当前进度</td></tr><tr><td>%m&nbsp; &nbsp;&nbsp;</td><td>总步数</td></tr></tbody></table>

也可以直接设置显示的值，如下图所示，显示小数。

![](https://i-blog.csdnimg.cn/blog_migrate/e51831a9f9adc964ed690030445d9900.png)

```
ui->progressBar1->setAlignment(Qt::AlignLeft | Qt::AlignVCenter);  // 对齐方式
 
ui->progressBar1->setFormat(QString("cur progress value:%1%").arg(QString::number(50.43, 'f', 2)));
```

**p3 设置如下，繁忙进度显示。**

只需设置最大值、最小值为 0 就行了。

```
    ui->progressBar3->setMinimum(0);
    ui->progressBar3->setMaximum(0);
```

#### 4. 设置[样式表](https://so.csdn.net/so/search?q=%E6%A0%B7%E5%BC%8F%E8%A1%A8&spm=1001.2101.3001.7020)

这里简单设置一下样式表。效果如下，仅供参考。

![](https://i-blog.csdnimg.cn/blog_migrate/ff4318ce9b9d7394c3b29c9bb744a5b8.png)

```
QString s1 = "QProgressBar {\
    border: 2px solid grey;\
    border-radius: 5px;\
    text-align: center;\
    color:#ff0000;\
}";
 
QString s2 = "QProgressBar::chunk {\
    background-color: #05B8CC;\
    width: 20px;\
    margin: 0.5px;\
}";
```

调用

```
ui->progressBar1->setStyleSheet(s1+s2);

```