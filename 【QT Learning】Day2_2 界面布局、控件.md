# 一、界面布局
1. 实现登陆窗口
2. 利用布局方式给窗口进行美化
3. 选取widget进行布局，水平布局、垂直布局、栅格布局
4. 给用户名、密码、登陆、退出按钮进行布局
5. 默认窗口与控件之间有九间隙，可以调整laoutLeftMargin
6.利用弹簧进行布局
<br>[视频教程](https://www.bilibili.com/video/BV1g4411H78N?p=23)
# 二、控件
1. 按钮组
<br>1.1 QPushButton 常用按钮
<br>1.2 QToolButton 工具按钮
<br>1.3 radioButton 单选按钮
<br>1.4 chekbox 多选按钮
```C++
#include "widget.h"
#include "ui_widget.h"
#include <QDebug>
Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //设置单选按钮 男默认选中
    ui->rBtnMan->setChecked(true);

    //选中女后 打印信息
    connect(ui->rBtnWoman,&QRadioButton::clicked,[=](){
        qDebug() << "选中了女了！";
    });

    //多选按钮  2是选中  0是未选中 1是半选
    connect(ui->cBox,&QCheckBox::stateChanged,[=](int state){
        qDebug() << state;
    });


    //利用listWidget写诗
//    QListWidgetItem * item = new QListWidgetItem("锄禾日当午");
//    //将一行诗放入到listWidget控件中
//    ui->listWidget->addItem(item);
//    item->setTextAlignment(Qt::AlignHCenter);


    //QStringList   QList<QString>
    QStringList list ;
    list << "锄禾日当午" << "旱地和下土" << "谁知盘中餐"<< "粒粒皆辛苦";
    ui->listWidget->addItems(list);


}

Widget::~Widget()
{
    delete ui;
}

```
<br>[视频教程](https://www.bilibili.com/video/BV1g4411H78N?p=24)
<br>2. QListWidget列表容器
<br>[视频教程](https://www.bilibili.com/video/BV1g4411H78N?p=25)
<br>3. QTreeWidget
```C++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //treeWidget树控件使用

    //设置水平头
    ui->treeWidget->setHeaderLabels(QStringList()<< "英雄"<< "英雄介绍");

    QTreeWidgetItem * liItem = new QTreeWidgetItem(QStringList()<< "力量");
    QTreeWidgetItem * minItem = new QTreeWidgetItem(QStringList()<< "敏捷");
    QTreeWidgetItem * zhiItem = new QTreeWidgetItem(QStringList()<< "智力");
    //加载顶层的节点
    ui->treeWidget->addTopLevelItem(liItem);
    ui->treeWidget->addTopLevelItem(minItem);
    ui->treeWidget->addTopLevelItem(zhiItem);

    //追加子节点
    QStringList heroL1;
    heroL1 << "刚被猪" << "前排坦克，能在吸收伤害的同时造成可观的范围输出";
    QTreeWidgetItem * l1 = new QTreeWidgetItem(heroL1);
    liItem->addChild(l1);

}

Widget::~Widget()
{
    delete ui;
}
```
<br>[视频教程](https://www.bilibili.com/video/BV1g4411H78N?p=26)
<br>4. QTableWidget
```C++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //TableWidget控件
    //设置列数
    ui->tableWidget->setColumnCount(3);

    //设置水平表头
    ui->tableWidget->setHorizontalHeaderLabels(QStringList()<<"姓名"<< "性别"<< "年龄");

    //设置行数
    ui->tableWidget->setRowCount(5);

    //设置正文
    //ui->tableWidget->setItem(0,0, new QTableWidgetItem("亚瑟"));
    QStringList nameList;
    nameList<< "亚瑟"<< "赵云"<< "张飞"<< "关羽" << "花木兰";

    QList<QString> sexList;
    sexList << "男"<< "男"<< "男"<< "男"<< "女";

    for(int i = 0 ; i < 5 ;i ++)
    {
        int col = 0;
        ui->tableWidget->setItem(i,col++, new QTableWidgetItem(nameList[i]));
        ui->tableWidget->setItem(i,col++, new QTableWidgetItem(sexList.at(i)));
        //int 转 QString
        ui->tableWidget->setItem(i,col++, new QTableWidgetItem( QString::number(i+18)));
    }
}

Widget::~Widget()
{
    delete ui;
}

```
<br>[视频教程](https://www.bilibili.com/video/BV1g4411H78N?p=27)
<br>5. 其他控件介绍
```C++
#include "widget.h"
#include "ui_widget.h"
#include <QMovie>

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //栈控件使用
    //设置默认定位 scrollArea
    ui->stackedWidget->setCurrentIndex(1);

    //scrollArea按钮
    connect(ui->btn_scrollArea,&QPushButton::clicked,[=](){
        ui->stackedWidget->setCurrentIndex(1);
    });

    //toolBox按钮
    connect(ui->btn_ToolBox,&QPushButton::clicked,[=](){
        ui->stackedWidget->setCurrentIndex(2);
    });

    //TabWidget按钮
    connect(ui->btn_TabWidget,&QPushButton::clicked,[=](){
        ui->stackedWidget->setCurrentIndex(0);
    });

    //下拉框
    ui->comboBox->addItem("奔驰");
    ui->comboBox->addItem("宝马");
    ui->comboBox->addItem("拖拉机");

    //点击按钮 选中拖拉机选项
    connect(ui->btn_select,&QPushButton::clicked,[=](){
        //ui->comboBox->setCurrentIndex(2);
        ui->comboBox->setCurrentText("拖拉机");
    });

    //利用QLabel显示图片
    ui->lbl_Image->setPixmap(QPixmap(":/Image/butterfly.png"));

    //利用QLabel显示 gif动态图片
    QMovie * movie = new QMovie(":/Image/mario.gif");
    ui->lbl_movie->setMovie(movie);
    //播放动图
    movie->start();
}

Widget::~Widget()
{
    delete ui;
}
```
<br>[视频教程](https://www.bilibili.com/video/BV1g4411H78N?p=28)
