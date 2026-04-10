[Docker 教程 | 菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)

[Linux 教程 | 菜鸟教程](https://www.runoob.com/linux/linux-tutorial.html)
## 介绍
将软件及其依赖环境打包成轻量可移植的容器环境，便于将环境快速移植
- 一些概念
- **容器（Container）**：轻量化的运行实例，包含应用代码、运行时环境和依赖库。基于镜像创建，与其他容器隔离，共享主机操作系统内核（比虚拟机更高效）。
- **镜像（Image）**：只读模板，定义了容器的运行环境（如操作系统、软件配置等）。通过分层存储（Layer）优化空间和构建速度。
- **Dockerfile**：文本文件，描述如何自动构建镜像（例如指定基础镜像、安装软件、复制文件等）。
- **仓库（Registry）**：存储和分发镜像的平台，如 **Docker Hub**（官方公共仓库）或私有仓库（如 Harbor）。
windows需先安装wsl安装linux子系统
[[WSL介绍及安装]]
## 安装
- 下载并执行Docker官方安装脚本
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

-  启动Docker服务
sudo systemctl start docker # 手动启动当前的服务
sudo systemctl enable docker # 给docker设置开机自启(永久生效)
```

- 拉取最新软件源
```
$ sudo apt-get update
```
- 设置仓库
- 安装依赖包，用于通过https来获取仓库
```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
- 添加GPG密钥，以验证Docker软件包的真实性
```
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
添加中科大镜像源
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
- 或使用docker engine安装，不再赘述
- 卸载docker
```
sudo rm -rf /var/lib/docker
```
## docker使用
- docker允许你在容器内运行程序，使用**docker run** 命令来在容器内运行一个应用程序
- run "Hello World"
```
runoob@runoob:~$ docker run ubuntu:15.10 /bin/echo "Hello world"
Hello world
```
- **ubuntu:15:10** 指定要运行的镜像，docker首先从本地主机上查找镜像是否存在，如果不存在，docker就会从Docker Hub上下载公共镜像
- **/bin/echo "Hello world"** :在启动的容器里执行的命令
可理解完整释义：Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。
![[Pasted image 20260410171201.png]]