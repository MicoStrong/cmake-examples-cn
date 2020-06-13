# Introduction
这个例子展示了如何使用 target_compile_features 函数来设置 C++ 标准。这是从CMake v3.1开始提供的。

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

## Using target_compile_features
在一个目标上调用target_compile_features函数，会查看传入的标志(flag)，并确定正确的编译器标志用于你的目标。
```cmake
target_compile_features(hello_cpp11 PUBLIC cxx_auto_type)
```
与其他 target_* 函数一样，您可以为选定的目标指定标志的可见性范围，这将影响目标的 INTERFACE_COMPILE_FEATURES 属性。

可用标志的列表可以从CMAKE_CXX_COMPILE_FEATURES变量中找到。你可以使用下面的代码来获取可用特性的列表。
```cmake
message("List of compile features: ${CMAKE_CXX_COMPILE_FEATURES}")
```
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
List of compile features: cxx_template_template_parameters;cxx_alias_templates;cxx_alignas;cxx_alignof;cxx_attributes;cxx_auto_type;cxx_constexpr;cxx_decltype;cxx_decltype_incomplete_return_types;cxx_default_function_template_args;cxx_defaulted_functions;cxx_defaulted_move_initializers;cxx_delegating_constructors;cxx_deleted_functions;cxx_enum_forward_declarations;cxx_explicit_conversions;cxx_extended_friend_declarations;cxx_extern_templates;cxx_final;cxx_func_identifier;cxx_generalized_initializers;cxx_inheriting_constructors;cxx_inline_namespaces;cxx_lambdas;cxx_local_type_template_args;cxx_long_long_type;cxx_noexcept;cxx_nonstatic_member_init;cxx_nullptr;cxx_override;cxx_range_for;cxx_raw_string_literals;cxx_reference_qualified_functions;cxx_right_angle_brackets;cxx_rvalue_references;cxx_sizeof_member;cxx_static_assert;cxx_strong_enums;cxx_thread_local;cxx_trailing_return_types;cxx_unicode_literals;cxx_uniform_initialization;cxx_unrestricted_unions;cxx_user_literals;cxx_variadic_macros;cxx_variadic_templates;cxx_aggregate_default_initializers;cxx_attribute_deprecated;cxx_binary_literals;cxx_contextual_conversions;cxx_decltype_auto;cxx_digit_separators;cxx_generic_lambdas;cxx_lambda_init_captures;cxx_relaxed_constexpr;cxx_return_type_deduction;cxx_variable_templates
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/L-cpp-standard/iii-compile-features/build


$ make VERBOSE=1
/usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/iii-compile-features -B/data/code/01-basic/L-cpp-standard/iii-compile-features/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
cd /data/code/01-basic/L-cpp-standard/iii-compile-features/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/iii-compile-features /data/code/01-basic/L-cpp-standard/iii-compile-features /data/code/01-basic/L-cpp-standard/iii-compile-features/build /data/code/01-basic/L-cpp-standard/iii-compile-features/build /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
Dependee "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Dependee "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Scanning dependencies of target hello_cpp11
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
[ 50%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
/usr/bin/c++     -std=gnu++11 -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/iii-compile-features/main.cpp
[100%] Linking CXX executable hello_cpp11
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
/usr/bin/c++      CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
[100%] Built target hello_cpp11
make[1]: Leaving directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles 0
```