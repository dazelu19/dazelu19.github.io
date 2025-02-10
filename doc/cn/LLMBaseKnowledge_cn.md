# 大模型基础

## 1. transformer
Transformer 绝对是需要深入学习的结构，下面的图是从文章中截来的。

![transformer architecture](https://github.com/dazelu19/dazelu19.github.io/blob/main/images/transformer.png)

## 2. [大模型的采样技术](https://nn.labml.ai/zh/)
### 2.1 贪婪采样
### 2.2 温度采样
### Top-K采样
### 核采样

## Q&A
#### 1 问题1：为什么要使用Feedforword结构？
在 Transformer 使用 **Feedforward Networks** 的主要目的是提升模型的表达能力，通过非线性变换进一步捕获输入数据的特征。

**丰富特征表示**
   多头自注意力机制（Multi-Head Attention）捕捉了输入序列之间的依赖关系，但它本质上是一个加权求和操作，并不能很好地提取复杂的非线性特征。前馈神经网络通过非线性激活函数（如ReLU）对每个位置的特征进行变换，从而增强模型的特征提取能力。

**逐位置的处理**
   Feedforward 是逐位置（Position-wise）的，也就是说它对每个时间步的特征进行独立的处理。这种设计保留了自注意力机制捕捉到的全局信息，同时进一步增加了每个位置的特征复杂性。

   数学公式为：
   
   $$FFN(x) = \text{ReLU}(xW_1 + b_1)W_2 + b_2$$
  
   其中 $(W_1, W_2)$ 是权重矩阵, $(b_1, b_2)$ 是偏置。

**提供非线性建模能力**
   线性变换和多头注意力机制本质上是线性操作，而前馈神经网络通过激活函数引入非线性能力，使得模型能够更好地拟合复杂的分布和模式。
   
**增加网络深度**
   Feedforward Networks 是编码器模块的重要组成部分，与多头注意力机制一起构成了编码器的核心。多个编码器层堆叠时，前馈网络提供了更多的建模层次，使模型更深，从而更好地捕捉复杂关系。

**有效的参数共享**
   Feedforward 网络在每个位置上共享参数，这减少了模型的参数量，并使得模型具有更好的训练效率和泛化能力。

---
#### 2 问题2： 在多头注意力机制中，为什么要除以 $\sqrt{d_k}$
根据《Attention Is All You Need》这篇论文，在多头注意力机制中除以 $(\sqrt{d_k})$ 的原因如下：

**点积值的大小控制**
论文指出，查询向量 $(Q)$ 和键向量 $(K)$ 的每个元素通常是均值为 0、方差为 1 的随机变量。当 $(d_k)$ 较大时，点积 $(QK')$ 的期望值仍为 0，但其方差会随 $(d_k)$ 增大而线性增长（方差为 $(d_k)$）。这种增长会导致点积值变得非常大，从而影响后续的 softmax 计算。

通过缩放因子 $\sqrt(d_k)$，可以将点积值限制在一个适当的范围，避免因数值过大而导致的计算不稳定问题。



**Softmax 的梯度问题**
当 $(QK')$ 的值过大时，softmax 函数的输入值会落入极值区域。在这种情况下，softmax 的梯度变得极小（梯度消失），这会阻碍模型的学习能力。通过除以 $\sqrt(d_k)$，点积值被缩小到较小范围，从而使 softmax 函数的梯度保持在合理范围内，促进模型训练。



**与加性注意力的对比**
论文中提到，未缩放的点积注意力在较大的 $(d_k)$ 值下性能会低于加性注意力（additive attention），后者使用前馈神经网络来计算查询和键之间的兼容性分数。而通过对点积注意力进行缩放后，二者在性能上的差异显著减小，同时点积注意力在计算效率和空间复杂度上更有优势。



![](https://github.com/dazelu19/dazelu19.github.io/blob/main/images/multi_head_attention.png)

### Reference
[1] transformer by hand: https://aibyhand.substack.com/p/8-can-you-calculate-a-transformer

