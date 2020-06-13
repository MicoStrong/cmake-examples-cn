# Introduction
正如“H-third-party-library”提到的第三方库，新版本的CMake允许你使用导入的目标别名(ALIAS target)链接第三方库。

本教程中的文件如下。
```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  - link:CMakeLists - 包含cmake相关命令
  - link:main.cpp - 包含main的源文件

# Requirements
本案例需要满足如下要求

  * CMake 版本 v3.5及以上
  * 将boost库安装在默认的系统位置。

# Concepts
## Imported Target
导入的目标是由FindXXX模块导出的只读目标。  
要包含boost文件系统，你可以执行以下操作。
```cmake
target_link_libraries( imported_targets
      PRIVATE
          Boost::filesystem
  )
```
这将自动链接Boost::filesystem和Boost::system，同时也包括Boost依赖的头文件目录
# Building the Example
```shell script
$ mkdir build

$ cd build/

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
-- Boost version: 1.58.0
-- Found the following Boost libraries:
--   filesystem
--   system
boost found
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/K-imported-targets/build

$ make
Scanning dependencies of target imported_targets
[ 50%] Building CXX object CMakeFiles/imported_targets.dir/main.cpp.o
[100%] Linking CXX executable imported_targets
[100%] Built target imported_targets


$ ./imported_targets
Hello Third Party Include!
Path is not relative
```