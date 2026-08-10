---
title: "Proteus: Lookup-Free Trellis-Coded Quantization by Lattice-Breaking Compute Codes for 2-Bit LLMs"
title_zh: Proteus：面向2比特大语言模型的无查找表格编码量化与格破环计算码
authors: "Zhengwu Yang, Xunchao Li, Ke Cheng, Kunlong Liu, Jianfeng Yang, Haoshuang Wang, Kaipeng Deng, Qingqing Dang, Yanlin Sha, Yanjun Ma, Dianhai Yu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/6bb5e4776b78544ffe34e2295084552ea6164485.pdf"
tags: ["query:ulbv"]
score: 10.0
evidence: 面向2比特大模型的无查找表格编码量化，直接对应2比特权重量化需求
tldr: 大型语言模型自回归解码常受显存带宽限制，超低比特权重量化需避免解码内核对码本或查找表的不规则访问。Proteus提出严格无查找表的格编码量化框架，通过格破环计算码和逐层仿射高斯化生成接近高斯的码值，实现零运行时表加载。实验表明其在2比特LLM上保持精度并显著提升解码效率，为极低比特量化推理提供高效GPU友好方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有2比特TCQ要么引入微查找表，要么因重叠放大伪影而损害精度，难以兼顾GPU友好性与量化质量。
method: 提出无查找表的格编码量化框架，采用格破环计算码与逐层仿射高斯化，实现零运行时表加载的码值计算。
result: 在2比特LLM上实现无表加载的量化推理，避免重叠伪影，有望提升内存受限场景的解码吞吐。
conclusion: Proteus表明可通过计算码替代查找表实现2比特LLM高效无损量化，推动极低比特推理落地。
---

## Abstract
Autoregressive decoding of large language models is frequently memory-traffic bound, so ultra-low-bit weight-only PTQ helps only if dequantization avoids irregular codebook or LUT access in the inner loop. Under the GPU-friendly bitshift trellis, existing 2-bit trellis-coded quantization (TCQ) pipelines either reintroduce micro-LUTs or suffer overlap-amplified artifacts because incoherence improves global Gaussianity but does not guarantee overlap-local joint geometry. We introduce Proteus a strictly lookup-free TCQ framework whose computed generator MUL-BAL uses cheap integer mixing plus a per-layer affine Gaussianizer to produce overlap-robust, near-Gaussian code values with zero runtime table loads. Proteus instantiates each layer by selecting from a tiny, pre-vetted candidate pool and then applies lightweight channel compensation and optional few-shot distillation that tune only per-layer affine statistics while keeping packed indices and the bitshift-trellis decoder fixed. On Llama 2 (7B–70B) at 2-bit PTQ, Proteus improves perplexity and zero-shot accuracy over strong TCQ/PTQ baselines and reduces end-to-end decode bandwidth at comparable throughput (e.g., 740 vs. 1020 GB/s on 70B).

---

## 论文详细总结（自动生成）

根据提供的论文元数据，以下是对论文《Proteus: Lookup-Free Trellis-Coded Quantization by Lattice-Breaking Compute Codes for 2-Bit LLMs》的详细中文总结：

---

# Proteus：面向2比特大语言模型的无查找表格编码量化与格破环计算码

## 1. 核心问题与整体含义

- **背景与动机**：大型语言模型（LLM）的自回归解码过程通常受限于显存带宽（memory-traffic bound）。超低比特（ultra-low-bit）权重量化（weight-only PTQ）虽能缓解带宽压力，但必须确保反量化（dequantization）过程不涉及对码本（codebook）或查找表（lookup table, LUT）的不规则访问，否则解码内核对LUT的访问会严重拖慢生成速度。
- **现有方法的缺陷**：在GPU友好的位移格子（bitshift trellis）结构下，已有的2比特格子编码量化（Trellis-Coded Quantization, TCQ）方案存在两大问题：
  1. 要么重新引入微查找表（micro-LUT），破坏了无表访问的初衷；
  2. 要么因“重叠放大伪影”（overlap-amplified artifacts）而损害模型精度，原因是虽然非相干性（incoherence）能提升全局高斯性，却无法保证重叠局部区域的联合几何特性。
- **研究价值**：解决2比特LLM量化中“GPU友好性”与“量化质量”难以兼得的矛盾，对推动极低比特量化推理的落地具有重要意义。

## 2. 方法论：核心思想与关键技术

- **核心思想**：提出一种严格无查找表（strictly lookup-free）的TCQ框架，用“计算”替代“查表”，在GPU友好的位移格子约束下，以纯整数运算与轻量仿射变换实时生成码值，实现零运行时表加载。
- **技术细节**：
  - **MUL-BAL生成器**：Proteus采用经过验证的生成器，名为MUL-BAL，使用廉价的整数混合（cheap integer mixing）操作，在格子结构中动态生成量化码值。
  - **格破环计算码（Lattice-Breaking Compute Codes）**：通过“格破环”机制破坏原有格子的规则重叠结构，生成对重叠鲁棒（overlap-robust）的码值，避免重叠放大伪影，同时逼近高斯分布。
  - **逐层仿射高斯化（Per-Layer Affine Gaussianizer）**：针对每个网络层应用一个仿射变换，对MUL-BAL生成的码值进行逐层高斯化处理，使其更接近高斯分布，从而提升量化鲁棒性。
  - **实例化流程**：每个层从预先筛选的微小候选池（tiny, pre-vetted candidate pool）中选择一个生成器实例化，随后应用轻量级通道补偿（channel compensation）和可选的少样本蒸馏（few-shot distillation）。
  - **保持固定部分**：所有调优仅涉及每层的仿射统计量（per-layer affine statistics），而打包索引（packed indices）与位移格子解码器（bitshift-trellis decoder）在推理过程中始终保持固定不变。
- **设计优势**：整个解码内循环无任何查表操作，码值由整数混合和仿射变换实时计算得出，既保证了GPU友好的计算模式，又维持了高精度。

## 3. 实验设计

- **数据集与场景**：聚焦于2比特权重PTQ场景下的语言建模与零样本任务。
- **Benchmark**：
  - 语言建模性能采用**困惑度（Perplexity, PPL）** 评估；
  - 通用能力采用**零样本准确率（zero-shot accuracy）** 评估；
  - 解码效率采用**端到端解码带宽（end-to-end decode bandwidth, GB/s）** 与**吞吐量（throughput）** 衡量。
- **测试模型规模**：Llama 2系列，覆盖**7B至70B**的参数规模范围。
- **对比方法**：与强TCQ基线和PTQ基线进行对比（具体方法名称在摘要中未逐一列出，但明确提及“strong TCQ/PTQ baselines”）。

## 4. 资源与算力

- 论文摘要与元数据中**未明确说明**使用的GPU型号、数量、训练/蒸馏时长或总计算资源消耗。仅能从使用了70B模型及少样本蒸馏等设置推断，实验涉及中等规模以上的计算资源需求。

## 5. 实验数量与充分性

- **实验规模**：元数据中仅描述了主要结果——在Llama 2（7B~70B）上测评了PPL与零样本准确率，并报告了一个70B模型上的带宽对比数据（740 vs. 1020 GB/s）。这些信息表明至少覆盖了多尺寸模型和多样化的评估指标，但**整体实验组数并未详细展开**，缺乏消融实验、不同量化方法在各模型尺度的逐项对比细节、候选池大小选择等敏感度分析的公开信息。
- **充分性与客观性**：
  - 实验覆盖面横跨7B至70B，具备一定规模代表性；
  - 同时报告了精度（PPL、零样本准确率）和效率（带宽、吞吐量）两类指标，比较全面；
  - 但未提供多数据集、多任务族的细粒度结果，也未报告方差或多次运行的稳定性。因此整体实验较为客观，但公开细节有限，充分性有待正式论文全文确认。

## 6. 主要结论与发现

- Proteus在2比特PTQ条件下，相比强TCQ/PTQ基线，在Llama 2系列上显著改善了困惑度与零样本准确率；
- 实现了**零运行时表加载**的2比特量化推理，有效避免了重叠伪影；
- 在70B模型上，端到端解码带宽需求从约1020 GB/s降至约740 GB/s，同时保持可比吞吐量，证明了其在显存受限场景下对解码吞吐的积极效果；
- 核心结论：**通过计算码替代查找表，可实现2比特LLM的高效、无损（精度保持）量化**，为极低比特推理的实际部署提供了GPU友好的方案。

## 7. 优点

- **方法创新性突出**：提出“格破环计算码”这一概念，在TCQ框架内首次实现严格的零查找表计算，兼顾编码质量与硬件友好性；
- **设计简洁高效**：仅需整数混合与仿射变换，无额外分支或内存访问，解码内核简洁，适合GPU并行结构；
- **调优代价低**：只调整每层的仿射统计量，不改变打包索引和解码器结构，适合PTQ场景，可快速适配新模型；
- **实验性能全面**：同时报告精度与效率两类指标，能有力证明方法在真实推理场景中的实用性；
- **应用意义重大**：为2比特常见TCQ方案“微LUT vs. 重叠伪影”的两难困境提供了有效解决方案。

## 8. 不足与局限

- **不透明性**：论文摘要及元数据未提供候选池的具体构造方式、MUL-BAL生成器的理论分析或与现有TCQ方法的详细数学对比，方法论的完整解读依赖全文；
- **实验细节缺失**：未提供多数据集/多任务族的消融研究、敏感度分析和统计显著性检验，无法判断方法在不同文本领域上的稳健性；
- **泛化性未知**：仅在Llama 2系列上验证，未说明在Mistral、Qwen等架构以及不同激活分布特性上的适用性；
- **带宽数据泛化性有限**：740 vs. 1020 GB/s的对比反映的是特定GPU与批处理配置下的带宽需求模型，未给出吞吐量、延迟的具体数值，结论的实际普适性需谨慎解读；
- **蒸馏额外开销**：可选的少样本蒸馏需额外计算成本，虽然仅调整仿射参数，但其对蒸馏集的质量与数量的依赖未被明确讨论；
- **元数据缺失**：算力资源、实现细节、代码开源与否等信息尚未公布，降低可复现性。

---

（完）
