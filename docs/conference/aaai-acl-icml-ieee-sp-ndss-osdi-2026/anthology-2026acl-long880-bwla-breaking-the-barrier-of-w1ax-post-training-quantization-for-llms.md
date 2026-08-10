---
title: "BWLA: Breaking the Barrier of W1AX Post-Training Quantization for LLMs"
title_zh: BWLA：突破大语言模型W1AX后训练量化的壁垒
authors: "Zhixiong Zhao, Zukang Xu, Dawei Yang"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.880.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 正交-克罗内克变换实现大模型1比特权重与低位激活联合后训练量化
tldr: LLM二值化虽然能把权重压到1比特，但激活重尾导致仍需高精度激活，无法端到端加速。提出BWLA，这是首个实现高精度1比特权重与低位激活（如6比特）联合PTQ的框架，用正交-克罗内克变换学习正交映射，使单峰权重变成对称双峰以便二值化。该方法在保持精度的同时显著降低计算与带宽成本，突破W1AX量化壁垒，推动LLM低位部署。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 730, \"height\": 612, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1516, \"height\": 636, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1585, \"height\": 745, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1256, \"height\": 577, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1502, \"height\": 606, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1503, \"height\": 550, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1660, \"height\": 1065, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1504, \"height\": 1072, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1614, \"height\": 2304, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long880/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1616, \"height\": 2159, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1651, \"height\": 740, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 801, \"height\": 449, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 797, \"height\": 457, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1402, \"height\": 640, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 816, \"height\": 729, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1640, \"height\": 2513, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1640, \"height\": 2513, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026acl-long880/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 993, \"height\": 265, \"label\": \"Table\"}]"
motivation: 现有二值化方法无法处理激活重尾分布，需保持高精度激活，阻碍端到端加速。
method: 提出BWLA，采用正交-克罗内克变换并通过EM学习正交映射，将单峰权重转为对称双峰以支持1比特权重和低位激活PTQ。
result: 首次在PTQ中同时实现高精度1比特权重和低位激活（如6比特）量化。
conclusion: 突破W1AX后训练量化壁垒，实现LLM端到端低位加速。
---

## Abstract
Large language models (LLMs) have driven major progress in NLP, yet their substantial memory and compute demands still hinder practical deployment. Binarization can compress weights to 1 bit, fundamentally lowering compute and bandwidth cost. However, existing methods cannot address activation heavy tails and thus must keep activations in high precision, preventing true end-to-end acceleration. To overcome this limitation, we propose BWLA, the first post-training quantization framework that preserves high accuracy while achieving 1-bit weight quantization together with low-bit activations (e.g., 6 bits). The Orthogonal-Kronecker Transformation (OKT) learns an orthogonal mapping via EM minimization, converting unimodal weights into symmetric bimodal forms while suppressing activation tails and incoherence. The Proximal SVD Projection (PSP) then performs lightweight low-rank refinement through proximal SVD projection, further enhancing quantizability with minimal overhead. On Qwen3-32B, BWLA reaches a Wikitext2 perplexity of 11.92 under 6-bit activations (vs. 38 from SOTA), improves five zero-shot tasks by more than 70%, and delivers 3.26× inference speedup, demonstrating strong potential for real-world LLM compression and acceleration. The code will be available at [BWLA](https://github.com/Kishon-zzx/BWLA).

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与研究动机

大语言模型（LLMs）在 NLP 领域取得重大进展，但参数量巨大导致部署成本极高。例如 LLaMA2-70B 在 FP16 精度下推理需要超过 120GB 内存，严重制约了在移动端和资源受限平台的部署。

**量化与二值化的演进困境：**
- 二值化可将权重压缩至 1 比特，从根本上降低计算和带宽成本；
- 但现有二值化方法（如 BiLLM、ARB-LLM）只关注权重，未处理**激活值重尾分布**和**极端离群值**问题，导致激活必须保持高精度，无法实现真正的端到端加速；
- 既有的 W1AX（1 比特权重 + 低位激活）方案（如 BitNet a4.8）依赖高成本的量化感知训练（QAT），不适用于实际部署场景；
- 现有 PTQ 方法面临两个根本性障碍：① 权重分布通常是单峰（高斯状），与二值码本 {−1, +1} 严重不匹配；② 激活分布存在显著重尾和极端离群值，在低比特下量化误差极大。

**核心研究问题：** 能否在纯后训练量化（PTQ）框架下，**同时**实现——① 将逐通道权重分布重塑为适合二值化的对称双峰形态；② 抑制激活离群值以缓解重尾问题——从而达成 1 比特权重与低位激活的统一量化？

---

### 2. 方法论：BWLA 框架

BWLA（Binarized Weights and Low-bit Activations）由两个核心模块组成，采用迭代优化的方式协同工作。

#### 2.1 核心思想

利用正交变换的性质（R⁻¹ = Rᵀ 保证前向传播不变性），将同一个正交变换同时作用于权重和激活：对权重，将单峰分布重塑为对称双峰分布以匹配二值码本；对激活，通过旋转分散相干性、抑制重尾离群值，使其更易于低比特量化。

#### 2.2 Orthogonal-Kronecker Transformation（OKT）

**目的：** 高效实现正交变换，同时实现权重的双峰化重塑和激活的平滑化。

**技术要点：**
- 将完整正交矩阵 R ∈ R^{m×m} 分解为 Kronecker 积形式 R = R₁ ⊗ R₂（其中 R₁ ∈ R^{n₁×n₁}，R₂ ∈ R^{n₂×n₂}，n₁n₂ = m）；
- 当 n₁ ≈ n₂ ≈ √m 时，计算复杂度从 O(m²) 降至 **O(m^{3/2})**，内存开销减少数个数量级（例如存储两个 64×64 矩阵而非一个 4096×4096 矩阵）；
- 优化目标为对称双分量高斯混合模型（GMM）的负对数似然，并加入正则项防止模式坍缩（mode collapse）；
- 通过 **EM 算法** 更新混合模型参数 Θ（闭式解），通过 **MM 算法（Majorization-Minimization）** 更新正交因子 R₁、R₂，每次迭代归结为加权的正交 Procrustes 问题，用 SVD 求解；
- 定理 1 从理论上证明：对于行服从高斯分布（单峰）的权重矩阵，存在正交变换使其收敛为对称双峰高斯混合分布。

#### 2.3 Proximal SVD Projection（PSP）

**目的：** 在 OKT 基础上，通过低秩修正消除仅靠正交旋转无法消除的结构性残差。

**技术要点：**
- 引入秩为 k 的残差矩阵 M = AB（k ≪ min{oc, ic}），修正后权重为 W_res = W − M；
- 通过**近端上界（proximal majorizer）**构建优化目标，保证目标函数单调下降；
- 求解归结为**截断 SVD 投影**：Y_t ≈ U_kΣ_kV_kᵀ，即对近端梯度点做秩 k 最佳逼近（Eckart-Young-Mirsky 定理）；
- 低秩残差以极小的参数开销吸收残余离群值，进一步增强双峰结构。

#### 2.4 算法流程概述

1. 初始化 R₁, R₂ 为单位矩阵或随机正交矩阵，M = 0；
2. 迭代 T_outer 轮（实验中 60 轮：OKT 40 轮 + PSP 20 轮）：
   - EM 更新 GMM 参数（责任值 r⁺、中心 cᵢ、方差 σ²ᵢ）；
   - 计算梯度并执行 PSP 的低秩截断 SVD 更新；
   - 交替执行两个正交 Procrustes 更新（SVD）以优化 R₁ 和 R₂；
3. 最终在变换后的坐标系中进行逐通道二值化（Sign 函数）和缩放/偏移参数计算。

---

### 3. 实验设计

#### 3.1 数据集与 Benchmark

- **困惑度评估：** WikiText2、C4；
- **零样本任务（7 项）：** ARC-Challenge、ARC-Easy、HellaSwag、LAMBADA-openai、LAMBADA-standard、PIQA、WinoGrande；
- **复杂推理任务（3 项）：** MMLU（多领域知识）、GSM8K（数学推理）、HumanEval（代码生成）；
- **校准数据：** 从 WikiText2 采样 128 个文本片段。

#### 3.2 评估模型覆盖

- **基础模型：** LLaMA2-7B/13B/70B、LLaMA3-8B、Qwen3-8B/14B/32B（共 7 个不同规模的模型，涵盖 7B 到 70B）；
- **指令微调模型：** Qwen3-32B-Instruct。

#### 3.3 对比方法

- **二值化方法：** BiLLM、ARB-LLM、DBellQuant（当前 SOTA PTQ 二值化方法）；
- **通用低比特 PTQ 方法：** RTN、GPTQ、OSTQuant（使用 2 比特权重评估）；
- **量化设置：** 权重采用对称逐通道量化，激活采用逐 Token 非对称量化。

---

### 4. 资源与算力

- **硬件：** 论文明确说明使用 NVIDIA A6000 GPU，但**未明确说明使用的 GPU 数量和具体部署细节**；
- **量化时间对比：** BWLA 在 LLaMA2-7B 上约 0.10 小时，LLaMA3-8B 约 0.12 小时，LLaMA2-13B 约 0.25 小时，LLaMA2-70B 约 1.4 小时；相比 OSTQuant 加速 3.0×-3.9×，相比 OmniQuant 加速 3.0×-6.8×；
- **推理效率评估：** 在批大小 4、1024 token prefill、256 token decoding 条件下测速，LLaMA2-13B 上 BWLA 达到 77.31 tokens/s，相比 FP16 实现 3.26× 加速，参数内存从 23.7GB 降至 3.94GB；
- **算力披露充分性：** 中规中矩——模型覆盖广但 GPU 具体数量未披露，整体在可接受的披露水平。

---

### 5. 实验数量与充分性

#### 5.1 实验规模总结

| 实验类型 | 数量/覆盖 | 评估内容 |
|---------|---------|---------|
| 主实验（W1A16 / W1A6） | 7 个基础模型 × 2 种激活位宽 | 表 1，含 Perplexity + 零样本准确率 |
| 指令微调实验 | Qwen3-32B-Instruct | 表 2，含 MMLU、HumanEval、GSM8K |
| 4-bit 激活实验 | 3 个模型 | 表 3，7 项零样本任务 |
| 模块消融（OKT/PSP） | 3 个模型 × 2 种位宽 | 表 4 |
| 超参消融（OKT 维度/PSP 秩比） | 3 个模型 | 图 4a/4b |
| 校准数据鲁棒性 | Qwen3-14B | 图 6a/6b |
| 效率分析 | 3 个模型 | 推理吞吐、内存、量化时间 |
| 损失收敛曲线 | LLaMA3-8B 全部层 | 图 7、图 8 |
| 分布可视化 | Qwen3-8B 第 12 层 | 图 9、图 10 |

#### 5.2 充分性与公平性评估

**充分之处：**
- 模型覆盖面广（7 个基础模型 + 1 个指令模型），覆盖 7B~70B 规模跨度；
- 与当前最先进的二值化 PTQ 方法（BiLLM、ARB-LLM、DBellQuant）均有对比；
- 在 16-bit、6-bit、4-bit 三种激活精度下均进行了评估，验证方法的鲁棒性；
- 提供了完整的模块消融实验和超参敏感性分析。

**不足之处：**
- **对比公平性瑕疵：** 表 1 中 RTN/GPTQ/OSTQuant 是 2-bit 权重而非二值化（1.07-1.16 bit），比较维度不一致，但这是此类论文的常见做法，且对 BWLA 更不利（其权重精度更高），反而说明 BWLA 优势更显著；
- 主实验对 Qwen3 系列在 A6 下 BiLLM 和 ARB-LLM 的结果出现困惑度高达 215-485 或 1e4 的情况，虽能说明方法优势，但需注意极端场景下数值稳定性的公允解读；
- 未在更多架构（如 Mistral、Gemma）上验证泛化性；
- 未与 QAT 方法（如 BitNet）直接对比精度-成本权衡，仅指出其训练成本高。

---

### 6. 主要结论与发现

1. **首次实现高精度 W1AX 纯 PTQ：** BWLA 是第一个无需重训练即可同时实现 1 比特权重 + 低位激活（6 比特）并保持高精度的 PTQ 框架；
2. **显著优于现有方法：** 在权重二值化（A16）下，相比 SOTA 二值化方法平均准确率提升 13%，困惑度降低 28%；在 A6 下，Qwen3-32B 的 WikiText2 困惑度从 SOTA 的 38 降至 11.92，零样本任务提升超过 70%；
3. **指令微调模型上的优势更突出：** 在 Qwen3-32B-Instruct 的 A6 实验中，现有方法几乎完全崩溃（MMLU 接近随机猜测 25%，HumanEval 和 GSM8K 归零），而 BWLA 相对未量化设置保持约 94% 的性能；
4. **效率显著：** 相比 FP16 实现 3.26× 推理加速，参数内存节省超 80%；
5. **核心洞察验证：** 单峰权重的双峰化重塑 + 激活离群值抑制是实现 W1AX 的关键，正交变换天然适合同时完成这两项任务。

---

### 7. 优点与亮点

1. **问题定位精准：** 明确指出现有二值化方法的根本瓶颈——只优化权重分布而不处理激活重尾，导致无法真正端到端加速；
2. **理论支撑扎实：** 提供定理 1 证明正交变换实现双峰化的理论可行性，并推导了 EM-MM 交替优化的收敛性保证；
3. **设计优雅高效：** 利用正交矩阵的逆等于转置这一性质，实现权重重塑与激活平滑的统一；Kronecker 分解大幅降低计算与存储开销，兼顾效果与效率；
4. **训练-free 的实用性：** 避免 QAT 的高昂训练成本，仅需几十轮轻量迭代（约 60 轮）即可收敛，且在 70B 模型上仅需 1.4 小时；
5. **实验全面深入：** 涵盖 7 个基础模型 + 1 个指令模型、4 种激活精度、10+ 个任务基准，提供了模块消融、超参敏感性、校准数据鲁棒性、损失收敛曲线和分布可视化等多维度分析；
6. **分布可视化验证充分：** 直观展示了变换前后权重从单峰到双峰、激活从重尾到集中的转变过程，增强了方法可信度。

---

### 8. 不足与局限

1. **极端低位激活稳定性不足：** 论文自述在 W1A4 下模型稳定性明显下降，当前激活平滑与离群值抑制技术在更激进的量化条件下仍不够充分；
2. **正交线性变换的表达能力限制：** OKT 依赖线性正交变换，可能无法完全捕捉现代 LLM 权重空间的非线性几何结构，论文指出轻量非线性变换可能带来进一步提升；
3. **格式支持有限：** 目前仅支持标准整数格式（INT4/INT6），未扩展到混合精度或新兴低精度浮点格式（如 MXFP4）；
4. **实验覆盖范围可扩展：** 未在更多架构族（Mistral、Gemma 等）和更长上下文/更大批量场景下验证；指令微调模型仅在 Qwen3-32B 上评估，且未在 A4 下评估；
5. **对比公平性与基线覆盖的潜在偏差：** 对某些对比方法（如 DBellQuant）缺少部分模型的完整结果；在 Qwen3 系上某些方法（BiLLM/ARB-LLM）出现了困惑度 1e4 级别的极端结果，虽能体现方法的相对优势，但应关注数值是否因实现细节或超参不当而失真；
6. **算力披露不完整：** 未说明具体使用的 GPU 数量和并行策略，影响复现的精确性；
7. **算法超参敏感性仍需进一步验证：** 论文通过校准数据鲁棒性实验（图 6）证明了对校准集不敏感，但 OKT 与 PSP 的迭代步数、Kronecker 维度选择等超参是否在不同模型规模上存在系统性最优模式，仍可进一步考察；
8. **应用范围受限：** 实验聚焦于英文 NLP benchmark，未覆盖多语言任务或实际部署中的动态稀疏性、变长请求等复杂场景。

---

（完）
