
## 0. 前期准备

- 环境
	- win11
	- gpu: RTX3080

- 安装huggingface-cli 并下载模型
	- huggingface-cli download --resume-download meta-llama/Meta-Llama-3.1-8B --local-dir {想要下载到的目录}
	- huggingface-cli download --resume-download hfl/llama-3-chinese-8b-instruct-v2 --local-dir F:\Data\Models\llama-3-chinese-8b-instruct-v2

- bash: 
	- 退出容器: `exit`
	- 重启容器: 
		- `docker start -ai FTLlama_lico2`
## 1. 安装docker

[【Docker】掌握 Docker魔法：Windows 11 平台上的完美容器部署终极指南_win11安装docker-CSDN博客](https://blog.csdn.net/joeyoj/article/details/136427362)

- BIOS开虚拟化
- 找到 **Resources** > **WSL Integration**，选中 **Enableinteration with my default WSL distro**和**Ubuntu**
- 在Windows中的C:\Users\<your_username>目录下创建一个.wslconfig文件，然后在文件中写入如下内容，然后用`wsl --shutdown`关闭WSL，之后再重启。
```
[experimental]
autoMemoryReclaim=gradual  
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
```

## 2. 拉取镜像

- 推荐镜像：NVIDIA PyTorch 镜像
	- `docker pull nvcr.io/nvidia/pytorch:23.09-py3`
		- cuda12.6; 自带pytorch
	- `docker pull nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04`
		- cuda11.3; 无pytorch
- - 如何查看cuda版本? 进入容器后`nvidia-smi`或者`nvcc -V`

## 3. 启动容器并挂载代码

- `docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it -v /mnt/f/Projects_Mobile/LLM/Finetuning/FT_llama-factory:/workspace --name FTLlama_lico2 nvcr.io/nvidia/pytorch:23.09-py3`
	- `--gpus all`：启用所有可用的 GPU。
	- `-it`：以交互模式运行容器。
	- `--rm`：容器退出后自动删除。(不选)
	- `-v /path/to/your/code:/workspace`：将你的代码目录挂载到容器的 `/workspace` 目录中。
	- `docker run --gpus all -it --rm -v /mnt/f/Projects_Mobile/LLM/Finetuning/llama3C7B:/workspace nvcr.io/nvidia/pytorch:23.09-py3`
	- ulimit表示不限制内存
	- 用FTLlama_lico2命名容器，容器停止后不删除
	- 退出容器: `exit`
	- 重启容器: `docker start -ai FTLlama_lico1`

-  `docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it -v /mnt/f/Projects_Mobile/LLM/Finetuning/llama3C7B_local:/workspace --name FTLlama_local nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04`
	- cuda11.3.1




## 4. LLama-Factory相关

参考[LLama-Factory](LLama-Factory)


## 5. 更新&打包镜像 并上传到超算平台

- **在ubuntu中执行以下命令来保存为一个新镜像**：
	- `docker commit <容器ID或容器名称> <新镜像名称>`
	- `docker commit FTLlama_lico2 ftllama3c7b_fjn:v1`
- **打包镜像**：
	- `docker save -o FTLlama_lico2.tar ftllama3c7b_fjn:v1`
	- 文件位置在以下目录 (win11子系统ubuntu)
		- `\\wsl$\Ubuntu\root`
- **上传到超算平台** : (当前在root目录下)
	- 最好在在workfile文件下单独创建一个run_file文件夹，用于存放环境
	- sudo scp FTLlama_lico2.tar pengyaxin@10.0.28.5:/dssg/home/pengyaxin/fjn/workfile/run_file/

## 6. 打包本地项目文件 并上传到超算平台

- 启动容器 (已经挂载了工作目录)
	- docker start -ai FTLlama_lico2
- 打包项目文件
	- tar -cvf FT_llama-factory.tar .
- 上传 .tar 文件到集群 (已经在workspace下)
	- sudo scp FT_llama-factory.tar pengyaxin@10.0.28.5:/dssg/home/pengyaxin/fjn/workfile/run_file/

上传 .tar 文件到集群
使用 scp 命令将 .tar 文件上传到集群。
sudo scp /media/usv012/zqq/docker/chat-scene.tar pengyaxin@10.0.28.5:/dssg/home/pengyaxin/zqq/workfile/run_file/
这会将文件上传到集群路径 /dssg/home/pengyaxin/zqq/workfile/run_file/。


F:\Projects_Mobile\LLM\Finetuning\FT_llama-factory
## 6. 退出/ 重启容器

bash: 
- 退出容器: `exit`
- 重启容器: 
	- `docker start -ai FTLlama_lico2`
