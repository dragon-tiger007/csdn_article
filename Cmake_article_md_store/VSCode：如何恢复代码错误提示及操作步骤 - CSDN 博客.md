> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_43762434/article/details/137014829?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522ccf0c8656d849d16da059ff690246ba4%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=ccf0c8656d849d16da059ff690246ba4&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-137014829-null-null.142^v101^control&utm_term=vscode%E7%A6%81%E7%94%A8%E4%BA%86%E6%B3%A2%E5%BD%A2%E6%9B%B2%E7%BA%BF&spm=1018.2226.3001.4187)

#### vscode 恢复错误提示

*   [一、产生原因](#_1)
*   [二、还原方法](#_6)
*   *   [1. 设置工作区](#1_7)
    *   [2.crtl+shift+p，enable error squiggles](#2crtlshiftpenable_error_squiggles_16)

一、产生原因
------

今天敲代码有个报错，不小心点了快速修复 + 禁用错误波形曲线，结果就没有错误提示了，查找相关资料后总结两种解决方法如下。  
![](https://i-blog.csdnimg.cn/blog_migrate/f6147ba5a921ee50d10dd1b335bede2a.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/962e2ab8733faff0effe5e650891eb98.png)

二、还原方法
------

### 1. 设置工作区

*   首先找到左下角的设置  
    ![](https://i-blog.csdnimg.cn/blog_migrate/1a47a80221b54c955cb554efd1980148.png)
    
*   选择工作区，查找 c_cpp.errorSquiggles，选择 Enabled，保存重启 vscode。  
    ![](https://i-blog.csdnimg.cn/blog_migrate/e95abfab3485c5022139196e7915651c.png)
    

### 2.crtl+shift+p，[enable](https://so.csdn.net/so/search?q=enable&spm=1001.2101.3001.7020) error squiggles

直接 crtl+shift+p 在顶部输入 enable error squiggles 回车即可。  
![](https://i-blog.csdnimg.cn/blog_migrate/3a582922979c18e3c65c20f691a5d4f9.png)