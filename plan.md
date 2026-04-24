# CS2916 Presentation Plan: 自我提升与自我进化算法

> **课程**: CS2916 大语言模型 | **时长**: 70 分钟 | **人数**: 4 人

---

## 一、核心叙事线

整场演讲围绕一个核心类比展开：

> **LLM 的成长路径，就像一个学生从"跟老师学"到"自己学"的过程。**
>
> Pre-training = 读教科书；SFT/RLHF = 老师批改作业；
> **Self-Improving = 自己出题、自己做、自己批、自己改。**

四位成员依次回答四个递进问题：
1. **为什么要自我提升？**（动机、背景与全景）
2. **怎么自己学？**（核心自我提升方法）
3. **怎么自己进化？**（自我博弈、AI 反馈与进化算法）
4. **能学到什么程度？有什么风险？**（理论边界、挑战与未来）

---

## 二、详细大纲

### Part 1：开场 —— 为什么 LLM 需要"自学"？（成员 A，约 15 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 0:00-2:00 | **Hook：互动投票** | 用举手/弹幕提问："你觉得 GPT-4 级别的模型背后需要多少人工标注数据？" 给出选项 A/B/C/D，揭晓答案后引出人工标注的高成本问题 |
| 2:00-4:30 | **瓶颈：人工标注不可持续** | 数据标注成本曲线（配图表）；RLHF 需要大量人类偏好数据；高质量标注者本身就很稀缺（"你去哪找 10 万个数学博士来标数据？"——幽默点）；展示 InstructGPT 的标注团队规模 vs 成果，量化说明人工标注的投入产出比 |
| 4:30-7:30 | **LLM 训练范式的演进** | 用 timeline 图梳理 LLM 训练的四个阶段：<br>**① Pre-training（大规模无监督）**→ GPT-1/2/3<br>**② SFT（监督微调）**→ InstructGPT, Alpaca<br>**③ RLHF（人类反馈强化学习）**→ ChatGPT, Claude<br>**④ Self-Improvement（自我提升）**→ 本次演讲的重点<br>每个阶段用一句话 + 一个代表性模型说明，重点讲清楚为什么每一步都是对前一步"瓶颈"的回应。引出"Data Wall"问题：互联网高质量文本即将被用尽，模型需要新的数据来源 |
| 7:30-10:00 | **核心思想：Bootstrap（自举）** | 用学生自学的类比讲清楚"用自己的输出来训练自己"这个看似矛盾的想法；引用 Baron Munchausen 自己拽头发把自己从沼泽里拉出来的故事（Self-improvement 文献中的经典典故）；深入解释 Bootstrap 为什么可行：**模型在"生成"和"判别"上的能力不对称**——批改作业往往比写作业简单，这是自我提升的认知基础 |
| 10:00-13:00 | **全景地图：分类框架** | 展示一张精心设计的分类图，把接下来要讲的所有方法放在统一框架中：<br>- **自我提升（Self-Improving）**：Self-Instruct, STaR, V-STaR, Self-Refine, ReST<br>- **自我博弈与 AI 反馈（Self-Play & AI Feedback）**：SPIN, Self-Rewarding LM, Constitutional AI, LLM Debate<br>- **自我进化（Self-Evolving）**：FunSearch, EvoPrompt, OpenELM<br>清晰定义三者的区别和联系；用一张表从"是否需要外部数据""是否需要微调""反馈来源"三个维度做对比 |
| 13:00-15:00 | **关键问题预告 + 过渡** | "一个模型真的能通过自己训练自己变得更强吗？这难道不是'自己批改自己的作业'吗？带着这个疑问，让我们来看看具体的方法。"<br>回顾全景图，高亮 Part 2 即将涉及的区域。交棒给成员 B |

**成员 A 需要准备的材料**：
- 一张人工标注成本的信息图
- LLM 训练范式 timeline 图（Pre-training → SFT → RLHF → Self-Improvement）
- 自举（Bootstrap）的直觉动画/示意图
- 方法全景分类图（这张图贯穿全场，后续成员讲到哪个方法就高亮哪个位置）
- "生成 vs 判别"能力不对称的示意图
- 互动投票的选项设计

---

### Part 2：核心方法 —— 自己出题，自己做，自己改（成员 B，约 19 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 15:00-18:30 | **Self-Instruct：自己给自己出题** | 核心流程图：Seed Tasks → LLM 生成 Instruction → LLM 生成 Response → 过滤 → 微调<br>关键论文：Wang et al., 2022 "Self-Instruct"<br>强调"种子任务"的重要性——你不是从零开始，你需要一点火种<br>**深入分析**：展示 Self-Instruct 的数据质量控制流程——ROUGE 相似度过滤、分类过滤等；量化结果：仅用 175 个 seed tasks 就生成了 52K 条高质量指令数据；GPT-3 + Self-Instruct ≈ InstructGPT 001 的效果，说明这条路可行 |
| 18:30-23:00 | **STaR & V-STaR：自举推理能力** | **STaR 核心思想**：让模型先尝试解题，答对了的 rationale 拿来当训练数据<br>关键创新：rationalization——答错的题，给它看答案，让它"事后解释"，这个解释也能拿来训练<br>论文：Zelikman et al., 2022 "STaR: Bootstrapping Reasoning With Reasoning"<br>用一个具体的数学推理例子走一遍完整的 STaR 流程（展示 inner loop）<br>（幽默点："就像考试不会做的题，看了答案之后觉得'啊我本来也想到了'——STaR 就是把这个过程形式化了"）<br><br>**V-STaR 扩展**（重要补充）：STaR 的问题是只用"做对的题"来训练，浪费了大量"做错的题"的信息<br>V-STaR 的解法：训练一个 **Verifier（验证器）**，用做对和做错的数据一起训练验证器，然后用验证器来指导生成<br>论文：Hosseini et al., 2024 "V-STaR: Training Verifiers for Self-Taught Reasoners"<br>对比图：STaR vs V-STaR 的数据利用效率 |
| 23:00-26:00 | **Self-Refine：迭代自我批评与修改** | 三步循环：Generate → Critique → Refine<br>关键点：不需要微调，纯 prompting<br>论文：Madaan et al., 2023 "Self-Refine"<br>**深入分析**：展示 Self-Refine 在不同任务上的改进幅度（代码优化、数学推理、对话生成等），讨论其局限性——多轮 refine 之后是否会收敛？什么时候该停？<br>**现场互动**：请一位同学给出一段"很烂的代码"（或者提前准备），现场用 ChatGPT 演示 Self-Refine 过程：先写→自评→改进，让观众直观感受迭代改进的效果 |
| 26:00-30:00 | **ReST / ReST^EM：强化自训练** | 核心思想：Generate（采样多个解）→ Improve（用奖励模型筛选）→ Train（用好的样本微调）<br>与 STaR 的区别：引入了显式的奖励信号，更接近 RL 的框架<br>论文：Gulcehre et al., 2023; Singh et al., 2023<br>**深入技术细节**：解释 ReST^EM 的 E-step（生成+筛选）和 M-step（微调）的类比，说明为什么这比简单的 rejection sampling 更有效<br>**"Beyond Human Data" 的突破**：Singh et al. 2023 的关键发现——在数学和代码任务上，模型通过 ReST^EM 可以超越人类标注数据的训练效果<br><br>**综合对比**（重要总结）：一张大表对比 Self-Instruct / STaR / V-STaR / Self-Refine / ReST 五种方法<br>对比维度：输入要求、是否需要微调、是否需要奖励模型、反馈来源、适用场景、代表性结果 |

**过渡**（34:00）：
> "到目前为止，我们看到的方法有一个共同特点：模型在 **单向地** 提升自己——自己出题、自己解题、自己改。但如果模型能和自己对弈呢？如果模型能自己当裁判呢？甚至，如果我们用进化的方式让模型群体自己'演化'呢？"

**成员 B 需要准备的材料**：
- 每个方法的流程图（统一视觉风格）
- STaR 的数学推理具体示例（step-by-step）
- STaR vs V-STaR 的对比图（数据利用效率）
- Self-Refine 的现场 demo（提前测试好，确保能跑通）
- ReST^EM 的 E-step / M-step 示意图
- 五种方法的综合对比表
- "Beyond Human Data" 的关键实验结果图

---

### Part 3：自我博弈与自我进化 —— 当模型自己和自己下棋（成员 C，约 19 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 34:00-38:00 | **SPIN：自我博弈微调** | 核心思想：类比 AlphaGo 的自我博弈——主模型 vs. 上一轮的自己<br>目标：让当前模型的输出与人类数据不可区分<br>论文：Chen et al., 2024 "Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models"<br>**深入讲解**：用博弈论的框架理解 SPIN：这是一个两人零和博弈，当且仅当模型分布等于目标分布时达到纳什均衡<br>数学直觉（用图示辅助）：min-max 目标函数的几何含义——主模型尝试让自己和人类数据越来越像，同时区分旧版本的自己和人类<br>关键结果：Zephyr-7B 在 SPIN 后超越了 GPT-4 在部分 benchmark 上的表现<br>讨论：SPIN 需要人类数据作为锚点，这既是优势（保证质量）也是局限（受限于人类数据的天花板） |
| 38:00-42:00 | **Self-Rewarding Language Models：自己当裁判** | 核心突破：不再需要单独的奖励模型（Reward Model），LLM 自己既当选手又当裁判<br>迭代流程：用自己生成 + 自己评分 → 筛选 → 微调 → 新一轮<br>论文：Yuan et al., 2024<br>**深入讲解**：LLM-as-a-Judge 的 prompt 设计——模型如何被引导做出高质量的评判？展示具体的评分 prompt 模板<br>**迭代提升的动态**：展示多轮迭代中模型生成质量和评判质量同时提升的曲线<br>与 RLHF 的关键区别对比图：RLHF = 固定的人类训练的 RM → 天花板；Self-Rewarding = RM 和 Policy 共同进化 → 可能突破天花板<br>（幽默点："这就像你自己给自己的论文打分——听起来很不靠谱，但意外地 work"） |
| 42:00-45:00 | **Constitutional AI：给自我进化加上"宪法"** | 核心思想：用一组人写的原则（Constitution）代替大量人工标注<br>两阶段：Critique → Revision（基于原则的自我批评）→ RLAIF（AI 反馈的强化学习）<br>论文：Bai et al., 2022<br>**具体示例**：展示一条宪法原则的完整应用流程——模型生成 → 根据原则自我批评 → 修订生成内容<br>强调这是 Anthropic 的核心方法论——与安全对齐的紧密联系<br>讨论：RLAIF vs RLHF 的实验对比，说明 AI 反馈可以达到甚至超越人类反馈的效果 |
| 45:00-48:00 | **LLM Debate：多模型对抗式自我提升** | 核心思想：两个（或多个）LLM 互相辩论，通过对抗产生更好的答案<br>论文：Du et al., 2023 "Improving Factuality and Reasoning in Language Models through Multiagent Debate"<br>流程：多个模型独立生成答案 → 互相阅读对方答案 → 辩论（多轮） → 收敛到更准确的答案<br>关键发现：Debate 能显著提升事实准确性和推理能力<br>与 Self-Refine 的区别：Self-Refine 是"自言自语"，Debate 是"互相挑战"——多视角 > 单视角<br>连接 AI Safety：Irving et al., 2018 提出 Debate 作为 AI alignment 的机制 |
| 48:00-53:00 | **进化算法 × LLM：FunSearch, EvoPrompt & OpenELM** | **FunSearch（DeepMind, 2024）**：用 LLM 做进化算法的 mutation operator，发现了超越人类已知最优解的数学构造<br>深入讲解 FunSearch 的完整架构：程序数据库（Programs Database）+ LLM Mutation + 自动评估器<br>关键洞察：搜索的不是"答案"而是"生成答案的程序"——可验证性是关键<br>结果展示：cap set 问题的新下界，bin packing 的新启发式算法<br><br>**EvoPrompt**：用进化算法优化 prompt，将遗传算法 / 差分进化引入 prompt engineering<br>论文：Guo et al., 2023<br><br>**OpenELM（2024）**：进化式 LLM 优化的更新进展——用 MAP-Elites 等质量多样性算法维护多样化的解群体<br><br>核心洞察总结：LLM 不只是被进化的对象，它本身就是进化过程中的"变异引擎"<br><br>**小互动/Quiz**："以下哪个是 FunSearch 真正发现的？" A. 新的排序算法 B. cap set 问题的新下界 C. 新的蛋白质结构 → 揭晓答案并简述 |

**过渡**（53:00）：
> "听到这里，你可能会觉得 Self-Improvement 是一个银弹——模型自己就能越变越强。但事情真的这么美好吗？自我提升有没有天花板？有什么陷阱？"

**成员 C 需要准备的材料**：
- SPIN 的自我博弈对比图（类比 AlphaGo）+ 纳什均衡的直觉图
- Self-Rewarding vs RLHF 的对比图 + 多轮迭代提升曲线
- Constitutional AI 的"宪法原则"完整应用示例
- LLM Debate 的多轮辩论流程图
- FunSearch 的完整架构图 + 结果展示（cap set 问题的图示）
- 互动 Quiz 的设计

---

### Part 4：天花板、陷阱与未来 —— 自我提升的边界在哪？（成员 D，约 17 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 53:00-57:00 | **三大核心挑战** | **1. 模型坍缩（Model Collapse）**：用自己的输出训自己 → 分布越来越窄 → 多样性丧失 → 质量下降<br>配图：Shumailov et al., 2023 的实验曲线——展示多代自训练后分布尾部逐渐消失的过程<br>直觉类比："就像近亲繁殖——基因库越来越小，最终退化"<br><br>**2. 奖励欺骗（Reward Hacking）**：Self-Rewarding 的风险——模型学会给自己"放水"<br>具体例子：模型可能学会生成"看起来正确"但实际错误的推理链，因为自己的评判器也会被欺骗<br>Sycophancy 问题：模型倾向于"讨好"评判者而非追求真正的正确性<br><br>**3. 分布偏移（Distribution Shift）**：生成数据偏离真实数据分布<br>连续自训练的漂移效应：小误差在迭代中被放大<br><br>用一个统一的例子把三个问题串起来：假设一个写作模型自训练——第 1 代还不错，第 5 代风格开始单一，第 10 代只会写一种类型的文章（Model Collapse）；如果加上自我评分，它可能学会给自己偏好的那种风格打高分（Reward Hacking）；最终生成的分布完全偏离了人类真实的写作多样性（Distribution Shift） |
| 57:00-60:00 | **理论视角：自我提升的上界** | 直觉问题："一个 60 分的模型能把自己训练到 90 分吗？"<br><br>**关键理论 1**：Huang et al., 2022 的发现——模型规模足够大时，Self-Consistency + CoT 就能自我提升<br>但有条件：需要 **足够的多样性** 和 **可靠的验证信号**<br><br>**关键理论 2**："生成-判别"能力差（回应 Part 1 的铺垫）<br>自我提升的本质：利用模型的判别能力（验证答案对错）来提升生成能力（产出好答案）<br>上界推论：当生成能力追上判别能力时，自我提升到达瓶颈<br><br>**Scaling 的视角**：三种 scaling 范式的对比<br>① Data Scaling：更多人类数据 → 遇到 Data Wall<br>② Compute Scaling：更大模型 → 遇到成本墙<br>③ Self-Improvement Scaling：自己生成数据 → 遇到 Model Collapse<br>三者的互补关系：未来可能需要三种 scaling 的组合 |
| 60:00-64:00 | **前沿展望（2024-2025）** | **1. Test-Time Compute Scaling（推理时自我提升）**<br>o1/o3/DeepSeek-R1 的核心思路：不只在训练时提升，推理时也能"自我提升"<br>具体机制：长链推理（Chain-of-Thought）+ 搜索（Tree Search）+ 自我验证<br>关键论文：Snell et al., 2024 "Scaling LLM Test-Time Compute"<br>核心发现：推理时的额外计算可以替代部分训练计算，小模型+更多推理时间 ≈ 大模型+少量推理时间<br><br>**2. Multi-Agent Self-Improvement（多智能体协同提升）**<br>多个模型互相提升：Debate、Collaboration、Specialization<br>未来方向：异构 agent 组合——一个擅长生成，一个擅长验证，一个擅长规划<br><br>**3. Self-Improving Agents（自我提升的智能体）**<br>不只是语言能力，而是在环境中行动并从反馈中学习<br>Voyager (Wang et al., 2023)：在 Minecraft 中自我提升的 agent，自动发现技能并积累技能库<br>从"语言模型的自我提升"到"Agent 的自我进化"<br><br>**4. 与 AI Safety 的关系**<br>递归自我提升（Recursive Self-Improvement）是 AI 安全研究的核心关切之一<br>"如果一个 AI 能反复改进自己，我们如何确保它改进的方向是人类想要的？"<br>Constitutional AI 的思路：用人类定义的原则约束自我进化的方向 |
| 64:00-67:00 | **总结与回顾** | 回到开场的全景地图，把所有方法点亮<br><br>**分层总结**：<br>Part 1 → "LLM 需要自我提升，因为人工标注不可持续"<br>Part 2 → "核心方法：自己出题（Self-Instruct）、自己推理（STaR）、自己改进（Self-Refine）、自己筛选（ReST）"<br>Part 3 → "进阶方法：自我博弈（SPIN）、自我评判（Self-Rewarding）、原则约束（Constitutional AI）、进化搜索（FunSearch）"<br>Part 4 → "自我提升有天花板（Model Collapse、Reward Hacking），但前沿方向正在突破这些限制"<br><br>回答开场的问题："模型真的能自己批改自己的作业吗？" → "可以，但需要精心设计的机制来避免'自欺欺人'。关键在于：**可靠的验证信号**、**足够的多样性**、以及**人类设定的原则约束**。"<br><br>留一个开放性思考："如果模型真的能无限自我提升，那人类标注者的角色会变成什么？从数据标注者变成原则制定者——从'做具体的事'到'定义什么是对的'。" |
| 67:00-70:00 | **Q&A** | 抛出 2-3 个引导性问题帮助观众提问：<br>1. "你觉得 Self-Improvement 最终会让人类标注者彻底失业吗？"<br>2. "如果你有一个 7B 的模型，你会优先用哪种 Self-Improvement 方法？"<br>3. "递归自我提升的安全风险，你觉得应该怎么应对？"<br>如时间允许，回答 2-3 个问题 |

**成员 D 需要准备的材料**：
- Model Collapse 的实验曲线图 + 直觉类比示意图
- 三种 Scaling 范式的对比图
- "生成-判别能力差"的理论示意图
- Test-Time Compute 的核心图（小模型+多推理 vs 大模型+少推理的效果曲线）
- Voyager Agent 的架构示意图
- 前沿方向的 timeline 图
- 全场总结的回顾 slide（重新展示全景图并高亮所有已讲内容）
- Q&A 预备答案

---

## 三、成员分工明细

### 成员 A：开场与背景（负责 Part 1）

**演讲内容**：0:00 - 15:00（约 15 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 负责开场 Hook（互动投票）、瓶颈分析、LLM 训练范式演进、Bootstrap 核心思想、全景分类图讲解 |
| **PPT 制作** | 制作 Part 1 的 slides（约 15-18 页）；**重点制作全景分类图**（这张图被全场复用，需要美观清晰）+ LLM 训练范式 timeline |
| **内容研究** | 调研人工标注成本数据（InstructGPT, RLHF 的标注规模）；Data Wall 问题的数据支撑；整理 Self-Improving 领域的综述性材料 |
| **全场职责** | 负责 **统一 PPT 风格模板**（配色、字体、排版），确保四位成员的 slides 视觉一致 |

**关键论文/资料**：
- Ouyang et al., 2022 "Training language models to follow instructions with human feedback"（InstructGPT，了解 RLHF 的标注成本）
- Villalobos et al., 2022 "Will we run out of data? An analysis of the limits of scaling datasets in Machine Learning"（Data Wall）
- 一篇 Self-Improvement 的 survey paper 作为全景框架参考

---

### 成员 B：核心自我提升方法（负责 Part 2）

**演讲内容**：15:00 - 34:00（约 19 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 讲解 Self-Instruct、STaR + V-STaR、Self-Refine、ReST 五种方法；负责现场 Self-Refine demo |
| **PPT 制作** | 制作 Part 2 的 slides（约 18-22 页）；每种方法一张清晰的流程图；STaR vs V-STaR 对比图 |
| **内容研究** | 深读五篇核心论文，提取核心算法流程和关键实验结果 |
| **Demo 准备** | 准备 Self-Refine 的现场演示（可以用 ChatGPT/Claude API，提前准备好 fallback 截图以防网络问题） |
| **全场职责** | 制作 **五种方法的对比表**（输入、是否需要微调、是否需要奖励模型、反馈来源、适用场景等维度） |

**关键论文**：
- Wang et al., 2022 "Self-Instruct: Aligning Language Models with Self-Generated Instructions"
- Zelikman et al., 2022 "STaR: Bootstrapping Reasoning With Reasoning"
- Hosseini et al., 2024 "V-STaR: Training Verifiers for Self-Taught Reasoners"
- Madaan et al., 2023 "Self-Refine: Iterative Refinement with Self-Feedback"
- Gulcehre et al., 2023 "Reinforced Self-Training (ReST) for Language Modeling"
- Singh et al., 2023 "Beyond Human Data: Scaling Self-Training for Problem-Solving with Language Models"

---

### 成员 C：自我博弈与自我进化（负责 Part 3）

**演讲内容**：34:00 - 53:00（约 19 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 讲解 SPIN、Self-Rewarding LM、Constitutional AI、LLM Debate、FunSearch/EvoPrompt/OpenELM；负责 Quiz 互动 |
| **PPT 制作** | 制作 Part 3 的 slides（约 18-22 页）；SPIN 的博弈示意图、Self-Rewarding vs RLHF 对比图、Debate 流程图 |
| **内容研究** | 深读相关论文，特别理解 SPIN 的博弈论直觉、Constitutional AI 的设计哲学、Debate 的 alignment 意义 |
| **互动设计** | 设计 FunSearch 相关的 Quiz 问题 |
| **全场职责** | 负责收集和整理所有引用的 **参考文献列表**，放在最后的 References slide 中 |

**关键论文**：
- Chen et al., 2024 "Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models"
- Yuan et al., 2024 "Self-Rewarding Language Models"
- Bai et al., 2022 "Constitutional AI: Harmlessness from AI Feedback"
- Du et al., 2023 "Improving Factuality and Reasoning in Language Models through Multiagent Debate"
- Irving et al., 2018 "AI safety via debate"
- Romera-Paredes et al., 2024 "Mathematical discoveries from program search with large language models"（FunSearch）
- Guo et al., 2023 "Connecting Large Language Models with Evolutionary Algorithms Yields Powerful Prompt Optimizers"（EvoPrompt）
- Lehman et al., 2024 "Evolution through Large Models"（OpenELM）

---

### 成员 D：挑战、理论与未来（负责 Part 4）

**演讲内容**：53:00 - 70:00（约 17 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 讲解三大挑战、理论上界、前沿展望（含 Test-Time Compute 和 Agent）、全场总结；引导 Q&A |
| **PPT 制作** | 制作 Part 4 的 slides（约 15-18 页）；Model Collapse 实验图、三种 Scaling 对比图、总结回顾 slide |
| **内容研究** | 深读 Model Collapse、理论分析、Test-Time Compute、Self-Improving Agent 等相关论文 |
| **Q&A 准备** | 提前准备 8-10 个可能的 Q&A 问题及简要回答，与全组共享 |
| **全场职责** | 负责 **排练统筹**（组织至少 2 次完整排练，记录时间，确保不超时） |

**关键论文**：
- Shumailov et al., 2023 "The Curse of Recursion: Training on Generated Data Makes Models Forget"
- Huang et al., 2022 "Large Language Models Can Self-Improve"
- Snell et al., 2024 "Scaling LLM Test-Time Compute Optimally can be More Effective than Scaling Model Parameters"
- Wang et al., 2023 "Voyager: An Open-Ended Embodied Agent with Large Language Models"
- Hosseini et al., 2024 "V-STaR: Training Verifiers for Self-Taught Reasoners"

---

## 四、互动环节设计

| 时间点 | 互动形式 | 负责人 | 说明 |
|--------|----------|--------|------|
| 0:00-2:00 | **举手投票** | 成员 A | "你觉得训练一个 ChatGPT 级别的模型需要多少人工标注？" |
| 23:00-26:00 | **现场 Demo** | 成员 B | Self-Refine 演示：给模型一个任务，让它自我批评并改进 |
| 48:00-53:00 | **Quiz 抢答** | 成员 C | FunSearch 相关的选择题，答对的同学可以获得小奖品（可选） |
| 67:00-70:00 | **开放讨论** | 成员 D | 总结时的思考题，引导 Q&A |

---

## 五、PPT 结构建议

```
Slide 0     : 封面（标题、课程信息、组员信息）           [成员 A 制作]
Slide 1-15  : Part 1 - 动机、演进与全景                  [成员 A 制作]
Slide 16-35 : Part 2 - 核心自我提升方法                   [成员 B 制作]
Slide 36-55 : Part 3 - 自我博弈、AI 反馈与进化            [成员 C 制作]
Slide 56-72 : Part 4 - 挑战、理论、展望与总结             [成员 D 制作]
Slide 73    : References                                   [成员 C 汇总]
Slide 74    : Thank You / Q&A                              [成员 D 制作]
```

**视觉风格建议**：
- 主色调：深蓝 + 白色 + 亮橙色（高亮重点用）
- 字体：英文用 Helvetica/Arial，中文用思源黑体
- 流程图风格统一，建议用同一套图标库
- 每张 slide 的文字不超过 5 行，重点用图表和动画表达
- 全景分类图在每个 Part 开始时复现，高亮当前讲解的部分（"你在这里"效果）

---

## 六、时间轴总览

```
 0:00 ──── 15:00 ──── 34:00 ──── 53:00 ──── 67:00 ── 70:00
 │  成员 A  │  成员 B   │  成员 C   │  成员 D         │
 │  15 min  │  19 min   │  19 min   │  17 min         │
 │          │           │           │                  │
 │ 动机     │ 核心方法   │ 博弈+进化  │ 挑战+理论+未来  │
 │ +范式演进│ +V-STaR   │ +Debate   │ +TTC +Agent     │
 │ +全景图  │ +Demo     │ +Quiz     │ +总结 +Q&A(3min)│
```

---

## 七、排练计划

| 次数 | 时间建议 | 重点 |
|------|----------|------|
| **第 1 次** | 正式前 7-10 天 | 每人只讲大纲，确认内容覆盖无遗漏、无重复，调整时间分配（注意 70 分钟的节奏，中段不能让观众疲倦） |
| **第 2 次** | 正式前 3-4 天 | 完整 PPT 排练，严格计时，练习过渡语，测试 Demo；特别注意 30-40 分钟时观众注意力可能下降——此处安排了 Demo 和 Quiz 来激活 |
| **第 3 次**（可选）| 正式前 1 天 | 精简版快速过一遍，重点练薄弱环节和 Q&A 应对 |

---

## 八、风险与备案

| 风险 | 备案 |
|------|------|
| Demo 现场翻车（网络问题/API 不响应） | 提前录好 Demo 视频 + 截图，可以无缝切换 |
| 某位成员超时 | 每人 PPT 中标记"可跳过"的 slide，排练时练习压缩版。70 分钟演讲更需要控时——每人设置"硬截止" |
| Q&A 冷场 | 成员 D 准备 2-3 个引导性问题（"有同学想问 xxx 吗？"） |
| 观众对数学公式失去兴趣 | 所有公式都配直觉解释和类比，绝不出现"这个公式很显然" |
| 70 分钟演讲中段观众疲劳 | 互动环节（Demo 在 ~24 min、Quiz 在 ~50 min）分布在中前段和中后段，保持注意力；过渡语设计要有"重新抓回注意力"的效果 |

---

## 九、相比 45 分钟版本的主要扩展

| 部分 | 原时长 | 新时长 | 新增内容 |
|------|--------|--------|----------|
| Part 1 | 10 min | 15 min | + LLM 训练范式演进 timeline、Data Wall 问题、"生成 vs 判别"能力差的直觉 |
| Part 2 | 12 min | 19 min | + V-STaR（验证器增强的自举推理）、Self-Instruct 的数据质量控制细节、ReST^EM 的 EM 算法类比、"Beyond Human Data" 突破 |
| Part 3 | 12 min | 19 min | + LLM Debate（多模型对抗辩论）、SPIN 的纳什均衡理论深入、Self-Rewarding 的迭代曲线、OpenELM |
| Part 4 | 11 min | 17 min | + 三种 Scaling 范式对比、Test-Time Compute 详细讲解（o1/o3/R1）、Voyager Agent、扩展 Q&A 至 3 分钟 |

---

## 十、核心参考文献

1. Wang et al., 2022. *Self-Instruct: Aligning Language Models with Self-Generated Instructions*
2. Zelikman et al., 2022. *STaR: Bootstrapping Reasoning With Reasoning*
3. Madaan et al., 2023. *Self-Refine: Iterative Refinement with Self-Feedback*
4. Gulcehre et al., 2023. *Reinforced Self-Training (ReST) for Language Modeling*
5. Chen et al., 2024. *Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models*
6. Yuan et al., 2024. *Self-Rewarding Language Models*
7. Bai et al., 2022. *Constitutional AI: Harmlessness from AI Feedback*
8. Romera-Paredes et al., 2024. *Mathematical Discoveries from Program Search with Large Language Models* (FunSearch)
9. Guo et al., 2023. *Connecting Large Language Models with Evolutionary Algorithms Yields Powerful Prompt Optimizers* (EvoPrompt)
10. Shumailov et al., 2023. *The Curse of Recursion: Training on Generated Data Makes Models Forget*
11. Huang et al., 2022. *Large Language Models Can Self-Improve*
12. Snell et al., 2024. *Scaling LLM Test-Time Compute Optimally can be More Effective than Scaling Model Parameters*
13. Singh et al., 2023. *Beyond Human Data: Scaling Self-Training for Problem-Solving with Language Models*
14. Hosseini et al., 2024. *V-STaR: Training Verifiers for Self-Taught Reasoners*
15. Du et al., 2023. *Improving Factuality and Reasoning in Language Models through Multiagent Debate*
16. Irving et al., 2018. *AI Safety via Debate*
17. Wang et al., 2023. *Voyager: An Open-Ended Embodied Agent with Large Language Models*
18. Villalobos et al., 2022. *Will we run out of data? An analysis of the limits of scaling datasets in Machine Learning*
19. Lehman et al., 2024. *Evolution through Large Models* (OpenELM)
