> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/dong__ge/article/details/123581117?ops_request_misc=%7B%22request%5Fid%22%3A%2286ACCF86-AB8B-47E9-8BAD-B2C5BA9F6513%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=86ACCF86-AB8B-47E9-8BAD-B2C5BA9F6513&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-123581117-null-null.142%5Ev100%5Epc_search_result_base8&utm_term=vmware%E6%9C%89%E7%9A%84%E6%9C%89%E7%BD%91%E6%9C%89%E7%9A%84%E6%B2%A1%E7%BD%91&spm=1018.2226.3001.4187)

#### 文章目录

*   *   [方法一：网络连接状态排查](#_14)
    *   [方法二：主机网络服务查询](#_40)
    *   [其他解决方法](#_54)

早上刚到公司，打开电脑，远程连接虚拟机，突然发现

`SSH`

连接失败！

What，什么情况，打开虚拟机，网络连接的按钮都没了？还报了一堆异常！

![](https://i-blog.csdnimg.cn/blog_migrate/899b42af20c2e82da278ad2e1c365616.png)

内心一阵恐慌，这种虚拟机突然崩溃的时候，虽然不经常遇到，一旦碰上，着实烦人。

**基本上，这种情况大多都会遇到，同样导致这样问题可能有很多，在这里记录一下两种解决方案，同时并分享一些其他相同问题的解决方法！**

### 方法一：网络连接状态排查

出现该问题，第一步进行网络状态排查，通常也是最有效的方法之一。

进入`Ctrl+Alt+T`打开终端，输入以下命令，查看网络状态信息。

```
sudo vim /var/lib/NetworkManager/NetworkManager.state

```

可以看到，网络状态信息`NetworkingEnabled=false`，

![](https://i-blog.csdnimg.cn/blog_migrate/3fcd710cda4265e35e702a9bf2dc5a90.png)

不知道怎么就被搞成了`false`，要想修改成`true`，需要以下步骤：

*   **关闭网络服务**：`sudo service network-manager stop`
*   **设置网络状态**：`sudo vim /var/lib/NetworkManager/NetworkManager.state`，设置为`true`
*   **打开网络服务**：`sudo service network-manager start`

此时，就看到了网络连接成功的标识。

![](https://i-blog.csdnimg.cn/blog_migrate/a35c269e416f725f31c840d0f8e6ac5b.png)

### 方法二：主机网络服务查询

还有另外一个原因，一般是自己的主机把服务给关掉了，或者是因为**电脑管家把这些软件给升级了**，然后把**虚拟机的网路服务给停止**了。

右击`我的电脑`，然后找到`管理`->`服务`，**确保下面虚拟机的网络服务是否打开**，然后虚拟机就有网了。

![](https://i-blog.csdnimg.cn/blog_migrate/5df31623c9db29b94a30e62a74e60a2a.png)

> 如果你的还没有解决，可以参考以下文章。

### 其他解决方法

[【VMware】虚拟机中 Ubuntu 无法连接网络的有效解决办法](https://blog.csdn.net/u013554213/article/details/79408084)

[VMware 虚拟机里连不上网的五种解决方案](https://blog.csdn.net/qq_36408196/article/details/103390303)

[VMware 虚拟机无法连接网络解决办法](https://blog.csdn.net/m0_37259197/article/details/78221016)