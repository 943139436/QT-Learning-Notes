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
