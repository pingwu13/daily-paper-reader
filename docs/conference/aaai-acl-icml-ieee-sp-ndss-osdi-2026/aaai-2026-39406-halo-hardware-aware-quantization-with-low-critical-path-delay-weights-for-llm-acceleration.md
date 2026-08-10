---
title: "HALO: Hardware-Aware Quantization with Low Critical-Path-Delay Weights for LLM Acceleration"
title_zh: HALO：面向LLM加速的硬件感知量化与低关键路径延迟权重
authors: "Rohan Juneja, Shivam Aggarwal, Safeen Huda, Tulika Mitra, Li-Shiuan Peh"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39406/43367"
tags: ["query:ulbv"]
score: 8.0
evidence: 面向LLM加速的硬件感知PTQ，考虑电路级部署特性
tldr: 传统量化方法仅约束位宽，未考虑乘累加单元的关键路径延迟和能量特性，限制了硬件加速潜力。HALO提出硬件感知的后训练量化框架，显式建模电路级时序与能耗特征，指导量化策略以利用时序裕量和节能机会。实验显示HALO在现代加速器上实现更高推理吞吐和能效，是面向硬件部署的低比特LLM量化方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1482, \"height\": 655, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 663, \"height\": 566, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 697, \"height\": 579, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 407, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 856, \"height\": 419, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 856, \"height\": 413, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1825, \"height\": 476, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 820, \"height\": 323, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 822, \"height\": 480, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 869, \"height\": 561, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 839, \"height\": 481, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 845, \"height\": 433, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 842, \"height\": 434, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 844, \"height\": 442, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39406/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 871, \"height\": 114, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39406/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1830, \"height\": 644, \"label\": \"Table\"}]"
motivation: 传统量化忽略电路级时序和能耗特性，无法充分利用加速器裕量。
method: 将MAC单元关键路径延迟与能量模型融入PTQ，进行硬件感知量化决策。
result: 在现代加速器上提升LLM推理效率与能效。
conclusion: HALO建立了从电路特性到量化配置的映射，可指导实际部署。
---

## Abstract
Quantization is critical for efficiently deploying large language models (LLMs). Yet conventional methods remain hardware-agnostic, limited to bit-width constraints, and do not account for intrinsic circuit characteristics such as the timing behaviors and energy profiles of Multiply-Accumulate (MAC) units. This disconnect from circuit-level behavior limits the ability to exploit available timing margins and energy-saving opportunities, reducing the overall efficiency of deployment on modern accelerators. To address these limitations, we propose HALO, a versatile framework for Hardware-Aware Post-Training Quantization (PTQ). Unlike traditional methods, HALO explicitly incorporates detailed hardware characteristics, including critical-path timing and power consumption, into its quantization approach. HALO strategically selects weights with low critical-path-delays enabling higher operational frequencies and dynamic frequency scaling without disrupting the architecture's dataflow. Remarkably, HALO achieves these improvements with only a few dynamic voltage and frequency scaling (DVFS) adjustments, ensuring simplicity and practicality in deployment. Additionally, by reducing switching activity within the MAC units, HALO effectively lowers energy consumption. Evaluations on accelerators such as Tensor Processing Units (TPUs) and Graphics Processing Units (GPUs) demonstrate that HALO significantly enhances inference efficiency, achieving average performance improvements of 270% and energy savings of 51% over baseline quantization methods, all with minimal impact on accuracy.

---

## 论文详细总结（自动生成）

# HALO：面向LLM加速的硬件感知量化与低关键路径延迟权重——论文详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景问题**：大语言模型（LLM）参数规模呈指数增长（每两年增长100倍），远超硬件性能提升速度（约3.1倍/代），导致推理成本日益高昂。量化是降低模型部署成本的关键手段，但现有量化方法存在根本性缺陷：
  - **硬件无关**：仅关注位宽约束，未考虑底层电路特性（如时序行为和能量特征）。
  - **黑箱处理**：将乘累加单元（MAC）视为黑箱，而MAC单元在TPU等加速器中占据77–80%的面积、消耗50–89%的功耗。
  - **忽视时序裕量**：无法利用电路级时序裕量和节能机会，限制了推理效率的进一步提升。
- **核心观察**：通过对MAC单元进行详细的静态时序分析和功耗分析，作者发现**量化后的不同权重值具有显著不同的关键路径延迟（critical-path-delay）和能耗特性**。例如，8位权重值64可实现3.7 GHz的工作频率，而权重值-127仅能运行在1.9 GHz。这一发现意味着：通过**策略性地选择低关键路径延迟的权重值**，可以在不牺牲精度的前提下提升系统运行频率、降低能耗。
- **研究意义**：本文首次将电路级时序和功耗特性引入LLM后训练量化（PTQ）流程，弥合了模型压缩与硬件适配之间的鸿沟，为在现代加速器（TPU、GPU）上高效部署LLM提供了新的软硬件协同优化思路。

## 2. 提出的方法论（HALO框架）

### 2.1 核心思想
- HALO是一个**硬件感知的后训练量化（PTQ）框架**，显式地将MAC单元的**关键路径时序**与**功耗特征**纳入量化决策。
- 核心逻辑：将权重按关键路径延迟分类，为"快速"的tile分配更高电压-频率（VF）点以提升速度，为"慢速"的tile分配较低VF点以节省能耗，同时保持精度。
- 框架接受硬件描述（如MAC频率与功耗曲线、DVFS配置、tile尺寸）和用户设计目标（精度/性能/能效），输出一组Pareto最优的量化模型及对应的DVFS调度方案。

### 2.2 三阶段流程（对应Algorithm 1）

**阶段一：敏感性感知均匀量化（Sensitivity-Aware Uniform Quantization）**
- **异常值与显著权重提取**：使用3σ规则识别异常值（outliers）；利用Fisher信息矩阵近似Hessian矩阵，基于梯度计算权重敏感性，保留前0.05%的高敏感性权重。异常值与显著权重合计不到总权重数的0.5%，对其采用逐通道量化并编码为稀疏矩阵。
- 敏感性计算公式：$F = \frac{1}{|D|}\sum_{d \in D} g_d g_d^\top$

**阶段二：关键路径延迟感知的非均匀量化（Critical-Path-Delay Aware Non-Uniform Quantization）**
- **基于Tile的敏感性分析**：将权重张量划分为固定尺寸的tile（默认128×128），计算每个tile的敏感性得分：
  $\Lambda_{T_k} = \frac{\sum_{i,j} g_{k,i,j}^2}{\text{tile rows} \times \text{tile cols}}$
- **动态敏感性映射**：按累积敏感性分布将tile分为低敏感与高敏感两类：低敏感tile激进量化（仅使用9种可运行于3.7 GHz的权重值），高敏感tile保留较精度（使用16种可运行于2.4 GHz的权重值）。
- 最终达到约3–4比特的有效位宽。

**阶段三：自适应DVFS策略（Adaptive DVFS Strategy）**
- **显著/异常值权重**：通过稀疏矩阵-向量乘法（SpMV）引擎执行，选择满足时序约束下能耗最低的(V, f)点。
- **高/低敏感tile**：按上述频率等级（3.7 GHz/2.4 GHz）分别选择DVFS配置。
- **DVFS调度优化**：将相同频率等级的tile分组执行，每组仅触发一次DVFS切换（每次数十纳秒至数微秒，远小于LLM推理时间），最小化切换开销。离线完成量化和频率分配，不改变数据流和数值正确性。

## 3. 实验设计

### 3.1 数据集与模型
- **模型**：LLaMA2家族（7B、13B）和OPT家族（1.3B、2.7B、6.7B、13B、30B）。
- **数据集**：C4和WikiText2，评估指标为困惑度（Perplexity，越低越好），序列长度2048。

### 3.2 对比基线
- RTN（Round-To-Nearest）量化
- SmoothQuant（W8A8、W4A8、W3A8）
- GPTQ（W4A8）
- ZeroQuant（ZQ-Local、ZQ-Global，W4A8）
- FP16理想情况作为参考上界

### 3.3 HALO变体
- **perf-opt**：性能优先
- **acc-opt**：精度优先
- **bal**：平衡配置（对应Pareto曲线的膝点）

### 3.4 硬件场景
- **脉动阵列（TPU类）**：自建SystemVerilog模拟器，22nm工艺综合；DVFS档位：(1.0V, 1.9GHz)、(1.1V, 2.4GHz)、(1.2V, 3.7GHz)。
- **GPU**：扩展AccelSim模拟NVIDIA 2080 Ti，使用AccelWattch和GPUWattch估算能耗；DVFS档位：(0.9V, 1.5GHz)、(1.0V, 2.0GHz)、(1.1V, 2.8GHz)。
- **Tile尺寸消融**：128×128、64×64、32×32。

## 4. 资源与算力

论文中**未明确说明**训练或推理所消耗的具体算力资源（如GPU型号、数量、总时长），仅提供以下间接信息：

- 使用22nm工艺对MAC单元进行综合与静态时序分析（Synopsys工具）。
- 使用自建的SystemVerilog脉动阵列模拟器和扩展的AccelSim GPU模拟器进行性能评估。
- 模型使用公开的预训练权重（LLaMA2、OPT），未涉及从头训练。
- 代码已开源（https://github.com/ecolab-nus/HALO），但未报告端到端的时间开销。

## 5. 实验数量与充分性评估

### 实验数量
- **精度评估**：涵盖4个模型（LLaMA2-7B/13B、OPT-1.3B/30B）×2个数据集，对比5种基线方法。
- **性能评估**：7个模型在脉动阵列上的执行时间；6个模型在GPU上的执行时间。
- **能耗评估**：7个模型的归一化能耗对比（脉动阵列和GPU各一组）。
- **Tile尺寸消融**：3种tile尺寸（128/64/32）在6个模型上的对比。
- **Pareto分析**：4个模型的性能-精度权衡曲线。

### 充分性评价
**优点方面**：
- 覆盖了从1.3B到30B的多种规模模型，验证了方法的可扩展性。
- 同时评估精度（perplexity）、性能（延迟）和能耗三个维度，角度全面。
- 包含多种HALO变体（perf-opt/acc-opt/bal），体现了不同优化目标的灵活性。

**不足方面**：
- 仅评估语言建模任务（perplexity），未覆盖下游任务（如常识推理、代码生成、翻译等），泛化性证据不足。
- 精度对比表（Table 2）中部分基线（GPTQ、ZQ-Local、ZQ-Global）仅报告了OPT模型的少数结果，对比不够完整。
- 未对比更先进的混合精度方法（如AWQ、SqueezeLLM等），削弱了"最先进"结论的说服力。
- GPU实验使用模拟器而非真实硬件，DVFS档位参数是假设值（如2.8 GHz），实际效果可能有所偏差。

## 6. 主要结论与发现

- **显著性能提升**：在脉动阵列上，HALO相比FP16平均获得**353%的加速**，相比其他量化基线最高提升**87%**；相比基线量化方法平均性能提升**270%**，能耗节省**51%**。
- **精度保持**：HALO（acc-opt和bal）的困惑度退化通常低于0.5，在W3A8/W4A8等效位宽下显著优于RTN和SmoothQuant；且模型规模越大，HALO相对基线的精度优势越明显。
- **能量优化机制**：HALO通过降低MAC单元内的开关活动实现节能，即使在更高VF点运行时，总能耗也未比W8A8增加。
- **Tile尺寸影响**：更小的tile（32×32相比128×128）带来额外15%的性能提升，同时精度也更好，说明细粒度控制有利于时序和能耗优化。
- **DVFS开销可忽略**：通过将同频tile分组调度，DVFS切换次数极少（2–3次），切换时间远小于推理时间，实际开销可忽略。

## 7. 方法优点

1. **首次将电路级时序特性引入量化**：不将MAC视为黑箱，而是基于真实电路分析（静态时序分析+功耗仿真），发现并利用权重值对关键路径延迟的影响，视角新颖。
2. **软硬件协同优化**：将量化与DVFS统一为单一优化框架，同时提升性能与能效，而非仅关注模型压缩率。
3. **无需改动硬件**：纯软件层面的方法，兼容现有GPU/TPU基础设施，实用性强。
4. **设计灵活性**：支持用户自定义目标（精度/性能/能效），输出Pareto最优解集，适应多样化部署需求。
5. **精度-性能-能耗三目标均衡**：通过敏感性感知的tile分类，在关键权重上保留精度、在非关键权重上激进优化，实现多目标权衡。

## 8. 不足与局限

1. **实验验证基于模拟**：性能与能耗均来自模拟器（自定义SystemVerilog模拟器、AccelSim），未见真实TPU/GPU硬件上的实测结果，DVFS参数（电压/频率组合）也是假设值而非实测值，实际效果存在不确定性。
2. **任务覆盖范围有限**：仅评估语言建模困惑度，未验证下游任务（如问答、推理、代码生成）上的精度影响，实际部署场景中的鲁棒性尚存疑。
3. **基线覆盖不完整**：未与AWQ、SqueezeLLM、OWQ等热门PTQ方法对比，也未与当前广泛使用的GPTQ进行完整对比（GPTQ结果仅部分模型可用）。
4. **DVFS实现细节未充分说明**：GPU上的DVFS频率切换是否考虑了真实GPU硬件的DVFS粒度和系统级限制，论文未做深入讨论。
5. **硬件适配范围有限**：DVFS档位设计基于特定假设（如NVIDIA 2.8 GHz峰值），不同厂商、不同架构硬件的适用性需进一步验证。
6. **异常值/显著权重的计算开销**：虽然声称处理时间不到总推理时间的1%，但离线阶段的敏感性分析、梯度计算和量化调优的具体时间成本未报告。

（完）
