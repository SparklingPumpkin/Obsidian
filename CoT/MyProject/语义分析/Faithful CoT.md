一种新颖的符号链，即Symbcot，即完全基于LLM的框架，该框架将符号表达式和逻辑规则与COT提示集成在一起。

从技术上讲，Symb CoT在LLM的基础上：
- 先将自然语言上下文翻译成符号格式，
- 用符号逻辑规则推导出解决问题的分步计划，
- 再由验证器检查翻译和推理链。

通过使用一阶逻辑和约束优化两种符号表达式在5个标准数据集上的全面评估，Symb Co T在性能上始终优于Co T方法，同时也刷新了当前的性能。

---
Logical reasoning (Cummins et al., 1991) stands out as a quintessential form of reasoning that, unlike other types, is crucial and challenging.