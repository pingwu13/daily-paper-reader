---
title: "TWLA: Achieving Ternary Weights and Low-Bit Activations for LLMs via Post-Training Quantization"
title_zh: TWLA：通过后训练量化实现LLM三值权重和低比特激活
authors: "Zhixiong Zhao, Zukang Xu, Zhixuan Chen, Xing Hu, Zhe jiang, Dawei Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1fa7ae814fc72e76b8c3d68d90f1e9eb3d7b8c7d.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: PTQ实现1.58比特三值权重量化与4比特激活量化，保持高精度
tldr: TWLA是一种面向LLM的后训练量化框架，解决三值化模型中激活分布重尾导致无法低比特化的问题。它通过欧氏到流形的非对称分量设计，达成1.58比特权重量化和4比特激活量化并保持高精度，从而加速端到端推理。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LLM三值化方法因激活重尾分布而不得不保留高精度激活，限制端到端推理加速。
method: 提出欧氏到流形非对称组件的PTQ框架，协同处理三值权重与4比特激活量化。
result: TWLA在1.58比特权重与4比特激活下维持高精度，显著减少存储与计算开销。
conclusion: 后训练三值权重与低比特激活可共同实现，为LLM高效推理铺路。
---

## Abstract
Large language models (LLMs) exhibit exceptional general language processing capabilities, but their memory and compute costs hinder deployment. Ternarization has emerged as a promising compression technique, offering significant reductions in model size and inference complexity. However, existing methods struggle with heavy-tailed activation distributions and therefore keep activations in high precision, fundamentally limiting end-to-end inference acceleration. 
To overcome this limitation, we propose **TWLA**, a post-training quantization (PTQ) framework that achieves 1.58-bit weight compression and 4-bit activation quantization while maintaining high accuracy. TWLA comprises three components: (1) Euclidean-to-Manifold Asymmetric Ternary Quantizer (E2M-ATQ) minimizes layer-output error under weight ternarization via a two-stage optimization from Euclidean initialization to manifold relocation; (2) Kronecker Orthogonal Tri-Modal Shaping (KOTMS) applies a Kronecker-structured orthogonal rotation to reshape weights into ternary-friendly tri-modal distributions, while the shared rotation statistically suppresses activation outliers; and (3) Inter-Layer Aware Activation Mixed Precision (ILA-AMP) explicitly introduces adjacent-layer second-order interaction costs in bit allocation and jointly optimizes for the layer-wise disparity of activation quantization gains induced by the shared orthogonal transform, preventing cascades triggered by a few weak layers.
Extensive experiments demonstrate that TWLA is a PTQ method that maintains high accuracy under the **W1.58A4** configuration, while delivering significant inference acceleration. The code is available at [TWLA](https://github.com/Kishon-zzx/TWLA).

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 一、核心问题与整体含义

- **研究背景**：大型语言模型（LLM）展现出强大的通用语言处理能力，但其巨大的内存占用和计算开销严重制约了实际部署。
- **现有方法局限**：三值化（ternarization）作为一种有前景的压缩技术，能大幅降低模型体积和推理复杂度。然而，现有三值化方法面临**激活值重尾分布（heavy-tailed activation distributions）** 的难题，因此不得不保留高精度激活，从而**从根本上限制了端到端推理加速**。
- **论文目标**：提出一个后训练量化（PTQ）框架 **TWLA**，在 **W1.58A4**（1.58 比特权重 + 4 比特激活）配置下保持高精度，同时显著加速端到端推理。

## 二、方法论

TWLA 包含三个核心组件，协同解决三值权重与低比特激活联合量化的挑战：

1. **E2M-ATQ（Euclidean-to-Manifold Asymmetric Ternary Quantizer，欧氏到流形非对称三值量化器）**
   - 目标：最小化权重三值化条件下的**层输出误差**。
   - 实现：通过两阶段优化，先从**欧氏空间初始化**，再在**流形空间进行重定位**，从而得到更优的三值量化方案。

2. **KOTMS（Kronecker Orthogonal Tri-Modal Shaping，Kronecker 正交三模态整形）**
   - 目标：使权重分布更适合三值化。
   - 实现：利用 **Kronecker 结构的正交旋转**，将权重分布整形为**三模态分布**（三值友好），同时该共享旋转还能在统计上**抑制激活离群值**。

3. **ILA-AMP（Inter-Layer Aware Activation Mixed Precision，层间感知激活混合精度）**
   - 目标：解决共享正交变换引起的**逐层激活量化增益差异**问题。
   - 实现：在比特分配中显式引入**相邻层的二阶交互成本**，并联合优化，从而避免少数薄弱层引发的级联性能退化。

## 三、实验设计

- **数据集/场景**：摘要中未明确列出具体数据集名称，仅表示进行了“大量实验”（Extensive experiments）。
- **Benchmark**：未在给定内容中给出具体的基准测试集（如语言建模、下游任务等）。
- **对比方法**：未列出具体对比的基线方法，但通常情况下会与现有三值化 LLM 方法和 PTQ 方法比较。
- **评价指标**：主要关注量化后的**精度保持**和**推理加速效果**，具体数值未在摘要中给出。

## 四、资源与算力

- **原文未明确说明**使用的 GPU 型号、数量、训练/量化耗时等算力信息。
- 由于本文属于后训练量化（PTQ）方法，通常算力需求远低于全训练/微调，但具体资源消耗仍需参考论文正文。

## 五、实验数量与充分性

- 摘要仅提到“大量实验”，并未展示具体的实验数量、数据集列表、消融研究细节。
- 因此，从现有文本**无法客观评估实验的充分性与公平性**。
- 若论文正文包含多数据集、多模型规模、消融实验及与 SOTA 对比，则实验设计可能是充分的，但本总结所依据的摘要信息不足以做出最终判断。

## 六、主要结论与发现

- TWLA 是一个**后训练量化（PTQ）框架**，能够同时实现 **1.58 比特权重压缩**和 **4 比特激活量化**，并保持高精度。
- TWLA 在 W1.58A4 配置下有效，验证了**三值权重和低比特激活可以联合使用**，为 LLM 高效端到端推理奠定了基础。
- 该方法能够显著减少存储与计算开销，带来实际推理加速。

## 七、优点

- **问题针对性强**：直接解决了三值化 LLM 中激活重尾分布导致无法低比特化的问题，突破了端到端加速瓶颈。
- **方法创新度较高**：结合欧氏到流形优化的非对称三值量化器、Kronecker 正交旋转整形以及层间感知混合精度分配，三者协同设计，具备较好的系统性。
- **实用价值明确**：后训练量化降低了部署成本，且 W1.58A4 配置显著压缩模型，对实际硬件部署友好。
- **代码开源**：提供了 GitHub 链接，有助于复现与后续研究。

## 八、不足与局限

- **实验细节缺失**：从摘要中无法获知具体数据集、模型规模、任务类型、对比方法及具体性能数字，难以独立验证其声明。
- **泛化性存疑**：仅在摘要级别声称“大量实验”，尚不清楚 TWLA 在不同模型大小（如 7B、13B、70B）和不同任务（如文本生成、推理、翻译）上的表现是否一致稳健。
- **潜在偏差风险**：若实验只选择有利场景，可能高估方法效果；需要更透明的实验设置和与既有最强基线（如 QuIP、GPTQ、BiLLM 等）的详细对比。
- **应用限制**：W1.58A4 的低比特激活可能对部分任务（如长文本推理、精度敏感任务）造成不可忽略的性能损失，且正交旋转可能增加运行时开销，需要结合工程实现验证实际加速比。

（完）
