# agent-blueprint — AI Agent Ruleset

<!--
  HOW THIS FILE WORKS
  ════════════════════════════════════════════════════════════
  • Lines 1–200 are loaded at EVERY session startup — critical context lives here.
  • Lines 201+ are loaded on demand — reference and detail content lives there.
  • Run /agents-init after adding new skills to skills/ to sync the tables below.
  • AGENTS.md is a universal format: Claude Code, OpenAI Codex, GitHub Copilot.
-->

---

> **Skills Reference** — load these on demand for detailed patterns:
> - [`agents-init`](skills/agents-init/SKILL.md) — Restructures AGENTS.md with skills reference table, auto-invoke table, and 200-line budget optimization. Run after /init or after adding new skills.

---

### Auto-invoke Skills

When performing these actions, ALWAYS load the corresponding skill FIRST:

| Action | Skill |
|--------|-------|
| Adding new skills to the repo | `agents-init` |
| After running /init on a new project | `agents-init` |
| Restructuring or enhancing AGENTS.md | `agents-init` |
| Syncing skills table after adding skills | `agents-init` |

---

## CRITICAL RULES — NON-NEGOTIABLE

### AGENTS.md Structure
- ALWAYS: Keep critical content (skills reference, auto-invoke, critical rules) within the first 200 lines
- ALWAYS: Run `/agents-init` after adding new skills to sync the tables — never edit them manually
- NEVER: Put Commands, Tech Stack, or Project Structure before line 200 — they load on demand
- NEVER: Omit a section — use placeholders if content is not yet defined

### Skills Authoring
- ALWAYS: Add `name` and `description` to every SKILL.md — both are required by OpenCode
- ALWAYS: Keep `description` specific enough for the agent to choose the skill correctly (max 1024 chars)
- ALWAYS: Keep each SKILL.md under 500 lines — move detailed reference to supporting files
- NEVER: Write a skill name that doesn't match its directory name — OpenCode will reject it
- NEVER: Use consecutive hyphens or start/end with `-` in skill names — invalid per spec

### Skills Discovery (agents-init behavior)
- ALWAYS: Scan `skills/*/SKILL.md` to auto-discover skills — do not hardcode skill names
- ALWAYS: Extract trigger phrases from the `description` frontmatter field to build the auto-invoke table
- NEVER: Invent content not present in the original AGENTS.md — use placeholders for missing sections

---

<!-- ══════════════════ 200-LINE BUDGET BOUNDARY ══════════════════ -->
<!-- Content below this line is loaded ON DEMAND — not at startup. -->
<!-- ══════════════════════════════════════════════════════════════ -->

---

## Tech Stack

This is a skill versioning repository — no application stack.

- Markdown — skill and documentation format
- YAML frontmatter — skill configuration (OpenCode + Claude Code compatible)

---

## Project Structure

```
agent-blueprint/
├── AGENTS.md                         # Universal AI agent ruleset (this file)
├── AGENTS-changes.md                 # Design decisions and docs for agents-init
├── README.md                         # Repository overview and deploy instructions
│
└── skills/
    └── agents-init/
        ├── SKILL.md                  # Skill entrypoint — source of truth
        └── template.md               # Canonical AGENTS.md template with comments
```

Skills live in `skills/<name>/SKILL.md`. This is the source of truth — deploy from here
to wherever you need the skill (see README.md for deploy instructions per tool).

---

## Commands

```bash
# Deploy skill globally for OpenCode
cp -r skills/agents-init ~/.agents/skills/

# Deploy skill globally for Claude Code
cp -r skills/agents-init ~/.claude/skills/

# Or symlink for live sync with the repo
ln -sf "$(pwd)/skills/agents-init" ~/.agents/skills/agents-init
```

---

## QA Checklist

When modifying this repo, verify:

- [ ] AGENTS.md critical content (skills + auto-invoke + rules) fits within 200 lines
- [ ] `skills/agents-init/SKILL.md` has valid `name` and `description` frontmatter
- [ ] Skill `name` matches the directory name exactly
- [ ] `template.md` reflects any structural changes to the AGENTS.md format

---

## Naming Conventions

| Entity | Pattern | Example |
|--------|---------|---------|
| Skill directory | kebab-case | `skills/agents-init/` |
| Skill file | always `SKILL.md` | `skills/agents-init/SKILL.md` |
| Supporting files | kebab-case `.md` | `template.md`, `examples.md` |
| Skill `name` frontmatter | must match directory | `name: agents-init` |
