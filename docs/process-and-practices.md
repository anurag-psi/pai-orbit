# Process & Practices

`modo` is a structured working methodology for software teams using Claude Code. This document explains why the methodology is designed the way it is, and how to get the most out of it.

---

## The core problem

Software teams lose context constantly. A feature gets half-designed in a conversation and then built from memory. A build session derails into a planning debate. Domain knowledge lives in someone's head and never gets written down. A data question gets answered with a guess.

`modo` imposes light discipline: **each slash command puts Claude into a distinct headspace with a defined output destination.** Switching is explicit. Outputs are saved to the docs directory. Nothing important lives only in a conversation window.

---

## The producer/consumer model

Every mode either produces or consumes:

| Mode | Produces | Consumes |
|------|----------|----------|
| `/domain` | `docs/domain/` | — |
| `/groom` | `docs/features/*/requirements.md` | `docs/domain/` |
| `/design` | `docs/features/*/design.md`, `docs/decisions/` | `docs/features/*/requirements.md`, `docs/domain/` |
| `/build` | Code, `docs/domain/product-capabilities.md` | All docs, task board |
| `/plan` | `docs/plans/`, board card moves | All docs, task board state |
| `/data` | `docs/reports/` | Database, `docs/domain/` |

`/plan` is the integrator — it consumes everything and decides what to work on next. It never produces domain knowledge, requirements, or designs.

The discipline: **switch modes when the headspace or output destination changes.** If you're in a build session and a design question surfaces that you can't answer in a sentence, stop and switch to `/design`. If you're in a planning session and realise a feature isn't groomed, stop and switch to `/groom`. Don't try to do both at once.

---

## Working style

**Lead functionally, go technical when you drive it.** In design and groom sessions, start with what the system does and for whom — not how it does it. Technical discussions are more productive when the functional intent is clear.

**Save everything.** Conversation context is ephemeral. A good discussion that doesn't end in a saved document is a conversation that will need to happen again. Every mode has a defined output destination; use it.

**Flag ambiguity, don't assume.** Requirements with hidden assumptions create build debt. Design decisions made without flagging uncertainty create architectural debt. When something is unclear, name it as an open question with an owner. "I don't know" is a better output than a confident guess.

**CLAUDE.md is the contract.** This file tells Claude everything project-specific: architecture, stack, conventions, key files, deployment. Keep it current. Stale CLAUDE.md leads to wrong assumptions that compound over time.

---

## How a session should flow

### Starting a build session

1. Open Claude Code in your project directory
2. Check the task board for the issue you're working on (or use `/agile-board` to find it)
3. Type `/build` — Claude reads CLAUDE.md and any relevant docs before writing a line of code
4. Claude confirms what it understood about the task before starting

### Starting a feature

1. `/groom` — clarify requirements with the domain expert and product owner; output to `docs/features/<feature>/requirements.md`
2. `/design` — if non-trivial, design the solution; output to `docs/features/<feature>/design.md` and/or `docs/decisions/`
3. `/agile-board` — create the task board item if it doesn't exist yet
4. `/build` — implement; reference the docs from steps 1–2
5. `/git` — commit and push

### Starting a planning session

1. `/plan` — read current board state, read `docs/domain/product-capabilities.md`, triage and sequence
2. Board card moves happen through `/agile-board` or the browser
3. Save non-trivial planning notes to `docs/plans/<topic>-<date>.md`

### Exploring data

1. `/data` — run queries with intent declared upfront; output to `docs/reports/`
2. If findings reveal a domain interpretation question: switch to `/domain`
3. If findings reveal a feature need: switch to `/groom`

---

## Docs structure

```
docs/
├── domain/        Expert knowledge — the science or logic behind the product
├── features/      One folder per feature: requirements.md, design.md
├── decisions/     Architecture Decision Records (ADRs)
├── ops/           Human-owned operational files — Claude does not modify these unless asked
├── backlog/       Feature parking lot — promoted to task board by human decision only
├── wip/           Ephemeral session captures — not a task board
├── plans/         Planning and prioritisation notes
└── reports/       Data analysis findings
```

**`docs/ops/` is human-owned.** Files here (field actions, pending decisions, operational runbooks) are written and maintained by people, not by Claude. Claude reads them for context but does not modify them without explicit instruction.

**`docs/backlog/feature-ideas.md` is a parking lot**, not a task board. Ideas here are promoted to the task board by a human. Claude does not move entries autonomously.

---

## Docs home: local vs remote platform

`modo` uses a **local-first** approach for docs. All modes write markdown to a local `docs/` directory. If your team uses Confluence or Notion, the `docs-writer` agent handles outbound sync.

**Why local-first?**
- Every mode works the same way regardless of docs home — no conditional MCP logic
- Works offline
- Diff and review before publishing
- Claude always reads local — one source of truth for its context

**Why outbound-only sync?**
Direct edits on Confluence or Notion don't get pulled back automatically. The intended flow is: Claude writes locally → team publishes to the remote platform. If someone edits Confluence directly and that change needs to reach Claude, feed it back explicitly in a session. Bidirectional sync creates conflict resolution problems that aren't worth the complexity for most teams.

---

## Memory

Claude Code's memory system (`.claude/projects/.../memory/`) allows Claude to accumulate project context across conversations. `modo` uses memory for:

- Team context: who people are, roles, preferences
- Project state: what was decided, what is in progress
- Feedback: corrections and working preferences

Memory supplements CLAUDE.md — it holds evolving context that doesn't belong in the stable project spec. CLAUDE.md is architecture; memory is living context.

---

## Operational skills

Beyond the core modes, `modo` projects grow domain-specific skills over time: skills that encode a recurring multi-step procedure (data backfill, seed data insertion, a structured review of domain-specific entities). These live in `.claude/skills/` in the project repo.

Run `/suggest-skills` after a few weeks of work to discover what recurring patterns are worth encoding as skills.

---

## What modo is not

- **A project management tool.** It helps Claude interact with your task board — it doesn't replace it.
- **A documentation generator.** It saves real outputs from real sessions — it doesn't generate docs from thin air.
- **A replacement for human judgment.** Modes and skills constrain Claude's behaviour; they don't replace the engineer or domain expert's decisions. Flag ambiguity, don't paper over it.
