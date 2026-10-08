---
title: "FM4NPP: A Scaling Foundation Model for Nuclear and Particle Physics"
title_zh: FM4NPP：面向核与粒子物理的可扩展基础模型
authors: "David Keetae Park, Shuhang Li, Yi Huang, Xihaier Luo, Haiwang Yu, Yeonju Go, Christopher Pinkenburg, Yuewei Lin, Shinjae Yoo, Joseph D. Osborn, Jin Huang, Yihui Ren"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qaI3cLFsiX"
tags: ["query:nuc-phys-ai"]
score: 5.0
evidence: 面向核与粒子物理探测器数据的可扩展基础模型
tldr: 大语言模型启发了科学基础模型的发展，但粒子物理探测器数据稀疏且空间分布特殊，与自然语言差异巨大，难以直接套用。本文构建含1100万以上粒子碰撞事件的数据集与配套下游任务，提出面向探测器数据的自监督训练方法。实验验证了该基础模型的神经可扩展性与跨任务泛化能力，为核与粒子物理的统一建模与探测器数据分析奠定基础。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 科学基础模型难以直接应用于稀疏、空间分布特殊的粒子物理探测器数据。
method: 构建超1100万粒子碰撞事件数据集与下游任务，并提出面向探测器数据的自监督训练方法。
result: 实验验证了该基础模型的神经可扩展性与跨任务泛化能力。
conclusion: 为核与粒子物理的探测器数据分析提供了可扩展的统一基础模型范式。
---

## Abstract
Large language models have revolutionized artificial intelligence by enabling large, generalizable models trained through self-supervision. This paradigm has inspired the development of scientific foundation models (FMs). However, applying this capability to experimental particle physics is challenging due to the sparse, spatially distributed nature of detector data, which differs dramatically from natural language. This work addresses if an FM for particle physics can scale and generalize across diverse tasks. We introduce a new dataset with more than 11 million particle collision events and a suite of downstream tasks and labeled data for evaluation. We propose a novel self-supervised training method for detector data and demonstrate its neural scalability with models that feature up to 188 million parameters. With frozen weights and task-specific adapters, this FM consistently outperforms baseline models across all downstream tasks. The performance also exhibits robust data-efficient adaptation. Further analysis reveals that the representations extracted by the FM are task-agnostic but can be specialized via a single linear mapping for different downstream tasks.

---

## 论文详细总结（自动生成）

# FM4NPP 论文总结

> 说明：所提供的 PDF 提取文本未包含论文正文，仅包含标题、元数据与 Abstract。因此以下总结主要基于摘要信息；涉及具体公式、实验细节、算力与完整实验组数时，只能指出“文中未明确说明/无法从现有文本确认”。

## 1. 核心问题与整体含义
- **研究动机**：大语言模型通过自监督学习展现出强通用性与可扩展性，这一范式启发了科学基础模型的发展。
- **核心问题**：能否将基础模型范式有效迁移到实验粒子物理，尤其是核与粒子物理探测器数据？
- **关键挑战**：探测器数据具有稀疏性、空间分布特殊，与自然语言的结构差异巨大，难以直接套用语言模型方法。
- **整体含义**：论文试图构建一个面向核与粒子物理的可扩展基础模型，实现跨任务泛化与统一建模，为探测器数据分析提供新范式。

## 2. 方法论
- **核心思想**：构建大规模粒子碰撞事件数据集与下游任务体系，并提出面向探测器数据的自监督训练方法，训练可扩展的物理基础模型。
- **关键技术细节**：
  - 使用自监督学习预训练探测器数据表示。
  - 模型规模最高达到 **1.88 亿参数**，验证神经可扩展性。
  - 下游任务采用 **冻结权重 + 任务特定 adapter** 的适配方式。
  - 分析表明，模型提取的表征是 **任务无关的**，但可通过 **单一线性映射** 针对不同下游任务进行特化。
- **算法流程（文字概括）**：
  1. 收集/构建大规模粒子碰撞事件数据；
  2. 对探测器数据进行自监督预训练；
  3. 得到通用基础模型 backbone；
  4. 冻结主干权重，接入任务特定 adapter；
  5. 在多个下游任务上评估泛化、数据效率与可扩展性；
  6. 通过线性映射分析表征的任务无关性与可特化性。
- **公式说明**：现有摘要未给出具体损失函数、网络结构或数学公式，无法进一步展开。

## 3. 实验设计
- **数据集**：
  - 构建了包含 **超过 1100 万粒子碰撞事件** 的数据集。
  - 配套提供下游任务与标注数据用于评估。
- **Benchmark**：
  - 论文提出了一套面向核与粒子物理探测器数据的下游任务评估套件，而非直接沿用自然语言或通用视觉 benchmark。
- **对比方法**：
  - 摘要仅说明与 **baseline models** 对比，并称在所有下游任务上持续优于基线。
  - 具体基线模型名称、结构、训练设置未在现有文本中给出。
- **评估维度**：
  - 神经可扩展性；
  - 跨任务泛化；
  - 数据高效适配；
  - 表征任务无关性与线性可特化性。

## 4. 资源与算力
- 所提供的摘要与元数据中 **未提及 GPU 型号、数量、训练时长、总算力或能耗**。
- 因此无法总结具体算力资源。若需确认，应查阅论文正文的实验设置部分。

## 5. 实验数量与充分性
- 从摘要可见，实验至少覆盖以下几类：
  - 模型规模扩展实验，最高至 **188M 参数**；
  - 多个下游任务上的性能对比；
  - 数据高效适配实验；
  - 表征分析实验，包括线性映射特化。
- 但现有文本 **未说明具体实验组数、消融实验数量、数据划分方式、统计显著性检验等**。
- 因此无法客观判断实验是否充分、公平。尤其需要确认：
  - baseline 是否足够强；
  - 下游任务是否覆盖典型粒子物理任务；
  - 是否存在数据泄漏或任务特定调参不公平；
  - 是否报告多次运行与误差范围。

## 6. 主要结论与发现
- 面向核与粒子物理探测器数据的基础模型可以具备 **神经可扩展性**，模型参数可达 188M。
- 在冻结权重并仅使用任务特定 adapter 的情况下，该基础模型 **在所有下游任务上一致优于 baseline**。
- 模型表现出 **稳健的数据高效适配能力**。
- 模型学到的表征具有 **任务无关性**，但可通过 **单一线性映射** 适配不同任务。
- 总体结论：该工作为核与粒子物理的探测器数据分析提供了可扩展、可泛化的统一基础模型范式。

## 7. 优点
- **领域创新性**：针对粒子物理探测器数据的稀疏、空间分布特性，提出专用自监督基础模型方法。
- **数据规模大**：构建超过 1100 万粒子碰撞事件的数据集与下游任务，具备较强领域价值。
- **评估方式合理**：冻结主干 + adapter 能较好检验通用表征质量，而非依赖全量微调。
- **可扩展性验证**：展示至 188M 参数的 scaling 行为，符合基础模型研究趋势。
- **表征分析有启发性**：指出表征任务无关但可通过线性映射特化，有助于理解物理基础模型内部表示。
- **应用潜力**：有望推动核与粒子物理中的统一建模与跨任务迁移。

## 8. 不足与局限
- **正文信息缺失**：本次提供的文本仅为摘要/元数据，无法核实方法细节、公式、网络结构与训练策略。
- **算力未报告**：缺少 GPU、训练时长等信息，难以评估可复现性与资源门槛。
- **实验细节不足**：未列出具体 baseline、任务清单、数据划分、消融实验与统计结果，公平性和充分性难以判断。
- **泛化边界未知**：模型是否跨探测器、跨实验、跨能量区间泛化，摘要未说明。
- **物理应用限制**：物理发现通常要求可解释性、系统误差控制与不确定性量化，摘要未涉及这些方面。
- **偏差与复现风险**：数据集构建方式、标注质量、任务选择偏差等均可能影响结论，需正文与复现实验验证。

（完）
