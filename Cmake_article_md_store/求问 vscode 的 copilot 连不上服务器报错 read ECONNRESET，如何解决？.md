> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.zhihu.com](https://www.zhihu.com/question/518408113) ![](https://pic1.zhimg.com/v2-9b4c9211b62dd3b37751111117f60f01_l.jpg?source=1def8aca)无名

[host 文件](https://zhida.zhihu.com/search?content_id=542602776&content_type=Answer&match_order=1&q=host%E6%96%87%E4%BB%B6&zhida_source=entity)修改、vpn 全局都没成，最后用 vscode 的 proxy 设置搞成了，代理至自己的 vpn 端口就成功了

![](https://picx.zhimg.com/v2-cc37467c2650fb4f66a339780770536f_r.jpg?source=1def8aca)![](https://pica.zhimg.com/50/v2-2e7674057369406209ac813f0ce48ac4_720w.jpg?source=1def8aca)

goland 也出了同样的问题，也可以配置代理

![](https://picx.zhimg.com/v2-698a2eceb7b5d93cd02a584c5e230be6_r.jpg?source=1def8aca)

 ![](https://picx.zhimg.com/cf8d7cca1b2f6640f06bcec55931069e_l.jpg?source=1def8aca) Kendrick

出现「[ERROR] [default] [2022-05-24T07:02:58.242Z] [GitHub Copilot](https://zhida.zhihu.com/search?content_id=484791305&content_type=Answer&match_order=1&q=GitHub+Copilot&zhida_source=entity) could not connect to server. Extension activation failed: "connect ECONNREFUSED 127.0.0.1:443"」报错的应该是坐标广东的同学。

去查了下 [DNS](https://zhida.zhihu.com/search?content_id=484791305&content_type=Answer&match_order=1&q=DNS&zhida_source=entity) 的解析结果：

![](https://picx.zhimg.com/v2-c72f1dcb0b145eea2984fedac18dc2e4_r.jpg?source=1def8aca)

不知道广东的运营商抽啥风了，把 github 的地址给解析到了 127.0.0.1。目前看应该是只有广东是这样。

可以自己在命令行里面用 `nslookup` 工具查询一下自己当前网络环境下 github 地址解析的结果，命令是`nslookup github.com` ，可以看到没改 DNS 之前解析的结果是`127.0.0.1` ，显然是不对的。

![](https://picx.zhimg.com/v2-b5e2c39b48419d8e04c2b191590218f7_r.jpg?source=1def8aca)

自己手动改一下 DNS，改成 8.8.8.8 或者 1.1.1.1，然后断网重连 (非必需步骤)，或是重启电脑 (非必需步骤)，应该就可以了。改完之后，查询结果应该如下：

![](https://pica.zhimg.com/v2-ad429bcb5428906cec63d1b028709c53_r.jpg?source=1def8aca)![](https://picx.zhimg.com/v2-abed1a8c04700ba7d72b45195223e0ff_l.jpg?source=1def8aca)火星居民

场景：  
[Mac m1](https://zhida.zhihu.com/search?content_id=485753722&content_type=Answer&match_order=1&q=Mac+m1&zhida_source=entity)

修改 /[etc/resolv.conf](https://zhida.zhihu.com/search?content_id=485753722&content_type=Answer&match_order=1&q=etc%2Fresolv.conf&zhida_source=entity) 不生效。  

![](https://picx.zhimg.com/v2-be5944bc4df395a47e84c7ca8b86154b_r.jpg?source=1def8aca)

看文件注释，文件时自动生成的，所以修改是无效的。  
可以按照百度的文档修改，实测是解决了我的问题。

[https://jingyan.baidu.com/article/47a29f244fb26e8115239900.html](https://link.zhihu.com/?target=https%3A//jingyan.baidu.com/article/47a29f244fb26e8115239900.html)

![](https://picx.zhimg.com/v2-e08d2c70d1f5001fde01696f11843959_r.jpg?source=1def8aca)![](https://picx.zhimg.com/v2-e6256db53209e6d5512efba397008d10_r.jpg?source=1def8aca)![](https://pic1.zhimg.com/v2-8bbeaea29c624135787b4f07f275715f_r.jpg?source=1def8aca)

vscode 需要重启触发 [Copilot](https://zhida.zhihu.com/search?content_id=485753722&content_type=Answer&match_order=1&q=Copilot&zhida_source=entity) 登录  

![](https://picx.zhimg.com/v2-74a25f69a6f009bdc146745d258b0b4d_r.jpg?source=1def8aca)

![](https://pic1.zhimg.com/v2-7b15a52f6718d393a357c29b87875a5e_l.jpg?source=1def8aca)雅乐 top

[解决 copilot 链接不上的问题，请大家看我上一篇文章](https://zhuanlan.zhihu.com/p/522807476)

如果 配置 [hosts](https://zhida.zhihu.com/search?content_id=491743831&content_type=Answer&match_order=1&q=hosts&zhida_source=entity) 依然无效 请大家再试试这两个 host

```
copilot config
140.82.112.3 github.com
140.82.112.3 api.github.com

# vscode copilot config
140.82.112.3 github.com
140.82.112.5 api.github.com

```

host 配置无效自查

1. 检查电脑是否开启其它代理

2.mac 改完 hosts 需要稍等一下才生效，验证方式浏览器尝试访问 github 官网，正常配置完 hosts 后，github 是能够访问的。如果发现配置完 hosts 后不能访问，请尝试切换 第一项 xxx.xx.xxx.x [github.com](https://link.zhihu.com/?target=http%3A//github.com) 的配置

3. 如果打开 vscode 后 copilot 提示：Your Copilot experience is not fully configured, complete your setup. 这是有可能是 copilot 没有完成配置，请打开 [https://github.com/github-copilot](https://link.zhihu.com/?target=https%3A//github.com/github-copilot) 登录账号一步一步进行配置~

最后~ 重启 vscode 就可以啦！！！

![](https://picx.zhimg.com/v2-ad5bbd26ea5f553f80924188d80feaab_l.jpg?source=1def8aca)Hogwarts

我在配置的时候出现了如下输出：

```
[error] [auth] Extension activation failed: "connect ECONNREFUSED 127.0.0.1:443"

```

在查看了很多博客之后终于成功了，简单而言就是在 hosts 文件中加上两行 [http://github.com](https://link.zhihu.com/?target=http%3A//github.com) 和 [http://api.github.com](https://link.zhihu.com/?target=http%3A//api.github.com)。

```
sudo vim /etc/hosts

```

修改 hosts：

```
# github config
140.82.112.3    github.com
140.82.112.5    api.github.com

```

注意如果 hosts 中已经有 [http://api.github.com](https://link.zhihu.com/?target=http%3A//api.github.com) 要删掉。我是在 [wsl](https://zhida.zhihu.com/search?content_id=666858377&content_type=Answer&match_order=1&q=wsl&zhida_source=entity) 中使用的，所以修改 / etc/wsl.conf 确保重启后不会自动生成 hosts：

```
sudo vim /etc/wsl.conf

[network]
generateHosts = false

```

重启 vscode 即可成功使用！

* * *

2024/07/02 更新了 [WSL 后可以使用镜像网络](https://zhida.zhihu.com/search?content_id=666858377&content_type=Answer&match_order=1&q=WSL%E5%90%8E%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8%E9%95%9C%E5%83%8F%E7%BD%91%E7%BB%9C&zhida_source=entity)，我的. wslconfig 文件配置如下：

```
[experimental]
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
hostAddressLoopback=true

```

这样和本机使用相同的代理即可正常访问。