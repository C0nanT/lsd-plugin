# LSD — Little Shit Done

> Lightweight Spec-Driven Development for Claude Code. Plan small, execute in isolation, ship fast.

## Why

If you use [GSD (Get Shit Done)](https://github.com/gsd-build/get-shit-done), you know it's a bazooka. Great for killing the main villain — but sometimes you just need to kill an ant.

GSD is the Hulk: solves anything by brute force, doesn't care about the cost, laser-focused on the big problem. LSD is Hawkeye: small, fast, plans well, optimized, and perfect for taking out the minor enemies without blowing up the budget.

On Claude Code's Pro plan, GSD can burn through your tokens before finishing a task. LSD was built to solve that — same **Spec-Driven Development** discipline (BRIEF → PLAN → EXEC → VERIFY), but every step runs in a fresh context and only passes what's strictly necessary forward through `.task/` markdown files.

Use LSD for the everyday stuff: a new page, a form, a component, a small API endpoint. Use GSD when the task actually deserves a bazooka.

> LSD is under active development. Planned improvements include parallelism in some phases — without significantly increasing token usage.

---

## Install

**Via Claude plugin manager:**
```bash
claude plugin install https://github.com/C0nanT/lsd-plugin
```

**Via git clone:**
```bash
git clone https://github.com/C0nanT/lsd-plugin
cp -r lsd-plugin/skills/lsd ~/.claude/skills/lsd
```

---

## Usage

| Command | What it does | Output |
|---------|-------------|--------|
| `/lsd [description]` | Gather requirements | `.task/BRIEF.md` |
| `/lsd plan` | Create execution plan | `.task/PLAN.md` |
| `/lsd exec <N>` | Execute phase N | `.task/PHASE-N.md` |
| `/lsd verify` | Verify all phases | `.task/VERIFY.md` |
| `/lsd clean` | Delete `.task/` | — |

### Workflow

```
/lsd add dark mode to the settings page
# answer a few questions → BRIEF.md is saved
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

Each `/clear` between steps keeps context lean — `.task/` is the only shared memory between steps.

---

## Contributing

Clone the repo and symlink the skill so edits reflect immediately in Claude Code:

```bash
git clone https://github.com/C0nanT/lsd-plugin
rm -rf ~/.claude/skills/lsd
ln -s $(pwd)/lsd-plugin/skills/lsd ~/.claude/skills/lsd
```

Edit the files in `skills/lsd/`, test with `/lsd` in Claude Code, then open a PR.

To publish your own fork as a plugin, push and run:

```bash
claude plugin marketplace add https://github.com/YOUR_USERNAME/lsd-plugin
claude plugin install lsd@YOUR_USERNAME-plugins
```

---

## Structure

```
skills/lsd/
├── SKILL.md          # Main skill routing logic
└── agents/
    ├── planner.md    # Creates PLAN.md from BRIEF.md
    ├── executor.md   # Executes a single phase
    └── verifier.md   # Verifies all phase validations
```
