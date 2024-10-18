
[LLaMA3+WebUI Docker部署&外网访问，你的大语言模型助手！](https://www.bilibili.com/video/BV1db421Y7sp?vd_source=14fa069dd1a8b00449a35e1427fe06a5)
## 1. 安装

首先安装nvidia驱动和docker,.为了让容器可以使用显卡，需要安装NVIDIA Container Toolkit(NVIDIA Docker), 此处不做赘述。

## 2. 启动 nvidia cuda 容器


```
docker pull nvcr.io/nvidia/pytorch:23.09-py3

docker run -it --gpus all \
	--ipc=host \
	--ulimit memlock=-1 \
	--ulimit stack=67108864 \
	-v /mnt/f/Projects_Mobile/LLM/Finetuning/llama3C7B:/workspace \
	--name Test-container \
	nvcr.io/nvidia/pytorch:23.09-py3
```

下载 llama3C7B

- 退出容器: `exit`
- 重启容器: `docker start -ai Test-container`

```
# 更新源
apt update

 

```