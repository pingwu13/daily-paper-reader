---
title: "MemeBQ:Memory Efficient Binary Quantization of LLMs"
title_zh: MemeBQ：大语言模型的高内存效率二值量化
authors: "Yuanhui Wang, Kunlong Liu, Minnan Pei, Zhangming Li, Peisong Wang, Qinghao Hu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39881/43842"
tags: ["query:ulbv"]
score: 9.0
evidence: 面向 LLM 的二值后训练量化，降低辅助标志位图的内存开销
tldr: 二值后训练量化虽大幅降低大语言模型的存储与计算开销，但现有方法往往引入额外的标志位图等辅助内存，且仍存在明显精度损失。MemeBQ 通过贪婪行聚类方法利用权重行间相似性，在不增加辅助存储的前提下压缩权重。该方法在保持二值化极限压缩能力的同时减少了部署内存，并缓解了精度下降。实验结果显示其内存效率优于现有二值 PTQ，推动了低比特 LLM 在实际设备上的应用。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39881/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 792, \"height\": 375, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39881/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1541, \"height\": 747, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 740, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1562, \"height\": 555, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1699, \"height\": 1013, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 835, \"height\": 319, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 803, \"height\": 301, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1845, \"height\": 818, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39881/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 881, \"height\": 400, \"label\": \"Table\"}]"
motivation: 现有二值 PTQ 方法虽压缩权重，但辅助标志位图带来额外内存开销且精度损失大。
method: 提出 MemeBQ，利用贪婪行聚类对权重进行划分，消除辅助标志位图的内存开销。
result: 实验表明该方法在降低内存的同时缓解二值化带来的精度下降，优于现有二值 PTQ。
conclusion: MemeBQ 为二值 LLM 部署提供了更省内存的高效方案。
---

## Abstract
Recent years have witnessed growing scholarly interest in binary post-training quantization (PTQ) techniques for large language models (LLMs). While state-of-the-art (SOTA) binary quantization methods significantly reduce memory footprint and computational demands, they introduce additional memory overhead beyond binary weight tensors to mitigate performance degradation. Moreover, binary LLMs still suffer from substantial accuracy loss.
To address these limitations, we propose MemeBQ, a novel binary PTQ framework for LLMs that reduces the memory overhead of auxiliary flag bitmaps in existing binary quantization methods. Specifically, we first design a greedy row clustering method, which leverages  the similarity between  the row vectors of weights to partition  the weight rows  into different groups. By sharing the common flag bitmap  within each row  group, we significantly mitigate the memory overhead associated with flag bitmaps. Besides, to improve the performance of binary LLMs, we propose a novel weight splitting  method for each row group of weights, which determines the flag bitmap's values in a fine-grained way.
Extensive experiments on OPT, Llama-2, and Llama-3 models   demonstrate that  MemeBQ reduces 50% extra memory demand while achieving comparable accuracy compared with current SOTA methods. Alternatively, MemeBQ outperforms SOTA binary quantization methods up to 7%   with the same extra bits on reasoning benchmarks.

---

## 论文详细总结（自动生成）

## MemeBQ：大语言模型的高内存效率二值量化

### 1. 论文的核心问题与整体含义（研究动机和背景）

近年来，大语言模型（LLM）参数量不断膨胀（如 Llama-2 70B），推理需要大量计算与存储资源，难以在边缘设备或低资源环境中部署。后训练量化（PTQ）因无需重训练/微调即可直接压缩预训练模型而备受关注，其中的**二值后训练量化（Binary PTQ）** 更是将权重压缩至约 1 bit，显著降低内存占用。

然而，现有 SOTA 二值量化方法（如 PB-LLM、BiLLM、ARB-LLM）普遍存在两个问题：

- 在二值权重张量之外，还需要额外存储**标志位图（flag bitmap）** 或掩码来记录权重划分信息（例如哪些权重是显著/非显著、稀疏/集中分布等），这部分额外开销约为 1 bit/参数，在超大规模模型上不可忽视。
- 二值量化本身仍存在明显的精度损失，现有方法在压缩与性能之间尚未取得令人满意的平衡。

本文的核心意义在于：**在不牺牲精度的前提下，降低二值量化所需的额外内存开销，使 LLM 的极低比特部署更加实用、高效。**

### 2. 论文提出的方法论：核心思想、关键技术细节

#### 核心思想

论文从两个关键观察出发：

- **观察一**：权重矩阵的各个行向量之间存在很高的相似性（例如 Llama2-7B 中，某一权值行与其最相似行的余弦相似度可达 85% 以上），这意味着相似行对应的标志位图存在大量冗余。
- **观察二**：先前方法使用**全局阈值**对非显著权重进行划分（稀疏/集中），在相似行中效果尚可，但在不相似的行上会造成更多量化误差。更细粒度的"按行分组"划分方式可能降低量化误差。

基于此，提出 **MemeBQ** 框架，由两个核心模块组成：

#### （1）相似性搜索（SimS）

**目标**：通过行聚类减少标志位图的存储开销。

**流程**：

1. 沿用前人做法，使用 Hessian 矩阵计算每个权重的量化敏感度 \( e_i = w_i^2 / [H^{-1}]_{ii}^2 \)，据此将权重矩阵划分为显著权重列与非显著权重列。
2. 对显著/非显著权重矩阵分别构建**行间余弦相似度矩阵** \( S \in R^{n×n} \)。
3. 采用**贪婪算法**：每次从未分组的行中，寻找与当前行余弦相似度最高的行，构成一组（每组大小为 g），并将组内所有行分配**同一个组索引**（存入 \( \tilde{m}_g \)）。
4. 额外存储重排索引 \( \tilde{m}_r \) 以支持后续 GBS 模块的子张量访问。

**效果**：一个组内所有行共享同一份标志位图，标志位图的大小从 \( n \times m \) 缩减为 \( (n/g) \times m \)，即减少了 `g` 倍内存。

#### （2）组内位图搜索（GBS）

**目标**：在共享位图的前提下，尽量让位图划分结果符合原始权重分布，从而弥补共享带来的精度损失。

**流程**：

1. 利用 \( \tilde{m}_r \) 对权重矩阵进行重排，并按组大小 g 进行 reshape/批处理，得到形如 \( R^{n_g \times m \times g} \) 的张量。
2. 对每组内的**每一列（通道）** 应用基于欧氏距离的 **k-means 聚类**（每批 2 个簇中心，即二值划分）。聚类目标函数为最小化组内各样本到簇中心的平方距离之和，并以迭代方式更新簇中心。
3. 根据聚类标签生成最终标志位图 \( M \in R^{n_g \times m} \)，分别对应显著/非显著权重区域。
4. 随后沿用 ARB-LLM RC 的方法，对显著列进行残差量化（获得 \( B_1, B_2 \)），对非显著列进行 1-bit 量化（获得 B），并计算精炼缩放因子 \( \alpha_{refine} \) 和零点 \( \mu_{refine} \)，最终量化权重为 \( \hat{W} = \alpha_{refine} \cdot B_{refine} + \mu_{refine} \)。

#### 内存公式（简化说明）

相比 ARB-LLM RC，MemeBQ 总共减少的存储量为 \( (n - n/g) \times m - (m/b) \times 8n \) bits。以具体模型为例，MemeBQ 实际存储约 2.22GB，而 ARB-LLM 约 2.69GB，证明了内存效率的提升。

### 3. 实验设计

#### 模型与数据集

- **模型**：OPT（1.3B / 6.7B / 66B）、Llama-2（7B / 13B / 70B）、Llama-3（8B / 70B）。
- **校准数据**：从 C4 数据集随机采样 128 条句子。
- **验证集**：
  - 困惑度（PPL）评估：WikiText2 和 C4 数据集。
  - 零样本问答评测：PIQA、BoolQ、OBQA、Winogrande、ARC-e、ARC-c、Hellaswag 共 7 个推理基准。

#### 对比方法

- RTN（round-to-nearest，2/3-bit）
- GPTQ（2/3-bit）
- PB-LLM、BiLLM、ARB-LLM RC（二值 PTQ 方法）

统一设定 block size = 128。

#### 实验分组

| 实验类型 | 评估维度 | 内容 |
|---------|---------|------|
| 主实验（PPL） | 语言建模困惑度 | OPT 全家桶（表 2）、Llama 系列（表 3） |
| 零样本 QA | 7 个推理任务平均准确率 | Llama-2-7B、Llama-3-8B、OPT-6.7B（表 4） |
| 削融实验 1 | 各模块有效性 | SimS 与 GBS 的开关组合（表 5） |
| 削融实验 2 | SimS 内部设计 | 聚类方式、相似度度量、与 MTN 基线对比（表 6） |
| 削融实验 3 | 组大小（groupsize）影响 | groupsize = 0/1/2/4 的精度与开销权衡（表 7） |

### 4. 资源与算力

- 单张 **NVIDIA A800（80GB）** GPU 即可完成逐层量化，包括 Llama-3-70B 在内的大模型也不需要多卡。
- 校准时间（以 LLaMA 为例）：
  - Llama2-7B：ARB-LLM 约 11,902s，MemeBQ 约 12,975s（增加约 9%）。
  - Llama2-13B：ARB-LLM 约 24,924s，MemeBQ 约 25,196s（增加约 1.1%）。
- 论文未说明 GPU 数量、总训练/校准 GPU 时数、能耗等细节，仅给出上述时间数据。

### 5. 实验数量与充分性

#### 实验数量

- **PPL 主实验**：OPT 3 个规模 + Llama 系列 5 个规模，均为多种方法横向对比，覆盖范围较广。
- **零样本评测**：3 个代表性模型 × 7 个数据集，全面评估推理能力。
- **削融实验**：3 组独立削融，分别验证模块贡献、聚类算法选型、组大小敏感性，结构完整。

#### 充分性与客观性评价

- **优点**：模型家族覆盖面广（OPT / Llama-2 / Llama-3），规模跨度大（1.3B 至 70B）；同时用 PPL 与 QA 准确率两种互补的评估指标；削融实验设计清晰，能够分离 SimS 与 GBS 各自的作用。
- **不足**：实验仅在英文数据集上进行；没有报告推理时的实际硬件加速比/吞吐量（只报告了存储量）；对 Llama-3-13B 标记为 N/A（该版本不存在）。整体而言，实验是较充分且公平的，但缺少部署侧（如推理延迟、端到端内存占用）的验证。

### 6. 论文的主要结论与发现

1. **内存效率**：MemeBQ 在保持与 ARB-LLM 相当精度的情况下，将额外标志位图开销减少 50%（即从 1.0 bit 降至 0.5 bit）。
2. **性能提升**：在相同比特数（1.08 bit 权重 + 1.0 bit 额外位）下，MemeBQ 大幅超越先前 SOTA：
   - Llama-2-7B PPL 从 16.44 降至 8.77；
   - Llama-3-8B PPL 从 27.42 降至 13.38；
   - 零样本评测平均准确率在 Llama-3-8B 上比 ARB-LLM 提高 7.27 个百分点。
3. **极限压缩优势**：在 OPT-66B 上，MemeBQ 总位宽约 2.01 bits，PPL 甚至优于 3-bit GPTQ；在 Llama-2-13B/70B 上超过 3-bit RTN。
4. **削融验证**：
   - 单独使用 SimS 会带来精度下降（PPL 16.44 → 21.67），符合"没有免费午餐"原则；
   - 单独使用 GBS 能大幅提升精度（PPL 16.44 → 8.77）；
   - 两者结合时，以 1.58 bits 总位宽达到与 ARB-LLM 2.08 bits 相当的精度（16.73 vs 16.44）。
5. **组大小权衡**：groupsize=2 时，以 0.5 bit 额外开销达到与 ARB-LLM 相当精度；groupsize=4 时，以 0.25 bit 额外开销即可匹配 BiLLM 的精度水平。

### 7. 优点

- **问题定位精准**：明确指出二值量化中"标志位图内存开销"这一此前被低估的瓶颈，并针对性地解决了它。
- **方法简洁有效**：SimS + GBS 两个模块分工明确——一个负责降内存、一个负责保精度，设计可解释性强。
- **利用数据内在结构**：通过行相似性聚类来共享位图，是一种资源高效且自然的思路；GBS 使用 k-means 进行细粒度地划分，比全局阈值方法更贴合局部权重分布。
- **落地方便**：采用逐层量化，单张 80GB GPU 即可处理 70B 模型，工程上可行性强。
- **实验覆盖面广**：模型、规模、数据集、评测指标、消融维度都较完整；同时公开代码，可复现性强。
- **内存公式清晰**：理论分析给出了与先前方法的明确内存差量，说服力强。

### 8. 不足与局限

- **阶段局限**：仅压缩权重，没有考虑激活量化，对实际端侧推理的加速效果描述不足。
- **精度仍非无损**：虽然优于 ARB-LLM，但在 0.5 bit 额外位配置下，零样本准确率仍有明显退化（如 Llama-3-8B 平均 59.54% → 47.36%），说明降低辅助位仍有代价。
- **额外索引也有开销**：SimS 中引入的组索引 \( \tilde{m}_g \) 和重排索引 \( \tilde{m}_r \) 虽然用 FP8 存储，但在极端低比特场景下仍不可忽略，且论文未讨论这些索引自身的压缩方案。
- **内存公式中位数偏差风险**：显著列的比例 l、组大小 g 等超参数对不同模型/任务的最优值可能不同，论文未做充分的敏感性分析。
- **评估局限性**：
  - 仅使用 PPL 和 7 个英文 QA 数据集，未涉猎代码生成、数学推理、多语言等任务；
  - 未报告部署侧数据（实际推理延迟、吞吐量、端到端内存占用）。
- **校准成本增加**：由于 GBS 引入了 k-means 聚类，校准时间比 ARB-LLM 高（7B 上约 9% 增加），虽可接受但若应用于更大模型群组（如按周/月级校准）成本仍会累积。
- **对硬件/算子支持依赖**：二值网络通常需要专用内核才能发挥硬件加速优势，论文未讨论在现有商业硬件上的实际加速效果。

（完）
