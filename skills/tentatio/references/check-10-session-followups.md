# Check 10: Session Follow-up Tracker

> Detects incomplete action items (`- [ ]`) in AI session summaries that may have been forgotten.

## Scope

- **Scopes**: `sessions`, `full`
- **Trigger**: `/tentatio --sessions` or `/tentatio --sessions 03` (month filter)
- **Target folder**: `00. INBOX/06. AI Sessions/`
- **Target type**: `session-summary`

## Detection

### Rule A: Stale Incomplete Tasks
- **Condition**: File has `- [ ]` items AND file `mtime` is >7 days old
- **Severity**: WARNING
- **Rationale**: Session summaries with unchecked tasks older than 7 days likely represent forgotten follow-ups

### Rule B: High Incomplete Count
- **Condition**: File has 5+ unchecked `- [ ]` items
- **Severity**: INFO
- **Rationale**: Sessions with many outstanding items may need prioritization or deliberate dismissal

### Rule C: No Follow-up Section
- **Condition**: File has `type: session-summary` but no `## ⏳ 미완료` or `## 📌 다음 세션 참고` section
- **Severity**: INFO
- **Rationale**: Session may have ended without proper follow-up documentation

## Classification

| Category | Threshold | Action |
|----------|-----------|--------|
| Overdue | `- [ ]` + mtime >30 days | WARNING: "30일 초과 미완료 {N}건 — 처리 또는 의도적 취소(`- [x]` 또는 삭제) 필요" |
| Stale | `- [ ]` + mtime 7-30 days | WARNING: "미완료 {N}건 — 다음 세션에서 처리 권장" |
| Recent | `- [ ]` + mtime <7 days | — (skip, still fresh) |
| Bulk | 5+ `- [ ]` in single file | INFO: "미완료 {N}건 집중 — 우선순위 분류 권장" |

## Auto-Fix

- **None**: Session follow-ups require human judgment (complete, delegate, or deliberately cancel)
- **Report only**: Generates summary table of all outstanding items across sessions

### Report Format
```markdown
## Session Follow-up Summary

| File | Age | Incomplete | Items |
|------|-----|------------|-------|
| 2026-03-06 - Project Setup... | 4d | 7 | config 자동수정, 테스트 실행... |
| 2026-03-04 - 퀵노트... | 6d | 5 | 아카이브 후보 15건... |
```

## Edge Cases

- Files with `status: archived` → skip (deliberately closed)
- Files with only `- [x]` (all complete) → skip
- `- [ ]` inside code blocks or callout examples → skip (not real tasks)
  - Detection: ignore lines where `- [ ]` appears after `>` followed by code fence or inside ` ``` ` blocks
- Sub-trigger `--sessions 03` → filter to `2026/03/` subfolder only
