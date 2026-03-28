# Check 3: Registry/Folder Sync [WARNING]

> Detects mismatches between Project Number Registry and actual folder structure.

---

## 1. Scope

- **Scopes**: `full`, `project`
- **Paths**: `50. PROJECTS/51. Active/`, `52. Someday/`, `53. Pending/`, `54. Archive/`, `90. SYSTEM/93. Registries/Project-Number-Registry.md`
- **Exclude**: `100. SECURE/**`

For `project` scope: check only specified project against registry.

---

## 2. Detection

**Implementation**:
1. Read `90. SYSTEM/93. Registries/Project-Number-Registry.md`
2. Parse Active table (`| Number | Name | Status | Created |`) and Archived table (`| Number | Name | Archived | Reason | Merged Into |`)
3. Extract Number column (e.g., `51a`, `51q`); match against folder name prefix (text before first `.` or space)
4. Glob `50. PROJECTS/5{1,2,3,4}. */*/` for actual folders
5. Compare: registry location vs actual folder location

---

## 3. Classification

| Result | Condition |
|--------|-----------|
| PASS | Registry and folder agree |
| FAIL (location mismatch) | Registry status ≠ actual folder |
| FAIL (unregistered) | Folder exists, no registry entry |
| FAIL (missing folder) | Registry entry, no matching folder |

---

## 4. Auto-Fix

Report only. Registry modifications require user judgment.

---

## 5. Edge Cases

- Projects in multiple locations → flag all
- Table format variations → handle both Active and Archived formats
- Special characters in folder names → use Glob results as-is
- Project number prefix → extract text before first `.` or space
