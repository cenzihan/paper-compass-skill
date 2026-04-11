# CLAUDE.md

This repository contains a Claude Code skill: `paper-compass`.

## 项目定位

`paper-compass` 用于在“读论文前”生成先修知识路线图，重点是：

1. 找出必须先懂的概念
2. 给出概念在论文中的证据锚点（章节 + 短引文）
3. 按依赖顺序、学习难度、时间成本组织学习路径
4. 结合 `memory.md` 做个性化剪枝

## Skill 路径

- Skill definition: `skills/paper-compass/SKILL.md`
- Output template: `skills/paper-compass/references/template.md`
- Memory format guide: `skills/paper-compass/references/memory-format.md`
- Resource policy: `skills/paper-compass/references/resource-sourcing.md`

## 运行入口

```text
/paper-compass <paper-input> [memory=<path/to/memory.md>] [lang=zh|en]
```

## 设计原则

- Evidence first: 无证据不进主清单
- Dependency first: 先基础后论文特有
- Personalization first: 已掌握内容降级或跳过
- Honest first: 信息不足时明确标注，不编造

