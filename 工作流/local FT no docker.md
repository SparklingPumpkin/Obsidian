[手把手教你微调llama3增强中文+微调垂直领域法律大模型](https://www.bilibili.com/video/BV1URsoefE7w?vd_source=14fa069dd1a8b00449a35e1427fe06a5)

## 1. 使用Llama

克隆Llama-Factory
https://github.com/hiyouga/LLaMA-Factory 
```
cd cd F:\Projects_Mobile\LLM\Finetuning\llama3C7B_local
git clone https://github.com/hiyouga/LLaMA-Factory.git
```

确定使用的模型
```

conda create -n llama_factory python=3.11.1
conda activate llama_factory
cd LLaMA-Factory
pip install -e.[metrics,modelscope,qwen]
```






