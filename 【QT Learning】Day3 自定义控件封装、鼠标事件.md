# 一、自定义控件封装
1. add new-> Qt -> 设计师界面类（.h .cpp .ui）
2. .ui中设计QSpinBox和QSlider两个控件
3. Widget中使用自定义控件，拖拽一个Widget（右键提升为->添加—>提升）
4. 实现功能：改变数字、滑动条跟着移动、信号槽监听。
5. 提供getNum 和 setNum 对外接口
6. 测试接口
```
#include "smallwidget.h"
#include "ui_smallwidget.h"

SmallWidget::SmallWidget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::SmallWidget)
{
    ui->setupUi(this);

    //QSpinBox移动 QSlider跟着移动

    void(QSpinBox:: * spSignal )(int) = &QSpinBox::valueChanged;
    connect(ui->spinBox , spSignal , ui->horizontalSlider , &QSlider::setValue);

    //QSlider滑动  QSpinBox数字跟着改变
    connect(ui->horizontalSlider, &QSlider::valueChanged,ui->spinBox,&QSpinBox::setValue);
}

//设置数字
void SmallWidget::setNum(int num)
{
    ui->spinBox->setValue(num);

}

//获取数字
int SmallWidget::getNum()
{
    return ui->spinBox->value();
}

SmallWidget::~SmallWidget()
{
    delete ui;
}

```
<br>[视频🔗](https://www.bilibili.com/video/BV1g4411H78N?p=30)

# 二、鼠标事件
1. 鼠标进入事件 enterEvent
2. 鼠标离开事件 leaveEvent
3. 鼠标按下 mousePressEvent(QMouseEvent ev)
4. 鼠标释放 mouseReleaseEvent
5. 鼠标移动 mouseMoveEvent
6. ev->x() ev->y() 获取鼠标xy坐标
7. ev->button() 可以判断所有按键 Qt::LeftButton  Qt::RightButton
8. ev->buttons()判断组合按键  判断move时候的左右键  结合 & 操作符
9. 格式化字符串  QString( “ %1  %2 ” ).arg( 111 ).arg(222)
10. 设置鼠标追踪    setMouseTracking(true);
```C++
#include "mylabel.h"
#include <QDebug>
#include <QMouseEvent>
myLabel::myLabel(QWidget *parent) : QLabel(parent)
{
    //设置鼠标追踪状态
    //setMouseTracking(true);
}


//鼠标进入事件
void myLabel::enterEvent(QEvent *event)
{
   // qDebug() << "鼠标进入了";

}

//鼠标离开事件
void myLabel::leaveEvent(QEvent *)
{
   // qDebug() << "鼠标离开了";
}

//鼠标按下
void myLabel::mousePressEvent(QMouseEvent *ev)
{

    //当鼠标左键按下  提示信息
//    if( ev->button() ==  Qt::LeftButton)
//    {
        QString str = QString( "鼠标按下了 x = %1   y = %2  globalX = %3 globalY = %4 " ).arg(ev->x()).arg(ev->y()).arg(ev->globalX()).arg(ev->globalY());
        qDebug() << str;
//    }
}

//鼠标释放
void myLabel::mouseReleaseEvent(QMouseEvent *ev)
{

//    if( ev->button() ==  Qt::LeftButton)
//    {
    QString str = QString( "鼠标释放了 x = %1   y = %2  globalX = %3 globalY = %4 " ).arg(ev->x()).arg(ev->y()).arg(ev->globalX()).arg(ev->globalY());

    qDebug() << str;
//    }

}

//鼠标移动
void myLabel::mouseMoveEvent(QMouseEvent *ev)
{
    if( ev->buttons() &   Qt::LeftButton )
    {
    QString str = QString( "鼠标移动了 x = %1   y = %2  globalX = %3 globalY = %4 " ).arg(ev->x()).arg(ev->y()).arg(ev->globalX()).arg(ev->globalY());

    qDebug() << str;
   }
}


bool myLabel::event(QEvent *e)
{
    //如果是鼠标按下 ，在event事件分发中做拦截操作
    if(e->type() == QEvent::MouseButtonPress)
    {
        QMouseEvent * ev  = static_cast<QMouseEvent *>(e);
        QString str = QString( "Event函数中：：鼠标按下了 x = %1   y = %2  globalX = %3 globalY = %4 " ).arg(ev->x()).arg(ev->y()).arg(ev->globalX()).arg(ev->globalY());
        qDebug() << str;


        return true; //true代表用户自己处理这个事件，不向下分发
    }


    //其他事件 交给父类处理  默认处理
    return QLabel::event(e);
}


```
