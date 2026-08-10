---
title: "GEMQ: Global Expert-Level Mixed-Precision Quantization for MoE LLMs"
title_zh: GEMQ：面向MoE大语言模型的全局专家级混合精度量化
authors: "Jianing Deng, Song Wang, Dongwei Wang, Zijie Liu, Tianlong Chen, Huanrui Yang, Jingtong Hu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e83bf0e33145e9a37db67643a75b8433031358e7.pdf"
tags: ["query:ulbv"]
score: 7.0
evidence: 全局专家级混合精度量化，支撑MoE大模型极端低比特部署
tldr: MoE-LLM专家参数造成巨大内存开销，混合精度量化虽可按专家重要性分配位宽，但现有逐层估计忽略量化引起的路由器偏移。提出GEMQ，以全局线性规划捕捉模型级专家重要性，并基于量化误差分析分配专家级位宽，从而支持极低比特量化。实验表明该方法能优化位宽分配，在MoE-LLM上逼近精度-内存Pareto前沿，为超低位部署提供有效支撑。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: MoE-LLM专家参数庞大，现有混合精度量化仅按层估计重要性，忽略量化导致的路由偏移，分配欠优。
method: 提出GEMQ，用全局线性规划建模模型级专家重要性，结合量化误差分析分配专家位宽，支持极端低比特量化。
result: 在MoE-LLM上改进混合精度量化分配，逼近精度-内存Pareto前沿。
conclusion: 为MoE-LLM极端低比特量化提供全局专家级位宽分配方法。
---

## Abstract
Mixture-of-Experts Large Language Models (MoE-LLMs) achieve strong performance but incur substantial memory overhead due to massive expert parameters.
Mixed-precision quantization mitigates this cost by allocating expert-wise bit-widths based on their importance, approaching the accuracy-memory Pareto frontier and enabling extreme low-bit quantization.
However, existing methods rely on layer-wise importance estimation and overlook router shifts induced by quantization, resulting in suboptimal allocation and routing.
In this work, we propose Global Expert-level Mixed-precision Quantization (GEMQ) to overcome these limitations via (1) a global linear-programming formulation that captures model-wide expert importance based on quantization error analysis, and (2) efficient router fine-tuning to adapt routing to quantized experts. These components are integrated into a progressive quantization framework that iteratively refines importance estimation and allocation.
Experiments demonstrate that GEMQ significantly reduces memory and accelerates inference with minimal accuracy degradation.

---

## 论文详细总结（自动生成）

# GEMQ：面向MoE大语言模型的全局专家级混合精度量化——论文总结

## 1. 核心问题与整体含义
- **背景**：Mixture-of-Experts（MoE）大语言模型（MoE-LLMs）在性能上表现出色，但由于包含大量专家参数，内存开销巨大。
- **量化动机**：混合精度量化（Mixed-precision Quantization）是一种有效压缩手段，它根据专家重要性为不同专家分配不同位宽，从而逼近“精度-内存”Pareto前沿，并支持极低比特量化。
- **现有方法的缺陷**：已有方法大多采用**逐层（layer-wise）重要性估计**，忽略了量化引起的**路由器偏移（router shifts）**——即路由机制对量化后专家的判断可能失真，导致位宽分配次优和路由不准确。
- **本文贡献**：提出 **GEMQ（Global Expert-level Mixed-precision Quantization）**，从**全局视角**对专家级位宽进行优化，同时引入路由器微调，以解决上述限制。

## 2. 方法论
- **核心思想**：不再按层独立估计重要性，而是通过**全局线性规划（global linear-programming）**建模模型级专家重要性，从而将位宽分配问题形式化为全局优化问题。
- **关键技术细节**：
  - 基于**量化误差分析（quantization error analysis）**来估计每个专家对全局精度的影响，作为线性规划的目标依据。
  - 设计**路由器微调（efficient router fine-tuning）**机制，使路由模块适应量化后的专家权重，减少量化导致的路由偏移。
  - 采用**渐进式量化框架（progressive quantization framework）**，通过迭代方式交替执行“重要性估计 → 位宽分配 → 量化 → 路由器微调”，逐步改进整体分配策略。
- **算法流程（文字描述）**：
  1. 对当前模型进行量化误差分析，获得各专家对模型输出的影响程度；
  2. 构建全局线性规划模型，以总内存或比特数约束为条件，求解最优专家级位宽分配；
  3. 按分配结果对专家进行量化；
  4. 固定量化后的专家，微调路由器以恢复路由精度；
  5. 重复上述步骤，直至位宽分配收敛或达到目标压缩率。

## 3. 实验设计
- **数据集与场景**：论文摘要中**未明确列出**具体使用的数据集或评测场景，仅笼统描述为“在 MoE-LLMs 上进行实验”。
- **Benchmark**：未提及具体基准（如语言建模困惑度、下游任务准确率等）。
- **对比方法**：摘要仅提及与“现有混合精度量化方法”对比，这些方法依赖逐层重要性估计；但**未给出具体方法名称或基线细节**。
- **评估指标**：包括内存减少量、推理加速效果、精度变化，以及是否接近“精度-内存”Pareto前沿。

## 4. 资源与算力
- **论文提供文本中未提及**：没有说明使用的 GPU 型号、数量、训练/微调时长、显存占用等具体算力信息。
- 因此无法从现有材料中总结资源消耗情况；若需要完整信息，须查阅论文全文。

## 5. 实验数量与充分性
- **公开文本中缺乏详细信息**：摘要没有给出实验组数、消融实验数量、不同模型规模或量化位宽组合等具体数据。
- **充分性判断**：由于缺少实验细节，难以客观评估实验的充分性和公平性。从摘要表述看，方法在减少内存和加速推理方面有明显效果，但缺少与多个强基线、多种模型规模、各类下游任务的横向比较。
- **潜在风险**：实验结果可能仅针对特定 MoE 模型或特定量化设置，普适性有待验证。

## 6. 主要结论与发现
- GEMQ 能够**显著减少内存占用并加速推理**，同时仅带来**很小的精度损失**。
- 相比逐层重要性估计的现有方法，GEMQ 的全局专家级位宽分配更能逼近“精度-内存”Pareto 前沿。
- 路由器微调能够有效缓解量化引起的路由偏移，从而提升量化后模型的整体性能。
- 整体上，GEMQ 为 MoE-LLM 的极端低比特部署提供了有效的位宽分配方案。

## 7. 优点
- **全局优化视角**：从模型整体出发估计专家重要性，克服了逐层方法忽略层间交互的缺陷。
- **考虑路由器偏移**：明确建模量化对路由机制的影响，并通过微调加以修复，设计针对性强。
- **渐进式迭代框架**：通过“估计-分配-量化-微调”闭环，能够持续改进分配质量，具有一定自适应性。
- **实用性**：支持极低比特量化，能直接缓解 MoE-LLM 的内存瓶颈，有利于边缘部署。

## 8. 不足与局限
- **信息不充分**：所提供的文本仅包含摘要，缺乏实验数据集、模型规模、基线详情、消融实验等关键信息，无法对结果进行独立验证。
- **训练/微调成本**：路由器微调可能引入额外计算开销，在超大规模模型上的成本未在摘要中说明。
- **可扩展性**：全局线性规划在专家数量极大时可能面临求解复杂度问题，文中未讨论其在实际大规模系统中的扩展性。
- **应用范围**：方法仅针对 MoE 架构；对稠密模型或混合异构模型是否适用尚不清楚。
- **偏差风险**：由于缺乏多场景验证，Pareto 前沿逼近的结论可能存在对特定模型或量化配置的依赖性。

（完）
