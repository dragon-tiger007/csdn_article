> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/ai12345678900008/article/details/152512081?spm=1001.2014.3001.5506)

引言
--

DINOv3 的模型由主干网络和特定任务头组成，主干网络权重易于获得，可从 Huggingface 或其镜像下载，但特定任务头不易获得。

特定任务头权重专门针对实际应用场景，一般是要留给使用者做二次训练的，官方提供的权重数量极其有限且不一定能够满足用户的需求。

DINOv3 的主干网络（backbone）只能用于图像的[特征提取](https://so.csdn.net/so/search?q=%E7%89%B9%E5%BE%81%E6%8F%90%E5%8F%96&spm=1001.2101.3001.7020)，不能直接用于图像分类，具体实践中，要冻结主干网络的权重，训练任务头，以实现特定的场景分类任务，这样的方法训练量小，且不影响模型的基本能力。

本文采用了 PyTorch Image Models（timm）库训练，这种方法与 DINOv3 原生类库的区别在于通用性，timm 支持包括 DINOv3 在内的大部分主流模型，使用 timm，学会一种模型的训练方法，对于其他模型的操作都是类似的。timm 还有一个好处是可以自动处理图片的尺寸，另外在 dinov3 代码库的最近一次更新中（2025-09-17），明确说明了 timm 对 DINOv3 的支持。

本例中，我们以只有 80 多 M 大小的 vit_small_patch16_dinov3.lvd1689m 为主干网络，使用包含猫、狗、马、鹿等共 10 个类别的 CIFAR-10 数据集训练一个图像分类任务头。这样模型小、数据集也不大，需要的资源也很小（CPU 或具有 1G 内存的 GPU），很适合入门使用。

一、加载模型
------

```
def load_model():
    # 加载 timm 中的 DINOv3（或对应 ViT）预训练骨干网络，设置 num_classes=0 表示只保留特征提取部分
    model = timm.create_model(
        'timm/vit_small_patch16_dinov3.lvd1689m',  # timm 模型名称（可替换为你需要的预训练模型）
        pretrained=True,  # 加载预训练权重
        num_classes=0)  # 不创建分类头，仅输出特征向量
    model.eval()  # 将模型置于评估模式（禁用训练时特定行为，例如 BatchNorm 的训练统计）
    # 冻结骨干网络的所有参数，避免在训练分类头时更新骨干的权重
    for param in model.parameters():
        param.requires_grad = False  # 将 requires_grad 设为 False，减小计算与显存开销
    print("模型装载完成")  # 提示信息：骨干网络已加载并冻结
    return model  # 返回只包含特征提取部分的骨干模型
```

二、定义并组合分类头
----------

```
def def_model(model):
    # 获取骨干网络输出的特征维度（num_features 在 timm 模型中表示特征向量的通道数）
    feature_dim = model.num_features  # 例如可能是 384 / 768 / 1024 等，取决于模型结构
    # 假设任务类别数为 10（CIFAR-10 示例），可根据实际数据集调整
    num_classes = 10
    # 定义分类头（一个简单的两层 MLP）：feature_dim -> 256 -> num_classes
    classifier = nn.Sequential(
        nn.Linear(feature_dim, 256),  # 全连接层：将特征降维到 256
        nn.ReLU(inplace=True),  # 非线性激活：ReLU（inplace=True 可减少内存占用）
        nn.Dropout(p=0.1),  # Dropout：0.1 的丢弃率，缓解过拟合
        nn.Linear(256, num_classes)  # 输出层：映射到最终类别数
    )
    # 定义一个包含 backbone 与 head 的完整模型包装类，方便直接前向传入图片进行训练/推理
    class CustomDINOv3(nn.Module):
        def __init__(self, backbone, head):
            super().__init__()
            self.backbone = backbone  # 骨干特征提取器
            self.head = head  # 分类头
 
        def forward(self, x):
            features = self.backbone(x)  # 通过骨干得到特征向量，形状 [batch_size, feature_dim]
            output = self.head(features)  # 将特征传入分类头得到 logits（未归一化分数）
            return output  # 返回 logits，用于后续计算 loss / 预测
 
    custom_model = CustomDINOv3(model, classifier)  # 实例化包装模型
    print("定义训练模型完成")  # 提示信息：完整训练模型（骨干+头）已生成
    return custom_model, num_classes, feature_dim  # 返回自定义模型、类别数与特征维度
```

三、加载数据集
-------

```
def load_mydataset(model):
    # 获取模型对应的数据预处理配置（例如 input_size、mean、std、interpolation 等）
    data_config = timm.data.resolve_model_data_config(model)
 
    # 为训练集创建 transform（包含数据增强：随机裁剪、翻转、颜色扰动等）
    train_transforms = timm.data.create_transform(
        **data_config, is_training=True)
 
    # 验证/测试使用的 transform（与训练不同：不包含随机数据增强，保证确定性）
    val_transforms = timm.data.create_transform(
        **data_config, is_training=False)
 
    # 自定义数据集根目录（假设结构为 ./mydataset/train/<class>/*, ./mydataset/val/<class>/*, ./mydataset/test/<class>/*）
    data_dir = 'H:/HTempWK/temp/Dinov3/mydataset'
 
    # 使用 ImageFolder 加载训练集：ImageFolder 要求每个类别在子文件夹下
    train_dataset = datasets.ImageFolder(
        root=os.path.join(data_dir, 'train'),  # 训练集文件夹路径
        transform=train_transforms  # 对训练集应用的 transform
    )
 
    # 使用 ImageFolder 加载验证集（不使用随机增强）
    val_dataset = datasets.ImageFolder(
        root=os.path.join(data_dir, 'val'),  # 验证集文件夹路径
        transform=val_transforms  # 对验证集应用的 transform（确定性）
    )
 
    # 使用 ImageFolder 加载测试集（同验证集的预处理）
    test_dataset = datasets.ImageFolder(
        root=os.path.join(data_dir, 'test'),  # 测试集文件夹路径
        transform=val_transforms  # 对测试集应用的 transform（确定性）
    )
 
    # 由于数据集较小，设置较小的 batch_size；num_workers=1 在 Windows 下较安全（可根据环境调大加速）
    train_loader = DataLoader(
        train_dataset, batch_size=2, shuffle=True, num_workers=1)  # 训练时 shuffle=True
    val_loader = DataLoader(
        val_dataset, batch_size=2, shuffle=False, num_workers=1)  # 验证/测试不打乱顺序
    test_loader = DataLoader(
        test_dataset, batch_size=2, shuffle=False, num_workers=1)  # 测试时同上
 
    # 打印数据集的一些基本信息，方便调试
    print(f"训练集类别: {train_dataset.classes}")  # 列出训练集的类别名称（ImageFolder 根据子文件夹名生成）
    print(f"训练集样本数: {len(train_dataset)}")  # 训练集样本数
    print(f"验证集样本数: {len(val_dataset)}")  # 验证集样本数
    print(f"测试集样本数: {len(test_dataset)}")  # 测试集样本数
    print("装载数据集完成")
 
    # 返回训练/验证/测试 的 DataLoader，以及用于创建 transform 的 data_config（后续保存模型或推理时可能用到）
    return train_loader, val_loader, test_loader, data_config
```

四、进行训练
------

```
def train_model(custom_model, train_loader, val_loader):
    custom_model.to(device)
    criterion = nn.CrossEntropyLoss()
    # 只对分类头的参数进行优化
    optimizer = optim.Adam(custom_model.head.parameters(), lr=0.001)
    num_epochs = 5
 
    best_val_acc = 0.0
 
    for epoch in range(num_epochs):
        # 训练阶段
        custom_model.train()
        running_loss = 0.0
        train_correct = 0
        train_total = 0
 
        for images, labels in train_loader:
            images, labels = images.to(device), labels.to(device)
            optimizer.zero_grad()
            outputs = custom_model(images)
            loss = criterion(outputs, labels)
            loss.backward()
            optimizer.step()
 
            running_loss += loss.item()
            _, predicted = torch.max(outputs.data, 1)
            train_total += labels.size(0)
            train_correct += (predicted == labels).sum().item()
 
        train_acc = 100 * train_correct / train_total
 
        # 验证阶段
        val_acc = evaluate_model(custom_model, val_loader, "验证集")
 
        print(f'Epoch [{epoch+1}/{num_epochs}], '
              f'Loss: {running_loss/len(train_loader):.4f}, '
              f'训练准确率: {train_acc:.2f}%, '
              f'验证准确率: {val_acc:.2f}%')
 
        # 保存最佳模型
        if val_acc > best_val_acc:
            best_val_acc = val_acc
            save_model(custom_model, 3, custom_model.backbone.num_features,
                       {'input_size': data_config['input_size']})
            print(f"保存最佳模型，验证准确率: {val_acc:.2f}%")
 
    print('模型训练完成')
```

五、测试集结果输出
---------

```
def evaluate_model(custom_model, data_loader, dataset_):
    correct = 0
    total = 0
    custom_model.eval()
    with torch.no_grad():
        for images, labels in data_loader:
            images, labels = images.to(device), labels.to(device)
            outputs = custom_model(images)
            _, predicted = torch.max(outputs.data, 1)
            total += labels.size(0)
            correct += (predicted == labels).sum().item()
 
    accuracy = 100 * correct / total
    print(f'{dataset_name}准确率: {accuracy:.2f}%')
    return accuracy
```

六、完整的训练脚本
---------

```
# 导入必要的库
import torch  # PyTorch：张量与自动微分、深度学习核心库
import torch.nn as nn  # 神经网络模块（包含常见层、容器、损失等）
import timm  # timm：模型库，用于快速加载预训练视觉模型
from torchvision import datasets  # torchvision 的数据集模块（这里用 CIFAR-10 演示）
from torch.utils.data import DataLoader  # DataLoader：用于构建可迭代的数据加载器
import torch.optim as optim  # 优化器（如 Adam、SGD 等）
import os  # 操作系统路径相关函数（用于拼接目录等）
import timm  # timm 库，用于获取模型对应的数据配置和 create_transform
from torchvision import datasets  # torchvision 的数据集工具（ImageFolder 等）
from torch.utils.data import DataLoader  # PyTorch 的 DataLoader，用于按批次加载数据
 
# 选择运行设备：优先使用 GPU（cuda），否则退回到 CPU
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")  # device 用于将张量/模型移动到对应设备
 
 
def load_model():
    # 加载 timm 中的 DINOv3（或对应 ViT）预训练骨干网络，设置 num_classes=0 表示只保留特征提取部分
    model = timm.create_model(
        'timm/vit_small_patch16_dinov3.lvd1689m',  # timm 模型名称（可替换为你需要的预训练模型）
        pretrained=True,  # 加载预训练权重
        num_classes=0)  # 不创建分类头，仅输出特征向量
    model.eval()  # 将模型置于评估模式（禁用训练时特定行为，例如 BatchNorm 的训练统计）
    # 冻结骨干网络的所有参数，避免在训练分类头时更新骨干的权重
    for param in model.parameters():
        param.requires_grad = False  # 将 requires_grad 设为 False，减小计算与显存开销
    print("模型装载完成")  # 提示信息：骨干网络已加载并冻结
    return model  # 返回只包含特征提取部分的骨干模型
 
 
def def_model(model):
    # 获取骨干网络输出的特征维度（num_features 在 timm 模型中表示特征向量的通道数）
    feature_dim = model.num_features  # 例如可能是 384 / 768 / 1024 等，取决于模型结构
    # 假设任务类别数为 10（CIFAR-10 示例），可根据实际数据集调整
    num_classes = 10
    # 定义分类头（一个简单的两层 MLP）：feature_dim -> 256 -> num_classes
    classifier = nn.Sequential(
        nn.Linear(feature_dim, 256),  # 全连接层：将特征降维到 256
        nn.ReLU(inplace=True),  # 非线性激活：ReLU（inplace=True 可减少内存占用）
        nn.Dropout(p=0.1),  # Dropout：0.1 的丢弃率，缓解过拟合
        nn.Linear(256, num_classes)  # 输出层：映射到最终类别数
    )
    # 定义一个包含 backbone 与 head 的完整模型包装类，方便直接前向传入图片进行训练/推理
    class CustomDINOv3(nn.Module):
        def __init__(self, backbone, head):
            super().__init__()
            self.backbone = backbone  # 骨干特征提取器
            self.head = head  # 分类头
 
        def forward(self, x):
            features = self.backbone(x)  # 通过骨干得到特征向量，形状 [batch_size, feature_dim]
            output = self.head(features)  # 将特征传入分类头得到 logits（未归一化分数）
            return output  # 返回 logits，用于后续计算 loss / 预测
 
    custom_model = CustomDINOv3(model, classifier)  # 实例化包装模型
    print("定义训练模型完成")  # 提示信息：完整训练模型（骨干+头）已生成
    return custom_model, num_classes, feature_dim  # 返回自定义模型、类别数与特征维度
 
 
def load_dataset(model):
    # 获取与模型对应的数据预处理配置（例如输入尺寸、mean/std、是否需要中心裁剪等）
    data_config = timm.data.resolve_model_data_config(model)  # 返回 dict，包含 'input_size','interpolation','mean','std' 等
    # 根据 data_config 创建训练时的数据变换（包括 resize / crop / normalize / augmentation 等）
    transforms = timm.data.create_transform(
        **data_config, is_training=True)  # is_training=True：启用训练时的数据增强（随机裁剪/翻转等）
    # 加载 CIFAR-10 训练集（示例），如果本地没有会自动下载到 ./data
    train_dataset = datasets.CIFAR10(
        root='./data', train=True, download=True, transform=transforms)  # train=True 表示加载训练集
    # 加载 CIFAR-10 测试集（示例）
    test_dataset = datasets.CIFAR10(
        root='./data', train=False, download=True, transform=transforms)  # train=False 表示加载测试集
    # 创建数据加载器（DataLoader）：用于按批次读取数据并进行多线程加速
    train_loader = DataLoader(
        train_dataset, batch_size=32, shuffle=True, num_workers=2)  # 训练时使用 shuffle=True
    test_loader = DataLoader(test_dataset, batch_size=32,
                             shuffle=False, num_workers=2)  # 测试时不打乱顺序
    print("装载数据集完成")  # 提示信息：数据加载器已准备好
    return train_loader, test_loader, data_config  # 返回训练/测试加载器与数据配置
 
 
def load_mydataset(model):
    # 获取模型对应的数据预处理配置（例如 input_size、mean、std、interpolation 等）
    data_config = timm.data.resolve_model_data_config(model)
 
    # 为训练集创建 transform（包含数据增强：随机裁剪、翻转、颜色扰动等）
    train_transforms = timm.data.create_transform(
        **data_config, is_training=True)
 
    # 验证/测试使用的 transform（与训练不同：不包含随机数据增强，保证确定性）
    val_transforms = timm.data.create_transform(
        **data_config, is_training=False)
 
    # 自定义数据集根目录（假设结构为 ./mydataset/train/<class>/*, ./mydataset/val/<class>/*, ./mydataset/test/<class>/*）
    data_dir = 'H:/HTempWK/temp/Dinov3/mydataset'
 
    # 使用 ImageFolder 加载训练集：ImageFolder 要求每个类别在子文件夹下
    train_dataset = datasets.ImageFolder(
        root=os.path.join(data_dir, 'train'),  # 训练集文件夹路径
        transform=train_transforms  # 对训练集应用的 transform
    )
 
    # 使用 ImageFolder 加载验证集（不使用随机增强）
    val_dataset = datasets.ImageFolder(
        root=os.path.join(data_dir, 'val'),  # 验证集文件夹路径
        transform=val_transforms  # 对验证集应用的 transform（确定性）
    )
 
    # 使用 ImageFolder 加载测试集（同验证集的预处理）
    test_dataset = datasets.ImageFolder(
        root=os.path.join(data_dir, 'test'),  # 测试集文件夹路径
        transform=val_transforms  # 对测试集应用的 transform（确定性）
    )
 
    # 由于数据集较小，设置较小的 batch_size；num_workers=1 在 Windows 下较安全（可根据环境调大加速）
    train_loader = DataLoader(
        train_dataset, batch_size=2, shuffle=True, num_workers=1)  # 训练时 shuffle=True
    val_loader = DataLoader(
        val_dataset, batch_size=2, shuffle=False, num_workers=1)  # 验证/测试不打乱顺序
    test_loader = DataLoader(
        test_dataset, batch_size=2, shuffle=False, num_workers=1)  # 测试时同上
 
    # 打印数据集的一些基本信息，方便调试
    print(f"训练集类别: {train_dataset.classes}")  # 列出训练集的类别名称（ImageFolder 根据子文件夹名生成）
    print(f"训练集样本数: {len(train_dataset)}")  # 训练集样本数
    print(f"验证集样本数: {len(val_dataset)}")  # 验证集样本数
    print(f"测试集样本数: {len(test_dataset)}")  # 测试集样本数
    print("装载数据集完成")
 
    # 返回训练/验证/测试 的 DataLoader，以及用于创建 transform 的 data_config（后续保存模型或推理时可能用到）
    return train_loader, val_loader, test_loader, data_config
 
 
def train_model(custom_model, train_loader, val_loader):
    custom_model.to(device)
    criterion = nn.CrossEntropyLoss()
    # 只对分类头的参数进行优化
    optimizer = optim.Adam(custom_model.head.parameters(), lr=0.001)
    num_epochs = 5
 
    best_val_acc = 0.0
 
    for epoch in range(num_epochs):
        # 训练阶段
        custom_model.train()
        running_loss = 0.0
        train_correct = 0
        train_total = 0
 
        for images, labels in train_loader:
            images, labels = images.to(device), labels.to(device)
            optimizer.zero_grad()
            outputs = custom_model(images)
            loss = criterion(outputs, labels)
            loss.backward()
            optimizer.step()
 
            running_loss += loss.item()
            _, predicted = torch.max(outputs.data, 1)
            train_total += labels.size(0)
            train_correct += (predicted == labels).sum().item()
 
        train_acc = 100 * train_correct / train_total
 
        # 验证阶段
        val_acc = evaluate_model(custom_model, val_loader, "验证集")
 
        print(f'Epoch [{epoch+1}/{num_epochs}], '
              f'Loss: {running_loss/len(train_loader):.4f}, '
              f'训练准确率: {train_acc:.2f}%, '
              f'验证准确率: {val_acc:.2f}%')
 
        # 保存最佳模型
        if val_acc > best_val_acc:
            best_val_acc = val_acc
            save_model(custom_model, 3, custom_model.backbone.num_features,
                       {'input_size': data_config['input_size']})
            print(f"保存最佳模型，验证准确率: {val_acc:.2f}%")
 
    print('模型训练完成')
    
    
def save_model(custom_model, num_classes, feature_dim, data_config):
    # 只保存分类头的权重
    torch.save({
        'classifier_state_dict': custom_model.head.state_dict(),
        'num_classes': num_classes,
        'feature_dim': feature_dim,
        'training_config': {
            'model_name': 'vit_small_patch16_dinov3.lvd1689m',
            'input_size': data_config['input_size']
        }
    }, 'dino_classifier_head.pth')
    print("模型权重保存完成")
    # 如果想要保存全部的模型参数，请取消下面代码的注释
    # torch.save(custom_model.state_dict(), 'dino_full.pth')
 
def evaluate_model(custom_model, data_loader, dataset_):
    correct = 0
    total = 0
    custom_model.eval()
    with torch.no_grad():
        for images, labels in data_loader:
            images, labels = images.to(device), labels.to(device)
            outputs = custom_model(images)
            _, predicted = torch.max(outputs.data, 1)
            total += labels.size(0)
            correct += (predicted == labels).sum().item()
 
    accuracy = 100 * correct / total
    print(f'{dataset_name}准确率: {accuracy:.2f}%')
    return accuracy
 
 
# 脚本主入口：当作为脚本直接运行时执行下面代码；作为模块导入时不会执行
if __name__ == '__main__':
    model = load_model()  # 加载模型与数据预处理配置
    custom_model, num_classes, feature_dim = def_model(model)  # 2) 定义并组合分类头
    train_loader, val_loader, test_loader, data_config = load_mydataset(model) # 加载数据集
    
    train_model(custom_model, train_loader, val_loader)# 进行训练
 
    # 最终在测试集上评估
    print("\n最终测试结果:")
    evaluate_model(custom_model, test_loader, "测试集")# 测试集输出的结果
```

七、训练结果展示
--------

```
 & E:/software/miniconda/envs/dinov3/python.exe h:/HTempWK/temp/Dinov3/demo1.py
模型装载完成
定义训练模型完成
训练集类别: ['class1', 'class2', 'class3']
训练集样本数: 6
验证集样本数: 6
测试集样本数: 6
装载数据集完成
验证集准确率: 50.00%
Epoch [1/5], Loss: 2.3281, 训练准确率: 16.67%, 验证准确率: 50.00%
模型权重保存完成
保存最佳模型，验证准确率: 50.00%
验证集准确率: 33.33%
Epoch [2/5], Loss: 2.0015, 训练准确率: 83.33%, 验证准确率: 33.33%
验证集准确率: 33.33%
Epoch [3/5], Loss: 1.8123, 训练准确率: 83.33%, 验证准确率: 33.33%
验证集准确率: 33.33%
Epoch [4/5], Loss: 1.5018, 训练准确率: 100.00%, 验证准确率: 33.33%
验证集准确率: 33.33%
Epoch [5/5], Loss: 1.1855, 训练准确率: 100.00%, 验证准确率: 33.33%
模型训练完成
 
最终测试结果:
测试集准确率: 50.00%
(dinov3) PS H:\HTempWK\temp> 
```