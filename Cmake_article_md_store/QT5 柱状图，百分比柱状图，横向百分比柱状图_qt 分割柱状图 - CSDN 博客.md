> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/yky189/article/details/102605006?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522ef48b63e8e54e9c65bf415b004a45787%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=ef48b63e8e54e9c65bf415b004a45787&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-102605006-null-null.142^v101^pc_search_result_base2&utm_term=QT%E6%9F%B1%E7%8A%B6%E5%9B%BE&spm=1018.2226.3001.4187)

```
**
 series->setLabelsVisible(true);//设置数据是否可见
**

```

QT 的官方**柱状图**，如下图  
![](https://i-blog.csdnimg.cn/blog_migrate/c8270228482677852368c661314f5036.png)  
QtCharts 画图表必须要的两样东西 QChartView，QChart 相当于画布画笔，就不多说了。无论画什么图主要差别在于画笔里面的东西。  
然后需要一根一根的柱子 QBarSet，再把一根一根的柱子加到柱子的列当中 QBarSeries，那么柱状图机画好了，可是没有 x、y 轴啊，哈哈哈，所有还需要为 QChart 和 QBarSeries 设置 x、y 轴。那么就完成了。具体实现如下：

```
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

//![1]设置数据，相当于多少根柱子，每根的高度，
    QBarSet *set0 = new QBarSet("Jane");
    QBarSet *set1 = new QBarSet("John");
    QBarSet *set2 = new QBarSet("Axel");
    QBarSet *set3 = new QBarSet("Mary");
    QBarSet *set4 = new QBarSet("Samantha");

    *set0 << 1 << 2 << 3 << 4 << 5 << 6;
    *set1 << 5 << 0 << 0 << 4 << 0 << 7;
    *set2 << 3 << 5 << 8 << 13 << 8 << 5;
    *set3 << 5 << 6 << 7 << 3 << 4 << 5;
    *set4 << 9 << 7 << 5 << 3 << 1 << 2;
//![1]

//![2]把数据加载到数据列表中
    QBarSeries *series = new QBarSeries();
    series->append(set0);
    series->append(set1);
    series->append(set2);
    series->append(set3);
    series->append(set4);

//![2]

//![3]初始化画笔
    QChart *chart = new QChart();
    chart->addSeries(series);
    chart->setTitle("Simple barchart example");
    chart->setAnimationOptions(QChart::SeriesAnimations);
//![3]

//![4]为QChart和QBarSeries设置坐标轴
    QStringList categories;
    categories << "Jan" << "Feb" << "Mar" << "Apr" << "May" << "Jun";
    QBarCategoryAxis *axisX = new QBarCategoryAxis();//初始化X轴
    axisX->append(categories);
    chart->addAxis(axisX, Qt::AlignBottom);
    series->attachAxis(axisX);

    QValueAxis *axisY = new QValueAxis();//初始化Y轴
    axisY->setRange(0,15);
    chart->addAxis(axisY, Qt::AlignLeft);
    series->attachAxis(axisY);
//![4]

//![5]设置图标的标题属性
    chart->legend()->setVisible(true);
    chart->legend()->setAlignment(Qt::AlignBottom);
//![5]

//![6]设置画布
    QChartView *chartView = new QChartView(chart);
    chartView->setRenderHint(QPainter::Antialiasing);
//![6]

//![7]
    QMainWindow window;
    window.setCentralWidget(chartView);
    window.resize(420, 300);
    window.show();
//![7]

    return a.exec();
}

```

**百分比柱状图**，如下  
![](https://i-blog.csdnimg.cn/blog_migrate/ee26c628a4367777f26ae258728cf5ef.png)做法跟上面的柱状图差不多，就是把数据序列 QBarSeries 换成 QPercentBarSeries 而已，其它完全一致

**横向百分比柱状图**，如下  
![](https://i-blog.csdnimg.cn/blog_migrate/d5ef68764e7d148726733f26ad5771bd.png)做法跟上面的柱状图差不多，就是把数据序列 QBarSeries 换成 QHorizontalPercentBarSeries 而已，其它完全一致