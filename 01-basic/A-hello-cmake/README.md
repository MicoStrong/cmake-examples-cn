# Introduction
```shell script
$ tree
.
├── CMakeLists.txt
└── main.cpp
```
- CMakeLists.txt : 包含cmake相关命令  
- main.cpp : 一段简单的Hello World源代码

# Concepts
## CMakeLists.txt
CMakeLists.txt 是存储所有 CMake 命令的文件。当cmake在一个文件夹中运行时，它会寻找这个文件，如果它不存在，cmake会以错误退出。
## Minimum CMake version
当使用CMake创建项目时，你可以指定支持CMake的最低版本。
```cmake
cmake_minimum_required(VERSION 3.5)
```
## Projects
项目名和项目使用语言(cmake中,c++是默认语言,但是还建议使用LANGUAGES参数显式指定项目使用语言)
```cmake
project(hello_cmake LANGUAGES CXX)
```
## Creating an Executable
add_executable()命令从指定的源文件中构建一个可执行文件，在本例中是main.cpp。  
add_executable()函数的第一个参数是要编译的可执行文件的名称，第二个参数是要编译的源文件列表。
```cmake
add_executable(hello_cmake main.cpp)
```
>一些人使用的速记方法是让项目名称和可执行文件名称相同。这允许你在CMakeLists.txt中指定如下。
>```cmake
>cmake_minimum_required(VERSION 2.6)
>project(hello_cmake LANGUAGES CXX)
>add_executable(${PROJECT_NAME} main.cpp)
>```
>在这个例子中，project() 函数将创建一个变量 ${PROJECT_NAME}，其值为 hello_cmake。然后，这个变量可以传递给 add_executable() 函数，以输出一个 'hello_cmake' 可执行文件。
## Binary Directory
你运行 cmake 命令的根目录或顶层目录被称为 CMAKE_BINARY_DIR，是你所有二进制文件的根目录。CMake 支持在原地(in-place)构建和源外(out-of-source)构建和生成二进制文件。
### In-Place Build
元帝构建会在与源代码相同的目录结构中生成所有临时构建文件。这意味着所有的Makefile和对象文件都与你的正常代码交错在一起。要创建一个就地编译目标，请在你的根目录下运行 cmake 命令。例如  
```shell script
$ cmake .
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/A-hello-cmake

A-hello-cmake$ tree
.
├── CMakeCache.txt
├── CMakeFiles
│   ├── 2.8.12.2
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   │   ├── a.out
│   │   │   └── CMakeCCompilerId.c
│   │   └── CompilerIdCXX
│   │       ├── a.out
│   │       └── CMakeCXXCompilerId.cpp
│   ├── cmake.check_cache
│   ├── CMakeDirectoryInformation.cmake
│   ├── CMakeOutput.log
│   ├── CMakeTmp
│   ├── hello_cmake.dir
│   │   ├── build.make
│   │   ├── cmake_clean.cmake
│   │   ├── DependInfo.cmake
│   │   ├── depend.make
│   │   ├── flags.make
│   │   ├── link.txt
│   │   └── progress.make
│   ├── Makefile2
│   ├── Makefile.cmake
│   ├── progress.marks
│   └── TargetDirectories.txt
├── cmake_install.cmake
├── CMakeLists.txt
├── main.cpp
├── Makefile
```
### Out-of-Source Build
源外构建允许您创建一个目录，该目录可以放在文件系统的任何地方。所有的临时构建文件和对象文件都位于这个目录中，以保持你的源代码树的干净。要创建一个源外编译，在编译文件夹中运行cmake命令，并将其指向包含CMakeLists.txt文件的目录。使用源外编译，如果你想从头开始重新创建你的 cmake 环境，你只需要删除你的编译目录，然后重新运行 cmake。  
例如
```shell script
$ mkdir build
$ cd build/
$ cmake ..
make: Nothing to be done for `..'.
matrim@freyr:~/workspace/cmake-examples/01-basic/A-hello-cmake/build$ cmake ..
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/A-hello-cmake/build
$ cd ..
$ tree
.
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 2.8.12.2
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── a.out
│   │   │   │   └── CMakeCCompilerId.c
│   │   │   └── CompilerIdCXX
│   │   │       ├── a.out
│   │   │       └── CMakeCXXCompilerId.cpp
│   │   ├── cmake.check_cache
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── hello_cmake.dir
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── DependInfo.cmake
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   └── progress.make
│   │   ├── Makefile2
│   │   ├── Makefile.cmake
│   │   ├── progress.marks
│   │   └── TargetDirectories.txt
│   ├── cmake_install.cmake
│   └── Makefile
├── CMakeLists.txt
├── main.cpp
```
注意：本教程中所有项目均使用源外构建
# Building the Examples
下面输出源于构建当前演示项目
```shell script
$ mkdir build
$ cd build/
$ cmake ..
-- The CXX compiler identification is GNU 9.3.0
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/leetcode/CLionProjects/cmake-examples-cn/01-basic/A-hello-cmake/build
$ cmake --build . 或者 make
$ ./hello_cmake 
Hello CMake!
```
