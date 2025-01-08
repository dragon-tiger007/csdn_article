> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/2301_77613763/article/details/142302740?ops_request_misc=%257B%2522request%255Fid%2522%253A%25226d35f666a597099dd4f61e215bd31916%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=6d35f666a597099dd4f61e215bd31916&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-3-142302740-null-null.142^v101^control&utm_term=github%E4%B8%8A%E4%BC%A0%E4%BB%A3%E7%A0%81&spm=1018.2226.3001.4187)

### 前提：

*   已有 **Github 账号**。没有的话自己去注册一个吧。
*   已有**一个建立好的项目**用于上传，任何项目都行
*   已在电脑上**安装好 git bash**，并且已配置好用户身份。  
    配置用户身份，就是在刚刚安装好 git 工具之后，打开 Git Bash，敲入两条命令：  
    git config `--`global user.name “你的名字”  
    git config `--`global user.email “你的邮箱地址”

所以读者要先做好上述准备条件才能进行后面的步骤哦，要是读者们想看我写上面准备工作的文章可以在评论区留言，评论很多的话我马上加班写！！！。

### 详细步骤：

### 一. 创建本地仓库

##### 1.1 首先找到你的项目代码位置

![](https://i-blog.csdnimg.cn/direct/44f40fb5445d40758dd96874cc012c2a.png)

##### 1.2 右键项目点击显示更多选项

![](https://i-blog.csdnimg.cn/direct/02b47742ae6141ad98c3054c20e9699f.png)

##### 1.3 再点击 Open Git Bash here, 要是读者还没有安装好 git bash，建议去安装一下再看下去

![](https://i-blog.csdnimg.cn/direct/8d7a0c157f7f404fa60f4c07e41421f5.png)

##### 1.4 在跳出的界面中输入 git init ，创建本地仓库，项目位置右侧出现 master 并出现下面的信息即创建成功

![](https://i-blog.csdnimg.cn/direct/9be8cd61e2134d82b84ff34b7c95ea27.png)

#### 二. 向本地仓库上传项目代码

2.1 使用 git add . 上传代码至本地仓库，注意空间有空格

![](https://i-blog.csdnimg.cn/direct/06f0427331424f9e89148d18678c2dd1.png)

2.2 我这边出现了一点小问题（原因是 Git 警告我，`src/Main.java` 文件的[换行符](https://so.csdn.net/so/search?q=%E6%8D%A2%E8%A1%8C%E7%AC%A6&spm=1001.2101.3001.7020)格式将从 LF（Unix 常用）变成 CRLF（Windows 常用）。这是因为你在 Windows 系统上操作）

  我希望在提交时自动将所有文件转换为 LF（适用于跨平台开发时保持一致性），使用 git config --global core.autocrlf  input ，接着再 git add . 上传代码至本地仓库就没有问题

![](https://i-blog.csdnimg.cn/direct/cbd5f9a0fa8244988dfcca898d3eb3a1.png)

#### 三. 向本地仓库添加提交信息（相当于你上一步上传完代码，现在对你这次提交的代码添加备注）

3.1 添加提交信息，输入： git commit -m "提交信息"

![](https://i-blog.csdnimg.cn/direct/5f049b9f86984d679b7a5369ae5c7672.png)

#### 四. 添加远程仓库

现在项目代码已经上传至本地仓库了，要上传至 [github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020)，首先我们要在 github 中创建好一个仓库

4.1 在 github 的右上角加号的地方选择 New repository 创建新仓库

![](https://i-blog.csdnimg.cn/direct/dbefe3f0a3d041f2a9989c12592aa841.png)

4.2 填写仓库信息，我们只要在那个 Repository name 下面填写上我们的仓库名即可

![](https://i-blog.csdnimg.cn/direct/a227329939894283bcd5a9a8009245f5.png)

![](https://i-blog.csdnimg.cn/direct/e0094149223b439aa1b6e63b1d371498.png)

4.3 划到下面点击 Create repository 创建仓库

![](https://i-blog.csdnimg.cn/direct/3887540db1844ae2b4ae6003422e6369.png)

4.4 远程仓库创建成功

![](https://i-blog.csdnimg.cn/direct/45ac5d6868b543458eca86b980705e81.png)

#### 五. 上传本地仓库中的代码至远程仓库

5.1 选择 SSH，并复制右侧的信息

![](https://i-blog.csdnimg.cn/direct/d66b80658da241aebf4e61b7a6886ee7.png)

5.2 连接远程仓库：回到 git bash, 输入 git remote add origin 复制 SSH 的信息。

这里粘贴的时候最好右键点击 Paste, 因为直接 Ctrl+V 会出现错误。

![](https://i-blog.csdnimg.cn/direct/ba581095d0e34ff6a315c87c9fec72e1.png)

5.2 上传代码至远程仓库，输入 git push origin master![](https://i-blog.csdnimg.cn/direct/df2fbbd9a9a9468cb18bd1718c04ba85.png)

5.3 回到 github 的仓库进行查看，发现仓库上已经有了我们上传的代码及我们提交的备注

![](https://i-blog.csdnimg.cn/direct/b73cd3ab51a64f4ab2355c520ff127a1.png)