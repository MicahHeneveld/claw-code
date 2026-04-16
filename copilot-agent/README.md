# copilot-agent

Portable Copilot GPT-Codex agent configuration, ported from the claw-code agent system.

Drop this folder into any repository to get the same instruction hierarchy, skill catalog,
agent role playbooks, and config defaults that claw-code uses — without needing the
`claw` CLI binary.

## How to use

### 1. Copy this folder into your target repo

```bash
cp -r copilot-agent/ /path/to/your-repo/
```

### 2. Wire up project-level instructions

Copilot agent mode reads instructions from `AGENTS.md` at the repository root (and ancestor
directories). Copy or symlink the team baseline:

```bash
cp copilot-agent/instructions/AGENTS.md AGENTS.md
```

Edit `AGENTS.md` to reflect the target repo's stack, verification commands, and working
agreement. The file is intentionally structured to match the sections Claw's `/init`
template produces.

### 3. Add a local override (optional)

For machine-local or personal preferences that should not be committed:

```bash
cp copilot-agent/instructions/local.md AGENTS.local.md
echo "AGENTS.local.md" >> .gitignore
```

### 4. Install skills

Each subdirectory under `skills/` is a self-contained skill playbook. Invoke them by
mentioning the skill name in your Copilot prompt:

```
Run the "review" skill on my last set of changes.
```

Skills translate directly from Claw's `SKILL.md` format. The folder layout is:

```
skills/
  <skill-name>/
    SKILL.md        # frontmatter (name, description) + prompt body
```

### 5. Use agent role definitions

The `agents/` folder contains role-scoped instruction files for multi-step tasks.
Reference a role when starting a long-running task:

```
Act as the Architect role (see copilot-agent/agents/architect.md) and design
the migration plan for X.
```

### 6. Apply config defaults

`config/settings.json` is a drop-in template for `.claw.json` / `.claw/settings.json`
if you are also using the `claw` CLI. For pure Copilot usage it documents the intended
permission and hook defaults as comments.

---

## Folder map

| Path | Purpose |
|---|---|
| `instructions/AGENTS.md` | Project-level system instructions (committed) |
| `instructions/local.md` | Local override template (gitignored per user) |
| `skills/review/SKILL.md` | Code-review skill playbook |
| `skills/plan/SKILL.md` | Planning / ultraplan skill playbook |
| `skills/security-review/SKILL.md` | Security review skill playbook |
| `agents/architect.md` | Architect role — design and decomposition |
| `agents/implementer.md` | Implementer role — tightly-scoped code changes |
| `agents/reviewer.md` | Reviewer role — quality gate and feedback |
| `config/settings.json` | Config defaults template |
| `config/hooks.md` | Hook pipeline documentation |

## Relationship to claw-code

| claw-code concept | Copilot equivalent in this folder |
|---|---|
| `CLAUDE.md` discovered by `prompt.rs` | `AGENTS.md` at repo root |
| `.claw/instructions.md` | `instructions/AGENTS.md` (same content, different path) |
| `.claw/agents/<name>.toml` | `agents/<role>.md` |
| `.claw/skills/<name>/SKILL.md` | `skills/<name>/SKILL.md` (identical format) |
| `.claw.json` / `.claw/settings.json` | `config/settings.json` |
| Pre/PostToolUse hooks | `config/hooks.md` + CI workflow steps |
| OmX planning loop | `skills/plan/SKILL.md` + PR workflow |
| clawhip notification routing | GitHub notification settings + PR assignments |
