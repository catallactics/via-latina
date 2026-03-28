# Vault Setup Guide

Via Latina works best with a structured Obsidian vault. This guide explains the recommended folder hierarchy and how to adapt it.

## Recommended Structure

```
Your Vault/
├── 00. INBOX/
│   ├── 01. Quick Notes/       ← indagatio creates notes here
│   │   └── notes/
│   └── 02. On-Hold/           ← locatio moves grouped notes here
│       └── [topic folders]/
│
├── 10. INCUBATING/            ← purgatio routes developing ideas here
├── 20. LITERATURE/            ← purgatio routes literature notes here
├── 30. KNOWLEDGE/             ← purgatio routes refined knowledge here
│
├── 50. PROJECTS/
│   ├── 51. Active/            ← elevatio creates projects here
│   ├── 52. Someday/           ← suspensio moves paused projects here
│   ├── 53. Pending/           ← dilatio moves waiting projects here
│   └── 54. Archive/           ← memorio moves completed projects here
│
└── 90. SYSTEM/
    ├── 91. Templates/
    ├── 93. Registries/        ← Project number registry
    └── 94. System Guides/
```

## Customizing

You can adapt folder names and numbers to your preference. The key requirement is that `_common/references/vault-structure.md` matches your actual vault layout. Update that file to reflect your structure, and all 12 skills will follow.

## Project Numbering

Projects use a 6-digit zero-padded numbering system: `50-000001`, `50-000002`, etc. The registry at `90. SYSTEM/93. Registries/Project-Number-Registry.md` tracks assignments.

## Templates

Via Latina skills expect certain frontmatter fields and note structures. Create these templates in your `91. Templates/` folder to ensure consistency.

### Quick Note Template

Used by `indagatio` when creating new notes in `00. INBOX/01. Quick Notes/`.

```yaml
---
created: {{date:YYYY-MM-DD}} {{time:HH:mm}}
type: quick-note
tags: []
status: inbox
difficulty:
source:
---

# {{title}}

## Content


## Source
-
```

### Project README Template

Used by `elevatio` when creating new projects.

```yaml
---
created: {{date:YYYY-MM-DD}}
type: project
tags: []
status: active
project_number: 50-NNNNNN
---

# Project Name

## Overview


## Progress
- [ ]

## Resources
-

## Related
-

## Meetings

| Date | Type | Link |
|------|------|------|
```

### Refined Note Template

Used by `purgatio` when refining quick notes into permanent notes.

```yaml
---
created: {{date:YYYY-MM-DD}} {{time:HH:mm}}
type:
tags: []
status: active
difficulty:
source: "[[original quick note]]"
---

# {{title}}

## Content


## Source
- **Original**: [[original quick note]]
```

### Daily Note Template

Used by `purgatio` and `retrospectio` for daily processing summaries.

```yaml
---
created: {{date:YYYY-MM-DD}}
type: daily-note
tags: [daily]
---

# {{date:YYYY-MM-DD}}

## Tasks
- [ ] #task Purgatio 처리

## Activity Log

| Time | Skill | Action | Detail |
|------|-------|--------|--------|

## Thoughts & Ideas

## Daily Processing Summary
```

### Customizing Templates

These templates use Obsidian's [Templater](https://github.com/SilentVoid13/Templater) syntax. Adapt the frontmatter fields to match your workflow — the key fields that Via Latina skills depend on are:

- `type` — must match values in `_common/references/type-standard.md`
- `status` — must match values in `_common/references/frontmatter-standard.md`
- `created` — ISO date format (YYYY-MM-DD)

## Daily Notes

`purgatio` and `retrospectio` integrate with Obsidian's daily notes. Place them wherever your vault stores daily notes — the skills will find them by date pattern.
