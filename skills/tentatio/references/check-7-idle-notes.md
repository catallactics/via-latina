# Check 7: Idle Notes [INFO]

> Detects long-idle notes in INCUBATING and CREATIONS folders.

---

## 1. Scope

- **Scopes**: `full`
- **Paths**: `10. INCUBATING/`, `70. CREATIONS/`
- **Exclude**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`, `README.md`

---

## 2. Detection

Glob all `.md` files → extract `created` and `status` from frontmatter → calculate age in days.

**Idle Thresholds:**

| Folder | Status | Threshold |
|--------|--------|-----------|
| `10. INCUBATING/` | `incubating` | 90 days |
| `70. CREATIONS/` | `draft` | 90 days |

**NOT flagged**: `active`, `refined`, `archived` statuses; notes without `created` (→ Metadata Gap instead).

---

## 3. Classification

| Result | Condition |
|--------|-----------|
| PASS | Age < threshold OR status not in idle set |
| IDLE | Age >= threshold AND status in idle set |
| Metadata Gap | Missing `created` |

---

## 4. Auto-Fix

Report only. User decides: develop, archive, or keep incubating.

---

## 5. Edge Cases

- No frontmatter → skip
- Date-only `created` (no time) → treat as midnight
- Nested subfolders (11. Ideas/, 72. Essays/) → included in scan
