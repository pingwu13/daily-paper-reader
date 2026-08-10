---
title: "pQuant: Towards Effective Low-Bit Language Models via Decoupled Linear Quantization-Aware Training"
title_zh: pQuant：通过解耦线性量化感知训练实现低比特语言模型
authors: "Wenzheng Zhang, Bingzheng Liu"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.1566.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 面向亚2比特LLM权重量化，提出解耦线性层的QAT方法，包含主1比特分支和高精度分支
tldr: 针对超低比特（亚2比特）LLM权重的量化感知训练，论文指出现有方法因参数敏感性均匀化而表达能力受限。提出pQuant，将线性层解耦为高效的1比特主分支和紧凑的高精度分支。实验表明，pQuant在提升超低比特模型精度和可扩展性的同时支持边缘部署。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 679, \"height\": 574, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 799, \"height\": 284, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1614, \"height\": 1180, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 714, \"height\": 499, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1635, \"height\": 418, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 789, \"height\": 566, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 799, \"height\": 336, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 794, \"height\": 426, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 755, \"height\": 424, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 703, \"height\": 405, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1566/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 753, \"height\": 385, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 805, \"height\": 249, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1661, \"height\": 794, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 806, \"height\": 264, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 695, \"height\": 223, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 661, \"height\": 230, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1659, \"height\": 495, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1651, \"height\": 184, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1566/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 615, \"height\": 230, \"label\": \"Table\"}]"
motivation: 现有低比特（亚2比特）LLM量化感知训练精度与可扩展性不足，参数敏感性趋于均匀化是关键瓶颈。
method: 将线性层参数解耦为两个分支：一个主1比特分支负责高效计算，一个紧凑高精度分支负责保留关键表达能力。
result: pQuant在超低比特权重量化中取得了更好的精度和可扩展性，相比现有QAT方法有显著提升。
conclusion: 解耦参数敏感性不均匀的参数分支可提升亚2比特QAT的有效性，促进LLM边缘部署。
---

## Abstract
Quantization-Aware Training from scratch has emerged as a promising approach for building efficient large language models (LLMs) with extremely low-bit weights (sub 2-bit), which can offer substantial advantages for edge deployment. However, existing methods still fail to achieve satisfactory accuracy and scalability. In this work, we identify a parameter democratization effect as a key bottleneck: the sensitivity of all parameters becomes homogenized, severely limiting expressivity. To address this, we propose pQuant, a method that decouples parameters by splitting linear layers into two specialized branches: a dominant 1-bit branch for efficient computation and a compact high-precision branch dedicated to preserving the most sensitive parameters. Through tailored feature scaling, we explicitly guide the model to allocate sensitive parameters to the high-precision branch. Furthermore, we extend this branch into multiple, sparsely-activated experts, enabling efficient capacity scaling. Extensive experiments indicate our pQuant achieves state-of-the-art performance in extremely low-bit quantization.

---

## 论文详细总结（自动生成）

# pQuant：通过解耦线性量化感知训练实现低比特语言模型

## 1. 核心问题与研究动机

- **背景**：大语言模型（LLM）虽然性能卓越，但巨大的计算和内存需求使其在资源受限设备（如边缘设备）上的部署面临挑战。
- **现有方案**：量化技术是缓解上述瓶颈的关键手段。其中，**极低比特（亚2比特）量化**是最激进的压缩方式，能大幅降低内存和计算开销，但精度损失严重。
- **研究路线**：从零开始的量化感知训练（QAT-Scratch）相比后训练量化（PTQ）和微调式 QAT，在超低比特场景下表现出更优的精度潜力。例如，BitNet 达到 FP16 基准 90.1% 的下游任务性能。
- **关键瓶颈发现**：论文揭示了一个此前未被识别的现象——**参数民主化（Parameter Democratization）效应**：在极端量化条件下，所有权重的敏感性趋于均匀化，模型丧失了区分"关键参数"与"普通参数"的能力，严重限制了模型的表达能力和可扩展性（即模型增大时性能增益呈次线性增长）。
- **核心假设**：如果能够恢复参数的差异化敏感性结构，让少数关键敏感参数获得更高精度的保护，则有望同时提升超低比特模型的精度和规模化能力。

## 2. 方法论

pQuant 的核心思想是**通过解耦线性层实现参数敏感性的有效分配**，具体包括以下关键技术：

### 2.1 解耦线性层架构

- 将 FFN（前馈网络）中的原始权重矩阵拆分为两个并行分支：
  - **1比特主分支**：承担绝大部分计算，保证推理效率（权重用 Sign 函数二值化为 ±1）；
  - **8比特高精度分支**：维度记为 r（远小于模型隐藏维度 D_model），专门保留最敏感的参数。
- 两分支输出加权求和后作为最终输出。
- 在 MHA（多头注意力）模块中，所有线性层采用纯 1 比特量化（未做解耦），因为 FFN 集中了更多敏感参数且其激活分布存在更多离群值。

### 2.2 特征缩放（Feature Scaling）

- 为两个分支的输出分别引入**可学习的缩放因子 α 和 β**，并初始化 α≫β（如 α=2.0，β=0.2），使高精度分支在反向传播中获得更强的梯度信号，从而引导模型主动将敏感参数分配到 8 比特路径。
- 消融实验表明，特征缩放并非仅仅影响早期优化动态，而是对模型最终性能产生持久的结构性影响。

### 2.3 量化方案

- **1比特权重**：`W_INT1 = Sign(W_Float - μ)`，其中 μ 为权重均值。用 λ（权重绝对值的均值）作为反量化缩放因子。
- **8比特激活和权重**：采用 AbsMax 方法，将值量化为 INT8 范围 [-128, 127]，公式为 `Q(X) = RoundClip(X × γ, -128+ε, 127+ε)`，γ 为缩放因子。
- 训练时保留 FP16 影子权重用于梯度更新（STE 直通估计器），推理时丢弃全部高精度副本，只保留量化后的参数。

### 2.4 高效扩展（稀疏专家机制）

- 将高精度分支扩展为 **N 个并行分支（专家）**，配合轻量级 top-1 Router（线性层 + Softmax），每个 token 只激活一个分支。
- 该设计与 MoE（如 DeepSeekMoE）结构相似但本质不同：不同之处在于精度差异（1-bit vs 8-bit）和维度设置。
- 核心优势：增加 N 不会增加单次前向传播的激活参数量，以接近恒定的推理成本实现容量扩展，使模型遵循更接近全精度模型的缩放规律。

## 3. 实验设计

### 3.1 训练数据

- C4、Wikipedia、ArXiv 三个数据集，批大小配置为 4M tokens（LLaMA-2）和 1M tokens（其他模型）。

### 3.2 评估基准

- **困惑度**：WikiText-2。
- **下游任务**：ARC-Easy、ARC-Challenge、BoolQ、PIQA、Winogrande、OpenbookQA、Hellaswag，共 7 个零样本任务，取平均准确率。

### 3.3 模型规模

- pQuant 在 300M、700M、1.3B、2.6B 四个规模上训练。
- 1比特占比约 95-96%，8比特占比约 4-5%（300M 和 700M 为 4%，1.3B 和 2.6B 为 5%）。

### 3.4 对比方法

- **训练基线**：BitNet（1-bit）、BitNet1.58（2-bit）、LLaMA-2（FP16），在同一配置（相同模型尺寸、数据预算100B tokens）下从头训练。
- **外部参考**：PTQ1.61（7B模型的PTQ）、OmniQuant（OPT-1.3B后训练量化）、OneBit（基于OPT-1.3B的微调式QAT）。

## 4. 资源与算力

- **硬件**：16 张 NVIDIA A100-80G GPU，1 TB CPU 内存，使用 DeepSpeed 框架。
- **训练时长**（论文附录 H 给出）：
  - 300M：1.9–2.3 天；
  - 700M：4.9–6.0 天；
  - 1.3B：8.5–11.1 天；
  - 训练时长随 8 比特分支数量 N 从 1 到 8 递增。
- 论文明确说明 QAT-Scratch 方法的训练成本显著高于传统 QAT 和 PTQ。

## 5. 实验数量与充分性评估

### 5.1 主要实验

- 四个模型规模（300M/700M/1.3B/2.6B）的完整评估，覆盖困惑度和 7 项下游任务，实验规模较充足。

### 5.2 消融实验

- **特征缩放**：验证 α/β 初始化的必要性（有/无缩放对比）。
- **高精度分支维度 r**：测试不同 r 值（256/576/768）的影响。
- **分支数量 N**：对比 N=1/2/4/8 的性能变化。
- **激活分支数**：单分支 vs. 双分支激活。
- **量化方法对比**：Native Mix（直接保留8%高精度参数）、Channel-wise、Group-wise 量化与 pQuant 架构的对比。

### 5.3 其他分析

- **敏感性分布可视化**：验证 pQuant 恢复差异化敏感性结构。
- **内存效率**：对比 pQuant、BitNet、BitNet1.58、LLaMA-2 的权重内存占用。
- **推理速度**：对比各模型单次前向传播的计算时间。
- **训练稳定性**：对比 pQuant 与 BitNet 的损失曲线。

### 5.4 充分性与公平性评价

- **优点**：pQuant 与 BitNet/BitNet1.58/LLaMA-2 在完全一致的模型尺寸和数据预算下对比，控制了变量，具有较强说服力；消融实验覆盖了方法各核心组件。
- **局限性**：与 OmniQuant、OneBit、PTQ1.61 的比较并非等条件——这些方法使用了更大模型（7B）或更多预训练数据（180B tokens vs 100B tokens）。论文对此有明确定位说明（非直接竞争，仅展示 QAT-Scratch 路线的潜力），但读者需注意对比的公平性边界。

## 6. 主要结论与发现

- **性能提升显著**：pQuant 在 1.28–1.35 比特位宽下，困惑度比 SOTA 1比特基线（BitNet）降低 32.0%（1.3B 规模下 PPL 从 21.8 降至 17.2），并在 7 项下游任务平均准确率上全面超越。
- **媲美 2 比特模型**：1.3B pQuant 与 2 比特 BitNet1.58 的平均准确率差距仅 0.4 个百分点（44.0 vs 44.4），但权重位数少 0.65 比特。
- **超越更大规模的外部基线**：2.6B pQuant 平均准确率 47.1，超过 1.3B LLaMA-2（FP16，45.4），也大幅超越 7B PTQ1.61（41.8）。
- **可扩展性优越**：当 N=8 时，pQuant 在 1.3B 规模下几乎匹配 LLaMA-2 的训练损失，而 BitNet 和 BitNet1.58 与 FP16 基线存在持续差距。**在相同参数预算下（总计1.3B），pQuant 匹配 BitNet1.58 性能的同时推理速度快 1.6 倍。**
- **缓解参数民主化**：敏感性分析证实 pQuant 成功恢复了差异化的敏感性分布，8比特分支有效聚集了高敏感参数。
- **极高的内存效率**：pQuant 权重内存比 LLaMA-2 减少 92%，比 BitNet1.58 减少 31%。在 7B 规模下，pQuant 内存占用仅 1.1GB 左右。

## 7. 优点与亮点

- **现象发现新颖**：识别并定义了"参数民主化"效应，为超低比特量化研究提供了新的分析视角，具有很强的启发性。
- **方法设计巧妙**：
  - 解耦架构同时兼顾效率（1比特主体）和表达力（8比特敏感分支），实现了两者间的有效平衡；
  - 特征缩放提供了端到端可学习的敏感参数分配机制，避免了手工预定义敏感位置的次优解；
  - 将高精度分支扩展为稀疏专家结构，以"总参数增/激活参数不变"的方式实现高效的容量扩展，设计逻辑清晰。
- **验证链条完整**：从敏感性分析（发现问题）→ 方法设计（提出解决方案）→ 敏感性再分析（验证方案生效），形成了完整的证据闭环。
- **满足边缘部署实践需求**：推理时所有缩放因子可融合，1比特权重打包存储（8参数/字节），支持基于 LUT（查表）的高效矩阵乘实现，兼容现有硬件（INT8 优于 FP8/MXFP8）。

## 8. 不足与局限

- **训练成本高昂**：QAT-Scratch 方法需要从头训练，且训练时需同时维护 FP16 影子权重和量化权重；以 1.3B 模型为例需 8.5–11.1 天（16张A100）。论文坦承这是相对于 PTQ 和微调式 QAT 的主要劣势。
- **模型规模有限**：实验最大仅到 2.6B 参数，未在更大规模（如 7B、70B）上验证方法有效性，结论的外推性有待检验。
- **高精度分支的物理内存开销**：当 N=8 时，总参数量增加约 30%（1.3B 增至 1.7B），虽然激活参数量不变，但模型的整体存储需求上升。论文承认这代表一种精度 vs. 内存的权衡。
- **对比公平性边界**：与 PTQ1.61、OmniQuant、OneBit 的比较并非在同等资源条件下进行，硬件场景聚焦在边缘设备推理（batch size=1 场景下的 GEMV），未验证在数据中心高吞吐场景下的表现。
- **训练稳定性依赖工程技巧**：pQuant 的稳定性部分依赖于两阶段学习率调度、权重衰减调节等训练技巧，方法对超参数的敏感性值得关注。

（完）
