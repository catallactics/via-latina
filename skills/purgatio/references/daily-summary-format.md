# Daily Summary Format Reference

> Format for purgatio processing summary added to daily notes.
> All information extracted from **refined notes**, not raw quick notes.

## Section Locations

Add to daily note (`60. JOURNAL/61. Daily Reviews/YYYY/MM/YYYY-MM-DD.md`):
1. `## 🧠 Thoughts & Ideas` - Key insights from all refined notes
2. `## 📊 Daily Processing Summary` - Processing statistics and details

---

## 🧠 Thoughts & Ideas Section

**Purpose**: Capture key insights from ALL refined notes (not just essays).

### Template

```markdown
## 🧠 Thoughts & Ideas

### Key Insights

#### [[{Refined Note Title 1}]]
- {Core insight or takeaway}
- {Important realization}

#### [[{Refined Note Title 2}]]
- {Core insight or takeaway}

### 🔮 AI Suggested Keywords
<!-- New concepts/keywords worth exploring based on today's notes -->

| Keyword | Related To | Why Explore |
|---------|------------|-------------|
| {keyword 1} | {refined note} | {brief reason} |
| {keyword 2} | {refined note} | {brief reason} |
| {keyword 3} | {refined note} | {brief reason} |

> [!tip] Exploration Suggestions
> Based on today's notes, you might want to explore:
> - {Suggestion 1 with context}
> - {Suggestion 2 with context}
```

### Extraction Rules

1. **Read each refined note** created today
2. **Extract 1-3 key insights** per note
3. **Identify new keywords** mentioned but not fully explored
4. **Suggest connections** to existing vault content

### AI Keyword Criteria

3 types: **Unmentioned Connections** (adjacent concepts user didn't mention), **Question Answers** (keywords to help answer user's questions), **Depth Extensions** (briefly mentioned concepts deserving exploration). Avoid: restating user's own words, generic buzzwords.

---

## 📊 Daily Processing Summary Section

**Purpose**: Statistics and details about today's purgatio processing.

### Template

```markdown
## 📊 Daily Processing Summary

> [!success] Purgatio completed (YYYY-MM-DD HH:mm)

### Refined Notes Created

| Refined Note | Source Quick Note | Location |
|--------------|-------------------|----------|
| [[{Refined Title 1}]] | {original filename} | {project}/refined/ |
| [[{Refined Title 2}]] | {original filename} | 72. Essays/ |

### Project Updates

| Project | Update Type | Details |
|---------|-------------|---------|
| [[{Project README}]] | Progress | {what changed} |
| [[{Project README}]] | New refined note | [[{note title}]] |

### Statistics

Same counters as `log-format.md` Daily Stats section. Key fields: Quick notes processed, Refined notes created, Projects updated, Essays created, New projects (via elevatio), Tasks carried over.
```

### Grouping Rules

- Group by project when multiple notes relate to same project
- Chronological order within groups
- Essays and reflections listed separately

---

**Source**: All summary info from refined notes (AI-enhanced, fact-checked), NOT raw quick notes.

---

## Related References

- `task-carry-over.md` - Task carry-over logic
- `destination-guide.md` - Where refined notes go
- `destination-guide.md` - Essay format (Personal/Reflective section)
