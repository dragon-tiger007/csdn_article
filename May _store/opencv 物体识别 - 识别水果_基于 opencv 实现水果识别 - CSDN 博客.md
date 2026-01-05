> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_52095705/article/details/121568221?spm=1001.2014.3001.5506)

### 前言

玩一玩用 opencv 做一些简单的物体识别

1. 思路讲解
-------

我们基于简单的 opencv 的阈值分割，通过这个阈值分割，我们能把我们需要识别的物体在二值图里面变成白色，其余的变成黑色。然后对我们分割出来的物体部分提取轮廓，算出覆盖轮廓的最小矩形，然后画出这个矩形框，并且表上我们物体的名字。

2. 样本展示
-------

![](https://i-blog.csdnimg.cn/blog_migrate/a4ad72bc3335977509f856992775478d.jpeg#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/12ee055a6e5d8a45ce781ad5bff47443.jpeg#pic_center)  
笔者就以这两张图片为例子，提取这两张图片里面的橙子。按照我们上面的思路，我们需要的是把橙子这个部分分割出来变成变成二值图的白色部分，其他部分变成黑色。

3. 代码实现
-------

首先我们读入一张橙子的图片，因为 opencv 默认读入的图片是 bgr 的形式，我们用的是 hsv 的颜色阈值，因此我们要将图片转换到 hsv。然后经过一个中值滤波去除噪声，再经过一个开运算。

```
image=cv2.imread("c4.jpeg")
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
hsv = cv2.medianBlur(hsv, 5)
mask = cv2.inRange(hsv, (11, 43, 46), (25, 255, 255))
line = cv2.getStructuringElement(cv2.MORPH_RECT, (15, 15), (-1, -1))
mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, line)

```

其中 cv2.inRange 会将图片中 hsv 值在 (11, 43, 46), 和(25, 255, 255) 中间的值变成白色，不在中间的值变成黑色。  
最后咱们处理好的二值图如下：  
![](https://i-blog.csdnimg.cn/blog_migrate/e4a1ee05b5a90ba7c91d59085638670d.png#pic_center)

然后就是提取轮廓，求出最大轮廓，这个最大轮廓也就是我们的橙子

```
contours, hierarchy = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    index = -1
    max = 0
    font = cv2.FONT_HERSHEY_SIMPLEX
    for c in range(len(contours)):
        area = cv2.contourArea(contours[c])
        if area > max:
            max = area
            index = c

```

随后就是对这个橙子的轮廓求外接矩形，然后把这个矩形画出来，并且再对应的位置上标上 orange。

```
    if index >= 0:
        x, y, w, h = cv2.boundingRect(contours[index])
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
        cv2.putText(image,"orange",(x, y), font, 1.2, (0, 0, 255), 2)

```

最后的结果图片如下：  
![](https://i-blog.csdnimg.cn/blog_migrate/2b59edf044b621d75534723497e695e4.png#pic_center)

4. 总结
-----

完整的代码如下：

```
import cv2

def process(image):
    hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
    hsv = cv2.medianBlur(hsv, 5)
    mask = cv2.inRange(hsv, (11, 43, 46), (25, 255, 255))
    line = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5), (-1, -1))
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, line)
    cv2.imshow("mask",mask)

    # 轮廓提取, 发现最大轮廓
    contours, hierarchy = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    index = -1
    max = 0
    font = cv2.FONT_HERSHEY_SIMPLEX
    for c in range(len(contours)):
        area = cv2.contourArea(contours[c])
        if area > max:
            max = area
            index = c
    # 绘制
    if index >= 0:
        x, y, w, h = cv2.boundingRect(contours[index])
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
        cv2.putText(image,"orange",(x, y), font, 1.2, (0, 0, 255), 2)
    return image


image=cv2.imread("c1.jpeg")
result = process(image)
cv2.imshow("result", result)
cv2.waitKey(0)
cv2.destroyAllWindows()


```

上面的代码是在图片中寻找图片中最大面积的橘子，下面我们设定一个面积阈值, 只要大于这个阈值就是我们需要识别的目标，代码如下：

```
import cv2

def process(image):
	#面积阈值
    min_area=100
    hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
    hsv = cv2.medianBlur(hsv, 5)
    mask = cv2.inRange(hsv, (11, 43, 46), (25, 255, 255))
    line = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5), (-1, -1))
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, line)
    cv2.imshow("mask",mask)


    contours, hierarchy = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    font = cv2.FONT_HERSHEY_SIMPLEX
    for c in range(len(contours)):
        area = cv2.contourArea(contours[c])
        if area > min_area:
        # 绘制
            x, y, w, h = cv2.boundingRect(contours[c])
            cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
            cv2.putText(image,"orange",(x, y), font, 1.2, (0, 0, 255), 2)
    return image


image=cv2.imread("c1.jpeg")
result = process(image)
cv2.imshow("result", result)
cv2.waitKey(0)
cv2.destroyAllWindows()

```

运行结果如下：  
![](https://i-blog.csdnimg.cn/blog_migrate/e0ff31c7c2d223a7be90265107d84e55.png#pic_center)

5. 扩展
-----

当然只要能通过调试 cv2.inRange 的参数以提取好的二值图，不止是橙子，也可以用于其他物品的识别，也可以用于颜色识别。

6. 缺点
-----

当图片里有其他在参数范围内的比我们目标更大的物体的时候干扰就会很大，导致识别错误，扩展到其他物体上的时候还可能会遇到参数难调的情况，因此在识别场景比较复杂和识别类别比较多的时候还是建议用深度学习。