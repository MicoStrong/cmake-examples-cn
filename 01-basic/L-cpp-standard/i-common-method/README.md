# Introduction
这个例子展示了一个设置C++标准的常用方法。这可以用于大多数版本的CMake。然而，如果你使用CMake的最新版本，还有更方便的方法。

本教程中的文件如下。
```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  - link:CMakeLists - 包含cmake相关命令
  - link:main.cpp - 包含main的源文件

# Concepts

## Checking Compile flags
CMake 支持尝试用你传递到函数 CMAKE_CXX_COMPILER_FLAG 中的任何标志来编译程序。结果会被存储在你传递进来的一个变量中。

例如
```cmake
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
```
这个例子将尝试编译一个带有-std=c++11标志的程序，并将结果存储在变量COMPILER_SUPPORTS_CXX11中。

include(CheckCXXCompilerFlag)这一行告诉CMake包含这个,函数CHECK_CXX_COMPILER_FLAG便可以使用。
## Adding the flag
一旦你确定了编译是否支持一个标志，你就可以使用标准的 cmake 方法将这个标志添加到一个目标上。在这个例子中，我们使用 CMAKE_CXX_FLAGS 来将这个标志添加到所有目标上。
```cmake
if(COMPILER_SUPPORTS_CXX11)#
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)#
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()
```
上面的例子只检查了gcc版本的编译标志，并且支持从C++11回退到标准化前的C++0x标志。在实际使用中，你可能希望检查C14，或者增加对不同编译方法的支持，例如`std=gnu11`。
# Building the Examples
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
-- Performing Test COMPILER_SUPPORTS_CXX11
-- Performing Test COMPILER_SUPPORTS_CXX11 - Success
-- Performing Test COMPILER_SUPPORTS_CXX0X
-- Performing Test COMPILER_SUPPORTS_CXX0X - Success
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/L-cpp-standard/i-common-method/build

$ make VERBOSE=1
/usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/i-common-method -B/data/code/01-basic/L-cpp-standard/i-common-method/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
make[2]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
cd /data/code/01-basic/L-cpp-standard/i-common-method/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/i-common-method /data/code/01-basic/L-cpp-standard/i-common-method /data/code/01-basic/L-cpp-standard/i-common-method/build /data/code/01-basic/L-cpp-standard/i-common-method/build /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
Dependee "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Dependee "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Scanning dependencies of target hello_cpp11
make[2]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
make[2]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
/usr/bin/cmake -E cmake_progress_report /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles 1
[100%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
/usr/bin/c++    -std=c++11   -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/i-common-method/main.cpp
Linking CXX executable hello_cpp11
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
/usr/bin/c++    -std=c++11    CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
make[2]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
/usr/bin/cmake -E cmake_progress_report /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles  1
[100%] Built target hello_cpp11
make[1]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles 0
```