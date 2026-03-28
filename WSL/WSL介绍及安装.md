[超详细的WSL教程：Windows上的Linux子系统_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1tW42197za/?spm_id_from=333.337.search-card.all.click&vd_source=c254c8f1269981c1751209d83de7bd01)

## 介绍
- 以下来自AI**~~复制~~借鉴**

**WSL（Windows Subsystem for Linux，适用于 Linux 的 Windows 子系统）** 是微软为 Windows 10/11 开发的一项**原生功能**，允许你在 Windows 系统中直接运行 Linux 环境（包括命令行工具、应用程序甚至图形界面软件），**无需传统虚拟机、无需双系统重启**。

### 核心优势（为什么用 WSL？）

- **轻量高效**：资源占用远低于 VMware/VirtualBox，启动秒开，动态分配内存。
- **无缝互通**
    
    - Linux 访问 Windows 文件：`/mnt/c/Users/你的用户名/`
    - Windows 访问 Linux 文件：`\\wsl$\发行版名称\`
    - 共用网络、剪贴板，可互相调用对方程序。
    
- **开发友好**：直接使用 Linux 工具链（`git`/`gcc`/`make`/`docker`），与 VS Code 深度集成Microsoft Learn。
- **多发行版**：支持 **Ubuntu、Debian、Kali、Fedora、OpenSUSE** 等Microsoft Learn。
- 现在一般使用**WSL2**
## 安装前
- 确定CPU已开启虚拟化
![[Pasted image 20260325201852.png]]
默认开启。如若关闭可在bios里修改
- 搜索框里搜索**启用或关闭Windows功能**
![[Pasted image 20260325202259.png]]
## 安装与卸载
- 安装**ubuntu22.04**系统
cmd中输入
```
wsl --install Ubuntu-22.04 --web-download
```

输入账号密码，重启电脑。
安装其他版本
```
wsl --list --online
```
可查看可安装的系统列表
![[Pasted image 20260325203417.png]]
安装其他系统。例：
```
wsl --install kali-linux --web-download
```
在终端中启动
```
wsl -d Ubuntu-22.04
```
或在此处启动
![[Pasted image 20260325210925.png]]
关闭可以"X"掉或输入**exit**关闭

启动安装的所有的子系统的指定一个。默认启动使用wsl命令即可。

- 修改默认。如设置ubuntu22.04为默认
```
wsl --set-default Ubuntu-22.04
```
- 误安装进行卸载
以管理员身份打开PowerShell
```
wsl --shutdown # 暂停所有wsl进程
```
```
wsl --unregister Ubuntu-22.04 # 卸载指定版本
```
## 在VS Code上进行WSL的进一步配置

### 安装vscode
- 略。官网下载即可
[Visual Studio Code - The open source AI code editor](https://code.visualstudio.com/)
### vscode里进行配置
- 打开插件市场，搜索wsl，安装
![[Pasted image 20260326152559.png]]


- 在左侧新增的wsl选项中选择已安装的wsl子系统，右键与当前窗口中打开
![[Pasted image 20260326152959.png]]
- Ctrl + Shift + ` 启动VS Code里终端
### 配置Ubuntu的国内源

- 此举有助于加快软件包的下载安装速度

这里设置为阿里云的国内镜像源
[Ubuntu镜像-Ubuntu镜像下载安装-开源镜像站-阿里云](https://developer.aliyun.com/mirror/ubuntu)

- 打开终端,执行
```
sudo nano /etc/apt/sources.list
```
- 清空原内容，复制粘贴相应的版本

![[Pasted image 20260326160614.png]]
- Ctrl + O 保存
- 回车
- Ctrl + X 退出