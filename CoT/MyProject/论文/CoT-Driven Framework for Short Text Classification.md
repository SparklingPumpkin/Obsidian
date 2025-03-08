## 1. 基本信息

- 发表时间：2025-02-24
- 期刊：KNOWLEDGE-BASED SYSTEM
	- IF：7.2
	- SCI 计算机科学1区

- 关键词：Short Text Classification, Chain-of-thought, Large Language Models
- 概括：
	- 背景：STC对于数字平台内容理解很重要。LLM和CoT增强了复杂任务推理能力，但对于STC等NLP任务仍然有局限性
	- 解决的问题：简短的文本分类（Short Text Classification）
	- 使用的技术：CoT。具体而言，本研究使用CoT增强LLM的STC推理能力，提出了句法和语义富集COT（SSE-COT）方法，有效地将STC任务分解为四个不同的步骤：
		- **核心概念识别**（Essential Concept Identification）：提取文本中的关键信息和核心概念。
		- **常识知识检索**（Common-Sense Knowledge Retrieval）：从外部知识库获取相关背景信息，以增强模型理解。
		- **文本重写**（Text Rewriting）：基于提取的概念和常识知识，对原始短文本进行改写，以优化分类任务的输入质量。
		- **分类**（Classification）：基于前述处理后的文本进行最终的短文本分类。
	- 此外，考虑到金融、医疗等领域的资源受限问题，我们进一步提出 **CoT 驱动的多任务学习框架**（CoT-Driven Multi-Task Learning, CDMT），以将这些能力扩展到较小的模型中。该框架首先从 LLMs 中提取推理过程（Rationales），然后对较小的模型进行微调，以提升其在 STC 任务中的表现。

	- 实验设计：在六个短文本基准数据集上进行了广泛的实验，验证了所提出方法的有效性。实验结果表明，SSE-CoT 在所有数据集上均取得了最先进（State-of-the-Art, SOTA）的性能，尤其是在 **Ohsumed** 和 **TagMyNews** 数据集上表现突出，实现了显著的性能提升。


## 2. SSE-COT


## 3. CDMT框架



