---
title: "D-QRELO: Training- and Data-Free Delta Compression for Large Language Models via Quantization and Residual Low-Rank Approximation"
title_zh: D-QRELO：基于量化与残差低秩近似的免训练免数据大语言模型增量压缩
authors: "Junlin Li, Shuangyong Song (宋双永), Guodong DU, Ngai Wong, Xuebo Liu, Yongxiang Li (李永翔), Min Zhang, Jing Li, Xuelong Li"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.1081.pdf"
tags: ["query:ulbv"]
score: 4.0
evidence: 面向微调大模型增量压缩的一比特量化与残差低秩近似
tldr: 大规模指令微调会带来大量微调模型实例，增量压缩虽能复用基座模型，但现有方法在大规模数据微调模型上因增量幅度、奇异值和熵增大而失效。提出D-QRELO，将粗粒度1比特量化与残差低秩近似结合，免训练、免数据地对增量权重压缩。该方法在降低存储的同时保持增量模型质量，为多微调模型的低成本维护提供了新工具。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 800, \"height\": 404, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1448, \"height\": 792, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 795, \"height\": 478, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 797, \"height\": 444, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 796, \"height\": 460, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 801, \"height\": 438, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 804, \"height\": 442, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 800, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1081/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 791, \"height\": 528, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 706, \"height\": 252, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1669, \"height\": 661, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1666, \"height\": 413, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 779, \"height\": 287, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 711, \"height\": 360, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 816, \"height\": 443, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl1081/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1478, \"height\": 659, \"label\": \"Table\"}]"
motivation: 大规模SFT导致微调模型激增，已有增量压缩方法在大规模数据微调模型上误差大。
method: 提出D-QRELO，结合粗粒度1比特量化与残差低秩近似，实现免训练免数据的增量压缩。
result: 对大规模数据微调的LLM增量权重压缩更有效，降低存储开销。
conclusion: 为LLM增量压缩提供量化与低秩残差结合的免训练方案。
---

## Abstract
Supervised Fine-Tuning (SFT) accelerates task-specific large language models (LLMs) development, but the resulting proliferation of fine-tuned models incurs substantial memory overhead. Delta compression addresses this by retaining a single pre-trained LLM with multiple compressed delta weights. However, existing methods fail on models fine-tuned with large-scale datasets. We find that larger SFT data scale amplifies delta parameter magnitude, singular values, and entropy, exacerbating compression errors. To tackle this, we propose D-QRELO ( D elta Compression via Q uantization and R sidual Lo w-Rank), a novel training- and data-free delta compression method. It combines coarse-grained one-bit quantization to capture the dominant structure of the delta, followed by compensated residual low-rank approximation to recover fine-grained details from the smaller residual error. Experiments on various LLMs spanning dense and MoE architectures across multiple domains under this challenging setting demonstrate that D-QRELO outperforms existing methods. Moreover, we establish key design principles for delta compression through extensive empirical analysis, demonstrating how task difficulty, architecture, and layer positioning create predictable patterns that can guide optimal compression strategies in production systems.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：监督微调（SFT）是将通用大语言模型（LLM）定制为领域/任务专用模型的主流方式，但微调模型的快速增殖带来了严重的存储和部署内存开销。现实场景往往需要同时部署或存档大量微调模型，例如多阶段推理、多智能体协作、模型版本回滚等。
- **问题**：delta 压缩技术通过存储一个基座模型和多个压缩后的 delta 权重（微调模型与基座模型的参数差异）来降低存储开销，是应对上述挑战的关键路径。然而，现有方法（如 BitDelta、Delta-CoMe）在基于大规模数据微调的"强"模型上出现显著的性能下降。
- **核心洞察**：作者发现，SFT 数据规模越大，delta 参数的**幅度（magnitude）、奇异值（singular values）和熵（entropy）**均显著增大（如表 1 所示，数据规模从 100K 增至 14M 时，平均 |Δ| 从 0.000492 升至 0.002147），这意味着 delta 参数信息更复杂、数值范围更宽，直接加剧了量化/低秩分解方法的绝对压缩误差，导致已有方法的性能急剧衰减（如图 1 所示）。
- **整体含义**：论文旨在解决大规模数据微调模型场景下 delta 压缩性能退化这一被忽视但关键的问题，提出一种免训练（training-free）、免数据（data-free）的新型 delta 压缩方法 D-QRELO，并系统性地探索 delta 压缩的设计原则。

## 2. 论文提出的方法论

**核心思想**：D-QRELO 采用两阶段互补的压缩策略——先用粗粒度的一比特量化捕获 delta 参数的主要符号和量级结构，再对残差进行低秩近似以恢复细粒度细节。这种"先粗后细"的设计有效规避了大数值范围对纯量化或纯 SVD 方法的误差放大问题。

**关键技术细节与流程**：

1. **Delta 定义**：将每个微调模型的权重表示为基座权重与 delta 之和：
   - Δ(k) = φ(k) − φ_Pre
   - 压缩目标为对每个 Δ(k) 以压缩率 ρ 进行编码。

2. **第一阶段：一比特量化（粗粒度）**
   - 对 delta 矩阵 Δ ∈ R^(n×m)，计算符号矩阵 Sign(Δ)，并确定全局缩放因子 α 为 Δ 所有元素绝对值的均值：
     - α = (1/nm) Σ|Δij|
   - 该 α 是 Frobenius 范数意义下的最优缩放（α = argmin ‖Δ − α·Sign(Δ)‖²_F），即量化误差最小化解。
   - 得到量化结果 Δ̂_quant = α ⊙ Sign(Δ)，存储成本为 1 bit/参数 + 一个标量。

3. **残差计算**：定义残差矩阵 R = Δ − Δ̂_quant，其数值范围显著小于原始 Δ，更适合低秩压缩。

4. **第二阶段：残差低秩近似（细粒度）**
   - 对 R 执行奇异值分解：R = UΣV⊤。
   - 设定目标压缩率后，选择截断秩 r = ⌈n·m·ρ1 / (n + m)⌉，使低秩分量参数数 r(n + m) + r 满足压缩目标 ρ1。
   - 得到低秩近似 R̂_lowrank = U_r Σ_r (V_r)⊤。

5. **最终重构**：Δ̂ = Δ̂_quant + R̂_lowrank，总压缩率 ρ = ρ1 + 1/b（b 为原始位宽，如 16-bit）。对于向量参数（如 LayerNorm 的 bias 等），按幅度阈值进行剪枝式压缩。

   **完整流程**：Δ → 一比特量化（1-bit，符号+全局缩放）→ 残差 R → SVD 截断（rank-r）→ 合并两个分量得到压缩后的 delta。

## 3. 实验设计

**评测任务与数据集**（覆盖数学、代码、知识、对齐、多模态五大类）：
- **数学**：GSM8K（基础算术）、MATH（竞赛数学）、AIME2024（高级竞赛题）
- **代码**：LiveCodeBench v6、HumanEval
- **知识**：GPQA-Diamond、MMLU-Pro
- **对齐**：IFEval（指令跟随）
- **多模态**：MME、TextVQA（视觉-文本联合理解）

**评估模型**（8 个，覆盖 dense 与 MoE 架构、标准/多模态 SFT 与推理蒸馏三种范式）：
- OpenMath2-LLaMA3.1-8B（数学 SFT）
- Qwen2-VL-7B-Instruct（多模态 SFT）
- DeepSeek-R1-Distill-LLaMA3-8B / Qwen-14B / Qwen-32B（推理蒸馏）
- DeepSeek-R1-0528-Qwen3-8B
- Qwen3-4B-Instruct-2507
- Qwen3-30B-A3B-Instruct-2507（MoE）

**对比方法**：
- 剪枝类：Random pruning、Magnitude pruning、Wanda pruning
- 低秩类：纯 SVD
- 量化类：BitDelta（1-bit 量化）
- 混合精度 SVD：Delta-CoMe（当前 SOTA）
- 所有方法在 1/8 压缩率下评估（BitDelta 因 1-bit 特性在 1/16 压缩率下评估）

**评测指标**：各数据集使用准确率（Accuracy），MME 使用 Score；同时定义 Performance Retention（PR）和 Drop 两个指标衡量压缩后的性能保留率。

## 4. 资源与算力

- 文中明确说明的实验环境：Ubuntu 20.04 系统、NVIDIA A800-SXM4 80GB GPU、Intel Xeon Platinum 8358P CPU @ 2.60 GHz、CUDA 12.4、PyTorch 2.5.1。
- **未明确说明**的部分：论文没有披露具体的 GPU 数量、总训练时长（如 SFT 模型微调耗时、推理评测耗时等）、能耗等详细算力成本，只给出了硬件环境，未做具体算力统计与对比分析。
- LoRA 对比实验的配置有说明：秩 512、α 512、warmup 0.04、峰值学习率 1e-4，参数量与 D-QRELO 匹配（约 10 亿参数）。

## 5. 实验数量与充分性

- **主实验**（表 2）：5 个代表性模型 × 12 项任务/数据集的全方法对比，覆盖多种架构和任务类型；
- **补充实验**（表 6）：3 个额外模型（DS-R1-LLaMA-8B、DS-R1-Qwen-14B/32B）上的完整对比；
- **消融实验**（表 3）：验证两阶段策略的互补性与顺序合理性（SVD+1-bit vs. 1-bit+SVD 的四种组合）；
- **压缩比敏感性分析**（图 3）：6 档压缩率下的性能保留变化；
- **任务难度分析**（图 4、5）：数学基准从 GSM8K 到 MATH 到 AIME 的梯度比较，以及 MATH 内 5 级难度的细粒度分析；
- **基座模型影响分析**（图 6）：对比 LLaMA vs. Qwen 家族、不同参数量（7B/14B/32B）的压缩表现；
- **LoRA 对比**（表 5）：与 LoRA 微调模型在同等参数量下的性能对比；
- **层敏感性分析**（图 7）：对模型前 0%-25%、25%-50%、50%-75%、75%-100% 层分别压缩的消融；
- **模块敏感性分析**（图 8、9）：对 Mapping、LayerNorm、Attention、MLP 四类模块分别压缩，并结合参数占比做归一化分析。

**充分性与公正性评价**：实验覆盖范围广，主实验加消融的系统性较强；评估了不同架构（dense/MoE）、不同微调范式（SFT/多模态/推理蒸馏）和不同领域，这在 delta 压缩文献中属于相当全面的设置。但在任务难度分析部分，图 4 中的模型不完备（OpenMath2 缺少 AIME24、DS-R1-LLaMA-8B 缺少 GSM8K），个别模型间的比较不完全对等；此外所有实验在单一硬件环境上完成，缺乏多环境复现。

## 6. 论文的主要结论与发现

- **D-QRELO 持续优于现有方法**：在相同压缩率下，D-QRELO 在几乎所有模型和任务上均取得最佳性能保留，平均 Drop 仅 6.17%，显著低于 Delta-CoMe（14.00%）、BitDelta（56.13%）和 SVD（35.62%），平均提升约 7.83 个百分点。
- **SFT 数据规模是决定压缩难度的关键因素**：更大规模数据微调产生幅度更大、奇异值更大、熵更高的 delta 参数，导致现有方法（依赖量化或低秩分解）误差放大；D-QRELO 通过两阶段残差补偿有效缓解了这一问题。
- **"一比特量化 + 残差 SVD"的组合优于相反顺序**（表 3）：先量化再低秩比先低秩再量化更有效，因为量化后的残差数值范围小、更适合 SVD 捕捉结构信息；而 SVD 残差含高频信息，量化难以保真。
- **压缩性能与任务难度呈负相关**：越难的数学任务（GSM8K < MATH < AIME24，MATH 内 1-5 级难度递增）压缩后性能下降越大，说明复杂推理所需的信息更容易被压缩误差破坏。
- **基座模型特性影响压缩效率**：Qwen 系列的性能保留显著优于 LLaMA；同家族内更大模型普遍压缩表现更好（32B > 14B > 7B）。
- **不同层和模块的压缩敏感性显著不同**：早期层（0%-25%）对压缩最敏感，深层则更鲁棒；MLP 占参数绝大多数、是绝对性能下降的主要来源，但 LayerNorm 单位参数的敏感度最高（"隐藏的脆弱点"），Mapping 和 Attention 次之。
- **D-QRELO 优于 LoRA**：在相同参数量（约 10 亿）下，全量微调后经 D-QRELO 压缩（71.12 平均分）显著优于直接 LoRA 微调（62.09 平均分），表明全量微调 + 高质量 delta 压缩优于直接参数高效微调。
- **实际收益**：D-QRELO 可在单张 80G GPU 上同时部署 16 个 LLaMA-3.1-8B 模型，而基线方案 8 个即 OOM；多模型推理延迟也显著降低（如 8 模型场景从 63ms 降至 45ms）。

## 7. 优点

- **方法设计精巧**：两阶段"先量化后低秩"的策略在理论上能针对性缓解大数值范围 delta 的误差累积问题，且在消融中验证了组合顺序的重要性，方法论具有一定的理论依据和实证支撑。
- **免训练、免数据**：D-QRELO 仅需对 delta 权重做代数运算（符号量化 + SVD），不依赖任何训练数据或微调过程，适用性强、效率高、易于部署。
- **系统性实证分析**：除主实验外，还从任务难度、基座模型身份与规模、层位置、模块类型四个维度系统探索了 delta 压缩的设计原则，是该领域首个较为全面的实证研究，为生产系统提供了可操作的压缩策略指导。
- **覆盖范围广**：涵盖 dense 与 MoE 架构、标准 SFT、多模态 SFT、推理蒸馏等多种范式，以及数学、代码、知识、对齐、多模态等任务域，验证了方法的泛化能力。
- **实际收益论证充分**：明确给出 GPU 内存节省和推理延迟加速的具体数据，展示了方法的工程价值。

## 8. 不足与局限

- **压缩率存在物理下限**：由于采用 1-bit 量化 + 低秩两阶段结构，在 16-bit 基座模型下总压缩率不能低于约 1/16（约 6.25%），即使残差分量可忽略时也无法突破，限制了其在高压缩需求场景下的适用性。
- **小数据量场景优势有限**：当 SFT 数据规模较小时，delta 参数本身稀疏且幅度小，简单方法（如直接 SVD 或量化）已能取得良好性能，D-QRELO 的复杂度未必带来显著增益，其优势主要体现在大规模数据微调场景。
- **算力消耗未详细披露**：论文未明确说明训练/评测所消耗的总 GPU 时长、成本等，难以全面评估方法的资源效率。
- **部分分析存在不完整性**：任务难度分析中个别模型的基准缺失（如 OpenMath2 无 AIME24 数据），模型间比较的严格对等性不足。
- **应用范围局限**：方法的性能仅在公开数据集和域内数据上验证，对隐私敏感场景（如医疗、金融）或数据分布差异大的实际场景的泛化性未做探索，存在不确定风险。
- **实验环境单一**：所有实验在相同硬件（A800）与软件栈上完成，缺少多环境、多框架的复现验证。

（完）
