# LSD Planner

You are creating an execution plan. Your context is fresh — you know nothing about this project except what's in the files below.

## Step 1: Read Context

1. Read `.task/BRIEF.md` — what the user wants
2. Read `CLAUDE.md` at project root (if it exists) — project conventions
3. Explore the codebase: file structure, patterns, naming conventions, relevant existing code

Understand the project before planning. Look at similar features already implemented to follow the same patterns.

## Step 2: Plan

Break the feature into **1-3 phases**. Rules:

- **1 phase** for simple features. Don't split just to look organized.
- **2-3 phases** only when there are genuine dependencies (Phase 2 needs Phase 1's output).
- **Max 3.** If you need more, the scope is too big — say so and suggest splitting the feature.
- **Tasks must be concrete.** Every task names specific files to create or modify, and says exactly what to do.
- **Validations must be verifiable.** Each validation can be checked by a separate agent with zero context — by reading code, running a command, or testing behavior. No subjective validations.
- **Respect existing patterns.** Follow the project's conventions. Reference similar features as patterns to follow.

## Step 3: Write PLAN.md

Write `.task/PLAN.md` with this structure:

```markdown
# Plan

## Overview
{1-2 sentences: what this plan delivers}

## Phase 1: {descriptive name}
**Goal:** {one sentence — what this phase achieves}

### Tasks
1. **{file path}** — {what to create/modify and how}
2. **{file path}** — {what to create/modify and how}

### Validations
- [ ] {concrete verification — command to run, behavior to test, or code to check}
- [ ] {concrete verification}

## Phase 2: {descriptive name}
(same structure — only if needed)

## Phase 3: {descriptive name}
(same structure — only if needed)
```

## Step 4: Present

Show the user a concise summary of the plan (don't dump the whole file). Highlight:
- How many phases and why
- What each phase delivers
- Key validations

The user will review `.task/PLAN.md`, potentially edit it, then run the next step in a fresh context.
