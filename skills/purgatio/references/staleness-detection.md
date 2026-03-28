# Staleness Detection Reference

> Detect Quick Notes that were already actioned via conversation (outdated content).

---

## Detection Strategy

### Trigger: Action Words

Scan Quick Note for action-indicating words:

**Korean:**
```
업데이트, 디벨롭, 구현, 방법, 아이디어, 구축, 추가, 수정, 만들, 개선, 변경, 설계, 작성
```

**English:**
```
update, develop, implement, method, idea, build, add, modify, create, improve, change, design, write
```

**If action word detected** → Proceed to 3-Phase Detection
**If no action word** → Skip Staleness Detection, process normally

---

## 3-Phase Detection (A+B+C)

### Phase A: AI Context Extraction

**Purpose**: Understand what the Quick Note is about.

**Logic**:
```
1. AI reads Quick Note content
2. Extract core intent:
   - What action is requested? (create, update, add...)
   - What is the target? (skill name, project, feature...)
   - What domain? (Via Latina, specific project, general vault...)

3. Output: Search terms for Phase B
```

---

### Phase B: Activity Log Search

**Purpose**: Check if related activity occurred after Quick Note creation.

**Search Locations**:
```
1. System guide updates: 90. SYSTEM/94. System Guides/{matching guide}/updates/
2. Project activity logs: {project}/logs/activity.md
3. AI Processing Log: 90. SYSTEM/96. Data/Log-00-Processing.md
```

**Logic**:
```
1. Get Quick Note creation time (frontmatter or filename)
2. Search locations for extracted terms
3. Filter: Only entries AFTER Quick Note creation
4. If match found → Proceed to Phase C
5. If no match → Process normally
```

**Search Strategy**:
- Check Via Latina UPDATE files modified after Quick Note date
- Grep activity logs for search terms
- Match against file modification timestamps

---

### Phase C: Confirmation

**Purpose**: Verify match and get user decision.

**Present to User (Korean output)**:
```
┌─────────────────────────────────────────────────┐
│ Stale Quick Note 감지                            │
│                                                 │
│ Quick Note: {title}                             │
│ 생성일: YYYY-MM-DD HH:mm                         │
│                                                 │
│ 관련 활동 발견:                                   │
│ - {log entry or update summary}                 │
│ - 날짜: YYYY-MM-DD                              │
│                                                 │
│ 선택:                                            │
│ 1. Skip (이미 처리됨으로 아카이브)                  │
│ 2. Process (추가 작업 필요)                       │
│ 3. Review (차이점 먼저 보기)                       │
└─────────────────────────────────────────────────┘
```

---

## Skip Handling

When user chooses "Skip":

1. Move Quick Note to Archive
2. Update frontmatter: `status: archived` + `superseded: true`
3. Add archive note:
   ```markdown
   ## Purgatio
   - Status: Archived (superseded — already implemented)
   - Detected: YYYY-MM-DD HH:mm
   - Evidence: {brief summary of matching activity}
   ```
4. Include in Daily Note report:
   ```
   - Skipped (superseded): [[Quick Note Title]]
   ```

---

## Edge Cases

| Case | Action |
|------|--------|
| No activity logs exist | Rely on Via Latina updates only; if no match → process normally |
| Ambiguous context | Ask user: "What project/skill is this related to?" |
| Partial implementation | User chooses "Process" → focus on unimplemented parts |
| Multiple matches | Show all to user, user decides relevance |

Runs between Step 2 (Collect) and Step 3 (Atomicity). Only non-superseded notes proceed.
