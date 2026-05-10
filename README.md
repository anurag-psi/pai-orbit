# modo

A structured developer methodology harness for Claude Code.

`modo` gives your project a shared vocabulary for how work gets done — distinct modes for building, designing, planning, and exploring data; operational skills for git, task management, and deployment; and a first-time setup that generates everything project-specific from a short conversation.

## What it is

Software teams waste context constantly: half-designed features get built, build sessions derail into planning debates, agronomic (or domain) questions get answered with guesses. `modo` imposes a light discipline: **each slash command puts Claude into a distinct headspace with a defined output destination.** Switching is explicit. Outputs are saved. Nothing important lives only in a conversation.

```
/build   → implementation session — reads docs, checks task board, ships
/design  → technical trade-offs — produces docs/decisions/ and docs/features/*/design.md
/groom   → feature requirements — produces docs/features/*/requirements.md
/plan    → roadmap and prioritisation — consumes docs, moves board cards
/data    → data exploration — produces docs/reports/
/domain  → domain knowledge — produces docs/domain/
/git     → commit, branch, PR — reads project branching model
/agile-board → task creation, card movement, team assignment
/deploy  → guided deployment with preflight and verification
```

## Install

```bash
# From the official marketplace
/plugin install modo@claude-plugins-official

# From the curiosityarc marketplace
/plugin marketplace add curiosityarc/modo
/plugin install modo@curiosityarc
```

## First run

After installing, run `/setup` in your project directory. It will:

1. Discover your repo structure and tech stack
2. Ask a short set of questions (task board, branching model, deployment, docs home, team)
3. Generate `.claude/modo-config.md`, `.claude/team.md`, a `CLAUDE.md` stub, stack-specific agents, and a `docs/` scaffold
4. Tell you exactly what to fill in by hand

Re-run `/setup` anytime the stack or team changes significantly.

## Docs

- [Process & Practices](docs/process-and-practices.md) — the methodology: why modes, working style, how sessions should flow
- [Capabilities](docs/capabilities.md) — reference for every mode, skill, and agent
- [Getting Started](docs/getting-started.md) — installation, first `/setup` walkthrough, first session

## Philosophy

**Producer/consumer.** `/domain` produces science. `/groom` produces requirements. `/design` produces architecture. `/build` produces code. `/plan` consumes all of the above to decide what to work on next. Switch modes when the headspace or output destination changes.

**Local-first docs.** All modes write markdown locally. If your team uses Confluence or Notion, `docs-writer` handles outbound sync. Local is Claude's working copy; the remote platform is the published surface. Edits should flow outward, not inward — bidirectional sync creates conflicts that are hard to resolve cleanly.

**Config over baked-in.** Modes contain methodology, not project specifics. Board URLs, branch naming conventions, deployment targets, and team handles all live in `.claude/modo-config.md` and `.claude/team.md`. `/setup` generates those; the modes read them.

## License

MIT
