# Efficient Region-Aware Neural Radiance Fields for High-Fidelity Talking Potrait Synthesis

> https://arxiv.org/abs/2307.09323





## Abstract

快速匯聚，實施渲染，表現良好通過小的模型尺寸





## Introduction



 Recently, Neural Radiance Fields (NeRF) [26]
is introduced into audio-driven talking portrait synthesis. 它提供了一种通过深层多层感知器（MLP）学习**从音频特征到相应视觉外观的直接映射的新方法。**



**通过用稀疏特征网格替换部分MLP网络**[33，27，6，8，15，5，16]，已经证明了与普通NeRF相比的巨大加速。







为了捕捉音频信号的区域影响，我们-
探索音频特征与
所提出的三平面Hash Repre的位置编码-
哨兵。而不是将原始功能和
通过基于MLP的大型学习视听相关性
编码器，我们提出了一个区域注意力模块-
通过调整音频功能以最适合某些空间区域
跨模态注意力机制。因此
肖像的某些部分可以获得更合适的特征
为精确的面部动作建模，而其他静态por-
动作仍然不受变化信号的影响。通过获得
区域意识，高质量高效的建模
可以实现局部运动。





## Related Work

### 2D-based talking portrait synthesis

