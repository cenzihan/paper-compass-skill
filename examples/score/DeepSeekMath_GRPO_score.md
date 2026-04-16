# 论文价值分析报告

## 0. 论文快照

- 标题: DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models
- 作者: Zhihong Shao, Peiyi Wang, Qihao Zhu, Runxin Xu, Junxiao Song, Xiao Bi, Haowei Zhang, Mingchuan Zhang, Y.K. Li, Y. Wu, Daya Guo (DeepSeek-AI)
- 年份: 2024
- 发表信息与 Venue: arXiv.org（preprint only，未发现正式发表记录）
- 来源: arXiv 2402.03300
- 对比集合状态: incomplete（未能找到 3 篇 2024-2026 年间的同领域 arXiv 论文）
- 评分置信度: medium（引用数据可靠，但对比集合不完整导致部分维度保守评分）

## 1. 分项评分

| 维度 | 满分 | 得分 | 评分依据 | 关键证据 |
|---|---:|---:|---|---|
| 发表与 Venue 信号 | 1.5 | 0.6 | arXiv preprint only，无验证的正式发表 venue | [API:Semantic Scholar] venue=arXiv.org, publicationVenue.name=arXiv.org |
| 作者与机构信号 | 1.0 | 0.8 | DeepSeek-AI 团队有 prior strong work（DeepSeek LLM, DeepSeek-Coder），作者在代码与推理领域有 track record | [Abstract] "DeepSeek-Coder-Base-v1.5 7B"; [API:Semantic Scholar] authors include prior DeepSeek-Coder authors |
| 引用量与引用增速 | 2.0 | 2.0 | 引用增速 top percentile，26 个月累计 5462 引用，月均约 210，远超 peer 集合 | [API:Semantic Scholar] citationCount=5462, influentialCitationCount=1654; 5462/max(26,6)=210/month |
| 被引论文质量 | 1.5 | 1.2 | influential citation 占比约 30%，高于典型阈值 | [API:Semantic Scholar] influentialCitationCount=1654，占比 30.3% |
| 技术增量与新颖性 | 2.0 | 1.5 | 引入 GRPO 算法（PPO variant，无 critic model）与 120B 数学语料构建方法，在 MATH benchmark 首次 open-source 达到 51.7% | [Section 1] "we introduce Group Relative Policy Optimization (GRPO)"; [Table 2] MATH=36.2% base, 51.7% RL |
| 业界贡献 / 开源 / 产品信号 | 1.0 | 0.8 | 模型权重与代码开源，GitHub repo 活跃，社区采用有初步证据 | [Abstract] "https://github.com/deepseek-ai/DeepSeek-Math"; 模型权重已发布 |
| 奠基潜力 / 方向性影响 | 1.0 | 0.4 | GRPO 方法有 early adoption 信号，但独立团队 follow-up 证据不足，尚处于 early promise 阶段 | [Section 5.2.1] unified paradigm analysis; 多篇论文引用 GRPO 但多为同一团队延续 |

## 2. 最终结论

- 总分: 7.3/10.0
- 等级: solid and worth reading, but not obviously field-defining (7.0-7.9 band)
- 阅读优先级: 推荐优先阅读，技术贡献明确（GRPO + 数学语料构建），引用增速 exceptional，但对比集合不完整导致保守评分
- 奠基潜力判断: early promise, not yet established，GRPO 有被采用信号，但缺乏多独立团队 follow-up 证据
- 一句话判断: 引用增速远超 peer，技术贡献清晰（GRPO 与大规模数学语料），但因对比集合不完整且尚无明确 field-defining 证据，保守定位于 solid paper

## 3. 相似论文对比集合（5 篇）

| Peer | 类别 | 论文 | arXiv | 年份 | Venue | 引用量 | 选入理由 |
|---|---|---|---|---:|---|---:|---|
| P1 | classic | Minerva: Solving Quantitative Reasoning Problems with Language Models | https://arxiv.org/abs/2206.14858 | 2022 | NeurIPS（推测） | 281 | 数学领域 LLM 预训练的奠基性工作，DeepSeekMath 明确引用并对比 |
| P2 | classic | Chain-of-Thought Prompting Elicits Reasoning in Large Language Models | https://arxiv.org/abs/2201.11903 | 2022 | NeurIPS 2022 | 209 | 推理 prompting 范式的奠基性工作，DeepSeekMath 使用 CoT 作为核心 reasoning format |
| P3 | recent | MetaMath: Bootstrap Your Own Mathematical Questions for Large Language Models | https://arxiv.org/abs/2309.12284 | 2023 | arXiv | 28 | 数学 reasoning 数据增强方法，同领域同任务，但发布于 2023 年（超出 strict 2-year window） |
| P4 | recent | WizardMath: Empowering Mathematical Reasoning for Large Language Models via Reinforced Evol-Instruct | https://arxiv.org/abs/2308.09583 | 2023 | arXiv | 27 | RL 方法用于数学 reasoning，与 DeepSeekMath 的 GRPO 直接对比 |
| P5 | recent | ToRA: A Tool-Integrated Reasoning Agent for Mathematical Problem Solving | https://arxiv.org/abs/2309.17452 | 2023 | arXiv | 20 | Tool-integrated reasoning 方法，同 benchmark（MATH/GSM8K），同任务 |

注：对比集合状态为 `incomplete`，因未能找到 3 篇 2024-2026 年发布的同领域 arXiv 论文。P3-P5 实际发布于 2023 年，超出 strict "last 2 years" 定义（2024-04 至 2026-04）。此状态下，不确定性维度（新颖性、奠基潜力）采用保守评分。

## 4. 横向对比观察

- 明显强于 peer 的点: 引用增速方面 DeepSeekMath 5462 引用，显著高于 Minerva 281、MetaMath 28、WizardMath 27、ToRA 20；benchmark 性能方面 MATH 51.7% 的 open-source 突破明显
- 与 peer 大体持平的点: 发布形态上均以 arXiv 为主；开源贡献上均有代码或模型开放
- 明显落后于 peer 的点: 相比 Minerva 和 Chain-of-Thought，缺少正式顶会发表记录；field-shaping 证据仍弱于更成熟的奠基工作

## 5. 分数形成原因

- 主要加分项: (1) Exceptional citation traction，26 个月 5462 引用，月均约 210；(2) influential citation 占比高，显示被引质量较强；(3) GRPO 算法贡献明确，且省去 critic model；(4) 大规模数学语料构建为模型性能提供明显支撑
- 主要扣分项: (1) Venue 仍为 arXiv only；(2) 对比集合不完整，导致 novelty 与 field-shaping 维度必须保守；(3) 缺乏多独立团队对 GRPO 的强 follow-up 证据
- 哪些新增证据会改变评分: 若后续有正式顶会/顶刊发表，publication 维度可提升；若发现多个独立团队系统性采用 GRPO，field-shaping 可上调；若找到 2024-2026 更完整的 peer 集合，可提高 novelty 评估精度

## 6. 是否值得优先读

- 结论: 值得优先阅读
- 适合优先阅读的人: 研究 LLM reasoning/RL 的研究者；关注数学 reasoning benchmark 的研究者；对数据构建 pipeline 感兴趣的研究者
- 不适合优先阅读的人: 只关注正式发表 venue 的读者；必须要求 field-defining 强证据的读者

## 7. **Sources**:

- [API:Semantic Scholar] paperId=35b142ea69598e6241f0011312128031df55895c, citationCount=5462, influentialCitationCount=1654, venue=arXiv.org, year=2024
- [API:OpenAlex] Minerva / MetaMath / WizardMath / ToRA / Chain-of-Thought metadata
- [arXiv:2402.03300](https://arxiv.org/abs/2402.03300)
- [GitHub: DeepSeek-Math](https://github.com/deepseek-ai/DeepSeek-Math)

**分数算术验证**: 0.6 + 0.8 + 2.0 + 1.2 + 1.5 + 0.8 + 0.4 = **7.3/10.0**

**Citation basis**: canonical-merged-record（Semantic Scholar 提供 merged count，无证据表明 arXiv 与 published version 存在分离记录）
