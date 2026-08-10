---
title: "NeUQI: Near-Optimal Uniform Quantization Parameter Initialization for Low-Bit LLMs"
title_zh: NeUQI：低比特大语言模型的近最优均匀量化参数初始化
authors: "Li Lin, Xinyu Hu, Xiaojun Wan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/5a831983fb7aafce84904a1a6c96258e67fdc44c.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 为低比特 LLM 均匀量化提供近最优参数初始化，以保持下游任务性能
tldr: 大语言模型在消费级 GPU 和端侧设备上部署受限于高内存开销和推理延迟，均匀量化的后训练量化是高效方案。NeUQI 关注量化参数初始化这一关键环节，提出接近最优的初始化策略，以提升低比特均匀量化的最终性能。实验表明该方法能有效降低部署成本和延迟，同时保持模型在语言建模与下游任务上的准确率。这项工作为均匀量化 LLM 的低比特部署提供了可靠且易用的初始化方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 低比特均匀量化 LLM 的部署受限于参数初始化次优，导致量化后性能下降明显。
method: 提出近最优的量化参数初始化方法，为低比特均匀量化提供更好的起点以提升 PTQ 性能。
result: 实验证明该方法在低比特 LLM 上降低了内存与解码延迟，且保持了模型性能。
conclusion: NeUQI 提供了即插即用的初始化策略，可提升现有均匀量化 PTQ 在下游任务中的表现。
---

## Abstract
Large language models (LLMs) achieve impressive performance across domains but face significant challenges when deployed on consumer-grade GPUs or personal devices such as laptops, due to high memory consumption and inference costs. Post-training quantization (PTQ) of LLMs offers a promising solution that reduces their memory footprint and decoding latency. In practice, PTQ with uniform quantization representation is favored due to its efficiency and ease of deployment, as uniform quantization is widely supported by mainstream hardware and software libraries. Recent studies on low-bit uniform quantization have led to noticeable improvements in post-quantization model performance; however, they mainly focus on quantization methodologies, while the initialization of quantization parameters remains underexplored and still relies on the conventional *Min-Max formula*. In this work, we identify the limitations of the *Min-Max formula*, move beyond its constraints, and propose **NeUQI**, a method that efficiently determines near-optimal initialization for uniform quantization. Our NeUQI simplifies the joint optimization of the scale and zero-point by deriving the zero-point for a given scale, thereby reducing the problem to a scale-only optimization. Benefiting from the improved quantization parameters, our NeUQI consistently outperforms existing methods in the experiments with the LLaMA and Qwen families on various settings and tasks. Furthermore, when combined with a lightweight distillation strategy, NeUQI even achieves superior performance to PV-tuning, a considerably more resource-intensive method.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **背景**：大语言模型（LLM）虽然在各领域表现优异，但部署在消费级 GPU 或笔记本电脑等个人设备上时，面临着高内存占用和高推理延迟的挑战。
- **现有方案**：后训练量化（PTQ）是降低内存占用和推理延迟的有效手段。其中，均匀量化（uniform quantization）由于被主流硬件和软件库广泛支持，在实际部署中尤其受到青睐。
- **被忽视的问题**：已有的低比特均匀量化研究主要关注量化方法本身（如如何调整权重、激活值的表示），却很少关注量化参数——缩放因子（scale）和零点（zero-point）的初始化。传统方法仍然使用简单的 Min-Max 公式来确定这些参数，而该初始化方式存在明显局限，可能导致量化后模型性能下降。
- **论文目标**：提出 NeUQI（Near-Optimal Uniform Quantization Parameter Initialization），为低比特均匀量化提供接近最优的初始化策略，从而提升 PTQ 后的模型性能，同时保持部署效率。

## 2. 论文提出的方法论

- **核心思想**：超越传统 Min-Max 公式，高效地为均匀量化确定接近最优的 scale 和 zero-point 初始化值。
- **关键技术细节**：
  - 将 scale 和 zero-point 的联合优化问题简化为仅对 scale 进行优化。
  - 具体做法是：对于给定的 scale，zero-point 可以通过解析方式求出，从而把二维的联合优化降维成一维的 scale 搜索。
  - 这种简化显著降低了优化复杂度，使得初始化过程高效易用。
- **算法流程（文字说明）**：
  1. 对于待量化的权重或激活张量，不再直接采用 min/max 极值计算 scale。
  2. 构造一个关于 scale 的优化目标（可能基于量化误差或对输出分布的影响）。
  3. 在该目标下搜索使量化误差最小的 scale。
  4. 由最优 scale 通过闭式推导得到对应的 zero-point。
  5. 使用得到的 scale 和 zero-point 进行后续均匀量化 PTQ。
- **即插即用**：NeUQI 可作为现有均匀量化 PTQ 方法的初始化模块，直接替换原有的 Min-Max 初始化，无需改动其他量化流程。

## 3. 实验设计

- **模型族**：使用 LLaMA 和 Qwen 系列模型进行实验，覆盖多种模型规模和设置。
- **任务/场景**：涉及多种任务和设置，包括语言建模与下游任务；同时评估量化的实际部署收益（内存占用与解码延迟）。
- **对比方法**：
  - 与现有的低比特均匀量化方法对比，NeUQI 因改进的初始化参数而表现更优。
  - 与 PV-tuning（一个资源消耗明显更高的方法）对比：NeUQI 在结合轻量蒸馏策略后，甚至能取得优于 PV-tuning 的性能。
- **数据集**：摘要中未明确列出具体数据集名称，只说明是“various settings and tasks”。

## 4. 资源与算力

- 摘要和元数据中**未明确说明**使用了多少 GPU 型号、数量或训练/推理时长。
- 仅能从文中推断：NeUQI 本身是针对 PTQ 的初始化步骤，开销较低；而与其对比的 PV-tuning 被认为是“considerably more resource-intensive”，说明 NeUQI 在算力需求上具有明显优势。
- 具体数值（如 GPU 小时数、硬件规格）无法从现有信息中获取，属于缺失信息。

## 5. 实验数量与充分性

- **实验数量**：摘要中未给出具体实验组数、数据集数量或消融实验细节。仅描述了跨 LLaMA / Qwen 两大模型族、多种设置和任务的对比实验，以及额外结合轻量蒸馏的增强实验。
- **充分性分析**：
  - 多模型族、多任务的实验设计具有较好的泛化性基础，符合主流量化论文的验证惯例。
  - 但缺少具体的数据集列表、任务名称、量化位宽（如 2bit/3bit/4bit）的划分、以及消融实验（如 scale-only 优化的有效性分析），因此很难完全判断实验的覆盖广度与公平性。
  - 由于摘要只提供了“consistently outperforms”的定性结论，没有误差棒或显著性检验信息，客观性证据不足。

## 6. 论文的主要结论与发现

- 传统 Min-Max 初始化是低比特均匀量化性能受限的一个关键原因，NeUQI 提供了更好的替代方案。
- 通过仅优化 scale、解析推导 zero-point，NeUQI 能高效地找到近最优初始化参数。
- 在 LLaMA 和 Qwen 系列模型上，NeUQI 始终优于现有 PTQ 方法，同时降低内存占用和解码延迟。
- 与轻量蒸馏结合时，NeUQI 的性能甚至超过了资源密集型的 PV-tuning 方法，展示了良好的成本-性能比。

## 7. 优点

- **角度新颖**：聚焦于量化参数初始化这一被普遍忽视的环节，为低比特均匀量化研究提供了新视角。
- **数学简化**：将二维联合优化降为一维 scale 优化，并通过推导得到 zero-point，方法简洁、计算开销低。
- **即插即用**：可作为现有 PTQ 流程的零侵入组件，易集成、易部署。
- **部署友好**：在提升性能的同时明显降低内存和延迟，符合边缘端/消费级硬件需求。
- **资源高效**：相比 PV-tuning 等资源密集型方法，NeUQI 使用轻量蒸馏即可达到或超越其效果，说明其实际训练成本更低。

## 8. 不足与局限

- **信息公开不足**：论文摘要未提供实验细节（具体数据集、任务、位宽、baseline 列表、硬件配置等），导致无法从当前信息中充分复现或验证其结论。
- **模型覆盖有限**：仅涉及 LLaMA 和 Qwen 两个系列，未说明是否适用于其他主流 LLM 架构（如 GPT-NeoX、Mistral 等）。
- **未讨论激活/权重量化的区分**：缺少关于该方法用于权重量化与激活量化时的具体效果差异的讨论。
- **没有消融和鲁棒性分析**：未证明 scale-only 优化的理论依据、不同搜索策略的敏感性，以及不同数据分布下的稳定性。
- **对比基线有限**：仅提到和现有方法以及 PV-tuning 对比，未与其他高级初始化方法（如基于 Hessian 的初始化、可学习缩放等）进行比较。
- **潜在偏差风险**：摘要强调“consistently outperforms”，但未展示最差情况或失败案例，可能存在选择性报告。

（完）
