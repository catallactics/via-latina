# Vault Folder Review Template

Quick Note template for reviewing internal vault folders.

---

## Output Format

```markdown
---
created: YYYY-MM-DD HH:mm
type: indagatio
source_type: vault-folder
source_path: <folder path>
scale: <by folder size>
tags: [indagatio, vault-review, <folder-type>]
status: inbox
---

# {Folder Name} Review

## Source
| Field | Value |
|-------|-------|
| Path | `{full vault path}` |
| Reviewed | YYYY-MM-DD HH:mm |

## Overview
| Item | Value |
|------|-------|
| Project Number | {if applicable} |
| Status | {from README frontmatter} |
| Created / Last Modified | {dates} |

## Folder Structure
{tree view}

## File Summary
- **README.md**: Goal + current state
- **notes/**: {count} files, brief per file
- **refined/**: {count} files, origins
- **logs/activity.md**: last 3 entries

## Progress
- [x] Completed items
- [ ] In-progress items
- [ ] Not started items

## Connections
- [[Related Project]] - {reason}

## Analysis
- Strengths: {observations}
- Improvements: {suggestions}

## Expansion Suggestions (External)
| Suggestion | Content | Context |
|------------|---------|---------|
| {type} | {specific} | {why} |

## Action Items
- [ ] {next steps}
```

---

## Scale: Small (<5 files) | Medium (5-20) | Large (20+)

## Analysis Guidelines

1. **Structure**: Identify folder type, check for standard structure (README, notes/, logs/), note non-standard organization
2. **Content**: README goal/status/progress, notes summary, logs patterns, refined outputs
3. **Progress**: Extract tasks, categorize (completed/in-progress/not started), identify blockers
4. **Connections**: Find wikilinks, related projects, knowledge dependencies
5. **Internal Improvements**: Documentation gaps, outdated content, missing structure
6. **Expansion Suggestions** (CRITICAL): Use indagatio's external source strength. Analyze current level, project purpose, tech stack, trends → recommend next-level resources, related tools, practical applications, communities

## Post-Review Workflow

Quick Note created → user review (add callouts) → purgatio → commentatio (if callouts)

## Example Triggers

`/indagatio [[50-NNNNNN. Project-Name]]`, "Review this project", `/indagatio 50. PROJECTS/51. Active/50-NNNNNN. Project-Name`
