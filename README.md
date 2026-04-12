# LSD — Little Shit Done

Lightweight feature planner and executor for Claude Code. Breaks features into 1-3 phases with isolated agent execution. Token-efficient: each step runs in a fresh context, communicating only through markdown files in `.task/`.

## Install

```bash
claude plugin install https://github.com/C0nanT/lsd-plugin
```

## Usage

```
/lsd [description]   # Gather requirements → writes .task/BRIEF.md
/lsd plan            # Create execution plan → writes .task/PLAN.md
/lsd exec <N>        # Execute phase N → writes .task/PHASE-N.md
/lsd verify          # Verify all phases → writes .task/VERIFY.md
/lsd clean           # Delete .task/ directory
```

### Typical workflow

```
/lsd add dark mode to the settings page
# answer a few questions
# /clear
/lsd plan
# review .task/PLAN.md, edit if needed
# /clear
/lsd exec 1
# review code changes
# /clear
/lsd verify
# /clear
/lsd clean
```

Each `/clear` between steps keeps context lean — the `.task/` directory is the only shared memory between steps.

## Why

Most tasks don't need a full GSD pipeline. LSD is for the small stuff: a new page, a form, a component, a small API endpoint. Plan once, execute in isolation, verify, done.

## Structure

```
skills/lsd/
├── SKILL.md          # Main skill routing logic
└── agents/
    ├── planner.md    # Creates PLAN.md from BRIEF.md
    ├── executor.md   # Executes a single phase
    └── verifier.md   # Verifies all phase validations
```
