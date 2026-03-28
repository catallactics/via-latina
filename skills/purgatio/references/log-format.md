# Log Format Reference

Format for recording AI processing results.

## Log Locations (Both Required)

| Log | Location | Purpose |
|-----|----------|---------|
| Global log | `90. SYSTEM/96. Data/Log-00-Processing.md` | Overall view |
| Per-project | `50-NNNNNN/logs/activity.md` | Project history |

## Global Log Format

File: `90. SYSTEM/96. Data/Log-00-Processing.md` (type: ai-log, tags: system/log, ai/processing)

### Daily Log Template

Each date section contains:
1. **Processed Quick Notes (N)**: `| Time | Title | Atomicity | Processing | Target |`
2. **Archive Moves**: `| Original | Refined |`
3. **Task Carry-Over**: `| From | Task | Tag | Status |`
4. **Daily Stats**: Processed/Archive/Projects/New projects/Split/Carried/Processing time
5. **Manual Review Required**: Items or "None"

## Per-Project Log Format

Follows `_common/references/logging-standard.md`. Purgatio-specific columns: `Time | Activity | Source | Changes`.

## Processing Types

| Type | Description | Display |
|------|-------------|---------|
| progress_update | Progress update | Progress update |
| note_added | Learning/idea note added | Note added |
| task_completed | Task completed | Task completed |
| reflection_added | Reflection added to daily note | Reflection added |
| essay_created | Essay promotion | Essay created |
| project_created | New project created | Project created |
| idea_added | Idea added | Idea added |
| meeting_prep | Meeting material created | Meeting prep |
| split | Split due to atomicity violation | Split (N items) |

## Atomicity Status

| Status | Display | Description |
|--------|---------|-------------|
| Atomic | ✓ | Single topic, no split needed |
| Split | ⚠️ | Split due to atomicity violation |
| User decision | ❓ | User decides split |

## Manual Review Required Cases

Cases logged as warnings:

1. **Uncertain project connection**
```markdown
- ⚠️ "New idea" - Could not find project to connect
```

2. **Atomicity split needed but user skipped**
```markdown
- ⚠️ "Multiple topics mixed" - Split suggestion ignored
```

3. **Overdue task**
```markdown
- 🔴 "Submit report" - Past deadline (2025-10-15)
```

4. **Long-pending task**
```markdown
- ⚠️ "Write meeting notes" - Incomplete for 5 days
```

## Monthly Summary (Optional)

Add as `## YYYY-MM Monthly Summary` on last day of month. Columns: Quick notes processed, Project updates, Essay promotions, New projects, Tasks completed. Add **Top projects** and **Trends** (avg daily, atomicity %, completion %).

