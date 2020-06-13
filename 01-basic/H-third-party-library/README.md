# Introduction
几乎所有稍显复杂的项目都会有包含第三方库、头文件或程序的要求。CMake支持使用find_package()函数来寻找这些工具的路径。这将从CMAKE_MODULE_PATH中的文件夹列表中搜索格式为 "FindXXX.cmake "的CMake模块。在linux上，默认的搜索路径将包括/usr/share/cmake/Modules。在我的系统中，这包括了对大约142个常用第三方库的支持。

本教程中的文件如下。
```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt[] - 包含cmake相关命令
  * main.cpp[] - 包含main的源文件

# Requirements
本例要求将boost库安装在默认的系统位置。

# Concepts

## Finding a Package
find_package()函数将从CMAKE_MODULE_PATH中的文件夹列表中以 "FindXXX.cmake "的形式搜索CMake模块。find_package参数的确切格式取决于你要找的模块，这通常在CMAKE_MODULE_PATH的顶部有记载。这通常在 FindXXX.cmake 文件的顶部有记载。

下面是一个寻找boost的基本例子。
```cmake
find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)
```
参数分别是:

  - Boost - 库的名称。这是用来查找模块文件FindBoost.cmake的一部分。
  - 1.46.1 - boost库最低版本号
  - REQUIRED - 告诉cmake,该库是必须的,如果找不到将导致cmake失败
  - COMPONENTS - 待查找库的列表
## Checking if the package is found
大多数软件包会设置一个变量XXX_FOUND，可以用来检查该软件包是否在系统中可用。

在这个例子中，这个变量是Boost_FOUND。
```cmake
if(Boost_FOUND)
    message ("boost found")
    include_directories(${Boost_INCLUDE_DIRS})
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()
```
## Exported Variables
在找到一个包之后，它通常会输出一些变量，这些变量可以告诉用户在哪里可以找到库、头文件或可执行文件。类似于XXX_FOUND变量，这些变量是特定于软件包的，通常被记录在FindXXX.cmake文件的顶部。

在这个例子中，输出的变量包括。

  - `Boost_INCLUDE_DIRS` - boost库头文件所在位置
在某些情况下，你也可以通过使用ccmake或cmake-gui来检查这些变量。

## Alias / Imported targets
大多数现代的 CMake 库都会在他们的模块文件中导出 ALIAS 目标。导入目标的好处是它们也可以在CMakeLists.txt中直接使用这些目标别名。

例如，从 CMake 的 v3.5+开始，Boost 模块就支持这个功能。类似于在libraires中使用自己的ALIAS目标，模块中的ALIAS可以让引用找到的目标变得更容易。

在Boost的情况下，所有的目标都是用Boost::标识符，然后用子系统的名称导出的。例如，你可以使用

  * `Boost::boost` 指代库的头文件
  * `Boost::system` 指代system库
  * `Boost::filesystem` 指代filesystem库
和自己写的目标一样，boost库中这些目标也包含了它们的依赖关系，所以针对 Boost::filesystem 的链接会自动添加 Boost::boost 和 Boost::system 的依赖关系。
链接一个boost目标，可以这样写
```cmake
target_link_libraries( third_party_include
      PRIVATE
          Boost::filesystem
  )
```
## Non-alias targets
虽然大多数现代的库都使用别名目标，但并不是所有的模块都已经更新。在库没有更新的情况下，你会经常发现以下变量。

- xxx_INCLUDE_DIRS - 一个指向库的包含目录的变量。

- xxx_LIBRARY - 一个指向库路径的变量。

这些变量可以被添加到你的 target_include_directories 和 target_link_libraries 中。
```cmake
# Include the boost headers
target_include_directories( third_party_include
    PRIVATE ${Boost_INCLUDE_DIRS}
)

# link against the boost libraries
target_link_libraries( third_party_include
    PRIVATE
    ${Boost_SYSTEM_LIBRARY}
    ${Boost_FILESYSTEM_LIBRARY}
)
```

# Building the Example
```cmake
$ mkdir build

$ cd build/

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
-- Boost version: 1.54.0
-- Found the following Boost libraries:
--   filesystem
--   system
boost found
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/H-third-party-library/build

$ make
Scanning dependencies of target third_party_include
[100%] Building CXX object CMakeFiles/third_party_include.dir/main.cpp.o
Linking CXX executable third_party_include
[100%] Built target third_party_include
matrim@freyr:~/workspace/cmake-examples/01-basic/H-third-party-library/build$ ./
CMakeFiles/          third_party_include
matrim@freyr:~/workspace/cmake-examples/01-basic/H-third-party-library/build$ ./third_party_include
Hello Third Party Include!
Path is not relative
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
-- Boost version: 1.54.0
-- Found the following Boost libraries:
--   filesystem
--   system
boost found
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/H-third-party-library/build

$ make
Scanning dependencies of target third_party_include
[100%] Building CXX object CMakeFiles/third_party_include.dir/main.cpp.o
Linking CXX executable third_party_include
[100%] Built target third_party_include

$ ./third_party_include
Hello Third Party Include!
Path is not relative
```