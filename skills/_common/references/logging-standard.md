# Unified Logging Standard

> Central reference for consistent logging across all Via Latina skills.

---

## Logging Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Central Logs                            │
├─────────────────────────────────────────────────────────────┤
│  Indagatio-Log.md        → External source URL tracking     │
│  Log-00-Processing.md    → purgatio processing history      │
└─────────────────────────────────────────────────────────────┘
                              +
┌─────────────────────────────────────────────────────────────┐
│                    Data Logs (96. Data/)                    │
├─────────────────────────────────────────────────────────────┤
│  Log-10-Incubating.md    → Ideas/Themes activity            │
│  Log-20-Literature.md    → Literature notes activity        │
│  Log-30-Knowledge.md     → Knowledge notes (Dataview)       │
│  Log-70-Creations.md     → Creative outputs activity        │
└─────────────────────────────────────────────────────────────┘
                              +
┌─────────────────────────────────────────────────────────────┐
│                    Project-Level Logs                       │
├─────────────────────────────────────────────────────────────┤
│  {project}/logs/activity.md  → Per-project activity         │
└─────────────────────────────────────────────────────────────┘
```

---

## Log Locations

| Log Type | Location | Purpose |
|----------|----------|---------|
| **Central: indagatio** | `00. INBOX/01. Quick Notes/Indagatio-Log.md` | URL duplicate check |
| **Central: purgatio** | `90. SYSTEM/96. Data/Log-00-Processing.md` | Processing overview |
| **Data: INCUBATING** | `90. SYSTEM/96. Data/Log-10-Incubating.md` | Ideas/themes tracking |
| **Data: LITERATURE** | `90. SYSTEM/96. Data/Log-20-Literature.md` | Literature notes tracking |
| **Data: KNOWLEDGE** | `90. SYSTEM/96. Data/Log-30-Knowledge.md` | Knowledge notes (Dataview) |
| **Data: CREATIONS** | `90. SYSTEM/96. Data/Log-70-Creations.md` | Creative outputs tracking |
| **Project** | `{project}/logs/activity.md` | Project-specific history |

---

## Project Activity Log Format

### File Location

```
50. PROJECTS/51. Active/{project}/logs/activity.md
```

### Template

```markdown
---
created: YYYY-MM-DD
type: activity-log
project: {Project-Name}
tags:
  - log/activity
---

# Activity Log

> Auto-updated by Via Latina skills.

---

## YYYY-MM-DD

| Time | Skill | Activity | Details |
|------|-------|----------|---------|
| HH:mm | [skill] | Activity type | Specific changes |

---
```

### Entry Format

```markdown
| HH:mm | [skill-name] | Activity | Details |
```

**Skill Tags (REQUIRED):**
- `[indagatio]` - External source analysis
- `[purgatio]` - Quick note processing
- `[elevatio]` - Project creation
- `[commentatio]` - Latin callout synthesis
- `[unio]` - Project merge
- `[suspensio]` - Project suspension (Active → Someday)
- `[dilatio]` - Project moved to pending (Active → Pending)
- `[memorio]` - Project archive
- `[vivificatio]` - Project restoration
- `[retrospectio]` - Periodic review
- `[locatio]` - Note placed on hold (Quick Notes → On-Hold)
- `[figuratio]` - Diagram generation from note
- `[auscultatio]` - Audio transcription

**Activity Types:**
| Type | Description |
|------|-------------|
| Project created | New project from note |
| Note refined | Quick note → refined note |
| Note added | New note added to project |
| Progress updated | README progress section updated |
| Synthesis created | commentatio output |
| Project absorbed | unio merge target |
| Project merged into | unio source |
| Project suspended | suspensio (Active → Someday) |
| Project moved to pending | dilatio (Active → Pending) |
| Project archived | memorio completion |
| Project restored | vivificatio revival |

---

## Folder-Level Log Format

### File Location

```
90. SYSTEM/96. Data/Log-{XX}-{Name}.md
```

### Template

```markdown
---
created: YYYY-MM-DD
type: folder-log
folder: {Folder-Name}
tags:
  - log/folder
---

# {Folder-Name} Activity Log

> Tracks skill activity within this folder.

---

## YYYY-MM-DD

| Time | Skill | File | Activity |
|------|-------|------|----------|
| HH:mm | [skill] | [[filename]] | Activity description |

---
```

### When to Log Here

Log to folder-level log when skill operates on files that:
1. Are NOT in a project folder
2. Belong to INCUBATING, LITERATURE, CREATIONS, or KNOWLEDGE

---

## Skill Logging Requirements

### MANDATORY: All Skills Must

1. **Check for logs/ folder** before logging
2. **Create logs/activity.md** if not exists (for projects)
3. **Create data log** if not exists (in `90. SYSTEM/96. Data/`)
4. **Include skill tag** in every entry: `[skill-name]`
5. **Use table format** for consistency
6. **Get actual timestamp** via `date` command (never estimate)

### Auto-Create Logic

```
Is target in 50. PROJECTS/?
  ├── YES → Check {project}/logs/activity.md exists
  │         └── NO → Create with template
  └── NO → Check 90. SYSTEM/96. Data/Log-{XX}-{Name}.md exists
            └── NO → Create with template
```

---

## Central Logs (Special Format)

### Indagatio-Log.md

Table format for URL tracking:

```markdown
| Date | Time | Source URL | Type | Scale | Quick Note |
|------|------|------------|------|-------|------------|
```

### Log-00-Processing.md

Daily sections with detailed processing info (see purgatio/references/log-format.md).

### Log-30-Knowledge.md

Dataview-based (no manual entries needed).

---

## Timestamp Acquisition

**CRITICAL**: Never estimate time. Always get actual system time:

```bash
date "+%H:%M"  # For Time column
date "+%Y-%m-%d"  # For date header
```

---

## Cross-Reference

When skill activity affects multiple locations:

1. Log to PRIMARY location (where main work happened)
2. Add cross-reference link in Details column

Example:
```markdown
| 14:30 | [unio] | Merged 50-NNNNNN | See also [[50-NNNNNN. Target/logs/activity\|absorbed project log]] |
```

---

## Skill-Specific Requirements

### elevatio

- Create `logs/activity.md` during project creation
- Log: `Project created from: [[source note]]`

### purgatio

- Log to BOTH central (`Log-00-Processing.md`) AND project log
- If target is non-project folder → log to folder-level log

### commentatio

- Log to project's `logs/activity.md` if project-related
- Log to folder-level log if non-project

### unio

- Log to BOTH main and absorbed project logs
- Main: `Merged {absorbed} in`
- Absorbed: `Merged into [[main project]]`

### suspensio

- Log to project log BEFORE moving to Someday
- Entry: `Project suspended | reason: {reason}`

### dilatio

- Log to project log BEFORE moving to Pending
- Entry: `[dilatio] moved to pending | reason: {reason}, trigger: {trigger}`

### memorio

- Log to project log BEFORE moving to archive
- Entry: `Project archived`

### vivificatio

- Log to project log AFTER restoration
- Entry: `Project restored from archive`

### retrospectio

- No project logging (creates files in JOURNAL)
- Consider: Central review log? (future enhancement)

---

## Migration Notes

### New Projects (No Existing Log)

For existing projects without `logs/activity.md`:
1. Create file with template
2. Add initial entry: `[system] Activity log created`

For folders without a data log:
1. Create file with template
2. Add initial entry: `[system] Folder log created`

### Legacy Migration (B+ Format) - v1.3.1

For existing activity.md files with non-standard format:

**Structure:**
```markdown
---
created: YYYY-MM-DD
type: activity-log
project: {Project-Name}
tags:
  - log/activity
---

# Activity Log

> Auto-updated by Via Latina skills.

---

## New Entries

| Time | Skill | Activity | Details |
|------|-------|----------|---------|

---

## 📜 Legacy Entries (Pre-v1.3.0)

{Original content preserved here}
```

**Migration Steps:**
1. Update frontmatter to standard (`type: activity-log`, `tags: log/activity`, add `project:`)
2. Add standard header with description
3. Add `## New Entries` section with empty table at top
4. Add `## 📜 Legacy Entries (Pre-v1.3.0)` heading
5. Move all existing content under Legacy section (preserve original format)

**Benefits:**
- Consistent frontmatter for Dataview queries
- New entries follow standard table format
- Legacy content preserved without data loss
- Clear visual separation between old and new
