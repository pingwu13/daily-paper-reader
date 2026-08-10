---
title: "LiftQuant: Continuous Bit-Width LLM via Dimensional Lifting and Projection"
title_zh: LiftQuant：通过维度提升与投影实现连续位宽大语言模型量化
authors: "Liulu He, Xuan Ang Liu, Juntao Liu, Taolue Feng, Ting Lu, Chunsheng Gan, ZHIYV PENG, Yuan Du, Huanrui Yang, Yijiang Liu, Li Du"
date: 2026-04-30
pdf: "https://openreview.net/pdf/431db6782d1e844292d1b485fd3e1bdea0956900.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 用一比特格投影实现低位大模型量化并连续调节位宽
tldr: 针对现有量化只能取整数位宽（如2、3比特）、难以匹配任意内存预算的部署间隙，提出LiftQuant框架。其核心是提升-投影机制：在一个高维提升空间中用简单的1比特格去逼近低维权向量，并通过提升维数与原始维数的比例实现准连续的有效位宽调节。该方法使得LLM可针对具体内存预算进行灵活部署，逼近更优的精度-内存Pareto前沿。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有整数位宽量化无法让LLM适配任意内存预算，存在部署间隙，难以达到真正Pareto最优部署。
method: 提出LiftQuant，通过提升-投影机制用高维空间中的1比特格逼近低维权向量，以提升维数比连续调节有效位宽。
result: 实现准连续位宽控制，使LLM可按内存预算灵活部署并改善精度-内存权衡。
conclusion: 为低比特LLM量化提供连续位宽调节的新机制，填补整数位宽部署间隙。
---

## Abstract
Existing quantization methods are fundamentally limited by rigid, integer-based bit-widths (e.g., 2, 3-bit), resulting in a "deployment gap" where Large Language Models cannot be optimally fitted to specific memory budgets. To bridge this gap, we introduce LiftQuant, a novel framework that enables continuous bit-width control for true Pareto-optimal deployment.  The core innovation is a "lift-then-project" mechanism which approximates low-dimensional weight vectors by projecting a simple 1-bit lattice from a higher-dimensional ``lifted" space.  Crucially, the effective bit-width is determined simply by the ratio of the lifted dimension to the original dimension, which allows the bit-width to be tuned quasi-continuous as the dimension is a flexible structural parameter. This projection generates a structured yet non-uniform codebook, capturing the expressive power of Vector Quantization (VQ).  While beneficial over VQ, LiftQuant's decoding path relies solely on linear transformations and 1-bit uniform quantizers, retaining hardware-friendly nature.  This flexibility is transformative: LiftQuant enables a 70B LLM to be compressed to 2.4 bits to precisely fit a 24GB GPU, where its performance significantly surpasses state-of-the-art 2-bit models fitted on the same device.  Our code and ckpt is available at \url{https://github.com/Heliulu/LiftQuant}.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题背景**：现有大语言模型（LLM）量化方法通常只能使用**刚性整数位宽**（如 2、3 比特）进行压缩，无法任意匹配具体的硬件内存预算。
- **部署间隙**：由于实际设备（如 GPU）的显存容量是连续的（如 24GB、48GB），而整数位宽只能提供离散的模型大小档位，导致模型要么超出内存预算，要么浪费可用显存，难以达到真正 Pareto 最优部署。
- **核心目标**：实现**连续位宽控制**，让同一个模型可以根据任意内存预算灵活调节压缩率，从而优化“精度-内存”权衡。

## 2. 方法论：LiftQuant 框架

- **核心思想**：提出 “lift-then-project”（提升-投影）机制。
  - 先将低维权向量**提升**到高维空间；
  - 在提升后的高维空间中，用一个简单的 **1 比特格（1-bit lattice）** 去近似原始权重向量；
  - 再将该近似结果**投影**回原始维度空间。
- **关键创新**：有效位宽由**提升维度与原始维度的比值**决定。由于维度作为结构参数可灵活调整，因此可以实现**准连续**的位宽调节（如 2.4 比特）。
- **码本特性**：上述投影过程产生一个**结构化但非均匀的码本**，能够捕获向量量化（Vector Quantization, VQ）的表达能力，但优于传统 VQ。
- **硬件友好性**：解码路径仅依赖**线性变换**和 **1 比特均匀量化器**，避免了 VQ 中复杂的查找表/码本检索，保持了硬件实现的高效性。
- **公式化说明**（文字描述）：假设原始权重为低维向量，选择一个更高的维度 d'，在该 d' 维空间中构造一个简单的 1-bit 格点集，对权重做近似后投影回原始维度；有效位宽 ≈ d'/d（或类似比例），通过调整 d' 实现连续位宽。

## 3. 实验设计

- **数据集与场景**：论文摘要中未明确列出具体评测数据集（如 WikiText、C4、下游任务基准等）。
- **Benchmark**：未明确说明使用了哪些标准量化基准（如 GPTQ、AWQ 常用评测任务）。
- **对比方法**：仅提到与“最先进的 2-bit 模型”在同一设备上对比，未给出完整对比方法列表（如 GPTQ、AWQ、OmniQuant、QuIP 等）。
- **实际展示**：给出了一个 70B LLM 压缩到 2.4 比特后适配 24GB GPU 的示例，并声称性能显著优于同设备上最先进的 2-bit 模型。

## 4. 资源与算力

- **文中未明确说明**训练/量化所需的 GPU 型号、数量、时长等具体算力信息。
- 仅提及一个部署案例：将 70B 模型压缩至 2.4 比特可在 24GB GPU 上运行，但未说明量化过程本身的算力开销。

## 5. 实验数量与充分性

- **实验数量**：根据提供的摘要文本，缺乏详细的实验数量信息。
  - 没有列出具体的评测数据集数量；
  - 没有展示不同位宽下的完整精度-内存曲线；
  - 没有消融研究（如对提升维度、投影方式、码本结构等的单独分析）。
- **充分性与客观性**：仅从摘要看，实验证据有限，尚不足以充分验证方法在所有场景下的有效性；对比基准不够全面，公平性难以评估。

## 6. 主要结论与发现

- LiftQuant 能够实现**准连续位宽控制**，突破了整数位宽的限制。
- 通过调整提升维度，可使 LLM 精确适配任意内存预算，例如 70B 模型以 2.4 比特部署在 24GB GPU 上。
- 在同一设备约束下，LiftQuant 的性能显著优于同位数（2-bit）的现有最先进方法，逼近更优的“精度-内存”Pareto 前沿。
- 方法在保持 VQ 表达能力的同时，仍保留硬件友好的解码结构。

## 7. 优点

- **概念新颖**：首次（据称）以“维度提升-投影”机制实现连续位宽量化，填补整数位宽与内存预算之间的部署间隙。
- **简单可控**：位宽仅由维度比决定，调节方式灵活、直观。
- **硬件友好**：无需复杂码本检索，仅线性变换+1-bit 量化，便于实际部署。
- **表达能力**：结构化非均匀码本兼顾了 VQ 的表达力和均匀量化器的硬件效率。
- **实际价值**：可针对具体 GPU 显存定制最优压缩率，对大规模模型部署具有现实意义。

## 8. 不足与局限

- **实验信息不透明**：摘要中未提供具体数据集、任务、基线方法及完整结果，难以判断泛化能力。
- **缺少消融分析**：未说明提升维度选择、投影函数设计、不同模型规模下的表现等关键因素。
- **缺乏与其他连续量化方法的对比**：如是否与基于混合精度搜索或自适应位宽的方法进行比较。
- **算力开销未提及**：提升到高维空间可能引入额外计算/存储开销，文中未量化。
- **适用性局限**：仅展示 70B 模型的一个案例，未能证明在小模型或多任务场景中的有效性。
- **潜在偏差风险**：由于没有完整实验细节，可能存在结果选择或展示偏差的可能性。

---

（完）
