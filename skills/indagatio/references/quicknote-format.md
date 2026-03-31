# Quick Note Format Reference

Output format for Indagatio-generated quick notes.

---

## Filename & Path

```
00. INBOX/01. Quick Notes/notes/YYYY.MM.DD-HHhMMm {Source Title} Analysis.md
```

---

## Frontmatter

```yaml
---
created: YYYY-MM-DD HH:mm
type: indagatio
source_type: github|web|paper|pdf|image|note|youtube|huggingface|topic-search
source_url: <original URL or path>
scale: small|medium|large
difficulty: 1-10
tool_used: <tool name>
has_images: true|false
has_videos: true|false
image_status: verified|unverified|manual_required
links_crawled: 0
tags: [indagatio, <source-type>, <topic-tags>]
status: inbox
---
```

---

## Body Structure (Full)

```markdown
# {Source Title} Analysis

## Source
| Field | Value |
|-------|-------|
| URL | <source URL> |
| Type | <source type> |
| Scale | <small/medium/large> |
| Tool | <tool used> |
| Fetched | YYYY-MM-DD HH:mm |

---

## 핵심 요약 (Core Summary)
{2-3 paragraphs: what this is and why it matters}

## 주요 개념 (Key Concepts)
### Concept 1 (English Name)
**무엇인가**: {full explanation}
**왜 중요한가**: {why it matters}
**예시**: {practical example}

## 구조 개요 (Structure Overview)
{For larger sources: section breakdown}

## 핵심 포인트 (Notable Points)
- Point with explanation

## Embedded Media
### Figures
![Description|600](verified-cdn-url)
*Figure N: Description*

### Embedded Videos
- URL: {youtube_url}
- Duration: {duration}
**Summary**: {Gemini analysis}
**Timestamps**: 00:00 - Topic

## Action Items
- [ ] Follow-up tasks

## 관련 페이지 (Related Pages Crawled)

> Omit entire section if links_crawled: 0 (Small scale or --no-crawl).
> For **separate notes** created by Atomicity Gate: add `parent_analysis: "[[Main Note Title]]"` to that note's frontmatter only. Main notes do NOT include parent_analysis.

| Page | URL | Relevance |
|------|-----|-----------|
| {Page Title} | {URL} | {1-line what this added to the analysis} |

## Vault Connections
- [[Related Note]] - {relevance}

> [!tip] 잠재적 연결 (Potential Connections)
> - [[Loosely Related Note]] - {why it might be relevant}

**Connection rules:**
- **Vault Connections**: Only CONFIRMED connections — same tool, same project, explicit mention, direct dependency
- **잠재적 연결 callout**: Loose/abstract connections (e.g. "both use AI generation") go here, NOT in main Vault Connections
- **When unsure**: Put it in 잠재적 연결, or omit entirely. False connections are worse than missing ones.
- **NEVER force-connect** to active projects (Octa, SNS Pipeline, etc.) without explicit relevance

## Raw Notes
> Key excerpts, code snippets, data points

## Purgatio
Refined: (to be filled by purgatio)
```

---

## Minimal Template (Small Scale)

Same frontmatter. Body: `# Analysis` → `## Source` (URL + date) → `## Summary` → `## Key Points` → `## Action Items` → `## Purgatio`

---

## Comment Insights Section (Community Sources Only)

Add after 핵심 포인트 for Reddit/HN/forums:

```markdown
## 댓글 인사이트 (Comment Insights - Cross-Verified)

### ✅ 검증 완료 (Verified)
**{insight}** (u/{author}, {score}점)
> "{quote}"
- 검증: {source} | 의미: {why}

### ⚠️ 부분 검증 (Partially Verified)
**{insight}** (u/{author}, {score}점)
> "{quote}"
- 근거: {confirmed} | 미확인: {unconfirmed}
```

---

## Source Type Specific Additions

| Source Type | Extra Sections |
|-------------|---------------|
| **Web (long)** | Article Structure (sections, word count, reading time) + Section Breakdown |
| **GitHub** | Repository Info (stars, language, license, last updated) + File Structure |
| **Paper** | Citation + Abstract + Key Contributions |
| **YouTube** | Video Info (channel, duration, published) + Transcript Summary + Timestamps |
