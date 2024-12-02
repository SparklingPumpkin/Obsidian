### 1. **输入预处理（Tokenization）**

- **分词（Tokenization）：** 输入的自然语言（比如一段文本）会通过分词器（Tokenizer）被拆分为基本单位（tokens）。这些tokens可以是单词、子词（subwords），或者在某些模型中甚至是字符。
    
    - 常见的分词方法包括 **Byte Pair Encoding (BPE)** 和 **WordPiece**。
    - 例如：句子 "I love AI" 可能会被分为 ["I", "love", "AI"] 或 ["I", "lo", "ve", "AI"]。
- **转化为数字表示（Token IDs）：** 每个token都会映射到一个唯一的数字ID，这些ID对应于模型的词汇表（Vocabulary）。
    

---

### 2. **嵌入层（Embedding Layer）**

- 每个token ID会被映射到一个高维向量（词向量，Word Embedding）。这个嵌入向量是一个固定维度的稠密向量，用来表示该token的语义信息。
    
- 嵌入层的作用是将离散的输入tokens转化为连续的数值表示，使其能够作为神经网络的输入。
    

---

### 3. **深度神经网络处理（Deep Neural Network Processing）**

- **Transformer架构：** 大多数现代LLM（例如GPT、BERT）使用的是基于Transformer的架构，它包括两个主要模块：
    
    - **多头自注意力机制（Multi-Head Self-Attention）：** 计算每个token与其他token之间的相关性，从而捕获上下文信息。
    - **前馈神经网络（Feedforward Neural Network）：** 在捕获上下文信息后，对每个token的嵌入向量进行进一步的非线性变换。
- 模型通过堆叠多层Transformer模块（通常几十到上百层），学习输入tokens之间的复杂关系。
    
- **位置编码（Positional Encoding）：** Transformer无法自然感知顺序，因此会加入位置编码，帮助模型理解输入序列的顺序信息。
    

---

### 4. **输出生成（Decoding）**

- 模型通过输出层生成结果。这一过程通常会将高维的输出向量投影回词汇表大小的维度。
    
- 使用 **Softmax函数** 计算每个词在词汇表中的概率分布，输出的token是概率最高的词或子词。
    
- **生成策略：**
    
    - Greedy Search：每次选择概率最高的词。
    - Beam Search：探索多个生成路径。
    - Sampling（例如Top-k或Top-p采样）：引入一定随机性，提升生成多样性。

---

### 5. **后处理**

- 将输出tokens转化为自然语言文本（通过分词器的反向映射）。
- 如果是交互式生成，还可能进行多轮迭代。