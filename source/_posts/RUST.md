---
title: RUST
author: hero576
tags:
  - language
categories:
  - programming
date: 2021-10-19 14:58:00
---
>
<!--more-->
[toc]

# 简介
## 介绍
- 开始是私人项目，专注于安全。后公司资助，2015年发布第一版。
- [Rust 程序设计语言 简体中文版](https://kaisery.github.io/trpl-zh-cn/ch01-01-installation.html)
- [官方网址](https://www.rust-lang.org/)

## 环境搭建
- [官方地址](https://www.rust-lang.org/learn/get-started)
## ubuntu
```bash
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
source ~/.barch_profile
rustc --version
```
## windows:[下载链接](https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe)

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

命令|说明
-|-
new|创建工程，`cargo new project`<br>--lib创建库，`cargo new --lib libname`
run|运行工程，在项目目录下运行：`cargo run`
build|编译工程
check|检查工程
test|测试

# 基本概念
## 基本属性
- 变量定义： `let name: type`
```rust
let a =1;println!("a={}",a);//不可变变量
```

类型|说明
-|-
可变性|默认不可变：`let a=1;`<br>可变：`let mut a=1;`
常量|`const MAX_POINTS:u32=10000;`
隐藏|定义重名变量，则会覆盖前一个`let a=1;let a:f32=1.1;`

## 数据类型
- rust是静态语言，在编译前要确定变量类型，编译器有自动推导的能力。

数据类型|说明
-|-
bool|`let a:bool=true;let b:bool=false;`
char|字符型是32位的，与其他语言不同，所以可以是一个汉字<br>`let a='a';let b='一';`
数字|i:8,16,32,64;u:8,16,32,64;f:32,64;<br>`let c:i8=-11;`
数组|方括号[type;size]其中size也是数组类型的一部分<br>`let arr:[u32;5]=[1,2,3,4,5];println!("arr[0]={}",arr[0]);`
自适应类型|isize,usize大小根机器位数有关<br>`println!("max={}",usize::max_value());`

复合数据类型|说明
-|-
元组|圆括号`let tup:(i32,f32,char)=(-3,3.2,'123');let (x,y,z)=tup;`
结构体|
枚举|
字典|

### 字符串

操作|说明
-|-
创建空字符串|`let mut s=String::new();`
创建有初值|`let mut s=String::from("hello");`<br>通过字面值`let mut s="hello".to_string();`
更新|push_str：`s.push_str(" world");s.push_str(&s0);`<br>push：`s.push('!')`只能添加一个字符<br>+：`let s3=s1+&s2;`,s1所有权给了s3<br>format!：`let s=format!("{}-{}",s1,s2);`不会获取所有权
索引|String：`let s2=s[0];`报错，rust不支持被索引，防止utf8边界错误<br>str：`let s="你好";let h=&hello[0..3];`，边界不同也会报错
遍历|chars：`for c in s.chars(){println!();}`<br>bytes：`for c in s.bytes(){println!();}`
析构|离开作用域会调用`drop`，释放内存

- 单引号：字符
- 双引号：表示引用

### 切片slices
- 字符串
```rust
let s1='hello'; //字面值，不可变
let s=String::from("hello world");
let h=&s[0..5];  // 指向s的0-5的空间
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

- 数组
```rust
let a = [1,2,3,4,5];
let ss= &a[1..3];
println!("ss={}",ss[0]);
println!("len ss: {}",ss.len());
```

### 结构体

操作|说明
-|-
定义|`struct User{name:String,count String,nonce:u64,active:bool}`
实例|`let mut xiao=User{name:String::from("xiao"),count:String::from("8454"),nonce:1000,active:true,}`
修改字段|前提是mut可变变量`xiao.nonce=2000;`
参数名字和字段名同名时可简写|`let name=String::from("xiao");let count=String::from("80808");nonce=1000;active=true;let xiao=User{name,count,nonce,active};`
从其他结构体创建实例|`let xiao2=User{name:String::from("xiao2"),..xiao};//此时xiao被借用`
元祖结构体|字段没有名字，圆括号`struct Point(i32,i32); let a=Point(10,20);println!("a.x={},a.y={}",a.0,a.1);`
没有任何字段的类单元结构体|`struct A{};`
打印结构体|`#[derive(Debug)]`<br>`println!("xiao={:?}",xiao);`<br>`println!("xiao={:#?}",xiao);`

- 方法
```rust
#[derive(Debug)]
struct Dog{
  name:String,
  weight:f32,
  height:f32,
}
impl Dog{
  fn get_name(&self)->&str{
    &(self.name[..])
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
<td> enum IpAddr{V4(String),V6(String),};<br>let i=IpAddr::V4(String::from("127.0.0.1"))<br>
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
  fn print(&self){ //取引用
    match *self{ // 获取内容需要解引用
      Message::Quit=>println!("Quit"),
      Message::Move{x,y}=>println!("Move({},{})",x,y),
      Message::Change(a,b,c)=>println!("change({},{},{})",a,b,c),
      // Message::Write(&s)=>println!("write({})",s), 引用无法打印
      _ => println!("write()")
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
- 标准库定义的一个枚举，原始形式：`enum Option<T>{Some(T),None,}`，T表示泛型
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

操作|说明
-|-
创建空的vector|`Vec<T>`<br>`let mut v:Vec<i32>=Vec::new();`
创建包含初值的vector|`Vec<T>`<br>`let mut v=vec![1,2,3];`
压栈|`v.push(1);`
丢弃vector|会丢弃全部元素，`{let v1=vec![1];}`
读取|方式一：`let one:&i32=&v[0];`<br>方式二(推荐)：`match v.get(1){Some(value)=>println!("two={}",value),_=>{},}`
遍历|不可变遍历`for i in &v{println!("i={}",i);}`<br>可变遍历`for i in &mut v{*i+=1;}`
存储不同的类型|利用枚举存储不同的类型，`enum Context{Text(String),FLoat(f32),Int(i32),};`<br>`let c=vec![Context::Text(String::from("1")),Context::Float(1.2),Int(12),];`


### hashmap
- `HashMap<Key,Value>`

操作|说明
-|-
导入包|`use std::collections::HashMap;`
创建|方式一：`let mut map:HashMap<String,i32>=HashMap::new();`<br>方式二：`let keys=vec![String::from("1")];let values=vec![1];let map:HashMap<String,i32>=kes.iter().zip(values.iter()).collect();`
直接插入|`map.insert(String::from("1"),1);`
不存在时插入|`map.entry(String::from("2")).or_insert(2);`
更新插入|`let count=map.entry(String::from("1")).or_insert(0);*count+=1;`
读取|`let k=String::from("1");if let Some(v)=map.get(&k){println!("v={}",v);}`
遍历|`for (k,v) in &map{println!("{}:{}",k,v);}`<br>`println!("{:?},map");`


## 函数
- 函数定义：`fn f1(){}`
- 函数调用
- 参数传入：`fn f1(a:i32,b:u32){}`参数定义时需要类型，无法自动推导
- 返回值：
  - 方法一：`fn f1(a:i32,b:i32)->i32{return a+b;}`
  - 方法二：`fn f1(a:i32,b:i32)->i32{a+b}` 语句没有返回值，所以不能有分号
  - 表达式是有返回值的：`let y = {let a=1;a+1};`


## 注释
- `//`

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
for i in &arr(){   //使用引用
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

### 函数的作用域：
```rust
fn ownership(s:String){
  println!("s={}",s); //退出后s就被回收了
}
fn unownership(s:String)->String{
  println!("s={}",s); //退出后将参数传回
  s
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

- 栈上的拷贝可以继续使用，或者实现了copy特征的类型也无需clone，包括：整型、浮点、布尔、字符、元组；
```rust
let a=1;
let b=a;
println!("a={},b={}",a,b);
```

## 引用和借用
- 引用：`& name`
- 解引用：`* name`，取里面的内容
- 借用：`&mut name`
- 创建一个指向值的引用，但并不拥有这个值，所以离开作用域不会被释放；
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

## 包

- 包（Packages）： Cargo 的一个功能，它允许你构建、测试和分享 crate。
- Crates ：一个模块的树形结构，它形成了库或二进制项目。
- 模块（Modules）和 use： 允许你控制作用域和路径的私有性。
- 路径（path）：一个命名例如结构体、函数或模块等项的方式

### 模块
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
  factory::produce_washing::produce();
}
```

### crate
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
...
[dependencies]
mylib={path="./mylib"}
```

- 使用库
```rust
//vi src/main.rs
use mylib::factory::produce_refrigerator as A;  // 设置别名
use mylib::factory::*; // 导入所有模块
fn main(){
  mylib::factory::produce_refrigerator::produce(); //绝对路径
  //produce_refrigerator::produce(); //使用use
  A::produce();
}
```

### mod的结构体
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

### 外部库
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

- 执行`cargo run`时，相应的库会自动下载和编译。


## 错误
- rust的错误分为：
  - 可恢复错误：用户报告的错误和重试操作是否合理，用枚举`Result<T,E>`实现
  - 不可恢复错误：通过`panic! unwrap expect`实现

### panic!
```rust
fn main(){
  panic!("crash!!");
}
```

### RUST_BACKTRACE 
- `RUST_BACKTRACE=1 cargo run`执行时会打印调试堆栈，非0即可


### Result<T,E>
- 原型
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
  let f=File::open("hello.txt").unwrap();
  //方式二
  let f=File::open("hello.txt").expect("Failed to open file");
  
}
```

### 错误传递
```rust
use std::io;
use std::io::Read;
use std::io::File;
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
//简写方式1
fn read_username2()->Result<String,io::Error>{
  let mut f=File::open("hello.txt")?;
  let mut s=String::new();
  match f.read_to_string(&mut s)?
  Ok(s)
}
//简写方式2
fn read_username2()->Result<String,io::Error>{
  let mut s=String::new();
  File::open("hello.txt")?.read_to_string(&mut s)?;
  Ok(s)
}
```

## 测试
- 执行`cargo new --lib mylib` 
```rust
//mylib/src/animal.rs
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
//mylib/src/lib.rs
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
impl GetIfo for Student{
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
- trait作为参数
```rust
fn print_info(item:impl GetInfo){
  println!("name={},age={}",s.get_name(),s.get_age());
}
```

- 默认实现
```rust
trait SchoolName{
  fn get_school_name(&self)->String{
    String::from("school")
  }
}

```


# 生命周期

# 迭代器

# 智能指针

# 线程

# 面向对象

# 高级特征






```rust
```
















