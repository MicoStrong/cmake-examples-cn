# Introduction
本例演示如何创建并连接静态库
```shell script
$ tree
.
├── CMakeLists.txt
├── include
│   └── static
│       └── Hello.h
├── README.md
└── src
    ├── Hello.cpp
    └── main.cpp

```
- CMakeLists.txt - 包含cmake相关命令  
- include/static/Hello.h - 头文件
- src/Hello.cpp - 源文件
- src/main.cpp - 包含main的源文件
# Concepts
## Adding a Static Library
`add_library()`函数用于从一些源文件中创建一个库。这个函数的调用如下。
```cmake
add_library(hello_library STATIC
    src/Hello.cpp
)
```
`add_library()`将生成必要的编译工具指令，这些指令用于将指定的源代码编译成一个库。  
第一个参数是该命令生成的目标名，也就是库的名字。一旦在该指令中指定目标名，后续整个CMakeLists.txt中都可以使用这个名字来引用库。实际生成库文件的名字比第一个参数给出的名字要稍显复杂，cmake还会添加lib前缀以及相关的后缀。其中后缀取决于平台（即操作系统）和 第二个参数（STATIC、SHARED），经过修改过后的名字才是库文件最终的名字。

举例：GNU/Linux平台下，
```cmake
add_library(hello_library STATIC 
    src/Hello.cpp
)
```
将生成静态库文件libhello_library.a

第二个参数可以是：  
- STATIC：生成静态库  
- SHARED：生成动态库  
- OBJECT：生成目标文件，用于同时生成静态库和动态库。特别注意一点，使用OBJECT只生产目标文件时，要求生成位置无关代码(position-independent code)。可以使用set_target_properties进行设置

STATIC告诉这将被用来创建一个静态库，库名为libhello_library.a，库使用的源文件在add_library函数调用中给出。

本例中，直接将源文件传递给`add_library()`调用，这是现代CMake所推荐的。
## Populating Including Directories
在这个例子中，我们使用`target_include_directories()`函数将include目录包含在库中，范围设置为PUBLIC。
```cmake
target_include_directories(hello_library
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)
```
这将导致include目录在如下地方被用到。
- 编译库时
- 当编译任何使用到该库的目标(target)时
>Tip
>当使用target_include_directories包含目录时，应该总是包含所有子目录的根目录，如本里中static的根目录是include。然后头文件或者源文件使用#include包含头文件时可以这样
>```c++
>#include "static/Hello.h"
>```
>使用这种方法意味着，当你在项目中使用多个库时，头文件名冲突的机会较少。
### Levels of visibility
cmake支持3中可见性级别  
这些可见性级别有如下含义：   
- 使用PRIVATE属性，编译选项将只应用于给定的目标，而不会应用于其他使用它的目标。
- 使用INTERFACE属性，给定目标上的编译选项将只被应用到使用它的目标上。
- 使用PUBLIC属性，编译选项将被应用到给定的目标和所有其他使用它的目标
## Linking a Library
当创建一个将使用你的库(本例中的hello_library)的可执行文件时，你必须告诉编译器关于这个库的信息，这可以使用 target_link_libraries()函数来完成。
```cmake
add_executable(hello_binary
    src/main.cpp
)

target_link_libraries( hello_binary
    PRIVATE
        hello_library
)
```
这将告诉CMake在链接时将hello_library链接到hello_binary可执行文件。它也会从链接的库目标中传播任何具有PUBLIC或INTERFACE范围的包含目录。  
一个被编译器调用的例子就是
```shell script
/usr/bin/c++ CMakeFiles/hello_binary.dir/src/main.cpp.o -o hello_binary -rdynamic libhello_library.a
```
# Building the Example
```shell script
$ mkdir build

$ cd build

$ cmake ..
-- The C compiler identification is GNU 4.8.4
-- The CXX compiler identification is GNU 4.8.4
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/C-static-library/build

$ cmake --build . 或者 make
Scanning dependencies of target hello_library
[ 50%] Building CXX object CMakeFiles/hello_library.dir/src/Hello.cpp.o
Linking CXX static library libhello_library.a
[ 50%] Built target hello_library
Scanning dependencies of target hello_binary
[100%] Building CXX object CMakeFiles/hello_binary.dir/src/main.cpp.o
Linking CXX executable hello_binary
[100%] Built target hello_binary

$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_binary  libhello_library.a  Makefile

$ ./hello_binary
Hello Static Library!
```