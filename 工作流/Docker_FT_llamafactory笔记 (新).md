
## 0. 前期准备

- 环境
	- Win11
	- GPU: RTX3080
	- Docker + Ubuntu

- 安装huggingface-cli 并下载模型
	- 如何安装huggingface-cli自行百度
	- huggingface-cli download --resume-download meta-llama/Meta-Llama-3.1-8B --local-dir {想要下载到的目录}
	- huggingface-cli download --resume-download hfl/llama-3-chinese-8b-instruct-v2 --local-dir F:\Data\Models\llama-3-chinese-8b-instruct-v2

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

- 退出 & 重启容器
	- 退出容器: `exit`
	- 重启容器: 
		- `docker start -ai FTLlama_lico2`


## 4. LLama-Factory微调

### 4.1. 安装llama-factory

```
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

- 如果出现环境冲突，请尝试使用 `pip install --no-deps -e .` 解决
- 完成安装后，可以通过使用 `llamafactory-cli version` 来快速校验安装是否成功

### 4.2. 数据集准备

[dataset_info.json](https://github.com/hiyouga/LLaMA-Factory/blob/main/data/dataset_info.json/)中包含了所有经过预处理的 **本地数据集** 以及 **在线数据集**。如果您希望使用自定义数据集，请 **务必** 在 `dataset_info.json` 文件中添加对数据集及其内容的定义。

#### 2.1现有CoT数据集 (Alpaca指令监督微调格式): 

```
{
        "instruction": "你是一个物理专家，你能够清晰准确地解决复杂的物理问题。下面将输入一个物理题目，请分步地给出详细的解题过程",

        "input": "一静止质量为m、荷电为e的电子在与均匀磁场垂直的平面内运动，如果忽略辐射，电子的轨道为半径为R的圆。设电子能量E >> mc²，求磁感应强度B的表达式。如果R = 30 m，E = 2.5 x 10¹⁰ eV，求出B的数值。",

        "output": "1. 我们首先考虑洛伦兹力作用下电子做匀速圆周运动的情形。电子所受到的洛伦兹力提供向心力，使得其在半径为R的圆周上运动。\n\n2. 对于一个带电粒子在磁场中做圆周运动，洛伦兹力的表达式为：\nF = e * v * B\n其中，e 是电子电荷量，v 是电子的速度，B 是磁感应强度。\n\n3. 向心力的表达式为：\nF = m * v² / R\n其中，m 是电子的质量，v 是电子的速度，R 是运动轨道的半径。\n\n4. 在圆周运动中，洛伦兹力提供了向心力，所以：\ne * v * B = m * v² / R\n\n5. 简化上式可以得到：\nB = m * v / (e * R)\n\n6. 现在考虑到电子的能量 E >> mc² 的条件，这意味着电子可以被视为高速运动（近似为相对论性运动）。相对论性动能和速度的关系为：\nE = sqrt[(pc)² + (mc²)²]，由于E >> mc²，可以近似为\nE ≈ pc\n\n7. 于是动量 p 可以表示为：\np ≈ E / c\n\n8. 对于高速粒子，动量 p 也可以写为：\np = mv（γ因子接近1时）\n\n9. 将上式代入B的表达式：\nB = m * v / (e * R) = (p * c) / (e * R * c) = E / (e * R * c)\n\n10. 因此磁感应强度 B 的表达式为：\nB = E / (e * R * c)\n\n11. 代入已知数值R = 30 m，E = 2.5 x 10¹⁰ eV，电子电荷量 e = 1.602 x 10⁻¹⁹ C，光速 c = 3 x 10⁸ m/s。\n注意将能量单位从电子伏特转换为焦耳：\n1 eV = 1.602 x 10⁻¹⁹ J\nE = 2.5 x 10¹⁰ eV x 1.602 x 10⁻¹⁹ J/eV = 4.005 x 10⁻⁹ J\n\n12. 代入B的表达式：\nB = 4.005 x 10⁻⁹ J / (1.602 x 10⁻¹⁹ C * 30 m * 3 x 10⁸ m/s)\n\n13. 计算：\nB = 4.005 x 10⁻⁹ / (1.602 x 10⁻¹⁹ * 30 * 3 x 10⁸)\nB = 4.005 x 10⁻⁹ / (1.4418 x 10⁻⁹)\nB ≈ 2.78 T\n\n答案：\nB ≈ 2.78 T"
}
```


### 4.3. SFT 训练

准备
```
docker start -ai FTLlama_lico2
cd LLaMA-Factory
```

调整dataset_info.json
```
"fjn_gpt4o_question&answer_no_fig": {
    "file_name": "fjn_gpt4o_question&answer_no_fig.json"
  },
```


运行微调代码
```
llamafactory-cli train Testfjn/yamls/llama3_lora_sft_fjn.yaml
```





## 5. 更新&打包镜像 并上传到超算平台

- **在ubuntu中执行以下命令来保存为一个新镜像**：
	- `docker commit <容器ID或容器名称> <新镜像名称>`
	- `docker commit FTLlama_lico2 ftllama3c7b_fjn:v1`
- **打包镜像**：
	- `docker save -o FTLlama_lico2.tar ftllama3c7b_fjn:v1`
	- 文件位置在以下目录 (win11子系统ubuntu)
		- `\\wsl$\Ubuntu\root`
- **上传到超算平台** : (当前在root目录下)
	- 应该要在校园网？
	- 最好在在workfile文件下单独创建一个run_file文件夹，用于存放环境
		- `sudo scp FTLlama_lico2.tar pengyaxin@10.0.28.5:/dssg/home/pengyaxin/fjn/workfile/run_file/`
		- 提示输入密码
		- 成功上传


## 6. 打包本地项目文件 并上传到超算平台

- 启动容器 (已经挂载了工作目录)
	- `docker start -ai FTLlama_lico2`
- 打包项目文件
	- `tar -cvf FT_llama-factory.tar .`
- 上传 .tar 文件到集群 (已经在workspace下)
	- `scp FT_llama-factory.tar pengyaxin@10.0.28.5:/dssg/home/pengyaxin/fjn/workfile/`
	- 步骤同前


## 7. 如何在超算平台进微调

### 7.1 终端登录
- 从ubuntu终端登录超算平台
	- `ssh pengyaxin@10.0.28.5`
	- 输入yes
	- 系统会提示输入密码，输入后按回车（注意输入密码时不会有任何回显）
	- 成功登录
- 转换环境
	- `ssh c22` （这个是彭老师的分区，其他分区要换指令）
- 查看GPU使用率
	- `nvidia-smi`

### 7.2 超算平台环境搭建

- 解压上传后的本地项目文件 (不包含镜像文件, web端即可操作)
- 转换 Docker 镜像为 Singularity 镜像
	- `cd fjn/workfile/run_file`
	- `singularity build ftllama3c7b_fjn.sif docker-archive://FTLlama_lico2.tar`

### 7.3 在平台运行项目

- 在run_file文件夹中创建`.sh` 以及 `.slurm` 文件  (建议与环境名保持一致)

- `ftllama3c7b_fjn.sh`文件用于存放运行命令, 示例如下: 
```
#!/usr/bin/env bash

# 设置 Python 路径
export PYTHONPATH=/dssg/home/sjc/workfile/bindfile/test1//lib/python3.6/site-packages:/dssg/home/sjc/workfile/bindfile/test2//lib/python3.6/site-packages（需要单独创建相应空文件夹）

# 指定使用的GPU编号 & 数量
export CUDA_VISIBLE_DEVICES=0,1
NGPUS=2

# 进入工作目录, 目录下应该有训练的主文件 `train.py`
cd /dssg/home/sjc/workfile/pcdet/tools/

# 设置一个标签, 用于标识这次训练的配置或实验
TAG32=nuscenes_baseline_0422_20ep

# 指定用于训练的配置文件路径, 通常包含模型&数据集&训练参数等信息
CFG32=cfgs/nuscenes_models/qy_ch_baseline_n.yaml

# 打印出训练标签, 用于标识当前正在进行的实验
echo "-------"${TAG32}"-------"

# 启动训练
python3 -m torch.distributed.launch --master_port 21007 --nproc_per_node=${NGPUS} train.py --launcher pytorch 2 --cfg_file ${CFG32}  --extra_tag ${TAG32} --**pretrained_model /dssg/home/qinyu/workfile/bevfusion/cbgs_transfusion_lidar.pth** --batch_size 8


# 输出睡眠信息
echo "-------sleep-------"
```

'启动训练' 注释 : 
- **`python3 -m torch.distributed.launch`**：使用 PyTorch 的分布式启动器来启动训练，这样可以在多个 GPU 上并行训练。
- **`--master_port 21007`**：指定主节点的端口，用于进程间通信。
- **`--nproc_per_node=${NGPUS}`**：指定每个节点使用的进程数，这里等于 2。
- **`train.py`**：指定要运行的训练脚本。
- **`--launcher pytorch 2`**：指定使用 PyTorch 的分布式启动器。
- **`--cfg_file ${CFG32}`**：传递训练配置文件路径。
- **`--extra_tag ${TAG32}`**：传递自定义标签，便于标识训练。
- **`--pretrained_model /dssg/home/qinyu/workfile/bevfusion/cbgs_transfusion_lidar.pth`**：指定一个预训练模型文件路径，用于初始化模型参数。
- **`--batch_size 8`**：设置训练时的批处理大小。


