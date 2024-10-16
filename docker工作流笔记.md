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

## 2. 接下来

- 推荐镜像：NVIDIA PyTorch 镜像
	`nvcr.io/nvidia/pytorch`
	如何查看cuda版本?

- **启动容器并挂载代码**
	- bash代码
	- ```docker run --gpus all -it --rm \   -v /path/to/your/code:/workspace \   nvcr.io/nvidia/pytorch:23.09-py3```
		- `--gpus all`：启用所有可用的 GPU。
		- `-it`：以交互模式运行容器。
		- `--rm`：容器退出后自动删除。
		- `-v /path/to/your/code:/workspace`：将你的代码目录挂载到容器的 `/workspace` 目录中。
		- `docker run --gpus all -it --rm \   -v F:\Projects_Mobile\LLM\Finetuning\llama3C7B:/workspace \   nvcr.io/nvidia/pytorch:23.09-py3`

