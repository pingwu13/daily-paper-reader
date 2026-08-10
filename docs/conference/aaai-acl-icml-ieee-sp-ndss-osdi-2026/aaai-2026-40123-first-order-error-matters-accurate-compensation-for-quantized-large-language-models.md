---
title: "First-Order Error Matters: Accurate Compensation for Quantized Large Language Models"
title_zh: 一阶误差至关重要：面向量化大语言模型的精确补偿
authors: "Xingyu Zheng, Haotong Qin, Yuye Li, Haoran Chu, Jiakai Wang, Jinyang Guo, Michele Magno, Xianglong Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40123/44084"
tags: ["query:ulbv"]
score: 8.0
evidence: 利用一阶梯度项对量化大模型做后训练补偿
tldr: 后训练量化中常用的二阶泰勒补偿忽略了渐进补偿带来的累积一阶偏差，导致误差建模失准。作者提出FOEM，在量化补偿中显式纳入一阶梯度项，以更准确地刻画潜权重与全精度模型之间的偏差。实验表明该方法能降低量化误差、提升量化LLM的性能，为低比特大模型PTQ补偿提供了更可靠的校准范式。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40123/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 870, \"height\": 811, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1674, \"height\": 785, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1701, \"height\": 1145, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1692, \"height\": 666, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1696, \"height\": 550, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 776, \"height\": 228, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 804, \"height\": 503, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 811, \"height\": 228, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 818, \"height\": 188, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40123/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 842, \"height\": 147, \"label\": \"Table\"}]"
motivation: 现有基于二阶近似的量化误差补偿假设一阶项可忽略，但渐进补偿过程会产生累积的一阶偏差，导致量化误差估计失准。
method: 提出FOEM，在PTQ补偿中显式引入一阶梯度项，更精确地对量化LLM权重进行校准。
result: 在LLM量化补偿任务上相比二阶方法降低量化误差，提升量化模型精度。
conclusion: 揭示一阶误差在补偿式量化中的重要性，为PTQ校准提供更准确模型。
---

## Abstract
Post-training quantization (PTQ) offers an efficient approach to compressing large language models (LLMs), significantly reducing memory access and computational costs. Existing compensation-based weight calibration methods often rely on a second-order Taylor expansion to model quantization error, under the assumption that the first-order term is negligible in well-trained full-precision models. However, we reveal that the progressive compensation process introduces accumulated first-order deviations between latent weights and their full-precision counterparts, making this assumption fundamentally flawed. To address this, we propose FOEM, a novel PTQ method that explicitly incorporates first-order gradient terms to improve quantization error compensation. 
FOEM approximates gradients by performing a first-order Taylor expansion around the pre-quantization weights. This yields an approximation based on the difference between latent and full-precision weights as well as the Hessian matrix. When substituted into the theoretical solution, the formulation eliminates the need to explicitly compute the Hessian, thereby avoiding the high computational cost and limited generalization of backpropagation-based gradient methods. This design introduces only minimal additional computational overhead.
Extensive experiments across a wide range of models and benchmarks demonstrate that FOEM consistently outperforms the classical GPTQ method. In 3-bit weight-only quantization, FOEM reduces the perplexity of Llama3-8B by 17.3% and increases the 5-shot MMLU accuracy from 53.8% achieved by GPTAQ to 56.1%. Moreover, FOEM can be seamlessly combined with advanced techniques such as SpinQuant, delivering additional gains under the challenging W4A4KV4 setting and further narrowing the performance gap with full-precision baselines, surpassing existing state-of-the-art methods.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 一、核心问题与整体含义（研究动机和背景）

- **研究背景**：大语言模型（LLM）参数规模庞大，部署时需要大量内存和计算资源。量化作为一种模型压缩技术，将高比特浮点参数转换为低比特定点格式，在不改变模型结构的前提下降低内存占用并加速推理。后训练量化（PTQ）无需梯度微调、效率高，是LLM部署的主流方案。
- **核心问题**：以GPTQ为代表的补偿式权重校准方法，基于OBD/OBS的传统假设，在泰勒展开建模量化误差时**忽略了一阶梯度项**，理由是“全精度模型已充分训练至局部最优，梯度接近零”。然而作者揭示：补偿过程存在**渐进性**——先量化列会通过补偿更新改变后续未量化列（潜权重）的数值，导致潜权重与其全精度版本之间产生**累积偏差**，因此后续列在量化时梯度**不可忽略**。此时二阶近似模型失准，补偿效果下降。
- **整体含义**：论文认为“忽略一阶项”这一假设在补偿式量化场景下是**根本性缺陷**，提出应当在量化误差模型中显式纳入一阶项以提升补偿精度。这一发现对PTQ误差建模的准确性构成了重要修正，为低比特LLM量化提供了更可靠的校准范式。

### 二、方法论：FOEM

**核心思想**：在GPTQ的逐列补偿框架下，将一阶梯度项显式纳入基于拉格朗日乘子的优化目标中，并对梯度进行近似，避免反传计算和Hessian的显式求逆，从而以几乎为零的额外开销获得更精确的补偿。

**关键技术细节与公式推导**：

1. **损失建模**：保留一阶项的逐列量化损失为
   δE = gδw⊤ + ½δwHδw⊤，
   其中g为梯度、H为Hessian矩阵，对应公式(9)。

2. **约束优化**：量化第q列并调整剩余潜权重δw时，优化目标为
   min [ gδw⊤ + ½δwHδw⊤ ] ，约束为 e_qδw⊤ + w_q − ŵ_q = 0，对应公式(10)。

3. **拉格朗日求解**：利用拉格朗日乘子法，得最优解为
   δw = −[(w_q − ŵ_q − gH⁻¹e_q⊤)/[H⁻¹]_{qq}]·[H⁻¹]_{q,:} − gH⁻¹，
   对应公式(13)；结合Cholesky分解（H⁻¹ = LL⊤，T = L⊤）简化为公式(14)。

4. **梯度近似（关键创新）**：直接计算g需反传或求Hessian逆，代价过高。作者利用**一阶泰勒展开**，发现g(Ẇ) ≈ (Ẇ − W)H（其中Ẇ为补偿后的潜权重、W为原始全精度权重），并基于“原始模型已收敛”假设g(W)≈0。进一步引入稳定因子β，取g = β(Ẇ − W)H（公式18），β默认0.1。

5. **Hessian消去（计算高效的核心）**：将上述梯度的近似代入公式(14)后，**H与H⁻¹项相互抵消**，得到最终补偿公式(19)：
   δw = −[(w_q − ŵ_q) − β(w_q − W̃e_q⊤)]/T_{qq} · T_{q,:} − β(Ẇ − W)。
   其中W̃为原始全精度权重。最终形式只含GPTQ原有的Cholesky三角矩阵和简单的**权重差运算**，无需计算Hessian、无需反传。

6. **算法流程（Algorithm 1）**：逐块（block）、逐列（column）进行：先量化当前列，再计算量化残差与一阶修正项的组合更新量E，并通过已分解的三角矩阵T对当前块内未量化列及后续块进行补偿。原文的lazy update机制可完全兼容，无需修改。

### 三、实验设计

**数据集**：
- **困惑度（PPL）** ：WikiText2、C4。
- **零样本推理基准（6项）** ：PIQA、Winogrande、ARC-Easy、ARC-Challenge、HellaSwag、BoolQ。
- **MMLU（5-shot）**。
- **校准数据**：从C4随机采样128个序列，序列长度2048（量化领域标准配置）。

**评估场景**：
- 权重仅量化（weight-only）：W3、W4，group size 128。
- 权重-激活联合量化：W4A4KV4（基于SpinQuant预训练旋转矩阵）。
- 额外场景：W3A16（Mamba SSM模型）、W4A16（vLLM部署测试）。

**对比方法**：RTN（round-to-nearest基线）、GPTQ（经典二阶补偿方法）、GPTAQ（最新补偿方法，官方配置α=0.2）；W4A4KV4场景基于SpinQuant旋转矩阵。

**模型覆盖**：Llama2-7B/13B、Llama3-8B/70B、Llama3.2-1B/3B、Qwen3-8B、Mistral-7B、Phi-1.5B、Mamba-1.4B。

### 四、资源与算力

- **量化校准硬件**：单张NVIDIA A800-80GB GPU；Llama3-70B评测需2×A800。
- **运行时长**：结合表8数据，Llama3-8B的W4A4KV4量化，GPTQ耗时825.50秒，GPTAQ耗时1112.20秒，FOEM耗时828.90秒，**与GPTQ几乎持平，比GPTAQ显著更快**。
- **推理速度**：表9显示W4A16权重量化部署下，输入速度从184.11提升至250.26 tokens/s（+约36%），输出从470.11提升至616.01 tokens/s（+约31%）。
- 论文未报告具体训练/校准总时长或其他算力细节（如功耗、内存占用等）。

### 五、实验数量与充分性

**实验组数量**（约9组核心实验，覆盖多表多场景）：
1. 表1：Llama2-7B/13B的W4/W3权重仅量化（多指标对比）。
2. 表2：Llama3-8B、Llama3.2-1B/3B的W4/W3权重仅量化。
3. 表3：Qwen3-8B、Mistral-7B、Phi-1.5B的W3权重仅量化（泛化性）。
4. 表4：Llama2-7B/13B、Llama3-8B的W4A4KV4权重-激活联合量化。
5. 表5：Llama3-70B的W3权重量化（大规模验证）。
6. 表6：β敏感性分析（β从0.1到1.0，共10组取值）。
7. 表7：Mamba-1.4B的W3A16量化及β敏感性（跨架构验证）。
8. 表8：量化时间对比（效率验证）。
9. 表9：vLLM实际推理速度测试。

每个数据集均采用预定义超参数，并使用3个不同随机种子重复实验取平均。

**充分性评估**：实验覆盖了多种主流模型架构（Llama系列、Qwen、Mistral、Phi、Mamba）、多种量化位宽（3/4bit权重、W4A4KV4、W3A16）、语言建模与零样本推理及MMLU等多种评估维度，并包含敏感性分析和效率验证，**整体比较充分且公平**（与GPTQ、GPTAQ使用相同的校准协议与设置）。但缺少与更多近期SOTA方法（如QuaRot、OmniQuant等）在激活量化场景的直接对比。

### 六、主要结论与发现

1. **一阶误差不可忽略**：补偿过程的渐进性使剩余未量化权重的梯度显著不为零，忽略一阶项会导致误差建模失准；引入一阶项可显著改善补偿效果。
2. **FOEM全面优于GPTQ**：在所有位宽、所有评测指标上均优于经典GPTQ方法。
3. **3-bit权重量化收益显著**：Llama3-8B上PPL降低17.3%（对比GPTQ），MMLU从GPTAQ的53.8%提升至56.1%。
4. **4-bit可与全精度持平**：如Llama2-13B在4bit下MMLU与FP16持平（55.5 vs 55.2，甚至略超）。
5. **跨架构泛化**：在Qwen3、Mistral、Phi及SSM结构Mamba上均一致改进。
6. **与SpinQuant兼容**：在W4A4KV4联合量化设置下进一步提升精度，缩小与全精度模型的差距。
7. **几乎零额外开销**：量化时间与GPTQ基本一致（828.9s vs 825.5s），远快于GPTAQ（1112.2s）。
8. **β取值具有鲁棒区间**：β<0.5时性能稳定提升，β>0.5时性能下降，β=0.1为默认稳健选择。

### 七、优点

- **理论洞察深刻**：明确指出补偿式量化中“一阶项可忽略”的潜在缺陷，并通过数学推导验证了一阶项在渐进补偿过程中的关键作用，对理解PTQ误差机理有重要意义。
- **数学推导优雅**：通过一阶泰勒展开近似梯度，代入拉格朗日解后Hessian项自然消去，最终只依赖GPTQ已有的Cholesky因子，**无需额外反传、无需显式计算/求逆Hessian**。
- **极低额外开销**：相比GPTQ仅增加权重差运算，量化时间几乎不变，显著优于GPTAQ的额外计算成本。
- **兼容性强**：可与GPTQ的lazy update机制、SpinQuant旋转矩阵等先进技术无缝结合，易集成到现有PTQ流程。
- **实验扎实**：覆盖多系列模型、多量化配置、多评估维度，并提供敏感性分析与效率验证，结果具有较高可信度。
- **部署收益明确**：在vLLM上的推理加速数据展示了实际部署价值。

### 八、不足与局限

- **β依赖经验设置**：稳定因子β（默认0.1）虽在多数模型上表现稳健，但缺乏理论上的最优确定方法，依赖网格搜索式的经验选择；且敏感性与模型规模/位宽的关系未深入探讨。
- **实验规模有限**：70B模型仅在3-bit权重量化下评测了PPL和0-shot平均准确率，未覆盖更多任务和更低位宽；缺少与更多近期SOTA方法（如QuaRot、OmniQuant等）的直接对比。
- **个别指标波动**：部分结果存在不一致现象（如表2中Llama3.2-1B在WinoG上FOEM为57.9，低于GPTAQ/GPTQ的61.4/60.6），说明FOEM并非在所有细粒度指标上一致领先，论文未对此作探讨。
- **W4A4KV4实验依赖SpinQuant预训练旋转矩阵**，未探究FOEM与自建旋转/其他激活量化方案的配合，泛化性证据相对有限。
- **应用边界**：方法主要针对线性层权重补偿，未涉及嵌入层、输出层等特殊结构的低比特量化，也未讨论对QAT等训练式量化的适用性。
- **研究范围**：未涉及多模态模型（如视觉-语言模型）的量化场景。

（完）
