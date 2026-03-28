# Check 9: Difficulty Validation [INFO]

> Validates difficulty metadata across learnable content for missing, out-of-range, or mismatched values.

---

## 1. Scope

- **Scopes**: `full`, `knowledge`
- **Paths**: `00. INBOX/`, `10. INCUBATING/`, `20. LITERATURE/`, `30. KNOWLEDGE/`, `50. PROJECTS/51. Active/`, `90. SYSTEM/94. System Guides/`
- **Exclude**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`, `README.md`, `*-Log.md`, `ARCHIVE.md`, `SUSPEND.md`, `PENDING.md`
- **Eligible types**: `quick-note`, `idea`, `indagatio`, `learning-note`, `literature`, `reference`, `guide`, `knowledge`, `concept`, `refined-note`, `analysis`

---

## 2. Detection

### Step 1: Read Frontmatter

For each `.md` file in scope, extract: `type`, `difficulty`, `maturity`, `tags`, `status`.

Skip files where `type` is not in eligible types list.

### Step 2: Validate Difficulty

**Rule A — Missing difficulty**: File has eligible type but no `difficulty:` field.
- Severity: INFO (difficulty is optional, Learning Queue infers from type+maturity+tags)
- Report only, no auto-fix (inference handles this)

**Rule B — Out-of-range**: `difficulty:` value is < 1 or > 10, or non-integer.
- Severity: WARNING
- Auto-fix: clamp to nearest valid value (0→1, 11→10, 4.5→5)

**Rule C — Type-difficulty mismatch**: Statistically improbable combinations.

| Type | Expected Range | Flag if |
|------|---------------|---------|
| quick-note, idea | 1-4 | d >= 7 |
| indagatio | 2-5 | d >= 8 |
| learning-note | 3-6 | d >= 9 |
| literature, reference, guide | 3-7 | d >= 9 or d <= 1 |
| knowledge, concept, refined-note | 4-8 | d <= 1 |
| analysis | 5-10 | d <= 2 |

- Severity: INFO (flags for review, not auto-fixed — user may intentionally set unusual values)

---

## 3. Classification

| Status | Condition | Action |
|--------|-----------|--------|
| Valid | difficulty in 1-10 AND within expected range | No action |
| Missing | No difficulty on eligible type | Informational |
| Out-of-range | difficulty < 1 or > 10 | Auto-fix (clamp) |
| Mismatch | Type-difficulty combination flagged | Review suggestion |

---

## 4. Auto-Fix

| Fix | Condition | Method |
|-----|-----------|--------|
| Clamp value | Out-of-range | `Edit` tool — set to clamped value |

NOT auto-fixed: missing difficulty (Learning Queue inference handles this), type-difficulty mismatches (may be intentional).

---

## 5. Edge Cases

- `status: archived` → skip (not in active learning)
- Files without `type:` → skip
- Multiple tags pulling difficulty in different directions → report only, no judgment
- Project README.md → skip (not learnable content)
- `difficulty: 0` → clamp to 1

---

## Report

> See `references/report-template.md` for output format.
