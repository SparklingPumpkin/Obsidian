
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

- 退出容器: `exit`
- 重启容器: `docker start Test-container`
- 重启容器并交互: `docker start -ai Test-container`

```
# 更新源
apt update

# 安装一些包
apt install wget tmux
```

## 3. llamafile下载llama

- 网址：
	- [Mozilla-Ocho/llamafile: Distribute and run LLMs with a single file. (github.com)](https://github.com/Mozilla-Ocho/llamafile)
- copy link：
	- https://huggingface.co/Mozilla/Meta-Llama-3-8B-Instruct-llamafile/resolve/main/Meta-Llama-3-8B-Instruct.Q5_K_M.llamafile?download=true  # 删去后缀

下载llamafile并给予可执行权限
```
wget https://huggingface.co/Mozilla/Meta-Llama-3-8B-Instruct-llamafile/resolve/main/Meta-Llama-3-8B-Instruct.Q5_K_M.llamafile

chmod +x ./Meta-Llama-3-8B-Instruct.Q5_K_M.llamafile
```

退出重启容器&启动bash进程:
```
exit

docker start Test-container

docker exec -it Test-container bash
```

在进程里启动tmux后台任务 (这样的话退出bash (容器) 后台任务还可以继续进行)
```
tmux

./Meta-Llama-3-8B-Instruct.Q5_K_M.llamafile gpu nvidia -ngl 999 --port 8080
```