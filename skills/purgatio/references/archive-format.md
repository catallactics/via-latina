# Archive Format Reference

Format for preserving original quick notes.

## Core Principles

1. **100% Voice Preservation** - Original as-is, no spelling corrections
2. **No deletion, only move** - Move original file to Archive folder
3. **Bidirectional links** - Original ↔ Refined note must link each other
4. **Processing result notation** - Record where it was refined to

## Archive Location

```
00. INBOX/01. Quick Notes/archive/
```

**Move flow**:
```
00. INBOX/01. Quick Notes/notes/2026.01.04-14h30m Title.md
                              ↓ (move after processing)
00. INBOX/01. Quick Notes/archive/2026.01.04-14h30m Title.md
```

## Status Flow

Quick note status changes:

```
pending (before processing) → archived (moved to archive)
```

| Status | Description | Location |
|--------|-------------|----------|
| `pending` | Awaiting processing | `00. INBOX/01. Quick Notes/notes/` |
| `archived` | Archived | `00. INBOX/01. Quick Notes/archive/` |

## Frontmatter Changes

Modify frontmatter when archiving:

```yaml
---
created: 2026-01-04 14:30
type: quick-note
status: archived              # pending → archived
tags:
  - quick-note
---
```

## Archived File Format

```markdown
---
created: YYYY-MM-DD HH:mm
type: quick-note
status: archived
tags:
  - quick-note
---

(Original content 100% preserved)
(No spelling corrections)
(Voice/emotion as-is)

---
## Purgatio
- **Refined**: [[Refined file path]]
```

## Bidirectional Links (Required)

> [!important] Wikilink Format
> Use filename only without path: `[[Filename]]`

### Original → Refined

At bottom of archived original file:
```markdown
---
## Purgatio
- **Refined**: [[PWM and analogWrite Basics]]
```

### Refined → Original

At bottom of refined file:
```markdown
---
## Source
- **Original**: [[2026.01.04-14h30m Arduino Step 05]]
```

## Archive for Split Notes

When split due to atomicity violation, note in original:

```markdown
---
## Purgatio
- **Status**: Split (3 topics)
- **Refined**:
  - [[50-000009. Hardware-Beginner-Guide/notes/05. PWM Learning]]
  - [[50-000003. Sample-Campaign/ideas/Design Concept]]
  - [[60. JOURNAL/61. Daily Reviews/2026-01-04]] (reflection)
```
