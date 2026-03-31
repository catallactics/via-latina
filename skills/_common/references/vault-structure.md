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

30. KNOWLEDGE/                      ← Established knowledge (6-category)
├── 31. Philosophy & Culture/       ← 철학, 문화이론, 정치사상
├── 32. Art & Aesthetics/           ← 미학, 예술, 영화, 인터넷미학
├── 33. Psychology & Society/       ← 심리학, 사회학, 문화비평
├── 34. AI & LLM/                   ← AI 도구, 모델, 에이전트
│   └── Claude Code/                ← Claude Code 전용
├── 35. Software & Dev/             ← 웹개발, DevOps, 비-AI 기술
└── 36. Life & Practical/           ← 건강, 학습법, 수리, 생활

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

## KNOWLEDGE Domain Classification (6-Category)

Content routing to KNOWLEDGE subfolders. **When unsure, `ls "30. KNOWLEDGE/"` to check current categories.**

| Domain | Subfolder | Examples |
|--------|-----------|----------|
| Philosophy & Culture | `31. Philosophy & Culture/` | 철학, 문화이론, 포스트모더니즘, 가속주의 |
| Art & Aesthetics | `32. Art & Aesthetics/` | 미학, 예술운동, 베이퍼웨이브, 영화, 인터넷미학 |
| Psychology & Society | `33. Psychology & Society/` | 심리학, 사회학, 노스탤지어, 주의경제 |
| AI & LLM | `34. AI & LLM/` | AI 도구, 모델, 에이전트, 프롬프트 엔지니어링 |
| AI — Claude Code | `34. AI & LLM/Claude Code/` | Claude Code 팁, 플러그인, 워크플로우 |
| Software & Dev | `35. Software & Dev/` | 웹개발, DevOps, 지식관리도구, 비-AI 기술 |
| Life & Practical | `36. Life & Practical/` | 건강, 가구, 학습법, 수리, 금융 |

**Fine-grained classification**: Use frontmatter `subdomain` field
```yaml
---
domain: philosophy-culture
subdomain: cultural-theory  # or philosophy, political-thought
---
```

**Domain values for frontmatter:**
- `philosophy-culture`
- `art-aesthetics`
- `psychology-society`
- `ai-llm`
- `software-dev`
- `life-practical`

---

## Extensibility

Folders can be added as needed. Current expandable folders:

### 30. KNOWLEDGE/
```
31-36              ← existing (6-category active)
37-39              ← reserved for future expansion
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
