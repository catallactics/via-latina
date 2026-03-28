# Task Carry-Over Reference

Logic for carrying incomplete tasks to next daily note.

## Core Principles

1. **Save point**: Daily note is like a game save file
2. **Context preservation**: Immediately see what was left incomplete yesterday
3. **Weekday/weekend distinction**: Carry only context-appropriate tasks

## Task Tag System

### Required Tags

| Tag | Description | Carry Condition |
|-----|-------------|-----------------|
| `#task/work` | Work-related | Weekdays (Mon-Fri) only |
| `#task/personal` | Personal daily | Always carry |
| `#task/hobby` | Hobby/side projects | Weekends (Sat-Sun) only |
| `#task/urgent` | Urgent | Always carry + highlight |

### Optional Tags

| Tag | Description |
|-----|-------------|
| `#task/deadline:YYYY-MM-DD` | Has deadline |
| `#task/project:ProjectName` | Connected project |
| `#task/waiting` | Waiting (don't carry) |
| `#task/someday` | Someday (don't carry) |

## Carry-Over Logic

### Step 1: Check Yesterday's Daily Note

```
60. JOURNAL/61. Daily Reviews/YYYY-MM-DD.md
```

Collect incomplete tasks from Tasks section:
```markdown
## Tasks
- [x] Completed task ← ignore
- [ ] Incomplete task #task/work ← collect
- [ ] Incomplete task #task/personal ← collect
```

### Step 2-3: Check Day Type & Filter

| Today | `#task/work` | `#task/personal` | `#task/hobby` | `#task/urgent` |
|-------|--------------|------------------|---------------|----------------|
| Weekday | ✓ Carry | ✓ Carry | ✗ Defer | ✓ Carry |
| Weekend | ✗ Defer | ✓ Carry | ✓ Carry | ✓ Carry |

**Deferred tasks**: Wait until matching day
- Friday work task → Carry on Monday
- Sunday hobby task → Carry next weekend

### Step 4: Execute Carry-Over

Add carry-over section to today's daily note:

```markdown
## Tasks

### Carried Over

> [!warning] Incomplete from yesterday
> - [ ] #task/work Write meeting notes ← [[2025-10-15]]
> - [ ] #task/personal Exercise ← [[2025-10-15]]

### Today
- [ ] 
```

## Frontmatter

Daily note frontmatter includes: `day_of_week`, `is_weekend`, `workday` (holidays: `is_holiday: true, workday: false`).

## Special Cases

| Case | Rule |
|------|------|
| Holidays | Treat as weekend |
| Long-pending (3+ days) | `⚠️ N days pending` + `[!warning]` callout asking user to act |
| Deadline today/past | Always carry + `🔴 Due today!` / `🔴 Overdue!` |
| `#task/waiting` | Don't carry, keep in original note |

## Record Format

Format: `- [ ] #task/work Task content (day N) ← [[Original date]]`. On completion: check `[x]` in current note, update project README if connected.
