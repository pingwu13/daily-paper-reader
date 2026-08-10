---
title: "BTC-LLM: Efficient Sub-1-Bit LLM Quantization via Learnable Transformation and Binary Codebook"
title_zh: BTC-LLM：基于可学习变换与二进制码本的高效亚1比特LLM量化
authors: "Hao Gu, Lujun Li, Hao Wang, Lei Wang, Zheyu Wang, Bei Liu, Jiacheng Liu, Qiyuan Zhu, Sirui Han, Yike Guo"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1066.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 利用二进制码本与可学习变换实现亚1比特LLM量化
tldr: 该文针对稀疏感知二值化在亚1比特量化中性能退化、掩码管理开销大且硬件兼容性有限的问题，提出BTC-LLM框架。该方法通过二进制码本对重复向量进行聚类，使用自定义距离度量和符号更新生成紧凑索引，并结合可学习变换进一步增强表达能力。实验表明该框架在亚1比特压缩下取得了更好的性能与硬件兼容性，为极低比特LLM量化提供了新方向。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 792, \"height\": 697, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1636, \"height\": 399, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 723, \"height\": 553, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1654, \"height\": 1025, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1653, \"height\": 448, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1466, \"height\": 2554, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1460, \"height\": 2523, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1329, \"height\": 2545, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1066/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1325, \"height\": 2565, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1647, \"height\": 1015, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1645, \"height\": 522, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 512, \"height\": 200, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 695, \"height\": 197, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 794, \"height\": 322, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 795, \"height\": 269, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1475, \"height\": 201, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 796, \"height\": 510, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1649, \"height\": 1733, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1066/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1649, \"height\": 971, \"label\": \"Table\"}]"
motivation: 稀疏感知二值化存在性能退化、掩码管理开销和硬件兼容性问题。
method: 构建二进制码本聚类权重向量，并引入可学习变换增强表征能力。
result: 在亚1比特压缩下实现更好性能和硬件兼容性。
conclusion: 二进制码本与可学习变换为极低比特LLM量化提供了高效新途径。
---

## Abstract
Binary quantization represents the most extreme form of compression, reducing weights to ± 1 for maximal memory and computational efficiency. While recent sparsity-aware binarization achieves sub-1-bit compression via weight pruning, it faces critical challenger: performance degradation, mask-management overhead, and limited hardware compatibility. In this paper, we present BTC-LLM, a novel sub-1-bit LLM quantization framework that leverages binary pattern clustering and weight transformation to overcome these limitations. Our approach incorporates two key innovations: (1) a Binary Codebook that clusters recurring vectors into compact indices using custom distance metrics and sign-based updates; (2) a Learnable Transformation that reduces outliers and promotes shared sign patterns among binary weights. This eliminates sparse masks, enabling efficient inference on standard hardware. Extensive evaluations across LLaMA, Qwen, and FBI-LLM families demonstrate that BTC-LLM achieves state-of-the-art results in extreme compression (1.11–0.7 bits). Notably, BTC-LLM compressed to 0.8 bits on LLaMA-2-13B maintains high performance—with only a 3.1% accuracy drop in zero-shot benchmarks—while delivering a 1.6 × speedup over FP16.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- 大语言模型（LLM）规模不断增大，部署时对内存和计算资源要求极高，模型量化成为关键压缩手段。
- 二进制量化将权重量化为 ±1，是压缩最激进的方案，能最大程度降低存储和计算开销。
- 近期“稀疏感知二值化”（sparsity-aware binarization）通过剪枝实现亚 1 比特压缩，但存在三个关键挑战：
  - **性能退化**：极端压缩导致模型精度明显下降；
  - **掩码管理开销**：剪枝产生的稀疏掩码需要额外存储和计算，抵消部分压缩收益；
  - **硬件兼容性有限**：稀疏格式与标准硬件（如 GPU 矩阵运算单元）不友好，难以高效推理。
- 论文提出 **BTC-LLM** 框架，旨在克服上述问题，实现真正高效、硬件友好的亚 1 比特 LLM 量化，为极低比特压缩提供新方向。

## 2. 论文提出的方法论：核心思想、关键技术细节

- 总体思路：放弃稀疏掩码方式，改用**二进制模式聚类**与**权重变换**来提升亚 1 比特量化的表达能力和硬件效率。
- 两大核心创新：
  - **二进制码本（Binary Codebook）**
    - 将重复出现的权重组（向量）聚类到一组紧凑的索引中；
    - 使用**自定义距离度量**衡量原始权重向量与码本条目之间的相似程度；
    - 通过**符号更新**方法迭代优化码本，使码本适应权重分布。
  - **可学习变换（Learnable Transformation）**
    - 对权重施加一个可学习的变换，减少离群值影响，使权重分布更规整；
    - 促进二值化后的权重形成一致的符号模式，从而提高码本聚类的效率和量化精度。
- 关键效果：
  - 由于不再需要稀疏掩码，模型可以在标准硬件上直接高效推理；
  - 通过变换+码本组合，在极端压缩下保留更多信息，缓解性能退化。

## 3. 实验设计：使用的数据集 / 场景 / benchmark / 对比方法

- 评估模型家族：**LLaMA**、**Qwen**、**FBI-LLM** 系列，覆盖不同规模与架构。
- 压缩目标：极端压缩范围 **1.11–0.7 bits**（即平均每权重比特数低于 1）。
- Benchmark：摘要中明确提及 **zero-shot benchmarks**（零样本任务评估），用于衡量量化后模型的下游任务表现；具体数据集列表（如常识推理、语言建模等）在摘要中未列出。
- 对比方法：摘要未列出具体对比方法名，但声称达到“state-of-the-art results”，说明与当前主流亚 1 比特量化方法进行了比较。
- 代表结果：LLaMA-2-13B 压缩至 **0.8 bits** 时，零样本基准准确率仅下降 **3.1%**，并相对 FP16 实现 **1.6× 推理加速**。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明**训练/量化所需的 GPU 型号、数量、时长、数据集规模等资源信息。
- 仅提供了推理加速数据（相对 FP16 的 1.6× 加速），但没有给出硬件环境细节。
- 因此无法从现有材料评估算力成本，需要查阅论文正文补充。

## 5. 实验数量与充分性

- 从论文元数据看，包含 **10 个表格**和 **9 张图**，暗示实验内容较为丰富，可能涵盖：
  - 不同模型规模（如 LLaMA-2-13B）下的量化性能；
  - 不同比特率（1.11–0.7 bits）的对比；
  - 与基线方法的性能比较；
  - 消融实验（验证码本和变换的贡献）等。
- 覆盖多个主流模型家族（LLaMA、Qwen、FBI-LLM）和多个压缩级别，具有一定代表性。
- 但摘要本身未给出详细实验清单，无法确认是否有跨任务、跨语言、长文本或更多基准测试。总体来看，仅凭摘要判断实验设计较为充分，但**具体公平性需依赖正文中的基线设置和统计信息**。

## 6. 论文的主要结论与发现

- BTC-LLM 在亚 1 比特量化下取得了当前最优（SOTA）表现，压缩范围 1.11–0.7 bits。
- 二进制码本 + 可学习变换能够有效替代稀疏掩码，既避免了掩码管理开销，又提升了硬件兼容性。
- 在极端压缩场景下，BTC-LLM 能保持较高的模型精度，例如 LLaMA-2-13B 在 0.8 bits 下仅损失 3.1% 的零样本准确率。
- 同时带来明显的推理加速（1.6× vs FP16），证明方法不仅压缩有效，而且在部署效率上也有收益。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：将码本聚类与可学习变换结合用于亚 1 比特量化，不同于传统的稀疏掩码路线，思路新颖。
- **硬件友好**：消除稀疏掩码后，模型可在标准硬件（如通用 GPU）上高效运行，实用性强。
- **压缩效率高**：达到 sub-1-bit 级别（甚至低至 0.7 bits），且性能损失可控。
- **验证充分**：在多个主流 LLM 家族上验证，且包含多个模型尺寸和比特率设置，结果具有广泛意义。

## 8. 不足与局限

- 摘要中未报告具体的数据集列表、基线方法与详细误差，难以独立评估实验的全面性和公平性。
- 只报告了 zero-shot benchmarks，未提及训练后量化是否影响指令跟随、推理、长文本生成等更广泛的能力。
- 算力资源（GPU 型号、数量、耗时）未披露，难以判断方法在真实部署中的成本。
- 硬件兼容性验证仅提及“标准硬件”，未详细说明是否覆盖 CPU、移动端、专用 AI 芯片等不同平台。
- 缺少与其他亚 1 比特方法在同等比特率下的精确性能对比表和统计显著性检验。
- 该方法是否适用于多模态 LLM 或更大规模模型（如 70B 以上）尚未从摘要看出。

（完）
