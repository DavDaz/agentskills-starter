# agents-init — Design Decisions & Documentation

This file documents the reasoning, design decisions, and usage patterns behind
the `agents-init` skill. It is the reference for anyone maintaining or extending this repo.

---

## What problem does this solve?

When you run `/init` (OpenCode or Claude Code), the tool generates an `AGENTS.md` with
useful raw content: project overview, tech stack, commands, conventions. But it doesn't
know about your skills directory, and it doesn't organize the file for the 200-line
startup budget that the AI agent runtime enforces.

`agents-init` bridges that gap: it takes the raw `/init` output and restructures it
into a high-quality, skills-aware, budget-optimized `AGENTS.md`.

---

## The 200-line startup budget — why it matters

AI agent runtimes (Claude Code, OpenCode, Codex) load only the **first 200 lines** of
`AGENTS.md` at session startup. Everything after line 200 is loaded on demand, which
means the agent has to explicitly read the file to get that content — it's not free context.

If you put your skills table at line 210, Claude doesn't know your skills exist until you
tell it to read the file. If your Critical Rules are at line 350, those rules are invisible
at startup. This is a quality issue, not a preference issue.

**Target allocation within 200 lines:**

| Block | Lines | Why here |
|-------|-------|----------|
| Header + usage comment | 1–8 | Orientation — tells the agent how to read this file |
| Skills Reference | 9–55 | The agent needs to know what tools are available at all times |
| Auto-invoke table | 56–130 | Without this, the agent won't load skills proactively |
| Critical Rules | 131–200 | Non-negotiables must be in every session context |

Tech Stack, Project Structure, Commands, QA Checklist, and Naming Conventions go after
line 200. The agent loads them on demand when it's working on relevant tasks.

---

## Skill location — one source of truth

This repo has a single location for skills: `skills/<name>/SKILL.md`.

The skill is deployed from here to wherever it needs to run. The repo is version control,
not the runtime location. Deploy it to:

| Tool | Global path |
|------|------------|
| OpenCode | `~/.agents/skills/agents-init/` |
| Claude Code | `~/.claude/skills/agents-init/` |
| OpenCode native | `~/.config/opencode/skills/agents-init/` |

Use a symlink if you want edits in the repo to reflect immediately without re-deploying:

```bash
ln -sf "$(pwd)/skills/agents-init" ~/.agents/skills/agents-init
```

When the skill is deployed globally, it runs in the context of whatever project the user
has open — it reads `AGENTS.md` and scans `skills/*/SKILL.md` relative to the project root,
not relative to this repo.

---

## How agents-init discovers skills

The skill uses `Glob` to find `skills/*/SKILL.md`. For each file found:

1. It reads the YAML frontmatter for `name` and `description`
2. It extracts trigger phrases from `description` using these patterns:
   - `"Use when [phrase]"` → phrase becomes a table row
   - `"Trigger: [phrase]"` / `"Triggers: [phrase]"` → phrase becomes a table row
   - Action verb phrases (`Creating X`, `Modifying Y`, `Writing Z`)
3. It builds two outputs: the Skills Reference list and the Auto-invoke table

**This means skill authors must write good `description` fields.** A skill with a vague
description like `"Helps with TypeScript"` will produce a vague auto-invoke row.
A skill with `"Use when writing TypeScript interfaces, creating utility types, or working
with generics"` produces three useful auto-invoke rows.

---

## What agents-init preserves from /init

The skill reads the original AGENTS.md and extracts:

| Variable | Extracted from |
|----------|---------------|
| `project_name` | First `#` heading |
| `tech_stack` | Technology mentions, version numbers |
| `commands` | All bash code blocks |
| `project_structure` | Directory trees |
| `critical_rules` | ALWAYS/NEVER/MUST statements, rule sections |
| `naming_conventions` | Naming tables or patterns |
| `qa_checklist` | Checklist items |
| `other_content` | Anything else |

**Nothing is discarded.** The skill reorganizes content — it doesn't delete it.
If a section has no corresponding content in the original file, a placeholder is inserted.

---

## How to add a new project skill

1. Create the skill directory:
   ```bash
   mkdir -p skills/<skill-name>
   ```

2. Create `skills/<skill-name>/SKILL.md` with frontmatter:
   ```yaml
   ---
   name: skill-name
   description: >
     One paragraph describing what this skill does.
     Use when [trigger phrase 1], [trigger phrase 2].
     Triggers: creating X, modifying Y, writing Z.
   ---

   ## Instructions
   ...
   ```

3. Run `/agents-init` to sync the AGENTS.md tables.

That's it. The skill handles the table entries automatically.

---

## Design decisions

### Why `disable-model-invocation: true`?

`agents-init` modifies `AGENTS.md`. If Claude could invoke it automatically, it might
restructure your file mid-session because it "seemed relevant." That's a side effect
you always want to control explicitly. The user decides when to run it.

### Why `allowed-tools: Read, Write, Glob`?

Principle of least privilege. The skill only needs to:
- Read existing files (`Read`)
- Scan for skill files (`Glob`)
- Write the restructured `AGENTS.md` (`Write`)

Giving it `Bash` or `Edit` is unnecessary and creates surface area for mistakes.

### Why not use `.claude/rules/` for modularization?

`.claude/rules/` is a Claude Code feature for path-scoped rules (e.g., "only apply
these TypeScript rules when working on `.ts` files"). It's excellent for large monorepos.

This repo uses `skills/` instead because:
- The Prowler convention (`skills/`) is more universal (works with OpenCode, Codex, Copilot)
- Path-scoped rules are a Claude Code-specific feature; skills are tool-agnostic
- Skills are explicit (you choose when to load them) rather than implicit (auto-applied by path)

For a Claude Code-only project, combining `.claude/rules/` for path-scoped rules
AND `skills/` for on-demand domain knowledge is a valid advanced setup.

### Why AGENTS.md and not CLAUDE.md?

Claude Code natively reads `CLAUDE.md`. `AGENTS.md` is the OpenCode/Codex convention.
This repo targets universal compatibility: the same file works across tools.

If you are Claude Code-only, you can rename `AGENTS.md` to `CLAUDE.md` and everything
works identically. The skill operates on `AGENTS.md` by default but can be adapted.

---

## Extending agents-init

If you want the skill to handle additional sections (e.g., Decision Trees, API Conventions),
add them to `template.md` under the 200-line boundary comment, then update Step 5
in `SKILL.md` to include the new variable in the assembly order.

Keep the skill under 500 lines. If it grows beyond that, extract detailed reference
material into a separate file in `skills/agents-init/` and link to it from SKILL.md.
