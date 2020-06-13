自从C++11和C++14发布以来，一个常见的用例是调用编译器来使用这些标准。随着CMake的发展，它增加了一些功能，使之更容易实现，而新版本的CMake也改变了实现的方式。

下面的例子显示了设置C++标准的不同方法，以及它们在什么版本的CMake中可用。

这些例子包括:

  - i-common-method. 一个简单的版本，应该适用于大多数版本的CMake。
  - ii-cxx-standard. 使用CMake v3.1中引入的CMAKE_CXX_STANDARD变量。
  - iii-compile-features. 使用 CMake v3.1 中引入的 target_compile_features 函数。
