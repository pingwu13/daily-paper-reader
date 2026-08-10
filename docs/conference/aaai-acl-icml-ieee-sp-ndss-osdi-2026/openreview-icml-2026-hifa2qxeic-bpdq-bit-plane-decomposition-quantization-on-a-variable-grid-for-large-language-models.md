---
title: "BPDQ: Bit-Plane Decomposition Quantization on a Variable Grid for Large Language Models"
title_zh: BPDQ：可变网格上的位平面分解大语言模型量化
authors: "Junyu Chen, Jungang Li, Jing Xiong, Wenjie Wang, Qingyao Yang, He Xiao, Zhen Li, Taiqiang Wu, Mengzhao Chen, Zhen Peng, Chaofan Tao, Long Shi, Hongxia Yang, Ngai Wong"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1dd5925af57c4c2dac081fc597dc929b6dcc35a2.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 通过位平面分解与可变网格提升2-3比特LLM权重PTQ精度
tldr: 针对LLM后训练量化在2-3比特时精度急剧下降的问题，论文提出位平面分解量化（BPDQ）。它通过位平面与标量系数构造可变量化网格，并利用二阶信息迭代修正，逐步补偿量化误差，在2-3比特区间显著优于固定均匀网格方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: PTQ在4比特表现良好，但2-3比特时固定均匀量化网格限制了误差最小化能力。
method: 用位平面和标量系数构造可变网格，结合二阶信息迭代细化并补偿量化误差。
result: 在2-3比特LLM量化中相比传统网格大幅提高精度。
conclusion: 形状可变网格为实现超低比特LLM量化提供了新思路。
---

## Abstract
Large language model inference is often bounded by memory footprint and bandwidth in resource-constrained deployments, making quantization fundamental to efficient serving. 
While post-training quantization (PTQ) maintains high fidelity at 4-bit, it deteriorates at 2-3 bits. 
In essence, existing methods enforce a shape-invariant quantization grid (e.g., the fixed uniform intervals of UINT2) for each group, severely restricting the feasible set for error minimization. 
To address this, we propose Bit-Plane Decomposition Quantization (BPDQ), which constructs a variable quantization grid via bit-planes and scalar coefficients, and iteratively refines them using second-order information while progressively compensating for quantization errors to minimize output discrepancy.
In the 2-bit regime, BPDQ enables serving Qwen2.5-72B on a single RTX 3090 with 83.85\% GSM8K accuracy (vs. 90.83\% at 16-bit).
Moreover, we theoretically show that the variable grid expands the feasible set, and that the quantization process consistently aligns with the optimization objective in Hessian-induced geometry.
The code is available at github.com/KingdalfGoodman/BPDQ.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **背景与动机**：大型语言模型（LLM）在资源受限的设备上部署时，推理性能和内存带宽往往成为瓶颈，量化是提升服务效率的关键技术。后训练量化（PTQ）在 4-bit 精度下能保持较高保真度，但在 2-3 bit 超低位宽下精度急剧下降。
- **核心问题**：现有 PTQ 方法对每个量化分组强制使用形状固定的量化网格（如 UINT2 的固定均匀间隔），这严重限制了误差最小化过程中的可行解空间，导致超低位宽下难以逼近原始模型输出。
- **整体含义**：论文试图打破固定网格的限制，通过“形状可变”的量化网格为超低位宽 LLM 量化提供新思路，从而在极低比特下实现可观精度与推理效率的平衡。

## 2. 提出的方法论

- **核心思想**：提出 **位平面分解量化（BPDQ, Bit-Plane Decomposition Quantization）**，用位平面（bit-planes）和标量系数构造 **可变量化网格**，替代传统固定均匀网格。
- **关键技术细节**：
  - 量化网格由多个位平面配合标量系数组合而成，通过改变这些系数/位平面的组合方式，使网格形状可变、能更灵活地贴合数据分布。
  - 利用 **二阶信息（Hessian）** 对网格参数进行迭代细化，同时逐步补偿先前量化造成的误差，以最小化输出差异。
  - 理论上证明：可变网格扩展了可行解集合；量化过程在 Hessian 诱导的几何空间中始终围绕优化目标更新，保证迭代方向一致。
- **算法流程（文字描述）**：
  1. 将权重/激活按位平面分解；
  2. 初始化标量系数，得到初始可变量化网格；
  3. 利用二阶信息迭代调整网格参数，量化后计算误差；
  4. 对误差进行补偿并更新下一轮量化，直到收敛或达到目标比特数。

## 3. 实验设计

- **场景/数据集**：摘要中明确提及 **GSM8K**（数学推理任务），以及 **Qwen2.5-72B** 模型。
- **Benchmark 与对比**：
  - 在 **2-bit** 量化设置下，BPDQ 使 Qwen2.5-72B 在单张 RTX 3090 上运行，GSM8K 准确率达到 **83.85%**，与 16-bit 全精度的 **90.83%** 对比。
  - 摘要未列出完整对比方法清单，但可推断其与传统固定网格量化（如 UINT2 均匀量化）及可能的其他 PTQ 方法进行比较。
- **实验覆盖**：目前可见的实验仅包含一个模型、一个数据集、一个比特位（2-bit）；标题声称面向 2-3 bit，但未在摘要中呈现 3-bit 结果。

## 4. 资源与算力

- **明确提及**：推理端使用 **单张 RTX 3090**，成功运行 Qwen2.5-72B。
- **未说明**：论文摘要中 **没有提及训练/量化过程所需的 GPU 数量、类型、训练时长、能耗等算力信息**，也未说明对比方法的资源消耗。因此无法基于现有文本评估整体计算成本。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，仅报道了一组端到端结果（Qwen2.5-72B + GSM8K + 2-bit），未包含多模型、多任务、多比特率或消融实验的细节。
- **充分性与客观性**：
  - 现有证据不足以全面验证方法的鲁棒性和泛化能力；
  - 缺少与同类 SOTA 方法的系统性公平对比（如不同校准集、不同模型族）；
  - 没有消融研究来分离“位平面分解”“可变网格”“二阶信息迭代”各组件贡献；
  - 因此，实验数量有限，目前只能视为初步验证，需更多证据支持。

## 6. 主要结论与发现

- BPDQ 在 2-bit 量化下显著优于传统固定均匀网格方法，使 72B 级模型能在消费级 GPU 上以可接受的精度运行。
- 理论分析表明：可变网格扩大了优化可行集，且迭代过程与 Hessian 几何下的优化目标保持一致，为超低位宽量化提供了理论支撑。
- 总体结论：**形状可变的量化网格是实现超低比特 LLM 量化的有效新方向**。

## 7. 优点

- **方法创新性**：从“固定均匀网格”转向“位平面 + 标量系数”的可变网格，突破了传统量化网格的形状限制，是量化空间设计的新思路。
- **理论分析**：同时给出可行集扩展和 Hessian 几何一致性的理论论证，增加了方法可信度。
- **实用潜力**：2-bit 下将 Qwen2.5-72B 部署到单张 RTX 3090，展示了显著的内存/带宽优势和实际应用价值。
- **可复现性**：论文提供代码仓库（github.com/KingdalfGoodman/BPDQ），有利于后续验证与扩展。

## 8. 不足与局限

- **摘要信息不完整**：全文细节（算法伪代码、定理证明、完整实验表）未在现有文本中呈现，无法深入评估。
- **实验覆盖不足**：未展示多个模型（如不同规模、不同架构）、多个任务（问答、生成、分类等）以及 3-bit 的结果，难以确认方法的普适性。
- **对比不够透明**：未列出具体对比方法、超参数设置和资源消耗，存在选择性报告的可能。
- **算力未知**：缺少量化过程本身的训练/调优开销，可能影响实际部署成本评估。
- **潜在偏差风险**：单一数据集（GSM8K）上的准确率可能高估数学推理能力，且未报告方差、多次运行稳定性等指标。

（完）
