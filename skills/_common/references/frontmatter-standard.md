# Frontmatter Standard Reference

> Vault-wide frontmatter field standards for Via Latina skills

---

## Created Field (Timestamp)

**CRITICAL**: Never estimate or guess time. Always get actual system time before writing:

```bash
date "+%Y-%m-%d %H:%M"  # Output: 2026-01-24 08:52
```

**Format**: `YYYY-MM-DD HH:mm` (24-hour, no seconds)

**Required for**: All new files with frontmatter

**Example**:
```yaml
---
created: 2026-01-24 08:52
type: quick-note
status: pending
---
```

**Why this matters**:
- AI tends to guess "clean" times (15:30, 16:00) which are often wrong
- Incorrect timestamps break chronological tracking
- Future staleness detection relies on accurate creation times

**Applies to all skills**: indagatio, purgatio, elevatio, commentatio, etc.

---

## Status Taxonomy

### Layer 1: Universal Lifecycle

All notes use this `status` field for lifecycle tracking:

```
inbox → pending → active → refined → archived
                    ↓         ↓
              someday /    logged
              waiting
```

| Status | Description | Usage |
|--------|-------------|-------|
| `inbox` | Just collected | Auto-created by indagatio |
| `pending` | Awaiting processing | Recognized but not started |
| `active` | In progress | Currently working on |
| `refined` | Work complete | Done but still relevant |
| `archived` | Stored away | No longer active |
| `logged` | Completed work product | Done, recorded for reference (session summaries, changelogs) |
| `someday` | On hold | Intentionally paused (internal decision) |
| `waiting` | External trigger wait | Waiting for external event (via dilatio) |

### Layer 2: Domain-Specific Fields

Additional fields for specific content types:

#### Literature Notes (20. LITERATURE/)

```yaml
status: [active|refined]
maturity: [foundational|growing|evergreen]
```

| Maturity | Description |
|----------|-------------|
| `foundational` | Core concept, foundation for others |
| `growing` | Actively being developed |
| `evergreen` | Complete, rarely changes |
| `internalized` / `이해완료` | Understood, periodic review only |

#### Knowledge Freshness (30. KNOWLEDGE/ and 20. LITERATURE/)

Optional fields for knowledge obsolescence tracking (tentatio Check 6):

```yaml
tier: [timeless|slow-decay|medium-decay|fast-decay|rapid-decay]  # Knowledge freshness tier
review_by: YYYY-MM-DD  # Next review date (auto-calculated by tentatio Check 6)
```

| Field | Description | Source |
|-------|-------------|--------|
| `tier` | Knowledge freshness classification | Auto-inferred from domain/type, or manually set |
| `review_by` | Next scheduled review date | Auto-calculated from tier threshold |

**Tier values** (based on Half-Life of Knowledge, Machlup 1962):

| Tier | Review Trigger | Domains |
|------|---------------|---------|
| `timeless` | None | Philosophy, mathematics, psychology core |
| `slow-decay` | 24 months | Engineering protocols, natural sciences |
| `medium-decay` | 12 months | Software, digital tools, platforms |
| `fast-decay` | 6 months | Social media, cutting-edge tech |
| `rapid-decay` | 3 months | AI/ML, LLM tools (Claude Code, ChatGPT, Cursor), GenAI |

**Rules**: User-set `tier:` overrides auto-inference. `maturity: evergreen` promotes tier by one level. See `tentatio/references/check-6-knowledge-freshness.md` for full inference rules.

#### Difficulty (All learnable content)

Optional field for content complexity, used by Learning Queue scoring.

```yaml
difficulty: 5  # Content complexity 1-10
```

| Range | Label | Description |
|-------|-------|-------------|
| 1-2 | 입문(Introductory) | Simple captures, basic ideas |
| 3-4 | 기초(Basic) | Learning notes, introductory literature |
| 5-6 | 중급(Intermediate) | Established knowledge, concept definitions |
| 7-8 | 심화(Advanced) | Deep analysis, multi-domain content |
| 9-10 | 전문가(Expert) | Research papers, architecture designs |

**Sources**: Auto-set by indagatio (from source_type × scale), refined by purgatio (from content analysis), or manually set by user. Learning Queue infers from type + maturity + tags if not set.

#### Project Notes (50. PROJECTS/)

```yaml
status: [active|refined|someday|waiting|archived]
phase: [planning|development|review|complete]
```

| Phase | Description |
|-------|-------------|
| `planning` | Initial planning stage |
| `development` | Active work |
| `review` | Finishing, reviewing |
| `complete` | Work done (before archive) |

#### Source Notes (20. LITERATURE/)

```yaml
status: [pending|active|refined]
reading: [to-read|to-start|reading|finished|abandoned]
```

| Reading | Description |
|---------|-------------|
| `to-read` | Not started |
| `to-start` | Course not started |
| `reading` | Currently reading/watching |
| `finished` | Completed |
| `abandoned` | Stopped, won't continue |

### Layer 3: Special States

| Status | Description | Usage |
|--------|-------------|-------|
| `incubating` | Idea developing | 10. INCUBATING/ |
| `on-hold` | Group deferral | 00. INBOX/02. On-Hold/ (via locatio) |
| `reference` | Lookup only | Not for active work |
| `draft` | Writing in progress | 70. CREATIONS/ essays |

---

## Meta Fields (Flat Structure)

For special status information, use flat fields:

```yaml
# When note is superseded
status: archived
superseded: true
superseded_date: 2026-01-14

# When project is merged
status: archived
merged_into: "[[Other Project]]"

# When restored from archive
status: active
restored_from: archived
restored_date: 2026-01-14

# When archived
status: archived
archived_date: 2026-01-14
```

**Rule**: Only add meta fields when needed. Don't include empty fields.

---

## Folder → Field Mapping

| Folder | Required Fields | Optional Fields |
|--------|-----------------|-----------------|
| 00. INBOX/01. Quick Notes/ | `status`, `type` | - |
| 20. LITERATURE/ | `status`, `type`, `reading` | `author`, `url` |
| 20. LITERATURE/ | `status`, `maturity`, `type` | `source`, `related`, `tier`, `review_by` |
| 30. KNOWLEDGE/ | `status`, `type`, `domain`, `subdomain` | `maturity`, `tier`, `review_by` |
| 50. PROJECTS/ | `status`, `phase`, `type` | - |
| 60. JOURNAL/ | `type` | `day_type` |
| 10. INCUBATING/ | `status`, `type` | - |
| 70. CREATIONS/ | `status`, `type` | - |

---

## Via Latina Skill Transitions

| Skill | Input Status | Output Status |
|-------|--------------|---------------|
| **indagatio** | - | `inbox` |
| **purgatio** (original) | `pending` | `archived` |
| **purgatio** (refined) | - | `refined` |
| **purgatio** (stale) | `pending` | `archived` + `superseded: true` |
| **elevatio** | - | `active`, `phase: planning` |
| **memorio** | `active`/`refined` | `archived` |
| **vivificatio** | `archived`/`someday`/`waiting` | `active` or `someday` + `restored_from: [source]` |
| **dilatio** | `active` | `waiting` |
| **suspensio** | `active` | `someday` |
| **unio** (absorbed) | `active` | `archived` + `merged_into: [[]]` |
| **locatio** | `pending` | `on-hold` |

---

## Default Values by Template

| Template | status | Domain Field |
|----------|--------|--------------|
| Quick Note | `pending` | - |
| Source Note | `pending` | `reading: to-read` |
| Course Source | `pending` | `reading: to-start` |
| Course Module | `active` | - |
| Literature Note | `active` | `maturity: foundational` |
| Project | `active` | `phase: planning` |
| Meeting Note | `active` | - |
| Incubating Note | `incubating` | - |

---

## Migration Notes

### Deprecated Values → Standard Values

| Old Value | → New Value |
|-----------|-------------|
| `completed` | → `refined` |
| `complete` | → `refined` |
| `in-progress` | → `active` |
| `open` | → `active` |
| `ready` | → `pending` |
| `to-read` | → `pending` + `reading: to-read` |
| `to-start` | → `pending` + `reading: to-start` |
| `developing` | → `incubating` |

### Literature Notes Migration

Literature notes that only have `status: growing/evergreen/foundational`:
- Move value to `maturity` field
- Add `status: refined` (or `active` if being edited)

---

## Validation

Before creating/modifying notes, check:

1. `status` value is from Layer 1 list
2. Domain field matches folder (maturity for Literature, etc.)
3. Meta fields only added when needed
4. No deprecated values used
