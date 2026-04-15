# Plan

## Overview
Eliminate the file-reading subagent spawned in LEAN_CC mode across all three agent files. The parent agent reads files directly, removing the ~18k token coldstart from those sub-spawns entirely.

## Phase 1: Remove file-reading subagents from LEAN_CC mode

**Goal:** Replace each `Agent(model=haiku)` file-reading spawn in LEAN_CC sections with direct Read/Glob calls in the parent context, so no subagent is spawned just to read files.

### Tasks

1. **`skills/lsd/agents/planner.md`** — Rewrite the `LEAN_CC` section: remove the Agent spawn. Instead, instruct the parent to read `.task/BRIEF.md`, `CLAUDE.md` (root), and use Glob for top-level structure directly. Keep the summary step so the parent distills what it read before planning.

2. **`skills/lsd/agents/executor.md`** — Same pattern: audit the LEAN_CC section, replace any Agent spawn for file reads with direct Read/Glob on `.task/PLAN.md`, `CLAUDE.md`, and relevant `PHASE-*.md` files.

3. **`skills/lsd/agents/verifier.md`** — Same pattern: replace any Agent spawn with direct reads of `.task/PLAN.md` and `PHASE-*.md`.

### Validations
- [ ] `agents/planner.md` LEAN_CC section contains no `Agent(` call — only Read/Glob/Grep instructions
- [ ] `agents/executor.md` LEAN_CC section contains no `Agent(` call — only Read/Glob/Grep instructions
- [ ] `agents/verifier.md` LEAN_CC section contains no `Agent(` call — only Read/Glob/Grep instructions
- [ ] User runs `/lsd plan` in LEAN_CC mode and no subagent is spawned for file reading (verify via Claude Code terminal — no "Starting agent…" line for a file-read sub-spawn)
