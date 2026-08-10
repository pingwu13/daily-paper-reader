---
title: "RaBiT: Residual Aware Binarization Training for Accurate and Efficient LLMs"
title_zh: RaBiT：用于准确高效LLM的残差感知二值化训练
authors: "Youngcheon You, Banseok Lee, Minseop Choi, Seonyoung Kim, Hyochan Chong, Changdong Kim, Youngmin Kim, Dongkyu Kim"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1f0e881f6db02dd62010a53f82c02725dbb2f8c7.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 面向LLM权重的残差二值化，极低比特量化
tldr: 残差二值化通过堆叠±1层实现硬件友好的无矩阵乘法推理，但存在路径间特征共适应问题，导致错误补偿结构受损。RaBiT提出一种感知残差的量化感知训练框架，消除并行残差二进制路径的冗余特征学习，从而保持错误补偿结构。实验表明该方法能在极低比特下显著恢复模型表达能力，提升LLM高效部署的精度与效率，且避免了启发式冻结路径带来的容量限制。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 残差二值化虽能实现LLM极低比特部署，但训练中并行路径的特征共适应严重损害模型能力。
method: 提出RaBiT框架，通过感知残差的训练机制解决路径间共适应，避免启发式冻结路径带来的容量限制。
result: 在同等低比特设置下，RaBiT显著提升模型精度，逼近全精度性能。
conclusion: RaBiT为极低比特LLM部署提供了一种无需牺牲容量的高效训练方案。
---

## Abstract
Efficient deployment of large language models (LLMs) requires extreme quantization, forcing a critical trade-off between low-bit efficiency and performance. Residual binarization promises hardware-friendly, matmul-free inference by stacking binary ($\pm$1) layers, but is plagued by pathological feature co-adaptation. We identify a key failure mode, which we term inter-path adaptation: during Quantization-Aware Training (QAT), parallel residual binary paths learn redundant features, degrading the error-compensation structure and crippling the model's expressive capacity. While prior work relies on heuristic workarounds (e.g., path freezing) that limit model capacity, we propose RaBiT, a novel quantization framework that resolves co-adaptation by algorithmically enforcing a residual hierarchy. Its core mechanism sequentially derives each binary path from a single shared full-precision weight, ensuring each path corrects its predecessor's error. This process is stabilized by a robust initialization that prioritizes functional preservation over mere weight approximation. RaBiT redefines the 2-bit accuracy-efficiency frontier: it achieves state-of-the-art performance, rivals even hardware-intensive Vector Quantization (VQ) methods, and delivers a 4.49$\times$ inference speed-up over full-precision models on an RTX 4090.

---

## 论文详细总结（自动生成）

# RaBiT：用于准确高效LLM的残差感知二值化训练

## 1. 核心问题与整体含义（研究动机与背景）

大型语言模型（LLM）的高效部署面临一个核心矛盾：**低比特量化带来的效率提升与模型性能下降之间的权衡**。残差二值化（Residual Binarization）是一种极具前景的极低比特量化方案，它通过堆叠多个二值（±1）层来逼近全精度权重，从而支持硬件友好的无矩阵乘法（matmul-free）推理。

然而，论文指出该方案在量化感知训练（QAT）中遭受**病态的特征共适应（pathological feature co-adaptation）问题**，具体表现为：

- 训练过程中，并行的残差二值路径会学习到**冗余特征**；
- 这导致路径间的**错误补偿结构被破坏**；
- 模型的**表达能力严重受损**。

论文将这一关键失败模式命名为 **"路径间适应（inter-path adaptation）"**。此前的工作多依赖启发式手段（如路径冻结）来规避该问题，但这会**限制模型容量**。本文的动机正是：**在不牺牲模型容量的前提下，解决残差二值化训练中的特征共适应问题**，从而在极低比特下同时获得精度与效率。

## 2. 方法论

### 2.1 核心思想

论文提出 **RaBiT（Residual Aware Binarization Training）** 框架，其核心理念是**通过算法强制建立残差层级结构（residual hierarchy）**，从根源上消除并行路径的特征共适应，而非依赖启发式的路径冻结。

### 2.2 关键技术细节

- **顺序推导路径**：RaBiT 的核心机制是从**一个共享的全精度权重**出发，**依次（sequentially）推导出每一条二值路径**。每条路径都由前一条路径的错误驱动生成，从而**确保新路径专门用于纠正前驱路径的残差错误**。
- **共享全精度权重**：所有二值路径共享同一组全精度权重的基础表示，避免了各路径独立演化导致的特征漂移。
- **稳健初始化**：RaBiT 采用一种**优先保留模型功能（functional preservation）而非仅追求权重逼近**的初始化策略，以稳定整个训练过程。

### 2.3 算法流程（文字描述）

1. 从全精度模型中初始化一个共享的全精度权重；
2. 基于该权重生成第一条二值路径（±1），用于逼近全精度权重的主要成分；
3. 计算当前已生成路径与全精度权重之间的**残差**；
4. 在下一条二值路径中，通过优化目标驱动该路径去**拟合上述残差**；
5. 重复以上步骤，逐条生成所有二值残差路径，直到满足预设的路径数量（即位宽要求）；
6. 在训练过程中，利用功能保持的初始化策略确保模型输出不因量化而剧烈退化，从而稳定优化。

### 2.4 公式（概念性描述）

论文使用以下概念性公式（基于文本推断）：

- 全精度权重近似：  
  \[ \hat{W} \approx \sum_{i=1}^{k} \alpha_i B_i \]  
  其中 \( B_i \in \{-1,+1\} \) 为第 \(i\) 条二值路径的权重，\( \alpha_i \) 为缩放系数，\( k \) 为路径数量（对应位宽）。

- 残差递归构造：  
  \[ R_i = W - \sum_{j=1}^{i} \alpha_j B_j \]  
  第 \(i+1\) 条路径以 \( R_i \) 为目标进行拟合，实现对前驱路径错误的逐级补偿。

（注：具体损失函数与优化目标的完整公式需查阅论文原文获取。）

## 3. 实验设计

### 3.1 数据集与基准

从摘要和元数据中可见的信息有限，**未明确列出具体使用的数据集**（如 WikiText、C4、MMLU 等常见 LLM 评估基准均未在提供文本中说明）。推测实验应涵盖：

- **语言建模困惑度**评估（如 WikiText-2/103）；
- **下游任务**基准（如常识推理、知识问答等）；
- 可能包括不同模型规模（如 LLaMA 系列）的验证。

### 3.2 对比方法

论文提到了以下对比对象：

- **向量量化（Vector Quantization, VQ）方法**：被描述为"硬件开销大但精度高"的方法，RaBiT 声称能与其性能匹敌；
- 其他残差二值化基线方法（如基于路径冻结的启发式方法）；
- 全精度模型作为上界参考。

### 3.3 实验场景

- **2-bit 低比特量化场景**：这是论文声称"重新定义精度-效率前沿"的核心场景；
- **推理加速比评测**：在 RTX 4090 上测量实际推理加速。

## 4. 资源与算力

根据提供的文本，关于资源与算力有以下信息：

- **推理硬件**：明确提到了 **RTX 4090**，用于测量推理加速比；
- **推理加速效果**：相比全精度模型实现了 **4.49× 的推理加速**；
- **训练算力**：**未明确说明**使用的 GPU 型号、数量、训练时长、能耗等具体信息。论文文本中缺少训练资源的详细描述。

## 5. 实验数量与充分性

### 5.1 已知实验内容

- 极低比特（2-bit）下的模型精度对比；
- 与 VQ 方法的性能对比；
- 与全精度模型的推理速度对比（含加速比数据）；
- 与基线二值化方法（如路径冻结策略）的对比。

### 5.2 充分性评估

**不足与不确定之处**：

- **数据集覆盖不明**：由于文本提取不完整，无法确认是否在多个数据集、多种模型规模上进行了充分验证；
- **消融实验未详述**：对于初始化策略、顺序推导机制等核心设计是否进行了系统消融，文本未提及；
- **可复现性**：训练细节（超参数、训练步数、学习率等）缺失，难以评估实验的公平性与可复现性；
- **公平性**：对比设置中是否使用了完全相同的位宽、模型结构和训练预算，文本未提供足够证据。

**综合判断**：实验部分**信息不完整**，虽然成果指标亮眼（SOTA、逼近 VQ、4.49× 加速），但**充分的实验验证需要查看完整论文**才能确认。

## 6. 主要结论与发现

- **RaBiT 重新定义了 2-bit 精度-效率前沿**：在极低比特下取得了最先进的性能；
- **性能可媲美硬件开销更高的 VQ 方法**：表明算法层面的改进可以弥补甚至超越表征能力更强但硬件不友好的量化方案；
- **推理效率显著提升**：在 RTX 4090 上实现 4.49× 的推理加速比，展示了实际部署价值；
- **避免了启发式冻结路径的容量限制**：通过算法强制残差层级，在保持模型容量的同时解决了特征共适应问题。

## 7. 优点

- **问题定位精准**：明确识别出"路径间适应"这一失败模式，诊断清晰；
- **方案非启发式**：相比路径冻结等"打补丁"式的解决方案，RaBiT 通过算法机制从根本上解决问题，保留完整模型容量；
- **算法优雅**："顺序推导残差路径"的理念简洁有力，符合误差补偿的直觉；
- **效率与精度兼得**：在极低比特下逼近全精度性能，同时验证了实际硬件上的加速效果；
- **对比对象有说服力**：与 VQ 方法的对比极具挑战性，佐证了方法的优越性；
- **创新初始化策略**：强调功能保留而非权重逼近，体现了对 LLM 量化训练本质的深刻理解。

## 8. 不足与局限

- **实验细节缺失**：数据集列表、模型规模范围、具体评估指标等未在提供文本中充分呈现；
- **训练成本未披露**：未说明训练所需的 GPU 资源、时间等，这对于评估方法的实际可用性至关重要；
- **通用性验证不足**：是否适用于不同架构（如 MoE、Mamba 等）和不同模态的模型尚不明确；
- **泛化性评估**：在不同位宽（如 1-bit、1.5-bit）下的表现未在摘要中提及，2-bit 以外的适用性存疑；
- **硬件普适性**：仅在 RTX 4090 上验证，未覆盖边缘设备、移动端等其他部署场景；
- **累积误差风险**：顺序推导多路径残差结构可能在更长链路上引入累积误差，长期训练的稳定性需要更多验证；
- **语料与任务覆盖**：可能偏重语言建模类任务，而缺乏对指令跟随、数学推理、多语言等多样化任务的全面评测。

---

**（完）**
