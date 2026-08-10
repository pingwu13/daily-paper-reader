---
title: Exploring Layer-wise Information Effectiveness for Post-Training Quantization in Small Language Models
title_zh: 探索小语言模型的分层信息有效性用于后训练量化
authors: "He Xiao, Qingyao Yang, Dirui Xie, Wendong XU, Zunhai Su, Runming Yang, Haobo Liu, Wenyong Zhou, Zhengwu Liu, Ngai Wong"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.771.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向LLM的极低比特PTQ，采用分层精度混合
tldr: 数十亿参数的LLM存在过参数化，很多层贡献信息少却占用大量内存和能耗。LieQ提出分层信息有效性驱动的后训练量化框架，在层内保持均匀位宽、层间混合精度，从而保留标准乘法内核，避免码本和不规则访存。其在小于8B模型上验证了极低比特压缩下保持精度，兼顾硬件效率，为小规模LLM部署提供了低比特方案。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl771/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 796, \"height\": 575, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl771/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1650, \"height\": 982, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl771/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 772, \"height\": 620, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl771/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 642, \"height\": 781, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl771/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 792, \"height\": 392, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl771/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1654, \"height\": 789, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl771/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1485, \"height\": 688, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl771/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1644, \"height\": 654, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl771/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1579, \"height\": 1416, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl771/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 809, \"height\": 245, \"label\": \"Table\"}]"
motivation: LLM层间信息贡献不均，统一低比特量化浪费资源或损害精度。
method: 基于信息有效性度量在层间分配比特精度，层内保持均匀位宽以适配标准内核。
result: 在子8B小模型上实现极低比特压缩并维持精度。
conclusion: LieQ提供了硬件友好且无需码本的极低比特PTQ方案。
---

## Abstract
Large language models with billions of parameters are often over-provisioned: many layers contribute little unique information yet dominate the memory and energy footprint during inference. We present LieQ (Layer-wise information effectiveness Quantization), a hardware-native, metric-driven post-training quantization framework that addresses the critical challenge of maintaining accuracy in sub-8B models, model parameters less than 8B, under extreme low-bit compression. LieQ keeps uniform bit-width within each layer while mixing precision across layers, preserving standard multiplication kernels and avoiding irregular memory access, codebooks, or irregular formats at inference time. Our method uncovers a strong correlation between layer-wise functional saliency and representational compactness, revealing that layers with higher training-induced energy concentration are functionally irreplaceable. Leveraging this insight, we propose a purely geometry-driven sensitivity proxy that enables automatic bit-width allocation under a target average-bit budget without expensive gradient updates or inference-based perplexity probing. Under an average weight bit-width approaching two bits per parameter, LieQ consistently reduces the large accuracy gap typically observed for naive uniform 2-bit baselines on Qwen3 and LLaMA3.x families, while retaining standard-kernel efficiency. These properties make LieQ a practical path toward deploying small language models on resource-constrained edge devices. Code will be available at: https://github.com/HeXiao-55/LieQ-official.git.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：数十亿参数的大语言模型（LLM）在推理时存在严重的“过参数化”问题——大量层贡献的独特信息很少，却占据了大部分内存与能耗。这一问题在小语言模型（SLM，参数小于 8B）上尤为突出：这类模型冗余度低，在极低比特（约 2-bit）压缩下性能崩塌严重。
- **核心问题**：如何在无需重训练（post-training quantization, PTQ）的前提下，对 sub-8B 模型实现“极低比特 + 高精度保持 + 硬件友好的存储布局”三者兼得。
- **整体含义**：论文提出 LieQ（Layer-wise information effectiveness Quantization）框架，采用**层内均匀位宽、层间混合精度**的策略，在保留标准乘法内核（无码本、无规则外内存访问）的同时，将平均位宽压低至约 2-bit/权重，为边缘设备部署 SLM 提供了一条实用路径。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

### 核心思想
- 论文发现：**层的“功能显著性”（functional saliency，即层被删除后困惑度上升幅度）与“表征紧凑性变化”（representational compactness shift，训练带来的奇异值熵下降）之间存在强相关（Spearman ρ > 0.8）**。
- 该相关性使我们可以用**纯几何代理指标**替代昂贵的逐层删除探测（perplexity probing）来识别关键层，从而进行自动化的层间比特分配。

### 关键技术细节

**① 功能显著性（Functional Saliency）**
- 定义基线负对数似然：
  \[
  L_{\text{base}} = -\frac{1}{N}\sum_{i=1}^{N}\frac{1}{T}\sum_{t=1}^{T}\log p_M(x_t^{(i)} | x_{<t}^{(i)})
  \]
  PPL_base = exp(L_base)。
- 将第 ℓ 层替换为恒等映射后得到扰动模型 M\ℓ，记录困惑度变化：
  \[
  \Delta PPL_\ell = PPL_{\backslash \ell} - PPL_{\text{base}}
  \]
- 该指标直接反映层的不可替代性，但需要 (L+1) 次前向传播，计算成本极高。

**② 表征紧凑性（Representational Compactness）**
- 对某一投影矩阵 \(W_P^{(\ell)}\) 的表示 \(Z = W_P^{(\ell)} h^{(\ell)}\) 及其随机初始化对应物 \(\tilde{Z}\) 做 SVD，得到奇异值 \(\{\sigma_k\}\) 和 \(\{\tilde{\sigma}_k\}\)。
- 定义能量分布：
  \[
  p_k = \frac{\sigma_k^2}{\sum_{j=1}^{K}\sigma_j^2}
  \]
- 表征紧凑度（奇异值熵的指数）：
  \[
  \text{Compact}(Z) = \exp\left(-\sum_{k=1}^{K}p_k \log p_k\right)
  \]
- 层有效性指标（相对变化）：
  \[
  \Delta r_\ell^{(P)} = \frac{\text{Compact}(\tilde{Z}) - \text{Compact}(Z)}{\text{Compact}(\tilde{Z})}
  \]
  正值表示训练使表示更集中于低秩流形 → 该层更不可替代。
- 辅助指标 Top-k 能量差：
  \[
  E_k(Z) = \frac{\sum_{i=1}^{k}\sigma_i^2}{\sum_j \sigma_j^2},\quad \Delta E_{k,\ell}^{(P)} = E_k(Z) - E_k(\tilde{Z})
  \]
  用于验证紧凑性指标的一致性。

**③ 几何驱动的层选择与闭式预算分配**
- 每层综合得分：\(s_\ell = \mathbb{E}_{P,\text{head}}[\Delta r_\ell^{(P,\text{head})}]\)。
- 按得分降序选择高精度层集合 \(S_{\text{hi}} = \text{TopK}_m(s_1,\dots,s_L)\)，其余为低精度层 \(S_{\text{lo}}\)。
- 分配规则：高精度层 4-bit，低精度层 2-bit。
- 在给定平均位宽预算 \(\bar{b} \in [2,4]\) 时，可闭式求解高精度层数：\(m = \text{round}(fL)\)，其中 \(f = (\bar{b}-2)/(4-2)\)；若各层参数不均，则按参数计数 $\sum_{\ell \in S_{\text{hi}}} N_\ell \geq f\sum_\ell N_\ell$ 选取最少的层集合。
- 压缩比定义：
  \[
  CR = \frac{\sum_\ell b_\ell N_\ell}{16\sum_\ell N_\ell}
  \]
- **极端配置**：在论文主实验中，取高精度层为几何得分最高的 1 层，其余全部 2-bit（即逼近平均 2-bit 预算），以验证极限压缩条件下的精度下限。

**④ 硬件友好性**
- 层内位宽均匀 → 张量连续、单层只需一个乘法内核，避免了码本查找、不规则访存和运行时变换带来的额外开销。
- LieQ 与现有 PTQ 后端（如 GPTQ、AWQ）正交，可作为“即插即用”的比特分配策略。

## 3. 实验设计：使用了哪些数据集/场景、benchmark、对比方法

- **模型**：Qwen3（0.6B/1.7B/4B/8B）、LLaMA3.x（1B/3B/8B）、LLaMA1/2-7B，涵盖了主流 sub-8B 小模型家族。
- **语言建模数据集**：WikiText-2、C4（同时使用 Dolly-15k、HH-RLHF 做相关性诊断分析）。
- **零样本推理基准**：ARC-C/ARC-E、BoolQ、HellaSwag、PIQA、Winogrande、MMLU 共 7 个任务。
- **对比方法**：GPTQ、AWQ、OmniQuant、PB-LLM、SliM-LLM、QuIP#、AQLM 等 SOTA PTQ/压缩方法。
- **评估场景**：
  - 2-bit 与 3-bit 平均位宽下的困惑度对比（Table 1、2）；
  - 零样本推理任务精度对比（Table 3）；
  - 单层投影（gate_proj）延迟微基准（Figure 4）；
  - 端到端解码吞吐与内存占用对比（Table 4）；
  - 消融研究：高精度层数从 1 到 16 变化（覆盖 2-bit 到 3-bit 区间）对平均推理准确率的影响（Figure 5）。

## 4. 资源与算力

- **明确信息**：论文在“Evaluation Protocol”部分明确说明，所有实验在**单个 NVIDIA RTX 3090（24 GB）GPU** 上完成，使用混合精度，关闭梯度检查点（gradient checkpointing）以保留用于秩分析的激活。
- **未明确信息**：论文未报告具体的总 GPU 训练时长、各实验的具体耗时，也未提及是否使用了多卡并行。总体属于单卡可复现的量级。

## 5. 实验数量与充分性

- **实验数量**：
  - 2 个主要困惑度表（Table 1：Qwen3 在 Wiki 和 C4 上 2-bit/3-bit；Table 2：LLaMA3 同协议）；
  - 1 个零样本推理表（Table 3：3 个模型 × 2 种位宽 × 7 个任务）；
  - 1 个端到端性能表（Table 4：3 个模型）；
  - 延迟微基准（2 个模型 × 多序列长度）；
  - 消融实验（4 个模型 × 5 种配置）；
  - 相关性分析（Figure 1、2、6：涉及 8+ 个模型、4 个数据集）。
- **充分性判断**：
  - 覆盖面广：覆盖了 Qwen、LLaMA 两大主流家族的多个尺寸，以及语言建模与零样本推理两类任务，实验规模相对充分。
  - 公平性较好：困惑度与推理任务均按已有基准协议执行，对比基线取自原文或标准实现；序列长度统一为 512 与 AWQ 对齐。
  - 可改进处：零样本推理实验仅抽测了 Q3-4B、L2-7B、L3-3B 三个模型，未覆盖全部模型尺寸；未与最新的旋转/码本类方法（QuIP#、AQLM）在同一模型和位宽下全面对齐比较；Table 3 中部分基线的不适用项（N/A）未做补齐，存在轻微比较不对称。

## 6. 论文的主要结论与发现

- **核心发现**：层的功能显著性可以通过几何指标（奇异值熵的紧凑度变化）高精度预测（Spearman ρ > 0.8），且这一相关性在 Qwen 和 LLaMA 家族中跨数据集稳定存在。
- **关键洞察**：训练使得某些层将信息高度集中到低秩流形上，这些层对量化噪声极其敏感，是“不可替代层”；而冗余性高的层可以承受 2-bit 压缩。
- **实践结论**：**“保护最少的关键层（1 层 4-bit）+ 其余全 2-bit”** 的极端配置即可大幅缩小 naive 2-bit 基线的精度差距，在 Qwen3/LLaMA3.x 上达到 SOTA 水平。
- **规模效应**：模型越小，层间重要性越不均匀（如 Qwen-0.6B 有明显的关键层聚类），LieQ 的收益越显著；模型增大后层间差异趋于平缓。
- **硬件结论**：层内均匀位宽的混合精度策略保留了标准乘法内核，延迟和内存均优于 FP16 基线，且避免了码本/旋转等非规则格式的部署负担。

## 7. 优点：方法或实验设计上的亮点

- **无探测开销**：完全基于静态几何特征（SVD 奇异值分布）进行层选择，无需梯度更新、无需逐层删除的困惑度探测、无需验证集推理。
- **理论可解释性**：将“量化敏感性”归因于奇异值流形的结构性质，为“为什么某些层在量化下崩坏”提供了可验证的解释视角，而非纯经验结论。
- **硬件原生性**：层内均匀位宽 + 层间混合精度的设计兼顾压缩率与标准内核兼容性，避免了码本、不规则访存、运行时旋转等部署负担。
- **闭式预算分配**：在目标平均位宽约束下，高精度层数可直接由闭式规则求解，算法简单、可复现。
- **即插即用**：LieQ 与 GPTQ/AWQ 等现有 PTQ 后端正交，可作为通用比特分配层嵌入不同框架。
- **实验诊断视角新颖**：同时给出功能显著性、表征紧凑度、Top-k 能量三个指标的一致性验证，增强了结论的可信度。

## 8. 不足与局限

- **实验覆盖有限**：零样本推理任务只选取了部分模型做完整对比；未覆盖更大尺寸模型（>8B）或 MoE 架构；激活量化（activation quantization）与 KV cache 量化未纳入实验范围。
- **对标不完整**：与旋转/码本类方法（QuIP#、AQLM）的比较仅在个别模型上完成，且未完全对齐位宽（如 AQLM 的 2.02/2.29-bit 与 LieQ 的 2.00-bit 存在微小差异）。
- **最优点选择依赖先验**：分析中“保护 top-1 层”这一极端配置的选取虽然验证了下限，但实际部署时最优保护层数可能因任务、模型而变，论文未给出自适应选择标准。
- **端到端优化空间**：作者自述端到端推理吞吐优化仍停留在“简单估计”层面，真实设备的系统级部署（如算子融合、内存调度）尚未充分展开。
- **计算成本未披露**：SVD 诊断需要一次包含充分多样性输入的 forward pass，其时间/内存开销在论文中未单独量化；对超大模型或超长序列的适用性未讨论。
- **潜在偏差风险**：相关性分析基于 4 个数据集、约 8 个模型，虽覆盖了两大家族，但未能完全排除特定架构（如不同激活函数、归一化方式）下的结论迁移风险。

（完）
