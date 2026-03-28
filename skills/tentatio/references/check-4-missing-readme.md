# Check 4: Missing README in Active Projects [WARNING]

> Active projects must have README.md as navigation hub.

---

## 1. Scope

- **Scopes**: `full`, `project`
- **Paths**: `50. PROJECTS/51. Active/`

---

## 2. Detection

Glob `50. PROJECTS/51. Active/*/` → for each directory, check if `README.md` exists.

---

## 3. Classification

| Result | Condition |
|--------|-----------|
| PASS | README.md exists |
| FAIL | README.md missing |

---

## 4. Auto-Fix

Report only. README creation requires template + content knowledge (elevatio's scope).

---

## 5. Edge Cases

- Only checks `51. Active/` (other folders may have incomplete structure by design)
- Does not check README content quality (memorio's checklist scope)
- Checks immediate project root only, not sub-subdirectories
