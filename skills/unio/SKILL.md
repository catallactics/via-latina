---
name: unio
description: "Obsidian project merge skill (Unio = union). Merges two projects: content integrated into main, absorbed project archived with proper frontmatter updates. Triggers on 'merge projects', 'combine', '/unio', '프로젝트 병합', '프로젝트 합치기', '이 프로젝트를 [X]에 합쳐줘'."
argument-hint: "[[project1]] [[project2]]"
disable-model-invocation: true
---

# Unio

> *Unio* (Latin): union, unity, becoming one

Skill for merging projects.

## Trigger Patterns

**Slash commands:**
- `/unio`
- `/unio [[50g]] [[50q]]`

**Natural language (English):**
- "Merge projects"
- "Combine these"
- "Merge them"

**Natural language (Korean):**
- "프로젝트 병합"
- "프로젝트 합치기"
- "이 프로젝트를 [X]에 합쳐줘"

## Core Principles

See `../_common/references/frontmatter-standard.md` for status taxonomy.

1. **User decides main**: User selects which project becomes main (suggest lower number as default)
2. **Frontmatter synced**: Absorbed project child files updated via child-frontmatter-update procedure before archive
3. **Quality over quantity**: Goal is quality information, not preserving everything
4. **Number permanent**: Absorbed project number never reused
5. **Not a Graveyard case**: Merge is intentional consolidation, NOT failure — no Graveyard entry needed (see memorio for Graveyard)

## Core Workflow

### Step 1: Target Confirmation

Present both projects and let user decide:

```
Merge targets:
- 50-000004. Platform-Course-Notes (2025-09-10)
- 50-000012. Learning-Roadmap (2025-10-15)

Which project should be the main?
- 50-000004: Created first
- 50-000012: Broader scope (entire AI learning roadmap)
```

**Default suggestion**: Lower number, but user can override based on:
- Scope (broader category should be main)
- Content volume
- Future direction

### Step 2: Collision Detection

Before moving any files, scan for filename collisions between the two projects:

```
Glob: {absorbed_folder}/**/*.md → list absorbed files
Glob: {main_folder}/**/*.md → list main files
```

**Compare relative paths** (e.g., `notes/api-guide.md` in both).

**If collisions found**, present to user BEFORE proceeding:

```
⚠️ Filename collisions detected:

| File | Main Project | Absorbed Project | Suggested Action |
|------|-------------|-----------------|------------------|
| notes/api-guide.md | Google API guide | OpenAI API guide | Keep both (different topics) |
| refined/architecture.md | System design v2 | System design v1 | Merge (overlapping) |
```

**Resolution strategy per collision:**

| Condition | Action |
|-----------|--------|
| Same topic, content overlaps | AI merges into single file (WebSearch to verify quality) |
| Same topic, one extends other | Keep the more complete version |
| Same filename, different topics | Keep both (add suffix `-from-{absorbed_number}`) |
| Merging would violate atomicity | Keep both (one note = one idea) |

**User confirms** each collision resolution before proceeding.

### Step 3: README Integration

Merge absorbed README content into main README:

1. Read both READMEs
2. Integrate relevant sections:
   - Progress items → Main's Progress
   - Resources → Main's Resources
   - Keywords → Main's Keywords
3. Add merge note to main README:

```markdown
## Merge History

| Date | Absorbed | Reason |
|------|----------|--------|
| 2025-11-02 | 50-000004. Platform-Course-Notes | Consolidated into broader AI learning roadmap |
```

**Do NOT create redirect document.** Absorbed README stays original in Archive.

### Step 4: File Move

Move non-colliding files from absorbed to main:

```
absorbed/notes/*.md    → main/notes/
absorbed/refined/*.md  → main/refined/
absorbed/scripts/*.py  → main/scripts/
```

Apply collision resolutions decided in Step 2 for colliding files.

### Step 5: Log to Both Projects

Log to both projects' `logs/activity.md` per `../_common/references/logging-standard.md`:

**Get actual timestamp first:**
```bash
date "+%Y-%m-%d"  # Date header
date "+%H:%M"     # Time column
```

**Main project log:**

```markdown
| HH:mm | [unio] | Project absorbed | Merged 50-000004 in, N files moved. See also [[50-000004. Platform-Course-Notes/logs/activity|absorbed project log]] |
```

**Absorbed project log (before archive):**

```markdown
| HH:mm | [unio] | Project merged into | Merged into [[50-000012. Learning-Roadmap]], N files moved to main |
```

### Step 6: Batch-Update Child Frontmatter (Absorbed Project)

Update `status:` in all child files of the **absorbed project** before moving to Archive.

See `../_common/references/child-frontmatter-update.md` for procedure.

**Unio-specific:**
- Target status: `archived`
- Preserve `status: refined` (consistent with memorio — valid in Archive per tentatio)
- README.md meta fields: `archived_date: YYYY-MM-DD`, `merged_into: "[[Main Project Name]]"`

**Preview table shown to user before applying:**

```
| File | Before | After |
|------|--------|-------|
| README.md | active | archived + archived_date + merged_into |
| notes/n01.md | active | archived |
| refined/r01.md | refined | (preserved) |
```

Proceed only after user confirmation.

### Step 7: Archive Absorbed Project

Move absorbed project folder to Archive:

```
Before: 50. PROJECTS/51. Active/50-000004. Platform-Course-Notes/
After:  50. PROJECTS/54. Archive/50-000004. Platform-Course-Notes/
```

**Important:**
- Folder name (including number) stays exactly the same
- Child frontmatter already updated in Step 6

### Step 8: Registry Update

**Update `90. SYSTEM/93. Registries/Project-Number-Registry.md`:**

See `../_common/references/registry-format.md` for detailed format.

1. Remove absorbed project from "Active Projects" table
2. Add to "Archived Projects" table with reason `merged`:

```markdown
| Number | Name | Archived | Reason | Merged Into |
|--------|------|----------|--------|-------------|
| 50-000004 | Platform-Course-Notes | 2025-11-02 | merged | 50-000012 |
```

**No Graveyard needed** — merge is not failure.

### Step 9: Report to Daily Note

Call purgatio in report-only mode to record merge in Daily Note.

**Report data:**
```yaml
skill: unio
action: projects_merged
summary:
  main_project: "50-000012"
  absorbed_project: "50-000004"
  files_moved: 5
  collisions_resolved: 1
  reason: "scope consolidation"
```

See `~/.claude/skills/purgatio/references/skill-report-format.md` for format.

---

## User Confirmation Points

1. **Main project selection** (required)
2. **Collision resolution** per file (if any collisions detected)
3. **Frontmatter update preview** (before applying child-frontmatter-update)

All confirmations happen BEFORE any irreversible operations (file moves, frontmatter edits).

## Reference Files

- `references/merge-examples.md` - Merge scenarios and examples
- `../_common/references/child-frontmatter-update.md` - Batch child frontmatter update procedure
- `../_common/references/registry-format.md` - Registry update format
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**
- `../_common/references/frontmatter-standard.md` - Status taxonomy (line 219: unio transition)

## Integration

### Inbound
- **user**: Direct call via `/unio` or "merge projects" / "프로젝트 병합"

### Outbound
- **purgatio**: Report merge completion (report-only mode) for Daily Note

### Related Skills
- **memorio**: Archive skill — unio's archive step follows the same child-frontmatter-update pattern as memorio. For archive-specific behaviors (Graveyard, research validation), see memorio.
- **vivificatio**: Inverse of archive (Archive → Active) — can restore a merged project if needed
- **tentatio**: Audits folder/status consistency — proper frontmatter updates prevent tentatio flags
