---
title: "LATMiX: Learnable Affine Transformations for Microscaling Quantization of LLMs"
title_zh: LATMiX：面向大语言模型微缩放量化的可学习仿射变换
authors: "Ofir Gordon, Lior Dikstein, Arnon Netzer, Idan Achituve, Hai Victor Habi"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d2dedffdfb1b38f9c8b46a10f1253169e4024603.pdf"
tags: ["query:ulbv"]
score: 8.0
evidence: 用可学习仿射变换替代旋转或哈达玛变换提升 LLM 量化鲁棒性，面向微缩放 MX 硬件
tldr: 后训练量化能有效降低大语言模型的存储与计算开销，但现有方法多局限于旋转或哈达玛变换，且难以适配现代硬件支持的微缩放(MX)格式。LATMiX 提出可学习仿射变换，从更一般的角度提升量化鲁棒性，降低激活离群值的影响。该工作为在 MX 硬件上部署低比特大模型提供了新路径，理论上也扩展了变换式 PTQ 的设计空间。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有 LLM 量化鲁棒性方法局限于旋转/Hadamard 变换，且传统格式难以适配现代 MX 硬件的需要。
method: 提出可学习仿射变换的 PTQ 方法，替代旋转/Hadamard 变换以减少激活离群值，适配 MX 数据格式。
result: 实验表明该方法能有效提升 LLM 量化后模型的性能，且兼容现代微缩放硬件。
conclusion: LATMiX 扩展了变换式量化方案，为 MX 硬件上的高精度低比特部署提供了可行途径。
---

## Abstract
Post-training quantization (PTQ) is a widely used approach for reducing the memory and compute costs of large language models (LLMs). Recent studies have shown that applying invertible transformations to activations can significantly improve quantization robustness by reducing activation outliers; however, existing approaches are largely restricted to rotation or Hadamard-based transformations.
Moreover, most studies focused primarily on traditional quantization schemes, whereas modern hardware increasingly supports the microscaling (MX) data format. Attempts to combine both showed severe performance degradation, leading prior work to introduce assumptions on the transformations.
In this work, we take a complementary perspective. First, we provide a theoretical analysis of transformations under MX quantization by deriving a bound on the quantization error. Our analysis emphasizes the importance of accounting for both the activation distribution and the underlying quantization structure.
Building on this analysis, we propose LATMiX, a method that generalizes outlier reduction to learnable invertible affine transformations optimized using standard deep learning tools. 
Experiments show consistent improvements in average accuracy for MX low-bit quantization over strong baselines on a wide range of zero-shot benchmarks, across multiple model sizes.

---

## 论文详细总结（自动生成）

# LATMiX：面向大语言模型微缩放量化的可学习仿射变换

## 1. 论文的核心问题与整体含义

- **研究动机**：后训练量化（PTQ）是降低大语言模型（LLM）存储与计算开销的常用方法。近年研究发现，对激活值施加可逆变换（如旋转或 Hadamard 变换）能显著减少激活离群值，从而提升量化鲁棒性。但现有方法大多局限于旋转或 Hadamard 类变换，且主要面向传统量化格式。
- **核心问题**：现代硬件日益支持微缩放（MX）数据格式，而将现有的旋转/Hadamard 类变换直接应用于 MX 量化时会出现严重的性能下降。此前工作被迫引入额外假设来缓解这一问题，缺乏对变换与 MX 量化结构之间关系的系统性理解。
- **整体含义**：论文试图从更一般化、可学习的角度解决变换式 PTQ 在 MX 硬件上的适配问题，从而为低比特大模型在下一代硬件上的高效部署提供新路径。

## 2. 论文提出的方法论

- **核心思想**：将离群值抑制从固定的旋转/Hadamard 变换推广为**可学习的可逆仿射变换**，并利用标准深度学习优化工具进行端到端优化。
- **理论支撑**：论文首先给出了 MX 量化下变换操作的量化误差上界推导。该分析表明，量化误差不仅取决于激活分布，还依赖于底层量化结构（即 MX 格式的特性），因此变换设计必须同时考虑两者。
- **方法名称**：LATMiX（Learnable Affine Transformations for Microscaling Quantization）。
- **实现路径**：通过优化可学习仿射变换的参数，使其更好匹配具体模型的激活分布与 MX 量化规则，从而降低量化误差，提升低比特量化后模型的性能。
- **与已有方法的区别**：不是手工设计固定的旋转或 Hadamard 矩阵，而是用可学习仿射变换统一并扩展了变换式 PTQ 的设计空间。

## 3. 实验设计

- **基准与数据集**：论文提及使用了“广泛的零样本基准”（a wide range of zero-shot benchmarks），但摘要中未列出具体数据集名称（如 WikiText、MMLU、HellaSwag、ARC 等）。
- **模型规模**：实验覆盖了“多种模型尺寸”（multiple model sizes），但具体型号未在摘要中给出。
- **对比方法**：与“强基线”（strong baselines）进行比较，这些基线应包含现有的基于旋转/Hadamard 变换的 PTQ 方法以及直接在 MX 格式下量化的方法，但摘要未指名具体算法名称。
- **评价指标**：采用 MX 低比特量化下的平均准确率作为主要评估指标。

## 4. 资源与算力

- **未明确说明**：论文摘要及提供的元数据中未提及使用的 GPU 型号、数量、训练时长、显存开销等算力信息。因此无法从现有文本中获知具体资源配置。
- 若需完整了解算力细节，需要查阅论文正文的实验设置部分。

## 5. 实验数量与充分性

- **已提及的实验范围**：包括跨多个零样本基准、多个模型规模、MX 低比特量化设置下的对比实验，并以平均准确率报告结果。
- **充分性评价**：从摘要看，实验覆盖了“多模型 + 多任务 + 多基线”的基本要求，具备一定的说服力。但由于未提供具体实验数量、消融实验和详细基线列表，无法完全判断其充分性。理想情况下还需包含：
  - 不同比特宽度（如 W4A4、W4A8）下的表现；
  - 不同 MX 配置（如 MX4、MX6）的适配性；
  - 对可学习仿射变换的消融研究；
  - 与更广泛 PTQ 方法的对比；
  - 训练成本与收敛性分析。
- **公平性风险**：若基线的旋转/Hadamard 变换均为固定且未进行类似的可学习优化，对比可能不完全对等；但论文声称相比“强基线”有持续改进，暗示实验设计较为严谨。

## 6. 论文的主要结论与发现

- **理论层面**：推导了 MX 量化下变换的量化误差上界，明确了激活分布与量化结构共同影响误差的机理，为设计变换提供了理论指导。
- **方法层面**：提出的 LATMiX 方法能够将离群值抑制推广为可学习仿射变换，并且可以兼容现代 MX 数据格式。
- **实证层面**：在 MX 低比特量化下，LATMiX 在多种模型规模和多个零样本基准上均稳定优于强基线，表明其有效性和泛化能力。
- **总体结论**：LATMiX 扩展了变换式 PTQ 的设计空间，为在 MX 硬件上实现高精度、低比特 LLM 部署提供了一条可行且通用的途径。

## 7. 优点

- **视角新颖**：从“可学习仿射变换”这一更一般视角出发，突破了旋转/Hadamard 变换的局限，丰富了量化鲁棒性方法的设计空间。
- **理论结合实践**：先推导 MX 量化误差上界，再基于理论设计方法，逻辑严密，具有一定的解释性。
- **硬件导向**：面向现代 MX 数据格式，解决了现有方法在微缩放硬件上性能严重退化的问题，具有较强的实际意义。
- **即插即用**：使用标准深度学习优化工具即可训练，无需为量化专门设计复杂优化器。
- **结果稳定**：在多个模型尺寸和多种零样本基准上均显示一致性提升，增强了结论的可信度。

## 8. 不足与局限

- **信息不完整**：从摘要和元数据无法获取具体数据集、模型名称、基线和比特配置，难以独立复现或进行深度评判。
- **实验细节缺失**：未提及消融实验、每项任务的详细结果、参数敏感性分析、训练开销、收敛速度以及不同仿射变换形式（如对角、带状等）的影响。
- **算力信息空白**：没有报告 GPU 类型、数量、时长等资源信息，这对评估方法实用性和可扩展性造成障碍。
- **适用范围不明**：虽然面向 LLM，但未说明是否适用于不同架构（如 LLaMA、Mistral、DeepSeek 等）或超大规模模型；也未讨论对长序列、多模态模型和低比特权重量化（仅权重、仅激活）的扩展性。
- **潜在偏差风险**：若主要实验集中于某一类模型系列或某一类 MX 配置，结论可能外推有限；论文声称“稳定优于强基线”，但没有给出误差棒或统计显著性检验信息。
- **理论假设**：量化误差上界的推导可能基于特定简化假设，其在极端低比特（如 2-bit）或异常分布下的适用性仍需验证。

（完）
