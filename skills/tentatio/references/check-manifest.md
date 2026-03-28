# Check Manifest

> Registry of all tentatio checks. SKILL.md Step 1 reads this file and filters by scope.

## Check Registry

| ID | Name | File | Severity | Auto-fix | Scopes | Added |
|----|------|------|----------|----------|--------|-------|
| 1 | Folder/Status Sync | check-1-folder-status.md | CRITICAL | Partial | full, project | 2026-02-19 |
| 2 | Non-Standard Status | check-2-status-values.md | WARNING | Partial | full | 2026-02-19 |
| 3 | Registry/Folder Sync | check-3-registry-sync.md | WARNING | No | full, project | 2026-02-19 |
| 4 | Missing README | check-4-missing-readme.md | WARNING | No | full, project | 2026-02-19 |
| 5 | Skill Health | check-5-skill-health.md | INFO | No | full, skills | 2026-02-19 |
| 6 | Knowledge Freshness | check-6-knowledge-freshness.md | INFO | Partial | knowledge | 2026-02-19 |
| 7 | Idle Notes | check-7-idle-notes.md | INFO | No | full | 2026-02-19 |
| 8 | Non-Standard Types | check-8-type-values.md | WARNING | Partial | full | 2026-03-04 |
| 9 | Difficulty Validation | check-9-difficulty-values.md | INFO | Partial | full, knowledge | 2026-03-10 |
| 10 | Session Follow-ups | check-10-session-followups.md | WARNING | No | sessions, full | 2026-03-10 |

## Scope Definitions

| Scope | Trigger | Checks |
|-------|---------|--------|
| `full` | `/tentatio` | 1, 2, 3, 4, 5, 7, 8, 9, 10 |
| `project` | `/tentatio 50x` | 1, 3, 4 |
| `skills` | `/tentatio --skills` | 5 |
| `knowledge` | `/tentatio --knowledge` | 6, 9 |
| `sessions` | `/tentatio --sessions` | 10 |

`--evolve` is not a check scope — triggers self-evolution (see SKILL.md).

**Lazy-loading**: Load only check files matching current scope, not all files.

## Standard Check Interface

Every `check-{N}-{slug}.md` has 5 sections: Scope, Detection, Classification, Auto-Fix, Edge Cases.
