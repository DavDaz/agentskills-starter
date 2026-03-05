---
name: skill-creator
description: >
  Crea nuevas skills para este repo siguiendo la estructura local del DGI.
  Trigger: When user asks to create a new skill, add agent instructions, or document patterns for AI.
version: "1.0"
metadata:
  scope: [root]
  auto_invoke: "Creating a new skill in this repository"
---

# Skill: skill-creator (DGI local)

## Cuándo usar esta skill

- El usuario pide crear una skill nueva
- Se quiere documentar un patrón repetible para el agente
- Se necesita guiar al agente en un flujo complejo

---

## Estructura de una skill

```
skills/{skill-name}/
├── SKILL.md              # Requerido — archivo principal
└── assets/               # Opcional — templates, schemas, ejemplos
    └── SKILL-TEMPLATE.md
```

---

## Template del SKILL.md

```markdown
---
name: {skill-name}
description: >
  {Descripción en una línea}.
  Trigger: {Cuándo debe cargarse esta skill}.
version: "1.0"
metadata:
  scope: [root]
  auto_invoke: "{Acción que dispara esta skill}"
---

## Cuándo usar

- {Condición 1}
- {Condición 2}

## Patrones críticos

{Las reglas más importantes}

## Comandos

```bash
{comando}  # {descripción}
```
```

---

## Campos del frontmatter

| Campo | Requerido | Descripción |
|-------|-----------|-------------|
| `name` | Sí | Identificador (lowercase, guiones) |
| `description` | Sí | Qué hace + trigger en un bloque |
| `version` | Sí | Versión semántica como string |
| `metadata.scope` | Sí | Siempre `[root]` en este repo |
| `metadata.auto_invoke` | Sí | Acción que activa el auto-invoke en AGENTS.md |

> **`scope` y `auto_invoke` son obligatorios** para que `skill-sync` registre la skill en la tabla de Auto-invoke del `AGENTS.md`.

---

## Checklist al crear una skill

- [ ] La skill no existe ya (verificar `skills/`)
- [ ] El nombre sigue la convención (lowercase, guiones)
- [ ] El frontmatter tiene `scope: [root]` y `auto_invoke`
- [ ] El contenido es claro y accionable
- [ ] Correr `./skills/skill-sync/assets/sync.sh` al terminar

---

## Convenciones de nombres

| Tipo | Patrón | Ejemplo |
|------|--------|---------|
| Skill genérica | `{tema}` | `dgi`, `skill-sync` |
| Skill de flujo | `{acción}-{objeto}` | `skill-creator` |

---

## Después de crear la skill

Correr el sync para actualizar `AGENTS.md` automáticamente:

```bash
./skills/skill-sync/assets/sync.sh
```

El setup.sh **no es necesario** volver a correrlo — el symlink ya apunta a toda la carpeta `skills/`.
