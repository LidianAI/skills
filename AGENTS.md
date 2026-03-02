# AGENTS.md - Lidian Skills

Guidelines for agents editing the `skills/` codebase.

## Core Standards

- Keep skill logic and instructions simple, clear, and maintainable.
- Prefer explicit wording over clever or ambiguous phrasing.
- Avoid over-complex workflows unless clearly required.

## Required Simplification Pass

After any feature/change to skills, always do a simplification pass before finishing:

- remove duplicate or redundant instructions
- tighten long/unclear wording
- reduce unnecessary decision branches
- ensure commands/examples are direct and consistent

If complexity is unavoidable, explain it briefly in the relevant skill docs.

## Tooling Rules

- Use `bun`/`bunx` where package tooling is needed.
- Keep examples aligned with current CLI/API behavior.
