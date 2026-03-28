# Architecture

## Skill Interconnections

Via Latina skills are not isolated — they form a directed graph of workflows.

```
┌─────────────────────────────────────────────────────────┐
│                    ORCHESTRATION MAP                     │
└─────────────────────────────────────────────────────────┘

  indagatio ──────▶ purgatio ──────▶ commentatio
  (external          (batch           (annotation
   sources)          processing)       synthesis)
                        │                  │
                        │                  ▼
                        ▼              elevatio ──▶ memorio
                     locatio           (project      (archive)
                     (on-hold)          create)
                                          │
                                    ┌─────┼─────┐
                                    ▼     ▼     ▼
                                dilatio  susp. vivific.
                                (wait)  (pause) (revive)
                                          │
                                          ▼
                                        unio
                                       (merge)

  tentatio ─────────────────────▶ retrospectio
  (vault audit)                   (periodic review)
      │
      └──── self-evolution (detects own coverage gaps)
```

## Cross-Skill Communication

Skills communicate through three mechanisms:

### 1. Direct Invocation
`purgatio` detects Latin callouts (`[!quaero]`, `[!facio]`, etc.) and invokes `commentatio` to process them.

### 2. Shared References
All skills read from `_common/references/` for consistent behavior:
- Status values, type taxonomy, vault paths, logging format

### 3. Trigger Conditions
`commentatio` monitors for patterns that suggest a project is forming (3+ `[!disco]` on the same topic) and suggests `elevatio`.

## Dual-Mode Pattern

Several skills operate in two modes:

| Mode | Triggered by | Effect |
|------|-------------|--------|
| **Full** | User (`/purgatio`) | Modifies vault, creates files, archives |
| **Report** | Other skills | Appends to daily log, no vault changes |

This prevents cascading modifications when skills call each other.

## Quality Feedback Loop

```
tentatio (audit) → finds issues → reports
                                     ↓
retrospectio (review) → summarizes → user decides
                                        ↓
                                   manual fixes or
                                   skill improvements
                                        ↓
                              tentatio evolves checks
```

`tentatio` doesn't just find problems — it identifies gaps in its own coverage and suggests new audit rules. This self-evolution pattern keeps the system adapting to vault changes.
