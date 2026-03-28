# Registry Update Format

> Update format for `90. SYSTEM/93. Registries/Project-Number-Registry.md`
> Used by: elevatio (new), memorio (archive), unio (merge), suspensio (someday), dilatio (pending)
> Note: vivificatio uses its own references/registry-restore-format.md (more detailed restore logic)

---

## Registry Structure

```markdown
## Active Projects
| Number | Name | Status | Created |
|--------|------|--------|---------|

## Someday Projects
| Number | Name | Suspended | Reason |
|--------|------|-----------|--------|

## Pending Projects
| Number | Name | Deferred | Trigger |
|--------|------|----------|---------|

## Archived Projects
| Number | Name | Archived | Reason | Merged Into |
|--------|------|----------|--------|-------------|

## Graveyard
| Number | Name | Archived | Reason | Failure Cause | Lessons |
|--------|------|----------|--------|---------------|---------|

## Next Available Number
50-NNNNNN
```

---

## Operation: New Project (elevatio)

### Add to Active Projects

```markdown
| 50-000033 | New-Project-Name | active | 2026-01-06 |  ← NEW
```

### Update Next Number

```markdown
~~50-000033~~ → 50-000034
```

### Number Sequence Rules

1. Check current highest in Active + Archived
2. **Never reuse** - Even if 50-000005 archived, skip to next
3. Sequence: 50-000001, 50-000002, ... (6-digit zero-padded)

---

## Operation: Archive (memorio)

### Move to Archived

```markdown
| 50-000003 | Project-Name | 2025-11-02 | completed | - |
```

### Reason Values

| Reason | Graveyard? | Description |
|--------|------------|-------------|
| `completed` | No | Goal achieved |
| `discontinued` | Yes | Stopped (ask lessons) |
| `on hold` | Yes | Paused (ask reason) |
| `merged` | No | Use unio operation |

### Graveyard Entry (discontinued/on hold only)

```markdown
| 50-000024 | Failed-Project | 2025-11-02 | discontinued | API ended | Have fallback plan |
```

**Required questions:**
1. "Why are you discontinuing?"
2. "What did you learn?"

---

## Operation: Merge (unio)

### Archive Absorbed Project

```markdown
| 50-000004 | Absorbed-Project | 2025-11-02 | merged | 50-000012 |
```

**No Graveyard needed** - merge is not failure.

---

## Operation: Restore (vivificatio)

### Move back to Active

```markdown
## Active Projects
| 50-000003 | Restored-Project | active | 2025-11-01 |  ← RESTORED
```

### Update frontmatter

```yaml
status: active
restored_from: archived
restored_date: 2026-01-14
```

---

## Operation: Suspend (suspensio)

### Move to Someday

```markdown
## Someday Projects
| 50-000003 | Project-Name | 2026-01-15 | Lost motivation |
```

### Remove from Active

Delete row from Active Projects table.

### Status Update

All child files + README.md:

```yaml
status: someday
```

### SUSPEND.md created in project folder

Contains suspension reason and optional resume conditions.

---

## Operation: Defer (dilatio)

### Move to Pending

```markdown
## Pending Projects
| 50-000003 | Project-Name | 2026-01-15 | Waiting for API release |
```

### Remove from Active

Delete row from Active Projects table.

### Status Update

All child files + README.md:

```yaml
status: waiting
```

### PENDING.md created in project folder

Contains trigger conditions for resumption.

---

## Number Reuse Prohibition

Project numbers are **permanent** - never reuse even after archive/merge.

```
Active: 50-000001, 50-000003, 50-000012
Archived: 50-000002, 50-000004

Next new = 50-000013 (NOT 50-000002 or 50-000004)
```
