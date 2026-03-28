# Hierarchical Summarization Reference

> Bulk result processing to prevent hallucination (Lost in the Middle)

## Overview

10+ search results in single context → information loss, hallucination. Solution: dynamic batching (Map-Reduce).

---

## Constants

```python
SAFE_THRESHOLD = 30_000      # Max tokens per batch
AVAILABLE_TOKENS = 25_000    # Usable (after 5K system overhead)
MIN_BATCH_SIZE = 5
MAX_BATCH_SIZE = 50
```

**Token estimation**: English ≈ chars/4, Korean/CJK ≈ chars/2. Sample first 5 results.

---

## Batch Size Calculation

```python
base_size = AVAILABLE_TOKENS // avg_tokens
if variance > 0.5: base_size *= 0.7
batch_size = clamp(base_size, MIN=5, MAX=50)
```

| Avg Tokens | Variance | Batch |
|------------|----------|-------|
| 500 | low | 50 |
| 1000 | low | 25 |
| 2000 | low | 12 |
| 5000 | high | 5 (min) |

---

## Hierarchy Depth

```
results ≤ batch_size         → Level 1 (direct)
results ≤ batch_size × 5    → Level 2 (batch → final)
results > batch_size × 5    → Level 3 (batch → meta → final)
```

---

## Batch Prompt (INFORMATION PRESERVATION)

> **CRITICAL**: Extract ALL unique information. Only remove duplicates.

```markdown
Process these {N} results. Extract ALL unique information.

## Rules
1. Extract EVERY distinct piece of information
2. Do NOT compress or summarize — preserve full detail
3. Note source URL for EVERY point
4. Flag duplicates, conflicts, and outdated info (pre-2024)

## Output
### Extracted Information
| # | Source URL | Information | Category | Notes |

### Duplicates Found / Conflicts Detected / Information Count
Total: {N}, Unique: {M}, Retention: {%} (target >90%)
```

**Item ordering** (Lost in the Middle mitigation): Place important items at start and end.

---

## Meta Summary (Level 3)

Synthesize batch summaries: Unified Key Insights (deduplicated), Source Quality Assessment, Consensus vs Controversy, Gaps. Preserve source URLs, confidence levels, contradiction flags.

---

## Final Synthesis

Create Quick Note with logical structure (not concatenation), deduplicated, attributed, uncertainty-marked, comprehensive.

---

## User Notification

```
📊 대량 결과 처리 모드: {N}개 결과 | ~{T} tokens/item | 배치: {B}개×{count} | Level {L}
[계속 / 조정 / 취소]
```

---

## Skip Conditions

Results <10 | Single source | `--no-batch` | Total tokens <25K | Image/video analysis

## Quality Check

- [ ] No info loss (spot-check) | Source attribution preserved | Conflicts noted | Output coherent
