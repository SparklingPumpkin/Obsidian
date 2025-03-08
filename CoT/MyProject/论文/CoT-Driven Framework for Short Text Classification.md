## 1. 基本信息

- 发表时间：2025-02-24
- 期刊：KNOWLEDGE-BASED SYSTEM
	- IF：7.2
	- SCI 计算机科学1区

- 关键词：Short Text Classification, Chain-of-thought, Large Language Models
- 解决的问题：简短的文本分类（Short Text Classification）
- 使用的技术：CoT
- 实验设计

- 概括：
	- STC对于数字平台内容理解很重要。
	- LLM和CoT增强了复杂任务推理能力，但对于STC等NLP任务仍然有局限性
	- 本研究使用CoT增强LLM的STC推理能力
		- 提出了句法和语义富集COT（SSE-COT）方法，有效地将STC任务分解为四个不同的步骤：（i）基本概念识别，（ii）常识性知识检索，（iii）文本重写和（iv）分类。
		- 此外，识别金融和医疗保健等部门的资源限制，然后我们引入了COT驱动的多任务学习（CDMT）框架，以将这些功能扩展到较小的模型。