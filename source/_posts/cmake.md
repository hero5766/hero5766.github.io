---
title: cmake
author: hero576
tags:
  - linux
categories:
  - programme
date: 2020-09-11 01:12:03
---

> cmake工具

<!--more-->

# 简介
- CMake实现了一种平台无关的编译配置工具，只需要编写CMakeList.txt文件，就可以实现“Write once, run everywhere”。文件 CMakeLists.txt 需要手工编写，也可以通过编写脚本进行半自动的生成。
- [官方文档](https://cmake.org/cmake/help/cmake2.4docs.html)
- [中文文档](https://www.hahack.com/codes/cmake)
- 在 linux 平台下使用 CMake 生成 Makefile 并编译的流程如下：
  1. 编写 CMake 配置文件 CMakeLists.txt 。
  2. 执行命令 cmake PATH 或者 ccmake PATH 生成 Makefile（ccmake 和 cmake 的区别在于前者提供了一个交互式的界面）。其中， PATH 是 CMakeLists.txt 所在的目录。
  3. 使用 make 命令进行编译。

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

命令|说明
-|-
ccmake [options] <path-to-source>| 生成Makefile，提供了一个图形化的操作界面
cmake [options] <path-to-source> | 生成Makefile




# 示例
## 单文件工程
- 单个文件
```cpp
//main.cpp 
#include <iostream>
int main(){
    std::cout<<"Hello word!"<<std::endl;
    return 0;
}
```


- 创建CMakeLists.txt并将其与main.cpp放在同一个目录下:
```cmake
PROJECT(main)# 项目信息
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)# CMake 最低版本号要求
AUX_SOURCE_DIRECTORY(. DIR_SRCS)# 将当前目录中的源文件名称赋值给变量 DIR_SRCS
ADD_EXECUTABLE(main ${DIR_SRCS})# 变量 DIR_SRCS 中的源文件需要编译 成一个名称为 main 的可执行文件
```

- 这里我们进入了 main.cpp 所在的目录后执行 “cmake .” 后就可以得到 Makefile 并使用 make 进行编译。

## 同一目录，多文件工程
- 文件目录

目录1|目录2
-|-|-
.|main.cpp
.|mathfunc.hpp
.|mathfunc.cpp

```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (Demo2)
# 指定生成目标
add_executable(Demo main.cc mathfunc.cpp)
```

- 在 add_executable 命令中增加了一个 mathfunc.cpp 源文件，如果文件过多，会比较繁琐。更省事的方法是使用 aux_source_directory 命令，该命令会查找指定目录下的所有源文件，然后将结果存进指定变量名。
```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (Demo2)
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
# 指定生成目标
add_executable(Demo ${DIR_SRCS})
```

## 不同目录，多文件工程

- 文件目录

目录1|目录2|目录3
-|-|-
.|main.cpp|
.|src/|test1.h
.|src/|test1.cpp

- 创建文件 CMakeLists.txt
```cmake
PROJECT(main)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6) 
ADD_SUBDIRECTORY( src ) # 指明本项目包含一个子目录 src
AUX_SOURCE_DIRECTORY(. DIR_SRCS)
ADD_EXECUTABLE(main ${DIR_SRCS}  )
TARGET_LINK_LIBRARIES( main Test ) # 指明可执行文件 main 需要连接一个名为Test的链接库
```

- 在子目录 src 中创建 CmakeLists.txt
```cmake
AUX_SOURCE_DIRECTORY(. DIR_TEST1_SRCS)
ADD_LIBRARY ( Test ${DIR_TEST1_SRCS}) # 将 src 目录中的源文件编译为共享库
```

- 依次执行命令 “cmake .” 和 “make”


## 在工程中查找并使用其他程序库的方法
- 在开发软件的时候我们会用到一些函数库,这些函数库在不同的系统中安装的位置可能不同,编译的时候需要首先找到这些软件包的头文件以及链接库所在的目录以便生成编译选项。例如一个需要使用博克利数据库项目,需要头文件db_cxx.h 和链接库 libdb_cxx.so ,现在该项目中有一个源代码文件 main.cpp ，放在项目的根目录中。

- 在项目的根目录中创建目录 cmake/modules/ ，在 cmake/modules/ 下创建文件 Findlibdb_cxx.cmake
```cmake
MESSAGE(STATUS "Using bundled Findlibdb.cmake...")# 将参数的内容输出到终端
FIND_PATH(# 指明头文件查找的路径
  LIBDB_CXX_INCLUDE_DIR
  db_cxx.h 
  /usr/include/ 
  /usr/local/include/ 
  #在 /usr/include/ 和 /usr/local/include/ 中查找文件db_cxx.h ,并将db_cxx.h 所在的路径保存在 LIBDB_CXX_INCLUDE_DIR中
)
FIND_LIBRARY(#用于查找链接库并将结果保存在变量中
  LIBDB_CXX_LIBRARIES NAMES  db_cxx
  PATHS /usr/lib/ /usr/local/lib/
  #在目录 /usr/lib/ 和 /usr/local/lib/ 中寻找名称为 db_cxx 的链接库,并将结果保存在 LIBDB_CXX_LIBRARIES
)
```

- 项目的根目录中的 CmakeList.txt
```cmake
PROJECT(main)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
SET(CMAKE_SOURCE_DIR .)
SET(CMAKE_MODULE_PATH ${CMAKE_ROOT}/Modules ${CMAKE_SOURCE_DIR}/cmake/modules) # 表示到目录 ./cmake/modules 中查找 Findlibdb_cxx.cmake
AUX_SOURCE_DIRECTORY(. DIR_SRCS)
ADD_EXECUTABLE(main ${DIR_SRCS})
FIND_PACKAGE( libdb_cxx REQUIRED) # 查找链接库和头文件，CMake 会到变量 CMAKE_MODULE_PATH 指示的目录中查找文件Findlibdb_cxx.cmake 并执行
MARK_AS_ADVANCED(
  LIBDB_CXX_INCLUDE_DIR
  LIBDB_CXX_LIBRARIES
)
IF (LIBDB_CXX_INCLUDE_DIR AND LIBDB_CXX_LIBRARIES)
MESSAGE(STATUS "Found libdb libraries")
   INCLUDE_DIRECTORIES(${LIBDB_CXX_INCLUDE_DIR})
    MESSAGE( ${LIBDB_CXX_LIBRARIES} )
    TARGET_LINK_LIBRARIES(main ${LIBDB_CXX_LIBRARIES}18 )
ENDIF (LIBDB_CXX_INCLUDE_DIR AND LIBDB_CXX_LIBRARIES)
# 如果 LIBDB_CXX_INCLUDE_DIR 和 LIBDB_CXX_LIBRARIES 都已经被赋值,则设置编译时到 LIBDB_CXX_INCLUDE_DIR 寻找头文件并且设置可执行文件 main 需要与链接库 LIBDB_CXX_LIBRARIES 进行连接。
```

- 完成 Findlibdb_cxx.cmake 和 CMakeList.txt 的编写后在项目的根目录依次执行 “cmake . ” 和 “make ” 可以进行编译

## 使用 cmake 生成 debug 版和 release 版的程序
- 在 Visual Studio 中我们可以生成 debug 版和 release 版的程序,使用 CMake 我们也可以达到上述效果。debug 版的项目生成的可执行文件需要有调试信息并且不需要进行优化,而 release 版的不需要调试信息但需要优化。这些特性在 gcc/g++ 中是通过编译时的参数来决定的,如果将优化程度调到最高需要设置参数-O3,最低是 -O0 即不做优化;添加调试信息的参数是 -g -ggdb ,如果不添加这个参数,调试信息就不会被包含在生成的二进制文件中。
- CMake 中有一个变量 CMAKE_BUILD_TYPE ,可以的取值是 Debug Release RelWithDebInfo 和 MinSizeRel。当这个变量值为 Debug 的时候,CMake 会使用变量 CMAKE_CXX_FLAGS_DEBUG 和 CMAKE_C_FLAGS_DEBUG 中的字符串作为编译选项生成 Makefile ,当这个变量值为 Release 的时候,工程会使用变量 CMAKE_CXX_FLAGS_RELEASE 和 CMAKE_C_FLAGS_RELEASE 选项生成 Makefile。

```cmake
PROJECT(main)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
SET(CMAKE_SOURCE_DIR .)
# 用于 debug 和 release 的编译选项
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
AUX_SOURCE_DIRECTORY(. DIR_SRCS)
ADD_EXECUTABLE(main ${DIR_SRCS})
```

- 执行 ccmake 命令生成 Makefile

## 自定义编译选项
- CMake 允许为项目增加编译选项，从而可以根据用户的环境和需求选择最合适的编译方案。
- 例如，可以将 MathFunctions 库设为一个可选的库，如果该选项为 ON ，就使用该库定义的数学函数来进行运算。否则就调用标准库中的数学函数库。
- 顶层的 CMakeLists.txt 文件中添加该选项
```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (Demo4)
# 加入一个配置头文件，用于处理 CMake 对源码的设置
configure_file (
  "${PROJECT_SOURCE_DIR}/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
  )
# 是否使用自己的 MathFunctions 库
option (USE_MYMATH
       "Use provided math implementation" ON)
# 是否加入 MathFunctions 库
if (USE_MYMATH)
  include_directories ("${PROJECT_SOURCE_DIR}/math")
  add_subdirectory (math)  
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
# 指定生成目标
add_executable(Demo ${DIR_SRCS})
target_link_libraries (Demo  ${EXTRA_LIBS})
```

- 其中：
  - 第7行的 configure_file 命令用于加入一个配置头文件 config.h ，这个文件由 CMake 从 config.h.in 生成，通过这样的机制，将可以通过预定义一些参数和变量来控制代码的生成。
  - 第13行的 option 命令添加了一个 USE_MYMATH 选项，并且默认值为 ON 。
  - 第17行根据 USE_MYMATH 变量的值来决定是否使用我们自己编写的 MathFunctions 库。

- 修改 main.cc 文件，让其根据 USE_MYMATH 的预定义值来决定是否调用标准库还是 MathFunctions 库
```cmake
#include <stdio.h>
#include <stdlib.h>
#include "config.h"
#ifdef USE_MYMATH
  #include "math/MathFunctions.h"
#else
  #include <math.h>
#endif

int main(int argc, char *argv[])
{
    if (argc < 3){
        printf("Usage: %s base exponent \n", argv[0]);
        return 1;
    }
    double base = atof(argv[1]);
    int exponent = atoi(argv[2]);
#ifdef USE_MYMATH
    printf("Now we use our own Math library. \n");
    double result = power(base, exponent);
#else
    printf("Now we use the standard library. \n");
    double result = pow(base, exponent);
#endif
    printf("%g ^ %d is %g\n", base, exponent, result);
    return 0;
}
```

- 编写 config.h.in 文件：上面的程序值得注意的是第2行，这里引用了一个 config.h 文件，这个文件预定义了 USE_MYMATH 的值。但我们并不直接编写这个文件，为了方便从 CMakeLists.txt 中导入配置，我们编写一个 config.h.in 文件，内容如下：
```ini
#cmakedefine USE_MYMATH
```

## 安装和测试
- CMake 也可以指定安装规则，以及添加测试。这两个功能分别可以通过在产生 Makefile 后使用 make install 和 make test 来执行。在以前的 GNU Makefile 里，你可能需要为此编写 install 和 test 两个伪目标和相应的规则，但在 CMake 里，这样的工作同样只需要简单的调用几条命令。

### 定制安装规则
- 首先先在 math/CMakeLists.txt 文件里添加下面两行：
```cmake
# 指定 MathFunctions 库的安装路径
install (TARGETS MathFunctions DESTINATION bin)
install (FILES MathFunctions.h DESTINATION include)
```

- 指明 MathFunctions 库的安装路径。之后同样修改根目录的 CMakeLists 文件，在末尾添加下面几行：
```cmake
# 指定安装路径
install (TARGETS Demo DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/config.h"
         DESTINATION include)
```

- 通过上面的定制，生成的 Demo 文件和 MathFunctions 函数库 libMathFunctions.o 文件将会被复制到 /usr/local/bin 中，而 MathFunctions.h 和生成的 config.h 文件则会被复制到 /usr/local/include 中。我们可以验证一下（顺带一提的是，这里的 /usr/local/ 是默认安装到的根目录，可以通过修改 CMAKE_INSTALL_PREFIX 变量的值来指定这些文件应该拷贝到哪个根目录）：
```bash
[ehome@xman Demo5]$ sudo make install
[ 50%] Built target MathFunctions
[100%] Built target Demo
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/bin/Demo
-- Installing: /usr/local/include/config.h
-- Installing: /usr/local/bin/libMathFunctions.a
-- Up-to-date: /usr/local/include/MathFunctions.h
[ehome@xman Demo5]$ ls /usr/local/bin
Demo  libMathFunctions.a
[ehome@xman Demo5]$ ls /usr/local/include
config.h  MathFunctions.h
```

### 为工程添加测试
- 添加测试同样很简单。CMake 提供了一个称为 CTest 的测试工具。我们要做的只是在项目根目录的 CMakeLists 文件中调用一系列的 add_test 命令。
```cmake
# 启用测试
enable_testing()
# 测试程序是否成功运行
add_test (test_run Demo 5 2)
# 测试帮助信息是否可以正常提示
add_test (test_usage Demo)
set_tests_properties (test_usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage: .* base exponent")
# 测试 5 的平方
add_test (test_5_2 Demo 5 2)
set_tests_properties (test_5_2
 PROPERTIES PASS_REGULAR_EXPRESSION "is 25")
# 测试 10 的 5 次方
add_test (test_10_5 Demo 10 5)
set_tests_properties (test_10_5
 PROPERTIES PASS_REGULAR_EXPRESSION "is 100000")
# 测试 2 的 10 次方
add_test (test_2_10 Demo 2 10)
set_tests_properties (test_2_10
 PROPERTIES PASS_REGULAR_EXPRESSION "is 1024")
```

- 上面的代码包含了四个测试。第一个测试 test_run 用来测试程序是否成功运行并返回 0 值。剩下的三个测试分别用来测试 5 的 平方、10 的 5 次方、2 的 10 次方是否都能得到正确的结果。其中 PASS_REGULAR_EXPRESSION 用来测试输出是否包含后面跟着的字符串。

- 让我们看看测试的结果：
```bash
[ehome@xman Demo5]$ make test
Running tests...
Test project /home/ehome/Documents/programming/C/power/Demo5
    Start 1: test_run
1/4 Test #1: test_run .........................   Passed    0.00 sec
    Start 2: test_5_2
2/4 Test #2: test_5_2 .........................   Passed    0.00 sec
    Start 3: test_10_5
3/4 Test #3: test_10_5 ........................   Passed    0.00 sec
    Start 4: test_2_10
4/4 Test #4: test_2_10 ........................   Passed    0.00 sec
100% tests passed, 0 tests failed out of 4
Total Test time (real) =   0.01 sec
```

- 如果要测试更多的输入数据，像上面那样一个个写测试用例未免太繁琐。这时可以通过编写宏来实现：
```bash
# 定义一个宏，用来简化测试工作
macro (do_test arg1 arg2 result)
  add_test (test_${arg1}_${arg2} Demo ${arg1} ${arg2})
  set_tests_properties (test_${arg1}_${arg2}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endmacro (do_test)
# 使用该宏进行一系列的数据测试
do_test (5 2 "is 25")
do_test (10 5 "is 100000")
do_test (2 10 "is 1024")
```
- 关于 CTest 的更详细的用法可以通过 man 1 ctest 参考 CTest 的文档。



[^1][^2][^3]

[^1]:https://www.ibm.com/developerworks/cn/linux/l-cn-cmake/
[^2]:https://www.hahack.com/codes/cmake/
[^3]:http://www.cppblog.com/skyscribe/archive/2009/12/14/103208.aspx

