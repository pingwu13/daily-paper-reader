---
title: "Decoupled-Value Attention for Prior-Data Fitted Networks: GP-Inference for Physical Equations"
title_zh: 面向先验数据拟合网络的解耦值注意力：用于物理方程的GP推断
authors: "Kaustubh Sharma, Simardeep Singh, Parikshit Pareek"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=noLMXTqgCp"
tags: ["query:micro-nuc-ai"]
score: 4.0
evidence: 用先验数据拟合网络构建物理系统快速代理模型
tldr: 针对高斯过程推断耗时、先验数据拟合网络在高维回归上效果受限的问题，本文提出解耦值注意力DVA。该方法仅从输入计算相似度，并让标签信息只经由值传播，以贴合高斯过程函数空间由核函数决定、预测均值为训练目标加权和的性质。实验显示其提升了先验数据拟合网络在高维回归上的表现，可作为物理系统的快速代理模型。其贡献在于为科学机器学习中的高效代理建模提供了新的注意力设计。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 高斯过程推断耗时，先验数据拟合网络可用单次前向替代贝叶斯推断以快速构建物理系统代理模型，但标准注意力在高维回归上效果有限。
method: 提出解耦值注意力DVA，仅从输入计算相似度并让标签仅通过值传播，从而更贴合核函数刻画函数空间、预测均值为训练目标加权和的高斯过程性质。
result: 该设计提升了先验数据拟合网络在高维回归任务上的有效性，可作为物理系统的快速代理模型。
conclusion: 为科学计算中快速代理模型的构建提供了更有效的注意力机制，可迁移到反应堆物理等需要快速代理的建模场景。
---

## Abstract
Prior-data fitted networks (PFNs) are a promising alternative to time-consuming Gaussian process (GP) inference for creating fast surrogates of physical systems. PFN reduces the computational burden of GP-training by replacing Bayesian inference in GP with a single forward pass of a learned prediction model. However, with standard Transformer attention, PFNs show limited effectiveness on high-dimensional regression tasks. We introduce Decoupled-Value Attention (DVA)-- motivated by the GP property that the function space is fully characterized by the kernel over inputs and the predictive mean is a weighted sum of training targets. DVA computes similarities from inputs only and propagates labels solely through values. Thus, the proposed DVA mirrors the GP update while remaining kernel-free. We demonstrate that PFNs are backbone architecture invariant and the crucial factor for scaling PFNs is the attention rule rather than the architecture itself. Specifically, our results demonstrate that (a) localized attention consistently reduces out-of-sample validation loss in PFNs across different dimensional settings, with validation loss reduced by more than 50\% in five- and ten-dimensional cases, and (b) the role of attention is more decisive than the choice of backbone architecture, showing that CNN, RNN and LSTM-based PFNs can perform at par with their Transformer-based counterparts. The proposed PFNs provide 64-dimensional power flow equation approximations with a mean absolute error of the order of $10^{-3}$, while being over $80\times$ faster than exact GP inference.

---

## 论文详细总结（自动生成）

> 说明：当前提供的 PDF 文本仅为 OpenReview 验证页面，未包含论文正文。以下总结主要依据题名、摘要与元数据生成；凡原文材料未明确说明之处，均标注为“未说明”或“无法确认”。

## 论文总结：面向先验数据拟合网络的解耦值注意力（DVA）

### 1. 核心问题与整体含义
- **研究动机**：高斯过程（GP）推断通常耗时，难以满足物理系统快速代理建模需求。
- **PFN 的潜力**：先验数据拟合网络（Prior-data Fitted Networks, PFNs）用单次前向传播替代 GP 中的贝叶斯推断，从而降低 GP 训练与推断的计算负担。
- **关键瓶颈**：标准 Transformer 注意力在高维回归任务上效果有限，限制了 PFN 作为物理系统快速代理模型的能力。
- **整体含义**：论文试图通过改进注意力机制，使 PFN 更贴近 GP 的函数空间性质，从而提升高维回归表现，并为科学机器学习中的高效代理建模提供新设计。

### 2. 方法论：解耦值注意力（DVA）
- **核心思想**：GP 的函数空间由输入上的核函数完全刻画，预测均值是训练目标的加权和。DVA 据此将“相似度计算”和“标签传播”解耦。
- **关键技术细节**：
  - 注意力相似度仅从输入 \(x\) 计算，不依赖标签 \(y\)。
  - 标签信息只通过 value 分支传播。
  - 输出预测可理解为训练目标值的加权和，权重来自输入之间的相似度。
- **与 GP 的关系**：DVA 镜像了 GP 更新形式，即预测均值由训练目标加权得到，但保持 **kernel-free**，不需要显式定义核函数。
- **局部注意力**：摘要提到 localized attention 可持续降低 PFN 的样本外验证损失，表明局部化/解耦的注意力权重设计是提升泛化的关键。
- **架构无关性**：论文强调 PFN 对 backbone 架构不敏感，真正影响扩展性的是注意力规则，而非 Transformer、CNN、RNN 或 LSTM 本身。

### 3. 实验设计
- **任务/场景**：
  - 高维回归任务。
  - 物理方程近似，具体包括 64 维潮流方程（power flow equation）近似。
  - 不同维度设置：5 维、10 维、64 维。
- **Benchmark/指标**：
  - 样本外验证损失（out-of-sample validation loss）。
  - 平均绝对误差（MAE）。
  - 推理速度，与精确 GP 推断比较。
- **对比方法/基线**：
  - 标准 Transformer 注意力构建的 PFN。
  - 不同 backbone 的 PFN：CNN、RNN、LSTM 与 Transformer 对比。
  - 精确 GP 推断（exact GP inference）。
  - 局部注意力与标准注意力的效果对比。
- **未说明项**：具体数据集名称、样本量、训练/测试划分、超参数搜索策略等未在现有材料中说明。

### 4. 资源与算力
- 现有材料**未明确说明**使用的 GPU 型号、数量、训练时长或总计算量。
- 摘要仅提到推理阶段比精确 GP 推断快 **80 倍以上**，但这属于速度结果，不等同于训练算力报告。
- 因此无法评估该方法的训练成本、能耗或可复现性所需硬件条件。

### 5. 实验数量与充分性
- 从现有信息看，实验至少覆盖：
  - 多个维度设置：5 维、10 维、64 维。
  - 多种 backbone：Transformer、CNN、RNN、LSTM。
  - 与精确 GP 的精度和速度对比。
  - 局部注意力/解耦注意力的效果验证。
- **充分性判断**：
  - 初步结果支持 DVA 在高维回归上的有效性。
  - 但受限于正文不可得，无法确认是否包含多数据集、多种物理方程、随机种子重复、误差棒或统计显著性检验。
  - 因此实验充分性只能视为“部分可确认”，尚不足以全面判断。
- **公平性**：
  - 与精确 GP 和标准 PFN 对比是合理基线。
  - 但若仅报告部分维度或部分任务，可能存在选择性展示风险；现有材料无法排除。

### 6. 主要结论与发现
- DVA 能提升 PFN 在高维回归任务上的表现。
- 在 5 维和 10 维设置中，样本外验证损失降低超过 **50%**。
- 注意力规则比 backbone 架构选择更关键；CNN、RNN、LSTM 构建的 PFN 可达到与 Transformer 版本相当的水平。
- PFN 在 64 维潮流方程近似中达到约 \(10^{-3}\) 量级的 MAE。
- 相比精确 GP 推断，所提方法速度提升超过 **80 倍**，可作为物理系统的快速代理模型。
- 元数据指出，该思路可迁移到反应堆物理等需要快速代理建模的场景。

### 7. 优点
- **动机清晰**：从 GP 的核加权性质出发设计注意力，理论直觉较强。
- **方法简洁**：解耦 query/key 与 value，使标签只通过 value 传播，易于理解和实现。
- **kernel-free**：在模拟 GP 更新形式的同时，不依赖显式核函数。
- **架构无关性发现**：强调注意力规则比 backbone 更重要，对 PFN 设计有指导意义。
- **高维与物理场景结合**：在 64 维潮流方程上展示精度和速度优势，具有科学计算应用潜力。
- **效率优势明显**：相比精确 GP 推断有显著加速，适合快速代理建模。

### 8. 不足与局限
- **正文不可得**：当前 PDF 文本仅为验证页面，无法核验方法细节、实验设置和理论分析。
- **实验覆盖有限**：现有材料主要涉及物理方程/潮流方程，未说明是否覆盖更广泛的回归数据集或真实科学场景。
- **算力未报告**：缺少 GPU 型号、数量、训练时长等，难以评估训练成本和可复现性。
- **对比基线有限**：主要对比精确 GP 和不同 backbone 的 PFN，未说明是否与其他代理模型或神经算子方法比较。
- **统计严谨性未知**：未说明随机种子、误差棒、显著性检验，结论稳健性无法完全确认。
- **应用限制**：方法仍依赖先验数据拟合与训练分布；对分布外物理条件、不同方程形式或极端工况的泛化能力未说明。
- **维度规模**：64 维已属较高，但相比真实大规模物理系统仍可能有限；更高维扩展性未明确验证。
- **可迁移性需验证**：元数据提到可迁移到反应堆物理等场景，但当前材料未给出实际迁移实验。

（完）
