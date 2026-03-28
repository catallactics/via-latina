# Latin Callouts

## Overview

Via Latina extends Obsidian's callout system with 6 Latin-named callout types. These serve as **workflow control markers** — when `purgatio` processes notes, it detects these callouts and routes them to `commentatio` for synthesis.

## The Six Callouts

### [!quaero] — I Ask
**Purpose:** Mark questions that need thorough research.

```markdown
> [!quaero] What is the difference between RAG and fine-tuning?
```

**What happens:** `commentatio` triggers a WebSearch and produces an answer with cited sources. Always searches — the user explicitly marked this as needing a researched answer.

---

### [!emendo] — I Improve
**Purpose:** Suggest specific improvements to content.

```markdown
> [!emendo] Add a comparison table for pricing tiers
```

**What happens:** `commentatio` applies the suggested edit directly to the target section.

---

### [!disco] — I Learn
**Purpose:** Mark topics for deeper study.

```markdown
> [!disco] How does WebSocket multiplexing work in HTTP/2?
```

**What happens:** `commentatio` creates a learning roadmap with keywords and resources. If 3+ `[!disco]` callouts appear on the same subject, `elevatio` is suggested to formalize it as a learning project.

---

### [!facio] — I Do
**Purpose:** Flag action items.

```markdown
> [!facio] Contact the agency for partnership pricing
```

**What happens:** Converted to a task item (`- [ ]`) in the project's action section.

---

### [!moneo] — I Remind
**Purpose:** Highlight important connections.

```markdown
> [!moneo] This mirrors the non-destructive editing pattern from DAWs
```

**What happens:** `commentatio` surfaces vault connections — finds related notes and adds wikilinks.

---

### [!cogito] — I Think
**Purpose:** Free-form musings and reflections.

```markdown
> [!cogito] What if we applied the Zettelkasten principle to API design?
```

**What happens:** `commentatio` acknowledges the thought and expands on it freely, adding related ideas and perspectives.

## Styling

Copy `extras/obsidian-css/latin-callouts.css` to your vault's `.obsidian/snippets/` folder and enable it in Settings → Appearance → CSS Snippets.

Each callout type gets a distinct color:
- **quaero** — Blue (questions)
- **emendo** — Amber (improvements)
- **disco** — Green (learning)
- **facio** — Pink (actions)
- **moneo** — Purple (important)
- **cogito** — Cyan (thinking)
