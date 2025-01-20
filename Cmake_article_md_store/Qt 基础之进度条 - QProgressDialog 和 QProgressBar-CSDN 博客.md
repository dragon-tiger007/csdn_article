> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_38204686/article/details/136069607)

#### Qt 基础之[进度条](https://so.csdn.net/so/search?q=%E8%BF%9B%E5%BA%A6%E6%9D%A1&spm=1001.2101.3001.7020) - QProgressDialog 和 QProgressBar

*   [引言](#_1)
*   [一、QProgressDialog 例程](#QProgressDialog_4)
*   *   [1.1 效果展示](#11__6)
    *   [1.2 源码](#12__9)
*   [二、QProgressBar 例程](#QProgressBar_35)
*   *   [2.1 效果展示](#21__36)
    *   [2.2 源码](#22__39)
*   [三、QProgressBar 进阶](#QProgressBar_71)

引言
--

`进度条`的作用是用于显示任务或操作的进度，以便用户了解当前任务的完成情况。它可以提供实时、可视的反馈，让用户知道任务的进展情况，以及剩余的时间或工作量。进度条可以用于各种应用场景，比如文件下载、软件安装、[视频播放](https://so.csdn.net/so/search?q=%E8%A7%86%E9%A2%91%E6%92%AD%E6%94%BE&spm=1001.2101.3001.7020)、上传文件、数据处理等，帮助用户更好地管理和掌控任务的执行。

*   `QProgressDialog`和`QProgressBar`用来是 Qt 框架中用于显示进度条的两个类，它们都是基于 QWidget 的派生类，用于在应用程序中显示任务的进度。

一、QProgressDialog 例程
--------------------

*   `QProgressDialog`是一个具有取消按钮的对话框。

### 1.1 效果展示

![](https://i-blog.csdnimg.cn/blog_migrate/e4f070fe8ff231bbc91a19ed27b26dd2.gif)  
点击按钮弹出进度条对话框，点击`不读了`关闭对话框。

### 1.2 源码

```
void MainWindow::on_pushButton_clicked()
{
    //创建一个进度条对话框 .....
    QProgressDialog *progressDialog = new QProgressDialog(this);
    progressDialog->setMinimumWidth(300);               // 设置最小宽度
    progressDialog->setWindowModality(Qt::NonModal);    // 非模态，其它窗口正常交互  Qt::WindowModal 模态
    progressDialog->setMinimumDuration(0);              // 等待0秒后显示
    progressDialog->setWindowTitle(tr("进度条框"));      // 标题名
    progressDialog->setLabelText(tr("正在读取"));        // 标签的
    progressDialog->setCancelButtonText(tr("不读了"));   // 取消按钮
    progressDialog->setRange(0, 10);
    for(int i=1; i <= 10; i++)
    {
        if(progressDialog->wasCanceled())
            return;
        progressDialog->setValue(i);
        // 延时
        QEventLoop eventloop;
        QTimer::singleShot(1000, &eventloop, SLOT(quit()));
        eventloop.exec();
    }
}

```

二、QProgressBar 例程
-----------------

### 2.1 效果展示

![](https://i-blog.csdnimg.cn/blog_migrate/a3bbe4202da66d0344a4d393167095bf.gif)  
在窗口中添加了一个`progressBar`，直接设置其`Range`和`Value`即可。

### 2.2 源码

```
void MainWindow::on_pushButton_clicked()
{
    // 创建一个进度条对话框 .....
    QProgressDialog *progressDialog = new QProgressDialog(this);
    progressDialog->setMinimumWidth(300);               // 设置最小宽度
    progressDialog->setWindowModality(Qt::NonModal);    // 非模态，其它窗口正常交互  Qt::WindowModal 模态
    progressDialog->setMinimumDuration(0);              // 等待0秒后显示
    progressDialog->setWindowTitle(tr("进度条框"));      // 标题名
    progressDialog->setLabelText(tr("正在读取"));        // 标签的
    progressDialog->setCancelButtonText(tr("不读了"));   // 取消按钮
    progressDialog->setRange(0, 10);

    // progressBar
    ui->progressBar->setRange(0, 10);

    // 显示进度
    for(int i=1; i <= 10; i++)
    {
        if(progressDialog->wasCanceled())
            return;
        progressDialog->setValue(i);
        ui->progressBar->setValue(i);
        // 延时
        QEventLoop eventloop;
        QTimer::singleShot(1000, &eventloop, SLOT(quit()));
        eventloop.exec();
    }
}

```

三、QProgressBar 进阶
-----------------

QProgressDialog 约等于 对话框 + `QProgressBar` + 按钮，以`QProgressBar`为切入点，深入了解 Qt 进度条.

*   1.  先设置最大值`maximum`和最小值`minimum`，然后给它提供当前值`v`时显示已完成的百分比。百分比的计算方法 = ( v − m i n i m u m ) / ( m a x i m u m − m i n i m u m ) (v - minimum) / (maximum-minimum) (v−minimum)/(maximum−minimum)。  
        ![](https://i-blog.csdnimg.cn/blog_migrate/d0f3eaee916536c5f6b9bd2c11515855.png)

```
void MainWindow::on_pushButton_clicked()
{
    i = 0;
    // 实现定时事件
    connect(timer, &QTimer::timeout, [this](){
        i++;
        ui->progressBar->setValue(i);
        if(i == ui->progressBar->maximum()){
            timer->stop();
        }
    });

    // progressBar
    ui->progressBar->setRange(0, 10);

    // 开启定时器
    timer->start(1000);
}

```

*   2.  获取其常用的属性  
        ![](https://i-blog.csdnimg.cn/blog_migrate/70b1a4a371c9965b5abc41587de8f568.png)

```
    // progressBar
    ui->progressBar->setRange(0, 10);
    qDebug()<<"当前值："<<ui->progressBar->value();
    qDebug()<<"最大值："<<ui->progressBar->maximum();
    qDebug()<<"最小值："<<ui->progressBar->minimum();
    qDebug()<<"方向："<<ui->progressBar->orientation();
    qDebug()<<"格式："<<ui->progressBar->format();

```

*   3.  当其最大值 = 最小值 = 0，进度条会没有显示但一直在动  
        ![](https://i-blog.csdnimg.cn/blog_migrate/35ee7b1f145b23aa22b94ab7a64ce3aa.gif)

```
void MainWindow::on_pushButton_clicked()
{
    i = 0;
    // 实现定时事件
    connect(timer, &QTimer::timeout, [this](){
        i++;
        ui->progressBar->setValue(i);
        if(i == ui->progressBar->maximum()){
            timer->stop();
        }
    });

    // progressBar
    ui->progressBar->setRange(0, 0);

    // 开启定时器
    timer->start(1000);
}

```

*   4.  格式 format：%p - 百分比. %v - 当前值. %m - 总值. `ui->progressBar->setFormat("%p"); // %v %m`  
        ![](https://i-blog.csdnimg.cn/blog_migrate/551740faccce00ee32cc408430933d59.png)  
        ![](https://i-blog.csdnimg.cn/blog_migrate/a76d57c608645688a7b982d3a45d72a6.png)  
        ![](https://i-blog.csdnimg.cn/blog_migrate/40bc8bd6443cfdc4fefdbadf71645526.png)
*   5.  自定义格式：`ui->progressBar->setFormat("%p% %v/%m");`  
        ![](https://i-blog.csdnimg.cn/blog_migrate/8d2673c6ae33f494346ab4ca40430071.png)