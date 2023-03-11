---
title: RUST
author: hero576
tags:
  - language
categories:
  - programming
date: 2021-10-19 14:58:00
---
<!--more-->

[toc]

# 简介

## 介绍

- 开始是私人项目，专注于安全。后公司资助，2015年发布第一版。
- [Rust 程序设计语言 简体中文版](https://kaisery.github.io/trpl-zh-cn/ch01-01-installation.html)
- [官方网址](https://www.rust-lang.org/)
- [标准库说明文档](https://doc.rust-lang.org)
## 环境搭建

- [官方地址](https://www.rust-lang.org/learn/get-started)

### ubuntu

```bash
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
source ~/.barch_profile
rustc --version
```

### windows:[下载链接](https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe)

## hello-world

- 建立工程：`cargo new hello`

```rust
//hello/src/main.rs
fn main(){
  println!("Hello, World!")
}
```

- 运行：`cargo run`

# cargo

| 命令    | 说明                                                                                                                  |
| ------- | --------------------------------------------------------------------------------------------------------------------- |
| new     | 创建工程，`cargo new project<br>`--lib创建库，`cargo new --lib libname`                                           |
| run     | 运行工程，在项目目录下运行：`cargo run`，项目目录会创建target文件夹，以及对应debug\可执行文件 <br>--release版本 |
| build   | 编译工程 <br>--release，在target生成release\可执行文件                                                            |
| check   | 检查工程语法                                                                                                          |
| test    | 测试                                                                                                                  |
| doc     | 根据三个反斜杠的注释，生成文档 <br>--open打开生成的html文档                                                       |
| login   | 登录crates.io账号                                                                                                     |
| publish | 发布自己crate包                                                                                                       |
| yank    | 撤回 <br> `cargo yank --vers 0.1.0`                                                                             |
| search    | 搜索包，可以去顶版本号 <br> `cargo search rand`                                                                             |

## 编译的优化

- 在Cargo.toml中编辑

```ini
[proifle.dev]
;优化级别，debug默认是0，release是3。值越高，编译时间越长
opt-level=0

[proifle.release]
opt-level=3
```

## 工作空间

- 除了cargo new建立工程外，也可以手动创建

```bash
mkdir mypro
echo '[workspace]\nmembers=["addr",]' > Cargo.toml
cargo new addr # 创建addr文件夹
cargo build # 创建target
cargo run -p addr
```

## 配置代理
```ini
# vi ~/.cargo/config
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with="ustc"
[source.ustc]
registry="git://mirrors.ustc.edu.cn/crates.io-index"
```

# 基本概念

## 基本属性

- 变量定义： `let name: type`

```rust
let a =1;println!("a={}",a);//不可变变量
```

| 类型   | 说明                                                       |
| ------ | ---------------------------------------------------------- |
| 可变性 | 不可变(默认)：`let a=1;<br>`<br />可变：`let mut a=1;` |
| 常量   | `const MAX_POINTS:u32=10000;`                            |
| 隐藏性 | 定义重名变量，则会覆盖前一个                               |

## 数据类型

- rust是静态语言，在编译前要确定变量类型，编译器有自动推导的能力。

| 数据类型   | 说明                                                                                                           |
| ---------- | -------------------------------------------------------------------------------------------------------------- |
| bool       | `let a:bool=true;let b:bool=false;`                                                                          |
| char       | 字符型是32位的，与其他语言不同，所以可以是一个汉字`<br>let a='a';let b='一';`                                |
| 数字       | i:8,16,32,64;u:8,16,32,64;f:32,64;`<br>let c:i8=-11;`                                                        |
| 数组       | 方括号[type;size]其中size也是数组类型的一部分`<br>let arr:[u32;5]=[1,2,3,4,5];println!("arr[0]={}",arr[0]);` |
| 自适应类型 | isize,usize大小根机器位数有关`<br>println!("max={}",usize::max_value());`                                    |

| 复合数据类型 | 说明                                              |
| ------------ | ------------------------------------------------- |
| 元组         | 圆括号 `​let tup:(i32,f32,char)=(-3,3.2,'123');println!("{}", tup.0);`<br>解包：`let (x,y,z)=tup;` |
| 结构体       |                                                   |
| 枚举         |                                                   |
| 字典         |                                                   |

### 字符串

| 操作         | 说明                                                                                                                                                                                                     |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 创建空字符串 | `let mut s=String::new();`                                                                                                                                                                             |
| 创建有初值   | `let mut s=String::from("hello");`<br>通过字面值 `let mut s="hello".to_string();`                                                                                                                    |
| 更新         | push_str：`s.push_str(" world");s.push_str(&s0);`<br>push：`s.push('!')`只能添加一个字符<br>+：`let s3=s1+&s2;`,s1所有权给了s3<br>format!：`let s=format!("{}-{}",s1,s2);`不会获取所有权 |
| 索引         | String：`let s2=s[0];`报错，rust不支持被索引，防止utf8边界错误<br>str：`let s="你好";let h=&hello[0..3];`，边界不同也会报错                                                                      |
| 遍历         | chars：`for c in s.chars(){println!();}`<br>bytes：`for c in s.bytes(){println!();}`                                                                                                                 |
| 析构         | 离开作用域会调用 `drop`，释放内存                                                                                                                                                                      |

- 单引号：字符
- 双引号：表示引用

### 切片slices

- 字符串slice是String中一部分值的引用

```rust
let s1='hello'; //字面值，不可变
let s=String::from("hello world");
let h=&s[0..5];  // 指向s的0-5的空间，
let h=&s[0..=4];  // 指向s的0-5的空间
let h=&s[..5];  // 指向s的0-5的空间
let h=&s[..=4];  // 指向s的0-5的空间
println!("h={}",h);
let w=&s[6..11];
let w=&s[6..];
let w=&s[..];

let ss=String::from("你好"); 
let w1=&ss[0..2]; // error，slince必须在有效的utf-8字节边界，否则报错
println!("w={}",w);
```

- 字面值就是slice，不可变的引用
```rust
let s2 = "hh";
```

- 数组

```rust
let a = [1,2,3,4,5];
let ss= &a[1..3];
println!("ss={}",ss[0]);
println!("len ss: {}",ss.len());
```

### 结构体

| 操作                         | 说明                                                                                                                             |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 定义                         | `struct User{name:String,count String,nonce:u64,active:bool}`                                                                  |
| 实例                         | `let mut xiao=User{name:String::from("xiao"),count:String::from("8454"),nonce:1000,active:true,}`                              |
| 修改字段                     | 前提是mut可变变量 `xiao.nonce=2000;`                                                                                           |
| 参数名字和字段名同名时可简写 | `let name=String::from("xiao");let count=String::from("80808");nonce=1000;active=true;let xiao=User{name,count,nonce,active};` |
| 从其他结构体创建实例         | `let xiao2=User{name:String::from("xiao2"),..xiao};//此时xiao被借用`                                                           |
| 元祖结构体                   | (1)字段没有名字，(2)圆括号 `struct Point(i32,i32); let a=Point(10,20);println!("a.x={},a.y={}",a.0,a.1);`                            |
| 没有任何字段的类单元结构体   | `struct A{};`                                                                                                                  |
| 打印结构体                   | 在结构体上方加上：`#[derive(Debug)]`<br>`println!("xiao={:?}",xiao);`<br>自动换行:`println!("xiao={:#?}",xiao);`                                      |

- 结构体方法

```rust
#[derive(Debug)]
struct Dog{
  name:String,
  weight:f32,
  height:f32,
}
impl Dog{
  fn get_name(&self)->&str{
    &(self.name[..])   // 返回引用
  }
  fn get_weight(&self)->f32{
    self.weight
  }
  fn get_height(&self)->f32{
    self.height
  }
  fn show(){
    println!("owfo");
  }
}

impl Dog{
  fn show2(){
    println!("owfo2");
  }
}

fn main(){
  let d = Dog{
    name:String::from("wangcai");
    weight:100;
    height:100;
  }
  let name = d.get_name();
  println!("dog={:#?}",d);
  Dog::show();
}
```

### 枚举

<table>
<tr><th>操作</th><th>说明</th></tr>
<tr>
<td>定义方式一(C语言方式)</td>
<td>enum IpAddr{V4,V6};<br>struct Ip{kind:IpAddr,addr:String}<br>let ip1=Ip{kind:IpAddr::V4,address:String::from("127.0.0.1"),};
</td>
<tr>
<td>定义方式二(推荐)</td>
<td> enum IpAddr{V4(S tring),V6(String),};<br>let i=IpAddr::V4(String::from("127.0.0.1"))<br>
</td>
</tr>
</tr>
<tr>
<td>定义不同类型</td>
<td> enum IpAddr{V4(u8,u8,u8,u8,),V6(String),};<br>let ip1=IpAddr::V4(127,0,0,1));<br>let ip2=IpAddr::V6(String::from("::1")));
</td>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>

- 用法

```rust
enum Message{
  Quit,  //等同于类单元结构体QuitMessage
  Move{x:i32,y:i32}, //结构体MoveMessage
  Write(String),  //元组WriteMessage
  Change(i32,i32,i32),//元组结构体ChangeMessage
};

```

- 方法及模式匹配

```rust
impl Message{
  fn print(&self){ //取引用，不会更改所有权
    match *self{ // 获取内容需要解引用
      Message::Quit=>println!("Quit"),
      Message::Move{x,y}=>println!("Move({},{})",x,y),
      Message::Change(a,b,c)=>println!("change({},{},{})",a,b,c),
      // Message::Write(&s)=>println!("write({})",s), 引用无法打印
      _ => println!("write()")
    }
    match self{ // 获取本身
      Message::Quit=>println!("Quit"),
      Message::Move{x,y}=>println!("Move({},{})",x,y),
      Message::Change(a,b,c)=>println!("change({},{},{})",a,b,c),
      Message::Write(s)=>println!("write({})",s), // 函数传入的是s，使用的时候也是s，所以可以使用
    }
  }
}
fn main(){
  let  quit= Message::Quit;
  quit.print();
  let mov1 = Message::Move{x:10,y:1};
  mov1.print();
}
```

### Option

- 标准库定义的一个枚举，原始形式：`enum Option<T>{Some(T),None,}`，T表示泛型。要么表示一个值，要么表示None
- 主要用法：
    1. 初始化值
    2. 作为在整个输入范围内没有定义的函数的返回值
    3. 作为返回值，用None表示出现的简单错误
    4. 作为结构体的可选字段
    5. 作为结构体中可借出或是可载入的字段
    6. 作为函数的可选参数
    7. 代表空指针
    8. 作为复杂情况的返回值

```rust
fn main(){
  let some_number=Some(5); //自动推导类型
  let some_string=Some(String::from("s")); 
  let none_number:Option<i32> = None;
  let x:i32 = 5;
  let y:Option<i32>=Some(5);
  //let sum = x+y; //error, 两者是不同类型
  let mut temp = 0;
  match y{
    Some(i)=>{temp=i;},
    None=>{}, //匹配不全时会报错
  }
  if let Some(value)=plus_one(y){
    println!("value={}",value);
  }else{   //不匹配也不会报错
    println!("none");
  }

  let sum = x+temp;
  println!("sum={}",sum);
}
fn plus_one(x:Option<i32>)->Option<i32>{
  match x{
    None=>None,
    Some(x)=>Some(x+1),
  }
}
```

### vector

| 操作                 | 说明                                                                                                                                                           |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 创建空的vector       | `Vec<T>`<br>`let mut v:Vec<i32>=Vec::new();`                                                                                                               |
| 创建包含初值的vector | `Vec<T>`<br>`let mut v=vec![1,2,3];`                                                                                                                       |
| 压栈                 | `v.push(1);`                                                                                                                                                 |
| 丢弃vector           | 会丢弃全部元素，`{let v1=vec![1];}`                                                                                                                          |
| 读取                 | 方式一：`let one:&i32=&v[0];`<br>方式二(推荐)越界不会崩溃：`match v.get(1){Some(value)=>println!("two={}",value),_=>{},}`                                              |
| 遍历                 | 不可变遍历 `for i in &v{println!("i={}",i);}`<br>可变遍历 `for i in &mut v{*i+=1;}`                                                                        |
| 存储不同的类型       | 利用枚举存储不同的类型，`enum Context{Text(String),FLoat(f32),Int(i32),};`<br>`let c=vec![Context::Text(String::from("1")),Context::Float(1.2),Int(12),];` |


### hashmap

- `HashMap<Key,Value>`

| 操作         | 说明                                                                                                                                                                                                |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 导入包       | `use std::collections::HashMap;`                                                                                                                                                                  |
| 创建         | 方式一：`let mut map:HashMap<String,i32>=HashMap::new();`<br>方式二，迭代器：`let keys=vec![String::from("1")];let values=vec![1];let map:HashMap<String,i32>=kes.iter().zip(values.iter()).collect();` |
| 直接插入     | `map.insert(String::from("1"),1);`                                                                                                                                                                |
| 不存在时插入 | `map.entry(String::from("2")).or_insert(2);`                                                                                                                                                      |
| 更新插入     | `let count=map.entry(String::from("1")).or_insert(0);*count+=1;`                                                                                                                                  |
| 读取         | 返回option，if或者match提取value<br>`let k=String::from("1");`<br>`if let Some(v)=map.get(&k){println!("v={}",v);}`                                                                                                                         |
| 遍历         | `for (k,v) in &map{println!("{}:{}",k,v);}`<br>                                                                                                                          |
|打印|`println!("{:?}, map");`|


## 函数

- 函数定义：`fn f1(){}`
- 函数调用
- 参数传入：`fn f1(a:i32,b:u32){}`参数定义时需要类型，无法自动推导
- 返回值：
  - 方法一：`fn f1(a:i32,b:i32)->i32{return a+b;}`
  - 方法二：`fn f1(a:i32,b:i32)->i32{a+b}` 语句没有返回值，所以不能有分号；表达式会有返回值
  - 表达式是有返回值的：`let y = {let a=1;a+1};`

## 注释

- 单行注释：`//...`
- 多行注释：`/* .. */`
- 文档注释：`/// 注释内容`，使用cargo doc生成的类似函数属性等的详细说明，注释可以使用markdown格式
- 包注释：`//! 注释内容`，使用cargo doc生成的对包的说明

## 控制流

### if

```rust
let y=1;
if y==1{
  println!("y==1");
}else if y==0{
  println!("y==0");
}else{
  println!("y!=1");
}
//let中使用if
let condition=true;
let x=if condition{
  5 // 类型要保证一致
  //"5" error
}else{
  6
};
println1("x={}",x);
```

### loop

```rust
let mut counter = 0;
loop{
  counter+=1;
  println!("hello");
  if counter>=10{
    break;  //无返回值
  }
}
let res = loop{
  counter+=1;
  if counter>=20{
    break counter*2; //返回值counter*2
  }
};
```

### while

```rust
let mut i=0;
while i!=10{
  i+=1;
}
```

### for

```rust
let arr:[u32;5]=[1,2,3,4,5];
// for i in arr.iter(){  //使用迭代器
for i in &arr{   //使用引用
  println!("{}",i);
}
```

### match

- 类似于switch

```rust
match a{
  1=>println!("1"),
  2=>println!("2"),
  _=>println!("default"),
}
```

# 所有权

- rust使用所有权机制管理内存，编译器在编译就会根据所有权对内存的使用进行检测

## 堆和栈

- 编译的时候，数据类型大小是固定的，就是分配在栈上，反之就是堆上

```rust
let x:i32=1; // 栈上
let s1=String::from("hello"); // string类型长度不确定，堆上
```

## 变量作用域

- 作用域范围：`{}`

### 变量的作用域
- 不可变类型直接删除，可变类型离开作用域会调用drop方法，类似c++中的析构函数。

- 移动
```rust
let s1 = String::from("1");
let s2 = s1;  // 拷贝移动之后，此时s1会被销毁
              // 目的是防止离开作用域时，s2和s1重复回收指向的空间
```

### 函数的作用域：
```rust
fn ownership(s:String){
  println!("s={}",s); //退出后s就被回收了
}
fn unownership(s:String)->String{
  println!("s={}",s); 
  s  //退出后，其他地方使用了该变脸，将所有权传回去了
}
fn copyi(i:i32){
  println!("i={}",i);
}
fn main(){
  let s=String::from("hello");
  let s2=String::from("hello");
  let i=5;
  ownership(s); //调用后就回收了
  unownership(s2);
  copyi(i);
}
```

### 移动和拷贝

- 堆上的移动后原来变量无效，反之多次释放同一块内存；

```rust
let s1=String::from("hello"); // string类型长度不确定，堆上
let s2=s1;
println!("s2={}",s2);
println!("s1={}",s1); // error,value borrowed here after moved
//防止drop时，重复析构，默认移动后s1就无效了
```

- 堆上的克隆相当于深拷贝；

```rust
let s1=String::from("hello"); // string类型长度不确定，堆上
let s2=s1.clone();
```

- 栈上的拷贝可以继续使用，或者实现了copy特征trait的类型也无需clone，包括：整型、浮点、布尔、字符、元组；

```rust
let a=1;
let b=a;
println!("a={},b={}",a,b);
```

## 引用和借用

- 引用：`& name`，与c++的引用是一样的，就是一个别名。但默认不能更改
- 解引用：`* name`，取里面的内容
- 借用：`&mut name`
- 创建一个指向值的引用，但并不拥有这个值，所以离开作用域不会被释放；当一个变量借用给其他变量后，rust会禁止其再使用，防止其他变量变化导致当前变脸也变化的问题

```rust
fn calcute_len(s:&String)->usize{ //引用
  s.len()
}
// fn modify_s(s:&String){ //error 修改时需要借用
fn modify_s(s:&mut String){ //修改时需要借用
  s.push_str(" world");
}
fn main(){
  let mut s1 = String:from("hello");
  let l1 = calcute_len(&s1);   //引用
  println!("{} len is {}",s1,l1); // s1仍可用
  // modify_s(&s1); //引用不能修改
  modify_s(&mut s1);
  println!("{} len is {}",s1,l1); 
}

```

- 可变引用

```rust
fn modify_s(s:&mut String){ //修改时需要借用
  s.push_str(" world");
}
fn main(){
  let mut s1 = String:from("hello");
  let ms = &mut s1;
  modify_s(&mut ms);
  println!("{}",s1); // error，可变引用被改变以后，原值不能再使用 
}
```

- 借用后不能再使用引用

```rust
let mut s1 = String:from("hello");
let r1 = &s1;
let r2 = &s1;
let r3 = &mut s1;
println!("{}",r1,r2,r3); // error，可变引用被改变以后，原值不能再使用 
```

- 悬垂引用（Dangling References）

```rust
fn main(){
  let ref_s = dangle(); //error，回传的是一个空指针
  println!("{}",ref_s);
}
fn dangle()->String{
  let s=String::from("hello");
  &s  //回传后s被释放，&s指向为空
}
```

# 包

- 包（Packages）： Cargo 的一个功能，它允许你构建、测试和分享 crate。
- Crate ：一个模块的树形结构，它形成了库或二进制项目。
- 模块（Modules）和 use： 允许你控制作用域和路径的私有性。
- 路径（path）：一个命名例如结构体、函数或模块等项的方式

## 模块

- mod模块创建默认是私有的，不能直接通过路径调用，需要声明pub

```rust
mod factory{
  pub mod produce_refrigerator{
    pub fn produce(){
      println!("refrigerator");
    }
  }
  mod produce_washing{
    fn produce(){
      println!("washing");
    }
  }
}
fn main(){
  factory::produce_refrigerator::produce();
  factory::produce_washing::produce(); // error 私有的
}
```

## crate

- 创建库：`cargo new --lib mylib`

```rust
//vi mylib/src/factory.rs
pub mod produce_refrigerator{
  pub fn produce(){
    println!("refrigerator");
  }
}
mod produce_washing{
  fn produce(){
    println!("washing");
  }
}
```

- 导出模块

```rust
//vi mylib/src/lib.rs
pub mod factory
```

- 配置模块:Cargo.toml

```ini
#vi Cargo.toml
...
[dependencies]
mylib={path="./mylib"}
```

- 使用库

```rust
//vi src/main.rs
use mylib::factory::produce_refrigerator as A;  // 设置别名
use mylib::factory::produce_refrigerator::produce;  
use mylib::factory::*; // 导入所有模块
fn main(){
  mylib::factory::produce_refrigerator::produce(); //绝对路径
  produce(); //使用use，简化路径
  A::produce();
}
```

## mod的结构体

```rust
mod modA{
  pub struct A{
    pub number:i32,
    name:String,
  }
  impl A{
    pub fn new_a()-> A{
      A{
        number:1,
        name:String::from("啊"),
      }
    }
    pub fn print(){
      println!("{:?}",self);
    }
  }
}
fn main(){
  let a = modA::A::new_a();
  a.print();
}
```

- 访问上一级的模块

```rust
pub mod modB{
  pub fn print(){
    println!("B");
  }
  pub mod modC{
    pub fn print(){
      println!("c");
      super::print();  //父模块函数
    }
  }
}
```

## 外部库

- 引入rustcrypto，在Cargo.toml添加引用

```ini
...
[dependencies]
rust-crypto="0.2"
```

- 在代码中引用

```rust
//src/main.rs
extern crate crypto;
use crypto::digest::Digest;
use crypto::sha3::Sha3;
fn main(){
  let mut hasher=Sha3:sha3_256();
  hasher.input_str("hello world");
  let res = hasher.result_str();
  println!("res={}",res);
}
```

- 执行 `cargo run`时，相应的库会自动下载和编译。

## 发布包

- 分享crate包到crates.io

## 发布与撤回

| 步骤                                   | 说明                                     |
| -------------------------------------- | ---------------------------------------- |
| 创建[crates.io](https://crates.io/)的账号 | 通过github注册，通过 `cargo login`登录 |
| 在 `Cargo.toml`添加描述              |                                          |
| 发布                                   | `cargo publish`                        |
| 撤回                                   | `cargo yank --vers 0.1.0`              |

```ini
[package]
name="package_name"
version="0.1.0"
license="MIT"# linux基金会的Softwae Package Data Exchange列出了可以使用的标示符
authors=["xxx"]
decription="xxxxx"
```

# 错误

- rust的错误分为：
  - 可恢复错误：用户报告的错误和重试操作是否合理，用枚举 `Result<T,E>`实现
  - 不可恢复错误：通过 `panic! unwrap expect`实现

## panic!/unwrap/except
- 直接抛出异常

```rust
fn main(){
  panic!("crash!!");
}
```

## RUST_BACKTRACE

- `RUST_BACKTRACE=1 cargo run`执行时会打印调试堆栈，非0即可

## Result<T,E>

- 错误处理的枚举原型，可以进行错误传递和处理。

```rust
enum Result<T,E>{
  Ok(T),
  Err(E),
}
```

```rust
use std::fs::File;
fn main(){
  let f=File::open("hello.txt");
  let r = match f{
    Ok(file)=>file;
    Error(error)=>panic!("error:{:?}",error);
  }
}
```

- 简写方式

```rust
fn main(){
  //方式一
  let f=File::open("hello.txt").unwrap(); //失败则panic
  //方式二
  let f=File::open("hello.txt").expect("Failed to open file"); //可以打印自己的提示
}
```

## 错误传递

```rust
use std::io;
use std::io::Read;
use std::fs::File;
fn main(){
  let r = read_username();
  match r{
    Ok(s)=>println!("s={}",s);
    Err(e)=>println!("error:{:?}",e);
  }
}
fn read_username()->Result<String,io::Error>{
  let f=File::open("hello.txt");
  let mut f = match f{
    Ok(file)=>file,
    Err(error)=>return Err(error),
  };
  let mut s=String::new();
  match f.read_to_string(&mut s){
    Ok(_)=>Ok(s),
    Err(error)=>Err(error),
  }
}

//简写方式1 使用？抛出error
fn read_username2()->Result<String,io::Error>{
  let mut f=File::open("hello.txt")?;
  let mut s=String::new();
  f.read_to_string(&mut s)?;
  Ok(s)
}
//简写方式2，直接连接的方式
fn read_username2()->Result<String,io::Error>{
  let mut s=String::new();
  File::open("hello.txt")?.read_to_string(&mut s)?;
  Ok(s)
}
```

# 测试

- 执行 `cargo new --lib mylib`

```rust
// vi mylib/src/animal.rs
pub mod Dog{
  pub fn hello()->bool{
    println!("wang");
    true
  }
}
pub mod Cat{
  pub fn hello()->bool{
    println!("miao");
    true
  }
}
// vi mylib/src/lib.rs
pub mod animal

#[cfg(test)]
mod tests{
  use crate::animal::*;
  #[test]
  fn it_works(){
    assert_eq!(2+2,4);
  }
  #[test]
  fn use_cat(){
    assert_eq!(true,Cat::hello());
  }
}
```

- 执行测试：`cargo test`

# 泛型

- 泛型是具体类型或其他属性的抽象替代。减少代码重复
- 泛型不会导致程序性能损失，rust在编译过程会对泛型进行单态化，转化为特定代码。

## 使用泛型

- 函数定义

```rust
//泛型满足特征：PartialOrd可排序，Copy有copy特征
fn largest<T:PartialOrd+Copy>(list:&[T])->T{
  let mut larger=list[0];
  for &item in list.iter(){
    if item>larger{
      larger=item;
    }
  }
  larger
}
fn main(){
  let number_list = vec![1,23,324,23,4235];
  let max_num = largest(&number_list);
}
```

- 结构体

```rust
#[derive(Debug)]
struct Point<T>{
  x:T,
  y:T,
}
#[derive(Debug)]
struct Point2d<T,U>{
  x:T,
  y:U,
}
fn main(){
  let p1=Point{x:1,y:2};
  println!("{:#?}",p1);
}
```

- 枚举

```rust
enum Option<T>{
  Some(T),
  None,
}
enum Result<T,E>{
  Ok(T),
  Err(E),
}
```

- 方法

```rust
struct Point<T>{
  x:T,
  y:T,
}
impl<T> Point<T>{
  fn get_x(&self)->&T{
    $self.x
  }
  fn get_y(&self)->&T{
    $self.y
  }
}
fn main(){
  let p=Point(x:1,y:2);
  println!("x={},y={}",p.get_x(),p.get_y());
}
```

# 特征trait

- trait 告诉 Rust 编译器某个特定类型拥有可能与其他类型共享的功能。类似于接口interface
- 通过trait以抽象的方式定义共享的行为
- trait bounds指定泛型是任何拥有特定行为的类型

## 使用

- 定义

```rust
pub trait GetInfo{
  // 特征定义
  fn get_name(&self)->&String;
  fn get_age(&self)->u32;
}
```

- 实现

```rust
#[derive(Debug)]
pub struct Student{
  pub name:String,
  pub age:u32,
}
impl GetInfo for Student{
  // 特征函数的具体实现
  fn get_name(&self)->&String{
    &self.name
  }
  fn get_age(&self)->u32{
    self.age
  }
}
fn main(){
  let s=Student{name:"xiao".to_string(),age=13};
  println!("name={},age={}",s.get_name(),s.get_age());
}
```

## 多态：trait作为参数

```rust
fn print_info(item:impl GetInfo){
  println!("name={},age={}",item.get_name(),item.get_age());
}
```

## 默认实现
- 定义trait时候提供默认行为，trait的类型可以使用默认的行为

```rust
trait SchoolName{
  // 在接口中直接实现
  fn get_school_name(&self)->String{
    String::from("school")
  }
}
// 默认实现
// impl SchoolName for Student{}
// 重载实现   
impl SchoolName for Student{   
  fn get_school_name(&self)->String{
    String::from("22 school")
  }
}
fn main(){
  let s=Student{name:"xiao".to_string(),age=13};
  let s_name = s.get_school_name();
}
```

## 限定范围的特征——trait_bound

- rust的一种语法糖，由于rust没有继承，所以通过trait_bound来限制类型间共享的功能。

```rust
// fn print_info(item:impl GetInfo){  //直接作为参数
fn print_info<T:GetInfo>(item:T){  //使用trait_bound
  // 限定T需要实现GetInfo这个trait，item满足trait
  println!("name={},age={}",item.get_name(),item.get_age());
}
```

- 指定多个trait bound

```rust
// 定义trait
trait GetName{
  fn get_name(&self) -> &String;
}
trait GetAge{
  fn get_age(&self) -> u32;
}
// 写法一
fn print_info<T:GetName+GetAge>(item:T){  //使用trait_bound
  println!("name={},age={}",item.get_name(),item.get_age());
}
// 写法二
fn print_info<T>(item:T) where T:GetName+GetAge // where 可以换行写，这样函数声明比较简洁
{  
  println!("name={},age={}",item.get_name(),item.get_age());
}
pub struct Student{
  pub name:String,
  pub age:u32,
}
impl GetName for Student{
  fn get_name(&self)->&String{
    &self.name
  }
}
impl GetAge for Student{
  fn get_age(&self)->u32{
    self.age
  }
}
fn main(){
  let s=Student{name:"xiao".to_string(),age:13};
  print_info(s);
}
```

## 返回trait类型

- 作为返回值的类型一定是确定的，不同类型具有trait特征但不能通过ifelse同时做返回值。

```rust
fn produce()->impl GetAge{
  if true{
    Student {
        name:String::from("xiao"),
        age:15,
    }
  }else{
    Teacher {  // error 返回值和Student类型不匹配
        name:String::from("xiao"),
        age:15,
    }    
  }
}
fn main(){
  let s=produce();
}
```

## trait bound有条件的实现方法

```rust
trait GetName{
  fn get_name(&self) -> &String;
}
trait GetAge{
  fn get_age(&self) -> u32;
}
struct People<T,U>{ // 声明people
  master:T,
  student:U,
}
// 绑定特征，有条件的实现方法
impl<T:GetName+GetAge,U:GetName+GetAge>People<T,U>{
  fn get_info(&self){
    println!("master name={},age={}",self.master.get_name(),self.master.get_age());
    println!("student name={},age={}",self.student.get_name(),self.student.get_age());
  }
}
struct Teacher{
  name:String,
  age:u32,
}
impl GetName for Teacher{
  fn get_name(&self)->&String{
    &(self.name)
  }
}
impl GetAge for Teacher{
  fn get_age(&self)->u32{
    self.age
  }
}
struct Student{
  name:String,
  age:u32,
}
impl GetName for Student{
  fn get_name(&self)->&String{
    &(self.name)
  }
}
impl GetAge for Student{
  fn get_age(&self)->u32{
    self.age
  }
}
fn main(){
  let s=Student{name:"xiao".to_string(),age:12};
  let t=Teacher{name:"xet".to_string(),age:25};
  let m=People{master:t,student:s};
  m.get_info();
}
```

## 有条件的实现trait
- 根据一个特征，绑定另一个特征。类似c++中的继承

```rust
trait GetName{
  fn get_name(&self) -> &String;
}
trait PrintName{
  fn print_name(&self);
}
impl<T:GetName>PrintName for T{
// 定义泛型，指定GetName特征，绑定PrintName
  fn print_name(&self){
    println!("name={}",self.get_name());
  }
}
struct Student{
  name:String,
}
impl GetName for Student{  // 实现了GetName，结合impl也就实现了PrintName方法
  fn get_name(&self)->&String{
    &(self.name)
  }
}

fn main(){
  let s=Student{name:"xiao".to_string()};
  s.print_name();
}
```

# 生命周期
- rust每一个引用都有生命周期，超出作用域则被释放
- 生命周期主要目标是避免悬垂引用
- 编译器使用**借用检查**来查看生命周期是否有效。

## 函数声明周期声明
- 防止悬停引用，也需要将参数的生命周期定义给返回值。
- 用自己指定的生命周期替换rust默认的生命周期

```rust
// fn longest(x:&str,y:&str)->x:&str{  //error生命周期要声明
fn longest<'a>(x:&'a str,y:&'a str)->&'a str{ // 'a也是一种泛型
  if x.len()>y.len(){
    x  // 局部变量的生命周期短，退出被销毁。所以需要手动声明
  }else{
    y
  }
}
fn main(){
  let s1=String::from("abc");
  let s2=String::from("ab");
  let r =longest(s1.as_str(),s2.as_str());
  println!("r={}",r);
}
```

## 结构体生命周期

```rust
#[derive(Debug)]
struct Student<'a>{
  name:&'a str,
}
fn main(){
  let n=String::from("a");
  let s=Student{name:&n};
  println!("{:#?}",s);
}
```

## 生命周期省略

- 早期rust必须显式声明声明周期，后来将明确的进行了简化。
- 函数或者方法的参数的声明周期成为<font color="red">输入声明周期</font>，返回值生命周期成为<font color="red">输出声明周期</font>。
- **三条省略规则**适用于fn和impl块的定义：
  - 每个引用的参数都有自己的声明周期参数。
    - 换句话说就是，有一个引用参数的函数有一个生命周期参数。`fn foo<'a>(x: &'a i32)`。
    - 有两个引用参数的函数有两个不同的生命周期参数，`fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`
    - 以此类推
  - 如果只有一个输入声明周期，那么它被赋予所有输出生命周期参数；
    - `fn foo(x:&i32)->&i32`等价于 `fn foo<'a>(x:&'a i32)->&'a i32`
  - 如果方法有多个输入生命周期参数，不过其中之一因为方法的缘故为&self或者&mut self，那么self的声明周期被赋予所有输出生命周期参数；
    - `fn foo(&self, x:&str)->&str`

```rust
fn get_str(s:&str)->&str{  // 可以省略生命周期的声名
  s
}
```

## 方法中的生命周期

```rust
struct Student<'a>{
  name:&'a str,
}
impl<'a> Student<'a>{
  fn get_name(&self)->&str{ //满足省略规则 输出返回的生命周期相同
    self.name
  }
  // fn get_name2(&self,s:&str)->&str{  // error,s和self是不同的生命周期，会报错
  fn get_name2<'a>(&self,s:&'a str)->&'a str{  // 需要显式标明生命周期
    s  
  }
}
fn main(){
  let name=String::from("hello");
  let s=Student{name:&name};
  println!("{}",s.get_name());
}
```

## 静态生命周期

- 定义方式：`'static`，静态周期存活于整个程序期间，所有字符字面值都拥有static生命周期

```rust
use std::fmt::Display;
fn fun<'a,T:Display>(x:&str,y:&str,ann:T)->&'a str{
  println!("ann is {}",ann);
  if x.len()<y.len(){
    x
  }else{
    y
  }
}
fn main(){
  let s:&'static str="hello";
  let s1=String::from("hello");
  let s2=String::from("hello111");
  let ann=129;
  let r=fun(s1.as_str(),s2.as_str(),ann);
  println!("r={}",r);
}
```

# 闭包

- 闭包可以保存到变量或者作为参数传递给其他函数的匿名函数。
- 闭包和函数的区别，闭包允许捕获调用者作用域中的值。

## 定义

- 格式：`fn fun = |x:type|->type{x+1};` 等价于 `fn fun(x:type)->type{x+1};`
- 简写：闭包会为每个参数和返回值类型进行推导
  - `fn fun = |x|{x+1};`
  - `fn fun = |x|x+1;`
- 闭包不能推导两次类型

```rust
let fun = |x|x;
let s1=fun(String::from("hello"));
let s2=fun(4); // 不能推导两次
```

```rust
fn main(){
  let use_closure=||{
    println!("this is closure");
  }
  use_closure();
}
```

## 闭包捕捉环境中的变量

```rust
fn main(){
  let x=4;
  let equal_x=|z|z==x;
  let y=4;
  assert!(equal_x(y));
}
```
## 闭包的trait
- 闭包有三种方式捕获环境，对应三种获取参数的方式
  - 获取所有权
  - 可变借用
  - 不可变借用(引用)
- 三种方式被编码为三个fn trait特征：
  - FnOnce：消费周围作用域的变量
  - FnMut：获取可变的借用值，可以改变变量
  - Fn：获取不可变的借用值

捕获环境方式trait|说明|例子
-|-|-
FnOnce|**获取所有权**：消费周围作用域的变量，将变量的所有权移入闭包中，once表示不能多次获取相同变量的所有权|`let equal_x2=move |z|z==x;`
FnMut|**可变借用**：获取可变的借用值，可以改变变量|
Fn|**不可变借用(引用)**：获取不可变的借用值|`let equal_x=|z|z==x;`

- 创建闭包时，rust会根据如何使用环境变量推断使用方式。所有闭包都实现了FnOnce，没有移动被捕获变量的所有权到闭包的也实现了FnMut，而不需要对捕获的变量进行可变访问的闭包实现了Fn。



```rust
fn main(){
  let x=vec![1,2,3];
  let equal_x=|z|z==x;
  let equal_x2=move |z|z==x; //此时x已经移入到闭包中了
  let y=[1,2,3];
  assert!(equal_x(y));
  assert!(equal_x2(y));
}
```

## 带有泛型的闭包

- 类似于回调函数

```rust
//实现一个缓存，只处理第一次传入的值并保存
struct Cache<T>
  where T:Fn(u32)->u32{
  calculation:T,
  value:Option<u32>,
}
impl<T> Cache<T>
  where T:Fn(u32)->u32{
  fn new(calculation:T)->Cache<T>{
    Cache{
      calculation,
      value:None,
    }
  }
  fn value(&mut self, arg:u32)->u32{
    match self.value{
      Some(v)=>v,
      None=>{
        let v=(self.calculation)(arg);
        self.value=Some(v);
        v
      },
    }
  }
}
fn main(){
  let mut c=Cache::new(|x|x+1);
  let v1=c.value(1);
  println!("v1={}",v1);
}
```


# 迭代器

## iterator trait
- 定义在标准库中，迭代器特征的定义方式如下：

```rust
trait Iterator{
  type Item;
  fn next(&mut self)->Option<Self::Item>; //type Item和Self::Item这种用法叫做定义trait的关联类型
  //next是要求实现的方法，一次返回一个元素，结束迭代器时返回None
}
```

## 使用

```rust
let v1=vec![1,2,3];
let mut v1_iter=v1.iter();//惰性，在使用迭代器之前，对v1不会有任何影响
// 需要定义为可变，因为trait中，next方法传的参数为mut引用
// for遍历
for val in v1_iter(){
  println!("val={}",val);
}
// 方式一使用next：
let mut v1_iter2=v1.iter();//惰性，在使用迭代器之前，对v1不会有任何影响
if let Some(v)=v1_iter2.next(){
  println!("v={}",v);
}
if let Some(v)=v1_iter2.next(){
  println!("v={}",v);
}
if let Some(v)=v1_iter2.next(){
  println!("v={}",v);
}
if let Some(v)=v1_iter2.next(){
  println!("v={}",v);
}else{
  println!("end");
}
// 方式二使用循环：
while let Some(v) = v1_iter2.next（）{
    print!("v={}", v);
}

//迭代可变引用
let mut v2=vec![1,2,3,4];
let mut v2_iter=v2.iter_mut();  // 迭代用
if let Some(v)=v2_iter.next(){
  *v=3;
}
println!("v2={:?}",v2);

//消费适配器
let v3=vec![1,2,3];
let v3_iter=v3.iter();
let total:i32=v3_iter.sum(); // 调用消费适配器sum求和
println!("total={}",total);

//迭代适配器map
let v4=vec![1,2,3];
let v4_:Vec<_>=v4.iter().map(|x|x+1).collect();
println!("v4_={:?}",v4_);

//迭代适配器filter
let v5=vec![1,12,23,2,32,5];
let v5_:Vec<_>=v5.into_iter().filter(|x|*x>10).collect();
println!("v5_={:?}",v5_);
```



## 自定义迭代器

```rust
struct Counter{
  count:u32,
}
impl Counter{
  fn new()->Counter{
    Counter{count:0}
  }
}
impl Iterator for Counter{
  type Item=u32;
  fn next(&mut self)->Option<Self::Item>{
    self.count+=1;
    if self.count<6{
      Some(self.count)
    }else{
      None
    }
  }
}
fn main(){
  let mut counter=Counter::new();
  for i in (0..6){
    if let Some(v)=counter.next(){
      println!("i={},v={}",i,v);
    }else{
      println!("i={},end",i);
      break;
    }
  }
}
```

 # 智能指针

- 智能指针是一类数据结构，他们的表现类似指针，但是也拥有额外的元数据和功能。Rust 标准库中不同的智能指针提供了多于引用的额外功能。例如：引用计数器。
- 在Rust中，引用是一类只借用数据的指针；智能指针在大部分情况下拥有他们指向的数据。

## 使用

- 智能指针通常使用结构体实现，并且需要实现两个特征Deref和Drop。
  - Deref trait：解引用，智能指针结构体实例表现的像引用一样，这样就可以编写既用于引用、又用于智能指针的代码。
  - Drop trait：析构，我们自定义当智能指针离开作用域时运行的代码。

## 标准库中最常用的智能指针

- Box`<T>`，用于在堆上分配值
- Rc`<T>`，一个引用计数类型，其数据可以有多个所有者
- Ref`<T>` 和 RefMut`<T>`，通过 RefCell`<T>` 访问。（ RefCell`<T>` 是一个在运行时而不是在编译时执行借用规则的类型）。

- Box`<T>`、Rc`<T>`或RefCell`<T>`区别
    - Rc`<T>`允许相同数据有多个拥有者，Box`<T>`和RefCell`<T>`有单一拥有者
    - Box`<T>`允许在编译时执行不可变或可变借用检查，Rc`<T>`仅允许在编译时执行不可变借用检查，RefCell`<T>`允许在运行时执行不可变或可变借用检查


## 堆上分配值的智能指针Box

- 最简单直接的智能指针是box，其类型是Box`<T>`。 box允许你将一个值放在堆上而不是栈上。留在栈上的则是指向堆数据的指针。
```rust
let b = Box::new(5); //b存储于栈上，5存储于堆上，b指向了5存储的位置
println!("b = {}", b);
```
- 除了数据被储存在堆上而不是栈上之外，box 没有性能损失。不过也没有很多额外的功能。
- box多用于如下场景：
  - 当有一个在编译时未知大小的类型，而又想要在需要确切大小的上下文中使用这个类型值的时候，例如string、vector
  - 当有大量数据并希望在确保数据不被拷贝的情况下转移所有权的时候
  - 当希望拥有一个值并只关心它的类型是否实现了特定 trait 而不是其具体类型的时候

- 链表
```rust
enum List {
    Cons(i32, List),
    Nil,
}
enum List2 {
    Cons(i32, Box<List>),
    Nil,
}
use crate::List::{Cons,Nil};
fn main() {

    // use List::Cons;
    // use List::Nil;
    // let list = Cons(1, Cons(2, Cons(3, Nil))); err 递归的时候没有确定的size，编译时List循环去查找要开辟的空间大小
    let list = Cons(1,Box::new(Cons(2,Box::new(Cons(3,Box::new(Nil)))))); // 使用box创建
}
```

## 解引用Deref Trait

- 类似于C语言，实现解引用必须实现Deref特征

```cpp
auto a=A::new(); // A必须实现Deref特征
auto b=&a;
auto c=*b; //解引用
```

- 使用解引用

```rust
fn main() {
    let x = 5;
    let y = &x;
    assert_eq!(5, x);
    assert_eq!(5, *y); // 解引用

    let z = Box::new(x); // 使用box解引用
    assert_eq!(5, *z);
}
```

- 自定义解引用

```rust
use std::ops::Deref;
struct MyBox<T>(T);
impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}
impl<T> Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &T {
        &self.0 // MyBox有一个字段，所以返回0
    }
}
fn main() {
    let x = 5;
    let y = MyBox::new(x);
    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```

- 解引用强制多态，Deref强制转换（deref coercions）是Rust在函数或方法传参上的一种便利。其将实现了Deref的类型的引用转换为原始类型通过Deref所能够转换的类型的引用。当这种特定类型的引用作为实参传递给和形参类型不同的函数或方法时，Deref强制转换将自动发生。这时会有一系列的deref方法被调用，把我们提供的类型转换成了参数所需的类型。

```rust
fn main() {
    let m = MyBox::new(String::from("Rust")); // string
    hello(&m); // mybox类型强制变为&string的解引用，变为字符串slice
}
fn hello(name: &str) { // string slice
    println!("Hello, {}!", name);
}
```

- Deref 强制转换如何与可变性交互，类似于如何使用 Deref trait 重载不可变引用的 * 运算符，Rust 提供了 DerefMut trait 用于重载可变引用的 * 运算符。
- 满足三种情况时会进行 Deref 强制转换5
  - 当 T: Deref<Target=U> 时从 &T 到 &U，不可变的T，到不可变的U
  - 当 T: DerefMut<Target=U> 时从 &mut T 到 &mut U，可变的T，到可变的U
  - 当 T: Deref<Target=U> 时从 &mut T 到 &U，可变的T，不可变的U
- 没有不可变的T，到可变的U。

## 析构Drop trait
- 在变量离开作用域时，执行析构函数的代码
- 在同一作用域中的变量先定义的后析构
- 显式调用析构函数是不允许的，rust提供了std::mem::drop()函数，可以提前释放资源

```rust
struct Dog{
    name: String,
    cnt: i32,
}
impl Drop for Dog{
    fn drop(&mut self){  // 一定是可变引用
        println!("Dog leave");
        self.count -= 1;
    }
}
fn main(){
    let a = Dog{name:String::from("121"),1};  // 先定义的后析构
    let b = Dog{name:String::from("121"),1};
    {
        let c = Dog{name:String::from("122"),1};
    }
    // b.drop();  显式调用析构函数是不允许的
    drop(b);
}
```

## 引用计数的只能指针Rc
- 多个变量共同拥有同一个所有权
- 通过Rc`<T>`允许程序的多个部分之间只读的共享数据，因为相同位置的多个可变引用可能会造成数据竞争。

- 实现一个链表
![](/images/pasted-399.png)

- 使用box指针无法实现多个变量同时引用
```rust
enum List{
    Cons(i32, Box<List>),
    Nil,
}
use crate::List::{Cons, Nil};
fn main(){
    let a=Cons(5,Box::new(Cons(10, Box::new(Nil))));
    let b=Cons(3,Box::new(a));
    let c=Cons(4,Box::new(a)); // error: use after move
}
```

- 使用Rc指针实现
```rust
enum List{
    Cons(i32, Rc<List>),
    Nil,
}
use crate::List::{Cons, Nil};
use std::rc::Rc;
fn main(){
    let a=Cons(5,Rc::new(Cons(10, Rc::new(Nil))));
    let b=Cons(3,Rc::clone(a)); // 第一种写法
    // let b=Cons(3,a.clone()); 第二种写法
    let c=Cons(4,Rc::clone(a)); // error: use after move
}
```

- Rc实现的原理
```rust
enum List{
    Cons(i32, Rc<List>),
    Nil,
}
use crate::List::{Cons, Nil};
use std::rc::Rc;
fn main(){
    let a=Cons(5,Rc::new(Cons(10, Rc::new(Nil))));
    println!("count after creating a={}", Rc::strong_count(&a));
    let b=Cons(3,Rc::clone(a));
    println!("count after b a={}", Rc::strong_count(&a));
    {
        let c=Cons(4,Rc::clone(a)); // error: use after move
        println!("count after c a={}", Rc::strong_count(&a));
    }
    println!("count after end a={}", Rc::strong_count(&a));
}
```

## 可借用的智能指针RefCell
- 内部可变性：允许在使用不可变引用时改变
- 通过RefCell`<T>`在运行时检查借用规则（通常情况下，是在编译时检查借用规则），RefCell`<T>`代表其数据的唯一所有权
- 类似于Rc`<T>`，RefCell`<T>`只能用于单线程场景
- 因为RefCell`<T>`允许在运行时执行可变借用检查，固可以在即便RefCell`<T>`自身是不可变的情况下修改其内部的值

```rust
#[derive(Debug)]
enum List{
    Cons(Rc<RcCell<i32>>, Rc<List>),
    Nil,
}
use crate::List::{Cons, Nil};
use std::rc::Rc;
use std::cell::RcCell;
fn main(){
    let value = Rc::new(RefCell::new(5));
    let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));
    let b = Cons(Rc::new(RefCell::new(6)), Rc::clone(&a));
    let c = Cons(Rc::new(RefCell::new(7)), Rc::clone(&a));
    println!("a:{:?}", a);
    println!("a:{:?}", b);
    println!("a:{:?}", c);
    *value.borrow_mut() += 10;
    println!("a:{:?}", a);
    println!("a:{:?}", b);
    println!("a:{:?}", c);
}
```

## 弱引用
### 引用循环
- 引用循环会造成程序的异常，例如无限的查找导致的堆栈溢出，以及释放后引用计数更新后没有减为0，导致的悬停引用。

```rust
#[derive(Debug)]
enum List{
    Cons(i32, RefCell<Rc<List>>),
    Nil,
}
impl List{
    fn tail(&self) -> Option<&RefCell<Rc<List>>>{
        match self{
            Cons(_, item) => Some(item),
            Nil => None,
        }
    }
}
use std::rc::Rc,
use crate::List::{Cons, Nil},
use std::cell::RefCell;
fn main(){
    let a = Rc::new(Cons(5, RefCell::new(Rc::new(Nil))));
    println!("a rc count {}", Rc::strong_count(&a));
    println!("a tail {:?}", a.tail());
    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));
    println!("a rc count {}", Rc::strong_count(&a));
    println!("b rc count {}", Rc::strong_count(&b));
    println!("b tail {:?}", b.tail());
    if let Some(link) = a.tail(){  // a的尾指向了b
        *link.borrow_mut() = Rc::clone(&b);
    }
    println!("a rc count {}", Rc::strong_count(&a));
    println!("b rc count {}", Rc::strong_count(&b));
    println!("b tail {:?}", b.tail()); // error，打印tail就会溢出，因为循环引用
}
```

### 弱引用Weak<T>
- weak解决了循环引用的问题，还能实现a->b->a，使用`*(&a.tail().unwrap()).take().upgrade()`可以取出末尾值。
- 特点：
    - 弱引用通过Rc::downgrade传递Rc实例的引用，调用Rc::downgrade会得到Weak`<T>`类型的智能指针，同时将weak_count加1，不是将strong_count加1；
    - weak_count无需计数为0就能使Rc实例被清理，只要将strong_count为0就可以；
    - 可以通过Rc::upgrade方法返回Option`<Rc<T>>`对象；

```rust
enum List{
    Cons(i32, RefCell<Weak<List>>),
    Nil,
}
impl List{
    fn tail(&self)->Option<&RefCell<Weak<List>>>{
        match self{
            Cons(_, item) => Some(item),
            Nil => None,
        }
    }
}
use std::rc::Rc,
use std::rc::Weak,
use crate::List::{Cons, Nil},
use std::cell::RefCell;
fun main(){
    let a = Rc::new(Cons(5, RefCell::new(Weak::new(Nil))));
    println!("a strong count={}, weak count", Rc::strong_count(&a), Rc::weak_count(&a));
    println!("a tail = {:?}", a.tail());
    let b = Rc::new(Cons(10, RefCell::new(Weak::new())));
    if let Some(link)=b.tail(){
        *link.borrow_mut() = Rc::downgrade(&a);
    }
    println!("a strong count={}, weak count", Rc::strong_count(&a), Rc::weak_count(&a));
    println!("b strong count={}, weak count", Rc::strong_count(&b), Rc::weak_count(&b));
    println!("b tail = {:?}", a.tail());
    if let Some(link)=a.tail(){
        *link.borrow_mut() = Rc::downgrade(&b);
    }
    println!("a strong count={}, weak count", Rc::strong_count(&a), Rc::weak_count(&a));
    println!("b strong count={}, weak count", Rc::strong_count(&b), Rc::weak_count(&b));
    println!("b tail = {:?}", a.tail());
}
```

### 树形结构
- 使用weak实现树形结构

```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell,
#[derive(Debug)]
struct Node{
    value: i32,
    parent: RefCell<Weak<Node>>,
    child: RefCell<Vec<Rc<Node>>>,
}

fn main(){
    let leaf = Rc::new(Node{
        value: 3,
        parent: RefCell::new(Weak::new()),
        child: RefCell::new(vec![]),
    });
    println!("leaf parent={:?}", leaf.parent.borrow().upgrade());
    println!("leaf strong count={}, weak count={}", Rc::strong_count(&leaf), Rc::weak_count(&leaf))
    let branch = Rc::new(Node{
        value: 5,
        parent: RefCell::new(Weak::new()),
        child: RefCell::new(vec![Rc::clone(&leaf)]),
    });
    *leaf.parent.borrow_mut()=Rc::downgrade(&branch);
    println!("leaf parent={:?}", leaf.parent.borrow().upgrade());
}
```


# 多线程
## 线程
- 进程是资源分配的最小单位，线程是CPU调度的最小单位
- 在使用多线程时，会遇到一些问题：
    1. 竞争状态：多个线程以不一致的顺序访问数据或资源；
    2. 死锁：两个线程相互等待对方停止使用其所拥有的资源，造成两者都永久等待；
    3. 特定情况下且难以重现和修复的bug
- 编程语言实现的线程叫做绿色线程，例如go实现的M:N模型，M个绿色线程对应N个OS线程。Rust只提供了1:1线程模型
- 运行时：代表二进制文件中包含的由语言本身提供的代码，这些代码根据语言的不同可大可小，不过非汇编语言都会有一定数量的运行时代码。通常，大家说一个语言“没有运行时”，是指这个语言的“运行时”很小。Rust、C都是几乎没有运行时的。

- 创建线程
```rust
use std::thread;
use std::time::Duration;
fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("number {} in spawn thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });
    handle.join().unwrap();  // thread join
    println!("+++++++++++++++++++++");
    for i in 1..5 {
        println!("number {} in main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
}
```

- 
```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    // let handle = thread::spawn(|| {  error 因为v可能在主线程会被释放，子线程会产生安全风险
    let handle = thread::spawn(move || { // 使用move移进子线程
        println!("v: {:?}", v);
    });
    // drop(v);
    // println!("in main thread, v: {:?}", v); // 主线程没有v的使用权了
    handle.join().unwrap();
    println!("Hello, world!");
}
```

## 线程间消息传递
### 通道
- 1、Rust中一个实现消息传递并发的主要工具是通道。通道由两部分组成，一个是发送端，一个是接收端，发送端用来发送消息，接收端用来接收消息。发送者或者接收者任一被丢弃时就可以认为通道被关闭了。
- 2、通道介绍
    - 通过mpsc::channel，创建通道，mpsc是多个生产者，单个消费者；
    - 通过spmc::channel，创建通道，spmc是一个生产者，多个消费者；
- 创建通道后返回的是发送者和消费者，示例：
```rust
let (tx, rx) = mpsc::channel();
let (tx, rx) = spmc::channel();
```

```rust
use std::thread;
use std::sync::mpsc;
fn main() {
    let (tx, rx) = mpsc::channel();
    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
        println!("val = {}", val); //error 调用send的时候，会发生move动作，所以此处不能再使用val
    });
    let received = rx.recv().unwrap();  // 等待
    println!("Got: {}", received);
}
// 循环发送消息
fn main1() {
    let (tx, rx) = mpsc::channel();
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });
    for recv in rx {
        println!("Got: {}", recv);
    }
}
// 多个生产者
fn main2() {
    let (tx, rx) = mpsc::channel();
    let tx1 = mpsc::Sender::clone(&tx);  // 也可以：tx.clone()
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });
    thread::spawn(move || {
        let vals = vec![
            String::from("A"),
            String::from("B"),
            String::from("C"),
            String::from("D"),
        ];
        for val in vals {
            tx1.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });
    for rec in rx {
        println!("Got: {}", rec);
    }
}
```
- 1、发送者的send方法返回的是一个Result`<T,E>`,如果接收端已经被丢弃了，将没有发送值的目标，此时发送会返回错误。
- 2、接受者的recv返回值也是一个Result`<T,E>`类型，当通道发送端关闭时，返回一个错误值。
- 3、接收端这里使用的recv方法，会阻塞到有一个消息到来。我们也可以使用try_recv()，不会阻塞，会立即返回。

### 互斥器mutex
- 通道类似于单所有权的方式，值传递到通道后，发送者就无法再使用这个值；
- 共享内存类似于多所有权，即多个线程可以同时访问相同的内存位置。

- 互斥器：mutex
    - 任意时刻，只允许一个线程来访问某些数据;
    - 互斥器使用时，需要先获取到锁，使用后需要释放锁。
```rust
use std::sync::Mutex;
fn main() {
    let m = Mutex::new(5);
    {
        let mut num = m.lock().unwrap();
        *num = 6;
    }//离开作用域时，自动释放
    println!("m = {:?}", m);
}
```
- Mutex`<T>`是一个智能指针，lock调用返回一个叫做MutexGuard的智能指针
- Mutex`<T>`内部提供了drop方法，实现当MutexGuard离开作用域时自动释放锁。

#### Arc`<T>`多线程使用Mutex
- RefCell`<T>`/Rc`<T>`与Mutex`<T>`/Arc`<T>`区别
    - Mutex`<T>`提供内部可变性，类似于RefCell
    - RefCell`<T>`/Rc`<T>`是非线程安全的， Mutex`<T>`/Arc`<T>`是线程安全的

```rust
use std::sync::Mutex;
use std::thread;
use std::rc::Rc;
use std::sync::Arc;
fn main_error1() {
   let counter = Mutex::new(0);
   let mut handles = vec![];
   for _ in 0..10 {
       let handle = thread::spawn(move || {  // error counter不能move到多个线程中
           let mut num = counter.lock().unwrap();*num += 1;});
       handles.push(handle);}
   for handle in handles {handle.join().unwrap();}
   println!("resut = {}", *counter.lock().unwrap());
}
fn main_error2() {
   //Rc<T> 不是线程安全的
   //let counter = Mutex::new(0);
   let counter = Rc::new(Mutex::new(0));
   let mut handles = vec![];
   for _ in 0..10 {
       let cnt = Rc::clone(&counter);
       let handle = thread::spawn(move || {let mut num = cnt.lock().unwrap();*num += 1;});  // error
       handles.push(handle);}
   for handle in handles {handle.join().unwrap();}
   println!("resut = {}", *counter.lock().unwrap());
}
fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];
    for _ in 0..10 {
        let cnt = Arc::clone(&counter);
        let handle = thread::spawn(move || {let mut num = cnt.lock().unwrap();*num += 1;});
        handles.push(handle);}
    for handle in handles {
        handle.join().unwrap();
    }
    println!("resut = {}", *counter.lock().unwrap());
}
```
## 并发：Sync和Send trait
- 有两个并发概念内嵌于语言中：std::marker中的Sync和Send trait。
### send
- 通过Send允许在线程间转移所有权
    - Send标记trait表明类型的所有权可以在线程间传递。几乎所有的Rust类型都是Send的，但是例外：例如Rc`<T>`是不能Send的。
    - 任何完全由Send类型组成的类型也会自动被标记为Send。
```rust
struct A {
	a
	b
	c
}
```
### Sync
- Sync允许多线程访问
    - Sync 标记 trait 表明一个实现了 Sync 的类型可以安全的在多个线程中拥有其值的引用，即，对于任意类型 T，如果 &T（T 的引用）是 Send 的话 T 就是 Sync 的，这意味着其引用就可以安全的发送到另一个线程。
    - 智能指针 Rc`<T>` 也不是 Sync 的，出于其不是 Send 相同的原因。RefCell`<T>`和 Cell`<T>` 系列类型不是 Sync 的。RefCell`<T>` 在运行时所进行的借用检查也不是线程安全的，Mutex`<T>` 是 Sync 的。

### 手动实现Send和Sync是不安全的
- 通常并不需要手动实现 Send 和 Sync trait，因为由 Send 和 Sync 的类型组成的类型，自动就是 Send 和 Sync 的。因为他们是标记 trait，甚至都不需要实现任何方法。他们只是用来加强并发相关的不可变性的。`


## 线程池



# 面向对象
- 封装继承多态
## rust中的对象
- 对象：数据和操作数据的过程
- Rust里面，结构体、枚举类型加上impl块

```rust
struct Dog {
    name: String,
}
impl Dog {
    fn print_name(&self) {
        println!("Dog name = {}", self.name);
    }
}
fn main() {
    let d = Dog{name: String::from("wangcai")};
    d.print_name();
}
```

## 封装

- 配置引用
```bash
cargo new main
cargo new getaver --lib
# vim Cargo.toml
[workspace]
members = [
    "getaver",
    "main"
]
# vim main/Cargo.toml
[dependencies]
getaver = {path = "../getaver"}
```

- 定义类，不是pub类型则为私有的
```rust
// vim getaver/src/lib.rs
pub struct AverCollect {
    list: Vec<i32>,
    aver: f64,
}
impl AverCollect {
    pub fn new() -> AverCollect {
        AverCollect{
            list: vec![],
            aver: 0.0,
        }
    }
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }
    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            },
            None => None,
        }
    }
    pub fn average(&self) -> f64 {
        self.aver
    }
    fn update_average(&mut self) {  // 私有的
        let total: i32 = self.list.iter().sum();
        self.aver = total as f64 / self.list.len() as f64;
    }
}
```

- 定义主函数
```rust
use getaver;
fn main() {
    let mut a = getaver::AverCollect::new();
    a.add(1);
    println!("average = {}", a.average());
    a.add(2);
    println!("average = {}", a.average());
    a.add(3);
    println!("average = {}", a.average());
    a.remove();
    println!("average = {}", a.average());
}
```

## 继承
- Rust里面没有继承的概念，可以通过tait来进行行为共享
```rust
trait A {
	fn sum() {
		//todo
	}
}
struct XXX {}
impl A for XXX {}
```

### 多态：trait对象——dyn
- 使用泛型是无法实现多态的，因为只要被调用，泛型已经确定，无法传入其他类型
- 使用dyn显式指明trait实现的对象
- 定义工作空间
```bash
cargo new main
cargo new gui --lib
# vim Cargo.toml
[workspace]
members = [
	"gui",
	"main",
]
# vim main/Cargo.toml
[dependencies]
getaver = {path = "../gui"}
```
- gui图像库的代码
```rust
pub trait Draw {
    fn draw(&self);
}
pub struct Screen {
    pub components: Vec<Box<dyn Draw>>, //trait对象，使用dyn关键字
}
impl Screen {
    pub fn run(&self) {
        for comp in self.components.iter() {
            comp.draw();
        }
    }
}
pub struct Button {
    pub width: u32,
    pub height: u32,
    pub label: String,
}
impl Draw for Button {
    fn draw(&self) {
        println!("draw button, width = {}, height = {}, label = {}", self.width, self.height, self.label);
    }
}
pub struct SelectBox {
    pub width: u32,
    pub height: u32,
    pub option: Vec<String>,
}
impl Draw for SelectBox{
    fn draw(&self) {
        println!("draw selectBox, width = {}, height = {}, option = {:?}", self.width, self.height, self.option);
    }
}
//pub struct Screen<T: Draw> {  // 泛型实现
//    pub components: Vec<T>,
//}
//impl<T> Screen <T>  // 泛型实现
//    where T: Draw {
//    pub fn run(&self) {
//        for comp in self.components.iter() {
//            comp.draw();
//        }
//    }
//}
```

- 主函数

```rust
use gui::{Screen, Button, SelectBox};
fn main() {
    let s = Screen {
        components: vec![
            Box::new(Button {
                width: 50,
                height: 10,
                label: String::from("ok"),
            }),
            Box::new(SelectBox {
                width: 60,
                height: 40,
                option: vec![
                    String::from("Yes"),
                    String::from("No"),
                    String::from("MayBe"),
                ],
            }),
        ],
    };
    //let s = Screen {
    //    components: vec![
    //        Button {
    //            width: 50,
    //            height: 10,
    //            label: String::from("ok"),
    //        },
    //        SelectBox {
    //            width: 60,
    //            height: 40,
    //            option: vec![
    //                String::from("Yes"),
    //                String::from("No"),
    //                String::from("MayBe"),
    //            ],
    //        },
    //    ],
    //};
    s.run();
}
```

### trait对象动态分发

1. trait对象动态分发
    - 在上述例子中，对泛型类型使用trait bound编译器进行的方式是单态化处理，单态化的代码进行的是静态分发（就是说编译器在编译的时候就知道调用了什么方法）。
    - 使用 trait 对象时，Rust 必须使用动态分发。编译器无法知晓所有可能用于 trait 对象代码的类型，所以它也不知道应该调用哪个类型的哪个方法实现。为此，Rust 在运行时使用 trait 对象中的指针来知晓需要调用哪个方法。
2. trait对象要求对象安全：只有 对象安全（object safe）的 trait 才可以组成 trait 对象。trait的方法满足以下两条要求才是对象安全的：
    - 返回值类型不为 Self
    - 方法没有任何泛型类型参数

# 模式
- 模式是Rust中特殊的语法，模式用来匹配值的结构。
- 模式由如下内容组成：
    1. 字面值
    2. 解构的数组、枚举、结构体或者元组
    3. 变量
    4. 通配符
    5. 占位符
## 模式的定义
### match
```rust
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
```
- **必须要匹配完所有的情况**
```rust
fn main() {
   let a = 1;    
   match a {
       0 => println!("Zero"),
       1 => println!("One"),
       _ => println!("other"),   // 其他情况
   };
   println!("Hello, world!");
}
```

### if let
```rust
fn main() {
   let color: Option<&str> = None;
   let is_ok = true;
   let age: Result<u8, _> = "33".parse();
   if let Some(c) = color {
       println!("color: {}", c);
   } else if is_ok {
       println!("is ok");
   } else if let Ok(a) = age {
       if a > 30 {
           println!("oh, mature man");
       } else {
           println!("oh, young man");
       }
   } else {
       println!("in else");
   }
}
```
### while let
- 只要模式匹配就一直执行while循环

```rust
fn main() {
   let mut stack = Vec::new();
   stack.push(1);
   stack.push(2);
   stack.push(3);
   while let Some(top) = stack.pop() {
       println!("top = {}", top);
   }//只要匹配Some(value),就会一直循环
}
```

### for
- 在for循环中，模式是直接跟随for关键字的值，例如 for x in y，x就是对应的模式
```rust
fn main() {
   let v = vec!['a', 'b', 'c'];
    //此处的模式是(index, value)
   for (index, value) in v.iter().enumerate() {
       println!("index: {}, value: {}", index, value);
   }
}
```
### let语句
- 格式：`let PATTERN = EXPRESSION`

```rust
fn main() {
   let (x, y, z) = (1, 2, 3); //(1, 2, 3)会匹配(x, y, z)，将1绑定到x，2绑定到y，3绑定到z
   println!("{}, {}, {}", x, y, z);
   let (x, .., z) = (1, 2, 3);  // 忽略，_表示一个值，..表示多个值
   println!("{}, {}", x, z);
}

```

### 函数
- 函数的参数也是模式
```rust
fn print_point(&(x, y): &(i32, i32)) {
    println!("x: {}, y: {}", x, y);
}
fn main() {
    let p = (3, 5);//&(3, 5) 匹配模式 &(x, y)
    print_point(&p);
}
```

## 模式的类型
- 模式存在不可反驳的和可反驳的
- 1、模式有两种：refutable（可反驳的）和 irrefutable（不可反驳的）。能匹配任何传递的可能值的模式被称为是不可反驳的。对值进行匹配可能会失败的模式被称为可反驳的。
- 2、只能接受不可反驳模式：函数、let语句、for循环。因为通过不匹配的值程序无法进行有意义的工作。
- 3、只能接受可反驳的模式：if let和while let表达式，因为它们的定义就是为了处理有可能失败的条件。
```rust
fn main() {
    //let a: Option<i32> = Some(5); //匹配Some(value), None
    //let b: Option<i32> = None; //匹配Some(value), None
    ////let Some(x) = a;  error
    //if let Some(v) = a {  correct
    if let v = 5 {  //不可反驳的模式，却使用了if let会报警告
        println!("v {}", v);
    }

    println!("Hello, world!");
}
```
## 模式匹配
### 匹配类型
1. 匹配字面值
```rust
fn main() {
   let x = 1;
   match x {
       1 => println!("one"),
       2 => println!("two"),
       _ => println!("xx"),
   };
}
```
2. 匹配命名变量
```rust
fn main() {
   let x = Some(5);
   let y = 10; //位置1
   match x {
       Some(50) => println!("50"),
       Some(y) => println!("value = {}", y), //此处的y不是位置1的y, 覆盖了花括号外面的y
       _ => println!("other"),
   };
   println!("x = {:?}, y = {:?}", x, y); //此处的y是位置1的y
}
```
3. 多个模式
```rust
fn main() {
   let x = 1;
   match x {
       1|2 => println!("1 or 2"), //|表示是或，匹配1或者2
       3 => println!("3"),
       _ => println!("xx"),
   };
}
```
4. 通过..匹配
```rust
fn main() {
    let x = 5;
    match x {
       1..=5 => println!("1 to 5"), // 等价于： 1|2|3|4|5 => println!("1 to 5"),
       _ => println!("xx"),
    };
    let x = 'c';
    match x {
        'a'..='j' => println!("1"),
        'k'..='z' => println!("2"),
        _ => println!("other"),
    }
}
```
## 解构并分解值
- 解构元祖、结构体、枚举、引用
### 解构结构体
```rust
struct Point {
    x: i32,
    y: i32,
}
fn main() {
    let p = Point{x: 1, y: 0};
    let Point{x: a, y: b} = p;//变量a和变量b匹配x和y
    // a=1,b=0
    let Point{x, y} = p;
    // 同名的：x=1,y=0
    match p {
        Point{x, y: 0} => println!("x axis"),
        Point{x: 0, y} => println!("y axis"),
        Point{x, y} => println!("other"),
    };
}
```

### 解构枚举类型
```rust
enum Message {
    Quit,
    Move{x: i32, y: i32},
    Write(String),
    ChangeColor(i32, i32, i32),
}
fn main() {
    let msg = Message::ChangeColor(0, 160, 255);
    match msg {
        Message::Quit => println!("quit"),
        Message::Move{x, y} => println!("move, x: {}, y: {}", x, y),
        Message::Write(text) => println!("write msg: {}", text),
        Message::ChangeColor(r, g, b) => println!("clolor, r: {}, g: {}, b: {}", r, g, b),
    };
}
```

### 解构嵌套的结构体和枚举
```rust
enum Color {
    Rgb(i32, i32, i32),
    Hsv(i32, i32, i32),
}
enum Message {
    Quit,
    Move{x: i32, y: i32},
    Write(String),
    ChangeColor(Color),
}
fn main() {
    let msg = Message::ChangeColor(Color::Hsv(0, 160, 255));
    match msg {
        Message::ChangeColor(Color::Rgb(r, g, b)) => println!("rgb color, r: {}, g: {}, b: {}", r, g, b),
        Message::ChangeColor(Color::Hsv(h, s, v)) => println!("hsv color, h: {}, s: {}, v: {}", h, s, v),
        _ => ()
    };
}
```

### 解构结构体和元组混合
```rust
struct Point{
    x: i32, 
    y: i32,
}
fn main() {
    let ((a, b), Point{x, y}) = ((1, 2), Point{x: 3, y: 4});
    println!("a: {}, b: {}, x: {}, y: {}", a, b, x, y);
}
```

## 忽略模式中的值
- 下划线_忽略
    - match中的选项
    - 函数中的参数
    - 多个值的忽略
- 变量前增加下划线_，可以忽略未使用的警告，但是还是会有所有权的移动；
- 使用单个下划线_，没有发生所有权的移动；
- 使用..忽略

```rust
fn foo(_: i32, y: i32) {
   println!("y = {}", y);
}
trait A {
   fn bar(x: i32, y: i32);
}
struct B {}
impl A for B {
   fn bar(_: i32, y: i32) {
       println!("y = {}", y);
   }
}
fn main() {
    foo(1, 2);
    let numbers = (1, 2, 3, 4);
    match numbers {
        (one, _, three, _) => println!("one: {}, three: {}", one, three),
    }
    let _x = 5;
    let _y = 5;

    let s = Some(String::from("hello"));
    if let Some(_c) = s {
    //if let Some(c) = s {
       println!("found a string");
    }
    //println!("s: {:?}", s);
    
    let s = Some(String::from("hello"));
    if let Some(_) = s {
        println!("found a string");
    }
    println!("s: {:?}", s);  // 仍可以打印
}
```

```rust
    let numbers = (1, 2, 3, 4, 5, 6, 7);
    match numbers {
        (first, .., last) => println!("first: {}, last: {}", first, last),
    };
    //match numbers {
    //    (.., second, ..) => {  // 不能这么使用，不能有歧义
    //        println!("{}", second);
    //    },
    //};
    println!("Hello, world!");
```


## 匹配守卫提供额外的条件
- 匹配守卫是一个指定于match分之模式之后的额外的if条件，必须满足才能选择此分支。

```rust
fn main() {
    let num = Some(4);
    match num {
        Some(x) if x < 5 => println!("< 5"),
        Some(x) => println!("x: {}", x),
        None => (),
    };
    
    let num = Some(4);
    let y = 10; // 位置1
    match num {
        Some(x) if x == y => println!("num == y"), //此处的y是位置1处的y，使用之前的变量
        Some(x) => println!("x: {}", x),
        None => (),
    };

    let x = 4;
    let y = false;
    match x {
        4|5|6 if y => println!("1"),//4|5|6 if y  等价于((4|5|6) if y)
        _ => println!("2"),
    }
}
```

## @运算符
- @运算符允许我们在创建一个存放值的变量的同时，测试这个变量的值是否满足匹配模式。
```rust
enum Message {
    Hello{id: i32},
}
fn main() {
    let msg = Message::Hello{id: 25};
    match msg {
        Message::Hello{id: id_va @ 3..=7} => println!("id_va: {}", id_va),
        Message::Hello{id: 10..=20} => println!("large"),
        Message::Hello{id} => println!("id: {}", id),
    }
}
```

# 不安全的rust

- 在此节之前讨论过的都是安全的Rust，即Rust在编译时会强制执行的内存安全保证。不会强制执行这类内存安全保证的，就是不安全的Rust。
- 不安全的Rust存在的两大原因：
    1. 静态分析本质上是保守的，就意味着某些代码可能是合法的，但是Rust也会拒绝。在此情况下，可以使用不安全的代码。
    2. 底层计算机硬件固有的不安全性。如果Rust不允许进行不安全的操作，有些任务根本就完成不了。
- 不安全的Rust具有的超级力量：Rust会通过unsafe关键字切换到不安全的Rust。不安全的Rust具有以下超级力量：
    1. 解引用裸指针
    2. 调用不安全的函数或者方法
    3. 访问或修改可变静态变量
    4. 实现不安全的trait

> 注意：unsafe并不会关闭借用检查器或禁用任何其它的Rust安全检查规则，它只提供上述几个不被编译器检查内存安全的功能。unsafe也不意味着块中的代码一定就是不ok的，它只是表示由程序员来确保安全。


## 解引用裸指针
- 裸指针分为不可变和可变的，分别写作*const T, *mut T。其特性：
    1. 允许忽略借用规则，可以同时拥有不可变和可变的指针，或者是多个指向相同位置的可变指针
    2. 不保证指向的内存是有效的
    3. 允许为空
    4. 不能实现任何自动清理的功能
```rust
fn main() {
    let mut num = 5;
    //创建不可变和可变的裸指针可以在安全的代码中，只是不能在不安全代码块之外解引用裸指针
    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;
    unsafe {
        println!("r1 is: {}", *r1);
        println!("r2 is: {}", *r2);
    }
    let add = 0x12345usize;   // 地址是一个不安全的
    let _r = add as *const i32;
}
```

## 调用不安全的函数或者方法
```rust
unsafe fn dangerous() {
    println!("do something dangerous");
}
fn foo() {
    let mut num = 5;
    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;
    unsafe {
        println!("*r1 = {}", *r1);
        println!("*r2 = {}", *r2);
    }
}
fn main() {
    unsafe {
        dangerous();
    }
    //dangerous(); //不能在不安全块之外调用
    foo();
}
```

## 语言交互接口FFI
- rust调用c语言的函数
```rust
extern "C" {
    fn abs(input: i32) -> i32;
}
fn main() {
    unsafe {
        println!("abs(-3): {}", abs(-3));
    }
}
```

- c语言调用rust库
```bash
mkdir unsafe_c && cd unsafe_c
cargo new foo --lib
```
- rust代码
```rust
// vim foo/Cargo.toml
[lib]
name = "foo"
crate-type = ["staticlib"]

// vim foo/src/lib.rs
#![crate_type = "staticlib"]
#[no_mangle]
pub extern fn foo() {
    println!("use rust");
}
// cargo build -> 在debug中生成了libfoo.a的库文件
```

- c语言代码
```c
// vim main.c
extern void foo();
int main() {
	foo();
	return 0;
}
// gcc -o main main.c libfoo.a -lpthread -ldll
```

## 访问或者修改可变静态变量
- 全局变量在rust就称为静态变量
```rust
static HELLO_WORLD: &str = "Hello, world";
fn main() {
    println!("{}", HELLO_WORLD);
}
```

- 静态变量和常量的区别：
    1. 静态变量有一个固定的内存地址（使用这个值总会访问相同的地址），常量则允许在任何被用到的时候复制其数据。
    2. 静态变量可以是可变的，虽然这可能是不安全（用unsafe包含）

- 可变静态变量
```rust
static mut COUNTER: u32 = 0;
fn add_counter(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}
fn main() {
    add_counter(3);
    add_counter(3);
    unsafe {
        println!("counter: {}", COUNTER);
    }
}
```

## 实现不安全的trait
1. 当至少有一个方法中包含编译器不安全的变量时，该trait就是不安全的。
2. 在trait之前增加unsafe声明其为不安全的，同事trait的实现也必须用unsafe标记。
```rust
unsafe trait Foo {
    fn foo(&self);
}
struct Bar();
unsafe impl Foo for Bar {
    fn foo(&self) {
        println!("foo");
    }
}
fn main() {
    let a = Bar();
    a.foo();
}
```

# 高级特征
## 关联类型
- 关联类型在trait定义中指定占位符类型
- 关联类型是一个将类型占位符与trait相关联的方式。
- trait 的实现者会针对特定的实现在这个类型的位置指定相应的具体类型。
- 如此可以定义一个使用多种类型的 trait。
```rust
//pub trait Iterator {
//    type Item;  // 占位符类型
//    fn next(&mut self) -> Option<Self::Item>;
//}

// 使用泛型会创建所有使用过的类型，不指定确定类型时，编译器会报异常
pub trait Iterator1<T> {
    fn next(&mut self) -> Option<T>;
}
struct A {
    value: i32,
}
impl Iterator1<i32> for A {
    fn next(&mut self) -> Option<i32> {
        println!("in i32");
        if self.value > 3 {
            self.value += 1;
            Some(self.value)
        } else {
            None
        }
    }
}
impl Iterator1<String> for A {
    fn next(&mut self) -> Option<String> {
        println!("in string");
        if self.value > 3 {
            self.value += 1;
            Some(String::from("hello"))
        } else {
            None
        }
    }
}
fn main() {
    let mut a = A{value: 3};
    //a.next();  // 不指定类型，编译器无法选择确定的next
    <A as Iterator1<i32>>::next(&mut a);  //完全限定语法，带上了具体的类型
    <A as Iterator1<String>>::next(&mut a);
}
```


## 默认泛型类型参数和运算符重载
1. 使用泛型类型参数时，可以为泛型指定一个默认的具体类型。
2. 运算符重载是指在特定情况下自定义运算符行为的操作。
- Rust并不允许创建自定义运算符或者重载运算符，
- 不过对于std::ops中列出的运算符和相应的trait，我们可以实现运算符相关trait来重载。
```rust
use std::ops::Add;
#[derive(Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}
impl Add for Point {
    type Output = Point;
    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}
#[derive(Debug)]
struct Millimeters(u32);
struct Meters(u32);
impl Add<Meters> for Millimeters{
    type Output = Millimeters;
    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + other.0 * 1000)
    }
}
fn main() {
    assert_eq!(Point{x: 1, y: 1} + Point{x:2, y: 2}, Point{x: 3, y: 3});
    let mi = Millimeters(1);
    let m = Meters(1);
    let r = mi + m;
    println!("r = {:?}", r);
}
//trait Add<RHS=Self> { //尖括号里面为默认类型参数，RHS是一个泛型类型参数（right hand side）
//    type Output;
//    fn add(self, rhs: RHS) -> Self::Output;
//}
```

## 完全限定语法
- 定义：`<Type as Trait>::function(.....)`

- 特征同名函数名的调用，有self的情况
```rust
trait A {
   fn print(&self);
}
trait B {
   fn print(&self);
}
struct MyType;
impl A for MyType {
   fn print(&self) {
       println!("A trait for MyType");
   }
}
impl B for MyType {
   fn print(&self) {
       println!("B trait for MyType");
   }
}
impl MyType {
   fn print(&self) {
       println!("MyType");
   }
}
fn main() {
   let my_type = MyType;
   my_type.print(); //等价于MyType::print(&my_type);
   A::print(&my_type);
   B::print(&my_type);
}
```

- 没有self时，需要使用完全限定语法，类似强制转换
```rust
trait Animal {
    fn baby_name() -> String;
}
struct Dog;
impl Dog {
    fn baby_name() -> String {
        String::from("Spot")
    }
}
impl Animal for Dog {
    fn baby_name() -> String {
        String::from("puppy")
    }
}
fn main() {
    println!("baby_name: {}", Dog::baby_name());
    //println!("baby_name: {}", Animal::baby_name()); //error
    println!("baby_name: {}", <Dog as Animal>::baby_name()); //完全限定语法
}
```

## 父 trait 用于在另一个 trait 中使用某 trait 的功能
- 有时我们可能会需要某个 trait 使用另一个 trait 的功能。
- 在这种情况下，需要能够依赖相关的 trait 也被实现。
- 这个所需的 trait 是我们实现的 trait 的 父（超） trait（supertrait）。
> 限定范围的特征——trait_bound
```rust
use std::fmt;
trait OutPrint: fmt::Display { //:表示要求实现fmt::Display trait
    fn out_print(&self) {
        let output = self.to_string();
        println!("output: {}", output);
    }
}
struct Point{
    x: i32,
    y: i32,
}
impl OutPrint for Point {}
impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}
fn main() {
    println!("Hello, world!");
}
```


## newtype 模式用以在外部类型上实现外部 trait
- 孤儿规则（orphan rule）：只要 trait 或类型对于当前 crate 是本地的话就可以在此类型上实现该 trait。
- 一个绕开这个限制的方法是使用 newtype 模式（newtype pattern）。

```rust
use std::fmt;
struct Wrapper(Vec<String>);  // 不使用Wrapper封装一层的话，Vec类型和Display特征都是外部的，不是本地定义的，根据孤儿规则是无法直接绑定外部的特征的，
impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({})", self.0.join(","))
    }
}
fn main() {
    let w = Wrapper(vec![String::from("hello"), String::from("World")]);
    println!("w = {}", w);
}
```
> 说明：
> 在上述例子中，我们在 `Vec<T>` 上实现 Display，而孤儿规则阻止我们直接这么做，因为 Display trait 和 `Vec<T>` 都定义于我们的 crate 之外。我们可以创建一个包含 `Vec<T>` 实例的 Wrapper 结构体，然后再实现。



## 类型别名
```rust
type Kilometers = i32;
fn main() {
    let x: i32 = 5;    
    let y: Kilometers = 6;
    let r: i32 = x + y;
    println!("x + y = {}", r);
}
```

- 类型别名的主要用途是减少重复。

```rust
use std::io::Error; //标准库中的std::io::Error结构体代表了所有可能的I/O错误
use std::fmt;
pub trait Write {
   fn write(&mut self, buf: &[u8]) -> Result<usize, Error>;
   fn flush(&mut self) -> Result<(), Error>;
   fn write_all(&mut self, buf: &[u8]) -> Result<(), Error>;
   fn write_fmt(&mut self, fmt: fmt::Arguments) -> Result<(), Error>;
}

//加上如下类型别名声明：
type Result<T> = std::result::Result<T, std::io::Error>;//result<T, E> 中 E 放入了 std::io::Error

//代码就可以精简：
pub trait Write {
   fn write(&mut self, buf: &[u8]) -> Result<usize>;
   fn flush(&mut self) -> Result<()>;

   fn write_all(&mut self, buf: &[u8]) -> Result<()>;
   fn write_fmt(&mut self, fmt: Arguments) -> Result<()>;
}
```

## 从不返回的never type
- Rust 有一个叫做 ! 的特殊类型。在类型理论术语中，它被称为 empty type，因为它没有值。
- 我们更倾向于称之为 never type。在函数不返回的时候充当返回值
```rust
fn bar() -> ! {
   loop{}
}
```

- ! 类型有利于维护Rust的分支类型推导能力
- 在rust中的一切表达式都有确定的类型，比如If，elseif，else表达式或者多重match表达式，如果要在if中返回1 (i32类型),else中返回`String::from("A")`是不行的，因为两者的类型不兼容，编译器不知道是返回i32还是String，无法在编译器确定大小，常见的是用Box确定固定类型大小或者使用泛型等技术进行单态化。而 ! 类型也是一种维护多分支表达式的一种手段，！类型可以与任何其他类型兼容，可以理解为空集合与任意另一集合的交集都是另一集合。使得多分支表达式的类型是唯一的，避免了歧义。

- 以下例子为《Rust程序设计语言》中第二章“猜猜看”游戏的例子
```ini
# vi Cargo.toml
[dependencies]
rand = "0.6.0"
```

```rust
use std::io;
use std::cmp::Ordering;
use rand::Rng;
fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1, 101);
    loop {
        println!("Please input your guess.");
        let mut guess = String::new();
        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue, //continue 的值是 !。
                                //当 Rust 要计算 guess 的类型时，它查看这两个分支。
                                //前者是 u32 值，而后者是 ! 值。
                                //因为 ! 并没有一个值，Rust 决定 guess 的类型是 u32
        };
        println!("You guessed: {}", guess);
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

- 

```rust
// panic!
// Option<T> 上的 unwrap 函数代码：
impl<T> Option<T> {
   pub fn unwrap(self) -> T {
       match self {
           Some(val) => val,
           None => panic!("called `Option::unwrap()` on a `None` value"),
       }
   }
}
```

## 动态大小类型和Sized trait
- 动态大小类型（dynamically sized types），有时被称为 “DST” 或 “unsized types”，
- 这些类型允许我们处理只有在运行时才知道大小的类型。
- 最典型的就是str
```rust
fn main() {
    let s1: &str = "hello";
    let s2: &str = "world!";

    //let s1: str = "hello";  // error 编译的时候不能确定size
    //let s2: str = "world!";
    println!("Hello, world!");
}
```
- &str有两个值：str的地址和长度, &str的大小在编译时就可以知道，长度就是2*usize
- 动态大小类型的黄金规则：必须将动态大小类型的值置于某种指针之后，如：`Box<str>\Rc<str>`

- 另一个动态大小类型是trait。每一个 trait 都是一个可以通过 trait 名称来引用的动态大小类型。
- 为了将 trait 用于 trait 对象，必须将他们放入指针之后，
- 比如 &Trait 或 `Box<Trait>`（`Rc<Trait>` 也可以）。
- 需要使用dyn显式指明trait实现的对象


## Sized trait
- 为了处理 DST，Rust 用Sized trait 来决定一个类型的大小是否在编译时可知。
- 这个 trait 自动为编译器在编译时就知道大小的类型实现。
```rust
fn generic<T>(t: T) {//T为编译时就知道大小的类型
   // --snip--
}

//等价于
fn generic<T: Sized>(t: T) {//T为编译时就知道大小的类型
   // --snip--
}

//如何放宽这个限制呢？Rust提供如下方式：
fn generic<T: ?Sized>(t: &T) {//T 可能是Sized，也可能不是 Sized 的
   // --snip--
}
```


## 函数指针
- 函数指针允许我们使用函数作为另一个函数的参数。
- 函数的类型是 fn ，fn 被称为 函数指针。指定参数为函数指针的语法类似于闭包。
```rust
fn add_one(x: i32) -> i32 {
    x + 1
}
fn do_twice(f: fn(i32) -> i32, val: i32) -> i32 {
    f(val) + f(val)
}
fn wrapper_func<T>(t: T, v: i32) -> i32 
    where T: Fn(i32) -> i32 {
    t(v)
}
fn func(v: i32) -> i32 {
    v + 1
}
fn main() {
    let r = do_twice(add_one, 5);
    println!("r = {}", r);
    let a = wrapper_func(|x| x+1, 1);  // 闭包
    println!("a = {}", a);
    let b = wrapper_func(func, 1);     // 函数指针
    println!("b = {}", b);
}
```
- 函数指针实现了Fn、FnMut、FnOnce(三个闭包trait特征)



- 返回闭包
```rust
//fn return_clo() -> Fn(i32) -> i32 {  // 未知的size大小，使用box智能指针
//    |x| x+1
//}
fn return_clo() -> Box<dyn Fn(i32)->i32> {
    Box::new(|x| x+1)
}
fn main() {
    let c = return_clo();
    println!("1 + 1 = {}", c(1));  // 解引用强制多态
    println!("1 + 1 = {}", (*c)(1));
}
```

## 宏
### 定义
- Rust中的宏主要有两种：一种是使用macro_rules！的声明宏，一种是过程宏。
- 而过程宏又主要分为三种：
    1. 自定义宏#[derive]，在结构体、枚举等上指定通过derive属性添加代码；
    2. 类属性宏，定义可用于任意项的自定义属性；
    3. 类函数宏，看起来像函数但是作用于作为参数传递的Token。
- [介绍](https://danielkeep.github.io/tlborm/book/mbe-macro-rules.html)

### 宏和函数
1. 宏是一种为写其它代码而写代码的方式。宏对于减少大量编写代码和维护代码非常有用。
2. 一个函数标签必须声明函数参数个数和类型，宏只接受可变参数。
3. 宏的定义比函数的定义更复杂。
4. 在调用宏 之前 必须定义并将其引入作用域，而函数则可以在任何地方定义和调用。


### 声明宏
<!-- 声明宏推荐bilibili：BV1Wv411W7FH，up是伽德斯克 -->
```rust
let v = vec![1, 2, 3];
```
- 创建声明宏
```ini
# vi cargo.toml
[workspace]
members = [
	"mac",
	"main",
]
# cargo new mac --lib
# cargo new main
# vi main/cargo.toml
[dependencies]
mac = {path = "../mac"}
```
```rust
//vi mac/src/lib.rs
#[macro_export]
macro_rules! my_vec { //my_vec! 模仿vec！
    ($($x: expr), *) => {  // 模式匹配，匹配一个或多个
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
// vim main/src/main.rs
use mac;
fn main() {
    let v = mac::my_vec![1, 2, 3];
    //mac::my_vec![1, 2, 3] 等价于
    //let mut temp_vec = Vec::new();
    //temp_vec.push(1);
    //temp_vec.push(2);
    //temp_vec.push(3);
    //temp_vec
    println!("v = {:?}", v);
}
```
### 过程宏
- 过程宏接收 Rust 代码作为输入，在这些代码上进行操作，然后产生另一些代码作为输出，而非像声明式宏那样匹配对应模式然后以另一部分代码替换当前代码。

- 定义过程宏的函数接受一个 TokenStream 作为输入并产生一个 TokenStream 作为输出。这也就是宏的核心：宏所处理的源代码组成了输入 TokenStream，同时宏生成的代码是输出 TokenStream。如下：
```rust
use proc_macro;
#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {
}
```
#### derive宏  

- 例如 #[derive(Debug)]自动实现了`fmt::Display`的trait
```rust
#[derive(Debug)]
struct A {
	a : i32,
}
```
- 说明：在hello_macro_derive函数的实现中，syn 中的 parse_derive_input 函数获取一个 TokenStream 并返回一个表示解析出 Rust 代码的 DeriveInput 结构体（对应代码syn::parse(input).unwrap();）。该结构体相关的内容大体如下：
```rust
DeriveInput {
    // --snip--
    ident: Ident {
        ident: "Pancakes",
        span: #0 bytes(95..103)
    },
    data: Struct(
        DataStruct {
            struct_token: Struct,
            fields: Unit,
            semi_token: Some(
                Semi
            )
        }
    )
}
```
#### 类属性宏
- 类属性宏与自定义派生宏相似，不同于为 derive 属性生成代码，它们允许你创建新的属性。
- 属性宏中只接受两个参数，第一个参数attr，指类似#[allow(unused)]中的unused，第二个参数item，指被属性所修饰的Item，比如结构体定义。属性宏是接收输入的标记树替换成新树，派生宏是接收输入的TokenTree，然后追加新的标记树。
- 例子：可以创建一个名为 route 的属性用于注解 web 应用程序框架（web application framework）的函数：
```rust
#[route(GET, "/")]
fn index() {
```
- #[route] 属性将由框架本身定义为一个过程宏。其宏定义的函数签名看起来像这样：
```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
```
- 说明：类属性宏其它工作方式和自定义derive宏工作方式一致。

#### 类函数宏
- 类函数宏定义看起来像函数调用的宏。类似于 macro_rules!，它们比函数更灵活。
- 例子：如sql！宏，使用方式为：
```rust
let sql = sql!(SELECT * FROM posts WHERE id=1);
```
则其定义为：
```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
```


# 库
## fs

fs函数|说明
-|-
fs::read|读取为vec
fs::read_to_string|读取为string
fs::read_dir|


# RUST网络
## TCP
- [官方文档](https://doc.rust-lang.org/std/net/index.html)

- server
```rust
use std::net::{TcpListener, TcpStream};
use std::thread;
use std::time;
use std::io::{self, Read, Write};
fn handle_client(mut stream: TcpStream) -> io::Result<()> {
    let mut buf = [0; 512];
    for _ in 0..1000 {
        let bytes_read = stream.read(&mut buf)?;
        if bytes_read == 0 {
            return Ok(());
        }
        stream.write(&buf[..bytes_read])?;
        thread::sleep(time::Duration::from_secs(1));
    }
    Ok(())
}
fn main() -> io::Result<()> {
    let listener = TcpListener::bind("127.0.0.1:8080")?;
    let mut thread_vec: Vec<thread::JoinHandle<()>> = Vec::new();
    for stream in listener.incoming() {
        let stream = stream.expect("failed");
        let handle = thread::spawn(move || {
            handle_client(stream).unwrap_or_else(|error| eprintln!("{:?}", error));
        });
        thread_vec.push(handle);
    }
    for handle in thread_vec {
        handle.join().unwrap();
    }
    Ok(())
}
```

- clients
```rust
use std::io::{self, prelude::*, BufReader, Write};
use std::net::TcpStream;
use std::str;
fn main() -> io::Result<()> {
    let mut stream = TcpStream::connect("127.0.0.1:8080")?;
    for _ in 0..10 {
        let mut input = String::new();
        io::stdin().read_line(&mut input).expect("Failed to read");
        stream.write(input.as_bytes()).expect("Failed to write");
        let mut reader = BufReader::new(&stream);
        let mut buffer: Vec<u8> = Vec::new();
        reader.read_until(b'\n', &mut buffer).expect("Failed to read into buffer");
        println!("read  from server: {}", str::from_utf8(&buffer).unwrap());
        println!("");
    }
    Ok(())
}
```

## UDP

- server

```rust
use std::net::UdpSocket;
fn main() -> std::io::Result<()> {
    let socket = UdpSocket::bind("127.0.0.1:8080")?;
    loop {
        let mut buf = [0u8; 1500];
        let (amt, src) = socket.recv_from(&mut buf)?;
        println!("size = {}", amt);
        let buf = &mut buf[..amt];
        buf.reverse();
        socket.send_to(buf, src)?;
    }
}
```

- clients

```rust
use std::net::UdpSocket;
use std::{io, str};
fn main() -> io::Result<()> {
    let socket = UdpSocket::bind("127.0.0.1:8000")?;
    socket.connect("127.0.0.1:8080")?;
    loop{
        let mut input = String::new();
        io::stdin().read_line(&mut input)?;
        socket.send(input.as_bytes())?;
        let mut buffer = [0u8; 1500];
        let (_size, _src) = socket.recv_from(&mut buffer)?;
        println!("recv: {}", 
            str::from_utf8(&buffer).expect("Could not write buffer as string"));
    }
}
```

## IpAddr


```rust
use std::net::{IpAddr, Ipv4Addr, Ipv6Addr};
// pub enum IpAddr {
//     V4(Ipv4Addr),
//     V6(Ipv6Addr),
// }
fn main() {
    let v4 = IpAddr::V4(Ipv4Addr::new(127, 0, 0, 1));
    let v6 = IpAddr::V6(Ipv6Addr::new(0, 0, 0, 0, 0, 0, 0, 1));
    assert_eq!("127.0.0.1".parse(), Ok(v4));
    assert_eq!("::1".parse(), Ok(v6));
    assert_eq!(v4.is_loopback(), true);
    assert_eq!(v6.is_loopback(), true);
    assert_eq!(IpAddr::V4(Ipv4Addr::new(192, 168, 0, 1)).is_loopback(), false);
    assert_eq!(v4.is_multicast(), false);
    assert_eq!(v6.is_multicast(), false);
    assert_eq!(IpAddr::V4(Ipv4Addr::new(224, 254, 0, 0)).is_multicast(), true);
    assert_eq!(v4.is_ipv4(), true);
    assert_eq!(v6.is_ipv6(), true);
    assert_eq!(v4.is_ipv6(), false);
    assert_eq!(v6.is_ipv4(), false);
}
```

## SocketAddr

```rust
use std::net::{IpAddr, Ipv4Addr, Ipv6Addr, SocketAddr};
fn main() {
    let mut v4 = SocketAddr::new(IpAddr::V4(Ipv4Addr::new(127, 0, 0, 1)), 8080);
    let mut v6 = SocketAddr::new(IpAddr::V6(Ipv6Addr::new(0, 0, 0, 0, 0, 65535, 0, 1)), 8080);
    assert_eq!(v4.ip(), IpAddr::V4(Ipv4Addr::new(127, 0, 0, 1)));
    v4.set_ip(IpAddr::V4(Ipv4Addr::new(192, 168, 0, 1)));
    assert_eq!(v4.ip(), IpAddr::V4(Ipv4Addr::new(192, 168, 0, 1)));
    v6.set_ip(IpAddr::V6(Ipv6Addr::new(0, 0, 0, 0, 0, 65535, 1, 1)));
    assert_eq!(v4.port(), 8080);
    v4.set_port(1025);
    v6.set_port(1025);
    assert_eq!(v4.is_ipv4(), true);
    assert_eq!(v6.is_ipv4(), false);
    assert_eq!(v4.is_ipv6(), false);
    assert_eq!(v6.is_ipv6(), true);
}
```

## IpNet
- 类别域间路由

```rust
use ipnet::{IpNet, Ipv4Net, Ipv6Net};
use std::net::{Ipv4Addr, Ipv6Addr};
use std::str::FromStr;
fn main() -> std::io::Result<()> {
    let _v4 = Ipv4Net::new(Ipv4Addr::new(10, 1, 1, 0), 24).unwrap(); // 24位前缀
    let _v6 = Ipv6Net::new(Ipv6Addr::new(0xfd, 0, 0, 0, 0, 0, 0, 0), 24).unwrap();  // 24位前缀
    let _v4 = Ipv4Net::from_str("10.1.1.0/24").unwrap();
    let _v6 = Ipv6Net::from_str("fd00::/24").unwrap();
    let v4: Ipv4Net = "10.1.1.0/24".parse().unwrap();
    let _v6: Ipv6Net = "fd00::/24".parse().unwrap();
    let _net = IpNet::from(v4);
    let _net = IpNet::from_str("10.1.1.0/24").unwrap();
    let net: IpNet = "10.1.1.0/24".parse().unwrap();
    println!("{}, hostmask = {}", net, net.hostmask());
    println!("{}, netmask = {}", net, net.netmask());
    assert_eq!(
        "192.168.12.34/16".parse::<IpNet>().unwrap().trunc(),
        "192.168.0.0/16".parse().unwrap()
    );
    Ok(())
}
```

## mio
- 用于异步，非堵塞的api
- [官方文档](https://crates.io/crates/mio)

```rust
// mio = {version="0.7.0",features=["os-poll", "tcp"]}
use mio::net::{TcpListener, TcpStream};
use mio::{Events, Interest, Poll, Token};
const SERVER: Token = Token(0);
const CLIENT: Token = Token(1);
fn main() -> std::io::Result<()> {
    let mut poll = Poll::new()?;
    let mut events = Events::with_capacity(128);
    let addr = "127.0.0.1:8080".parse().unwrap();
    let mut server = TcpListener::bind(addr)?;
    poll.registry().register(&mut server, SERVER, Interest::READABLE)?;
    let mut client = TcpStream::connect(addr)?;
    poll.registry().register(&mut client, CLIENT, Interest::READABLE | Interest::WRITABLE)?;
    loop {
        poll.poll(&mut events, None)?;
        for event in events.iter() {
            match event.token() {
                SERVER => {
                    let connection = server.accept();
                    println!("SERVER recv a connection!");
                    drop(connection);
                }
                CLIENT => {
                    if event.is_writable() {println!("CLIENT write");}
                    if event.is_readable() {println!("CLIENT read");}
                    return Ok(())
                }
                _ => unreachable!(),
            }
        }
    }
}
```


## pNet
- 跨平台的网络api
- [官方文档](https://crates.io/crates/pnet)
```rust
//pnet = "0.26.0"
use pnet::datalink::Channel::Ethernet;
use pnet::datalink::{self, NetworkInterface};
use pnet::packet::ethernet::{EthernetPacket, EtherTypes};
use pnet::packet::ip::IpNextHeaderProtocols;
use pnet::packet::ipv4::Ipv4Packet;
use pnet::packet::tcp::TcpPacket;
use pnet::packet::Packet;
use std::env;
fn main() {
    let interface_name = env::args().nth(1).unwrap();
    let interfaces = datalink::interfaces();
    let interface = interfaces.into_iter().filter(|iface: &NetworkInterface| iface.name == interface_name).next().expect("Error get interface");
    let (_tx, mut rx) = match datalink::channel(&interface, Default::default()) {
        Ok(Ethernet(tx, rx)) => (tx, rx),
        Ok(_) => panic!("Other"),
        Err(e) => panic!("error: {}", e),
    };
    loop {
        match rx.next() {
            Ok(packet) => {
                let packet = EthernetPacket::new(packet).unwrap();
                handle_packet(&packet);
            },
            Err(e) => {println!("Some error: {}", e);}
        }
    }
}
fn handle_packet(ethernet: &EthernetPacket) {
    match ethernet.get_ethertype() {
        EtherTypes::Ipv4 => {
            let header = Ipv4Packet::new(ethernet.payload());
            if let Some(header) = header {
                match header.get_next_level_protocol() {
                    IpNextHeaderProtocols::Tcp => {
                        let tcp = TcpPacket::new(header.payload());
                        if let Some(tcp) = tcp {println!("Tcp packet {}: {} to {}: {}",header.get_source(),tcp.get_source(),header.get_destination(),tcp.get_destination());}
                    }
                    _ => println!("igno"),
                }
            }
        }
        _ => println!("ignoring"),
    }
}
```


## dns解析
- 解析域名
- [dns解析器](https://crates.io/crates/trust-dns-resolver)
- [官方文档1](https://trust-dns.org/index.html)

```rust
// trust-dns-resolver = "0.11.0"
// trust-dns = "0.16.0"
use std::env;
use trust_dns_resolver::Resolver;
use trust_dns_resolver::config::*;
use trust_dns::rr::record_type::RecordType;
fn main() {
    let args: Vec<String> = env::args().collect();
    if args.len() != 2 {
        eprintln!("Please provide a name to query!");
        std::process::exit(1);
    }
    let query = format!("{}.", args[1]);
    println!("use default config:");
    let resolver = Resolver::new(ResolverConfig::default(), ResolverOpts::default()).unwrap();
    let response = resolver.lookup_ip(query.as_str());
    for ans in response.iter() {println!("{:?}", ans);}
    println!("use system config:");
    let resolver = Resolver::from_system_conf().unwrap();
    let response = resolver.lookup_ip(query.as_str());
    for ans in response.iter() {println!("{:?}", ans);}
    println!("use ns:");
    let ns = resolver.lookup(query.as_str(), RecordType::NS);
    for ans in ns.iter() {println!("{:?}", ans);}
}
```

## serde序列化
- serde crate 是 Serde 生态的核心。
- serde_derive crate 提供必要的工具，
- 使用过程宏来派生 Serialize 和 Deserialize。
- 但是serde只提供序列化和反序列化的框架，具体的操作还需要依赖具体的包，
- 如serde_json和serde_yaml等。
- [官方文档](https://docs.serde.rs/serde)
```rust
// serde = { version = "1.0.106", features = ["derive"] }
// serde_json = "1.0"
// serde_yaml = "0.8"
use serde::{Deserialize, Serialize};
#[derive(Debug, Serialize, Deserialize)]
struct ServerConfig {
    workers: u64,
    ignore: bool,
    auth_server: Option<String>,
}
fn main() {
    let config = ServerConfig {workers: 100,ignore: false,auth_server: Some("auth.server.io".to_string()),};
    {
        println!("json:");
        let serialized = serde_json::to_string(&config).unwrap();
        println!("serialized: {}", serialized);
        let deserialized: ServerConfig = serde_json::from_str(&serialized).unwrap();
        println!("deserialized: {:#?}", deserialized);
    }
    {
        println!("yaml:");
        let serialized = serde_yaml::to_string(&config).unwrap();
        println!("serialized: {}", serialized);
        let deserialized: ServerConfig = serde_yaml::from_str(&serialized).unwrap();
        println!("deserialized: {:#?}", deserialized);
    }
}

```

## 构建脚本
- 在实际的项目中，有些包需要编译第三方非Rust代码，例如 C库；有些包需要链接到 C库，当然这些库既可以位于系统上， 也可以从源代码构建。其它的需求则有可能是需要构建代码生成 。 在Cargo中，提供了构建脚本，来满足这些需求。

- 指定的build命令应执行的Rust文件，将在包编译其它内容之前，被编译和调用，从而具备Rust代码所依赖的构建或生成的组件。Build通常被用来：

- 构建一个捆绑的C库；
- 在主机系统上找到C库；
- 生成Rust模块；
- 为crate执行所需的某平台特定配置。

```rust
include!(concat!(env!("OUT_DIR"), "/hello.rs"));

fn main() {
    println!("{}", say_hello());
}
```





