# Introduction
本例演示如何创建并连接静态库
```shell script
$ tree
.
├── CMakeLists.txt
├── include
│   └── shared
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
## Adding a Shared Library
与前面关于静态库的例子一样，`add_library()`函数也是用来从一些源文件中创建一个共享库(又称动态库)。其调用方法如下。
```cmake
add_library(hello_library SHARED
    src/Hello.cpp
)
```
这将被用来创建一个共享库，库名为libhello_library.so，共享库依赖的源码通过第3个参数传递给add_library()函数。
## Alias Target
顾名思义，目标别名是一个目标的替代名称，在只读上下文中可以用来代替真正的目标名称。
```cmake
add_library(hello::library ALIAS hello_library)
```
后文将看到，如何在与其他目标链接时使用别名来引用目标。
## Linking a Shared Library
链接一个共享库和链接一个静态库是一样的。当创建可执行文件时，使用target_link_library()函数来指明使用到的库。
```cmake
add_executable(hello_binary
    src/main.cpp
)

target_link_libraries(hello_binary
    PRIVATE
        hello::library
)
```
这告诉CMake使用共享库别名hello来链接hello_binary可执行文件。
# Building the Example
````shell script
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/D-shared-library/build

$ cmake --build . 或者 make
Scanning dependencies of target hello_library
[ 50%] Building CXX object CMakeFiles/hello_library.dir/src/Hello.cpp.o
Linking CXX shared library libhello_library.so
[ 50%] Built target hello_library
Scanning dependencies of target hello_binary
[100%] Building CXX object CMakeFiles/hello_binary.dir/src/main.cpp.o
Linking CXX executable hello_binary
[100%] Built target hello_binary

$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_binary  libhello_library.so  Makefile

$ ./hello_binary
Hello Shared Library!
````