
## 拉取镜像

尽量选择自带cuda的镜像如：1.11.1-cuda11.3-cudnn8-devel

1，卸载旧版本docker

sudo apt-get remove docker docker-engine docker.io containerd runc

2，设置Docker的存储库并从中安装

更新包索引并安装包以允许通过 HTTPS 使用存储库：apt apt

sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

3，添加泊坞的官方 GPG 密钥

 sudo mkdir -p /etc/apt/keyrings

 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

4，使用以下命令设置存储库：

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

5，[安装docker](https://so.csdn.net/so/search?q=安装docker&spm=1001.2101.3001.7020)引擎
      更新包索引，并安装最新版本的 Docker 引擎、容器化和 Docker 撰写

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin



**####使用国内源安装docker**

sudo apt-get update

sudo nano /etc/apt/sources.list.d/docker.list

curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-get update 

sudo apt-get install docker-ce docker-ce-cli containerd.io



6，通过运行映像来验证是否正确安装了 Docker 引擎。hello-world

sudo service docker start 

sudo docker run hello-world

7，换Docker国内镜像源

sudo vim /etc/docker/daemon.json

添加 文件内容

{
  "registry-mirrors" : [
    "http://registry.docker-cn.com",
    "http://docker.mirrors.ustc.edu.cn","https://alzgoonw.mirror.aliyuncs.com"，
    "http://hub-mirror.c.163.com"
  ],
  "insecure-registries" : [
    "registry.docker-cn.com",
    "docker.mirrors.ustc.edu.cn"
  ],
  "debug" : true,
  "experimental" : true
}
8，重启docker

sudo systemctl daemon-reload

sudo gpasswd -a $USER docker

newgrp docker

docker ps

9，验证是否正确安装Docker

sudo service docker start 

sudo docker run hello-world

出现Hello from Docker！表示成功

sudo docker version

sudo docker images

10，拉取镜像

docker pull  pytorch/pytorch:1.9.1-cuda11.1-cudnn8-devel 

docker pull pytorch/pytorch:2.3.1-cuda12.1-cudnn8-devel

docker pull nvcr.io/nvidia/cuda:11.8.0-devel-ubuntu22.04

docker pull nvcr.io/nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04





使用dockerfile文件制作命令

```
docker build -t mmdetection3d docker/
```



## 制作容器

1，在本地创建容器（并赋予共享空间15G）###注意创建容器的同时需要将使用的框架映射到workspace下，使用的是-v命令

```
docker run -itd --gpus all -it --name=fy(名字)   --shm-size 15g   -v /home/fy/workspace（本地需要映射文件名字）/:/workspace/file -v /home/nuscenes(数据集)/:/workspace pytorch/pytorch:1.9.1-cuda11.1-cudnn8-devel  /bin/bash
```

2，进入容器

```
docker exec -it fy（名字）/bin/bash
ln -s /workspace/nuscenes /workspace/bevfusion/data/nuscenes
```

###注意事项：查看容器内的文件目录、传输文件等操作只能在容器外操作

###软连接命令 ln -s

## 安装环境

----步骤同本地安装一样

1，创建环境   ####由于没有安装anaconda，这一步添加了

```
#####apt-get update

#####apt-get install -y wget bzip2

#####wget https://repo.anaconda.com/archive/Anaconda3-2023.03-Linux-x86_64.sh -O anaconda.sh

#####bash anaconda.sh -b -p /opt/anaconda  \# 运行安装脚本

#####rm anaconda.sh \# 删除安装脚本

######\# export PATH=/opt/anaconda/bin:$PATH  #添加 Anaconda 到 PATH 

#####echo 'export PATH=/opt/anaconda/bin:$PATH' >> ~/.bashrc #为了使这个设置在每次启动 shell 时都生效，添加到 .bashrc 

####### source ~/.bashrc重新加载 .bashrc 

conda create -n bevfusion python==3.9 -y
```

2，激活环境

```
conda activate bevfusion
or
source activate bevfusion
```

3，安装pytorch，本文档安装的是cuda11.1对应的版本。###注意根据自己的cuda版本安装

pip install torch==1.10.1+cu111 torchvision==0.11.2+cu111 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu111/torch_stable.html

cuda11.3安装：

```
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch -y
```

cuda11.8安装用：pip install torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu118

cuda12.1安装用：pip install torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu121  或者  

```
conda install pytorch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 pytorch-cuda=12.1 -c pytorch -c nvidia
or
conda install pytorch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 pytorch-cuda=11.8 -c pytorch -c nvidia
```



4，验证是否成功安装

```
(pcdet) ypx@ypx:~/ws/OpenPCDet$ python
Python 3.7.16 (default, Mar 29 2022, 02:18:16) 
[GCC 7.5.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> import torchvision
>>> print(torch.__version__)
1.9.0
>>> print(torchvision.__version__)
0.9.0
>>> print(torch.cuda.is_available())
True
>>> print(torch.cuda.device_count())
4
>>> print(torch.version.cuda)
11.8
>>> exit()
```

5，安装spconv，本文档以cuda11.1为基础，可直接安装spconv2.x，cuda10.2以下的版本需手动安装spconv1.x。

pip install spconv-cu111(cuda11.1版本安装) 

#cuda11.8： 

```
pip install spconv-cu118  
```

 #cuda11.3： 

```
pip install spconv-cu113  
```

 

#cuda12.2： 

```
pip install spconv-cu120
```

6，放入OpenPCDet文件，可采用文件映射到容器内的方式。本文采用cp方式将文件夹直接放入workspace下。**####此步骤如果在创建容器时已经映射好则可以跳过**

docker cp path/OpenPCDet（文件原路径） 88205310f09b（容器id）:/workspace（放入容器的路径）####此步骤需在容器外执行即先退出容器在操作

7，编译OpenPCDet

```
cd OpenPCDet
pip install -r requirements.txt 
python setup.py develop 
```

8，验证OpenPCDet是否安装成功

```
python 
>>> import pcdet
>>> exit()
```

9，修改文件 OpenPCDet/tools/scripts/dist_train.sh    ###修改文件权限 sudo chmod 777 filename 或者 sudo chown -R username filename

 将python -m torch.distributed.launch --nproc_per_node=${NGPUS} --rdzv_endpoint=localhost:${PORT} train.py --launcher pytorch ${PY_ARGS}  改为  python -m torch.distributed.launch --nproc_per_node=${NGPUS} --launcher pytorch ${PY_ARGS}

10，修改OpenPCDet/pcdet/datasets/init.py

注释不需要的数据集

![image-20240512113019888](C:\Users\QinYu\AppData\Roaming\Typora\typora-user-images\image-20240512113019888.png)

11，多卡训练
bash scripts/dist_train.sh 2（GPU数量） --cfg_file cfgs/nuscenes_models/bevfusion.yaml --pretrained_model /home/shu-usv001/qinyu/BEVfusion/tools/cbgs_transfusion_lidar.pth（预训练权重路径） --epochs 3 --batch_size 4 --extra_tag bevtrain 

```
CUDA_VISIBLE_DEVICES=0,1 bash scripts/dist_train.sh 2 --cfg_file cfgs/nuscenes_models/bevfusion.yaml --pretrained_model cbgs_transfusion_lidar.pth --epochs 1 --batch_size 4 --extra_tag bevtrain
```

CUDA_VISIBLE_DEVICES=0,1 --nproc_per_node=2 train.py
--launcher
pytorch
--cfg_file
./cfgs/nuscenes_models/qy_ch_baseline_n.yaml
--extra_tag
test1_baseline_nuscenes_0421_1ep
--batch_size
2
--epochs
1

12补装环境

报错：ImportError: libGL.so.1: cannot open shared object file: No such file or directory

```
apt-get update

apt-get install -y libgl1
```

报错：ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory

```
apt-get update

apt-get install -y libglib2.0-0
```

报错：ModuleNotFoundError: No module named 'av2'

```
pip install av2
```

报错：ModuleNotFoundError: No module named 'kornia'

```
pip install kornia
```

报错：ModuleNotFoundError: No module named 'cv2'

```
pip install opencv-python
```

验证报错：ModuleNotFoundError: No module named 'nuscenes'

```
pip install nuscenes-devkit
```

报错：AttributeError: module 'numpy' has no attribute 'int'.

```
pip uninstall numpy -y

pip install numpy==1.23.1
```

ModuleNotFoundError: No module named 'tensorboardX'

```
pip install tensorboardX
```

ModuleNotFoundError: No module named 'SharedArray'

```
pip install SharedArray
```

ModuleNotFoundError: No module named ‘pyquaternion’

```
pip install pyquaternion
```

13修改local_rank

修改trian.py  **cuda11.8及以上需要修改**，**11.8以下不用修改**

parser.add_argument('**--local_rank**', type=int, default=None, help='local rank for distributed training')

改为parser.add_argument('**--local-rank**', type=int, default=None, help='local rank for distributed training')



##  上传集群

1，打包镜像

```
将容器打包成镜像  docker commit container_name new_image_name:tag
sudo docker save -o /home/shu-usv001/qinyu/ai_bevfusion.tar(文件路径/文件名) xxxx(镜像id)
```

2，上传镜像到集群（最好在在workfile文件下单独创建一个run_file文件夹，用于存放环境，.sh/.slurm）

```
scp -r /home/shu-usv001/qinyu/ai_bevfusion.tar(本地路径/文件名) qinyu@10.0.28.59(用户名@ip):/dssg/home/qinyu/workfile/run_file/(集群文件路径)
```

3，转sing镜像

ssh mgt02（转换环境）

```
singularity build bevfusion.sif(自定义集群环境名) docker-archive://ai_bevfusion.tar
```

4，创建.sh/.slurm文件

在run_file文件夹中创建.sh/.slurm。建议与环境名保持一致。

bevfusion.sh用于存放运行命令（**加粗位置需要根据个人文件位置进行修改**）

#!/usr/bin/env bash

export PYTHONPATH=/dssg/home/sjc/workfile/bindfile/test1//lib/python3.6/site-packages:/dssg/home/sjc/workfile/bindfile/test2//lib/python3.6/site-packages（需要单独创建相应空文件夹）

export CUDA_VISIBLE_DEVICES=0,1
NGPUS=2

**cd /dssg/home/sjc/workfile/pcdet/tools/**(用于找到运行文件train.py)

TAG32=nuscenes_baseline_0422_20ep
CFG32=cfgs/nuscenes_models/qy_ch_baseline_n.yaml
echo "-------"${TAG32}"-------"
python3 -m torch.distributed.launch --master_port 21007 --nproc_per_node=${NGPUS} train.py --launcher pytorch 2 --cfg_file ${CFG32}  --extra_tag ${TAG32} --**pretrained_model /dssg/home/qinyu/workfile/bevfusion/cbgs_transfusion_lidar.pth** --batch_size 8
echo "-------sleep-------"

![image-20240531155918006](C:\Users\QinYu\AppData\Roaming\Typora\typora-user-images\image-20240531155918006.png)

bevfusion.slurm用于提交任务（**加粗位置需要根据个人文件位置进行修改**）

#!/bin/bash

#SBATCH --job-name=**bevfusion**
#SBATCH --partition=GPU
#SBATCH -N 1                  
#SBATCH --ntasks-per-node=32 
#SBATCH --output=**/dssg/home/qinyu/workfile/run_file/log**/%j.out
#SBATCH --error=**/dssg/home/qinyu/workfile/run_file/log**/%j.err
#SBATCH --gres=gpu:2

module --ignore-cache load "singularity"
singularity run --nv **bevfusion.sif** bash **bevfusion.sh**



补装环境：

1，ssh mgt02

2，cd /dssg/home/qinyu/workfile/run_file/(进入含有run app_sing_img.sif的文件夹)

3，singularity run app_sing_img.sif 进入沙盒模式,

4，source activate bevfusion（激活环境）

5，pip install xxxx.whl

补充nuscenes工具包





涉及的相关命令

软连接：sudo ln -s /源文件路径 /目标文件路径 （示例 sudo ln -s /media/usv001/71519744-1d2c-4864-bf81-aec1d3aa48a4/nuscenes_src/nuscenes /home/usv001/qinyu/BEVfusion/data/nuscenes）

删除文件夹：rm -rf /文件路径

报错packaging.version.InvalidVersion: Invalid version: '0.6.0+'+'   

apt update

apt install git

pip install --upgrade pip setuptools

git config --global --add safe.directory /workspace/bevfusion  ###添加一个安全目录
