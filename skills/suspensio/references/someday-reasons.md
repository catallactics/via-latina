# Someday Reasons Reference

Detailed descriptions of suspension reasons for suspensio skill.

## Reason Categories

### low_priority

**Description**: Project is valid but other work takes precedence.

**Examples**:
- "I want to finish Project A first"
- "This is nice-to-have, not urgent"
- "Will revisit after current sprint"

---

### waiting_for

**Description**: Blocked by external dependency outside your control.

**Examples**:
- "Waiting for API access approval"
- "Need client feedback first"
- "Dependent on library v2 release"

---

### blocked

**Description**: Technical or resource constraint prevents progress.

**Examples**:
- "Need to learn X skill first"
- "Hardware not available"
- "Budget not approved"

---

### exploring_alternatives

**Description**: Considering whether different approach is better.

**Examples**:
- "Might use different framework"
- "Evaluating buy vs build"
- "Reconsidering scope"

**Resume trigger**: When decision is made.

---

### time_constraint

**Description**: Simply no bandwidth currently.

**Examples**:
- "Too busy with work"
- "Personal matters taking priority"
- "Will pick up next quarter"

**Resume trigger**: When schedule allows.

---

## Usage in SUSPEND.md

```yaml
---
suspended: 2025-12-01
reason: waiting_for
---
```

The frontmatter reason should be one of:
- `low_priority`
- `waiting_for`
- `blocked`
- `exploring_alternatives`
- `time_constraint`

User's actual words go in the body under "일시 중단 이유" section.
