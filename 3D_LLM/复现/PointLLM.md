## 1. docker 环境配置

- 环境：
	- Ubuntu 20.04
	- NVIDIA Driver: 515.65.01
	- CUDA 11.7
	- Python 3.10.13
	- PyTorch 2.0.1
	- Transformers 4.28.0.dev(transformers.git@cae78c46)

- docker：
	- 拉取镜像: `docker pull nvcr.io/nvidia/pytorch:22.08-py3`
	- 创建容器: `docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it -v /mnt/f/Projects_Mobile/3DLLM/pointLLM:/workspace --name pointLLM nvcr.io/nvidia/pytorch:22.08-py3`
	- 初始化conda: `conda init`
	- 重启容器: `docker start -ai pointLLM`
	- `conda activate`
	- `pip show torch`
	- `pip install torch==2.0.1`
	- `pip install --upgrade torchvision torchtext torch-tensorrt --index-url https://download.pytorch.org/whl/cu117`
	- 


## 2. 服务器环境搭建

- vscode ssh远程文件夹连接
	- `ssh usv012@58.199.168.55`
	- 输入密码
	- `cd /home/zqq/fjn`
	- 






