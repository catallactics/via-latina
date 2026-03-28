# Notes Numbering Convention

> Numbering system for files in project `notes/` folder

---

## Two Numbering Systems

| Prefix | Purpose | Expansion | Example |
|--------|---------|-----------|---------|
| `p10.` | Phase-based learning (expandable) | p11, p12, p13... | `p10. Setup.md` |
| `n01.` | Reference notes (sequential) | Renumber or n03a | `n01. Reference-Links.md` |

**When to use which:**
- **p-series**: Learning curriculum with phases that may have sub-topics
- **n-series**: Standalone reference materials, guides, cheatsheets

**IMPORTANT**: `p` already means "Phase" - do NOT include "Phase" in filename!
- ✅ `p10. Setup.md`
- ❌ `p10. Phase-1-Setup.md` (redundant)

**Mixed usage**: Both can coexist in same `notes/` folder:
```
notes/
├── p10. Setup.md               ← Phase 1 main
├── p11. Installation.md        ← Phase 1 sub-topic
├── p20. Basics.md              ← Phase 2 main
├── n01. Reference-Links.md     ← Reference material
├── n02. CheatSheet.md          ← Reference material
└── n03. Shortcuts.md           ← Reference material
```

---

## p-Series Format (Phase-Based Learning)

```
p10. Topic.md              ← Phase 1 main
p11. SubTopicA.md          ← Phase 1 sub-topic
p12. SubTopicB.md          ← Phase 1 sub-topic
p20. AnotherTopic.md       ← Phase 2 main
p21. SubTopic.md           ← Phase 2 sub-topic
p30. FinalTopic.md         ← Phase 3 main
```

**Pattern**: `p` + 2-digit number + `.` + space + topic name (NO "Phase" in name!)

**Why p10, p20 instead of p01, p02:**
- Leaves room for sub-topics (p11, p12, p13... up to p19)
- No renumbering needed when adding sub-phases
- Maximum 9 sub-topics per phase (usually sufficient)

**Progressive complexity note:**
For learning projects, phase numbers should generally increase with complexity — but there is no fixed number-to-complexity mapping. The principle is "monotonically increasing difficulty," not "p10 must be basic." Reference material useful across multiple phases may be better as n-series notes. Different projects have different phase counts and structures.

---

## n-Series Format (Reference Notes)

```
n01. File-Name.md
n02. Another-File.md
...
n99. Last-File.md
```

**Pattern**: `n` + 2-digit number + `.` + space + filename

---

## README Link Format

**Hide numbers in alias**: Wikilink alias should NOT include n## prefix

```markdown
✅ Correct:
[[notes/n01. Arduino Introduction|Arduino Introduction]]

❌ Wrong:
[[notes/n01. Arduino Introduction|n01. Arduino Introduction]]
```

**Reason**: Numbers are for Finder/Obsidian sorting only, not for display

---

## Numbering Rules

- **Curriculum order** (primary): follow roadmap sequence, references last
- **Creation order** (secondary): if no curriculum exists
- **Insertion**: renumber (recommended) or use letter suffix (n03a, temporary)

---

Type-specific examples: see each `*-project.md` reference file.

---

## refined/ Folder

`refined/` uses `r##.` numbering:
- Personal writings, refined thoughts from purgatio
- Pattern: `r` + 2-digit number + `.` + space + title
- Per-project numbering (each project starts from r01)

**Examples:**
```
refined/
├── r01. Arduino Setup Guide.md
├── r02. LED Experiment Results.md
└── r03. Weekly Retrospective.md
```

**Comparison:**
| Folder | Nature | Numbering |
|--------|--------|-----------|
| `notes/` | Materials to READ (ordered) | p10. (phases) or n01. (refs) |
| `refined/` | My WRITINGS from purgatio | r01., r02., r03... |

