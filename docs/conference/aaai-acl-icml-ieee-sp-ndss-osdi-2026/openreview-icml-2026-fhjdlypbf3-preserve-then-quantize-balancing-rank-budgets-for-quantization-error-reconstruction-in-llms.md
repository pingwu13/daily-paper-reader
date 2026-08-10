---
title: "Preserve-Then-Quantize: Balancing Rank Budgets for Quantization Error Reconstruction in LLMs"
title_zh: 先保留再量化：为LLM量化误差重构平衡秩预算
authors: "Yoonjun Cho, Dongjae Jeon, Soeun Kim, Moongyu Jeon, Albert No"
date: 2026-04-30
pdf: "https://openreview.net/pdf/81544add9ba73493ad6b417ba2d6f8735d3acef9.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向LLM权重的低秩误差重构PTQ方法
tldr: 量化误差重构（QER）通过权重近似为量化值加低秩校正来减少PTQ精度损失，但先前方法将全部秩预算用于误差重构并不最优。该文提出结构化残差重构（SRR），在量化前先保留激活缩放权重的顶部奇异子空间，仅量化残差，并用剩余秩进行误差重构。理论指导的秩分配可更有效地保护主导方向，提升低比特PTQ的最终精度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 全秩预算用于误差重构忽略了权重的内在低秩结构和主导方向损坏。
method: 在量化前保留top-k奇异子空间，量化残差，并用余下秩重建误差。
result: 理论指导的秩分配显著改善低比特PTQ精度。
conclusion: SRR为LLM低比特量化提出更优的秩预算分配框架。
---

## Abstract
Quantization Error Reconstruction (QER) reduces accuracy loss in Post-Training Quantization (PTQ)
by approximating weights as $\mathbf{W} \approx \mathbf{Q} + \mathbf{L}\mathbf{R}$, using a rank-$r$ correction to reconstruct quantization error.
Prior methods devote the full rank budget to error reconstruction,
which is suboptimal when $\mathbf{W}$ has intrinsic low-rank structure and quantization corrupts dominant directions.
We propose Structured Residual Reconstruction (SRR),
a rank-allocation framework that preserves the top-$k$ singular subspace of the activation-scaled weight before quantization,
quantizes only the residual, and uses the remaining rank $r-k$ for error reconstruction.
We derive a theory-guided criterion for selecting $k$ by balancing quantization-exposed energy 
and unrecoverable error under rank constraints.
We further show that resulting $\mathbf{Q}+\mathbf{L}\mathbf{R}$ parameterization naturally supports Quantized Parameter-Efficient Fine-Tuning (QPEFT),
and stabilizes fine-tuning via gradient scaling along preserved directions.
Experiments demonstrate consistent perplexity reductions across diverse models and quantization settings in PTQ,
along with a 5.9 percentage-point average gain on GLUE under 2-bit QPEFT.
The project page is available at https://ai-isl.github.io/srr.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义

- **研究背景**：大语言模型（LLM）规模庞大，部署时需要后训练量化（Post-Training Quantization, PTQ）来压缩模型、降低内存与推理成本。
- **已有方法**：量化误差重构（QER）将权重近似为 \( \mathbf{W} \approx \mathbf{Q} + \mathbf{L}\mathbf{R} \) 的形式，其中 \(\mathbf{Q}\) 是量化后的权重，\(\mathbf{L}\mathbf{R}\) 是低秩校正项，用于重构量化过程中损失的信息。
- **存在的不足**：先前 QER 方法把全部秩预算（rank budget）都用于误差重构，忽略了两个重要事实：一是权重本身可能具有内在低秩结构，二是量化过程会破坏权重的主导方向。因此在权重低秩性明显时，这样的秩预算分配并非最优。
- **核心问题**：如何更合理地分配秩预算——既保护权重的主导奇异子空间，又尽可能重构量化误差，从而提升低比特量化 LLM 的精度。

### 2. 方法论

- **核心思想**：先保留，再量化（Preserve-Then-Quantize）。
- **SRR 框架**（Structured Residual Reconstruction，结构化残差重构）：
  - 在量化之前，先对激活缩放后的权重进行奇异值分解，保留其 top-k 奇异子空间（主导方向）。
  - 只对残差部分进行量化，避免主导方向被量化噪声破坏。
  - 使用剩余的秩 \(r-k\) 对量化误差进行低秩重构，最终参数化形式仍为 \(\mathbf{W} \approx \mathbf{Q} + \mathbf{L}\mathbf{R}\)。
- **技术细节**：论文推导了理论指导的秩分配准则，通过平衡两类损失来选择保留秩 \(k\)：
  - 量化暴露的能量（quantization-exposed energy）；
  - 在给定秩约束下不可恢复的误差（unrecoverable error under rank constraints）。
- **其他贡献**：
  - 证明该 \(\mathbf{Q}+\mathbf{L}\mathbf{R}\) 参数化可以自然地适配量化参数高效微调（QPEFT）；
  - 在微调时沿保留方向进行梯度缩放，能够稳定微调过程。

### 3. 实验设计

- **评估场景**：
  - PTQ（后训练量化）：以困惑度（perplexity）为主要指标，覆盖多种模型与量化设置；
  - 2-bit QPEFT（量化参数高效微调）：使用 GLUE 基准，报告平均性能。
- **对比方法**：主要对比的是将全部秩预算用于误差重构的先前 QER 类方法。
- **已知结论**：SRR 在 PTQ 中跨模型、跨量化设置都带来一致的困惑度下降；在 2-bit QPEFT 下，GLUE 平均提升 5.9 个百分点。
- **待明确点**：现有提供的文本仅含摘要，未列出具体模型名称、具体数据集（如 WikiText 等）、具体量化算法（如 GPTQ、AWQ 等）和超参数设置。

### 4. 资源与算力

- 论文提供文本中未明确说明使用的 GPU 型号、数量、训练或推理时长，因此无法给出具体的算力细目。
- 需要注意的是，SRR 方法需要额外进行 SVD 分解和秩分配计算，可能会带来一定的预处理开销，但现有信息中没有给出具体的耗时或算力对比数据。

### 5. 实验数量与充分性

- 实验覆盖两个主要场景：PTQ 与 QPEFT；在 PTQ 中称“多种模型与量化设置”，在 QPEFT 中使用 GLUE。
- 从摘要看，实验具备基本的多模型、多设置覆盖，并与先前的秩分配策略进行直接对比，能够体现方法的增益。
- 但是，由于可获取的文本只有摘要，缺少完整实验表格、消融研究、标准差/显著性检验和基线复现细节，因此无法从现有材料判断实验是否在统计充分性、消融完整性和对比公平性上完全达标。

### 6. 主要结论与发现

- 将秩预算从“全部用于误差重构”改为“部分用于保留主导奇异子空间、只量化残差余量”，在低比特 PTQ 设定下能够始终带来困惑度下降，即精度提升。
- 理论指导的秩分配能够有效平衡量化暴露能量与不可恢复误差，从而显著改善低比特量化后的模型性能。
- 在 2-bit QPEFT 场景中，该框架在 GLUE 上平均获得 5.9 个百分点的提升，说明秩分配框架对微调同样有效。

### 7. 优点

- 针对量化误差重构中的秩预算分配问题提出重新思考，并给出理论指导的秩选择准则。
- 方法简洁有效，同一框架同时支持 PTQ 和 QPEFT，通用性较强。
- 利用权重内在低秩结构，在量化前后保持主导方向，原理清晰、可解释性好。
- 实验跨多种模型和量化设置体现出一致的性能提升，显示方法具有较好的鲁棒性。

### 8. 不足与局限

- 本次可获取的文本仅包含摘要，缺乏具体实验细节（模型列表、数据集、秩设置、超参数等），难以完全验证其严谨性。
- 未说明算力消耗与预处理开销；SVD 分解在大规模模型上可能带来不可忽略的计算成本。
- 实验覆盖以困惑度和 GLUE 为主，任务范围有限，对生成任务、长文本、多语言、代码等场景的推广效果尚不明确。
- 未与其他前沿量化方法（如 1-bit 量化、混合精度量化、显式低秩适配方法等）展开详细比较。
- 保留 top-k 子空间的做法对奇异值衰减不明显的权重可能收益有限；理论准则中的 \(k\) 选择也可能依赖额外启发式或超参数。

（完）
