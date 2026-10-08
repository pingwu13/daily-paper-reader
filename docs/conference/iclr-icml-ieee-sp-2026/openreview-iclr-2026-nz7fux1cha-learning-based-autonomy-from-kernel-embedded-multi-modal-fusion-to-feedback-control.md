---
title: Learning-Based Autonomy from Kernel-Embedded Multi-modal Fusion to Feedback Control
title_zh: 从核嵌入多模态融合到反馈控制的基于学习自主性
authors: Lakshman Mahto
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=nZ7fUX1cHa"
tags: ["query:micro-nuc-ai"]
score: 4.0
evidence: 融合多模态传感与反馈控制的基于学习自主性
tldr: 针对复杂系统需将异构传感器融合、数据驱动动力学学习与反馈控制统一为端到端自主回路的问题，本文提出基于核嵌入的自主框架。该方法通过加性/乘性核与条件均值嵌入把多模态数据映射到联合再生核希尔伯特空间，用核岭回归、深度核学习或贝叶斯深度网络学习动力学，并经动态规划或强化学习合成策略。工作给出闭式估计、有限样本与迭代复杂度刻画，并支持风险敏感规划与基于控制障碍函数的安全性。其贡献在于为安全自主控制提供了统一框架。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 复杂系统需要将异构传感器融合、数据驱动动力学学习与反馈控制耦合为端到端自主回路，但现有方法往往割裂处理。
method: 提出端到端自主框架，用加性/乘性核与条件均值嵌入将多模态数据映射到RKHS，用核岭回归或贝叶斯深度网络学习动力学，并经动态规划或强化学习合成控制策略。
result: 给出闭式估计、有限样本与迭代复杂度刻画，并支持风险敏感规划与控制障碍函数的安全性保障。
conclusion: 为需要安全自主运行与闭环控制的应用提供了统一的理论与方法框架。
---

## Abstract
In this work, we develop an end-to-end autonomy loop that couples \emph{kernel-embedded} multi-modal fusion with data-driven dynamics learning and feedback control. Heterogeneous sensor streams are embedded into a joint Reproducing Kernel Hilbert Space (RKHS) via additive/product kernels and conditional mean embeddings; dynamics are learned with kernel ridge regression (KRR), Deep Kernel Learning (DKL), or Bayesian deep neural networks (BDNNs); and policies are synthesized via dynamic programming (discrete and continuous-time HJB) or reinforcement learning with RKHS value functions. We present closed-form estimators, finite-sample and iteration-complexity characterizations, risk-sensitive planning with uncertainty, and safety via control barrier functions. We provide deployable algorithms, results and experiment in simulated robotics and precision irrigation.

---

## 论文详细总结（自动生成）

# 论文总结：从核嵌入多模态融合到反馈控制的基于学习自主性

> 说明：当前可获取的“PDF 提取文本”实际为 OpenReview 的浏览器验证/CAPTCHA 页面，未包含论文正文。以下总结主要依据论文摘要与 Markdown 元数据（title、tldr、motivation、method、result、conclusion、source 等）。凡正文未明确提供的实验、算力、对比方法等细节，均标注为“未说明/无法核验”。

## 1. 核心问题与整体含义
- **研究动机**：复杂系统需要把异构传感器融合、数据驱动动力学学习与反馈控制统一为一个端到端自主回路，但现有方法往往将这三部分割裂处理。
- **整体含义**：论文试图构建一个统一的“基于学习的自主性”框架，使多模态感知、动力学建模与控制策略合成能够在同一理论体系下衔接，服务于需要安全自主运行和闭环控制的应用。
- **关键词**：核嵌入、多模态融合、再生核希尔伯特空间（RKHS）、数据驱动动力学、反馈控制、风险敏感规划、控制障碍函数（CBF）。

## 2. 方法论
- **核心思想**：提出端到端自主框架，将核嵌入多模态融合、数据驱动动力学学习与反馈控制耦合起来。
- **多模态融合**：
  - 通过加性核/乘性核以及条件均值嵌入，把异构传感器流映射到联合 RKHS。
  - 目标是在统一空间中表示多模态数据，便于后续学习和控制。
- **动力学学习**：
  - 使用核岭回归（KRR）、深度核学习（DKL）或贝叶斯深度神经网络（BDNN）学习系统动力学。
- **策略合成**：
  - 通过动态规划求解，包括离散时间和连续时间 HJB 方程；
  - 或通过强化学习，并使用 RKHS 值函数。
- **理论刻画**：
  - 给出闭式估计；
  - 给出有限样本与迭代复杂度刻画；
  - 支持带不确定性的风险敏感规划；
  - 通过控制障碍函数提供安全性保障。
- **输出形式**：论文声称提供可部署算法、结果，并在模拟机器人和精准灌溉中进行实验。

## 3. 实验设计
- **场景**：摘要提到实验涉及 **模拟机器人（simulated robotics）** 与 **精准灌溉（precision irrigation）**。
- **数据集**：未说明使用了哪些具体数据集。
- **Benchmark**：未说明基准测试或标准 benchmark。
- **对比方法**：未说明与哪些方法进行了对比。
- **评价指标**：未说明性能指标、安全性指标或统计显著性分析。
- **可确认信息**：至少存在两个仿真应用场景；但具体实验设置、任务定义和结果细节无法从当前材料核验。

## 4. 资源与算力
- 当前材料中**未明确说明** GPU 型号、GPU 数量、训练时长、计算集群或其他算力资源。
- 因此无法判断该工作的训练成本、推理成本和可复现性所需资源。

## 5. 实验数量与充分性
- 从摘要只能确认论文在“模拟机器人”和“精准灌溉”中给出了实验结果，可能对应至少两个应用场景。
- 未说明是否进行了消融实验、不同核函数对比、不同动力学学习器对比、不同策略优化方法对比、安全性验证或鲁棒性测试。
- 未说明实验重复次数、统计显著性、基线公平性等。
- 因此，**实验充分性、客观性和公平性无法从当前材料判断**。元数据标注为 ICLR-2026-Rejected-Public、score 4.0，提示该工作可能在评审中存在争议或验证不足，但需正文与评审意见才能具体判断。

## 6. 主要结论与发现
- 论文提出一个统一框架，将核嵌入多模态融合、数据驱动动力学学习与反馈控制整合为端到端自主回路。
- 该框架支持多种动力学学习方式（KRR、DKL、BDNN）和多种策略合成方式（动态规划/HJB、强化学习）。
- 论文给出了闭式估计、有限样本与迭代复杂度刻画，并引入风险敏感规划与控制障碍函数安全性。
- 最终结论是：该工作为需要安全自主运行与闭环控制的应用提供了统一的理论与方法框架。

## 7. 优点
- **统一性强**：将感知融合、动力学学习和反馈控制放在同一个自主回路中，回应了现有方法割裂的问题。
- **理论工具丰富**：使用 RKHS、核嵌入、条件均值嵌入等工具，并提供闭式估计与复杂度刻画。
- **安全性关注**：引入风险敏感规划和 CBF，适合安全关键自主系统。
- **方法灵活**：动力学学习和策略合成均可选择多种实现路径。
- **应用导向明确**：面向机器人仿真与精准灌溉等闭环控制场景。

## 8. 不足与局限
- **材料受限**：当前 PDF 文本为验证页面，无法核验论文正文、公式、算法流程和实验细节。
- **实验信息不足**：未提供数据集、benchmark、对比方法、评价指标和消融实验，难以判断实验是否充分、公平。
- **算力未报告**：未说明 GPU 型号、数量、训练时长等资源信息。
- **真实世界验证不足**：摘要仅提到模拟机器人和精准灌溉，是否进行真实系统部署未说明。
- **可扩展性与实时性未知**：核方法在高维、大规模异构传感器流上的计算复杂度和实时控制可行性未在摘要中说明。
- **安全保证依赖假设**：CBF 和风险敏感规划通常依赖模型假设与约束条件，摘要未给出具体假设和失效边界。
- **评审信号**：来源标注为 ICLR-2026-Rejected-Public、score 4.0，提示该工作可能在评审中未获高分，但具体原因需结合正文和审稿意见判断。

（完）
