# Merge Examples

Merge scenarios and examples.

---

## Scenario 1: Category-Based Merge

```
50-000004. Platform-Course-Notes      (specific platform)
50-000012. Learning-Roadmap   (entire AI learning)
```

**User decision**: 50q is main (broader category)

**Result**:
- 50q: Stays in Active, absorbs 50g content
- 50g: Moves to Archive (original preserved)

---

## Scenario 2: Content Volume-Based Merge

```
50-NNNNNN. Side-Project      (3 files)
50-NNNNNN. Main-Project      (20 files)
```

**User decision**: 50f is main (more content)

**Result**:
- 50f: Stays in Active
- 50d: Moves to Archive

---

## Scenario 3: Future Direction-Based Merge

```
50-NNNNNN. Old-Approach      (previous approach)
50-NNNNNN. New-Direction     (new direction)
```

**User decision**: 50m is main (going forward with this direction)

**Result**:
- 50m: Stays in Active, integrates only useful content from 50a
- 50a: Moves to Archive

---

## README Integration Example

### Before (50g README)

```markdown
# Platform-Course-Notes

## Progress
- [x] Online Course Module 1 completed
- [ ] Advanced Topics in progress

## Resources
- Online learning platform
```

### Before (50q README)

```markdown
# Learning-Roadmap

## Progress
- [x] Roadmap created
- [ ] Math basics

## Resources
- Learning plan document
```

### After (50q README - integrated)

```markdown
# Learning-Roadmap

## Progress
- [x] Roadmap created
- [x] Online Course Module 1 completed (from 50-000004)
- [ ] Math basics
- [ ] Advanced Topics in progress (from 50-000004)

## Resources
- Learning plan document
- Online learning platform (from 50-000004)

## Merge History

| Date | Absorbed | Reason |
|------|----------|--------|
| 2025-11-02 | 50-000004. Platform-Course-Notes | Platform-specific learning consolidated into overall roadmap |
```

---

## Registry Record Example

```markdown
## Archived Projects

| Number | Name | Archived | Reason | Merged Into |
|--------|------|----------|--------|-------------|
| 50-000004 | Platform-Course-Notes | 2025-11-02 | merged | 50-000012 |
```


File collision handling and number reuse rules: see SKILL.md Step 3 and Core Principle 4.
