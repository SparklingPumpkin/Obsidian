
## 0. 前期准备

- 环境
	- win11
	- gpu: RTX3080

- 安装huggingface-cli 并下载模型
	- huggingface-cli download --resume-download meta-llama/Meta-Llama-3.1-8B --local-dir {想要下载到的目录}
	- huggingface-cli download --resume-download hfl/llama-3-chinese-8b-instruct-v2 --local-dir F:\Data\Models\Pre-trained_Models

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


## 4. 安装llama-factory

```
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

- 如果出现环境冲突，请尝试使用 `pip install --no-deps -e .` 解决
- 完成安装后，可以通过使用 `llamafactory-cli version` 来快速校验安装是否成功




## 4. 更新镜像 & 容器内调试和安装依赖

**创建 Dockerfile 进行自定义调整**： 基础镜像往往不完全满足所有需求，你通常需要进一步调整它，添加自定义的代码、安装额外的依赖等。这时就需要编写一个 `Dockerfile` 来描述如何基于基础镜像构建出你所需要的定制镜像。
#### 4.0 项目结构：

/llama3C7B 
│ 
├── Dockerfile 
├── requirements.txt 
├── train.py 
└── data/

#### 4.1 调整环境

进入运行中的容器，手动安装缺失的 Python 包, 并记录.

#### 4.2 编写Dockerfile
```
# 使用 NVIDIA 官方 PyTorch 镜像 cuda12.6

FROM nvcr.io/nvidia/pytorch:23.09-py3

  

# 设置工作目录

WORKDIR /workspace

  

# 将 requirements.txt 文件复制到容器中（用于安装 Python 库）

COPY requirements.txt .

  

# 更新 apt 包列表并安装常用工具

RUN apt-get update && apt-get install -y \

    git \

    curl \

    && rm -rf /var/lib/apt/lists/*

  

# 安装 Python 依赖库

RUN pip install --no-cache-dir --upgrade pip && \

    pip install --no-cache-dir -r requirements.txt

  

# 将代码文件夹挂载到容器中

COPY . .

  

# 设置环境变量以确保 CUDA 和 cuDNN 正常工作

ENV CUDA_HOME=/usr/local/cuda

ENV PATH=/usr/local/cuda/bin:$PATH

ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

  

# 默认运行命令，可以根据需求调整

CMD ["bash"]
```

#### 4.3 编写requiements.txt
```
torch
transformers
datasets
scipy
numpy
pandas
scikit-learn
tqdm
accelerate
peft

```

- 根据Dockerfile构建Docker镜像
	- 在存放 `Dockerfile` 的目录下运行以下命令构建镜像：
	- `docker build -t llama3-C-7B-f-env .`
	- 这会从当前目录（`Dockerfile` 所在的目录）构建镜像，并命名为 `llama3-C-7B-f-env`。



#### 4.4 重新构建镜像

在存放 `Dockerfile` 的目录下运行以下命令构建镜像：
`docker build -t Test-llama3C7B-env .`

#### 4.5 调整环境

1. 用新镜像创建新容器
2. 重复步骤4.1， 直到所有环境配置完成。
#### 4.6 (可选) 导出conda虚拟环境
##### 4.6.1 导出conda环境

在你的 Conda 虚拟环境中，运行以下命令，将环境配置导出为 `environment.yml` 文件：
`conda activate your-env-name`
`conda env export > environment.yml`
##### 4.6.2 创建 Dockerfile
```
# 使用 NVIDIA 的 PyTorch 镜像作为基础镜像
FROM nvcr.io/nvidia/pytorch:23.09-py3

# 设置工作目录
WORKDIR /workspace

# 安装 Conda（如果基础镜像没有内置）
# 如果基础镜像中已包含 Conda，可跳过以下部分
RUN apt-get update && apt-get install -y wget bzip2 && \
    wget https://repo.anaconda.com/archive/Anaconda3-2023.07-Linux-x86_64.sh && \
    bash Anaconda3-2023.07-Linux-x86_64.sh -b && \
    rm Anaconda3-2023.07-Linux-x86_64.sh

# 将 environment.yml 复制到容器中
COPY environment.yml .

# 使用 Conda 安装环境
RUN conda env create -f environment.yml

# 激活 Conda 环境
SHELL ["conda", "run", "-n", "your-env-name", "/bin/bash", "-c"]

# 确保每次容器启动时自动激活 Conda 环境
RUN echo "conda activate your-env-name" >> ~/.bashrc

# 设置默认命令
CMD ["bash"]

```

##### 4.6.3 将 `environment.yml` 文件与 Dockerfile 一起放置
```
/my-llm-docker-project 
├── Dockerfile 
└── environment.yml
```
##### 4.6.4 回到 步骤4.4


### 5. 运行微调代码

进入容器后，你可以在挂载的 `/workspace` 目录中找到你的代码，并开始运行微调任务。假设你有一个 Python 脚本 `train.py`，你可以执行以下命令来进行模型训练：

`cd /workspace python train.py`

### 6. 退出/ 重启容器

bash: 
- 退出容器: `exit`
- 重启容器: 
	- `docker start -ai FTLlama_lico1`
	- `docker start -ai FTLlama_local`
	- `docker start -ai FTLlama_lico2`