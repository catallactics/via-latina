---
name: suspensio
description: "Obsidian project suspension skill (Suspensio = suspension). Moves active projects from 51. Active/ to 52. Someday/. Triggers on 'suspend project', 'someday', '/suspensio', '이 프로젝트 일시 중단', '나중에 할게'. Preserves project number and original state, creates SUSPEND.md with reasoning. Use when you're choosing to pause a project internally (lost motivation, priority shift), not waiting for an external event (that's dilatio)."
argument-hint: "[[project]]"
disable-model-invocation: true
---

# Suspensio

> *Suspensio* (Latin): suspension, pause, temporary hold

Skill for suspending active Obsidian projects (moving to Someday).

## Trigger Patterns

- "Suspend this project"
- "Move to someday"
- "Put on hold"
- `/suspensio`
- `/suspensio [[50x]]`
- "이 프로젝트 나중에"
- "someday로 이동"
- "일시 중단"

## Core Principles

See `../_common/references/frontmatter-standard.md` for shared principles (number preserved, links preserved, frontmatter synced).

1. **SUSPEND.md created**: Separate file with suspension reason + resume conditions
2. **History in Registry**: Suspension reason and date recorded in central registry
3. **Lighter than memorio**: No research validation, simpler workflow

## Relationship with Other Skills

| Skill | Direction | Use Case | Trigger Type |
|-------|-----------|----------|--------------|
| suspensio | Active → Someday | Temporary pause, low priority | Internal decision |
| dilatio | Active → Pending | External trigger wait | External event |
| memorio | Active → Archive | Completed or discontinued | - |
| vivificatio | Someday/Pending/Archive → Active | Restore and resume | - |

---

## Core Workflow

### Step 1: Project Verification

Verify user-specified project:
1. Identify by project name or number (e.g., "50c", "Project-Name")
2. Find corresponding folder in `50. PROJECTS/51. Active/`
3. Verify README.md exists

### Step 2: Collect Suspension Reason

Ask user for suspension reason:
- "Why are you suspending this project?"
- Capture user's exact words

**Reason categories:**

| Reason | Description |
|--------|-------------|
| low_priority | Other projects take precedence |
| waiting_for | Waiting for external dependency |
| blocked | Technical or resource blocker |
| exploring_alternatives | Considering different approaches |
| time_constraint | No time currently |

See `references/someday-reasons.md` for detailed descriptions.

### Step 3: Collect Resume Conditions

Ask user when they expect to resume:
- "What conditions would trigger resuming this project?"
- Examples: "When X feature is released", "Next quarter", "When I have more time"

### Step 4: Create SUSPEND.md

Create `SUSPEND.md` in project root (before moving):

```markdown
---
suspended: YYYY-MM-DD
reason: [low_priority|waiting_for|blocked|exploring_alternatives|time_constraint]
---

# ⏸️ Suspended: [Project Name]

## 일시 중단 이유

> **사용자**: [User's exact words]

## 재개 조건

[Resume conditions from Step 3]

## 메모

[Optional notes]
```

**Location**: Project root, alongside README.md

### Step 5: Log to Project Activity

**Before moving**, record in project's `logs/activity.md`:

```markdown
## YYYY-MM-DD

| Time | Activity | Source | Changes |
|------|----------|--------|---------|
| HH:mm | [suspensio] suspended | - | reason: [reason], SUSPEND.md created |
```

### Step 6: Batch-Update Child Frontmatter

Update `status:` in all child files before moving.

See `../_common/references/child-frontmatter-update.md` for procedure.

**Suspensio-specific:**
- Target status: `someday`
- Do not preserve `refined` (all statuses change to `someday`)
- README.md meta field: `suspended_date: YYYY-MM-DD`

### Step 7: Execute Suspension

Move folder. Child file frontmatter already updated in previous step.

```
Before: 50. PROJECTS/51. Active/50-NNNNNN. Project-Name/
After:  50. PROJECTS/52. Someday/50-NNNNNN. Project-Name/
```

**Important:**
- Keep folder name (including number) exactly the same
- Only SUSPEND.md is new file added

### Step 8: Registry Update

**Update `90. SYSTEM/93. Registries/Project-Number-Registry.md`:**

See `../_common/references/registry-format.md` for detailed format.

1. Change status to `someday` in "Active Projects" table
2. Add to "Someday Projects" section (create if doesn't exist):
   - Project number
   - Project name
   - Suspension date
   - Reason (brief)

### Step 9: Report to Daily Note

Call purgatio in report-only mode to record suspension in Daily Note.

**Report data:**
```yaml
skill: suspensio
action: project_suspended
summary:
  project_number: "50c"
  project_name: "Project-Name"
  reason: "low_priority"
  suspend_md_created: true
```

See `~/.claude/skills/purgatio/references/skill-report-format.md` for format.

---

## User Confirmation Points

1. Confirm project to suspend
2. Suspension reason (user's words)
3. Resume conditions

## Reference Files

- `../_common/references/child-frontmatter-update.md` - Batch child frontmatter update procedure
- `references/someday-reasons.md` - Detailed reason descriptions
- `../_common/references/registry-format.md` - Registry update format
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

## Integration

### Inbound
- **user**: Direct call via `/suspensio` or "suspend project"

### Outbound
- **purgatio**: Report suspension (report-only mode) for Daily Note

### Related Skills
- **dilatio**: External trigger wait (Active → Pending) - for external events
- **memorio**: Full archive (Active → Archive) - for completed/discontinued
- **vivificatio**: Restore (Someday/Pending/Archive → Active) - inverse operation

