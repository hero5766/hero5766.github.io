---
title: cmake
author: hero576
tags:
  - linux
categories:
  - programme
date: 2020-08-03 22:03:00
---

> cmake工具

<!--more-->

# 简介
- CMake实现了一种平台无关的编译配置工具，只需要编写CMakeList.txt文件，就可以实现“Write once, run everywhere”。文件 CMakeLists.txt 需要手工编写，也可以通过编写脚本进行半自动的生成。
- [官方文档](https://cmake.org/cmake/help/cmake2.4docs.html)

# 语法
## 注释
- 符号"#"后面的内容被认为是注释

## 函数
- 格式：`命令名称(参数1 参数2)`
- 参数之间用空格间隔


|函数||
|-|-|
|PROJECT(projectname)|设置项目名称|
|CMAKE_MINIMUM_REQUIRED(VERSION 2.6)|限定cmake版本|
|AUX_SOURCE_DIRECTORY(. DIR_SRCS)|将目录赋值给变量|




# 命令

# 单文件工程
- 例如源文件：main.cpp
```c
#include <iostream>
int main(){
    std::cout<<"Hello word!"<<std::endl;
    return 0;
}
```

- 创建CMakeLists.txt并将其与main.cpp放在同一个目录下:
```c
PROJECT(main)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
AUX_SOURCE_DIRECTORY(. DIR_SRCS)
ADD_EXECUTABLE(main ${DIR_SRCS})
```

















[^1][^2][^3]

[^1]:https://www.ibm.com/developerworks/cn/linux/l-cn-cmake/
[^2]:https://www.hahack.com/codes/cmake/
[^3]:http://www.cppblog.com/skyscribe/archive/2009/12/14/103208.aspx

