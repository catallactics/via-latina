---
name: indagatio
description: "Use this skill whenever the user wants to ingest, analyze, or summarize an external source and convert it into quick notes for their Obsidian vault. Triggers on: any URL (GitHub repos, webpages, YouTube videos, arXiv papers, Reddit threads, HuggingFace), any file path (PDFs, images), vault folder paths for review, or the command '/indagatio'. Also triggers on Korean requests like '분석해줘', '리뷰해줘', '정리해줘', '요약해줘', '퀵 노트로 변환' when paired with an external source or link. Also triggers on English requests like 'analyze this repo', 'summarize this paper/article/video', 'extract key concepts from this'. The core intent is: user points at something outside the vault (or a vault folder to review) and wants structured quick notes created from it."
argument-hint: "<URL, path, or topic>"
context: fork
agent: general-purpose
---

# Indagatio

> *Indagatio* (Latin): investigation, tracking, inquiry

Skill for converting external sources and internal vault content into quick notes.

---

## Trigger Patterns

### Direct Invocation
- `/indagatio <URL or path>`
- `/indagatio --large <URL>` (force large scale)
- `/indagatio arxiv:2401.12345`

### Natural Language (Korean)
- "이 GitHub 분석해줘", "이 논문 리뷰해줘", "이 페이지 정리해줘"
- "이 영상 요약해줘", "이 PDF 분석해줘", "이 이미지 설명해줘"
- "이 프로젝트 리뷰해줘", "Arduino 프로젝트 분석해줘"

### Natural Language (English)
- "Analyze this repo", "Summarize this paper", "Review this article/project"

---

## Core Principles

1. **WebFetch-first for all web URLs**: ALWAYS try WebFetch before Playwright. Most sites (docs, blogs, news) are SSR and WebFetch gets full content. Playwright ONLY after WebFetch returns empty/broken content requiring JS rendering. See `references/tool-selection.md` Playwright Gate.
2. **Multi-pass extraction**: WebFetch over-summarizes; always use multiple passes
3. **Verification mandatory**: Never trust constructed URLs; validate before output
4. **Scale-appropriate depth**: Small/Medium/Large determines thoroughness
5. **Bilingual output**: Korean with English terms in parentheses
6. **Tool-specific handling**: Each source type has optimal tool selection
7. **Cross-source verification**: Every source needs complementary sources checked
8. **Scope expansion**: Proactively discover related topics user didn't think of
9. **Quality goal**: Produce HIGHER quality than manual curation (if output is narrower → scope expansion insufficient)
10. **Adaptive batching**: Dynamically batch large results to prevent hallucination

**Self-check**: Is this more comprehensive than manual? Did I discover unknown info? If not → redo Step 2b.

**Cross-source recommendations:**

| Source | Check Also |
|--------|-----------|
| GitHub | Official docs, blogs, videos |
| Official Docs | GitHub implementation, community |
| Blog/Personal | Official docs + GitHub (verify) |
| Paper | GitHub impl, announcements, follow-ups |
| Video | Official docs, GitHub, text guides |
| Community | Official source for original info |

---

## Source Types & Tools

| Source Type | Pattern | Tool |
|-------------|---------|------|
| **GitHub** | `github.com/user/repo` | skill-seekers github |
| **Web (multi-page)** | docs, guides | skill-seekers scrape |
| **Web (single page)** | news, blog, article, docs | WebFetch (multi-pass) — **DEFAULT for all web URLs** |
| **Web (dynamic/SPA)** | WebFetch returns empty/broken + confirmed JS-only | Playwright MCP — **ONLY after WebFetch fails** |
| **Reddit** | `reddit.com/*` | Playwright MCP (`old.reddit.com`) |
| **PDF** | `*.pdf` | skill-seekers pdf |
| **Paper** | `arxiv:`, DOI | paper-search MCP |
| **YouTube** | `youtube.com/watch?v=` | Gemini CLI `/indagatio` (user runs manually, then provides output) |
| **Image** | `*.png`, `*.jpg` | Read (Vision) |
| **HuggingFace** | `huggingface.co/...` | WebFetch |
| **Note** | `[[note name]]` | Obsidian MCP |
| **Vault Folder** | `[[50-NNNNNN. Project]]`, vault path | Obsidian MCP + Glob |
| **Topic Search** | keyword, concept | Brave Search → WebSearch (fallback) |

→ See `references/source-detection.md` for URL patterns.
→ See `references/tool-selection.md` for tool decision tree and prompts.
→ See `../_common/references/frontmatter-standard.md` for status taxonomy.

---

## Operation Mode

```
Memory Check → Source Detect → Scale → Fetch (multi-pass + link crawl) → Verify → Analyze → Quick Note → Log → Memory Save
```

indagatio ends after Quick Note creation. Daily Note logging handled by purgatio.

---

## Core Workflow

### Step 0: Memory Check

Check claude-mem for cached topic info before fetching externally.

```
mcp__plugin_claude-mem_mcp-search__search: query: "{topic}", limit: 5
```

If found → ask user: "캐시된 정보가 있습니다 (검증일: YYYY-MM-DD). 새로 분석할까요?"
If not found → proceed to Step 1.

---

### Step 1: Source Detection

→ See `references/source-detection.md` for regex patterns and detection order.

**1a. Multi-source**: Same topic → merge; different topics → separate notes; independent → parallel.

**1b. Detection order**: file extension → URL patterns → special prefixes → Obsidian link → vault path → general URL → topic search.

**1c. Duplicate Check**: Search `00. INBOX/01. Quick Notes/Indagatio-Log.md` for URL. If found → ask user.

**1d. MCP & Dependency Check**:
- skill-seekers not connected → notify user with install command, offer WebFetch fallback
- multi-model-ai not available for YouTube → notify user, skip video analysis

**1e. Existing Note Check**:
1. Search vault: `Glob: **/*{topic}*.md` + `Grep: {topic}` in vault path
2. If found → present options: A) New note (with comparison after), B) Enhance existing, C) Cancel
3. If A → provide comparison after completion (sources count, scope, unique info, merge recommendation)

### Step 2: Scale Assessment

→ See `references/scale-algorithm.md` for decision tree.

| Scale | Single Page | Analysis Depth |
|-------|-------------|----------------|
| **Small** | <1,500 words, <5 sections | Quick summary |
| **Medium** | 1,500-5,000 words, 5-10 sections | Structured overview |
| **Large** | 5,000+ words, 10+ sections | Comprehensive breakdown |

**User Override**: `--small`, `--medium`, `--large`, `--no-crawl` (skip link crawl), `--no-expand` (skip scope expansion).

### Step 2b: Scope Expansion (MANDATORY)

**After assessing primary source, ALWAYS search for related topics:**

1. Brave Search (1순위, Pro Plan: 50/sec parallel, unlimited):
   - General: `"{topic} guide tutorial"` count: 15
   - Community: `"{topic} site:reddit.com OR site:news.ycombinator.com"` count: 10
   - FAQ: `"{topic} FAQ troubleshooting"` count: 10
   - News (if relevant): `"{topic} 2025 2026 update release"` count: 10
   - 429 error → WebSearch fallback

2. Present scope expansion suggestion to user with discovered subtopics

3. If confirmed → fetch additional sources per subtopic

**Reddit note**: Use `old.reddit.com` (www blocks Playwright).

**Skip**: `--no-expand`, highly specific single API endpoint, explicit scope limit.

### Step 2c: Bulk Result Processing

When Topic Search / Scope Expansion returns 10+ results → hierarchical processing to prevent hallucination.

**Skip when**: <10 items, single source, `--no-batch`, total tokens < 30K.

→ See `references/hierarchical-summarization.md` for batch calculation, hierarchy, and processing rules.

### Step 3: Content Fetching (MULTI-PASS — MANDATORY ALL PASSES)

**Run ALL applicable passes in sequence. Never skip a pass. Pass 1b is NOT optional for Medium/Large.**

**3a. Pass 1 — Structure + Links** (all scales):
WebFetch with structure+links extraction prompt. Extract headings, word counts, content types, and ALL inline hyperlinks as a LINK INVENTORY.

**3b. Pass 1b — Related Link Crawl (MANDATORY for Medium/Large)**:
**Never skip when scale ≥ Medium. This step is required.**
1. Parse link inventory from Pass 1 output
2. Filter: exclude anchors (#), nav, auth, assets → score (same-domain+shared path=3, same-domain=2, external=1) → select top N (Medium: max 3, Large: max 6)
3. Fetch each selected link with single-pass WebFetch
4. Apply Atomicity Gate: distinct concept (1500+ words, independent meaning, different scope) → separate quick note + `[[wikilink]]`; supporting detail → inline
5. Set `links_crawled` in frontmatter to actual count fetched

This step takes ~34s per link (~100-200s total). No excuse to skip.
**Skip ONLY when**: Small scale, `--no-crawl` flag, non-web source, or 0 links in inventory.

→ See `references/tool-selection.md` Pass 1b for full procedure, filtering rules, and Atomicity Gate criteria.

**3c. Pass 2 — Detail** (Medium/Large):
WebFetch with detail extraction prompt. Incorporate context from Pass 1b linked pages.

**3d. Pass 3 — Images** (if images detected in Pass 1):
WebFetch with image URL extraction prompt. Verify CDN patterns.

**Tool selection order for web URLs**:
1. **WebFetch (DEFAULT)** — use for ALL web URLs including docs, blogs, news, articles
2. **Playwright (FALLBACK ONLY)** — ONLY when WebFetch returns empty/broken content. See `references/tool-selection.md` Playwright Gate.
3. Doc frameworks (Mintlify, Docusaurus, GitBook, Nextra, VitePress, MkDocs) are SSR → WebFetch always works

**Key rules**:
- WebFetch does NOT return actual image URLs → use explicit extraction prompt (Pass 3d)
- If image URLs not extractable → placeholder `![Figure N: Description](URL_NOT_AVAILABLE)`
- YouTube: Prompt user to run Gemini CLI `/indagatio <URL>` manually → provide output file → parse
- NEVER call `gemini` CLI from Claude Code Bash (Google ToS violation)
- Reddit: always `old.reddit.com` (exception: Playwright allowed for Reddit)

→ See `references/tool-selection.md` for all prompts, code examples, and fallback workflows.

### Step 4: Verification (MANDATORY)

**4a. Image URL Verification**: Check URL pattern validity (CDN patterns OK, hex strings you generated → INVALID). Invalid → placeholder + warning.

**4b. Content Completeness**:

| Scale | Minimum |
|-------|---------|
| Small | 500+ words, 3+ concepts |
| Medium | 1,500+ words, 6+ concepts |
| Large | 3,000+ words, 10+ concepts |

If thin → re-fetch with more specific prompts.

**4c. Language Balance**: Technical terms need English in parentheses. Concepts explained, not just listed.

**4d. Comment Insights** (Reddit, HN, forums only):

Filter comments by quality (technical detail, specific data, corrections to OP → High; practical tips → Medium; opinions without evidence → Low).

**Cross-verify every extracted comment** via Brave Search/official docs:
- ✅ Verified (2+ sources), ⚠️ Partial (1 source), ❌ Unverified, 🚫 Debunked

→ See `references/quicknote-format.md` for 댓글 인사이트 output section format.

**Skip**: Non-community sources, <5 comments, no High quality comments, `--no-comments`.

**4e. Difficulty Assessment**:

Assess content difficulty (1-10) based on source_type × scale baseline:

| source_type | small | medium | large |
|-------------|-------|--------|-------|
| youtube     | 2     | 3      | 4     |
| web         | 3     | 4      | 5     |
| github      | 4     | 6      | 8     |
| paper       | 6     | 7      | 9     |
| pdf         | 4     | 5      | 7     |
| huggingface | 5     | 7      | 8     |
| topic-search| 3     | 5      | 6     |
| image       | 2     | 3      | 4     |
| note        | 3     | 4      | 5     |

Content complexity adjustment: +1 if multi-domain, +1 if requires prior knowledge, -1 if introductory. Clamp to 1-10.

Add `difficulty: {value}` to quick note frontmatter after `scale:`.

### Step 5: Content Analysis & Writing

**Bilingual Term Rule**: `한국어 용어 (English Term)` — all technical terms, proper nouns, abbreviations, tool names.

**Depth**: Small → bullets OK; Medium → 2-3 sentences/concept; Large → full paragraph with examples.

**AI Processing**: Structure → Key concepts (full explanations) → Why it matters → Practical examples → Action items → Vault connections.

### Step 6: Quick Note Generation

**CRITICAL: Timestamp Acquisition** — NEVER guess. Always run before creating file:
```bash
date "+%Y.%m.%d-%Hh%Mm"  # Filename
date "+%Y-%m-%d %H:%M"   # Frontmatter
```

**Output Path**: `00. INBOX/01. Quick Notes/notes/YYYY.MM.DD-HHhMMm {Source Title} Analysis.md`

→ See `references/quicknote-format.md` for full template.

### Step 7: Log Recording

Append to `00. INBOX/01. Quick Notes/Indagatio-Log.md`:

```markdown
| YYYY-MM-DD | HH:mm | <source_url> | <source_type> | <scale> | <image_status> | [[Quick Note Title]] |
```

### Step 8: Memory Storage

Save verified information to claude-mem after Quick Note creation:
```
mcp__plugin_claude-mem_mcp-search__save_memory:
  title: "{topic}", text: "핵심 정보...\n출처: {URL}\n검증일: YYYY-MM-DD"
```

**Skip**: Vault folder review, image-only analysis, failed fetches.

---

### Step 9: Completion Verification

Before reporting completion, verify:

1. Quick note file created at expected path (file must exist)
2. `links_crawled` > 0 in frontmatter (Medium/Large web source)
3. `관련 페이지` section present with populated table (Medium/Large)
4. Atomicity Gate decisions made for each crawled link
5. Log entry appended to Indagatio-Log.md
6. Memory saved to claude-mem (if applicable)

**If any verification fails → fix before reporting completion.**

---

## User Confirmation Points

1. Duplicate URL found
2. Scale override needed
3. MCP fallback
4. Output location (project-related → suggest project folder)
5. Image extraction failed

---

## Quality Checklist

- [ ] Images: CDN URLs (not hallucinated)
- [ ] Content depth meets scale minimum
- [ ] Korean + English terms
- [ ] Concepts explained, not listed
- [ ] Cross-source: complementary sources suggested
- [ ] Comment insights cross-verified (community sources)
- [ ] Scope expanded: discovered related topics
- [ ] Related links: key cross-references followed (Medium/Large)
- [ ] Better than manual curation

**If unchecked → back to Step 2b.**

---

## Vault Folder Review

Special workflow for reviewing internal vault folders (projects, system guides, etc.).

→ See `references/vault-folder-review.md` for full template and analysis guidelines.

**Key differences from external**: Data from internal vault; focus on status + progress (not content extraction); output includes expansion suggestions based on external resources.

**Expansion suggestions**: Context-dependent analysis of current level, project purpose, tech stack, trends → recommend next-level resources, related tools, practical applications, communities.

---

## Integration

- **Inbound**: User direct call via `/indagatio` or natural language
- **Outbound**: Quick Notes processed by purgatio; URLs logged in Indagatio-Log.md

---

## Reference Files

- `references/source-detection.md` - URL pattern matching
- `references/scale-algorithm.md` - Scale determination
- `references/quicknote-format.md` - Output template
- `references/tool-selection.md` - Tool decision tree and prompts
- `references/vault-folder-review.md` - Vault folder review template
- `references/hierarchical-summarization.md` - Bulk result processing
- `references/reddit-guide.md` - Reddit navigation
- `../_common/references/logging-standard.md` - Unified logging

---

## Output Format Rules

**SKILL.md content**: English (context efficiency)
**User-facing output**: Korean (user preference)
**Trigger patterns**: English + Korean (accessibility)
