[English](./README.en.md)

# paper-compass

Paper Compass | 论文学习路径、价值评分与阅读路线图 Skills

- `/paper-compass-learnpath`：读论文前，先生成先修学习路径
- `/paper-compass-score`：对论文做价值分析和严格评分，帮助你优先筛选值得读的论文
- `/paper-compass-roadmap`：给定一个论文文件夹和学习目标，生成多篇论文的阅读顺序路线图

## 1. `paper-compass-learnpath`

- 支持 `arXiv ID`、`arXiv URL`、普通论文 URL、和本地 PDF
- 从论文正文里抽取必须先懂的概念，结合 `memory.md` 跳过已掌握内容
- 为每个概念绑定论文章节证据和短引文，并按依赖顺序、难度、时间成本生成学习路径

现有 learnpath 示例报告：

| 论文 | arXiv ID | Venue | 说明 | 报告 |
|---|---|---|---|---|
| ReAct | 2210.03629 | ICLR 2023 | LLM Agent 奠基工作 | [ReAct_agent_report.md](./examples/learnpath/ReAct_agent_report.md) |
| Vision Transformer (ViT) | 2010.11929 | ICLR 2021 | 70,000+ citations | [ViT_report.md](./examples/learnpath/ViT_report.md) |
| Gated Attention | 2505.06708 | NeurIPS 2025 | Best Paper | [Gated_Attention_report.md](./examples/learnpath/Gated_Attention_report.md) |
| QLoRA | 2305.14314 | NeurIPS 2023 | 10,000+ citations | [QLoRA_report.md](./examples/learnpath/QLoRA_report.md) |

调用方式：

```bash
/paper-compass-learnpath <paper-input> [memory=<path/to/memory.md>] [lang=zh|en]
```

示例：

```bash
/paper-compass-learnpath 2010.11929
/paper-compass-learnpath ./papers/vit.pdf lang=zh
/paper-compass-learnpath 2305.14314 memory=~/Documents/know/memory.md lang=en
```

## 2. `paper-compass-score`

- 支持 `arXiv ID`、`arXiv URL`、普通论文 URL、和本地 PDF
- 根据期刊/会议质量、作者、引用量、技术增量和业界贡献等多维度为论文综合打分
- 会真实抓取同领域相关论文做横向对比，包含近年的论文和经典论文，避免凭空判断

现有 score 示例报告：

| 论文 | 年份 | Venue | 评分 | 说明 | 报告 |
|---|---:|---|---:|---|---|
| Update or Wait: How to Keep Your Data Fresh | 2017 | IEEE Transactions on Information Theory | 8.4 | AoI 领域奠基性论文价值分析 | [Update_or_Wait_AoI_score.md](./examples/score/Update_or_Wait_AoI_score.md) |
| CLIP | 2021 | ICML 2021 | 10.0 | 视觉语言预训练奠基论文价值分析 | [CLIP_score.md](./examples/score/CLIP_score.md) |
| Segment Anything (SAM) | 2023 | ICCV 2023 | 10.0 | 通用分割基础模型论文价值分析 | [SAM_score.md](./examples/score/SAM_score.md) |
| DeepSeekMath (GRPO) | 2024 | arXiv | 7.3 | 数学推理与 GRPO 方法论文价值分析 | [DeepSeekMath_GRPO_score.md](./examples/score/DeepSeekMath_GRPO_score.md) |

调用方式：

```bash
/paper-compass-score <paper-input> [lang=zh|en]
```

示例：

```bash
/paper-compass-score 1706.03762
/paper-compass-score https://arxiv.org/abs/2210.03629 lang=zh
/paper-compass-score ./papers/1706.03762.pdf lang=en
```

## 3. `paper-compass-roadmap`

- 支持论文文件夹路径、自然语言学习目标、可选的 `memory.md` 以及 `lang=zh|en`
- 输出一个 Markdown 报告，包含阅读顺序、链路图、每篇论文的定位理由，以及 2-3 篇必要的外部补充论文
- 默认采用“两阶段”流程：先做轻量扫描，只在必要时对 1-2 篇关键论文补充更深阅读

现有 roadmap 示例报告：

| 主题 | 文件夹 | 说明 | 报告 |
|---|---|---|---|
| LoRA 学习路线图 | `peft_paper` | 从 LoRA 基础到寻找创新点的一条主线 | [LoRA_roadmap.md](./examples/roadmap/LoRA_roadmap.md) |

调用方式：

```bash
/paper-compass-roadmap <folder-path> goal="natural-language learning goal" [memory=<path/to/memory.md>] [lang=zh|en]
```

示例：

```bash
/paper-compass-roadmap ./papers/moe goal="I want to understand MoE routing, efficient training, and serving tradeoffs" lang=zh
/paper-compass-roadmap ./reading_set goal="Build a solid path into RLHF and GRPO" memory=~/Documents/know/memory.md lang=en
```

## 安装

```bash
mkdir -p ~/.claude/skills/paper-compass-learnpath
mkdir -p ~/.claude/skills/paper-compass-score
mkdir -p ~/.claude/skills/paper-compass-roadmap
git clone https://github.com/cenzihan/paper-compass-skill.git
cp -r paper-compass-skill/skills/paper-compass-learnpath ~/.claude/skills/paper-compass-learnpath
cp -r paper-compass-skill/skills/paper-compass-score ~/.claude/skills/paper-compass-score
cp -r paper-compass-skill/skills/paper-compass-roadmap ~/.claude/skills/paper-compass-roadmap
```

安装后重启 Claude Code，即可调用这三个 skill。

## 项目结构

```text
paper-compass/
├── README.md
├── README.en.md
├── CLAUDE.md
├── LICENSE
├── examples/
│   ├── learnpath/
│   ├── score/
│   └── roadmap/
├── papers/
└── skills/
    ├── paper-compass-learnpath/
    │   ├── SKILL.md
    │   └── references/
    ├── paper-compass-score/
    │   ├── SKILL.md
    │   └── references/
    └── paper-compass-roadmap/
        ├── SKILL.md
        └── references/
```

## License

MIT
