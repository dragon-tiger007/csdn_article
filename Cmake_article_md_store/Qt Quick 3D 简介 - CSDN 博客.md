> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/m0_73443478/article/details/127559382?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522a1619705a6ac0f962797ca2019567666%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=a1619705a6ac0f962797ca2019567666&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-9-127559382-null-null.142^v102^pc_search_result_base5&utm_term=QT%20quick3d&spm=1018.2226.3001.4187)

![](https://i-blog.csdnimg.cn/blog_migrate/1dd1c002c778d1f98632a33e3f82c97f.png)

Qt Quick 3D 简介
--------------

前言
--

Qt Quick 3D 是 Qt 自带的一套 3D 图形系统，与传统的 Qt 3D 不同的是，Qt Quick 3D 采用 [QML](https://so.csdn.net/so/search?q=QML&spm=1001.2101.3001.7020) 来进行开发。本节则对 Qt Quick 3D 进行一次简单的介绍。

配置场景
----

在 main.qml 中设置整个场景（entire scene）。

在使用 Qt Quick 3D 之前，为了能够使用 QtQuick3D 模块中的类型，我们必须导入它:

```
 import QtQuick
 import QtQuick3D
```

为了绘制任何 3D 场景，我们需要在 Qt Quick 场景中有一个 3D 视口。这是由 View3D 类提供的，这是我们定义场景的地方。在一个应用程序中也可以有多个视图。

首先需要从定义场景的环境开始，用天蓝色清除背景颜色。这里在 SceneEnvironment 中为视图的 environment 属性指定了天蓝色。SceneEnvironment 描述了与场景环境相关的各种属性，如色调映射设置，基于图像照明的光[探针](https://so.csdn.net/so/search?q=%E6%8E%A2%E9%92%88&spm=1001.2101.3001.7020)设置，背景模式，或环境遮挡参数。除此之外，它还可以控制抗走样。在这里设置 clearColor 和 backgroundMode 属性以获得蓝色背景。

```
 environment: SceneEnvironment {
     clearColor: "skyblue"
     backgroundMode: SceneEnvironment.Color
 }
```

Meshes 网格
---------

在之前的文章中已经介绍过 3D 模型需要转换为. me[sh 文件](https://so.csdn.net/so/search?q=sh%E6%96%87%E4%BB%B6&spm=1001.2101.3001.7020)然后传递给 QML 进行使用。

为了使场景更有趣一点，需要添加一些网格。在 Quick 3D 中有许多方便的内置网格，例如球体、立方体、圆锥或圆柱体。这些是通过使用特殊的标识符来引用的，比如在模型节点的源属性中使用 #Sphere，#Cube，或者#Rectangle。除了内置的原语外，还可以指定. mesh 文件。为了从 FBX 或 glTF2 资产生成. mesh 文件，需要使用 Balsam 资产导入工具处理这些资产。下面是添加蓝色球体和红色扁平圆柱的代码:

```
 Model {
     position: Qt.vector3d(0, -200, 0)
     source: "#Cylinder"
     scale: Qt.vector3d(2, 0.2, 1)
     materials: [ DefaultMaterial {
             diffuseColor: "red"
         }
     ]
 }
 
 Model {
     position: Qt.vector3d(0, 150, 0)
     source: "#Sphere"
 
     materials: [ DefaultMaterial {
             diffuseColor: "blue"
         }
     ]
 
     SequentialAnimation on y {
         loops: Animation.Infinite
         NumberAnimation {
             duration: 3000
             to: -150
             from: 150
             easing.type:Easing.InQuad
         }
         NumberAnimation {
             duration: 3000
             to: 150
             from: -150
             easing.type:Easing.OutQuad
         }
     }
 }
```

相机
--

然后定义一个摄像机，它指定如何将 3D 场景的内容投影到 2D 表面上。在这里使用了透视图相机，它为我们提供了一个透视图投影。正投影也可能通过正投影相机类型。相机的默认方向是向前矢量沿负 Z 轴，向上矢量沿正 Y 轴。这里将相机在 Z 轴上移动回 300。此外，它在 Y 轴上移动了一点，并在 X 轴上稍微旋转，使其看起来稍微向下。

```
 PerspectiveCamera {
     position: Qt.vector3d(0, 200, 300)
     eulerRotation.x: -30
 }
```

### 灯光

最后设置一个灯光进行照射，3D 模型反射这些灯光后才能被肉眼所看见。

```
 DirectionalLight {
     eulerRotation.x: -30
     eulerRotation.y: -70
 }
```