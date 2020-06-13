# Introduction
这个例子展示了如何使用CMAKE_CXX_STANDARD变量来设置C++标准。这个变量从CMake v3.1开始就有了。

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

## Using CXX_STANDARD property
设置 CMAKE_CXX_STANDARD 变量会影响所有目标的 CXX_STANDARD 属性。这将导致CMake在编译时设置适当的标志。
>注意：CMAKE_CXX_STANDARD 变量在没有失败的情况下会返回到最接近的标准。例如，如果你请求 -std=gnu11`，你可能最终会得到`std=gnu0x。
> 这可能会导致在编译时出现意外的失败。
# Building the Examples
```shell script
$ mkdir build
$ cd build


$ cmake ..
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build

$ make VERBOSE=1
/usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/ii-cxx-standard -B/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
cd /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/ii-cxx-standard /data/code/01-basic/L-cpp-standard/ii-cxx-standard /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
Dependee "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Dependee "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Scanning dependencies of target hello_cpp11
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
[ 50%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
/usr/bin/c++     -std=gnu++11 -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/ii-cxx-standard/main.cpp
[100%] Linking CXX executable hello_cpp11
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
/usr/bin/c++      CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
[100%] Built target hello_cpp11
make[1]: Leaving directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
```