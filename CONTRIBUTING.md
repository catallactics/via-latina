# Contributing to Via Latina

Thanks for your interest in contributing! Via Latina is a knowledge lifecycle system built on Claude Code skills, and we welcome contributions that extend or improve the ecosystem.

## How to Contribute

### Reporting Issues

- Use GitHub Issues for bugs, broken workflows, or inconsistencies
- Include your Claude Code version and Obsidian version
- Describe what you expected vs. what happened

### Suggesting New Skills

New skills should:

1. **Follow Latin naming** — pick a Latin word that describes the skill's purpose
2. **Integrate with the pipeline** — connect to existing skills where it makes sense
3. **Use `_common/references/`** — share the same data model (frontmatter, types, vault structure)
4. **Include references/** — document edge cases, formats, and decision logic

### Submitting PRs

1. Fork the repo
2. Create a feature branch
3. Add or modify skills in `skills/`
4. Update README.md if adding a new skill
5. Open a PR with a clear description

## Skill Structure

Each skill follows this structure:

```
skills/
└── your-skill/
    ├── SKILL.md           # Main skill definition (required)
    └── references/        # Supporting documents (optional)
        ├── format.md
        └── edge-cases.md
```

### SKILL.md Frontmatter

```yaml
---
name: skill-name
description: "When to trigger this skill..."
argument-hint: "<expected input>"
---
```

## Code of Conduct

Be respectful, constructive, and collaborative. We're building tools for knowledge, not arguments.
