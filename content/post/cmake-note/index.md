+++
author = "aobara"
title = "Cmake Note"
description = ""
date = "2025-02-17"
categories = [
    "编程",
]
+++

## 起手式
```Cmake
cmake_minimum_required(VERSION 3.21.0)
project(myproj VERSION 0.1.0 LANGUAGES C CXX)
add_executable(myapp main.cpp ...)#编译为myapp.exe
add_library(test STATIC test.hpp)#编译为静态库
add_library(math SHARED main.hpp)#编译为动态库
```

## 生成配置
```sh
mkdir cmake-build 
cd cmake-build
cmake ..


cmake --build .
//启用多线程
cmake --build . -- -m:4
cmake --build . -- -j:4
```
build.sh
```sh
set -e

mkdir -p build
cd build

cmake ..
cmake --build . --target myapp --config Release

```
## 基本用法
```cmake
set(BUILD_SHARED_LIBS ON) #设置bool值
message(STATUS/NOTICE/FATAL_ERROR "BUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}")
#给布尔变量设置默认值。参数：变量的变量名，注释，默认值。
option(BUILD_LIBS "Compile sources into a library" OFF)
 #打印变量
```
>message("This is the NOTICE message")  
>This is the NOTICE message  
>
>message(STATUS "This is the STATUS message")  
>-- This is the STATUS message  
>
>message(FATAL_ERROR "This is the FATAL_ERROR message")  
>CMake Error at CMakeLists.txt:5 (message):  
>  This is the FATAL_ERROR message  
>
>-- Configuring incomplete, errors occurred!

```cmake
list(APPEND sources main.cpp test.hpp tes.cpp)
add_executable(myapp ${sources})   #列举

#访问环境变量
if(DEFINED ENV{TEMP})
  message("TEMP=$ENV{TEMP}")
endif()

#条件变量
if(BUILD_LIBS)
    ...
else()
    ...
endif()
#布尔表达式
NOT BUILD_LIBS 取反
A AND B 且
A OR B 或
A STREQUAL B 两个字符串 A 和 B 相等
EXIST PATH 路径为 PATH 的文件或目录存在
DEFINED VAR 变量 VAR 有定义
```
## 添加附加依赖项
```cmake
target_link_libraries(myapp PRIVATE math)
target_include_directories(myapp PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/include")

事实上：
add_executable(myapp main.cpp test.cpp)
等于
add_executable(myapp main.cpp)
target_link_libraries(myapp PRIVATE test)

如果子模块中有定义的宏
target_compile_definitions(module PRIVATE 宏名)
```
## 生成动态库
```cpp
//要在需生成动态库的模块中配置
#ifdef EXPORT
#define DECL_API __declspec(dllexport)
#else
#define DECL_API __declspec(dllimport)
#endif
```