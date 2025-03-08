> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_73414031/article/details/142698294)

#### 随着 [Qt 6](https://so.csdn.net/so/search?q=Qt%206&spm=1001.2101.3001.7020) 的发布，QtQuick3D 模块带来了新的 3D 渲染和交互能力，使得在 Qt 中创建 3D 场景变得更加简单和直观。本文将带您从一个简单的 QML 3D 应用开始，详细讲解各个相关领域的概念、代码实现以及功能特点。

##### 什么是 Qt Quick 3D？

Qt Quick 3D 是 Qt 6 中引入的一个模块，旨在为用户提供便捷的 3D 场景管理和渲染工具。与 [Qt 5](https://so.csdn.net/so/search?q=Qt%205&spm=1001.2101.3001.7020) 中的 Qt 3D 模块相比，QtQuick3D 更加简化，且集成了 Qt Quick 的能力，让 2D 和 3D 内容的结合更加自然。

##### [QML](https://so.csdn.net/so/search?q=QML&spm=1001.2101.3001.7020) 3D 应用的基本结构

QtQuick3D 中，您可以通过 QML 来定义 3D 场景、物体、灯光、摄像机等。这些内容与 Qt Quick 2D 界面元素可以并存和协作，允许在同一个窗口中同时渲染 2D 和 3D 元素。

以下是我们将要分析的 QML 代码示例，这个例子展示了如何通过 QML 创建一个简单的 3D 场景，并与用户交互。

代码示例程序:

main.cpp

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
 
int main(int argc, char *argv[])
{
#if defined(Q_OS_WIN) && QT_VERSION_CHECK(5, 6, 0) <= QT_VERSION && QT_VERSION < QT_VERSION_CHECK(6, 0, 0)
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
#endif
 
    QGuiApplication app(argc, argv);
 
    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/qt/qml/qtquickapplication2/main.qml")));
    if (engine.rootObjects().isEmpty())
        return -1;
 
    return app.exec();
}
```

##### main.qml

```
import QtQuick.Window
import QtQuick3D
import QtQuick
Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")
    View3D {//显示3D的舞台（摄影棚）
        id: _view
        anchors.fill: parent
 
        environment: SceneEnvironment {//environment设置渲染场景
            //SceneEnvironment为场景的渲染方式定义了一组全局属性。
            clearColor: "red"//环境颜色
            backgroundMode: SceneEnvironment.Color
            antialiasingMode: SceneEnvironment.MSAA
            antialiasingQuality: SceneEnvironment.High
        }
        camera: camera
        PerspectiveCamera {//定义用于查看三维场景内容的透视摄影机（摄像机）。
            id: camera
            position: Qt.vector3d(0, -900, 800)//摄像机位置
            eulerRotation.x: 45//x轴旋转45度
        }
 
        MouseArea {
            anchors.fill: parent
            onClicked: (mouse)=> {
                let ret = _view.pick(mouse.x, mouse.y)
                if(ret.objectHit !== null)
                    ret.objectHit.materials[0].diffuseColor =
                           Qt.rgba(Math.random(),Math.random(), Math.random(), 1.0)
 
            }
        }
 
        DirectionalLight {//场景灯光
           eulerRotation: Qt.vector3d(1, 0, 0)
           castsShadow: true
           brightness: 3
        }
 
        Model {//物体（拍照对象）
            id: _sphere
            source: "#Sphere"//球
            z: 300
            pickable: true
            scale: Qt.vector3d(2, 2, 2)
            materials: DefaultMaterial {//定义三维材质材质
                diffuseColor: Qt.rgba(0.6, 0.5, 0.2, 1.0)//着色
                 diffuseLightWrap:0//
            }
        }
 
 
        Model {
            source: "#Rectangle"
            pickable: true
            scale: Qt.vector3d(10, 10, 1)
            materials: DefaultMaterial {
                diffuseColor: Qt.rgba(0.7, 0.2, 0.2, 1.0)
            }
        }
    }
 
}
```

运行结果:

![](https://i-blog.csdnimg.cn/direct/f8c4486bc2894d19ae301abc0cd2a0c2.png)

##### 代码分析

###### 1. 创建主窗口

首先，我们需要一个窗口来容纳 3D 内容。这里我们使用 `Window` 元素，并设置基本的窗口属性，例如宽度、高度和标题。

```
Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")
```

*   **`Window`**：定义一个窗口，宽度为 640，高度为 480，并设置标题为 “Hello World”。
*   **`qsTr("Hello World")`**：Qt 的国际化功能，便于将文本翻译成不同语言。

###### 2. 定义 3D 视图

要显示 3D 内容，首先需要创建一个 `View3D`，这是 QtQuick3D 中的主要容器，类似于 2D 场景中的 `Item`。`View3D` 是一个 3D 场景的承载体，所有 3D 内容都将在这个视图中渲染。

qml

```
View3D {
    id: _view
    anchors.fill: parent
```

*   **`View3D`**：这是一个 3D 视图容器，用于渲染 3D 场景。`anchors.fill: parent` 将其大小设置为充满整个窗口。
*   **`id: _view`**：为 3D 视图定义一个 ID 以便后续引用。

###### 3. 设置场景环境

为了优化 3D 渲染效果，我们可以通过 `SceneEnvironment` 来设置全局的渲染环境。例如，可以定义背景颜色、抗锯齿模式等。

```
environment: SceneEnvironment {
    clearColor: "red"
    backgroundMode: SceneEnvironment.Color
    antialiasingMode: SceneEnvironment.MSAA
    antialiasingQuality: SceneEnvironment.High
}
```

`environment: SceneEnvironment { clearColor: "red" backgroundMode: SceneEnvironment.Color antialiasingMode: SceneEnvironment.MSAA antialiasingQuality: SceneEnvironment.High }`

*   **`SceneEnvironment`**：定义了 3D 场景的渲染环境。
    *   `clearColor`：设置场景背景为红色。
    *   `backgroundMode`：指定背景填充模式为颜色填充。
    *   `antialiasingMode` 和 `antialiasingQuality`：启用多重采样抗锯齿（MSAA）并设置其质量为高。

###### 4. 添加摄像机

3D 场景的观察依赖于摄像机。`PerspectiveCamera` 是一种常用的透视摄像机，它能产生与现实世界类似的透视效果。

```
camera: camera
PerspectiveCamera {
    id: camera
    position: Qt.vector3d(0, -900, 800)
    eulerRotation.x: 45
}
```

*   **`PerspectiveCamera`**：定义了一个透视摄像机，用于观察 3D 场景。
    *   `id: camera`：为摄像机设置 ID。
    *   `position`：设置摄像机的位置，x、y 和 z 坐标为 (0, -900, 800)。
    *   `eulerRotation.x: 45`：将摄像机绕 X 轴旋转 45 度，使其向下倾斜以获得俯视视角。

###### 5. 添加光照

光照是 3D 渲染中不可或缺的一部分，它决定了物体的明暗和阴影效果。这里使用 `DirectionalLight` 来模拟来自特定方向的光源。

```
DirectionalLight {
    eulerRotation: Qt.vector3d(1, 0, 0)
    castsShadow: true
    brightness: 3
}
```

`DirectionalLight { eulerRotation: Qt.vector3d(1, 0, 0) castsShadow: true brightness: 3 }`

*   **`DirectionalLight`**：定义了场景中的一个方向光源。
    *   `eulerRotation`：设置光源的旋转方向。
    *   `castsShadow`：启用阴影投射效果。
    *   `brightness`：设置光源亮度为 3。

###### 6. 创建 3D 模型

3D 模型是场景中的核心部分。在 QtQuick3D 中，通过 `Model` 元素定义 3D 物体，并使用 `source` 属性加载预定义的形状（例如球体、矩形等）。

```
Model {
    id: _sphere
    source: "#Sphere"
    z: 300
    pickable: true
    scale: Qt.vector3d(2, 2, 2)
    materials: DefaultMaterial {
        diffuseColor: Qt.rgba(0.6, 0.5, 0.2, 1.0)
    }
}
```

*   **`Model`**：创建了一个 3D 模型，源为一个球体（`source: "#Sphere"`）。
    *   `z: 300`：将球体沿 z 轴平移。
    *   `pickable`：模型是可拾取的，也就是说可以用鼠标进行选择和交互。
    *   `scale`：将球体按比例放大 2 倍。
    *   **`materials`**：指定了球体的材质，`diffuseColor` 定义了其漫反射颜色为金黄色。

再创建一个矩形模型：

```
Model {
    source: "#Rectangle"
    pickable: true
    scale: Qt.vector3d(10, 10, 1)
    materials: DefaultMaterial {
        diffuseColor: Qt.rgba(0.7, 0.2, 0.2, 1.0)
    }
}
```

此模型定义了一个矩形，并设置了红色的材质。

###### 7. 交互：鼠标点击事件

通过 `MouseArea`，可以捕捉鼠标点击事件，并与 3D 模型进行交互。通过调用 `pick` 函数，可以检测鼠标点击的模型，并修改其颜色。

```
MouseArea {
    anchors.fill: parent
    onClicked: (mouse)=> {
        let ret = _view.pick(mouse.x, mouse.y)
        if(ret.objectHit !== null)
            ret.objectHit.materials[0].diffuseColor = Qt.rgba(Math.random(), Math.random(), Math.random(), 1.0)
    }
}
```

*   **`MouseArea`**：定义了一个鼠标区域，覆盖整个 3D 视图。
    *   `onClicked`: 当点击时，调用 `_view.pick(mouse.x, mouse.y)` 检测点击到的物体，若成功选中物体，则将其材质的漫反射颜色随机改变。

##### C++ 代码入口

为了启动 QML 应用，我们还需要一个 C++ 程序入口。`main.cpp` 文件提供了应用程序的启动逻辑，并加载 `QML` 文件。

```
#include <QGuiApplication>
#include <QQmlApplicationEngine>
 
int main(int argc, char *argv[])
{
#if defined(Q_OS_WIN) && QT_VERSION_CHECK(5, 6, 0) <= QT_VERSION && QT_VERSION < QT_VERSION_CHECK(6, 0, 0)
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
#endif
 
    QGuiApplication app(argc, argv);
 
    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/qt/qml/qtquickapplication2/main.qml")));
    if (engine.rootObjects().isEmpty())
        return -1;
 
    return app.exec();
}
```

*   **`QGuiApplication`**：负责应用程序的基本设置和生命周期管理。
*   **`QQmlApplicationEngine`**：用于加载和执行 QML 文件。在这里，加载了 `main.qml`，该文件定义了 3D 场景和其所有组件。
*   **`app.exec()`**：进入应用程序的事件循环，保持应用程序持续运行。