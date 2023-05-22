---
date created: 2023-05-07 10:35
date updated: 2023-05-07 10:53
---

## 重要的指令

- `cmake_minimum_required` --- 指定cmake最小版本

```cmake
cmake_minimum_required(VERSION 2.8.3)
```

- `project()` --- 指定工程名

```cmake
project(SWAP)
```

- `set` 定义变量

```cmake
set(SRC sayhello.cpp hello.cpp)
# SRC --- sayhello.cpp hello.cpp
```

- `include_directories` 添加多个头文件搜索路径

```cmake
include_directories(./include)
```

- `link_directories`添加库文件搜索路径

```cmake
link_directories(./src)
```

- `add_library` 生成库文件

```cmake
add_library({name} [SHARED|STATIC|MODULE] {路径})

add_library(hello SHARED ${SRC})
```

- `add_compile_options` 添加编译参数

```cmake
add_compile_options(<options>)
add_compile_options(-Wall -std=c++11 -O2)
```

- `add_executable` 生成可执行文件

```cmake
add_executable({exename} {sources})
add_executable(main main.cpp)
#编译文件
```

- `target_link_directories` 为target添加需要连接的共享库

相当于`g++ -i`

```cmake
target_link_directories(main hello)
#main --- 输出的可执行文件
#hello --- 我们指定的库文件
```

- `add_subdirectory` 添加存放源文件的子目录，并可以指定中间二进制和目标二进制的存放位置

```cmake
add_subdirectory(source_dir [binary dir] [exclude from all])
add_subdirectory(src)
#子目录中必须有cmake文件
```

- `aux_source_directory` 发现目录下所有的源代码文件并将列表放入一个变量中，通常用来自动构建源文件列表

```cmake
aux_source_directory(. SRC)
add_executable(main ${SRC})
```


## 常用变量

- `${CMAKE_C_FLAGS}` gcc编译选项
- `${CMAKE_CXX_FLAGS}`  g++编译选项

```cmake
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS}  --std=c++11)
#在编译选项之后添加 --std=c++11
```

- `${CMAKE_BUILD_TYPE}` 设置编译类型 ---- Debug Release

```cmake
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_BUILD_TYPE Release)
```

- `${CMAKE_BINARY_DIR} ${PROJECT_BINARY_DIR} ${<projectname>_BINARY_DIR}` 

三者指向相同，都是用来

- `${CMAKE_SOURCE_DIR} ${PROJECT_SOURCE_DIR} ${<projectname>_SOURCE_DIR}` 
- `${CMAKE_C_COMPILER} ${CMAKE_CXX_COMPILER}` 指定编译器
- `EXEXUTABLE_OUTPUT_PATH`  指定可以执行文件输出的目录
- `LIBRARY_OUTPUT_PATH`  库文件输出的目录

## 实战

```sh
➜  swap tree
.
├── CMakeLists.txt
├── include
│   └── swap.h
├── main.cpp
└── src
    └── swap.cpp
```



```cmake
cmake_minimum_required(VERSION 3.8)

project(SWAP)

#添加参数
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O2 -Wall")

include_directories(include)

add_executable(main main.cpp src/swap.cpp)
```