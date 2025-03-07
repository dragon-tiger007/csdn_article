> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/xiezhongyuan07/article/details/109245920?ops_request_misc=%257B%2522request%255Fid%2522%253A%252211c75b66fcf383a44bc7cd0e44382331%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=11c75b66fcf383a44bc7cd0e44382331&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-109245920-null-null.142^v102^pc_search_result_base5&utm_term=qml%E4%B8%8Ec%2B%2B%E4%BA%A4%E4%BA%92&spm=1018.2226.3001.4187)

[QML](https://so.csdn.net/so/search?q=QML&spm=1001.2101.3001.7020) 作为一种灵活高效的界面开发语言已经越来越得到业界的认可。QML 负责界面，C++ 负责逻辑，这也是 Qt 官方推荐的开发方式。那么 QML 与 C++ 的交互必然是每一个 Qt 开发老师需要掌握并且精通的。

接下来，我们会对 QML 与 C++ 交互的几种方式进行详细讲解。我们通过创建项目，通过例子来实现、体验并应用这几种交互方式，让我们由浅入深理解其中的原理。

首先，QML 与 C++ 的交互大致可以分为 4 种形式：

1.  注册 C++ 对象到 QML，在 QML 中访问 C++ 对象；
2.  QML 暴露对象给 C++ 进行交互；
3.  C++ 创建 QML 对象并进行交互；
4.  C++ 对象与 QML 通过信号槽进行交互；

这四种交互方式，是每一个要学习 QML 的程序员必须要深刻理解并熟掌握的。

接下来，我们会分为 4 章，对 QML 与 C++ 交互的这四种方式 进行详细介绍；今天重点介绍第一种方式：**注册 C++ 对象到 QML 中，在 QML 中调用 C++ 的方法；**

=========================== 正文开始 =============================

在正式开始之前，我们先把今天得重点标出来，并且解释一下：

**1. C++ 对象注册到元对象系统**

> QQmlApplicationEngine::rootContext()->setContextProperty()

**2. Q_INVOKABLE 宏定义是将 C++ 的 函数（方法）声明为元对象系统可调用的函数** （我们这里先不展开 Q_INVOKABLE ，下面会详细讲到）

我们今天介绍的 "注册 C++ 对象到 QML，在 QML 中访问 C++ 对象" 中，就是应用了上面这两条重要的知识（方法）。接下来我们会使用 Qt (5.12) 自带的 IDE Qt Creator 来进行实际操作。

#### 一、 我们首先创建一个新的工程。

（“文件” - “新建文件或项目” - “Application (Qt Quick)” - “Qt Quick Application - Empty”）

![](https://i-blog.csdnimg.cn/blog_migrate/6ba18adfa161ec0b32702879f9e7b2b5.png)

![](https://i-blog.csdnimg.cn/blog_migrate/fc20cdbe5b3e9fbe86371dcc6e0c0076.png)

创建完成就是上面这样子的：里面只有一个 main.cpp 与一个默认的 main.qml。 我们既然要做的是 C++ 与 QML 交互，只有一个 qml 文件是不够的，还需要一个 C++ 类。

#### 二、创建一个 C++ 的类 MyQmlClass 来与 QML 进行交互

项目名称 右键 - "Add New..." -  “C++” - “C++ Class” 

![](https://i-blog.csdnimg.cn/blog_migrate/40b179ac6008f71b42aa0e91c31d753d.png)

![](https://i-blog.csdnimg.cn/blog_migrate/cfed715b649218c39c575d69b042db22.png)

![](https://i-blog.csdnimg.cn/blog_migrate/717779a66dc3a841401a830f08b77f40.png)

类名随便起，我们这里叫做 MyQmlClass, 基类选择 QObject 并包含 QObject , 然后填写头文件名，源文件名。

![](https://i-blog.csdnimg.cn/blog_migrate/eb65a8df9e1f64543fca838ac7abe2ad.png)

这样我们就成功添加了一个 C++ 的类。到这里，我们交互双方 C++ 与 QML 的文件都已经有了，我们接下来就可以进入重点了。

首先，我们针对 C++ 类进行改造，增加 一个私有变量 m_Value 与 两个公共的函数对其进行读写：

```
#ifndef MYQMLCLASS_H
#define MYQMLCLASS_H
 
#include <QObject>
 
class MyQmlClass : public QObject
{
    Q_OBJECT
public:
    explicit MyQmlClass(QObject *parent = nullptr);
 
    Q_INVOKABLE void setValue(int value);    
    Q_INVOKABLE int getValue();
 
signals:
 
private:
    int m_Value;
};
 
#endif // MYQMLCLASS_H
```

其中 定义的两个函数 setValue 与 getValue 分别是对 m_Value 进行写与读，实现如下：

```
#include "MyQmlClass.h"
 
MyQmlClass::MyQmlClass(QObject *parent) : QObject(parent)
{
 
}
 
void MyQmlClass::setValue(int value)
{
    m_Value = value;
}
 
int MyQmlClass::getValue()
{
    return m_Value;
}
```

这时候细心地老师已经发现了在新增的两个函数 与 平时我们定义函数有点不一样，就是函数前增加了一个 Q_INVOKABLE。这是个什么呢？这就是我们正文开始前，给大家说的本节的关键点之一。那 Q_INVOKABLE 什么作用呢？ 我们先来看看 QT 官方对 Q_INVOLKABLE 的定义与描述：

![](https://i-blog.csdnimg.cn/blog_migrate/29387a46783ecd8082bda939421a3b32.png)

翻译一下，大概意思就是说，Q_INVOKABLE 是个宏定义，这个宏将 函数 声明为元对象系统可调用的函数。这里有几个关键点：

*   Q_INVOKABLE 是个宏定义
*   这个宏定义 针对的是 函数， 不是变量
*   经过 Q_INVOKABLE 声明过得函数 可以被元对象系统调用
*   QtQuick 也在元对象系统内，所以在 QML 中也可以访问这个被声明了的函数

好了，声明了函数，我们现在是不是可以在 QML 中调用 C++ 的方法了呢？ 别着急，还有一个重要的工作要做，就是我们需要将 C++ 的对象，注册到 QML 中去。我们需要用到本文开头说的两个重点中的另一个：

```
QQmlApplicationEngine::rootContext()->setContextProperty()

```

我们接下来要打开 main.cpp ，通过 QML 引擎 QQmlApplicationEngine 进行注册。 

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
 
#include "MyQmlClass.h"
 
int main(int argc, char *argv[])
{
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
 
    QGuiApplication app(argc, argv);
 
    QQmlApplicationEngine engine;
 
    ///
    //声明 对象
    MyQmlClass myQmlImp;
 
    //将对象进行注册到QML中
    engine.rootContext()->setContextProperty("myQmlImp", &myQmlImp);
 
    ///
 
    const QUrl url(QStringLiteral("qrc:/main.qml"));
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    engine.load(url);
 
    return app.exec();
}
```

上面的代码中，我们首先定义了一个 C++ 的对象 myQmlImp ，然后使用了 QQmlApplicationEngine 获取 其中的 QQmlContext，调用注册的方法 setContextProperty

这里的 QQmlContext 类我们这里先不展开，我们就先记住这样用，后面我们再详细的讲一下。对于 setContextProperty 函数其实就是设置一个属性，参数就是 key-value

key ：自定义字符串，为了好记，我们这里叫做对象的名字 "myQmlImp"

value : 对象引用，对象指针，这里就是 & myQmlImp

再来看一下官方的定义：

![](https://i-blog.csdnimg.cn/blog_migrate/0e85594c8f946d1485d9e9250721864e.png)

做完了这两步，我们就做了 C++ 的工作，在 QML 中 就可以 直接拿到对象 myQmlImp 来调用使用 Q_INVOKABLE 定义的方法了。

#### 二、 改造 QML 

 我们的思路是这样的，在 QML 中，定义一个 “获取” 的 Button 与 显示获取值得 Label， 点击 Button 就会获取 C++ 中的 m_value 值， 在 Lable 上进行展示。

1， 我们打开 main.qml 新增加一个 “获取” Button 

![](https://i-blog.csdnimg.cn/blog_migrate/cec50720140112c1e39f2e196ad06107.png)

我们看到定义 Button 的报错了，这是因为新建的项目中，只包含 QtQuick 与 QtQuick.Window 两个包，默认不包含 Button 的控件， Button 控件 放在 Controls 包里面。我们先引入 QtQuick.Controls 2.3

![](https://i-blog.csdnimg.cn/blog_migrate/befe7c41f721da3c4d51c489e596d1c4.png)

![](https://i-blog.csdnimg.cn/blog_migrate/deffcc282bfb91995071238aba932093.png)

![](https://i-blog.csdnimg.cn/blog_migrate/1754b57fc7829432f93f46fd7cc729cd.png)

导入完 QtQuick.Controls 项目中就包含了三个可用的包，我们以后在具体看一下每个包都是什么功能，包含什么组件。接下来，我们看看刚才的报错好了没？

![](https://i-blog.csdnimg.cn/blog_migrate/ae822c942bfb062ac9126ae914a4b046.png)

这时候，我们看到，刚才的错误已经消失了，其实 QtQuick.Controls 里面，都是 Qt 官方包装好的一些常用控件，比如收 Button、Label, SpinBox, CheckBox, RadioButton 等等我们传统 QWidget 常见的控件。

我们的思路很简单，就是 QML 中来调用上面 C++ 暴露出来的读写函数。所以我们在 QML 中定义一个 “获取” Button ，点击它我们就来调用 C++ 中的 getValue() 函数，然后我们需要一个 Label 将获取的 C++ 的值进行展示，所以开始吧：  
 

```
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.3
 
Window {
    visible: true
    width: 640
    height: 480
    title: qsTr("Hello World")
 
    Label{                         //Label用于显示获取C++的值
        id: label                  //显示控件，唯一标识ID：label
        text: ""                   //初始化内容清空
 
        anchors.bottom: getBtn.top //显示控件的下方放到btn的上方
        anchors.left: getBtn.left  //显示控件的左方与btn的左侧对齐
    }
 
    Button{                       //Button 用于获取值
        id: getBtn                 //按钮控件，唯一标识ID：getBtn
        text: "获取"                //按钮显示文字
        width: 120                 //按钮宽度
        height: 40                 //按钮高度
 
        anchors.centerIn: parent   //按钮放到窗口中心
 
        onClicked: {               //点击按钮事件;
            label.text = myQmlImp.getValue()
        }
    }
}
```

在代码里，我们定义了一个 Label 用来显示获取的值， 定义了一个 Button 用来触发获取 C++ 的函数。 其中：

*   我们将 “获取” 按钮放到窗口中心，将显示 Label 放到了 “获取” 按钮上方。（对控件定位不太熟悉的老师需要专门学习一下 anchors，最基础也是最重要的了）
*   点击 “获取” 按钮，会执行 onClicked:{} 函数，也就是在 onClicked 里面会调用 C++ 里面暴露的函数
*   myQmlImp 就是 我们在 main.cpp 注册的对象的名称。通过 myQmlImp 我们就能访问 注册对象里使用 Q_INVOKABLE 声明的函数了。

我们看一下‘’效果：

![](https://i-blog.csdnimg.cn/blog_migrate/528d56256af3a08dbfe39a5e2c00a992.gif)

我们点击了 “获取” 按钮，然后在它上方的 label 就显示出一串数字。（因为我们在 c++ 里面定义的 m_Value 没有初始化，所以这个值是个随机的）

到这里，我们就在 QML 中获取了 C++ 代码中的值。可能到这里还有老师感觉不太真实，那么我们就继续进行验证，我们的思路是这样的:

1.  我们增加一个 TextFiled 用于输入我们想给 。
2.  再增加一个 “设置” 按钮，将 1 中输入的值，给 C++ 中的 m_Value 设值。
3.  然后我们再点击刚才的 “获取” 按钮，将 C++ 中的 m_Value 值读取并显示出来。

这时候我们需要对原来的 QML 进行改造，新增加一个输入框 TextField , 进行数值输入； 还需要新增加一个 “设置” 按钮，点击按钮将值赋给 C++ 中的 m_Value

好了，开始：

```
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.3
 
Window {
    visible: true
    width: 640
    height: 480
    title: qsTr("Hello World")
 
 
    Label{                         //Label用于显示获取C++的值
        id: label                  //显示控件，唯一标识ID：label
        text: ""                   //初始化内容清空
 
        anchors.bottom: getBtn.top //显示控件的下方放到btn的上方
        anchors.left: getBtn.left  //显示控件的左方与btn的左侧对齐
    }
 
    Button{                       //Button 用于获取值
        id: getBtn                 //按钮控件，唯一标识ID：getBtn
        text: "获取"                //按钮显示文字
        width: 120                 //按钮宽度
        height: 40                 //按钮高度
 
        anchors.centerIn: parent   //按钮放到窗口中心
 
        onClicked: {               //点击按钮事件;
            label.text = myQmlImp.getValue()
        }
    }
 
    TextField{                      //文字输入控件
        id: textField               //唯一ID
        width: getBtn.width         //也可以直接设置成120
        height: getBtn.height       //也可以直接设置成40
 
        anchors.top: getBtn.bottom  //放到“获取”按钮下方10个像素
        anchors.topMargin: 10
        anchors.left: getBtn.left   //与“获取”按钮左对齐
    }
 
    Button{
        id: setBtn
        text: "设置"
        width: textField.width      //可以设置成getBtn.width或者120
        height: textField.height    //可以设置成getBtn.height或者40
 
        anchors.top: textField.bottom
        anchors.left: textField.left
 
        onClicked: {
            var value = textField.text
            myQmlImp.setValue(value)
        }
 
    }
 
 
}
```

代码中，id 为 setBtn 就是我们设置按钮，在他的 onClicked 函数里，我们先获取 TextField 的值，然后通过 myQmlImp 调用 使用 Q_INVOKABLE 修饰过的 setValue 函数。来完成 QML 对 C++ 的另一次操作。

来一起看一下效果：

![](https://i-blog.csdnimg.cn/blog_migrate/4f661b1b38f3fb38e3514d863ac15470.gif)

好了，在这个例子里面，我们在 QML 中有两次调用了 C++ 中的函数，setValue 与 getValue。

最后再来重申一下 QML 与 C++ 交互的第一种方式：**注册 C++ 对象到 QML 中，在 QML 中调用 C++** 函数。今天的重点有两点：

1.  C++ 中想要暴露的方法必须用 Q_INVOKABLE 修饰
2.  需要使用对调用对象进行注册
    
    ```
     QQmlApplicationEngine::rootContext()->setContextProperty（）
    
    
    ```
    
    接下来我们会对 QML 与 C++ 交互的剩下三种形式进行详细解读并实践。
    

这时候，细心地小伙伴可能发现了，我们上面这种方法是：**在 C++ 里定义了一个对象，然后将这个对象注册到 QML 里面。在 QML 里面访问的就是 C++ 定义的对象。**

那么，如果我不想在 C++ 里面定义对象，而是想在 QML 中定义一个对象，并在 QML 中使用， 该怎么办呢？下面一起来学一下将 C++ 类名注册到 QML ，并在 QML 声明一个对象并进行访问！

我们首先来认识一下一个新的函数 qmlRegisterType:

![](https://i-blog.csdnimg.cn/blog_migrate/edaccb83e340c228425cf854d984c43d.png)

 官方描述就是说，qmlRegisterType 就是一个函数模板。将 C++ 的类型注册到 QML 系统中，并且带有版本号，方便版本管理。 我们就把 main.cpp 中的函数改造一下：

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
 
#include "MyQmlClass.h"
 
int main(int argc, char *argv[])
{
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
 
    QGuiApplication app(argc, argv);
 
    QQmlApplicationEngine engine;
 
//    方式一：注册定义好的对象到 QML
//    MyQmlClass myQmlImp;
//    engine.rootContext()->setContextProperty("myQmlImp", &myQmlImp);
 
//    方式二：注册类到 QML 对象
    qmlRegisterType<MyQmlClass>("com.company.myqmlclass", 1, 0, "MyQmlClass");
 
    const QUrl url(QStringLiteral("qrc:/main.qml"));
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    engine.load(url);
 
    return app.exec();
}
```

其中：qmlRegisterType 模板函数中的 “com.company.myqmlclass” 为自定义的控件名称，类似于 C++ 中的库名称。我们在 QML 中需要 import 这个控件名， “MyQmlClass” 为 C++ 注册的类名， 1 和 0 为自定义版本号，方便版本管理。

然后我们就可以在 QML 中进行 刚注册的类进行定义一个对象了。

![](https://i-blog.csdnimg.cn/blog_migrate/e05931fb254f95397ca494639f21d2c0.png)

在 QML 文件中，我们首先使用 import  com.company.myqmlclass 1.0  类似于 C++ 中的 include ,1.0 为 C++ 注册时候带的版本号。

然后我们定义了一个 MyQmlClass 一个对象，id 叫做 myQmlImp。 这和在 C++ 中定义一个对象，然后将对象注册到 C++ 中效果是一样的。由于这里我们定义的控件 ID 与前面例子中的 ID 是一样的，所以，我们现在点击 “设置”、“获取” 按钮也都是可以使用的，我们来试一下效果：

![](https://i-blog.csdnimg.cn/blog_migrate/692bb8279effe69232d91854b22cffb7.gif)

这样，我们就达到了与方式一一样的效果，不过这种方式多了一个版本管理功能，而且 C++ 端注册的时候， C++ 并没有实例化，而是在 QML 中进行了实例化。那么如果 C++ 类产生很多复杂的派生类，则在 QML 中可以有一个很好的调试环境。