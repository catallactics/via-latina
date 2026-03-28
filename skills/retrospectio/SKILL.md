---
name: retrospectio
description: "Obsidian periodic review skill (Retrospectio = looking back). Creates weekly/monthly review files and provides AI-generated summary suggestions. Triggers on '/retrospectio', '주간 리뷰 만들어줘', 'weekly review', 'create weekly review'. Use this skill whenever the user wants to reflect on the past week's work, create a periodic summary, or generate review documents from daily notes."
argument-hint: "[this week / last week / YYYY-Www]"
allowed-tools: Read, Glob, Grep, Write, Edit
---

# Retrospectio

> *Retrospectio* (Latin): looking back, review, reflection

Lightweight skill for creating periodic reviews with AI-assisted summaries.

## Trigger Patterns

- `/retrospectio` (default: this week)
- `/retrospectio last week` / `저번 주`
- `/retrospectio 2026-W02`
- "주간 리뷰 만들어줘"
- "이번 주 정리"

## Core Principles

1. **AI complements Dataview**: AI adds value where Dataview can't (summaries, comparisons, suggestions)
2. **Lightweight operation**: Minimal scanning, let templates do the heavy lifting
3. **User-editable output**: AI suggestions are proposals, not final content
4. **Template-first**: Template_62 + Dataview handles structure; skill handles intelligence

---

## Core Workflow

### Step 1: Period Determination

Parse user input: empty → this week (Mon-Sun), `last week`/`저번 주` → previous week, `YYYY-Www` → specific ISO week. See `references/period-calculation.md` for full parsing logic.

### Step 2: Create File

**Path**: `60. JOURNAL/62. Weekly Reviews/YYYY/YYYY-Www.md` *(on-demand, no fixed schedule)*

Apply Template_62 (already has Dataview queries for auto-aggregation).

If file exists: Ask overwrite/cancel.

### Step 3: Quick Scan Daily Reviews

Read Daily Reviews for the week (7 files max). Extract task completion counts (`## ✅ Tasks`), project mentions (`## 📊 Daily Processing Summary`), and skill activity counts (`## 📊 Activity Log`). No complex analysis—just counts.

### Step 4: AI Summary Suggestions

Generate 3 suggestions for user:

```markdown
> [!tip] AI 제안 (수정/삭제 가능)
>
> **이번 주 한 줄**: {프로젝트 X 집중, Y개 태스크 완료}
>
> **저번 주 대비**: {태스크 완료율 65%→77% (+12%)}
>
> **다음 주 포커스**: {Z 프로젝트 마무리 권장}
```

Insert into `## 📝 Week Summary` section.

User can edit/delete these suggestions.

### Step 5: Done

```
✅ 2026-W02.md 생성 완료
   위치: 60. JOURNAL/62. Weekly Reviews/2026/

   AI 제안이 추가되었습니다. 나머지는 직접 채워주세요:
   - What worked / didn't work
   - 다음 주 우선순위
```

---

## User Confirmation Points

1. **File overwrite**: If weekly review file already exists
2. **Period correction**: If detected period seems wrong ("Did you mean last week?")

---

> **Note**: Template_62 + Dataview handles daily note links, task display, and section structure—not this skill.

## Reference Files

- `references/period-calculation.md` - ISO week calculation

**Integration**: purgatio (optional review creation report)

Language policy: see global CLAUDE.md Language Policy table.
