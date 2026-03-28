# Destination Guide Reference

Routing logic for commentatio synthesis outputs.

---

## Decision Flow

`Source from project? → YES: project/refined/ | NO: Check content type → MATCH: route | NO MATCH: ask user`

---

## Destination Map

| Source Type | Destination |
|-------------|-------------|
| Project quick note / doc review | `{project}/refined/` |
| Paper/article analysis | `20. LITERATURE/` |
| Concept deep dive | `20. LITERATURE/` or `30. KNOWLEDGE/{domain}/` |
| Course material | Related project or `20. LITERATURE/` |
| Undeveloped idea | `10. INCUBATING/11. Ideas/` |
| Recurring theme | `10. INCUBATING/12. Themes/` |
| Skill/workflow review | `90. SYSTEM/` or related project |
| GitHub/API docs review | Related project |
| General research | `20. LITERATURE/` |

---

## Detection Logic

**Project detection**: Check file path (`50. PROJECTS/`), source content (project name mentions), callout content ([!facio] project references).

**Content type keywords**:

| Keywords | Destination |
|----------|-------------|
| skill, workflow, automation | `90. SYSTEM/` or related project |
| learn, study, concept, theory | `20. LITERATURE/` or `30. KNOWLEDGE/` |
| paper, research, analysis | `20. LITERATURE/` |
| idea, maybe, someday | `10. INCUBATING/11. Ideas/` |
| recurring, pattern, theme | `10. INCUBATING/12. Themes/` |

---

## Fallback: Ask User

When destination unclear after content analysis + vault search:

```markdown
> [!question] Destination
> **Topic**: {main topic}
> **Options**:
> 1. `20. LITERATURE/` (learning/research)
> 2. `30. KNOWLEDGE/{domain}/` (established knowledge)
> 3. `10. INCUBATING/11. Ideas/` (idea)
> 4. `{project}/refined/`
> 5. New project (elevatio)
```

---

## Filename Convention

`{Topic} Synthesis.md`

## Multiple Projects

Save to primary project's `refined/`, add links to related projects, update their READMEs.

## Archive Rule

Quick Note → Archive (purgatio). Synthesis → Stays in destination (permanent output, not raw input).

## Extensibility

See `_common/references/vault-structure.md` for folder structure.
