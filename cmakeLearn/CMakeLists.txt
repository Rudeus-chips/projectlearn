# 设置 CMake 最低版本
cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本
project(MyProject VERSION 1.0)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 添加头文件目录
include_directories(include)

# 添加源文件
set(SOURCES
    src/hello.cpp
    src/hello.h
)

# 生成可执行文件
add_executable(MyProject ${SOURCES})