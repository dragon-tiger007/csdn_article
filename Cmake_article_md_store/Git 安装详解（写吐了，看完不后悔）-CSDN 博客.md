> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_45730223/article/details/131693287?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522271afb53cd7d50109058ad70710f6b7f%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=271afb53cd7d50109058ad70710f6b7f&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-131693287-null-null.142^v101^control&utm_term=git%E5%AE%89%E8%A3%85&spm=1018.2226.3001.4187)

> Git 是一个非常流行的[分布式版本控制系统](https://so.csdn.net/so/search?q=%E5%88%86%E5%B8%83%E5%BC%8F%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6%E7%B3%BB%E7%BB%9F&spm=1001.2101.3001.7020)，它帮助开发者管理和跟踪项目中的代码变化。通俗地说，可以认为 Git 就像是一个代码的时间机器，它记录了项目从开始到结束的每一次代码变动。
> 
> 无论你是个人开发者还是团队成员，掌握 Git 都能提高你的工作效率、简化[代码管理](https://so.csdn.net/so/search?q=%E4%BB%A3%E7%A0%81%E7%AE%A1%E7%90%86&spm=1001.2101.3001.7020)，并为未来的版本控制奠定坚实基础，所以学习 git 还是非常必要的。

### Git 的安装

> 本文主要记录的是 windows 下的 git 的安装。

#### 下载安装包

##### 官网下载

下载的地址就是官网即可：[https://git-scm.com/](https://git-scm.com/)

进来直接选择 windows 的安装包下载

![](https://i-blog.csdnimg.cn/blog_migrate/086934dc37e1d80de5e23b2f90513b1e.png)

进来之后选择自己合适的版本即可，这里提供了两种类型的 git：

1.  独立安装程序（Standalone Installer）：32 位和 64 位 Git for Windows 的安装程序。独立安装程序是一种简便的方式，将 Git for Windows 直接安装到计算机上，使其成为系统的一部分。
2.  便携版（Portable）：32 位和 64 位 Git for Windows 的便携式版本，也被称为 “存储在可移动设备上的版本”。便携版可以在没有安装过程的情况下直接运行，非常适合携带在便携式存储设备（如 USB 闪存驱动器）中使用，方便在不同计算机之间使用 Git。

一般选择 64 位的安装包即可。

这里另外提供了通过 winget 安装的方式：`winget install --id Git.Git -e --source winget`

*   **winget 是什么**？

> winget 是由微软推出的一款命令行工具，用于在 Windows 操作系统上管理应用程序的安装、卸载和更新。它旨在提供一种简单、统一的方式来获取和管理各种软件。
> 
> 下面是一些 winget 的特点和功能：
> 
> 1.  快速安装：winget 可以通过使用一个简单的命令来快速安装指定的应用程序。只需输入应用程序的名称或标识符，winget 就会从 Microsoft Store 或其他软件源中下载和安装应用程序。
> 2.  简化更新：使用 winget 可以轻松地检查并更新已安装应用程序的最新版本。你可以运行`winget upgrade`命令来查找更新并进行更新操作。
> 3.  详细信息查询：通过运行`winget show`命令，你可以获取有关已安装应用程序的详细信息，例如发布者、版本号、安装位置等。
> 4.  一键卸载：当你想要卸载某个应用程序时，winget 提供了`winget uninstall`命令来帮助你更快捷地卸载应用程序。
> 5.  自定义安装参数：对于某些应用程序，winget 允许你使用自定义安装参数以满足特定需求。你可以在安装命令中指定选项和参数来自定义安装过程。
> 6.  软件源管理：winget 支持多个软件源，包括 Microsoft Store、winget 官方源和其他第三方源。你可以使用`winget source`命令来管理和配置这些软件源。
> 
> 需要注意的是，为了使用 winget 工具，你需要在 Windows 10 版本 2004 或更高版本的操作系统上安装它。同时，不是所有的应用程序都可以通过 winget 进行安装和管理，因为这需要应用程序的开发者将其注册到 winget 的软件源中。
> 
> 总的来说，winget 提供了一种方便、快捷的方式来管理 Windows 系统上的应用程序，帮助用户更轻松地安装、升级和卸载软件。

![](https://i-blog.csdnimg.cn/blog_migrate/1f907c02263e25bc45bb9f2ebe219abb.png)

##### 阿里镜像

官网点击下载，一般是从 [GitHub](https://so.csdn.net/so/search?q=GitHub&spm=1001.2101.3001.7020) 下载，可能会被墙，所以也可以使用阿里镜像下载，下载地址：

[https://registry.npmmirror.com/binary.html?path=git-for-windows/](https://registry.npmmirror.com/binary.html?path=git-for-windows/)

![](https://i-blog.csdnimg.cn/blog_migrate/1b0f05001f27cb01c640ba3289b3a580.png)

> 选择对应的版本，前面带有 rc0、rc1 字样的都是预发布的候选版本，一般选不带这个字样的即可；
> 
> 版本选择 windows 可执行的文件安装；

![](https://i-blog.csdnimg.cn/blog_migrate/94a5d993c854fc668efe549d79c596b3.png)

*   各版本区别

> 1.  Git-2.40.1-32-bit.exe / Git-2.40.1-64-bit.exe：
>     *   这些是 Git 的 Windows 可执行文件版本。
>     *   "32-bit" 版本适用于 32 位的 Windows 操作系统，而 "64-bit" 版本适用于 64 位的 Windows 操作系统。
>     *   这些版本可以通过运行可执行文件安装 Git 并在命令行或图形化界面中使用。
>     *   适用于 Windows 操作系统上的一般 Git 使用场景。
> 2.  Git-2.40.1-32-bit.tar.bz2 / Git-2.40.1-64-bit.tar.bz2：
>     *   这些是针对 32 位和 64 位系统的 tar 压缩文件版本。
>     *   "tar.bz2" 是一种常见的文件压缩格式，通常在 Unix 和 Linux 系统上使用。
>     *   这些版本需要先解压缩，然后在命令行中使用解压后的文件路径来运行 Git 命令。
>     *   适用于 Unix 和 Linux 操作系统上的 Git 使用场景。
> 3.  MinGit-2.40.1-32-bit.zip / MinGit-2.40.1-64-bit.zip：
>     *   这些是包含了 Git 最小安装配置的 32 位和 64 位 Windows 操作系统的 zip 压缩文件版本。
>     *   "MinGit" 是一个轻量级的 Git 发行版，只包含最基本的 Git 工具，没有图形化界面。
>     *   这些版本可以在 Windows 操作系统上解压缩后直接使用，无需进行安装。
>     *   适用于需要一个轻量级 Git 环境的 Windows 用户。
> 4.  MinGit-2.40.1-busybox-32-bit.zip / MinGit-2.40.1-busybox-64-bit.zip：
>     *   这些也是针对 32 位和 64 位 Windows 操作系统的 zip 压缩文件版本。
>     *   "busybox" 是一个 Linux 工具集，提供了用于模拟 Unix 环境的多个工具。
>     *   这些版本结合了 Git 和 busybox 工具，提供了更丰富的 Unix/Linux 命令支持。
>     *   适用于 Windows 用户需要在 Git 环境中使用额外 Unix/Linux 工具的场景。
> 5.  pdbs-for-git-32-bit-2.40.1.1.ceee26d5ca-1.zip / pdbs-for-git-64-bit-2.40.1.1.ceee26d5ca-1.zip：
>     *   这些是包含调试符号（PDBs）的 Git 版本，用于调试 Git 代码。
>     *   "32-bit" 版本适用于 32 位的 Windows 操作系统，而 "64-bit" 版本适用于 64 位的 Windows 操作系统。
>     *   调试符号版本的 Git 可用于分析和调试 Git 源代码。
>     *   适用于开发人员需要对 Git 源代码进行调试的场景。
> 6.  PortableGit-2.40.1-32-bit.7z.exe / PortableGit-2.40.1-64-bit.7z.exe：
>     *   这些是针对 32 位和 64 位 Windows 操作系统的 7z 自解压执行文件版本。
>     *   "PortableGit" 是一个便携式版本的 Git，可以在不安装的情况下在计算机上运行。
>     *   这些版本无需安装，只需运行自解压执行文件即可使用。
>     *   适用于需要将 Git 作为便携式工具在不同计算机上使用的场景。
> 7.  v2.40.1.windows.1.tar.gz / v2.40.1.windows.1.zip：
>     *   这些是针对 Windows 操作系统的 tar.gz 和 zip 压缩文件版本。
>     *   这些版本可能是包含完整 Git 安装的版本，但没有特定的 32 位或 64 位限制。
>     *   用户需要先解压缩这些文件，然后在命令行中使用解压后的文件路径来运行 Git 命令。
>     *   适用于 Windows 操作系统上的一般 Git 使用场景。

#### 开始安装

##### 双击安装包

点击`next`下一步

![](https://i-blog.csdnimg.cn/blog_migrate/5776a22c489d6d99dcbede5054a901ea.png)

##### 确认安装目录

根据个人使用习惯决定即可，然后点击下一步；

![](https://i-blog.csdnimg.cn/blog_migrate/cec74f71fe747ced8de3919caceb5af2.png)

##### 选择安装的组件

一般默认就够用，选择后下一步即可；

> 1.  Windows Explorer integration（Windows 资源管理器集成）：
>     *   选择此选项后，Git 会将一些功能集成到 Windows 资源管理器中。
>     *   这样，在 Windows 资源管理器中你可以直接执行 Git 相关操作，如查看文件状态标记、执行 Git 命令等。
> 2.  Git Bash Here：
>     *   选择此选项后，右键单击文件或文件夹时会在菜单中添加 "Git Bash Here" 选项。
>     *   这样你可以通过该选项打开 Git Bash 终端并自动切换到所选文件或文件夹所在的目录。
> 3.  Git GUI Here：
>     *   选择此选项后，右键单击文件或文件夹时会在菜单中添加 "Git GUI Here" 选项。
>     *   这样你可以通过该选项打开 Git GUI 图形化界面并自动切换到所选文件或文件夹所在的目录。
> 4.  Git LFS (Large File Support)：
>     *   选择此选项后，Git 会安装 Git LFS 扩展，用于管理大型文件，如图像、音频和视频文件。
>     *   使用 Git LFS 可以更高效地处理大型文件，并避免将它们存储在 Git 仓库中造成不必要的负担。
> 5.  Asociate .git* configuration files with the default text editor：
>     *   选择此选项后，Git 会关联. gtiignore、.gitattributes 等扩展名为. gt + 的配置文件与系统默认的文本编辑器。
>     *   这样你可以直接双击这些文件，在默认文本编辑器中打开并进行编辑。
> 6.  Associate .sh files to be run with Bash：
>     *   选择此选项后，Git 会关联. sh 扩展名的文件与 Bash 终端。
>     *   这样你可以直接双击. sh 文件，在 Bash 终端中运行脚本。
> 7.  Check daily for Git for Windows updates：
>     *   选择此选项后，Git 会每天检查是否有 Git for Windows 的更新版本，并在有更新时提醒你进行更新。
> 8.  (NEW!) Add a Git Bash Profile to Windows Terminal：
>     *   选择此选项后，Git 会将一个 Git Bash 配置文件添加到 Windows Terminal 中。
>     *   Windows Terminal 是 Windows 上的一个多功能终端应用程序，添加 Git Bash 配置文件后可以直接在 Windows Terminal 中使用 Git Bash。
> 9.  (NEW!) Scalar (Git add-on to manage large scale repositories)：
>     *   选择此选项后，Git 会安装 Scalar，这是一个 Git 的附加组件，用于管理大规模仓库。
>     *   Scalar 提供了一些工具和功能，使大规模仓库的克隆、检出等操作更高效。

![](https://i-blog.csdnimg.cn/blog_migrate/8fe8fe239306869fc5bdf17beac547b8.png)

##### 开始菜单目录

可以更改名称、不添加或者改到其他目录，一般不动；

![](https://i-blog.csdnimg.cn/blog_migrate/733a4f51d9f9eb57b3bfdce40fcb12a2.png)

##### 默认编辑器选择

> 选择 Git 使用的默认编辑器是指设置 Git 在执行某些需要打开编辑器的操作时，默认使用的文本编辑器。这些操作包括编写提交消息、解决合并冲突等。
> 
> 默认的是 vim 编辑器，熟悉一点命令就会操作，没有 notepad 之类的简单，但是也不难，使用默认的 vim 即可；
> 
> 如果要使用其他的编辑器也可以切换，但是可能需要配置一下环境啥的，具体的可以百度下；

![](https://i-blog.csdnimg.cn/blog_migrate/4396eb15350ab9a6bb751b5fb160213b.png)

##### 初始化新项目的主干名称

这个都可以，自己知道是哪个就行

> 在最新的 Git 版本中，关于选择默认分支名称（Default Branch Name），有以下几个选项：
> 
> 1.  让 Git 决定（Let Git decide）： 这是 Git 2.28 版本之前的默认行为。即在创建新的仓库时，Git 会使用默认的分支名称`master`。
> 2.  覆盖新的默认分支名称（Override the default branch name for new repositories）： 由于技术和文化因素的考虑，Git 2.28 版本引入了一个新的默认分支名称的选项。你可以将默认分支更改为其他名称（如`main`）。

![](https://i-blog.csdnimg.cn/blog_migrate/68d554d00938779db891b9853609ecd4.png)

##### 调整 git 的环境变量

一般也是默认的第二个就行

> 1.  “Use Git from Git Bash only”（仅使用 Git Bash 中的 Git）： 这是最谨慎的选择，因为它不会修改你的系统环境变量（PATH）。你只能在 Git Bash 中使用 Git 命令行工具。
> 2.  “Git from the command line and also from 3rd-party software”（从命令行和第三方软件中使用 Git）： 这是推荐的选项，它会将一些最基本的 Git 包装器添加到你的系统环境变量（PATH），以避免在环境中混乱地添加可选的 Unix 工具。你将能够从 Git Bash、命令提示符和 Windows PowerShell 中使用 Git，并且可以在 PATH 中寻找 Git 的任何第三方软件。
> 3.  “Use Git and optional Unix tools from the Command Prompt”（从命令提示符中使用 Git 和可选的 Unix 工具）： 这个选项会将 Git 和可选的 Unix 工具都添加到你的系统环境变量（PATH）中。需要注意的是，这将覆盖 Windows 中的一些工具（如 "find" 和 "sort"）。只有当你完全理解这些影响并愿意接受时，才应选择这个选项。
> 
> 根据你的需求和对系统环境变量的了解，选择合适的选项进行安装。如果你不确定该选择哪个选项，推荐选择第二个选项，因为它提供了最大的灵活性，并且能够在多个环境中使用 Git。

![](https://i-blog.csdnimg.cn/blog_migrate/ae55bcc25d12b98813b53892b012d896.png)

##### 选择 SSH 可执行文件

一般也是默认的即可

> 1.  “Use bundled OpenSSH”（使用捆绑的 OpenSSH）： 这个选项使用 Git 自带的 ssh.exe。Git 将会安装自己的 OpenSSH（以及相关的二进制文件），并使用它们。
> 2.  “Use external OpenSSH”（使用外部 OpenSSH）： 这是一个新选项！这个选项使用外部的 ssh.exe。Git 不会安装自己的 OpenSSH（和相关的二进制文件），而是使用在系统环境变量 PATH 中找到的 OpenSSH。

> 在选择 SSH 可执行文件时，是指在 Git 配置中设置使用哪个 SSH 客户端程序来进行远程操作和身份验证。
> 
> 为什么要选择 SSH 可执行文件呢？这是因为 Git 使用 SSH 协议与远程仓库进行安全通信和身份验证。SSH（Secure Shell）是一种加密的网络协议，用于在不安全网络上安全地执行远程命令和传输数据。通过 SSH，Git 能够连接到远程 Git 仓库并进行操作，例如推送和拉取代码。
> 
> 选择适当的 SSH 可执行文件对于 Git 很重要，原因如下：
> 
> 1.  安全性：SSH 提供了一种安全的通信渠道，通过加密和身份验证来保护数据的传输和访问。选择可靠的 SSH 可执行文件有助于确保 Git 与远程仓库之间的通信是安全的，防止数据泄露和未经授权的访问。
> 2.  兼容性：不同平台和操作系统可能支持不同的 SSH 客户端程序。通过选择适合你操作系统的 SSH 可执行文件，可以确保 Git 在你的环境中正常工作并与远程仓库进行通信。
> 3.  功能和性能：不同的 SSH 客户端程序可能具有不同的功能和性能特点。根据你对功能和性能的需求，选择适合的 SSH 可执行文件可以提供更好的用户体验和效率。

![](https://i-blog.csdnimg.cn/blog_migrate/830f4f54b4ca04d3c03f3f091fafc8e8.png)

##### 选择 HTTPS 后端传输

基本的使用的话，使用 OpenSSL 库即可

> 在选择 HTTPS 传输后端时，可以选择 Git 使用哪个 SSL/TLS 库进行 HTTPS 连接。以下是两个可选项及其含义：
> 
> 1.  使用 OpenSSL 库： 选择此选项将指示 Git 使用 OpenSSL 库来处理 HTTPS 连接。OpenSSL 是一种广泛使用的开源 SSL/TLS 库，提供了安全的加密和身份验证功能。选择此选项后，Git 将使用预配置的 ca-bundle.crt 文件来验证服务器证书。这个文件中包含了受信任的根证书，用于验证远程服务器的证书是否有效和可信任。
> 2.  使用本机 Windows Secure Channel 库： 选择此选项将指示 Git 使用 Windows 本地的 Secure Channel 库来处理 HTTPS 连接。这个库是 Windows 操作系统提供的默认 SSL/TLS 实现，能够与 Windows 证书存储一起工作。选择此选项后，Git 将使用 Windows 证书存储来验证服务器证书。这意味着 Git 将使用操作系统中的证书管理机制，例如 Windows 证书管理器和 Active Directory 域服务，来验证远程服务器的证书。此选项还允许您使用公司内部的根 CA 证书，例如通过 Active Directory 域服务分发的证书。
> 
> 选择合适的 HTTPS 传输后端取决于您的操作系统和环境要求。如果您使用的是 Windows 操作系统，并且希望能够与 Windows 证书存储一起工作并使用公司内部的根 CA 证书，那么选择本机 Windows Secure Channel 库是一个不错的选择。如果您使用的是其他操作系统或有特定需求，如使用特定版本的 SSL/TLS 库或自定义证书存储机制，那么选择 OpenSSL 库可能更适合。

![](https://i-blog.csdnimg.cn/blog_migrate/3e4bc1765791fba804f24f7caea458be.png)

##### 配置行尾转换

这里也选择第一个，可以保证在 Windows 和 Unix 环境下检出的文件都使用正确的行尾符号，减少由于行尾符号差异引起的问题。

> 1.  Checkout Windows style, commit Unix style line endings： 这个选项表示在检出（checkout）文本文件时，Git 会将行尾符号 LF （Unix 风格）自动转换为 CRLF （Windows 风格）。而在提交（commit）文本文件时，Git 会将行尾符号 CRLF 转换回 LF。这适用于跨平台项目，特别是在 Windows 环境下进行开发，并且希望在 Windows 上保留 CRLF 行尾符号的习惯。该选项需要将 "core.autocrlf" 设置为 "true"。
> 2.  Checkout as-is, commit Unix-style line endings： 这个选项表示在检出文本文件时，Git 不会执行任何行尾符号的转换，保持原样。但是在提交文本文件时，Git 会将行尾符号 CRLF 转换为 LF。这适用于跨平台项目，特别是在 Unix 环境下进行开发，并且希望在提交时统一使用 LF 行尾符号。该选项需要将 "core.autocrlf" 设置为 "input"。
> 3.  Checkout as-is, commit as-is： 这个选项表示在检出和提交文本文件时都不执行行尾符号的转换，保持原样。这个选项通常不推荐用于跨平台项目，因为不同操作系统使用不同的行尾符号（CRLF 或 LF）。如果项目中的文件包含不一致的行尾符号，可能会导致问题。该选项需要将 "core.autocrlf" 设置为 "false"。

![](https://i-blog.csdnimg.cn/blog_migrate/02a38ffece6b7d499bd9bf373b299931.png)

##### 配置 Git Bash 使用的终端模拟器

相比之下，cmd 的劣势是较大的，推荐选择第一个

> 1.  使用 MinTTY： Git Bash 将使用 MinTTY 作为终端仿真器。MinTTY 具有可调整大小的窗口、非矩形选择以及 Unicode 字体的特性。它适用于与 Win32 控制台程序（如交互式 Python 或 node.js）一起使用，并提供更好的兼容性和功能。在 MinTTY 环境下运行 Windows 控制台程序时，需要使用 "winpty" 来启动。
> 2.  使用 Windows 默认控制台窗口： Git 将使用 Windows 的默认控制台窗口 (cmd.exe) 作为终端仿真器。这个选项适用于与传统的 Windows 控制台程序一起使用，如交互式 Python 或 node.js。然而，Windows 默认控制台窗口的功能相对有限，默认的滚动回退（scroll-back）功能有限，需要配置 Unicode 字体才能正确显示非 ASCII 字符，并且在 Windows 10 之前，它的窗口大小不可自由调整，只允许矩形文本选择。

![](https://i-blog.csdnimg.cn/blog_migrate/b9ddfbcc31ff91e45e91d627b42a0738.png)

##### git pull 默认行为

通常也是选择默认方式

> 默认情况下，‘git pull’ 的行为取决于 git 配置中的 merge.default 参数。通常有以下三个选项可供选择：
> 
> 1.  Default (fast forward or merge)： 这是’git pull’ 的标准行为：如果可能，将当前分支快进到被拉取的分支，否则创建一个合并提交。
> 2.  Rebase： 将当前分支变基到被拉取的分支上。如果没有本地提交需要变基，则相当于快进操作。
> 3.  Only ever fast-forward： 只进行快进操作，将当前分支快进到被拉取的分支。如果不可行，则操作失败。
> 
> 默认情况下，大多数 git 库配置为执行 Default（fast forward or merge）行为。这意味着在 ‘git pull’ 命令时，Git 会尝试使用快进操作将当前分支更新到已拉取分支的最新状态。如果无法进行快进操作，例如存在冲突，Git 将创建一个合并提交。

![](https://i-blog.csdnimg.cn/blog_migrate/6a1a059a9b2cf5404679f5427e84bd29.png)

##### 选择凭证助手

> 在 Git 中，凭据助手用于管理和存储您在与远程代码库进行身份验证时使用的凭据，例如用户名和密码。根据上述选项，有两个选择：
> 
> 1.  Git Credential Manager： 使用跨平台的 Git Credential Manager（GCM）。Git Credential Manager 是一个凭据助手工具，可以帮助您在访问远程 Git 存储库时自动处理身份验证。它能够安全地存储并检索您的凭据。如果您选择此选项，Git 会配置使用 GCM 作为凭据助手。
> 2.  None： 不使用凭据助手。如果您选择此选项，Git 将不会配置任何凭据助手，并在需要身份验证时，每次都会要求您手动输入凭据。
> 
> 选择哪个凭据助手适合您取决于您的需求和偏好。如果您希望自动处理身份验证并避免频繁输入凭据，可以选择 Git Credential Manager。如果您更倾向于手动输入凭据或者使用其他凭据管理工具，则可以选择 None。
> 
> 设置凭据助手的方式取决于您所使用的操作系统和 Git 版本。您可以通过运行以下命令来查看或更改凭据助手的配置：
> 
> ```
> git config --get credential.helper
> git config --global credential.helper <helper>
> 
> ```
> 
> 其中，`<helper>` 可以是 “manager” 或 “none”。通过更改 `credential.helper` 配置参数，您可以选择相应的凭据助手或不使用凭据助手。

![](https://i-blog.csdnimg.cn/blog_migrate/186e6bec323abfe546acc313115dc749.png)

##### 配置额外选项

默认选择即可

> 根据提供的选项，有两个额外功能可以配置：
> 
> 1.  启用文件系统缓存： 通过将 “core.fscache” 设置为 “true”，文件系统数据将被批量读取并缓存到内存中，以用于某些操作。这将显著提高性能。启用文件系统缓存可以加快某些 Git 操作的速度，特别是对于频繁访问文件系统的操作。注意，现有的代码库不受此设置的影响。
> 2.  启用符号链接： 启用符号链接功能需要具备 “SeCreateSymbolicLink” 权限。启用符号链接功能后，您可以在 Git 仓库中创建和使用符号链接（也称为软连接）。符号链接可以在文件系统中指向其他文件或目录，类似于快捷方式。请注意，此设置对现有的代码库没有影响，只会影响新创建的仓库。
> 
> 选择是否启用这些功能取决于您的需求和操作环境。如果您希望改善某些操作的性能，并且具备适当的权限，则可以启用文件系统缓存和符号链接功能。
> 
> 要配置这些选项，您可以使用以下命令：
> 
> ```
> git config --global core.fscache true     # 启用文件系统缓存
> git config --global core.symlinks true    # 启用符号链接
> 
> ```
> 
> 其中，`--global` 标志表示将配置应用于全局 Git 配置。您还可以使用 `--local` 标志将配置应用于当前仓库的配置文件。

##### 配置实验选项

一般不用开启，直接下一步安装即可

> 有两个实验性功能可以配置：
> 
> 1.  启用伪终端的实验性支持： 启用此功能后，您可以在 Git Bash 窗口中运行原生的控制台程序，如 Node 或 Python，而无需使用 winpty。尽管该功能还存在已知的错误，但它提供了更好的控制台支持。如果您希望在 Git Bash 中运行原生控制台程序，并且愿意接受可能出现的问题，可以启用伪终端的实验性支持。
> 2.  启用内置文件系统监视器的实验性支持： 启用此功能后，Git 将自动运行一个内置的文件系统监视器。该监视器可以加速常见操作（如 git status、git add、git commit 等），特别是对于包含许多文件的工作树。这样可以提高 Git 在处理大型代码库时的响应速度。请注意，这是一个实验性功能，可能会有一些限制和问题。
> 
> 选择是否启用这些实验性功能取决于您的需求和偏好。如果您希望尝试新功能并了解其优势和限制，并且愿意接受潜在的问题和错误，请启用这些功能。
> 
> 这些实验性选项可能会在后续版本中进行修改或删除，因此请谨慎使用。

![](https://i-blog.csdnimg.cn/blog_migrate/ad8e6ad55602f7e3d41e1fdfd40090ed.png)

##### 安装完成

> 可以通过选择安装的快捷方式来启动应用程序。 点击 “Finish” 退出设置。 您可以选择以下操作：
> 
> *   “Launch Git Bash”：启动 Git Bash 终端。
> *   “View Release Notes”：查看版本说明。
> 
> 通过选择 “Launch Git Bash”，您可以打开 Git Bash 终端，它是一个命令行界面，您可以在其中执行 Git 命令和其他命令。
> 
> 通过选择 “View Release Notes”，您可以查看关于当前安装版本的发行说明，了解有关该版本的新功能、更改和修复的信息。

![](https://i-blog.csdnimg.cn/blog_migrate/d63e85143d5d7fa67e2017ee48e1d029.png)

命令行窗口输入`git --version`或者`git -v`可以验证一下

![](https://i-blog.csdnimg.cn/blog_migrate/6e23a8a464f8be2b429b42d0b054c892.png)

### Git 功能简介

在 Windows 安装好的 Git 上，您会得到以下功能：

*   **Git Bash**：Git Bash 提供了一个模拟 Linux 终端的命令行界面，在这里可以执行 Git 命令。它是一种强大的工具，适用于熟悉 Linux 或 macOS 终端界面的开发人员。您可以在 Git Bash 中输入各种 Git 命令，比如克隆代码库、提交更改、合并分支等。
*   **Git CMD**：Git CMD（也称为 Git 命令提示符）是另一种在 Windows 上运行 Git 命令的命令行界面。它提供了与 Git Bash 类似的功能，适用于喜欢使用 Windows 命令提示符的用户。
*   **Git FAQs**：Git FAQs 是一份常见问题解答文档，提供了关于 Git 的常见疑问和解决方法。它包含了一些常见的问题和解答，可以帮助您解决在使用 Git 过程中遇到的问题。
*   **Git GUI**：Git GUI 是一个图形化界面工具，用于执行 Git 操作。它提供了直观的用户界面，有助于不熟悉命令行的开发人员进行版本控制、提交更改、查看历史记录等操作。
*   **Git Release Notes**：Git Release Notes（版本说明）是每个 Git 版本的详细信息记录，包含了新功能、改进、修复的问题和已知问题等内容。通过查看版本说明，您可以了解特定版本的 Git 更新情况和变更点。

![](https://i-blog.csdnimg.cn/blog_migrate/b241b8445a07a2bfa0803b6a43ede472.png)

### 设置用户名

在使用 Git 之前，建议设置全局的用户名称和电子邮件地址，这样每次提交代码时就可以自动关联您的身份信息。

以下是设置 Git 全局用户名称和电子邮件地址的步骤：

1.  打开命令行工具（如终端或命令提示符）。
2.  运行以下命令设置全局用户名：

```
git config --global user.name "Your Name"

```

将 “Your Name” 替换为您自己的姓名或昵称。

1.  运行以下命令设置全局用户电子邮件地址：

```
git config --global user.email "email@example.com"

```

将 “[email@example.com](mailto:email@example.com)” 替换为您的有效电子邮件地址。

这两个设置是可选的，但建议进行配置。它们会将您的姓名和电子邮件地址与每次 Git 提交相关联，以方便其他人识别您所做的更改。

设置一次后，您无需每次都输入这些信息，Git 将自动使用您配置的全局用户信息。如果需要针对特定项目使用不同的用户信息，可以在该项目的目录中运行不带 `--global` 标志的相同命令，具体命令将会将配置限定在当前项目中。

总结来说，虽然设置全局用户名称和电子邮件地址在使用 Git 之前并非强制要求，但是这是一个良好的实践，可以提供更好的跟踪和标识能力。

**可以使用以下命令查看配置的 Git 全局用户名和邮箱信息**：

```
$ git config --global user.name
$ git config --global user.email

```

如果您看到了您期望的用户名和邮箱，则表示您已成功配置了全局用户信息。如果未显示任何输出，则表示尚未设置全局用户信息。

![](https://i-blog.csdnimg.cn/blog_migrate/32ddb924ba263ab7d86c4e125d7c1c86.png)

至此，基本就完成了 Git 的安装和简单的一个配置啦。后续就是学习 Git 的一些入门操作了。

感谢阅读！

原文链接：[https://blog.jiumoz.top/archives/git-an-zhuang-xiang-jie](https://blog.jiumoz.top/archives/git-an-zhuang-xiang-jie)