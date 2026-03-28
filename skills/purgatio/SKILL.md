---
name: purgatio
description: "Use this skill whenever the user wants to process, refine, or organize their quick notes. This includes: batch processing today's (or any date's) quick notes, refining rough notes into polished permanent notes, deciding where notes should be routed in the vault, splitting multi-topic notes for atomicity, archiving originals, generating daily processing summaries, or checking if any notes warrant new projects (calls elevatio). Activate for /purgatio. Also activate when users mention 'quick notes' (퀵 노트) needing processing, refinement, or organization — in any language including Korean (e.g., '퀵 노트 처리해줘', '오늘 노트 정리'). Key signals: 'process notes', 'refine notes', 'quick notes', 'daily summary', 'daily processing', or any request involving tidying up rough/raw notes captured throughout the day."
argument-hint: "[date or --select or \"keyword\"]"
---

# Purgatio

> *Purgatio* (Latin): purification, cleansing, refinement

Skill for processing daily quick notes into permanent assets.

## Quick Note Types

| Type | Characteristic | Processing | Latin Callouts |
|------|----------------|------------|----------------|
| User-written | No callouts | purgatio (context parsing) | Not needed |
| AI/External-based | Latin callouts | purgatio detects → commentatio | Required |

Flow: `indagatio output → user adds Latin callouts → purgatio detects → calls commentatio`

## Trigger Patterns

**Batch (all notes for date)**:
- `/purgatio`, `/purgatio 2025-10-15`
- "Process today's quick notes", "Daily processing"

**Interactive (user selects)**:
- `/purgatio --select`
- "Process quick notes, let me choose"

**Single note**:
- `/purgatio "keyword"`, `/purgatio 새 스킬`
- "Process the note about [topic]"

---

## Dual Mode

| Mode | Trigger | Flow |
|------|---------|------|
| **A: Full Processing** | User direct (`/purgatio`) | Quick notes → Refinement → Archive → Daily Note |
| **B: Report-Only** | Other skills (elevatio, unio, memorio, commentatio) | Receive data → Append to Daily Note Activity Log |

See `references/skill-report-format.md` for report format.

---

## Commentatio Detection

Detect Latin callouts (`[!quaero]`, `[!emendo]`, `[!disco]`, `[!facio]`, `[!moneo]`, `[!cogito]`) during quick note processing.

**Regex**: `>\s*\[!(quaero|emendo|disco|facio|moneo|cogito)\]`

**Rule**: ANY single Latin callout → call commentatio → include synthesis in Daily Note report.

**Mixed content**: Note can have regular content AND Latin callouts. Purgatio processes entire note, commentatio extracts callouts separately. Standard Obsidian callouts (`[!note]`, `[!warning]`, etc.) do NOT trigger.

**Edge cases**: Empty callout still triggers (ask user to clarify). Nested callouts both processed. Callout inside code block NOT detected.

## Vault Structure

See `../_common/references/vault-structure.md` for full structure.
See `../_common/references/frontmatter-standard.md` for status taxonomy and field standards.

**Key Paths**:
- Quick notes: `00. INBOX/01. Quick Notes/notes/`
- Archive: `00. INBOX/01. Quick Notes/archive/`
- Projects: `50. PROJECTS/51. Active/`
- Daily reviews: `60. JOURNAL/61. Daily Reviews/`
- Essays: `70. CREATIONS/72. Essays/`

## Core Workflow

### Timestamps

Always use `date "+%Y-%m-%d %H:%M"` — never guess time.

### Step 1: Daily Note Preparation (MANDATORY FIRST)

**Run BEFORE any note processing. Never skip.**

1. **Find today's daily note**: `61. Daily Reviews/YYYY/MM/YYYY-MM-DD.md`
   - If missing → create from template (see `references/daily-summary-format.md`)
2. **Task Carry-Over**: Read previous daily note → collect `- [ ]` tasks
   - Filter by day type (see `references/task-carry-over.md`)
   - Add to `### 🔄 Carried Over` section
   - If no incomplete tasks → add `> [!info] No incomplete tasks from yesterday`
3. **Add purgatio task to Today**: `- [ ] #task/hobby Purgatio 처리`

This step takes <30 seconds. No excuse to skip.

### Step 2a: Collect Quick Notes

Filename pattern: `2026.01.04-14h30m Title`

| Mode | Trigger | Behavior |
|------|---------|----------|
| Batch (default) | `/purgatio`, `/purgatio 2025-10-20` | All notes for date |
| Interactive | `/purgatio --select` | User selects from numbered list |
| Single | `/purgatio "keyword"` | Filter by keyword, ask if ambiguous |

**Optimization**: Parallel Read all selected notes in one message. For incremental runs, skip already-archived files.

### Step 2b: Staleness Detection

See `references/staleness-detection.md` for full logic.

Action words trigger detection → AI extracts context → Search activity logs → User confirms (Skip/Process/Review). On skip: archive with `superseded: true`.

---

### Step 3: Atomicity Validation

See `references/atomicity-check.md` for detailed criteria.

**Detection patterns**: Topic transition signals ("and", "also"), mixed projects
**Action**: If split needed, confirm with user before splitting

### Step 4: Create AI-Refined Version

**Title Rules**:
- Archive: Keep original filename (with timestamp) - identifies as original
- Refined: `r{##}. {Content Title}` format
  - `r` = refined prefix
  - `{##}` = sequential number (01, 02, 03...)
  - Per-project numbering (each project starts from r01)
  - ❌ `2026-01-04 Arduino Log`
  - ❌ `Arduino Log 01` (no prefix)
  - ✅ `r01. Arduino Setup Guide`
  - ✅ `r02. Work and Hobby Boundaries`

**Project Folder Structure**:
```
project/
├── README.md
├── notes/      ← Reference materials, guides (things to READ)
├── refined/    ← Processed thoughts → outputs (my WRITINGS)
├── logs/       ← Simple records (what I DID)
└── scripts/    ← Code files
```

**Location Decision**:

See `references/destination-guide.md` for full routing logic and destination map.

**Quick Reference**:
- Project-related → `{project}/refined/`
- Reflection/personal → `70. CREATIONS/72. Essays/`
- Source-based (image, video, report) → `20. LITERATURE/`
- Knowledge/concept → `30. KNOWLEDGE/{domain}/` (see vault-structure.md for domain classification)
- Idea needing development → `10. INCUBATING/11. Ideas/`
- New project needed → call elevatio
- No clear fit → Fallback Logic (ask user)

**When to call elevatio** (new project creation):
1. No existing project covers this topic
2. Note meets "New Project Criteria" below
3. User confirms "this deserves a new project"

**New Project Criteria** (should meet most):
- Expected work duration: 1+ weeks
- Multiple notes/tasks will spawn from this
- Has clear goal or deliverable
- Doesn't fit into any existing project

**용어 영어 병기**: 한국어 작성 시 기술 용어에 `용어 (English term)` 형식 적용 (see CLAUDE.md Foreign Term Notation).

**Common Processing**:
- Spelling correction (archive keeps original)
- Questions → Q&A format documentation
- Connect with existing notes via Wikilink
- Term definitions, next action suggestions
- **Fact-checking and duplicate checking → deferred to Step 4b**

**Difficulty Assessment** (during refinement):

When creating the refined version, assess and set `difficulty: 1-10`:

1. If source note already has `difficulty:` → inherit (adjust ±1 if refinement changes complexity)
2. Otherwise, assess based on refined content:
   - Concept density: 1-2 concepts → low (3-4), 3-5 → medium (5-6), 6+ → high (7-8)
   - Technical jargon: none → -1, moderate → 0, heavy → +1
   - Prior knowledge required: none → -1, some → 0, specialized → +1
3. Clamp result to 1-10

Add `difficulty: {value}` to refined note frontmatter.

**Image Detection** (auto):

See `references/image-detection.md` for keywords and format.

- Scan for hardware/visual keywords (breadboard, circuit, LED, diagram, etc.)
- If found and no image/ASCII exists → add `[!image-needed]` placeholder
- If ASCII exists → note "photo recommended" in placeholder
- Placeholder searchable for later image addition (elevatio or manual)

**Meeting Notes** → Route based on project association

See `references/meeting-brief-format.md` for detailed format and structure.

| Meeting Type | Destination | Examples |
|-------------|-------------|----------|
| Project-specific | `{project}/refined/` | 프로젝트 피드백, 기술 미팅 |
| Series/recurring | `65. Meeting Records/post/` | 팀 정기회의, All Hands |
| No project yet | `65. Meeting Records/post/` | 외부 미팅, 아키텍처 리뷰 |
| Pre-meeting prep | `65. Meeting Records/pre/` | 미팅 브리프, 준비 자료 |

**Key rules:**
- Project meetings go to project `refined/` folder (follow rXX naming)
- Non-project meetings go to `65. Meeting Records/post/`
- Pre/Post detection is content-based (no hardcoding)
- Pre ↔ Post: Bidirectional links
- Project meetings: backlink in file (`## 🏠 Project Home`) + README 역링크
- Indexing: `logs/meetings-index.md` (master), `logs/{series}.md` (recurring)

**Bidirectional linking required**:
- Refined: `## Source\n- Original: [[original title]]`
- Archive: `## Purgatio\n- Refined: [[refined title]]`

**Cross-project linking**: Keep note in **primary project** (whose capability is exercised), add bidirectional wikilinks to related projects' READMEs. If ambiguous → ask user.

### Step 4b: Verification (Duplicate & Fact Check)

See `references/verification-checklist.md`. Runs AFTER refinement, BEFORE archiving.

3 phases: A. Duplicate Check → B. Fact Check → C. Correction. Intensity: opinion=Light, technical=Standard, indagatio=Deep, meeting=Light. Always check: similar note exists? wikilinks valid?

---

### Step 5: Move Original to Archive

Move (never delete) to `archive/`. Process: patch `status: archived` → append Purgatio section → move. See `references/archive-format.md`.

### Step 6: Type-Specific Refinement

**6a. Project-Related**: Create in `refined/` (NOT notes/). Reflect actionable ideas back into project notes/README (methodology→phase note, prerequisites→README curriculum, references→wikilinks). Raw reasoning stays only in refined/.

**6b. Reflection/Personal**: Refine to essay → `70. CREATIONS/72. Essays/`. See `references/destination-guide.md`.

### Step 7: Finalize Daily Note

**7a. Mark purgatio task complete**:
```markdown
- [x] #task/hobby Purgatio 처리 — N건 처리 완료
```

**7b. Add Processing Summary** to `## 📊 Daily Processing Summary` section.
See `references/daily-summary-format.md` for format.

**7c. Add Key Insights** to `## 🧠 Thoughts & Ideas` section.
Extract 1-3 insights per refined note created.

### Step 8: Log Recording

**Central Log**: Record processing details in `90. SYSTEM/96. Data/Log-00-Processing.md`

**Location-Based Log**: Additionally log to destination-specific location:

| Destination | Log Location |
|-------------|--------------|
| `50. PROJECTS/51. Active/{project}/` | `{project}/logs/activity.md` |
| `10. INCUBATING/` | `90. SYSTEM/96. Data/Log-10-Incubating.md` |
| `20. LITERATURE/` | `90. SYSTEM/96. Data/Log-20-Literature.md` |
| `30. KNOWLEDGE/` | `90. SYSTEM/96. Data/Log-30-Knowledge.md` |
| `70. CREATIONS/` | `90. SYSTEM/96. Data/Log-70-Creations.md` |

**Create log file if not exists** (see `../_common/references/logging-standard.md`).

See `references/log-format.md` for format.

### Step 9: Completion Check

1. Verify refined version created
2. Verify Step 4b verification completed (duplicate + fact check)
3. Verify original moved to Archive
4. Verify bidirectional links
5. Verify Step 1 completed (task carry-over + daily note)
6. Verify Step 7 completed (task marked done + processing summary)
7. Verify log recorded

**Important**: Do NOT delete. Move to Archive only.

## User Confirmation Points

1. Split method when atomicity violated
2. Ambiguous project connection
3. New project creation decision
4. Reflection → Essay (confirm before refining)

## Integration

### Outbound (purgatio calls)
- **commentatio**: When Latin callouts detected in quick note
- **elevatio**: When new project creation needed

### Inbound (other skills call purgatio)
- **elevatio**: After project creation → report-only mode
- **unio**: After project merge → report-only mode
- **memorio**: After project archive → report-only mode
- **commentatio**: After synthesis (if called directly) → report-only mode

### Daily Note as Central Hub
All skill activities flow through purgatio (report-only mode) to Daily Note:
```
Any Skill → purgatio(report_only) → Daily Note Activity Log
```

---

## Reference Files

- `references/destination-guide.md` - Routing logic and destination map
- `references/log-format.md` - Global and per-project log format
- `references/daily-summary-format.md` - Daily note summary format
- `references/staleness-detection.md` - Stale note detection logic
- `references/atomicity-check.md` - Atomicity validation criteria
- `references/meeting-brief-format.md` - Meeting document format
- `references/archive-format.md` - Archive format
- `references/verification-checklist.md` - **Duplicate & fact-check workflow (Step 4b)**
- `references/task-carry-over.md` - Task carry-over logic
- `../_common/references/vault-structure.md` - Vault structure
- `../_common/references/frontmatter-standard.md` - Frontmatter standards
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

