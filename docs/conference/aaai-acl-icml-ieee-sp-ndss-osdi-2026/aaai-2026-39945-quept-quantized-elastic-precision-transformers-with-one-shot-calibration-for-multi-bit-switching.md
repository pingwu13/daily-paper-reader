---
title: "QuEPT: Quantized Elastic Precision Transformers with One-Shot Calibration for Multi-Bit Switching"
title_zh: QuEPT：单次校准的多比特切换量化弹性精度 Transformer
authors: "Ke Xu, Yixin Wang, Zhongcheng Li, Hao Cui, Jinshui Hu, Xingyi Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39945/43906"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向 LLM 的逐块多比特后训练量化，一次校准支持低比特及混合精度切换
tldr: 弹性精度量化能通过一次优化支持多种比特宽度部署，但在 Transformer 尤其是大语言模型上受限于较高的存储和优化成本。QuEPT 提出单次校准的逐块多比特重构方案，在小规模数据上完成校准，通过级联不同低秩适配器动态适配多种预设位宽。该方法支持在均匀量化和混合精度量化之间实时切换，无需重复优化。实验表明其在提高精度和鲁棒性的同时维持了部署弹性，为多场景 LLM 量化部署提供了高效方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39945/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1756, \"height\": 1028, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39945/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 869, \"height\": 538, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39945/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 878, \"height\": 391, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39945/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 708, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39945/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1762, \"height\": 1103, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39945/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 875, \"height\": 1398, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39945/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 878, \"height\": 466, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39945/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 302, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39945/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 882, \"height\": 307, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39945/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 875, \"height\": 278, \"label\": \"Table\"}]"
motivation: 弹性精度量化因 Transformer 结构的高存储与优化成本难以应用于大语言模型。
method: 提出 QuEPT，通过单次校准和级联低秩适配器重构逐块多比特误差，支持实时切换位宽。
result: 实验显示 QuEPT 能以较低开销实现多种位宽切换，并提升量化精度与鲁棒性。
conclusion: QuEPT 为 LLM 的多比特弹性部署提供了一种低成本的后训练量化方案。
---

## Abstract
Elastic precision quantization enables multi-bit deployment via a single optimization pass, fitting diverse quantization scenarios. Yet, the high storage and optimization costs associated with the Transformer architecture, research on elastic quantization remains limited, particularly for large language models. This paper proposes QuEPT, an efficient post-training scheme that reconstructs block-wise multi-bit errors with one-shot calibration on a small data slice. It can dynamically adapt to various predefined bit-widths by cascading different low-rank adapters, and supports real-time switching between uniform quantization and mixed precision quantization without repeated optimization. To enhance accuracy and robustness, we introduce Multi-Bit Token Merging (MB-ToMe) to dynamically fuse token features across different bit-widths, improving robustness during bit-width switching. Additionally, we propose Multi-Bit Cascaded Low-Rank adapters (MB-CLoRA) to strengthen correlations between bit-width groups, further improve the overall performance of QuEPT. Extensive experiments demonstrate that QuEPT achieves comparable or better performance to existing state-of-the-art post-training quantization methods.

---

## 论文详细总结（自动生成）

# QuEPT：单次校准的多比特切换量化弹性精度 Transformer——论文详细中文总结

## 1. 论文核心问题与整体含义

**研究动机与背景：**

- 模型量化是降低 Transformer 模型计算与内存开销的关键技术，但**绝大多数量化方法仅支持单一预设位宽**（如 W4A4 或 W8A8），若要改变位宽必须重新进行优化，部署灵活性差。
- **弹性精度量化（Elastic Precision Quantization）** 可通过一次优化过程同时支持多种位宽部署，在统一共享原始权重的前提下，实现实时切换均匀量化和混合精度配置，满足多样化的部署场景。
- 然而，弹性量化在 CNN 上取得显著成功后，**在 Transformer 架构上进展缓慢**，原因包括：Transformer 存在密集的 token 间依赖、注意力驱动的动态稀疏性、激活值范围波动大，以及各层位宽敏感度高度异质，导致量化噪声被放大。
- 现有面向 LLM 的多比特量化方法（如 Any-Precision LLM、MatQuant）仍存在明显不足：Any-Precision LLM 仅量化权重而忽略激活量化；两者在低精度下的性能仍不及 SOTA 单比特后训练量化方法；且**未能解决激进低比特量化对中/高比特表示的负面影响**（竞争冲突问题）。

**核心目标：** 提出一种高效的后训练弹性量化框架 QuEPT，以**单次校准（one-shot calibration）** 支持 Transformer（ViT、LLM、MLLM）在多种位宽下的实时部署，同时保证与单一比特 SOTA 方法相当甚至更优的性能。

## 2. 方法论：核心思想、关键技术细节与算法流程

**整体框架：**

QuEPT 是一种**逐块（block-wise）重构的后训练量化（PTQ）方法**。在每层块的量化误差重构中，联合优化 LoRA 低秩补偿矩阵 R、权重剪裁参数 α/β；权重 W 与量化尺度 S 保持冻结。整个过程只需一小部分校准数据（128 或 1024 个样本），即可完成对所有目标位宽的一次联合优化。

**（1）弹性量化建模（公式 1–5）：**

- 目标函数为最小化所有目标位宽 b∈B 下量化模型与全精度模型逐块输出的期望误差（公式 1）。
- 权重量化公式（公式 2）将 LoRA 参数 BA 直接嵌入量化操作中，量化尺度 `s_w` 通过剪裁参数 α、β 动态计算（公式 4），零点和激活量化尺度也相应定义（公式 3、5）。
- 同时训练剪裁参数与 LoRA，使剪裁机制先处理权重离群值的大误差，为低秩适配器提供更平滑、更易修正的误差信号。

**（2）Multi-Bit Token Merging（MB-ToMe，多比特 Token 融合）：**

- **动机：** 不同位宽之间存在竞争冲突——低比特量化噪声会污染中/高比特特征表示。需要一个不偏向任何特定精度的 token 融合策略，在逐块重构时传递稳健的特征。
- **三种候选策略对比：**
  - Case 1（随机选择）：纯度随机，无法产生良好集成的混合特征；
  - Case 2（均匀融合）：低/中/高位宽输出按 1:1:1 权重融合，保持连续但丢失高比特细节；
  - Case 3（选择性融合，最终采用）：基于量化鲁棒性筛选 token——预计算高比特（8-bit）与低比特（4-bit）特征间的 **cosine 相似度**，相似度最高的前 p% token 被视为稳定 token，**保留其高精度特征**（锚点）；其余 token 则通过加权平均融合三种位宽的特征。公式即 X′ₖ = X^Hₖ（若 k∈Φ），否则为 λ₁X^H + λ₂X^M + λ₃X^L。
- 该策略保留了高比特关键信息，又平滑了不同精度间的特征差异。文中通过 K-S 统计量表明，融合分歧较大的 token 后，特征数值分布更均匀、对位宽变化更鲁棒。

**（3）Multi-Bit Cascaded LoRA（MB-CLoRA，多比特级联低秩适配器）：**

- **动机：** 独立为每个位宽维护 LoRA 参数浪费存储且削弱位宽间相关性；完全共享一个 LoRA 又无法捕捉不同位宽的错误差异。
- **级联结构：** 构造统一的参数空间 A∈R^{r×q}、B∈R^{p×r}。目标位宽集合划分为低比特组（BL）、中比特组（BM）、高比特组（BH）。某个位宽 b 的有效 LoRA 补偿矩阵为 R(b)=B[:, :r_b]·A[:r_b, :]（公式 7），其中有效秩 r_b 按位宽组单调递增（公式 8）：
  - 高比特组：r_b = r_h（小秩）
  - 中比特组：r_b = r_h + r_m（包含高比特参数）
  - 低比特组：r_b = r_h + r_m + r_l（包含全部参数）
- 这种级联方式形成自然的**继承机制**：低比特组自动复用高/中比特组学到的补偿方向，再额外添加专属补偿，缓解低比特误差大而适配能力不足的问题。
- 训练时从低/中/高三组各采样一个位宽，依次以 MAE 损失（公式 9）更新共享的 A、B 与剪裁参数，实现多精度协同优化。

**（4）混合精度扩展：**

- QuEPT 天然支持不额外训练的混合精度方案：通过 KL 散度评估每层敏感性，用 DP 算法确定最优逐层位宽分配，只需为每层选择匹配的 LoRA 切片即可。

**推理流程：** 部署时根据目标位宽选择对应的 LoRA 切片（与全精度权重相加后一起量化），LoRA 在推理时无额外开销。

## 3. 实验设计：数据集、基准与对比方法

**任务与模型覆盖：**

| 模型家族 | 具体模型 | 数据集 / 基准 |
|---------|---------|--------------|
| 视觉 Transformer | ViT-S, ViT-B, DeiT-S, Swin-S | ImageNet 分类 Top-1 准确率 |
| 大语言模型 | LLaMA2-7B, LLaMA2-13B, LLaMA3-8B | WikiText2、C4 困惑度；PIQA、ARC-C、ARC-E、HellaSwag、WinoGrande 五个零样本任务的平均准确率 |
| 多模态大模型 | LLaVA-OneVision-7B | MMMU、OCRBench、TextVQA、VizWiz、SEED |
| 混合精度实验 | LLaMA2-7B | WikiText2、C4 困惑度（平均位宽 2.25/3.00/4.00） |

**校准数据规模：**
- ViT：ImageNet 中随机选取 1024 张未标记图片；
- LLM：C4 的 128 个样本；
- MLLM：ShareGPT4V 改进版本 COCO Caption 中的 128 个图像-文本对。

**对比基线：**
- ViT：PTQ4ViT、PD-Quant、RepQ-ViT、ERQ（单比特 PTQ），PTMQ（多比特 PTQ）。
- LLM：AWQ、GPTQ、OmniQuant、AffineQuant、SmoothQuant、QLLM、QuaRot、DuQuant。
- MLLM：MBQ、AWQ、SmoothQuant。
- 混合精度：SKIM、SqueezeLLM、QuIP#、OmniQuant。

**量化设置：**
- ViT/LLM/MLLM 权激活量化：位宽集合 {4,5,6,7,8} 分组为低 {4}、中 {5,6}、高 {7,8}。
- MLLM 与 LLaMA 的权重量化探索了 2–8 bits（如 W2、W3A16、W4A8）。
- LLM 量化前先应用 Hadamard 矩阵旋转进行离群值平滑（借鉴 QuaRot）。

## 4. 资源与算力

- 论文**仅在表格注脚和正文中披露了 ViT 系模型的训练时间**：在**单张 Nvidia RTX 3090 GPU** 上，ViT-S 为 **17 分钟**、ViT-B 为 **36 分钟**、DeiT-S 为 **17 分钟**、Swin-S 为 **38 分钟**；相比之下，PTMQ 需要 430–950 分钟，QuEPT 的时间开销约为 PTMQ 的 **1/26**。
- 对于 LLM 和 MLLM 实验，**论文未明确说明 GPU 型号、数量或具体训练时长**，仅指出 QuEPT 作为 PTQ 方法整体训练开销远低于 QAT 类方法。另外，ViT 的对比基线也标注了单卡训练时间（如 ERQ 为 9–20×N 分钟，N 为卡数），说明 QuEPT 在效率上具有优势。

## 5. 实验数量与充分性分析

**实验组数量：**
- **主实验 3 大类**：ViT 表（4 种模型 × 5 种位宽）、LLM 表（3 种模型 × 4–5 种位宽）、MLLM 表（1 种模型 × 4 种配置）；
- **混合精度实验 1 组**（LLaMA2-7B，与 4 种方法对比）；
- **消融实验 3 组**：MB-CLoRA 三种共享策略对比（表 4）、MB-ToMe 三种融合策略对比（表 5）、模块组合消融（表 6，Clip/MB-CLoRA/MB-ToMe/MAE 损失的有无组合）。

**充分性评估：**

- **优点：** 实验覆盖了视觉、纯文本、多模态三大 Transformer 领域，模型规模从 ViT-S（约 22M）到 LLaMA3-8B，具备较好的广度；每个基准都对比了当前 SOTA 的单一比特 PTQ 方法与多比特方法，基线选择全面且公平（同为 uniform quantizer、同为优化方法等条件都加以标注）。
- **不足：**
  - LLM 的消融实验仅在 LLaMA2-7B 上开展，ViT 消融仅在 ViT-S 上开展，未覆盖更大规模模型；
  - 混合精度实验仅使用 LLaMA2-7B 一个模型，泛化验证有限；
  - 表 2 中 QuEPT 在 LLaMA2-13B W4A4 上的 PPL（5.53/7.16）略低于 DuQuant（5.42/7.05），表明在该场景下并非全面最优，论文也坦承了这一点；
  - 未报告多次重复实验的方差，也未说明校准数据随机抽样对结果稳定性的影响。

## 6. 主要结论与发现

1. **QuEPT 在 Vi

3. **QuEPT 在混合精度场景下无需额外训练即可获得具有竞争力的结果。** 在 LLaMA2-7B 上，当平均位宽分别为 2.25、3.00 和 4.00 时，QuEPT 的 WikiText2 困惑度均低于所有对比方法（SKIM、SqueezeLLM、QuIP#、OmniQuant），表明其逐层敏感性分析与 DP 位宽分配策略能够有效挖掘混合精度配置的潜力，同时借助 MB-CLoRA 的位宽切片共享机制实现了零成本切换。

4. **模块设计是性能提升的关键。** 消融实验表明：（a）MB-CLoRA 的级联结构优于独立 LoRA 与完全共享 LoRA，证明了低秩补偿参数的继承机制能有效缓解位宽间的竞争冲突；（b）MB-ToMe 的选择性 token 融合优于随机选择与均匀融合，说明基于量化鲁棒性保留高比特锚点token是稳定多比特特征分布的有效手段；（c）剪裁参数与 LoRA 联合优化、MAE 损失函数的引入均带来一致性能提升，验证了整体方案设计的合理性。

5. **QuEPT 在实际部署中具有显著工程价值。** 单次校准后，模型可随时切换至目标位宽或按层混合精度分配，无需重新优化；推理时 LoRA 切片与权重融合后不再产生额外计算，存储开销也仅增加少量低秩参数，这使其尤其适合资源受限的边缘端与动态服务场景。

---

## 7. 论文贡献总结

- **首次系统性解决了 Transformer 弹性精度量化中的位宽竞争冲突问题**，提出 MB-ToMe 与 MB-CLoRA 两个核心组件，分别从特征分布稳定性和低秩补偿参数继承两个角度消除多精度协同训练的负面影响。
- **提出了一种通用的后训练弹性量化框架**，可无缝应用于视觉 Transformer、大语言模型和多模态大模型，且与现有均匀量化、混合精度策略正交，兼容性好。
- **在保持与单一比特 SOTA 方法可比甚至更优性能的前提下，将多比特量化校准成本降低了一个数量级以上**（ViT 上约为 PTMQ 的 1/26），为实际部署提供了高性价比的解决方案。
- **实验覆盖全面且对比严谨**，在大规模模型（LLaMA2-13B、LLaMA3-8B）和多模态任务上均验证了方法的有效性，并公开了训练时间等关键可复现细节。

---

## 8. 局限性与未来研究方向

- **低比特极端场景仍有提升空间。** 如表 2 所示，QuEPT 在 LLaMA2-13B W4A4 上的困惑度略逊于 DuQuant，说明当前框架在 4-bit 权重+激活量化上尚未完全达到最优，需要进一步改进低比特下的误差补偿机制（如引入更高秩或动态特征依赖）。
- **校准数据敏感性与结果稳定性未充分研究。** 论文未报告多次随机采样校准数据的方差，也未探讨校准集大小对最终性能的影响，这在实际应用中可能影响可复现性。
- **混合精度实验的规模有限。** 仅验证了 LLaMA2-7B 一种模型，未来应在更大模型（如 70B）和更丰富的任务上检验混合精度分配算法的泛化能力。
- **对长序列与多 token 交互场景的适应性有待验证。** MB-ToMe 依赖 token 间相似度预计算，其计算开销与记忆复杂度随序列长度增加而上升，在长文本或高分辨率图像输入下的可行性需要进一步优化。
- **未与更多最新弹性精度方法（如 Any-Precision LLM 的更全面变体、MCore 等）进行对比**，随着该领域快速发展，未来可以纳入更多基线以验证性能优势的普适性。

总体而言，QuEPT 以简洁而有效的设计解决了 Transformer 弹性量化中的核心难题，在性能、效率和灵活性之间取得了良好平衡，为多比特 AI 模型的低成本部署提供了一条可行路径。

（完）
