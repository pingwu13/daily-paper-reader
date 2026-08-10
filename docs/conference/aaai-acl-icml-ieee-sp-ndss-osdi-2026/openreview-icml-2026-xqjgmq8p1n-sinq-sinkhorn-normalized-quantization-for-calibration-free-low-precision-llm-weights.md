---
title: "SINQ: Sinkhorn-Normalized Quantization for Calibration-Free Low-Precision LLM Weights"
title_zh: SINQ：面向免校准低精度LLM权重的Sinkhorn归一化量化
authors: "Lorenz K Muller, Philippe Bich, Jiawei Zhuang, Ahmet Çelik, Luca Benfenati, Lukas Cavigelli"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d259753f326fef0c4bdddf8303c71be3b5cc503e.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向LLM权重的免校准PTQ，低比特下保持困惑度
tldr: 现有低比特PTQ在≤4比特时困惑度下降，部分原因是异常值导致共享尺度的参数精度受损，尤其在免校准均匀量化中。SINQ引入第二轴缩放因子和快速Sinkhorn-Knopp算法，规范化每行与每列方差，从权重矩阵结构恢复列尺度，从而近似激活感知量化。该方法无需校准数据即可提升低比特LLM权重量化精度，显著缓解困惑度退化。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 免校准均匀量化在低比特下普遍存在困惑度退化，源于异常值在共享尺度上的精度问题。
method: 增加第二轴尺度因子，并用Sinkhorn-Knopp算法快速规范化行与列方差。
result: 无需校准即恢复列尺度，显著缓解≤4比特下的困惑度退化。
conclusion: SINQ增强了PTQ方法在低比特免校准场景下的性能，提升部署效率。
---

## Abstract
Post-training quantization has emerged as the most widely used strategy for deploying large language models at low precision. Still, current methods show perplexity degradation at bit-widths $\leq 4$, partly because representing outliers causes precision issues in parameters that share the same scales as these outliers. This problem is especially pronounced for calibration-free, uniform quantization methods. We introduce SINQ to augment existing post-training quantizers with an additional second-axis scale factor and a fast Sinkhorn–Knopp–style algorithm that finds scales to normalize per-row and per-column variances. 
We show that this approximates activation-aware quantization by recovering column scales from the weight matrix structure that are predictive of the typical activation magnitudes the matrix received during training. 
Our method has no interactions between layers and can be trivially applied to new architectures to quantize any linear layer.
We evaluate our method on the Qwen3 model family, among others. SINQ reduces the perplexity gap on WikiText2 and C4 by over 50% against uncalibrated uniform quantization baselines, incurs zero to negligible compute overhead, and can be further enhanced by combining it with calibration and non-uniform quantization levels. Code is available in the supplementary.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- 大语言模型（LLM）在低精度部署中，后训练量化（PTQ）是最广泛使用的策略。
- 然而，现有方法在比特宽度 ≤ 4 时会出现明显的困惑度（perplexity）退化。
- 根本原因是：权重的异常值（outliers）导致共享同一缩放尺度的其他参数精度受损。
- 该问题在免校准（calibration-free）的均匀量化方法中尤为严重，限制了低比特部署的可行性与效率。
- 因此，论文旨在设计一种免校准、低开销的量化增强方法，在低比特下保持模型精度，从而提升 LLM 的部署经济性。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **方法名称**：SINQ（Sinkhorn-Normalized Quantization，Sinkhorn 归一化量化）。
- **核心思想**：通过引入一个额外的“第二轴”缩放因子，并利用快速 Sinkhorn–Knopp 风格算法，对权重矩阵的**每行和每列方差**进行归一化，从而恢复权重矩阵中隐含的列尺度信息。
- **关键洞察**：权重矩阵的结构中包含了与训练时典型激活幅度相关的列尺度，恢复这些尺度可以近似实现激活感知量化（activation-aware quantization），而无需实际访问校准数据。
- **技术细节**：
  - 在原有均匀量化器的基础上，增加一个第二轴的缩放因子，用于调整权重矩阵的列方差。
  - 使用 Sinkhorn–Knopp 算法迭代优化缩放因子，使行方差和列方差同时被规范化。
  - 方法逐层独立，没有层间交互，因此可以轻松应用于任意新架构的线性层。
- **公式/算法流程（文字说明）**：
  1. 对每个线性层的权重矩阵，计算当前的行/列方差。
  2. 通过快速 Sinkhorn–Knopp 迭代，寻找一组缩放因子，使得归一化后的矩阵行方差和列方差达到平衡。
  3. 在量化前应用这些缩放因子，量化后再缩放回原范围，从而实现低比特表示。
- **附加特性**：可与其他量化技术结合，例如基于校准（calibration）的量化或非均匀量化，从而进一步提升性能。

### 3. 实验设计：数据集、基准与对比方法

- **数据集**：
  - WikiText2
  - C4
- **模型**：
  - 主要在 Qwen3 模型家族上进行评估，同时提及“among others”（也包含其他模型）。
- **对比方法**：
  - 主要是未校准的均匀量化基线（uncalibrated uniform quantization baselines）。
- **评估指标**：
  - 困惑度（perplexity）差距。
- **主要结果**：
  - 相比未校准均匀量化基线，SINQ 在 WikiText2 和 C4 上将困惑度差距降低了超过 50%。
  - 计算开销为零到可忽略不计。
- **额外实验**：
  - 与校准（calibration）方法结合。
  - 与非均匀量化级别结合，显示可进一步增强性能。

### 4. 资源与算力

- 论文摘要及提供的内容中**未明确说明**使用的 GPU 型号、数量、训练/量化运行时长等资源信息。
- 仅提及计算开销“零到可忽略不计”，但这指的是推理时的额外计算成本，而非训练或评估所需的硬件资源。
- 因此，目前无法评估其能耗或硬件需求。

### 5. 实验数量与充分性

- 摘要中报告了在两组数据集（WikiText2、C4）上的结果，并覆盖 Qwen3 等模型。
- 进行了与未校准均匀量化基线的直接对比，以及与其他增强方案（校准、非均匀量化）的结合性实验。
- 但论文正文/摘要未提供详细实验列表（如不同比特宽度、不同模型规模、消融研究、重复实验次数、标准差等）。
- **充分性评估**：
  - 从摘要看，核心结论有数据支撑，但缺乏细节；由于只有摘要，无法判断实验设计的全面性和统计显著性。
  - 对比基线较为单一（仅未校准均匀量化），对于与 SOTA 校准感知方法的对比未在摘要中给出。
  - 公平性方面：使用公开数据集和标准困惑度指标，有利于公平比较，但仍需要更多消融和跨模型验证。

### 6. 论文的主要结论与发现

- SINQ 可以通过免校准的方式恢复权重矩阵的列尺度，从而近似激活感知量化，显著缓解低比特（≤4）下的困惑度退化。
- 在 WikiText2 和 C4 上，与未校准均匀量化基线相比，困惑度差距缩小超过 50%。
- 额外计算开销几乎为零，且方法不依赖于层间信息，易于适配新架构。
- 与校准和非均匀量化结合时，性能可以进一步提升，说明 SINQ 具有良好正交性和可扩展性。

### 7. 优点

- **免校准**：无需任何标定数据，适合无法获取训练/校准数据的场景。
- **低开销**：增量计算成本可忽略，适合部署环境。
- **通用性强**：逐层独立，不加层间交互，可快速适配新模型架构。
- **正交性**：可与现有量化方法（校准、非均匀量化）无缝结合。
- **理论动机清晰**：从权重矩阵结构恢复列尺度，从原理上解释了对激活感知量化的近似。
- **实际效果显著**：在典型基准上超过 50% 的困惑度差距缩减，说明方法有效。

### 8. 不足与局限

- **信息不完整**：当前只提供摘要，缺乏方法细节（如 Sinkhorn-Knopp 算法的具体迭代次数、收敛条件、缩放因子的数值范围等），难以复现。
- **实验覆盖有限**：报告中只明确提及 Qwen3 家族和 WikiText2/C4，缺乏更多模型架构（如 Llama、Mistral 等）和更多任务的验证。
- **对比基线不足**：仅对比了未校准均匀量化基线，未与免校准的先进方法（如基于 Hessian、基于异常值感知的量化）进行全面比较。
- **比特宽度范围不明**：虽然提到 ≤4 比特，但未细化为 3-bit、2-bit 或 4-bit 的具体结果。
- **未讨论异常值分布差异**：不同模型/层的异常值特征可能不同，方法是否对所有类型稳健未说明。
- **资源信息缺失**：未提及硬件配置，可能影响实际部署评估。
- **应用限制**：主要针对线性层权重量化，对嵌入层、归一化层或注意力 softmax 等组件的处理未提及。

（完）
