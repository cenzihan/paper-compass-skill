---
name: paper-compass
description: "Paper Compass. Build prerequisite learning paths before reading a paper. Extract concepts, anchor evidence to sections, rank order and difficulty, recommend resources. Use when user gives arXiv ID/link/PDF and asks what to learn first."
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

### C4: Impact and Venue Verification

- **For ALL papers**: Must search online for venue and impact information.
- **NEVER claim "preprint" or "unpublished" without exhaustive verification**.
- **For recent arXiv papers (within past 2 years)**: These are HIGH PRIORITY for venue verification:
  - Many arXiv papers get published at conferences (NeurIPS, ICLR, ICML, ACL, etc.) within months
  - Conference cycles: NeurIPS (Dec), ICLR (May), ICML (Jul), ACL (May-Aug)
  - A paper submitted to arXiv in May 2025 could be published at NeurIPS 2025 (Dec 2025)
  - MUST search: `{paper_title} + {year} + venue/conference/award`
  - MUST check: Semantic Scholar publication status, OpenReview, conference award pages
- **Impact data**: Search for:
  - Citation count from Semantic Scholar or Google Scholar
  - Awards (e.g., "NeurIPS 2025 Best Paper", "ICLR 2024 Outstanding Paper")
  - Significant downstream applications
- **If search tools fail**: Mark as `信息不足` or `venue待验证`, NOT "preprint/unpublished"
- **If paper has won awards**: MUST explicitly mention in "影响力" field with award name and year

### C5: Output Structure Compliance

- Section 0 (论文快照) must follow this exact structure:
  ```
  - 标题: {title}
  - 作者: {authors}
  - 年份: {year}
  - 发表信息与venue: {venue_name} | JCR 分区: {Q1/Q2/Q3/Q4/N/A} | CCF 等级: {A/B/C/N/A}
  - 来源: {paper_url_or_path}
  - **影响力**: {citation_count_and_awards_if_known_or_search_online}
  ```
- Section 6 (30 分钟快速起步) must only contain:
  - `关键实验结论: {1-3 sentences summarizing key findings}`
  - No numbered steps, no formulas, no extra content
- Section 7 must be `## 7. **Sources**:` followed by reference links

## Input Normalization

| User Input | Rule |
|---|---|
| `2010.11929` or `arxiv:2010.11929` | Convert to arXiv ID, use multi-source fallback |
| `https://arxiv.org/abs/...` | Extract ID, use multi-source fallback |
| `https://arxiv.org/pdf/...` | Extract ID, use multi-source fallback |
| `https://arxiv.org/html/...` | Extract ID, use multi-source fallback |
| Local PDF path | Parse directly with Read tool |
| Other paper URL | Fetch and parse if readable |

### arXiv Paper Access Strategy (Multi-Source Fallback)

When accessing arXiv papers, use this priority order. If one fails, try the next:

1. **Skill: arxiv**: Use `/arxiv` skill to search `{id}` → get title, authors, abstract, year, download paper
2. **Skill: semantic-scholar**: Use `/semantic-scholar` skill to search by title → get venue, citations, publication status, awards
3. **WebFetch HTML**: Try `https://arxiv.org/html/{id}` (easier to parse than PDF)
4. **WebFetch PDF**: Try `https://arxiv.org/pdf/{id}` (parse full content)
5. **If all fail**: Mark as `信息不足`, continue with skill results only

**CRITICAL**: 
- **NEVER use WebSearch** - it only works in US and will fail in other regions (returns 0 results)
- Use Skill tool to invoke `/arxiv` and `/semantic-scholar` instead - they use APIs directly, no region restriction
- If WebFetch fails with "domain verification" error, IMMEDIATELY fallback to skill-based approach
- NEVER give up after one failed attempt
- `/arxiv` + `/semantic-scholar` skills can provide enough metadata (title, authors, abstract, venue, citations) for a complete report

## Workflow

### Step 1: Read Paper and Build Section Map

**Multi-Source Paper Access** (follow this order, fallback if one fails):

```
Priority 1: Skill /arxiv {id} → title, authors, abstract, year, download paper
Priority 2: Skill /semantic-scholar {title} → venue, citations, publication status, awards
Priority 3: WebFetch HTML https://arxiv.org/html/{id} → full sections
Priority 4: WebFetch PDF https://arxiv.org/pdf/{id} → full content
Priority 5: If all fail → continue with skill metadata only, mark sections as 信息不足
```

Extract and record:

- Title, authors, year (from /arxiv skill)
- Publication metadata:
  - venue name (from /semantic-scholar skill)
  - JCR quartile (journal-only; otherwise `N/A`)
  - CCF rank (if applicable; otherwise `N/A`)
- Impact data (from /semantic-scholar skill):
  - Citation count
  - Awards (e.g., best paper awards - check if paper won awards)
  - Notable downstream applications
- Section titles and numbers (if full content accessible via WebFetch)
- Key areas: method, experimental setup, critical appendix details

**When WebFetch fails**:
- Use /arxiv and /semantic-scholar skill results for metadata (title, authors, abstract, venue, citations)
- Mark section-level evidence as `信息不足` if full paper not accessible
- Still produce a valid report with available information

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
