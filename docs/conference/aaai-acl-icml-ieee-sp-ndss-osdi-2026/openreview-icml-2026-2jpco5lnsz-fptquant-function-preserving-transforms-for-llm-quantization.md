---
title: "FPTQuant: Function-Preserving Transforms for LLM Quantization"
title_zh: FPTQuant：面向LLM量化的函数保持变换
authors: "Boris van Breugel, Yelysei Bondarenko, Paul N. Whatmough, Markus Nagel"
date: 2026-04-30
pdf: "https://openreview.net/pdf/84a2a646b8b2f823742a7a1c8d1b60f2b3f85d2c.pdf"
tags: ["query:ulbv"]
score: 7.0
evidence: 用于低比特LLM量化中减少离群值影响的函数保持变换
tldr: 该文针对低比特LLM量化中大幅离群值导致性能退化的问题，提出FPTQuant方法。它设计了三种轻量且保持函数等价性的变换：可合并的pre-RoPE变换、值变换和动态缩放变换，利用Transformer固有等变性重新塑造中间激活分布。这些变换在保持模型功能的同时有效降低了离群值的影响，为低比特量化提供了新的结构先验。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 低比特LLM量化中大幅离群值导致精度受损，需要保持函数等价的变换。
method: 设计三种轻量函数保持变换，利用Transformer等变性和独立性重塑激活。
result: 在低比特量化中显著降低离群值影响，提升模型性能。
conclusion: 函数保持变换可作为低比特LLM量化中的通用预处理模块。
---

## Abstract
Large language models (LLMs) require substantial compute, and thus energy, at inference time. While quantizing weights and activations is effective at improving efficiency, naive quantization of LLMs can significantly degrade performance due to large magnitude outliers. This paper describes FPTQuant, which introduces three novel, lightweight, and expressive function-preserving transforms (FPTs) to facilitate quantization of transformers: (1) a mergeable pre-RoPE transform for queries and keys, (2) a mergeable transform for values, (3) a cheap, dynamic scaling transform. By leveraging the equivariances and independencies inherent to canonical transformer operation, we designed these FPTs to maintain the model’s function while shaping the intermediate activation distributions to be more quantization friendly. FPTQuant requires no custom kernels and adds virtually no overhead during inference. The FPTs are trained both locally to reduce outliers, and end-to-end such that the outputs of the quantized and full-precision models match. FPTQuant enables static INT4 quantization with minimal overhead and shows SOTA speed-up of up to 3.9x over FP. Empirically, FPTQuant has an excellent accuracy-speed trade-off—it is performing on par or exceeding most prior work and only shows slightly lower accuracy compared to a method that is up to 29% slower.

---

## 论文详细总结（自动生成）

# FPTQuant：面向LLM量化的函数保持变换——论文总结

## 1. 核心问题与整体含义

- **研究背景**：大型语言模型（LLMs）在推理时需要大量计算和能量。权重量化和激活量化能有效提升推理效率，是降低推理成本的重要手段。
- **核心问题**：对LLMs进行朴素量化会因激活中存在大幅度离群值（outliers）而导致显著的性能退化。如何在低比特量化下保持精度，是当前的主要挑战。
- **研究动机**：已有方法多引入特殊处理逻辑或复杂内核，增加部署成本。论文希望设计一种轻量、通用、且保持模型数学等价性的预处理变换，在不改变模型功能的前提下，重塑激活分布以减少离群值影响，从而同时获得高精度和高效率。
- **整体含义**：FPTQuant 提出了一类可作为通用预处理模块的函数保持变换（FPTs），为低比特LLM量化提供了新的结构先验，并可在几乎零推理开销下实现静态 INT4 量化。

## 2. 方法论

### 核心思想
利用Transformer本身固有的等变性（equivariance）和独立性（independency），设计多种轻量变换。这些变换在数学上保持模型输出不变（即函数保持，function-preserving），但在数值上重新分配了激活的尺度，使中间激活分布更有利于量化，从而正面应对离群值。

### 三种函数保持变换
1. **可合并的 pre-RoPE 变换（Mergeable Pre-RoPE Transform）**  
   用于查询（Queries）和键（Keys）。在RoPE旋转位置编码之前应用尺度变换，利用RoPE与特定缩放操作的可交换/可合并性质，使得变换能够被合并到后续算子中，不增加额外推理成本。

2. **可合并的值变换（Mergeable Value Transform）**  
   用于值（Values）向量。对attention输出中的值分支进行缩放变换，同样利用线性算子（如输出投影）的可合并性，保持函数等价，同时重塑值激活的分布。

3. **动态缩放变换（Dynamic Scaling Transform）**  
   一种廉价且可动态适应输入统计的缩放方案，用于进一步压缩离群值的动态范围，同时保持推理时的低开销。

### 训练方式
- **局部训练（Locally）**：以直接减小离群值为目标，对每个变换的参数进行优化。
- **端到端训练（End-to-end）**：与整体模型联合优化，使量化模型的输出与全精度模型输出尽量对齐，从而减少量化误差的累积。

### 关键特性
- **无需自定义内核**：所有变换均可通过标准算子合并实现，兼容现有推理框架。
- **推理几乎零额外开销**：变换在训练后与相邻层合并，不改变推理时实际的计算图。

## 3. 实验设计

- **基准与场景**：面向Transformer架构的LLM低比特量化任务，重点评估静态 INT4 量化性能。
- **主要对比方法**：与多个已有低比特量化方法进行比较，包括先前的主流离群值抑制方法和量化方案。
- **数据集**：具体数据集列表（如语言建模基准或下游任务集）在提供的摘要中未详细列出，仅概括性地报告了精度和速度的对比结果。
- **评测指标**：
  - **准确性（Accuracy）**：量化模型与全精度模型的输出匹配度及任务指标。
  - **推理速度（Speed-up）**：相对FP（全精度）基线的加速比，最高可达3.9倍。
  - **速度-精度权衡**：与“速度最慢方法”（比FPTQuant慢29%）相比，仅表现出略低的准确率，但速度优势明显。

## 4. 资源与算力

- **文中情况**：目前提供的摘要和元数据中**未明确提及**使用的GPU型号、数量、训练时长、批大小或具体计算资源消耗。
- **推论**：仅说明“训练是轻量的”，且推理时无自定义内核、几乎无额外开销，但并未给出精确的算力统计。

## 5. 实验数量与充分性

- **实验组数量**：摘要层面仅概括性地指出了与大多数先前工作相当或超过、与一个慢29%的方法相比略逊，**具体实验组数（如不同数据集数量、消融实验数量）未在此提供文本中展开**。
- **充分性评估**：
  - 从报告的结果来看，覆盖了准确率和速度两个核心维度，并给出了与多类方法的横向对比，具备初步说服力。
  - 但由于缺少详细的实验设置（数据集、模型规模、量化配置、消融设计等）描述，在给定信息下**无法完全判断实验的全面性和绝对公平性**。
  - 可推测原文应有更完整的实验章节（含多个LLM和任务），但本次提供的文本不足以对此作出确定性结论。

## 6. 主要结论与发现

- 函数保持变换能够在不改变模型功能的前提下，通过重塑激活分布有效抑制离群值，显著提升低比特（INT4）量化性能。
- FPTQuant 支持静态 INT4 量化，推理时几乎零额外开销，且无需自定义内核，部署友好。
- 在速度方面表现出 **SOTA级别的加速（最高3.9倍于FP基线）**，同时保持了良好的精度。
- 在速度-精度权衡上，与最先进的慢速方案相比，仅牺牲极小的准确率，换来大幅速度提升，体现出较好的综合表现。

## 7. 优点

- **方法设计巧妙**：利用Transformer的等变性和独立性，深刻抓住模型内部结构先验，而非暴力抑制离群值。
- **轻量且实用**：三种变换均可合并到现有算子中，无需自定义内核，不增加推理负担，工程成本低。
- **通用性潜力**：设计的FPTs是通用预处理模块，可应用于不同Transformer模型，具备迁移潜力。
- **训练方式灵活**：局部训练加快收敛/离群值减少，端到端训练保证量化输出对齐，兼顾效率与精度。
- **综合平衡优秀**：在速度与精度之间取得了很好的折中，性能与先进方法相当或更优，同时提速明显。

## 8. 不足与局限

- **实验覆盖信息不足**：从提供的文本看，未具体列出测试数据集、模型规模与任务类型，无法全面评估泛化性。
- **量化范围较窄**：主要展示静态INT4量化；其他位宽（如3bit、混合精度）和权重/激活同时量化的场景是否同样有效尚不明确。
- **对Transformer架构的依赖**：方法依赖Transformer等变性，无法直接适用于非自注意力或结构变化较大的模型。
- **训练开销表述模糊**：虽然提到训练是轻量的，但未给出实际训练时间或采样等细节，较难与其他方法进行公平的成本比较。
- **对比公平性风险**：虽然报告了与慢29%的方法的差距，但未详细说明硬件、实现优化、批处理等条件的一致性，存在一定对比偏差风险。
- **仅在摘要层面**：本文档仅是摘要与元数据，未呈现完整的定性/定量分析，因此局限性的判断还需依赖原文全文。

（完）
