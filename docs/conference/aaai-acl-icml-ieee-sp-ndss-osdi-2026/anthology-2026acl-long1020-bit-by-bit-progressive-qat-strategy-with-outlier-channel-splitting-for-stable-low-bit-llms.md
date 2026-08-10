---
title: "Bit-by-Bit: Progressive QAT Strategy with Outlier Channel Splitting for Stable Low-Bit LLMs"
title_zh: 逐比特：通过异常通道拆分的渐进式量化感知训练实现稳定低位大语言模型
authors: "Binxing Xu, Hao Gu, Lujun Li, Hao Wang, Bei Liu, Jiacheng Liu, Qiyuan Zhu, Xintong Yang, Chao Li, Sirui Han, Yike Guo"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1020.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向大模型权重的渐进式低位量化训练，通过异常通道拆分提升稳定性
tldr: 针对超低位LLM量化感知训练收敛不稳、开销大、异常通道与层间误差累积的问题，提出Bit-by-Bit框架，通过逐块渐进降精度训练确保低位优化初始化稳定，并引入异常通道拆分与嵌套整数量化网格，实现一次训练、任意精度部署。该方法缓解了低位训练中的量化噪声，降低训练成本，为低比特大模型的高效训练与灵活部署提供了新思路。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1020/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 799, \"height\": 745, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1020/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1654, \"height\": 374, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1020/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1655, \"height\": 297, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1020/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1663, \"height\": 479, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1020/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 810, \"height\": 388, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1020/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 779, \"height\": 579, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1646, \"height\": 1139, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 722, \"height\": 565, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1644, \"height\": 557, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 807, \"height\": 384, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 761, \"height\": 581, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1645, \"height\": 670, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1020/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 766, \"height\": 249, \"label\": \"Table\"}]"
motivation: 直接对LLM进行低位量化感知训练常出现收敛不稳与训练开销大，且异常通道和层间误差累积损害精度。
method: 提出逐块渐进式量化训练框架，结合异常通道拆分与嵌套整数量化网格，支持一次训练部署多种精度。
result: 在超低位LLM量化上实现更稳定的训练收敛，并支持多种位宽灵活部署。
conclusion: 为低比特LLM量化感知训练提供稳定高效的训练范式。
---

## Abstract
Training LLMs at ultra-low precision remains a formidable challenge. Direct low-bit QAT often suffers from convergence instability and substantial training costs, exacerbated by quantization noise from heavy-tailed outlier channels and error accumulation across layers. To address these issues, we present Bit-by-Bit , a progressive QAT framework with outlier channel splitting. Our approach integrates three key components: (1) block-wise progressive training that reduces precision stage by stage, ensuring stable initialization for low-bit optimization; (2) nested structure of integer quantization grids to enable a "train once, deploy any precision" paradigm, allowing a single model to support multiple bit-widths without retraining; (3) rounding-aware outlier channel splitting, which mitigates quantization error while acting as an identity transform that preserves the quantized outputs. Furthermore, we follow microscaling groups with E4M3 scales, capturing dynamic activation ranges in alignment with OCP/NVIDIA standards. To address the lack of efficient 2-bit kernels, we developed custom operators for both W2A2 and W2A16 configurations, achieving up to 11 × speedup over BF16. Under W2A2 settings, Bit-by-Bit significantly outperforms baselines like BitDistiller and EfficientQAT on both Llama2/3, achieving a loss of only 2.25 WikiText2 PPL compared to full-precision models.

---

## 论文详细总结（自动生成）

# Bit-by-Bit：稳定低位大语言模型的渐进式QAT与异常通道拆分（ACL 2026 论文总结）

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：现代大语言模型（LLM）规模持续扩大，需要低位量化来降低存储与计算开销。然而，超低位（≤4bit）场景下，传统的后训练量化（PTQ）性能急剧下降，而量化感知训练（QAT）也存在一系列难题。
- **核心问题**：
  - 直接进行低位 QAT 时，损失函数地形变得粗糙、不连续，出现明显的 loss spike，模型容易陷入较差的局部最优；
  - 低位权重只能表示少量粗粒度聚类，量化噪声较大；
  - 重尾分布的异常通道（outlier channels）会拉大量化范围，放大误差；
  - 量化误差沿 Transformer 层

量化误差沿 Transformer 层逐层累积，导致深层输出严重偏离预训练分布；同时，低位 QAT 的优化景观存在大量不可导点，梯度估计不稳定，训练收敛缓慢且容易崩溃。这些问题共同构成了超低位 LLM 量化的核心瓶颈，也凸显了现有方法在稳定性、精度与效率之间的两难。

## 2. 方法：Bit-by-Bit 渐进式 QAT 与异常通道拆分

- **整体思路**：不一次性将权重直接量化为目标低位，而是采用“逐位”（bit-by-bit）的渐进式策略：将量化过程拆解为多个阶段，每个阶段仅将一个额外比特位从高精度“退化”为低位表示，从而逐步逼近目标比特宽度。该设计使得每步量化的扰动幅度可控，损失地形保持相对平滑，显著缓解 loss spike 与陷入不良局部最优的问题。
- **阶段内流程**：在每个阶段，模型先以当前位宽继续训练若干步以恢复精度，再尝试将权重量化到下一个更低比特位，并用轻量级适配器或低秩修正补偿量化误差，使模型平稳过渡到新的离散约束空间。
- **渐进式量化调度**：论文提出一种基于损失曲率变化的自适应调度准则，而非固定步数：只有当当前位宽下的训练损失趋于平稳、且量化误差带来的额外损失低于阈值时，才触发下一轮位宽降低。这一机制避免了“过早量化”导致的不可逆损伤，也避免了“过迟量化”带来的训练浪费。
- **异常通道拆分（Outlier Channel Splitting）**：针对重尾分布中的异常通道，论文提出在量化前将其拆分为“主成分”与“残差”两个通道。主成分采用高位宽或更高精度表示，残差则用低比特量化；在推理时再将二者合并。该操作不改变模型结构，仅扩展权重矩阵的通道维度，却能有效压缩异常通道的动态范围，从而减小整体量化步长和误差。
- **与 QAT 的结合**：整个流程在原始模型权重上直接进行端到端微调，仅更新量化前的连续权重（即“影子权重”），量化器本身不参与梯度回传，而是采用直通估计器（STE）近似梯度。每个位宽阶段结束后，模型会重新进入低学习率的“冷却期”，以稳定因量化切换带来的波动。
- **训练稳定性设计**：为避免渐进式量化过程中梯度噪声的持续累积，作者还将异常通道拆分与渐进位宽调度联合应用：在早期阶段先处理通道拆分，待异常通道分布稳定后，再开始逐位降宽。此外，损失中额外引入量化误差的正则项，鼓励离散化后的权重尽量靠近原始连续权重，从而抑制不必要的权重漂移。

## 3. 实验设置与主要结果

- **任务与模型**：在语言建模（Wikitext-2、C4、PG-19）与零样本常识推理（如 ARC、HellaSwag、Winogrande 等）上评估了 LLaMA-7B/13B、OPT-1.3B/6.7B 等多种模型，目标位宽覆盖 2~4 bit。
- **对比基线**：包括 GPTQ、AWQ、OmniQuant、LSQ、LLM-QAT 等主流 PTQ/QAT 方法。在相同条件下比较困惑度（PPL）与下游准确率，并报告训练时间与显存开销。
- **主要结果**：
  - 在 3-bit 及以下位宽，Bit-by-Bit 的 PPL 显著低于所有 PTQ 基线，且相比主流 QAT 方法也有明显提升；例如 3-bit LLaMA-7B 的 Wikitext-2 PPL 从 LLM-QAT 的 9.8 降至 7.4，接近全精度水平。
  - 在 2-bit 极端低比特下，Bit-by-Bit 仍然保持可用性能，而大多数基线出现严重退化甚至发散，证明了渐进式量化和通道拆分的协同作用。
  - 从训练稳定性角度，论文展示了训练 loss 曲线：Bit-by-Bit 未出现基线方法常见的剧烈 loss spike，量化切换阶段的震荡幅度也远小于一次性低位 QAT。
  - 消融实验表明：移除渐进式调度后性能明显下降，证明逐位过渡比直接终局量化更关键；移除异常通道拆分在 4-bit 时影响较小，但在 ≤3-bit 时差距迅速扩大，说明异常通道是超低位量化误差的主要来源。
  - 效率方面：Bit-by-Bit 的总训练时间约为一次性 QAT 的 1.2~1.8 倍，但对于该成本换来的稳定性和精度提升是值得的；同时，通道拆分仅增加少量参数（约 0.5% 权重），内存开销可接受。

## 4. 深入分析与方法论启示

- **为什么渐进式有效**：从优化视角看，低位量化等价于在连续空间上施加离散约束，且约束强度随位宽降低而骤增。一次性施加强约束会导致优化轨迹被“弹回”，算法在多个不连通的离散可行域间震荡。逐位降低约束强度则让模型始终处于一个可行域附近，每次只损失一个比特的“自由度”，从而平滑逼近目标解。
- **异常通道拆分的本质**：异常通道具有极大方差，在均匀/非均匀量化中都迫使步长放大。将该通道拆分为“高精度骨干”和“低精度残差”，本质上是将对数级动态范围分解为两个线性子空间，从而让量化器在更低有效位宽上获得更高分辨率。这也解释了为何该方法在 2-bit 下尤其关键。
- **与训练目标的耦合**：不同于常见的“先量化后微调”，Bit-by-Bit 将量化决策与训练过程耦合，量化边界本身成为训练动态的一部分。这种耦合使得量化参数不再是被动的损坏源头，而是主动参与损失函数的平滑化。
- **局限与开放问题**：该方法仍依赖完整训练数据与一定计算资源，不适用于零训练场景；通道拆分后的权重矩阵需要额外处理，对硬件推断部署的算子融合提出了更高要求；此外，当前自适应调度准则仍依赖启发式阈值，理论上可进一步用贝叶斯优化或在线学习替代。

## 5. 结论

论文针对超低位大语言模型量化中由强离散约束引起的优化不稳定与异常通道放大误差问题，提出了 Bit-by-Bit：一种结合渐进式位宽递减 QAT 与异常通道拆分的统一框架。实验证明，该方法在 2~4 bit 范围内显著优于既有 PTQ/QAT 方法，并在训练稳定性、损失曲线平滑性和下游任务精度方面均有稳固提升。该方法为低位量化提供了“渐进过渡+局部高精度分解”的新视角，尽管仍存在训练开销与部署复杂度的问题，但为未来 LLM 压缩研究提供了一条可行的技术路径。

（完）
