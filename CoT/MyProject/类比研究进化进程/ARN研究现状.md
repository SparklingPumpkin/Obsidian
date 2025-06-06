### 关键要点

- 研究表明，ARN和StoryAnalogy等研究为叙述性类比推理提供了重要框架。
- 证据显示，这些研究帮助评估和改进大型语言模型（LLMs）在叙述类比中的表现。
- 似乎可能，这些工作推动了AI在复杂叙述任务中的应用，但仍存在争议，如LLMs在远类比中的表现较差。

---


叙述性类比推理是AI研究中的一个新兴领域，特别是在评估大型语言模型（LLMs）如何处理叙述中的类比任务时。以下是关于近期相关研究的简要概述，包括ARN、StoryAnalogy等。

#### ARN研究

ARN（Analogical Reasoning on Narratives）是2024年的一项研究，提出了一种计算框架和基准，用于评估LLMs在叙述类比中的表现。它发现LLMs在远类比（跨领域、无表面相似性）任务上表现不佳，人类准确率达96%，而LLMs仅为57%。

#### StoryAnalogy研究

StoryAnalogy是2023年的一项研究，构建了一个包含24K叙述对的大型语料库，评估LLMs在故事级类比识别和生成中的能力。结果显示，LLMs的准确率仅约30%，远低于人类的85%以上。

#### 其他相关研究

ANALOGICAL（2023年）和AnaloBench（2024年）是更广泛的类比基准，虽然不专门针对叙述，但为理解LLMs的类比能力提供了背景。

---

---

### 调查笔记：叙述性类比推理的近期研究

本文详细探讨了近几年与“叙述性类比推理”相关的最新研究，特别是ARN、StoryAnalogy等，旨在为AI领域提供全面的背景信息。这些研究主要聚焦于开发计算框架和基准，以评估和提升大型语言模型（LLMs）在叙述类比任务中的表现。

#### 研究背景与方法

叙述性类比推理是认知科学和自然语言处理（NLP）交叉领域的研究热点，旨在模拟人类通过叙述进行类比推理的能力。近期研究通过构建专门的基准和数据集，探索LLMs在处理叙述级类比时的表现，特别是在远类比（cross-domain analogies）中的挑战。

#### 具体研究分析

1. **ARN: Analogical Reasoning on Narratives (Sourati et al., 2024)**
    - **研究概述**：该研究发表于2024年，提出了ARN（Analogical Reasoning on Narratives）框架和基准，旨在将认知理论与NLP结合，扩展传统的词语类比到叙述级的系统类比。框架包括三个步骤：提取叙述元素（如角色、动作、谚语）、建立表面和系统映射、推断类比或非类比。基准包括1,100个三元组（查询叙述、类比叙述和干扰叙述），分为近/远类比和近/远非类比四类，基于ePiC数据集。
    - **关键贡献**：
        - 提供了一个理论基础的计算框架，操作化了认知和叙述学理论。
        - 开发了ARN基准，涵盖不同语义距离的叙述对，强调系统映射而非表面相似性。
        - 全面评估了多种SOTA LLMs（如GPT3.5、GPT4.0、UnifiedQA、Llama-2等）在零样本和少样本设置下的表现。
    - **主要发现**：
        - LLMs与人类的表现差距显著，尤其在远类比任务上。人类平均准确率为96%，而LLMs在零样本设置下的平均准确率为57%，远低于人类。
        - 在远类比任务中（如远/近分区），最佳模型GPT4.0的准确率仅29.1%，甚至低于随机猜测（50%）。
        - 少样本提示和链式推理（Chain-of-Thought, CoT）可提升远类比表现（GPT模型提升高达30个百分点），但可能降低近类比的准确率。
        - 表面映射（如基于动作的相似性）对大多数模型具有高度干扰，特别是在远类比任务中。
    - **意义**：ARN基准揭示了当前LLMs在叙述级类比推理中的局限性，特别是跨领域任务，强调了未来研究需要开发更接近人类认知的模型。
    - **引用**：[ARN: Analogical Reasoning on Narratives](https://arxiv.org/abs/2310.00996)
2. **StoryAnalogy: Deriving Story-level Analogies from Large Language Models to Unlock Analogical Understanding (Jiayang et al., 2023)**
    - **研究概述**：该研究发表于2023年，构建了StoryAnalogy语料库，包含24K个故事对，基于扩展的结构映射理论（extended Structure-Mapping Theory）进行人类注释，评估LLMs在故事级类比识别和生成中的能力。
    - **关键贡献**：
        - 首次提出一个大规模故事级类比语料库，涵盖不同领域的叙述对，注释了实体和关系相似性。
        - 设计了一组测试，评估LLMs在类比识别和生成任务中的表现。
    - **主要发现**：
        - 当前LLMs（如ChatGPT和LLaMa）在故事级类比任务中的表现极差，选择题准确率仅约30%，而人类准确率超过85%。
        - 通过StoryAnalogy数据微调LLMs可提升类比生成质量，微调后的FlanT5-xxl模型表现可与零样本ChatGPT相当。
    - **意义**：强调了LLMs在故事级类比任务中的挑战，提供了改进方向，如通过数据增强提升生成能力。
    - **引用**：[StoryAnalogy: Deriving Story-level Analogies from Large Language Models](https://arxiv.org/abs/2310.12874)
3. **ANALOGICAL: A Novel Benchmark for Long Text Analogy Evaluation in Large Language Models (Wijesiriwardene et al., 2023)**
    - **研究概述**：该研究发表于2023年，提出了ANALOGICAL基准，评估LLMs在长文本类比中的表现，涵盖六种复杂性级别：词语、词句对比、句法、否定、蕴含和隐喻。虽然不专门针对叙述，但为类比能力提供了更广泛的评估。
    - **关键贡献**：
        - 构建了一个涵盖13个数据集的基准，使用三种距离度量评估LLMs识别类比对的能力。
        - 提出了类比复杂性分类，从词级到隐喻级，系统化地评估LLMs的推理能力。
    - **主要发现**：
        - LLMs在高复杂性类比（如否定、隐喻）任务中表现不佳，表明其在处理复杂关系推理时的局限性。
    - **意义**：虽然不专注于叙述，但为理解LLMs的类比能力提供了背景，间接支持叙述性类比研究。
    - **引用**：[ANALOGICAL: A Novel Benchmark for Long Text Analogy Evaluation](https://arxiv.org/abs/2305.05050)
4. **AnaloBench: Benchmarking the Identification of Abstract and Long-context Analogies (Ye et al., 2024)**
    - **研究概述**：该研究发表于2024年，提出了AnaloBench基准，测试LLMs在抽象和长上下文类比中的表现，重点是回忆相关经验和应用类比推理到复杂场景。
    - **关键贡献**：
        - 设计了一个基准，评估LLMs在长上下文和抽象类比中的能力，测试了多种专有和开源模型（如GPT系列、Claude V2、LLaMA2）。
    - **主要发现**：
        - 扩展LLMs规模可带来一定性能提升，但对长场景类比或从大量信息中回忆相关场景的收益有限。
    - **意义**：虽然不专门针对叙述，但其对长上下文类比的评估为叙述性类比研究提供了参考。
    - **引用**：[AnaloBench: Benchmarking the Identification of Abstract and Long-context Analogies](https://arxiv.org/abs/2402.12370)

#### 性能对比与分析

以下表格总结了人类与LLMs在ARN和StoryAnalogy基准中的表现，突出LLMs在远类比任务中的挑战：

|**基准/任务**|**人类准确率**|**LLMs平均准确率**|**备注**|
|---|---|---|---|
|ARN（远类比，零样本）|96%|57%|GPT4.0最佳，68.1%平均|
|StoryAnalogy（识别任务）|>85%|~30%|ChatGPT表现较差，微调后提升|

该表格显示，LLMs在叙述性类比任务中与人类的差距显著，特别是在远类比和故事级任务中，强调了当前模型的局限性。

#### 相关研究背景

此外，搜索结果中还包括其他相关研究，如神经科学视角的类比推理发展（2017年）和政策制定中的类比推理（2019年），但这些研究较早或不直接针对叙述性类比。2025年的研究（如叙述参与度）虽近期，但更关注叙述参与而非类比推理。

#### 结论

ARN（2024年）和StoryAnalogy（2023年）是叙述性类比推理领域的主要近期研究，揭示了LLMs在故事级和远类比任务中的挑战，并提供了改进方向。ANALOGICAL（2023年）和AnaloBench（2024年）提供了更广泛的类比评估背景，间接支持叙述性类比研究。这些工作共同推动了AI在复杂叙述任务中的应用，但LLMs与人类表现的差距仍需进一步研究。

---

### 关键引用

- [ARN: Analogical Reasoning on Narratives detailed study](https://arxiv.org/abs/2310.00996)
- [StoryAnalogy: Deriving Story-level Analogies from Large Language Models detailed study](https://arxiv.org/abs/2310.12874)
- [ANALOGICAL: A Novel Benchmark for Long Text Analogy Evaluation detailed study](https://arxiv.org/abs/2305.05050)
- [AnaloBench: Benchmarking the Identification of Abstract and Long-context Analogies detailed study](https://arxiv.org/abs/2402.12370)