# Tool Selection Guide

Decision tree for selecting the right tool based on source type.

---

## Tool Inventory

| Tool | Priority | Purpose |
|------|----------|---------|
| Brave Search | **1순위** | Web search — discovery, timeline, context (Pro: 50/sec, unlimited) |
| WebSearch | **2순위** | Fallback on Brave 429 |
| WebFetch | **보조** | Single known URL deep-read (multi-pass) |
| Playwright | - | Dynamic/SPA, login-required |
| Read | - | Local files, images |
| paper-search | - | Academic papers |
| skill-seekers | - | Large-scale scraping |
| Obsidian MCP | - | Vault notes |
| Gemini CLI `/indagatio` | - | YouTube video analysis (user runs manually) |

**Brave Pro extras**: Infobox, FAQ, Discussions, Schema data. Sources section mandatory after search.

### Research Workflow Pattern

**Brave-first, WebFetch-assist**:
1. **Brave Search** for discovery — what exists, version timelines, community context. Snippets are direct source quotes = low distortion. Sufficient for most research.
2. **WebFetch** only when deep-reading a specific known URL (e.g., official docs page for exact config syntax). Requires exact URL — guessed URLs often 404. Content processed by a small AI model = may over-summarize or miss nuances.

**Anti-pattern**: Starting with WebFetch by guessing URLs → 404 failures → wasted calls. Always discover URLs via Brave first.

---

## Default Tool by Source

| Source | Tool |
|--------|------|
| GitHub | skill-seekers scrape_github |
| Web (multi-page) | skill-seekers scrape_docs |
| Web (single) | WebFetch (MULTI-PASS) |
| Web (dynamic/SPA) | Playwright MCP |
| PDF | skill-seekers scrape_pdf |
| Paper | paper-search |
| HuggingFace | WebFetch |
| Image | Read (Vision) |
| Note | Obsidian MCP |
| YouTube | Gemini CLI `/indagatio` (user manual) |
| Topic Search | Brave → WebSearch |

---

## WebFetch - MULTI-PASS STRATEGY

**CRITICAL**: WebFetch over-summarizes and does NOT return actual image URLs.

**Pass 1 - Structure + Links**: `"Extract complete structure: all headings with descriptions, word count per section, content types (diagrams/code/tables/images), embedded YouTube URLs, main thesis. Also extract ALL inline hyperlinks as a LINK INVENTORY at the end: for each link, provide [anchor text](full URL) and which section it appears in. Prioritize same-domain links and documentation cross-references over external/navigation links. Do NOT summarize content."`

**Pass 1b - Related Link Crawl** (Medium/Large only):

Follow the source page's own cross-references to build richer analysis context.

**This step is MANDATORY for Medium and Large scales. Do not skip to Pass 2 without completing Pass 1b. The link crawl produces the `관련 페이지` section and sets `links_crawled` in frontmatter — both are required output fields.**

1. **Parse link inventory** from Pass 1 output
2. **Filter** (exclude → score → select):
   - EXCLUDE: anchor-only (`#`), auth (`/login`, `/signup`), assets (`.css`/`.js`/`.png`), nav (`/home`, `/about`, `/contact`, `/pricing`, blog index), already-visited URLs
   - SCORE: same-domain + shared path prefix = 3, same-domain other = 2, relevant anchor text + external = 1
   - SELECT: top N by score (Medium: max 3, Large: max 6)
3. **Fetch each** selected link (single WebFetch call per link):
   Prompt: `"Extract key information from this page relevant to {main_page_title}. Focus on: definitions, specifications, configuration options, API details, examples, and constraints. Be comprehensive. Do NOT summarize — preserve detail."`
4. **Compile** linked content as supplementary context for Pass 2
5. **Track**: maintain visited-URL set (normalize: strip trailing `/`, strip `#fragment`, resolve relative paths)

**Depth limit**: 1 hop only. Never follow links found on linked pages.
**Budget guard**: If main page >8000 words, reduce max links by 2.
**Skip**: Small scale, `--no-crawl`, non-web source, 0 links in inventory, all links filtered out.

### Inline vs Separate Note (Atomicity Gate)

After fetching a linked page, decide how to integrate:

**Inline into main note** when:
- Content is tightly coupled (config details, API options, parameter reference)
- Content is small (<1500 words) and has no independent identity
- Content only makes sense in context of the main topic

**Create separate quick note + `[[wikilink]]`** when ANY of:
- Content represents a **distinct concept** with independent meaning AND is substantial (1500+ words or 5+ sections)
- Content has a clearly **different scope/purpose** than the main page (e.g., "how to use" vs "how to build")
- Content would be useful as **standalone reference** from other notes (e.g., API reference, configuration guide)

When in doubt, prefer separate note — it's easier to merge later than to split.

**Separate note workflow**:
1. Generate as standard indagatio quick note (same frontmatter format, `source_type: web`)
2. Add `parent_analysis: "[[Main Note Title]]"` to frontmatter
3. In the main note, replace inline content with: `→ 별도 노트로 분리: [[Separate Note Title]] — {1-line relevance}`
4. In the main note's `관련 페이지` table, mark Relevance as `별도 노트 생성`
5. Increment `links_crawled` count for both inline and separate

**Examples**:
- Channels docs → `channels-reference` (빌드 가이드, 1500+ words, distinct scope) → **별도 노트**
- Channels docs → `#enterprise-controls` (same-page anchor, config detail) → **인라인**
- Channels docs → `remote-control` (독립 기능, 자체 개념) → **별도 노트**
- Channels docs → `permissions#permission-modes` (보조 참조) → **인라인**

**Fallback**: If Pass 1 AI model strips all links, run a dedicated link-extraction pass: `"List ONLY the URLs present as hyperlinks on this page, one per line, with their anchor text. Do not add any other text."`

---

**Pass 2 - Detail** (Medium/Large): `"For each section: key concepts with full explanations, all code examples verbatim, important quotes, examples/case studies, metrics. Incorporate context from related linked pages where relevant. Be comprehensive."`

**Pass 3 - Images** (if detected): `"List ALL image URLs: exact src URL (cdn.sanity.io, cloudfront, imgur patterns), alt text, description. Return ONLY actual URLs. Do NOT construct or guess."`

---

## Playwright MCP — Playwright Gate (STRICT)

**NEVER use Playwright as first choice for any web URL.** WebFetch is the default for ALL web pages.

### Playwright Gate: ALL conditions must be met

1. **WebFetch attempted first** — ran at least Pass 1 and got empty/broken/`<div id="root"></div>`-only content
2. **Confirmed JS-only rendering** — the page is a client-side SPA with no server-side rendering
3. **Not a documentation site** — docs sites (Mintlify, Docusaurus, GitBook, ReadTheDocs, Nextra, VitePress, MkDocs, Starlight) are SSR and work with WebFetch
4. **Not a blog/news/article** — static content, always WebFetch

### When Playwright IS appropriate

- Login-required pages (behind auth wall)
- Reddit (must use `old.reddit.com`)
- Single-page apps confirmed to have no SSR (WebFetch returned empty body)
- Pages requiring interaction (click tabs, expand sections, infinite scroll)

### Workflow (only after gate passes)

`browser_navigate` → `browser_wait_for` → `browser_snapshot` → extract. For iframe video: check snapshot for iframe elements → extract YouTube URLs.

**Auth**: Navigate login → show browser → user logs in → continue.

| Aspect | WebFetch | Playwright |
|--------|----------|------------|
| Speed | ~34s | ~60-90s |
| JavaScript | No | Full |
| Login | No | Session persistence |
| Memory | None | Chromium process |
| Default | **YES** | NO (fallback only) |

---

## skill-seekers MCP

**Multi-page docs**: `generate_config(name, url, desc)` → `estimate_pages(config)` → `scrape_docs(config, scrape_only=true)` → Read output → Quick Note.

**GitHub**: `scrape_github(repo: "owner/repo", name: output_name, include_issues: true/false)`

**PDF**: `scrape_pdf(pdf_path: path, name: output_name)`

---

## paper-search MCP

`search_arxiv(query, maxResults)` | `download_paper(paperId, platform: "arxiv", savePath)`

---

## YouTube Analysis (Gemini CLI — User Manual Step)

**Why Gemini CLI**: Google owns YouTube → native URL support, actual video analysis. If you have a Gemini subscription → no extra cost. Gemini API from Claude Code = double billing + incomplete analysis. Calling `gemini` CLI from Claude Code Bash = Google ToS violation.

**Workflow**:
1. Claude Code detects YouTube URL in `/indagatio` input
2. Prompt user: "YouTube 영상은 Gemini CLI로 분석이 필요합니다. 별도 터미널에서 실행해주세요:"
3. Provide command: `gemini` → `/indagatio <YouTube URL>`
4. User saves Gemini output to file (e.g., `/tmp/gemini-analysis.md`)
5. User returns to Claude Code: `/indagatio /tmp/gemini-analysis.md`
6. indagatio parses structured output → converts to vault Quick Note format

**Gemini CLI Custom Command**: `~/.gemini/commands/indagatio.toml`
- Registered as `/indagatio` in Gemini CLI
- Accepts YouTube URL as argument: `/indagatio https://youtube.com/watch?v=...`
- Outputs structured analysis with sections: VIDEO_INFO, CORE_SUMMARY, TIMESTAMPS, SECTIONS, KEY_CONCEPTS, TOOLS_AND_SERVICES, KEY_INSIGHTS, COMMANDS_AND_CODE, QUOTES, ACTION_ITEMS, KEYWORDS, DIFFICULTY
- All sections map directly to indagatio Quick Note fields (see `Gemini-YouTube-Analyzer-Prompt.md` in vault)

**Parsing Gemini Output**: When indagatio receives a local MD file with `---BEGIN ANALYSIS---` marker:
- `source_type: gemini-analysis` (auto-detected)
- Parse each `## SECTION_NAME` block into corresponding Quick Note sections
- VIDEO_INFO → frontmatter + Source table
- CORE_SUMMARY → 핵심 요약
- KEY_CONCEPTS → 주요 개념 (무엇/왜/예시 already in correct structure)
- TIMESTAMPS → Embedded Videos section
- DIFFICULTY.level → frontmatter `difficulty:`
- KEYWORDS → frontmatter `tags`

**Fallback** (if user cannot run Gemini CLI):
- Brave Search + WebFetch multi-source cross-check (text-only, no timestamps)
- Warn user: "영상 직접 분석 없이 텍스트 기반으로만 생성합니다"

**News Prompt**: Per-topic analysis (name, content, tools, importance). Category org. Top 5 ranked.

---

## Embedded Media Extraction

### Image URL Problem

WebFetch returns AI-processed markdown → `<img src>` lost → Claude may HALLUCINATE URLs.

**Solution**: Pass 3 extraction → verify CDN patterns → placeholder if failed.

**Valid CDNs**: `cdn.sanity.io`, `cloudfront.net`, `amazonaws.com`, `githubusercontent.com`, `imgur.com`, site-specific.
**Suspicious**: long random hex, generic names, pattern-based constructions.

| Source | Method | Reason |
|--------|--------|--------|
| Official docs | External link | Low deletion risk |
| Personal/temp | Local to `92. Attachments/` | May be deleted |
| Failed | Placeholder + warning callout | Honest limitation |

**Local naming**: `{note-name}-fig{N}-{description}.{ext}`

### Embedded Videos

Detection: WebFetch → if fails → Playwright (iframe query). Extract YouTube → Gemini. Non-YouTube → "Direct viewing required: {URL}".

---

## Selection Flowchart

```
Input
  ├─ Keyword/Topic? → Brave Search
  ├─ Local file? → PDF: scrape_pdf | Image: Read | Other: Read
  ├─ URL?
  │   ├─ github.com → scrape_github
  │   ├─ arxiv.org → paper-search
  │   ├─ youtube.com → Gemini CLI /indagatio (user manual step)
  │   ├─ huggingface.co → WebFetch
  │   ├─ reddit.com → Playwright (old.reddit.com) — Reddit exception
  │   ├─ Multi-page → scrape_docs
  │   └─ Single page (DEFAULT PATH):
  │       1. WebFetch (3-pass) — ALWAYS try first
  │       2. Content empty/broken? → Check Playwright Gate
  │       3. Gate passes? → Playwright
  │       4. Gate fails? → Report limitation
  ├─ [[...]] → Obsidian MCP
  └─ arxiv:/DOI → paper-search
```

**IMPORTANT**: Steps 2-4 only reached if WebFetch genuinely fails. Most sites (docs, blogs, news) succeed at step 1.

---

## Fallback Chain

skill-seekers → WebFetch → Playwright → report error | paper-search → WebFetch | YouTube → Gemini CLI (user manual) → Brave+WebFetch (text-only fallback) | Obsidian → filesystem Read | Image extraction → placeholder
