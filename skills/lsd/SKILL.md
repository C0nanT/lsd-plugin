---
name: lsd
description: "Little Shit Done — lightweight feature planner and executor. Breaks features into 1-3 phases with isolated agent execution. Token-efficient: each step runs in fresh context, communicating only through markdown files. Only triggers when explicitly invoked via /lsd."
argument-hint: "<brief|plan|exec N|verify|clean> [description]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - Agent
  - AskUserQuestion
---

# Little Shit Done (LSD)

Lightweight planning and execution system. Each step runs in **fresh context** — the user does `/clear` between steps. The `.task/` directory is the only shared memory between steps.

## Commands

| Command | What it does | Reads | Writes |
|---------|-------------|-------|--------|
| `/lsd [description]` | Gather requirements | — | `BRIEF.md` |
| `/lsd plan` | Create execution plan | `BRIEF.md` | `PLAN.md` |
| `/lsd exec <N>` | Execute phase N | `PLAN.md`, `PHASE-*.md` | `PHASE-N.md` |
| `/lsd verify` | Verify all validations | `PLAN.md`, `PHASE-*.md` | `VERIFY.md` |
| `/lsd clean` | Delete `.task/` | — | — |

## Lean Mode

**Check for `--lean` in `$ARGUMENTS` before doing anything else.**

If `--lean` is present:
1. Strip `--lean` from `$ARGUMENTS` (the remaining text is the real argument for routing)
2. Ask the user: "Are you running Claude Code or Cursor?"
3. Wait for their answer
4. If they say **Claude Code** (or "CC"): set lean mode to `LEAN_CC` for the rest of this invocation
5. If they say **Cursor**: set lean mode to `LEAN_CURSOR` for the rest of this invocation

If `--lean` is **not** present: skip this entire section. Nothing changes. Proceed directly to Routing.

---

## Routing

Parse `$ARGUMENTS` (after stripping `--lean` if it was present) and execute the matching step below. If no argument or the argument doesn't match a command, treat it as the brief step (the argument is the feature description).

**Routing rules:**
- `plan` → Step: Plan
- `exec <N>` or `exec` → Step: Execute (N required, prompt if missing)
- `verify` → Step: Verify
- `clean` → Step: Clean
- Anything else (or empty) → Step: Brief (treat argument as feature description)

---

## Step: Brief

Gather what the user wants. If they provided a description, use it as starting point.

Ask **only what's missing** — skip questions already answered in the description. Cover these:
1. **What** — What feature or change?
2. **Behavior** — How should it work?
3. **Constraints** — Tech preferences, patterns to follow or avoid?
4. **Done** — How do you verify it works?

Keep it to 3-5 questions max. Be conversational, not an interrogation.

After gathering answers, create `.task/BRIEF.md`:

```markdown
# Brief

## Feature
{what they want — be specific}

## Expected Behavior
{how it should work, user-facing description}

## Constraints
{tech choices, patterns, things to avoid — "None" if none}

## Done Criteria
{concrete, verifiable conditions for "done"}
```

**After completing**, tell the user:
```
Brief saved to .task/BRIEF.md
Next: review the brief, then /clear and run /lsd plan
```

---

## Step: Plan

Read the planner instructions from `agents/planner.md` (path relative to this skill's directory). Then follow those instructions to create the plan.

**After completing**, tell the user:
```
Plan saved to .task/PLAN.md
Review the plan. If you want changes, edit .task/PLAN.md directly.
Next: /clear and run /lsd exec 1
```

---

## Step: Execute

Parse the phase number N from the argument (`exec 1`, `exec 2`, etc.). If missing, prompt.

Read the executor instructions from `agents/executor.md` (path relative to this skill's directory). Then follow those instructions to execute phase N.

**After completing**, tell the user:
```
Phase N complete. Summary saved to .task/PHASE-N.md
Review the code changes and the summary.
Next: /clear and run /lsd exec {N+1}  (or /lsd verify if this was the last phase)
```

---

## Step: Verify

Read the verifier instructions from `agents/verifier.md` (path relative to this skill's directory). Then follow those instructions to verify all phases.

**After completing**, tell the user:
```
Verification saved to .task/VERIFY.md
If all passed: /clear and run /lsd clean
If failures: fix manually or re-run /lsd exec <N> for the failing phase
```

---

## Step: Clean

Delete the entire `.task/` directory. Confirm with the user before deleting.

```
Cleaned up .task/ — all done!
```
