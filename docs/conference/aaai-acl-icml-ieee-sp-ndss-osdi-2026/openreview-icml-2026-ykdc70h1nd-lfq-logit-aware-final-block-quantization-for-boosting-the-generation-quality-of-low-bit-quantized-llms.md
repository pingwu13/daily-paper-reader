---
title: "LFQ: Logit-aware Final-block Quantization for Boosting the Generation Quality of Low-Bit Quantized LLMs"
title_zh: LFQ：面向低比特量化 LLM 生成质量提升的 Logit 感知末块量化
authors: "Jung Hyun Lee, June Yong Yang, Jungwook Choi, Eunho Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/14f472d61196a8c6291bccd0aaf37ef395aa9c83.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 面向 LLM 的低比特仅权重量化，通过 logit 感知末块量化提升生成质量
tldr: 低比特仅权重量化能大幅降低大语言模型部署内存，但逐块 PTQ 在基础语言建模上虽接近全精度，却在生成任务、尤其是长回答和长思维链场景下显著退化。LFQ 发现问题是未优化 LM Head 且仅用 MSE 损失，导致量化模型的 token 概率分布偏移。为此提出 logit 感知的末块量化方法，显式优化最终层的输出分布。实验表明该方法能提升低比特模型的生成质量，增强长序列与思维链场景下的准确性，弥补了逐块 PTQ 的短板。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 逐块低比特 PTQ 在生成任务上质量退化，原因是忽略 LM Head 并仅使用 MSE 目标。
method: 提出 LFQ，在末块量化中引入 logit 感知目标，优化未嵌入层与 token 概率分布。
result: 实验显示 LFQ 显著提升低比特量化 LLM 在长文本与思维链生成上的质量。
conclusion: LFQ 为生成导向的低比特 LLM 部署提供了有效的量化优化方案。
---

## Abstract
As large language models continue to scale, low-bit weight-only post-training quantization (PTQ) offers a practical solution to their memory-efficient deployment. Although block-wise PTQ is capable of matching the full-precision (FP) baseline on basic language modeling and understanding, its quality is degraded for \textit{generative} tasks---especially at longer responses and extended chains of thought, which is critical in boosting task accuracy. We attribute this shortfall to two factors: (i) the omission of the unembedding layer (the LM head) in block-wise optimization and (ii) the reliance on the mean squared error (MSE) objective. Both factors cause the token probability distribution of the quantized model to misalign with that of the FP model, yielding notable accuracy drops on text generation benchmarks. To rectify the discrepancy, we introduce \emph{Logit-aware Final-block Quantization (LFQ)}, a simple yet effective enhancement to block-wise PTQ that quantizes the final Transformer block by minimizing the cross-entropy between the logits of the FP model and those of its quantized counterpart. By aligning token probabilities at the logit level in the final block, LFQ consistently improves the accuracy of complex generation tasks over state-of-the-art block-wise PTQ across diverse model families, while maintaining parity with FP baselines on language modeling and understanding.

---

## 论文详细总结（自动生成）

# 中文总结

> 说明：以下总结仅基于提供的论文元数据与英文摘要，未包含完整论文正文细节，部分信息（如具体数据集、算力配置、实验数量）在原文中若未提及则无法给出。

## 1. 核心问题与整体含义

- **研究背景**：大语言模型（LLM）规模持续增长，低比特仅权重量化（weight-only post-training quantization, PTQ）是降低部署内存开销的实用方案。
- **核心问题**：现有的逐块（block-wise）PTQ 在基础语言建模和理解任务上能匹配全精度（FP）基线，但在**生成任务**上质量明显退化，尤其是在长回复和长思维链（extended chains of thought）场景下，严重影响任务准确率。
- **原因剖析**：论文将其归因于两点：
  1. 逐块优化时**忽略了去嵌入层（LM head）**；
  2. 仅使用**均方误差（MSE）** 作为优化目标。
  这两点导致量化模型的 token 概率分布与全精度模型产生错位（misalignment）。
- **整体含义**：该工作聚焦于解决低比特量化模型在生成任务上的质量短板，对于 LLM 的高效且高质部署具有重要意义。

## 2. 方法论

- **核心思想**：提出 **Logit-aware Final-block Quantization（LFQ）**，作为一种对逐块 PTQ 的简单而有效的增强手段，专门量化最后一个 Transformer 块，并在 logit 层面显式对齐全精度模型与量化模型的输出分布。
- **关键技术细节**：
  - 在量化最后一个 Transformer 块时，引入 **logit 感知目标**。
  - 通过**最小化全精度模型 logits 与量化模型 logits 之间的交叉熵（cross-entropy）**，实现 token 概率分布的对齐。
  - 该方法覆盖了传统逐块 PTQ 忽略的 LM head 影响，弥补了 MSE 目标无法捕捉概率分布差异的不足。
- **流程描述**：不需要完整的算法伪代码，其核心是在现有 block-wise PTQ 流水线中，将最终块的量化损失从 MSE 替换/增加为 logit 级别的交叉熵损失，从而引导量化后的最终块输出概率与全精度模型保持一致。

## 3. 实验设计

- **评测场景**：包括复杂文本生成任务、长回复生成、长思维链场景，以及基础的语言建模与理解任务。
- **Benchmark**：摘要中未具体列出数据集名称（如 MMLU、GSM8K、AlpacaEval 等），仅笼统提到“text generation benchmarks”。
- **对比方法**：
  - 与**最先进的逐块 PTQ 方法**（SOTA block-wise PTQ）对比；
  - 以**全精度（FP）模型**作为性能上界参照；
  - 覆盖**多种模型家族（diverse model families）**，但未明确模型名称。

## 4. 资源与算力

- 提供的摘要中**未明确说明**实验使用的 GPU 型号、数量、训练/量化时长等算力资源信息。
- 也未提及量化过程的时间开销或内存占用变化。

## 5. 实验数量与充分性

- 摘要仅给出了概括性结论，缺乏具体实验数量、数据集明细、消融实验等细节。
- 根据现有信息，实验至少覆盖：
  - 多模型家族下的生成任务评估；
  - 语言建模与理解任务验证；
  - 与 SOTA 方法的对比。
- **充分性评价**：由于缺少关键细节（如每个数据集上的具体数字、方差、消融、不同比特宽度的测试），无法从摘要层面充分判断实验的全面性与公平性；但研究范围横跨生成与理解任务，并跨多个模型家族，初步具备一定广度。

## 6. 主要结论与发现

- LFQ 能够**持续提升低比特量化 LLM 在复杂生成任务上的准确性**，尤其改善长回答和长思维链场景下的退化问题。
- 相比现有 SOTA 逐块 PTQ，LFQ 在不同模型家族上均有一致性提升。
- 同时，LFQ **不会损害**语言建模和理解任务上的性能，与全精度基线保持相当水平。
- 验证了“在最终块进行 logit 级交叉熵对齐”这一思路的有效性，填补了逐块 PTQ 在生成任务上的短板。

## 7. 优点

- **方法简洁有效**：作为对现有 block-wise PTQ 的“即插即用”增强，实现成本低，易于集成。
- **针对性明确**：直接针对生成任务退化的两大根因（忽略 LM head、MSE 目标不合适）提出改进，逻辑清晰。
- **实验覆盖较广**：涉及多种模型家族，并同时评估生成与理解两类任务，结论有一定普适性。
- **保持稳健**：在提升生成质量的同时，不牺牲基础语言建模能力，满足部署场景的综合需求。

## 8. 不足与局限

- **信息不透明**：摘要中未提供具体数据集、模型规模、量化比特数（如 W4/W3/W2）等关键实验细节，导致可复现性和可验证性受限。
- **适用边界未知**：是否对极低比特（如 2-bit）或极大参数模型同样有效，尚不明确。
- **缺少资源与效率分析**：未讨论 LFQ 带来的额外计算开销、量化时间成本以及对端侧部署的影响。
- **未提及失败案例**：没有说明在哪些场景下 LFQ 可能不适用或提升有限。
- **局限性声明缺失**：从提供的摘要无法看到作者对潜在偏差或限制的讨论。

（完）
