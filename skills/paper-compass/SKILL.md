---
name: paper-compass
description: "Paper Compass. Build a prerequisite learning path before reading a research paper. Extract prerequisite concepts, anchor each concept to paper sections and short evidence quotes, rank learning order and difficulty, recommend paper/video resources, personalize by reading memory.md, and support output language selection via lang=zh|en (default zh). Use when user gives an arxiv ID/link/PDF/local paper and asks what to learn first."
user_invocable: true
---

# Paper-Compass

Do one thing: produce an actionable prerequisite learning path before the user reads a paper.

## Language Interface

- Supported parameter: `lang=zh|en`
- Default output language: `zh`
- If `lang` is not provided, output in Chinese.
- If `lang=en`, output all report sections and notes in English.
- If `lang=zh`, all report section titles must be Chinese.
- Keep technical terms unchanged when translation could reduce precision.

## Constraints

### C0: Evidence First

- Every prerequisite concept must include at least 1 evidence anchor.
- Evidence format: `[Section] "short quote"`.
- Keep quotes short: at most 25 English words (or similarly short in other languages).
- If direct evidence is missing, mark `evidence=indirect` and lower confidence.
- Do not put non-evidenced concepts into the `Must Learn` section.

### C1: Prerequisite-Oriented Scope

- Include only concepts required to understand the paper.
- Order by dependency: foundation -> bridge -> paper-specific.
- For each concept, provide star difficulty and a minimum learning goal.
- Star difficulty format:
  - 1 -> `⭐`
  - 2 -> `⭐⭐`
  - 3 -> `⭐⭐⭐`
  - 4 -> `⭐⭐⭐⭐`
  - 5 -> `⭐⭐⭐⭐⭐`

### C2: Personalization First

- If user provides `memory=<path>`, read that file first.
- If not provided, try `~/Documents/know/memory.md`.
- If the file does not exist, continue and mark memory as not loaded.
- Do not reteach mastered knowledge unless needed for paper-specific deltas.

### C3: Honesty First

- Explicitly write `信息不足` (for zh) or `insufficient information` (for en) when needed.
- Never fabricate sections, quotes, or resource links.
- Use `low-confidence` when certainty is limited.

## Input Normalization

| User Input | Rule |
|---|---|
| `2010.11929` or `arxiv:2010.11929` | Convert to `https://arxiv.org/html/2010.11929` |
| `https://arxiv.org/abs/...` | Convert to `/html/` |
| `https://arxiv.org/pdf/...` | Try `/html/` first, fallback to PDF |
| `https://arxiv.org/html/...` | Use directly |
| Local PDF path | Parse directly |
| Other paper URL | Fetch and parse if readable |

## Workflow

### Step 1: Read Paper and Build Section Map

Extract and record:

- Title, authors, year
- Publication metadata:
  - publication status (`preprint` or `published`)
  - venue name
  - venue type (`journal` / `conference` / `workshop` / `preprint`)
  - JCR quartile (journal-only; otherwise `N/A`)
  - CCF rank (if applicable; otherwise `N/A`)
- Section titles and numbers (e.g., `3.2 Transformer Encoder`)
- Key areas: method, experimental setup, critical appendix details

For arXiv papers, prefer HTML to reduce PDF parsing errors.
If metadata cannot be verified, explicitly use `N/A` or `insufficient information` instead of guessing.

### Step 2: Load User Prior Knowledge (`memory.md`)

Read `references/memory-format.md`, then classify user knowledge into:

- `mastered`
- `familiar`
- `basic`
- `unknown`

If a concept is not present, default to `unknown`.

### Step 3: Extract Prerequisite Candidates

Use these signals:

- Core modules, operators, and training strategies in method sections
- Required prior models/theory (e.g., Transformer, contrastive learning, quantization)
- Experimental settings that materially affect conclusions

Tag each candidate:

- `role=must`: essential to understand the main method
- `role=bridge`: helps with key details
- `role=optional`: useful but not required

### Step 4: Bind Evidence and Section Usage

For each `must/bridge` concept, bind at least one evidence item:

- `section`: section name or number
- `quote`: short original quote
- `usage`: one-sentence explanation of usage in this paper

If the concept appears across multiple sections, include multiple evidence anchors.

### Step 5: Build Learning Order and Difficulty

Topologically sort by dependencies to produce `order`.

Difficulty levels:

- `1`: term-level, ~30-60 minutes
- `2`: standard module-level, ~1-2 hours
- `3`: mechanism reasoning-level, ~2-4 hours
- `4`: implementation/theory detail-level, ~4-8 hours
- `5`: cross-paper synthesis-level, >8 hours

For each concept provide:

- `minimum_goal`: what "good enough" means
- `estimated_time`: suggested time investment

### Step 6: Prune with Memory

- `mastered` and not central to this paper's novel delta: downgrade to `skip/review-optional`
- `familiar`: keep a minimal refresher path
- `basic/unknown`: keep in primary path

Explicitly state:

- Which concepts are skipped and why
- Which familiar concepts are still kept due to paper-specific novelty

### Step 7: Recommend Resources (Paper + Video)

For each `must` concept recommend:

- At least 1 paper/survey link
- At least 1 video link (lecture/talk/tutorial)

Follow `references/resource-sourcing.md`.

When `lang=zh`:

- Keep original paper links as mandatory references.
- Add more Chinese-learning links when relevant:
  - bilibili lectures/tutorials
  - Zhihu columns/answers with technical depth
  - CSDN posts only when they are implementation-useful and not low-quality copy.
- Anthropic official docs/blog articles can be included when they clarify core concepts.
- Prefer high-signal Chinese resources over generic summaries.

### Step 8: Generate Report

Select template by language:

- `lang=zh` -> `references/template.zh.md`
- `lang=en` -> `references/template.en.md`
- fallback -> `references/template.md`

Read selected template and write:

- File name: `{timestamp}--paper-compass-{short-title}__learnpath.md`
- Path: current working directory (`./`)

After writing, report the absolute output path to the user.

## Output Quality Checklist

- Every `Must Learn` item includes section-grounded evidence.
- Learning order is executable (no dependency inversion).
- Difficulty and time estimates are internally consistent.
- Resource links are valid and concept-relevant.
- Personalization clearly explains memory-driven differences.
