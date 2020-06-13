# Introduction
当使用CMake构建时，可以设置C和C++编译器。这个例子和hello-cmake的例子一样，只是它展示了将编译器从默认的gcc改为clang的最基本方法。

本教程中的文件如下。

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * link:CMakeLists.txt - 包含cmake相关命令
  * link:main.cpp - 包含main的源文件

# Concepts

### Compiler Option

cmake提供了一些变量用于控制编译器和链接器
  - CMAKE_C_COMPILER - C语言编译器
  - CMAKE_CXX_COMPILER - C++语言编译器
  - CMAKE_LINKER - 链接器
>注意：
>在这个例子中，clang-3.6是通过命令sudo apt-get install clang-3.6安装的。
### Setting Flags
正如在“F-build-type”示例中所描述的那样，你可以使用cmake-gui或通过命令行传递来设置 CMake 选项。
下面是一个通过命令行传递编译器的例子。
```shell script
cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
```
设置这些选项后，当你运行make时，clang将被用来编译你的二进制文件。这可以从make输出中的以下几行看到。
```shell script
/usr/bin/clang++-3.6     -o CMakeFiles/hello_cmake.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/main.cpp
Linking CXX executable hello_cmake
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cmake.dir/link.txt --verbose=1
/usr/bin/clang++-3.6       CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake -rdynamic
```
# Building the Examples
```shell script
$ mkdir build.clang

$ cd build.clang/

$ cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
-- The C compiler identification is Clang 3.6.0
-- The CXX compiler identification is Clang 3.6.0
-- Check for working C compiler: /usr/bin/clang-3.6
-- Check for working C compiler: /usr/bin/clang-3.6 -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/clang++-3.6
-- Check for working CXX compiler: /usr/bin/clang++-3.6 -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang

$ make VERBOSE=1
/usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang -B/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
make -f CMakeFiles/hello_cmake.dir/build.make CMakeFiles/hello_cmake.dir/depend
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
cd /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/DependInfo.cmake --color=
Dependee "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/DependInfo.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/depend.internal".
Dependee "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/depend.internal".
Scanning dependencies of target hello_cmake
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
make -f CMakeFiles/hello_cmake.dir/build.make CMakeFiles/hello_cmake.dir/build
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles 1
[100%] Building CXX object CMakeFiles/hello_cmake.dir/main.cpp.o
/usr/bin/clang++-3.6     -o CMakeFiles/hello_cmake.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/main.cpp
Linking CXX executable hello_cmake
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cmake.dir/link.txt --verbose=1
/usr/bin/clang++-3.6       CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake -rdynamic
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles  1
[100%] Built target hello_cmake
make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles 0

$ ./hello_cmake
Hello CMake!
```