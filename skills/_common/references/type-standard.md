# Type Taxonomy Standard

> Canonical list of allowed `type` values for all vault files.
> Created 2026-03-04 as part of vault restructuring (#S963).

---

## Core Types (15)

| # | Type | Absorbed From | Description |
|---|------|---------------|-------------|
| 1 | `session-summary` | — | GenAI chat session summaries |
| 2 | `quick-note` | — | Raw inbox captures |
| 3 | `indagatio` | — | Skill-generated investigation notes |
| 4 | `daily` | `daily ` (trailing space) | Daily journal entries |
| 5 | `learning-note` | `learning-roadmap`, `learning-log` | Learning-related notes |
| 6 | `guide` | `system-guide`, `system-doc`, `system-note`, `master-guide`, `technical-guide`, `quickstart` | How-to and system documentation |
| 7 | `literature` | `literature-note` | Literature/source analysis notes |
| 8 | `meeting` | `meeting/post`, `meeting/prep`, `meeting/brief`, `meeting/agd`, `meeting-series` | Meeting notes (use `meeting_role` for subtype) |
| 9 | `project` | `project-note`, `project-report`, `project-update`, `project-document` | Project-related documents |
| 10 | `changelog` | `update-log`, `update`, `update-summary` | Change logs and update records |
| 11 | `reference` | `reference-note`, `quick-reference` | Lookup/reference material |
| 12 | `log` | `activity-log`, `folder-log`, `work-log`, `ai-log` | Activity and work logs |
| 13 | `plan` | `strategy`, `creative-brief` | Plans and strategies |
| 14 | `concept` | — | Concept definitions |
| 15 | `idea` | `brainstorm` | Ideas and brainstorms |

---

## Special Purpose Types (13)

| Type | Description |
|------|-------------|
| `essay` | Creative/reflective writing |
| `moc` | Map of Content navigation files |
| `dashboard` | Dashboard views |
| `people` | People profiles |
| `template` | Note templates |
| `refined-note` | Purgatio-processed refined outputs |
| `analysis` | Deep analysis documents |
| `checklist` | Checklists and action items |
| `weekly-review` | Weekly review documents |
| `knowledge` | Knowledge domain notes |
| `verification-report` | Tentatio audit reports |
| `hold-record` | Locatio on-hold placement records |
| `commentatio-synthesis` | Commentatio skill synthesis outputs |

---

## Meeting Subtype Field

For meeting notes, use `type: meeting` with optional `meeting_role`:

```yaml
type: meeting
meeting_role: post    # post|prep|brief|agd
```

| Role | Description |
|------|-------------|
| `post` | Post-meeting summary |
| `prep` | Pre-meeting preparation |
| `brief` | Meeting brief |
| `agd` | Agenda document |

---

## Absorption Rules

### Migration Mapping (Old → New)

| Old Type | → New Type |
|----------|------------|
| `daily ` (trailing space) | `daily` |
| `learning-roadmap` | `learning-note` |
| `learning-log` | `learning-note` |
| `system-guide` | `guide` |
| `system-doc` | `guide` |
| `system-note` | `guide` |
| `master-guide` | `guide` |
| `technical-guide` | `guide` |
| `quickstart` | `guide` |
| `literature-note` | `literature` |
| `meeting/post` | `meeting` + `meeting_role: post` |
| `meeting/prep` | `meeting` + `meeting_role: prep` |
| `meeting/brief` | `meeting` + `meeting_role: brief` |
| `meeting/agd` | `meeting` + `meeting_role: agd` |
| `meeting-series` | `meeting` |
| `project-note` | `project` |
| `project-report` | `project` |
| `project-update` | `project` |
| `project-document` | `project` |
| `update-log` | `changelog` |
| `update` | `changelog` |
| `update-summary` | `changelog` |
| `reference-note` | `reference` |
| `quick-reference` | `reference` |
| `activity-log` | `log` |
| `folder-log` | `log` |
| `work-log` | `log` |
| `ai-log` | `log` |
| `strategy` | `plan` |
| `creative-brief` | `plan` |
| `brainstorm` | `idea` |

### Empty Type Inference (by folder location)

| Folder Pattern | Inferred Type |
|----------------|---------------|
| `00. INBOX/06. GenAI Chats/` | `session-summary` |
| `00. INBOX/01. Quick Notes/` | `quick-note` |
| `60. JOURNAL/61. Daily Reviews/` | `daily` |
| `65. Meeting Records/` | `meeting` |
| `50. PROJECTS/` (README.md) | `project` |
| `20. LITERATURE/` | `literature` |
| `30. KNOWLEDGE/` | `knowledge` |
| `40. MAPS/` | `moc` |
| `90. SYSTEM/91. Templates/` | `template` |
| `90. SYSTEM/94. System Guides/` | `guide` |
| `10. INCUBATING/` | `idea` |
| `70. CREATIONS/` | `essay` |

---

## Validation

Full valid set (28 types):

```
session-summary|quick-note|indagatio|daily|learning-note|guide|literature|meeting|project|changelog|reference|log|plan|concept|idea|essay|moc|dashboard|people|template|refined-note|analysis|checklist|weekly-review|knowledge|verification-report|hold-record|commentatio-synthesis
```

Any `type` value not in this set is non-standard and should be migrated.
