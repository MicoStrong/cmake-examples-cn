# Introduction
分别使用不同目录保存头文件和源文件
```shell script
$ tree
.
├── CMakeLists.txt
├── include
│   └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp
```
- CMakeLists.txt - 包含cmake相关命令  

- include/Hello.h - 头文件

- src/Hello.cpp - 源文件

- src/main.cpp - 包含main的源文件
# Concepts
## Directory Paths
CMake 语法内置了一些变量，这些变量可以用来帮助在你的项目或源代码树中找到有用的目录。这些变量包括
	
| Variable | Info |
| ---------------|----------------|
| CMAKE_SOURCE_DIR | 源码根目录 |
| CMAKE_CURRENT_SOURCE_DIR | 如果使用子项目和目录，则为当前的源目录。 |
| PROJECT_SOURCE_DIR | 当前cmake项目的源代码目录。 |
| CMAKE_BINARY_DIR | 就是前文手动创建的build目录，在这个目录下执行cmake命令 |
| CMAKE_CURRENT_BINARY_DIR | 你当前所在的构建目录 |
| PROJECT_BINARY_DIR | 当前项目的构建目录 |
## Source Files Variable
创建一个包含源文件的变量，可以让你更清楚地了解这些文件，并轻松地将它们添加到多个命令中，例如，`add_executable()`函数
```cmake
# Create a sources variable with a link to all cpp files to compile
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

add_executable(${PROJECT_NAME} ${SOURCES})
```
>在SOURCES变量中设置特定文件名的另一种方法是使用GLOB命令使用通配符模式匹配来查找文件。
>```shell script
>file(GLOB SOURCES "src/*.cpp")
>```
对于现代的CMake来说，不推荐使用变量来表示源文件，而是直接在add_xxx函数中声明源文件。典型的做法是在add_xxx函数中直接声明源文件。
 这对于 glob 命令来说特别重要，因为如果你添加了一个新的源文件，可能不会总是显示正确的结果。
 ## Including Directories
 当你有不同的include文件夹时，你可以使用`target_include_directories()`函数让编译器知道它们。当编译这个目标时，这将会把这些目录添加到编译器中，并使用`-I`标志，例如`-I/directory/path`。
 ```cmake
target_include_directories(target
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)
```
`PRIVATE`标识符指定了include的范围。这对库来说很重要，在下一个例子中会解释。更多关于该函数的细节请看[这里](https://cmake.org/cmake/help/v3.0/command/target_include_directories.html)
# Building the Example
## Standard Output
下面显示了构建此示例的标准输出。
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build

$ cmake --build . 或者 make
Scanning dependencies of target hello_headers
[ 50%] Building CXX object CMakeFiles/hello_headers.dir/src/Hello.cpp.o
[100%] Building CXX object CMakeFiles/hello_headers.dir/src/main.cpp.o
Linking CXX executable hello_headers
[100%] Built target hello_headers

$ ./hello_headers
Hello Headers!
```
## Verbose Output
在前面的例子中(即01-basic)，当运行make命令时，输出只显示构建的状态。要想看到完整的输出以便调试，可以在运行make时添加VERBOSE=1标志。
VERBOSE的输出如下图所示，检查输出可以看到c++编译器命令中加入的include目录
```shell script
$ make clean

$ cmake --build . -- VERBOSE=1 或者 make VERBOSE=1
/usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/hello_headers -B/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
make -f CMakeFiles/hello_headers.dir/build.make CMakeFiles/hello_headers.dir/depend
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
cd /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/hello_headers /home/matrim/workspace/cmake-examples/01-basic/hello_headers /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles/hello_headers.dir/DependInfo.cmake --color=
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
make -f CMakeFiles/hello_headers.dir/build.make CMakeFiles/hello_headers.dir/build
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles 1
[ 50%] Building CXX object CMakeFiles/hello_headers.dir/src/Hello.cpp.o
/usr/bin/c++    -I/home/matrim/workspace/cmake-examples/01-basic/hello_headers/include    -o CMakeFiles/hello_headers.dir/src/Hello.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/hello_headers/src/Hello.cpp
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles 2
[100%] Building CXX object CMakeFiles/hello_headers.dir/src/main.cpp.o
/usr/bin/c++    -I/home/matrim/workspace/cmake-examples/01-basic/hello_headers/include    -o CMakeFiles/hello_headers.dir/src/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/hello_headers/src/main.cpp
Linking CXX executable hello_headers
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_headers.dir/link.txt --verbose=1
/usr/bin/c++       CMakeFiles/hello_headers.dir/src/Hello.cpp.o CMakeFiles/hello_headers.dir/src/main.cpp.o  -o hello_headers -rdynamic
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles  1 2
[100%] Built target hello_headers
make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles 0
```