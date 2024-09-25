# QA
- 为什么需要用到这种框架
	- 仅依靠LLM没法进行大规模统筹化的思考，需要用框架去束缚和提示LLM
	- 束缚和提示LLM：让LLM能够专注于解决一个个小问题，最终放到框架中就解决了一个大问题。
	- 类似于身体手脚- 框架；大脑- LLM

# 1 Introduction

LangChain (Language Chain) 是一个 LLM 编程框架，官方的定义如下
- LangChain是一个基于语言模型开发应用程序的框架。它可以实现以下应用程序：
	- 数据感知：将语言模型连接到其他数据源
	- 自主性：允许语言模型与其环境进行交互

优势：
- 组件化
- 链式连接（有预设的链可以直接使用）

类似应用：
- rootindex

# 2 组件解释

**Prompts**
- Prompts用来管理 LLM 输入的工具，在从 LLM 获得所需的输出之前需要对提示进行相当多的调整，最终的Promps可以是单个句子或多个句子的组合，它们可以包含变量和条件语句。
- LangChain会根据用户的问题自动  调用一些已有的Prompts框架，或基于此生成新的框架

**Chains**
- 是一种将LLM和其他多个组件连接在一起的工具，以实现复杂的任务。
- 一些类型
	- LLMChain
		- 仅仅依赖LLM
	- SimpleSequentialChain
		- 多个输入输出LLM连接（可以是同一种LLM）
	- SequentialChain
		- 多个分支多输入多输出LLM连接
	- RouterChain
		- 有类似分类任务的更复杂的Chain

**Agents**
- 相当于是LangChain的核心，即框架。
- 可以看作是一种使用LLM做出决策的工具，它们可以执行特定的任务并生成文本输出。Agents通常由三个部分组成：Action、Observation和Decision。Action是代理执行的操作，Observation是代理接收到的信息，Decision是代理基于Action和Observation做出的决策。

- 目前有的开源Agents：BabyAGI
- 其他Agents：AutoGPT（写论文用的）

**Memory**
- 是一种用于存储数据的工具，由于LLM 没有任何长期记忆，它有助于在多次调用之间保持状态。
- 一些类型
	- ConversationBufferMemory
	- ConversationBufferWindowMemary
	- ConversationTokenBufferMemory
	- ConversationSummaryMMemo

![[Pasted image 20240924222345.png]]
# 3 原理

![[Pasted image 20240924220729.png]]

用到了**知识库**。
