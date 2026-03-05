# AGENTS.md

Instructions for AI coding agents working in this repository.

## Repository Overview

[1 párrafo: qué hace el proyecto, tipo (frontend / backend / fullstack / docs / monorepo), stack principal]

---

## Build / Lint / Test Commands

| Command | Script | Notes |
|---------|--------|-------|
| Install | `TODO:` | |
| Build | `TODO:` | |
| Lint | `TODO:` | MUST run before commit |
| Test (all) | `TODO:` | |
| Test (single) | `TODO:` | Preferir sobre test suite completo |
| Format | `TODO:` | |
| Type check | `TODO:` | MUST pass before PR |

---

## Architecture

[Diagrama ASCII de la estructura principal]

```
[root]/
├── [módulo-1]/   ← [responsabilidad]
├── [módulo-2]/   ← [responsabilidad]
└── [módulo-3]/   ← [responsabilidad]
```

### Key Mapping

| Domain | Files |
|--------|-------|
| [dominio] | `[path/to/files]` |

---

## Navigation Protocol (MANDATORY)

### Step 1: [Punto de entrada]

Always start at `[archivo índice o entry point]` to understand the structure.

### Step 2: Go to the relevant file

Go directly to the file identified. **DO NOT** read all files in a directory.

### Step 3: Cross-reference only when needed

Only read `[archivo de referencia]` if asked about [tipo de consulta específica].

---

## Style Guide

### Naming Conventions

- **Files**: [kebab-case / camelCase / snake_case] — e.g. `[example.ts]`
- **Components**: [PascalCase] — e.g. `[UserProfile.tsx]`
- **Functions**: [camelCase] — e.g. `[getUserById]`
- **Constants**: [SCREAMING_SNAKE_CASE] — e.g. `[MAX_RETRIES]`

### Code Conventions

- [convención concreta y verificable]
- [convención concreta y verificable]

---

## Verification

- After any change, run `[test command]` — ALL tests must pass
- After API changes, run `[validation command]`
- NEVER commit if tests are red
- NEVER skip type checking

---

## What NOT To Do

1. **Do NOT [anti-patrón 1]** — [por qué]
2. **Do NOT [anti-patrón 2]** — [por qué]
3. **Do NOT [anti-patrón 3]** — [por qué]
4. **Do NOT duplicate content** — summarize and link instead
5. **Do NOT invent [reglas/paths/commands]** — if unsure, say so explicitly

---

## Sources of Truth

| Domain | Source | Path |
|--------|--------|------|
| [dominio] | [descripción] | `[path/to/file]` |

---

## AI Skills

Use these skills for detailed patterns on-demand:

| Skill | Description | Path |
|-------|-------------|------|
| `[skill-name]` | [descripción] | [SKILL.md](skills/[skill-name]/SKILL.md) |

### Auto-invoke Skills

When performing these actions, ALWAYS invoke the corresponding skill FIRST:

| Action | Skill |
|--------|-------|
| [acción que dispara la skill] | `[skill-name]` |
