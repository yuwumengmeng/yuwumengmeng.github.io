---
title: LLM 与多模态大模型理论基础
date: 2025-12-22 16:00:00
tags: [LLM, VLM]
categories: Science Documentation
---

## 第一章：Transformer 架构的数学范式 (Mathematical Paradigm of Transformer)

**核心目标**：从线性代数与概率论角度，解析 Transformer 替代 RNN/CNN 的理论必然性。

### 1.1 归纳偏置 (Inductive Bias) 的转移

* **序列建模的演变**：对比 RNN 的时序依赖（Sequential Dependency）与 Transformer 的全局依赖（Global Dependency）。
* **计算复杂度分析**：分析 Self-Attention 在长序列下的  复杂度瓶颈与并行计算优势。

### 1.2 注意力机制的数学形式 (Formalism of Attention Mechanism)

* **Scaled Dot-Product Attention**：


* **缩放因子 (Scaling Factor) **：从统计学角度分析其对 Softmax 梯度消失问题的缓解作用（维持方差稳定）。


* **多头注意力 (Multi-Head Attention)**：分析多子空间（Subspaces）特征捕获的线性投影变换。
* **位置编码 (Positional Encoding)**：
* 绝对位置编码：正弦/余弦函数的频域特性。
* 相对位置编码：RoPE (Rotary Positional Embeddings) 的复数旋转性质及其在外推性（Extrapolation）上的数学优势。



### 1.3 前馈网络与层归一化 (FFN & LayerNorm)

* **FFN 的记忆属性**：探讨 FFN 层作为 Key-Value Memory Networks 的理论解释。
* **Pre-Norm vs Post-Norm**：分析残差连接中的梯度流与训练稳定性差异。

---

## 第二章：预训练范式与缩放定律 (Pre-training Paradigms & Scaling Laws)

**核心目标**：探讨模型能力的来源以及算力分配的最优解。

### 2.1 架构分化与目标函数 (Architectural Divergence & Objective Functions)

* **Auto-Regressive (AR, Decoder-only)**：
* 代表：GPT 系列、Llama、DeepSeek。
* 目标函数：最大化似然估计 (MLE) 。


* **Auto-Encoding (AE, Encoder-only)**：
* 代表：BERT、RoBERTa。
* 机制：Masked Language Modeling (MLM) 与双向上下文理解。


* **Encoder-Decoder**：
* 代表：T5、BART。
* 应用：Seq2Seq 任务（机器翻译、摘要）的归纳偏置优势。



### 2.2 缩放定律 (Scaling Laws)

* **幂律关系 (Power Law)**：Kaplan 等人提出的损失函数  与参数量 、数据集大小 、计算量  的关系。
* **Chinchilla Optimality**：DeepMind 关于 Compute-Optimal 模型的结论（Token 数与参数量的线性比例优化）。

---

## 第三章：高效推理与架构创新：以 DeepSeek 为例 (Efficiency & DeepSeek Architecture)

**核心目标**：分析在显存墙（Memory Wall）与算力限制下，现代大模型的架构突围。

### 3.1 混合专家模型 (MoE - Mixture of Experts)

* **稀疏激活 (Sparse Activation)**：从稠密模型 (Dense) 到稀疏模型的计算图变换。
* **路由机制 (Routing Mechanisms)**：
* Top-K Gating 原理。
* **DeepSeek-V3 的创新**：无辅助损失负载均衡 (Auxiliary-Loss-Free Load Balancing) 与 细粒度专家分割 (Fine-grained Expert Segmentation) 的设计理念。
* 共享专家 (Shared Experts) 对知识冗余的抑制作用。



### 3.2 键值缓存优化 (KV Cache Optimization)

* **MQA (Multi-Query Attention) 与 GQA (Grouped-Query Attention)**：通过共享 Key-Value 头降低显存带宽压力。
* **MLA (Multi-Head Latent Attention)**：
* DeepSeek 的核心贡献。通过低秩矩阵分解 (Low-Rank Matrix Factorization) 压缩 KV Cache。
* **数学原理**：将 KV 投影到低维 Latent Space，推理时还原，实现 KV 缓存的极度压缩与 RoPE 的解耦兼容。



---

## 第四章：多模态基础与 Vision Transformer (Multimodal Foundations & ViT)

**核心目标**：解析 Agent 的“眼睛”，即视觉编码器与多模态对齐技术。

### 4.1 Vision Transformer (ViT)

* **Patch Embedding**：将 2D 图像切片线性展开为 1D 序列，消除 CNN 的局部感受野限制，引入全局注意力。
* **Inductive Bias 的缺失与数据需求**：分析 ViT 为何相比 CNN 需要更强的预训练数据（JFT-300M）或更强的正则化。

### 4.2 多模态对齐技术 (Multimodal Alignment)

* **CLIP (Contrastive Language-Image Pre-training)**：
* 双塔架构 (Two-Tower)。
* 对比学习损失 (Contrastive Loss) 在高维空间中的语义对齐原理。


* **LMM (Large Multimodal Models) 架构设计**：
* **Projector (投影层)**：Linear vs MLP vs Q-Former (BLIP-2)。
* **Visual Instruction Tuning**：如何将视觉特征映射到 LLM 的 Embedding 空间（如 LLaVA 架构）。



---

## 第五章：推理增强与对齐 (Reasoning Enhancement & Alignment)

**核心目标**：从概率生成向逻辑推理的范式转变，涵盖 DeepSeek-R1 的最新路径。

### 5.1 对齐算法 (Alignment Algorithms)

* **RLHF (Reinforcement Learning from Human Feedback)**：
* Bradley-Terry 模型在偏好建模中的应用。
* PPO (Proximal Policy Optimization) 算法中的 KL 散度约束。


* **DPO (Direct Preference Optimization)**：推导 DPO 如何将强化学习问题转化为二分类优化问题，消除 Reward Model 的显式训练。

### 5.2 System 2 思维与测试时计算 (Test-Time Compute)

* **Chain of Thought (CoT)**：中间推理步骤对概率分布的平滑作用。
* **DeepSeek-R1 与 GRPO**：
* **GRPO (Group Relative Policy Optimization)**：摒弃 Value Model，利用群体采样计算基线（Baseline），降低 RL 训练成本。
* **纯强化学习的涌现**：通过规则奖励（Rule-based Reward）在冷启动阶段激发模型的推理与自我反思（Self-Verification）能力。
