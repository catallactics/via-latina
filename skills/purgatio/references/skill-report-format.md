# Skill Report Format Reference

Standard format for skills reporting to purgatio (report-only mode).

---

## Report Data Structure

All skills send this structure to purgatio:

```yaml
skill: string          # skill name (elevatio, unio, memorio, commentatio, retrospectio, vivificatio)
timestamp: datetime    # when action completed
action: string         # what was done
status: string         # success / warning / error
outputs: list          # created/modified files
summary: object        # skill-specific summary data
daily_entry: string    # pre-formatted table row for daily note
```

---

## Skill-Specific Summary Fields

| Skill | Action | Summary Keys | Daily Entry Pattern |
|-------|--------|-------------|---------------------|
| elevatio | project_created | project_number, project_name, source_note, project_type | `\| {num}. {name} \| [[source]] \|` |
| unio | projects_merged | main_project, absorbed_project, files_moved, reason | `\| {absorbed} \| {main} \| {reason} \|` |
| memorio | project_archived | project_number, project_name, reason, graveyard_entry | `\| {num}. {name} \| {reason} \|` |
| retrospectio | weekly_review_created | period, period_type, days_covered, task_completion_rate, projects_active | `\| [[period]] \| {dates} \| {completion} \| {projects} \|` |
| vivificatio | project_restored | project_number, project_name, from, to, reason | `\| {num}. {name} \| {direction} \| {reason} \|` |
| purgatio | daily_processing | quick_notes_processed, refined_notes_created, projects_updated, tasks_carried | `\| [[refined]] \| {source} \| {location}/ \|` |

### Commentatio Report (Full Example)

```yaml
skill: commentatio
timestamp: 2026-01-06 10:30
action: synthesis_created
status: success
outputs:
  - path: "50. PROJECTS/51. Active/50-NNNNNN. Project/refined/Analysis.md"
    type: synthesis
summary:
  source_note: "[[Skill Review]]"
  callout_counts:
    quaero: 3
    emendo: 2
    disco: 1
    facio: 1
    moneo: 0
    cogito: 0
  key_insights:
    - "Registry structure needs improvement"
    - "PWM concept requires further study"
daily_entry: |
  | [[Skill Review Analysis]] | [[Skill Review]] | quaero:3, emendo:2, disco:1, facio:1 |
```

---

## Daily Entry Table Headers

| Skill | Table Headers |
|-------|---------------|
| Purgatio | Refined Note \| Source \| Location |
| Commentatio | Synthesis \| Source \| Callouts |
| Elevatio | Project \| Created From |
| Unio | Absorbed \| Into \| Reason |
| Memorio | Project \| Reason |
| Retrospectio | Review \| Period \| Completion \| Projects |
| Vivificatio | Project \| Direction \| Reason |

**Note**: indagatio는 Quick Note만 생성하고 Daily Note에 직접 기록하지 않음. purgatio가 Quick Note 처리 시 Daily Note에 포함됨.

---

## Daily Note Append Logic

All skill reports go to `## 📊 Activity Log` in daily note (`60. JOURNAL/61. Daily Reviews/YYYY/MM/YYYY-MM-DD.md`).

### Activity Log Structure

```markdown
## 📊 Activity Log

### Purgatio (HH:mm)
| Refined Note | Source | Location |
|--------------|--------|----------|
| [[Note]] | quick note | project/refined/ |

### Commentatio (HH:mm)
| Synthesis | Source | Callouts |
|-----------|--------|----------|
| [[Analysis]] | [[Original]] | quaero:2, disco:1 |

### Elevatio (HH:mm)
| Project | Created From |
|---------|--------------|
| 50-NNNNNN. Project-Name | [[Quick Note]] |
```

### Append Steps

1. **Find daily note**: `60. JOURNAL/61. Daily Reviews/YYYY/MM/YYYY-MM-DD.md` (create from template if missing)
2. **Find `## 📊 Activity Log`** (create at end if missing)
3. **Find `### {Skill Name}`** subsection (create with table header if missing)
4. **Append** table row with timestamp

**Multiple runs**: Combine timestamps in heading `### Purgatio (10:30, 15:45)` and append rows to same table.

**Critical**: Never overwrite existing Activity Log content. Read → parse → append → write back.

### Error Handling

| Situation | Action |
|-----------|--------|
| Daily Note not found | Create from template |
| Activity Log not found | Create section |
| Skill subsection not found | Create subsection |
| Write permission error | Log warning, continue |

---

## Validation Rules

Before sending report:

1. **Required fields**: skill, timestamp, action, status
2. **Outputs**: At least one output path
3. **Daily entry**: Must be valid markdown table row
4. **Status values**: success, warning, error only

---

## Error Reports

When skill encounters error, set `status: error` with `error: { message, details }` and `daily_entry: | ⚠️ {skill} error | {message} |`.

---

## Related References

- `daily-summary-format.md` - Full daily note structure
