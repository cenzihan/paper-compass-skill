# Memory Format Guide

`paper-compass-learnpath` accepts flexible `memory.md` formats.

## Preferred Format

```markdown
- Transformer Encoder: familiar
- Position Embedding: basic
- LoRA: mastered
- Quantization: unknown
```

Supported levels:

- `mastered`
- `familiar`
- `basic`
- `unknown`

## Also Acceptable

### Checklist style

```markdown
- [x] LoRA
- [ ] Quantization
```

Parsing rule:

- `[x]` -> `familiar` (or higher if explicit)
- `[ ]` -> `unknown`

### Table style

```markdown
| Topic | Level |
|---|---|
| Transformer Encoder | familiar |
| LoRA | mastered |
```

## Normalization Rules

- Case-insensitive matching
- Ignore punctuation differences where possible
- Keep the original phrase as alias for matching paper terms

## Conflict Resolution

- If the same concept appears multiple times with different levels, use the highest-confidence explicit level in this order:
  - `mastered` > `familiar` > `basic` > `unknown`
- If no explicit level can be inferred, default to `unknown`

