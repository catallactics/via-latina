# Report Template

> Format for tentatio audit reports saved to vault

## Report Location

```
90. SYSTEM/Verification-Reports/YYYY-MM-DD-tentatio.md
```

Create directory if it doesn't exist.

## Report Structure

### Frontmatter

```yaml
created: YYYY-MM-DD HH:mm
type: verification-report
tags: [tentatio, quality-audit]
status: reference
```

### Header

```markdown
# Tentatio 감사 보고서
> 생성: YYYY-MM-DD HH:mm | 범위: {scope label}
```

### 요약 (Summary Table)

```markdown
| 검사 | 심각도 | 발견 | 자동수정 |
|------|--------|------|----------|
```

Footer: `**총 {total}건 발견** ({fixable}건 자동수정 가능)`

### Section Formats (by check)

| # | Section Header | Key Columns |
|---|----------------|-------------|
| 1 | 폴더/상태 동기화 [CRITICAL] | 파일, 현재 상태, 예상 상태, 폴더 — split README(자동수정 가능) vs 개별 파일(보고만) |
| 2 | 비표준 필드값 [WARNING] | Sub-tables: status/phase/maturity — 파일, 현재 값, 권장 값, 자동수정 |
| 3 | 레지스트리/폴더 동기화 [WARNING] | 유형, 프로젝트, 상세 |
| 4 | README 누락 [WARNING] | 프로젝트, 경로 |
| 5 | 스킬 건강 [INFO] | 스킬, 문제, 상세 |
| 6 | 지식 신선도 [INFO] | Sub-tables: 조치 필요, 조기 경보, 현재 유효(공부 가이드), 메타데이터 부재 |
| 7 | 장기 방치 노트 [INFO] | Sub-tables: INCUBATING(90일+), CREATIONS(90일+ draft) — 파일, 상태, 나이, 경로 |
| 8 | 비표준 타입값 [WARNING] | 파일, 현재 값, 권장 값, 자동수정 |
| 9 | 난이도 검증 [INFO] | Sub-tables: 범위 초과(1-10 밖), 불일치(타입 기반 기대치 vs 실제), 누락 — 파일, 현재 값, 예상 범위, 조치 |
| 10 | 세션 후속 조치 [WARNING] | 파일, 세션 날짜, 후속 항목, 상태 |

Omit sections with 0 findings.

### Section 6 Knowledge Extras

**Header**: `> 근거: 지식의 반감기 (Machlup, 1962). 분야별 노후화 속도에 기반한 리뷰 트리거.`

| Sub-section | Columns |
|-------------|---------|
| 조치 필요 | 파일, 도메인, 추정 등급, 나이, 리뷰 트리거, 조치 |
| 조기 경보 | 파일, 등급, 나이, 다음 리뷰 |
| 현재 유효 (공부 가이드) | 등급, 노트 수, 공부 방식 |
| 메타데이터 부재 | 파일, 누락 필드, 영향 |

**공부 방식 text** (verbatim):
- timeless: `깊이 숙달 - 시간 투자 가치 높음`
- slow-decay: `깊이 공부 + 24개월마다 세부사항 확인`
- medium-decay: `필요 시 학습 + 12개월마다 재검증`
- fast-decay (유효): `핵심만 파악 + 6개월마다 최신화 확인`

**Footer**: `**조치 필요: {n}건 | 유효: {n}건 | 메타데이터 부재: {n}건**`

### 학습 우선순위 가이드 (Knowledge scope only)

```markdown
## 학습 우선순위 가이드

지식의 반감기에 따른 학습 전략:

1. **timeless ({n}건)**: 이것들이 핵심. 깊이 공부하면 평생 가치.
   → 철학, 심리학, 수학 개념들. 투자 대비 수익이 가장 높음.
2. **slow-decay ({n}건)**: 깊이 공부할 가치 있음. 2년마다 세부사항만 확인.
   → 프로토콜, 자연과학 응용. "원리는 영원, 예시는 갱신."
3. **medium-decay ({n}건)**: 필요할 때 학습. 모든 것을 외울 필요 없음.
   → 도구/플랫폼. "어디서 찾는지만 알면 됨."
4. **fast-decay ({n}건 중 {m}건 기한 초과)**: 최신 버전으로 교체 우선.
   → AI 연구, 최신 기법. "6개월 전 정보는 이미 절반이 낡았을 수 있음."
```

### 수정 내역 (when auto-fixes applied)

```markdown
| 시간 | 파일 | 변경 전 | 변경 후 |
|------|------|---------|---------|
```

## Scope → Sections Matrix

| Trigger | Scope Label | Sections |
|---------|-------------|----------|
| `/tentatio` | `전체 (Full)` | 1, 2, 3, 4, 5, 7, 8, 9, 10 |
| `/tentatio 50-NNNNNN` | `프로젝트: 50-NNNNNN. Name` | 1, 3, 4 |
| `/tentatio --skills` | `스킬 건강 (Skills only)` | 5 |
| `/tentatio --knowledge` | `지식 신선도 (Knowledge only)` | 6, 9 + 학습 우선순위 |
| `/tentatio --knowledge 34` | `지식 신선도: 34. AI & LLM` | 6 (filtered) + 학습 우선순위 |
| `/tentatio --sessions` | `세션 후속 조치 (Sessions)` | 10 |
| `/tentatio --evolve` | `자가 진화 분석 (Evolve)` | Self-Evolution section |

## Self-Evolution Report Format (--evolve)

```markdown
## 자가 진화 분석
> 생성: YYYY-MM-DD HH:mm

### 커버리지 현황
| 볼트 영역 | 담당 체크 | 상태 |
|-----------|----------|------|

### 감지 결과
| # | 유형 | 대상 | 설명 |
|---|------|------|------|

### 제안
| 유형 | 대상 | 제안 |
|------|------|------|

**커버됨: {n}개 영역 | 미커버: {m}개 영역 | 감지 결과: {k}건**
```

Detection types: Uncovered Folder, Unvalidated Field, Unregistered Check, Manifest Drift, Structure Drift.
