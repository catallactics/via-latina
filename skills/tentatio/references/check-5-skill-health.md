# Check 5: Skill Health [INFO]

> Checks health of skill directories under `~/.claude/skills/`.

---

## 1. Scope

- **Scopes**: `full`, `skills`
- **Paths**: `~/.claude/skills/`
- **Exclude**: `~/.claude/skills/_common/` (shared references, not a skill)

---

## 2. Detection

| Sub-Check | What | How |
|-----------|------|-----|
| 5a: Broken References | Files referenced in SKILL.md don't exist | Grep `references/`, `scripts/`, `assets/` paths in SKILL.md → resolve relative → check exists |
| 5b: Outdated Skills | `updated:` field >6 months old | Parse date from frontmatter; only flag if field exists and is stale |
| 5c: Missing Frontmatter | Required fields absent | Check YAML frontmatter for `name`, `description` |

---

## 3. Classification

| Result | Sub-Check | Condition |
|--------|-----------|-----------|
| PASS | 5a | All referenced files exist |
| FAIL | 5a | Referenced file not found |
| PASS | 5b | `updated:` current or absent |
| FAIL | 5b | `updated:` >6 months old |
| PASS | 5c | Required frontmatter present |
| FAIL | 5c | Required frontmatter missing |

---

## 4. Auto-Fix

Report only. Skill fixes require developer judgment.

---

## 5. Edge Cases

- Package version checking: deferred (expensive)
- Self-check (tentatio checking itself): fine and expected
- Skills without `references/`: not an error
