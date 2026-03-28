# Child Frontmatter Batch-Update Procedure

> Shared procedure used by memorio, suspensio, dilatio, vivificatio, and unio when transitioning projects between lifecycle folders.

---

## Purpose

When a project moves between `50. PROJECTS/` subfolders, all child `.md` files with a `status:` field must be updated to match the destination folder. This prevents the systemic folder/status mismatches that tentatio detects.

## Procedure

### 1. Glob Files

```
Glob: {project_folder}/**/*.md
```

Collect all `.md` files recursively in the project folder.

### 2. Filter

For each file:
1. Check if file has YAML frontmatter (starts with `---`)
2. Check if frontmatter contains `status:` field
3. **Skip** files without frontmatter or without `status:` field
4. **Skip** skill artifact files: `ARCHIVE.md`, `SUSPEND.md`, `PENDING.md`

### 3. Update Status

For each qualifying file, update `status:` to the target value.

**Exception (memorio and unio)**: Preserve `status: refined` — this value is valid in Archive per tentatio/frontmatter-standard.

### 4. README.md Extra Fields

README.md receives additional meta fields alongside the status update:

| Skill | Meta Field | Value |
|-------|-----------|-------|
| memorio | `archived_date` | `YYYY-MM-DD` |
| suspensio | `suspended_date` | `YYYY-MM-DD` |
| dilatio | `pending_date` | `YYYY-MM-DD` |
| vivificatio | `restored_from` | Previous folder name (e.g., `archived`) |
| vivificatio | `restored_date` | `YYYY-MM-DD` |
| unio | `archived_date` | `YYYY-MM-DD` |
| unio | `merged_into` | `"[[Main Project Name]]"` |

### 5. Preview Changes

Before applying, display a summary table to the user:

```
| 파일 | 변경 전 | 변경 후 |
|------|---------|---------|
| README.md | active | archived |
| notes/n01. Guide.md | active | archived |
| refined/r01. Design.md | refined | (보존) |
```

Proceed only after user confirmation.

## Target Status Mapping

| Skill | Target Status | Preserve `refined`? |
|-------|--------------|---------------------|
| memorio | `archived` | Yes |
| suspensio | `someday` | No |
| dilatio | `waiting` | No |
| vivificatio | `active` or `someday` | No |
| unio | `archived` | Yes |

## Implementation Notes

- Use `Edit` tool for frontmatter updates
- Process README.md first (canonical status indicator)
- Count and report: files updated, files skipped, files preserved
