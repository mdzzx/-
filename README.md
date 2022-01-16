#  QT笔记

###  常用接口

* ```Qt
  //窗口类
  setWindowTitle("第一个程序");  //设置窗口标题
  this->resize(500,400);   //设置窗口大小
  this->setFixedSize(500,400); //固定窗口大小，此后无法修改
  
  ```

* ```QT
  //按钮类
  #include <QPushButton>  //包含头文件
  //创建按钮
  QPushButton* btu = new QPushButton;
  btu->setParent(this);  //设置父亲(嵌入到指定父亲窗口)
  btu->resize(100,80);     //设置按钮尺寸
  btu->setText("开始游戏"); //设置按钮文本
  QFont font("楷体",20,10,1);//设置字体对象(字体，大小，宽度/加粗，是否倾斜)
  btu->setFont(font);//设置按钮文本字体
  btu->move(100,100);//设置按钮位置
  btu->setStyleSheet(.........); //设置样式表(了解)
  btu->show();   //显示
  ```


* ```QT
  //控制台输出
  #include <QtDebug> //包含头文件
  student::~student()
  {
      qDebug() <<"这是学生的析构函数";
  }
  ```

* QLabel* label;       //标签
      QProgressBar* bar;   //进度条
      QTextEdit* txtEdit;  //文本编辑框
      QSlider* slider;     //滑动条

    showNormal();       			//让窗口大小正常
    btn3->resize(100,80);             		//设置按钮尺寸
    btn3->setText("BUTTON");         		 //设置按钮文本
    label->setText("");    		//设置标签文本
    bar->setValue(80);                		//设置进度条进度
    txtEdit->show();
    txtEdit->setText("hello world come on baby");  //设置文本
    slider->setValue(90);              	               //设置滑动条

###  QT对象树

*  当创建的对象在堆区时，如果指定的父亲是QObject派生下来的或者QObject子类派生下来的
   类，可以不用管释放的操作，该对象会放入到对象树中，由QT自己管理其释放。
*  对象树一定程度上简化了内存回收机制.
   * QObject是以对象树的形式组织起来的.当创建QT对象时,需为其设置一个父对象，那么这个对象就会自动添加到其父对象的children()列表。当父列表析构时，这个列表中的所有对象也会被析构。
   * QWidget是能够在屏幕上显示的窗口父类。QWidget继承自QObject，因此也继承了对象树关系.添加QWidget为父亲的组件会自动添加到列表中自动析构.
   * 构造时自上而下，释放时自下而上，构造父亲再构造派生类，释放派生类再释放父类.析构时先看有没有孩子 有孩子则去析构孩子以此类推 找到最下层孩子进行释放).

![20200408194225588](C:\Users\zyy\Downloads\20200408194225588.png)

###  QT信号槽

* 信号槽是 Qt 框架引以为豪的机制之一。所谓信号槽，实际就是观察者模式。当某个事件发生之后，比如，按钮检测到自己被点击了一下，它就会发出一个信号（signal）。这种发出是没有目的的，类似广播。如果有对象对这个信号感兴趣，它就会使用连接（connect）函数，意思是，将想要处理的信号和己的一个函数（称为槽（slot））绑定来处理这个信号。也就是说，当信号发出时，被连接的槽函数会自动被回调。

* signals:信号-函数声明（无需实现）  系统大多数类都内置了信号   开发者也可以自定义信号

* slot:槽-本质就是个函数  一般是类的成员函数  必须有声明且有实现  系统大多数类也内置了槽   开发者也可以自定义槽

* 这就类似观察者模式：当发生了感兴趣的事件，某一个操作就会被自动触发。这里提一句，Qt 的信号槽使用了额外的处理来实现，并不是 GoF 经典的观察者模式的实现方式。）

* 信号和槽是Qt特有的信息传输机制，是Qt设计程序的重要基础，它可以让互不干扰的对象建立一种联系。

* 槽的本质是类的成员函数，其参数可以是任意类型的。和普通C++成员函数几乎没有区别，它可以是虚函数；也可以被重载；可以是公有的、保护的、私有的、也可以被其他C++成员函数调用。唯一区别的是：槽可以与信号连接在一起，每当和槽连接的信号被发射的时候，就会调用这个槽。

* QT存在大量的信号 和 槽函数是通过connect函数建立起两个者的关系的连接函数connect
  参数1 信号发送者 
  参数2 发送的信号    （取函数地址）
  参数3 信号的接收者
  参数4 处理的槽函数  （取函数地址）

* 例

  ```QT
  //最大化按钮
  auto btnMax = new QPushButton("最大化",this);
  //点击按钮后窗口最大化
  //(被观察者：btnMax,信号：clicked，观察者：this（当前窗口）,槽：showMaximized)
  //建立连接
  connect(btnMax,&QPushButton::clicked,this,&QWidget::showMaximized);
  
  auto btnMin = new QPushButton("最小化",this);
  btnMin->move(100,0);
  connect(btnMin,&QPushButton::clicked,this,&QWidget::showMinimized);
  
  auto btnNormal = new QPushButton("正常化",this);
  btnNormal->move(200,0);
  connect(btnNormal,&QPushButton::clicked,this,&QWidget::showNormal);
  ```

###  Lamda表达式

* C++11新特性  匿名函数表达
* 语法格式： [可访问的外部变量](参数表)->返回值
* [可访问外部变量]（参数表）->返回值{函数体}
* [] 表示不使用外部变量
  [a,b,c] 表示使用外部变量a,b,c
  [=] 表示使用值传递方式使用外部可访问的所有变量
  [&] 表示使用引用传递方式使用外部可访问的所有变量
* 通过发送信号  调用lamda表达式方式
  依然使用连接函数
  connect(信号发送者，信号，lamda表达式)；

```QT
 //设置一个按钮
    //鼠标按下打印 被打了
    //鼠标抬起打印 反抗了
    auto btn = new QPushButton("老鼠",this);
    //当点击按钮时调用Lamda表达式
    connect(btn,&QPushButton::pressed,[]()->void{
        qDebug()<<"老鼠被打了!";
    });

    connect(btn,&QPushButton::released,[]()->void{
        qDebug()<<"老鼠反抗了!";
    });
```

