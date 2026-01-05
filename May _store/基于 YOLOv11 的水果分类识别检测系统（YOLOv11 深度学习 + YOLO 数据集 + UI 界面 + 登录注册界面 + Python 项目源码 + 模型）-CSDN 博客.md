> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/BQAIT/article/details/156330905?spm=1001.2014.3001.5506)

###  一、项目介绍

本文基于 YOLOv11 [深度学习框架](https://so.csdn.net/so/search?q=%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E6%A1%86%E6%9E%B6&spm=1001.2101.3001.7020)，设计并实现了一种高效的水果分类识别检测系统，支持 6 类水果的精准检测，包括金冠苹果（golden delicious）、澳洲青苹果（granny smith）、梨（pear）、红蛇果（red delicious）、红油桃（red nectarine）及黄桃（yellow peach）。系统采用 YOLOv11 算法，结合包含 5,481 张训练图像和 263 张验证图像的自定义数据集进行模型训练，实现了高准确率的目标检测与分类。此外，系统集成用户友好的 UI 界面，并配备登录注册功能，提升了交互体验与数据安全性。实验结果表明，该系统在复杂场景下具有良好的鲁棒性和实时性，可应用于农业自动化、智能零售等场景。

**引言**  
随着计算机视觉技术的快速发展，基于深度学习的目标检测算法在农业自动化、食品质检等领域展现出广泛应用前景。水果分类识别作为其中的典型任务，对提升分拣效率、减少人工成本具有重要意义。然而，传统方法受限于光照、遮挡及果实形态多样性等问题，检测精度与泛化能力不足。YOLOv11 作为 YOLO 系列的最新改进模型，在检测速度与精度上取得了进一步平衡，为复杂场景下的水果识别提供了新思路。

本文基于 YOLOv11 构建了一套端到端的水果分类检测系统，针对 6 类常见水果进行优化训练。数据集包含 5,481 张训练图像与 263 张验证图像，覆盖多角度、多背景的果实样本。系统结合 PyQt 等框架开发了可视化 UI 界面，集成用户登录注册模块，确保数据管理的安全性与便捷性。实验验证表明，该系统在测试集上实现了 0.988 的 mAP（平均精度），同时满足实时性需求。本研究为农业智能化应用提供了可行的技术方案，并为 YOLOv11 在细粒度目标检测中的性能优化提供了实践参考。

**目录**

 [一、项目介绍](#t1)

[二、项目功能展示](#t2)

[2.1 用户登录系统](#t3)

[2.2 检测功能](#t4)

[2.3 检测结果显示](#t5)

[2.4 参数配置](#t6)

[2.5 其他功能](#t7)

[3. 技术特点](#t8)

[4. 系统流程](#t9)

[三、数据集介绍](#t10)

[1. 数据集组成](#t11)

[数据集配置文件](#t12)

[四、项目环境配置](#t13)

[创建虚拟环境](#t14)

[安装所需要库](#t15)

[五、模型训练](#t16)

[训练代码](#t17)

[训练结果](#t18)

[六、核心代码](#t19)

[🔐登录注册验证](#t20)

[🎯 多重检测模式](#t21)

[🖼️ 沉浸式可视化](#t22)

[⚙️ 参数配置系统](#t23)

[✨ UI 美学设计](#t24)

[🔄 智能工作流](#t25)

[七、项目源码（视频简介）](#t26)

[基于深度学习 YOLOv11 的水果分类识别检测系统（YOLOv11+YOLO 数据集 + UI 界面 + 登录注册界面 + Python 项目源码 + 模型）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1c8bzzME6D?spm_id_from=333.788.videopod.sections&vd_source=549d0b4e2b8999929a61a037fcce3b0f "基于深度学习YOLOv11的水果分类识别检测系统（YOLOv11+YOLO数据集+UI界面+登录注册界面+Python项目源码+模型）_哔哩哔哩_bilibili")

基于深度学习 YOLOv11 的水果分类识别检测系统（YOLOv11+YOLO 数据集 + UI 界面 + 登录注册界面 + Python 项目源码 + 模型）

### 二、项目功能展示

✅ 用户登录注册：支持密码检测和安全性验证。

✅ 三种检测模式：基于 YOLOv11 模型，支持图片、视频和实时摄像头三种检测，精准识别目标。

✅ 双画面对比：同屏显示原始画面与检测结果。

✅ 数据可视化：实时表格展示检测目标的类别、置信度及坐标。

✅智能参数调节：提供置信度滑块，动态优化检测精度，适应不同场景需求。

✅科幻风交互界面：深色主题搭配动态光效，减少视觉疲劳，提升操作体验。

✅多线程高性能架构：独立检测线程保障流畅运行，实时状态提示，响应迅速无卡顿。

![](https://i-blog.csdnimg.cn/direct/f5553f76a1594cfb827f314652986beb.png)![](https://i-blog.csdnimg.cn/direct/25f813d7150547948372373bd734426f.png)![](https://i-blog.csdnimg.cn/direct/fe87b987d580408ba86be352de83e1f9.png)![](https://i-blog.csdnimg.cn/direct/7df2be115d1f4b6fb6864e5da3260856.png)

#### 2.1 用户登录系统

*   提供用户登录和注册功能
    
*   用户名和密码验证
    
*   账户信息本地存储 (accounts.json)
    
*   密码长度至少 6 位的安全要求
    

#### 2.2 检测功能

*   **图片检测**：支持 JPG/JPEG/PNG/BMP 格式图片的火焰烟雾检测
    
*   **视频检测**：支持 MP4/AVI/MOV 格式视频的逐帧检测
    
*   **摄像头检测**：实时摄像头流检测 (默认摄像头 0)
    
*   检测结果保存到 "results" 目录
    

#### 2.3 检测结果显示

*   显示原始图像和检测结果图像
    
*   检测结果表格展示，包含：
    
    *   检测到的类别
        
    *   置信度分数
        
    *   物体位置坐标 (x,y)、
        

#### 2.4 参数配置

*   模型选择
    
*   置信度阈值调节 (0-1.0)
    
*   IoU(交并比) 阈值调节 (0-1.0)
    
*   实时同步滑块和数值输入框
    

#### 2.5 其他功能

*   检测结果保存功能
    
*   视频检测时自动保存结果视频
    
*   状态栏显示系统状态和最后更新时间
    
*   无边框窗口设计，可拖动和调整大小
    

### 3. 技术特点

*   采用多线程处理检测任务，避免界面卡顿
    
*   精美的 UI 设计，具有科技感的视觉效果：
    
    *   发光边框和按钮
        
    *   悬停和按下状态效果
        
    *   自定义滑块、表格和下拉框样式
        
*   检测结果保存机制
    
*   响应式布局，适应不同窗口大小
    

### 4. 系统流程

1.  用户登录 / 注册
    
2.  选择检测模式 (图片 / 视频 / 摄像头)
    
3.  调整检测参数 (可选)
    
4.  开始检测并查看结果
    
5.  可选择保存检测结果
    
6.  停止检测或切换其他模式
    

### 三、数据集介绍

本研究的**水果分类识别检测系统**采用**自定义 YOLO 格式数据集**，共包含 **6 类水果**，分别为：**金冠苹果（golden delicious）、澳洲青苹果（granny smith）、梨（pear）、红蛇果（red delicious）、红油桃（red nectarine）和黄桃（yellow peach）**。

##### **1. 数据集组成**

*   **训练集（Training Set）**：**5,481 张图像**
    
*   **验证集（Validation Set）**：**263 张图像**
    

#### 数据集配置文件

数据集采用标准化 YOLO 格式组织：

```
train: F:\水果分类检测数据集\水果分类检测数据集\images\train
val: F:\水果分类检测数据集\水果分类检测数据集\images\val
test: # test images (optional)
 
nc: 6
names: ['golden delicious', 'granny smith', 'pear', 'red delicious', 'red nectarine', 'yellow peach']
 
```

![](https://i-blog.csdnimg.cn/direct/86d78f2b47844e799658d396f8e6c1a7.png)![](https://i-blog.csdnimg.cn/direct/50300f0dbe014b4eba5a54bc6fc33651.png)![](https://i-blog.csdnimg.cn/direct/7594d52a742c4a8c97a7216181c1970e.png)![](https://i-blog.csdnimg.cn/direct/44e43117f16f4a98b0ce8d6238cece60.jpeg)![](https://i-blog.csdnimg.cn/direct/48940b9489c64d7080f1bd5a30f68914.jpeg)![](https://i-blog.csdnimg.cn/direct/1fa5a415aca34d3e9919c3044e108423.jpeg)![](https://i-blog.csdnimg.cn/direct/fc249069ae3d4586bf14458654476f2f.jpeg)

### 四、项目环境配置

#### 创建虚拟环境

首先新建一个 Anaconda 环境，每个项目用不同的环境，这样项目中所用的依赖包互不干扰。

终端输入

conda create -n yolov11 python==3.9

![](https://i-blog.csdnimg.cn/direct/7893743e43a141919e03c53b4fca7a96.png)

**激活虚拟环境**

conda activate yolov11  
 

![](https://i-blog.csdnimg.cn/direct/23c4f03220b3401c9831ee11903e96a6.png)

**安装 cpu 版本 pytorch**

```
pip install torch torchvision torchaudio

```

![](https://i-blog.csdnimg.cn/direct/d05699a50fe14d5dbaa96ca8c40fb191.png)

#### **安装所需要库**

pip install -r requirements.txt

**pycharm 中配置 anaconda**

![](https://i-blog.csdnimg.cn/direct/cd0723a7a34645d5ba34e4a1cb89def7.png)

![](https://i-blog.csdnimg.cn/direct/486596de6b324e36a2eb77d787f64605.png)

### 五、模型训练

#### 训练代码

```
from ultralytics import YOLO
 
model_path = 'yolo11s.pt'
data_path = 'data.yaml'
 
if __name__ == '__main__':
    model = YOLO(model_path)
    results = model.train(data=data_path,
                          epochs=100,
                          batch=8,
                          device='0',
                          workers=0,
                          project='runs',
                          name='exp',
                          )
```

```
根据实际情况更换模型
# yolov11n.yaml (nano)：轻量化模型，适合嵌入式设备，速度快但精度略低。
# yolov11s.yaml (small)：小模型，适合实时任务。
# yolov11m.yaml (medium)：中等大小模型，兼顾速度和精度。
# yolov11b.yaml (base)：基本版模型，适合大部分应用场景。
# yolov11l.yaml (large)：大型模型，适合对精度要求高的任务。

```

*   `--batch 8`：每批次 8 张图像。
*   `--epochs 100`：训练 100 轮。
*   `--datasets/data.yaml`：数据集配置文件。
*   `--weights yolov11s.pt`：初始化模型权重，`yolov11s.pt` 是预训练的轻量级 YOLO 模型。

#### 训练结果

![](https://i-blog.csdnimg.cn/direct/1c9f8b8267f1430984e0d9464e2d23cb.png)

![](https://i-blog.csdnimg.cn/direct/581a4f1449f74141823dcdb4e5ccf615.png)

![](https://i-blog.csdnimg.cn/direct/03be53bf3add40d3a741bb26901495e5.png)

![](https://i-blog.csdnimg.cn/direct/71704b58365f4aef88ddac82f9625bde.png)

![](https://i-blog.csdnimg.cn/direct/56faff5cb7f7411cb24008993a192e1d.png)![](https://i-blog.csdnimg.cn/direct/fe351c43b55d41e9a323879610282640.png)![](https://i-blog.csdnimg.cn/direct/efda7e2e4d1c4698a83d336b576d5d48.png)![](https://i-blog.csdnimg.cn/direct/1ed2922c54df4a6a9c862f5ce4f79a18.png)![](https://i-blog.csdnimg.cn/direct/083e3eec40ce409c85df3f6236a03b4c.png)![](https://i-blog.csdnimg.cn/direct/bea5ec3ff5a747949fa437f39c49adbb.jpeg)

![](https://i-blog.csdnimg.cn/direct/992f0e3afb21473a87fcf10ddd752e43.jpeg)

### 六、核心代码

![](https://i-blog.csdnimg.cn/direct/fd481eda5efa4061bf9b4f1c7370d44d.png)

```
import sys
 
import cv2
import numpy as np
from PyQt5.QtWidgets import QApplication, QMessageBox, QFileDialog
from PyQt5.QtCore import QThread, pyqtSignal
from ultralytics import YOLO
from UiMain import UiMainWindow
import time
import os
from PyQt5.QtWidgets import QDialog
from LoginWindow import LoginWindow
 
class DetectionThread(QThread):
    frame_received = pyqtSignal(np.ndarray, np.ndarray, list)  # 原始帧, 检测帧, 检测结果
    finished_signal = pyqtSignal()  # 线程完成信号
 
    def __init__(self, model, source, conf, iou, parent=None):
        super().__init__(parent)
        self.model = model
        self.source = source
        self.conf = conf
        self.iou = iou
        self.running = True
 
    def run(self):
        try:
            if isinstance(self.source, int) or self.source.endswith(('.mp4', '.avi', '.mov')):  # 视频或摄像头
                cap = cv2.VideoCapture(self.source)
                while self.running and cap.isOpened():
                    ret, frame = cap.read()
                    if not ret:
                        break
 
                    # 保存原始帧
                    original_frame = frame.copy()
 
                    # 检测
                    results = self.model(frame, conf=self.conf, iou=self.iou)
                    annotated_frame = results[0].plot()
 
                    # 提取检测结果
                    detections = []
                    for result in results:
                        for box in result.boxes:
                            class_id = int(box.cls)
                            class_name = self.model.names[class_id]
                            confidence = float(box.conf)
                            x, y, w, h = box.xywh[0].tolist()
                            detections.append((class_name, confidence, x, y))
 
                    # 发送信号
                    self.frame_received.emit(
                        cv2.cvtColor(original_frame, cv2.COLOR_BGR2RGB),
                        cv2.cvtColor(annotated_frame, cv2.COLOR_BGR2RGB),
                        detections
                    )
 
                    # 控制帧率
                    time.sleep(0.03)  # 约30fps
 
                cap.release()
            else:  # 图片
                frame = cv2.imread(self.source)
                if frame is not None:
                    original_frame = frame.copy()
                    results = self.model(frame, conf=self.conf, iou=self.iou)
                    annotated_frame = results[0].plot()
 
                    # 提取检测结果
                    detections = []
                    for result in results:
                        for box in result.boxes:
                            class_id = int(box.cls)
                            class_name = self.model.names[class_id]
                            confidence = float(box.conf)
                            x, y, w, h = box.xywh[0].tolist()
                            detections.append((class_name, confidence, x, y))
 
                    self.frame_received.emit(
                        cv2.cvtColor(original_frame, cv2.COLOR_BGR2RGB),
                        cv2.cvtColor(annotated_frame, cv2.COLOR_BGR2RGB),
                        detections
                    )
 
        except Exception as e:
            print(f"Detection error: {e}")
        finally:
            self.finished_signal.emit()
 
    def stop(self):
        self.running = False
 
 
class MainWindow(UiMainWindow):
    def __init__(self):
        super().__init__()
 
        # 初始化模型
        self.model = None
        self.detection_thread = None
        self.current_image = None
        self.current_result = None
        self.video_writer = None
        self.is_camera_running = False
        self.is_video_running = False
        self.last_detection_result = None  # 新增：保存最后一次检测结果
 
        # 连接按钮信号
        self.image_btn.clicked.connect(self.detect_image)
        self.video_btn.clicked.connect(self.detect_video)
        self.camera_btn.clicked.connect(self.detect_camera)
        self.stop_btn.clicked.connect(self.stop_detection)
        self.save_btn.clicked.connect(self.save_result)
 
        # 初始化模型
        self.load_model()
 
    def load_model(self):
        try:
            model_name = self.model_combo.currentText()
            self.model = YOLO(f"{model_name}.pt")  # 自动下载或加载本地模型
            self.update_status(f"模型 {model_name} 加载成功")
        except Exception as e:
            QMessageBox.critical(self, "错误", f"模型加载失败: {str(e)}")
            self.update_status("模型加载失败")
 
    def detect_image(self):
        if self.detection_thread and self.detection_thread.isRunning():
            QMessageBox.warning(self, "警告", "请先停止当前检测任务")
            return
 
        file_path, _ = QFileDialog.getOpenFileName(
            self, "选择图片", "", "图片文件 (*.jpg *.jpeg *.png *.bmp)")
 
        if file_path:
            self.clear_results()
            self.current_image = cv2.imread(file_path)
            self.current_image = cv2.cvtColor(self.current_image, cv2.COLOR_BGR2RGB)
            self.display_image(self.original_image_label, self.current_image)
 
            # 创建检测线程
            conf = self.confidence_spinbox.value()
            iou = self.iou_spinbox.value()
            self.detection_thread = DetectionThread(self.model, file_path, conf, iou)
            self.detection_thread.frame_received.connect(self.on_frame_received)
            self.detection_thread.finished_signal.connect(self.on_detection_finished)
            self.detection_thread.start()
 
            self.update_status(f"正在检测图片: {os.path.basename(file_path)}")
 
    def detect_video(self):
        if self.detection_thread and self.detection_thread.isRunning():
            QMessageBox.warning(self, "警告", "请先停止当前检测任务")
            return
 
        file_path, _ = QFileDialog.getOpenFileName(
            self, "选择视频", "", "视频文件 (*.mp4 *.avi *.mov)")
 
        if file_path:
            self.clear_results()
            self.is_video_running = True
 
            # 初始化视频写入器
            cap = cv2.VideoCapture(file_path)
            frame_width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
            frame_height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
            fps = cap.get(cv2.CAP_PROP_FPS)
            cap.release()
 
            # 创建保存路径
            save_dir = "results"
            os.makedirs(save_dir, exist_ok=True)
            timestamp = time.strftime("%Y%m%d_%H%M%S")
            save_path = os.path.join(save_dir, f"result_{timestamp}.mp4")
 
            fourcc = cv2.VideoWriter_fourcc(*'mp4v')
            self.video_writer = cv2.VideoWriter(save_path, fourcc, fps, (frame_width, frame_height))
 
            # 创建检测线程
            conf = self.confidence_spinbox.value()
            iou = self.iou_spinbox.value()
            self.detection_thread = DetectionThread(self.model, file_path, conf, iou)
            self.detection_thread.frame_received.connect(self.on_frame_received)
            self.detection_thread.finished_signal.connect(self.on_detection_finished)
            self.detection_thread.start()
 
            self.update_status(f"正在检测视频: {os.path.basename(file_path)}")
 
    def detect_camera(self):
        if self.detection_thread and self.detection_thread.isRunning():
            QMessageBox.warning(self, "警告", "请先停止当前检测任务")
            return
 
        self.clear_results()
        self.is_camera_running = True
 
        # 创建检测线程 (默认使用摄像头0)
        conf = self.confidence_spinbox.value()
        iou = self.iou_spinbox.value()
        self.detection_thread = DetectionThread(self.model, 0, conf, iou)
        self.detection_thread.frame_received.connect(self.on_frame_received)
        self.detection_thread.finished_signal.connect(self.on_detection_finished)
        self.detection_thread.start()
 
        self.update_status("正在从摄像头检测...")
```

#### 🔐登录注册验证

对应文件：`LoginWindow.py`

```
# 账户验证核心逻辑
def handle_login(self):
    username = self.username_input.text().strip()
    password = self.password_input.text().strip()
    
    if not username or not password:
        QMessageBox.warning(self, "警告", "用户名和密码不能为空！")
        return
    
    if username in self.accounts and self.accounts[username] == password:
        self.accept()  # 验证通过
    else:
        QMessageBox.warning(self, "错误", "用户名或密码错误！")
 
# 密码强度检查（注册时）
def handle_register(self):
    if len(password) < 6:  # 密码长度≥6位
        QMessageBox.warning(self, "警告", "密码长度至少为6位！")
```

#### 🎯 **多重检测模式**

对应文件：`main.py`

图片检测

```
def detect_image(self):
    file_path, _ = QFileDialog.getOpenFileName(
        self, "选择图片", "", "图片文件 (*.jpg *.jpeg *.png *.bmp)")
    if file_path:
        self.detection_thread = DetectionThread(self.model, file_path, conf, iou)
        self.detection_thread.start()  # 启动检测线程
```

视频检测

```
def detect_video(self):
    file_path, _ = QFileDialog.getOpenFileName(
        self, "选择视频", "", "视频文件 (*.mp4 *.avi *.mov)")
    if file_path:
        self.video_writer = cv2.VideoWriter()  # 初始化视频写入器
        self.detection_thread = DetectionThread(self.model, file_path, conf, iou)
```

实时摄像头

```
def detect_camera(self):
    self.detection_thread = DetectionThread(self.model, 0, conf, iou)  # 摄像头设备号0
    self.detection_thread.start()
```

#### 🖼️ **沉浸式可视化**

对应文件：`UiMain.py`

双画面显示

```
def display_image(self, label, image):
    q_img = QImage(image.data, w, h, bytes_per_line, QImage.Format_RGB888)
    pixmap = QPixmap.fromImage(q_img)
    label.setPixmap(pixmap.scaled(label.size(), Qt.KeepAspectRatio))  # 自适应缩放
```

结果表格

```
def add_detection_result(self, class_name, confidence, x, y):
    self.results_table.insertRow(row)
    items = [
        QTableWidgetItem(class_name),  # 类别列
        QTableWidgetItem(f"{confidence:.2f}"),  # 置信度
        QTableWidgetItem(f"{x:.1f}"),  # X坐标
        QTableWidgetItem(f"{y:.1f}")   # Y坐标
    ]
```

#### ⚙️ **参数配置系统**

对应文件：`UiMain.py`

双阈值联动控制

```
# 置信度阈值同步
def update_confidence(self, value):
    confidence = value / 100.0
    self.confidence_spinbox.setValue(confidence)  # 滑块→数值框
    self.confidence_label.setText(f"置信度阈值: {confidence:.2f}")
 
# IoU阈值同步  
def update_iou(self, value):
    iou = value / 100.0
    self.iou_spinbox.setValue(iou)
```

#### ✨ **UI 美学设计**

对应文件：`UiMain.py`

科幻风格按钮

```
def create_button(self, text, color):
    return f"""
    QPushButton {{
        border: 1px solid {color};
        color: {color};
        border-radius: 6px;
    }}
    QPushButton:hover {{
        background-color: {self.lighten_color(color, 10)};
        box-shadow: 0 0 10px {color};  # 悬停发光效果
    }}
    """
```

动态状态栏

```
def update_status(self, message):
    self.status_bar.showMessage(
        f"状态: {message} | 最后更新: {time.strftime('%H:%M:%S')}"  # 实时时间戳
    )
```

#### 🔄 **智能工作流**

对应文件：`main.py`

线程管理

```
class DetectionThread(QThread):
    frame_received = pyqtSignal(np.ndarray, np.ndarray, list)  # 信号量通信
    
    def run(self):
        while self.running:  # 多线程检测循环
            results = self.model(frame, conf=self.conf, iou=self.iou)
            self.frame_received.emit(original_frame, result_frame, detections)
```

**`七、项目源码（视频简介）`**
------------------

![](https://i-blog.csdnimg.cn/direct/917765c744d64f64ba56d0f177798e63.png)

![](https://i-blog.csdnimg.cn/direct/dc91a18345fc4f4a9dac5530a019acff.jpeg)

[基于深度学习 YOLOv11 的水果分类识别检测系统（YOLOv11+YOLO 数据集 + UI 界面 + 登录注册界面 + Python 项目源码 + 模型）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1c8bzzME6D?spm_id_from=333.788.videopod.sections&vd_source=549d0b4e2b8999929a61a037fcce3b0f "基于深度学习YOLOv11的水果分类识别检测系统（YOLOv11+YOLO数据集+UI界面+登录注册界面+Python项目源码+模型）_哔哩哔哩_bilibili")

基于深度学习 YOLOv11 的水果分类识别检测系统（YOLOv11+YOLO 数据集 + UI 界面 + 登录注册界面 + Python 项目源码 + 模型）