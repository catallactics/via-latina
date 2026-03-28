---
name: dilatio
description: "Obsidian project pending skill (Dilatio = delay/postponement). Moves active projects from 51. Active/ to 53. Pending/ when waiting for external triggers. Triggers on 'pending project', 'waiting for', '/dilatio', '이 프로젝트 대기', '외부 트리거 대기'. Preserves project number and original state, creates PENDING.md with trigger conditions. Use when a project can't proceed until something external happens (API release, hardware arrival, partner response)."
argument-hint: "[[project]]"
disable-model-invocation: true
---

# Dilatio

> *Dilatio* (Latin): delay, postponement, deferment

Skill for moving active Obsidian projects to Pending status (waiting for external triggers).

## Trigger Patterns

**Slash commands:**
- `/dilatio`
- `/dilatio [[50x]]`
- `/dilatio 50w`

**Natural language (English):**
- "Move to pending"
- "Waiting for [X]"
- "Put on hold until [X]"
- "External dependency"

**Natural language (Korean):**
- "이 프로젝트 대기"
- "pending으로 이동"
- "외부 대기 중"
- "[X] 나오면 다시 할 거야"
- "[X] 출시되면 재개"

## Core Principles

See `../_common/references/frontmatter-standard.md` for shared principles (number preserved, links preserved, frontmatter synced).

1. **PENDING.md created**: Separate file with pending reason + trigger conditions
2. **History in Registry**: Pending reason and date recorded in central registry
3. **External trigger focus**: Use for definite external events, NOT internal priority decisions

## Difference from Suspensio

| Aspect | Dilatio (Pending) | Suspensio (Someday) |
|--------|-------------------|---------------------|
| Destination | 53. Pending | 52. Someday |
| Trigger type | **External event** | Internal decision |
| Certainty | Will definitely resume | May or may not resume |
| Examples | New release, client feedback, API approval | Low priority, no time, exploring |
| File created | PENDING.md | SUSPEND.md |
| Resume trigger | When event occurs | When I decide |

**Key distinction**:
- **Dilatio**: "I'm waiting for X to happen" (external, certain)
- **Suspensio**: "I'm choosing to pause this" (internal, uncertain)

## Relationship with Other Skills

See `../suspensio/SKILL.md` for shared skill relationship diagram.

| Skill | Direction | Trigger Type | Resume Certainty |
|-------|-----------|--------------|------------------|
| suspensio | Active → Someday | Internal (my decision) | Uncertain |
| dilatio | Active → Pending | External (event) | Certain |
| memorio | Active → Archive | - | No resume expected |
| vivificatio | Any → Active | - | - |

---

## Core Workflow

### Step 1: Project Verification

Verify user-specified project:
1. Identify by project name or number (e.g., "50w", "API-Migration")
2. Find corresponding folder in `50. PROJECTS/51. Active/`
3. Verify README.md exists

### Step 2: Collect Pending Reason

Ask user for pending reason:
- "What external event are you waiting for?"
- Capture user's exact words

**Reason categories:**

| Reason | Description | Examples |
|--------|-------------|----------|
| awaiting_release | External release/launch | New collection, software version, product launch |
| awaiting_feedback | Feedback/review wait | Client confirmation, team review, approval |
| awaiting_resource | Resource availability | Budget approval, equipment, personnel |
| awaiting_dependency | Dependency resolution | Other project completion, API availability |
| awaiting_schedule | Scheduled date/event | Specific date, season, event |

See `references/pending-reasons.md` for detailed descriptions.

### Step 3: Collect Trigger Condition

Ask user for specific trigger:
- "What exactly needs to happen for you to resume this project?"
- This should be a **specific, observable event**

**Good trigger examples:**
- "Python 4.0 정식 출시"
- "Client approves mockup"
- "Python 4.0 released"
- "Budget approved for Q2"

### Step 4: Collect Expected Timing (Optional)

Ask user for expected timing:
- "When do you expect this trigger to occur? (optional)"
- Format: YYYY-MM or descriptive (e.g., "Q2 2026", "March", "After conference")

### Step 5: Create PENDING.md

Create `PENDING.md` in project root (before moving):

```markdown
---
pending: YYYY-MM-DD
reason: [awaiting_release|awaiting_feedback|awaiting_resource|awaiting_dependency|awaiting_schedule]
expected: YYYY-MM
---

# ⏳ Pending: [Project Name]

## 대기 이유

> **사용자**: [User's exact words]

## 트리거 조건

[Specific event that will trigger resumption]

## 예상 시점

[When the trigger is expected to occur]

## 메모

[Optional notes]
```

**Location**: Project root, alongside README.md

### Step 6: Log to Project Activity

**Before moving**, record in project's `logs/activity.md`:

```markdown
## YYYY-MM-DD

| Time | Skill | Activity | Details |
|------|-------|----------|---------|
| HH:mm | [dilatio] | Project moved to pending | reason: [reason], trigger: [trigger] |
```

### Step 7: Batch-Update Child Frontmatter

Update `status:` in all child files before moving.

See `../_common/references/child-frontmatter-update.md` for procedure.

**Dilatio-specific:**
- Target status: `waiting`
- Do not preserve `refined` (all statuses change to `waiting`)
- README.md meta field: `pending_date: YYYY-MM-DD`

### Step 8: Execute Move

Move folder. Child file frontmatter already updated in previous step.

```
Before: 50. PROJECTS/51. Active/50w. API-Migration/
After:  50. PROJECTS/53. Pending/50w. API-Migration/
```

**Important:**
- Keep folder name (including number) exactly the same
- Only PENDING.md is new file added

### Step 9: Registry Update

**Update `90. SYSTEM/93. Registries/Project-Number-Registry.md`:**

See `../_common/references/registry-format.md` for detailed format.

1. Change status to `pending` in "Active Projects" table
2. Add to "Pending Projects" section (create if doesn't exist):
   - Project number
   - Project name
   - Pending date
   - Trigger condition (brief)

**Pending Projects section format:**

```markdown
### Pending Projects (53. Pending/)

| Number | Name | Status | Pending Since | Trigger |
|--------|------|--------|---------------|---------|
| 50-000006 | API-Migration | pending | 2025-12-01 | API v3 정식 출시 |
```

### Step 10: Report to Daily Note

Call purgatio in report-only mode to record in Daily Note.

**Report data:**
```yaml
skill: dilatio
action: project_pending
summary:
  project_number: "50w"
  project_name: "API-Migration"
  reason: "awaiting_release"
  trigger: "API v3 정식 출시"
  expected: "2026-03"
  pending_md_created: true
```

See `~/.claude/skills/purgatio/references/skill-report-format.md` for format.

---

## User Confirmation Points

1. Confirm project to move to pending
2. Pending reason (user's words)
3. Trigger condition (specific event)
4. Expected timing (optional)

## Reference Files

- `../_common/references/child-frontmatter-update.md` - Batch child frontmatter update procedure
- `references/pending-reasons.md` - Detailed reason descriptions
- `../_common/references/registry-format.md` - Registry update format
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

## Integration

### Inbound
- **user**: Direct call via `/dilatio` or "waiting for [X]"

### Outbound
- **purgatio**: Report pending status (report-only mode) for Daily Note

### Related Skills
- **suspensio**: Internal pause (Active → Someday) - for internal decisions
- **memorio**: Full archive (Active → Archive) - for completed/discontinued
- **vivificatio**: Restore (Pending → Active) - inverse operation

