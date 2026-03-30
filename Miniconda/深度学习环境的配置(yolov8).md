## 介绍

- 可以创造完全隔离的虚拟环境，解决不同项目之间依赖的冲突
- 包管理，安装，卸载，更新软件包，自动解析并安装环境
- 跨平台
## 简单使用

- 安装Anaconda3
- 添加到系统PATH路径

常用的 conda 指令:

- 创建新的 python 环境：`conda create -n env_name python=3.x`
- 查看已有的 python 环境：`conda env list`
- 进入已有的 python 环境：`conda activate env_name`
- 退出当前的 python 环境：`conda deactivate`

常用的pip指令
- 安装包
```
pip install 包名(==版本号)(可指定版本)  (--timeout 6000)(指定更长的超时时间，防止因网络问题下载失败)
```

```
pip install ultralytics 
pip install opencv-python
```
- 更新包
```
pip install --upgrade 包名
```

```
pip install --upgrade ultralytics
```
- 卸载包
```
pip uninstall 包名
```
- 查看已安装的包
```
pip list
```
- 查看某个包的信息/版本
```
pip show 包名
```
- 导出当前环境的依赖，作备份用(默认运行命令所在路径)
```
pip freeze > requirements.txt
```
- 从文件批量安装依赖
```
pip install -r requirements.txt
```
- pip
```
pip --version # 查看版本

python -m pip install --upgrade pip  # 更新pip功能
```
- 本地安装yolo源码包
![[Pasted image 20260330193245.png]]


安装ultralytics
- 运行
```
pip install -e .
```
-e意思是可编辑模式,  **.** 表示是当前目录。其中**pyproject.toml** 是python安装的配置文件。运行命令后会识别并以可编辑形式安装。可以自己进行训练

- 启用默认的检测，可以检测人像汽车等
```
yolo predict model=yolov8n.pt source=0 show=True
```
