# Learning Project Guide

## README Structure

```markdown
# {Project Name}

> [!abstract] Project Overview
> Systematic learning roadmap for mastering {topic}
> - **Goal**: {specific learning goal}
> - **Duration**: {estimated period}
> - **Level**: Beginner/Intermediate/Advanced

## Quick Navigation

### Learning Path
- [[notes/p10. Basics|Phase 1: Basics]]
- [[notes/p20. Intermediate|Phase 2: Intermediate]]
- [[notes/p30. Advanced|Phase 3: Advanced]]

### Resources
- [[notes/n01. Reference-Links|Reference Links]]
- [[notes/n02. CheatSheet|CheatSheet]]
- [[notes/n03. Shortcuts|Shortcuts]]

## Prerequisites

> [!warning] Check Before Starting
> {prerequisite knowledge/skills list}

- [ ] {prerequisite 1}
- [ ] {prerequisite 2}

## Learning Roadmap

### Phase 1: Basics (Week 1-2)
| Step | Topic | Time | Status |
|------|-------|------|--------|
| 1 | {topic} | ~Xh | 🔲 |

### Phase 2: Intermediate (Week 3-4)
...

### Phase 3: Advanced (Week 5-6)
...

## Progress Tracker

- [ ] Phase 1 complete
- [ ] Phase 2 complete
- [ ] Phase 3 complete

## Weekly Schedule

{time allocation example}

## Related Resources

### In Vault
- [[Related Guide]]
- [[Related Project]]

### External
- [Official Docs](url)
- [Recommended Course](url)

## Activity Log
[[logs/activity|Full History]]
```

---

## Phase Design Principles

### Progressive Complexity
- Each phase should only require knowledge from previous phases
- First phase always establishes foundation: "what is this?" → install → first successful run
- Strategy/comparison content requires prerequisite hands-on experience — place in later phases
- Reference material (tool catalogs, spec tables) may work better as n-series reference notes than learning phases

### Anti-patterns (illustrative examples, not rigid rules)

**Strategy Before Basics**: Starting Phase 1 with tradeoff analysis before the learner has installed the tool.
- *Example*: "4 integration paths comparison" as p10 instead of "install and first run"

**Meta-phase as Learning**: Treating project QA work (verification against docs, error correction) as a learning phase.
- *Fix*: Distribute corrections as inline `[!info]` callouts in relevant phases

**Late Discovery Cramming**: Discovering a prerequisite mid-project and forcing it into a later phase with a warning callout.
- *Fix*: Restructure phases to place it in the correct position

**Reference Dump as Phase**: Placing a large reference catalog (e.g., 18-tool spec table) as a learning phase.
- *Fix*: Consider making it a reference note (n-series) instead

### Phase Validation Questions (soft checks)

Run these after drafting phase structure for any learning project:
1. "Can a learner with zero prior knowledge start at Phase 1 and succeed?"
2. "Does any phase reference concepts not yet introduced in a previous phase?"
3. "Is strategy/comparison content placed AFTER the learner has enough hands-on context to evaluate tradeoffs?"
4. "Could any phase note work better as a reference note (n-series) instead?"

## Required Notes

**Numbering**: See `notes-numbering.md` for p-series (phases) and n-series (refs).

### 1. Phase Notes (p10., p20., p30.)

```markdown
# Phase X: {Phase Name}

## Learning Goals
- {goal 1}
- {goal 2}

## Content

### Topic 1: {topic}
{content}

### Topic 2: {topic}
{content}

## Practice Tasks
- [ ] {task 1}
- [ ] {task 2}

## Key Takeaways
- {point 1}
- {point 2}
```

### 2. Reference Notes (n01., n02., n03.)

```markdown
# {Reference Topic}

## Overview
{brief description}

## Quick Reference
{key information, tables, code snippets}

## Related
- [[notes/p10. Topic|Phase 1]]
- [[README]]
```

**Common reference files:**
- `n01. Reference-Links.md` - External resources
- `n02. CheatSheet.md` - Quick reference / syntax
- `n03. Shortcuts.md` - Keyboard shortcuts / tips

