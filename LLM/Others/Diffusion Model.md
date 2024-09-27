
# 1 Diffusion 如何运作

>The sculpture is already complete within the marble block, before I start my work. It is already there, I just have to chisel away the superfluous material  -- Michelangelo

## 1.1 简单步骤

1. 输入目标图片需求
2. Simple一张与目标图片相同大小的纯噪声图片P_1000
3. 降噪 -- 输入P_1000 & 当前步骤序号：1000
4. 得到新的图片P_999
5. 重复3-4
6. 得到最终图片P_1
![[Pasted image 20240927170403.png]]
![[Pasted image 20240927171507.png]]

## 1.2 内部架构

![[Pasted image 20240927171659.png]]  

## 1.3 如何得到这样一个数据集

- Forward Process (Diffusion Process)
- ![[Pasted image 20240927173148.png]]
## 1.4 Text-to-Image

- 数据集LAION
- 新的Denoise过程：加入了文字标签
	- ![[Pasted image 20240927174516.png]]
- Noise Predictor
	- ![[Pasted image 20240927174723.png]]
- Forward Process
	- ![[Pasted image 20240927175609.png]]
## 1.5 公式代码

![[Pasted image 20240927175812.png]]




# 2 Stable Diffusion

![[Pasted image 20240927181108.png]]
























