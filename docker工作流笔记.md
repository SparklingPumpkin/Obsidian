## 1. 安装docker

[【Docker】掌握 Docker魔法：Windows 11 平台上的完美容器部署终极指南_win11安装docker-CSDN博客](https://blog.csdn.net/joeyoj/article/details/136427362)

BIOS开虚拟化

## 2. 接下来

- 推荐镜像：NVIDIA PyTorch 镜像
	`nvcr.io/nvidia/pytorch`
	如何查看cuda版本?

- **启动容器并挂载代码**
	- `docker run --gpus all -it --rm \   -v /path/to/your/code:/workspace \   nvcr.io/nvidia/pytorch:23.09-py3`
		- `--gpus all`：启用所有可用的 GPU。
		- `-it`：以交互模式运行容器。
		- `--rm`：容器退出后自动删除。
		- `-v /path/to/your/code:/workspace`：将你的代码目录挂载到容器的 `/workspace` 目录中。
		- `docker run --gpus all -it --rm \   -v /path/to/your/code:/workspace \   nvcr.io/nvidia/pytorch:23.09-py3`

