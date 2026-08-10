---
title: Theory-optimal Quantization Based on Flatness
title_zh: 基于平坦度的理论最优量化
authors: "Xiusheng Huang, Zhe Li, Xuanwu Yin, Lu Wang, Yequan Wang, Dong Li, Emad Barsoum, Kang Liu"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1767.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向低比特LLM后训练量化的理论驱动离群值抑制
tldr: 该文聚焦LLM后训练量化中激活离群值导致低比特性能严重退化的问题，首先建模量化误差与离群值之间的数学关系，并提出新指标Flatness来刻画离群值分布。基于该指标，作者推导了理论上的最优量化变换，以更有效地压缩离群值、改善低比特量化效果。实验表明该方法能在较低比特下保持模型质量，为后训练量化中的离群值处理提供了理论指导。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1767/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1616, \"height\": 462, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1767/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1473, \"height\": 903, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1651, \"height\": 471, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1652, \"height\": 1401, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1649, \"height\": 204, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1501, \"height\": 250, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1658, \"height\": 564, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1650, \"height\": 1626, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 744, \"height\": 354, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1516, \"height\": 199, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1767/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 706, \"height\": 256, \"label\": \"Table\"}]"
motivation: 激活离群值是低比特LLM量化的主要障碍，现有变换后仍存在离群模式。
method: 建模量化误差与离群值关系，提出Flatness指标并推导理论最优变换。
result: 有效压缩离群值，在较低比特下显著改善量化性能。
conclusion: 理论驱动的离群值度量与变换可为低比特后训练量化提供原则性方法。
---

## Abstract
Post-training quantization has emerged as a widely adopted technique for compressing and accelerating the inference of Large Language Models (LLMs). The primary challenges in LLMs quantization stem from activation outliers, which significantly degrade model performance especially at lower bit precision. While recent approaches attempt to mitigate outliers through linear transformations across feature dimensions, our analysis reveals that the transformed weights and activations still exhibit persistent outlier patterns with concentrated magnitude distributions. In this paper, we first model the mathematical relationship between quantization error and outliers, and then introduce a new metric Flatness to quantify the distribution of outliers. Based on this, we derive the theoretical optimal solution with respect to Flatness. Building on these insights, we propose Bidirectional Diagonal Quantization (BDQ), a novel post-training quantization framework that effectively disperses outlier patterns through optimized matrix transformations. BDQ strategically distributes outlier magnitudes across matrix dimensions via learned diagonal operations. Extensive experiments demonstrate that BDQ establishes a new quantization benchmark. It achieves less than 1% accuracy drop in W4A4 quantization on the LLaMA-3-8B model. In the more challenging W2A4KV16 experiment, compared to state-of-the-art approaches, BDQ reduces the performance gap by 39.1% on the DeepSeek-R1-Distill-LLaMA-70B model.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究主题**：面向大语言模型（LLM）的低比特后训练量化（Post-Training Quantization, PTQ）。
- **核心痛点**：激活值中的离群点（outliers）严重损害低比特量化精度，导致模型性能显著下降，尤其是在 W4A4 甚至更低比特设置下。
- **现有方法的不足**：已有的旋转变换（rotation）、缩放等方法虽能缓解离群问题，但变换后的权重和激活中仍然存在离群模式，且这些方法多为启发式，缺乏对"离群值与量化误差"之间直接数学关系的建模，也未对量化空间的整体分布进行显式优化。
- **论文的核心主张**：通过数学建模揭示离群值与量化误差呈平方级关系，提出新指标 Flatness 量化离群值分布，并基于此推导出理论最优的矩阵变换形式（双向对角变换），据此设计 BDQ 量化框架，从理论层面指导离群值抑制。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **数学建模（第 3 节）**：
  - 推导单个离群值对量化误差的影响：当存在大离群值 w_outlier 时，量化步长 Δ 从 c/(2^b−1) 扩至 |w_outlier|/(2^b−1)，导致所有非离群值的量化误差上限扩大，量化误差与离群值之间呈比例关系。
  - 扩展至整个矩阵后，总量化误差 E[ε²] ≈ p·w²_outlier·x，即与离群值的大小呈**平方级关系**，证实离群值是量化误差的主导因素。

- **Flatness 指标（第 4 节）**：
  - 受信息熵启发，定义矩阵的 Flatness 为：F = Σ (W²_ij/(α_i β_j)) · ln(W²_ij/(α_i β_j))，其中 α_i、β_j 为行、列能量权重因子。
  - 通过约束 Σ W²_ij/(α_i β_j) = 1 和能量约束 Σ α_i W²_ij β_j = C，利用拉格朗日乘子法求最优解。
  - 理论推导得出：最优解满足"行独立"与"列独立"结构，即 α_i 仅由第 i 行数据决定、β_j 仅由第 j 列数据决定，形成 V = d₁ W d₂ 的双向对角形式（d₁ = diag(√α_i)，d₂ = diag(√β_j)），为理论上的最优 Flatness 及最优量化误差形式。

- **BDQ 框架（第 5.1 节）**：
  - 每个 Transformer 块学习四组等价变换对，每组由两个可学习对角矩阵 ⟨Λ₁, Λ₂⟩ 和一个可学习旋转矩阵 R 组成。
  - 前向推理重构为：y = Q(Λ₁xΛ₂R) · Q(RᵀΛ₂⁻¹WΛ₁⁻¹)。
  - 旋转矩阵由 Hadamard 矩阵及附加正交矩阵构成，用于进一步打散强列相关场景下的能量集中。
  - 附录中通过与 FlatQuant 的 Kronecker 分解对比，论证了双向对角结构的优势：参数独立性强（每个元素仅依赖两个独立参数）、优化多凸性（易于收敛）、误差界 O(k) 显著优于 FlatQuant 的 Ω(k·64²)、信息保持更完整（不损失秩）。

- **递归交叉熵损失 RCE（第 5.2 节）**：
  - 针对小校准集（128 条样本）导致的过拟合问题（实验显示传统交叉熵损失随训练步长增加，零样本任务性能反而下降、Flatness 上升）。
  - 利用量化后模型预测分布 p 的高置信度（Top-50 token 命中率达 99.36%），提出同时拟合标签分布 q 和预测分布 p 的损失：L_RCE = −Σ(q_i log p_i − p_i log(δ·p_i + (1−δ)·q_i))。
  - δ 平衡两种分布的权重，实验表明 0.5 为最优。

### 3. 实验设计：数据集、场景、benchmark 与对比方法

- **模型**：LLaMA 系列（LLaMA-2 7B/13B/70B、LLaMA-3 8B/70B）、DeepSeek-R1-Distill-LLaMA 8B/70B。
- **量化设置**：
  - 常规：W4A4KV4、W3A3KV3、W2A4KV16。
  - 量化方法统一采用 GPTQ。
- **数据集**：
  - PPL 指标：WikiText2、C4。
  - 零样本下游任务：HellaSwag、LAMBADA、PIQA、WinoGrande、ARC-Easy、ARC-Challenge（lm-evaluation-harness 框架）。
  - 补充实验：MMLU、MATH（GSM8K 8-shot + MATH 4-shot）。
- **对比方法**：QuaRot、SpinQuant、FlatQuant（即 SOTA 基线），以及 FP16 全精度模型作为参考。
- **基准**：在 PPL（↓）、各任务准确率（↑）及平均准确率（Avg）上对比。

### 4. 资源与算力

- 论文未明确说明 GPU 型号、数量及训练时长。
- 仅在推理效率实验中提及使用了 **NVIDIA A100 80GB 和 AMD MI250** 两种 GPU。
- 作者在 Limitations 中明确承认：受计算资源限制，未在更大规模的 LLM 上实验，也未在不同类型 GPU 上进行广泛验证。

### 5. 实验数量与充分性

- **实验数量丰富**：
  - 主实验：4 个模型家族（LLaMA-2、LLaMA-3、DeepSeek-R1-Distill 各两个规模）在 3 种量化设置（W4A4KV4、W3A3KV3、W2A4KV16）下 vs. 3 个基线方法的系统对比。
  - 消融实验：Only-BDQ vs. BDQ+RCE（表 3）；旋转矩阵有无（表 9）；超参数 δ 取值范围 0~1（表 8）。
  - 效率实验：NVIDIA/AMD 双平台、6 种序列长度的 Prefill Speedup 和 Memory Saving。
  - 其他：候选 token 命中率统计（表 4）、MMLU/MATH 补充评测（表 7）。
- **充分性评估**：
  - 优点：覆盖模型范围广、量化比特设置全面（含极低比特 W2A4KV16）、同时包含生成 PPL 与下游任务准确率双维度评测，消融实验设计合理。
  - 不足：缺少在更大规模模型（>70B）上的验证；零样本任务数量有限（仅 6 个，未覆盖 MM LU 等更多元 benchmark 于主实验）；未报告多次运行的标准差等统计显著性信息。

### 6. 论文的主要结论与发现

- 离群值与量化误差呈平方级关系，是低比特量化性能退化的主导因素。
- 提出的 Flatness 指标能有效刻画矩阵中离群值的分布，其理论最优解（双向对角变换 V = d₁Wd₂）既是最优 Flatness 形式，也是最利于降低量化误差的形式。
- BDQ 框架在几乎所有模型和设置下均超越 SOTA 基线：
  - W4A4KV4 下，LLaMA-3-8B 平均准确率 71.46%，仅比 FP16（73.23%）低不到 1.8 个百分点（摘要称 <1%）。
  - W2A4KV16 下，DeepSeek-R1-Distill-LLaMA-70B 相比 FlatQuant 性能差距缩小 39.1%。
  - LLaMA3-70B 在 W4A4KV4 下与全精度模型性能基本持平。
- RCE 损失有效缓解了小校准集导致的过拟合，进一步提升量化性能（Avg 比 Only-BDQ 高 0.42%）。
- BDQ 在推理效率上显著优于全精度模型：NVIDIA A100 上 LLaMA2-70B 最高 3.44× prefill 加速；AMD MI250 上最高 3.74× 内存节省。

### 7. 优点

- **理论贡献扎实**：首次建模离群值与量化误差的数学关系，提出可量化的 Flatness 指标并给出严格数学推导的最优解，使量化方法从启发式走向理论指导。
- **方法设计精巧**：双向对角变换 + Hadamard 旋转的组合简洁且可解释，附录从参数独立性、凸性、误差界、秩保持四个维度与 FlatQuant 做了深入理论对比。
- **RCE 损失设计合理**：利用量化模型自身预测分布的高置信度，有效抑制小校准集过拟合，具有通用意义。
- **实验体系完整**：多模型、多比特设置、多任务、双 GPU 平台的系统性评测，结果一致性地支持方法有效性。

### 8. 不足与局限

- **理论假设的局限**：Flatness 推导假设矩阵元素可视为概率分布且离群值稀疏，对于离群值密集或分布形态复杂的实际场景，最优性可能不再严格成立（论文未充分讨论此边界条件）。
- **旋转矩阵的引入**：虽然附录给出了正交变换消除列相关性的理论证明，但 V = d₁Wd₂ 与引入 R 后的形式之间的最优性衔接缺乏严格的统一性论证。
- **实验覆盖有限**：
  - 未在超大规模模型（>70B，如 100B+/400B+）上验证；
  - 未在不同 GPU 架构（除 A100、MI250 外）上测试泛化性；
  - 主实验未包含 MMLU 等更全面的 benchmark（MMLU 仅在附录的 LLaMA-2-7B 单模型上补充）。
- **资源信息不透明**：未披露训练时间、GPU 数量等关键信息，影响可复现性和效率评估。
- **收敛性分析不足**：虽然论文声称旋转矩阵为可学习的 Hadamard 矩阵，但未说明其端到端学习过程的收敛性、初始化敏感度等问题。

（完）
