BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation
[多模态 CLIP/BLIP/BLIP-2论文理论知识总结_blip2-CSDN博客](https://blog.csdn.net/m0_59805198/article/details/134909942)

![[Pasted image 20241126212121.png]]
## 1. 动机

- Model perception: 
	- Both encoder-based model & encoder-decoder model cannot 直接地完成识别类和生成类任务
	- 希望一个模型把所有任务都解决--unified-framework
- Data perception：
	- 当有过多noisy数据集


## 2. 贡献

- Multimodal Mixture of Encoder-Decoder 模型结构
- Cap filter model 清理数据集
## 3. 模型介绍

![[Pasted image 20241126214507.png]]

>图中相同颜色共享参数

- 图像：
	- 一个N层ViT


