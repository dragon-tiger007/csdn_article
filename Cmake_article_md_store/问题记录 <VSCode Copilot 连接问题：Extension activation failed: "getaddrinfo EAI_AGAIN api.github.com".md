> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/o2iginal/p/17800963.html)

问题描述
----

VSCode 使用 Copilot 时遇到如下问题：

```
Extension activation failed: "getaddrinfo EAI_AGAIN api.github.com"


``` 

解决方式
----

笔者尝试了修改 hosts、代理、重装插件等方法，但没有起效。

下面的方法解决了问题（在 VSCode 中设置 proxy）

1.  打开代理，查看代理 http 地址，复制；
    
2.  打开 VSCode，打开 settings，搜索‘proxy’，如下图：  
    [![](https://img2023.cnblogs.com/blog/3165412/202310/3165412-20231031181918115-55339981.png)](https://img2023.cnblogs.com/blog/3165412/202310/3165412-20231031181918115-55339981.png)
    
3.  将下图位置设置 http 代理  
    [![](https://img2023.cnblogs.com/blog/3165412/202310/3165412-20231031181941565-76502558.png)](https://img2023.cnblogs.com/blog/3165412/202310/3165412-20231031181941565-76502558.png)
    

至此，再次测试 Copilot 插件，已可正常连接。

参考
--

*   [https://blog.csdn.net/gongfpp/article/details/128876310](https://blog.csdn.net/gongfpp/article/details/128876310)

*   [问题描述](#问题描述)
*   [解决方式](#解决方式)
*   [参考](#参考)

  

__EOF__

[![](https://pic.cnblogs.com/avatar/3165412/20230407085237.png)](https://pic.cnblogs.com/avatar/3165412/20230407085237.png) *   **本文作者：** [O2iginal](https://www.cnblogs.com/o2iginal)
*   **本文链接：** [https://www.cnblogs.com/o2iginal/p/17800963.html](https://www.cnblogs.com/o2iginal/p/17800963.html)
*   **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://msg.cnblogs.com/msg/send/o2iginal)我。
*   **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-nd/4.0/ "BY-NC-SA") 许可协议。转载请注明出处！
*   **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。