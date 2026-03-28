# Check 1: Folder/Status Sync [CRITICAL]

> Detects files whose `status:` frontmatter contradicts the folder they reside in.

---

## 1. Scope

- **Scopes**: `full`, `project`
- **Paths**: `50. PROJECTS/51. Active/`, `52. Someday/`, `53. Pending/`, `54. Archive/`
- **Exclude**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`

For `project` scope: scan only specified project folder.

---

## 2. Detection

**Expected Status by Folder:**

| Folder | Valid `status:` values |
|--------|----------------------|
| `51. Active/` | `active`, `refined`, `pending`, `draft` |
| `52. Someday/` | `someday` |
| `53. Pending/` | `waiting` |
| `54. Archive/` | `archived`, `refined` |

**Implementation**: For each project subfolder in `50. PROJECTS/`:
1. Determine parent folder → expected status set
2. Glob all `*.md` recursively; Grep `^status:` to extract value
3. If value NOT in expected set → flag as mismatch
4. Separate README.md (auto-fixable) from other files (report-only)

---

## 3. Classification

| Result | Condition |
|--------|-----------|
| PASS | Status matches expected set |
| FAIL (README) | README.md mismatch → auto-fixable |
| FAIL (file) | Individual file mismatch → report only |

---

## 4. Auto-Fix

README.md only: change status to match folder (e.g., `active` → `archived` in `54. Archive/`).
Individual notes: report only (status may be intentional historical record).
Method: `Edit` tool.

---

## 5. Edge Cases

- No `status:` field → skip
- `refined` valid in both Active and Archive
- `pending` valid in Active (awaiting processing within active project)
- Log files → skip (typically no status frontmatter)
