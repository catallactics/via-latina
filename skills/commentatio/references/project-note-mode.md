# Project Note Mode Reference

Detailed handling for commentatio's Project Note Mode.

---

## Mode Detection

`IF path contains "50. PROJECTS/" → Project Note Mode | ELSE → Synthesis Mode` (see SKILL.md)

---

## Scope Detection Algorithm

Find nearest heading above callout: H3 (primary) → H2 (secondary) → H1 (tertiary). If edit doesn't fit in scope, expand to parent heading.

### Example

```markdown
# Project Alpha Concept          ← H1
## 준비 작업                      ← H2 (secondary scope)
### 옵션 A: 초안 구성             ← H3 (primary scope)

> [!emendo] 새로운 항목 리스트로 재구성
```

Scope: `### 옵션 A: 초안 구성` → if broader edit needed → expand to `## 준비 작업`.

---

## Callout Processing Examples

### [!emendo] - Direct Edit

**Before**: `## 비주얼 무드` with `> [!emendo] 색상 팔레트 추가` above content.
**After**: Callout removed, `### 색상 팔레트` subsection added with color specs.

### [!facio] - Add Task

**Before**: `## 준비 작업` with `> [!facio] API 문서 최신 버전 확인`.
**After**: Callout removed, `- [ ] API 문서 최신 버전 확인` added to `## Next Steps`.

### [!quaero] - Answer Question

**Before**: `## 이론적 배경` with `> [!quaero] Hyperreality와 Simulacra의 차이점?`.
**After**: Callout removed, `> [!info]` answer block inserted below relevant content.

---

## Logging Format

After each edit, add to `## Log` section:

```markdown
| 날짜 | 내용 |
|------|------|
| 2025-11-15 | [commentatio] {섹션명} {action summary} ({callout type}) |
```

---

## Edge Cases

| Case | Handling |
|------|----------|
| No Log section | Create `## Log` at end of note |
| Multiple callouts | Process sequentially, log each separately |
| Callout outside heading | Note-wide scope, add to most appropriate section |
| Conflicting edits | Process in appearance order, later edits consider earlier changes |
