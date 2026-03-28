---
name: commentatio
description: "Obsidian annotation synthesis skill (Commentatio = commentary/reflection). Two modes: (1) Synthesis Mode - creates new document from annotated materials, (2) Project Note Mode - directly edits project notes based on Latin callouts. Triggers on '/commentatio', Latin callouts, direct invocation, '주석 정리해줘', '라틴 콜아웃 처리'. Use this skill whenever notes contain Latin callouts (quaero, emendo, disco, facio, moneo, cogito) or when the user wants to synthesize annotated content into a cohesive document."
argument-hint: "[[file name]]"
---

# Commentatio

> *Commentatio* (Latin): commentary, reflection, deep thinking

Skill for processing Latin callout annotations - either synthesizing into new documents or directly editing project notes.

## Philosophy

```
Material + My Annotations = Deep Understanding
```

**Core Principle**: Latin callouts = marking my thoughts on content I didn't write.

**Two Modes**:
- **Synthesis Mode**: External materials + annotations → New synthesis document
- **Project Note Mode**: Project notes + annotations → Direct edits to same note

Unlike purgatio (context parsing for user-written content), commentatio processes explicit annotations on non-user-written content.

## When to Use Latin Callouts

| Situation | Latin Callout | Alternative |
|-----------|--------------|-------------|
| indagatio output with comments | Required | - |
| AI-generated document feedback | Required | - |
| External source annotations | Required | - |
| Project note review | Available | - |
| Existing content modification | Available | Quick Note → purgatio |
| New thoughts/ideas | Not needed | purgatio handles context |

**Key distinction**:
- **User writes new content** → purgatio (no callouts needed)
- **User annotates existing content** → commentatio (callouts mark annotations)

---

## Trigger Patterns

**Direct invocation:**
- `/commentatio`
- `/commentatio [[file name]]`
- "Process my comments", "Synthesize annotations"

**Called from purgatio:**
- When quick note contains Latin callouts

---

## Latin Callouts

Commentatio uses 6 Latin callouts to distinguish from regular Obsidian callouts:

| Callout | Meaning | Korean | AI Response |
|---------|---------|--------|-------------|
| `[!quaero]` | I ask | 질의 | Answer + related resources |
| `[!emendo]` | I improve | 제의 | Specific improvement suggestions |
| `[!disco]` | I learn | 탐구 | Learning roadmap + keywords |
| `[!facio]` | I do | 과제 | Convert to task |
| `[!moneo]` | I remind | 각인 | Highlight + vault connections |
| `[!cogito]` | I think | 공상 | Acknowledge + expand freely |

### Usage Examples

```markdown
> [!quaero] 질의
> What exactly is PWM?

> [!emendo] 제의
> This structure should be reorganized

> [!disco] 탐구
> Need to study this concept deeper

> [!facio] 과제
> Update the README

> [!moneo] 각인
> Must remember this for future!

> [!cogito] 공상
> Random thought about this...
```

See `references/callout-types.md` for detailed callout handling.

---

## Input Types

| Type | Example | Detection |
|------|---------|-----------|
| Quick note + annotations | `2026.01.05-14h11m Skill Review.md` | Latin callouts |
| AI report + annotations | Saved conversation with comments | Latin callouts |
| GitHub docs + annotations | README copied with comments | Latin callouts |
| Paper/article + annotations | Quotes with comments | Latin callouts |

---

## Operating Modes

Commentatio operates in two modes based on file location:

| Mode | Detection | Output |
|------|-----------|--------|
| **Synthesis Mode** | Files outside `50. PROJECTS/` | New synthesis document |
| **Project Note Mode** | Files inside `50. PROJECTS/` | Direct edit to same note |

```
IF file path contains "50. PROJECTS/"
  → Project Note Mode (direct edit)
ELSE
  → Synthesis Mode (create document)
```

---

## Core Workflow

### Step 1: Identify Source & Mode

1. Receive file path (from purgatio or direct call)
2. Read file content
3. **Detect mode**: Check if path contains `50. PROJECTS/`
4. Identify source type (quick note, project note, external doc, etc.)

### Step 2: Extract Callouts

1. Parse all Latin callouts from content
2. Categorize by type (quaero, emendo, disco, etc.)
3. Extract surrounding context for each callout

```
For each callout:
  - Callout type
  - Callout content (user's annotation)
  - Surrounding context (what it refers to)
```

### Step 3: Process Each Callout Type

See `references/callout-types.md` for detailed per-callout handling.

**Critical rule**: `[!quaero]` → **WebSearch ALWAYS** (user explicitly marked as question requiring thorough answer).

---

## Project Note Mode (Direct Edit)

**When**: File path contains `50. PROJECTS/`

Callouts trigger **direct edits** to the note instead of creating a synthesis document.

### Callout → Action Map

| Callout | Action | Output Location |
|---------|--------|-----------------|
| `[!emendo]` | Direct section edit | Target section |
| `[!facio]` | Add task `- [ ]` | `## Action Items` or `## Next Steps` |
| `[!quaero]` | Add Q&A (WebSearch) | Below target section |
| `[!disco]` | Add learning roadmap | Target section or new section |
| `[!moneo]` | Add connection links | `## Related` or target section |
| `[!cogito]` | Add expanded content | Below target section |

### Processing Flow

1. **Scope Detection**: Find nearest heading above callout (H3 → H2 → H1 → note-wide). See `references/project-note-mode.md` for algorithm and examples.
2. **Execute Edit**: Apply callout-type action within scope.
3. **Remove Callout**: Delete processed callout block.
4. **Log**: Per `../_common/references/logging-standard.md`. Activity type = callout type.
5. **Report**: Return edit summary to purgatio (or call purgatio report-only mode if direct).

---

## Synthesis Mode (Document Creation)

**When**: File path does NOT contain `50. PROJECTS/`

This is the original workflow - creates a new synthesis document.

### Step 4: Generate Synthesis

**Bilingual Term Rule**:
When writing in Korean, include English in parentheses: `한국어 용어 (English Term)`.
- Example: `임베딩 (Embedding)`, `벡터 데이터베이스 (Vector Database)`
- Applies to: Technical terms, proper nouns, abbreviations

Create comprehensive synthesis document.

See `references/output-format.md` for template.

**Structure**: Summary → Callout Responses (by type) → Action Items → Key Insights → Connected Knowledge → Source. See `references/output-format.md` for full template.

### Step 5: Determine Destination

| Condition | Destination |
|-----------|-------------|
| Project-related | `{project}/refined/` |
| Learning/research | `20. LITERATURE/` |
| Knowledge/concept | `30. KNOWLEDGE/{domain}/` (see vault-structure.md) |
| Undeveloped idea | `10. INCUBATING/11. Ideas/` |
| Recurring theme | `10. INCUBATING/12. Themes/` |
| Skill review | `90. SYSTEM/` or project |
| Unclear | Ask user |

See `references/destination-guide.md` for routing logic.

### Step 6: Report to Daily Note

**If called from purgatio:**
- Return synthesis data to purgatio
- Purgatio includes in daily report

**If called directly:**
- Call purgatio in report-only mode

**Report data:**
```yaml
skill: commentatio
action: synthesis_created
summary:
  source_note: "[[Original]]"
  callout_counts:
    quaero: 3
    emendo: 2
    disco: 1
  key_insights:
    - "Insight 1"
    - "Insight 2"
```

See `~/.claude/skills/purgatio/references/skill-report-format.md` for format.

---

## Elevatio Trigger

If analysis reveals project-worthy topic (3+ disco on same subject, multiple facio suggesting ongoing work, or explicit "project"/"deep dive" mention), suggest elevatio. See `references/elevatio-trigger.md` for full criteria and suggestion format.

---

## User Confirmation Points

1. Destination for synthesis (if unclear)
2. Elevatio trigger (if detected)

---

## Reference Files

- `references/callout-types.md` - Detailed callout handling
- `references/output-format.md` - Synthesis template
- `references/destination-guide.md` - Routing logic
- `references/elevatio-trigger.md` - When to suggest new project
- `references/project-note-mode.md` - Project Note Mode examples and edge cases
- `../_common/references/logging-standard.md` - **Unified logging standard (v1.3.0)**

---

## Integration

### Inbound
- **purgatio**: Calls commentatio when Latin callouts detected

### Outbound
- **purgatio**: Report synthesis (report-only mode) for Daily Note
- **elevatio**: Suggest new project when appropriate

---

## Output Format Rules

**SKILL.md content**: English (context efficiency)
**User-facing output**: Korean (user preference)
**Trigger patterns**: English + Korean (accessibility)
