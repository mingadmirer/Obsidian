---
data: 2026-03-25
---


[CMake教程 | 菜鸟教程](https://www.runoob.com/cmake/cmake-tutorial.html)

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
