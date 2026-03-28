# Source Detection Reference

URL and path pattern matching for source type identification.

---

## Detection Order

1. File extension check
2. URL domain patterns
3. Special prefixes
4. Vault path check
5. Obsidian link format
6. URL check (general web)
7. Topic/Keyword search (fallback)
8. Default: unknown

---

## Pattern Definitions

### GitHub
```regex
^https?://(www\.)?github\.com/[^/]+/[^/]+
```
- `github.com/user/repo`, `github.com/user/repo/blob/...`, `github.com/user/repo/tree/...`

### arXiv
```regex
^https?://arxiv\.org/(abs|pdf)/\d{4}\.\d{4,5}
^arxiv:\d{4}\.\d{4,5}
```

### DOI
```regex
^10\.\d{4,}/[^\s]+
^https?://(dx\.)?doi\.org/10\.\d{4,}/
```

### HuggingFace
```regex
^https?://huggingface\.co/[^/]+(/[^/]+)?
```
- Models, datasets, spaces

### YouTube
```regex
^https?://(www\.)?(youtube\.com/(watch\?v=|shorts/|live/|embed/)|youtu\.be/|youtube-nocookie\.com/embed/)[\w-]+
```
- `youtube-nocookie.com` used in iframe embeds (WebFetch may miss — use Playwright if needed)

### PDF (Local)
```regex
\.(pdf)$
```

### Image (Local)
```regex
\.(png|jpg|jpeg|gif|webp|svg|bmp)$
```

### Obsidian Note
```regex
^\[\[.+\]\]$
```
- `[[Note Name]]`, `[[folder/Note Name]]`

### Vault Folder
```regex
^\[\[\d+[a-z]?\. .+\]\]$  (project pattern)
^(\d+\. [A-Z]+|50\. PROJECTS|90\. SYSTEM)/  (vault path)
```
- `[[50-000009. Beginner-Guide]]` → folder; ends with `.md` → note; else Glob to check

### Topic Search
```
No URL, no file extension, no Obsidian link — a phrase or keyword (2+ words or >3 chars)
```
- `Claude Code best practices`, `React hooks 2026`, `MCP server development`

---

Tool selection → See `tool-selection.md`
