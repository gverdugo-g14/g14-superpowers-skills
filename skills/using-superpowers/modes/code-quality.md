# Code Quality Mode

## Goal

Improve code quality: refactor, standards, and technical reviews.

## Base Flow

1. Identify tech debt or quality issues.
2. Apply refactor or adjustments with small changes.
3. Verify tests stay green.
4. Request or process reviews when needed.

## Execution Diagram

```dot
digraph code_quality_mode_flow {
    "Identify quality issues" [shape=box];
    "Plan small refactor" [shape=box];
    "Apply change" [shape=box];
    "Verify tests" [shape=box];
    "Review needed?" [shape=diamond];
    "Request or process review" [shape=box];

    "Identify quality issues" -> "Plan small refactor";
    "Plan small refactor" -> "Apply change";
    "Apply change" -> "Verify tests";
    "Verify tests" -> "Review needed?";
    "Review needed?" -> "Request or process review" [label="yes"];
}
```

## Skills

### Recommended

- `../../requesting-code-review/SKILL.md` — Review quality changes before merge.
- `../../receiving-code-review/SKILL.md` — Process feedback with technical rigor.
- `../../verification-before-completion/SKILL.md` — Verify before claiming done.

### Optional

- `../../test-driven-development/SKILL.md` — Refactor guided by tests.
- `../../systematic-debugging/SKILL.md` — If refactor introduces failures.
- `../../writing-plans/SKILL.md` — Large or multi-step refactors.
