---
title: Qt5
author: hero576
date: 2020-07-17 21:21:09
tags:
  - C/C++
categories:
  - programme
---
> qt界面开发
<!--more-->


# 简介
- [官方网址](https://www.qt.io)
- 5.9是qt长期维护的版本

## 安装
- 安装qt程序会自动安装qt creater，插件根据使用情况选择，下载qt vs addin，方便qt在vs的开发。

- linux环境配置
  - sudo apt-get install -y g++ make
  - sudo apt-get install -y libgl1-mesa-dev
  - 下载qt-source-linux-x64.run，并执行安装

## 调式
- windows需要下载并安装调式工具：windows10sdk，选择Debugging Tools for Windows

![安装windows调式工具](/images/pasted-58.png)

- 安装完成后，在`c:\Program Files(x86)\Windows Kits\10\Debuggers\x86`文件夹下，有一个`cdb.exe`的调式工具

- 配置Qt调式文件路径，如果调式工具是后安装的，这个需要手动配置。

![配置Qt调式](/images/pasted-59.png)

- 调试快捷键

|按键|说明|
|-|-|
|F9|设置断点|
|F5|执行程序，知道运行到下一个断点|
|F10|单步执行，不进入函数|
|F11|单步执行，遇到函数则进入|

# Qt项目介绍
## 创建项目
- 创建项目时有如下模板：Application、Library、其他项目、Non Qt project、Import Project

### Application模板
- 用于创建窗体应用

|项目类型|说明|
|-|-|
|Qt Widgets Application|带有窗体的qt项目|
|Qt Console Application||
|Qt Quick Application||
|Qt Quick Controls 2 Application||

- 窗体程序都继承与基类widgets，还有其派生类，对应不同窗体程序。
|widgets基类及其派生|说明|
|-|-|
|QMainWindow|主窗口，带菜单栏|
|QWidget|简单窗体|
|QDialog|对话框|

### Library模板
- 用于链接库程序

|项目类型|说明|
|-|-|
|C++库|分为了共享库(win:dll文件，linux:so文件)和静态库|
|Qt Quick Extension Plugin||
|Qt creator插件||


## 生成文件
- 源码文件
![生成的源码文件目录](/images/pasted-57.png)

|文件名称|说明|
|-|-|
|pro文件|用户项目配置，生成项目，编译文件|
|headers|存放头文件
|sources|存放main.cpp和其他函数实现文件|
|forms|存放界面文件，可以用qt界面编辑器打开编辑，里面存的是xml|
|pro.user文件|存放用户的qt配置信息，配置的编译环境等信息，当更换其他电脑时，这个文件需要删除，在新的环境下重新生成|

- 编译文件
  - makefile文件：qt会自动生成makefile文件，指定了项目配置
  - ui_widget：将源码xml文件转成cpp文件
  - debug/release：qt编译连接后生成的obj、EXE、lib、cpp等文件

## 项目配置
### Qt配置库和头文件
#### 库文件准备
- qt引用其他的库文件的配置，比如opencv。
- 头文件：`include`一般放在与项目同级别文件夹或者项目目录的src下；
- 库文件：`*.lib`文件放在项目路径的lib文件夹下；
- 动态链接库：`*.dll`文件放在项目路径的bin文件夹下；

#### 引用库的配置
- 在qt项目右键，选择添加库，引用外部库，设置库文件路径；

![添加库](/images/pasted-60.png)

- 添加完成后，执行qmake，生成对应的makefile

![执行qmake](/images/pasted-61.png)

- 指定构建目录

![配置build配置](/images/pasted-63.png)

- 配置运行配置，路径(working directory)，让可执行文件找到对应的dll文件

![配置run配置](/images/pasted-62.png)

### 新增ui和qrc
- 项目右键新建，可以选择以下ui类型

|ui文件|说明|
|-|-|
|Qt Item Model||
|Qt 设计师界面类|自动生成绑定好.h和.cpp的ui文件|
|Qt Designer From|仅生成ui文件，需要自己手动绑定|
|Qt Resource File||
|QML File||
|QtQuick UI File||
|JS File||

### VS+Qt配置项目
#### 输出、调试、库、头文件设置
- 右键项目，点击属性

![项目属性配置](/images/pasted-64.png)

- 常规：输出路径、平台命令集、
- 调试：工作目录、环境
- C/C++：常规-->附加包含目录
- 连接器：常规-->输出文件、附加库目录、输入-->附加依赖项、系统-->子系统（子系统中：控制台表示从main为入口，显示黑窗口；窗口表示从winmain为入口，没有）

## QMake
### QT编译经历的步骤
- 编译pro生成makefile
- jom或者make编译makefile
  - 生成界面源码：`uic.exe widgget.ui -o ui_widget.h`
  - 生成信号槽代码：`moc.exe widget.h moc_widget.cpp`

### 手动创建pro
#### 创建pro
- 在空的项目文件目录中创建pro文件，写入下面的配置
```javascript
//添加QT的库
QT += widgets core opencv ...
QT -= console
//头文件的引用，默认是相对于输出路径，$$PWD代表pro所在文件路径
INCLUDEPATH += ../../include
INCLUDEPATH += $$PWD../../include
//库文件的引用
LIBS += -L"../../lib" -lopencv_world320
//指定生成目录
DESTDIR += ../../bin
//指定生成可执行程序的名字
TARGET = testq
//指定用到的源码，根据需求添加文件名即可
SOURCES += main.cpp \
        preson.h \
        person.cpp
//添加控制台
CONFIG += console
```

- 使用cmd，执行`qmake -o makefile testqmake.pro`，生成makefile
  - 如果提示缺少环境变量，可以在`c:\Program File(x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat`中找到环境变量的配置。

- 编译makefile：`jom /f makefile.Debug`，在debug文件夹中就有编译文件了。

- 上述语句可以写进批处理文件中
```bat
call "c:\Program File(x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"
qmake -o makefile testqmake.pro
jom /f makefile.Debug
pause
```

- 在linux中，批处理文件添加执行权限：`chmod +x make.sh`
```bash
export PATH=/opt/Qt5.9.0/5.9/gcc_64/bin:$PATH
qmake -o makefile testqmake.pro
make
```

#### 配置qmake和jom执行路径
- 添加环境变量`c:\qt\qt5.9.0\5.9\msvc2015\bin`和`c:\qt\qt5.9.0\Tools\QtCreator\bin`

#### 导入vs开发环境
- 方法一：`qmake -tp vc testqmake.pro`
- 方法二：使用vs插件导入



## 创建动态库和静态库
- qt创建库的配置是`TEMPLATE=lib`，如果是静态库，还要加上配置`CONFIG += staticlib`

### qtcreator创建
- 新建项目，选择Library-->c++库-->共享库，并设置类名：`testlib`
- pro文件会自动生成
```javascript
//定义宏，用以区分是库函数调用，还是执行调用
DEFINES += TESTLIB_LIBRARY
```

#### pro手动创建
- 手动创建pro文件，testlib.h、testlib.cpp文件
- pro文件配置
```javascript
SOURCES += testlib.cpp
HEADERS += testlib.h
//生成文件名称
TARGET=libdll
//动态库，如果是静态库，还需CONFIG += staticlib
TEMPLATE=lib
CONFIG += staticlib
//打开控制台，用以输出cout
CONFIG += console
//定义宏，表示项目是库项目
DEFINES += TESTLIB_LIBRARY
//输出路径
DESTDIR = ../../lib
DLLDESTDIR = ../../bin
```

### 调用动态库
- 引用头文件：`#include <testlib.h>`
- 配置库路径：有两种方法，一个是在项目右键中选择添加库的方式配置，再一种是配置pro文件
```javascript
//配置库头文件路径
INCLUDEPATH += ../../include
//配置库路径
LIBS += -L../../lib -llibdll
```

## debug和release编译设置
- pro配置：`CONFIG += debug_and_release`，这个是默认设置，即生成debug也生成release。
- 另一种配置写法
```javascript
CONFIG(debug,debug|release){
//生成文件名称
  TARGET = testdll_d // debug_binary
}else{
//生成文件名称
  TARGET = testdll   // release_binary
}
```


## 跨平台编译
- Qt支持多种平台的开发，包括：win32、linux、mac、unix
- 测试是什么环境，可以在pro文件添加语句：`message($$QMAKESPEC)`，执行QMake，就可以显示了
- 查看支持的环境配置列表：`$QTPATH/Qt5.9.0/5.9/clang_64/mkspecs`
- pro配置：
```javascript
//方法一：
win32:INCLUDEPATH += C:/mylibs/headers
//方法二：
win32{
  INCLUDEPATH += C:/mylibs/headers
}
!linux{
  message("not linux")
}
win32|linux{
  message("win or linux")
}
```

# Qt信号槽
- 信号槽是Qt中最关键的技术，作为`c++`程序，最关注的就是性能，对于信号槽的理解，对提升性能有很大的帮助。
- 槽是一个线程，排队执行信号，如果遇到时间复杂度高的任务，应该使用多线程处理。

## 信号槽的原理
- 类似windows的消息机制：在系统注册回调函数，指定类型消息调用，由主循环一直调用。
- 信号槽的区别是，不关心消息的类型，以及由谁接收。信号可以发送给不同的接收者。

### 组成
- 主要包含：信号函数（只发送，不需要知道接收者），槽函数（普通函数，只接收不管通信），QObject将两者绑定。

### 过程
- 绑定信号函数和槽函数
- 调用信号函数（将信号写入队列）
- 主线程从队列中获取信号

![信号槽关系图](/images/pasted-65.png)


### 代码实现
- 主循环的建立
```c
// 入口程序建立主循环对象
QApplication a(arg,argv);
// 在exec中，执行循环并处理相关调用，直至获取退出的操作
return a.exec();
```

- 信号槽的绑定
  - 设计器添加信号槽：拖动、添加，这里添加的是内部的信号槽函数，生成在ui的xml中
  - 自定义实现的槽函数：当我们对界面改变时，而不想占用主线程的线程，防止一些异常的发生，就需要自己定义。在其他线程绑定槽函数手动添加信号槽。
    - Q_OBJECT：在类中包含Q_OBJECT的宏，就会生成对应的moc代码
    - 手动创建信号 signals
    - 手动创建槽 public slots
    - 两者绑定：
      - 方式一：设计器将槽函数绑定，在ui自动生成的cpp中加入自己定义的函数`QObject::connect(pushBtn,SIGNAL(clicked()),testsignalClass,SLOT(ViewSlot))`
      - 方式二：手动绑定。在类的构造器中添加：`connect(ui.testBtn,SIGNAL(clicked()),this,SLOT(TestSlot()))`
```c
class testsignal :public QWidget{
  Q_OBJECT
public:
  testsignal(QWidget *p = Q_NULLPTR);
signals:
  void ViewSig();  //只需声明，信号函数会自动做定义
public slots:
  void ViewSlot(); // 槽函数需要自己手动完成定义
  void TestSlot();
}
```

# QThread
- Qt实现了线程类，使用方法相对简单，只需要继承`QThread`类，重写`void run(){}`方法，处理线程功能即可。调用启动方法`void start(){}`。终止`void stop(){}`，不建议使用，线程手动退出可能会造成不可预知的错误，尽量使空间正常释放。一般是在run中的循环一次结束时判断是否需要退出，防止数据泄露。


## 利用信号执行槽函数
```c
#include <QtWidgets/QApplication>
#include <QWidget>
#include <QThread>
class XThread:public QThread{
public:
  XThread();~XThread();
  testQWidget* w = NULL;
  void run(){
    w->Hide(); // 执行信号
  };
};
class testQWidget{
  Q_OBJECT
public:
  testQWidget(){connect(this,SIGNAL(Hide),this,SLOT(hide()));};virtual ~testQWidget(){};
signals:
  void Hide(){};
}
int main(int argc, char *argv[]){
  QApplication a(argc,argv);
  testQWidget w;
  w.show();
  XThread xt;
  xt.w = &w;
  xt.start();
  return a.exec();
}
```

## 在QThread中添加信号，绑定到QWidget
```c
#include <QtWidgets/QApplication>
#include <QWidget>
#include <QThread>
class XThread:public QThread{
  Q_OBJECT
public:
  XThread(){};
  ~XThread(){terminate();}; //在项目退出时，保证线程退出，这个是不安全的方法
  void run(){
    msleep(1000);
    Move(0,0);
  };
signals:
  void Move(int x,int y);  //只需声明，无需实现
};
class testQWidget{
  Q_OBJECT
public:
  testQWidget(){};virtual ~testQWidget(){};
public slots:
  void move(int x,int y){QWidget::move(x,y);}
}
int main(int argc, char *argv[]){
  QApplication a(argc,argv);
  w.show();
  XThread xt;
  QObject::connect(&xt,SIGNAL(Move(int,int)),&w,SLOT(move(int,int)))
  xt.w = &w;
  xt.start();
  return a.exec();
}
```

# Qt核心窗口类QWidget
- QWidget是所有用户界面对象的基类
- 窗口部件接收鼠标、键盘等事件
- 屏幕上绘制自己
- 父子关系有相对坐标

## 手动创建QWidget对象
```c
QWidget w;
w.show();//显示包含子窗口，是一个槽函数
w.setWindowTitle("my widget");  //设置title
w.hide();//隐藏包含子窗口，槽函数，资源并未释放
```

## 窗口的属性
### 设置窗口坐标和大小

|函数|说明|
|-|-|
|`QRect geometry()`|获取坐标和宽高|
|`setGeometry(x,y,width,height)`|设置坐标和宽高|
|`x();y();width();height()`|获取坐标和宽高|
|`move(x,y);resize(width,height)`|设置坐标和宽高|


### 设置窗口最大化、最小化、全屏、正常

|函数|说明|
|-|-|
|`setWindowState(Qt::WindowMaximized)`|设置窗口状态|
|`showMinimized()`|设置|
|`showMaximized()`|设置|
|`showNormal()`|设置|
|`showFullScreen()`|设置|

|Qt::WindowMaximized|说明|
|-|-|
|`WindowMinimized`|窗口最小化|
|`WindowMaximized`|窗口最大化|
|`WindowNoState`|窗口无状态|
|`WindowFullScreen`|窗口全屏|
|`WindowActive`|活动状态|

### 定制窗口类型

|函数|说明|
|-|-|
|`CustomizeWindowHint`|自定义|
|`setWindowFlags`|设置类型，指定多个值|
|`setWindowFlag(Qt::WindowCloseButtonHint,flase)`|设置类型|

|窗口按钮|说明|
|-|-|
|`Qt::WindowCloseButtonHint`|关闭按钮|
|`Qt::WindowWindowMinimizeHint`|最小化按钮|
|`Qt::WindowWindowMaximizeHint`|最大化按钮|
|`Qt::FramelessWindowHint`|无边框的窗体|
|`Qt::WindowTitleHint|Qt::CustomWindowHint`|标题栏保留，取出所有按钮|
|`Qt::CustomWindowHint`|全部自定义|

# Qt文本处理
## 字符集
### 编码
- ASCii：7位字符集，128个字符，最高位作为奇偶校验，在转码传输时会被丢弃。
- ISO-8859-1扩展ASCii：128-255拉丁
- ANSI标准：没够国家标准学会，多字节字符集（MBCS），0-127依旧是1个字节代表1个字符，2个字节来表示1个字符
- GB2312编码：ANSI 6763常用汉字，两个大于127的字符表示一个汉字
- GBK编码：在GB2312的基础上扩展汉字21003个
- UTF-8编码：变长的编码方式，单字节与ASCii相同，对于n个字节，首字节前n位为1，n+1为0，后面字节前两位都为10。

![UTF-8编码](/images/pasted-66.png)

- UTF-16编码：两个字节或四个字节
- UTF-32编码：四个字节

### 字节序BOM
- LE(littleEndian)：小字节字节序，地位在前：`0x001A23`--`23 1A 00`
- BE(bigEndian)：大字节字节序，地位在前：`0x001A23`--`00 1A 23`
- BOM字节序标志头：

|字节序标志头|字节序|
|-|-|
|`FE FF`|BE|
|`FF FE`|LE|

## QString
- 16-bit Qchar ushort Unicode 4.0

### 常用功能

|函数|说明|
|-|-|
|`==`|相等判断|
|`isNull()`|是否null|
|`isEmpty()`|是否为空|
|`+=`|字符拼接|
|`append(char*)`|添加字符串|
|`%1 %2 arg()`|格式化字符串，arg(int,最小宽度,进制数)|
|`QString::number(int)`|数字转字符串|
|`QString.toInt()`|字符串转数字，注意不要转换浮点型，返回为0|
|`QString.toDouble()`|字符串转数字|
|`QChar.toLatin1()`|字符串转数字|
|`for(QString::iterator itr=itstr.begin();itr!=itstr.end();itr++)
`<br>`for(int i=0;i<itstr.size();i++)`|字符串遍历，返回QChar类型|
|`itstr.indexOf(string,[startIndex])`|字符串查找，找不到返回-1|
|`itstr.lastIndexOf(string)`|字符串查找|
|`itstr.indexOf(QRegExp())`|字符串正则表达式查找|
|`itstr.chop(int)`|字符串结去除尾部n个字符，直接改变原对象|
|`itstr.left(int)`|返回从字符串左边开始的n个字符|
|`itstr.right(int)`|返回从字符串右边开始的n个字符，从最右边开始|
|`itstr.replace(string,string)`|字符串替换，直接改变原对象|
|`itstr.replace(QRegExp(),string)`|字符串正则替换|
|`QStringList itstr.split(sep)`|字符串切割，也支持正则表达式|


```c
#include<QDebug>
QString str;
if (str.isNull()){qDebug()<<"str is null"<<endl;};
if (str.isEmpty()){qDebug()<<"str is empty"<<endl;};
str = QString("name=%1 age=%2").arg("xxx").arg(12);
```

## QT中文乱码问题
- 默认字符集设置
- 文件字符集格式vs qtcreator设置
- 字符集转换QStringLiteral
- 通过预编译宏，设置原文件是gbk的编码转utf8：`#pragma execution_character_set("UTF-8")`

### 字符集设置

|函数|说明|
|-|-|
|`codec = QTextCodec::codecForName("UTF-8");`|设置本地编码格式|
|`QTextCodec::setCodecForLocale(codec);`|本地处理的字符集|
|`QTextCodec::availableCodecs();`|查询支持哪些格式字符集|
|`QString::fromLocal9Bit(string);`|根据本地编码方式存入QString，默认是GBK|
|`QString::toStdString();`|把QString转为char* 或者string|
|`QString::toStdWString();`|把QString转为宽字符集，以支持win64，例如：L"1234"|
|`QString::toLocal9Bit();`|把QString转为本地编码格式|
|`QStringLiteral("中文")`|将多字节编码转为UTF-8|

```c
#include <QDebug>
#include <QMessageBox>
QString str = "中文";
qDebug<<str; //此时打印为乱码
str = QStringLiteral("中文"); //将多字节编码转为UTF-8
qDebug<<str; //此时正常输出中文
QMessageBox::information(0,"title",str);
```

# Qt控件
## QLabel
- 继承于QWidget

### 显示文字
```c
ui.label->setText(QString("String"))
```

### 样式设置
- 设计器设置：右击label空间，选择“改变样式表”，选择添加颜色等操作。在属性编辑器中选择对齐方式。

- 代码设置：
```c
label.setStyleSheet(QString::fromUtf8("color:rgb(255,0,0);"));
```


### 显示图片
- 在属性编辑器中，选择pixmap，选择图片进行显示
- 代码设置
```c
label.setPixmap(QPixmap(QString::fromUtf8(":/项目名/Resources/test.jpg")));
```

- 通过样式设置
```css
backgroud-images:url(:/项目名/Resources/test.jpg);
border-images:url(:/项目名/Resources/test.jpg);
images:url(:/项目名/Resources/test.jpg);
```

### 播放gif动画
```c
QMovie *mov = new QMovie("test.gif");
lab.setMovie(mov)
mov->start();
```


### 创建QLabel
```c
QLabel *lab = new QLabel(this); //this表示属于哪个窗体的，如果为null则单独显示一个label
lab->setGeometry(0,0,200,200);
lab->setStyleSheet("background-color:rgb(255,0,0)");
lab.show();
```

### QLabel的富文本模式
- 前面QLabel都是PlainText模式，支持\n换行，QLabel也支持富文本RichText模式，支持html。
- 在属性编辑器中，点击textFormat选择RichText，双击即可用html编辑

```c
lab->setTextFormat(Qt::RichText);
```

#### 链接事件
- QLabel链接事件绑定了两个槽函数

|函数|说明|
|-|-|
|void linkActivated(const QString &link)|点击事件|
|void linkHovered(const QString &link)|悬停事件|

```c
//---------类中定义槽-------------
public slots:
  void Act(QString url);
  void Hov(QString url);
//---------ui中绑定-------------
connect(ui.label,SIGNAL(linkActivated()),this,SLOT(Act()));
connect(ui.label,SIGNAL(linkHovered()),this,SLOT(Hov()));
```

### QLabel选择和编辑

|函数|说明|
|-|-|
|setTextInteractionFlags|设置是否可以选择、编辑|
|selectedText|获取选择的文本|
|setSelection(startIndex,length)|设置选了什么文本|

|Flags|说明|
|-|-|
|NoTextInteraction||
|TextSelectableByMouse||
|TextSelectableByKeyboard||
|LinksAccessibleByMouse||
|LinksAccessibleByKeyboard||
|TextEditable||


## QPushButton
### 事件设置

|事件信号|说明|
|-|-|
|click()||
|click(bool) |是否选中|
|pressed()||
|released()||

### 快捷键设置
- 在按钮添加快捷键，有两种方法，在按钮名前加`&`符号，那么按钮名的首字母+`Alt`就是快捷键了。第二种方法就是代码设置

|事件信号|说明|
|-|-|
|setShortcut||
|setShortcut(tr("Alt+F7"))|`ui->shortcut->setShortcut(tr("X"));`|
|setShortcut(tr("Ctrl+x,Ctrl+c"))|组合按键|

### 样式设置
- 样式设置和label设置差不多，背景图片设置中有flat设置，可以取消按钮的凹凸效果，样式可以自己更改。
- 圆角边框，渐变援交光泽按钮
```c
QPushButton::!hover{
  background-color:qradialgradient(spread:reflact......)
}
QPushButton::hover{
  border-radius:10px;
  background-color:qradialgradient(spread:reflact......)
}
```

## QLineEdit
### 属性方法槽 

|事件信号|说明|
|-|-|
|setText()| 设置文本，不发信号|
|text()| 获取文本|
|setPlaceholderText()|设置提示文字|
|setClearButtonEnabled|默认关闭清空内容|
|setReadOnly|设置是否只读|
|setMaxLength|默认长度3万多|
|undo()|撤销|
|redo()|恢复|

### 信号事件

|事件信号|说明|
|-|-|
|editingFinished |按下回车或者焦点移开，触发事件|
|returnPressed |按下回车|
|textChanged|用户输入或者setText()调用，激发事件|
|textEdited|只有用户手动修改激发事件|

### 输入验证（正则）
- 格式掩码

|掩码|说明|
|-|-|
|`setInputMask("000.000.000.000;_")`|格式化字符串，后面的`;_`代表不填写时默认显示`_`|

![掩码格式](/images/pasted-67.png)

### 复杂验证QValidator、QRegExpValidator
```c
//整型
QIntValidator *ival = new QIntValidator();
ival->setRange(10,1000);
ui.iedit->setValidator(ival);
//浮点型
//默认科学计数法，会在输入完校验
QDoubleValidator *dval = new QDoubleValidator();
dval->setRange(1,199); //结合写法：dval->setRange(1,199,3); 
dval->setDecimal(3);
//更改为普通计数法
dval->setNotation(QDoubleValidator::StandardNotation);
ui.dedit->setValidator(ival);
//正则
QRegExpValidator *pReg new QRegExpValidator(QRegExp("\d+"),this);
ui.email->setVlidator(pReg);
//-------在信号函数校验--------
const QValidator *v = ui.email->validator();
int pos = 0;
//返回：Invalid 0；Intermediate 1；Acceptable 2
if v->validate(ui.email->text(),pos)!=QValidator::Acceptable{
  ui.label->setText(QString::fromLocal8bit("格式不正确"));
}
```


### 显示样式

|函数|说明|
|-|-|
|`setEchoMode(QLineEdid::Normal)`||

|Flags|说明|
|-|-|
|Normal||
|NoEcho||
|Password||
|PasswordEchoOnEdit||

## QObject
### 遍历所有QObject子节点
- 遍历子节点，获取节点对象名称`QObjectList children()`、`objectName`，
- 转换子节点-获取节点对象类型

|函数|说明|
|-|-|
|`QObjectList cs = this->children()`|遍历子节点|
|`cs[1]->objectName`|获取节点对象名称|
|`QMetaObject *mobj = QObject->metaObject()`|获取节点对象类型的元数据|
|`metaObject()->className`|获取类的名称|
|`qobject_cast<QCheckBox*>(cb)`|转换子节点，转换失败返回NULL|


## QLayout
- 布局器依赖于QWidget

|布局|说明|
|-|-|
|Vertical Layout|垂直布局|
|Horizontal Layout|水平布局|
|Grid Layout|网格布局|
|From Layout|表单布局|

|函数|说明|
|-|-|
|`QSize sizeHint()`|推荐尺寸只能重载修改|
|`QSizePolicy::PolicyFlag`|尺寸策略|
|`setContentsMargins`|设置边距|
|`setMinimumSize(width,height)`|设置最小尺寸|
|`setMaximumSize(width,height)`|设置最大尺寸|
|`setSpacing(width)`|设置间距|
|`QGridLayout::setVerticalSpacing(width)`|设置水平间距|
|`QGridLayout::setHorizontalSpacing(width)`|设置垂直间距|
|`QFormLayout::addRow(tr("&name"),nameLineEdit)`||
|`QFormLayout::insertRow(row,tr("&name"),nameLineEdit)`||
|`QLayoutItem *itemAt(row,ItemRole)`||
|`QLayoutItem::widget()`|转为widget，否则返回null|


|PolicyFlag|说明|
|-|-|
|GrowFlag |必要时超过推荐|
|ExpandFlag|尽可能扩展|
|ShrinkFlag|必要时小于推荐|
|IgnoreFlag|缺省大小忽略|

### 手动添加QLayout
- 添加QVBoxLayout/QHBoxLayout
```c
QWidget *w = new QWidget;
QVBoxLayout *lay = new QVBoxLayout(w); //绑定方式一
//w->setLayout(lay)绑定方式二
QPushButton *btn = new QPushButton("button");
lay->addWidget(btn1);
w.show()
```

- 添加QGridLayout
```c
lay->addWidget(btn,int row,int column,Qt::Alignment);
```

- 添加QFromLayout：插入项目addRow和insertRow、遍历项目、表单验证
```c
```


## QCheckBox
- 选择项，继承于QAbstractButton，支持快捷键

|函数|说明|
|-|-|
|text()|获取文本信息|
|setText()|设置文本信息|
|bool isChecked() const|状态|
|void setChecked(bool)|设置状态|
|void toggled()|设置状态|
|setAutoExclusive()|设置排他，同一级别的，可以放在不同的widget中，相互不影响|

|事件|说明|
|-|-|
|clicked(bool)|点击事件|
|toggled(bool)|变化触发|

### QButtonGroup
- QCheckBox可以放入组中管理，加入组中默认设为单选，可以设置是否排他

|函数|说明|
|-|-|
|setEclusive(bool)|设置是否单选|

|事件|说明|
|-|-|
|`void buttonClicked(QAbstractButton *button)`||
|`void buttonClicked(int id)`||
|`void buttonPressed(QAbstractButton *button)`||
|`void buttonPressed(int id)`||
|`void buttonReleased(QAbstractButton *button)`||
|`void buttonReleased(int id)`||
|`void buttonToggled(QAbstractButton *button, bool checked)`||
|`void buttonToggled(int id,bool checked)`||

```c
QButtonGroup *group = new QButtonGroup(this);
group->addButton(ui.checkBox1);
group->addButton(ui.checkBox2);
group->setEclusive(false);
//绑定手动点击事件
QObject::connect(group,SIGNAL(buttonClicked(QAbstractButton*)),this,SLOT(Test(QAbstractButton *)));
```

### QRadioButton
- 类似于QCheckBox，相关属性、函数基本相同
- 默认是单选的

## QComboBox
- 下拉列表，还可以设置选项的图标

|函数|说明|
|-|-|
|`void addItem(const QString &text,const QVariant &userData = QVariant())`|新增数据，默认在结尾插入|
|`void addItem(const QIcon &icon,const QString &text,const QVariant &userData=QVariant())`|新增数据以及图标|
|`void addItems(const QStringList &texts)`|新增一组数据|
|`void insertItems(int index,const QStringList &list)`|根据索引插入|
|`void insertItem(int index,const QString &text,const QVariant &userData=QVariant())`||
|`void insertItem(int index,const QIcon &icon,const QString &text,const QVariant &userData=QVariant())`||
|`void removeItem(int index)`|删除|
|`[slot]void QComboBox::clearEditText()`|清空|
|`[slot]void QComboBox::clear()`|清空|
|`QVariant itemText(int index)`|获取文字|
|`QVariant itemData(int index)`|获取传入的userData|


|事件|说明|
|-|-|
|`void activated(int index)(const QString &text)`|用户选中时触发，支持两种输入|
|`void currentIndex(int index)(const QString &text)`|代码和用户选中都会触发|
|`void currentTextChanged(const QString &text)`|文字变化触发，前提是可编辑模式|
|`void editTextChanged(const QString &text)`|只有可编辑时编辑触发|
|`void highlighted(int index)(const QString &text)`|未选中，鼠标移动到选项高亮时触发|


- QVariant用户自定义类型

|函数|说明|
|-|-|
|`void setValue(const T &value)`||
|`Q_DECLARE_METATYPE(Test)`|注册类型|

```c
struct STU{
  int a;
  int b;
}
Q_DECLARE_METATYPE(STU);
QVariant var:
struct STU my;
var.setValue(my);
ui.comboBox->addItem("Item001",var);
QVariant var2 = ui.comboBox->itemData(0);
a = var2.value<STU>().a
```

## QSlider
- 滑动条

|函数|说明|
|-|-|
|`setRange(int min,int max)`|设定范围，建议设置跟像素匹配|
|`setSingleStep(键盘操作)`|设置单步长度|
|`setPageStep(鼠标操作)`|设置单步长度|
|`setOrientation Horizontal Vertical`|设置方向|


|事件|说明|
|-|-|
|`void setValue(int value)`|鼠标拖动时触发|
|`void sliderMoved(int value)`|鼠标拖动时触发|
|`void sliderPressed()`|点击滑块|
|`void sliderReleased()`|松开滑块|
|`void valueChanged(int value)`|点击和设置值都会触发|


### QSlider重载鼠标事件
- `void mousePressEvent(QMouseEvent e)`

```c
#include<QSlide>
class XSlide:public QSlider{
public:
XSlide(QWidget *p=Null):QSlider(p){};~XSlide();
virtual MousePressEvent(QMouse Event *e){
  QSlider::mousePressEvent(e);
  double p = (double)e->pos().x()/(double)width();
  int val = p*(maximum()-minium())+minimum();
  setValue(val);
}
}
```

## QListWidget
- 列表

|函数|说明|
|-|-|
|`selectionMode`|单选多选模式|
|`flow`|从上到下还是从左到右|
|`gridSize`|每一行的宽高|
|`iconSize`|图标宽高|

```c
//插入数据3种方法
QListWidgetItem *item = new QListWidgetItem;
item.setText(itemText);
listWidget->insertItem(row,newItem);
new QListWidgetItem("xxx"."listWidget");
void addItem(const QString &label)
[slot]void QListWidget::clear();
```

|事件|说明|
|-|-|
|`listWidget->setEditTriggers(QAbstractItemView::AllEditTriggers)`||
|`item->setFlags(Qt::ItemIsSelectable|Qt::ItemIsEnabled|Qt::ItemIsEditable)`||
|`No EditTriggers 0 No Editing possible`||
|`Currentchanged 1 Editing start whenever current item changes`||
|`for(int i=0;i<ui.listWidget->count();i++){ui.listWidget->item(i)->text();}`|遍历|
|`listWidget->sortItems(Qt::DescendingOrder|AscendingOrder)`|排序|

- QListWidget还可以使用`setItemWidget`添加更多的控件。

## QTableWidget
- 表格控件

|函数|说明|
|-|-|
|`horizontalHeader() verticalHeader`|标题显示|
|`setVerticalScrollBarPolicy`|滚动条显示|
|`selectionMode`|选择模式|
|`setSelectionBehavior`|选择行、列、一项|
|`setCornerButtonEnable`|右上角全选按钮|
|`setShowGrid setGridStyle`|网格显示|
|`setSortingEnable`|排序按钮|
|`setColumnCount(5);setRowCount(5);`<br>`setHorizontalHeaderItem(int column,QTableWidgetItem *itme);`<br>`setHorizontalHeaderLabels(const QStringList &labels);`<br>`setVertivalHeaderItem(int column,QTableWidgetItem *itme);`<br>`setVertivalHeaderLabels(const QStringList &labels);`<br>`QTableWidgetItem *hotzontalHeaderItem(int column) const`|插入标题|
|`setRowCount(5)`<br>`[slot]void insertColumn(int column);[slot]void insertRow(int row)`<br>`setCellWidget(row,column,new QLineEdit);void setItem(row,column,QTableWidgetItem *item)`|插入数据|
|`QList<QTableWidgetItem*>selectedItems()//空行无法选择`<br>`std::set<int>vecItemIndex;QItemSelectionModel *sel = TAB->selectionModel();QModelIndexLIst list = sele->selectedIndexes();`|选择数据|
|`setRowCount(int);[slot]void removeColumn(int column);[slot]void removeRow(int);QmodelIndex currentIndex();`|删除数据|

|事件|说明|
|-|-|
|`cellEntered(int,int);setMouseTracking`|要先设置鼠标追踪，才能获取鼠标悬停事件，即使没有内容的表格也触发，空行有效|
|`itemEntered(QTableWidgetItem *item);`|只有有元素的表格触发，空行无效|
|`cellChanged(int,int)`||
|`cellClicked(int,int)`||
|`cellDoubleClicked(int,int)`||
|`itemChanged(QTableWidgetItem*item)`||


## QTreeWidget

|函数|说明|
|-|-|
|`header()->setVisible(true)`|是否显示标题|
|`setSortingEnabled`|点击标题排序|
|`setAnimated`|动画展开|
|`setVerticalScrollBarPolicy setHorizontalScrollBarPolicy`|滚动条显示|
|`selectionMode`|选择模式|
|`setSelectionBehavior`|选择行、列、项|
|`QTreeWidgetItem *h=NULL;tree->setHeaderItem(new QTreeWidgetItem())//清理原来的标题，不清理数据；ui.treeWidget->clear();h=tree->headerItem();h->setText(0,QStringLiteral("text"));`|设置标题|
|`inserTopLevelItem(int,QTreeWidgetItem*);`<br>`addTopLevelItem(QTreeWidgetItem*)`<br>`new QTreeWidgetItem(treeWidget)`<br>`int topLevelItemCount()const`<br>`void setItemWidget(QTreeWidgetItem *,int,QWidget*)`<br>`QList<QTreeWidgetItem*>selectedItems()const`<br>`void setColumnCount(int columns)`<br>`void addChild(QTreeWidgetItem*)`<br>`void addChildren(const QList<QTreeWidgetItem*>`<br>`void setText(int col,const QString &text)`<br>`QTreeWidgetItem *parent()const`<br>`QTreeWidget * treeWidget()const`|插入读取|


|事件|说明|
|-|-|
|`void collapseItem(const QTreeWidgetItem*)`||
|`void expandItem(const QTreeWidgetItem*)`||
|`currentItemChanged`||
|`itemActivated`||
|`itemClicked`||
|`itemCollapsed`||
|`itemDoubleClicked`||
|`itemEntered`||
|`itemExpanded`||
|`itmePressed`||


## QDIalog
- 继承于QWidget
- 派生类：
```c
QColorDialog、QFileDialog、QFontDialog、QInputDialog、QProgressDialog
QErrorMessage、QMessageBox
```

- 常用方法

|函数|说明|
|-|-|
|`show`||
|`exec`|阻塞有返回值 show|
|`[slot]accept()`|确认，返回值QDialog::Accepted|
|`[slot]reject()`|拒绝，返回值QDialog::Rejected|
|`done(int);`|更多返回值|
|`result()`|获取返回值|





## QProgressbar
- 进度条，一般是展示进度。要新开线程进行处理，防止主窗口堵塞。




# QMainWindow
## 菜单

- QMenuBar、QMenu、QAction


## 工具栏
- QToolBar

## 状态栏
- QStatusBar

# Qt事件
## QEvent事件重载

- `bool event(QEvent*)`返回true，则不继续向父级事件传递，直接结束。所有事件在这个函数中获取到。
- 派生类
```c
QKeyEvent、QMouseEvent、QTouchEvent、QWheelEvent
```

- QWidget重载事件函数，这样防止重载event事件时，发生的强转。
```c
virtual void keyPressEvent(QKeyEvent* e);
virtual void keyReleaseEvent(QKeyEvent* e);
virtual void leaveEvent(QEvent* e);
virtual void enterEvent(QEvent* e);
virtual void mouseDoubleEvent(QMouseEvent* e);
virtual void mouseMoveEvent(QMouseEvent* e);
virtual void mousePressEvent(QMouseEvent* e);
virtual void mouseReleaseEvent(QMouseEvent* e);
virtual void moveEvent(QMouseEvent* e);
virtual void resizeEvent(QResizeEvent* e);
virtual void showEvent(QShowEvent* e);
virtual void wheelEvent(QWheelEvent* e);
virtual void paintEvent(QPaintEvent* e);
```

- QMouseEvent坐标
  - 相对(本地)坐标：x(),y()
  - 窗口坐标：QPoint windowPos()
  - 屏幕坐标：screenPos()、QCursor::pos()
  - 相对坐标转换为屏幕坐标：QWidget::mapToGlobal(ev->pos());
  
- 按键状态
  - button & Qt::LeftButton|Qt::RightButton|Qt::MidButton
  

# Qt图像绘制
## QPainter
## QImage