# 一、QT简介

1. 跨平台图形界面引擎

2. 发展历史

1991年由奇趣科技开发。

3. 优点
3.1  跨平台
3.2 接口简单，容易上手
3.3 一定程度上简化了内存回收

4. 版本
4.1 商业版
4.2 开源版
5. 成功案例

5.1 Linux桌面环境KDE
5.2 谷歌地图
5.3 VLC多媒体播放器
5.4 WPS Office…

# 二、QT常用快捷键和命名规范
## 1.Windows版
1. 注释 ctrl + /
2. 运行 ctrl + r
3. 编译 ctrl + b
4. 查找 ctrl + f
5. 字体缩放 ctrl + 鼠标滚轮
6. 整行移动 ctrl + shift + ⬆️（上移）或⬇️（下移）
7. 自动对齐 ctrl + i
8. 帮助文档 F1
9. 同名之间的.h和.cpp切换 F4
## 2.Mac版
 将Windows版中的ctrl键换为Mac上的command键即可。
 
## 3.命名规范
1.  类名 首字母大写，单词和单词之间首字母大写
2. 函数名 变量名称 首字母小写，单词和单词之间首字母大写

# 三、创建一个QT程序
1. 点击创建项目后，选择项目路径以及给项目起名称（注意⚠️：**名称不能有中文和空格，路径不能有中文路径**）
2. 默认创建窗口类，myWidget：基类有**QWidget 、QMainWindow、QDialog**三种选择。
3. main函数
3.1 QApplication a 应用程序对象，**有且只有一个**
3.2 myWidget w;实例化窗口对象
3.3 w.show() 显示窗口
3.4 return a.exec()让应用程序对象进入到消息循环机制中，代码阻塞到当前行


# 四、添加按钮常用的API
1. 创建QPushButton * btn = new QPushButton
2. 设置父亲 setParent(this)
3. 设置文本 setText("文字")
4. 设置位置 move（宽，高）
5. 重新指定窗口大小 resize（宽，高）
6. 设置窗口标题 setWindowTitle()
7. 设置窗口固定大小 setFixedSize（）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011718035013.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjg3NzQyNg==,size_16,color_FFFFFF,t_70)

# 五、对象树
1. 当创建的对象在堆区时候，如果指定的父亲是QObject派生下来的类或者QObject子类派生下来的类，可以不用管理释放的操作，将对象会放入到对象树中。
2. 一定程度上简化了内存回收机制
# 六、信号和槽
1. 连接函数 ：connect(btn ,  &QPushButton::click , this , &QWidget::close );

       参数1  信号的发送者
       参数2  发送的信号（函数地址）
       参数3  信号的接受者
       参数4  处理的槽函数 （函数的地址）

2. 自定义信号和槽

   2.1 自定义信号：写到signals下；返回void；需要声明，**不需要实现**；可以有参数，可以重载。
 
    2.2 自定义槽函数：返回void；需要声明，也需要实现；可以有参数可以重载；写到public slot或者public或者全局函数下。
    
    2.3 触发自定义信号
    emit 自定义信号

    2.4 自定义信号和槽的重载时，需要函数指针明确指向函数的地址。
     QString 转成 char * : QString.ToUtf8().Data()
     
     2.5 断开信号：disconnect(btn ,  &QPushButton::click , this , &QWidget::close );

     2.6 拓展
     
     信号可以连接信号；
一个信号可以连接多个槽函数；
多个信号可以连接同一个槽函数；
**信号和槽函数的参数 必须类型一一对应**；
信号的参数个数 可以多于槽函数的参数个数；
信号槽可以断开连接  disconnect

# 七、Lambda表达式
1. []标识符  匿名函数 
 = 值传递
& 引用传递
2. () 参数 
3. {} 实现体
4. mutable 修饰 值传递变量 ，可以修改拷贝出的数据，改变不了本体
5. 返回值 []() ->int {
