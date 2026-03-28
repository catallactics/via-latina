# Destination Guide Reference

> Routing logic for processed quick notes. This file is extensible - add new destinations as needed.

## Decision Flow

`Quick Note → Existing project? → YES: project/refined/ | NO: Check Destination Map → Match: route | No match: idea? → YES: 10. INCUBATING/ | NO: Fallback (ask user)`

## Destination Map

### Project-Related Content

| Content Type | Destination | Notes |
|--------------|-------------|-------|
| Project progress/work | `{project}/refined/` | My processed outputs |
| Reference materials | `{project}/notes/` | External materials to read |

### Meeting Content

**Project meetings go to project folders. Non-project meetings go to `65. Meeting Records/`.**

| Meeting Type | Destination | Notes |
|-------------|-------------|-------|
| Project-specific post | `{project}/refined/rXX.` | Follows project rXX naming |
| Series/recurring post | `65. Meeting Records/post/` | Ops Team, All Hands, etc. |
| No-project post | `65. Meeting Records/post/` | New collab, strategy review |
| Pre-meeting prep | `65. Meeting Records/pre/` | Prep Doc + Brief |
| Meeting index | `65. Meeting Records/logs/` | Series index, master index |

**Project meeting detection:**
- Meeting content references a specific existing project → project `refined/`
- Meeting is about general operations, strategy, or no project exists → `65. Meeting Records/post/`

**Connection:**
- Project meetings: `## 🏠 Project Home` backlink + README 역링크
- Non-project meetings: `## 🔗 Vault Connections` with related links
- Pre ↔ Post: Bidirectional links
- All meetings: registered in `logs/meetings-index.md`

See `meeting-brief-format.md` for detailed format and processing logic.

### Personal/Reflective Content

| Content Type | Destination | Notes |
|--------------|-------------|-------|
| Personal reflection | `70. CREATIONS/72. Essays/` | Introspective writing |
| Life lessons learned | `70. CREATIONS/72. Essays/` | Personal growth |
| Opinion pieces | `70. CREATIONS/72. Essays/` | Perspective writing |

**Essay Format** (`YYYY-MM-DD {Title}.md`):
```yaml
type: essay
status: draft
tags: [essay/reflection]
```
Principles: Preserve core emotion/intent, structure scattered thoughts into logical flow, include notable original quotes as `>[!quote]`, bidirectional links to source required.

### Knowledge Content (OECD 6-Category)

Route to OECD-based subfolders (see `vault-structure.md` for domain classification):

| Content Type | Destination | Notes |
|--------------|-------------|-------|
| Humanities & Arts | `30. KNOWLEDGE/31. Humanities & Arts/` | 철학, 미학, 예술, 언어, 역사 |
| Social Sciences | `30. KNOWLEDGE/32. Social Sciences/` | 심리학, 사회학, 경제학, 정치학 |
| Engineering & Tech | `30. KNOWLEDGE/33. Engineering & Tech/` | 컴퓨터과학, 전자공학, 기술 |

### Incubating Content

| Content Type | Destination | Notes |
|--------------|-------------|-------|
| Idea needing development | `10. INCUBATING/11. Ideas/` | Ideas requiring further development |
| Recurring theme | `10. INCUBATING/12. Themes/` | Themes that appear repeatedly |
| Pre-project exploration | `10. INCUBATING/11. Ideas/` | Exploration before project creation |

**Incubating → Project promotion conditions**:
- Idea becomes concrete and actionable
- Multiple related notes accumulate
- User decides "let's make this a project"
- → Call **elevatio**

### Source-Based Content

| Content Type | Destination | Notes |
|--------------|-------------|-------|
| Book/article notes | `20. LITERATURE/21. Books/` | Literature notes |
| Image/artwork thoughts | `20. LITERATURE/23. Media/` | Visual source notes |
| Video/podcast notes | `20. LITERATURE/23. Media/` | Media source notes |
| Podcast notes | `20. LITERATURE/23. Media/` | Audio source notes |
| Report/analysis | `20. LITERATURE/25. Reports/` | 견적서, 분석 보고서, indagatio 결과 |
| Academic paper | `20. LITERATURE/24. Papers/` | Research papers |
| External tool analysis | `20. LITERATURE/25. Reports/` | GitHub repo, tool analysis |

### Creative Content

| Content Type | Destination | Notes |
|--------------|-------------|-------|
| Story/fiction ideas | `70. CREATIONS/72. Essays/` | Creative writing |
| Art/design concepts | `70. CREATIONS/72. Essays/` | Creative outputs |
| Content ideas | `70. CREATIONS/72. Essays/` | Content creation |

### New Project Trigger

Call **elevatio** when: New Project Criteria met (see SKILL.md), user requests, or incubating idea is ready.

---

## Meta-work Routing (3 Categories)

> Routes system/skill optimization notes to appropriate destinations instead of accumulating in Quick Notes.

**Detection Keywords**: "스킬", "토큰", "optimization", "diet", "vault", "진단", "Ralph Loop", "SKILL.md", "tentatio", "purgatio", "체크", "시스템 개선"

### Category 1: System

Vault structure, folder organization, configuration changes.

| Content | Destination |
|---------|-------------|
| Vault diagnostics/audits | `90. SYSTEM/97. Verification-Reports/` |
| System guides | `94. System Guides/{matching guide}/` |

### Category 2: Skill

Skill optimization, bug fixes, new skill design.

| Content | Destination |
|---------|-------------|
| Skill-related notes | `94. System Guides/{matching guide}/` |

### Category 3: Engineering Knowledge

Reusable technical knowledge discovered during system/skill work.

| Content | Destination |
|---------|-------------|
| All engineering insights | `30. KNOWLEDGE/33. Engineering & Tech/` |

**Rules**:
- Archive original to `Quick Notes/archive/` (same as standard lifecycle)
- Never place notes directly in top-level folders (only README/Log allowed at top level)
- Subdirectories must follow the numbering system

---

## Relocated File Detection

When a Quick Note has a `relocated-from` frontmatter tag, it was bulk-moved from another location and keyword-based routing may not match its original purpose. Route these notes based on their `relocated-from` path instead of content keywords.

```yaml
# Example frontmatter
---
relocated-from: 90. SYSTEM/94. System Guides/Claude-Code/
---
```

**Routing rule**: Parse the `relocated-from` path → route to the original parent folder if it still exists, or the closest matching destination.

---

## Pre-Refined Document Handling

When Quick Note already has `status: complete`, indagatio output structure, or no callouts needing processing:
1. Create Archive stub (summary + `purgatio: stub` frontmatter)
2. Move original to destination
3. Link stub → destination. Stub avoids duplicate content while maintaining archive trail.

---

## Fallback Logic

When no destination matches: Present options to user (add to existing project / elevatio / incubate / suggest folder / keep in INBOX). Learn from user decisions for future routing.

---

## Related References

- `vault-structure.md` - Full vault folder structure
- SKILL.md - New Project Criteria
- `archive-format.md` - Archive format after processing
