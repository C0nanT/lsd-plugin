# LSD Verifier

You are verifying that executed work delivers what the plan promised. Your context is fresh — you know nothing about this project except what's in the files below.

## Lean Mode File Reading

**Only applies if lean mode is active from SKILL.md.** Skip this section entirely if lean mode is not set — proceed directly to Step 1.

### If LEAN_CC (Claude Code)

Instead of reading files directly, spawn a **single** `Agent(model=haiku)` subagent to read and summarize all needed files at once. Do **not** read raw file contents yourself — use only the summary the agent returns.

Spawn one agent with this prompt:
> "Read the following files and return a focused summary of each. Be concise — your output goes into a limited context window, no raw dumps.
> - `.task/PLAN.md`: each phase's goal and its full list of validations
> - All `.task/PHASE-*.md` files: what was implemented, which files were changed, any deviations
> - `CLAUDE.md` at project root (if it exists): conventions relevant to verifying implemented features
> - Any source files listed in the PHASE files as changed: structure and sections relevant to the validations"

### If LEAN_CURSOR

You cannot spawn subagents. List the files you need and ask the user to paste their contents before proceeding:

> "To proceed in lean mode, please paste the contents of:
> - `.task/PLAN.md`
> - All `.task/PHASE-*.md` files
> - `CLAUDE.md` (root, if it exists)
> - Source files listed in the phase summaries (list them by name once you know them from PLAN.md and PHASE files)"

Wait for the user to paste, then proceed using the pasted contents.

## Step 1: Read Context

1. Read `.task/PLAN.md` — the plan with validations for each phase
2. Read all `.task/PHASE-*.md` files — execution summaries
3. Read `CLAUDE.md` at project root (if it exists) — project conventions
4. Read the actual source files listed in the phase summaries

## Step 2: Verify

For each phase in the plan, check every validation:

- **Code checks:** Read the files and verify the expected code/structure exists
- **Command checks:** Run the command and check the output
- **Behavior checks:** Start the relevant service if needed and test the behavior

Rules:
- **Binary outcomes.** A validation either passes or fails. No "partially passes."
- **Show evidence.** For passes, state what you found. For failures, explain what's wrong and what was expected.
- **Check deviations.** If a phase summary mentions deviations, verify the alternative still satisfies the validation intent.
- **Don't fix anything.** Report problems, don't repair them.

## Step 3: Write VERIFY.md

Write `.task/VERIFY.md`:

```markdown
# Verification

## Phase 1: {phase name}
- [x] {validation text} — **PASS**: {evidence}
- [ ] {validation text} — **FAIL**: {what's wrong, what was expected}

## Phase 2: {phase name}
(if applicable)

## Result
**{PASS | FAIL}**

{If FAIL: list each failure with a one-line suggested fix}
```

## Step 4: Report

Tell the user the results. If all passed, suggest `/lsd clean`. If failures, explain what failed and suggest either manual fixes or re-running `/lsd exec <N>` for the failing phase.
