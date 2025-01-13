> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/2401_84670644/article/details/140532123)

当我们使用 pip 下载 python 的各种包时可能会遇到下载错误和下载过慢。

更改 [pip 源](https://so.csdn.net/so/search?q=pip%20%E6%BA%90&spm=1001.2101.3001.7020)可以帮助加快包的安装速度，特别是在某些国内网络环境下。

#### 1. 查看当前 pip 配置

首先，可以使用以下命令查看当前 pip 的配置信息：

```
pip config list


```

#### 2. 选择合适的源

```
清华大学 ：https://pypi.tuna.tsinghua.edu.cn/simple/
 
阿里云：http://mirrors.aliyun.com/pypi/simple/
 
中国科学技术大学 ：http://pypi.mirrors.ustc.edu.cn/simple/
 
华中科技大学：http://pypi.hustunique.com/
 
豆瓣源：http://pypi.douban.com/simple/
 
腾讯源：http://mirrors.cloud.tencent.com/pypi/simple
 
华为镜像源：https://repo.huaweicloud.com/repository/pypi/simple/
```

#### 3. 临时使用新源

可以使用以下命令临时切换到新的源（例如，切换到清华大学的源）：

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package


```

#### 4. 永久修改 pip 源，恢复原来的源

如果要永久修改 pip 的源，可以使用 `pip config` 命令。例如，将 pip 源配置为清华大学的源：

```
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple


```

这样就会将全局的 pip 源配置为清华大学的源。如果要恢复到默认的官方源，可以使用：

```
pip config unset global.index-url


```

#### 5. 验证更改

修改配置后，可以再次运行 `pip config list` 命令来验证是否已经成功更改了 pip 源。