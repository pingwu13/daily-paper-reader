---
title: "Agentic Surrogates: Automating Proxy Models of Simulators with Compute Aware Intelligence"
title_zh: 智能代理代理模型：以计算感知智能自动化仿真器的代理模型
authors: "Pradeep Kumar Shetty, Antonio Massoni Abinader, Tammy Lam"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=qxZZ9s3kNN"
tags: ["query:micro-nuc-ai"]
score: 4.0
evidence: 领域无关的智能体框架，自动化构建仿真器的代理模型
tldr: 高保真仿真器的代理模型对加速科学计算至关重要，但其构建长期依赖人工、样本效率低且难以复现，尤其在物理约束严格、仿真昂贵的场景下。本文提出一个领域无关的智能体框架，由推理引擎端到端编排候选生成、批量仿真调用、热启动重训练、不确定性与标定以及动态采集切换。实验表明该框架能以最少的墙钟时间和仿真调用次数达到用户指定的精度目标。这为科学计算中代理模型的自动化构建提供了可复用的通用范式，可迁移至核反应堆代理建模等任务。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 高保真仿真器代理模型的构建长期依赖人工、样本效率低且难以复现，尤其在物理约束严格、仿真昂贵的场景下。
method: 提出领域无关的智能体框架，由推理引擎编排候选生成、批量仿真调用、热启动重训练、不确定性与标定以及动态采集切换。
result: 该框架以最少的墙钟时间和仿真调用次数达到用户指定的精度目标。
conclusion: 为科学计算中代理模型的自动化构建提供了可复用的通用范式，可迁移到核反应堆代理建模。
---

## Abstract
Proxy (surrogate) models are indispensable for accelerating scientific computation, yet creating them remains a manual, sample-inefficient, and non-reproducible process—especially when simulators are costly and constrained by physics. We present a domain-agnostic agentic framework that automates end-to-end surrogate construction for high-fidelity simulators. At its core, a reasoning engine orchestrates the entire loop, encompassing space-filling candidate generation, batched simulator invocation, warm-start retraining, uncertainty/calibration, and dynamic acquisition switching to achieve user-specified accuracy with minimal wall-clock time and simulator calls. Crucially, this reasoning engine also intelligently constructs and orchestrates an adaptive ensemble of models, where individual components or techniques are dynamically selected based on the specific characteristics of different input and output combinations. For instance, if the agent discerns simpler dependencies, such as linear relationships or reliance on fewer inputs within a particular output ensemble, it can deploy models with reduced complexity or optimized for computational efficiency. Conversely, for complex, highly non-linear, or high-dimensional problems, it will automatically integrate and leverage more sophisticated architectures within this ensemble. This approach ensures that the overall surrogate is a finely tuned composition of expert models, each optimally suited to distinct aspects of the simulator's behavior. The controller manages acquisition rules as a portfolio of experts (residual-error, MC-Dropout variance, EI/EGO, hybrid, randomized) and optimizes a compute-aware objective (error reduction per minute), with extensible support for multi-fidelity scheduling. The framework remains neutral to underlying architecture (e.g., ANNs, PINNs, operator networks such as FNO/DeepONet) and integrates physics-aware stopping alongside MC-Dropout and conformal prediction for calibrated uncertainty.
We evaluate this approach on energy-related scientific modeling problems, including industrial-style flow/process simulators and a PDE proxy. Our findings reveal consistent and substantial performance improvements compared to fixed strategies, demonstrating notable advancements in both sample efficiency and time-to-target accuracy. Prior pilots in oil & gas demonstrated substantially reduced predictive errors compared to the best single acquisition policy while converging faster; here we generalize the method beyond any single domain, preserving these gains under diverse physics and cost profiles. A key novelty lies in the central contribution of introducing compute-aware regret guarantees, which are emphasized up front.

---

## 论文详细总结（自动生成）

# 论文总结：Agentic Surrogates: Automating Proxy Models of Simulators with Compute Aware Intelligence

> 说明：抓取到的 PDF 内容实际为 OpenReview 验证页面，未能提取论文正文。以下总结主要依据论文摘要与元数据；凡正文未明确说明之处，均已标注为“未说明/无法确认”。元数据标注该文来源为 ICLR-2026-Rejected-Public，score 为 4.0。

## 1. 核心问题与整体含义
- **研究动机**：高保真仿真器的代理模型（surrogate/proxy models）对加速科学计算至关重要，但其构建长期依赖人工、样本效率低、难以复现，尤其在物理约束严格、仿真调用昂贵的场景下。
- **核心问题**：如何自动化、端到端地为昂贵高保真仿真器构建高质量代理模型，并在用户指定精度下尽量减少墙钟时间与仿真调用次数。
- **整体含义**：论文提出一个领域无关的智能体框架，试图把代理模型构建从手工流程转为可复用的自动化范式，并可迁移到核反应堆代理建模等任务。
- **关键词**：代理模型、智能体、计算感知、采集策略、自适应集成、不确定性校准、多保真调度。

## 2. 方法论
- **核心思想**：由一个推理引擎端到端编排代理模型构建循环，自动决定候选样本、模型选择、采集策略与停止时机。
- **算法流程（文字说明）**：
  - 空间填充式候选生成；
  - 批量调用仿真器；
  - 热启动重训练；
  - 不确定性估计与标定；
  - 动态切换采集策略；
  - 在达到用户指定精度或满足物理感知停止条件后终止。
- **自适应模型集成**：推理引擎会根据不同输入/输出组合的特征，动态选择或组合模型与技术。
  - 对简单依赖，如线性关系或较少输入，使用低复杂度或计算高效模型；
  - 对复杂、高非线性或高维问题，自动集成更复杂架构。
- **采集策略组合**：控制器将采集规则视为“专家组合”，包括残差误差、MC-Dropout 方差、EI/EGO、混合、随机等，并优化计算感知目标，即“每分钟误差降低量”。
- **多保真与架构中立**：支持多保真调度扩展；框架不绑定具体架构，可兼容 ANN、PINN、FNO/DeepONet 等算子网络。
- **不确定性处理**：结合物理感知停止、MC-Dropout 与 conformal prediction，以提供校准后的不确定性。
- **理论亮点**：摘要强调引入“计算感知遗憾保证”（compute-aware regret guarantees）是核心新颖性，但未给出具体假设、界或证明细节。

## 3. 实验设计
- **场景/数据**：
  - 能源相关科学建模问题；
  - 工业风格流动/过程仿真器；
  - 一个 PDE 代理任务；
  - 先前在油气领域的试点。
- **Benchmark**：摘要未明确给出标准 benchmark 名称，更像领域内案例评估，而非公开统一基准。
- **对比方法**：
  - 固定策略；
  - 最佳单一采集策略。
- **评估指标**：
  - 样本效率；
  - 达到目标精度的时间（time-to-target accuracy）；
  - 预测误差；
  - 墙钟时间；
  - 仿真调用次数。
- **公平性说明**：摘要声称在多样物理与成本 profile 下保持增益，但未说明预算对齐、重复次数、统计检验等细节。

## 4. 资源与算力
- 摘要与元数据**未说明**使用的 GPU 型号、数量、训练时长、CPU 配置或总计算资源。
- 文中强调“最小墙钟时间”和“最少仿真调用次数”，但这是优化目标与评估指标，不等同于硬件算力披露。
- 因此，无法从现有材料判断该框架的实际计算开销与可复现算力需求。

## 5. 实验数量与充分性
- 从摘要可识别的实验簇大致包括：
  - 能源相关 flow/process 仿真器；
  - PDE 代理；
  - 油气领域试点；
  - 多类采集策略与固定策略对比。
- **具体实验组数、数据集规模、消融实验、随机种子、统计显著性检验均未说明**。
- 由于缺少正文，无法确认实验是否充分、是否客观公平。
- 从摘要看，作者有跨领域、跨物理与成本 profile 泛化的意图，但证据细节不足，需正文验证。

## 6. 主要结论与发现
- 该框架能以较少的墙钟时间和仿真调用次数，达到用户指定的精度目标。
- 相比固定策略，方法在样本效率与达到目标精度的时间上取得一致且显著的提升。
- 在先前油气试点中，相比最佳单一采集策略，预测误差显著降低且收敛更快。
- 方法被推广到单一领域之外，声称在多样物理与成本 profile 下仍保持增益。
- 为科学计算中代理模型的自动化构建提供了可复用的通用范式，可迁移至核反应堆代理建模等任务。

## 7. 优点
- **端到端自动化**：将候选生成、仿真调用、重训练、不确定性与采集切换整合为一个智能体循环。
- **领域无关**：不绑定具体物理领域，具备跨任务迁移潜力。
- **自适应集成**：根据输入/输出组合动态选择模型复杂度，兼顾效率与精度。
- **采集策略组合化**：将多种采集规则作为专家组合，而非固定单一策略。
- **计算感知目标**：优化“每分钟误差降低”，贴近实际科学计算成本约束。
- **不确定性校准**：结合 MC-Dropout 与 conformal prediction，并支持物理感知停止。
- **架构中立与多保真**：兼容 ANN、PINN、FNO/DeepONet，并支持多保真调度扩展。
- **理论卖点**：提出计算感知遗憾保证，若成立，可为自动化代理建模提供更强理论支撑。

## 8. 不足与局限
- **材料限制**：当前 PDF 提取内容为验证页，无法获得正文，许多关键细节无法核实。
- **实验披露不足**：未说明数据集名称、规模、标准 benchmark、具体实验组数、消融与统计检验。
- **算力未披露**：未给出 GPU 型号、数量、训练时长等，影响复现与成本评估。
- **理论细节缺失**：compute-aware regret guarantees 的假设、证明与适用范围未在摘要中展开。
- **应用限制**：方法依赖可调用的仿真器、成本模型与合适的候选空间；若仿真极贵或物理约束极强，仍可能受限。
- **偏差风险**：结果主要来自作者自述与领域内案例，缺少第三方复现和公开基准对比。
- **评审信号**：元数据标注为 ICLR-2026-Rejected-Public，score 4.0，提示其贡献或实验充分性可能未获会议认可，需谨慎看待结论强度。

（完）
