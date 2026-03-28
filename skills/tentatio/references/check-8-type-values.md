# Check 8: Non-Standard Type Values [WARNING]

> Detects type values not in the official taxonomy.

---

## 1. Scope

- **Scopes**: `full`
- **Paths**: Vault-wide
- **Exclude**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`

---

## 2. Detection

**Valid values**: → See `_common/references/type-standard.md` for canonical list.

**Core (15)**: `session-summary|quick-note|indagatio|daily|learning-note|guide|literature|meeting|project|changelog|reference|log|plan|concept|idea`

**Special (13)**: `essay|moc|dashboard|people|template|refined-note|analysis|checklist|weekly-review|knowledge|verification-report|hold-record|commentatio-synthesis`

**Implementation**: Grep `^type:` vault-wide → extract → normalize (strip quotes, trim) → check against valid set. Flag empty fields.

---

## 3. Classification

| Result | Condition | Action |
|--------|-----------|--------|
| PASS | Value in valid set | — |
| FAIL (deprecated) | Value in migration mapping | Auto-fix |
| FAIL (empty) | No value after `type:` | Infer from folder |
| FAIL (unknown) | Not valid, not deprecated | Report, suggest closest |

---

## 4. Auto-Fix

### Deprecated Type Mapping

| Found | Replace with | Additional |
|-------|-------------|------------|
| `system-guide`, `system-doc`, `system-note`, `master-guide`, `technical-guide`, `quickstart` | `guide` | — |
| `literature-note` | `literature` | — |
| `meeting/post`, `meeting/prep`, `meeting/brief`, `meeting/agd` | `meeting` | Add `meeting_role: {subtype}` |
| `meeting-series` | `meeting` | — |
| `project-note`, `project-report`, `project-update`, `project-document` | `project` | — |
| `update-log`, `update`, `update-summary` | `changelog` | — |
| `reference-note`, `quick-reference` | `reference` | — |
| `activity-log`, `folder-log`, `work-log`, `ai-log` | `log` | — |
| `strategy`, `creative-brief` | `plan` | — |
| `brainstorm` | `idea` | — |
| `learning-roadmap`, `learning-log` | `learning-note` | — |

### Empty Type Inference (user confirmation required)

| Folder Pattern | Inferred Type |
|----------------|---------------|
| `00. INBOX/06. GenAI Chats/` | `session-summary` |
| `00. INBOX/01. Quick Notes/` | `quick-note` |
| `60. JOURNAL/61. Daily Reviews/` | `daily` |
| `60. JOURNAL/62. Weekly Reviews/` | `weekly-review` |
| `65. Meeting Records/` | `meeting` |
| `50. PROJECTS/` (README.md) | `project` |
| `20. LITERATURE/` | `literature` |
| `30. KNOWLEDGE/` | `knowledge` |
| `40. MAPS/` | `moc` |
| `90. SYSTEM/91. Templates/` | `template` |
| `90. SYSTEM/94. System Guides/` | `guide` |
| `10. INCUBATING/` | `idea` |
| `70. CREATIONS/` | `essay` |

Method: `Edit` tool.

---

## 5. Edge Cases

- Quoted values → strip quotes
- Multiple spaces → trim
- Empty type field → attempt folder inference
- Meeting subtypes with slash (`meeting/post`) → migrate to separate `meeting_role` field
