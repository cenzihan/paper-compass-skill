# Resource Sourcing Policy

For each `must` concept, provide:

- 1-2 paper/survey resources
- 1-2 video resources

Language-specific requirement:

- If `lang=zh`, keep original paper links and add Chinese community resources when high quality (bilibili, Zhihu, CSDN).
- If `lang=en`, prioritize English-first academic resources.

## Video Resource Rules (CRITICAL)

**NEVER fabricate video links** - this is the #1 source of hallucination.

### Allowed Video Sources (Verified Channels Only)

Only use these **verified, stable** video sources. Do NOT invent new links:

**Chinese (lang=zh):**
- 李宏毅机器学习课程: https://www.youtube.com/c/HungyiLeeNTU (频道主页) 或 https://speech.ee.ntu.edu.tw/~hylee/ml/ (课程主页)
- 清华大学计算机系: 搜索时仅使用官方频道链接
- 如果找不到特定视频，提供频道主页 + 关键词搜索建议

**English (lang=en):**
- Stanford CS231n: https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO6sV
- Stanford CS224n: https://www.youtube.com/playlist?list=PLoROMvodv4rOhcuXBZ1S2B8I8qDZA1y-
- 3Blue1Brown: https://www.youtube.com/c/3blue1brown
- Andrej Karpathy: https://www.youtube.com/c/AndrejKarpathy

### Video Search Protocol

When you need a video resource:

1. **FIRST**: Check if the concept matches one of the verified channels above
2. **IF NO MATCH**: Search via WebFetch or Skill tool
   - Use WebFetch to search: `https://www.bilibili.com/search?keyword={concept}`
   - Extract actual video BV IDs from search results
3. **VERIFY**: Before using any bilibili link, verify with WebFetch: `https://www.bilibili.com/video/{bv_id}`
4. **IF SEARCH FAILS**: Mark as `video_status: search_failed` and suggest search keywords instead

### bilibili Link Format Rules

- Valid format: `https://www.bilibili.com/video/BV{10-character-code}`
- BV codes are 10 characters (BV1xxxxxxxxx), NOT arbitrary strings
- Example valid BV: `BV1Wv411B7qL` (verified)
- If you don't have a verified BV code, DO NOT write a bilibili link

### When Video is Unavailable

Write explicitly:
```
video_status: unavailable
search_suggestion: 在 bilibili 搜索 "{concept} 教程" 或 YouTube 搜索 "{concept} tutorial"
```

NEVER write a fake bilibili link just to fill the "video" slot.

## Ranking Rules

Prefer resources in this order:

1. Original or seminal papers
2. High-quality surveys/tutorial papers
3. Official conference tutorials / university lectures
4. Reputable educational channels with technical depth

## Link Quality Rules

- Use stable URLs when possible (`arxiv.org`, official conference pages, official channel uploads).
- Anthropic official docs/blog articles are allowed as high-quality explanatory references when relevant.
- Avoid low-information aggregator pages.
- Avoid paywalled-only links when a free alternative exists.

## Matching Rules

- Each resource must map to one concrete concept.
- Add one-line rationale: why this resource is useful for this concept.
- If only one good source exists, provide one and mark `limited-source`.
- For `lang=zh`, ensure each core concept still has at least one original paper/survey link (do not replace papers with only community posts).

## Honesty Rules

- If no reliable resource is found, write:
  - `resource_status: unavailable`
  - `note: no high-confidence open resource found`
- Never fabricate a title or URL.
