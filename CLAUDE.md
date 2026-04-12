# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code plugin that ships a single skill: `/lsd` (Little Shit Done). The entire plugin is **markdown files** — no build step, no tests, no package manager.

## Development setup

To test edits live in Claude Code, symlink the skill instead of copying it:

```bash
rm -rf ~/.claude/skills/lsd
ln -s $(pwd)/skills/lsd ~/.claude/skills/lsd
```

After editing any file under `skills/lsd/`, the change is immediately active — just invoke `/lsd` in a Claude Code session.

## Architecture

```
skills/lsd/
├── SKILL.md          # Entry point — parses $ARGUMENTS, routes to steps, defines Brief/Clean inline
└── agents/
    ├── planner.md    # Read by SKILL.md "Plan" step — agent instructions for creating PLAN.md
    ├── executor.md   # Read by SKILL.md "Execute" step — agent instructions for implementing a phase
    └── verifier.md   # Read by SKILL.md "Verify" step — agent instructions for checking validations
```

**How skill invocation works:**
- Claude Code loads `SKILL.md` as the skill's prompt when `/lsd` is called
- `SKILL.md` handles Brief and Clean directly (no agent file needed)
- For Plan, Execute, and Verify, `SKILL.md` instructs Claude to read the corresponding `agents/*.md` file and follow those instructions
- Each agent file is written as if addressed to a fresh Claude instance with no project context

**Shared state between steps:**
- `.task/BRIEF.md` — requirements gathered in the Brief step
- `.task/PLAN.md` — execution plan with phases and validations
- `.task/PHASE-N.md` — per-phase execution summary (commit hashes, deviations, notes)
- `.task/VERIFY.md` — verification results

The user runs `/clear` between steps to keep each step's context lean.

## Plugin metadata

- `.claude-plugin/plugin.json` — plugin name and description (consumed by Claude's plugin manager)
- `.claude-plugin/marketplace.json` — marketplace listing for `claude plugin marketplace add`
