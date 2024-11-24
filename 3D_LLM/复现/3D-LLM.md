## 1. 服务器环境搭建

- vscode ssh远程文件夹连接
	- `ssh usv012@58.199.168.55`
	- 输入密码
	- 数据集文件储存在`cd /media/usv012/fjn/data`
	- `cd /media/usv012/fjn/projects/3D-LLM`

```
$ conda create -n lavis python=3.8
$ conda activate lavis

$ git clone https://github.com/salesforce/LAVIS.git SalesForce-LAVIS
$ cd SalesForce-LAVIS
$ pip install -e .

$ pip install positional_encodings

```

- 手动下载了两个模型参数文件
- 运行推理模式
```
git clone https://github.com/UMass-Foundation-Model/3D-LLM.git
cd /media/usv012/fjn/projects/3D-LLM/3D-LLM/3DLLM_BLIP2-base

# 物体模式
python inference.py --mode object --visualize
# 场景模式
python inference.py --mode scene --visualize


```