# Callout Types Reference

Detailed handling for each Latin callout type in commentatio.

Overview table: see SKILL.md `## Latin Callouts`.

---

## [!quaero] - Questions

**Processing**: Parse question type (factual/conceptual/comparative) → Search vault → WebSearch → Synthesize.

```markdown
### Question: {user's question}

> [!quote] Context
> {surrounding text that prompted question}

**Answer**: {comprehensive answer}

**Sources**:
- [[Vault Note]] - {relevance}
- [External Link](url) - {relevance}

**Related Keywords**: #keyword1 #keyword2
```

---

## [!emendo] - Improvements

**Processing**: Understand current state → Analyze scope → Propose specific steps with before/after.

```markdown
### Improvement: {user's request}

> [!quote] Current State
> {what exists now}

**Analysis**: {why change is needed}

**Proposed Changes**:
1. {step 1}
2. {step 2}

**Before → After**:
- Before: {current}
- After: {improved}
```

---

## [!disco] - Study Items

**Processing**: Identify core concept + prerequisites → Create learning roadmap → Extract keywords.

```markdown
### Study: {topic}

**Prerequisites**: {prereq list}

**Learning Roadmap**:
1. **Basics** (~Xh) - {topic} - [[Resource]]
2. **Intermediate** (~Xh) - {topic}
3. **Advanced** - {topic}

**Keywords**: #keyword1 #keyword2
**Vault Connections**: [[Related Note 1]], [[Related Note 2]]
```

---

## [!facio] - Actions

**Processing**: Extract task + deadline → Format as action item → Connect to project.

```markdown
### Action: {task description}

- [ ] {formatted task} #task/type

**Project**: [[Project README]] (if applicable)
**Priority**: {high/medium/low}
```

---

## [!moneo] - Important

**Processing**: Identify importance → Highlight → Find vault connections.

```markdown
### Important: {what to remember}

> [!important] Key Insight
> {the important thing, highlighted}

**Why It Matters**: {context}
**Connections**: [[Note 1]] - {how}, [[Note 2]] - {how}
```

---

## [!cogito] - Musings

**Processing**: Acknowledge thought → Expand freely → Note creative associations.

```markdown
### Musing: {user's thought}

**Expansion**: {free-form exploration}
**Possible Threads**: {where this might lead}
**Sparks**: {creative connections}
```

---

## Multiple Callouts

Process each independently, group by type in output, synthesize overall insights. For pattern detection (3+ disco → elevatio, quaero+disco → comprehensive learning, multiple facio → grouped action list), see `elevatio-trigger.md`.

---

## Edge Cases

- **Empty callout**: Ask user to clarify
- **Very long callout**: Summarize first, then respond
- **Nested context** (`> quote > > [!quaero]`): Include both levels in context
