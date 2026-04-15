# LSD Executor

You are executing one phase of a plan. Your context is fresh — you know nothing about this project except what's in the files below.

## Lean Mode File Reading

**Only applies if lean mode is active from SKILL.md.** Skip this section entirely if lean mode is not set — proceed directly to Step 1.

### If LEAN_CC (Claude Code)

Instead of reading files directly, spawn a **single** `Agent(model=haiku)` subagent to read and summarize all needed files at once. Do **not** read raw file contents yourself — use only the summary the agent returns.

Spawn one agent with this prompt:
> "Read the following files and return a focused summary of each. Be concise — your output goes into a limited context window, no raw dumps.
> - `.task/PLAN.md`: overall plan summary and specifically Phase N (replace N with the assigned phase number) — goal, tasks, and validations
> - `CLAUDE.md` at project root (if it exists): project conventions, patterns, and anything relevant to implementing features
> - All `.task/PHASE-*.md` files for phases before N (if any): what was built, which files were changed, and any notes for the next phase
> - Source files referenced in Phase N's tasks: structure, patterns, and sections relevant to the tasks"

### If LEAN_CURSOR

You cannot spawn subagents. List the files you need and ask the user to paste their contents before proceeding:

> "To proceed in lean mode, please paste the contents of:
> - `.task/PLAN.md`
> - `CLAUDE.md` (root, if it exists)
> - `.task/PHASE-*.md` files for earlier phases (if any)
> - Source files referenced in Phase N's tasks (list them by name once you know them from PLAN.md)"

Wait for the user to paste, then proceed using the pasted contents.

## Step 1: Read Context

1. Read `.task/PLAN.md` — the full plan (focus on your assigned phase)
2. Read `CLAUDE.md` at project root (if it exists) — project conventions
3. If executing Phase 2+, read `.task/PHASE-*.md` summaries from earlier phases to understand what was already built
4. Read the source files referenced in your phase's tasks before modifying them

## Step 2: Implement

Execute **only the tasks listed under your assigned phase**. Rules:

- **Follow the plan exactly.** Don't add features, refactor surrounding code, or "improve" things beyond scope.
- **Follow existing patterns.** Match the project's naming conventions, file structure, import style. Read similar files before writing new ones.
- **Commit after each logical unit.** Group related changes into atomic commits with descriptive messages.
- **Handle blockers.** If a task can't be implemented as planned (missing dependency, wrong assumption, conflicting code), implement the closest working alternative and document the deviation.
- **No extras.** No tests, docstrings, README updates, or comments unless the plan explicitly asks for them.

## Step 3: Write Summary

After completing all tasks, write `.task/PHASE-{N}.md`:

```markdown
# Phase {N}: {phase name}

## Completed Tasks
1. **{file path}** — {what was done} (`{commit hash}`)
2. **{file path}** — {what was done} (`{commit hash}`)

## Files Changed
- `{path}` — {brief description of change}
- `{path}` — {brief description of change}

## Deviations
{any differences from the plan and why — or "None"}

## Notes
{anything the next phase or verifier should know — or "None"}
```

## Step 4: Report

Tell the user what was done in a brief summary. They will review the code and the summary, then decide to continue to the next phase or verify.
