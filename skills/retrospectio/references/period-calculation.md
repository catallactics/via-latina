# Period Calculation Reference

ISO week calculation logic for retrospectio.

---

## ISO Week Standard

- Week starts on **Monday**
- Week 1 is the week containing the first Thursday of the year
- Week numbers: 01-52 (or 53 in some years)

---

## Input Parsing

| Input Format | Example | Parsing Logic |
|-------------|---------|---------------|
| (empty) | - | Current week |
| `last week` | - | Current week - 1 |
| `저번 주` | - | Current week - 1 |
| `YYYY-Www` | `2026-W02` | Direct ISO week |
| `M월 W주차` | `3월 2주차` | Calculate ISO week for that month |
| `YYYY-MM-Ww` | `2026-03-W2` | Calculate ISO week for that month |
| Date range | `2026-01-06 ~ 2025-11-15` | Find containing ISO week |

---

## Week Dates Calculation

Given year and week number, calculate Monday and Sunday:

```javascript
function getWeekDates(year, week) {
  // Find January 4th (always in week 1)
  const jan4 = new Date(year, 0, 4);
  const dayOfWeek = jan4.getDay() || 7; // Sunday = 7

  // Calculate Monday of week 1
  const week1Monday = new Date(jan4);
  week1Monday.setDate(jan4.getDate() - dayOfWeek + 1);

  // Calculate target week's Monday
  const targetMonday = new Date(week1Monday);
  targetMonday.setDate(week1Monday.getDate() + (week - 1) * 7);

  // Sunday is 6 days after Monday
  const targetSunday = new Date(targetMonday);
  targetSunday.setDate(targetMonday.getDate() + 6);

  return { monday: targetMonday, sunday: targetSunday };
}
```

---

## Month Week Parsing ("M월 W주차")

Find first Monday of/after month start → offset to week N → convert to ISO week number.

## Korean Input Patterns

| Regex | Example |
|-------|---------|
| `저번\s*주` | 저번 주, 저번주 |
| `지난\s*주` | 지난 주, 지난주 |
| `이번\s*주` | 이번 주, 이번주 |
| `(\d{1,2})월\s*(\d{1,2})주차` | 3월 2주차 |

---

