# Registry Restore Format Reference

Format for updating Project-Number-Registry.md during restoration.

---

## Registry Location

`90. SYSTEM/93. Registries/Project-Number-Registry.md`

---

## Restoration Update Process

### 1. Update Archived Projects Table

Find the project row in `### Archived Projects (54. Archive/)`:

**Before:**
```markdown
| 51u | Sample-Project | archived | 2025-11-08 | Skill development complete |
```

**After:**
```markdown
| 51u | Sample-Project | restored | 2025-11-08 | Restored to Active 2025-11-15 |
```

### 2. Add to Destination Table

#### If Active (51. Active/)

Add row to `### Active Projects (51. Active/)`:

```markdown
| 51u | Sample-Project | active | 2025-10-20 | Restored 2025-11-15 |
```

#### If Someday (52. Someday/)

Add row to `### Someday Projects (52. Someday/)`:

```markdown
| 51u | Sample-Project | someday | 2025-10-20 | Restored 2025-11-15 |
```

---

## Someday Section Creation

If `### Someday Projects (52. Someday/)` doesn't exist:

Insert after Active Projects section:

```markdown
### Someday Projects (52. Someday/)

| Number | Name | Status | Created | Notes |
|--------|------|--------|---------|-------|
| 51u | Sample-Project | someday | 2025-10-20 | Restored 2025-11-15 |
```

---

## Status Values

| Status | Meaning | Used By |
|--------|---------|---------|
| `active` | In 51. Active/ | elevatio, vivificatio |
| `someday` | In 52. Someday/ | vivificatio |
| `archived` | In 54. Archive/ | memorio |
| `restored` | Was archived, now restored | vivificatio |
| `completed` | Finished, in Archive | memorio |
| `merged` | Absorbed by unio | unio, memorio |

---

## Changelog Entry

Add to `## Changelog` section:

```markdown
| 2025-11-15 | vivificatio | 51u restored from Archive to Active |
```

---

## Related References

- `destination-selection.md` - When to choose Active vs Someday
- `conflict-resolution.md` - Handling edge cases
