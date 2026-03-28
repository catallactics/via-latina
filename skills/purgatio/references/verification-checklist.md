# Verification Checklist Reference

> Post-refinement verification workflow for duplicate detection and fact-checking.
> Runs as Step 4b, AFTER refined note creation but BEFORE archiving.

---

## When to Run

**Always run** after creating a refined note. Verification intensity scales with note type:

| Note Type | Duplicate Check | Fact Check | Depth |
|-----------|----------------|------------|-------|
| User-written (opinion/reflection) | Light | Skip | Low |
| User-written (technical) | Standard | Standard | Medium |
| indagatio output (web/docs) | Standard | **Deep** | High |
| indagatio output (paper/research) | Standard | **Deep** | High |
| Meeting notes | Light | Light | Low |

---

## Phase A: Duplicate Check

### Purpose
Prevent creating notes that substantially overlap with existing project content.

### Procedure

```
1. Identify destination project/folder
2. Read related existing notes (parallel reads)
   - Same project: README + notes in same folder
   - For refined/: read existing refined/ notes
   - For notes/: read notes with similar topic
3. Compare section-by-section
4. Classify each section
```

### Classification

| Classification | Criteria | Action |
|---------------|----------|--------|
| **Unique** | Content not found elsewhere | Keep as-is |
| **Partial overlap** | Similar topic, different angle/depth | Keep with cross-reference callout |
| **Duplicate** | Same information, same depth | Remove or replace with reference link |
| **Contradicts** | Conflicts with existing note | Investigate, flag to user |

### Cross-Reference Callout Format

When content overlaps but serves different purpose:

```markdown
> [!note] Related Content
> Detailed coverage in [[existing-note|Note Title]]. This section provides [navigation/summary/alternative angle].
```

### Output

Report to user before proceeding:

```
### Duplicate Check Results

| Section | Overlap With | Classification | Action |
|---------|-------------|---------------|--------|
| {section name} | {existing note} | Unique/Partial/Duplicate | Keep/Add ref/Remove |
```

---

## Phase B: Fact Check

### Purpose
Verify factual claims before they become permanent vault content.

### What to Check

| Check Type | Target | Method |
|-----------|--------|--------|
| **URL validity** | All URLs in note | WebFetch (fetch and verify response) |
| **Technical terms** | API names, library names, version numbers | WebSearch or official docs |
| **Numeric claims** | Statistics, counts, measurements | Cross-reference with source |
| **Structural claims** | "X has N sections", "consists of..." | Verify against actual source |
| **Implementation claims** | Config values, code behavior, system state | Read actual source files (config, code, dashboards) |
| **Temporal claims** | Release dates, "as of 2026" | WebSearch for latest info |

**Priority**: Implementation claims > Structural claims > URLs > Numbers > Technical terms > Dates

**Methods**: Read (config/code files), WebFetch (URLs), WebSearch (versions/dates), Grep (vault), official docs (APIs)

### Implementation Verification (for technical/system notes)

When a note describes implementation details (config values, weights, file counts, code behavior):

1. **Identify source of truth**: config files, code, dashboards
2. **Read actual files** and compare against note claims
3. **Mark discrepancies**: note may be outdated if implementation changed after writing
4. **Correct refined version** to match current implementation state
5. **Add timestamp caveat** if implementation is actively evolving

Common discrepancy causes:
- Note written mid-implementation, values changed later
- Feature added after note was written (e.g., new dimension/weight)
- Counts changed as vault grew (file counts, tag counts)

**Markers**: No marker = verified, `⚠️` = partial, `[!warning]` callout = unverified/changing

---

## Phase C: Correction

### If Issues Found

| Issue | Action |
|-------|--------|
| Duplicate section | Remove, add cross-reference link |
| Incorrect fact | Correct in refined note |
| Broken URL | Remove or replace with working URL |
| Outdated info | Update with current info + add date caveat |
| Structural error | Correct structure description + add warning callout |

### Correction Log

After corrections, report changes:

```
### Verification Results

| Item | Status | Detail |
|------|--------|--------|
| URL: {url} | ✅/❌ | {result} |
| Claim: "{claim}" | ✅/⚠️/❌ | {finding} |
| Duplicate: {section} | Removed/Kept | {reason} |
```

---

## Skip Conditions

Verification can be **lightened** (not skipped entirely) when:

1. **Personal reflection/essay**: No factual claims to verify
2. **Meeting notes**: First-hand account, facts are user's own
3. **Task/todo notes**: Action items, not factual claims

Even for skipped cases, always do:
- Basic duplicate check (does a similar note already exist?)
- Link validity check (do wikilinks point to real notes?)

---

## Integration with Daily Note

Verification results are included in the Daily Processing Summary:

```markdown
### Verification

| Refined Note | Checks | Issues Found | Corrections |
|-------------|--------|-------------|-------------|
| [[note]] | Dup ✅ Fact ✅ | {count} | {count} |
```

---

