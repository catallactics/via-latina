---
name: elevatio
description: "Obsidian project creation skill (Elevatio = elevation). Creates new projects from notes - assigns project number, builds folder structure, creates README. Triggers on 'make this a project', 'create project from', '/elevatio', '이것을 프로젝트로 만들어줘', '새 프로젝트 생성', '프로젝트로 승격'. Called directly or via purgatio when new project needed. Use this skill whenever the user wants to formalize a note or idea into a structured project with registry number and folder hierarchy."
argument-hint: "[[note name]]"
---

# Elevatio

> *Elevatio* (Latin): elevation, raising, lifting

Skill for creating new projects from notes.

## Trigger Patterns

**Direct invocation:**
- "Make this a project"
- "Create project from this"
- "This deserves its own project"
- `/elevatio`
- `/elevatio [[note name]]`

**Called from purgatio:**
- When purgatio determines a note needs a new project

## Core Responsibility

**One job: Create new projects.**

Does NOT handle:
- Adding notes to existing projects (purgatio does this)
- Refining notes without project creation (purgatio does this)
- Daily note processing (purgatio does this)

---

## Workflow

### Step 1: Analyze Source Note

1. Read source note content
2. Extract key topics and keywords
3. Verify no existing project covers this topic
   - If related project exists → suggest using purgatio instead
   - If no related project → continue

### Step 2: Determine Project Type

**Project types:** Learning, Development, Research, Creative

Load `references/project-types.md` for criteria.
See `../_common/references/frontmatter-standard.md` for status/phase fields.

**Learning project check:**
When source note contains mixed complexity levels (basic setup + advanced strategy), plan phases to separate them by complexity. Identify which content requires prior knowledge and sequence accordingly. See `references/learning-project.md` → Phase Design Principles.

**User confirmation required:**
1. Project name
2. Project type (verify auto-detection)

### Step 3: Assign Project Number

**Registry-based assignment:**

1. Read `90. SYSTEM/93. Registries/Project-Number-Registry.md`
2. Find "Next Available Number"
3. Reserve that number

**Numbering system:**
- Format: `50-NNNNNN` (6-digit zero-padded)
- Range: 50-000001 ~ 50-999999
- Current: check Registry for next available

**Note:** Numbers are never reused, even after archive.

### Step 4: Create Folder Structure

```
50. PROJECTS/51. Active/50-NNNNNN. {Project Name}/
├── README.md
├── notes/
├── refined/
└── logs/
    └── activity.md
```

- `scripts/` created only when needed

**Notes Numbering**: `p10.` (phases, expandable) / `n01.` (refs, sequential).
See `references/notes-numbering.md` for detailed guide.

### Step 5: Create README

**용어 영어 병기 규칙**:
README 작성 시 중요 용어는 `용어 (English term)` 형식으로 영어 병기.
- 예: `임베딩 (Embedding)`, `벡터 데이터베이스 (Vector Database)`
- 대상: 기술 용어, 전문 용어, 고유명사, 약어

Use template: `90. SYSTEM/91. Templates/Template_50. Project.md`

Configure based on project type:
- Load `references/{type}-project.md`
- Apply type-specific sections

### Step 5b: Phase Validation (Learning projects only)

After drafting phase structure, run Phase Validation Questions from `references/learning-project.md`.
If any anti-pattern detected, flag to user with suggested restructuring before creating files.
This is a soft check — user has final say on structure.

### Step 6: Update Registry

Update `90. SYSTEM/93. Registries/Project-Number-Registry.md`:
1. Add new row to "Active Projects" table
2. Increment "Next Available Number"

See `../_common/references/registry-format.md` for detailed format.

### Step 7: Handle Source Note

- **Direct call**: Move source to `{project}/refined/` as first refined note
- **From purgatio**: Return project path; purgatio handles note placement

### Step 8: Check Image Placeholders

After project creation, scan for `[!image-needed]` placeholders:

1. Search all `.md` files in project folder
2. If placeholders found → notify user with list
3. Display summary: "3 images needed in this project"

**User options:**
- Add images now (remove placeholder after)
- Defer to later (placeholders remain searchable)

See `references/image-placeholder.md` for format and removal workflow.

### Step 9: Log Recording

Add entry to `logs/activity.md`:
```markdown
## YYYY-MM-DD
- Project created from: [[source note]]
- Type: {project-type}
```

### Step 10: Report to Daily Note

- **Direct call**: Call purgatio in report-only mode with project creation data
- **From purgatio**: Return data; purgatio includes in daily report

See `~/.claude/skills/purgatio/references/skill-report-format.md` for format.

---

## User Confirmation Points

1. Project name
2. Project type
3. Confirm no existing project covers this topic

---

## Reference Files

- `references/project-types.md` - Type determination criteria
- `references/notes-numbering.md` - Notes folder file numbering (p10. for phases, n01. for refs)
- `references/learning-project.md` - Learning type setup
- `references/dev-project.md` - Development type setup
- `references/research-project.md` - Research type setup
- `references/creative-project.md` - Creative type setup
- `../_common/references/registry-format.md` - Registry update format (new projects)
- `references/image-placeholder.md` - Image placeholder search and handling
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

---

## Integration

### Inbound
- **purgatio**: Calls elevatio when new project needed during processing
- **commentatio**: Calls elevatio when synthesis reveals project-worthy topic

### Outbound
- **purgatio**: Report project creation (report-only mode) for Daily Note
- **unio**: Merge projects when needed
- **memorio**: Archive completed projects when done

---

## Output Format Rules

**SKILL.md content**: English (context efficiency)
**User-facing output**: Korean (user preference)
**Trigger patterns**: English + Korean (accessibility)
