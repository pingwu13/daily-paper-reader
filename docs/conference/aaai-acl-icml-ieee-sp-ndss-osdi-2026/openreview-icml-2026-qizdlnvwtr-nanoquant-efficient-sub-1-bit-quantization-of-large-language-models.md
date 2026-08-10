---
title: "NanoQuant: Efficient Sub-1-Bit Quantization of Large Language Models"
title_zh: NanoQuant：大语言模型的高效亚1比特量化
authors: "Hyochan Chong, Dongkyu Kim, Changdong Kim, Minseop Choi"
date: 2026-04-30
pdf: "https://openreview.net/pdf/c5df747263f5d256ee156e20438943db11965c35.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 通过低秩二值分解对LLM权重进行亚1比特后训练量化
tldr: 为突破LLM二值（1比特）甚至亚1比特压缩效率低的难题，提出NanoQuant后训练量化方法。它将权重量化建模为低秩二值矩阵分解问题，利用ADMM求解器高效地初始化潜二值矩阵和缩放因子，再对初始化参数做轻量微调。该方法在避免大量数据和算力的同时，将LLM权重压缩到二值与亚1比特水平，为超低位权重量化的高效部署提供了新途径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法难以高效地将LLM压缩到二值或亚1比特，常需大量数据计算或引入额外存储。
method: 提出NanoQuant，将量化建模为低秩二值分解问题，用ADMM高效初始化潜二值矩阵与缩放因子并进一步微调。
result: 在二值和亚1比特权重量化上实现高效PTQ压缩，降低存储和部署开销。
conclusion: 给出LLM亚1比特PTQ的高效低秩二值化分解方案。
---

## Abstract
Weight-only quantization has become a standard approach for efficiently serving large language models (LLMs). However, existing methods fail to efficiently compress models to binary (1-bit) levels, as they either require large amounts of data and compute or incur additional storage. In this work, we propose NanoQuant, a post-training quantization (PTQ) method to compress LLMs to both binary and sub-1-bit levels. NanoQuant formulates quantization as a low-rank binary factorization problem, and compresses full-precision weights to low-rank binary matrices and scales. Specifically, it utilizes an efficient alternating direction method of multipliers (ADMM) solver to precisely initialize latent binary matrices and scales, and then tunes the initialized parameters through a block and model reconstruction process. Consequently, NanoQuant establishes a new Pareto frontier in low-memory post-training quantization, and enables sub-1-bit compression. NanoQuant makes large-scale deployment feasible on consumer hardware. For example, it compresses Llama-2-70B by 24$\times$ in just 13 hours on a single H100, enabling a 70B model to operate on a consumer 8 GB GPU.

---

## 论文详细总结（自动生成）

# NanoQuant 论文总结

## 1. 核心问题与整体含义
- 大语言模型（LLM）的服务部署通常采用仅权重量化来降低内存与计算开销。
- 现有权重量化方法在压缩到二值（1-bit）水平时效率低下，要么依赖大量数据和算力，要么引入额外存储开销。
- NanoQuant 旨在实现**高效的后训练量化（PTQ）**，将 LLM 压缩到**二值（1-bit）甚至亚 1-bit 水平**，使大规模模型能在消费级硬件上部署。
- 整体意义在于：提出一种新的低内存量化范式，推动 LLM 在资源受限环境下的可用性。

## 2. 方法论
- **核心思想**：将权重量化问题建模为**低秩二值矩阵分解（low-rank binary factorization）**。
  - 全精度权重被分解为低秩二值矩阵和缩放因子的组合。
  - 通过这种分解，模型权重可由极低位宽表示，从而突破 1-bit 限制，实现亚 1-bit 压缩。
- **关键流程**：
  1. **ADMM 初始化**：利用交替方向乘子法（Alternating Direction Method of Multipliers, ADMM）高效求解低秩二值分解问题，精确初始化潜在的二值矩阵与缩放因子。
  2. **轻量微调**：对初始化后的参数进行分块（block-wise）和整体模型重建（model reconstruction），以补偿量化误差。
- 该方法避免了传统全精度训练或大规模校准数据依赖，属于后训练量化的一种高效变体。

## 3. 实验设计
- **数据集**：摘要中未明确提及使用的具体数据集或校准数据来源。
- **场景/Benchmark**：未说明具体的评测基准（如常见语言任务、困惑度等）。
- **对比方法**：未列出与哪些已有二值/亚 1-bit 量化方法进行对比。
- 文中所给唯一具体实例：将 **Llama-2-70B** 压缩 24 倍，并能将该 70B 模型部署在 **8 GB 显存的消费级 GPU**上。

## 4. 资源与算力
- 摘要明确提到：压缩 Llama-2-70B 模型仅需 **1 块 H100 GPU**，耗时 **13 小时**。
- 未提及训练/量化过程中其他硬件资源、能耗、内存占用等细节。
- 因此，关于算力的信息极不完整，无法进行更全面的资源评估。

## 5. 实验数量与充分性
- 从摘要看，实际可验证的实验证据非常有限：
  - 仅给出一个模型（Llama-2-70B）的压缩比和运行环境示例。
  - 未提供针对多种模型规模、多种语言任务、多种位数（如 1-bit、0.5-bit 等）的系统性评估。
  - 未提及消融实验、对比实验、稳健性分析等。
- 因此，现有摘要信息不足以判断实验的充分性、客观性和公平性，需要查看完整论文才能评估。

## 6. 主要结论与发现
- NanoQuant 在**低内存后训练量化**领域建立了新的**帕累托前沿**——在压缩效率与部署可行性之间取得更优平衡。
- 成功实现了**亚 1-bit 级别的 LLM 权重量化**。
- 通过高效的低秩二值分解，避免了现有二值量化方法对大量数据和算力的需求，同时不引入额外存储开销。
- 具体示例表明，可在单块 H100 上于 13 小时内完成 70B 模型的 24 倍压缩，并在 8 GB 消费级 GPU 上运行，验证了消费者硬件部署的可行性。

## 7. 优点
- **高效性**：相比以往二值量化方法，显著降低了对数据和计算资源的需求。
- **无需额外存储**：通过低秩二值分解避免额外的辅助参数存储。
- **突破位宽极限**：支持二值甚至亚 1-bit 量化，极大压缩模型体积。
- **实用部署**：能够在单卡上完成大规模 LLM 压缩，并适配消费级 GPU 内存。
- **方法新颖**：将量化问题形式化为低秩二值分解并用 ADMM 高效求解，具有一定理论和方法价值。

## 8. 不足与局限
- **信息缺失**：本总结仅基于摘要和元数据，论文正文未提供，因此缺乏实验细节、数学公式和对比结果。
- **实验覆盖不足**：未展示该方法在不同模型族（如 Qwen、Mistral）上的泛化能力。
- **性能影响未知**：压缩到亚 1-bit 后，模型在具体任务上的精度损失程度没有在摘要中给出。
- **基准与对比不明**：未提供与 SOTA 二值/亚 1-bit 量化方法的客观对比，无法判断相对优势。
- **应用限制**：仅提到一个 70B 规模的示例，对于更小或更大模型、不同架构的适用性尚待验证。

> 总结：NanoQuant 在方法构思上颇具创新，但当前可见信息不足以全面评估其实验质量，需要进一步阅读完整论文以确认其性能与可靠性。

（完）
