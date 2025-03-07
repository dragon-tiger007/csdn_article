> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/tanxuan231/article/details/124990316)

#### [Qml](https://so.csdn.net/so/search?q=Qml&spm=1001.2101.3001.7020) 与 C++ 交互 4：C++ 信号与 Qml 的槽函数的连接

*   [使用场景](#_7)
*   [整体思路](#_11)
*   [1、建立 C++ 信号](#1C_15)
*   [2、C++ 实例注册到 qml](#2Cqml_33)
*   [3、qml 中建立槽函数](#3qml_53)
*   *   *   [`Connections` 类型](#Connections_54)
    *   [建立槽函数](#_69)
*   [运行结果](#_82)
*   [使用属性](#_91)

![](https://i-blog.csdnimg.cn/blog_migrate/7c46db91d836c7d24ea25e7c36313bab.png)  
**更多资讯、知识，微信公众号搜索：“上官宏竹”。**

使用场景
----

比如在 C++ 中更改某些数据，实时更新到 [UI 界面](https://so.csdn.net/so/search?q=UI%E7%95%8C%E9%9D%A2&spm=1001.2101.3001.7020)中。

整体思路
----

C++ 中建立一个信号并发送，在`Qml`中使用`Connections`建立一个对应的响应槽函数，当 C++ 发送信号时，触发`Qml`对应的槽函数。

1、建立 C++ 信号
-----------

首先我们声明一个信号如下，然后在点击的槽函数中将信号发出去。

```
{
signals:
    void myClassInfoSignal();	// C++的信号名
}

void MyClass::myRectSlot(int count)
{
    qDebug()<<__FUNCTION__<<" count: "<<count;

    emit myClassInfoSignal();	// 发射信号
}

```

2、C++ 实例注册到 qml
---------------

使用`qmlRegisterSingletonInstance`方法或者`engine.rootContext()->setContextProperty`的方法，将 C++ 中的实例注册到`qml`中。

注意：在`Windows`系统上使用`engine.rootContext()->setContextProperty`的方法，会出现如下的告警错误：

```
QML Connections: Detected function "onMyClassInfoSignal" in Connections element. This is probably intended to be a signal handler but no signal of the target matches the name.
ReferenceError: MyclassApi is not defined

```

大家可以改用`qmlRegisterSingletonInstance`方法进行注册，在`import`注册号的模块名后，对应的告警会消失；或者先进行属性设置即先调用`engine.rootContext()->setContextProperty`方法，然后再加载`qml`文件。

```
engine.load(url);

auto root = engine.rootObjects();
auto button = root.first()->findChild<QObject*>("mybutton");

```

3、qml 中建立槽函数
------------

#### `Connections`类型

简单说下`Connections`类型。`Connections`分为两部分：一个是 target 属性，表示发出信号的对象。另一个是对应信号的槽函数。  
![](https://i-blog.csdnimg.cn/blog_migrate/bae81a15e1574c9d2834e88b216c1d8a.png#pic_center)  
上图表示 targetA 发出两个信号 asignal1 和 asignal2，然后对应的`Connections`可以写作如下，来接收处理这两个信号：

```
Connections {
	target: targetA;
	function onAsignal1() {
		// 信号处理槽函数
	}
	function onAsignal2() {
		// 信号处理槽函数
	}	
}

```

### 建立槽函数

我们使用`Connections`类型，来达到 C++ 信号和`Qml`槽函数的自动连接处理。`Connections`类型中的`target : Object`，表示与来自哪一个对象的信号连接。而槽函数的命名，则需要以小写的`on`开头，并连接 C++ 的信号函数名字，且第一个字母需要大写。整个槽函数如下：

```
Connections {
	target: MyclassApi	// 已注册的C++中的对象名字

	// 槽函数 on + C++的信号名称（首字母大写）
	function onMyClassInfoSignal() {
		console.log("haha C++ signal is coming...")
	}
}

```

运行结果
----

```
MyClass::myRectSlot  count:  1
qml: haha C++ signal is coming...
MyClass::myRectSlot  count:  2
qml: haha C++ signal is coming...

```

使用属性
----

在 C++ 类中，可以使用`Q_PROPERTY`属性定义读写接口以及属性变化的信号，当这个信号与`qml`的槽函数连接时，需要注意，如果是使用`QtCreator`自动生成的接口，那么`READ`接口不能返回引用，否则在`qml`中调用这个`READ`接口的返回值是未定义；并且接口定义前需要加上`Q_INVOKABLE`，否则`qml`无法识别这个接口。

比如这个属性定义：`Q_PROPERTY(QString key READ getKey WRITE setKey NOTIFY keyChanged)`，自动生成的`READ`接口如下：

```
// 头文件声明
const QString& getKey() const;

// 源文件实现
const QString& MyLogic::getKey() const
{
    return m_key;
}

```

需要将上述的定义和实现更改为：

```
// 头文件声明
Q_INVOKABLE const QString getKey() const;

// 源文件实现
const QString MyLogic::getKey() const
{
    return m_key;
}

```

**完整实现可以微信搜索公众号：“上官宏竹”，或者扫下面的二维码，关注并回复：“qml_4”，获取资源链接。有任何疑问也可以公众号里留言咨询。**

**更多资讯、知识，微信公众号搜索：“上官宏竹”。**  
![](https://i-blog.csdnimg.cn/blog_migrate/8c92ddb002e7e9ed74583c572b6f6bbe.png)