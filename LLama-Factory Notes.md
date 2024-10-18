
[数据处理 - LLaMA Factory](https://llamafactory.readthedocs.io/zh-cn/latest/getting_started/data_preparation.html)
## 1. 安装llama-factory

```
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

- 如果出现环境冲突，请尝试使用 `pip install --no-deps -e .` 解决
- 完成安装后，可以通过使用 `llamafactory-cli version` 来快速校验安装是否成功


## 2. 数据集准备

[dataset_info.json](https://github.com/hiyouga/LLaMA-Factory/blob/main/data/dataset_info.json/)中包含了所有经过预处理的 **本地数据集** 以及 **在线数据集**。如果您希望使用自定义数据集，请 **务必** 在 `dataset_info.json` 文件中添加对数据集及其内容的定义。







