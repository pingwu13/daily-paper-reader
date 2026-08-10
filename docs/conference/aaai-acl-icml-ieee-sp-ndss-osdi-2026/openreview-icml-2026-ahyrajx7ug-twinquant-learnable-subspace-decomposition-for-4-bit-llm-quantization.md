---
title: "TwinQuant: Learnable Subspace Decomposition for 4-Bit LLM Quantization"
title_zh: TwinQuant：面向4比特LLM量化的可学习子空间分解
authors: "Haodong WANG, Junjie Liu, Zicong Hong, Qianli Liu, Jian Lin, Song Guo, Xu Chen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/16d936de0fd4233471a8b33c2cf315dc3123a345.pdf"
tags: ["query:ulbv"]
score: 6.0
evidence: 面向4比特LLM量化的可学习子空间分解
tldr: 该文针对4比特量化导致大语言模型精度下降的问题，提出TwinQuant框架。与以往基于SVD的分解方法不同，TwinQuant学习量化友好的子空间分解，并联合优化残差分量和低秩分量以最小化后量化误差。实验表明其在4比特权重量化上优于已有方法，为低比特权重压缩提供了新的分解思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 4比特量化导致LLM精度显著下降，现有分解未最小化后量化误差。
method: TwinQuant学习量化友好的子空间分解，联合优化残差与低秩分量。
result: 在4比特LLM量化上优于既有方法，缓解精度损失。
conclusion: 可学习子空间分解能够更好地匹配量化目标，提升低比特精度。
---

## Abstract
4-bit quantization reduces the memory footprint and latency of large language model inference, but its aggressive precision reduction can severely degrade accuracy. Prior methods address this by decomposing each weight matrix into two components (e.g., via singular value decomposition) and quantizing them separately, assigning the bulk of values to a low-precision residual component while handling outliers with a high-precision low-rank component. However, such decompositions are designed to minimize the real-valued energy of the residual, rather than the post-quantization error of the residual and low-rank components. We propose TwinQuant, a 4-bit quantization framework that learns quantization-friendly decomposed subspaces and jointly reshapes both the low-rank and residual components. TwinQuant learns component-specific transformations via a joint optimization over the Stiefel and general linear manifolds, flattening their distributions and reducing dynamic-range imbalance. To enable efficient end-to-end execution, we further design a fused dual-component kernel that pipelines the two-stage low-rank computation on-chip and merges both components with a single epilogue, avoiding intermediate global-memory traffic. Across LLaMA3 and Qwen3 models, TwinQuant preserves near-FP16 accuracy and delivers up to $1.8\times$ end-to-end speedup over an FP16 baseline.

---

## 论文详细总结（自动生成）

好的，我将按照您的要求，对TwinQuant这篇论文进行结构化、深入的总结。

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：大语言模型（LLM）在实际部署中面临巨大的内存占用和推理延迟挑战。4比特量化是一种有效的模型压缩手段，能显著减少内存占用和提升推理速度。
- **核心问题**：4比特量化过于激进，导致模型精度严重下降，在复杂任务上难以保持接近FP16的性能水平。
- **已有方法的不足**：现有主流方案（如基于SVD的分解方法）将权重矩阵分解为残差分量（低精度、处理多数数值）和低秩分量（高精度、处理离群值），分别量化。然而，这些分解方法的设计目标是**最小化实值分解的残差能量**，并未真正考虑**量化后**的误差。即分解目标与量化目标之间存在错配，导致量化后的整体误差并非最优。

## 2. 论文提出的方法论（TwinQuant）

- **核心思想**：TwinQuant提出“量化友好”的子空间分解，通过学习使分解出的两个分量更容易被量化，并**联合优化**两个分量，直接最小化后量化误差。
- **技术细节**：
  - **可学习子空间分解**：TwinQuant不再使用固定的SVD分解，而是引入可学习的分解矩阵，将权重矩阵分解为低秩分量和残差分量的组合。
  - **联合优化策略**：在优化过程中，同时在Stiefel流形（约束正交性）和一般线性流形上对两个分量进行协同优化。通过这种方式，TwinQuant能够**平滑两个分量的数值分布并降低动态范围差异**，使它们都更适合低比特量化。
  - **推理加速内核**：为实现高效端到端执行，设计了融合的双分量计算内核。该内核在芯片上**流水线化处理两阶段低秩计算**，并**通过单一的epilogue将两个分量的计算结果合并**，避免了中间结果在全局内存中的往返传输，提升了实际推理速度。
- **算法流程概述**：将权重矩阵输入可学习分解层 → 生成低秩分量和残差分量 → 在流形约束下联合优化两个分量的变换，使其分布平坦化 → 对两个分量执行4比特量化 → 使用融合内核在推理时同时计算两个分量的输出并合并。

## 3. 实验设计

- **使用的模型**：实验在**LLaMA3**和**Qwen3**系列模型上进行。
- **评估场景**：主要衡量在4比特权重量化下的**模型精度**（与FP16基线对比）以及**端到端推理速度**。
- **对比方法**：
  - FP16基线（全精度参考）。
  - 现有基于分解的量化方法（如基于SVD的方法）。
  - 其他已有4比特量化方案（文中未详细列出所有方法名称，但核心对比是“优于已有方法”）。

## 4. 资源与算力

- **未明确说明**：由于提供的文本信息有限，文中**未详细披露**训练或校准所使用的GPU型号、数量、训练时长或具体算力规模。仅在推理性能部分提到端到端加速比。

## 5. 实验数量与充分性

- **实验覆盖**：从摘要来看，实验横跨了两个主流模型家族（LLaMA3和Qwen3），涵盖了精度和速度两项关键指标。
- **充分性分析**：
  - **相对有限**：提供的文本仅给出了摘要级别的结果概述，**未列出具体的实验组数**（如不同模型规模、不同下游任务数量、不同量化配置的对比）以及**消融实验**（如验证联合优化的必要性、流形选择的影响、内核对速度的贡献等）。
  - **客观性**：摘要描述中采用了“near-FP16 accuracy”和“up to 1.8× speedup”等相对客观的表述，但没有提供具体数值表格，无法完全验证公平性。因此，仅凭所提供的信息来看，实验充分性属于**中等水平**，**缺乏足够细节来彻底评估其严谨程度**。

## 6. 论文的主要结论与发现

- TwinQuant通过引入**可学习的量化友好分解**，有效解决了传统分解与量化目标错配的问题。
- 在4比特量化条件下，TwinQuant能够在**LLaMA3和Qwen3**模型上保持**接近FP16的精度水平**，显著缓解了因精度降低带来的性能损失。
- 其设计的**融合双分量内核**能够有效提升推理速度，在端到端场景下实现了相对于FP16基线最高**1.8倍**的加速。

## 7. 优点

- **方法创新性**：突破了以往基于SVD固定分解的思路，提出“量化友好”的可学习分解，直击后量化误差最小化的核心目标，观点新颖。
- **系统级优化**：不仅关注算法精度，还针对实际部署设计了融合内核，通过流水线和单epilogue合并，解决了多分量计算的内存瓶颈，兼顾了算法与硬件效率。
- **性能表现突出**：在关键的4比特量化场景下，同时实现了精度保持和明显的速度提升。

## 8. 不足与局限

- **信息缺失**：公开信息中缺少具体实验配置、超参数设置、具体数值结果以及消融实验，无法进行深入的定量分析和复现比较，可能影响结论的全面性和可信度。
- **量化范围局限**：摘要主要聚焦于**权重量化（Weight Quantization）**，对激活值的量化效果或极端低比特下的表现未明确提及，在实际部署中可能需要对激活也进行量化以最大化收益。
- **额外开销**：可学习分解与联合优化涉及流形上的迭代优化，**这部分额外计算成本**（校准阶段的耗时）以及推理时低秩分量的额外计算开销是否在所有场景下都能被FP16加速所抵消，需要更全面的分析。
- **应用范围不确定性**：实验仅在LLaMA3和Qwen3两个架构上进行，对更广泛的模型族（如编码器模型、多模态模型）以及不同参数量规模下的有效性还未得到验证。

（完）
