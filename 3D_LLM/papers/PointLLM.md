# PointLLM: Empowering Large Language Models to Understand Point Clouds

## 背景

几个3D领域LLM的应用
- 理解3D结构--LVLM--仍然存在的问题：诸如深度模糊、遮挡和视点依赖性
- 通过口头命令交互式地创建和编辑3D内容

端到端多模态LLM目前困难
- 获取大规模的多模态 instruction-following data（指令跟随数据）

## 贡献

- PointLLM通过**端到端训练**提供了对**点云对象**的直接和全面的理解，实现了准确、开放和自由形式的交互。
- 提出了一种在GPT-4的帮助下利用大规模点云caption数据集Cap3D的**自动数据生成技术**，生成的数据集遵循统一的指令遵循模板，由Brief-description instrutions和complex instrutions 组成，分别有助于潜在空间对齐和指令调整。

## 模型结构

- 如图所示，PointLLM是一个生成模型，旨在完成包含点云和文本的多模态句子。该模型有三个模块组成：预训练的point cloud encoder、 projector和LLM，

  - point cloud encoder：包括了多层的transformer block
  - projector：包含多个线性层和GULU激活函数

- 双阶段训练

  - 第一阶段：特征对齐阶段，冻结点云编码器和LLM的参数，并仅仅训练MLP projector，在这个阶段，训练过程使用简短的描述指令，旨在有效地将点特征与文本标记空间对齐。
  - 第二阶段：指令调整阶段，冻结点云编码器，同时联合训练projector和LLM；第二阶段使用复杂指令，帮助模型建立理解和响应包括点云数据在内的复杂指令的能力。