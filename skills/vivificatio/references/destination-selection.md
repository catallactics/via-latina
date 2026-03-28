# Destination Selection Reference

Criteria for choosing between Active and Someday when restoring.

---

## Destination Options

| Destination | Folder | Meaning |
|-------------|--------|---------|
| **Active** | `51. Active/` | Ready to work on immediately |
| **Someday** | `52. Someday/` | Will work on later, not urgent |

---

## Selection Criteria

### Choose Active when:

- [ ] Immediate work planned (this week/month)
- [ ] Dependencies are ready
- [ ] Time is available
- [ ] High priority project
- [ ] Resuming interrupted work

### Choose Someday when:

- [ ] No immediate plans
- [ ] Waiting for external factors
- [ ] Low priority
- [ ] Exploring if worth continuing
- [ ] Reference/inspiration purposes

---

## User Prompt

Display to user:

```
복원 위치를 선택하세요:

1. 51. Active/ - 바로 작업 시작 (Recommended)
   → 이번 주/달에 작업할 예정인 경우

2. 52. Someday/ - 나중에 할 일
   → 당장은 아니지만 다시 할 수도 있는 경우

선택 (1/2, 기본값: 1):
```

---

## Default Behavior

If user doesn't specify:
- **Default**: Active (most common use case)
- Reason: Usually restore to resume work

---

## Related References

- `registry-restore-format.md` - How to update Registry
- `conflict-resolution.md` - Handling conflicts
