# PAI-Orbit — Cursor adapter

This is a **lossy** build of PAI-Orbit for Cursor.

## What's here

- `.cursor/rules/*.mdc` — one rule file per PAI-Orbit mode (build, design, arch, etc.). Set `alwaysApply: false` so the agent picks them up by relevance, not unconditionally.
- `.cursor/rules/skills.mdc` — concatenated skills reference (Cursor has no skill system).
- `templates/` — project scaffolding templates, copied verbatim.

## What's lost vs the Claude Code plugin

- **No command system.** Modes cannot be invoked as `/build`, `/design`, etc. They become rule documents the agent reads.
- **No skill invocation.** Skills become reference documents only.
- **No agents.** Claude Code's named sub-agents (docs-writer, cross-repo-impact) have no Cursor analog.
- **No hooks.** PreToolUse/PostToolUse safety and lint hooks are dropped. Replicate with Cursor's own command-execution settings if needed.

## How to install

Copy this directory's `.cursor/` into your project root (or merge with existing `.cursor/`).
