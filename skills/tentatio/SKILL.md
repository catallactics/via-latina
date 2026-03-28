---
name: tentatio
description: "Vault-wide quality audit skill (Tentatio = testing, trial, proof). Detects batch metadata inconsistencies: folder/status mismatches, non-standard status values, registry/folder sync errors, missing READMEs, skill health issues, and knowledge freshness. Complements real-time hooks (which catch single-file issues at write time) by scanning historically accumulated problems across the entire vault. Triggers on '/tentatio', 'vault audit', 'check vault health', 'run quality checks', etc."
argument-hint: "[project / --skills / --knowledge / --evolve]"
context: fork
agent: Plan
---

# Tentatio

> *Tentatio* (Latin): testing, trial, proof

Vault-wide batch quality audit. Catches systemic inconsistencies that real-time hooks cannot detect (root cause: lifecycle skills move folders but don't always batch-update child frontmatter).

## Trigger Patterns

| Trigger | Scope | Description |
|---------|-------|-------------|
| `/tentatio` | `full` | Full vault audit |
| `/tentatio 50x` | `project` | Single project audit |
| `/tentatio --skills` | `skills` | Skill health only |
| `/tentatio --knowledge` | `knowledge` | Knowledge freshness only |
| `/tentatio --knowledge 33` | `knowledge` | Knowledge freshness for specific domain folder |
| `/tentatio --sessions` | `sessions` | Session follow-up tracker |
| `/tentatio --evolve` | — | Self-evolution analysis |

## Vault Structure

→ See `../_common/references/vault-structure.md` for full structure.
→ See `../_common/references/frontmatter-standard.md` for status taxonomy.

**Key Paths**:
- Projects: `50. PROJECTS/51. Active/`, `52. Someday/`, `53. Pending/`, `54. Archive/`
- Knowledge: `30. KNOWLEDGE/`, `20. LITERATURE/`
- Registry: `90. SYSTEM/93. Registries/Project-Number-Registry.md`
- Reports: `90. SYSTEM/97. Verification-Reports/`
- Skills: `~/.claude/skills/`

**Always excluded**: `100. SECURE/**`, `90. SYSTEM/91. Templates/**`

## Checks

10 checks via `references/check-manifest.md`. Load only checks matching current scope (lazy-loading).

## Core Workflow (4 Steps)

### Step 1: Collect & Scan

1. Read `references/check-manifest.md`
2. Determine scope from trigger → filter manifest by scope
3. For each matching check: load reference file, execute sequentially
4. Each check follows: Scope → Detection → Classification → Auto-Fix
5. Collect issues as: `{check_id, severity, file_path, current_value, expected_value}`

### Step 2: Report

Display Korean summary table, save detailed report to `90. SYSTEM/Verification-Reports/YYYY-MM-DD-tentatio.md`.

→ See `references/report-template.md` for format and section matrix.

### Step 3: Fix (optional)

Ask user: "자동수정 가능한 {M}건을 수정할까요?"

Auto-fix patterns (see individual check Section 4 for details):
- README.md folder/status sync → change status to match folder
- Deprecated values → 1:1 mapping per `frontmatter-standard.md`
- Deprecated types → 1:1 mapping per `type-standard.md`
- Knowledge metadata → user-confirmed tier/review_by

Individual notes in archived projects: **report only**.
Method: `Edit` tool for frontmatter edits.

### Step 4: Done

Report saved. If auto-fixes applied, show count and suggest re-running `/tentatio`.

### Step 5: Self-Evolution (--evolve)

Runs independently of standard checks.

1. Load check-manifest.md → all registered checks and scopes
2. Run 5 detection rules:
   - Uncovered Folders: vault top-level folders not in any check's Scope.paths
   - Unvalidated Fields: frontmatter fields used in 10+ files without validation
   - Unregistered Checks: check-*.md files not in manifest
   - Manifest Drift: manifest entries with no matching file
   - Structure Drift: vault-structure.md entries vs actual vault folders (find mismatches)
3. Generate coverage matrix
4. Present gaps to user via AskUserQuestion → user confirms before new check creation

Report format: → See `references/report-template.md` Self-Evolution section.

## Excluded Checks

Hooks handle: standard markdown links, AI model anachronisms, Unicode checkboxes, verification markers. Purgatio handles stale inbox notes. Broken wikilinks deferred.

## Integration

Standalone audit tool. Related: memorio/suspensio/dilatio (root cause of mismatches), purgatio (individual note quality), elevatio (initial project creation). Root cause fix: lifecycle skills now batch-update child frontmatter (see `_common/references/child-frontmatter-update.md`).

## Output Format Rules

**Skill files**: English (token efficiency)
**User-facing output**: Korean (user preference)
