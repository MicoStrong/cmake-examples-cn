# Introduction
这个例子展示了如何设置一个包含子项目的CMake项目。顶层的CMakeLists.txt调用子目录中的CMakeLists.txt来创建以下内容。

  * sublibrary1 - 静态库
  * sublibrary2 - 只有头文件的库
  * subbinary - 可执行文件

本案例涉及到文件如下：

```
$ tree
.
├── CMakeLists.txt
├── subbinary
│   ├── CMakeLists.txt
│   └── main.cpp
├── sublibrary1
│   ├── CMakeLists.txt
│   ├── include
│   │   └── sublib1
│   │       └── sublib1.h
│   └── src
│       └── sublib1.cpp
└── sublibrary2
    ├── CMakeLists.txt
    └── include
        └── sublib2
            └── sublib2.h
```

  * CMakeLists.txt - 顶层CMakeLists.txt
  * subbinary/CMakeLists.txt - 用于生成可执行文件
  * subbinary/main.cpp - 用于生产可知性文件的源文件
  * sublibrary1/CMakeLists.txt - 用于生成静态库
  * sublibrary1/include/sublib1/sublib1.h
  * sublibrary1/src/sublib1.cpp
  * sublibrary2/CMakeLists.txt - 生产只有头文件的库
  * sublibrary2/include/sublib2/sublib2.h
>Tip
>在这个例子中，我把头文件移到了每个项目的include目录下的一个子文件夹中，而把目标需要的include设置成根include文件夹。这是一个防止文件名冲突的好主意，使用的时候你必须像下面这用包含头文件。
>```c++
>#include "sublib1/sublib1.h"
>```
>这也意味着，如果你为其他用户安装你的库，默认的安装位置将是/usr/local/include/sublib1/sublib1.h
# Concepts
## Adding a Sub-Directory
一个CMakeLists.txt文件可以包含和调用包含CMakeLists.txt文件的子目录。
```cmake
add_subdirectory(sublibrary1)
add_subdirectory(sublibrary2)
add_subdirectory(subbinary)
```
## Referencing Sub-Project Directories
当使用project()命令创建一个项目时，CMake会自动创建一些变量，这些变量可以用来引用项目的细节。这些变量可以被其他子项目或主项目使用。例如，要引用不同项目的源目录，你可以使用.
```cmake
${sublibrary1_SOURCE_DIR}
${sublibrary2_SOURCE_DIR}
```
| Variable | Info |
| ---------------|----------------|
| PROJECT_NAME | 当前project()设置的项目名称 |
| CMAKE_PROJECT_NAME | project()命令设置的第一个项目的名称，即顶层项目 |
| PROJECT_SOURCE_DIR | 当前项目的源代码目录 |
| PROJECT_BINARY_DIR | 当前项目的构建目录 |
| name_SOURCE_DIR | 名为 "name "的项目源码目录，在本例中，创建的源码目录为sublibrary1_SOURCE_DIR、sublibrary2_SOURCE_DIR和subbinary_SOURCE_DIR |
| name_BINARY_DIR | 名为 "name "的项目二进制目录，在本例中，创建的二进制目录为sublibrary1_BINARY_DIR、sublibrary2_BINARY_DIR和subbinary_BINARY_DIR |

## Header only Libraries
如果你想创建一个只有头文件的库，cmake通常对这样的库设置可见性级别INTERFACE，这将创建一个一个没有任何构建输出的目标。更多细节可以从[这里](https://cmake.org/cmake/help/v3.4/command/add_library.html#interface-libraries)找到
```cmake
add_library(${PROJECT_NAME} INTERFACE)
```
When creating the target you can also include directories for that target using
the +INTERFACE+ scope. The +INTERFACE+ scope is use to make target requirements that are used in any Libraries
that link this target but not in the compilation of the target itself.
```cmake
target_include_directories(${PROJECT_NAME}
    INTERFACE
        ${PROJECT_SOURCE_DIR}/include
```

## Referencing Libraries from Sub-Projects

If a sub-project creates a library, it can be referenced by other projects by
calling the name of the project in the `target_link_libraries()` command. This
means that you don't have to reference the full path of the new library and it
is added as a dependency.
```cmake
target_link_libraries(subbinary
    PUBLIC
        sublibrary1
```
Alternatively, you can create an alias target which allows you to reference the
target in read only contexts.

To create an alias target run:
```cmake
add_library(sublibrary2)
add_library(sub::lib2 ALIAS sublibrary2)
```
To reference the alias, just it as follows:
```cmake
target_link_libraries(subbinary
    sub::lib2
```
## Include directories from sub-projects

When adding the libraries from the sub-projects, starting from cmake v3, there is
no need to add the projects include directories in the include directories of the
binary using them.

This is controlled by the scope in the `target_include_directories()` command when creating
the libraries. In this example because the subbinary executable links the sublibrary1
and sublibrary2 libraries it will automatically include the `${sublibrary1_SOURCE_DIR}/inc`
and `${sublibrary2_SOURCE_DIR}/inc` folders as they are exported with the
 +PUBLIC+ and +INTERFACE+ scopes of the libraries.

# Building the example
```shell script
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
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/02-sub-projects/A-basic/build

$ make
Scanning dependencies of target sublibrary1
[ 50%] Building CXX object sublibrary1/CMakeFiles/sublibrary1.dir/src/sublib1.cpp.o
Linking CXX static library libsublibrary1.a
[ 50%] Built target sublibrary1
Scanning dependencies of target subbinary
[100%] Building CXX object subbinary/CMakeFiles/subbinary.dir/main.cpp.o
Linking CXX executable subbinary
[100%] Built target subbinary
```
