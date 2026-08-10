---
title: "ACBQ: Adaptive Cross-Block Quantization of Large Language Models"
title_zh: ACBQ：大语言模型的自适应跨块量化
authors: "Hailing Wang, Jianglin Lu, Yitian Zhang, Huimin Zeng, Yun Fu"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1971.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 支持极低比特权重与联合量化的后训练量化框架
tldr: 该文指出现有PTQ方法难以同时支持权重-激活联合量化和极低比特权重量化，原因是深层Transformer中量化误差跨层传播。为此提出ACBQ框架，通过跨块自适应机制协同处理层间依赖，抑制误差累积。该框架在极低比特权重和联合量化设置下均显著提升了性能，为LLM部署提供了一种统一而高效的PTQ方案。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1971/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 619, \"height\": 409, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1971/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 746, \"height\": 634, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1971/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 658, \"height\": 657, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1971/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 787, \"height\": 681, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1658, \"height\": 1044, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1611, \"height\": 651, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 820, \"height\": 645, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1619, \"height\": 249, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 833, \"height\": 271, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1661, \"height\": 1088, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1661, \"height\": 1095, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1660, \"height\": 1026, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1662, \"height\": 1073, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1660, \"height\": 1026, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1663, \"height\": 1073, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1662, \"height\": 1076, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1655, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long1971/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1630, \"height\": 361, \"label\": \"Table\"}]"
motivation: 现有PTQ难以同时支持权重-激活联合量化与极低比特权重量化。
method: ACBQ提出跨块自适应机制，处理层间依赖并抑制误差累积。
result: 在极低比特权重和联合量化设置下均显著改善性能。
conclusion: 跨层误差感知的自适应量化可统一支撑多种低比特压缩需求。
---

## Abstract
Post-training quantization (PTQ) has emerged as a promising approach for reducing the memory footprint and computational cost of large language models (LLMs), enabling efficient deployment without full model retraining. However, existing PTQ methods struggle to simultaneously support weight–activation joint quantization and extreme low-bit weight quantization. This limitation primarily arises from the depth of LLMs and their strong cross-layer dependencies, which cause quantization errors to propagate and accumulate across layers, ultimately leading to significant performance degradation. In this paper, we present ACBQ, a simple yet effective framework that simultaneously addresses weight–activation joint quantization and extreme weight quantization. We first propose a granular quantization strategy that treats self-attention and FFN as separate quantization units with module-specific optimization objectives. To mitigate the propagation and accumulation of quantization errors across layers, we introduce an adaptive cross-block quantization strategy that explicitly accounts for cross-layer dependencies by encouraging consistency across blocks. Extensive experiments across diverse LLMs, including OPT and the LLaMA family, demonstrate that ACBQ achieves superior performance under both W4A4 and highly aggressive W2 settings, while incurring negligible additional computational overhead.

---

## 论文详细总结（自动生成）

# ACBQ：大语言模型的自适应跨块量化（论文详细总结）

## 一、核心问题与研究动机

- **背景**：大语言模型（LLM）参数量巨大，推理时的内存占用和计算开销极高（如 GPT-3 需数百 GB 内存）。后训练量化（PTQ）通过将高精度权重/激活转换为低比特表示（如 int4），可将内存占用降低最高 8 倍，且不需要完整重训练，仅需少量校准数据，因此成为高效的模型压缩部署方案。
- **核心难题**：现有 PTQ 方法难以**同时**支持两类关键需求——① 权重-激活联合量化（如 W4A4）与 ② 极端低比特权重量化（如 W2）。其根本原因在于 LLM 深度极深、层间依赖性强，量化误差会随层数逐层**传播和累积**，最终导致模型性能显著退化。
- **现有方法的不足**：
  - SmoothQuant、QuaRot、SpinQuant 等通过重缩放、旋转变换处理激活离群值，但**未显式建模跨层误差累积**，在 W2 等极端低比特场景下性能有限。
  - OmniQuant 虽在可微分框架中做块级重建，但同样**未显式建模跨层依赖**。
  - CBQ 虽部分考虑了跨层依赖，但依赖重叠块窗口，导致优化成本和内存消耗显著增加。
- **本文目标**：提出一个简单而有效的统一框架 ACBQ，同时解决权重-激活联合量化与极端低比特权重量化问题，且额外计算开销可忽略。

## 二、方法论

### 1. 模块级优化（Module-Wise Optimization, MWO）——块内粒度细化

**动机（三个关键洞察）**：
- 自注意力（Self-Attention）与 FFN 功能角色不同：注意力负责跨 token 的全局信息聚合，FFN 逐 token 独立增强表征；块级统一重建损失会忽视这种功能分离。
- 残差连接分别作用于 Self-Attention 和 FFN 两条独立路径（信息旁路），量化策略应尊重这种模块解耦结构。
- 两者激活分布差异显著（即使施加旋转变换后依然存在），块级统一策略无法适配这种分布差异。

**具体设计**：
- 将 Self-Attention 和 FFN 视为**独立量化单元**，分别构造模块专属优化目标。
- **Self-Attention 损失**：
  - L1：量化前后自注意力模块输出的 L2 重建损失；
  - L2：注意力保持损失——用 KL 散度对齐全精度与量化模型的注意力矩阵，保留 token 间依赖结构；
  - 总损失：L_self-attn = L1 + λL2（λ 为平衡系数）。
- **FFN 损失**：对 gate/up/down 三个投影层集体量化，构造量化前后 FFN 输出的 L2 重建损失，保留逐 token 变换能力。

### 2. 自适应跨块量化（Adaptive Cross-Block Quantization, ACBQ/ACBR）——块间误差补偿

**动机**：
- 通过 Hessian 矩阵可视化发现，不同 Transformer 块之间存在明显的二阶依赖关系（非对角元素幅值大），表明量化误差会跨块传播甚至被下游块放大。
- 联合量化所有块虽可抑制误差累积，但计算开销过高，需高效识别强依赖块。

**具体设计**：
- 以**激活熵**作为高效指标估计块间依赖 D(k, k+1)（基于先验观察：块间搜索与逐块搜索的离散化误差差值与激活熵强相关）。
- 定义非连续块间的依赖为中间所有相邻块依赖的累加。
- 通过阈值 h₀ 进行自适应分组：平均依赖超过阈值的连续块被划分到同一组，进行联合重建，同时约束最大块大小以避免计算开销过大。
- 跨块损失函数：最小化一组块（i 到 i+n）量化前后复合函数输出的 L2 重建误差，促使早期量化表示与下游全精度计算保持对齐。

### 3. 辅助技术
- 使用旋转变换（如 QuaRot）重分布激活离群值，降低量化误差。

## 三、实验设计

### 1. 评测模型
- LLaMA（7B、13B、30B）、LLaMA-2（7B、13B、70B）、LLaMA-3（8B、70B）、OPT（30B、66B），覆盖多尺度。

### 2. 数据集
- **语言建模**：WikiText-2 和 C4 上的困惑度（perplexity），上下文长度 2048 tokens。
- **零样本推理**：9 个下游任务——BoolQ、LAMBADA、OpenBookQA、SIQA、PIQA、ARC（Challenge 与 Easy）、HellaSwag、WinoGrande。

### 3. 对比方法
SmoothQuant、GPTQ、OmniQuant、AWQ、QuaRot、SpinQuant、CBQ 等 SOTA PTQ 方法。

### 4. 比特设置
覆盖广泛场景：W4A16KV16、W4A4KV16、W4A4KV4、W3A16KV16、W2A16KV16、W4A8KV16。

### 5. 校准设置
- 初始化：Pile 数据集 8 个样本（1024 tokens）网格搜索 α 和 z；
- 优化：512 个 Pile 样本，学习率 5e-5（大模型降至 2e-5），batch size 4，W4A4 训练 20 epochs、W2A16 训练 5 epochs，λ=10。

## 四、资源与算力

- 论文**正文未明确报告 GPU 型号与数量**，也未给出完整训练时长。
- 附录提供了效率对比（LLaMA-7B）：
  - 校准时间（GPU 小时）：CBQ 为 32.99h（权重only）/ 33.52h（权重-激活），ACBQ 仅需 **1.29h / 1.87h**；
  - 峰值 GPU 内存：CBQ 为 41GB，ACBQ 为 **39GB**。
- 即 ACBQ 在校准时间和内存占用上均优于 CBQ，但具体硬件规格（GPU 型号、并行配置等）未说明。

## 五、实验数量与充分性

### 实验规模
- **主实验**：3 张主表（Table 1-3），覆盖多模型、多比特配置的困惑度与零样本准确率。
- **补充零样本结果**：附录 Table 6-12 共 7 张表，按模型逐一细列 9 个任务的完整准确率。
- **消融实验**：Table 4（W2A16）与 Table 13（W4A4），验证 MWO、ACBR、BWQ 各组件贡献。
- **超参数敏感性**：Table 14，对 λ 进行 5 组取值分析。
- **效率评估**：Table 5，与 CBQ 的校准时间/内存对比。

### 充分性评估
- **充分**：模型覆盖 3 个 LLaMA 系列 + OPT，横跨 7B~70B 多尺度；比特配置从较温和的 W4A16 到极端的 W2/W4A4；对比方法涵盖主流的基于旋转、基于重建、基于跨块的多类 PTQ 方法。消融较完整，能有效归因各组件贡献。
- **公平性**：与主流方法在同一基准（WikiText-2、C4、9 个零样本任务）上比较，设置遵循先前工作的标准协议；W2/W4A4 等极端设置下多数基线严重退化，对比清晰。但**未报告多次运行的方差**，也未与 QAT 方法系统对比。

## 六、主要结论与发现

1. ACBQ 在 W4A4（联合量化）和 W2（极端低比特权重量化）两种场景下，均持续取得最优或次优表现，且仅引入可忽略的额外计算开销。
2. 在较大模型（如 LLaMA-3 70B）上优势尤为明显（W4A4KV4 平均准确率 71.25%，显著高于 SpinQuant 的 66.31%），说明 ACBQ 在更具挑战性的低比特条件下鲁棒性更强。
3. 模块级（MWO）优化相比块级（BWQ）重建能更有效降低量化误差，验证了将 Self-Attention 与 FFN 分开优化的合理性。
4. 自适应跨块重建（ACBR）进一步抑制了跨层误差传播，与 MWO 结合后达到最佳性能。
5. 效率上 ACBQ 校准时间远低于 CBQ（约 25 倍加速），峰值内存也更低，实用性强。

## 七、优点

1. **问题切入精准**：同时面向联合量化与极端低比特量化两个痛点，提出统一框架，而非仅优化单一场景。
2. **模块粒度创新**：首次用实证（激活分布图、Hessian 可视化）支撑将 Self-Attention 与 FFN 分离量化的必要性，逻辑链条完整且可解释性强。
3. **自适应分组机制**：用激活熵作为依赖代理指标、按阈值自适应分组，在抑制跨层误差累积与计算开销之间取得良好平衡，避免了 CBQ 重叠窗口的高成本。
4. **实验全面扎实**：覆盖 3 大家庭 12 个模型、6 种比特配置、2 个困惑度数据集 + 9 个零样本任务，消融、超参数敏感性、效率对比俱备，说服力较强。
5. **开销低、实用性好**：校准时间仅约 1~2 GPU 小时，推理阶段无额外负担，适合实际部署。

## 八、不足与局限

1. **仍不及 QAT 性能**：在极端低比特设定下，ACBQ 的精度仍落后于量化感知训练（QAT）方法。
2. **优化时间较长**：虽然远快于 CBQ，但整个优化过程仍需数小时，存在进一步加速空间。
3. **依赖启发式分组指标**：激活熵作为跨层依赖的代理指标，可能无法完全捕捉复杂的跨层交互，特定场景下分组未必最优（作者计划用梯度/Hessian 灵敏度指标改进）。
4. **实验覆盖局限**：
   - 未覆盖更大规模（100B+）模型或 MoE 架构；
   - 未报告多次运行的方差/置信区间，难以评估统计显著性；
   - 硬件资源（GPU 型号、数量）未披露，影响效率结论的可复现性。
5. **公平性细节**：与部分基线（如 AWQ、GPTQ）对比时，未详细说明是否采用相同的分组大小（group size）与校准数据设置（仅 Table 3 注明 group size=128），可能带来轻微对比偏差。

（完）
