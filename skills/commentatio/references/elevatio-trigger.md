# Elevatio Trigger Reference

When commentatio should suggest creating a new project via elevatio.

---

## Trigger Conditions

| Condition | Pattern | Score |
|-----------|---------|-------|
| Multiple study items | 3+ `[!disco]` on same subject | +3 |
| Ongoing work | 3+ `[!facio]` suggesting continued effort | +2 |
| Explicit mention | User writes "project" or "deep dive" | +2 |
| Scope expansion | Synthesis reveals multi-topic, weeks-long scope | +1 |

**Suggest elevatio when score >= 3.**

---

## Suggestion Format

```markdown
> [!question] New Project Suggestion
>
> **Detected Pattern**: {N} study items on {topic}, {N} action items
> **Suggested Project**: {name} ({Learning/Development/Research/Creative})
>
> Create new project via elevatio?
> - [ ] Yes, create project
> - [ ] No, keep as synthesis only
```

**Project type mapping**: disco-dominant → Learning, facio → Development, quaero → Research, cogito → Creative.

---

## After Elevatio

1. Call elevatio with project name, type, source
2. Move synthesis to `{new project}/refined/`
3. Update frontmatter: `project: "50x. New-Project"`, `promoted_to_project: true`

---

## Skip Conditions

Do NOT suggest if: already in project, only 1-2 callouts, single quaero without depth, or already suggested this session.

---

## User Override

User can reject suggestion, request elevatio without trigger, or specify different project type.
