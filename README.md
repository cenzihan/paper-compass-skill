[English](./README.en.md)

# paper-compass

Paper Compass | 论文前置学习路径生成

在读论文之前，先给你一张"先学什么"的路线图。

## 示例报告

以下为使用 `/paper-compass` 生成的完整报告示例：

| 论文 | arXiv ID | Venue | 影响力 | 报告链接 |
|---|---|---|---|---|
| Vision Transformer (ViT) | 2010.11929 | ICLR 2021 | 70,000+ 引用 | [ViT_report.md](./examples/ViT_report.md) |
| Gated Attention | 2505.06708 | NeurIPS 2025 (Best Paper) | 最佳论文奖 | [Gated_Attention_report.md](./examples/Gated_Attention_report.md) |
| QLoRA | 2305.14314 | NeurIPS 2023 | 10,000+ 引用 | [QLoRA_report.md](./examples/QLoRA_report.md) |

## 功能亮点

- 解析论文输入：支持 arXiv ID、arXiv URL、普通 URL、本地 PDF。
- 证据驱动：每个知识点都要标注"论文哪一章会用到 + 原文短证据"，避免幻觉
- 学习路径：给出先后顺序、依赖关系、学习难度、预估投入。
- 个性化裁剪：读取 `memory.md`，跳过你已经掌握的内容。
- 资源推荐：每个知识点推荐论文链接和视频链接，并支持国内社区（如知乎、CSDN、Bilibill）与国外社区（如 arxiv、Reddit、Hacker News、authorpic）的讨论资源。
- 报告输出：生成结构化学习清单（Markdown）。

## 安装

```bash
mkdir -p ~/.claude/skills/paper-compass
git clone https://github.com/cenzihan/paper-compass-skill.git
cp -r paper-compass-skill/skills/paper-compass ~/.claude/skills/paper-compass
```

安装后重启 Claude Code，即可用 `/paper-compass` 直接调用。

## 使用方式

```bash
/paper-compass <paper-input> [memory=<path/to/memory.md>] [lang=zh|en]
```

示例：

```bash
/paper-compass 2010.11929
/paper-compass https://arxiv.org/abs/2010.11929 lang=en
/paper-compass ./papers/vit.pdf lang=zh
/paper-compass 2305.14314 memory=~/Documents/know/memory.md lang=en
```

语言参数：

- `lang=zh`（默认）
- `lang=en`

## 输入支持

| 输入类型 | 示例 |
|---|---|
| arXiv ID | `2010.11929` |
| 本地 PDF | `./papers/vit.pdf` |
| 其他论文 URL | `https://...` |

## 输出内容

1. 论文快照（标题、任务、关键章节）
2. 先修知识清单（按顺序）
3. 每个知识点的证据锚点（章节 + 原文短句）
4. 难度与学习时长
5. 资源清单（论文 + 视频）
6. 基于 `memory.md` 的个性化建议

## `memory.md` 个性化

支持读取你已有知识，例如：

```markdown
- LoRA: mastered
- Quantization: familiar
- Transformer Encoder: familiar
- Position Embedding: basic
```

当分析 QLoRA 论文时，系统会优先保留"4-bit quantization / NF4 / double quantization"等差异点，弱化或跳过你已掌握的 LoRA 基础。

## 项目结构

```text
paper-compass/
├── README.md
├── README.en.md
├── CLAUDE.md
├── LICENSE
├── examples/
│   ├── ViT_report.md
│   ├── Gated_Attention_report.md
│   └── QLoRA_report.md
└── skills/
    └── paper-compass/
        ├── SKILL.md
        └── references/
            ├── template.md
            ├── template.zh.md
            ├── template.en.md
            ├── memory-format.md
            └── resource-sourcing.md
```

## License

MIT