> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.zhihu.com](https://www.zhihu.com/question/534628600) ![](https://pica.zhimg.com/v2-7b15a52f6718d393a357c29b87875a5e_l.jpg?source=1def8aca)雅乐 top

GitHub Copilot could not connect to server. Extension activation failed: "connect ECONNREFUSED 127.0.0.1:443"

最近 vscode 打开驾驶员的时候发现登不上了，自己看了下原来是需要配置 hosts 文件。

mac ：找到 访达 -> Shift + Command + G -> 调出对话框后输入 /etc/hosts -> 在当前文件夹用 vim 或其他方式添加

```
# vscode copilot config
140.82.112.5 github.com
140.82.112.5 api.github.com

```

windwos: Windows 文件夹 -> System32 文件夹 -> [drivers 文件夹](https://zhida.zhihu.com/search?content_id=486461345&content_type=Answer&match_order=1&q=drivers%E6%96%87%E4%BB%B6%E5%A4%B9&zhida_source=entity) -> etc 文件夹 -> hosts 文件 -> 在当前文件夹用 记事本 或其他方式添加

```
# vscode copilot config
140.82.112.5 github.com
140.82.112.5 api.github.com

```

最后重启 vscode 就可以啦~

如果不能编辑请检查当前操作权限，希望能帮到大家~！

![](https://pic1.zhimg.com/v2-e5127cda714bc87bb7ea0d2467d47740_l.jpg?source=1def8aca)FPXtheHun

不是科学上网的问题之前都可以连接的，就是因为 ip 变了连不上