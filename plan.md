# CS2916 Presentation Plan: 自我提升与自我进化算法

> **课程**: CS2916 大语言模型 | **时长**: 45 分钟 | **人数**: 4 人

---

## 一、核心叙事线

整场演讲围绕一个核心类比展开：

> **LLM 的成长路径，就像一个学生从"跟老师学"到"自己学"的过程。**
>
> Pre-training = 读教科书；SFT/RLHF = 老师批改作业；
> **Self-Improving = 自己出题、自己做、自己批、自己改。**

四位成员依次回答四个递进问题：
1. **为什么要自我提升？**（动机与全景）
2. **怎么自己学？**（核心自我提升方法）
3. **怎么自己进化？**（自我博弈与进化算法）
4. **能学到什么程度？有什么风险？**（理论边界、挑战与未来）

---

## 二、详细大纲

### Part 1：开场 —— 为什么 LLM 需要"自学"？（成员 A，约 10 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 0:00-1:30 | **Hook：互动投票** | 用举手/弹幕提问："你觉得 GPT-4 级别的模型背后需要多少人工标注数据？" 给出选项 A/B/C/D，揭晓答案后引出人工标注的高成本问题 |
| 1:30-3:30 | **瓶颈：人工标注不可持续** | 数据标注成本曲线（配图表）；RLHF 需要大量人类偏好数据；高质量标注者本身就很稀缺（"你去哪找 10 万个数学博士来标数据？"——幽默点） |
| 3:30-5:30 | **核心思想：Bootstrap（自举）** | 用学生自学的类比讲清楚"用自己的输出来训练自己"这个看似矛盾的想法；引用 Baron Munchausen 自己拽头发把自己从沼泽里拉出来的故事（Self-improvement 文献中的经典典故） |
| 5:30-8:00 | **全景地图：分类框架** | 展示一张精心设计的分类图，把接下来要讲的所有方法放在统一框架中：<br>- **自我提升（Self-Improving）**：Self-Instruct, STaR, Self-Refine, ReST<br>- **自我博弈（Self-Play）**：SPIN, Self-Rewarding LM<br>- **自我进化（Self-Evolving）**：Constitutional AI, FunSearch, EvoPrompt<br>清晰定义三者的区别和联系 |
| 8:00-10:00 | **关键问题预告 + 过渡** | "一个模型真的能通过自己训练自己变得更强吗？这难道不是'自己批改自己的作业'吗？带着这个疑问，让我们来看看具体的方法。" 交棒给成员 B |

**成员 A 需要准备的材料**：
- 一张人工标注成本的信息图
- 自举（Bootstrap）的直觉动画/示意图
- 方法全景分类图（这张图贯穿全场，后续成员讲到哪个方法就高亮哪个位置）
- 互动投票的选项设计

---

### Part 2：核心方法 —— 自己出题，自己做，自己改（成员 B，约 12 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 10:00-13:00 | **Self-Instruct：自己给自己出题** | 核心流程图：Seed Tasks → LLM 生成 Instruction → LLM 生成 Response → 过滤 → 微调<br>关键论文：Wang et al., 2022 "Self-Instruct"<br>强调"种子任务"的重要性——你不是从零开始，你需要一点火种 |
| 13:00-16:30 | **STaR：自举推理能力** | 核心思想：让模型先尝试解题，答对了的 rationale 拿来当训练数据<br>关键创新：rationalization——答错的题，给它看答案，让它"事后解释"，这个解释也能拿来训练<br>论文：Zelikman et al., 2022 "STaR: Bootstrapping Reasoning With Reasoning"<br>用一个具体的数学推理例子走一遍流程<br>（幽默点："就像考试不会做的题，看了答案之后觉得'啊我本来也想到了'——STaR 就是把这个过程形式化了"） |
| 16:30-19:00 | **Self-Refine：迭代自我批评与修改** | 三步循环：Generate → Critique → Refine<br>关键点：不需要微调，纯 prompting<br>论文：Madaan et al., 2023 "Self-Refine"<br>**现场互动**：请一位同学给出一段"很烂的代码"（或者提前准备），现场用 ChatGPT 演示 Self-Refine 过程：先写→自评→改进，让观众直观感受迭代改进的效果 |
| 19:00-22:00 | **ReST / ReST^EM：强化自训练** | 核心思想：Generate（采样多个解）→ Improve（用奖励模型筛选）→ Train（用好的样本微调）<br>与 STaR 的区别：引入了显式的奖励信号，更接近 RL 的框架<br>论文：Gulcehre et al., 2023; Singh et al., 2023<br>一张对比表：Self-Instruct vs STaR vs Self-Refine vs ReST 的异同 |

**过渡**（22:00）：
> "到目前为止，我们看到的方法有一个共同特点：模型在 **单向地** 提升自己。但如果模型能和自己对弈呢？如果模型能自己当裁判呢？甚至，如果我们用进化的方式让模型群体自己'演化'呢？"

**成员 B 需要准备的材料**：
- 每个方法的流程图（统一视觉风格）
- STaR 的数学推理具体示例（step-by-step）
- Self-Refine 的现场 demo（提前测试好，确保能跑通）
- 四种方法的对比表

---

### Part 3：自我博弈与自我进化 —— 当模型自己和自己下棋（成员 C，约 12 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 22:00-25:00 | **SPIN：自我博弈微调** | 核心思想：类比 AlphaGo 的自我博弈——主模型 vs. 上一轮的自己<br>目标：让当前模型的输出与人类数据不可区分<br>论文：Chen et al., 2024 "Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models"<br>数学直觉（不要太深，给一个 min-max 的大致概念就好）<br>关键结果：Zephyr-7B 在 SPIN 后超越了 GPT-4 在部分 benchmark 上的表现 |
| 25:00-28:30 | **Self-Rewarding Language Models：自己当裁判** | 核心突破：不再需要单独的奖励模型（Reward Model），LLM 自己既当选手又当裁判<br>迭代流程：用自己生成 + 自己评分 → 筛选 → 微调 → 新一轮<br>论文：Yuan et al., 2024<br>与 RLHF 的关键区别对比图<br>（幽默点："这就像你自己给自己的论文打分——听起来很不靠谱，但意外地 work"） |
| 28:30-31:00 | **Constitutional AI：给自我进化加上"宪法"** | 核心思想：用一组人写的原则（Constitution）代替大量人工标注<br>两阶段：Critique → Revision（基于原则的自我批评）→ RLAIF（AI 反馈的强化学习）<br>论文：Bai et al., 2022<br>强调这是 Anthropic 的核心方法论——与安全对齐的紧密联系 |
| 31:00-34:00 | **进化算法 × LLM：FunSearch & EvoPrompt** | FunSearch（DeepMind, 2024）：用 LLM 做进化算法的 mutation operator，发现了超越人类已知最优解的数学构造<br>EvoPrompt：用进化算法优化 prompt<br>核心洞察：LLM 不只是被进化的对象，它本身就是进化过程中的"变异引擎"<br>**小互动/Quiz**："以下哪个是 FunSearch 真正发现的？" A. 新的排序算法 B. cap set 问题的新下界 C. 新的蛋白质结构 → 揭晓答案并简述 |

**过渡**（34:00）：
> "听到这里，你可能会觉得 Self-Improvement 是一个银弹——模型自己就能越变越强。但事情真的这么美好吗？"

**成员 C 需要准备的材料**：
- SPIN 的自我博弈对比图（类比 AlphaGo）
- Self-Rewarding vs RLHF 的对比图
- Constitutional AI 的"宪法原则"示例
- FunSearch 的结果展示（cap set 问题的图示）
- 互动 Quiz 的设计

---

### Part 4：天花板、陷阱与未来 —— 自我提升的边界在哪？（成员 D，约 11 分钟）

| 时间 | 内容 | 备注 |
|------|------|------|
| 34:00-37:00 | **三大核心挑战** | **1. 模型坍缩（Model Collapse）**：用自己的输出训自己 → 分布越来越窄 → 多样性丧失 → 质量下降（配图：Shumailov et al., 2023 的实验曲线）<br>**2. 奖励欺骗（Reward Hacking）**：Self-Rewarding 的风险——模型学会给自己"放水"<br>**3. 分布偏移（Distribution Shift）**：生成数据偏离真实数据分布<br>用一个统一的例子把三个问题串起来 |
| 37:00-39:30 | **理论视角：自我提升的上界** | 直觉问题："一个 60 分的模型能把自己训练到 90 分吗？"<br>Huang et al., 2022 "Large Language Models Can Self-Improve" 的关键发现：模型规模足够大时，Self-Consistency + CoT 就能自我提升<br>但有条件的：需要 **足够的多样性** 和 **可靠的验证信号**<br>Scaling 的视角：数据 scaling vs. 计算 scaling vs. self-improvement scaling |
| 39:30-42:00 | **前沿展望（2024-2025）** | 1. **Test-Time Compute Scaling**：o1/o3 的思路——不只在训练时提升，推理时也能"自我提升"<br>2. **Multi-Agent Self-Improvement**：多个模型互相提升（debate, collaboration）<br>3. **Self-Improving Agents**：不只是语言能力，而是在环境中行动并从反馈中学习<br>4. **与 AI Safety 的关系**：递归自我提升（Recursive Self-Improvement）是 AI 安全研究的核心关切之一 |
| 42:00-44:00 | **总结与回顾** | 回到开场的全景地图，把所有方法点亮<br>一句话总结每个 Part 的核心 takeaway<br>回答开场的问题："模型真的能自己批改自己的作业吗？" → "可以，但需要精心设计的机制来避免'自欺欺人'"<br>留一个开放性思考："如果模型真的能无限自我提升，那人类标注者的角色会变成什么？" |
| 44:00-45:00 | **Q&A 引导** | 抛出 2-3 个引导性问题帮助观众提问<br>如时间允许，回答 1-2 个问题 |

**成员 D 需要准备的材料**：
- Model Collapse 的实验曲线图
- 理论上界的直觉图示
- 前沿方向的 timeline 图
- 全场总结的回顾 slide（重新展示全景图并高亮所有已讲内容）

---

## 三、成员分工明细

### 成员 A：开场与背景（负责 Part 1）

**演讲内容**：0:00 - 10:00（约 10 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 负责开场 Hook（互动投票）、瓶颈分析、Bootstrap 核心思想、全景分类图讲解 |
| **PPT 制作** | 制作 Part 1 的 slides（约 10-12 页）；**重点制作全景分类图**（这张图被全场复用，需要美观清晰） |
| **内容研究** | 调研人工标注成本数据（InstructGPT, RLHF 的标注规模）；整理 Self-Improving 领域的综述性材料 |
| **全场职责** | 负责 **统一 PPT 风格模板**（配色、字体、排版），确保四位成员的 slides 视觉一致 |

**关键论文/资料**：
- Ouyang et al., 2022 "Training language models to follow instructions with human feedback"（InstructGPT，了解 RLHF 的标注成本）
- 一篇 Self-Improvement 的 survey paper 作为全景框架参考

---

### 成员 B：核心自我提升方法（负责 Part 2）

**演讲内容**：10:00 - 22:00（约 12 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 讲解 Self-Instruct、STaR、Self-Refine、ReST 四种方法；负责现场 Self-Refine demo |
| **PPT 制作** | 制作 Part 2 的 slides（约 12-15 页）；每种方法一张清晰的流程图 |
| **内容研究** | 深读四篇核心论文，提取核心算法流程和关键实验结果 |
| **Demo 准备** | 准备 Self-Refine 的现场演示（可以用 ChatGPT/Claude API，提前准备好 fallback 截图以防网络问题） |
| **全场职责** | 制作 **四种方法的对比表**（输入、是否需要微调、是否需要奖励模型、适用场景等维度） |

**关键论文**：
- Wang et al., 2022 "Self-Instruct: Aligning Language Models with Self-Generated Instructions"
- Zelikman et al., 2022 "STaR: Bootstrapping Reasoning With Reasoning"
- Madaan et al., 2023 "Self-Refine: Iterative Refinement with Self-Feedback"
- Gulcehre et al., 2023 "Reinforced Self-Training (ReST) for Language Modeling"
- Singh et al., 2023 "Beyond Human Data: Scaling Self-Training for Problem-Solving with Language Models"

---

### 成员 C：自我博弈与自我进化（负责 Part 3）

**演讲内容**：22:00 - 34:00（约 12 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 讲解 SPIN、Self-Rewarding LM、Constitutional AI、FunSearch/EvoPrompt；负责 Quiz 互动 |
| **PPT 制作** | 制作 Part 3 的 slides（约 12-15 页）；SPIN 的博弈示意图、Self-Rewarding vs RLHF 对比图 |
| **内容研究** | 深读相关论文，特别理解 SPIN 的博弈论直觉和 Constitutional AI 的设计哲学 |
| **互动设计** | 设计 FunSearch 相关的 Quiz 问题 |
| **全场职责** | 负责收集和整理所有引用的 **参考文献列表**，放在最后的 References slide 中 |

**关键论文**：
- Chen et al., 2024 "Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models"
- Yuan et al., 2024 "Self-Rewarding Language Models"
- Bai et al., 2022 "Constitutional AI: Harmlessness from AI Feedback"
- Romera-Paredes et al., 2024 "Mathematical discoveries from program search with large language models"（FunSearch）
- Guo et al., 2023 "Connecting Large Language Models with Evolutionary Algorithms Yields Powerful Prompt Optimizers"（EvoPrompt）

---

### 成员 D：挑战、理论与未来（负责 Part 4）

**演讲内容**：34:00 - 45:00（约 11 分钟）

| 职责 | 具体任务 |
|------|----------|
| **演讲** | 讲解三大挑战、理论上界、前沿展望、全场总结；引导 Q&A |
| **PPT 制作** | 制作 Part 4 的 slides（约 10-12 页）；Model Collapse 实验图、总结回顾 slide |
| **内容研究** | 深读 Model Collapse、理论分析、Test-Time Compute 等相关论文 |
| **Q&A 准备** | 提前准备 5-8 个可能的 Q&A 问题及简要回答，与全组共享 |
| **全场职责** | 负责 **排练统筹**（组织至少 2 次完整排练，记录时间，确保不超时） |

**关键论文**：
- Shumailov et al., 2023 "The Curse of Recursion: Training on Generated Data Makes Models Forget"
- Huang et al., 2022 "Large Language Models Can Self-Improve"
- Snell et al., 2024 "Scaling LLM Test-Time Compute"
- Hosseini et al., 2024 "V-STaR: Training Verifiers for Self-Taught Reasoners"

---

## 四、互动环节设计

| 时间点 | 互动形式 | 负责人 | 说明 |
|--------|----------|--------|------|
| 0:00-1:30 | **举手投票** | 成员 A | "你觉得训练一个 ChatGPT 级别的模型需要多少人工标注？" |
| 16:30-19:00 | **现场 Demo** | 成员 B | Self-Refine 演示：给模型一个任务，让它自我批评并改进 |
| 31:00-34:00 | **Quiz 抢答** | 成员 C | FunSearch 相关的选择题，答对的同学可以获得小奖品（可选） |
| 42:00-44:00 | **开放讨论** | 成员 D | 总结时的思考题，引导 Q&A |

---

## 五、PPT 结构建议

```
Slide 0   : 封面（标题、课程信息、组员信息）         [成员 A 制作]
Slide 1-10: Part 1 - 动机与全景                       [成员 A 制作]
Slide 11-25: Part 2 - 核心自我提升方法                 [成员 B 制作]
Slide 26-40: Part 3 - 自我博弈与自我进化               [成员 C 制作]
Slide 41-52: Part 4 - 挑战、展望与总结                 [成员 D 制作]
Slide 53  : References                                 [成员 C 汇总]
Slide 54  : Thank You / Q&A                            [成员 D 制作]
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
 0:00 ──── 10:00 ──── 22:00 ──── 34:00 ──── 45:00
 │  成员 A  │  成员 B   │  成员 C   │  成员 D  │
 │  10 min  │  12 min   │  12 min   │  11 min  │
 │          │           │           │          │
 │ 动机     │ 核心方法   │ 博弈+进化  │ 挑战+未来 │
 │ +全景图  │ +Demo     │ +Quiz     │ +总结+QA │
```

---

## 七、排练计划

| 次数 | 时间建议 | 重点 |
|------|----------|------|
| **第 1 次** | 正式前 7-10 天 | 每人只讲大纲，确认内容覆盖无遗漏、无重复，调整时间分配 |
| **第 2 次** | 正式前 3-4 天 | 完整 PPT 排练，严格计时，练习过渡语，测试 Demo |
| **第 3 次**（可选）| 正式前 1 天 | 精简版快速过一遍，重点练薄弱环节和 Q&A 应对 |

---

## 八、风险与备案

| 风险 | 备案 |
|------|------|
| Demo 现场翻车（网络问题/API 不响应） | 提前录好 Demo 视频 + 截图，可以无缝切换 |
| 某位成员超时 | 每人 PPT 中标记"可跳过"的 slide，排练时练习压缩版 |
| Q&A 冷场 | 成员 D 准备 2-3 个引导性问题（"有同学想问 xxx 吗？"） |
| 观众对数学公式失去兴趣 | 所有公式都配直觉解释和类比，绝不出现"这个公式很显然" |

---

## 九、核心参考文献

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
