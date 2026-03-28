# Reddit Navigation & Usage Guide

> Quick reference for Reddit navigation patterns relevant to indagatio.

---

## Core Concepts

- **Subreddit**: `/r/name` — topic-specific community (e.g., `/r/LocalLLaMA`, `/r/ClaudeAI`)
- **User**: `/u/name` — `OP` = Original Poster, `[deleted]` = deleted user

---

## URL Patterns

| Type | Pattern |
|------|---------|
| Post | `reddit.com/r/{sub}/comments/{id}/{title}/` |
| Comment | `reddit.com/r/{sub}/comments/{id}/{title}/{comment_id}/` |
| JSON API | append `.json` |
| Classic UI | `old.reddit.com` (Playwright-friendly) |
| Sort | `?sort=top&t=all` |

---

## Search Syntax

**Google**: `site:reddit.com {query}`

**Reddit native**: `author:username`, `subreddit:name`, `flair:tag`, `selftext:word`, `title:word`, `site:example.com`

---

## Indagatio Integration

### CRITICAL: Use `old.reddit.com`

New Reddit has aggressive bot detection. Always use `old.reddit.com` for Playwright access.

### Workflow

1. Brave Search: `site:reddit.com {query}`
2. Identify high-score, recent discussions
3. Playwright with `old.reddit.com` URLs
4. Extract: focus on top comments (highest value)

### Extraction Priority

| Priority | Content |
|----------|---------|
| High | Top-voted answers, OP's question/context |
| Medium | Controversial opinions, expert flairs |
| Low | Award counts, karma |
