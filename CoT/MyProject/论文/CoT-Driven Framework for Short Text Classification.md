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

**Syntactic and Semantic Enrichment CoT（SSE-CoT）** 主要用于提高LLMs在短文本分类任务中的表现。短文本的主要挑战在于**语义稀疏**（semantic sparsity）和**句法模糊**（syntactic ambiguity）。SSE-CoT通过四步推理过程，逐步增强LLMs对短文本的理解，使其分类更精准。

1. **关键概念识别（Key Concept Identification）**
    
    - 目的：识别短文本中的核心概念，为后续步骤提供基础。
    - 过程：
        - 通过CoT推理，让LLMs识别文本中最重要的实体、动词和关键词。
        - 例如，给定文本："Del Potro says make French Open"，模型应识别出 **"Del Potro"（网球运动员）** 和 **"French Open"（网球赛事）**。
    - 输入模板：

        `Given the short text "<文本>", identify key concepts.`
        
    - 形式化表示： $K_1 = f_{\text{identify}}(C_1, I_1)$
    - 其中，$K_1​$ 是识别出的关键概念，$C_1$​ 是上下文，$I_1$​ 是指令。
    
2. **常识知识检索（Common-Sense Knowledge Retrieval）**
    
    - 目的：从LLMs的内在知识库中提取相关常识，以填补短文本的语义缺失。
    - 过程：
        - 让LLMs基于关键概念，检索相关背景知识。
        - 例如，"French Open" 的背景知识包括 "one of the four Grand Slam tournaments held annually in Paris on clay courts"。
    - 输入模板：
        
        `Given the key concepts "<概念>", retrieve related common-sense knowledge.`
        
    - 形式化表示： $S = f_{\text{retrieve}}(C_2, I_2)$

3. **文本重写（Text Rewriting）**
    
    - 目的：基于提取的常识知识，对短文本进行改写，以提升可读性和分类准确性。
    - 过程：
        - 让LLMs利用关键概念和检索的知识，重写短文本，使其语法清晰、信息完整。
        - 例如，"Del Potro says make French Open" 可以被改写为 **"Del Potro, the esteemed professional tennis player, has made statements regarding the French Open, one of the four major Grand Slam tournaments held annually in Paris on its iconic clay courts."**
        
    - 输入模板：
        
        `Refine and enhance the language of "<文本>", ensuring accuracy, fluency, and clarity.`
        
    - 形式化表示： $R = g(C_3, I_3)$

4. **短文本分类（Short Text Classification）**
    
    - 目的：基于重写后的文本进行分类。
    - 过程：
        - 让LLMs根据改写后的文本进行分类，并输出类别。
        - 例如，"Del Potro says make French Open" 经过SSE-CoT后，被正确分类为 **"sport"** 而非误分类为 "world"。
    - 输入模板：
        
        `Given the refined text "<文本>", classify it into one of the categories: [health, sport, entertainment, business, sci_tech, U.S., world].`
        
    - 形式化表示： $y_i = \arg\max p(y | R, I_4)$
## 3. CDMT框架

在 CoT-Driven Multi-Task Learning（CDMT）框架中，训练较小模型时使用了 **三种监督信号**（three distinct supervision signals），即：

1. **SSE-CoT 提供的推理过程（rationales from SSE-CoT）**：
    - 同上

2. **DA-CoT 提供的推理过程（rationales from DA-CoT）**：
    
    - DA-CoT（Domain Augmentation CoT）是一种利用领域知识增强 CoT 推理的策略。
    - 该方法结合了领域相关的外部知识，使得推理更贴合具体任务。
    - 训练小模型时，DA-CoT 产生的推理过程也是一种监督信号，帮助小模型学习更丰富的领域知识。
3. **真实标签（ground truth）**：
    
    - 真实标签是传统监督学习中的主要监督信号，表示数据集提供的正确分类标签。
    - 训练过程中，小模型不仅要拟合真实标签，还要学习 SSE-CoT 和 DA-CoT 的推理逻辑，以提高分类性能。

