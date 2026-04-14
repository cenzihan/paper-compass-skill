# Scoring Rubric

Total score: `10.0`

All dimension scores must use `0.1` granularity.

## Rating Bands

- `9.0-10.0`: exceptional; strong evidence of long-term importance, possibly field-shaping
- `8.0-8.9`: very strong; high-priority reading candidate
- `7.0-7.9`: solid and worth reading, but not obviously field-defining
- `6.0-6.9`: selective reading; useful for specific needs
- `< 6.0`: lower priority unless the user has a narrow reason to read it

## 1. Publication / Venue Signal (`max 1.5`)

Base score:

- `1.5`: flagship top-tier conference or top-tier journal, clearly verified
- `1.2`: strong venue or strong journal, but not topmost in-field signal
- `0.9`: secondary venue, workshop, or solid but lower publication signal
- `0.6`: arXiv/preprint only, with no verified publication venue yet
- `0.3`: venue signal unclear or unverifiable

Optional adjustment:

- `+0.1`: oral, spotlight, best paper, or similarly verified distinction
- `-0.1`: venue mismatch, unclear publication claim, or weak verification

Clamp to `[0.0, 1.5]`.

## 2. Author and Affiliation Signal (`max 1.0`)

- `1.0`: multiple authors with strong prior track record in the same area, or clearly influential labs
- `0.8`: strong author signal with partial prior record in the area
- `0.6`: mixed signal; some relevant credibility but not clearly strong
- `0.4`: limited prior track record in this area
- `0.2`: insufficient verified information

Evidence should prefer:

- prior landmark papers
- author citation profiles
- institution / lab signal

## 3. Citation Traction (`max 2.0`)

Compute:

- `months_since_release = max(months, 6)`
- `citation_velocity = citation_count / months_since_release`

Compare the target paper to the 5-paper peer set plus the target itself.

Score by percentile of `citation_velocity`:

- top band -> `2.0`
- strong upper band -> `1.7`
- above-median -> `1.4`
- around median -> `1.1`
- below median -> `0.8`
- clearly weak -> `0.5`
- insufficient data -> `0.3`

Recency guardrail:

- Do not punish very recent papers purely for age.
- Always normalize by `max(months, 6)`.

## 4. Quality of Citing Papers (`max 1.5`)

Use a verified citing-paper sample when available.

- `1.5`: cited by multiple high-quality follow-up papers, strong venues, or influential surveys
- `1.2`: good citing-paper quality with clear external uptake
- `0.9`: mixed citing-paper quality
- `0.6`: weak or shallow citing signal
- `0.3`: insufficient citing-paper evidence

Recency guardrail:

- If the paper is under 6 months old and citing evidence is sparse, start from a neutral provisional score around `0.7` and adjust only with verified evidence.

## 5. Technical Novelty and Delta (`max 2.0`)

Score relative to the 5 comparison papers:

- `2.0`: clearly introduces a new paradigm, benchmark, framing, or method family
- `1.6`: strong technical delta with broad practical or conceptual consequences
- `1.2`: meaningful but bounded improvement or synthesis
- `0.8`: incremental change on existing methods
- `0.4`: weak novelty or mostly repackaging

Evidence should include:

- section-grounded claims from the paper
- peer-paper comparison points

## 6. Industry / Open-Source / Product Contribution (`max 1.0`)

- `1.0`: strong verified industry uptake, productization, benchmark adoption, or major open-source ecosystem impact
- `0.8`: one strong external adoption signal
- `0.5`: limited but credible contribution signal
- `0.2`: mostly speculative or internal-only signal
- `0.0`: no verified industry or ecosystem signal

## 7. Field-Shaping Potential (`max 1.0`)

This is a conservative forward-looking dimension.

- `1.0`: multiple strong signals that the paper may define a direction
- `0.8`: strong evidence of direction-setting potential
- `0.5`: promising, but not yet clearly shaping the field
- `0.2`: weak evidence
- `0.0`: no verified evidence

To score `0.8` or higher, require at least 2 of these:

- reused terminology / paradigm / benchmark
- follow-up work from multiple independent groups
- used as a baseline or reference point by strong papers
- meaningful industry or open-source adoption beyond the original authors
