---
data: 2026-03-25
---


[CMake教程 | 菜鸟教程](https://www.runoob.com/cmake/cmake-tutorial.html)

[CMake 保姆级教程（上） | 爱编程的大丙](https://subingwen.cn/cmake/CMake-primer/)

[CMake 保姆级教程（下） | 爱编程的大丙](https://subingwen.cn/cmake/CMake-advanced/)

[文档教程](subingwen.cn)
## CMake介绍及使用流程
### 介绍

1. ==CMake==是个一个开源的跨平台自动化建构系统，用来管理软件建置的程序，并不依赖于某特定编译器，并可支持多层目录、多个应用程序与多个函数库。
2. CMake通过使用简单的配置文件 CMakeLists.txt，自动生成不同平台的构建文件（如 ==Makefile、Ninja==构建文件、Visual Studio 工程文件等），简化了项目的编译和构建过程。
3. CMake本身不是构建工具，而是生成构建系统的工具，它生成的构建系统可以使用不同的编译器和工具链。
### 使用流程

- **CMakeLists.txt 文件：** CMake 的配置文件，用于定义项目的构建规则、依赖关系、编译选项等。每个 CMake 项目通常包含一个或多个 CMakeLists.txt 文件。
- 使用流程
1. **编写 CMakeLists.txt 文件：** 定义项目的构建规则和依赖关系。
2. **生成构建文件：** 使用 CMake 生成适合当前平台的构建系统文件（例如 Makefile、Visual Studio 工程文件）。
3. **执行构建：** 使用生成的构建系统文件（如 `make`、`ninja`、`msbuild`）来编译项目。
![[Pasted image 20260325190241.png]]

- [基于此编写的笔记](https://www.bilibili.com/video/BV14s4y1g7Zj/?spm_id_from=333.337.search-card.all.click&vd_source=c254c8f1269981c1751209d83de7bd01)
### 源文件流程
- 预处理，编译，汇编，链接
![[Pasted image 20260325192240.png]]
### 安装
- 下载相应的msi(微软安装程序)。
- 添加对应的**cmake/bin**文件夹于环境变量**path**中，以便使用命令行时可以直接进行查找
- linux系统中已默认安装
## 编写一个简单的CMakeLists.txt

使用**WSL+VS Code**进行相关学习
[[WSL介绍及安装]]

- 注释
```
#
#[[]]
```
- 指定使用的cmake最低版本
```
cmake_minimum_required(VERSION 3.0)
```
- 定义工程名称，工程版本，工程描述，支持语言等
```
# PROJECT 指令的语法是：
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
       [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
       [DESCRIPTION <project-description-string>]
       [HOMEPAGE_URL <url-string>]
       [LANGUAGES <language-name>...])
```
- 生成可执行文件
```
add_executable(可执行程序名 源文件名称)

# 例
add_executable(MyExecutable main.cpp other_file.cpp)
```
- 运行。CMakeLists.txt文件在当前目录
```
cmake . 

# cmake ..  编译在上一级的CMakeLists.txt
```
  - 在cmake运行目录里生成Makefile等文件。执行make命令，生成可执行文件
  - 可再单独建立文件夹执行cmake , make 命令。

## 其他一些操作

- 以下不包含所有操作，遇到需要使用的功能可以查询官方文档
### 使用set定义变量
- 定义变量处理多文件编译
```
# 方式1: 各个源文件之间使用空格间隔
# set(SRC_LIST add.c  div.c   main.c  mult.c  sub.c)

# 方式2: 各个源文件之间使用分号 ; 间隔
set(SRC_LIST add.c;div.c;main.c;mult.c;sub.c)
add_executable(app  ${SRC_LIST})
```
- 指定C++标准。以便可以使用C++新版本的特性
```
#增加-std=c++11
set(CMAKE_CXX_STANDARD 11)
#增加-std=c++14
set(CMAKE_CXX_STANDARD 14)
#增加-std=c++17
set(CMAKE_CXX_STANDARD 17)
```
- 在执行cmake命令的时候指定出这个宏的值
```
#增加-std=c++11
cmake CMakeLists.txt文件路径 -DCMAKE_CXX_STANDARD=11
#增加-std=c++14
cmake CMakeLists.txt文件路径 -DCMAKE_CXX_STANDARD=14
#增加-std=c++17
cmake CMakeLists.txt文件路径 -DCMAKE_CXX_STANDARD=17
```

- 指定可执行程序输出的路径
```
set(HOME /home/robin/Linux/Sort)
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
# 文件路径不存在会自动生成
# 由于可执行程序是基于 cmake 命令生成的 makefile 文件然后再执行 make 命令得到的，所以如果此处指定可执行程序生成路径的时候使用的是相对路径 ./xxx/xxx，那么这个路径中的 ./ 对应的就是 makefile 文件所在的那个目录。
```
### 搜索文件
- 查找某个路径下的所有源文件
```
aux_source_directory(< dir > < variable >)

# dir：要搜索的目录
# variable：将从dir目录下搜索到的源文件列表存储到该变量中
```
- 使用file命令查找，支持递归查找和查找源文件，功能更强大
```
file(GLOB/GLOB_RECURSE 变量名 要搜索的文件路径和文件类型)

# GLOB: 将指定目录下搜索到的满足条件的所有文件名生成一个列表，并将其存储到变量中。
# GLOB_RECURSE：递归搜索指定目录，将搜索到的满足条件的文件名生成一个列表，并将其存储到变量中。
```
- 举例
```
file(GLOB MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
file(GLOB MAIN_HEAD ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)

# CMAKE_CURRENT_SOURCE_DIR 宏表示当前访问的 CMakeLists.txt 文件所在的路径
```
### 包含头文件路径
- 设置要包含的头文件路径
```
include_directories(headpath)
```
- 以上内容综合举例
![[Pasted image 20260328191245.png]]

![[Pasted image 20260328191429.png]]

- 其中的**PROJECT_SOURCE_DIR**宏一般指工程的根目录
### 制作动态库或静态库
 - 此已满足生成一些静态库或动态库以提供给第三方使用
  1. 制作静态库
  ```
  add_library(库名称 STATIC 源文件1 [源文件2] ...) 
  ```
  2. 制作动态库
  ```
  add_library(库名称 SHARED 源文件1 [源文件2] ...) 
  ```
  - 指定动态库的输出路径
  1. 仅适用于动态库
  ```
  # 设置动态库生成路径
	set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
	add_library(calc SHARED ${SRC_LIST})
  # 通过set命令给EXECUTABLE_OUTPUT_PATH宏设置了一个路径，这个路径就是可执行文件生成的路径。
  ```
  2. 两者都适用
```
# 由于在Linux下生成的静态库默认不具有可执行权限，所以在指定静态库生成的路径的时候就不能使用EXECUTABLE_OUTPUT_PATH宏了，而应该使用LIBRARY_OUTPUT_PATH，这个宏对应静态库文件和动态库文件都适用。

# 设置动态库/静态库生成路径
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
```
### 链接包含系统自带的一些静态库和动态库文件
- 链接静态库
```
link_libraries(<static lib> [<static lib>...])

# 参数一: 指定出要链接的静态库的名字
# 可以是全名 libxxx.a
# 也可以是掐头（lib）去尾（.a）之后的名字 xxx

# 参数2-N：要链接的其它静态库的名字
```
- 该静态库不是系统提供的，还需要指定路径
```
# 包含静态库路径
link_directories(${PROJECT_SOURCE_DIR}/lib)
# 链接静态库
link_libraries(calc)
```

- 链接动态库
```
target_link_libraries(
    <target> 
    <PRIVATE|PUBLIC|INTERFACE> <item>... 
    [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)
    
#	target：指定要加载的库的文件的名字
#	
#	该文件可能是一个源文件
#	该文件可能是一个动态库/静态库文件
#	该文件可能是一个可执行文件
#	PRIVATE|PUBLIC|INTERFACE：动态库的访问权限，默认为PUBLIC
#	
#	如果各个动态库之间没有依赖关系，无需做任何设置，三者没有没有区别，一般无需指定，使用默认的PUBLIC 即可。
#	
#	动态库的链接具有传递性，如果动态库 A 链接了动态库B、C，动态库D链接了动态库A，此时动态库D相当于也链接了动态库B、C，并可以使用动态库B、C中定义的方法。
```
- 在cmake中指定要链接的动态库的时候，应该将命令写到生成了可执行文件之后
```
# 添加并指定最终生成的可执行程序名
add_executable(app ${SRC_LIST})
# 指定可执行程序要链接的动态库名字
target_link_libraries(app pthread)
```
- 链接自己编写的动态库时需要指定路径
```
# 指定要链接的动态库的路径
link_directories(${PROJECT_SOURCE_DIR}/lib)
# 添加并生成一个可执行程序
add_executable(app ${SRC_LIST})
# 指定要链接的动态库
target_link_libraries(app pthread calc)
```
### target_link_libraries 和  link_libraries 
- 这两个都可用于链接库
- target_link_libraries 用于指定一个目标（如可执行文件或库）在编译时需要链接哪些库。它支持指定库的名称、路径以及链接库的顺序
```
target_link_libraries(target_name [item1 [item2 [...]]]
                      [<debug|optimized|general> <lib1> [<lib2> [...]]])
```
- link_libraries 用于设置全局链接库，这些库会链接到之后定义的所有目标上。它会影响所有的目标，适用于全局设置，但不如 target_link_libraries 精确
```
link_libraries(lib1 lib2 [...])
```
### 字符串进行拼接
- 使用set
```
set(变量名1 ${变量名1} ${变量名2} ...)

# 将从第二个参数开始往后所有的字符串进行拼接，最后将结果存储到第一个参数中，如果第一个参数中原来有数据会对原数据就行覆盖。
```
- 使用list
```
list(APPEND <list> [<element> ...])

# APPEND表示数据追加
```
![[Pasted image 20260328201108.png]]

![[Pasted image 20260328201125.png]]

### 字符串移除
```
# 移除 main.cpp
list(REMOVE_ITEM SRC_1 ${PROJECT_SOURCE_DIR}/main.cpp)
```

## 一些常用的宏定义

![[Pasted image 20260328201610.png]]