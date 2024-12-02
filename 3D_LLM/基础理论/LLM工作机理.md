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


----------
## 参数量计算

一个具有**70亿参数（7B）**的大语言模型（例如GPT-3 7B版本或类似规模模型）的设计通常具有以下特征：

---

### 1. **基本公式与参数分布**

语言模型中的参数主要分布在以下几个部分：

- **嵌入层（Embedding Layer）：** 用于将词汇表中的token映射到向量空间。
- **Transformer层：** 包括多头自注意力和前馈网络，是模型主要参数的来源。
- **输出层（Output Layer）：** 用于将隐藏状态映射回词汇表大小。

对于一个Transformer模型，总参数量可以粗略估算为：

参数量=嵌入层参数+L⋅(自注意力参数+前馈网络参数)+输出层参数\text{参数量} = \text{嵌入层参数} + L \cdot (\text{自注意力参数} + \text{前馈网络参数}) + \text{输出层参数}参数量=嵌入层参数+L⋅(自注意力参数+前馈网络参数)+输出层参数

其中，LLL 是Transformer层数，且：

- 嵌入层参数 ≈dmodel⋅vocab_size\approx d_\text{model} \cdot \text{vocab\_size}≈dmodel​⋅vocab_size
- 自注意力参数 ≈4⋅dmodel2\approx 4 \cdot d_\text{model}^2≈4⋅dmodel2​
- 前馈网络参数 ≈8⋅dmodel2\approx 8 \cdot d_\text{model}^2≈8⋅dmodel2​ （两层线性变换）
- 输出层参数 ≈dmodel⋅vocab_size\approx d_\text{model} \cdot \text{vocab\_size}≈dmodel​⋅vocab_size

---

### 2. **典型规模配置（7B 参数）**

以7B参数为目标，模型通常采用以下配置：

- **层数（LLL）：** 通常为 32~40 层 Transformer。
- **宽度（dmodeld_\text{model}dmodel​）：** 通常为 4096。
- **词汇表大小（vocab_size）：** 通常为 50,000。
- **注意力头数（nheadsn_\text{heads}nheads​）：** 通常为 32，意味着每个头的维度为 dhead=dmodel/nheads=4096/32=128d_\text{head} = d_\text{model} / n_\text{heads} = 4096 / 32 = 128dhead​=dmodel​/nheads​=4096/32=128。

---

### 3. **参数计算细节**

#### **嵌入层参数**

嵌入层参数=dmodel⋅vocab_size=4096⋅50000=2.048×108 （约2亿）\text{嵌入层参数} = d_\text{model} \cdot \text{vocab\_size} = 4096 \cdot 50000 = 2.048 \times 10^8 \, \text{（约2亿）}嵌入层参数=dmodel​⋅vocab_size=4096⋅50000=2.048×108（约2亿）

#### **每层Transformer参数**

每层Transformer由两部分构成：

1. **自注意力参数：**
    
    - Query, Key, Value权重： 3⋅dmodel23 \cdot d_\text{model}^23⋅dmodel2​
    - 输出权重： dmodel2d_\text{model}^2dmodel2​
    
    自注意力参数=4⋅dmodel2=4⋅40962=6.71×107 （约6700万）\text{自注意力参数} = 4 \cdot d_\text{model}^2 = 4 \cdot 4096^2 = 6.71 \times 10^7 \, \text{（约6700万）}自注意力参数=4⋅dmodel2​=4⋅40962=6.71×107（约6700万）
2. **前馈网络参数：**
    
    - 两个线性变换层： 2⋅dmodel⋅(4⋅dmodel)=8⋅dmodel22 \cdot d_\text{model} \cdot (4 \cdot d_\text{model}) = 8 \cdot d_\text{model}^22⋅dmodel​⋅(4⋅dmodel​)=8⋅dmodel2​
    
    前馈网络参数=8⋅dmodel2=8⋅40962=1.34×108 （约1.34亿）\text{前馈网络参数} = 8 \cdot d_\text{model}^2 = 8 \cdot 4096^2 = 1.34 \times 10^8 \, \text{（约1.34亿）}前馈网络参数=8⋅dmodel2​=8⋅40962=1.34×108（约1.34亿）

总计每层Transformer的参数：

每层Transformer参数=6.71×107+1.34×108=2.01×108 （约2亿）\text{每层Transformer参数} = 6.71 \times 10^7 + 1.34 \times 10^8 = 2.01 \times 10^8 \, \text{（约2亿）}每层Transformer参数=6.71×107+1.34×108=2.01×108（约2亿）

#### **所有Transformer层参数**

假设有 L=36L = 36L=36 层：

所有Transformer层参数=L⋅每层参数=36⋅2.01×108=7.24×109 （约72亿）\text{所有Transformer层参数} = L \cdot \text{每层参数} = 36 \cdot 2.01 \times 10^8 = 7.24 \times 10^9 \, \text{（约72亿）}所有Transformer层参数=L⋅每层参数=36⋅2.01×108=7.24×109（约72亿）

#### **输出层参数**

通常，输出层共享嵌入层权重，因此无需额外计算。

---

### 4. **验证总参数量**

- 嵌入层参数：2.048×108 (≈2亿)2.048 \times 10^8 \, (\approx 2亿)2.048×108(≈2亿)
- Transformer层参数：7.24×109 (≈72亿)7.24 \times 10^9 \, (\approx 72亿)7.24×109(≈72亿)

总参数量：

总参数量=2.048×108+7.24×109≈7.44×109 （约7B）\text{总参数量} = 2.048 \times 10^8 + 7.24 \times 10^9 \approx 7.44 \times 10^9 \, \text{（约7B）}总参数量=2.048×108+7.24×109≈7.44×109（约7B）

---

### 5. **结论**

- **层数：** 通常为 **36** 层（或类似）。
- **宽度（dmodeld_\text{model}dmodel​：** 通常为 **4096**。
- **每层参数：** 大约 **2亿**。
- **总参数量：** 按上述计算，约为 **7B**。


------

## 另一种计算方法

[大模型7B、13B参数是如何分布的？ - 知乎](https://zhuanlan.zhihu.com/p/654495506)


![[Pasted image 20241202182340.jpg]]


以baichuan-7B为例，分解各模块参数

- total # 7000559616
	- _embed_tokens_ # 64000 * 4096 = _**262144000**_
	- _layers_ # 202383360 * 32 = _**6476267520**_
		- DecoderLayer # 202383360
			- input_layernorm # 4096
			- self_attn # 50331648 + 16777216 = 67108864
				- W_pack（QKV） # 4096 * 4096 * 3 = 50331648
				- o_proj # 4096 * 4096 = 16777216
			- post_attention_layernorm # 4096
			- mlp # 4096 * 11008 * 3 = 135266304
- _norm_ # _**4096**_
- _lm_head_ # 4096 * 64000 = _**262144000**_


















