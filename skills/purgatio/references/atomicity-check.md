# Atomicity Check Reference

Criteria for validating quick note atomicity (one note = one idea).

## Atomicity Principle

```
Atomic note = Single idea that cannot be meaningfully split further
```

**Goal**: Units easy to connect, search, and reuse later

## Violation Detection Patterns

### 1. Topic Transition Signals

Following expressions may indicate mixed topics:

| Pattern | Examples |
|---------|----------|
| Transition conjunctions | "and also", "by the way", "different topic" |
| Time transitions | "in the morning", "after lunch", "in the evening" |
| Topic switches | "separately from that", "another thought" |
| Numbered lists | "first... second..." (when different topics) |

### 2. Project Mixing

Multiple projects mentioned in one quick note:

```markdown
# Bad Example (Atomicity violation)

Finished Arduino step 5.
Also tried TouchDesigner, it's more fun than expected.
Oh and I need to prepare for tomorrow's meeting...
```

Split into 3:
- Arduino learning progress
- TouchDesigner first impression
- Meeting prep task

Other violation patterns: **Time period mixing** (morning/evening in one note), **Emotion/fact mixing** (objective record + subjective reflection). For borderline cases, user decides.

## Atomicity Maintenance Criteria

### Cases NOT Needing Split

1. **Detailed record of single activity**
```markdown
# OK - Atomic

Arduino Step 5 PWM learning
- Understood analogWrite() function
- Grasped duty cycle concept
- Still confused about: frequency setting
- Tomorrow: review then Step 6
```
→ All single topic "Arduino Step 5 learning"

Also OK: **Connected flow of thoughts** (extension of single topic), **Cause-effect chain** (emotion → cause → solution in one context).

### Cases Needing Split

1. **Different projects**
2. **Unconnected separate ideas**
3. **Activities from different times/contexts**

## Split Method

### Step 1: Identify Topics

Extract independent topics from note content:

```
Original: "Did Arduino, also tried TD, need to prep for meeting"
     
Topic 1: Arduino learning
Topic 2: TouchDesigner exploration
Topic 3: Meeting prep task
```

### Step 2: User Confirmation

```markdown
> [!question] Atomicity Check
> 3 topics detected in this quick note:
> 1. Arduino learning progress
> 2. TouchDesigner first impression
> 3. Meeting prep task
>
> Split them?
> - [ ] Split all (recommended)
> - [ ] Keep 1+2, split 3 only
> - [ ] Don't split
```

### Step 3: Execute Split

Each split note: keep original time, note source, assign topic-specific title. Archive record lists all split-to links.

## Decision Criteria Summary

| Situation | Decision | Reason |
|-----------|----------|--------|
| Detailed record of one activity | Keep | Single context |
| Extension of one idea | Keep | Connected thoughts |
| Different projects | Split | No connection |
| Different time activities | Split | Different context |
| Fact + reflection | Case by case | Depends on connection strength |

**When uncertain, ask user**
