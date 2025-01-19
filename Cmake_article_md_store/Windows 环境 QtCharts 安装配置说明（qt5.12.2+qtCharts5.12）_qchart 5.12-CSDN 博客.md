> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/aoxuestudy/article/details/110132657)

### Windows 环境 QtCharts 安装配置说明（qt5.12.2+qtCharts5.12）

[qtCharts5.12 下载地址](https://github.com/qt/qtcharts/tree/5.12)  
我这里选择 5.12，你也可以选择你需要的版本  
![](https://i-blog.csdnimg.cn/blog_migrate/5cabe3a6302acff12090d1b5e7b874a0.png#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/22d1e39f83a30752528d0d9265ded0c2.png#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/c605ea7b1d97e0c9e5082f817cee9c12.png#pic_center)  
分别下了 QtCharts 5.7 、5.12 和 5.12.2 这三个版本，使用 qt5.12.2 来编译发现都报错

### 解决方法

1.  维护安装  
    （1）双击运行 MaintenanceTool.exe  
    ![](https://i-blog.csdnimg.cn/blog_migrate/110cc45d20448244b68f6b9340eed37c.png#pic_center)  
    （2）断网运行，免得需要输入登录 QT 账号  
    ![](https://i-blog.csdnimg.cn/blog_migrate/569caaf2900eb61d1213a73cbe48602a.png#pic_center)  
    （3）选择 **skip** 跳过  
    ![](https://i-blog.csdnimg.cn/blog_migrate/923a069ed5e38b64a7038d521d876cff.png#pic_center)  
    （4）点击**设置**  
    ![](https://i-blog.csdnimg.cn/blog_migrate/ce4b879d4f44a8954a86890978bd1b2b.png#pic_center)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/a5a28fb98f5e4a945be849ba0f8ab4c9.png#pic_center)  
    临时库的值  
    http://mirrors.ustc.edu.cn/qtproject/online/qtsdkrepository/windows_x86/root/qt/

（5）一定要勾选 QtCharts  
**注意**：如果没有发现 qtCharts，那就把 qt 整个卸载了，然后重装，重装的时候一定要勾选 Qt Charts  
![](https://i-blog.csdnimg.cn/blog_migrate/bf36bb832437aa2382ea73bf1a7d6357.png#pic_center)  
（5）点击**下一步**  
（6）安装完成后，不用做任何配置，工程自动识别 QtCharts