
[数据处理 - LLaMA Factory](https://llamafactory.readthedocs.io/zh-cn/latest/getting_started/data_preparation.html)
## 1. 安装llama-factory

```
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

- 如果出现环境冲突，请尝试使用 `pip install --no-deps -e .` 解决
- 完成安装后，可以通过使用 `llamafactory-cli version` 来快速校验安装是否成功


## 2. 数据集准备

[dataset_info.json](https://github.com/hiyouga/LLaMA-Factory/blob/main/data/dataset_info.json/)中包含了所有经过预处理的 **本地数据集** 以及 **在线数据集**。如果您希望使用自定义数据集，请 **务必** 在 `dataset_info.json` 文件中添加对数据集及其内容的定义。

#### 2.1现有CoT数据集 (Alpaca指令监督微调格式): 

```
{
        "instruction": "你是一个物理专家，你能够清晰准确地解决复杂的物理问题。下面将输入一个物理题目，请分步地给出详细的解题过程",

        "input": "一静止质量为m、荷电为e的电子在与均匀磁场垂直的平面内运动，如果忽略辐射，电子的轨道为半径为R的圆。设电子能量E >> mc²，求磁感应强度B的表达式。如果R = 30 m，E = 2.5 x 10¹⁰ eV，求出B的数值。",

        "output": "1. 我们首先考虑洛伦兹力作用下电子做匀速圆周运动的情形。电子所受到的洛伦兹力提供向心力，使得其在半径为R的圆周上运动。\n\n2. 对于一个带电粒子在磁场中做圆周运动，洛伦兹力的表达式为：\nF = e * v * B\n其中，e 是电子电荷量，v 是电子的速度，B 是磁感应强度。\n\n3. 向心力的表达式为：\nF = m * v² / R\n其中，m 是电子的质量，v 是电子的速度，R 是运动轨道的半径。\n\n4. 在圆周运动中，洛伦兹力提供了向心力，所以：\ne * v * B = m * v² / R\n\n5. 简化上式可以得到：\nB = m * v / (e * R)\n\n6. 现在考虑到电子的能量 E >> mc² 的条件，这意味着电子可以被视为高速运动（近似为相对论性运动）。相对论性动能和速度的关系为：\nE = sqrt[(pc)² + (mc²)²]，由于E >> mc²，可以近似为\nE ≈ pc\n\n7. 于是动量 p 可以表示为：\np ≈ E / c\n\n8. 对于高速粒子，动量 p 也可以写为：\np = mv（γ因子接近1时）\n\n9. 将上式代入B的表达式：\nB = m * v / (e * R) = (p * c) / (e * R * c) = E / (e * R * c)\n\n10. 因此磁感应强度 B 的表达式为：\nB = E / (e * R * c)\n\n11. 代入已知数值R = 30 m，E = 2.5 x 10¹⁰ eV，电子电荷量 e = 1.602 x 10⁻¹⁹ C，光速 c = 3 x 10⁸ m/s。\n注意将能量单位从电子伏特转换为焦耳：\n1 eV = 1.602 x 10⁻¹⁹ J\nE = 2.5 x 10¹⁰ eV x 1.602 x 10⁻¹⁹ J/eV = 4.005 x 10⁻⁹ J\n\n12. 代入B的表达式：\nB = 4.005 x 10⁻⁹ J / (1.602 x 10⁻¹⁹ C * 30 m * 3 x 10⁸ m/s)\n\n13. 计算：\nB = 4.005 x 10⁻⁹ / (1.602 x 10⁻¹⁹ * 30 * 3 x 10⁸)\nB = 4.005 x 10⁻⁹ / (1.4418 x 10⁻⁹)\nB ≈ 2.78 T\n\n答案：\nB ≈ 2.78 T"
}
```


## 3. SFT 训练



