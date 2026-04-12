# 论文先修罗盘报告

## 0. 论文快照

- 标题: ReAct: Synergizing Reasoning and Acting in Language Models
- 作者: Shunyu Yao, Jeffrey Zhao, Dian Yu, Nan Du, Izhak Shafran, Karthik Narasimhan, Yuan Cao
- 年份: 2022 (arXiv), 2023 (ICLR published)
- 发表信息与venue: ICLR 2023 | JCR 分区: N/A | CCF 等级: A (ICLR为CCF-A类会议)
- 来源: https://arxiv.org/abs/2210.03629
- **影响力**: 高度影响力论文，被广泛引用（估计数千次引用），成为LLM Agent领域的奠基性工作，激发了后续大量工具使用、Agent架构研究（如AutoGPT、LangChain等）

## 1. 必学先修知识（按顺序）

| 顺序 | 知识点 | 为什么必学 | 论文使用位置 | 证据锚点 | 难度 | 最小学习目标 | 预计时间 |
|---|---|---|---|---|---|---|---|
| 1 | Large Language Models (LLMs) | ReAct的核心载体，需要理解LLM的基本能力和限制 | Section 1 Introduction | `[Sec 1] "large language models (LLMs) have demonstrated impressive performance"` | ⭐⭐ | 理解LLM的基本架构、能力边界、训练范式 | 1-2h |
| 2 | In-context Learning / Few-shot Prompting | ReAct的核心实现方式，通过few-shot示例来引导模型行为 | Section 2, 3.2 Methods | `[Sec 3.2] "prompted with few-shot in-context examples to generate both domain-specific actions and free-form language thoughts"` | ⭐⭐⭐ | 理解prompting机制、few-shot learning原理、context的作用 | 2-3h |
| 3 | Chain-of-Thought (CoT) Reasoning | 论文的核心对比基准，ReAct建立在CoT基础上 | Section 1, 3.2 | `[Sec 1] "chain-of-thought reasoning is a static black box...not grounded in the external world"` | ⭐⭐⭐ | 理解CoT的思想链机制、优点和局限性（hallucination问题） | 2-4h |
| 4 | Agent-Environment Interaction Framework | ReAct的理论框架基础，理解action/observation循环 | Section 2 | `[Sec 2] "an agent receives an observation ot∈O from the environment and takes an action at∈A"` | ⭐⭐⭐ | 理解agent policy、state transition、action space概念 | 2-3h |
| 5 | Multi-hop Question Answering | 论文主要实验场景之一(HotpotQA)，理解任务特点 | Section 3.1 | `[Sec 3.1] "HotPotQA...a multi-hop question answering benchmark that requires reasoning over two or more Wikipedia passages"` | ⭐⭐ | 理解multi-hop QA的定义、挑战、典型数据集 | 1-2h |
| 6 | Interactive Decision Making | 论文另一实验场景(ALFWorld/WebShop)，理解agent决策问题 | Section 4 | `[Sec 4] "two language-based interactive decision-making tasks...require agents to act over long horizons with sparse rewards"` | ⭐⭐⭐ | 理解long-horizon planning、sparse reward、task decomposition | 2-3h |

## 2. 桥接知识（可选但有帮助）

| 知识点 | 角色 | 论文使用位置 | 证据 | 难度 | 建议动作 |
|---|---|---|---|---|---|
| Self-Consistency (CoT-SC) | bridge | Section 3.2 | `[Sec 3.2] "sampling 21 CoT trajectories with decoding temperature 0.7...adopting the majority answer"` | ⭐⭐ | review（了解采样投票机制） |
| Imitation Learning vs RL | bridge | Section 4 | `[Sec 4] "outperforms imitation and reinforcement learning methods"` | ⭐⭐⭐ | review（理解IL/RL作为baseline的意义） |
| PaLM / GPT-3 模型家族 | bridge | Section 3, Appendix A.1 | `[Sec 2] "frozen large language model, PaLM-540B"` | ⭐⭐ | review（了解模型scale对性能的影响） |
| Wikipedia API / Tool Use | bridge | Section 3.1 | `[Sec 3.1] "search[entity]...lookup[string]...finish[answer]"` | ⭐ | learn（理解tool use的action space设计） |
| FEVER Fact Verification | bridge | Section 3.1 | `[Sec 3.1] "FEVER...fact verification benchmark"` | ⭐⭐ | review（理解事实验证任务的特点） |
| ALFWorld Environment | bridge | Section 4 | `[Sec 4] "ALFWorld...synthetic text-based game"` | ⭐⭐ | learn（理解text game环境的设计） |

## 3. 个性化增量（基于 memory.md）

- 已跳过（已掌握）:
  - 无（memory.md未加载）
- 虽然熟悉但仍保留（论文特有增量）:
  - 无
- 新出现的高优先级短板:
  - Chain-of-Thought reasoning的局限性（hallucination、error propagation）
  - Agent-Environment交互框架的形式化定义
  - ReAct特有的thought-action interleaving设计

**注意**: 用户memory.md文件未找到（路径: ~/Documents/know/memory.md 不存在），建议首次阅读此论文的用户重点关注上述必学知识。

## 4. 推荐学习资源

### Large Language Models (LLMs)

- 原论文/综述:
  - "Language Models are Few-Shot Learners" (GPT-3 paper) - https://arxiv.org/abs/2005.14165
  - "PaLM: Scaling Language Modeling with Pathways" - https://arxiv.org/abs/2204.02311
- 视频（优先高质量，含中文社区）:
  - 李宏毅教授 LLM课程 - https://www.youtube.com/watch?v=李宏毅相关课程链接
  - Andrej Karpathy "Let's build GPT" - https://www.youtube.com/watch?v=kCc8FmEb1nY
- 说明:
  - GPT-3论文是few-shot prompting的开创性工作；Karpathy视频适合理解Transformer到LLM的演进

### In-context Learning / Few-shot Prompting

- 原论文/综述:
  - "Language Models are Few-Shot Learners" - https://arxiv.org/abs/2005.14165
  - "Prompt Programming for Large Language Models" - https://arxiv.org/abs/2112.00361
- 视频:
  - 吴恩达 ChatGPT Prompt Engineering 课程 - https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/
  - bilibili: Prompt Engineering教程合集
- 说明:
  - 吴恩达课程系统性讲解prompt engineering；GPT-3论文提供理论基础

### Chain-of-Thought (CoT) Reasoning

- 原论文/综述:
  - "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models" - https://arxiv.org/abs/2201.11903 (Wei et al., 2022)
  - "Self-Consistency Improves Chain of Thought Reasoning" - https://arxiv.org/abs/2203.11171
- 视频:
  - Jason Wei CoT讲解 (Google Research) - 相关talk视频
  - bilibili: Chain-of-Thought推理原理讲解
- 说明:
  - Wei et al. 2022是CoT的开创性论文，ReAct直接对比此文；self-consistency是重要改进

### Agent-Environment Interaction Framework

- 原论文/综述:
  - "Reinforcement Learning: An Introduction" (Sutton & Barto) - http://incompleteideas.net/book/the-book.html
  - "SayCan: Grounding Language in Robotic Affordances" - https://arxiv.org/abs/2204.01691
- 视频:
  - David Silver RL课程 - https://www.deepmind.com/learning-resources/-introduction-to-reinforcement-learning-david-silver
  - bilibili: 强化学习基础课程
- 说明:
  - Sutton & Barto教材是RL/Agent理论圣经；SayCan是LLM作为agent的先驱工作

### Multi-hop Question Answering

- 原论文/综述:
  - "HotpotQA: A Dataset for Diverse, Explainable Multi-hop Question Answering" - https://arxiv.org/abs/1809.09600
  - "Question Answering Survey" - https://arxiv.org/abs/2101.08682
- 视频:
  - Yang et al. HotpotQA介绍视频
  - bilibili: 多跳问答任务讲解
- 说明:
  - HotpotQA是本论文的核心测试集；理解其multi-hop设计对理解实验至关重要

### Interactive Decision Making

- 原论文/综述:
  - "ALFWorld: Aligning Text and Embodied Environments" - https://arxiv.org/abs/2010.03768
  - "WebShop: Towards Scalable Real-world Web Interaction" - https://arxiv.org/abs/2207.01206
- 视频:
  - ALFWorld demo videos
  - Yann LeCun World Models talk
- 说明:
  - ALFWorld和WebShop是本论文的实验环境；理解这些环境的设计有助于理解agent决策挑战

## 5. 建议学习顺序

1. 阶段 A（基础）: Large Language Models → In-context Learning/Few-shot Prompting
2. 阶段 B（机制桥接）: Chain-of-Thought Reasoning → Agent-Environment Framework
3. 阶段 C（论文特有）: Multi-hop QA + Interactive Decision Making → 直接阅读ReAct论文

**总预计时间**: 约10-15小时（不含论文精读）

**建议阅读策略**:
- 先阅读Section 1 Introduction和Section 2 ReAct Framework，理解核心思想
- 跳读Section 3/4的实验细节，重点看Figure 1的示例
- 回看Appendix C的Prompt设计，理解实际实现
- 最后精读实验结果和Failure Analysis (Table 2, Appendix E)

## 6. 30 分钟快速起步

关键实验结论: ReAct通过交错生成reasoning traces（thought）和task-specific actions，在知识密集型推理任务（HotpotQA/FEVER）和交互决策任务（ALFWorld/WebShop）上显著优于纯推理(CoT)或纯行动(Action-only)方法；ReAct+CoT-SC组合方法达到最佳性能，说明内外部知识的协同是关键。

## 7. **Sources**:

- [ReAct arXiv Paper](https://arxiv.org/abs/2210.03629)
- [ReAct Project Page](https://react-lm.github.io/)
- [Chain-of-Thought Paper](https://arxiv.org/abs/2201.11903)
- [HotpotQA Dataset](https://arxiv.org/abs/1809.09600)
- [ALFWorld Environment](https://arxiv.org/abs/2010.03768)
- [WebShop Benchmark](https://arxiv.org/abs/2207.01206)
- [Self-Consistency Paper](https://arxiv.org/abs/2203.11171)
- [GPT-3 Paper](https://arxiv.org/abs/2005.14165)
- [PaLM Paper](https://arxiv.org/abs/2204.02311)
- [SayCan Paper](https://arxiv.org/abs/2204.01691)