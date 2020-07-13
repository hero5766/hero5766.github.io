title: C++
author: hero576
tags:
  - C/C++
categories:
  - programme
date: 2020-06-30 19:20:00
---
> `c++`记录
<!--more-->

# 简介
## 背景
- 1982 年，美国 AT&T 公司贝尔实验室的 Bjarne Stroustrup 博士在 c 语言的基础上引入并扩充了面向对象的概念，发明了一种新的程序语言。为了表达该语言与 c 语言的渊源关系，它被命名为 `C++`。而 Bjarne Stroustrup（本贾尼·斯特劳斯特卢普）博士被尊称为`C++`语言之父。

## 历史
- 在“C with Class”阶段，研制者在 C 语言的基础上加进去的特征主要有：类及派生类、共有和私有成员的区分、类的构造函数和析构函数、友元、内联函数、赋值运算符的重载等。
- 1985 年公布的 `C++`语言 1.0 版的内容中又添加了一些重要特征：虚函数的概念、函数和运算符的重载、引用、常量（constant）等。
- 1989 年推出的 2.0 版形成了更加完善的支持面向对象程序设计的 `C++`语言，新增加的内容包括：类的保护成员、多重继承、对象的初始化与赋值的递归机制、抽象类、静态成员函数、const 成员函数等。
- 1993 年的 `C++`语言 3.0 版本是 `C++`语言的进一步完善，其中最重要的新特征是模板（template）,此外解决了多重继承产生的二义性问题和相应的构造函数与析构函数的处理等。
- 1998 年 `C++`标准（`ISO/IEC14882 Standard for the C++ Programming Language`）得到了国际标准化组织（ISO）和美国标准化协会（ANSI）的批准，标准 `C++`语言及其标准库更体现了 `C++`语言设计的初衷。名字空间的概念、标准模板库（STL）中增加的标准容器类、通用算法类和字符串类型等使得 `C++`语言更为实用。此后 `C++`是具有国际标准的编程语言，该标准通常简称 `ANSI C++`或 `ISO C++ 98` 标准，以后每 5 年视实际需要更新一次标准。
- 后来又在 2003 年通过了 `C++`标准第二版（ISO/IEC 14882:2003）：这个新版本是一次技术性修订，对第一版进行了整理——修订错误、减少多义性等，但没有改变语言特性。这个版本常被称为 `C++03`。
- 此后，新的标准草案叫做 `C++ 0x`。对于 `C++ 0x` 标准草案的最终国际投票已于 2011 年 8 月 10 日结束，并且所有国家都投出了赞成票，`C++0x` 已经毫无异议地成为正式国际标准。先前被临时命名为 `C++0x` 的新标准正式定名为 ISO/IEC 14882:2011，简称 `ISO C++11` 标准。`C++ 11` 标准将取代现行的 `C++`标准 `C++98` 和 `C++03`。国际标准化组织于 2011 年 9 月 1 日出版发布《ISO/IEC 14882:2011》，名称是：`Information technology -- Programming languages -- C++ Edition: 3`。
- `C++`标准第四版，2014年8月18日发布。正式名称为ISO/IEC 14882:2014。`C++14`是`C++11`的增量更新，主要是支持普通函数的返回类型推演，泛型 lambda，扩展的 lambda 捕获，对 constexpr 函数限制的修订，constexpr变量模板化等。
- `C++17`
- `C++20`
  
## 内容
- `C++`语言的名字，如果看作 c 的基本语法，是由操作数 c 和运算符后`++`构成。`C++`是本身这门语言先是 c,是完全兼容 c.然后在此基础上`++`。这个`++`包含三大部分，`c++`对 c的基础语法的扩展，面向对像(继承，封装，多态)，STL 等
  
# `C++`对C的扩展(Externsion)
## 类型增强
- 类型检查更严格
  - 比如，把一个 const 类型的指针赋给非 const 类型的指针。c 语言中可以通的过，但是在 `c++`中则编不过去。
```c
int main()
{
const int a = 100;
int b = a;
const int *pa = &a;
int *pb = pa;
return 0;
}
```

- 布尔类型（bool）
  - c 语言的逻辑真假用 0 和非 0 来表示。而 `c++`中有了具体的类型。
  
- 真正的枚举(enum)
  - c 语言中枚举本质就是整型，枚举变量可以用任意整型赋值。而 c++中枚举变量，只能用被枚举出来的元素初始化。
```c
enum season {SPR,SUM,AUT,WIN};
```

- 表达式的值可被赋值
  - c 语言中表达式通常不能作为左值的，即不可被赋值，`c++`中某些表达式是可以赋值的。
```c
int a,b = 5;
(a = b) = 10;  //b的值给了a，10的值又继续给了a
(a<b? a:b) = 200;
```
  

## 输入与输出(cin /cout)
### cin && cout
  - cin 和 cout 是 `C++`的标准输入流和输出流。他们在头文件 iostream 中定义。cin是类对象，>>流输入运算符，重载。
  
|流名 |含义| 隐含设备| 流名| 含义| 隐含设备|
|--|--|--|--|--|--|
|cin| 标准输入| 键盘| cerr| 标准错误输出| 屏幕|
|cout| 标准输出| 屏幕| clog| cerr 的缓冲输出| 屏幕|

### 格式化
#### 设置域宽及位数
- 对于实型，cout 默认输出六位有效数据，setprecision(2) 可以设置有效位数，`setprecision(n)<<setiosflags(ios::fixed)`合用，可以设置小数点右边的位数。
```c
#include <iostream>
#include <iomanip>
using namespace std;
int main()
{
printf("%c\n%d\n%f\n",'a',100,120.00);
printf("%5c\n%5d\n%6.2f\n",'a',100,120.00);
cout<<setw(5)<<'a'<<endl<<setw(5)<<100<<endl
<<setprecision(2)<<setiosflags(ios::fixed)<<120.00<<endl;
return 0;
}
```

#### 按进制输出
- 输出十进制，十六进制，八进制。默认输出十进制的数据。
```c
int i = 123;
cout<<i<<endl;
cout<<dec<<i<<endl;
cout<<hex<<i<<endl;
cout<<oct<<i<<endl;
cout<<setbase(16)<<i<<endl;
```

#### 设置填充符
- 还可以设置域宽的同时，设置左右对齐及填充字符。
```c
int main()
{
cout<<setw(10)<<1234<<endl;
cout<<setw(10)<<setfill('0')<<1234<<endl;
cout<<setw(10)<<setfill('0')<<setiosflags(ios::left)<<1234<<endl;
cout<<setw(10)<<setfill('-')<<setiosflags(ios::right)<<1234<<endl;
return 0;
}
```

## 函数重载(function overload)
### 重载规则
- 重载规则：
  1. 函数名相同。
  2. 参数个数不同，参数的类型不同，参数顺序不同，均可构成重载。
  3. 返回值类型不同则不可以构成重载。
- 有的函数虽然有返回值类型，但不与参数表达式运算，而作一条单独的语句。

### 匹配原则
- 匹配原则：
  1. 严格匹配，找到则调用。
  2. 通过隐式转换寻求一个匹配，找到则调用。

- `C++` 允许，int 到 long 和 double，double 到 int 和 float 隐式类型转换。遇到这种情型，则会引起二义性。

### 原理
- `C++`利用 name mangling(倾轧)技术，来改名函数名，区分参数不同的同名函数。
- 实现原理：用 v-c- i-f- l- d 表示 void char int float long double 及其引用。

### extern “C” 
- name mangling 发生在两个阶段，.cpp 编译阶段，和.h 的声明阶段。只有两个阶段同时进行，才能匹配调用。
- c++ 完全兼容 c 语言，那就面临着，完全兼容 c 的类库。由.c 文件的类库文件中函数名，并没有发生 name mangling 行为，而我们在包含.c 文件所对应的.h 文件时，.h 文件要发生name manling 行为，因而会发生在链接的时候的错误。
- `C++`为了避免上述错误的发生，重载了关键字 extern。只需要,要避免 name manling的函数前，加 extern "C" 如有多个，则 extern "C"{}
```c
#ifdef __cplusplus  //判断是否是c++环境
extern "c"{
#endif
...  // 对c的标准库函数声明，不倾轧
#ifdef _cplusplus
}
#endif
```

## 操作符重载(operator overload)
- `c++`认为一切操作符都是函数，而函数是可以重载的。
```c
struct COMP
{
  float real;
  float image;
};
COMP operator+(COMP one, COMP another)
{
  one.real += another.real;
  one.image += another.image;
  return one;
}
```

- 示例中重载了一个全局的操作符+号用于实现将两个自定义结构体类型相加。本质是`operator+`函数的调用。

## 默认参数(default parameters)
- 通常情况下，函数在调用时，形参从实参那里取得值。对于多次调用用一函数同一实参时，`C++`给出了更简单的处理办法。给形参以默认值，这样就不用从实参那里取值了。
```c
float volume(float length, float weight = 4,float high = 5){
  return length*weight*high;
}
```

- 规则
  1. 默认的顺序，是从右向左，不能跳跃。
  2. 函数声明和定义一体时，默认认参数在定义(声明)处。声明在前，定义在后，默认参数在声明处。 
  3. 一个函数，不能既作重载，又作默认参数的函数。当你少写一个参数时，系统无法确认是重载还是默认参数。

## 引用(Reference)
- 变量名，本身是一段内存的引用,即别名(alias)。此处引入的引用，是为己有变量起一个别名。
- 引用是一种声明关系，声明时必须初始化，引用不开辟空间。
```c
int a;
int &b = a;  //b是a的引用
```

### 规则
1. 引用没有定义，是一种关系型声明。声明它和原有某一变量(实体)的关系。故而类型与原类型保持一致，**且不分配内存**。与被引用的变量有相同的地址。
2. 声明的时候必须初始化，一经声明，不可变更。
3. 可对引用，再次引用。多次引用的结果，是某一变量具有多个别名。
4. &符号前有数据类型时，是引用。其它皆为取地址。(变量前面的&是取地址：&a)
```c
int a,b;
int &r = a;
int &r = b; //错误，不可更改原有的引用关系
float &rr = b; //错误，引用类型不匹配
cout<<&a<<&r<<endl; //变量与引用具有相同的地址。
int &ra = r; //可对引用更次引用，表示 a 变量有两个别名，分别是 r 和 ra
```

### 引用的本质
- 引用的本质是指针，`C++`对裸露的内存地址(指针)作了一次包装。又取得的指针的优良特性。所以再对引用取地址，建立引用的指针没有意义。
- 引用的本质是，是对常指针 type * const p 的再次包装。`char &rc == *pc double &rd == *pd`

### 注意点：
1. 可以定义指针的引用，但不能定义引用的引用。
```c
int a;
int* p = &a;
int*& rp = p; // ok
int& r = a;
int&& rr = r; // error
```

2. 可以定义指针的指针(二级指针)，但不能定义引用的指针。
```c
int a;
int* p = &a;
int** pp = &p; // ok
int& r = a;
int&* pr = &r; // error
```

3. 可以定义指针数组，但不能定义引用数组，可以定义数组引用。
```c
int a, b, c;
int* parr[] = {&a, &b, &c}; // ok
int& rarr[] = {a, b, c}; // error
int arr[] = {1, 2, 3};
int (&rarr)[3] = arr; // ok，必须是int[3]类型的引用
```

4. 常引用：const 引用有较多使用。它可以防止对象的值被随意修改。因而具有一些特性。
  - **const 对象的引用必须是 const 的**，将普通引用绑定到 const 对象是不合法的。 这个原因比较简单。既然对象是 const 的，表示不能被修改，引用当然也不能修改，必须使用 const 引用。实际上，const int a=1; int &b=a;这种写法是不合法的，编译不过。
  - **const 引用可使用相关类型的对象(常量,非同类型的变量或表达式)初始化**。这个是const 引用与普通引用最大的区别。const int &a=2;是合法的。double x=3.14; const int&b=a;也是合法的。
  - 常引用原理：const 引用的目的是，禁止通过修改引用值来改变被引用的对象。const 引用的初始化特性较为微妙，可通过如下代码说明：
```c
double val = 3.14;
const int &ref = val; // int const & int & const ??
double & ref2 = val;
cout<<ref<<" "<<ref2<<endl;
val = 4.14;
cout<<ref<<" "<<ref2<<endl;
```

  - 上述输出结果为 3 3.14 和 3 4.14。因为 ref 是 const 的，在初始化的过程中已经给定值，不允许修改。而被引用的对象是 val，是非 const 的，所以 val 的修改并未影响 ref的值，而 ref2 的值发生了相应的改变。那么，为什么非 const 的引用不能使用相关类型初始化呢？实际上，const 引用使用相关类型对象初始化时发生了如下过程：`int temp = val;const int &ref = temp;`如果 ref 不是 const 的，那么改变 ref 值，修改的是 temp，而不是 val。期望对 ref 的赋值会修改 val 的程序员会发现 val 实际并未修改。
```c
int i=5;
const int & ref = i+5;
//此时产生了与表达式等值的无名的临时变量，
//此时的引用是对无名的临时变量的引用。故不能更改。
cout<<ref<<endl;
```

5. 尽可能使用 const：原因如下：
  - 使用 const 可以避免无意修改数据的编程错误。
  - 使用 const 可以处理 const 和非 const 实参。否则将只能接受非 const 数据。
  - 使用 const 引用，可使函数能够正确的生成并使用临时变量（如果实参与引用参数不匹配，就会生成临时变量）

## new/delete Operator
- c 语言中提供了 malloc 和 free 两个系统函数(库:stdlib.h)，完成对堆内存的申请和释放。而 `c++`则提供了两关键字 new 和 delete ;
 
### new/new[]用法:
1. 开辟单变量地址空间
```c
int *p = new int; //开辟大小为 sizeof(int)空间
int *a = new int(5); //开辟大小为 sizeof(int)空间，并初始化
//类型转换
c:   int *p = (int*)malloc(sizeof(int));
c++:  int *p = static_cast<int*>(malloc(size(int)));
```

2. 开辟数组空间
```c
一维: int *a = new int[100]{0};开辟一个大小为 100 的整型数组空间
初始化：memset(a,0,sizeof(int[100]));
指针数组：int **p = new int*[5]{NULL};
二维: int (*a)[6] = new int[5][6]{{0}};
三维: int (*a)[5][6] = new int[3][5][6]{{{0}}};
四维维及其以上:依此类推.
```

### delete /delete[]用法:
```c
//1. int *a = new int;
delete a; //释放单个 int 的空间
//2.int *a = new int[5];
delete []a; //释放 int 数组空间
```

- 返回值
```c
//c 语言版本
char *ps = (char*)malloc(100);
if(ps == NULL)
return -1;
//C++ 内存申请失败会抛出异常
try{
int *p = new int[10];
}catch(const std::bad_alloc e) {
return -1;
}
//C++ 内存申请失败不抛出异常版本
int *q = new (std::nothrow)int[10];
if(q == NULL)
return -1;
```

### 注意事项
1. new/delete 是关键字，效率高于 malloc 和 free.
2. 配对使用，避免内存泄漏和多重释放。
3. 避免，交叉使用。比如 malloc 申请的空间去 delete，new 出的空间被 free;

## 内联函数(inline function)
- c 语言中有宏函数的概念。宏函数的特点是内嵌到调用代码中去，避免了函数调用的开销。但是由于宏函数的处理发生在预处理阶段，缺失了语法检测和有可能带来的语意差错。例如传入`i++`。
- `C++`提供了 inline 关键字，实现了真正的内嵌。
```c
//宏优点：内嵌代码，辟免压栈与出栈的开销
//缺点: 代码替换，易使生成代码体积变大，易产生逻辑错误，无类型检查
#define SQR(x) ((x)*(x))
//函数优点：高度抽象，避免重复开发，类型检查
//缺点: 压栈与出栈，带来开销
inline int sqr(int x){  //内联函数集合了宏和函数的优点
  return x*x;
}
int main(){
  int i=0;
  while(i<5){
    // printf("%d\n",SQR(i++));
    printf("%d\n",sqr(i++));
  }
  return 0;
}
```

- 内联的优缺点
  - 优点：避免调用时的额外开销（入栈与出栈操作）
  - 代价：由于内联函数的函数体在代码段中会出现多个“副本”，因此会增加代码段的空间。
  - 本质：以牺牲代码段空间为代价，提高程序的运行时间的效率。
  - 适用场景：函数体很“小”，且被“频繁”调用
  - 编译器会根据内联函数的代码数量，决定是否内联；但是不声明内联的函数不会自动内联。

## 类型强转(type cast)
- 类型转换有 c 风格的，当然还有 `c++`风格的。c 风格的转换的格式很简单（TYPE EXPRESSION)，但是 c 风格的类型转换有不少的缺点，有的时候用 c 风格的转换是不合适的，因为它可以在任意类型之间转换，比如你可以把一个指向 const 对象的指针转换成指向非const 对象的指针，把一个指向基类对象的指针转换成指向一个派生类对象的指针，这两种转换之间的差别是巨大的，但是传统的 c 语言风格的类型转换没有区分这些。还有一个缺点就是，c 风格的转换不容易查找，他由一个括号加上一个标识符组成，而这样的东西在 `c++`程序里一大堆。所以 `c++`为了克服这些缺点，引进了新的类型转换操作符

### 静态类型转换
- 语法格式：`static_cast<目标类型> (标识符)`
- 转化规则：在一个方向上可以作隐式转换，在另外一个方向上就可以作静态转换。
```c
int a = 10;
int b = 3;
a=b;b=a; //两个相互可以隐式转换，此时两个方向都可以静态转换
b=static_cast<float>(a); 
a=static_cast<float>(b); //float = int int = float
int *p; void *q;
q = p; // void*可以等于int*，但是反过来不可以，需要强制转换
p = static_cast<int*>(q);
char *p = static_cast<char*>(malloc(100)); //malloc开辟的void*变量，强转为char*
```

### 重解释类型转换
- 语法格式：`reinterpret_cast<目标类型> (标识符)`
- 转化规则：“通常为操作数的位模式提供较低层的重新解释”也就是说将数据以二进制存在形式的重新解释，在双方向上都不可以隐式类型转换的，则需要重解释类型转换。
```c
char *p; int *q;
p = q;q = p;  //两个类型之间无法相互隐式转化
int x = 0x12345648;
char *p = reinterpret_cast<char*>(&x);
int a[5] = {1,2,3,4,5};
int *q = reinterpret_cast<int*>((reinterpret_cast<int>(a) +1));
```

### (脱)常类型转换
- 语法格式：`const_cast<目标类型> (标识符)`，<font color="blue">目标类类型只能是指针或引用</font>
- 转化规则：用来移除对象的常量性(cast away the constness)使用 const_cast 去除 const 限定的目的不是为了修改它的内容，而通常是为了函数能够接受这个实际参数。
- 可以改变 const 自定义类的成员变量，但是对于内置数据类型，却表现未定义行为。
> CSDN：Depending on the type of the referenced object, a write operation through the resulting pointer, reference, or pointer to data member might produce undefined behavior.

```c
//应用场景 1：
void func0(const int & ref){} //参数为const类型引用
void func(int & ref){} //别人己经写好的程序或类库
int main(void){
const int m = 44;
func0(m);//可以传入，同时不能更改
func(const_cast<int&>(m)); //将m脱const，传入参数是引用的类库
return 0;}
```

```c
//脱掉 const 后的引用或指针可以改吗？const变量一定不能改
const int x = 200;
int & a =const_cast<int&>(x); // int &a = x;  脱常转为引用
a = 300;
cout<<a<<":"<<x<<"---"<<a<<":"<<&x<<endl; //地址相同，值不同
int *p =const_cast<int*>(&x); // int *p = &x;脱常转为指针
*p = 400;
cout<<x<<":"<<*p<<"---"<<&x<<":"<<p<<endl;//地址相同，值不同
struct A {int data;};
const A xx = {1111};  //复杂类型的常量
A &a1 = const_cast< A&>(xx);a1.data = 222;  
cout<<a1.data<<xx.data<<endl;   //地址变化了，值同步更改了
A *p1 = const_cast<A*>(&xx);p1->data = 333;
cout<<p1->data<<xx.data<<endl;
```

- const补充：宏在预处理阶段替换了所有宏，而const变量是在编译阶段替换了所有变量。const永远不去改变。

### 动态类型转换
- 语法格式：`dynamic_cast<目标类型> (标识符)`
- 转化规则：用于多态中的父子类之间的强制转化

## 命名空间(namespace scope)
- 命名空间为了大型项目开发，而引入的一种避免命名冲突的一种机制。比如说，在一个大型项目中，要用到多家软件开发商提供的类库。在事先没有约定的情况下，两套类库可能在存在同名的函数或是全局变量而产生冲突。项目越大，用到的类库越多，开发人员越多，这种冲突就会越明显。


## 系统 string 类

# `C++`面向对象
## 封装(Encapsulation)

## 类与对象(Class &&object)

## 友元(Friend)

## 运算符重载(Operator OverLoad)

## 继承与派生(Inherit&&Derive)

## 多态（PolyMorphism）

# `C++`高级
## 模板(Templates)

## 输入输出 IO 流

## 文件 IO 流

## 异常(Exception)

- 异常捕获
```c
try{
	int *pi = new int[100];
}catch(const std::bad_alloc e){
	cout<<e.what<<"some err"<<endl;
}
int *pi = new (std::nothrow)int[100];// 不抛出异常
if(pi==NULL)exit(1);
```

## STL

## C11

## Boost






















# 7