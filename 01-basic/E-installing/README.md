# Introduction
这个例子展示了如何生成一个`make install`目标，以便在你的系统上安装文件和二进制文件。  
本案例基于之前的共享库例子。  
本案例使用的文件如下。
```
$ tree
.
├── cmake-examples.conf
├── CMakeLists.txt
├── include
│   └── installing
│       └── Hello.h
├── README.adoc
└── src
    ├── Hello.cpp
    └── main.cpp
```

  - CMakeLists.txt[] - 包含cmake相关命令
  - cmake-examples.conf[] - 配置文件
  - include/installing/Hello.h[] - 头文件
  - src/Hello.cpp[] - 源文件
  - src/main.cpp[] - 包含main的源文件

# Concepts
## Installing
CMake提供了支持`make install`，安装目标的功能，允许用户安装二进制文件、库和其他文件。基本的安装位置由变量 CMAKE_INSTALL_PREFIX 控制，它可以用 ccmake 或调用 cmake ... -DCMAKE_INSTALL_PREFIX=/install/location 来设置。  
安装的文件由install()函数控制。
```cmake
install (TARGETS cmake_examples_inst_bin
    DESTINATION bin)
```
将二进制目标 cmake_examples_inst_bin 安装到 ${CMAKE_INSTALL_PREFIX}/bin 中。
```cmake
install (TARGETS cmake_examples_inst
    LIBRARY DESTINATION lib)
```
将共享库目标 cmake_examples_inst 安装到 ${CMAKE_INSTALL_PREFIX}/lib 中。
>注意：这在windows上可能无法工作。在有DLL目标的平台上，您可能需要添加以下内容。
>```cmake
>install (TARGETS cmake_examples_inst
>     LIBRARY DESTINATION lib
>     RUNTIME DESTINATION bin)
>```
```cmake
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/
    DESTINATION include)
```
将针对 cmake_examples_inst 库开发的头文件安装到 ${CMAKE_INSTALL_PREFIX}/include 目录中。
```cmake
install (FILES cmake-examples.conf
    DESTINATION etc)
```
将配置文件安装到 ${CMAKE_INSTALL_PREFIX}/etc 中。  
在执行`make install`命令之后，CMake会生成一个install_manifest.txt文件，其中包括所有安装文件的详细信息。  
>注意：如果你以root身份运行make install命令，install_manifest.txt文件将由root拥有。

# Building the Example
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/E-installing/build

$ make
Scanning dependencies of target cmake_examples_inst
[ 50%] Building CXX object CMakeFiles/cmake_examples_inst.dir/src/Hello.cpp.o
Linking CXX shared library libcmake_examples_inst.so
[ 50%] Built target cmake_examples_inst
Scanning dependencies of target cmake_examples_inst_bin
[100%] Building CXX object CMakeFiles/cmake_examples_inst_bin.dir/src/main.cpp.o
Linking CXX executable cmake_examples_inst_bin
[100%] Built target cmake_examples_inst_bin

$ sudo make install
[sudo] password for matrim:
[ 50%] Built target cmake_examples_inst
[100%] Built target cmake_examples_inst_bin
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/bin/cmake_examples_inst_bin
-- Removed runtime path from "/usr/local/bin/cmake_examples_inst_bin"
-- Installing: /usr/local/lib/libcmake_examples_inst.so
-- Installing: /usr/local/etc/cmake-examples.conf

$ cat install_manifest.txt
/usr/local/bin/cmake_examples_inst_bin
/usr/local/lib/libcmake_examples_inst.so
/usr/local/etc/cmake-examples.conf

$ ls /usr/local/bin/
cmake_examples_inst_bin

$ ls /usr/local/lib
libcmake_examples_inst.so

$ ls /usr/local/etc/
cmake-examples.conf

$ LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib cmake_examples_inst_bin
Hello Install!
```
## Extra Notes
覆盖默认安装位置  
如前所述，默认的安装位置是由CMAKE_INSTALL_PERFIX设置的，默认为/usr/local/。  
如果你想为所有用户改变这个默认位置，你可以在添加任何二进制文件或库之前，在你的顶层CMakeLists.txt中添加以下代码。
```cmake
if( CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT )
  message(STATUS "Setting default CMAKE_INSTALL_PREFIX path to ${CMAKE_BINARY_DIR}/install")
  set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "The path to use for make install" FORCE)
endif()
```
本例将默认的安装位置设置为构建目录下。
## DESTDIR
如果你想分阶段安装，以确认所有的文件都包含在里面，`make install` 安装程序时支持DESTDIR参数。
```shell script
make install DESTDIR=/tmp/stage
```
这将为所有的安装文件创建安装路径 ${DESTDIR}/${CMAKE_INSTALL_PREFIX}。在这个例子中，它会将所有文件安装到/tmp/stage/usr/local这个路径下。
```shell script
$ tree /tmp/stage
/tmp/stage
└── usr
    └── local
        ├── bin
        │   └── cmake_examples_inst_bin
        ├── etc
        │   └── cmake-examples.conf
        └── lib
            └── libcmake_examples_inst.so
```
## Uninstall
默认情况下，CMake不支持`make uninstall`。关于如何卸载目标的细节，请看这个 [FAQ](https://gitlab.kitware.com/cmake/community/-/wikis/FAQ)  
要想从这个例子中删除文件，你可以使用一个简单的方法。
```shell script
sudo xargs rm < install_manifest.txt
```