---
title: "WUSH: Near-Optimal Adaptive Transforms for LLM Quantization"
title_zh: WUSH：面向LLM量化的近最优自适应变换
authors: "Jiale Chen, Vage Egiazarian, Roberto L. Castro, Torsten Hoefler, Dan Alistarh"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e7e6ac3d59a279c67636b5371d44d9472885847d.pdf"
tags: ["query:ulbv"]
score: 9.0
evidence: 结合Hadamard主干与二阶矩的自适应变换，降低低比特LLM量化误差
tldr: 针对低比特权激活联合量化中的异常值问题，论文推导了块级线性变换的闭式最优解。WUSH结合Hadamard主干与数据相关的二阶矩分量，构造非正交近似最优变换。在整数和浮点量化器下均证明接近最优，能显著降低低比特量化误差。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 固定且数据无关的Hadamard等变换难以处理低比特量化中的极端异常值，最优性缺乏理论刻画。
method: 推导RTN AbsMax块量化下闭式最优线性块变换，融合Hadamard主线与二阶矩数据相关分量。
result: WUSH在FP和INT量化器下被证明近最优，能有效压缩动态范围、降低低比特量化误差。
conclusion: 数据自适应的非正交变换是提升LLM低比特量化的有效途径。
---

## Abstract
Quantizing LLM weights and activations is a standard approach for efficient deployment, but a few extreme outliers can stretch the dynamic range and amplify low-bit quantization errors. Prior transform-based mitigations (e.g., Hadamard rotations) are fixed and data-agnostic, and their optimality for quantization has remained unclear. We derive closed-form optimal linear blockwise transforms for joint weight-activation quantization under standard RTN AbsMax-scaled block quantizers, covering both integer and floating-point formats. The resulting construction, WUSH, combines a Hadamard backbone with a data-dependent second-moment component to form a non-orthogonal transform that is provably near-optimal for FP and INT quantizers under mild assumptions while admitting an efficient fused GPU implementation. Empirically, WUSH improves W4A4 accuracy over the strongest Hadamard-based baselines (e.g., on Llama-3.1-8B-Instruct in MXFP4, it gains +2.8 average points with RTN and +0.7 with GPTQ) while delivering up to 5.8$\times$ per-layer throughput over BF16 via FP4 MatMul. Source code is available at https://github.com/IST-DASLab/WUSH.

---

## 论文详细总结（自动生成）

### 论文总结：WUSH——面向LLM量化的近最优自适应变换

#### 1. 核心问题与整体含义
- **研究背景**：量化LLM的权重和激活是实现高效部署的标准方法，但少数极端异常值会拉大动态范围，从而放大低比特量化误差。
- **已有方案局限**：先前基于变换的缓解手段（如Hadamard旋转）是固定的且数据无关（data-agnostic），其最优性缺乏理论刻画，难以应对低比特量化中的极端异常值。
- **核心问题**：能否设计一种**数据自适应**的块级线性变换，在理论上证明其最优性，并在实际中显著降低低比特量化误差？
- **整体含义**：本文从理论上推导了联合权激活量化下块级线性变换的闭式最优解，并据此构建了实际可用的近最优非正交变换，为低比特量化提供了新的方法论支撑。

#### 2. 方法论
- **核心思想**：将Hadamard变换作为主干（backbone），融入数据相关的**二阶矩分量**，构造一个**非正交**的近似最优线性块变换，从而有效压缩动态范围。
- **理论推导**：在标准RTN + AbsMax缩放的块量化器下（同时覆盖整数INT与浮点FP格式），推导出块级线性变换的**闭式最优解**。
- **算法结构**：WUSH = Hadamard主干 + 数据相关二阶矩分量，构成非正交变换。
- **最优性保证**：在温和假设下，该变换被证明对FP和INT量化器均接近最优。
- **实现细节**：支持高效的融合GPU实现（fused GPU kernel），不引入显著推理开销。

#### 3. 实验设计
- **任务场景**：低比特联合权激活量化（W4A4），重点对比MXFP4（FP格式）下的表现。
- **Benchmark与模型**：在Llama-3.1-8B-Instruct上进行评测。
- **对比方法**：最强的Hadamard类基线（如Hadamard旋转方法），以及RTN和GPTQ两种量化器设置。
- **计量指标**：
  - 量化后模型准确率（平均分）；
  - 逐层吞吐量（对比BF16基线）。

#### 4. 资源与算力
- **文中未明确说明**：摘要与元数据中未披露GPU型号、数量、训练/推理时长等具体算力信息。
- **可确定的**：WUSH采用融合GPU实现，在FP4 MatMul下相对BF16可实现高达**5.8×逐层吞吐加速**，说明其计算开销较低。
- **说明**：由于缺乏明确资源描述，无法评估其总耗能与训练成本。

#### 5. 实验数量与充分性
- **已有实验**：
  - 单一模型（Llama-3.1-8B-Instruct）；
  - 两种量化器设置（RTN、GPTQ）；
  - 两类指标（准确率、吞吐量）；
  - 一种格式（MXFP4）的详细结果。
- **充分性评估**：
  - **正面**：完成了与最强基线的对比，且同时覆盖RTN与GPTQ，具有一定代表性；
  - **不足**：模型与数据集覆盖偏窄（单一模型、未见多种下游任务或更大规模模型），缺乏消融研究说明各组件的独立贡献，也未报告不同比特宽度（如W3A3、W2A2）下的表现。
  - **总体结论**：属于**初步验证性实验**，证明了方法的有效性与潜力，但全面性与泛化性证据有限。

#### 6. 主要结论与发现
- WUSH在FP和INT量化器下被证明**接近最优**，能有效压缩动态范围、降低低比特量化误差。
- 具体结果：
  - 在Llama-3.1-8B-Instruct、MXFP4格式下，相比最强Hadamard基线，W4A4准确率：
    - **RTN设置**：平均提升 +2.8 分；
    - **GPTQ设置**：平均提升 +0.7 分；
  - 通过FP4 MatMul实现最高**5.8×**逐层吞吐提升（对比BF16）。
- **总体结论**：数据自适应的非正交变换是提升LLM低比特量化的有效途径。

#### 7. 优点
- **理论贡献明确**：首次为块级线性变换在RTN AbsMax量化下的最优性提供闭式解，填补了理论空白。
- **方法设计巧妙**：Hadamard主干 + 二阶矩数据自适应分量的组合，兼顾了效率与效果。
- **强理论保证**：在温和假设下证明近最优性，非经验性调参。
- **工程实现可靠**：高效的融合GPU kernel，实际吞吐提升显著。
- **结果可信**：在RTN和GPTQ两种设置下均超过最强Hadamard基线，结论一致。

#### 8. 不足与局限
- **实验覆盖有限**：仅评测单一模型（Llama-3.1-8B-Instruct），未涉及更大规模（如70B）或更多架构（如Mistral、Qwen）的模型。
- **任务多样性不足**：未报告多种下游基准（如MMLU、GSM8K、WikiText等）上的详细结果，难以判断通用性。
- **缺乏消融分析**：未单独分析Hadamard主干与二阶矩分量各自的贡献，以及不同块大小的影响。
- **理论假设的局限性**：近最优性依赖于“温和假设”，在极端异常值分布下是否仍成立需要进一步验证。
- **未报告资源消耗**：未提及训练/校准数据的规模、所需时间或GPU资源配置，影响可复现性评估。
- **潜在偏差风险**：基准选择聚焦于Hadamard类方法，未与更多量化方法（如GPTQ、AWQ等）直接对比变换本身的有效性。

（完）
