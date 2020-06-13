本文案例不建议学习，因为不能做到跨平台。《CMake Cookbook》"Controlling compiler flags"一节中对如何指定编译器选项作了更好阐述。
# Introduction
CMake 支持以多种不同的方式设置编译标志。  
- 使用target_compile_definitions()函数。  
- 使用 CMAKE_C_FLAGS 和 CMAKE_CXX_FLAGS 变量。  
本教程使用文件如下。
```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt[] - 包含cmake相关命令
  * main.cpp[] - 包含main的源文件
# Concepts
# Set Per-Target C++ Flags
在现代CMake推荐为每个target单独设置编译标志，这些标志可以通过target_compile_definitions()函数进行设置。