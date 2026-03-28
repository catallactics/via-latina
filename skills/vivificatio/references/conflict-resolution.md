# Conflict Resolution Reference

Handling conflicts during project restoration.

---

## Conflict Types

### 1. Same Number Exists

**Scenario:** Trying to restore 50u, but 50u already exists in Active/Someday.

**Cause:** Should never happen (numbers are unique). Indicates data inconsistency.

**Resolution:**
```
오류: 50u가 이미 51. Active/에 존재합니다.
데이터 불일치가 있습니다. Registry를 확인해주세요.
```

**Action:** Do not proceed. Manual investigation required.

### 2. Same Name Exists

**Scenario:** Restoring "Sample-Project" but different project with same name exists.

**Resolution:**
```
경고: "Sample-Project"라는 이름의 프로젝트가 이미 존재합니다.
- 복원하려는 프로젝트: 50-000008. Sample-Project
- 기존 프로젝트: 50-NNNNNN. Sample-Project

진행하시겠습니까? (프로젝트 번호가 다르므로 공존 가능)
```

**Action:** Allow with user confirmation (numbers are different).

### 3. Archive Folder Empty

**Resolution:**
```
아카이브된 프로젝트가 없습니다.
54. Archive/ 폴더가 비어있습니다.
```

**Action:** Exit gracefully.

### 4. Project Not in Archive

**Resolution:**
```
50-000008. Sample-Project는 Archive에 없습니다.
현재 위치: 51. Active/

아카이브된 프로젝트 목록:
- 50-000003. Archived-Campaign (archived: 2025-11-02)
```

**Action:** Show available archived projects.

### 5. Registry Inconsistency

**Resolution:**
```
경고: Registry와 실제 폴더 상태가 다릅니다.

폴더: 50-000008. Sample-Project 존재 (54. Archive/)
Registry: 50u 항목 없음

복원을 진행하면 Registry가 업데이트됩니다.
진행하시겠습니까?
```

**Action:** Proceed with user confirmation, fix Registry.

---

## Error Messages (Korean)

| Error | Message |
|-------|---------|
| Not in archive | `{project}는 Archive에 없습니다` |
| Number conflict | `{number}가 이미 {destination}에 존재합니다` |
| Empty archive | `아카이브된 프로젝트가 없습니다` |
| Registry mismatch | `Registry와 폴더 상태가 다릅니다` |

---

## Related References

- `registry-restore-format.md` - Registry update format
- `destination-selection.md` - Choosing destination
