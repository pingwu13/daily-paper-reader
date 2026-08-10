---
title: "SPEED-Q: Staged Processing with Enhanced Distillation Towards Efficient Low-Bit On-Device VLM Quantization"
title_zh: SPEED-Q：面向端侧VLM高效低位量化的分阶段处理与增强蒸馏
authors: "Tianyu Guo, Shanwei Zhao, Shiai Zhu, Chenguang Ma"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39296/43257"
tags: ["query:ulbv"]
score: 4.0
evidence: 面向端侧VLM的低位权重量化部署，与LLM部署需求相邻
tldr: 面向智能手机、机器人等端侧设备上VLM部署的资源约束，关注1B到2B参数VLM的激进低位权重量化，提出SPEED-Q框架。它采用分阶段处理与增强蒸馏相结合的方式，系统性地对VLM权重进行低比特量化。相关工作在降低内存和带宽需求的同时保持任务性能，为低位量化方法从纯语言模型迁移到多模态端侧部署提供了参考。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39296/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 810, \"height\": 625, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39296/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 820, \"height\": 413, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39296/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 860, \"height\": 446, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39296/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 851, \"height\": 544, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39296/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 861, \"height\": 425, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39296/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1785, \"height\": 763, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39296/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 265, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39296/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1794, \"height\": 1309, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39296/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 850, \"height\": 227, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39296/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 810, \"height\": 357, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39296/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1827, \"height\": 475, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39296/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1773, \"height\": 312, \"label\": \"Table\"}]"
motivation: 端侧设备部署VLM受资源限制，而1B到2B参数的VLM低位权重量化研究仍较少。
method: 提出SPEED-Q，通过分阶段处理与增强蒸馏进行VLM低比特权重量化。
result: 在端侧VLM低位部署上改善量化精度与效率。
conclusion: 拓展了低比特权重量化到VLM端侧部署场景。
---

## Abstract
Deploying Vision-Language Models (VLMs) on edge devices (e.g., smartphones and robots) is crucial for enabling low-latency and privacy-preserving intelligent applications. Given the resource constraints of these devices, quantization offers a promising solution by improving memory efficiency and reducing bandwidth requirements, thereby facilitating the deployment of VLMs. However, existing research has rarely explored aggressive quantization on VLMs, particularly for the models ranging from 1B to 2B parameters, which are more suitable for resource-constrained edge devices. In this paper, we propose SPEED-Q, a novel Staged Processing with EnhancEd Distillation framework for VLM low-bit weight-only quantization that systematically addresses the following two critical obstacles: (1) significant discrepancies in quantization sensitivity between vision (ViT) and language (LLM) components in VLMs; (2) training instability arising from the reduced numerical precision inherent in low-bit quantization. In SPEED-Q, a staged sensitivity adaptive mechanism is introduced to effectively harmonize performance across different modalities. We further propose a distillation-enhanced quantization strategy to stabilize the training process and reduce data dependence. Together, SPEED-Q enables accurate, stable, and data-efficient quantization of complex VLMs. SPEED-Q is the first framework tailored for quantizing entire small-scale billion-parameter VLMs to low bits. Extensive experiments across multiple benchmarks demonstrate that SPEED-Q achieves up to 6x higher accuracy than existing quantization methods under 2-bit settings and consistently outperforms prior on-device VLMs under both 2-bit and 4-bit settings.

---

## 论文详细总结（自动生成）

# SPEED-Q 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：将视觉-语言模型（VLM）部署到端侧设备（如智能手机、机器人）是实现低延迟和隐私保护智能应用的关键，但VLM的大体量使其在资源受限设备上的部署面临巨大挑战。量化通过降低权重或激活的精度来减少内存占用和带宽需求，是促进端侧部署的有效手段。
- **现有研究的空白**：已有量化工作大多关注8/4比特量化，且仅量化语言模块（LLM），视觉模块（ViT）保持FP16精度。对于更适合端侧资源约束的1B到2B参数VLM的**激进的低位（2比特）全模型量化**，几乎未被探索。以InternVL3-1B为例，ViT编码器含304M/938M参数，忽略视觉模块将严重阻碍模型整体压缩。
- **核心挑战**：
  - **跨模块敏感度差异显著**：如图2所示，在量化感知训练（QAT）过程中，LLM的梯度量级远高于ViT，统一量化策略会导致精度严重下降。
  - **训练不稳定**：低位量化固有的数值精度降低带来振荡和收敛困难。
  - **数据依赖强**：QAT需要大量校准数据，而多模态数据的收集远比纯文本数据困难，制约了实用性和泛化性。
- **整体含义**：论文首次提出了针对1-2B参数VLM的2/4比特权重量化框架，使得端侧部署高级VLM成为可能，在模型体积大幅缩减的同时保持较高的任务性能，为VLM在更广泛的端侧设备上落地提供了新路径。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

SPEED-Q框架由两大核心策略组成，针对上述挑战逐一击破：

### 2.1 分阶段量化策略（Staged Quantization Strategy）

- **核心思想**：将端到端量化误差最小化过程分解为三个顺序阶段，逐步适配不同模块的敏感度差异，避免联合量化（Joint Quantization）带来的优化困难。
- **算法流程（文字说明）**：
  - **Stage 1（ViT量化）**：仅对敏感度较低的ViT模块应用分组量化，利用自适应取整（Adaptive Rounding，参考Nagel et al. 2020）机制最小化每个块的局部重构误差。
  - **Stage 2（Projector对齐）**：冻结已量化的ViT，只训练Projector层，使其输出分布适应量化后ViT的特征，从而与原始LLM期望的分布对齐，弥补ViT量化带来的特征偏移。
  - **Stage 3（LLM量化与联合微调）**：冻结量化后的ViT，将Projector和更敏感的LLM进行联合量化感知训练，恢复端到端性能并优化低位约束下的对齐。
- **形式化表达**：VLM量化目标被建模为 L = f(Q_ViT, Q_LLM; S_stage, S_opt)，其中S_stage表示分阶段量化安排，S_opt表示优化目标设计。

### 2.2 增强蒸馏量化策略（Distillation-enhanced Quantization）

该策略包含三个关键组件，系统性地解决初始化、数据效率与优化稳定性的挑战：

- **非对称裁剪（Asymmetric Clipping）**：在QAT之前，对每层权重进行离线的逐层非对称裁剪，自动搜索最优裁剪值α*和β*，最小化量化前后模型输出之间的L2距离：**α*, β* = arg min || f(W_c)x - Wx ||**，其中W_c = clip(W, α, β)。这有效缓解了初始浮点权重中离群值对量化的负面影响。
- **自蒸馏策略（Self-distillation Strategy）**：将固定的预训练BF16模型作为教师，量化模型作为学生。蒸馏损失结合反向KL与正向KL散度：**L_distill = γD_KL(Ps||Pt) + (1−γ)D_KL(Pt||Ps)**。其中γ根据教师模型在随机小批量上的平均Token概率自适应估计，使得当教师高置信度时优先反向KL（让学生聚焦强预测），否则优先正向KL（匹配完整分布），从而实现更稳定有效的知识迁移。
- **多损失优化策略（Multi-loss Optimization Strategy）**：在自蒸馏基础上引入交叉熵损失（对真实标签的监督）作为显式锚点，总损失定义为：**L_total = λL_distill + ζL_CE(Y, Ps)**，实验中设λ=ζ=1。两种损失互补——前者稳定特征空间对齐并降低数据依赖，后者保证目标任务的保真度，共同避免训练振荡。

### 2.3 双层级量化（Bilevel Quantization）

采用llama.cpp启发式的双层级量化方案：对第一层分组的尺度向量S再进行第二层分组量化，以进一步节省存储开销，降低整体平均位宽。

## 3. 实验设计：数据集、基准与对比方法

- **评估数据集（8个主流基准）**：MMBench、MMStar（综合多模态评估）、ScienceQA、MMMU（视觉推理）、AI2D、OCRBench（文字识别与理解）、SEED-Bench（视觉感知）、HallusionBench（幻觉评估），均基于VLMEvalKit工具包实现。
- **被量化/评估的模型**：
  - 小型端侧VLM（1-2B）：InternVL3-1B、InternVL3-2B、Qwen2-VL-2B、InternVL2.5-1B。
  - 轻量级预设端侧VLM：SmolVLM-256M/500M、FastVLM-600M、H2OVL-800M（与其BF16对比）。
  - 大规模VLM泛化验证：InternVL3-8B。
  - 与QAT基线QSLAW对比：LLaVA-13B（DocVQA和Real-WorldQA）。
- **对比方法**：RTN（Round-to-Nearest）、GPTQ（ICLR'23）、AWQ（MLSys'24）、MBQ（CVPR'25）、QSLAW（ACM MM'24），均为当前主流量化方法。
- **实验场景**：2-bit和4-bit权重量化（仅权重量化，weight-only），对比方法仅量化LLM，SPEED-Q同时量化ViT+LLM。为保证公平，4-bit下对比方法用w4g128，SPEED-Q用w4g32g128（平均位宽4.25 vs 4.28 bits相当）；2-bit下对比方法用w2g16，SPEED-Q用w2g16g16。
- **训练数据**：公开开源数据集，每个数据集采样10%，总计约68万训练样本。
- **消融实验**：在InternVL3-1B 4-bit量化下，分别评估多损失策略（任务损失 vs 蒸馏损失 vs 两者联合）和分阶段策略（联合量化 vs 分阶段量化）的有效性。
- **端侧效率评估**：在三星Galaxy S25 Ultra上对比FP16/4-bit/2-bit InternVL2.5-1B的文件大小和运行内存占用。
- **定性分析**：展示2-bit InternVL2.5-1B在主题识别、文字识别、地标识别等任务上的实际输出质量对比。

## 4. 资源与算力

- **论文未明确披露**具体使用的GPU型号、数量及训练时长等信息。
- 仅在实现细节中提及：使用DeepSpeed框架进行训练，全部训练数据取自公开开源数据集，采样后约68万样本。
- 作为参考，可推测其训练成本在合理范围内（68万样本对1-2B模型的QAT，单卡至数卡可完成），但论文未给出精确算力指标，这是其在复现透明度方面的一个小缺憾。

## 5. 实验数量与充分性

- **实验总量**：实验数量较多，覆盖全面，主要包括：
  1. 主实验（3个模型家族×2种位宽×4个对比方法+BF16基线，约30行结果）；
  2. 轻量级VLM对比实验（3个预设端侧模型BF16对比2个模型的2/4-bit量化）；
  3. 与QSLAW的专项对比（LLaVA-13B上2个数据集）；
  4. 消融实验（多损失策略、分阶段策略各两组对照）；
  5. 大规模模型泛化实验（InternVL3-8B，2种位宽）；
  6. 端侧效率实测（三星Galaxy S25 Ultra）；
  7. 定性案例分析。
- **充分性评估**：
  - **优点**：数据集覆盖8个多模态基准，任务类型多样（综合理解、推理、OCR、感知、幻觉）；模型覆盖不同规模（1B/2B/8B）、不同家族（InternVL、Qwen、LLaVA、FastVLM），外推性好；对比方法选取全面且均为近年SOTA；在2-bit下对比方法普遍失效的情况下，报告了RTN/GPTQ/MBQ等方法的崩溃值（如0.04），彰显对比的客观性。
  - **可改进之处**：消融实验仅在单个模型（InternVL3-1B）的4-bit设置下进行，对2-bit极端场景的消融覆盖不足；未报告多次运行的方差或置信区间，无法判断结果的统计显著性；对超参数（λ、ζ、γ）的敏感性未见分析。

## 6. 论文的主要结论与发现

- **分阶段量化优于联合量化**：将ViT和LLM的量化过程解耦为三阶段，可以显著降低优化难度，提升训练稳定性，在各基准上均优于一步到位的联合量化。
- **多损失联合优化有效**：自蒸馏损失与任务交叉熵损失互补，联合使用比单独使用任一损失均有明显提升。
- **4-bit精度几乎无损**：SPEED-Q量化的4-bit模型相比BF16原模型平均性能下降仅约3%（如InternVL3-1B从63.67降到62.44），同时显著减小模型体积。
- **2-bit极端量化下保持可用性**：现有方法在2-bit下基本崩溃（准确率降至1-18%），而SPEED-Q在InternVL3-1B上仍保持51.48的平均分（约为BF16的80%），在InternVL3-2B上达到59.47，实现了最多**6倍**于现有方法的精度提升。
- **压缩效果显著**：同时量化ViT和LLM使模型文件大幅缩小。InternVL3-1B的FP16模型为1789.5MB，4-bit为485.0MB，2-bit为315.0MB；2-bit InternVL2.5-1B在三星S25 Ultra上运行时内存仅139MB。
- **对轻量级VLM也有效**：将SPEED-Q应用于FastVLM-600M，4-bit下精度仅下降4.78%，2-bit下降15.06%。
- **可以超越原生轻量级VLM**：4-bit量化的InternVL2.5-1B（485MB）在所有基准上超越BF16的FastVLM-600M（1517MB），平均分高出近8分；2-bit版本（315MB）以1/5的体积达到与FastVLM相当的性能。
- **可推广到大模型**：在InternVL3-8B上，SPEED-Q在2-bit下领先MBQ约6.8个平均分，说明方法具有良好的泛化性。

## 7. 优点：方法与实验设计的亮点

1. **问题定位精准**：识别出1-2B参数VLM是端侧部署的"甜蜜点"，同时指出现有研究对全模型低比特量化的忽视，填补了该空白。
2. **分阶段处理设计巧妙**：将量化过程与模型结构（ViT→Projector→LLM）的敏感度差异结合，以解耦方式降低优化难度，逻辑清晰且可解释性强。
3. **自适应蒸馏机制新颖**：通过教师置信度动态调节正/反向KL权重（γ因子），使知识迁移策略随训练进度自适应，兼顾拟合稳定性与分布匹配度，是论文的技术亮点。
4. **数据效率意识的强化**：通过自蒸馏减少了对外部大规模多模态标注数据的依赖，对QAT在VLM上的实用性有实际意义。
5. **双层量化的工程价值高**：对量化参数本身进行量化，在不损失性能的前提下进一步压缩平均位宽，对端侧部署更加友好。
6. **实验对比公允**：用平均位宽对齐的方式（4.25 vs 4.28 bits）确保对比的公平性；在量化组件数量更多（ViT+LLM）的情况下仍取得更好性能，增加了说服力；同时报告了端侧实测数据（S25 Ultra），增强实际部署说服力。
7. **已验证方法的通用性**：从1B扩展到8B，从InternVL/Qwen扩展到LLaVA、FastVLM，展示了跨模型规模的适用性。

## 8. 不足与局限

1. **算力成本未披露**：未提供GPU型号、数量或训练时长等算力信息，难以评估方法的实际训练成本和对算力的要求。
2. **仅限权重量化**：论文只量化权重（weight-only），尾注提到未来将扩展到权重-激活量化（weight-activation quantization），当前方案对推理加速的收益仍受限于带宽瓶颈，未释放激活量化的额外加速潜力。
3. **2-bit消融覆盖不足**：消融实验仅在4-bit InternVL3-1B上进行，未验证分阶段策略和多损失机制在2-bit极端条件下的独立贡献，不确定这些设计在更极限场景下的有效性边界。
4. **缺少统计显著性检验**：未报告多次运行的方差、标准差或置信区间，实验结果的稳定性缺乏统计层面的保证。
5. **蒸馏对校准数据的依赖性仍有保留**：虽然声称减少数据依赖，但仍需68万训练样本；对仅少量（如几千）样本的极端低数据场景未做验证。
6. **端侧模型对比存在位宽不对称**：与轻量级VLM（SmolVLM、FastVLM等）对比时，这些模型是BF16而非量化后的同条件对比，虽然展示了效果优势，但若用同类方法量化这些轻量模型后对比则更具说服力（论文部分尝试了对FastVLM的量化，但未对其他两个进行）。
7. **模型文件大小计算基准不统一**：表2中"Quantize LLM only"的行未标明是哪个对比方法的结果，在解释上稍显模糊。
8. **附录信息缺失**：文中未见更多消融细节（如γ系数的具体演化、不同裁剪粒度的影响等），限制了研究的可复现性深度。

---

（完）
