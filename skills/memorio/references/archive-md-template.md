# ARCHIVE.md Template

Template for archive documentation file.

## File Location

```
50. PROJECTS/54. Archive/50x. Project-Name/
├── ARCHIVE.md    ← This file
├── README.md     ← Preserved original
└── ...
```

## Template

```markdown
---
archived: YYYY-MM-DD
reason: [completed|discontinued|on_hold|merged|superseded]
---

# 📦 Archive: [Project Name]

## 아카이브 이유

> **사용자**: [User's exact words - quote verbatim]

## 검증

[Research summary - what was found]

### 검증 결과

[One of:]
- ✅ **검증됨**: [Evidence summary]
- ⚠️ **부분 검증**: [What's confirmed, what's uncertain]
- 🔄 **뉘앙스**: [User's view valid with caveats]
- ❌ **반박됨**: [Contradicting evidence - user confirmed proceed]

### 출처

- [Source title](URL)
- [Source title](URL)

## 교훈

[Optional - lessons learned from this project]
[User may provide, or skip]
```

## Field Descriptions

| Field | Required | Description |
|-------|----------|-------------|
| `archived` | Yes | Date in YYYY-MM-DD format |
| `reason` | Yes | One of: completed, discontinued, on_hold, merged, superseded |
| 아카이브 이유 | Yes | User's exact words, quoted |
| 검증 | Yes | Research findings |
| 검증 결과 | Yes | Validation status with evidence |
| 출처 | Yes | URLs used for validation |
| 교훈 | No | Lessons learned (if user provides) |

## Reason Categories & Validation Status

See SKILL.md Steps 3-4 for reason categories and validation status definitions.

## Example

```markdown
---
archived: 2025-12-01
reason: superseded
---

# 📦 Archive: Prompt-Library-Manager

## 아카이브 이유

> **사용자**: 프롬프트가 중요한 시대는 지남

## 검증

2024-2025년 이후 LLM 발전으로 정교한 프롬프트 엔지니어링의 중요성이 감소함.

### 검증 결과

✅ **검증됨**: 최신 모델들(Claude 3.5+, GPT-4o+)은 의도 파악 능력이 향상되어 복잡한 프롬프트 없이도 원하는 결과를 얻을 수 있음. Agentic AI와 tool use 패러다임으로 전환 중.

### 출처

- [The End of Prompt Engineering?](https://example.com/article1)
- [Anthropic Blog: Claude's Improved Understanding](https://anthropic.com/blog)

## 교훈

프롬프트 라이브러리보다 스킬/에이전트 시스템이 더 효과적.
```
