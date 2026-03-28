# Output Format Reference

Template for commentatio synthesis documents.

---

## File Naming

`{Topic} Synthesis.md` — e.g., `Skill Review Synthesis.md`, `PWM Concept Synthesis.md`

---

## Frontmatter

```yaml
---
created: YYYY-MM-DD
type: commentatio-synthesis
source: "[[Original File]]"
callouts:
  quaero: N
  emendo: N
  disco: N
  facio: N
  moneo: N
  cogito: N
tags:
  - commentatio
  - synthesis
  - {topic-tags}
status: active
---
```

---

## Document Structure

```markdown
# {Topic} Synthesis

## 📋 Source Summary
{2-3 sentence summary}
**Source**: [[Original File]] | **Processed**: YYYY-MM-DD HH:mm

---

## 💬 Callout Responses

### Questions (quaero)
#### Q: {question}
> [!quote] Context
> {context}
**Answer**: {answer}
**Sources**: [[Link1]], [External](url)

### Improvements (emendo)
#### Improve: {request}
**Current**: {state} | **Proposed**: {change}

### Study Items (disco)
#### Study: {topic}
**Learning Path**: 1. {step} - [[Resource]]
**Keywords**: #keyword1 #keyword2

### Important (moneo)
> [!important] {insight}
Connected to: [[Note1]], [[Note2]]

### Musings (cogito)
**Expansion**: {exploration}

---

## ✅ Action Items
- [ ] {task} #task/type

## 🔑 Key Insights
1. **{Insight}**: {explanation}

## 🔗 Connected Knowledge
- [[Note]] - {relevance}

## 📊 Processing Stats
| Callout | Count |
|---------|-------|
| quaero/emendo/disco/facio/moneo/cogito | N each |

## Source
**Original**: [[Original File]] | **Processed by**: commentatio | **Date**: YYYY-MM-DD
```

---

## Section Rules

- **Order**: Summary → quaero → disco → emendo → moneo → cogito → Actions → Insights → Connected → Stats → Source
- **Skip empty sections**
- **Minimal version** (few callouts): `# Synthesis` → `## Summary` → `## Responses` → `## Actions` → `## Source`
