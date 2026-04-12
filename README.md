# LSD — Little Shit Done

Lightweight feature planner and executor for Claude Code. Breaks features into 1-3 phases with isolated agent execution. Token-efficient: each step runs in a fresh context, communicating only through markdown files in `.task/`.

## Install

**Via Claude plugin manager (recomendado):**
```bash
claude plugin install https://github.com/C0nanT/lsd-plugin
```

**Via git clone (manual):**
```bash
git clone https://github.com/C0nanT/lsd-plugin
cp -r lsd-plugin/skills/lsd ~/.claude/skills/lsd
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

If you use [GSD (Get Shit Done)](https://github.com/mozartelio/gsd), you know it's a bazooka. Great for killing the main villain — but sometimes you just need to kill an ant.

GSD is the Hulk: solves anything by brute force, doesn't care about the cost, laser-focused on the big problem. LSD is Hawkeye: small, fast, plans well, optimized, and perfect for taking out the minor enemies without blowing up the budget.

On Claude Code's Pro plan, GSD can burn through your tokens before finishing a task. LSD was built to solve that — same **Spec-Driven Development** discipline (BRIEF → PLAN → EXEC → VERIFY), but every step runs in a fresh context and only passes what's strictly necessary forward through `.task/` markdown files.

Use LSD for the everyday stuff: a new page, a form, a component, a small API endpoint. Use GSD when the task actually deserves a bazooka.

> LSD is under active development. Planned improvements include parallelism in some phases — without significantly increasing token usage.

## Structure

```
skills/lsd/
├── SKILL.md          # Main skill routing logic
└── agents/
    ├── planner.md    # Creates PLAN.md from BRIEF.md
    ├── executor.md   # Executes a single phase
    └── verifier.md   # Verifies all phase validations
```
