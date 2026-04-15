---
name: paper-compass-score
description: "Paper Compass Score. Evaluate a paper's research value with a strict 10-point rubric, evidence-backed reasoning, and a mandatory 5-paper arXiv comparison set. Use when user wants to score a paper, decide whether it is worth reading, or judge whether it has field-shaping potential."
user_invocable: true
---

# Paper-Compass-Score

Do one thing: produce a rigorous paper-value analysis report that helps the user decide whether a paper is worth prioritizing.

## Language Interface

- Supported parameter: `lang=zh|en`
- Default output language: `zh`
- If `lang=en`, output the full report in English.
- If `lang=zh`, output all report section titles in Chinese.
- Keep technical terms unchanged when translation would reduce precision.

## Constraints

### C0: Evidence First

- Every scoring claim must have evidence.
- Paper-content evidence format: `[Section] "short quote"`.
- Metadata evidence format: `[API:source] field=value`.
- Comparison evidence format: `[Peer Pn] {paper_title} | {reason}`.
- Keep direct quotes short: at most 25 English words.
- If evidence is missing, explicitly write `信息不足` or `insufficient information`.

### C1: Fixed Scoring Rubric

- Total score must be `10.0`.
- Final score must use `0.1` granularity.
- Read `references/scoring-rubric.md` and use that rubric exactly.
- Do not invent new dimensions.
- Do not change weights dynamically.

### C1.5: Arithmetic Consistency Is Mandatory

- The displayed final score MUST equal the arithmetic sum of the 7 dimension scores.
- In the report, the Section 1 final score is DEFINED by Section 2: it must be computed from the 7 itemized scores in `## 2. Score Breakdown` / `## 2. 分项评分`.
- There is no hidden bonus, penalty, or manual override after summation.
- Derive the rating band from the final arithmetic score, not the other way around.
- Before writing the report, recompute the sum once and verify:
  - `final_score = publication + author + citation_traction + citation_quality + novelty + industry + field_shaping`
- If the sum and displayed final score differ even by `0.1`, fix the dimension scores or the final score before writing.
- Never write Section 1 first and then backfill Section 2. Score Section 2 first, sum it, then write Section 1.

### C2: Mandatory 5-Paper Comparison Set

- You MUST build a comparison set of exactly 5 arXiv papers found online.
- The set must contain:
  - 3 recent papers from the last 2 years
  - 2 classic papers in the same field
- Read `references/comparison-selection.md` before selecting them.
- Every selected peer paper must include:
  - title
  - arXiv link
  - year
  - venue if available
  - citation count if available
  - one-sentence reason for inclusion
- If fewer than 5 qualified arXiv papers can be verified, mark `comparison_status=incomplete` and set confidence to `low`.
- In the incomplete-comparison case, score uncertain dimensions conservatively so that the arithmetic final score does not exceed `7.4/10.0`.
- Do NOT apply a hidden post-hoc cap after summing, because that would break score consistency.

### C3: Conservative Forward-Looking Judgment

- High score does NOT automatically mean `field-defining`.
- Only use `有潜力奠基新方向` / `potentially field-shaping` when evidence supports it.
- Strong field-shaping language requires at least 2 verified signals:
  - follow-up work from multiple groups
  - reused terminology / paradigm / benchmark
  - strong citing-paper quality
  - real industry or open-source adoption
- If the paper is too new, write `early promise, not yet established`.

### C4: Metadata Verification Is Required

- For all papers, verify publication signal and citation signal with APIs.
- Never call a paper `unpublished`, `preprint only`, or `classic` without verification.
- For arXiv papers, use arXiv API first, then Semantic Scholar, then OpenAlex as needed.
- For non-arXiv papers or local PDFs, extract title first, then search APIs by title.
- For published-paper discovery, IEEE/ACM/Springer venue lookup, or ambiguous title search, you MAY use `/semantic-scholar` because it has better built-in filtering for venue papers and citation metadata.

### C5: Output Structure Is Fixed

- Read the selected template before writing.
- Output must follow the template exactly.
- Section `7. **Sources**:` must always be present.

## Input Normalization

| User Input | Rule |
|---|---|
| `2010.11929` or `arxiv:2010.11929` | Convert to arXiv ID, then fetch metadata and content |
| `https://arxiv.org/abs/...` | Extract arXiv ID |
| `https://arxiv.org/pdf/...` | Extract arXiv ID |
| `https://arxiv.org/html/...` | Extract arXiv ID |
| Local PDF path | Parse title/abstract from file, then search online metadata by title |
| Other paper URL | Fetch readable content, then search metadata by title/DOI |

## Required Data Sources

Use Bash + `curl` or small scripts for metadata calls. Prefer APIs over generic web search.

Required sources:

1. `arXiv API`
   - title
   - authors
   - abstract
   - year
   - arXiv categories
2. `Semantic Scholar API`
   - venue
   - citation count
   - influential citation count
   - TLDR
   - author list
3. `OpenAlex API` or another verified scholarly API
   - cited-by trend or backup citation metadata
   - citing paper sample when needed
4. Full paper text
   - downloaded PDF or readable HTML

Do not rely on a single unstable source for all metrics.

Recommended source roles:

- `arXiv API`: authoritative for arXiv identity and abstract metadata
- `arXiv PDF/HTML`: authoritative for paper content and quote extraction
- `/semantic-scholar` or Semantic Scholar API: best for venue discovery, citation counts, DOI, and published-paper retrieval
- `OpenAlex API`: backup cross-check for citations and related-paper metadata

## Workflow

### Step 1: Fetch Target Paper Metadata and Content

Extract and verify:

- title
- authors
- year
- venue and publication signal
- citation count
- influential citation count if available
- abstract or TLDR
- full text sections when accessible

For arXiv papers:

1. Use arXiv API for title, authors, abstract, year
2. Download PDF with retry, timeout, redirect-following, and a browser-like user agent
3. Verify the local PDF is non-empty before treating the download as successful
4. If PDF fails, try arXiv HTML as the content fallback
5. Use Semantic Scholar for venue and citation metadata
6. Use OpenAlex as a backup or cross-check for citation data
7. Keep all downloads inside the current workspace under `./papers/`, never `/tmp`

Recommended download pattern:

```bash
mkdir -p papers
curl -L --retry 5 --retry-delay 2 --retry-all-errors \
  --connect-timeout 15 --max-time 120 \
  -A "Mozilla/5.0" \
  -o ./papers/ARXIV_ID.pdf https://arxiv.org/pdf/ARXIV_ID

test -s ./papers/ARXIV_ID.pdf || \
curl -L --retry 3 --retry-delay 2 --retry-all-errors \
  --connect-timeout 15 --max-time 60 \
  -A "Mozilla/5.0" \
  -o ./papers/ARXIV_ID.html https://arxiv.org/html/ARXIV_ID
```

Failure handling:

- If both PDF and HTML fail, continue with abstract-level scoring only where necessary.
- If full text is unavailable, lower confidence for novelty judgments and write `信息不足` / `insufficient information` where needed.
- When using Read, always open `./papers/ARXIV_ID.pdf` or `./papers/ARXIV_ID.html` from the current workspace.

For local PDFs or generic URLs:

1. Extract title from the file/page
2. Prefer `/semantic-scholar` or Semantic Scholar search by title when you need better venue-aware retrieval
3. Verify the best match before continuing

### Step 2: Derive the Comparison Theme

Use the target paper's:

- title
- abstract
- keywords
- problem setting
- modality
- method family

Produce a compact comparison theme, for example:

- “LLM agent planning with tool use”
- “vision transformer architectures for image classification”
- “parameter-efficient fine-tuning for LLMs”

This theme will constrain all peer-paper selection.

### Step 3: Select 5 Similar arXiv Papers

Read `references/comparison-selection.md`, then build the peer set.

Recommended retrieval strategy for peer discovery:

1. Start from the target paper's title, abstract, and method keywords.
2. Use `/semantic-scholar` or Semantic Scholar search to find high-quality related published papers and strong citation candidates.
3. Keep only candidates that have verified arXiv IDs or arXiv links for the mandatory 5-paper set.
4. Use OpenAlex or direct API checks as a cross-check when ranking candidate peers.
5. Exclude venue-only papers from the mandatory 5-paper arXiv set, but you may still cite them as supporting context outside the table.

Recent papers:

- exactly 3 papers
- published or posted within the last 2 years
- must match the same field and at least 2 overlap signals

Classic papers:

- exactly 2 papers
- no year limit
- must be foundational or repeatedly reused in the same area

For each peer paper, record:

- `peer_id` (`P1` to `P5`)
- title
- arXiv URL
- year
- venue
- citation count
- overlap reason

### Step 4: Score With the Fixed Rubric

Read `references/scoring-rubric.md`.

Apply every dimension:

1. Publication / venue signal
2. Author and affiliation signal
3. Citation traction
4. Quality of citing papers
5. Technical novelty and delta
6. Industry / open-source / product contribution
7. Field-shaping potential

Scoring rules:

- Each dimension must have:
  - score
  - max score
  - evidence
  - one-sentence rationale
- Scores must use `0.1` granularity.
- Round only at the end of each dimension, then sum the 7 displayed Section 2 scores to get the final score.
- After summing, run an explicit arithmetic check before writing Section 1.
- Section 1, Section 5, and Section 6 must all be consistent with the same final score.

### Step 5: Write a Strict Comparative Verdict

The verdict must answer:

- Is this paper worth prioritizing?
- Is it stronger than its 5-paper comparison set on the key dimensions?
- Is it a strong paper, a useful paper, or mostly a low-priority paper?
- Does it show early signs of becoming field-shaping?

Also state:

- what drove the score up
- what drove the score down
- what evidence could materially change the score later

### Step 6: Generate the Report

Select template by language:

- `lang=zh` -> `references/template.zh.md`
- `lang=en` -> `references/template.en.md`
- fallback -> `references/template.md`

Write:

- File name: `{timestamp}--paper-compass-score-{short-title}__score.md`
- Path: current working directory (`./`)

After writing, report the absolute output path.

## Output Quality Checklist

- Exactly 5 peer papers are listed, or incompleteness is made explicit.
- Every score line has evidence.
- The comparison set includes 3 recent papers and 2 classics.
- The final verdict is consistent with the score and evidence.
- Strong claims about `field-shaping` are conservative and evidence-backed.
- The final score exactly matches the sum of the 7 dimension scores.
