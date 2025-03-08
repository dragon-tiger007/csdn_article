> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/zjgo007/article/details/122922420)

       如果我们想在 [QML](https://so.csdn.net/so/search?q=QML&spm=1001.2101.3001.7020) 中使用 3D 且你之前没有三维程序开发的基础，使用 Qt Quick 3D 是个不错的选择，下面我介绍如何使用 Qt Quick 3D 加载 3d 模型。注意：Qt Quick 3D 从 Qt 5.15 之后开始被添加到 Qt 中，三维模型使用了.[mesh](https://so.csdn.net/so/search?q=mesh&spm=1001.2101.3001.7020) 格式的模型文件，关于如何将 3D 场景（如. obj）转换为. mesh，可参考我的博客：[Qt Quick 3D 中将 3D 场景 (如. obj) 转换为. mesh](https://blog.csdn.net/zjgo007/article/details/122878344 "Qt Quick 3D中将3D场景(如.obj)转换为.mesh")

**步骤一：在新工程中添加模块：**

> `import QtQuick3D 1.15`

**步骤二：切换到 Qt Creator 的设计师模块（此处是为了介绍可视化开发，手撸代码也是可以的）**

**选择设计师模式为 “3D Preset”, 如图：**

![](https://i-blog.csdnimg.cn/blog_migrate/5a6b4542e8122108f391384a6e208ae1.png)

        此时设计师界面中显示了类似于 [C4D](https://so.csdn.net/so/search?q=C4D&spm=1001.2101.3001.7020) 的三维编辑器，使用滚轮可以缩放，按住 Alt 后使用鼠标左键可以旋转查看的位置。

**步骤三：在左侧的控件选择栏中选择并拖入 View3D 模块**

        View3D 模块拖入后自带了一个简单的 3D 模板，在 3D Editor 中可看到模板样式，如图：

![](https://i-blog.csdnimg.cn/blog_migrate/252fbb61ba60ff70b30172541d711790.png)

模板结构如下：

![](https://i-blog.csdnimg.cn/blog_migrate/daab12e39bf7de12ccd4b3faab4c7af0.png)

> SceneEnvironment：渲染的环境相关设置
> 
> Node：3D 的节点，类似于 quick 中的 Item。便于对多个控件单一的同一控制
> 
> DirectionalLight：光源
> 
> PerspectiveCamera：远景相机（显示的 3D 模型根据相机的远近进行缩放）
> 
> Model：模型，用于显示加载 3D 模型
> 
> DefaultMaterial：模型材质设置

 **步骤四：加载自定义的 3D 模型**

        在 qrc 资源中添加模型文件 “test.mesh” 并修改 Model 的模型源路径 source，该模型由一个. obj 转换而成，转换方法为：[Qt Quick 3D 中将 3D 场景 (如. obj) 转换为. mesh](https://blog.csdn.net/zjgo007/article/details/122878344 "Qt Quick 3D中将3D场景(如.obj)转换为.mesh")，调整镜头位置，运行程序如下：

![](https://i-blog.csdnimg.cn/blog_migrate/09c6f0d3e108a26991dc35c973767b9f.png)

示例程序 github 源码：[https://github.com/zjgo007/QtQuick3D/tree/master/Show3dModel](https://github.com/zjgo007/QtQuick3D/tree/master/Show3dModel "https://github.com/zjgo007/QtQuick3D/tree/master/Show3dModel")

下一篇：[Qt Quick 3D 系列（二）：鼠标控制 3D 模型旋转缩放](https://blog.csdn.net/zjgo007/article/details/122922509 "Qt Quick 3D系列（二）：鼠标控制3D模型旋转缩放") 

主要 QML 源码：

```
import QtQuick 2.15
import QtQuick.Window 2.15
import QtQuick3D 1.15
 
Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")
 
    View3D {
        id: view3D
        anchors.fill: parent
        environment: sceneEnvironment
        SceneEnvironment {
            id: sceneEnvironment
            antialiasingQuality: SceneEnvironment.High
            antialiasingMode: SceneEnvironment.MSAA
        }
 
        Node {
            id: node
            DirectionalLight {
                id: directionalLight
            }
 
            PerspectiveCamera {
                id: camera
                z: 15
            }
 
            Model {
                id: cubeModel
                source: "test.mesh"
                DefaultMaterial {
                    id: cubeMaterial
                    diffuseColor: "#4aee45"
                }
                materials: cubeMaterial
            }
        }
    }
}
```