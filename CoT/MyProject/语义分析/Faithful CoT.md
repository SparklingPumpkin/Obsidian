一种新颖的符号链，即Symbcot，即完全基于LLM的框架，该框架将符号表达式和逻辑规则与COT提示集成在一起。

从技术上讲，Symb CoT在LLM的基础上：
- 先将自然语言上下文翻译成符号格式，
- 用符号逻辑规则推导出解决问题的分步计划，
- 再由验证器检查翻译和推理链。

通过使用一阶逻辑和约束优化两种符号表达式在5个标准数据集上的全面评估，Symb Co T在性能上始终优于Co T方法，同时也刷新了当前的性能。

---
Logical reasoning (Cummins et al., 1991) stands out as a quintessential form of reasoning that, unlike other types, is crucial and challenging. 逻辑推理( Cummins et al , 1991)作为一种典型的推理形式脱颖而出，与其他类型的推理不同，它是关键的和具有挑战性的。

逻辑推理需要严格的逻辑运算，**严重依赖符号表达式和严格的推理规则来**表示问题的内部结构。普通文本往往不足以支持这种精确的逻辑，特别是在需要严格逻辑表达的场景中。例如，如图1所示，在处理逻辑推理问题时，使用一阶逻辑( First-Order Logic，FOL )等符号表示比CoT中的完全自然语言理据更具有代表性和精确性，通过明确的推理规则实现严格的逻辑推理。

## 3 SymbCoT for Symbolic Reasoning
### 3.1 问题定义

	▶ Example:  
	<Premises> (P ) 
	A hawk never lands. Some birds are hawks. 
	<Statement> (S) 
	All birds land. 
	<Answer> 
	False. / True. / Unknow.

### 3.2 模型


