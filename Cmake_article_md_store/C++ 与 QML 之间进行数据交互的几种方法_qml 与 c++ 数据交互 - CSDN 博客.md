> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/m0_60259116/article/details/134163270?ops_request_misc=%257B%2522request%255Fid%2522%253A%252211c75b66fcf383a44bc7cd0e44382331%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=11c75b66fcf383a44bc7cd0e44382331&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-2-134163270-null-null.142^v102^pc_search_result_base5&utm_term=qml%E4%B8%8Ec%2B%2B%E4%BA%A4%E4%BA%92&spm=1018.2226.3001.4187)

一、属性绑定
------

这是最简单的方式，可以在 [QML](https://so.csdn.net/so/search?q=QML&spm=1001.2101.3001.7020) 中直接绑定 C++ 对象的属性。通过在 C++ 对象中使用 Q_PROPERTY 宏定义属性，然后在 QML 中使用绑定语法将属性与 QML 元素关联起来。

1.  person.h

```
#include <QObject>
 
class Person : public QObject
{
    Q_OBJECT
    /* 使用 Q_PROPERTY 定义交互的属性 */
    Q_PROPERTY(QString name READ getName WRITE setName NOTIFY nameChanged)
    Q_PROPERTY(int age READ getAge WRITE setAge NOTIFY ageChanged)
 
public:
    explicit Person(QObject *parent = nullptr)
        : QObject(parent), m_name(""), m_age(0)
    {
    }
 
    /* 为属性提供 getter 和 setter 方法 */
    QString getName() const { return m_name; }
    void setName(const QString& name) { m_name = name; emit nameChanged(); }
 
    int getAge() const { return m_age; }
    void setAge(int age) { m_age = age; emit ageChanged(); }
 
signals:
    /* 信号与属性对应，通过信号通知其他对象属性的变化 */
    void nameChanged();
    void ageChanged();
 
private:
    QString m_name;
    int m_age;
};
 
```

> **本文福利， 免费领取 C++ 学习资料包、技术视频 / 代码，1000 道大厂面试题，内容包括（C++ 基础，网络编程，数据库，中间件，后端开发，音视频开发，Qt 开发）↓↓↓↓↓↓见下面↓↓文章底部点击免费领取↓↓**

2.main.cpp

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
#include "person.h"
 
int main(int argc, char *argv[])
{
    /* 启用Qt应用程序的高DPI缩放功能 */
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
 
    /* 创建一个Qt应用程序的实例 */
    QGuiApplication app(argc, argv);
 
    // 创建Person对象
    Person person;
 
    QQmlApplicationEngine engine;
 
    /* 将Person对象作为QML上下文属性 */
    engine.rootContext()->setContextProperty("person", &person);
 
    const QUrl url(QStringLiteral("qrc:/main.qml"));
    /* 将 QQmlApplicationEngine 对象的 objectCreated 信号连接到一个 lambda 函数上 */
    /* lambda 函数用于在 QML 文件中的根对象被创建时进行处理，检查对象是否成功创建，如果创建失败则退出应用程序 */
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    /* 加载QML文件并显示用户界面 */
    engine.load(url);
 
    return app.exec();
}
 
 
```

3.main.qml

```
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.5
 
Window {
    visible: true
    width: 480
    height: 800
    title: qsTr("Hello World")
 
    Column {
        spacing: 10
 
        TextField {
            placeholderText: "请输入姓名"
            text: person.name // 与Person对象的name属性绑定
            onTextChanged: person.name = text // 当文本改变时，更新Person对象的name属性
        }
 
        Slider {
            from: 0
            to: 100
            value: person.age // 与Person对象的age属性绑定
            onValueChanged: person.age = value // 当滑块值改变时，更新Person对象的age属性
        }
 
        Text {
            text: "姓名：" + person.name
        }
 
        Text {
            text: "年龄：" + person.age
        }
    }
 
}
 
```

二、信号与槽
------

C++ 对象可以发出信号，而 QML 中的元素可以连接到这些信号上。这样，当 C++ 对象的状态发生变化时，可以通过信号与槽机制将这些变化传递给 QML 界面。

1.  myobject.h

```
#include <QObject>
#include <QtDebug>
 
class MyObject : public QObject
{
    Q_OBJECT
 
public:
    explicit MyObject(QObject *parent = nullptr) : QObject(parent) {}
 
signals:
    void mySignal(QString message);
 
public slots:
    void mySlot(const QString& message) { qDebug() << "Received message from QML:" << message; emit mySignal("Hello from C++");}
};
 
```

2.main.cpp

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
#include "myobject.h"
 
int main(int argc, char *argv[])
{
    /* 启用Qt应用程序的高DPI缩放功能 */
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
 
    /* 创建一个Qt应用程序的实例 */
    QGuiApplication app(argc, argv);
 
    /* 将自定义 C++ 类型注册到 QML 中的函数, 将自定义 C++ 类型注册到 QML 中的函数 */
    qmlRegisterType<MyObject>("com.example", 1, 0, "MyObject");
 
    QQmlApplicationEngine engine;
    const QUrl url(QStringLiteral("qrc:/main.qml"));
    /* 将 QQmlApplicationEngine 对象的 objectCreated 信号连接到一个 lambda 函数上 */
    /* lambda 函数用于在 QML 文件中的根对象被创建时进行处理，检查对象是否成功创建，如果创建失败则退出应用程序 */
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    /* 加载QML文件并显示用户界面 */
    engine.load(url);
 
    return app.exec();
}
 
```

3.main.qml

```
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.5
import com.example 1.0
 
Window {
    visible: true
    width: 480
    height: 800
    title: qsTr("Hello World")
 
    /* 定义 sendToCpp 信号 */
    signal sendToCpp(string message)
 
    /* Connections 组件用于连接 myObject 的 onMySignal 信号 */
    Connections {
        target: myObject
        onMySignal: console.log("Received message from C++:", message)
    }
 
    MyObject {
        id: myObject
        /* 将 onMySignal 信号传递到 sendToCpp信号上，便于 QML 处理 */
        onMySignal: sendToCpp(message)
    }
 
    Button {
        text: "Send message to C++"
        anchors.centerIn: parent
        /* 单击按钮时，会将信号传递到 C++ 的 mySlot 槽上 */
        onClicked: myObject.mySlot("Hello from QML")
    }
}
 
```

三、模型视图
------

模型视图（Model-View）：可以使用 C++ 中的数据模型（QStandardItemModel）来提供数据给 QML 界面。QML 中的视图元素（如 ListView 或 GridView）可以使用这些模型来显示数据。

1.  mymodel.h

```
#ifndef MYMODEL_H
#define MYMODEL_H
 
#include <QAbstractListModel>
#include <QList>
 
class MyModel : public QAbstractListModel
{
    Q_OBJECT
 
public:
    explicit MyModel(QObject *parent = nullptr);
 
    enum {
        NameRole = Qt::UserRole + 1,
        AgeRole,
        EmailRole
    };
 
    // 重写以下几个虚函数
    int rowCount(const QModelIndex &parent = QModelIndex()) const override;
    QVariant data(const QModelIndex &index, int role = Qt::DisplayRole) const override;
    QHash<int, QByteArray> roleNames() const override;
 
private:
    struct Person {
        QString name;
        int age;
        QString email;
    };
 
    QList<Person> m_persons;
};
 
#endif // MYMODEL_H
```

2.mymodel.cpp

```
age-cpp">#include "mymodel.h"
MyModel::MyModel(QObject *parent)
    : QAbstractListModel(parent)
{
    // 初始化一些数据
    m_persons.append({"Alice", 25, "alice@example.com"});
    m_persons.append({"Bob", 30, "bob@example.com"});
    m_persons.append({"Charlie", 35, "charlie@example.com"});
}
int MyModel::rowCount(const QModelIndex &parent) const
{
    Q_UNUSED(parent);
    return m_persons.count();
}
QVariant MyModel::data(const QModelIndex &index, int role) const
{
    if (!index.isValid())
        return QVariant();
    if (index.row() >= m_persons.count() || index.row() < 0)
        return QVariant();
    const Person &person = m_persons[index.row()];
    if (role == NameRole)
        return person.name;
    else if (role == AgeRole)
        return person.age;
    else if (role == EmailRole)
        return person.email;
    return QVariant();
}
QHash<int, QByteArray> MyModel::roleNames() const
{
    QHash<int, QByteArray> roles;
    roles[NameRole] = "name";
    roles[AgeRole] = "age";
    roles[EmailRole] = "email";
    return roles;
}
```

3.main.cpp

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
#include "mymodel.h"
 
int main(int argc, char *argv[])
{
    /* 启用Qt应用程序的高DPI缩放功能 */
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
 
    /* 创建一个Qt应用程序的实例 */
    QGuiApplication app(argc, argv);
 
    QQmlApplicationEngine engine;
 
    MyModel myModel;
    engine.rootContext()->setContextProperty("myModel", &myModel);
 
    const QUrl url(QStringLiteral("qrc:/main.qml"));
    /* 将 QQmlApplicationEngine 对象的 objectCreated 信号连接到一个 lambda 函数上 */
    /* lambda 函数用于在 QML 文件中的根对象被创建时进行处理，检查对象是否成功创建，如果创建失败则退出应用程序 */
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    /* 加载QML文件并显示用户界面 */
    engine.load(url);
 
    return app.exec();
}
 
```

4.main.qml

```
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.5
 
Window {
    visible: true
    width: 480
    height: 800
    title: qsTr("Hello World")
 
    ListView {
        anchors.fill: parent
        model: myModel
        delegate: Item {
            width: parent.width
            height: 60
            Column {
                Text { text: name }
                Text { text: age }
                Text { text: email }
            }
        }
    }
}
 
```

四、QML 类型注册
----------

QML 类型注册（QML Type Registration）：可以将 C++ 对象注册为自定义的 QML 类型，使得 QML 可以直接创建和使用这些对象。通过在 C++ 中使用 [Q_PROPERTY](https://so.csdn.net/so/search?q=Q_PROPERTY&spm=1001.2101.3001.7020) 宏和 Q_INVOKABLE 函数，可以将 C++ 类注册为 QML 类型。我需要这样一个案例

1.  myobject.h

```
#include <QQmlEngine>
#include "QDebug"
 
class MyObject : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QString name READ name WRITE setName NOTIFY nameChanged)
public:
    explicit MyObject(QObject *parent = nullptr) : QObject(parent) {}
    QString name() const { return m_name; }
    void setName(const QString &name) { m_name = name; emit nameChanged(); }
    Q_INVOKABLE void printName() { qDebug() << "Name:" << m_name; }
 
    static void registerQmlType()
    {
        qmlRegisterType<MyObject>("com.example", 1, 0, "MyObject");
    }
 
signals:
    void nameChanged();
private:
    QString m_name;
};
 
```

2.main.cpp

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
#include "myobject.h"
 
int main(int argc, char *argv[])
{
    /* 启用Qt应用程序的高DPI缩放功能 */
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
 
    /* 创建一个Qt应用程序的实例 */
    QGuiApplication app(argc, argv);
 
    QQmlApplicationEngine engine;
 
    MyObject::registerQmlType();
 
    const QUrl url(QStringLiteral("qrc:/main.qml"));
    /* 将 QQmlApplicationEngine 对象的 objectCreated 信号连接到一个 lambda 函数上 */
    /* lambda 函数用于在 QML 文件中的根对象被创建时进行处理，检查对象是否成功创建，如果创建失败则退出应用程序 */
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    /* 加载QML文件并显示用户界面 */
    engine.load(url);
 
    return app.exec();
}
 
```

3.main.qml

```
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.5
import com.example 1.0
 
Window {
    visible: true
    width: 480
    height: 800
    title: qsTr("Hello World")
 
    MyObject {
        id: myObject
        name: "John"
    }
 
    /* 垂直布置组件 */
    Column {
        anchors.fill: parent        // 大小为父组件的大小
        anchors.margins: 40         // 与父组件四周的间隔
        spacing: 10                 // 子组件之间的间隔
 
        Text {
            text: myObject.name
        }
 
        Button {
            text: "Print Name"
            onClicked: myObject.printName()
        }
    }
}
```

> **本文福利， 免费领取 C++ 学习资料包、技术视频 / 代码，1000 道大厂面试题，内容包括（C++ 基础，网络编程，数据库，中间件，后端开发，音视频开发，Qt 开发）↓↓↓↓↓↓见下面↓↓文章底部点击免费领取↓↓**