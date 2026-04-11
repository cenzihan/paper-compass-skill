# paper-compass

Paper Compass

Build a prerequisite roadmap before you start reading a research paper.

## Highlights

- Parses paper input from arXiv IDs, arXiv URLs, generic URLs, and local PDFs.
- Evidence-driven output: each concept includes section usage and a short quote.
- Learning path planning: order, dependency, difficulty, and time estimate.
- Personalized trimming with `memory.md` to skip what you already know.
- Resource recommendations: paper links and video links for each concept.
- Structured report output in Markdown.

## Install

```bash
mkdir -p ~/.claude/skills/paper-compass
git clone https://github.com/cenzihan/paper-compass-skill.git
cp -r paper-compass-skill/skills/paper-compass ~/.claude/skills/paper-compass
```

After installation, restart Claude Code and use `/paper-compass` directly.

## Usage

```bash
/paper-compass <paper-input> [memory=<path/to/memory.md>] [lang=zh|en]
```

Examples:

```bash
/paper-compass 2010.11929
/paper-compass https://arxiv.org/abs/2010.11929 lang=en
/paper-compass ./papers/vit.pdf lang=zh
/paper-compass 2305.14314 memory=~/Documents/know/memory.md lang=en
```

Language option:

- `lang=zh` (default)
- `lang=en`

## Supported Inputs

| Input Type | Example |
|---|---|
| arXiv ID | `2010.11929` |
| Local PDF | `./papers/vit.pdf` |
| Other paper URL | `https://...` |

## Output Structure

1. Paper snapshot (title, task, key sections)
2. Ordered prerequisite concept list
3. Evidence anchors per concept (section + short quote)
4. Difficulty and time estimate
5. Resource list (papers + videos)
6. Personalized suggestions based on `memory.md`

## ViT Example (Excerpt)

| Order | Prerequisite | Where Used In Paper | Evidence Anchor (Example) | Difficulty |
|---|---|---|---|---|
| 1 | Patch Embedding | Sec 3.1 Vision Transformer | `"...split an image into fixed-size patches..."` | 2 |
| 2 | Position Embedding | Sec 3.1 / 3.2 | `"...add position embeddings to patch embeddings..."` | 2 |
| 3 | Transformer Encoder | Sec 3.2 | `"...standard Transformer encoder..."` | 3 |
| 4 | MSA + MLP Block | Sec 3.2 | `"...alternating layers of MSA and MLP..."` | 3 |
| 5 | Large-scale Pretraining | Sec 4 | `"...pre-train on large datasets..."` | 3 |

## `memory.md` Personalization

The skill can use your existing knowledge profile, for example:

```markdown
- LoRA: mastered
- Quantization: familiar
- Transformer Encoder: familiar
- Position Embedding: basic
```

When analyzing the QLoRA paper, it prioritizes deltas like `4-bit quantization / NF4 / double quantization`, while downgrading or skipping LoRA basics you already know.

## Project Structure

```text
paper-compass/
├── README.md
├── README.en.md
├── CLAUDE.md
├── LICENSE
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


