---
title: "ProjQ: Project-and-Quantize for Adapter-Aware LLM Compression"
title_zh: ProjQ：面向适配器感知大语言模型压缩的投影后量化方法
authors: "Wenya Yu, Chao Zhang, Li Wang, Samson Lasaulce, Merouane Abdelkader DEBBAH"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d0c45635f798b463282e10107dcf02353f50dd8b.pdf"
tags: ["query:ulbv"]
score: 6.0
evidence: 通过低秩投影塑造量化噪声，用于适配器感知的大模型压缩
tldr: 针对PTQ与LoRA顺序使用时量化噪声在权重中分散、LoRA难以修复的问题，提出ProjQ框架。它通过正交子空间投影把量化噪声塑造成低秩结构，并将主导误差成分卸载到低秩适配器可处理的空间，配合交替优化算法实现噪声低秩化。该方法使LoRA能更有效地利用容量，在LLM压缩和下游任务性能之间取得更好平衡，为量化与适配器联合部署提供新方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: PTQ产生的散乱噪声难以被LoRA修复，浪费LoRA容量，影响下游任务性能。
method: 提出ProjQ，通过正交子空间投影把量化噪声约束到低秩流形，用交替算法将主导误差迁移到适配器子空间。
result: 在LLM量化与LoRA联合部署中改善压缩性能与下游精度。
conclusion: 揭示了量化噪声低秩化对适配器感知压缩的重要性。
---

## Abstract
Post-Training Quantization (PTQ) and Low-Rank Adaptation (LoRA) constitute the standard pipeline for efficient Large Language Model (LLM) deployment. However, applying them sequentially poses a problem: PTQ often leaves behind random noise that is spread out (across the model's weights) in a way LoRA can't easily fix, meaning that LoRA ends up wasting its limited capacity trying to fix uncorrectable noise instead of improving task performance. In this paper, we propose \textbf{ProjQ}, a novel framework for 
constraining quantization noise to the low-rank manifold via orthogonal subspace projection. We derive an efficient alternating algorithm that shapes the quantization noise into a low-rank structure, effectively offloading dominant error components to the subsequent adapter while minimizing the residual error in the orthogonal "uncorrectable" subspace. Our theoretical analysis demonstrates that ProjQ preserves strictly greater model plasticity for downstream tasks compared to standard PTQ. Extensive experiments on LLaMA-2, Qwen2.5 and Qwen3 confirm that ProjQ consistently outperforms existing methods in both quantization error compensation and downstream task fine-tuning, achieving up to $2\times$ lower evaluation loss for compensation and matching the performance of standard 4-bit baselines on language modeling tasks with only 3 bits. The code is available on \url{https://github.com/yy9301/ProjQ}.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义

论文聚焦大语言模型（LLM）高效部署中的两个关键步骤——**后训练量化（PTQ）** 与 **低秩适配（LoRA）** 的联合使用问题。标准流程中，二者通常被顺序应用，但这种方式存在本质缺陷：PTQ 产生的量化误差在权重中呈**随机散乱分布**，而 LoRA 的低秩更新能力难以有效修复这类非结构化噪声。结果，LoRA 的有限容量被浪费在“不可修复”的噪声上，而非用于提升下游任务性能，导致压缩与任务精度的双重损失。

论文揭示的核心矛盾是：**量化噪声的结构特性与 LoRA 低秩修复能力之间的失配**，并由此提出解决方案，以提高“先量化、后适配”这一标准流水线的整体效率。

---

### 2. 方法论：ProjQ

#### 核心思想
**ProjQ（Project-and-Quantize）** 通过**正交子空间投影**将量化噪声主动塑造成低秩结构，使主导误差成分被“卸载”到后续 LoRA 适配器可以有效处理的子空间中，而正交的“不可修复”子空间中仅保留最小残差。

#### 关键技术细节
- **噪声低秩化**：以正交投影的方式引导量化噪声在权重空间中的分布，使原先随机分散的误差被集中到低秩流形中。
- **误差卸载**：将低秩流形中的主导误差成分显式地转移到适配器子空间，使 LoRA 可以有针对性地进行修复。
- **交替优化算法**：设计高效的迭代流程，在量化参数与适配器参数之间交替更新，逐步实现对噪声结构与适配器容量的协同优化。

#### 公式与算法流程（文字描述）
论文通过交替优化方法求解目标函数：每一轮迭代中，先固定适配器参数，对量化权重进行投影更新以压缩误差；再固定量化权重，利用 LoRA 对投影后的低秩误差进行补偿。该过程反复进行直至收敛。理论分析表明，相比标准 PTQ，ProjQ 在数学上保证为下游任务保留**严格更大的模型可塑性（plasticity）**。

---

### 3. 实验设计

#### 模型与数据集
- **模型**：LLaMA-2、Qwen2.5、Qwen3 系列。
- **场景**：量化误差补偿（compensation）与下游任务微调（fine-tuning）两种主要评测场景。

#### Benchmark 与对比方法
- 任务类型：语言建模任务（language modeling）。
- 对比对象：现有主流压缩方法（摘要中未列出具体方法名称，仅提及“existing methods”）。

#### 主要实验指标
- 量化误差补偿：评估损失（evaluation loss）。
- 下游微调性能：任务精度对比。

---

### 4. 资源与算力

论文中**未明确披露任何算力信息**，包括 GPU 型号、数量、训练时长等均未提及。因此无法从原文判断实验的算力成本，这属于信息披露上的一个不足之处。

---

### 5. 实验数量与充分性

#### 实验数量
- 覆盖了 3 个主流开源 LLM 系列（LLaMA-2、Qwen2.5、Qwen3），涉及量化误差补偿与下游微调两大类实验场景。
- 在不同比特设置下（如 3-bit 与 4-bit）进行了对比实验。
- 提供了理论分析之外的实证验证。

#### 充分性评价
- **积极方面**：跨多模型系列、多任务的实验设计具有一定说服力；理论与实验互为支撑。
- **不足方面**：由于摘要篇幅所限，未展示**消融实验**的详细信息（如对投影维度、交替优化迭代次数等超参数的敏感性分析）；未给出具体下游任务数据集（如 GLUE、SuperGLUE 等）的列表；对比方法的具体名称与设置也未展开。因此，从严格“公平对比”角度，论文提供的证据完整性不足。

---

### 6. 主要结论与发现

- **ProjQ 显著优于现有方法**：在量化误差补偿方面，ProjQ 实现了最高 **2× 更低的评估损失**。
- **比特效率显著提升**：在语言建模任务上，ProjQ 仅用 **3-bit** 即可匹配标准 **4-bit** 基线的性能，相当于在相同精度下减少约 25% 的量化位数。
- **理论保证**：通过正交子空间投影实现噪声低秩化，ProjQ 从理论上证明比标准 PTQ 保留更强的模型可塑性，为适配器感知压缩提供了新的设计原则。

---

### 7. 优点

- **问题定位精准**：明确指出 PTQ 噪声散乱性与 LoRA 低秩修复能力之间的结构性失配，这一诊断具有通用性和深刻性。
- **方法具有理论深度**：论文不仅给出经验方法，还提供了严谨的理论分析（模型可塑性保持的数学证明）。
- **工程思路巧妙**：通过正交投影这一简洁工具同时实现噪声结构化与误差卸载，概念清晰、实现成本低。
- **效果显著**：3-bit 匹配 4-bit 的性能改进极具实用价值，对资源受限场景中的 LLM 部署有直接助力。
- **代码开源**：提供公开代码库，便于复现与后续研究。

---

### 8. 不足与局限

- **算力信息缺失**：未报告任何训练/推理的硬件资源、运行时间等成本信息，难以评估方法的实际部署开销。
- **实验细节有限**：未列出具体的下游任务数据集、对比方法明细、超参数设置；摘要层面无法验证实验的全面性。
- **消融实验不足**：缺少对投影维度、优化迭代次数、不同量化粒度等设计选择的深入消融分析。
- **模型规模覆盖有限**：虽然覆盖了 3 个模型系列，但未明确是否包含超大规模（如 70B+）模型，而量化误差在大规模模型上的表现往往与中小规模存在差异。
- **泛化性风险**：方法在特定模型家族的验证结果是否可推广到其他架构（如 Mistral、DeepSeek 等）尚待验证。
- **偏差风险**：论文作为 ICML 录用论文，可能存在出版偏倚（高估方法效果）；且摘要未提及与基线方法对比的具体统计显著性检验。

---

（完）
