> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_67105081/article/details/137545156?spm=1001.2014.3001.5506)

**目录**

[1. 环境配置](#t0)

[2. 数据集获取](#t1)

[2.1 网上搜索公开数据集](#t2)

[2.2 自制数据集](#t3)

[2.2.1 Labelimg 安装](#t4)

[2.2.2 Labelimg 使用](#t5)

[2.3 数据集转换及划分](#t6)

[2.3.1 数据集 VOC 格式转 yolo 格式](#t7)

[2.3.2 数据集划分](#t8)

[3. 训练模型](#t9)

[3.1 创建 data.yaml](#t10)

[3.2 训练模型](#t11)

[4. 模型测试](#t12)

[5. 可视化界面](#t13)

训练自己的数据集分为 4 部分，先配置环境，再获取制作自己的数据集，然后修改配置训练，最后验证训练结果，附带可视化界面。yolov8 训练起来较为简单，如果有其他目标检测的数据集理论上可以直接拿来用，从第 3 训练模型开始看，新手小白 0 基础建议一步一步跟着来，哪里看不懂的或者遇到哪有问题可以发到评论区交流~

1. [环境配置](https://so.csdn.net/so/search?q=%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE&spm=1001.2101.3001.7020)
-------------------------------------------------------------------------------------------------------

在训练 yolov8 模型前环境必须配置完成，还不会配置环境的可以看我的这篇博客

[深度学习目标检测：yolov8(Ultralytics) 环境配置，适合 0 基础小白，超详细 - CSDN 博客文章浏览阅读 2.9w 次，点赞 155 次，收藏 604 次。深度学习目标检测 yolov8 小白教程环境搭建，超级简单的教程，纯新手方便理解，一看就会。_ultralytics![](https://g.csdnimg.cn/static/logo/favicon32.ico)https://blog.csdn.net/qq_67105081/article/details/137519207](https://blog.csdn.net/qq_67105081/article/details/137519207 "深度学习目标检测：yolov8(Ultralytics)环境配置，适合0基础小白，超详细-CSDN博客") 环境配置完验证之后就可以获取数据集。

点击下载训练源码 [夸克网盘下载](https://pan.quark.cn/s/f26376f0de96 "夸克网盘下载") ，建议先全部转存提前下载，若有需要下载的资源失效，可至公众号获取百度盘链接下载。

  YOLOv8 网络结构图，论文必备，无水印图可 微信公众号 - 笑脸惹桃花 回复 “888” 获取。

![](https://i-blog.csdnimg.cn/direct/f0b14b5b4d784d288c2f68a235f0d427.png)​

 2. 数据集
-------

数据集可以使用网上公开的跟自己研究相契合的数据集，或者是搜索 / 拍摄自己研究所需要的图片进行标注制作成数据集，这里两种方法都详细介绍一下，比如这里做一个安全帽检测的研究。

### 2.1 网上搜索公开数据集

使用网上公开的数据集，可供寻找的网站也有很多，这里仅介绍我使用过效果不错的网站

#### 2.1.1 搜索引擎

最基础的搜索方式，需要做什么方面的研究就在上面搜索，

#### 2.1.2 Kaggle

[Kaggle: Your Machine Learning and Data Science CommunityKaggle is the world’s largest data science community with powerful tools and resources to help you achieve your data science goals.https://www.kaggle.com/https://www.kaggle.com/![](https://csdnimg.cn/release/blog_editor_html/release2.4.2/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=P7R7)https://www.kaggle.com/](https://www.kaggle.com/ "Kaggle: Your Machine Learning and Data Science CommunityKaggle is the world’s largest data science community with powerful tools and resources to help you achieve your data science goals.https://www.kaggle.com/https://www.kaggle.com/")

在搜索框输入安全帽的英文（因为是英文网站，都需要翻译成英文后搜索）Safety helmet （找不到结果可以多尝试不同的关键词）

![](https://i-blog.csdnimg.cn/blog_migrate/365f9d2189da0708a3608eebc9c16222.png)​

搜索后就可以找到相关的内容，点击 datasets 筛选数据集，下载几个看一下数据集是否为目标检测的数据格式，一般文件夹为 JPEGImages 和 Annotations 包含这两个就可以使用

![](https://i-blog.csdnimg.cn/blog_migrate/12f411709fc89a0e120edd66a17240e8.png)​

点进去查看相关数据是否符合要求，点击 download 即可下载。

#### 2.1.3 Roboflow

[Roboflow Universe: Computer Vision DatasetsDownload free, open source datasets and pre-trained computer vision machine learning models.https://universe.roboflow.com/https://universe.roboflow.com/![](https://csdnimg.cn/release/blog_editor_html/release2.4.2/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=P7R7)https://universe.roboflow.com/](https://universe.roboflow.com/ "Roboflow Universe: Computer Vision DatasetsDownload free, open source datasets and pre-trained computer vision machine learning models.https://universe.roboflow.com/https://universe.roboflow.com/")

该网站非常适合获取目标检测数据集，文件标注格式齐全，非常推荐使用，在搜索框输入安全帽的英文（因为是英文网站，都需要翻译成英文后搜索）Safety helmet （找不到结果可以多尝试不同的关键词），找到跟自己研究相关的

![](https://i-blog.csdnimg.cn/direct/882bbe0b9d1445078c63d402db457629.png)​

点进去后，可以看到关于数据集的介绍，我们点击左侧的 Datasat，查看数据集。

![](https://i-blog.csdnimg.cn/direct/193c662883614ff79c900d29fc144ba5.png)​

点击右侧 Download Dataset 下载，该网站可自选下载格式，我们选择 Pascal VOC 格式的，格式转换起来也较为方便，下载的数据一般会划分好训练验证测试集，可以全部打乱重分也可以直接用划分好的。

![](https://i-blog.csdnimg.cn/direct/05c2bd9862354922929d14a4e7aec2e5.png)​

若是下载到分割数据集，即 json 格式的标注可以看我的这篇文章转为 txt。

[深度学习数据集分割 json 文件转目标检测 txt 文件_json 图像分割数据集格式 - CSDN 博客文章浏览阅读 1.3k 次，点赞 11 次，收藏 23 次。本文介绍了一种方法，用于将包含多边形标注的 JSON 文件转换为文本文件，以便于目标检测任务中使用矩形框形式的标注。作者通过 Python 脚本获取每个多边形的类别、坐标点信息并计算出框选区域，然后保存为 txt 文件。https://blog.csdn.net/qq_67105081/article/details/138123877?spm=1001.2014.3001.5502https://blog.csdn.net/qq_67105081/article/details/138123877?spm=1001.2014.3001.5502![](https://csdnimg.cn/release/blog_editor_html/release2.4.2/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=P7R7)https://blog.csdn.net/qq_67105081/article/details/138123877?spm=1001.2014.3001.5502](https://blog.csdn.net/qq_67105081/article/details/138123877?spm=1001.2014.3001.5502 "深度学习数据集分割json文件转目标检测txt文件_json图像分割数据集格式-CSDN博客文章浏览阅读1.3k次，点赞11次，收藏23次。本文介绍了一种方法，用于将包含多边形标注的JSON文件转换为文本文件，以便于目标检测任务中使用矩形框形式的标注。作者通过Python脚本获取每个多边形的类别、坐标点信息并计算出框选区域，然后保存为txt文件。https://blog.csdn.net/qq_67105081/article/details/138123877?spm=1001.2014.3001.5502https://blog.csdn.net/qq_67105081/article/details/138123877?spm=1001.2014.3001.5502")

### 2.2 自制数据集

自制数据集需要先获取一定数量的目标图片，可以拍摄或者下载，图片足够之后使用标注工具 Labelimg 进行标注

#### 2.2.1 [Labelimg 安装](https://so.csdn.net/so/search?q=Labelimg%E5%AE%89%E8%A3%85&spm=1001.2101.3001.7020)

使用 Labelimg 建议使用 python3.10 以下的环境，这里创建一个 python3.8 的虚拟环境，不会创建的可以去看我这篇博客[点击这里](https://blog.csdn.net/qq_67105081/article/details/137519207 "点击这里")。

```
conda create -n labelimg python=3.8

```

这里创建完之后进入 labelimg 环境

```
conda activate labelimg

```

进入 labelimg 环境之后通过 pip 下载 labelimg（需要关闭加速软件）。

```
pip install labelimg

```

安装完成之后就可以使用

#### 2.2.2 Labelimg 使用

在使用 labelimg 之前，需要准备好数据集存放位置，这里推荐创建一个大文件夹为 data，里面有 JPEGImages、Annotations 和 classes.txt，其中 JPEGImages 文件夹里面放所有的图片，Annotations 文件夹是将会用来对标签文件存放，classes.txt 里存放所有的类别，每种一行。

![](https://i-blog.csdnimg.cn/direct/657f40d2a8e8422baf135824776040e3.png)​

classes.txt 里存放所有的类别，可以自己起名，需要是英文，如果有空格最好用下划线比如 no_hat

![](https://i-blog.csdnimg.cn/blog_migrate/d5af58d88b4dfebf49fdee38eee7dca1.png)​

上述工作准备好之后，在 labelimg 环境中 cd 到 data 目录下，如果不是在 c 盘需要先输入其他盘符 +:

例如 d: 回车之后再输入文件路径，接着输入以下命令打开 labelimg

```
labelimg JPEGImages classes.txt

```

打开软件后可以看到左侧有很多按钮，open dir 是选择图片文件夹，上面选过了

![](https://i-blog.csdnimg.cn/blog_migrate/df59d2bfa09ae938a650bafc0a101ae8.png)​

点击 change save dir 切换到 Annotations 目录之中，点击 save 下面的图标切换到 pascal voc 格式

切换好之后点击软件上边的 view，将 Auto Save mode（切换到下一张图会自动保存标签）和 Display Labels（显示标注框和标签） 保持打开状态。

常用快捷键：

A：切换到上一张图片

D：切换到下一张图片

W：调出标注十字架

del ：删除标注框

例如，按下 w 调出标注十字架，标注完成之后选择对应的类别，这张图全部标注完后按 d 下一张

![](https://i-blog.csdnimg.cn/blog_migrate/0ea592a014a21f97f514df40c17d7171.png)​

所有图像标注完成后数据集即制作完成。

### 2.3 数据集转换及划分

#### 2.3.1 数据集 VOC 格式转 yolo 格式

如何查看自己数据集格式，打开 Annotations 文件夹，如果看到文件后缀为. xml，则为 VOC 格式，如果文件后缀为. txt 则为 yolo 格式，后缀名看不到请搜索 如何显示文件后缀名。yolov8 训练需要转为 yolo 格式训练，转换代码如下：

```
# 作者：CSDN-笑脸惹桃花 https://blog.csdn.net/qq_67105081?type=blog
# github:peng-xiaobai https://github.com/peng-xiaobai/Dataset-Conversion
 
import os
import xml.etree.ElementTree as ET
 
# 定义类别顺序
categories = ['hat','nohat']
category_to_index = {category: index for index, category in enumerate(categories)}
 
# 定义输入文件夹和输出文件夹
input_folder = r'f:\data\Annotations'  # 替换为实际的XML文件夹路径
output_folder = r'f:\data\labels'  # 替换为实际的输出TXT文件夹路径
 
# 确保输出文件夹存在
os.makedirs(output_folder, exist_ok=True)
 
# 遍历输入文件夹中的所有XML文件
for filename in os.listdir(input_folder):
	if filename.endswith('.xml'):
		xml_path = os.path.join(input_folder, filename)
		# 解析XML文件
		tree = ET.parse(xml_path)
		root = tree.getroot()
		# 提取图像的尺寸
		size = root.find('size')
		width = int(size.find('width').text)
		height = int(size.find('height').text)
		# 存储name和对应的归一化坐标
		objects = []
 
		# 遍历XML中的object标签
		for obj in root.findall('object'):
			name = obj.find('name').text
			if name in category_to_index:
				category_index = category_to_index[name]
			else:
				continue  # 如果name不在指定类别中，跳过该object
 
			bndbox = obj.find('bndbox')
			xmin = int(bndbox.find('xmin').text)
			ymin = int(bndbox.find('ymin').text)
			xmax = int(bndbox.find('xmax').text)
			ymax = int(bndbox.find('ymax').text)
 
			# 转换为中心点坐标和宽高
			x_center = (xmin + xmax) / 2.0
			y_center = (ymin + ymax) / 2.0
			w = xmax - xmin
			h = ymax - ymin
 
			# 归一化
			x = x_center / width
			y = y_center / height
			w = w / width
			h = h / height
 
			objects.append(f"{category_index} {x} {y} {w} {h}")
 
		# 输出结果到对应的TXT文件
		txt_filename = os.path.splitext(filename)[0] + '.txt'
		txt_path = os.path.join(output_folder, txt_filename)
		with open(txt_path, 'w') as f:
			for obj in objects:
				f.write(obj + '\n')
```

需要自行将类别替换，这里顺序要记住，文件夹也对应替换

#### 2.3.2 数据集划分

训练自己的 yolov8 检测模型，数据集需要划分为训练集、验证集和测试集，这里提供一个参考代码, 划分比例为 8：1：1，也可以按照自己的比例划分

```
# 作者：CSDN-笑脸惹桃花 https://blog.csdn.net/qq_67105081?type=blog
# github:peng-xiaobai https://github.com/peng-xiaobai/Dataset-Conversion
import os
import shutil
import random
 
# random.seed(0)  #随机种子，可自选开启
def split_data(file_path, label_path, new_file_path, train_rate, val_rate, test_rate):
	images = os.listdir(file_path)
	labels = os.listdir(label_path)
	images_no_ext = {os.path.splitext(image)[0]: image for image in images}
	labels_no_ext = {os.path.splitext(label)[0]: label for label in labels}
	matched_data = [(img, images_no_ext[img], labels_no_ext[img]) for img in images_no_ext if img in labels_no_ext]
 
	unmatched_images = [img for img in images_no_ext if img not in labels_no_ext]
	unmatched_labels = [label for label in labels_no_ext if label not in images_no_ext]
	if unmatched_images:
		print("未匹配的图片文件:")
		for img in unmatched_images:
			print(images_no_ext[img])
	if unmatched_labels:
		print("未匹配的标签文件:")
		for label in unmatched_labels:
			print(labels_no_ext[label])
 
	random.shuffle(matched_data)
	total = len(matched_data)
	train_data = matched_data[:int(train_rate * total)]
	val_data = matched_data[int(train_rate * total):int((train_rate + val_rate) * total)]
	test_data = matched_data[int((train_rate + val_rate) * total):]
 
	# 处理训练集
	for img_name, img_file, label_file in train_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'train', 'images')
		new_label_dir = os.path.join(new_file_path, 'train', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	# 处理验证集
	for img_name, img_file, label_file in val_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'val', 'images')
		new_label_dir = os.path.join(new_file_path, 'val', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	# 处理测试集
	for img_name, img_file, label_file in test_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'test', 'images')
		new_label_dir = os.path.join(new_file_path, 'test', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	print("数据集已划分完成")
 
if __name__ == '__main__':
	file_path = r"f:\data\JPEGImages"  # 图片文件夹
	label_path = r'f:\data\labels'  # 标签文件夹
	new_file_path = r"f:\VOCdevkit"  # 新数据存放位置
	split_data(file_path, label_path, new_file_path, train_rate=0.8, val_rate=0.1, test_rate=0.1)
```

代码可以自动划分各种格式的图片及标签文件，且无论图片及标签数量是否对应，均会对应移动到相同的文件夹下，同时给出出现差异的图片或标签文件名，方便小白快速查找原因。划分完成之后数据集的准备工作就好了，划分之后的文件夹如下图。

![](https://i-blog.csdnimg.cn/direct/9392ca043f4948bb89c9166a61af5dc8.png)​

3. 训练模型
-------

需要下载源码，这里选择的是 yolov8 v8.2.0 版本的，本文演示使用的安全帽检测数据集点击，注意，此数据集的两个标签分别为'person','hat' 。[网盘下载](https://pan.quark.cn/s/2f5a0ce67843 "网盘下载")。

不会下载源码的可以看我的这篇博客 [查看文章](https://blog.csdn.net/qq_67105081/article/details/137519207 "查看文章") ，也可以点击链接获取点击 [夸克网盘下载](https://pan.quark.cn/s/f26376f0de96 "夸克网盘下载") ，这里使用 v8.2.0 版本演示，我上传了多版本可以全部转存避免后续找不到，cat 图片一并上传，下载 8.3.0 以下版本，（压缩包内附带 yolov8n.pt、yolov8s.pt 和 yolov8m.pt 预训练权重，链接资源失效请评论区反馈，看到会补，或者至公众号下载）可以下载下图所示几个预训练权重文件，常规使用 yolov8n.pt 即可。

有了源码之后需要修改里面的参数，导入自己的数据集。

### 3.1 创建 data.yaml

在 yolov8 根目录下（也就是本文所用的 ultralytics-8.2.0 目录下）创建一个新的 data.yaml 文件，也可以是其他名字的例如 hat.yaml 文件，文件名可以变但是后缀需要为. yaml，内容如下，test 可有可无

```
train: f:/VOCdevkit/train/images  # train images (relative to 'path') 128 images
val: f:/VOCdevkit/val/images  # val images (relative to 'path') 128 images
test: f:/VOCdevkit/test/images
 
nc: 2
 
# Classes
names: ['hat','nohat']
```

其他路径和类别自己替换，需要和上面数据集转换那里类别顺序一致。

### 3.2 训练模型

这是使用官方提供的预训练权重进行训练，使用 yolov8n.pt，也可以使用 yolov8s.pt，模型大小 n<s<m<l<x，训练时长成倍增加。

![](https://i-blog.csdnimg.cn/blog_migrate/5662405756326bfc3fe327409beea211.png)​

下载完成之后放入 ultralytics-8.2.0 根目录中，创建一个 yolov8_train.py 文件，内容如下，关闭 amp 训练。没有 GPU 需要改 device="cpu" 。

```
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
  model = YOLO('ultralytics/cfg/models/v8/yolov8n.yaml')
  model.load('yolov8n.pt')  #注释则不加载
  results = model.train(
    data='data.yaml',  #数据集配置文件的路径
    epochs=200,  #训练轮次总数
    batch=16,  #批量大小，即单次输入多少图片训练
    imgsz=640,  #训练图像尺寸
    workers=8,  #加载数据的工作线程数
    device= 0,  #指定训练的计算设备，无nvidia显卡则改为 'cpu'
    optimizer='SGD',  #训练使用优化器，可选 auto,SGD,Adam,AdamW 等
    amp= True,  #True 或者 False, 解释为：自动混合精度(AMP) 训练
    cache=False  # True 在内存中缓存数据集图像，服务器推荐开启
)
```

这里用哪个模型对应哪个 yaml，如果使用 yolov8s.pt 则对应 yolov8s.yaml，

epochs 是训练轮数，可以由少变多看训练效果，workers 和 batch 根据电脑性能来，如果运行不起来则相应降低，最好为 2 的 n 次方。

也可以使用命令行执行训练

```
yolo task=detect mode=train model=yolov8n.yaml pretrained=yolov8n.pt data=data.yaml epochs=200 imgsz=640 device=0 workers=8 batch=64 amp=False optimizer='SGD' cache=False


```

训练过程如图，耐心等待训练完成即可，训练完成后会生成. pt 权重文件，可以用来验证训练效果。

![](https://i-blog.csdnimg.cn/blog_migrate/2185126cbfaa8b4e518f69fd19e993d8.png)​

4. 模型测试
-------

找到之前训练的结果保存路径，创建一个 yolov8_predict.py 文件，内容如下

```
from ultralytics import YOLO
# 加载训练好的模型，改为自己的路径
model = YOLO('runs/detect/train/weights/best.pt')
# 修改为自己的图像或者文件夹的路径
source = 'test1.jpg' #修改为自己的图片路径及文件名
# 运行推理，并附加参数
model.predict(source, save=True)
```

运行后就会得到预测模型结果。

![](https://i-blog.csdnimg.cn/blog_migrate/6e1d76fd59c693167c66afea23b9743d.png)​

或者使用命令行指令进行预测，权重和图片路径自己修改。

```
yolo predict model=runs/detect/train/weights/best.pt source='test1.jpg'

```

可以打开对应路径下查看预测的图片效果，模型就训练好啦~

测试集上推理模型精度代码如下，可新增 v8_val.py，输入下方代码，更改模型路径及数据集路径即可。

```
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
    model = YOLO('runs/detect/train/weights/best.pt')  #修改为自己训练的模型路径
    model.val(data='hat.yaml',  #修改为自己的数据集yaml文件
              split='test',
              imgsz=640,
              batch=16,
              iou=0.6,  #阈值可以改，mAP50为0.5的情况下
              conf=0.001,
              workers=8,
              )
```

 运行后即出现测试集上各类别的精度及总体精度。

5. 可视化界面
--------

很多同学的需求是制作出一个可视化界面 ui 作为系统来展示预测的效果，这里我分享了两个简单的图片预测的界面，导入模型权重文件和图片就可以进行预测并展示，pyqt5 写的可以参考这篇文章 [点击这里](https://blog.csdn.net/qq_67105081/article/details/147745193?spm=1001.2014.3001.5501 "点击这里") ，界面如下，YOLOv11 和 YOLOv8 是通用的。

![](https://i-blog.csdnimg.cn/direct/5474e8f4929e4f2688bdc3e27bc6c867.png)​

![](https://i-blog.csdnimg.cn/direct/9087bf86903f4f8b83be3214637b868c.png)

pyside6 可以参考 [这篇文章](https://blog.csdn.net/qq_67105081/article/details/144974796?spm=1001.2014.3001.5502 " 这篇文章") 和 [这篇文章](https://blog.csdn.net/qq_67105081/article/details/146458458?spm=1001.2014.3001.5501 "这篇文章")，效果如下：

![](https://i-blog.csdnimg.cn/direct/7522c36c7df345ecbf2d1fd55b1fd943.png)​

![](https://i-blog.csdnimg.cn/direct/0c6deaff2c9444fc9f71a42c367ed109.png)

免费的功能较为简单，只有图片检测显示。

写了一个进阶版的程序，可以对图片，视频和本地及云摄像头进行检测并展示，pyside6 和 pyqt5 界面如下。感兴趣可以通过公众号获取，需要定制系统也可以联系我。

![](https://i-blog.csdnimg.cn/direct/bba571091a344286bab7eba2af81a8b8.png)​

![](https://i-blog.csdnimg.cn/direct/edf2da9af685490d8de69c241bc0d5dd.png)​

![](https://i-blog.csdnimg.cn/direct/073a0e341d174758840f1c815152a797.gif)​

![](https://i-blog.csdnimg.cn/direct/c7f59cb2a1c643afb89f3301c18d172a.png)​

![](https://i-blog.csdnimg.cn/direct/089375017432475e8ab66c2888ca0c08.png)

遇到报错可以打开评论区交流。  关注微信公众号 快速联系我~