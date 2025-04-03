> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/chengyq116/article/details/125109922?ops_request_misc=%7B%22request%5Fid%22%3A%228c75db4651fcc032f600d1a8a15e9916%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=8c75db4651fcc032f600d1a8a15e9916&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-125109922-null-null.142%5Ev102%5Epc_search_result_base5&utm_term=windows%E6%80%8E%E4%B9%88%E5%88%87%E6%8D%A2%E7%9B%AE%E5%BD%95&spm=1018.2226.3001.4187)

#### 

Windows 命令行切换目录

*   [1. Windows 上一级目录、当前目录和根目录](#1_Windows__6)
*   [2. Windows 进入 `C:\Program Files (x86)\AMD APP SDK\3.0` 目录](#2_Windows__CProgram_Files_x86AMD_APP_SDK30__88)
*   [3. Windows 进入 `D:\Program Files\AMD APP SDK\3.0` 目录](#3_Windows__DProgram_FilesAMD_APP_SDK30__139)
*   [References](#References_226)

**Windows Commands - Command-line reference A-Z**  
[https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)  
[https://docs.microsoft.com/zh-cn/windows-server/administration/windows-commands/windows-commands](https://docs.microsoft.com/zh-cn/windows-server/administration/windows-commands/windows-commands)

1. Windows 上一级目录、当前目录和根目录
-------------------------

`cd ..` - 进入上一级目录，`..` 表示上一级目录。  
`cd .` - 进入当前目录，`.` 表示当前目录。  
`cd \` - 进入根目录，`\` 表示根目录。

```
Microsoft Windows [版本 10.0.19043.1706]
(c) Microsoft Corporation。保留所有权利。

C:\Users\cheng>dir
 驱动器 C 中的卷是 OS
 卷的序列号是 06C1-1F42

 C:\Users\cheng 的目录

2022/05/23  21:26    <DIR>          .
2022/05/23  21:26    <DIR>          ..
2020/08/29  15:57    <DIR>          .android
2020/04/29  14:48    <DIR>          .dnx
2021/10/29  21:36    <DIR>          .vscode
2020/07/12  23:37    <DIR>          3D Objects
2020/05/02  16:43    <DIR>          ansel
2020/07/12  23:37    <DIR>          Contacts
2022/05/21  22:46    <DIR>          Desktop
2022/06/01  22:44    <DIR>          Documents
2022/05/10  22:41    <DIR>          Downloads
2020/07/12  23:37    <DIR>          Favorites
2020/07/12  23:37    <DIR>          Links
2020/07/12  23:37    <DIR>          Music
2021/08/09  22:48    <DIR>          OneDrive
2020/07/12  23:37    <DIR>          Pictures
2020/07/12  23:37    <DIR>          Saved Games
2021/02/20  23:45    <DIR>          Searches
2022/06/03  08:32    <DIR>          Videos
               0 个文件              0 字节
              19 个目录 25,219,067,904 可用字节

C:\Users\cheng>
C:\Users\cheng>cd ..

C:\Users>dir
 驱动器 C 中的卷是 OS
 卷的序列号是 06C1-1F42

 C:\Users 的目录

2021/08/25  23:22    <DIR>          .
2021/08/25  23:22    <DIR>          ..
2022/05/23  21:26    <DIR>          cheng
2020/07/12  23:25    <DIR>          Public
               0 个文件              0 字节
               4 个目录 25,209,688,064 可用字节

C:\Users>
C:\Users>cd \

C:\>
C:\>dir
 驱动器 C 中的卷是 OS
 卷的序列号是 06C1-1F42

 C:\ 的目录

2020/04/28  08:09    <DIR>          Apps
2020/04/28  08:09    <DIR>          backup
2020/05/05  18:32    <DIR>          dell
2020/05/07  10:57    <DIR>          Downloads
2020/04/28  08:09    <DIR>          Drivers
2020/07/12  23:26    <DIR>          Intel
2019/12/07  17:14    <DIR>          PerfLogs
2022/04/03  11:47    <DIR>          Program Files
2022/01/20  22:07    <DIR>          Program Files (x86)
2021/08/25  23:22    <DIR>          Users
2022/06/01  22:24    <DIR>          Windows
2020/07/12  22:00    <DIR>          Windows10Upgrade
               0 个文件              0 字节
              12 个目录 25,215,115,264 可用字节

C:\>

```

2. Windows 进入 `C:\Program Files (x86)\AMD APP SDK\3.0` 目录
---------------------------------------------------------

`cd \Program Files (x86)\AMD APP SDK\3.0` - 进入 `\Program Files (x86)\AMD APP SDK\3.0` 目录。

```
Microsoft Windows [版本 10.0.19043.1706]
(c) Microsoft Corporation。保留所有权利。

C:\Users\cheng>cd \Program Files (x86)\AMD APP SDK\3.0

C:\Program Files (x86)\AMD APP SDK\3.0>dir
 驱动器 C 中的卷是 OS
 卷的序列号是 06C1-1F42

 C:\Program Files (x86)\AMD APP SDK\3.0 的目录

2022/01/20  22:07    <DIR>          .
2022/01/20  22:07    <DIR>          ..
2022/01/20  22:07    <DIR>          bin
2022/01/20  22:07    <DIR>          docs
2022/01/20  22:07    <DIR>          include
2022/01/20  22:07    <DIR>          lib
               0 个文件              0 字节
               6 个目录 25,213,906,944 可用字节

C:\Program Files (x86)\AMD APP SDK\3.0>cd \

C:\>dir
 驱动器 C 中的卷是 OS
 卷的序列号是 06C1-1F42

 C:\ 的目录

2020/04/28  08:09    <DIR>          Apps
2020/04/28  08:09    <DIR>          backup
2020/05/05  18:32    <DIR>          dell
2020/05/07  10:57    <DIR>          Downloads
2020/04/28  08:09    <DIR>          Drivers
2020/07/12  23:26    <DIR>          Intel
2019/12/07  17:14    <DIR>          PerfLogs
2022/04/03  11:47    <DIR>          Program Files
2022/01/20  22:07    <DIR>          Program Files (x86)
2021/08/25  23:22    <DIR>          Users
2022/06/01  22:24    <DIR>          Windows
2020/07/12  22:00    <DIR>          Windows10Upgrade
               0 个文件              0 字节
              12 个目录 25,213,906,944 可用字节

C:\>

```

3. Windows 进入 `D:\Program Files\AMD APP SDK\3.0` 目录
---------------------------------------------------

`D:` + `cd D:\Program Files\AMD APP SDK\3.0` - 进入 `D:\Program Files\AMD APP SDK\3.0` 目录

```
Microsoft Windows [版本 10.0.19043.1706]
(c) Microsoft Corporation。保留所有权利。

C:\Users\cheng>
C:\Users\cheng>D:

D:\>
D:\>cd D:\Program Files\AMD APP SDK\3.0

D:\Program Files\AMD APP SDK\3.0>
D:\Program Files\AMD APP SDK\3.0>cd \

D:\>
D:\>cd \Program Files\AMD APP SDK\3.0

D:\Program Files\AMD APP SDK\3.0>
D:\Program Files\AMD APP SDK\3.0>cd \

D:\>

```

`cd /d D:\Program Files\AMD APP SDK\3.0` - 进入 `D:\Program Files\AMD APP SDK\3.0` 目录

Parameter `/d` - Changes the current drive as well as the current directory for a drive.  
参数 `/d` - 更改[驱动器](https://so.csdn.net/so/search?q=%E9%A9%B1%E5%8A%A8%E5%99%A8&spm=1001.2101.3001.7020)的当前驱动器以及当前目录。

```
Microsoft Windows [版本 10.0.19043.1706]
(c) Microsoft Corporation。保留所有权利。

C:\Users\cheng>cd /d D:\Program Files\AMD APP SDK\3.0

D:\Program Files\AMD APP SDK\3.0>dir
 驱动器 D 中的卷是 SOFTWARE
 卷的序列号是 72F2-2543

 D:\Program Files\AMD APP SDK\3.0 的目录

2022/01/24  23:57    <DIR>          .
2022/01/24  23:57    <DIR>          ..
2022/01/20  22:07    <DIR>          bin
2015/10/09  11:37               578 glut_notice.txt
2022/01/20  22:07    <DIR>          include
2022/01/20  22:07    <DIR>          lib
2015/10/09  11:37             3,238 LICENSE-llvm.txt
2015/10/09  11:37            36,243 LICENSE-mingw.txt
2022/01/20  22:07    <DIR>          samples
2022/01/23  13:53       145,008,530 samples.rar
2022/01/20  22:23    <DIR>          samples_backup
2022/01/23  13:54       145,023,833 samples_backup.rar
2022/01/20  22:07               241 VersionInfo.txt
2022/01/24  23:57            13,462 新建 Microsoft Excel 工作表.xlsx
               7 个文件    290,086,125 字节
               7 个目录 254,238,978,048 可用字节

D:\Program Files\AMD APP SDK\3.0>

```

`cd /d E:\source_code` - 进入 `E:\source_code` 目录

```
Microsoft Windows [版本 10.0.19043.1706]
(c) Microsoft Corporation。保留所有权利。

C:\Users\cheng>cd /d E:\source_code

E:\source_code>dir
 驱动器 E 中的卷是 DATA
 卷的序列号是 8EF7-66A5

 E:\source_code 的目录

2020/05/31  16:54    <DIR>          .
2020/05/31  16:54    <DIR>          ..
2017/07/08  08:30    <DIR>          tensor2tensor-1.0.12
2020/05/31  16:53           210,001 tensor2tensor-1.0.12.zip
               1 个文件        210,001 字节
               3 个目录 307,615,502,336 可用字节

E:\source_code>

```

References
----------

[1] Yongqiang Cheng, [https://yongqiang.blog.csdn.net/](https://yongqiang.blog.csdn.net/)