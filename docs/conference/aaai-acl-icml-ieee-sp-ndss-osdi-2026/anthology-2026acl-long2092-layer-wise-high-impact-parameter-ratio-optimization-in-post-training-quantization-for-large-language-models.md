---
title: Layer-Wise High-Impact Parameter Ratio Optimization in Post-Training Quantization for Large Language Models
title_zh: 大语言模型后训练量化中层内高影响力参数比例的优化
authors: "Cuong Pham, Anh Dung Hoang, Cuong C. Nguyen, Trung Le, Gustavo Carneiro, Thanh-Toan Do"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.2092.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 按层优化高影响力参数的FP16保留比例，提升极低比特PTQ精度
tldr: 针对极低比特后训练量化下高影响力参数导致精度损失的问题，论文提出二次规划框架，为每层自适应计算需要保留为FP16的高影响力参数比例。相比固定比例方案，该方法更好地感知层间敏感性差异，在超低比特下显著减少精度下降。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long2092/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1656, \"height\": 547, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long2092/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1662, \"height\": 1188, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long2092/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1659, \"height\": 1475, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long2092/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1640, \"height\": 627, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1483, \"height\": 745, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1487, \"height\": 576, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1571, \"height\": 351, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1406, \"height\": 449, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 767, \"height\": 196, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 740, \"height\": 304, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 807, \"height\": 220, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long2092/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1450, \"height\": 973, \"label\": \"Table\"}]"
motivation: 现有保留高影响力参数为FP16的PTQ方法对各层采用固定比例，忽视层间敏感性差异。
method: 建立二次优化模型，在整体约束下求解每层高影响力参数的最优保留比例。
result: 在极低比特设置下相比固定比例基线大幅降低精度损失。
conclusion: 层自适应的高精度参数保留能有效支撑LLM超低比特部署。
---

## Abstract
Large language models (LLMs) have advanced natural language processing, but their massive parameter counts create computational and memory challenges during deployment. Post-training quantization (PTQ) has emerged as a promising approach to mitigate these challenges. While existing PTQ methods can effectively quantize LLMs, they experience substantial accuracy loss at extremely low bit-widths due to high-impact parameters. Several approaches address this by retaining high-impact parameters in FP16 format, but they apply fixed ratios across all layers, overlooking layer-wise sensitivity variations. We propose a quadratic optimization framework that determines layer-specific ratios of high-impact parameters while considering inter-layer dependencies. We quantize high-impact parameters to moderate bit-widths while the remaining parameters are quantized to extremely low bit-widths. Under the same resource budget, this preserves more high-impact parameters than methods retaining a few in FP16 format. Our framework enables leveraging advanced quantization methods for high-impact parameters while applying lightweight computational quantization methods to the rest, achieving an effective balance between computational efficiency and accuracy during quantization process.

---

## 论文详细总结（自动生成）

## 论文中文总结

### 1. 核心问题与整体含义

- **研究背景**：大语言模型（LLMs）参数规模庞大，部署时面临严重的计算和内存挑战。后训练量化（PTQ）是缓解这一问题的有效手段，但现有 PTQ 方法在极低比特宽度（如 2-bit 权重量化）下会遭遇显著的精度损失。
- **现存问题**：导致精度损失的关键因素之一是**高影响力参数**（high-impact parameters），即对量化误差极为敏感的参数。已有方法（如 AWQ、CherryQ、SqueezeLLM）通过将少数高影响力参数保留为 FP16 或高精度格式来缓解问题，但普遍采用**跨层固定比例**，忽略了不同层之间参数重要性的显著差异。
- **核心含义**：论文提出按层自适应地确定高影响力参数的保留比例，在固定资源预算下更合理地分配量化精度资源，从而在极低比特宽度下显著降低精度损失。

### 2. 方法论

- **核心思想**：
  - 利用 Fisher Information 近似 Hessian 矩阵的对角元，作为每个参数的影响力分数（impact score）。
  - 将每一层中影响力最高的参数以中等比特宽度（如 3-bit/4-bit）量化，其余参数以极低比特宽度（如 2-bit）量化。
  - 通过**二次优化（quadratic optimization）** 确定每层高影响力参数的最优比例，并考虑同 block 内层间的相互依赖。
- **关键技术细节**：
  - 用泰勒展开将量化损失变化近似为 \(\frac{1}{2}\Delta^\top H \Delta\)。
  - 定义每层的 one-hot 选择向量 \(\delta_l\)，将整个网络的混合量化误差转化为 \(\delta^\top M \delta\) 的二次目标函数。
  - 通过对角元近似和非对角元近似构造矩阵 \(M\)，不同 block 之间的交互置零，同 block 内通过极化恒等式计算交叉项。
  - 在显式内存预算约束下，通过求解约束二次整数优化问题获得最优比例向量 \(\delta\)。
  - 最后采用**混合量化策略**：高影响力参数使用 AdaRound（可学习舍入矩阵优化），普通参数使用 OmniQuant 的 learnable weight clipping，兼顾精度与计算效率。

### 3. 实验设计

- **数据集**：
  - 语言建模（困惑度评估）：WikiText-2 和 C4。
  - 零样本下游任务：HellaSwag、PIQA、WinoGrande、ARC-easy、ARC-challenge。
  - 校准数据：从 WikiText-2 中随机采样 128 条序列，每条 2048 tokens。
- **模型**：LLaMA-2-7B、LLaMA-2-13B、OPT-125M。
- **对比方法**：GPTQ、AWQ、OmniQuant、SqueezeLLM、CBQ，以及全精度 FP16 基线。
- **量化配置**：W2A16、W3A16，支持 per-group（g64/g128）和 per-channel 量化。

### 4. 资源与算力

- 文中仅提及实验在 **NVIDIA A100 GPU** 上完成，**未明确说明 GPU 数量、训练时长或总计算量**。
- 高影响力参数优化需要大量前向评估（每层 × 每个候选比例），因此实际算力需求较高，但论文未给出具体量化数据。

### 5. 实验数量与充分性

- **实验数量**：
  - 核心实验包括 2-bit/3-bit 量化在 LLaMA-2-7B/13B 上的语言建模困惑度（表 1、2）。
  - 零样本下游任务精度评估（表 3）。
  - 消融实验：验证最优比例 vs 固定比例、混合量化策略 vs 单一量化的贡献（表 4）。
  - 附录补充实验：OPT-125M 上的 2-bit 量化、候选比例集合敏感性、不同校准数据集的稳定性、不同高影响力比特宽度（16/4/3）在相同预算下的对比。
- **充分性评价**：
  - 实验设计较为全面，覆盖了主流模型、数据集和 SOTA 对比方法，并通过消融实验验证了各模块的有效性。
  - 存在一定局限：模型覆盖主要集中在 LLaMA-2 系列和 OPT-125M，未涉及更大规模（如 70B）或更多样化的模型架构（如 Mistral、Qwen）；实验以英文文本为主，未覆盖多语言场景。

### 6. 主要结论与发现

- 在极低比特量化（尤其是 W2A16）下，提出的方法显著优于 GPTQ、OmniQuant、CBQ 等 SOTA 方法。
- **层自适应比例**优于固定比例：在 W2A16 下，平均下游任务精度提升约 2.4%，困惑度降低 0.15（相比固定比例方案）。
- **混合量化策略**进一步带来提升：结合优化后的比例与混合量化，相比仅固定比例方案，平均精度提升 2.76%，困惑度降低 1.22。
- 在同一资源预算下，将高影响力参数量化到 3-bit/4-bit（而非保留 FP16）可以保留更多高影响力参数，获得更好性能。
- 提出的方法有效缩小了量化模型与全精度模型之间的性能差距（W3A16g128 下差距约 4%）。

### 7. 优点

- **方法创新性强**：将高影响力参数比例选择建模为带约束的二次规划问题，数学框架清晰、原理扎实。
- **考量层间差异**：首次在 PTQ 框架中系统性地按层优化高影响力参数比例，避免固定比例子优。
- **混合精度策略灵活高效**：对高影响力参数用 AdaRound、对其余参数用轻量级方法，在精度和计算开销之间取得良好平衡。
- **实验严谨**：设计多组消融实验验证每个模块的贡献，且对超参数（候选比例集合、高影响力比特宽度）进行了敏感性分析。

### 8. 不足与局限

- **手工设定高影响力比特宽度**：论文承认高影响力参数的比特宽度 \(b_H\) 是手动指定的，未必是资源约束下的最优选择，缺乏自适应调节机制。
- **近似假设的局限**：利用 Fisher Information 近似 Hessian，并假设跨 block 的层间交互为零，实际可能存在一定的精度损失。
- **优化复杂度较高**：构建矩阵 M 需要大量的前向评估（每个层 × 每个候选比例），在层数极多或 block 内层数较多的模型中开销较大。
- **候选比例集合影响**：实验结果对候选比例集合 B 的粒度有一定敏感性，粗粒度集合可能影响最优解的精度。
- **资源信息不透明**：未报告完整的训练/优化时间和 GPU 资源消耗，难以评估实际部署可行性。

（完）
