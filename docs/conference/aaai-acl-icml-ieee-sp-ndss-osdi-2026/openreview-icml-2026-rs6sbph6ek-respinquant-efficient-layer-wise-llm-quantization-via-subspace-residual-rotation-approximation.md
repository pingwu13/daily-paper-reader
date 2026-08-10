---
title: "ReSpinQuant: Efficient Layer-Wise LLM Quantization via Subspace Residual Rotation Approximation"
title_zh: ReSpinQuant：基于子空间残差旋转逼近的高效逐层大语言模型量化
authors: "Suyoung Kim, Sunghyun Wee, Hyeonjin Kim, Kyomin Hwang, Hyunho Lee, Nojun Kwak"
date: 2026-04-30
pdf: "https://openreview.net/pdf/45980850254f4f9bfcdc15faf301bac9325c86b8.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 面向LLM的逐层旋转逼近量化，旨在将旋转融合到权重中
tldr: 针对旋转后训练量化中全局旋转表达力不足、逐层旋转又难以融合到权重并带来在线计算开销的问题，ReSpinQuant 提出子空间残差旋转逼近的逐层量化框架，在增强各层自适应能力的同时实现旋转矩阵的权重融合。实验表明该方法能显著降低在线计算成本并保持量化精度，为低比特大模型量化提供了高效的新方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有全局旋转PTQ表达受限，逐层旋转虽精度高但无法融合到权重，导致在线计算开销大。
method: 提出子空间残差旋转逼近，在逐层量化中近似融合旋转矩阵到权重，兼顾自适应性与推理效率。
result: 相比全局与逐层旋转方法，在保持精度的同时降低了在线计算开销，提升了LLM量化效率。
conclusion: ReSpinQuant为旋转式LLM量化提供了高效且高精度的逐层实现，可推动低比特部署。
---

## Abstract
Rotation-based Post-Training Quantization (PTQ) has emerged as a promising solution for mitigating activation outliers in the quantization of Large Language Models (LLMs). Global rotation methods achieve inference efficiency by fusing activation rotations into attention and FFN blocks, but suffer from limited expressivity as they are constrained to use a single learnable rotation matrix across all layers. To tackle this, layer-wise transformation methods emerged, achieving superior accuracy through localized adaptation. However, layer-wise methods cannot fuse activation rotation matrices into weights, requiring online computations and causing significant overhead. In this paper, we propose **ReSpinQuant**, a quantization framework that resolves such overhead by leveraging offline activation rotation fusion and matching basis using efficient residual subspace rotation. This design reconciles the high expressivity of layer-wise adaptation with only negligible inference overhead. Extensive experiments on W4A4 and W3A3 quantization demonstrate that ReSpinQuant achieves state-of-the-art performance, outperforming global rotation methods and matching the accuracy of computationally expensive layer-wise methods with minimal overhead.

---

## 论文详细总结（自动生成）

## 论文总结：ReSpinQuant：基于子空间残差旋转逼近的高效逐层大语言模型量化

> **说明**：以下总结基于所提供论文的摘要与元数据（来源为 ICML-2026-Accepted）。由于原始 PDF 内容未完整提供，部分实验细节（如具体数据集、对比方法实施细则、算力配置等）无法一一核实，文中将用"（未提供）"标明信息缺失之处，并基于领域常识与已有摘要内容做适当推断。

---

### 1. 核心问题与整体含义（研究动机与背景）

- **背景**：大语言模型（LLM）在量化过程中常因激活值存在极端离群点而严重损害量化精度。基于旋转的后训练量化（Rotation-based PTQ）通过在激活域上施加正交旋转，将离群点分散到更多维度，从而显著缓解这一问题，已成为该领域中一种有前景的解决方案。
- **已有方法的尚存问题**：
  - **全局旋转方法**（Global rotation）将单个可学习旋转矩阵应用于所有层，可事先与注意力模块和前馈网络（FFN）的权重进行融合，因此推理时无额外在线计算开销；但单矩阵的表达能力有限，无法适应各层不同的权重分布，导致精度受限。
  - **逐层旋转方法**（Layer-wise rotation）为每一层单独学习旋转矩阵，具备更高的局部自适应能力，通常能获得更优精度；但该方式无法将激活旋转矩阵融合到权重中，必须在推理时在线计算旋转变换，带来显著的额外计算开销。
- **核心研究问题**：如何在不牺牲推理效率的前提下，获得类似逐层旋转的自适应表达能力？
- **总体意义**：ReSpinQuant —— 一种新颖的量化框架，通过"离线激活旋转融合 + 匹配基向量 + 高效残差子空间旋转逼近"的方式，在保留逐层方法高精度的同时，将推理开销降到与全局旋转方法相当的水平，为低比特大模型部署提供了新思路。

---

### 2. 方法论：核心思想、关键技术细节与流程

- **核心思想**：将逐层旋转的**高表达能力**与全局旋转的**权重融合/低在线开销**进行统一。其关键在于：在逐层量化过程中，用一个**残差子空间旋转**来近似最优的逐层旋转，使得该近似旋转能够被离线融合进权重，而非在线计算。
- **子空间残差旋转逼近（Efficient Residual Subspace Rotation）**：
  1. 首先基于全局旋转得到基础解，或直接选取一种基础旋转；
  2. 计算逐层最优旋转与基础旋转之间的**残差**；
  3. 将这个残差限制在一个低维子空间中进行近似，从而该残差同样可被融合到权重中，不需要在推理时逐层计算旋转。
- **关键设计要素**：
  - **离线激活旋转融合**：将激活旋转操作在离线阶段并入注意力层与FFN的权重（如权重矩阵与旋转矩阵相乘），从而推理时无额外在线变换成本。
  - **匹配基（matching basis）**：确保子空间近似在数学上能够与各层权重进行有效对齐，降低近似误差。
  - 整体上相当于在逐层精度与全局计算开销之间取一个平衡点：表达力高于全局旋转、在线开销低于传统逐层旋转。
- **与已有方法的关系**：ReSpinQuant 不是完全抛弃逐层方法，而是对其旋转矩阵施加"可融合性"约束；也不同于全局旋转的单矩阵方案，它为各层保留自适应残差子空间。

---

### 3. 实验设计

- **量化设置**：
  - 实验覆盖 **W4A4**（权重4比特，激活4比特）和 **W3A3**（权重3比特，激活3比特）两种低比特量化配置。
- **对比方法**：
  - **全局旋转方法**（如基于旋转的PTQ基线，如SpinQuant等）；
  - **逐层旋转方法**（在线计算开销高的精细方法）。
- **数据集/场景**：
  - 论文摘要与元数据中**未明确列出**具体的评测数据集（如 WikiText-2、C4 等常见LLM困惑度基准，或具体下游任务）。
  - 按领域惯例推断，大概率使用了常见的语言建模困惑度基准（如 WikiText-2）及若干零样本下游任务，但**具体细节缺失**。
- **评估指标**：
  - 从摘要推测采用困惑度（Perplexity）或零样本任务准确率等指标，但**未具体说明**。
- **是否提及消融或附加实验**：
  - 元数据**未提供**消融实验信息，无法判断是否包含残差维数选择、基础旋转选择等分析。

---

### 4. 资源与算力

- 原文摘要与元数据中**未提供**任何关于 GPU 型号、数量、训练时长、显存占用等计算资源的信息。
- 需要指出：由于仅提供摘要，无法获取正文中可能存在的实验环境（如使用多少块 A100/H100、微调与验证耗时等）细节。

---

### 5. 实验数量与充分性

- **可用信息**：仅能确认 W4A4 与 W3A3 两组主要量化实验，以及与其他旋转方法的性能对比。
- **充分性评估**：
  - **不足之处**：仅凭摘要无法判断实验数据集的数量、是否覆盖多任务、是否有消融实验、是否报告了不同模型规模（如 7B、13B、70B 等）的表现。
  - **客观性风险**：摘要声称"达到最优性能（state-of-the-art）并匹配昂贵的逐层方法"，但缺少详细的误差条、多种随机种子、以及不同架构的验证。
  - 因此，单从摘要看**实验充分性和公平性无法充分确认**；结论中关于"高效性"的具体加速倍数与内存节省情况也未展示。

---

### 6. 主要结论与发现

- **精度方面**：ReSpinQuant 在 W4A4 与 W3A3 设置下，**优于全局旋转方法**，并能够**匹敌逐层旋转方法**的精度水平。
- **效率方面**：将旋转融合到权重中的做法，使其在线计算开销远低于传统逐层旋转方法，几乎与全局旋转方法持平。
- **核心贡献总结**：ReSpinQuant 解决了"逐层旋转无法融合权重"这一关键痛点，证明了**可以在不损失逐层自适应精度的情况下实现高效的端到端量化推理**，为旋转式LLM量化的实际部署提供了更优的解。

---

### 7. 优点

- **方法设计上的亮点**：
  - 巧妙利用**子空间残差逼近**，在数学构造上天然支持权重融合，避免了复杂的在线合并运算，思路新颖。
  - 兼具**逐层方法的表达能力**与**全局方法的推理效率**，兼顾了精度与部署实用性。
  - 将问题转化为离线可以完成的矩阵重参数化，使得推理时几乎零额外成本，工程实现友好。
- **实验设计上的亮点**（如果正文中有扩展）：
  - 同时覆盖 W4A4 与 W3A3 两种极端低比特设置，考察了方法在高压缩压力下的鲁棒性。
  - 与全局方法和逐层方法均做对比，能清晰地展示方法在精度-效率曲线上的定位。

---

### 8. 不足与局限

- **信息不全**：
  - 由于本次仅有摘要与元数据，未提供完整的实验表格、算法伪代码与公式，因此难以复现或深入验证。
- **可能存在的实验覆盖不足**：
  - 未明确提及模型规模（如小模型/大模型）与多个任务上的泛化性验证；
  - 未展示与"自适应分组量化"或"K-Means量化"等非旋转类近期的先进方法对比，缺乏更宽泛的SOTA基线；
  - 未讨论残差子空间维数的选取敏感性、不同基础旋转对性能的影响等关键超参数分析。
- **方法局限性**：
  - 子空间残差逼近的本质是在"完全逐层旋转"上做近似，必然存在一定信息损失，可能会在更低的比特（如W2A2）或极大规模模型上出现性能瓶颈，文中未讨论该情况。
  - 对不同模型架构（如 MoE、卷积型混合架构）的适用性未知。
- **效率增益的量化证据不足**：
  - 摘要中仅称"开销可忽略"，但未提供实测的每秒 Token 数、延迟、内存对比等具体数据，缺乏量化的说服力。
- **结果可复现性**：
  - 源信息中未给出代码库或超参数设置，复现存在一定障碍。

---

## 结语

ReSpinQuant 在旋转式 LLM 量化的精度-效率权衡问题上提出了一个优雅且富有潜力的解决方案：用残差子空间逼近逐层旋转，并将其与权重融合，从而同时获得逐层自适应能力和全局方法般的低推理开销。若正文实验严谨、覆盖足够，该方法有望在低比特 LLM 的落地部署中具有较高的实用价值。但当前提供的材料无法支持对实验充分性、计算资源配置和实际加速效果的详细评估，建议在获取完整论文后进一步核实。

（完）
