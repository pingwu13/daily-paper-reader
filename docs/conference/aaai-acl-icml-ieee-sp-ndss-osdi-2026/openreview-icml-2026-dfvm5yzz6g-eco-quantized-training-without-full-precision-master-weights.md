---
title: "ECO: Quantized Training without Full-Precision Master Weights"
title_zh: ECO：无需全精度主权重的量化训练
authors: "Mahdi Nikdan, Amir Zandieh, Dan Alistarh, Vahab Mirrokni"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3954f06cbb4a2080139c90408a409f051f0848e9.pdf"
tags: ["query:ulbv"]
score: 4.0
evidence: 面向LLM的量化训练方法，但非面向推理的低比特权重量化
tldr: 针对现有LLM量化训练仍需高精度主权重、带来额外显存开销的问题，ECO提出误差补偿优化器，直接对量化参数应用更新并将量化误差注入优化器动量，从而取消主权重。该方法尤其有利于稀疏混合专家模型，显著降低参数与优化器状态的内存占用，为大规模LLM量化训练提供更高效方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有量化训练依赖全精度主权重保存更新，显存开销大，尤其在稀疏混合专家模型中更为突出。
method: 提出误差补偿优化器，在每步更新后量化权重，并将量化误差注入动量，实现无主权重的量化训练。
result: 实验表明ECO在保持训练精度的同时消除主权重存储，降低LLM与SMoE模型训练显存开销。
conclusion: ECO推动量化训练摆脱全精度缓冲依赖，为大规模模型训练节省显存提供新思路。
---

## Abstract
Quantization has significantly improved the compute and memory efficiency of Large Language Model (LLM) training. However, existing approaches still rely on accumulating their updates in high-precision: concretely, gradient updates must be applied to a high-precision weight buffer, known as $\textit{master weights}$. This buffer introduces substantial memory overhead, particularly for Sparse Mixture of Experts (SMoE) models, where model parameters and optimizer states dominate memory usage. To address this, we introduce the Error-Compensating Optimizer (ECO), which eliminates master weights by applying updates directly to quantized parameters. ECO quantizes weights after each step and carefully injects the resulting quantization error into the optimizer momentum, forming an error-feedback loop with no additional memory. We prove that, under standard assumptions and a decaying learning rate, ECO converges to a constant-radius neighborhood of the optimum, while naive master-weight removal can incur an error that is inversely proportional to the learning rate. We show empirical results for pretraining small Transformers (30--800M), a Gemma-3 1B model, and a 2.1B parameter Sparse MoE model with FP8 quantization, and fine-tuning DeepSeek-MoE-16B in INT4 precision. Throughout, ECO matches baselines with master weights up to near-lossless accuracy, significantly shifting the static memory vs validation loss Pareto frontier.

---

## 论文详细总结（自动生成）

# 论文总结：ECO——无需全精度主权重的量化训练

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：量化技术已显著提升大语言模型（LLM）训练的计算和内存效率，是当前大规模模型训练的重要优化方向。
- **核心问题**：现有量化训练方法仍需要将梯度更新累积在高精度的“主权重”（master weights）缓冲中，这一缓冲带来大量额外显存开销。尤其在稀疏混合专家（SMoE）模型中，模型参数和优化器状态占据主导内存，主权重的开销变得更加突出。
- **研究含义**：论文提出一种新的训练范式，目标是完全消除训练过程中的全精度主权重依赖，从而进一步压缩显存占用，使大规模量化训练更加高效。

## 2. 方法论：误差补偿优化器（ECO）
- **核心思想**：取消高精度主权重，将优化器更新直接作用于量化参数，并通过误差反馈机制补偿由量化引入的误差。
- **关键技术细节**：
  - 每一步优化后，对权重进行量化；
  - 将量化产生的误差（即量化前后权重之差）精心注入到优化器的动量中，形成无需额外内存的误差反馈回路（error-feedback loop）；
  - 这样既保留历史梯度信息，又避免了单独存储高精度主权重。
- **理论保证**：在标准假设和衰减学习率下，论文证明 ECO 能够收敛到最优解附近的一个恒定半径邻域；而朴素地移除主权重会导致误差与学习率成反比，即难以稳定收敛。
- **总体流程（文字说明）**：前向/反向计算 → 计算梯度 → 更新量化权重（直接应用更新） → 量化权重 → 计算量化误差 → 将误差注入动量 → 进行下一步迭代。

## 3. 实验设计
- **预训练场景**：
  - 小型 Transformer 模型，参数量 30M 至 800M；
  - Gemma-3 1B 模型；
  - 2.1B 参数规模的稀疏 MoE 模型，使用 FP8 量化。
- **微调场景**：
  - DeepSeek-MoE-16B 模型，使用 INT4 精度。
- **对比方法**：与保留主权重的基线方法（baseline with master weights）进行对比。
- **评测指标**：主要关注训练/微调精度（验证损失）以及静态显存占用，并报告静态显存 vs 验证损失的 Pareto 前沿。

## 4. 资源与算力
- 论文提供的文本中并未明确说明使用的 GPU 型号、数量或具体训练时长，因此无法从摘要中确认算力配置细节。

## 5. 实验数量与充分性
- 论文至少开展了 4 组主要实验，覆盖不同模型规模（30M 到 16B）、不同架构（稠密 Transformer 和稀疏 MoE）以及不同量化精度（FP8 和 INT4），并涵盖预训练与微调两类任务。
- **充分性评价**：实验覆盖了从极小到较大规模模型的多个量级，且同时验证了 ECO 在稠密和稀疏模型上的适用性，整体设置较全面；但摘要中未提及具体的消融实验细节（如误差注入机制的变体比较），因此从摘要角度看，消融的完整程度尚不明确。
- **公平性**：所有对比均以保留主权重的基线为参照，这为评估 ECO 的有效性提供了合理的对照基准。

## 6. 主要结论与发现
- ECO 在保持接近无损（near-lossless）训练精度的前提下，彻底消除了主权重的存储需求。
- 与保留主权重的基线相比，ECO 显著改善了静态显存与验证损失之间的 Pareto 前沿，即在相同显存预算下获得更低的验证损失，或在相同精度下占用更少显存。
- 这一优势在稀疏 MoE 模型中尤为突出，因为此类模型的参数和优化器状态在内存中占比更高。

## 7. 优点
- **创新性强**：首次提出面向 LLM 量化的无主权重重训练方法，打破了现有量化训练对高精度缓冲的依赖。
- **理论支撑扎实**：给出了收敛性证明，并指出了朴素去主权重方法的理论缺陷，增强了方法的可信度。
- **零额外内存开销**：误差反馈回路复用优化器动量，无需额外存储，设计优雅。
- **应用范围广**：在稠密与稀疏模型、不同参数量级、不同量化精度以及预训练和微调场景中均得到验证。
- **实践价值高**：对显存资源受限的大规模模型训练场景具有直接的实际意义。

## 8. 不足与局限
- **算力信息缺失**：论文未报告 GPU 型号、数量和训练时长，这为复现和横向比较带来一定困难。
- **实验细节有限**：摘要中未展示消融实验和更细粒度的参数分析（如误差注入比例、学习率调参等），削弱了对方法鲁棒性的充分验证。
- **任务覆盖有限**：实验主要涉及语言模型的预训练和微调，未涵盖其他模态（如视觉或多模态）或更大规模（如千亿级）模型的验证。
- **精度覆盖不足**：虽测试了 FP8 和 INT4，但未涉及更低的比特宽度（如 INT2/INT3 或二值化），对于极端量化场景的有效性仍未知。
- **长期训练表现未知**：论文的收敛性证明基于衰减学习率假设，但在实际长时间大规模训练中是否稳定、是否会出现误差累积，仍需进一步验证。

（完）
