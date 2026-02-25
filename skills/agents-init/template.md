# AGENTS.md — Canonical Template
# Used by the agents-init skill as the reference structure.
# Lines marked with [PLACEHOLDER] must be replaced with real content.
# Lines marked with [GENERATED] are auto-built by agents-init from skills/.
# Lines marked with [PRESERVE] come from the original /init output.
#
# ─────────────────────────────────────────────────────────────────
# DO NOT commit this file — it is an internal reference for the skill.
# ─────────────────────────────────────────────────────────────────

---

# [PRESERVE: project_name] — AI Agent Ruleset

<!--
  HOW THIS FILE WORKS
  ════════════════════════════════════════════════════════════
  • Lines 1–200 are loaded at EVERY session startup — critical context lives here.
  • Lines 201+ are loaded on demand — reference and detail content lives there.
  • Run /agents-init after adding new skills to skills/ to sync the tables.
  • AGENTS.md is a universal format: Claude Code, OpenAI Codex, GitHub Copilot.
-->

---

## [GENERATED: skills_reference]

<!-- FORMAT (≤8 skills): compact blockquote list — one line per skill -->
> **Skills Reference** — load these on demand for detailed patterns:
> - [`skill-name`](skills/skill-name/SKILL.md) — One-line description (max 80 chars)

<!-- FORMAT (>8 skills): table -->
<!--
## Available Skills

| Skill | Description | File |
|-------|-------------|------|
| `skill-name` | One-line description | [SKILL.md](skills/skill-name/SKILL.md) |
-->

<!-- PLACEHOLDER (no skills found):
> **Skills Reference** — no skills found in `skills/`.
> Add skills at `skills/<name>/SKILL.md` and run `/agents-init` to sync.
-->

---

### [GENERATED: auto_invoke_table]

<!-- Auto-invoke table is sorted alphabetically by Action column. -->
<!-- Every skill must appear at least once. Multiple rows per skill if multiple triggers. -->

When performing these actions, ALWAYS load the corresponding skill FIRST:

| Action | Skill |
|--------|-------|
| Trigger phrase extracted from skill description | `skill-name` |

<!-- PLACEHOLDER (no skills):
| _No skills registered yet — run /agents-init after adding skills_ | — |
-->

---

## [PRESERVE: critical_rules] — CRITICAL RULES — NON-NEGOTIABLE

<!--
  ALWAYS/NEVER statements that the agent must never violate.
  Group by domain. Keep concise — this section MUST fit within the 200-line budget.
  Format: ALWAYS: <rule> / NEVER: <rule>
-->

### [Domain — e.g. Models, API, Tests, Commits]
- ALWAYS: [rule]
- NEVER: [rule]

<!-- PLACEHOLDER (no rules in original /init output):
> No critical rules defined yet.
> Add ALWAYS/NEVER statements grouped by domain (e.g. Models, API, Tests).
> Example:
> ### API
> - ALWAYS: Validate all inputs before processing
> - NEVER: Return raw database errors to the client
-->

---

<!-- ══════════════════ 200-LINE BUDGET BOUNDARY ══════════════════ -->
<!-- Content below this line is loaded ON DEMAND — not at startup. -->
<!-- ══════════════════════════════════════════════════════════════ -->

---

## [PRESERVE: tech_stack] — Tech Stack

<!--
  Format: Name | Version | Purpose (one line per technology)
  Example:
  - TypeScript 5.4 — primary language
  - Node.js 20 — runtime
  - Vitest 1.6 — unit testing
-->

<!-- PLACEHOLDER:
> Tech stack not documented. Add one-liners: `Name | Version | Purpose`.
-->

---

## [PRESERVE: project_structure] — Project Structure

<!--
  Directory tree. Keep shallow (2–3 levels max). Annotate key directories.
  Example:
  ```
  project-root/
  ├── src/           # Application source
  │   ├── api/       # API layer
  │   └── core/      # Business logic
  └── tests/         # Test suites
  ```
-->

<!-- PLACEHOLDER:
> Project structure not documented. Add a directory tree with inline comments.
-->

---

## [PRESERVE: commands] — Commands

<!--
  Group by purpose. Include the exact commands — no paraphrasing.
  Example:
  ```bash
  # Development
  npm run dev

  # Testing
  npm run test
  npm run test:e2e

  # Linting
  npm run lint
  npm run lint:fix
  ```
-->

<!-- PLACEHOLDER:
> Commands not documented. Add build, test, lint, and run commands.
-->

---

## [PRESERVE: qa_checklist] — QA Checklist

<!--
  Checklist items the agent must verify before considering a task done.
  Example:
  - [ ] All tests pass (`npm run test`)
  - [ ] Linting passes with no errors
  - [ ] No regressions in existing behavior
  - [ ] New code has test coverage
-->

<!-- PLACEHOLDER:
> QA checklist not defined. Add checklist items the agent should verify.
-->

---

## [PRESERVE: naming_conventions] — Naming Conventions

<!--
  Table format: Entity | Pattern | Example
  Example:
  | Entity | Pattern | Example |
  |--------|---------|---------|
  | Component | PascalCase | `UserCard` |
  | Hook | camelCase with `use` prefix | `useAuthStore` |
  | File | kebab-case | `user-card.tsx` |
  | Constant | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
-->

<!-- PLACEHOLDER:
> Naming conventions not documented. Add a table: Entity | Pattern | Example.
-->
