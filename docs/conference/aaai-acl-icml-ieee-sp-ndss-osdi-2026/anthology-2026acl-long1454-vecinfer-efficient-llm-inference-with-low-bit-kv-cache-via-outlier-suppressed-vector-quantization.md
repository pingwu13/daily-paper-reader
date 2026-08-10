---
title: "VecInfer: Efficient LLM Inference with Low-Bit KV Cache via Outlier-Suppressed Vector Quantization"
title_zh: VecInfer：基于离群值抑制向量量化的低比特KV缓存高效LLM推理
authors: "Dingyu Yao, Chenxu Yang, Zhengyang Tong, Zheng Lin, Wei Liu, Jian Luan, Weiping Wang"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1454.pdf"
tags: ["query:ulbv"]
score: 7.0
evidence: 用Hadamard变换抑制离群值以支持低比特向量量化
tldr: 该文面向KV cache在LLM推理中的显存瓶颈，提出VecInfer，一种用于激进KV cache压缩的向量量化方法。通过平滑和Hadamard变换抑制key cache中的离群值，使码本能够更全面地覆盖数据分布，从而降低量化误差。该方法在极低比特下也保持了较好的推理质量，并提升了推理效率，展示了Hadamard变换在低比特量化中的有效性。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1629, \"height\": 503, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 755, \"height\": 401, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 814, \"height\": 395, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1599, \"height\": 359, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1649, \"height\": 314, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 806, \"height\": 291, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 811, \"height\": 416, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 810, \"height\": 413, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1671, \"height\": 1649, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1660, \"height\": 439, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1454/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 813, \"height\": 772, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 808, \"height\": 258, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1651, \"height\": 1412, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1644, \"height\": 540, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 795, \"height\": 534, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 798, \"height\": 602, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1650, \"height\": 369, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1649, \"height\": 356, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 888, \"height\": 249, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1652, \"height\": 711, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1454/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1391, \"height\": 573, \"label\": \"Table\"}]"
motivation: KV cache显存占用大，现有VQ在超低比特下因key cache离群值性能退化。
method: VecInfer用平滑和Hadamard变换抑制key cache离群值，改善码本覆盖。
result: 在极低比特KV cache压缩下降低量化误差，保持推理质量。
conclusion: Hadamard等非相干变换可有效提升低比特向量量化的鲁棒性。
---

## Abstract
The Key-Value (KV) cache introduces substantial memory overhead during large language model (LLM) inference. Although existing vector quantization (VQ) methods reduce KV cache usage and provide flexible representational capacity across bit-widths, they suffer severe performance degradation at ultra-low bit-widths due to key cache outliers that hinder effective codebook utilization. To address this challenge, we propose VecInfer, a novel VQ method for aggressive KV cache compression while enabling efficient inference. By applying smooth and Hadamard transformations, VecInfer suppresses outliers in the key cache, enabling the codebook to comprehensively cover the original data distribution and thereby reducing quantization difficulty. To facilitate efficient deployment, we design an optimized CUDA kernel that fuses computation with dequantization to minimize memory access overhead. Extensive evaluations demonstrate that VecInfer consistently outperforms existing quantization baselines across both long-context understanding and mathematical reasoning tasks. With only 2-bit quantization, VecInfer achieves performance comparable to full precision, while delivering up to 2.7× speedup in large-batch self-attention computation and 8.3× reduction in single-batch end-to-end latency on Llama-3.1-8B with a 196k sequence length.

---

## 论文详细总结（自动生成）

## 论文总结

### 一、论文的核心问题与整体含义（研究动机和背景）

大型语言模型（LLM）在实际推理中依赖 **Key-Value（KV）缓存** 来避免重复计算注意力，但 KV 缓存的大小随序列长度线性增长，在长上下文场景下会带来严重的显存（GPU 内存）占用和计算开销问题。

KV 缓存压缩的主要技术路线包括**标量量化（SQ）** 和**向量量化（VQ）**。VQ 相比 SQ 具有更高的位宽灵活性，并能利用向量内元素间的相关性提高存储效率。然而，现有 VQ 方法在**超低比特位宽**（如 1.5 bit、2 bit）下存在显著的精度退化问题，其根本原因是 **key cache 中存在大量离群值（outliers）**——这些离群向量远离码本（codebook）中的任何质心，导致码本条目无法被充分利用，码本学习也具有很强的任务依赖性，从而大幅增加量化难度。

针对这一关键瓶颈，论文提出 **VecInfer**——一种基于**离群值抑制向量量化**的新型低比特 KV 缓存压缩方法。其核心思路是在量化前通过**平滑（Smooth）变换**和 **Hadamard 正交变换**的双重组合来抑制 key cache 中的离群值，使数据分布更均匀，让码本更好地覆盖原始数据分布；同时设计**融合反量化与注意力计算的高效 CUDA 内核**，使压缩不仅能节省显存，还能带来实际推理加速。

### 二、论文提出的方法论

#### 2.1 总体流程

VecInfer 的核心框架如图 4 所示：在 Prefill 阶段，对变换后的 key 和原始 value 分别进行向量量化；在 Decoding 阶段，对新到达的 key/value 做在线变换和量化后，与已量化部分拼接，并在注意力计算时通过融合内核完成反量与计算。

#### 2.2 双重等价变换（Dual Equivalent Transformation）

**（1）平滑变换（Smooth Transformation）**

借鉴 SmoothQuant 的计算不变性思想，对 keys 做逐通道缩放，并对 queries 施加逆缩放，以保持 QK⊤ 乘积的数学等价性：

\[
q \leftarrow q\,\mathrm{diag}(\lambda),\qquad K \leftarrow K\,\mathrm{diag}(\lambda)^{-1}
\]

其中缩放因子 \(\lambda\) 在离线校准阶段预计算：\(\lambda_i = \sqrt{\max(|K_i|)}\)。

**（2）Hadamard 变换**

平滑变换只能解决通道间的方差差异 ，无法处理通道内的方差差异（逐通道本身的极端值）。因此进一步引入正交的 Hadamard 矩阵 \(H_D\)（满足 \(H_D H_D^\top = I\)），对 queries 和 keys 同时施加旋转：

\[
q \leftarrow qH_D,\qquad K \leftarrow KH_D
\]

Hadamard 矩阵按 Walsh–Hadamard 递归结构构造。由于每个变换后元素是原始多个维度元素的加权和，由中心极限定理可近似服从高斯分布（论文 Lemma 1），从而将离群值**重新分布**到相邻元素上，获得更均匀的分布。

变换后的注意力分数可写为：

\[
s = (\underbrace{q\,\mathrm{diag}(\lambda)H_D}_{\tilde{q}}) \cdot (\underbrace{K\,\mathrm{diag}(\lambda)^{-1}H_D}_{\tilde{K}})^\top
\]

#### 2.3 向量量化 KV 缓存

- 离线阶段：从校准数据采样离群值抑制后的 keys，通过 K-Means（最多 30 次迭代）预训练 key 和 value 的码本 \(C_k, C_v\)。
- 量化阶段：Prefill 时对变换后的 keys \(\tilde{K}\) 和原始 values \(V\) 分别执行 VQ：\(\tilde{K}_q = \mathrm{VQ}(\tilde{K}, C_k)\), \(V_q = \mathrm{VQ}(V, C_v)\)。
- 解码阶段：新到 keys 在线做双重变换后，用预训练码本量化并拼接。
- 反量化阶段：注意力的 key 分支使用预计算的查找表（lookup table）替代逐元素反量化，value 分支则从码本查表获取质心后参与计算。

论文发现，经过变换之后 keys 的量化敏感性仍高于 values，因此可采用混合精度分配——为 keys 分配更高的比特位宽。

#### 2.4 硬件高效自定义内核

为实现实际推理加速，论文设计了**融合反量化和注意力计算的 CUDA 内核**，主要优化手段包括：

- **细粒度分块计算**：采用三维 grid 配置（batch_size, num_heads, num_splits），每个线程块处理一块量化的 K/V 对，计算出部分注意力输出。
- **异步流水线执行**：利用 `memcpy_async` API 使显存传输与计算重叠——计算 \(s^{(i)}\) 时异步加载 \(V_q^{(i)}\)，计算 \(o^{(i)}\) 时异步预取 \(\tilde{K}_q^{(i+1)}\)。
- **共享内存布局优化**：优化 K/V 码的共享内存排列以降低 bank conflict。

### 三、实验设计

#### 3.1 模型覆盖

论文在 6 个主流开源模型上进行了实验：

- Llama-3.1-8B-Instruct
- Mistral-7B-Instruct-v0.3
- Qwen2.5-14B-Instruct
- DeepSeek-R1-Distill-Llama-8B
- DeepSeek-R1-Distill-Qwen-14B
- Qwen3-8B

#### 3.2 任务与数据集

**长上下文任务**：使用 LongBench 的 13 个数据集，覆盖 6 大类别：

- 单文档问答：Qasper、MultiFieldQA-en
- 多文档问答：HotpotQA、2WikiMultihopQA
- 摘要：GovReport、MultiNews
- 少样本学习：TREC、SAMSum、TriviaQA
- 代码补全：LCC、RepoBench-P
- 合成任务：PassageCount、PassageRetrieval-en

**数学推理任务**：GSM8K、MATH500、AIME24、AMC2023（后两者每题生成 8 个回复），最大输出长度设 16,384 或 32,768 tokens。

#### 3.3 对比基线

- **KIVI**：主流标量量化方法（采用 bngm 标记，n 比特 + 组大小 m）
- **MILLION**：向量量化方法（采用 dnbm 标记，子向量维度 n + m 比特编码）
- 全精度 FP16 基线、SDPA、FlashAttention2

#### 3.4 主要实验配置

- 量化位宽范围为 1.25 到 4 bit
- 所有方法的 residual length 设为 128
- 校准数据：Pile 数据集中 256 个随机样本（每个 512 tokens），仅需数秒
- 码本预训练数据：Qasper 数据集，K-Means 聚类，最大 30 轮迭代

### 四、资源与算力

论文在实验部分明确提到使用了 **NVIDIA H100（80GB）和 A100（40GB）GPU**，但**未说明具体的 GPU 数量**。其实验主要聚焦于推理阶段的延迟和吞吐量评估，而非模型训练，因此训练算力需求不大。校准过程仅需数秒即可在 H100 上完成。由于论文未明确交代 GPU 数量和完整功耗/时间开销，这部分信息存在一定缺失。

### 五、实验数量与充分性

#### 5.1 实验数量

论文的实验量较丰富，具体包括：

| 实验维度 | 实验数量 |
|---|---|
| LongBench 上的精度对比（3 个模型×多档位宽） | 约 30+ 组配置 |
| 数学推理任务（3 个推理模型×4 个数据集×多档位宽） | 约 36 组配置 |
| Kernel 级性能对比（2 种 GPU×2 种 headdim×5 种 batch×4 种序列长度） | 80+ 组延迟数据 |
| 端到端解码延迟对比 | 多组（不同位宽和序列长度） |
| 消融实验（变换组合、码本大小、KV 位宽分配、码本任务无关性） | 约 20+ 组配置 |

#### 5.2 充分性判断

- **广度充分**：模型架构覆盖 7B/8B/14B，任务覆盖长上下文理解与数学推理，位宽覆盖 1.25-4 bit，对比涵盖 SQ 和 VQ 两大路线。
- **公平性较好**：与同类 VQ 方法（MILLION）在相同比特位宽和配置下对比，消融实验也细致区分了不同变换的独立贡献。
- **可改进之处**：AIME24 仅 30 题，样本量偏小；所有实验均为英文任务，未覆盖多语言场景；数学推理实验未报告 LongBench 上的所有模型在数学任务上的完整对比（如 Qwen2.5-14B 未出现在表 3 中）。

### 六、论文的主要结论与发现

- VecInfer 在 1.25 至 4 bit 的宽位宽范围内**一致优于现有基线**（KIVI 和 MILLION），在超低比特（1.5 bit）下优势尤其明显，挽救了两类基线性能几乎崩塌的场景。
- 在 2-bit 量化下，VecInfer 在 LongBench 上仅比全精度平均下降 **2.1%**，比同属 VQ 路线的 MILLION 平均提升 **14.5%**；在数学推理任务上可媲美全精度模型，而 KIVI 和 MILLION 在 2-bit 下已无法生成连贯回复。
- 效率方面：Llama-3.1-8B 在 196k 序列长度下，2-bit 配置实现 H100 大 batch self-attention 最高 **2.7×** 加速（A100 为 2.8×），单 batch 端到端解码延迟最高降低 **8.3×**；输入长度越长，加速优势越明显。
- **Hadamard 变换是离群值抑制的关键**：单独使用 Hadamard 比单独使用平滑变换带来更大提升，两者组合效果最佳。
- 码本具有**任务无关性**：在不同数据集上预训练的码本几乎不损失精度。

### 七、优点

1. **问题诊断精准**：通过可视化分析（图 1）清晰展示了 VQ 方法在低比特性能退化的根因——key cache 中的离群向量使码本条目利用率极低，切中现有方法的要害。
2. **方法设计巧妙**：利用计算不变性原理，将 SmoothQuant 和 Hadamard 旋转两种技术迁移到 VQ 场景，并辅以 SVD 几何直观解释（图 3/图 11），理论分析和实证验证相结合。
3. **兼顾精度与效率**：不仅关注压缩精度，还设计了硬件对齐的融合内核，通过异步流水线等优化真正实现了推理加速，而非停留在理论压缩率。
4. **实验系统全面**：覆盖多模型、多任务、多数据宽度、多设备，并包含充分的消融分析（变换顺序、码本大小、KV 位宽分配等）。
5. **码本可复用性**：验证了码本任务无关性（表 7），降低了下游部署的成本。

### 八、不足与局限

1. **工程集成挑战**：论文在 Limitations 部分明确指出，将 VecInfer 无缝集成到 vLLM、SGLang 等主流推理框架存在困难，因为这些框架对 KV cache 压缩缺乏原生支持或灵活 API，限制了实际部署便利性。
2. **与稀疏注意力结合未深入**：作者指出将向量量化与稀疏注意力模式结合用于混合精度 KV cache 压缩很有前景，但相关精度与效率之间的权衡尚未充分探索。
3. **模型覆盖有限**：虽然覆盖了主流开源模型，但均为 Llama/DeepSeek/Qwen 系列（基于 GQA 架构），未验证在纯 MHA 或非对称注意力架构（如编码器-解码器模型）上的通用性。
4. **数学推理评测规模偏小**：AIME24 仅含 30 题，统计波动性较大；数学推理任务未对 LongBench 上所有模型进行同等覆盖，存在实验不对称性。
5. **预填充阶段未评估**：论文的效率评估主要聚焦解码阶段的自注意力和端到端延迟，未报告预填充阶段（compute-bound）的吞吐量影响。
6. **资源信息不完整**：未披露 GPU 数量、总评估耗时长、码本训练的具体开销等，影响复现和成本估算。
7. **Hadamard 变换的限制**：Hadamard 矩阵要求维度满足 \(D = 2^k\)，对非 2 的幂次维度的模型需要额外的 padding 或截断处理，论文未讨论该场景。
8. **校准数据依赖性**：虽然码本被证明任务无关，但平滑因子的校准仍依赖 Pile 数据分布，对极端领域偏移（如代码、法律文本）的鲁棒性未经专门测试。

（完）
