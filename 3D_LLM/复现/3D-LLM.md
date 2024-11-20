## 1. 服务器环境搭建

- vscode ssh远程文件夹连接
	- `ssh usv012@58.199.168.55`
	- 输入密码
	- 数据集文件储存在`cd /media/usv012/fjn/data`
	- `cd /home/usv012/fjn/Projects/3D-LLM`

```
$ conda create -n lavis python=3.8
$ conda activate lavis

$ git clone https://github.com/salesforce/LAVIS.git SalesForce-LAVIS
$ cd SalesForce-LAVIS
$ pip install -e .

$ pip install positional_encodings

```


