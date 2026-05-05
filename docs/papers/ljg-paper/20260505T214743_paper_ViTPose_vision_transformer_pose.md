---
title: paper_ViTPose_vision_transformer_pose
date: 2026-05-05 Tue 21:47
identifier: 20260505T214743
source: /home/along/src/paper/ai4sports/ViTPose.pdf
authors: Yufei Xu, Jing Zhang, Qiming Zhang, Dacheng Tao
venue: NeurIPS 2022
---

# 问题

Vision Transformer 在图像分类、目标检测上都很强，但在姿态估计上，大家还在用 CNN 提特征 + Transformer 做后处理。没人试过"纯 Transformer 能不能直接做姿态估计"。

ViTPose 就是来回答这个问题的：用最简单的 plain ViT 做骨干网络，加一个轻量级解码器，不需要任何花哨设计，就能在 MS COCO 上达到 SOTA（80.9 AP）。

# 翻译

## 锚点：一个极简的姿态估计器

想象一个姿态估计系统，它的骨干网络就是标准的 ViT——没有层级结构、没有跨层连接、没有专门为姿态设计的模块。输入图片，ViT 提取特征，然后一个两层反卷积解码器输出关键点热图。就这么简单。

ViTPose 证明：姿态估计不需要复杂设计，plain ViT + 简单解码器就够了。

## 四个"好"：简单、可扩展、灵活、可迁移

**简单（Simplicity）**
- 骨干网络：plain ViT，没有层级结构，只是堆叠 Transformer 层
- 解码器：两层反卷积 + 一层预测层（甚至可以简化成一层双线性上采样 + 一层预测层）
- 没有跨层连接、没有交叉注意力、没有专门为姿态设计的模块

**可扩展（Scalability）**
- 从 ViT-B（100M 参数）到 ViT-H（600M 参数）到 ViTAE-G（1B 参数）
- 只需要换骨干网络，解码器不变
- 在吞吐量-精度曲线上达到新的 Pareto 前沿

**灵活（Flexibility）**
- 输入分辨率：可以适应不同分辨率（256×192 到 384×288）
- 多数据集联合训练：加多个解码器头，每个数据集一个头
- 预训练策略：可以用 MAE 在 ImageNet 上预训练，也可以用更小的姿态数据集预训练
- 冻结注意力：训练时可以冻结注意力模块，只训练 FFN 和解码器

**可迁移（Transferability）**
- 大模型的知识可以通过"知识 token"迁移到小模型
- 在小模型的输入中加一个可学习的 token，用大模型的输出监督这个 token
- 小模型性能提升，推理时不需要大模型

## 效果

MS COCO val：ViTPose-H 达到 78.3 AP（超过 HRFormer-B 的 75.6 AP）
MS COCO test-dev：ViTPose-G 达到 80.9 AP（SOTA）
吞吐量：ViTPose-B 达到 430 fps（比 HRNet-W48 的 230 fps 快近一倍）

# 核心概念

## Plain ViT 的特征表示能力

**一句话**：plain ViT（没有层级结构、没有专门设计）的特征表示能力足够强，不需要复杂的解码器就能做好姿态估计。

**对比**：HRFormer 用多分辨率并行 Transformer 模块，TransPose 用 CNN 提特征再用 Transformer 处理。ViTPose 直接用 plain ViT，解码器只有两层反卷积。

**为什么重要**：这说明姿态估计的瓶颈不在"如何设计网络结构"，而在"如何提取好的特征"。MAE 预训练的 plain ViT 已经学到了足够好的特征，简单解码器就能用好这些特征。

## 知识 token 迁移

**一句话**：在小模型的输入中加一个可学习的 token，用大模型的输出监督这个 token，让小模型学到大模型的知识。

**类比**：就像一个学生（小模型）在考试时可以看老师（大模型）的答案——不是直接抄答案，而是学老师的解题思路。知识 token 就是这个"思路"的载体。

**为什么重要**：传统知识蒸馏需要大模型和小模型同时推理，计算量大。知识 token 只在训练时需要大模型，推理时小模型独立运行，没有额外开销。

# 洞见

**姿态估计的复杂度不在网络结构，在预训练**——ViTPose 用最简单的结构（plain ViT + 两层解码器）就达到 SOTA，关键是 MAE 预训练提供了好的初始化。这个洞见对所有密集预测任务都有意义：与其设计复杂的网络结构，不如用更好的预训练方法。

# 博导审稿

问题定位准确。现有姿态估计方法要么用 CNN 提特征 + Transformer 后处理，要么设计复杂的层级 Transformer。ViTPose 证明 plain ViT 就够了，这个发现有价值。

实验扎实。在 MS COCO、MPII、AIC 三个数据集上都验证了有效性。消融实验覆盖了解码器设计、预训练策略、多数据集训练、知识迁移等多个方面。

但有一个根本性的限制：ViTPose 的成功高度依赖 MAE 预训练。论文自己也承认"pre-training using smaller unlabelled pose datasets can also provide a good initialization"，但没有深入分析"为什么 MAE 预训练对姿态估计这么有效"。如果没有 MAE 预训练，plain ViT 还能这么强吗？

另一个问题：ViTPose 的"简单"是有代价的——它需要更大的模型（ViT-H 有 600M 参数，HRNet-W48 只有 60M 参数）。虽然吞吐量更高，但内存占用也更大。这个 trade-off 论文没有详细讨论。

**判决：strong accept**。证明了 plain ViT 在姿态估计上的潜力，实验扎实，是 Transformer 姿态估计的重要 baseline。

# 启发

**迁移**：ViTPose 的"plain ViT + 简单解码器"框架可以迁移到其他密集预测任务——语义分割、深度估计、光流估计。关键是用好的预训练方法（如 MAE）提供初始化，不需要复杂的解码器设计。

**混搭**：ViTPose 的知识 token 迁移 + UD-AQA 的不确定性估计可以组合：用大模型的知识 token 不只传递"预测结果"，还传递"不确定性估计"。小模型不仅学到大模型的预测能力，还学到大模型的置信度判断能力。

**反转**：ViTPose 假设"plain ViT 的特征足够好，不需要复杂解码器"。但反过来想：如果特征不够好（比如没有 MAE 预训练），复杂解码器能不能弥补？能不能设计一个"自适应解码器"，根据特征质量动态调整复杂度——特征好时用简单解码器，特征差时用复杂解码器？
