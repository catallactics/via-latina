# Check 6: Knowledge Freshness [INFO]

> Classifies knowledge notes by obsolescence rate and flags overdue reviews.

---

## 1. Scope

- **Scopes**: `knowledge`
- **Paths**: `30. KNOWLEDGE/`, `20. LITERATURE/`
- **Exclude**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`, `20. LITERATURE/22. Courses/**`, `README.md`, `*-Log.md`
- **Sub-trigger**: `/tentatio --knowledge 33` → scan only `33. Engineering & Tech/`

---

## 2. Detection

### Step 1: Read Frontmatter

For each `.md` file in scope, extract: `created`, `domain`, `subdomain`, `type`, `maturity`, `tier`, `review_by`.

Validate `domain` against: `humanities-arts|social-sciences|engineering-tech|natural-sciences|medical-sciences|agricultural-sciences`. Flag invalid as WARNING.

### Step 2: Determine Tier

Priority (first match wins):
1. **User-set `tier:`** → use directly (always final)
2. **Type override**: `research-paper` → `fast-decay`; `tool-guide` → `medium-decay`
3. **Tag override**: any tag matching `ai/*`, `ml/*`, `llm/*`, `ai-*`, `tool/claude-code`, `tool/chatgpt`, `tool/cursor` → `rapid-decay`
4. **Domain/subdomain inference** (see below)
5. **Fallback** → "tier inference failed" in metadata gap section

**Domain → Tier Defaults:**

| Domain | Default | Subdomain Overrides |
|--------|---------|---------------------|
| humanities-arts | timeless | all subdomains → timeless |
| social-sciences | slow-decay | psychology, sociology → timeless |
| engineering-tech | medium-decay | ai, ml, llm → rapid-decay; protocols → slow-decay |
| natural-sciences | slow-decay | mathematics → timeless |
| medical-sciences | slow-decay | — |
| agricultural-sciences | slow-decay | — |

Unlisted subdomain → use domain default. Unknown domain → Metadata Gap.

**Maturity Override** (applies to rules 2-5 only, NOT user-set tier):
`maturity: evergreen` promotes tier one level: rapid→fast, fast→medium, medium→slow, slow→timeless.

### Step 3: Calculate Age

```
age_months = (current_date - created) in months

Thresholds: timeless→none, slow-decay→24mo, medium-decay→12mo, fast-decay→6mo, rapid-decay→3mo
```

---

## 3. Classification

| Status | Condition | Action |
|--------|-----------|--------|
| Valid | `timeless` OR age < threshold | No action |
| Early Warning (rapid) | `rapid-decay` AND age 1.5-3 months | Informational |
| Early Warning (fast) | `fast-decay` AND age 3-6 months | Informational |
| Needs Refresh | `slow/medium-decay` AND age > threshold | Verify details |
| Needs Re-research | `rapid-decay` AND age > 3 months | Rewrite/replace |
| Needs Re-research | `fast-decay` AND age > 6 months | Rewrite/replace |
| Incomplete | `maturity: growing` AND age > 6 months | Finish note |
| Metadata Gap | Missing `domain` or `created` | Report for manual fix |

---

## 4. Auto-Fix

All fixes require user confirmation (AskUserQuestion):

| Fix | Condition | Method |
|-----|-----------|--------|
| Add `review_by` | Overdue note without `review_by` | `Edit` tool — set to today + threshold |
| Add `tier` | Inferred tier not yet persisted | `Edit` tool — write inferred value |

NOT auto-fixed: content updates, status transitions (report classification communicates needed action).

---

## 5. Edge Cases

- Missing `domain`/`created` → Metadata Gap
- User `tier:` conflicts with inference → user wins
- README.md and Log files → skip
- LITERATURE without `domain` → infer from folder path if possible
- `review_by` in future → always valid regardless of age
- `maturity: evergreen` + user-set `tier: fast-decay` → respect user tier

---

## Report

→ See `references/report-template.md` Section 6 and 학습 우선순위 가이드 for output format.
