## 1. 基本信息

- 发表时间：2025-02-24
- 期刊：KNOWLEDGE-BASED SYSTEM
	- IF：7.2
	- SCI 计算机科学1区

- 关键词：Short Text Classification, Chain-of-thought, Large Language Models
- 贡献
	- 第一个研究CoT再STC上的应用
	- SSE-CoT解决STC的语义稀疏和句法模糊问题
	- CDMT压缩能力
	- 6个短文本分类任务实验，最优性能，小模型也有效
- 概括：
	- 背景：STC对于数字平台内容理解很重要。LLM和CoT增强了复杂任务推理能力，但对于STC等NLP任务仍然有局限性
	- ![[Pasted image 20250313104337.png]]
	- 解决的问题：简短的文本分类（Short Text Classification），**主要挑战**如下
		- **语义稀疏**（semantic sparsity）
		- **句法模糊**（syntactic ambiguity）
	- 使用的技术：CoT。具体而言，本研究使用CoT增强LLM的STC推理能力，提出了句法和语义富集COT（SSE-COT）方法，有效地将STC任务分解为四个不同的步骤：
		- **核心概念识别**（Essential Concept Identification）：提取文本中的关键信息和核心概念。
		- **常识知识检索**（Common-Sense Knowledge Retrieval）：从外部知识库获取相关背景信息，以增强模型理解。
		- **文本重写**（Text Rewriting）：基于提取的概念和常识知识，对原始短文本进行改写，以优化分类任务的输入质量。
		- **分类**（Classification）：基于前述处理后的文本进行最终的短文本分类。
	- 考虑到金融、医疗等领域的**资源受限**问题，我们进一步提出 **CoT 驱动的多任务学习框架**（CoT-Driven Multi-Task Learning, CDMT），以将这些能力扩展到较小的模型中。该框架首先从 LLMs 中提取推理过程（Rationales），然后对较小的模型进行微调，以提升其在 STC 任务中的表现。

	- 实验设计：在六个短文本基准数据集上进行了广泛的实验，验证了所提出方法的有效性。实验结果表明，SSE-CoT 在所有数据集上均取得了最先进（State-of-the-Art, SOTA）的性能，尤其是在 **Ohsumed** 和 **TagMyNews** 数据集上表现突出，实现了显著的性能提升。


## 2. SSE-COT (Semantic and Syntactic Enrichment CoT)

SSE-CoT 主要用于提高LLMs在短文本分类任务中的表现。短文本的主要挑战在于**语义稀疏**（semantic sparsity）和**句法歧义**（syntactic ambiguity）。SSE-CoT通过四步推理过程，逐步增强LLMs对短文本的理解，使其分类更精准。
![[Pasted image 20250315141615.png]]

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



**Domain Augmentation CoT（DA-CoT）** 旨在增强小型模型对特定领域的理解，例如医疗、金融等领域。其核心思想是：

1. 识别短文本中的 **关键实体、动作、事件**（domain-specific entities, actions, events）。
2. 检索领域知识，提供更深入的上下文理解。

## 3. DA-CoT (Domain Augmentation CoT)

![[Pasted image 20250315141643.png]]
1. **关键概念识别**
    
    - 过程：
        - 与SSE-CoT类似，但DA-CoT在识别概念时，强调 **领域特定术语**（如医学、金融术语）。
    - 输入模板：

        `Given the short text "<文本>", identify the key components, considering the main entities, actions, and events described.`
        
    - 形式化表示：$K_2 = f_{\text{identify}}(C_1, I_1)$
    
2. **领域知识检索**
    
    - 过程：
        - 让LLMs基于识别出的关键概念，**补充领域背景知识**。
        - 例如，在医学领域，"Ohsumed" 数据集中的 "17 strokes occur during sleep" 可能需要补充 "Strokes can happen at night due to reduced blood circulation and undiagnosed hypertension."
    - 输入模板：

        `Provide a summary of the identified components, including their interrelations and the overall significance within the context of the text.`
        
    - 形式化表示： $O = f_{\text{enrich}}(C_2, I_2)$

## 3. CoT-Driven Multi-Task Learning (CDMT)

CDMT利用 **多任务学习（Multi-Task Learning, MTL）** 将SSE-CoT和DA-CoT的推理能力迁移到小型模型。具体包括以下三项任务：

1. **真实标签 -- 短文本分类任务（Primary Task）**
    
    - 目标：让小型模型学习基于增强文本的正确分类。
    - 损失函数：$L_{\text{label}} = \frac{1}{N} \sum_{i=1}^{N} \ell(f_s(x'_i), y_i)$
2. **SSE-CoT推理任务（Secondary Task）**
    
    - 目标：让小型模型学习如何生成SSE-CoT的推理过程。
    - 损失函数： $L_{\text{SSE}} = \frac{1}{N} \sum_{i=1}^{N} \ell(f_s(x_i), r_i)$
3. **DA-CoT推理任务（Tertiary Task）**
    
    - 目标：让小型模型学习如何生成DA-CoT的领域推理过程。
    - 损失函数：$L_{\text{DA}} = \frac{1}{N} \sum_{i=1}^{N} \ell(f_s(x_i), o_i)$

最终的损失函数：

$L = L_{\text{label}} + \lambda_1 L_{\text{SSE}} + \lambda_2 L_{\text{DA}}$

-----
- 为什么 SSE-CoT 之后还需要 DA-CoT？

SSE-CoT 和 DA-CoT 侧重点不同，它们**互补**，共同提升短文本分类的效果：

| **方法**  | **主要目标**                     | **主要作用**               | **主要优势**                  |
| ------- | ---------------------------- | ---------------------- | ------------------------- |
| SSE-CoT | **提升 LLMs 在短文本分类的理解能力**      | **语法+语义增强，提供更清晰的分类依据** | **适用于所有短文本，提高 LLM 解析能力**  |
| DA-CoT  | **增强领域知识，提高专业领域 STC 任务的准确性** | **引入专业领域的背景知识，使分类更精准** | **适用于专业领域任务，提高小型模型的泛化能力** |

## 4. 相关工作

- 文本分类（Text Classification）
- STC
- LLM CoT
	- 发现 LLMs 可作为 **隐式知识库（Implicit Knowledge Base）**，在部分 NLP 任务上超越传统方法。
	- 但在 **命名实体识别（NER）**、**关系抽取（Relation Extraction）** 等基本 NLP 任务上，LLMs 仍然无法达到 SOTA（State-of-the-Art）水平。
	- **局限性**
		- CoT 主要用于 **复杂推理任务**，但在短文本分类（STC）中的应用仍然是一个空白领域。
		- 直接使用 LLMs 进行短文本分类成本高，且可能无法有效处理语义稀疏问题。
- 知识蒸馏（Knowledge Distillation, KD）
	- 传统知识蒸馏方法：**GKD** 和 **MINILLM** 主要从教师模型中提取特定的输出，例如分类标签，将直接知识从教师转移到学生模型，简化了复杂的概念，以促使较小模型更容易地学习。
	- CoT 结合知识蒸馏：
		- **MT-CoT** 通过蒸馏 LLMs 的推理过程，使小型模型能够具备 **多任务学习（Multi-Task Learning）** 能力。
		- **Socratic CoT** 进一步优化了蒸馏过程，使小模型更好地理解推理过程，提高在复杂任务中的表现。




