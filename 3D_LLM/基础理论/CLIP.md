## 流程

[CLIP](https://blog.csdn.net/weixin_49787382/article/details/143176470)
- 图片通过image encoder 得到图像特征嵌入
- 文本通过text encoder 得到文本特征嵌入
- 对于每一对图像-文本，模型计算图像特征嵌入(image feature embedding)和文本特征嵌入(text feature embedding)之间的**相似性**(这里是余弦相似度矩阵)
- 使用对比学习(Contrastive Learning)的损失函数来优化，使得正确配对的图像和文本为正样本(接近1)，而错误配对为负样本(接近0)
![[Pasted image 20241201133657.png]]
## 细节

- CLIP的核心创新在于构建了一个**统一的特征空间**，使得图像和文本可以直接在这个空间内进行比较和匹配，从而实现跨模态的理解和转换。
- 两个核心组件构成：图像编码器和文本编码器
	- 用于把两者都编码到之前提到的**特征空间**

- 图像编码器
	- **基于CNN的ResNet系列** ：包括ResNet50、ResNet101等变体。
	- **基于Transformer的Vision Transformer (ViT)** ：包括ViT-B/32、ViT-B/16和ViT-L/14等版本。
- 文本编码器
	- 注意输出与图像编码器输出同一维度
	- 基于Transformer的架构
		- 12层Transformer模块
		- 隐藏层尺寸为512
		- 使用8个注意力头
		- 词汇表大小为49,152（使用Byte Pair Encoding）
		- 序列长度限制为76
	- ![[Pasted image 20241125164833.png]]
- 特征映射
	- 为了将图像和文本特征映射到共同的特征空间，CLIP引入了两个可学习的投影矩阵
		- `I_e = l2_normalize(np.dot(I_f, W_i), axis=1)`
		- `T_e = l2_normalize(np.dot(T_f, W_t), axis=1)`
	- 这样处理后的图像和文本嵌入向量就处于同一个d_e维空间中，可以通过计算余弦相似度来评估它们的相关性。



