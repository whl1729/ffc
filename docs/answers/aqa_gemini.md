# **动作质量评估（AQA）在体育运动领域的研究进展综述**

随着计算机视觉技术的飞速发展，视频理解已从简单的动作识别（Action Recognition）演进到更具挑战性的动作质量评估（Action Quality Assessment, AQA）阶段。动作识别侧重于判定“视频中发生了什么”，而动作质量评估则旨在量化“动作完成得有多好” 1。在体育竞技、医疗康复和工业培训等领域，这种对动作精细度、规范性以及专业水平的自动化评价具有极高的学术价值和应用潜力 2。特别是在 2024 年巴黎奥运会等大型赛事的背景下，针对潜水、体操、花样滑冰等高难度竞技项目的 AQA 研究已成为智能体育分析的核心热点 5。

## **动作质量评估的定义与多层级任务架构**

动作质量评估是一项通过分析视频或运动数据，对人类动作执行情况进行客观、精确评分的技术任务 1。其核心在于建立一个从高维视频表征到一维质量分值（或多维扣分项）的映射函数。根据任务复杂度和标注密度的不同，当前的 AQA 研究可以被划分为三个渐进的层级。

### **基础评估层级：粗粒度评分与分类**

在早期的研究范式中，AQA 被简化为对整个动作序列进行单一分值的回归。这种方法通常将视频输入预训练的特征提取器，如 3D 卷积神经网络（3D CNN），然后通过全连接层输出一个连续的质量分数 4。尽管这种方式在处理相对简单的动作时有效，但它往往忽略了动作内部的时空细微差别。此外，部分研究也将 AQA 建模为分类问题，通过预测动作属于特定的质量等级（如“优”、“良”、“差”）来进行初步量化 4。

### **过程感知层级：细粒度子动作分析**

现代 AQA 研究越来越多地关注动作的内部时间结构。例如，在跳水运动中，一个完整的规程（Routine）包含起跳（Take-off）、空中飞行（Flight）和入水（Entry）等关键阶段 9。过程感知型方法通过对视频进行时域分割，识别这些子动作（Sub-actions）并分别评估其质量 3。通过构建细粒度的语义对应关系，模型能够更准确地模拟人类裁判的判分逻辑，从而提供更具解释性的反馈 10。

### **综合反馈层级：多任务与跨模态理解**

最高层级的 AQA 任务不仅要求给出总分，还需提供详细的扣分项说明、语音反馈或动作改进建议 3。这要求模型具备多任务学习能力，同时利用视频、音频（如花样滑冰中的配乐协调性）以及传感器数据（如 IMU、肌电信号）进行综合研判 6。

下表总结了 AQA 在不同体育细分领域的应用范畴及其核心关注点：

| 体育领域 | 代表项目 | 评估核心指标 | 典型数据源 |
| :---- | :---- | :---- | :---- |
| **竞技体操/跳水** | 跳水、跳马、花样滑冰 | 动作难度、姿态稳定性、技术完成度 | 单/多视角视频、裁判评分 5 |
| **有氧/传统健身** | 健身操、八段锦、瑜伽 | 动作幅度、呼吸节奏、动作流利度 | RGB 视频、人体骨架 12 |
| **力量训练** | 举重、哑铃练习 | 身体对齐度、负重平衡、受伤风险评估 | 多视角视频、sEMG 信号 14 |
| **户外/极端运动** | 滑雪、冲浪、空中技巧 | 空中姿态、翻腾周数、落地平稳度 | 视频、UAV 视角、IMU 传感器 13 |

## **特征提取与时空表征学习的技术演进**

特征提取是 AQA 系统的基石。体育运动视频具有背景复杂、动作瞬时性强、同类动作间差异细微等特点，这对表征学习提出了极高的要求 4。

### **基于 3D 卷积神经网络的视觉表征**

3D CNN（如 C3D, I3D, SlowFast）曾是 AQA 领域的主导技术。这些网络通过引入时间维度的卷积核，能够同时捕获视频帧中的空间外观特征和帧间的运动信息 8。然而，3D CNN 面临计算成本高昂且难以处理长视频依赖的局限。为了增强模型对运动细节的敏感度，研究者们引入了光流（Optical Flow）分支，通过双流网络（Two-stream networks）显式捕捉物体的运动轨迹和速度变化 18。

### **骨架拓扑建模与图卷积网络（GCN）**

由于体育运动本质上是人体的动力学展现，基于骨架（Skeleton-based）的 AQA 方法逐渐兴起。该方法将人体关节视为节点，骨骼视为边，构建出非欧几里得空间的图结构 19。

1. **时空图卷积网络（ST-GCN）**：ST-GCN 及其变体通过在空间图上进行节点卷积和在时间序列上进行一维卷积，有效建模了人体部位间的协调性和时间演化 21。  
2. **部分级别建模（PL-GCN）**：为了捕捉局部的技术细节，最新的研究提出了部分级别图卷积网络。不同于手动定义身体结构，PL-GCN 允许模型通过可学习的图池化（Graph Pooling）操作，自动聚合关键关节以形成“身体部分”表征（如专注于跳水运动员的腰腹力量或腿部伸展） 23。  
3. **拓扑感知与位置鲁棒性**：如 GCN-PSN 等框架，通过学习拓扑敏感的姿态嵌入，能够在不同视角、尺度和位置下保持评估的稳定性，这对于在复杂户外场景（如滑雪场）中的 AQA 尤为重要 24。

### **Transformer 与长程时间建模**

随着 Transformer 架构在计算机视觉领域的成功，AQA 也开始利用注意力机制来建模视频中的长程依赖。时间解析 Transformer（Temporal Parsing Transformer, TPT）可以自适应地学习动作不同阶段的重要性，并通过自注意力机制捕捉关键帧（Keyframes）对最终质量评分的影响 11。此外，Transformer 易于扩展至多模态输入，使得视觉、音频和语义信息的对齐变得更加高效 6。

## **核心回归范式与学习策略**

动作质量评估的本质是一个回归任务，但由于评分的主观性和样本的高度相似性，传统的直接回归面临巨大的优化压力。研究界为此探索了多种改进范式。

### **对比回归（Contrastive Regression, CoRe）**

对比回归是近年来 AQA 领域的重大突破。其核心逻辑在于：与其直接预测一个视频的绝对分值，不如通过对比该视频与一个参考示例（Exemplar）之间的差异来回归相对分值 17。

这种方法的数学表达通常涉及成对的输入。设查询视频为 ![][image1]，参考视频为 ![][image2]，其对应的真实分数为 ![][image3] 和 ![][image4]。模型的目标是预测相对分数 ![][image5]。在推理阶段，通过聚合多个参考示例的预测结果，可以得到更稳健的最终得分 28。对比回归的优势在于它能够显著降低由于拍摄条件、运动员个体差异带来的噪声干扰，强迫模型关注那些真正决定动作质量的技术指标 27。

### **评分不确定性建模**

裁判评分中的模糊性和主观分歧是 AQA 无法回避的问题。为了量化这种不确定性，研究者提出了不确定性感知评分分布学习（Uncertainty-aware Score Distribution Learning, USDL） 31。

* **分布回归**：USDL 不预测单一标量值，而是预测一个分数的概率分布（通常建模为高斯分布）。这种方法能够有效容忍标签噪声，并提供预测结果的置信度 13。  
* **不确定性驱动训练（UD-AQA）**：利用条件变分自编码器（CVAE）捕获裁判评估中的内在歧义。UD-AQA 模型通过采样潜空间来模拟多个评分者的判定过程，并根据预测的不确定度动态调整样本的训练权重（优先学习确定性高的样本），从而提升模型的泛化能力 8。

### **多任务与指令对齐学习**

为了提升 AQA 的实用性，模型需要将评估分数与具体的领域知识相对齐。

* **指令对齐（CoFInAl）**：受大语言模型微调的启发，CoFInAl 将 AQA 任务重新表述为从粗到细的指令对齐过程。模型首先学习粗粒度的评分原型（Grade Prototypes），再利用固定的细粒度子评分原型进行修正，这一过程模拟了裁判查阅评分标准手册的行为，增强了判分的权威性 12。  
* **联合动作识别与质量评估**：许多最新框架（如 SkatingVerse）采用多任务架构，同时进行动作类型识别和分值回归。这能够共享底层特征，并利用动作类别的先验知识来约束评分范围 34。

## **体育 AQA 数据集与基准测试**

数据集的质量和标注颗粒度直接决定了 AQA 技术的发展。从早期的数以百计的样本到如今包含多模态标注的数千个样本，数据集的演进反映了该领域对“精细化”的追求。

### **常用运动 AQA 数据集对比**

下表汇总了当前主流的运动 AQA 数据集及其核心参数：

| 数据集 | 运动项目 | 样本数 | 核心特性 |
| :---- | :---- | :---- | :---- |
| **AQA-7** | 跳水、体操、滑雪等 7 类 | 1,189 | 早期多动作 AQA 基准，标注相对粗略 36 |
| **MTL-AQA** | 跳水 | 1,412 | 引入多任务标注（难度系数、分值、文本描述） 38 |
| **Diving48** | 跳水 | 18,404 | **超大规模**，专注细粒度动作识别与质量区分 40 |
| **FineDiving** | 跳水 | 3,000 | **首个过程感知数据集**，提供两级语义和时间戳标注 9 |
| **FineSkiing** | 空中技巧滑雪 | \- | 提供详尽的扣分项描述和技术阶段划分 16 |
| **SkatingVerse** | 花样滑冰 | 11,671 | 涵盖识别、分割、提议和评估的全方位基准 34 |
| **FLEX** | 健身训练 | 38 主体 | **多视角、多模态**（RGB, sEMG, 3D），涵盖 20 类健身动作 14 |
| **MMW-AQA** | 风帆冲浪 | \- | 真实野外场景，包含视频、IMU 和 GPS 数据 13 |

### **过程感知标注的重要性**

以 FineDiving 为例，该数据集的推出标志着 AQA 进入了“可解释评价”时代。它将每一次跳水动作标注为连续的阶段，并为每个阶段关联了语义标签（如“4.5 周翻腾-抱膝”） 10。这种标注结构允许开发出“时间段划分注意力”（Temporal Segmentation Attention, TSA）等先进模块，使模型能够精确地对比查询动作与模范动作在同一技术节点的执行优劣 9。

## **细分运动项目的 AQA 深入分析**

不同体育项目对 AQA 算法的侧重点各有不同，这催生了一系列高度专业化的评估模型。

### **竞技类水上与体操项目**

跳水和体操是 AQA 最早且最成熟的应用领域。这些动作通常具有极高的瞬时角速度和复杂的空间旋转。

* **视觉关注点**：入水时的垂直度、水花控制（Splash Control）以及空中翻腾时的姿态紧凑度 9。  
* **技术挑战**：极短时间内的剧烈姿态变换。这要求特征提取器具备高帧率采样能力和极强的动态捕捉能力。多分支模型通常利用空间-时间多级特征融合来应对这些挑战 18。

### **艺术类与长序列运动**

花样滑冰和节奏体操不仅评估技术动作，还强调艺术感染力。

* **多模态对齐**：研究表明，音频信息是改善这类项目 AQA 准确性的关键。模型必须学习“动作-音乐”的协调性（Action-Music Coordination），即动作的起承转合是否与背景音乐的节拍和旋律同步 6。  
* **长程依赖**：花样滑冰视频通常持续数分钟，包含多个技术单元。长程建模技术（如自适应频率感知网络 AFA）对于捕捉此类长序列中的技术一致性至关重要 12。

### **康复训练与大众健身**

不同于竞技体育追求极致，康复和健身 AQA 关注的是动作的“正确性”和“预防伤害”。

* **专家知识引导**：在物理康复评估中，通过整合医疗专家的经验知识（如特定关节的活动范围），可以显著提升 GCN 模型对非标准动作的敏感性 20。  
* **人体-物体交互**：健身 AQA（如举重）需要同时考虑人与器材的关系。FLEX 数据集通过引入表面肌电信号（sEMG），能够直接评估肌肉的收缩状态，这是仅靠视觉特征无法触及的生理维度 14。

### **跨物种动作评估的借鉴**

尽管用户查询聚焦于运动项目，但动作质量评估的逻辑已延伸至生物行为学。例如针对“猪”等畜牧动物的行为识别与步态分析（PIG 数据集） 36。

这种跨领域的研究提供了宝贵的算法借鉴：通过姿态估计和非接触式生理监测（如心率、呼吸率和加速度数据），可以评估动物的健康福利状态 45。其针对复杂拥挤场景（如猪圈）下的多目标跟踪和姿态判别技术，也为团操课等多人运动 AQA 提供了技术参考 46。

## **评估指标与性能衡量**

为了客观评价 AQA 模型的性能，研究界采纳了一套标准化的度量衡体系。

### **Spearman 等级相关系数（SRCC）**

SRCC 是 AQA 领域最核心的指标，用于衡量模型预测评分的排序与专家真实评分排序之间的一致性。其定义为：

![][image6]  
其中 ![][image7] 为两组排名之间的差值，![][image8] 为样本数量 35。SRCC 越高，说明模型对动作“好坏顺序”的判定越接近人类专家。

### **相对 ![][image9] 距离（R\-![][image9]）与均方误差（MSE）**

由于不同体育项目的评分量纲不同（如跳水总分通常超过 100，而有些项目采用 10 分制），R\-![][image9] 通过对误差进行标准化处理，使得跨数据集的性能比较成为可能 17。MSE 则用于衡量绝对得分的预测偏差。

## **未来展望与挑战**

尽管动作质量评估在 2024 年至 2025 年间取得了显著突破，但要实现真正媲美专业裁判的自动化系统，仍需克服以下障碍：

1. **数据获取与专家标注的稀缺性**：构建高质量 AQA 数据集不仅需要视频，更需要昂贵的裁判级标注 12。自监督学习（Self-supervised Learning）和从动作识别数据集中迁移知识（Transfer Learning）将是未来的重点研究方向 3。  
2. **极端视角与多视角融合的稳健性**：目前大多数 AQA 研究依赖于广播级视角。如何利用无人机（UAV）捕获的野外视角或多个运动相机捕获的同步视角，构建鲁棒的 3D 动作理解模型，是提升实用性的关键 13。  
3. **可解释性与实时反馈**：运动员和教练员需要的不仅仅是一个分数，更是“为什么扣分”和“如何改进”。开发具备自然语言解释能力的生成式 AQA 模型，并将计算开销降低到边缘设备可部署的水平，是技术落地的核心 10。  
4. **通用性与持续学习**：现有的模型往往局限于单一项目。如何训练一个通用的动作质量评估大模型，使其能够快速适应新的运动规则或极少见的动作动作（Few-shot learning），是该领域迈向通用人工智能（AGI）的必经之路 4。

动作质量评估不仅是计算机视觉领域的一次深层次探索，更是技术赋能体育、改善人类运动表现的重要工具。随着多模态感知技术的不断精进，我们有望在不久的将来见证实时、精准且充满智慧的“AI 裁判”走进千家万户的训练场 2。

#### **引用的著作**

1. A Survey of Deep Learning in Sports Applications: Perception, 访问时间为 五月 5, 2026， [https://www.computer.org/csdl/journal/tg/2025/10/10938940/25p2Z9OhGM0](https://www.computer.org/csdl/journal/tg/2025/10/10938940/25p2Z9OhGM0)  
2. (PDF) Athlete action quality assessment based on transfer neural, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/403453906\_Athlete\_action\_quality\_assessment\_based\_on\_transfer\_neural\_network\_quality\_score\_decoupling\_in\_complex\_sports\_scenarios](https://www.researchgate.net/publication/403453906_Athlete_action_quality_assessment_based_on_transfer_neural_network_quality_score_decoupling_in_complex_sports_scenarios)  
3. Semantic-aware self-supervised learning using progressive sub, 访问时间为 五月 5, 2026， [https://pmc.ncbi.nlm.nih.gov/articles/PMC12913629/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12913629/)  
4. A Decade of Action Quality Assessment: Largest Systematic Survey, 访问时间为 五月 5, 2026， [https://arxiv.org/html/2502.02817v1](https://arxiv.org/html/2502.02817v1)  
5. Hierarchical NeuroSymbolic Approach for Comprehensive and, 访问时间为 五月 5, 2026， [https://openaccess.thecvf.com/content/CVPR2024W/CVsports/papers/Okamoto\_Hierarchical\_NeuroSymbolic\_Approach\_for\_Comprehensive\_and\_Explainable\_Action\_Quality\_Assessment\_CVPRW\_2024\_paper.pdf](https://openaccess.thecvf.com/content/CVPR2024W/CVsports/papers/Okamoto_Hierarchical_NeuroSymbolic_Approach_for_Comprehensive_and_Explainable_Action_Quality_Assessment_CVPRW_2024_paper.pdf)  
6. Language-Guided Audio-Visual Learning for Long-Term Sports, 访问时间为 五月 5, 2026， [https://openaccess.thecvf.com/content/CVPR2025/papers/Xu\_Language-Guided\_Audio-Visual\_Learning\_for\_Long-Term\_Sports\_Assessment\_CVPR\_2025\_paper.pdf](https://openaccess.thecvf.com/content/CVPR2025/papers/Xu_Language-Guided_Audio-Visual_Learning_for_Long-Term_Sports_Assessment_CVPR_2025_paper.pdf)  
7. Survey of Action Quality Assessment \- GitHub Pages, 访问时间为 五月 5, 2026， [https://haoyin116.github.io/Survey\_of\_AQA/](https://haoyin116.github.io/Survey_of_AQA/)  
8. Uncertainty-Driven Action Quality Assessment \- arXiv, 访问时间为 五月 5, 2026， [https://arxiv.org/html/2207.14513v2](https://arxiv.org/html/2207.14513v2)  
9. FineDiving: A Fine-Grained Dataset for Procedure-Aware Action, 访问时间为 五月 5, 2026， [https://openaccess.thecvf.com/content/CVPR2022/papers/Xu\_FineDiving\_A\_Fine-Grained\_Dataset\_for\_Procedure-Aware\_Action\_Quality\_Assessment\_CVPR\_2022\_paper.pdf](https://openaccess.thecvf.com/content/CVPR2022/papers/Xu_FineDiving_A_Fine-Grained_Dataset_for_Procedure-Aware_Action_Quality_Assessment_CVPR_2022_paper.pdf)  
10. FineDiving: A Fine-grained Dataset for Procedure-aware Action, 访问时间为 五月 5, 2026， [https://sites.google.com/view/finediving](https://sites.google.com/view/finediving)  
11. Visual-Semantic Alignment Temporal Parsing for Action Quality, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/385336552\_Visual-semantic\_Alignment\_Temporal\_Parsing\_for\_Action\_Quality\_Assessment](https://www.researchgate.net/publication/385336552_Visual-semantic_Alignment_Temporal_Parsing_for_Action_Quality_Assessment)  
12. CoFInAl: Enhancing Action Quality Assessment with Coarse-to-Fine, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/382789583\_CoFInAl\_Enhancing\_Action\_Quality\_Assessment\_with\_Coarse-to-Fine\_Instruction\_Alignment](https://www.researchgate.net/publication/382789583_CoFInAl_Enhancing_Action_Quality_Assessment_with_Coarse-to-Fine_Instruction_Alignment)  
13. MMW-AQA: Multimodal In-the-Wild Dataset for Action Quality, 访问时间为 五月 5, 2026， [https://ieeexplore.ieee.org/iel8/6287639/10380310/10584527.pdf](https://ieeexplore.ieee.org/iel8/6287639/10380310/10584527.pdf)  
14. FLEX: A Largescale Multimodal, Multiview Dataset for Learning, 访问时间为 五月 5, 2026， [https://arxiv.org/html/2506.03198v4](https://arxiv.org/html/2506.03198v4)  
15. Multimodal Action Quality Assessment \- ResearchGate, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/378313065\_Multimodal\_Action\_Quality\_Assessment](https://www.researchgate.net/publication/378313065_Multimodal_Action_Quality_Assessment)  
16. FineSkiing: A Fine-grained Benchmark for Skiing Action Quality, 访问时间为 五月 5, 2026， [https://arxiv.org/html/2511.10250](https://arxiv.org/html/2511.10250)  
17. Group-Aware Contrastive Regression for Action Quality Assessment, 访问时间为 五月 5, 2026， [https://openaccess.thecvf.com/content/ICCV2021/papers/Yu\_Group-Aware\_Contrastive\_Regression\_for\_Action\_Quality\_Assessment\_ICCV\_2021\_paper.pdf](https://openaccess.thecvf.com/content/ICCV2021/papers/Yu_Group-Aware_Contrastive_Regression_for_Action_Quality_Assessment_ICCV_2021_paper.pdf)  
18. Comprehensive Action Quality Assessment Through Multi-Branch, 访问时间为 五月 5, 2026， [https://ieeexplore.ieee.org/iel8/6046/10844992/11154451.pdf](https://ieeexplore.ieee.org/iel8/6046/10844992/11154451.pdf)  
19. Graph Convolutional Neural Network for Human Action Recognition, 访问时间为 五月 5, 2026， [https://www.computer.org/csdl/journal/ai/2021/02/09420299/1tdUOQ13tHG](https://www.computer.org/csdl/journal/ai/2021/02/09420299/1tdUOQ13tHG)  
20. (PDF) An Expert-Knowledge-Based Graph Convolutional Network, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/380583834\_An\_Expert-Knowledge-based\_Graph\_Convolutional\_Network\_for\_Skeleton-based\_Physical\_Rehabilitation\_Exercises\_Assessment](https://www.researchgate.net/publication/380583834_An_Expert-Knowledge-based_Graph_Convolutional_Network_for_Skeleton-based_Physical_Rehabilitation_Exercises_Assessment)  
21. A Survey on Skeleton-Based Activity Recognition using Graph, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/355115703\_A\_Survey\_on\_Skeleton-Based\_Activity\_Recognition\_using\_Graph\_Convolutional\_Networks\_GCN](https://www.researchgate.net/publication/355115703_A_Survey_on_Skeleton-Based_Activity_Recognition_using_Graph_Convolutional_Networks_GCN)  
22. Graph Convolutional Network with Multi-View Topology for ... \- MDPI, 访问时间为 五月 5, 2026， [https://www.mdpi.com/2073-8994/17/8/1235](https://www.mdpi.com/2073-8994/17/8/1235)  
23. Part-Level Graph Convolutional Network for Skeleton-Based Action, 访问时间为 五月 5, 2026， [https://ojs.aaai.org/index.php/AAAI/article/view/6759/6613](https://ojs.aaai.org/index.php/AAAI/article/view/6759/6613)  
24. A Topology-Aware Graph Convolutional Network for Human Pose, 访问时间为 五月 5, 2026， [https://arxiv.org/html/2511.01194v1](https://arxiv.org/html/2511.01194v1)  
25. A Topology-Aware Graph Convolutional Network for Human Pose, 访问时间为 五月 5, 2026， [https://arxiv.org/abs/2511.01194](https://arxiv.org/abs/2511.01194)  
26. Enhancing Long-Term Action Quality Assessment: A Dual-Modality, 访问时间为 五月 5, 2026， [https://www.mdpi.com/1424-8220/25/18/5824](https://www.mdpi.com/1424-8220/25/18/5824)  
27. Group-aware Contrastive Regression for Action Quality Assessment, 访问时间为 五月 5, 2026， [https://arxiv.org/abs/2108.07797](https://arxiv.org/abs/2108.07797)  
28. Pairwise Contrastive Learning Network for Action Quality Assessment, 访问时间为 五月 5, 2026， [https://www.ecva.net/papers/eccv\_2022/papers\_ECCV/papers/136640450.pdf](https://www.ecva.net/papers/eccv_2022/papers_ECCV/papers/136640450.pdf)  
29. Action Quality Assessment with Temporal Parsing Transformer, 访问时间为 五月 5, 2026， [https://www.ecva.net/papers/eccv\_2022/papers\_ECCV/papers/136640416.pdf](https://www.ecva.net/papers/eccv_2022/papers_ECCV/papers/136640416.pdf)  
30. Multi-Stage Contrastive Regression for Action Quality Assessment, 访问时间为 五月 5, 2026， [https://arxiv.org/html/2401.02841v1](https://arxiv.org/html/2401.02841v1)  
31. Uncertainty-Aware Score Distribution Learning for Action Quality, 访问时间为 五月 5, 2026， [https://www.semanticscholar.org/paper/Uncertainty-Aware-Score-Distribution-Learning-for-Tang-Ni/d47e67c04e884cc83fff781ee9157f07acc0a558](https://www.semanticscholar.org/paper/Uncertainty-Aware-Score-Distribution-Learning-for-Tang-Ni/d47e67c04e884cc83fff781ee9157f07acc0a558)  
32. Uncertainty-Aware Score Distribution Learning for Action Quality, 访问时间为 五月 5, 2026， [https://openaccess.thecvf.com/content\_CVPR\_2020/papers/Tang\_Uncertainty-Aware\_Score\_Distribution\_Learning\_for\_Action\_Quality\_Assessment\_CVPR\_2020\_paper.pdf](https://openaccess.thecvf.com/content_CVPR_2020/papers/Tang_Uncertainty-Aware_Score_Distribution_Learning_for_Action_Quality_Assessment_CVPR_2020_paper.pdf)  
33. Uncertainty-Driven Action Quality Assessment \- OpenReview, 访问时间为 五月 5, 2026， [https://openreview.net/attachment?id=V0uty-KZjaU\&name=pdf](https://openreview.net/attachment?id=V0uty-KZjaU&name=pdf)  
34. A large‐scale benchmark for comprehensive evaluation on human, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/381009317\_SkatingVerse\_A\_large-scale\_benchmark\_for\_comprehensive\_evaluation\_on\_human\_action\_understanding](https://www.researchgate.net/publication/381009317_SkatingVerse_A_large-scale_benchmark_for_comprehensive_evaluation_on_human_action_understanding)  
35. A Multi-modality and Multi-task Dataset of Figure Skating, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/372162590\_Fine-grained\_Action\_Analysis\_A\_Multi-modality\_and\_Multi-task\_Dataset\_of\_Figure\_Skating](https://www.researchgate.net/publication/372162590_Fine-grained_Action_Analysis_A_Multi-modality_and_Multi-task_Dataset_of_Figure_Skating)  
36. xiaobai1217/Awesome-Video-Datasets \- GitHub, 访问时间为 五月 5, 2026， [https://github.com/xiaobai1217/Awesome-Video-Datasets](https://github.com/xiaobai1217/Awesome-Video-Datasets)  
37. CVonline: Image Databases \- Informatics Homepages Server, 访问时间为 五月 5, 2026， [https://homepages.inf.ed.ac.uk/rbf/CVonline/Imagedbase.htm](https://homepages.inf.ed.ac.uk/rbf/CVonline/Imagedbase.htm)  
38. Diving Video Analysis Using Multitask Learning Action Quality, 访问时间为 五月 5, 2026， [https://oasis.library.unlv.edu/cgi/viewcontent.cgi?article=1295\&context=durep\_posters](https://oasis.library.unlv.edu/cgi/viewcontent.cgi?article=1295&context=durep_posters)  
39. FineSports: A Multi-person Hierarchical Sports Video Dataset for, 访问时间为 五月 5, 2026， [https://openaccess.thecvf.com/content/CVPR2024/papers/Xu\_FineSports\_A\_Multi-person\_Hierarchical\_Sports\_Video\_Dataset\_for\_Fine-grained\_Action\_CVPR\_2024\_paper.pdf](https://openaccess.thecvf.com/content/CVPR2024/papers/Xu_FineSports_A_Multi-person_Hierarchical_Sports_Video_Dataset_for_Fine-grained_Action_CVPR_2024_paper.pdf)  
40. Deep Learning in Sports: A Comprehensive Survey | PDF \- Scribd, 访问时间为 五月 5, 2026， [https://www.scribd.com/document/733456652/A-Survey-of-Deep-Learning-in-Sports-Applications](https://www.scribd.com/document/733456652/A-Survey-of-Deep-Learning-in-Sports-Applications)  
41. Action recognition results on the Diving-48 dataset. We compare, 访问时间为 五月 5, 2026， [https://www.researchgate.net/figure/Action-recognition-results-on-the-Diving-48-dataset-We-compare-different-Top-1\_tbl2\_377325584](https://www.researchgate.net/figure/Action-recognition-results-on-the-Diving-48-dataset-We-compare-different-Top-1_tbl2_377325584)  
42. FineDiving: A Fine-grained Dataset for Procedure-aware Action, 访问时间为 五月 5, 2026， [https://github.com/xujinglin/finediving](https://github.com/xujinglin/finediving)  
43. FineDiving: A Fine-grained Dataset for Procedure-aware Action, 访问时间为 五月 5, 2026， [https://arxiv.org/abs/2204.03646](https://arxiv.org/abs/2204.03646)  
44. Group-aware Contrastive Regression for Action Quality Assessment, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/359005148\_Group-aware\_Contrastive\_Regression\_for\_Action\_Quality\_Assessment](https://www.researchgate.net/publication/359005148_Group-aware_Contrastive_Regression_for_Action_Quality_Assessment)  
45. AI in Sustainable Pig Farming: IoT Insights into Stress and Gait, 访问时间为 五月 5, 2026， [https://www.preprints.org/manuscript/202307.1059](https://www.preprints.org/manuscript/202307.1059)  
46. A Review of Posture Detection Methods for Pigs Using Deep Learning, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/371530352\_A\_Review\_of\_Posture\_Detection\_Methods\_for\_Pigs\_Using\_Deep\_Learning](https://www.researchgate.net/publication/371530352_A_Review_of_Posture_Detection_Methods_for_Pigs_Using_Deep_Learning)  
47. A Large Multi-Object Tracking Dataset in Multiple Sports Scenes, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/377426986\_SportsMOT\_A\_Large\_Multi-Object\_Tracking\_Dataset\_in\_Multiple\_Sports\_Scenes](https://www.researchgate.net/publication/377426986_SportsMOT_A_Large_Multi-Object_Tracking_Dataset_in_Multiple_Sports_Scenes)  
48. VIDEO ACTION DIFFERENCING \- ICLR Proceedings, 访问时间为 五月 5, 2026， [https://proceedings.iclr.cc/paper\_files/paper/2025/file/26f62bf3b74cda621831f550c6ee2dce-Paper-Conference.pdf](https://proceedings.iclr.cc/paper_files/paper/2025/file/26f62bf3b74cda621831f550c6ee2dce-Paper-Conference.pdf)  
49. (PDF) Towards Improved and Interpretable Action Quality, 访问时间为 五月 5, 2026， [https://www.researchgate.net/publication/355117393\_Towards\_Improved\_and\_Interpretable\_Action\_Quality\_Assessment\_with\_Self-Supervised\_Alignment](https://www.researchgate.net/publication/355117393_Towards_Improved_and_Interpretable_Action_Quality_Assessment_with_Self-Supervised_Alignment)  
50. Contrastive Self-Supervised Pre-Training for Video Quality, 访问时间为 五月 5, 2026， [https://pubmed.ncbi.nlm.nih.gov/34874856/](https://pubmed.ncbi.nlm.nih.gov/34874856/)  
51. Lightweight Domestic Pig Behavior Detection Based on YOLOv8, 访问时间为 五月 5, 2026， [https://www.mdpi.com/2076-3417/15/11/6340](https://www.mdpi.com/2076-3417/15/11/6340)

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAZCAYAAADTyxWqAAABPklEQVR4XuWUvUpDQRBGR1BRjH8QFCGiiI1YiYXPYKGFleADaG0hWFnkGUIgCGJhI/gCVoKNoI2FWGtjZamd6Pc5E5gdLxf3kkLwwIHdmWWyO7m7Iv+KkTAfMj0TcMrNB9044bPArWSFyK3L0f00nTIjuug6Jhx7cC4GixgVLfYUE8Y8fIzBMljsPQaNZ7gSg2V0+xEZh4ewLybKKCq2LLqrbN4kLTYAz2HbxX7Ng2ixus3vRf/BSlyJFmuI9ucS1vyCHE5Fi63CNbMy/KpZ7E70iGWsw1lzOOS+2RAt9gE3Q64L73EL7sALeAMnkxXGEnyFizHh4A9t25jt2HW5BDb7KAYdfEle4ILNuTsWrAQfg2Mb82h8SbJuhYeNPhEtcCA/b0s2vBXTojvkR94TeMSzGMxlDDZhx+xP03+RLw7fOWgNjs4sAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAZCAYAAADTyxWqAAABIklEQVR4XmNgGFGAG43PAcXIQACIxZD4bEhsFPAfCw5CUcHAcBpJDoSLUKVRgSQDRNFhdAkkkAHE8uiC2AAvA8Swh+gSUKAAxNfRBfEBkGHf0AWh4BEQG6IL4gOw8EAH/EBcCcSM6BL4ADbDtBkgriIZfGVANYwViFcB8XQkMaLBVQaIYSJQ/kUGSAySBQ4wQAyTYYCEzy4g5kFWQApYyAAxzBiIzaGYbABK1SDDzjBAvIgPOACxL7ogMgBJggz7C8R+aHIwAMrHk4A4GYjjGCCxjRVoAvFbIFZBl0AC/4A4ggFSEIAMA6VBrAAU2PXogkgA5CqQy58B8VwgDkGVJg2A8i8okpABzqKIGAAqigShbCEgtkGSIwuACkr0wnMUkAEAULgzDzEjcJkAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAaCAYAAABVX2cEAAABRklEQVR4Xu2UvytFYRjHv5IySFYMZLilJJNSslssFmVUrsHComwGidEoq8H/YLhMJlGKRFFiks2g/Ph+e9577nufzrnOue5g8KlPnfM873nO8/7oBf5pFUf0iy7QDXpJ5+ge7YrG/cgYvaOjUawdVlzmZgnZH2zSFx9sRAXZxdZg+dw8wIoN+wTpoZ0+2IhT1NbmGlZU69UUs/QDtYJS79vxoKKoo3XUF52oG9EE3fQAVmzV5TJRJ+c+GOiFbcy8T2QxQ+99MDBEn5G+w6lUYFNpc/EBehOMKdEtukjPYAc6QSf7k67Qjih+RV/peBSr/qD6YzWhmSXc0hH6DjsKh7Bpn9DBZJSxTx+jdzWSewk86qQcnvvpbpQrjIpNh+dJuCkWZZle0GP6hIL3Wxoq0Ae7936NdnyHvqEFnekW1hUup1zuD/INbWZDjMcENgsAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAZCAYAAADTyxWqAAABPUlEQVR4Xu2ULUtEQRSGXxHBL1QQFJOGRRDEP+BvMBkUtFk2K2I2GHbjRptBRIxWw2IyGkxiUBBMFoNBEH1fzoyMhzuX2a36wMPemTMfZ2fuucCfZoAu0i49oXuhf4zOhOdirugX3aGH9I5u0mM6noyrZZJe04YPkBvYBsU0kZ9wRF99Zx1PyC/WCRYTF1vyATJFh31nHev0E7ZgVO1WOqgUvRIb+L1YdCgZ1xcT9BS22K6LZdGuq74zoPPTLR74QA5NuPWdgTnYxWz5QA5d+YvvDGzD/qbOM2WWrqHiLLuonjBP74MpqtszWIld0JE0+ECX6QfsVTinj7DSWvgZZVzS5/Cs7PbhklgJv6pNpa6C1pfCZyreYJuqVtuoHlPMO2zDyCgdTNo9oYxU9BGd3XTS7hlloo9k3xn9Y3wDOwE7NIZ6YDUAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHUAAAAZCAYAAAAPMmGdAAAD8ElEQVR4Xu2ZTahUZRjH/1FCYVqZVOLijhJCVKvUCIpQ/FpECCoFmQYtDHHVIlEEr0iLWoRYIogbFyJIhNDCiKBLgvi1LIJQ0BDCQt1kcC/08fx8zplz7jNn5s6xmXMGPD94uHee9513nvN/3u8jNTQ0NDSMIP+YHYjOPphttsnsG7ODZg8m/nfbNerlSbNTib1k9oA8thX5SiMCOh6X67gq8a3MisvxiNm/Zn/EghlAoCmz383eNzthdtpsjdmbuXp1sUf+XLvNtss77mH5c5LgUQEdt8h1/ESuI7qi461cvb4hoYyyO3IBnp9e3JW35CIRUJ635e3MCf4qIaZdZptjgTEuHw0x7jpBx/3RKdfxTHT2wzp5bzgqb4TG+3ngH81+i05jsYr9VULHZDQ+EQuMV822RWeNPCXXC90i+JmKS3Pb7BWzh+TDPk3sTFDv5+g0njP7KTor5pg8PmahCGvV/OiskZ1yHZ+OBXId0bMUs+SJJKFAchHjertGd6iH0UYePs8LvqpJk/qeOmedh8PnuiGpxDoe/ICOMf6eIP5JZQlNmZT/yKLgj3ynLLGpFY3cIgh0QUkrw5jZL+qMr5WrMyo8ps440XFZvlK/MCrZbUVINA3Tg3rxjNlX6gxoeb5SF+aa/VrSyrJEnbGR6LF8pRGhSEeWxdKQ0NeiU74OfS1veEco68VaeSBXVLw+1AlnZ45cPBMzzL3ymdmRPu2d5DtlYRZDR2ItpSNTLokr2kjAanmjRdMpPf3F6Exgp3ZN5afLQdJtJLKW8kwTwV+GQSe1l47EWkpHDt4krhvpZQQW4Qc/js4EpuyLKj5KVAGJ6yUmz/NldNZILx2JtW8dXzC7EZ0FcCFBwx8E/00VB8N13AV5+zMxrI0Sx5VuSX1WxUcHNiqsv8TEWl8VHFWKdAR0LFpTW2brlV3FtvlC/oU4XUS7JE8qP0DCUvCxbkY+UvENU5XQw3+QJyoPO32eaUPwE/M5+cXLtyq+ABgWXKOiYyv40Q8diS3P62afm22ULwPTdE6n1TLGg8Oj8vNUurPkJoqNx5R8U9XRgypkoXwkvmx2WR4fl/jn5ZvCN7Kqd2EJGk/+57tX2yXDBx2/l+t4VtN1/EudOnJ7Rw5IJDNO7Jz/i8eVXTYwfdCbWGO5aqyblrKHJcal8tH5afI5QtxcGQJ/J3NlwwYdtyb/k0B0JFZ0LLocoYP+KU/8vlDWkMBImZC/NgQ2T3weVXjZkn+rRNx1LnMjCYKwtyC58Lfu8eK8Iri2TTeAxH5I3Y+j9z28ISGx7OhLX5xXDEsIG9ZmhPYBFzG8s6zz/W/DAPlQ2RFubyhraGhoGBD/AfKL879XU06UAAAAAElFTkSuQmCC>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAAwCAYAAACsRiaAAAADq0lEQVR4Xu3dzatuUxwH8CUUXXkpdVNKRpRbBoqEme6MZEKZmZCMCEP8AzKUSGYGMhODOzh1hwYykLoZIGWE3FDyur7tvZ111j17n+e83nvu8/nUt2c9a+1zOp3Rr7XXSykAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAXFwP1Xxcc0U/AADA7qWo+rfmzppbtslz43iy5Lqax2quL8PPxSubwwAA7OTpmq/H3NCN3Vbzd9fXO9N3dG4sW4u6+2qubr4DALDg9poHx/bPNY82Y5MUZCm4ll5jPtB3dM6On2/VXFlzuhkDAGDBRs01ZXhtuSQF27m+cwUpzE7WPF6Ggu/7mu/KZpEIAMAOUoilmIqvyvYzbJPfau7tO2dkVu7Zsf1lOwAAwO6kYLtqbL9fc74Z611btj6/pF2z9mfTBgBglzaadgq2zKLNyUaBflPCnGkjws1lKN6mV64v1DwztgEAWMGpMmw8iM9qXmzGej/0HQtS/MUHZfi9CQAAe5QZsHfLUFzNyfq2pV2i/aaFHBGSDQYnav4pw/EgmaFTuAEAx9obZTjzbKeDaI9airUlT/QdC1LEAQAca7kJ4FIq2JZ2hWbG7Jeyu7/34ZrX+k4AgMOSA2Dz6u/3fmAfLrWC7aDl1gMAgCORoy2mwirrt55sxiLnmeWA2LnMudwLNgCAI/NjzT1jO6f2H9Rl5ksFWxb9vz2TrH8DAKDR3q35YbnwfDIzbAAAF1mKqhxV8XoZLjQ/CHm1+lIZfvetZVgjBwDAHuT0/qfKsIB+6Wyy4yKzg3mtupOPal6t+aQfOCD5X6YIBgDYt3fK5VGoTT7tO2ZMl7gvXV+1ql9r/qp5pOt/s2yuDQQA2LMv+o5jLBe6T1dTrSKzcef6zj1KYdYXbHkt7HBdAGDtpDCaNkWcbgfKhRexZ91c++zJZizOdt/3Y7uCLWy6AADWyvTqtt3V2hZE02vOyPjzZRh/eezb+H90OCQ4O1x/avr2Y65gc98oALBWMmN2U9lapJ1v2t827TybtM+uul4tu1r7s+GmZIPGduYKto2+AwDgcvfemDhVc38Z1opFO8MWmWVrn/2jbD570OYKNjNsAMDayYzatPPyzPj5+fiZHa+tu8vWZ3ODw/TsQZsr2NoZQACAtXBH9729gD0bDNpdoieaduTmhaOU2bxv+k4AgHW36jlsRyHnsN3VdwIArLtVbzo4bG46AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA7Jf0o5j4ymPbrlAAAAAElFTkSuQmCC>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAYCAYAAADzoH0MAAABCElEQVR4XmNgGNaAEYglgdgMiM3R5IgCmkD8H4qL0OSIBp4MEAOk0SWIBXsYIAaQDZ4D8T90QXzgOBD/BOI7QHyFAWL7VhQVeIA3EB8CYgkg5gTiNQwQA6qQFeECpxggikFRBwMuQPwEiGWQxHACkObfaGKtDBDnc6CJg1wIwnAASiwgA5YiiSkxQAIQ5Ap0IM+A6lK4AciJBaQRFPogg0CKWZHkMAAPA8QAXySxh1AxEIgGYhYg5gfiCiB+ABVHASDF86FsUwaEASDDd0HF0xkgKfIBlI8BQIElBsTMUD7I2cIMqP5dDsQnkPgkg7dAnIMuSCzgYoAkLhCtjSZHNBBggITLKEACAOwELBUqi0pdAAAAAElFTkSuQmCC>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAYCAYAAADOMhxqAAAAuUlEQVR4XmNgGAWDCVwA4mNAvAeI+YH4KhA/AOJfQPwRoQwCGIG4GYhZgfg/EG8HYieouAcQ/wNiYbhqIHABYh4glmaAaMhAkwNpkEQSY+CA0jZA/ByIlZDkyhkghnAjiYEBCxCvAeJWNPH3QPwTTQwMQM55AMR+aOIg008wQPwjhCxRBJUE+QUG9IH4KxAbA3EOEC9HkgM7B6QBGWgC8UMGiIdvAbEZsqQUEMsgC0CBAANaCI0CmgAAhrgePACZnxMAAAAASUVORK5CYII=>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAZCAYAAADe1WXtAAABDUlEQVR4XmNgGAVIgAOIBdAFgYAHXYAUMAmI/2PBc5AVkQt+M0AMoyqAuZBqQIQBYuBWdAlKgCkDxNBWdAlyAQsQrwHi50CshCZHNgB5/SoDxOug5EUVkM4A8boLugQUCAOxGJQtCcS1QNwIxCpwFVgAyOsgQ3F53YMBkRF2A7EtA8RXID2MMEXoAOR1kAJudAkGSHgvh7I5gTgJiJkZIK4/BcTGUDkMgCt98gPxHgZEMmMF4p0MkCAAgXIoxgAgl4AMBMU8OgAZCJKLRpdgQKQYB2RBNQbUfI4Lg7wOMgAdgCxcjC5ICQAFQx8DJIypAoqBuBCJH4TEJguAks83IH4CxI+g2AZFxSgY/AAAWP44Ee7Raa8AAAAASUVORK5CYII=>