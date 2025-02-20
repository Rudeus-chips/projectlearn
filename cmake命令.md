使用 CMake 编译自己写的项目是一个标准化的流程，以下是详细的步骤和示例：

---

### **1. 项目结构**
假设你的项目结构如下：
```
my_project/
├── CMakeLists.txt       # CMake 配置文件
├── include/             # 头文件目录
│   └── mylib.h
├── src/                 # 源文件目录
│   ├── main.cpp
│   └── mylib.cpp
└── build/               # 构建目录（可选）
```

---

### **2. 编写 `CMakeLists.txt`**
在项目根目录下创建 `CMakeLists.txt` 文件，内容如下：

```cmake
# 设置 CMake 最低版本
cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本
project(MyProject VERSION 1.0)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 添加头文件目录
include_directories(include)

# 添加源文件
set(SOURCES
    src/main.cpp
    src/mylib.cpp
)

# 生成可执行文件
add_executable(MyProject ${SOURCES})
```

---

### **3. 创建构建目录**
在项目根目录下创建一个 `build` 目录，用于存放构建生成的文件：
```bash
mkdir build
cd build
```

---

### **4. 运行 CMake**
在 `build` 目录中运行 CMake，生成构建系统文件（如 Makefile）：
```bash
cmake ..
```

---

### **5. 编译项目**
使用生成的构建系统文件编译项目：
- **Linux/macOS**：
  ```bash
  make
  ```
- **Windows**：
  ```bash
  cmake --build .
  ```

编译完成后，生成的可执行文件会位于 `build` 目录中。

---

### **6. 运行可执行文件**
在 `build` 目录中运行生成的可执行文件：
```bash
./MyProject
```

---

### **7. 清理构建文件**
如果需要清理构建生成的文件，可以运行：
- **Linux/macOS**：
  ```bash
  make clean
  ```
- **Windows**：
  删除 `build` 目录中的所有文件。

---

### **8. 高级配置**
#### **添加库**
如果你的项目需要链接外部库，可以在 `CMakeLists.txt` 中添加：
```cmake
# 查找库
find_package(SomeLibrary REQUIRED)

# 链接库
target_link_libraries(MyProject SomeLibrary)
```

#### **生成静态库或动态库**
如果需要将部分代码编译为库：
```cmake
# 生成静态库
add_library(MyLibrary STATIC src/mylib.cpp)

# 生成动态库
add_library(MyLibrary SHARED src/mylib.cpp)

# 链接库到可执行文件
target_link_libraries(MyProject MyLibrary)
```

#### **添加子目录**
如果项目包含多个子目录，可以使用 `add_subdirectory`：
```cmake
add_subdirectory(subproject)
```

---

### **9. 示例项目**
#### **`include/mylib.h`**
```cpp
#ifndef MYLIB_H
#define MYLIB_H

void print_message();

#endif
```

#### **`src/mylib.cpp`**
```cpp
#include "mylib.h"
#include <iostream>

void print_message() {
    std::cout << "Hello from mylib!" << std::endl;
}
```

#### **`src/main.cpp`**
```cpp
#include "mylib.h"

int main() {
    print_message();
    return 0;
}
```

#### **`CMakeLists.txt`**
```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

include_directories(include)

set(SOURCES
    src/main.cpp
    src/mylib.cpp
)

add_executable(MyProject ${SOURCES})
```

---

### **10. 常见问题**
- **问题**：CMake 找不到编译器。  
  **解决**：确保已安装编译器（如 GCC、Clang 或 MSVC），并将其添加到系统 `PATH`。

- **问题**：`make` 报错。  
  **解决**：检查 `CMakeLists.txt` 中的路径和文件名是否正确。

- **问题**：Windows 上无法生成构建文件。  
  **解决**：指定生成器，如：
    ```bash
    cmake -G "MinGW Makefiles" ..
    ```

---

通过以上步骤，你可以使用 CMake 编译自己的项目。如果有更多问题，请随时告诉我！