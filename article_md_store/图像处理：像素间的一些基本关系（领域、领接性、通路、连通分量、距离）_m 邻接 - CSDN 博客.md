> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_45965683/article/details/115459476)

#### [像素](https://so.csdn.net/so/search?q=%E5%83%8F%E7%B4%A0&spm=1001.2101.3001.7020)间的一些基本关系

*   [领域](#_1)
*   *   [相邻像素——4 邻域](#4_2)
    *   [相邻像素——D 邻域](#D_8)
    *   [相邻像素——8 邻域](#8_14)
*   [邻接性](#_19)
*   *   [像素间的邻接性——4 邻接](#4_24)
    *   [像素间的邻接性——8 邻接](#8_27)
    *   [像素间的邻接性——m 邻接](#m_31)
*   [通路](#_55)
*   [连通分量](#_95)
*   [距离](#_105)

领域
--

### 相邻像素——4 邻域

> *   4 邻域：像素 p(x,y) 的 4 邻域是： (x+1,y)；(x-1,y)；(x,y+1)；(x,y-1)
> *   用 N4（p）表示像素 p 的 4 邻域 ：

![](https://i-blog.csdnimg.cn/blog_migrate/053c0f040d8f43b6d7dfd0c7ec6364c3.png)

### 相邻像素——D 邻域

> *   D 邻域（ diagonal ）定义：像素 p(x,y) 的 D 邻域是：对角上的点 (x+1,y+1)；(x+1,y-1)；(x-1,y+1)；(x-1,y-1)
> *   用 ND（p）表示像素 p 的 D 邻域 :

![](https://i-blog.csdnimg.cn/blog_migrate/78c627169920495b44ec2fdea4c517f0.png)

### 相邻像素——8 邻域

*   8 邻域定义：像素 p(x,y) 的 8 邻域是：4 邻域的点 ＋ D 邻域的点
*   用 N8§ 表示像素 p 的 8 邻域。  
    -N8（p）= N4（p）+ ND（p）  
    ![](https://i-blog.csdnimg.cn/blog_migrate/c89fc555d17c352870c2076c4a7dca8d.png)

邻接性
---

> *   邻接性是描述区域和边界的重要概念
> *   两个像素邻接的两个必要条件是：  
>     **两个像素的位置是否相邻**  
>     **两个像素的灰度值是否满足特定的相似性准则（或者是否相等）**

### 像素间的邻接性——4 邻接

*   对于具有值 V 的像素 p 和 q，如果 q 在集合 N4（p）中，则称这两个像素是 4 邻接的。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/57162f0be0f0ef80eeb5ed8aeb6b59d0.png)

### 像素间的邻接性——8 邻接

对于具有值 V 的像素 p 和 q，如果 q 在集合 N8（p）中，则称这两个像素是 8 邻接的 。  
![](https://i-blog.csdnimg.cn/blog_migrate/546a06248fe9a0dbb1210c4f94510c70.png)

### 像素间的邻接性——m 邻接

对于具有值 V 的像素 p 和 q，如果:

*   q 在集合 N4(p）中，或
*   q 在集合 ND（p）中，并且 N4（p）与 N4(q) 的交集为空（没有值 V 的像素）。则称这两个像素是 m 邻接的，即 4 邻接和 D 邻接的混合连通  
    ![](https://i-blog.csdnimg.cn/blog_migrate/ecbe6f4e2520c26cd3b41db1c8ff353b.png)  
    **m 邻接可消除 8 邻接产生的二义性**  
    ![](https://i-blog.csdnimg.cn/blog_migrate/5f1ad2af74c626dda6a016edb4c6ea28.png)  
    在图 2 中，8 邻域中的中间的那个 1 可以有两条路到达右上角的那个 1，这就是所说的二义性，这个情况在边缘检测里面是很不希望的。  
    如图 3 所示，改成 m 邻域以后，中间的 1 像素和右上角的像素是 8 连通的却不是 m 连通的，这可以从 m 连通的定义得到，如果用 M 连通从中间的 1 到右上角的 1 就只有一条路。  
    **像素间的邻接性关系：**

<table><thead><tr><th>若 p 和 q 是 4 邻接，那么它们肯定是 8 邻接？</th><th>对</th></tr></thead><tbody><tr><td>若 p 和 q 是 8 邻接，那么它们肯定是 4 邻接？</td><td>错</td></tr><tr><td>若 p 和 q 是 4 邻接，那么它们肯定是 m 邻接？</td><td>对</td></tr><tr><td>若 p 和 q 是 m 邻接，那么它们肯定是 4 邻接？</td><td>错</td></tr><tr><td>若 p 和 q 是 8 邻接，那么它们肯定是 m 邻接？</td><td>错</td></tr><tr><td>若 p 和 q 是 m 邻接，那么它们肯定是 8 邻接？</td><td>错</td></tr></tbody></table>

例如：  
![](https://i-blog.csdnimg.cn/blog_migrate/ff1a362b02ca3a4111f9e0f8b26d333c.png)  
V={1}, 红圈所表示的两个像素，是 4 邻接？8 邻接？还是 m 邻接？  
答案：不是 4 邻接，是 8 邻接，是 m 邻接

通路
--

定义：一条从具有坐标 (x,y) 的像素 p, 到具有坐标 (s,t) 的像素 q 的通路。  
(x0,y0),(x1,y1),…,(xn,yn) 的不同像素的序列。其中，(x0,y0) = (x,y)，(xn,yn) = (s,t)，(xi,yi) 和 (xi-1,yi-1) 是邻接的，1 ≤ i ≤ n，n 是路径的长度。如果(x0,y0) = (xn,yn) , 则该通路是闭合通路  
可依据特定的邻接类型定义 4 通路、8 通路和 m 通路。  
例题：

1.  V={2,3,4}，计算 p 和 q 之间的 4 通路、8 通路和 m 通路的最短长度。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/efb693d076f2069a8b38f14354c24497.png)  
    1）最短 4 通路, v={2,3,4}  
    ![](https://i-blog.csdnimg.cn/blog_migrate/6d43fbdfbf6d571e515c2a0c92339117.png)  
    由图可知，从 p 到 q 是无法到达的，即没有 4 通路，也不存在最短 4 通路

2）最短 8 通路，v={2,3,4}  
![](https://i-blog.csdnimg.cn/blog_migrate/d478b1dd31f99db35ecc2efaed166ad6.png)  
最短 8 通路为 4。即记住只要满足 p 的周围 8 个值在 V 值内都可以走，最短距离优先考虑斜线。

3）最短 m 通路 ，V={2，3，4}  
![](https://i-blog.csdnimg.cn/blog_migrate/187a8722de29c61880f14e35eded1cf1.png)  
最短 m 通路为 5，即简单的说最短 m 通路是在最短 8 通路的基础上，优先考虑斜线且必须满足 N4（p）与 N4(q) 的交集为空（没有值 V 的像素）。

例 2：V={0,1}，计算 p 和 q 之间的最短 4 通路、8 通路和 m 通路。  
![](https://i-blog.csdnimg.cn/blog_migrate/da4daf5b9351a49bd9ba49e8d21450ad.png)  
1）最短 4 通路，v={0，1}  
![](https://i-blog.csdnimg.cn/blog_migrate/ddfc4d0f9e670c58661b8e1c00f675bc.png)  
p、q 间无 4 通路。也可通过 q 的 4 领域值有无值 v 的像素快速判断是否存在 4 通路  
2）最短 8 通路：  
![](https://i-blog.csdnimg.cn/blog_migrate/b37f5631759ee4eabb5fdbaf2b5d58d0.png)  
最短 8 通路长度为 4。

3）最短 m 通路  
![](https://i-blog.csdnimg.cn/blog_migrate/0ecc17a69021c1578a7bd95616ad78c3.png)  
最短 m 通路为 5

<table><thead><tr><th>若 p 和 q 之间存在 4 通路，则两者之间必存在 m 通路</th><th>对</th></tr></thead><tbody><tr><td>若 p 和 q 之间存在 m 通路，则两者之间必存在 4 通路</td><td>错</td></tr><tr><td>若 p 和 q 之间存在 m 通路，则两者之间必存在 8 通路</td><td>对</td></tr><tr><td>若 p 和 q 之间存在 8 通路，则两者之间必存在 m 通路</td><td>对</td></tr><tr><td>若 p 和 q 之间存在 8 通路，则两者之间必存在 4 通路</td><td>错</td></tr><tr><td>若 p 和 q 之间存在 4 通路，则两者之间必存在 8 通路</td><td>对</td></tr></tbody></table>

连通分量
----

令 S 是图像中的一个像素子集。如果在 S 中全部像素之间存在一个通路，则可以说 p 和 q 在 S 中是连通（connected）的。对于 S 中的任何像素 p，S 中连通到该像素的像素集称为 S 的连通分量。如果 S 仅有一个连通分量，则集合 S 称为连通集 (connected set)。

例：计算所给图中的连通域个数（分别用 4 连通和 8 连通求）。  
![](https://i-blog.csdnimg.cn/blog_migrate/d7335ef53322b6eb8103a4da86579f61.png)  
4 连通的连通域有 6 个，分别是：  
{A,B}、{C}、{D，E}、{F,G,H}、{I}、{J，K}  
8 连通的连通域有 2 个，分别是：  
{A,B,C,D,E}、{F,G,H,I,J,K}

距离
--

像素 p(x,y) 和 q(s,t) 间的欧式距离定义如下：  
![](https://i-blog.csdnimg.cn/blog_migrate/2ee10e819ecb15fe8b6dc7d887236d4b.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/ea61ccaeb4ec1a7da1483e595ebd32b7.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/2343e91de6f5afcc1e8eff2e6aeb7bd9.png)  
1）D4 距离举例  
具有与 (x,y) 距离小于等于某个值 r 的那些像素形成一个菱形

例如，与点 (x,y)（中心点 0）D4 距离小于等于 2 的像素，形成下边固定距离的轮廓  
具有 D4 = 1 的像素是 (x,y) 的 4 邻域  
![](https://i-blog.csdnimg.cn/blog_migrate/cbc5fb859fb39f4b3db499d19323eaf1.png)  
D8 距离举例  
对比 D4 距离，D8 距离具有与 (x,y) 距离小于等于某个值 r 的那些像素形成一个正方形  
例如，与点 (x,y)（中心点 0）D8 距离小于等于 2 的像素，形成下边固定距离的轮廓  
具有 D8 = 1 的像素是 (x,y) 的 8 邻域  
![](https://i-blog.csdnimg.cn/blog_migrate/1b82dde2fd73bb974eba566565af76bb.png)