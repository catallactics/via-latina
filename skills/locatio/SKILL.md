---
name: locatio
description: "Obsidian On-Hold placement skill (Locatio = placing, setting aside). Moves MOC-grouped Quick Notes from 01. Quick Notes to 02. On-Hold for deferred processing. Triggers on '/locatio', 'hold these notes', 'on-hold로 이동', '이 노트들 보류', '아직 처리하기 이른 노트'. Creates topic folders with HOLD.md. Use this skill whenever notes aren't ready for full processing yet but shouldn't be lost — organizes them by topic with clear exit strategies for later retrieval."
argument-hint: "[[MOC - Topic]]"
disable-model-invocation: true
---

# Locatio

> *Locatio* (Latin): placing, setting aside, positioning

Skill for placing grouped Quick Notes on hold (moving to 02. On-Hold).

## Trigger Patterns

- `/locatio`
- `/locatio [[MOC - Topic]]`
- "Hold these notes"
- "Move to on-hold"
- "Put these notes on hold"
- "on-hold로 이동"
- "이 노트들 보류"
- "MOC 노트들 hold"

## Core Principles

1. **MOC preserved**: MOC stays in 40. MAPS, only wikilinks updated
2. **HOLD.md created**: Separate file with hold reason + exit strategy
3. **Topic folder**: Group name derived from MOC name (e.g., "Anthropic-Engineering")
4. **Frontmatter updated**: All notes get `status: on-hold` + hold fields
5. **Purgatio bypass**: On-hold notes are filtered out of purgatio processing

## Relationship with Other Skills

| Skill | Direction | Use Case | Target |
|-------|-----------|----------|--------|
| locatio | Quick Notes → On-Hold | Group deferral | MOC-linked notes |
| purgatio | Quick Notes → Various | Daily processing | Individual notes |
| elevatio | Notes → Active | Create project | Single note/group |

---

## Core Workflow

### Step 1: Identify Target Group

Identify notes to place on hold:

**Option A: By MOC**
1. User specifies MOC (e.g., `[[MOC - Anthropic Engineering]]`)
2. Parse MOC file for linked notes in `01. Quick Notes/notes/`
3. List all matching notes

**Option B: By Tag/Topic**
1. User specifies tag or topic
2. Search notes with matching tags in Quick Notes
3. List all matching notes

**Option C: Manual Selection**
1. User lists specific notes to hold

### Step 2: Collect Hold Reason

Ask user why these notes are being held:

```
AskUserQuestion:
- Header: "Hold Reason"
- Options:
  - purgatio_bypass: "Purgatio 처리 시 갈 곳 없음"
  - not_ready: "학습/정리 필요"
  - exploring: "아직 탐색 중"
  - overload: "바로 프로젝트화하면 과부하"
```

### Step 3: Collect Exit Strategy

Ask user about future plan:

```
AskUserQuestion:
- Header: "Exit Strategy"
- Options:
  - knowledge: "학습 후 30. KNOWLEDGE로 정리"
  - project: "프로젝트화 (elevatio)"
  - archive: "더 이상 필요 없으면 archive"
```

### Step 4: Create Folder Structure

Create topic folder in 02. On-Hold:

```
00. INBOX/02. On-Hold/{Topic-Name}/
├── HOLD.md
└── notes/
```

**Topic-Name**: Derive from MOC name
- `MOC - Anthropic Engineering` → `Anthropic-Engineering`
- `MOC - AI Tools` → `AI-Tools`

### Step 5: Create HOLD.md

Create `HOLD.md` in topic folder:

```markdown
---
created: YYYY-MM-DD HH:mm
type: hold-record
group: {Topic-Name}
note_count: {count}
moc: "[[MOC - {Topic}]]"
tags: [type/hold-record]
---

# ⏸️ On-Hold: {Topic Name}

## Hold 사유

> [User's reason category and explanation]

## 포함된 노트 ({count}개)

- [[note1]]
- [[note2]]
...

## 관련 MOC (40. MAPS에 유지)

- [[MOC - {Topic}]]

## 활용 계획

- [ ] {Exit strategy option 1}
- [ ] {Exit strategy option 2}
- [ ] {Exit strategy option 3}

## Exit Strategy

See `references/exit-strategies.md` for detailed exit paths.

---

*Hold Date: YYYY-MM-DD*
*Reason: {reason category}*
```

### Step 6: Move Notes

Move all identified notes to `02. On-Hold/{Topic}/notes/`:

```bash
# Move each note file
mv "01. Quick Notes/notes/{note}.md" "02. On-Hold/{Topic}/notes/"
```

### Step 7: Update Frontmatter

Update each note's frontmatter:

**Before:**
```yaml
status: inbox
```

**After:**
```yaml
status: on-hold
hold_date: YYYY-MM-DD
hold_group: {Topic-Name}
hold_moc: "[[MOC - {Topic}]]"
```

### Step 8: Update MOC Links

Update MOC file wikilinks to new location:

**Before:**
```markdown
[[00. INBOX/01. Quick Notes/notes/{note}|Alias]]
```

**After:**
```markdown
[[00. INBOX/02. On-Hold/{Topic}/notes/{note}|Alias]]
```

Also update any dataview queries in MOC if present.

### Step 9: Report Summary

Display: group name, note count, location, HOLD.md status, MOC link, exit options (elevatio/knowledge/memorio).

---

## Folder Structure

```
00. INBOX/02. On-Hold/{Topic-Name}/
├── HOLD.md
└── notes/          ← moved notes

40. MAPS/42. MOCs/MOC - {Topic}.md  ← links updated only
```

## Frontmatter Convention

### On-Hold Notes

```yaml
---
created: YYYY-MM-DD HH:mm
type: indagatio
status: on-hold           # Changed from inbox
hold_date: YYYY-MM-DD     # Added
hold_group: {Topic-Name}  # Added
hold_moc: "[[MOC - X]]"   # Added
tags: [...]
---
```

### HOLD.md

See Step 5 template above for HOLD.md frontmatter.

## User Confirmation Points

1. Confirm target notes (list all before proceeding)
2. Hold reason selection
3. Exit strategy selection

## Reference Files

- `references/hold-reasons.md` - Detailed reason descriptions
- `references/exit-strategies.md` - Exit path descriptions
- `../_common/references/frontmatter-standard.md` - Frontmatter fields

## Integration

### Inbound
- **user**: Direct call via `/locatio` or "hold these notes"
- **purgatio**: May suggest locatio for grouped expert notes

### Outbound
- **elevatio**: When user decides to projectize held notes
- **memorio**: When user decides to archive held notes

### Related Skills
- **purgatio**: Daily processing (filters out on-hold notes)
- **elevatio**: Project creation from notes
- **memorio**: Archive workflow

---

## Difference from Suspensio

| Aspect | Locatio | Suspensio |
|--------|---------|-----------|
| Target | Quick Note groups | Individual projects |
| Source | 01. Quick Notes | 51. Active |
| Destination | 02. On-Hold | 52. Someday |
| Registry | None | Project Registry |
| MOC | Preserved in 40. MAPS | N/A |
| File created | HOLD.md | SUSPEND.md |

