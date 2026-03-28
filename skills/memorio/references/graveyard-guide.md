# Graveyard Guide

> 프로젝트 실패/중단 기록 가이드
> "역사를 잊은 자에게 미래는 없다"

---

## Graveyard란?

프로젝트가 중단되거나 실패했을 때 그 이유와 교훈을 기록하는 공간.
성공한 프로젝트는 기록하지 않음 - 실패에서 배우기 위한 목적.

---

## 기록 대상

| 상황 | Graveyard 기록 |
|-----|---------------|
| 목표 달성 (completed) | ❌ |
| 중단 (discontinued) | ✅ 필수 |
| 일시 중지 (on hold) | ✅ 필수 |
| 병합 (merged) | ❌ |

---

## 필수 질문

아카이브 시 사용자에게 반드시 물어볼 것:

### 1. 중단 사유 (Failure Cause)

```
"왜 이 프로젝트를 중단하나요?"
```

**예시 답변:**
- "외부 API 서비스가 종료됨"
- "우선순위가 변경됨"
- "기술적으로 불가능함을 발견"
- "시간/자원 부족"
- "다른 프로젝트에 병합할 필요성"

### 2. 배운 점 (Lessons Learned)

```
"이 경험에서 무엇을 배웠나요?"
```

**예시 답변:**
- "외부 의존성 있는 프로젝트는 대안 플랜 필요"
- "MVP부터 시작했어야 함"
- "요구사항을 먼저 명확히 해야 함"
- "혼자 하기엔 범위가 너무 컸음"

---

## Registry 기록 형식

`90. SYSTEM/93. Registries/Project-Number-Registry.md`의 Graveyard 섹션:

```markdown
## ⚰️ Graveyard

| Number | Name | Archived | Reason | 💀 Failure Cause | 📚 Lessons |
|--------|------|----------|--------|------------------|------------|
| 51x | Failed-Project | 2026-01-11 | discontinued | API 서비스 종료 | 외부 의존성 대안 플랜 필수 |
| 51y | Paused-Project | 2026-01-11 | on hold | 우선순위 변경 | 시작 전 우선순위 재확인 |
```

---

## 기록 원칙

- **정직하게**: 실패를 미화하지 않기, 진짜 이유 기록 (❌ "잘 안 됐음" → ✅ "3주 예상 8주 소요, 요구사항 분석 부족")
- **구체적으로**: 나중에 읽어도 이해 가능한 수준
- **교훈 중심**: "다음에는 ~ 하자" 형태

---

## Related

- `archive-checklist.md` - 아카이브 전 체크리스트
- `../../_common/references/registry-format.md` - Registry 업데이트 형식
