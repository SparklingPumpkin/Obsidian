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

- 手动从huggingface下载了两个模型参数文件（具体文件：推理程序报错的时候会显示要下载的路径）

## 2. 推理

- 运行推理模式
```
git clone https://github.com/UMass-Foundation-Model/3D-LLM.git
cd /media/usv012/fjn/projects/3D-LLM/3D-LLM/3DLLM_BLIP2-base

# 物体模式
python inference.py --mode object --visualize
# 场景模式
python inference.py --mode scene --visualize


```

## 3. 训练

任务类型（数据集类型）
- 3dmv_vqa
- ScanQA_v1.0
	- 简单介绍：ScanQA 是一个基于 3D 场景的问答任务，要求在 3D 点云中找到与自然语言问题相关的答案。
	- 输入：
		- 3D场景（通常点云）
		- 自然语言指令（如“Where is the sofa located?”）
	- 输出：
		- 自然语言答案
- SQA3D







