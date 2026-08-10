---
title: "Bi-VLM: Binary Post-Training Quantization for Vision-Language Models"
title_zh: Bi-VLM：视觉-语言模型的二值后训练量化
authors: "Xijun Wang, Rayyan Abdalla, Junyun Huang, Chengyuan Zhang, Ruiqi Xian, Dinesh Manocha"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37989/41951"
tags: ["query:ulbv"]
score: 6.0
evidence: 面向视觉-语言模型的二值后训练量化，追求极低比特权重精度
tldr: 尽管大规模视觉-语言模型性能优异，但其计算与存储需求巨大。Bi-VLM 通过二值后训练量化将权重压缩至极低比特，直接应对这一瓶颈。方法围绕视觉-语言模型的结构特性设计量化策略，旨在尽可能保持多模态任务性能。实验表明二值 PTQ 能在极低比特下显著降低资源消耗，为后续多模态模型的边缘部署提供了新思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37989/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1826, \"height\": 704, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37989/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1828, \"height\": 362, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37989/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1831, \"height\": 365, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37989/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1829, \"height\": 364, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37989/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 164, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37989/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 871, \"height\": 168, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37989/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 867, \"height\": 122, \"label\": \"Table\"}]"
motivation: 视觉-语言模型计算与存储开销大，现有二值量化技术难以满足其极低比特权重精度需求。
method: 提出 Bi-VLM 二值后训练量化框架，针对视觉-语言模型特性设计量化策略，实现极低比特权重压缩。
result: 实验验证了二值 PTQ 在视觉-语言模型上的可行性，能有效降低计算与存储需求。
conclusion: Bi-VLM 为多模态模型超低位宽量化部署提供有效解决方案。
---

## Abstract
We address the critical gap between the computational demands of vision-language models and the possible ultra-low-bit weight precision (bitwidth

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：视觉-语言模型（VLM）在各多模态任务上表现出色，但庞大的参数量和高昂的计算/内存开销严重制约了其在可穿戴设备、移动端、FPGA 等资源受限环境中的部署。
- **核心问题**：现有的后训练量化（PTQ）方法在 8-bit 和 4-bit 精度下表现良好，但推到超低比特（≤2 bits）时性能急剧退化；已有的二值化 PTQ 方法（如 PB-LLM、BiLLM）在 VLM 场景下也远未达到可用水平。
- **论文意义**：本文提出 Bi-VLM，首次系统性地探索了 VLM 的 ≤2 bits 后训练二值量化，填补了极低比特量化与 VLM 部署需求之间的空白，为多模态模型在边缘设备上的高效部署提供了可行方案。

## 2. 方法论

- **核心思想**：基于权重分布的统计特性，将每一层的权重按高斯分位数非均匀地划分为“异常/显著权重（salient）”与多个“非显著权重（unsalient）”子集，对显著权重用 2-bit 量化、非显著权重用 1-bit 二值化，从而在极低平均位宽下兼顾精度与压缩率。

- **关键技术细节**：
  1. **高斯分布假设与验证**：直方图分析表明视觉模型和语言模型的权重均呈现近似零均值的高斯分布形态，为分位数分割提供了依据。
  2. **基于高斯分位数的权重分割**：以层为单位，利用均值 μₗ 和标准差 σₗ，通过逆高斯 CDF（probit 函数）计算 z-score 阈值，将权重划分为显著集 Sₗ 和 N_uns 个非显著子集 S_c(k)，使每个子集的实际权重比例与分位数一致。
  3. **显著性感知混合量化**：
     - 显著权重 W_sal：使用 2-bit 量化，求解带行级缩放因子的凸优化问题（交替优化缩放向量 a 和二进制矩阵 B），并通过指数自适应量化级别（公式 14）对异常值分布的不均匀尾部提供更好的分辨率。
     - 非显著权重 W_uns(k)：严格二值化为 {−1, +1}，缩放因子由解析闭式解（公式 18）给出，即最优缩放 = ⟨W, B⟩ / ‖B‖²_F。
  4. **自适应显著性搜索**：将显著权重的比例 p_sal 视为可优化变量，以归一化重构误差（公式 20）为目标，使用 Brent 无梯度优化方法搜索最优 p_sal，在性能与压缩率之间取得平衡。
  5. **算法流程**：Algorithm 1 处理显著权重的行级 2-bit 量化；Algorithm 2 处理非显著权重的二值化；Algorithm 3 给出整体流程（分割 → 优化 p_sal → 混合量化 → 重构）。

## 3. 实验设计

- **基准数据集（4 个）**：
  - **MME**（Perception / Cognition）：多模态感知与认知能力评测。
  - **MMMU**：大学级别多学科知识与推理能力评测。
  - **ScienceQA-IMG**：科学问答多模态推理任务。
  - **VizWiz-VQA**：面向盲人的视觉问答任务。

- **模型（3 个 VLM）**：
  - Llama 3.2-Vision instruction 11B
  - Llava-One-Vision 7B
  - Qwen2.5-VL-7B-Instruct

- **对比方法**：
  - AWQ（Activation-aware Weight Quantization，SOTA 4-bit 方法）
  - BiLLM（面向 LLM 的二值化 PTQ，SOTA 超低比特方法）
  - 对比设置包括“仅量化语言模型部分”（L）和“量化整个 VLM”（all）两种场景。

- **实验配置**：校准集 64 个样本；显著权重用 2-bit、非显著权重用 1-bit；显著权重的比例上限约为 5%；量化后平均位宽约 1.0–1.1 bits。

## 4. 资源与算力

- **论文未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。
- 但论文在效率对比中给出了量化耗时数据：在 Llama 3.2-Vision-instruction-11B + ScienceQA-IMG 设置下，Bi-VLM 的量化耗时为 **9.6 分钟**，而 BiLLM 为 **139.8 分钟**，即实现了约 **14.6 倍的量化速度提升**，同时精度更高、平均位宽更低。

## 5. 实验数量与充分性

- **主实验**：3 个模型 × 4 个基准 × 2 种量化场景（语言模型部分 / 整个 VLM），共 12 组对比结果，覆盖了不同模型规模和架构，实验规模较为充实。
- **消融实验**：包含 3 组 ——
  1. 组件消融（固定二值 → +混合量化 → +自适应显著性搜索 → +多非显著子集），准确率从 6.33 逐步提升至 58.35，验证了各模块的贡献。
  2. 模块敏感性分析（视觉编码器、适配器、语言模型分别量化），发现语言模型最敏感、适配器最不敏感。
  3. 效率对比（量化速度、平均位宽、精度）。
- **充分性评估**：
  - 优点：基准多样、模型规模覆盖 7B–11B、包含语言模型与整体 VLM 两种量化粒度、消融设计合理，实验较为客观公平。
  - 不足：所有实验均在约 1.0–1.1 bits 单一超低位宽下进行，未探索 4-bit/8-bit 或不同位宽组合；只对比了 AWQ 和 BiLLM 两个基线，缺少与其他近期超低比特 PTQ 方法的横向对比；每个数据集仅报告单一指标，统计分析（多次运行方差等）未提供。

## 6. 主要结论与发现

- Bi-VLM 在语言模型量化场景下，相比 SOTA 方法在四个基准上提升 **3%–47%**；在整个 VLM 量化场景下提升 **4%–45%**，且在多数基准上接近甚至达到全精度模型的性能。
- 语言模型部分对量化最敏感，视觉编码器敏感性中等，适配器/投影层几乎不敏感，说明在 VLM 中应优先保护语言模型的权重精度。
- 基于高斯分位数的非均匀权重分割有效适配了 VLM 权重的分布特性；显著性感知的混合量化（2-bit + 1-bit）是实现超低位宽下保持性能的关键。
- 通过优化显著权重比例，可在性能与压缩之间自适应取舍，且优化过程高效，整体量化速度远快于 BiLLM。

## 7. 优点

- **问题定位精准**：首次针对 VLM 提出 ≤2 bits 的后训练二值量化方案，填补了该精度区间的空白。
- **方法设计有据可依**：从权重分布的高斯统计特性出发设计分割策略，有充分的理论与实证支撑。
- **可扩展性好**：量化目标函数基于重构误差，可推广到不同模型系列（Llama、Llava、Qwen）和不同量化位宽组合。
- **性能与效率兼顾**：在显著提升精度的同时，量化速度比 BiLLM 快一个数量级以上。
- **实验设计较为完整**：多个基准、多个模型、两种量化粒度（语言模型 vs. 整体 VLM）以及多组消融，验证了各模块的独立贡献。

## 8. 不足与局限

- **位宽覆盖有限**：仅探索了约 1.0–1.1 bits 的极低比特场景，未验证在 4-bit/8-bit 等较大位宽下的表现，也未分析不同位宽组合（如 3-bit + 1-bit）的潜力。
- **基线对比不够充分**：仅对比 AWQ（4-bit）和 BiLLM（1-bit）两种方法，缺少与其他新近超低比特 PTQ 方法（如 PB-LLM、SpQR、Q-VLM 等）的对比，部分优势可能来自基线选择。
- **个别子任务表现欠佳**：在 Llama 3.2-Vision 的 MME Cognition 任务上性能低于 BiLLM，说明某些认知型任务对极低比特量化的容忍度更低，方法仍存在短板。
- **实际硬件验证缺失**：论文未在真实边缘设备（如手机、FPGA）上验证加速比和能耗优势，也未报告量化模型端到端推理的延迟数据，实际部署收益仍待验证。
- **未涉及激活量化**：Bi-VLM 仅量化权重，激活仍保持全精度，限制了在推理时对计算量（而非仅存储量）的进一步压缩。
- **超参数依赖经验**：指数自适应量化的参数 α=1.4 为经验设定，未讨论其敏感性或自适应调整策略。

（完）
