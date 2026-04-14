# Comparison Selection Policy

The comparison set must contain exactly 5 arXiv papers:

- 3 recent papers from the last 2 years
- 2 classic papers in the same field

## Theme Matching Rules

A peer paper must match at least 2 of the following 4 signals:

1. same task or problem setting
2. same method family
3. same modality or data setting
4. same evaluation goal or benchmark style

## Recent-Paper Rules

- Must be from the last 2 years relative to the current date
- Prefer papers from different author groups
- Prefer papers with verified venue or citation metadata
- Avoid duplicate arXiv versions of the same work

## Classic-Paper Rules

- No date limit
- Must be foundational, highly reused, or widely cited in the area
- Prefer papers that the target paper or recent peer papers implicitly build on

## Selection Procedure

1. Extract keywords from the target paper title and abstract.
2. Infer the comparison theme in one short phrase.
3. Search arXiv candidates by title keywords, area keywords, and method keywords.
4. Filter candidates using the overlap rules above.
5. Pick 3 recent papers and 2 classics.
6. Record why each paper belongs in the set.

## Required Output Fields Per Peer Paper

- `peer_id`
- `type` (`recent` or `classic`)
- `title`
- `arXiv URL`
- `year`
- `venue`
- `citation_count`
- `selection_reason`

## Failure Handling

If you cannot verify a candidate online, do not include it.

If fewer than 5 verified peer papers remain:

- state `comparison_status=incomplete`
- reduce confidence to `low`
- cap the final score at `7.4/10.0`
