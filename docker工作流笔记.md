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
	- `nvcr.io/nvidia/pytorch`
	- 如何查看cuda版本? 进入容器后`nvidia-smi`或者`nvcc --version`
	- 拉取容器 `docker pull nvcr.io/nvidia/pytorch:23.09-py3`

## 3. 挂载目录

- **启动容器并挂载代码**
	- bash代码
	- ```docker run --gpus all -it --rm -v /path/to/your/code:/workspace nvcr.io/nvidia/pytorch:23.09-py3```
		- `--gpus all`：启用所有可用的 GPU。
		- `-it`：以交互模式运行容器。
		- `--rm`：容器退出后自动删除。
		- `-v /path/to/your/code:/workspace`：将你的代码目录挂载到容器的 `/workspace` 目录中。
		- `docker run --gpus all -it --rm -v /mnt/f/Projects_Mobile/LLM/Finetuning/llama3C7B:/workspace nvcr.io/nvidia/pytorch:23.09-py3`
	- 或者使用`docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it -v /mnt/f/Projects_Mobile/LLM/Finetuning/llama3C7B:/workspace --name Test-container nvcr.io/nvidia/pytorch:23.09-py3
`
		- 表示不限制内存
		- 用Test-container命名容器，容器停止后不删除
		- 重启容器：`docker start -ai Test-container`

### 4. 容器内调试和安装依赖

**创建 Dockerfile 进行自定义调整**： 基础镜像往往不完全满足所有需求，你通常需要进一步调整它，添加自定义的代码、安装额外的依赖等。这时就需要编写一个 `Dockerfile` 来描述如何基于基础镜像构建出你所需要的定制镜像。

如果需要安装额外的依赖，可以在容器中使用 `pip` 安装所需库。例如：
`pip install transformers datasets`

### 5. 运行微调代码

进入容器后，你可以在挂载的 `/workspace` 目录中找到你的代码，并开始运行微调任务。假设你有一个 Python 脚本 `train.py`，你可以执行以下命令来进行模型训练：

`cd /workspace python train.py`

### 6. 退出/ 重启容器

bash: 
- 退出容器: `exit`
- 重启容器: `docker start -ai Test-container`