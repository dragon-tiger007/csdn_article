> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_37529913/article/details/128312187)

       本文详细的介绍了 [QStackedWidget](https://so.csdn.net/so/search?q=QStackedWidget&spm=1001.2101.3001.7020) 控件的各种操作，例如：[新建界面](#1%20%E6%96%B0%E5%BB%BA%E7%95%8C%E9%9D%A2)、[页面切换](#2%20%E9%A1%B5%E9%9D%A2%E5%88%87%E6%8D%A2)、[添加页面](#3%20%E6%B7%BB%E5%8A%A0%E9%A1%B5%E9%9D%A2)、[addWidget](#4.1%20addWidget)、[count](#%C2%A04.2%20count)、[currentIndex](#4.3%20currentIndex%C2%A0%C2%A0) 、[currentWidget](#4.4%C2%A0currentWidget)、[indexOf](#4.5%C2%A0%20indexOf)、[insertWidget](#%C2%A04.6%20insertWidget)、[removeWidget](#4.7%20removeWidget)、[widget](#4.8%20widget)、[setCurrentIndex 槽函数](#5.1%20setCurrentIndex)、[setCurrentWidget 槽函数、](#5.2%20setCurrentWidget)[currentChanged 信号、](#6.1%20currentChanged)[widgetRemoved 信号、](#6.2%20widgetRemoved) [](#6.1%20currentChanged) [样式表](https://so.csdn.net/so/search?q=%E6%A0%B7%E5%BC%8F%E8%A1%A8&spm=1001.2101.3001.7020)等操作。  
本系列 QT 全面详解文章目前共有十八篇，本系列文章较为详细的讲述了 QT 控件的基础操作和使用，也谢谢大家的关注、点赞、收藏。

![](https://i-blog.csdnimg.cn/blog_migrate/dd6a729e482f10da19d1415979a739f0.png)

>  本文作者原创，转载请附上文章出处与本文链接。

**QT QStackedWidget 控件 使用详解目录**

[1 新建界面](#t0)

[2 页面切换](#t1)

[2.1 插入第三页](#t2)

 [2.2 代码实现](#t3)

[3 添加页面](#t4)

[3.1 新建界面](#t5)

[3.2 代码实现](#t6)

[4 所有函数](#t7)

[4.1 addWidget](#t8)

 [4.2 count](#t9)

[4.3 currentIndex](#t10) 

[4.4 currentWidget](#t11)

[4.5  indexOf](#t12)

 [4.6 insertWidget](#t13)

[4.7 removeWidget](#t14)

[4.8 widget](#t15)

[5 槽函数](#t16)

[5.1 setCurrentIndex](#t17)

[5.2 setCurrentWidget](#t18)

[6 信号槽](#t19)

[6.1 currentChanged](#t20)

[6.2 widgetRemoved](#t21)

 [7 .h](#t22)

[8 .cpp](#t23)

[9 样式表](#t24)

[10 其它文章 ：](#t25)

1 新建界面
------

![](https://i-blog.csdnimg.cn/blog_migrate/2f60054d2d62451340cd4fe3c97eae2e.png)

2 页面切换
------

### 2.1 插入第三页

![](https://i-blog.csdnimg.cn/blog_migrate/2f0be6bb26c91e58256900016da74b01.png)

###  2.2 代码实现

![](https://i-blog.csdnimg.cn/blog_migrate/6e5b007219ec1cb932fcdc960daf2a24.gif)

```
 
//    connect(ui->pushButton, &QPushButton::clicked, [=]() {
//        ui->stackedWidget->setCurrentIndex(1);
//        });
 
//    connect(ui->pushButton_2, &QPushButton::clicked, [=]() {
//        ui->stackedWidget->setCurrentIndex(2);
//        });
 
//    connect(ui->pushButton_3, &QPushButton::clicked, [=]() {
//        ui->stackedWidget->setCurrentIndex(0);
//        });
}
 
MainWindow::~MainWindow()
{
    delete ui;
}
 
 
void MainWindow::switchPage(){
    QPushButton *button = qobject_cast<QPushButton*>(sender());
    if(button==ui->pushButton)
        ui->stackedWidget->setCurrentIndex(0);
    else if(button==ui->pushButton_2)
        ui->stackedWidget->setCurrentIndex(1);
    else if(button==ui->pushButton_3)
        ui->stackedWidget->setCurrentIndex(2);
 
    int i = 0;
    ui->stackedWidget->widget(i);
}
 
 
void MainWindow::on_pushButton_clicked()
{
    switchPage();
}
 
void MainWindow::on_pushButton_2_clicked()
{
    switchPage();
}
 
void MainWindow::on_pushButton_3_clicked()
{
    switchPage();
}
```

3 添加页面
------

### 3.1 新建界面

        新建一个 QT 设计师界面        newWidgetDialog

![](https://i-blog.csdnimg.cn/blog_migrate/d41e3a1b8db0279f92d7c1fcc9aede44.png)

### 3.2 代码实现

```
#include "newwidgetdialog.h"
 
newWidgetDialog *widgetDialog;
 
widgetDialog = new newWidgetDialog();
qDebug() << "页面索引: " << ui->stackedWidget->addWidget(widgetDialog);
 
 
 ui->stackedWidget->setCurrentIndex(3);
```

![](https://i-blog.csdnimg.cn/blog_migrate/c806bba9fc3e88d7ec7763993b781369.png)

4 所有函数
------

### 4.1 [addWidget](https://so.csdn.net/so/search?q=addWidget&spm=1001.2101.3001.7020)

        添加页面，并返回页面对应的索引

```
qDebug() << "页面索引: " << ui->stackedWidget->addWidget(widgetDialog);

```

![](https://i-blog.csdnimg.cn/blog_migrate/a40c6d8763807f7ed9a032929043167d.png)

###  4.2 count

        获取页面数量

```
qDebug() << "页面索引总数: " << ui->stackedWidget->count();

```

![](https://i-blog.csdnimg.cn/blog_migrate/4909787ddd081ad1fc0c7afd5dd97dac.png)

### 4.3 currentIndex  

        获取当前页面的索引

```
qDebug() << "获取当前页面的索引: " << ui->stackedWidget->currentIndex();

```

### ![](https://i-blog.csdnimg.cn/blog_migrate/e90c2f70d30ed3439c43d993893e5419.png)

### 4.4 currentWidget

        获取当前页面

```
qDebug() << "获取当前页面: " << ui->stackedWidget->currentWidget();

```

![](https://i-blog.csdnimg.cn/blog_migrate/7ae2facd520e975beb9d30dac101200f.png)

### 4.5  indexOf

        获取 QWidget 页面所对应的索引

```
qDebug() << "获取QWidget页面所对应的索引: " << ui->stackedWidget->indexOf(ui->page);

```

![](https://i-blog.csdnimg.cn/blog_migrate/aec0de3034ee5f33ad7d25d6ef635be8.png)

###  4.6 insertWidget

        在索引 index 位置添加页面

```
    /* 在当前索引位置添加页面 (添加完后原有页面向后挪动) */
    ui->stackedWidget->insertWidget(0,widgetDialog);
```

### 4.7 removeWidget

        移除 QWidget 页面，并没有被删除，只是从布局中移动，从而被隐藏。

```
 ui->stackedWidget->removeWidget(ui->page);
 
 
 
/* 在当前索引位置添加页面 (添加完后原有页面向后挪动) */
ui->stackedWidget->insertWidget(0,ui->page);
```

### 4.8 widget

        返回索引值为 index 的组件

```
    qDebug() << ui->stackedWidget->widget(0);
    qDebug() << ui->stackedWidget->widget(100);
```

![](https://i-blog.csdnimg.cn/blog_migrate/adeafd56b5ea111356484a5f3bed89f0.png)

5 槽函数
-----

.        槽函数有两个，分为 setCurrentWidget 、 setCurrentIndex 一个是根据索引来显示页面，一个是根据窗口部件来显示页面

### 5.1 setCurrentIndex

        索引设置当前显示页

```
ui->stackedWidget->setCurrentIndex(2);

```

### 5.2 setCurrentWidget

        窗口部件设置当前显示页

```
ui->stackedWidget->setCurrentWidget(ui->page_2);

```

6 信号槽
-----

### 6.1 currentChanged

        当前页面发生变化时候发射，index 为新的索引值

### 6.2 widgetRemoved

        页面被移除时候发射，index 为页面对应的索引值

```
private slots:
 
    void currentChanged(int index);
 
    void widgetRemoved(int index);
 
connect(ui->stackedWidget, SIGNAL(currentChanged(int)), SLOT(currentChanged(int )));
connect(ui->stackedWidget, SIGNAL(widgetRemoved(int)), SLOT(widgetRemoved(int )));
 
 
 
void MainWindow::currentChanged(int index)
{
    qDebug() << "页面发生变化: " << index;
}
 
void MainWindow::widgetRemoved(int index)
{
    qDebug() << "页面被移除: " << index;
}
```

![](https://i-blog.csdnimg.cn/blog_migrate/2edbfce6009f24796a74303d0b1cca8e.png)

 7 .h
-----

```
/******************************************************************************
 * Copyright CSDN 双子座断点 Co., Ltd.
 * Copyright www.dreambeging.vip Co., Ltd.
 * All right reserved. See COPYRIGHT for detailed Information.
 *
 * @file       mainwindow.h
 * @project    stackedWidget_Test
 * @version    V 1.0
 *
 * @author     断点<dream.2017@qq.com>
 * @date       2022/12/14
 * @history
 *****************************************************************************/
 
#ifndef MAINWINDOW_H
#define MAINWINDOW_H
 
#include <QMainWindow>
#include <QDebug>
#include "newwidgetdialog.h"
 
#pragma execution_character_set("utf-8")
QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE
 
class MainWindow : public QMainWindow
{
    Q_OBJECT
 
public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();
 
private slots:
    void on_pushButton_clicked();
 
    void on_pushButton_2_clicked();
 
    void on_pushButton_3_clicked();
 
    void on_pushButton_4_clicked();
 
    void currentChanged(int index);
 
    void widgetRemoved(int index);
 
 
private:
    Ui::MainWindow *ui;
 
    void switchPage();
 
 
    newWidgetDialog *widgetDialog;
 
    QString Title;
    QString Version;
    QString BlogText;
};
#endif // MAINWINDOW_H
```

8 .cpp
------

```
/******************************************************************************
 * Copyright CSDN 双子座断点 Co., Ltd.
 * Copyright www.dreambeging.vip Co., Ltd.
 * All right reserved. See COPYRIGHT for detailed Information.
 *
 * @file       mainwindow.cpp
 * @project    stackedWidget_Test
 * @version    V 1.0
 *
 * @author     断点<dream.2017@qq.com>
 * @date       2022/12/14
 * @history
 *****************************************************************************/
 
#include "mainwindow.h"
#include "ui_mainwindow.h"
 
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
 
    ui->stackedWidget->setStyleSheet("#stackedWidget{border:2px solid rgb(45,226,42);"
                                                "border-top-left-radius: 5px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 5px;}");
 
 
    Title = "QT QStackedWidget  CSDN 双子座断点 ";
    Version = "V 1.0 ";
    BlogText = "https://blog.csdn.net/qq_37529913?type=lately/";
    setWindowTitle(Title + Version + BlogText);
 
 
    widgetDialog = new newWidgetDialog();
    qDebug() << "页面索引: " << ui->stackedWidget->addWidget(widgetDialog);
 
 
    connect(ui->stackedWidget, SIGNAL(currentChanged(int)), SLOT(currentChanged(int )));
    connect(ui->stackedWidget, SIGNAL(widgetRemoved(int)), SLOT(widgetRemoved(int )));
 
 
 
 
//    connect(ui->pushButton, &QPushButton::clicked, [=]() {
//        ui->stackedWidget->setCurrentIndex(1);
//        });
 
//    connect(ui->pushButton_2, &QPushButton::clicked, [=]() {
//        ui->stackedWidget->setCurrentIndex(2);
//        });
 
//    connect(ui->pushButton_3, &QPushButton::clicked, [=]() {
//        ui->stackedWidget->setCurrentIndex(0);
//        });
}
 
MainWindow::~MainWindow()
{
    delete ui;
}
 
 
void MainWindow::currentChanged(int index)
{
    qDebug() << "页面发生变化: " << index;
}
 
void MainWindow::widgetRemoved(int index)
{
    qDebug() << "页面被移除: " << index;
}
 
 
 
void MainWindow::switchPage(){
    QPushButton *button = qobject_cast<QPushButton*>(sender());
    if(button==ui->pushButton)
        ui->stackedWidget->setCurrentIndex(0);
    else if(button==ui->pushButton_2)
        ui->stackedWidget->setCurrentIndex(1);
    else if(button==ui->pushButton_3)
        ui->stackedWidget->setCurrentIndex(2);
 
    int i = 0;
    ui->stackedWidget->widget(i);
}
 
 
void MainWindow::on_pushButton_clicked()
{
    switchPage();
}
 
void MainWindow::on_pushButton_2_clicked()
{
    switchPage();
}
 
void MainWindow::on_pushButton_3_clicked()
{
    switchPage();
}
 
void MainWindow::on_pushButton_4_clicked()
{
 
    //ui->stackedWidget->setCurrentIndex(3);
 
 
    //qDebug() << "页面索引总数: " << ui->stackedWidget->count();
 
   // qDebug() << "获取当前页面的索引: " << ui->stackedWidget->currentIndex();
 
    //qDebug() << "获取当前页面: " << ui->stackedWidget->currentWidget();
 
    //qDebug() << "获取QWidget页面所对应的索引: " << ui->stackedWidget->indexOf(ui->page);
 
    /* 在当前索引位置添加页面 (添加完后原有页面向后挪动) */
    //ui->stackedWidget->insertWidget(0,widgetDialog);
 
    ui->stackedWidget->removeWidget(ui->page_3);
 
//    qDebug() << ui->stackedWidget->widget(0);
//    qDebug() << ui->stackedWidget->widget(100);
 
//    ui->stackedWidget->insertWidget(0,ui->page);
 
    //qDebug() << "页面索引总数: " << ui->stackedWidget->stackingMode();
 
    //qDebug() << "获取可见索引: " << ui->stackedWidget->currentWidget();
 
 
 
    //ui->stackedWidget->setCurrentIndex(2);
 
    //ui->stackedWidget->setCurrentWidget(ui->page_2);
}
```

9 样式表
-----

QT 控件重绘_双子座断点的博客 - CSDN 博客_qt 重绘

QT 样式表_双子座断点的博客 - CSDN 博客

QT 样式表属性完整版_双子座断点的博客 - CSDN 博客

Qt 系统字体_双子座断点的博客 - CSDN 博客

  
10 其它文章 ：
------------

QT TextEdit 控件_双子座断点的博客 - CSDN 博客_qt textedit

QT QComboBox 使用详解_双子座断点的博客 - CSDN 博客

QT QtableView 操作详解_双子座断点的博客 - CSDN 博客_qtableview 增删改查

Qt QStandardItemModel(1. 超级详细用法)_双子座断点的博客 - CSDN 博客_qstandardmodel

Qt QStandardItemModel(2. 超级详细函数)_双子座断点的博客 - CSDN 博客_qstandarditemmodel 点击事件

QT QRadioButton 使用详解_双子座断点的博客 - CSDN 博客_qt radiobutton

QT QLineEdit 使用详解_双子座断点的博客 - CSDN 博客_qt qlineedit

Qt QMessageBox 使用详解_双子座断点的博客 - CSDN 博客_qt message

QChart 折线图、饼状图、条形图、曲线图_双子座断点的博客 - CSDN 博客_qchart 样式

QChart 属性详解_双子座断点的博客 - CSDN 博客_setanimationoptions

QCharts QValueAxis 使用_双子座断点的博客 - CSDN 博客_qvalueaxis

Qt 5 等待提示框 (开源 动态图)_双子座断点的博客 - CSDN 博客_qt 等待对话框

QtDataVisualization 数据 3D 可视化_双子座断点的博客 - CSDN 博客_qtdatavisualizatio

QT QSpinBox 整数计数器控件 使用详解_双子座断点的博客 - CSDN 博客

  
QT QDoubleSpinBox 浮点计数器控件 (使用详解)_双子座断点的博客 - CSDN 博客_qdoublespinbox 信号槽

  
[QT QSlider、QHorizontalSlider、QVerticalSlider 控件 使用详解_双子座断点的博客 - CSDN 博客](https://blog.csdn.net/qq_37529913/article/details/128228223?spm=1001.2014.3001.5501 "QT QSlider、QHorizontalSlider、QVerticalSlider 控件 使用详解_双子座断点的博客-CSDN博客")