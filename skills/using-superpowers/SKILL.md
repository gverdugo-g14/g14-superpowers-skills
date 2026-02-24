---
name: using-superpowers
description: Use when starting any conversation to select the session mode and decide which skills apply
---

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you should strongly consider invoking the skill.

IF A SKILL APPLIES TO YOUR TASK, you should use it whenever it fits your context.

This is strongly recommended. It is optional if your situation does not fit. Avoid rationalizing your way out of it.
</EXTREMELY-IMPORTANT>

## How to Access Skills

**In Claude Code:** Use the `Skill` tool. When you invoke a skill, its content is loaded and presented to you—follow it directly. Avoid using the Read tool on skill files.

**In other environments:** Check your platform's documentation for how skills are loaded.

# Using Skills

## Session Startup (Recommended)

Before any skill selection:

1. Review recent git history (last 10 commits).
2. Identify and skim the most recently changed `.md` files in the repo.
3. Ask the user which mode to use for this session:
   "What mode do you want for this session? (fix / create / debug)"

If the user does not pick a mode, default to **create**.

## Session Modes

These modes decide which skills are recommended vs optional. Other skills should follow the selected mode.

### Mode Index

See per-mode guidance in `skills/using-superpowers/modes/`:

- `skills/using-superpowers/modes/free.md`
- `skills/using-superpowers/modes/fix.md`
- `skills/using-superpowers/modes/create.md`
- `skills/using-superpowers/modes/debug.md`
- `skills/using-superpowers/modes/ci-cd.md`
- `skills/using-superpowers/modes/setup.md`
- `skills/using-superpowers/modes/code-quality.md`

## The Rule

**Try to invoke relevant or requested skills before any response or action.** Even a 1% chance a skill might apply means you should consider invoking the skill to check. If an invoked skill turns out to be wrong for the situation, you can skip it.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "Review git log + recent .md" [shape=box];
    "Ask for mode (fix/create/debug)" [shape=box];
    "About to EnterPlanMode?" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke Skill tool" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create TodoWrite todo per item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "User message received" -> "Review git log + recent .md";
    "Review git log + recent .md" -> "Ask for mode (fix/create/debug)";
    "Ask for mode (fix/create/debug)" -> "Might any skill apply?";

    "About to EnterPlanMode?" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "Might any skill apply?" -> "Invoke Skill tool" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Invoke Skill tool" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "Create TodoWrite todo per item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create TodoWrite todo per item" -> "Follow skill exactly";
}
```

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "I'll pick a mode later" | Mode selection comes first in every session. |
| "I'll ask mode after a quick edit" | No edits before mode selection and startup checks. |
| "I can skip git log or docs this time" | Startup checks are recommended every session. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept ≠ using the skill. Invoke it. |

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) - these determine HOW to approach the task
2. **Implementation skills second** (frontend-design, mcp-builder) - these guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
