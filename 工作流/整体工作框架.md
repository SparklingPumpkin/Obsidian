
## 1. 获取数据集

可选方案:
1. 利用现有数据集 (物理QACoT-GPT4o)
	1. 校验数据集准确性
	2. 根据需求更改数据集的CoT结构
2. 利用语料库构建数据集 (目前语料库为物理20G/40G,有md版本)
	1. 暂时没完整思路, 以下为零散思路
	2. RAG获取答案
	3. 网络搜寻数学物理问题
3. 现有数据集改进
	1. 从论文中寻找合适的数学物理问题数据集
	2. 对这些数据集进行CoT构建
	3. 校验数据集准确性, 并删去有错CoT

## 2. 寻找更合适的CoT方案

1. 现有的CoT架构可能对当前领域问题
	1. 不合适
	2. 效果不好
2. 对CoT进行改进 (idea1)
3. 在LLM (暂拟GPT-4o) 上进行实验, 需要得到比现有CoT方案更好的结果 (Accuracy_LLM_1)  

## 3. Model Compression

1. LLM Distilling
	1. SLM (暂拟llama3.1-8B) Fine-tuning (idea2)
		1. Lora Fine-tuning
		2. Prefix Fine-tuning
		3. Adapter Fine-tuning
		4. 创新 Fine-tuning (idea3)
	2. SLM 量化

## 4. 最终结果

最后得到: 
1. 一个新的CoT方案
2. 一个效果良好的 微调 SLM
3. (可能) 一种新的微调方案




