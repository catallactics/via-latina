# Check 2: Non-Standard Field Values [WARNING]

> Detects field values not in official taxonomy.

---

## 1. Scope

- **Scopes**: `full`
- **Paths**: Vault-wide
- **Exclude**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`

---

## 2. Detection

**Valid values**: → See `_common/references/frontmatter-standard.md` for canonical lists.

| Field | Scan Scope | Valid Set |
|-------|------------|----------|
| `status` | Vault-wide | `inbox\|pending\|active\|refined\|archived\|logged\|someday\|waiting\|incubating\|on-hold\|reference\|draft` |
| `phase` | `50. PROJECTS/` | `planning\|development\|review\|complete` |
| `maturity` | `20. LITERATURE/`, `30. KNOWLEDGE/` | `foundational\|growing\|evergreen` |

**Implementation**: For each field, Grep `^{field}:` in scan scope → extract → normalize (strip quotes, trim) → check against valid set.

---

## 3. Classification

| Result | Condition | Action |
|--------|-----------|--------|
| PASS | Value in valid set | — |
| FAIL (deprecated) | Value in deprecated mapping | Auto-fix |
| FAIL (unknown) | Not valid and not deprecated | Report, suggest closest |

---

## 4. Auto-Fix

**Deprecated → standard** (1:1, safe to auto-fix):

| Found | Replace with |
|-------|-------------|
| `complete` | `refined` |
| `completed` | `refined` |
| `in-progress` | `active` |
| `open` | `active` |
| `ready` | `pending` |
| `developing` | `incubating` |

→ Full deprecated mapping also in `_common/references/frontmatter-standard.md`.

Non-deprecated unknown values and non-standard `phase`/`maturity`: report only, suggest closest.
Method: `Edit` tool.

---

## 5. Edge Cases

- Quoted values (`status: "active"`) → strip quotes
- Multiple spaces after colon → trim
- Empty field (`status:` no value) → flag as unknown
- YAML boolean coercion (`status: true`) → flag as unknown
