- CLIP的核心创新在于构建了一个**统一的特征空间**，使得图像和文本可以直接在这个空间内进行比较和匹配，从而实现跨模态的理解和转换。
- 两个核心组件构成：图像编码器和文本编码器
	- 用于把两者都编码到之前提到的**特征空间**

- 图像编码器
	- **基于CNN的ResNet系列** ：包括ResNet50、ResNet101等变体。
	- **基于Transformer的Vision Transformer (ViT)** ：包括ViT-B/32、ViT-B/16和ViT-L/14等版本。
- 文本编码器
	- 基于Transformer的架构
		- - 12层Transformer模块
		- 隐藏层尺寸为512
		- 使用8个注意力头
		- 词汇表大小为49,152（使用Byte Pair Encoding）
		- 序列长度限制为76
	- ![[Pasted image 20241125164833.png]]



