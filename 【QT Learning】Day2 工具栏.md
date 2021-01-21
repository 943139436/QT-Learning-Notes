# 一、菜单栏、工具栏、状态栏、浮动窗口
1. 菜单栏(setMenuBar)最多只有一个
2. 工具栏(addToolBar)可以有多个
3. 状态栏(setStatusBar)最多一个
4. 浮动窗口（addDockWidget）可以多个
5. 核心部件（setCentralWidget）只能一个

```C++
#include "mainwindow.h"
#include <QMenuBar>
#include <QToolBar>
#include <QDebug>
#include <QPushButton>
#include <QStatusBar>
#include <QLabel>
#include <QDockWidget>
#include <QTextEdit>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    //重置窗口大小
    resize(600,400);

    //菜单栏  只能最多有一个
    //菜单栏创建
    QMenuBar * bar = menuBar();
    //将菜单栏放入到窗口中
    setMenuBar(bar);

    //创建菜单
    QMenu * fileMenu = bar->addMenu("文件");
    QMenu * editMenu = bar->addMenu("编辑");

    //创建菜单项
    QAction * newAction = fileMenu->addAction("新建");
    //添加分割线
    fileMenu->addSeparator();
    QAction * openAction = fileMenu->addAction("打开");




    //工具栏  可以有多个
    QToolBar * toolBar = new QToolBar(this);
    addToolBar(Qt::LeftToolBarArea,toolBar);

    //后期设置 只允许 左右停靠
    toolBar->setAllowedAreas( Qt::LeftToolBarArea | Qt::RightToolBarArea );

    //设置浮动
    toolBar->setFloatable(false);

    //设置移动 (总开关)
    toolBar->setMovable(false);

    //工具栏中可以设置内容
    toolBar->addAction(newAction);
    //添加分割线
    toolBar->addSeparator();
    toolBar->addAction(openAction);
    //工具栏中添加控件
    QPushButton * btn = new QPushButton("aa" , this);
    toolBar->addWidget(btn);


    //状态栏 最多有一个
    QStatusBar * stBar = statusBar();
    //设置到窗口中
    setStatusBar(stBar);
    //放标签控件
    QLabel * label = new QLabel("提示信息",this);
    stBar->addWidget(label);

    QLabel * label2 = new QLabel("右侧提示信息",this);
    stBar->addPermanentWidget(label2);

    //铆接部件 （浮动窗口） 可以有多个
    QDockWidget * dockWidget = new QDockWidget("浮动",this);
    addDockWidget(Qt::BottomDockWidgetArea,dockWidget);
    //设置后期停靠区域，只允许上下
    dockWidget->setAllowedAreas( Qt::TopDockWidgetArea | Qt::BottomDockWidgetArea );


    //设置中心部件 只能一个
    QTextEdit * edit = new QTextEdit(this);
    setCentralWidget(edit);
}

MainWindow::~MainWindow()
{

}

```
# 二、添加资源文件
1. 将图片拷贝到项目位置下
2. 右键项目->添加新文件->Qt->Qt resource file->给资源文件起名（res）
3. 点击生成的res.qrc，添加文件前缀（\）和文件
4. 使用时":+前缀名+文件名"
# 三、对话框 
1. 模态和非模态对话框
<br>1.1 模态对话框：不可以对其他窗口进行操作
<br>1.2 非模态对话框：可以对其他窗口进行操作
2. 标准对话框
2.1 消息对话框(QMessageBox)<br>
2.2 颜色对话框（QColorDialog)<br>
2.3 文件对话框（QFileDialog）<br>
2.4 字体对话框（QFontDialog）<br>
```C++
#include "mainwindow.h"
#include "ui_mainwindow.h"
#include <QDialog>
#include <QDebug>
#include <QMessageBox>
#include <QColorDialog>
#include <QFileDialog>
#include <QFontDialog>
MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    //点击新建按钮 弹出一个对话框
    connect(ui->actionNew,&QAction::triggered,[=](){
        //对话框 分类
        //模态对话框 （不可以对其他窗口进行操作） 非模态对话框 （可以对其他窗口进行操作）
        //模态创建 阻塞
//        QDialog dlg(this);
//        dlg.resize(200,100);
//        dlg.exec();

//        qDebug() << "模态对话框弹出了";


        //非模态对话框
//          QDialog * dlg2 = new QDialog (this);
//          dlg2->resize(200,100);
//          dlg2->show();
//          dlg2->setAttribute(Qt::WA_DeleteOnClose); //55号 属性
//          qDebug() << "非模态对话框弹出了";



        //消息对话框
        //错误对话框
        //QMessageBox::critical(this,"critical","错误");

        //信息对话框
        //QMessageBox::information(this,"info","信息");

        //提问对话框
        //参数1  父亲  参数2  标题  参数3  提示内容  参数4 按键类型  参数5 默认关联回车按键
//        if (QMessageBox::Save  ==  QMessageBox::question(this,"ques","提问",QMessageBox::Save|QMessageBox::Cancel,QMessageBox::Cancel))
//        {
//            qDebug() << "选择的是保存";

//        }
//        else
//        {
//            qDebug() << "选择的是取消";
//        }


        //警告对话框
        //QMessageBox::warning(this,"warning","警告");

        //其他标准对话框
        //颜色对话框
//        QColor color =  QColorDialog::getColor(QColor(255,0,0));
//        qDebug() << "r = " << color.red() << " g = " << color.green() << " b  = " << color.blue() ;

        //文件对话框  参数 1  父亲  参数2  标题   参数3  默认打开路径  参数4  过滤文件格式
        //返回值是 选取的路径
//         QString str = QFileDialog::getOpenFileName(this,"打开文件","C:\\Users\\zhangtao\\Desktop","(*.txt)");
//         qDebug() << str;

         //字体对话框
        bool flag;
        QFont font = QFontDialog::getFont(&flag,QFont("华文彩云",36));
        qDebug() << "字体：" << font.family().toUtf8().data() << " 字号 "<< font.pointSize() << " 是否加粗"<< font.bold() << " 是否倾斜"<<font.italic();
    });
}

MainWindow::~MainWindow()
{
    delete ui;
}

```
