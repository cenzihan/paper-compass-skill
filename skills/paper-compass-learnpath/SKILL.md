---
name: paper-compass-learnpath
description: "Paper Compass Learnpath. Build prerequisite learning paths before reading a paper. Extract concepts, anchor evidence to sections, rank order and difficulty, recommend resources. Use when user gives arXiv ID/link/PDF and asks what to learn first."
user_invocable: true
---

# Paper-Compass-Learnpath

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

- **For ALL papers**: Must fetch venue and impact information via API.
- **NEVER claim "preprint" or "unpublished" without verification**.
- **For recent arXiv papers (within past 2 years)**: These are HIGH PRIORITY for venue verification:
  - Many arXiv papers get published at conferences (NeurIPS, ICLR, ICML, ACL, etc.) within months
  - Conference cycles: NeurIPS (Dec), ICLR (May), ICML (Jul), ACL (May-Aug)
- **Impact data**: Fetch via Semantic Scholar API (Bash + curl):
  - Citation count
  - Venue name
  - TLDR (one-line summary)
- **If API fails**: Mark as `venue待验证` or `citations待验证`, NOT "preprint/unpublished"

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

**arXiv resources (PDF, HTML, abs) are PRIMARY - always use first:**

1. **arXiv API (abs)**: Fetch `https://export.arxiv.org/api/query?id_list={id}` → get title, authors, abstract, year
2. **Stable PDF download with retry**: use `curl` with redirect-following, retry, timeout, and a browser-like user agent
3. **HTML fallback**: if PDF still fails, fetch `https://arxiv.org/html/{id}` when available
4. **Semantic Scholar API (supplement)**: `curl -s "https://api.semanticscholar.org/graph/v1/paper/ARXIV:{id}?fields=venue,citationCount,tldr"` → get venue, citations

**Priority Order:**
```
Priority 1: arXiv API → metadata (title, authors, abstract, year)
Priority 2: Stable PDF download + Read tool → full paper content (sections, quotes, methods)
Priority 3: arXiv HTML fallback → recover readable sections when PDF fails
Priority 4: Semantic Scholar API via curl → venue, citations, TLDR (supplement)
```

**CRITICAL**: 
- **NEVER use WebSearch** - it only works in US and will fail in other regions
- For arXiv full-text access, prefer direct arXiv/API access first; `semantic-scholar` is only a metadata supplement, not the primary full-text path
- **NEVER use WebFetch for arxiv.org** - domain verification will block it; use curl/download instead
- arXiv PDF is the BEST source for full paper content, but downloads may intermittently fail; always use retry before giving up
- Prefer `https://arxiv.org/pdf/{id}` over hardcoding `.pdf` suffix when downloading
- A successful PDF download must leave a non-empty local file before continuing
- **NEVER download to `/tmp` or other system temp paths**; always save files under the current working directory in `./papers/`
- When using Read, always read `./papers/{id}.pdf` or `./papers/{id}.html` from the current workspace so Windows path resolution does not fail

## Workflow

### Step 1: Fetch Paper Metadata and Content

**Use Bash + Python/curl for arXiv access and full-text retrieval** (no WebSearch, no WebFetch for arxiv.org):

```bash
# 1. Get arXiv metadata
python3 -c "
import urllib.request, xml.etree.ElementTree as ET, json
NS = 'http://www.w3.org/2005/Atom'
url = 'https://export.arxiv.org/api/query?id_list=ARXIV_ID'
with urllib.request.urlopen(url, timeout=30) as r:
    root = ET.fromstring(r.read())
# Extract title, authors, abstract, year, pdf_url...
"

# 2. Always download to the current workspace under ./papers/
mkdir -p papers
curl -L --retry 5 --retry-delay 2 --retry-all-errors \
  --connect-timeout 15 --max-time 120 \
  -A "Mozilla/5.0" \
  -o ./papers/ARXIV_ID.pdf https://arxiv.org/pdf/ARXIV_ID

# 3. Verify local PDF exists and is non-trivial
test -s ./papers/ARXIV_ID.pdf

# 4. If PDF failed, try HTML fallback
curl -L --retry 3 --retry-delay 2 --retry-all-errors \
  --connect-timeout 15 --max-time 60 \
  -A "Mozilla/5.0" \
  -o ./papers/ARXIV_ID.html https://arxiv.org/html/ARXIV_ID

# 5. Get Semantic Scholar metadata (venue, citations)
curl -s "https://api.semanticscholar.org/graph/v1/paper/ARXIV:ARXIV_ID?fields=venue,citationCount,publicationVenue,tldr"
```

**Then read content**:
```bash
# Prefer Read tool on ./papers/ARXIV_ID.pdf
# If the PDF download failed but HTML exists, parse ./papers/ARXIV_ID.html instead
# Do not read /tmp/... paths on Windows
```

Extract and record:

- Title, authors, year (from arXiv API)
- Publication metadata:
  - venue name (from Semantic Scholar API via curl)
  - JCR quartile (journal-only; otherwise `N/A`)
  - CCF rank (if applicable; otherwise `N/A`)
- Impact data (from Semantic Scholar API via curl):
  - Citation count
  - TLDR (one-line summary)
- Section titles and quotes (prefer PDF via Read tool, fallback to HTML)
- Key areas: method, experimental setup, critical appendix details

**When download or APIs fail**:
- If arXiv API fails, stop claiming verified title/year metadata
- If PDF fails but HTML works, continue with lowered confidence for quote extraction
- Mark section-level evidence as `信息不足` if neither PDF nor HTML is accessible
- Mark venue/citations as `待验证` if Semantic Scholar API fails
- Still produce a valid report with available information
- If direct Semantic Scholar API lookup is weak or ambiguous, you may use `/semantic-scholar` only as a metadata cross-check for venue, citations, DOI, and TLDR

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

- File name: `{timestamp}--paper-compass-learnpath-{short-title}__learnpath.md`
- Path: current working directory (`./`)

After writing, report the absolute output path to the user.

## Output Quality Checklist

- Every `Must Learn` item includes section-grounded evidence.
- Learning order is executable (no dependency inversion).
- Difficulty and time estimates are internally consistent.
- Resource links are valid and concept-relevant.
- Personalization clearly explains memory-driven differences.
