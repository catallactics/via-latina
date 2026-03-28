---
name: vivificatio
description: "Obsidian project revival skill (Vivificatio = bringing to life). Restores projects from 54. Archive/, 52. Someday/, or 53. Pending/ back to 51. Active/. Triggers on '/vivificatio', 'restore project', 'revive', '프로젝트 복원해줘', '다시 시작해줘'. Preserves project number, updates Registry. Inverse of memorio/suspensio/dilatio."
argument-hint: "[[project]]"
disable-model-invocation: true
---

# Vivificatio

> *Vivificatio* (Latin): restoration, revival, bringing back

Skill for restoring suspended/archived projects. Inverse operation of memorio and suspensio.

## Trigger Patterns

- `/vivificatio`, `/vivificatio [[50u]]`, `/vivificatio 50u`
- "Restore/Revive/Reactivate project", "Bring back from archive"
- "프로젝트 복원해줘", "아카이브에서 꺼내줘", "이 프로젝트 다시 시작해줘", "50u 복원"

## Core Principles

See `../_common/references/frontmatter-standard.md` for shared principles (number preserved, links preserved, frontmatter synced).

1. **Registry sync**: State changes recorded in central registry
2. **Flexible destination**: User chooses Active or Someday
3. **Memorio inverse**: Exact reverse of archive operation

---

## Core Workflow

### Step 1: Identify Target

1. Parse user input for project identifier
   - Project number: "50u"
   - Project name: "Indagatio"
   - Wikilink: `[[50u. Sample-Project]]`

2. Search in:
   - `50. PROJECTS/52. Someday/`
   - `50. PROJECTS/53. Pending/`
   - `50. PROJECTS/54. Archive/`

3. Verify project exists
   - If not found: Display error, list available archived projects

**Error message:**
```
해당 프로젝트를 54. Archive/에서 찾을 수 없습니다.

아카이브된 프로젝트 목록:
- 50u. Sample-Project (archived: 2025-11-08)
```

### Step 2: Destination Selection

See `references/destination-selection.md` for criteria.

Ask user:
```
복원 위치를 선택하세요:

1. 51. Active/ - 바로 작업 시작 (Recommended)
2. 52. Someday/ - 나중에 할 일

기본값: Active
```

### Step 3: Check Conflicts

See `references/conflict-resolution.md` for all conflict scenarios and resolution logic.

### Step 4: Batch-Update Child Frontmatter

Update `status:` in all child files before moving.

See `../_common/references/child-frontmatter-update.md` for procedure.

**Vivificatio-specific:**
- Target status: `active` (if restoring to Active) or `someday` (if restoring to Someday)
- Do not preserve `refined` (all statuses change to target)
- README.md meta fields: `restored_from: [archived|someday|waiting]`, `restored_date: YYYY-MM-DD`

### Step 5: Execute Restoration

Move folder. Child file frontmatter already updated in previous step.

```
Before: 50. PROJECTS/54. Archive/50u. Sample-Project/
After:  50. PROJECTS/51. Active/50u. Sample-Project/
        OR
        50. PROJECTS/52. Someday/50u. Sample-Project/
```

**Important:**
- Keep folder name (including number) exactly the same

### Step 6: Log to Project Activity

Log to project's `logs/activity.md`: Activity `[vivificatio] restored`, Changes `Archive → Active, reason: [reason]`. See `../_common/references/logging-standard.md`.

### Step 7: Registry Update

Update `90. SYSTEM/93. Registries/Project-Number-Registry.md`:

See `references/registry-restore-format.md` for detailed format.

### Step 8: Restoration Note (Optional)

Ask: `README.md에 복원 기록을 추가할까요? (Y/n)`

If yes, append `## Restoration History` table (columns: Date, From, To, Reason).

### Step 9: Report to Daily Note

Call purgatio in report-only mode. Key fields: `skill: vivificatio`, `action: project_restored`, project number/name, from/to, reason, readme_updated. See `~/.claude/skills/purgatio/references/skill-report-format.md`.

---

## User Confirmation Points

1. **Project selection**: Confirm which archived project to restore
2. **Destination choice**: Active or Someday
3. **Restoration reason**: Why restoring (for record)
4. **README update**: Whether to add restoration history

---

## Reference Files

- `../_common/references/child-frontmatter-update.md` - Batch child frontmatter update procedure
- `references/registry-restore-format.md` - Registry update format
- `references/destination-selection.md` - Active vs Someday criteria
- `references/conflict-resolution.md` - Conflict handling logic
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

---

## Integration

### Inbound
- **user**: Direct call via `/vivificatio` or natural language

### Outbound
- **purgatio**: Report restoration (report-only mode)

### Related Skills
- **memorio**: Inverse relationship (Active → Archive)
- **suspensio**: Inverse relationship (Active → Someday)
- **dilatio**: Inverse relationship (Active → Pending)
