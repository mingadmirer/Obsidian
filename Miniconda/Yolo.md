## 最简命令
```
yolo predict model=yolov8n.pt source=0 show=True
```
- yolo
启动命令
- predict
任务，检测
- model
指定模型
- source
默认参数0，指定电脑摄像头
- show=True
显示窗口

## 常用参数
- conf
置信度，大于conf(百分数)的被检测出
- result.boxs
存储着检测到的信息，包括xywh信息，置信度，类别等
## 数据集的构建
- 视频类
使用opencv将视频存为图片。BGR形式需要先转换为RGB形式
- labelimg
- 环境安装
```
pip install labelimg
```
- 启动命令
```
labelimg
```
选择open dir(打开需要标注的文件夹)，选择将标注文件存到哪里(或change save dir)
![[Pasted image 20260401230530.png]]
选择View确定第一项打开，为自动保存
![[Pasted image 20260401232127.png]]
确定此处设置为yolo模式
右键创建描述框，或按W

标注完产生.txt数据文件(datasets)

## 其他数据集标注工具

[Make Sense](https://www.makesense.ai/)

roboflow公开数据集

https://public.roboflow.com/object-detection

https://universe.roboflow.com/

## 训练
文件结构
- images: 存放图片
  - train：训练集图片
  - val: 验证集图片
- labels：存放标签
  - train: 训练集标签文件，一一对应
  - val: 验证集标签文件，一一对应

整体放在datasets文件里

- 编写配置文件(.yaml文件)
-  例:
![[Pasted image 20260405212502.png]]

```
yolo task=detect mode=train model=./yolov8n.pt data=yoloc-bvn.yaml epochs=300 workers=1 batch=8
```
model = '预训练权重' ， data，训练的文件，epoch = 多少轮 ，batch,每次训练多少图片