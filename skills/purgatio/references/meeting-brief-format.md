# Meeting Format Reference

> Guide for processing meeting-related quick notes.
> 4 document types: **Agd** (Agenda), **Prep**, **Brief**, **Post**

---

## Meeting Document Types

| Type | Abbr | Purpose | Audience | When |
|------|------|---------|----------|------|
| **Agenda** | Agd | Topics, time allocation, structure | All attendees (shared) | Before meeting |
| **Prep** | Prep | Research, strategy, materials | Internal only | Before meeting |
| **Brief** | Brief | Condensed key points | Self (quick reference) | During meeting |
| **Post** | Post | Discussion record, decisions | Internal | After meeting |

### File Naming Convention

**Pattern**: `{Brand} {Topic} {Type}.md`

| Type | Pattern | Example |
|------|---------|---------|
| Agenda | `{Brand} {Topic} Agd` | `Acme Q3 Launch Agd.md` |
| Prep | `{Brand} {Topic} Prep` | `Acme Q3 Launch Prep.md` |
| Brief | `{Brand} {Topic} Brief` | `Acme Q3 Launch Brief.md` |
| Post | `{Brand} {Topic}` | `Acme Q3 Launch.md` |

**Notes**:
- Post has NO suffix (most common document)
- NO date in filename (date in frontmatter)

### Recurring Meeting Naming

**Pattern**: `{Series} Y{YY}M{MM}W{N}`

| Component | Description |
|-----------|-------------|
| Series | Abbreviated series name (e.g., `Ops Team`) |
| Y{YY} | Year (e.g., `Y26` = 2026) |
| M{MM} | Month (e.g., `M01` = January) |
| W{N} | Week number in month (1-5) |

**Examples**:
```
Ops Team Y26M01W1     ← 2026년 1월 1주차
Ops Team Y26M01W2     ← 2026년 1월 2주차
Ops Team Y26M02W1     ← 2026년 2월 1주차
Ops Team Y27M01W1     ← 2027년 (자동 리셋)

All Hands Y26M01W3    ← 전체 미팅도 동일
All Hands Y26M02W1
```

**Multiple meetings per week** (if 2x/week):
- `Ops Team Y26M01W1a`, `Ops Team Y26M01W1b`

**Why this format**:
- Avoids infinite sequential numbering (m01 → m127...)
- Natural reset each year/month
- Self-documenting (year, month, week visible)
- Series name already abbreviated, so total length manageable

### Frontmatter Types

```yaml
type: meeting/agd    # Agenda
type: meeting/prep   # Preparation
type: meeting/brief  # Brief
type: meeting/post   # Post-meeting notes
```

---

## Meeting Folder Structure

```
60. JOURNAL/65. Meeting Records/
├── logs/        ← Indexing/navigation
│   ├── meetings-index.md      ← Master index
│   ├── {series-name}.md       ← Recurring meeting series index
│   └── ...
│
├── pre/         ← Pre-meeting materials (Agd, Prep, Brief)
│   └── ...
│
└── post/        ← Meeting content
    └── ...
```

**Principles:**
- Content folders (pre/, post/) = flat only (no subfolders)
- Complex organization = handled by logs/

---

## Meeting Workflow

```
[Pre-Meeting]
Quick note (agenda/prep/concepts)
    ↓
Choose document type(s):
├─ Agd   → 65. Meeting Records/pre/  (shared with attendees)
├─ Prep  → 65. Meeting Records/pre/  (internal research)
└─ Brief → 65. Meeting Records/pre/  (personal quick ref)

[Post-Meeting]
Quick note (meeting memos)
    ↓
Post → 65. Meeting Records/post/
    ↓
Update connections (Pre ↔ Post, Project README, Series index)
```

---

## Document Type Templates

### Common Frontmatter Fields

All types share: `created`, `meeting_date`, `attendees`, `company`, `tags: [meeting, {type}]`, `status: active`.

Type-specific: Agd adds `meeting_time`, `duration`, `location`. Post adds `{topic-tag}` to tags.

```yaml
type: meeting/agd    # or meeting/prep, meeting/brief, meeting/post
```

### Type 1: Agenda (Agd) — Shared with all attendees

Key sections: `[!info] Meeting Info` → `## Agenda` (시간/안건/담당 table) → `## Meeting Objectives` → `## Pre-read Materials` → `## 🔗 Vault Connections` → `## Source`

**Previous meeting items**: Add as separate `### 추가 확인 필요 (이전 미팅 기반)` table. Never merge into main agenda.

### Type 2: Prep — Internal research (NOT shared)

Key sections: `[!info] Meeting Info` → `## 미팅 목표` → `## 배경 / Context` → `## 핵심 자료 / Key Materials` (detailed subsections) → `## 논의 전략` → `## 질문 사항` (checklist) → `## 🔗 Vault Connections` → `## Source` → `## 🏠 Project Home`

### Type 3: Brief — Condensed Prep for quick reference

Key sections: `[!abstract] One-liner` → `## Big Picture` → Element sections → `## Key Points to Remember` → `## References` → `## 🔗 Vault Connections` → `## 🏠 Project Home`

**Brief writing**: Natural speakable sentences (not concept dictionaries). Keywords in **(parentheses)** at paragraph end.

### Type 4: Post — Meeting record (topic-based, NOT speaker-based)

Key sections: `[!abstract] Summary` → `## Meeting Notes` (topic subsections) → `## 결론` → `## Action Items` (checklist) → `## 🔗 Vault Connections` → `## Source`

### Vault Connections (All Types)

All 4 types link bidirectionally: `Agd ↔ Prep ↔ Brief ↔ Post`. Only Post recorded in project README `## Meetings`.

---

## Pre/Post Detection (Content-Based)

AI determines from content (no hardcoding): Agd (agenda/time allocation), Prep (research/strategy), Brief (condensed reference), Post (minutes/decisions). Default = Post.

---

## Indexing Structure

Three log files in `65. Meeting Records/logs/`:

1. **meetings-index.md** (Master): 정기 시리즈, 프로젝트별, 단발 미팅 links
2. **{series-name}.md** (Series Overview): 개요 (목적/주기/참석자), 연도별 기록 links
3. **{series-name} {YYYY}.md** (Yearly Log): 날짜/Post/Prep table

---

## Processing Logic

`Quick Note → Pre/Post detection → Pre: sub-type (Agd/Prep/Brief) → 65. Meeting Records/pre/ | Post → 65. Meeting Records/post/ → Update connections (pre↔post, README, series/master index)`

---

## Related References

- `destination-guide.md` - Refined note routing
- `daily-summary-format.md` - Daily note summary format
