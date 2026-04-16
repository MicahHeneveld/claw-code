# AGENTS.md

This file provides instructions to Copilot GPT-Codex agent mode when working with code
in this repository.  Edit the sections below to reflect the actual stack, commands, and
conventions of your project.

---

## Detected stack

- Languages: <!-- e.g. Rust, Python, TypeScript -->
- Frameworks: <!-- e.g. Next.js, React, NestJS, or "none detected" -->

## Verification

Run these commands before shipping any change:

```bash
# Replace with the repo's real commands
cargo fmt
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
```

- `src/` and `tests/` are both present; update both surfaces together when behavior changes.

## Repository shape

- `src/` — primary source tree
- `tests/` — validation surfaces; review alongside code changes
- `docs/` — documentation; update when public APIs or behaviour change

## Working agreement

- Prefer small, reviewable changes. Do not bundle unrelated fixes in a single PR.
- Read relevant code before changing it; keep changes tightly scoped to the request.
- Do not add speculative abstractions, compatibility shims, or unrelated cleanup.
- Do not create files unless they are required to complete the task.
- If an approach fails, diagnose the failure before switching tactics.
- Be careful not to introduce security vulnerabilities (command injection, XSS, SQL injection).
- Report outcomes faithfully: if verification fails or was not run, say so explicitly.
- Actions that affect shared systems, publish state, or delete data require explicit user
  authorisation or a durable workspace instruction before proceeding.

## Instruction file precedence

Copilot resolves instruction files from the innermost directory outward:

1. `AGENTS.md` at repo root (this file) — committed team baseline
2. `AGENTS.local.md` at repo root — machine-local overrides (gitignored)
3. Subdirectory `AGENTS.md` files — module-level guidance

Later (more specific) files take precedence over earlier (more general) ones.

## Tool and permission expectations

- File reads and workspace-local edits: allowed freely.
- Bash commands that modify shared infrastructure, publish artefacts, or destroy data:
  confirm with the user before running.
- Network requests outside the repo: flag before issuing unless the task explicitly asks
  for them.
- When in doubt, prefer the more conservative action and ask.
