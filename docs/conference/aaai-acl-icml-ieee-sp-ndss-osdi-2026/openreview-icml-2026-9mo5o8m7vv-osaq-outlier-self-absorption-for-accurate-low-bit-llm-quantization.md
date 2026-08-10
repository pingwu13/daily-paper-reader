---
title: "OSAQ: Outlier Self-Absorption for Accurate Low-bit LLM Quantization"
title_zh: OSAQ：面向精确低比特大语言模型量化的离群值自吸收
authors: "Zhikai Li, Zhen Dong, Xuewen Liu, Jing Zhang, Qingyi Gu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/27206182571b9d469a9198bc1d13079cbca95977.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 通过二阶低秩加性权重抑制处理低比特 LLM 量化中的系统性离群值
tldr: 大语言模型推理中的高内存与延迟可通过仅权重量化缓解，但权重中系统性离群值使低比特量化性能远未令人满意，已有缩放、旋转等方法作用有限。OSAQ 提出离群值自吸收量化，利用二阶低秩加性权重抑制，主动消除离群值对量化的干扰。该方法在保持极低比特压缩的前提下显著降低量化误差。实验表明其准确性和鲁棒性优于现有低比特量化方案，为低比特 LLM 部署提供了新的离群值处理思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 权重系统性离群值导致低比特量化性能不佳，现有缩放和旋转方法难以满足精度需求。
method: 提出 OSAQ，通过二阶低秩加性权重抑制吸收离群值，提升低比特仅权重量化精度。
result: 实验显示 OSAQ 在低比特 LLM 上量化误差显著降低，准确率优于以往离群值处理方法。
conclusion: OSAQ 为低比特 LLM 量化提供了高效可靠的离群值自吸收机制。
---

## Abstract
Large Language Models (LLMs) have demonstrated remarkable capabilities in understanding and generation tasks. However, their massive parameter scale leads to significant resource consumption and latency during inference. Post-training weight-only quantization offers a promising solution by reducing model size and accelerating token generation through alleviating the memory-bound issue. Nevertheless, there are inherent systematic outliers in weights, and although some efforts have attempted to address them, such as scaling and rotation, the performance of low-bit quantization remains far from satisfactory. In this paper, we propose Outlier Self-Absorption Quantization (OSAQ), which performs second-order low-rank derived additive weight suppression for low-bit weight-only LLM quantization. Specifically, we observe that Hessian exhibits low-rank consistency across different inputs, with certain directions persistently lacking strength. Leveraging this property, we construct an additive weight transformation based on the Hessian’s null space, thereby suppressing weight outliers without affecting the task loss. This additive transformation can be absorbed into the weights offline, requiring no inter-layer transformations and introducing no inference overhead. Moreover, the construction is efficiently achieved by a closed-form solution, without resource-intensive training or iterative procedures. Extensive experiments across models of varying scales and tasks are conducted, and the results show that OSAQ effectively suppresses outliers and improves low-bit quantization performance.

---

## 论文详细总结（自动生成）

# OSAQ：面向精确低比特大语言模型量化的离群值自吸收

## 1. 核心问题与整体含义

- **背景与动机**：大语言模型（LLM）参数规模庞大，导致推理时资源消耗和延迟很高。后训练仅权重量化（Post-training Weight-only Quantization）是一种有效手段，可减小模型体积并缓解内存瓶颈、加速生成。但是，权重中存在的**系统性离群值**使低比特量化性能远未达到理想水平。
- **现有方法的不足**：已有工作尝试通过缩放（scaling）或旋转（rotation）等策略处理离群值，但效果有限。
- **论文核心问题**：如何在不引入推理开销、不依赖训练的情况下，主动消除权重离群值对低比特量化的干扰？
- **整体含义**：本文提出 OSAQ，利用权重 Hessian 矩阵的低秩特性，通过**二阶低秩加性权重抑制**吸收离群值，在极低比特压缩下显著降低量化误差，为低比特 LLM 部署提供了新的离群值处理思路。

## 2. 论文提出的方法论

- **核心思想**：**离群值自吸收**（Outlier Self-Absorption）。通过观测发现，Hessian 矩阵在不同输入下呈现**低秩一致性**，某些方向持续缺乏强度（即 Hessian 的零空间方向）。利用这一性质，可以构造一种加性权重变换，在保持任务损失不变的前提下，抑制权重离群值。
- **关键技术细节**：
  1. 基于 Hessian 的**零空间**（null space）构造加性变换，使变换后的权重在关键方向上不变，而离群值方向被抑制。
  2. 该加性变换可在**离线阶段**直接吸收进权重中，不需要跨层变换，因此**推理阶段零额外开销**。
  3. 构造过程存在**闭式解**（closed-form solution），无需昂贵的训练或迭代优化，计算效率高。
- **方法名称**：Outlier Self-Absorption Quantization（OSAQ），即离群值自吸收量化。
- 整体流程可概括为：
  - 计算权重的 Hessian 信息 → 提取低秩零空间 → 构造加性抑制矩阵 → 离线合并到权重中 → 执行标准低比特量化。

## 3. 实验设计

- **实验范围**：根据摘要，论文在**多种不同规模模型**和**多种任务**上开展了“大量实验”（Extensive experiments），用于验证 OSAQ 的有效性。
- **Benchmark**：摘要中**未给出具体数据集名称**（例如 WikiText、PTB、MMLU 等）和任务类型，也没有给出基线方法的具体列表。
- **对比方法**：从上下文推断，可能对比了现有离群值处理方法（如缩放和旋转类方法）以及标准低比特量化方案，但论文文本中未列出具体名称。
- **评估指标**：一般是困惑度（perplexity）、零样本任务准确率等，但摘要中未给出具体数值。

## 4. 资源与算力

- **未明确说明**：论文提供的材料中**没有提及**使用了何种 GPU 型号、GPU 数量、训练/推理时长或总算力消耗。
- 因此，无法评估方法的资源开销与可复现性。

## 5. 实验数量与充分性

- **实验数量**：只提到“Extensive experiments”，没有说明具体实验组数、消融实验数量或覆盖的模型架构/参数规模范围。
- **充分性判断**：从摘要文字看，实验覆盖了多样化的模型和任务，具备一定广度，但**细节缺失**，无法判断是否进行了充分的消融（如对零空间秩的影响、不同比特宽度的敏感性、不同离群值强度的鲁棒性等）。
- **客观与公平性**：缺少基线细节和随机种子、超参数等说明，因此无法从现有信息判断实验是否完全客观公平。

## 6. 论文的主要结论与发现

- **结论**：OSAQ 能够有效抑制权重中的系统性离群值，显著提升低比特仅权重量化的性能。
- **发现**：
  - Hessian 矩阵在低比特量化场景下存在低秩一致性，且具有可利用的零空间；
  - 基于该零空间的加性权重变换，可以在不损害任务损失的前提下吸收离群值；
  - 所提方法不引入推理开销，也不需要训练或迭代过程，实际部署友好。

## 7. 优点

- **方法论亮点**：
  - 使用**二阶信息（Hessian）**，对离群值的刻画更本质，优于简单的缩放或旋转；
  - **离线可吸收**的加性变换，无层间依赖，推理零开销；
  - **闭式解**构造，避免了昂贵的训练和迭代，计算高效；
  - 针对性解决了“仅权重量化”中权重离群值这一核心痛点。
- **部署价值**：在保持极低比特压缩率的同时提升精度，有望直接用于资源受限场景下的 LLM 推理。

## 8. 不足与局限

- **信息不完整**：论文元数据和摘要未提供具体实验细节（数据集、基线、超参、消融、算力等），难以全面评估其有效性和可复现性。
- **假设局限性**：方法依赖 Hessian 低秩一致性与零空间的存在性，虽观察成立，但**不同模型/任务/位宽下的普适性**仍需更多验证。
- **潜在风险**：加性权重变换虽然保持任务损失不变，但可能对某些敏感权重或特殊结构（如超低比特下的噪声放大）不够鲁棒，未来需进一步分析。
- **应用限制**：目前主要针对仅权重量化（weight-only）场景，对于同时量化权重和激活的全量化（weight-activation quantization）问题，可能需额外处理。
- **实验覆盖不足**：缺少与当前主流 SOTA 方法的完整对比，也缺少对极端低比特（如 2-bit 或以下）的详细性能分析。

（完）
