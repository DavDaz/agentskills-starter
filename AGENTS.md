# agent-blueprint — AI Agent Ruleset

<!--
  HOW THIS FILE WORKS
  ════════════════════════════════════════════════════════════
  • Lines 1–200 are loaded at EVERY session startup — critical context lives here.
  • Lines 201+ are loaded on demand — reference and detail content lives there.
  • Run /init-agents after adding new skills to skills/ to sync the tables.
  • AGENTS.md is a universal format: Claude Code, OpenAI Codex, GitHub Copilot, OpenCode.
-->

---

> **Skills Reference** — load these on demand for detailed patterns:
> - [`init-agents`](skills/init-agents/SKILL.md) — Generates AGENTS.md / CLAUDE.md for any project. Trigger: initializing or creating AGENTS.md.
> - [`skill-creator`](skills/skill-creator/SKILL.md) — Guide to create new skills correctly. Trigger: creating a new skill in this repository.
> - [`skill-sync`](skills/skill-sync/SKILL.md) — Syncs Auto-invoke tables from skill metadata. Trigger: after creating or modifying a skill.

---

### Auto-invoke Skills

When performing these actions, ALWAYS load the corresponding skill FIRST:

| Action | Skill |
|--------|-------|
| After creating/modifying a skill | `skill-sync` |
| Creating a new skill in this repository | `skill-creator` |
| Initializing or creating AGENTS.md / CLAUDE.md for a project | `init-agents` |
| Regenerate AGENTS.md Auto-invoke tables | `skill-sync` |
| Troubleshoot why a skill is missing from Auto-invoke | `skill-sync` |

---

## CRITICAL RULES — NON-NEGOTIABLE

### AGENTS.md Structure
- ALWAYS: Keep Skills Reference + Auto-invoke + Critical Rules within the first 200 lines
- ALWAYS: Run `skill-sync` after adding or modifying skills — never edit Auto-invoke manually
- NEVER: Put Commands or Project Structure before line 200 — they load on demand
- NEVER: Omit a section — use `TODO:` placeholders if content is not yet defined

### Skills Authoring
- ALWAYS: Add `name` and `description` frontmatter to every SKILL.md
- ALWAYS: Add `metadata.scope` and `metadata.auto_invoke` so skill-sync works correctly
- ALWAYS: Keep skill name matching its directory name exactly
- ALWAYS: Keep each SKILL.md under 500 lines — move detail to supporting files in `assets/`
- NEVER: Use consecutive hyphens or start/end with `-` in skill names

### Skills Discovery
- ALWAYS: Scan `skills/*/SKILL.md` to auto-discover — never hardcode skill names
- NEVER: Invent content not present in the original repo — use `TODO:` for missing sections

---

<!-- ══════════════════ 200-LINE BUDGET BOUNDARY ══════════════════ -->
<!-- Content below this line is loaded ON DEMAND — not at startup. -->
<!-- ══════════════════════════════════════════════════════════════ -->

---

## Tech Stack

Documentation-only repository — no application stack.

- Markdown — skill and documentation format
- YAML frontmatter — skill configuration (Claude Code + OpenCode + Codex compatible)
- Bash — setup and sync scripts

---

## Project Structure

```
agent-blueprint/
├── AGENTS.md                              # Universal AI agent ruleset (this file)
├── README.md                              # Quickstart, diagrams, install instructions
│
└── skills/
    ├── setup.sh                           # Distributes skills to each AI tool
    ├── setup_test.sh                      # Tests for setup.sh
    ├── init-agents/
    │   ├── SKILL.md                       # Generates AGENTS.md for any project
    │   └── assets/
    │       └── AGENTS-TEMPLATE.md         # Template with placeholders
    ├── skill-creator/
    │   ├── SKILL.md                       # Guide to create new skills correctly
    │   └── assets/
    │       └── SKILL-TEMPLATE.md          # Template for new skills
    └── skill-sync/
        ├── SKILL.md                       # Syncs Auto-invoke from skill metadata
        └── assets/
            └── sync.sh                    # Script that updates AGENTS.md tables
```

---

## Commands

```bash
# Distribute skills to all AI tools (run from the project you want to configure)
./skills/setup.sh --all

# Configure only Claude Code
./skills/setup.sh --claude

# Sync Auto-invoke tables from skill metadata
./skills/skill-sync/assets/sync.sh

# Dry run — preview what sync.sh would change
./skills/skill-sync/assets/sync.sh --dry-run

# Run setup script tests
./skills/setup_test.sh
```

---

## QA Checklist

When modifying this repo, verify:

- [ ] Critical content (skills + auto-invoke + rules) fits within 200 lines in AGENTS.md
- [ ] Every SKILL.md has valid `name`, `description`, `metadata.scope`, `metadata.auto_invoke`
- [ ] Skill `name` matches its directory name exactly
- [ ] `skill-sync` runs clean with no errors after changes
- [ ] `setup.sh --all` runs clean with no errors
