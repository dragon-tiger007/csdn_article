> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/YanDabbs/article/details/139991444?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165ac6390177c6b06c6fc8f0ef14b640%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=165ac6390177c6b06c6fc8f0ef14b640&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-139991444-null-null.142^v101^control&utm_term=Permission%20denied%20%28publickey%29%20%20github%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95&spm=1018.2226.3001.4187)

#### 问题现象：

从 [github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020) 克隆代码或者推送仓库的时候提示如下错误信息：

```
Permission denied (publickey).
fatal: The remote end hung up unexpectedly

```

#### 解决方法：

只是其中的一种，我遇到的是这种解决方法：

```
git config --global core.sshCommand "'C:\Windows\System32\OpenSSH\ssh.exe'"

```

#### 环境：

系统：windows 11  
[shell](https://so.csdn.net/so/search?q=shell&spm=1001.2101.3001.7020): PowerShell

#### 备注：

我试了重新生成 ssh 的密钥，还有通过 ssh 链接 github 都是正常的，测试命令如下：

```
ssh -T git@github.com

```

返回如下：

```
Hi liu-shichao! You've successfully authenticated, but GitHub does not provide shell access.

```

说明 [ssh 配置](https://so.csdn.net/so/search?q=ssh%20%E9%85%8D%E7%BD%AE&spm=1001.2101.3001.7020)是正确的，碰巧看到了上边那种解决方法，顺利解决了，可能是之前用过别的 git 工具修改过这个配置。