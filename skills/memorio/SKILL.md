---
name: memorio
description: "Obsidian project archive skill (Memorio = memory). Moves completed projects from 51. Active/ to 54. Archive/. Triggers on 'archive project', 'project complete', '/memorio', '이 프로젝트 아카이브해줘', '프로젝트 완료'. Preserves project number and original state, creates ARCHIVE.md with validated reasoning. Includes research validation and Graveyard for discontinued projects."
argument-hint: "[[project]]"
disable-model-invocation: true
---

# Memorio

> *Memorio* (Latin): memory, preservation, remembrance

Skill for archiving completed Obsidian projects.

## Trigger Patterns

- "Archive this project"
- "Project complete"
- "Move to archive"
- `/memorio`
- `/memorio [[50c]]`
- "이 프로젝트 아카이브해줘"
- "[project name] 아카이브"

## Core Principles

See `../_common/references/frontmatter-standard.md` for shared principles (number preserved, links preserved, frontmatter synced).

1. **ARCHIVE.md created**: Separate file with archive reason + validated research
2. **History in Registry**: Archive reason and date recorded in central registry

## Core Workflow

### Step 1: Project Verification

Verify user-specified project:
1. Identify by project name or number (e.g., "50c", "Sample-Campaign")
2. Find corresponding folder in `50. PROJECTS/51. Active/`
3. Verify README.md exists

### Step 2: Completion Status Check

Read README.md and verify:
1. Check completion rate in Progress section
2. Notify user if incomplete items exist
3. Confirm force archive (possible even when incomplete)

### Step 3: Collect Archive Reason

Ask user for archive reason:
- "Why are you archiving this project?"
- Capture user's exact words

**Reason categories:**
| Reason | Description |
|--------|-------------|
| Completed | Goal achieved |
| Discontinued | No longer proceeding |
| On Hold | May resume later |
| Merged | Merged into another project (via unio) |
| Superseded | Replaced by other tools/skills |

### Step 4: Research & Validation

**Validate user's reason with research:**

Use available tools based on topic:
- **Brave Search**: General trends, industry news, current state
- **paper-search MCP**: Academic validation for technical claims
- **WebFetch**: Official documentation, specific sources

**Research output:**
- ✅ Validated: Evidence supports user's reasoning
- ⚠️ Partial: Some support, some uncertainty
- 🔄 Nuanced: User's view is valid but with caveats
- ❌ Refuted: Evidence contradicts (inform user, proceed if confirmed)

**If refuted**: Present findings to user, ask if they still want to archive.

### Step 5: Create ARCHIVE.md

Create `ARCHIVE.md` in project root (before moving). See `references/archive-md-template.md` for full template.

**Location**: Project root, alongside README.md

### Step 6: Log to Project Activity

**Before moving**, record in project's `logs/activity.md`:

```markdown
## YYYY-MM-DD

| Time | Activity | Source | Changes |
|------|----------|--------|---------|
| HH:mm | [memorio] archived | - | reason: [reason], ARCHIVE.md created |
```

### Step 7: Batch-Update Child Frontmatter

Update `status:` in all child files before moving.

See `../_common/references/child-frontmatter-update.md` for procedure.

**Memorio-specific:**
- Target status: `archived`
- Preserve `status: refined` (valid in Archive per tentatio)
- README.md meta field: `archived_date: YYYY-MM-DD`

### Step 8: Execute Archive

Move folder. Child file frontmatter already updated in previous step.

```
Before: 50. PROJECTS/51. Active/50c. Sample-Campaign/
After:  50. PROJECTS/54. Archive/50c. Sample-Campaign/
```

**Important:**
- Keep folder name (including number) exactly the same
- Only ARCHIVE.md is new file added

### Step 9: Registry Update

**Update `90. SYSTEM/93. Registries/Project-Number-Registry.md`:**

See `../_common/references/registry-format.md` for detailed format.

1. Change status to `archived` in "Active Projects" table
2. Add row to "Archived Projects" table:
   - Project number
   - Project name
   - Archive date
   - Reason (brief)

**For discontinued/on hold (Graveyard record):**

> [!warning] Those who forget history have no future

3. **Add to "Graveyard" table** if applicable:
   - Failure reason: Be specific
   - Lessons learned: What to do differently

### Step 10: Report to Daily Note

Call purgatio in report-only mode to record archive in Daily Note.

**Report data:**
```yaml
skill: memorio
action: project_archived
summary:
  project_number: "50c"
  project_name: "Sample-Campaign"
  reason: "completed"
  archive_md_created: true
  research_validation: "validated"
```

See `~/.claude/skills/purgatio/references/skill-report-format.md` for format.

---

## Archive Folder Structure

```
50. PROJECTS/
├── 51. Active/
│   └── ... (active projects)
├── 52. Someday/
│   └── ... (suspended projects via suspensio)
├── 53. Pending/
│   └── ... (pending projects via dilatio)
└── 54. Archive/
    └── 50c. Sample-Campaign/
        ├── ARCHIVE.md    ← NEW: Archive reason + validation
        ├── README.md     ← UPDATED: status synced to folder
        └── ...           ← UPDATED: child file status synced
```

## User Confirmation Points

1. Confirm project to archive
2. Force archive if incomplete items exist
3. Archive reason (user's words)
4. Confirm if research refutes user's reasoning

## Research Tools Priority

| Topic Type | Primary Tool | Fallback |
|------------|--------------|----------|
| Tech trends | Brave Search | WebFetch |
| Academic claims | paper-search | Brave Search |
| Tool/API status | WebFetch (official docs) | Brave Search |
| Industry news | Brave Search | - |

## Reference Files

- `../_common/references/child-frontmatter-update.md` - Batch child frontmatter update procedure
- `references/archive-md-template.md` - ARCHIVE.md template
- `../_common/references/registry-format.md` - Registry update format
- `references/archive-checklist.md` - Pre-archive verification checklist
- `references/graveyard-guide.md` - Failure/discontinuation recording guide
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

## Integration

### Inbound
- **user**: Direct call via `/memorio` or "archive project"
- **unio**: Archive absorbed project after merge

### Outbound
- **purgatio**: Report archive completion (report-only mode) for Daily Note

### Related Skills
- **suspensio**: Lighter alternative (Active → Someday) for internal decisions
- **dilatio**: External wait (Active → Pending) for external trigger wait
- **vivificatio**: Inverse operation (Archive → Active)

