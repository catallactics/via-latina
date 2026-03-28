# Vault Structure Reference

> Full vault structure referenced by purgatio skill

```
00. INBOX/                          ← Collection
├── 01. Quick Notes/                ← Quick note container
│   ├── notes/                      ← Quick note originals
│   ├── archive/                    ← Processed originals (no delete, preserve emotions)
│   └── Indagatio-Log.md            ← External source URL log
├── 02. On-Hold/                    ← Deferred notes (locatio managed, topic folders + HOLD.md)
├── 03. Reports/                    ← Indagatio output reports
├── 04. Legacy/                     ← Pre-system legacy content
├── 05. People/                     ← Permanent reference — people notes
├── 06. AI Sessions/                ← AI session logs (YYYY/MM/ structure)
│   └── {YYYY}/{MM}/               ← Year/Month subfolders
└── 07. Clippings/                  ← Web clippings

10. INCUBATING/                     ← Ideas in development
├── 11. Ideas/                      ← Ideas, pre-project exploration
└── 12. Themes/                     ← Recurring themes

20. LITERATURE/                     ← Literature notes (source-dependent)
├── 21. Books/
├── 22. Courses/
├── 23. Media/                      ← Video, podcast, visual media notes
├── 24. Papers/                     ← Academic/research papers
└── 25. Reports/                    ← Analysis reports, indagatio results

30. KNOWLEDGE/                      ← Established knowledge (OECD 3-category)
├── 31. Humanities & Arts/          ← 인문학, 예술, 철학
├── 32. Social Sciences/            ← 사회과학, 심리학, 경제학
└── 33. Engineering & Tech/         ← 공학, 컴퓨터과학, 기술

40. MAPS/                           ← Navigation
├── 41. Dashboards/
└── 42. MOCs/

50. PROJECTS/                       ← Projects
├── 51. Active/                     ← AI refined versions (project-related)
├── 52. Someday/                    ← Someday/maybe (suspended projects)
├── 53. Pending/                    ← Pending projects (external trigger wait)
└── 54. Archive/                    ← Completed projects

60. JOURNAL/                        ← Journal
├── 61. Daily Reviews/              ← Summary reports
│   └── {YYYY}/{MM}/                ← Year/Month subfolders (e.g., 2026/02/)
└── 65. Meeting Records/            ← Meeting materials (not project dev docs)

70. CREATIONS/                      ← Creations
└── 72. Essays/                     ← Reflections/personal writings

90. SYSTEM/                         ← System
├── 91. Templates/
├── 92. Attachments/
├── 93. Registries/
├── 94. System Guides/
├── 95. Scripts/
├── 96. Data/                       ← System data files + logs
│   ├── Log-00-Processing.md        ← purgatio processing history
│   ├── Log-10-Incubating.md        ← Ideas/Themes activity
│   ├── Log-20-Literature.md        ← Literature notes activity
│   ├── Log-30-Knowledge.md         ← Knowledge notes (Dataview)
│   └── Log-70-Creations.md         ← Creative outputs activity
└── 97. Verification-Reports/       ← Vault diagnostics, audit reports
```

## Key Paths

| Purpose | Path |
|---------|------|
| Quick note originals | `00. INBOX/01. Quick Notes/notes/` |
| Quick note archive | `00. INBOX/01. Quick Notes/archive/` |
| Indagatio log | `00. INBOX/01. Quick Notes/Indagatio-Log.md` |
| Active projects | `50. PROJECTS/51. Active/` |
| Daily reviews | `60. JOURNAL/61. Daily Reviews/{YYYY}/{MM}/` |
| Essays/reflections | `70. CREATIONS/72. Essays/` |
| Meeting materials | `60. JOURNAL/65. Meeting Records/` |
| Processing log | `90. SYSTEM/96. Data/Log-00-Processing.md` |

---

## KNOWLEDGE Domain Classification (OECD 3-Category)

Content routing to KNOWLEDGE subfolders based on OECD classification:

| Domain | Subfolder | Examples |
|--------|-----------|----------|
| Humanities & Arts | `31. Humanities & Arts/` | 철학, 미학, 예술, 언어, 역사, Postmodernism, Vaporwave |
| Social Sciences | `32. Social Sciences/` | 심리학, 사회학, 경제학, 정치학, Nostalgia, Attention Economy |
| Engineering & Tech | `33. Engineering & Tech/` | 컴퓨터과학, 전자공학, 기계공학, OAuth, AI |

**Fine-grained classification**: Use frontmatter `subdomain` field
```yaml
---
domain: humanities-arts
subdomain: philosophy  # or arts, history, linguistics
---
```

**Domain values for frontmatter:**
- `humanities-arts`
- `social-sciences`
- `engineering-tech`

---

## Extensibility

Folders can be added as needed. Current expandable folders:

### 30. KNOWLEDGE/
```
31-33              ← existing (OECD 3-category active)
```

### 10. INCUBATING/
```
11. Ideas/         ← existing
12. Themes/        ← existing
13. Experiments/   ← available
14-19.             ← reserved
```

**Naming convention**: `{number}. {Name}/`

**After creating folder**:
1. Update this vault-structure.md
2. Update destination-guide.md
3. Consider creating template
