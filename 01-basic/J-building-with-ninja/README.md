# Introduction
如前所述，CMake是一个元构建系统(meta-build system)，可以用来为许多其他构建工具创建构建文件。这个例子展示了如何让 CMake 使用 ninja 构建工具。

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
## Generators
CMake generators 负责为底层构建系统编写输入文件。
例如 Makefiles 。
运行 cmake --help 会显示可用的生成器。对于 cmake v2.8.12.2，我的系统中支持的生成器包括
```shell script
Generators

The following generators are available on this platform:
  Unix Makefiles              = Generates standard UNIX makefiles.
  Ninja                       = Generates build.ninja files (experimental).
  CodeBlocks - Ninja          = Generates CodeBlocks project files.
  CodeBlocks - Unix Makefiles = Generates CodeBlocks project files.
  Eclipse CDT4 - Ninja        = Generates Eclipse CDT 4.0 project files.
  Eclipse CDT4 - Unix Makefiles
                              = Generates Eclipse CDT 4.0 project files.
  KDevelop3                   = Generates KDevelop 3 project files.
  KDevelop3 - Unix Makefiles  = Generates KDevelop 3 project files.
  Sublime Text 2 - Ninja      = Generates Sublime Text 2 project files.
  Sublime Text 2 - Unix Makefiles
                              = Generates Sublime Text 2 project files.Generators
```
如[本文](https://stackoverflow.com/a/25946781)所指定，CMake包括不同类型的生成器，例如命令行，IDE和Extra生成器

## Command-Line Build Tool Generators
这些生成器是针对命令行构建工具的，比如Make和Ninja。选择的工具链必须在使用 CMake 生成构建系统之前进行配置。

支持的生成器包括：

  - Borland Makefiles
  - MSYS Makefiles
  - MinGW Makefiles
  - NMake Makefiles
  - NMake Makefiles JOM
  - Ninja
  - Unix Makefiles
  - Watcom WMake
>注意：在这个例子中，通过命令 `sudo apt-get install ninja-build` 来安装 ninja。

## IDE Build Tool Generators
这些生成器适用于自带编译器的集成开发环境。例如Visual Studio和Xcode，它们本身就包含一个编译器。

支持的生成器包括：

  - Visual Studio 6
  - Visual Studio 7
  - Visual Studio 7 .NET 2003
  - Visual Studio 8 2005
  - Visual Studio 9 2008
  - Visual Studio 10 2010
  - Visual Studio 11 2012
  - Visual Studio 12 2013
  - Xcode

## Extra Generators
这些生成器可以创建一个配置，以便与其他IDE工具一起工作，配置必须包含在IDE或命令行生成器中。
支持的生成器包括：

 - CodeBlocks
 - CodeLite
 - Eclipse CDT4
 - KDevelop3
 - Kate
 - Sublime Text 2
## Calling a Generator
例如，要想调用 CMake generator，你可以使用 -G 命令行开关。
```shell script
cmake .. -G Ninja
```
做完上述工作后，CMake将生成所需的Ninja构建文件，可以使用ninja命令来运行。
# Building the Examples
```shell script
$ mkdir build.ninja

$ cd build.ninja/

$ cmake .. -G Ninja
-- The C compiler identification is GNU 4.8.4
-- The CXX compiler identification is GNU 4.8.4
-- Check for working C compiler using: Ninja
-- Check for working C compiler using: Ninja -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler using: Ninja
-- Check for working CXX compiler using: Ninja -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/J-building-with-ninja/build.ninja

$ ninja -v
[1/2] /usr/bin/c++     -MMD -MT CMakeFiles/hello_cmake.dir/main.cpp.o -MF "CMakeFiles/hello_cmake.dir/main.cpp.o.d" -o CMakeFiles/hello_cmake.dir/main.cpp.o -c ../main.cpp
[2/2] : && /usr/bin/c++      CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake  -rdynamic && :

$ ls
build.ninja  CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_cmake  rules.ninja

$ ./hello_cmake
Hello CMake!
```