# Scale Assessment Algorithm

Determine processing scale for analysis depth (not tool selection).

---

## Scale Definitions

| Scale | Multi-page | Single-page | Time | Link Crawl |
|-------|-----------|-------------|------|------------|
| **Small** | <10 pages | <1,500 words, <5 sections | <30s | None |
| **Medium** | 10-100 pages | 1,500-5,000 words, 5-10 sections | 1-5min | Max 3 links |
| **Large** | 100+ pages | 5,000+ words, 10+ sections | 5+min | Max 6 links |

**Link Crawl Budget**: Adds ~34s per link. Budget guard: if main page >8000 words, reduce max by 2. Skip: `--no-crawl`, non-web source.

---

## Decision Tree

```
Input
  ├─ GitHub? → README only: Small | 1 repo: Medium | Multiple repos: Large
  ├─ PDF? → <50 pages: Small | 50+: Medium
  ├─ Paper? → Single: Small | Multiple: Medium
  ├─ YouTube/Image/Note/HuggingFace? → Always Small
  ├─ Topic Search? → Always Small
  └─ Web?
      ├─ Multi-page → estimate_pages: <50: Medium | 50+: Large
      └─ Single page → by content length (sections + word count)
```

---

## User Override

| Flag | Effect |
|------|--------|
| `--small` / "Just briefly" | Quick summary |
| `--medium` | Structured overview |
| `--large` / "Full document" | Comprehensive analysis |

---

## Analysis Depth by Scale

| Scale | Output |
|-------|--------|
| Small | Quick summary (1-2 paragraphs), bullets OK |
| Medium | Structured overview, 2-3 sentences/concept |
| Large | Comprehensive breakdown, full paragraphs with examples |

Tool selection → See `tool-selection.md`
