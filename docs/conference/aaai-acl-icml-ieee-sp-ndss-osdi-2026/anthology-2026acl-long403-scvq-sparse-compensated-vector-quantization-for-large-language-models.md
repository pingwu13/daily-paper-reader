---
title: "SCVQ: Sparse-Compensated Vector Quantization for Large Language Models"
title_zh: SCVQ：面向大语言模型的稀疏补偿向量量化
authors: "Zixuan Zhou, Yujun Diao, Zicheng Kong, Dehua Ma, Zhenbo Xu, Pei Pei Li, Zhaofeng He"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.403.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 面向LLM权重的稀疏补偿向量量化
tldr: 该文针对LLM向量量化中码本存储大、索引查找开销高以及超低比特下性能退化的问题，提出SCVQ框架。SCVQ采用显著性加权的K均值聚类和对称约束来缩小码本规模，并通过统一结构化表示减少索引成本。该设计在超低比特宽度下保持较低性能损失，同时提升推理效率，为LLM的向量量化部署提供了新方案。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long403/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1658, \"height\": 596, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long403/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1572, \"height\": 831, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long403/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 791, \"height\": 1548, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long403/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1432, \"height\": 328, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long403/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 795, \"height\": 380, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 817, \"height\": 311, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 735, \"height\": 321, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1302, \"height\": 741, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1490, \"height\": 628, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 613, \"height\": 742, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 678, \"height\": 398, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 685, \"height\": 357, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 614, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 780, \"height\": 656, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 710, \"height\": 413, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1009, \"height\": 885, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 828, \"height\": 650, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 840, \"height\": 473, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 582, \"height\": 412, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1076, \"height\": 705, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long403/table-016.webp\", \"caption\": \"\", \"page\": 0, \"index\": 16, \"width\": 1092, \"height\": 1020, \"label\": \"Table\"}]"
motivation: LLM向量量化存在码本存储和索引开销大，超低比特下性能退化明显。
method: 提出SCVQ，采用显著性加权K均值、对称约束和统一结构化表示降低开销。
result: 缩小码本规模、降低索引成本，同时提升超低比特量化精度。
conclusion: SCVQ以稀疏补偿方式实现高效LLM向量量化，兼顾压缩与推理效率。
---

## Abstract
Large Language Models (LLMs) are primarily constrained by memory and bandwidth bottlenecks during deployment. Although Vector Quantization (VQ) has emerged as a promising solution, existing methods incur inference overhead due to massive codebook storage and intensive index lookups. Moreover, these methods typically suffer from non-negligible performance degradation under ultra-low bitwidth regimes. To bridge this gap, we propose Sparse-Compensated Vector Quantization (SCVQ), a novel framework designed for high-efficiency LLM vector quantization. SCVQ introduces a salience-aware weighted K-means clustering scheme with symmetry constraints to reduces codebook size and indexing costs. Central to our approach is a unified structured representation that consolidates outliers, salient weights, and quantization residuals into a single sparse compensation matrix. This design effectively preserves critical model information while leveraging VQ-specific properties to enable efficient custom kernels. Extensive experiments across multiple benchmarks demonstrate SCVQ’s superior performance. Specifically, SCVQ achieves a perplexity of 5.78 on WikiText-2 for LLaMA-2-7B at 2-bit quantization, while delivering a 1.4× end-to-end inference speedup over existing baselines.

---

## 论文详细总结（自动生成）

# SCVQ：面向大语言模型的稀疏补偿向量量化（论文总结）

## 1. 论文的核心问题与整体含义（研究动机）

- **背景**：大语言模型（LLM）在部署时面临严重的计算和带宽瓶颈，后训练量化（PTQ）是缓解这一问题的关键技术。当前向量量化（VQ）方法比标量量化能更好地捕捉权重结构相关性，但其存在两大核心缺陷：
  - **显式码本方法**（如 AQLM、VPTQ）：依赖大规模码本存储和 LUT 查表操作，在解码时带来大量内存带宽和索引开销，制约端到端推理吞吐。
  - **隐式码本方法**（如 QuIP#）：需要 Hadamard 变换等预处理，部署复杂且引入额外运行延迟。
- **根本矛盾**：现有 VQ 方法对所有权重一视同仁，忽略了 LLM 中少数显著权重对性能的主导性影响，在超低比特（如 2-bit）下性能退化严重。
- **研究目标**：在保持高压缩率的同时，实现高精度、硬件友好的向量量化框架。

## 2. 方法论：核心思想、关键技术细节

### 2.1 整体框架

SCVQ 基于标准 K-means 向量量化框架，首次将**稀疏补偿**与**向量量化**集成。核心思想：显式地将异常值、显著权重和量化残差从量化流程中隔离出来，存为统一的稀疏矩阵，从而用更小的码本实现更高的重建精度。

### 2.2 归一化与显著性估计

- **双归一化（Bi-Normalization）**：对权重矩阵按行、列独立缩放，使分布接近各向同性高斯分布，与 K-means 的球形聚类假设对齐，降低量化失真。
- **复合显著性因子（Salience Factor）**：由局部显著性（利用激活值 L2 范数）和全局显著性（利用梯度均值）的逐元素乘积构成，用于衡量每个权重对量化敏感性的影响。经验上显著权重沿行方向连续分布，天然与 VQ 的块分组对齐。

### 2.3 稀疏补偿矩阵

统一稀疏矩阵 C 由三部分组成：

- **异常值（W_outlier）**：L2 范数显著偏离球形分布的权重子向量；
- **显著向量（W_salient）**：显著性因子最高的子向量；
- **残差补偿（R）**：对量化残差按显著性加权的 L2 距离排序，选取 top 比例的子向量进行补偿。

三者合并后其总稀疏率远小于各自稀疏率之和（因为向量选择存在大量重叠），存储开销极小。

### 2.4 增强 K-means

- **显著性加权 K-means**：以显著性因子作为子向量的加权系数，使用显著加权 K-means++ 初始化，同时固定零向量为质心以完美重建被稀疏矩阵移出的向量。
- **对称约束 K-means**：利用双归一化后权重子向量的轴向对称性（用 Kolmogorov-Smirnov 检验自动识别最强对称维度），将对称维度的值取绝对值并存储符号位（bitmask），从而将码本大小减少 2^d̄ 倍。
- **码本量化**：对码本采用 INT8 对称逐行量化，每个质心向量独立 scale factor。

### 2.5 微调

端到端微调阶段选择两个可学习组件——**缩放向量**和**稀疏补偿矩阵的非零元素**，以全精度模型作为教师，使用置信度感知 KL 散度目标（Confidence-Aware KL Divergence，来自 BitDistiller）进行知识蒸馏微调。在 2.04 BPW 配置下仅占模型总参数约 2–3%。

### 2.6 硬件优化

- 设计了专用 LUT 解码内核，通过位运算提取符号掩码和码本索引，条件恢复对称维度符号；
- 提出 **VSR（Vector Sparse Row）稀疏格式**：相比 CSR 将列索引存储减少 d 倍（每个子向量只需一个索引），实现合并内存访问并消除 bank 冲突；
- 自定义 VSR 格式的 CUDA SpMV 内核，采用静态稀疏模式以支持前向推理和反向微调。

## 3. 实验设计

### 3.1 数据集与 Benchmark

- **困惑度评测**：WikiText-2、C4 验证集（上下文长度 4096）；
- **零样本任务**：WinoGrande（WINO）、PIQA、ARC-Easy（ARCE）、ARC-Challenge（ARCC）；
- **效率评测**：单张 NVIDIA A100 GPU 上、batch size = 1 的解码吞吐量（tokens/s）。

### 3.2 评测模型

- 主要为 LLaMA-2 系列（7B、13B、70B）；
- 附录补充 LLaMA-3.1-8B、LLaMA-3.2-3B 和 Qwen3 系列（4B/8B/14B/32B）。

### 3.3 对比方法

- 2-bit 端到端微调类：AQLM、QuIP#、VPTQ；
- 无微调 PTQ 类：GPTQ、AWQ、SpQR；
- 其他 VQ 方法：SqueezeLLM、GPTVQ。

## 4. 资源与算力

- 全部实验使用 **NVIDIA A100 GPU**（80GB），数量根据模型规模为 1 到 8 张；
- 计算局部显著性（Slocal）使用 1536 个 batch 共 6144 条 RedPajama 样本；
- 微调使用 1024 条 WikiText-2 样本，上下文长度 1024；
- **论文未明确报告具体训练时长/GPU 小时数**。设备型号和数量数据充足，但训练时间与能耗信息缺失。

## 5. 实验数量与充分性

- **主实验**：LLaMA-2 三个规模 × 2-bit 量化 × 2 项困惑度 + 4 项零样本任务，以及吞吐量对比。
- **消融实验**（表 4）：依次移除对称约束、码本量化、残差矩阵、显著矩阵、异常值矩阵、去除微调和朴素 K-means；并在 2.04 和 3.20 BPW 两种配置下进行。
- **BPW 扩展实验**（表 11/表 6）：从约 2.04 到 3.60 BPW 六档配置，覆盖 7B/13B/70B。
- **附录补充实验**：
  - 码本量化精度消融（INT4/INT8/FP16，对称/非对称）；
  - 稀疏率消融（单组件和联合组件）
  - 超参数 α 消融
  - 微调组件消融（缩放向量、稀疏矩阵、码本的不同组合）
  - 吞吐量消融（CSR vs VSR、FP16 codebook、较少对称维度）
  - LLaMA-3 和 Qwen3 系列补充评测
  - 无微调条件下与各类 PTQ 方法的对比。

**评价**：实验整体非常充分，覆盖多模型家族、多位宽、多任务、多类型消融（算法+硬件），且与多个 SOTA 基线进行了对比，具有一定的客观性和公平性。但部分消融结果（如无微调对比表 15）在正文中提及较少，且某些对比基线未统一披露微调数据规模。

## 6. 主要结论与发现

- SCVQ 在 2-bit（约 2.04 BPW）下全面优于 AQLM、QuIP#、VPTQ 等 SOTA VQ 方法：LLaMA-2-7B 的 WikiText-2 困惑度达 5.78，相比 AQLM（6.14）有显著优势；
- 在 2.04 BPW 下，LLaMA-2-7B 实现 **235.4 tokens/s** 的解码吞吐，相比 FP16 基线（103.7 tokens/s）提速约 2.3×，相比 AQLM（165.9 tokens/s）提速约 1.4×；
- 消融研究表明，**异常值矩阵、微调和显著性加权 K-means** 是性能贡献最大的三个组件；
- 对称约束可以在不损失精度的前提下显著压缩码本（如 2.04→2.16 BPW 时困惑度仅增加约 0.09）；
- VSR 格式相比 CSR 有效降低索引开销，缓解内存带宽压力，是推理吞吐提升的关键因素；
- 微调阶段只微调缩放向量+稀疏矩阵（而非码本）是最优选择，同时更新码本和稀疏矩阵会产生优化冲突。

## 7. 优点

- **创新性强**：首次将 VQ 与稀疏补偿统一为单一结构化表达，将异常值、显著权重和残差合并处理，在概念和实现上均具独创性；
- **硬件协同设计**：VSR 格式 + CUDA 内核与算法深度耦合，稀疏模式设计与 VQ 块结构天然匹配，实现了理论压缩与实际加速的统一；
- **对称约束巧妙**：利用 KS 检验自动发现归一化后权重的轴态对称性，以极小精度损失换取码本规模减半，具有普适性；
- **方法通用性**：补偿框架与具体 VQ 方案正交，可扩展至其他聚类/量化范式；
- **实验严谨**：细致区分各稀疏组件贡献，揭示组件间的重叠效应，对 BPW 计算和存储分析有清晰的公式化表述。

## 8. 不足与局限

- **理论根基薄弱**：稀疏子向量的启发式选择缺乏理论保障；α 超参数仅靠线性搜索确定；
- **实现范围受限**：由于维度灾难，只做了 d=8 的子向量实验，未拓展至更高维 VQ；
- **基础设施细节缺失**：未报告精确的训练 GPU 时长、能耗等信息，影响可复现性评估；
- **模型覆盖仍有限**：虽然补充了 LLaMA-3 和 Qwen3，但未涵盖更大规模的 MoE 或非 decoder-only 架构；
- **潜在偏差风险**：部分基线结果直接引用原文数据，微调数据规模/流程未完全统一；附录实验较多依赖同一组校准数据，可能对 WikiText-2 有轻微过拟合倾向；
- **动态场景适用性未知**：偏重 weight-only 量化场景，激活量化或 KV cache 量化的扩展未涉及。

（完）
