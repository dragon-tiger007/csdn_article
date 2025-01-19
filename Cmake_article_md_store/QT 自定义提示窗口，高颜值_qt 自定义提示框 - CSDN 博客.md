> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_56533319/article/details/142416522?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522de02337a183fb7c2a65ff61c220088da%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=de02337a183fb7c2a65ff61c220088da&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-4-142416522-null-null.142^v101^pc_search_result_base2&utm_term=qt%E5%BC%B9%E7%AA%97%E8%AE%BE%E8%AE%A1&spm=1018.2226.3001.4187)

一 环境展示
------

基本通用，如果不能用，部分函数自行百度替代即可。

![](https://i-blog.csdnimg.cn/direct/c6c30946173142c28fa1b37373770614.png)

二 效果展示
------

![](https://i-blog.csdnimg.cn/direct/40990569d65148cba943cf7e60812de5.gif)
--------------------------------------------------------------------------

三 创建 QDialog 类
--------------

这个类的好处就是可以直接性的阻塞窗口，下去可以自行研究

![](https://i-blog.csdnimg.cn/direct/efeb08cd7b7649e58c189eb491846628.png)

### 1 右击项目点击添加

![](https://i-blog.csdnimg.cn/direct/0626aad3c79b49dcb46204446da17ea3.png)

### 2 点击 qt 添加 Qt 设计师界面类

![](https://i-blog.csdnimg.cn/direct/46296d57094d49a49ad4085ac23b7a5b.png)

### 3 选择 Dialog without button

![](https://i-blog.csdnimg.cn/direct/347b4f0e809448eeab17fac0bdce68e5.png)

### 4 自定义类名，此处为 Custom_warning

![](https://i-blog.csdnimg.cn/direct/1ced343b03fc4588a32d072b3e650205.png)

四 [UI 设计](https://so.csdn.net/so/search?q=UI%E8%AE%BE%E8%AE%A1&spm=1001.2101.3001.7020)
---------------------------------------------------------------------------------------

### 1 界面设计

此处 UI 设计比较简单

![](https://i-blog.csdnimg.cn/direct/b7a72627788a44e0af6236045b9ebda9.png)

### 2 图片资源

由于[文件加密](https://so.csdn.net/so/search?q=%E6%96%87%E4%BB%B6%E5%8A%A0%E5%AF%86&spm=1001.2101.3001.7020)，此处分享不出来。可以去矢量图网去查找自己想要的。

此处推荐：[iconfont - 阿里巴巴矢量图标库](https://www.iconfont.cn/?spm=a313x.manage_type_mylikes.i3.2.2fcc3a817XaHZe "iconfont-阿里巴巴矢量图标库")

![](https://i-blog.csdnimg.cn/direct/ffe3117a16354b74bc4db3bc2152cf90.png)![](https://i-blog.csdnimg.cn/direct/bf889f1059954efb84e1122bc4163b44.png)

五 源代码
-----

### 1 H 文件

```
#ifndef CUSTOM_WARNING_H
#define CUSTOM_WARNING_H
 
#include <QWidget>
#include <QMouseEvent>
#include <QPainter>
#include <QDebug>
#include <QDialog>
 
namespace Ui {
class Custom_warning;
}
 
class Custom_warning : public QDialog
{
    Q_OBJECT
 
public:
    enum PUSHBUTTON_NAME{
        OK,
        NO,
        YES,
    };
 
    explicit Custom_warning(QString text, Custom_warning::PUSHBUTTON_NAME _type, QWidget *parent = nullptr);
    ~Custom_warning();
 
    static int Display(QString text, Custom_warning::PUSHBUTTON_NAME type,QWidget *parent = nullptr);
    void Ui_Init();
 
 
 
    QString label_text;
    PUSHBUTTON_NAME type;
    int ret_type = NO;
 
 
/*--------------无边框全部参数---------------------*/
private slots:
    void on_pushButton_close_clicked();
 
    void on_pushButton_clicked();
 
private:
    int MARGIN = 0;     //可以修改大小的距离
    bool _isleftpressed = false; //判断是否是左键点击
    int _curpos = 0;    //鼠标左键按下时光标所在区域
    QPoint _plast;      //获取鼠标左键按下时光标在全局(屏幕而非窗口)的位置
 
 
    int countRow(QPoint p);
    int countFlag(QPoint p, int row);
    void setCursorType(int flag);
    void System_Prompt();
    void paintEvent(QPaintEvent *event);
    void mouseMoveEvent(QMouseEvent *event);
    void mouseReleaseEvent(QMouseEvent *event);
    void mousePressEvent(QMouseEvent *event);
    void Maximization_And_Recovery();
private:
    Ui::Custom_warning *ui;
};
 
#endif // CUSTOM_WARNING_H
```

### 2 CPP 文件

```
#include "custom_warning.h"
#include "ui_custom_warning.h"
 
Custom_warning::Custom_warning(QString _text, Custom_warning::PUSHBUTTON_NAME _type, QWidget *parent) :
    QDialog(parent),
    label_text(_text), type(_type), ui(new Ui::Custom_warning)
{
    ui->setupUi(this);
 
    Ui_Init();
 
 
}
 
Custom_warning::~Custom_warning()
{
    delete ui;
}
 
/**
* @brief UI init
* @retval None
*/
void Custom_warning::Ui_Init(void)
{
    this->setWindowFlags(Qt::FramelessWindowHint);      //设置为无边框窗口
    this->setAttribute(Qt::WA_TranslucentBackground);   //设置为透明
 
    //禁用右键菜单
    ui->label_icon->setContextMenuPolicy(Qt::NoContextMenu);
    ui->label_text->setContextMenuPolicy(Qt::NoContextMenu);
    setWindowModality(Qt::ApplicationModal); //阻塞除当前窗体之外的所有的窗体
 
    QPixmap originalPixmap(":/pages/custom_control/Custom_warning/messageBox_warning.png"); // 加载原始图片
    QPixmap scaledPixmap = originalPixmap.scaled(40, 40, Qt::KeepAspectRatio, Qt::SmoothTransformation); // 平滑缩放图片保持长宽比
    ui->label_icon->setPixmap(scaledPixmap); // 设置缩放后的图片给 QLabel
 
    ui->pushButton_close->setIcon(QIcon(":/window_close.png"));
 
    ui->label_text->setText(label_text);
 
    switch (type)
    {
        case PUSHBUTTON_NAME::OK:
            ui->pushButton->setText("OK");
            break;
        case PUSHBUTTON_NAME::NO:
            ui->pushButton->setText("NO");
            break;
        case PUSHBUTTON_NAME::YES:
            ui->pushButton->setText("YES");
            break;
    }
    //提示声
    QApplication::beep();
}
 
 
/**
* @brief 获取输入框的值
* @param text:输入文本
* @param type:按键值
* @retval
*/
int Custom_warning::Display(QString text, Custom_warning::PUSHBUTTON_NAME type,QWidget *parent)
{
    Custom_warning *custom_warning = new Custom_warning(text, type ,parent);
 
    custom_warning->exec(); // 弹出模态对话框并等待用户操作
 
    return custom_warning->ret_type;
}
 
 
/**
* @brief 关闭 槽函数
* @retval None
*/
void Custom_warning::on_pushButton_close_clicked()
{
    this->close();
}
 
/**
* @brief 按钮 槽函数
* @retval None
*/
void Custom_warning::on_pushButton_clicked()
{
    this->close();
    ret_type = PUSHBUTTON_NAME::YES;
}
 
 
/*===========================================无边框初始化==================================================*/
//鼠标按下事件
/*
 *作用：
 *1.判断是否时左键点击 _isleftpressed
 *2.获取光标在屏幕中的位置 _plast
 *3.左键按下时光标所在区域 _curpos
 */
void Custom_warning::mousePressEvent(QMouseEvent *event)
{
    Q_UNUSED(event);
    if (event->button() == Qt::LeftButton)
    {
        this->_isleftpressed = true;
        QPoint temp = event->globalPos();
        _plast = temp;
        _curpos = countFlag(event->pos(), countRow(event->pos()));
    }
}
 
 
//鼠标释放事件
/*
 *作用：
 *1.将_isleftpressed 设为false
 *2.将光标样式恢复原样式  setCursor(Qt::ArrowCursor);
 */
void Custom_warning::mouseReleaseEvent(QMouseEvent *event)
{
    Q_UNUSED(event);
    if (_isleftpressed)
        _isleftpressed = false;
    setCursor(Qt::ArrowCursor);
}
 
 
//鼠标移动事件
void Custom_warning::mouseMoveEvent(QMouseEvent *event)
{
    Q_UNUSED(event);
    if(this->isFullScreen()) return;    //窗口铺满全屏，直接返回，不做任何操作
    int poss = countFlag(event->pos(), countRow(event->pos()));
    setCursorType(poss);
    if (_isleftpressed)//是否左击
    {
        QPoint ptemp = event->globalPos();
        ptemp = ptemp - _plast;
        if (_curpos == 22)//移动窗口
        {
            ptemp = ptemp + pos();
            move(ptemp);
        }
        else
        {
            QRect wid = geometry();
            switch (_curpos)//改变窗口的大小
            {
            case 11:wid.setTopLeft(wid.topLeft() + ptemp); break;//左上角
            case 13:wid.setTopRight(wid.topRight() + ptemp); break;//右上角
            case 31:wid.setBottomLeft(wid.bottomLeft() + ptemp); break;//左下角
            case 33:wid.setBottomRight(wid.bottomRight() + ptemp); break;//右下角
            case 12:wid.setTop(wid.top() + ptemp.y()); break;//中上角
            case 21:wid.setLeft(wid.left() + ptemp.x()); break;//中左角
            case 23:wid.setRight(wid.right() + ptemp.x()); break;//中右角
            case 32:wid.setBottom(wid.bottom() + ptemp.y()); break;//中下角
            }
            setGeometry(wid);
        }
        _plast = event->globalPos();//更新位置
    }
}
 
 
//获取光标在窗口所在区域的 列  返回行列坐标
int Custom_warning::countFlag(QPoint p,int row)//计算鼠标在哪一列和哪一行
{
    if(p.y()<MARGIN)
        return 10+row;
    else if(p.y()>this->height()-MARGIN)
        return 30+row;
    else
        return 20+row;
}
 
 
//获取光标在窗口所在区域的 行   返回行数
int Custom_warning::countRow(QPoint p)
{
    return (p.x()<MARGIN) ? 1 : (p.x()>(this->width() - MARGIN) ? 3 : 2);
}
 
 
//根据鼠标所在位置改变鼠标指针形状
void Custom_warning::setCursorType(int flag)
{
    switch(flag)
    {
    case 11:
    case 33:
        setCursor(Qt::SizeFDiagCursor);
        break;
    case 13:
    case 31:
        setCursor(Qt::SizeBDiagCursor);break;
    case 21:
    case 23:
        setCursor(Qt::SizeHorCursor);break;
    case 12:
    case 32:
        setCursor(Qt::SizeVerCursor);break;
    case 22:
        setCursor(Qt::ArrowCursor);
        QApplication::restoreOverrideCursor();//恢复鼠标指针性状
        break;
    }
}
 
 
/**
* @brief 重写绘图事件
* @retval None
*/
void Custom_warning::paintEvent(QPaintEvent *event)
{
    Q_UNUSED(event);
 
    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing, true); // 设置抗锯齿
//    painter.setPen(QPen(QColor(180, 179, 181), 5)); // 设置边框颜色和宽度为
 
    // 创建线性渐变色
    QLinearGradient gradient(0, 0, 0, this->height());
    gradient.setColorAt(0, QColor(245, 245, 247)); // 起始颜色
    gradient.setColorAt(1, QColor(240, 239, 238)); // 终止颜色
 
    // 使用渐变色填充背景
    painter.setBrush(gradient);
    painter.setPen(Qt::NoPen);
    painter.drawRoundedRect(0, 0, this->width(), this->height(), 10, 10); // 绘制圆角矩形
}
 
 
```

### 3 UI 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>Custom_warning</class>
 <widget class="QDialog" >
  <property >
   <rect>
    <x>0</x>
    <y>0</y>
    <width>155</width>
    <height>101</height>
   </rect>
  </property>
  <property >
   <size>
    <width>16777215</width>
    <height>101</height>
   </size>
  </property>
  <property >
   <font>
    <pointsize>12</pointsize>
   </font>
  </property>
  <property >
   <string>Form</string>
  </property>
  <property >
   <string notr="true">QPushButton#pushButton{
	border: 2px solid #a7a7a7;
	padding: 2px 20px; /*增加按钮的内边距，使其扩大*/
}
 
QPushButton{
    border-radius: 4px;
    /*background-color: rgb(225, 225, 225);*/
    padding: 3px 23px; /*增加按钮的内边距，使其扩大*/
}
 
QPushButton#pushButton_close{
	background-color: rgb(255, 255, 255);
}
 
QPushButton#pushButton_close:pressed {
	background-color: rgb(173, 180, 189);
}
 
QPushButton::pressed,QPushButton::checked{
    background-color: rgb(173, 180, 189);/*按钮按下的颜色*/
}
</string>
  </property>
  <layout class="QGridLayout" >
   <item row="0" column="0">
    <layout class="QVBoxLayout" >
     <property >
      <number>0</number>
     </property>
     <item>
      <layout class="QHBoxLayout" >
       <item>
        <spacer >
         <property >
          <enum>Qt::Horizontal</enum>
         </property>
         <property >
          <size>
           <width>40</width>
           <height>20</height>
          </size>
         </property>
        </spacer>
       </item>
       <item>
        <widget class="QPushButton" >
         <property >
          <size>
           <width>23</width>
           <height>23</height>
          </size>
         </property>
         <property >
          <size>
           <width>23</width>
           <height>23</height>
          </size>
         </property>
         <property >
          <string/>
         </property>
        </widget>
       </item>
      </layout>
     </item>
     <item>
      <layout class="QHBoxLayout" >
       <property >
        <number>0</number>
       </property>
       <property >
        <number>0</number>
       </property>
       <property >
        <number>0</number>
       </property>
       <item>
        <widget class="QLabel" >
         <property >
          <font>
           <family>Arial</family>
           <pointsize>11</pointsize>
          </font>
         </property>
         <property >
          <string>IOCN</string>
         </property>
         <property >
          <set>Qt::AlignCenter</set>
         </property>
        </widget>
       </item>
       <item>
        <widget class="QLabel" >
         <property >
          <font>
           <family>Arial</family>
           <pointsize>11</pointsize>
          </font>
         </property>
         <property >
          <string>TEXT</string>
         </property>
         <property >
          <set>Qt::AlignCenter</set>
         </property>
        </widget>
       </item>
      </layout>
     </item>
     <item>
      <layout class="QHBoxLayout" >
       <item>
        <spacer >
         <property >
          <enum>Qt::Horizontal</enum>
         </property>
         <property >
          <size>
           <width>40</width>
           <height>20</height>
          </size>
         </property>
        </spacer>
       </item>
       <item>
        <widget class="QPushButton" >
         <property >
          <font>
           <family>Arial</family>
           <pointsize>11</pointsize>
          </font>
         </property>
         <property >
          <string>按钮</string>
         </property>
        </widget>
       </item>
       <item>
        <spacer >
         <property >
          <enum>Qt::Horizontal</enum>
         </property>
         <property >
          <size>
           <width>40</width>
           <height>20</height>
          </size>
         </property>
        </spacer>
       </item>
      </layout>
     </item>
    </layout>
   </item>
  </layout>
 </widget>
 <resources/>
 <connections/>
</ui>
```

### 4 UI 文件 样式表（UI 漂亮很重要）

```
QPushButton#pushButton{
	border: 2px solid #a7a7a7;
	padding: 2px 20px; /*增加按钮的内边距，使其扩大*/
}
 
QPushButton{
    border-radius: 4px;
    /*background-color: rgb(225, 225, 225);*/
    padding: 3px 23px; /*增加按钮的内边距，使其扩大*/
}
 
QPushButton#pushButton_close{
	background-color: rgb(255, 255, 255);
}
 
QPushButton#pushButton_close:pressed {
	background-color: rgb(173, 180, 189);
}
 
QPushButton::pressed,QPushButton::checked{
    background-color: rgb(173, 180, 189);/*按钮按下的颜色*/
}
```

六 使用实例
------

此函数可以返回是否点击确认值

```
包含头文件之后
Custom_warning::Display("大于72h需要重新激活", Custom_warning::YES);
```