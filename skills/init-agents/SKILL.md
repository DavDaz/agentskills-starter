---
name: init-agents
description: >
  Genera un AGENTS.md (o CLAUDE.md) completo para cualquier proyecto analizando
  su estructura, stack y skills disponibles.
  Trigger: Cuando el usuario quiera inicializar o crear un AGENTS.md / CLAUDE.md
  en un proyecto nuevo o existente.
version: "1.0"
metadata:
  scope: [root]
  auto_invoke: "Initializing or creating AGENTS.md / CLAUDE.md for a project"
allowed-tools: Read, Glob, Grep, Bash, Write
---

# Skill: init-agents

## Cuándo usar

- El usuario quiere crear un `AGENTS.md` o `CLAUDE.md` desde cero
- El proyecto no tiene instrucciones para agentes de IA
- El usuario quiere mejorar un `AGENTS.md` existente con skills y auto-invoke
- El usuario corre `/init-agents` explícitamente

## Protocolo de ejecución (seguir en orden)

### Paso 1: Analizar el repositorio

Explorar el repo para entender:

```bash
# Estructura raíz
ls -la

# Stack detection
cat package.json 2>/dev/null || cat pyproject.toml 2>/dev/null || cat requirements.txt 2>/dev/null || cat go.mod 2>/dev/null

# Comandos disponibles
cat Makefile 2>/dev/null || cat package.json | jq '.scripts' 2>/dev/null

# Si ya existe AGENTS.md o CLAUDE.md
cat AGENTS.md 2>/dev/null || cat CLAUDE.md 2>/dev/null
```

Detectar:
- **Tipo de proyecto**: docs-only / frontend / backend / fullstack / monorepo
- **Stack**: lenguajes, frameworks, herramientas de build/test/lint
- **Comandos reales**: build, test, lint, format, typecheck
- **Estructura de carpetas**: módulos principales y sus responsabilidades
- **Fuentes de verdad**: schemas, specs, docs canónicas

### Paso 2: Detectar skills relevantes

Buscar skills disponibles en el proyecto y en el usuario:

```bash
# Skills del proyecto
ls .claude/skills/ 2>/dev/null

# Skills del usuario (globales)
ls ~/.claude/skills/ 2>/dev/null
```

Mapear stack detectado → skills disponibles usando esta tabla:

| Stack detectado | Skills a incluir |
|----------------|-----------------|
| React / JSX | `react-19` |
| Next.js / App Router | `nextjs-15` |
| TypeScript | `typescript` |
| Tailwind CSS | `tailwind-4` |
| Zod | `zod-4` |
| Zustand | `zustand-5` |
| Vercel AI SDK | `ai-sdk-5` |
| Django / DRF | `django-drf` |
| Playwright | `playwright` |
| Pytest | `pytest` |
| Vitest | `vitest` |
| JSON:API | `jsonapi` |
| TDD workflow | `tdd` |

Solo incluir las skills que EXISTEN en `.claude/skills/` o `~/.claude/skills/`.
NO inventar skills que no existen.

### Paso 3: Generar el AGENTS.md

Usar el template en [assets/AGENTS-TEMPLATE.md](assets/AGENTS-TEMPLATE.md) como base.

**Reglas de calidad obligatorias:**
- Máximo 200 líneas (si supera, usar `@path/to/file` para importar secciones)
- Cada comando DEBE ser verificable — correrlo antes de incluirlo
- Si falta información, poner `TODO:` explícito, nunca inventar
- Usar MAYÚSCULAS para reglas críticas: `MANDATORY`, `DO NOT`, `MUST`
- Cada regla debe ser verificable: "use 2-space indent" > "format code properly"

**Secciones requeridas (en este orden):**

1. **Repository Overview** — 1 párrafo: qué hace, tipo de proyecto, stack
2. **Build / Lint / Test Commands** — tabla con comandos VERIFICADOS
3. **Architecture** — módulos con rutas relativas reales
4. **Navigation Protocol (MANDATORY)** — cómo navegar sin leer todo
5. **Style Guide** — naming, formato, convenciones reales del repo
6. **Verification** — cómo el agente valida que su cambio es correcto
7. **What NOT To Do** — anti-patrones específicos del repo
8. **Sources of Truth** — tabla dominio → archivo fuente
9. **AI Skills** — solo si hay skills disponibles detectadas en Paso 2
10. **Auto-invoke Skills** — tabla acción → skill (solo skills detectadas)

### Paso 4: Verificar antes de escribir

Antes de escribir el archivo final:
- [ ] Todos los comandos en la tabla Build/Lint/Test existen y funcionan
- [ ] Todos los paths en Architecture existen en el repo
- [ ] Todas las skills en la tabla existen en `.claude/skills/` o `~/.claude/skills/`
- [ ] El archivo tiene menos de 200 líneas
- [ ] No hay información inventada (si falta algo → `TODO:`)

### Paso 5: Escribir y notificar

1. Escribir el archivo como `AGENTS.md` en la raíz del proyecto
2. Informar al usuario:
   - Qué secciones se generaron
   - Qué TODOs quedaron pendientes
   - Si debe correr `./skills/setup.sh` para distribuir a otras herramientas
   - Si debe correr `skill-sync` para mantener Auto-invoke actualizado

---

## Patrones críticos

### Sobre comandos
- Correr `npm run build`, `pytest`, etc. para verificar que existen ANTES de incluirlos
- Si un comando falla o no existe → marcarlo como `TODO: verificar`
- Nunca poner comandos genéricos que el agente "asumiría" que existen

### Sobre arquitectura
- Solo incluir rutas que existan en el repo
- Para monorepos: incluir tabla de componentes con su stack
- Para repos docs-only: describir la estructura de archivos y su propósito

### Sobre skills
- Solo listar skills que EXISTEN, nunca asumir
- Si el stack sugiere una skill pero no existe en el sistema → no incluirla
- La tabla Auto-invoke solo debe tener acciones relevantes al proyecto actual

### Sobre el límite de 200 líneas
- Si el repo es complejo (monorepo), dividir con `@path/to/component/AGENTS.md`
- La raíz referencia a los componentes, los componentes tienen sus propios AGENTS.md
- Cada componente puede tener sus propias skills y auto-invoke

---

## Anti-patrones a evitar en el AGENTS.md generado

- ❌ Comandos no verificados ("npm test" sin saber si existe ese script)
- ❌ Paths inventados ("src/components/" si la carpeta no existe)
- ❌ Skills que no están instaladas en el sistema
- ❌ Reglas genéricas que el agente ya sigue ("write clean code")
- ❌ Documentación de APIs (linkear, no copiar)
- ❌ Versiones exactas de dependencias (eso va en package.json)
- ❌ Descripciones archivo por archivo del codebase completo

---

## Comandos

```bash
# Distribuir AGENTS.md a todas las herramientas después de generar
./skills/setup.sh --all

# Solo Claude Code
./skills/setup.sh --claude

# Sincronizar Auto-invoke desde metadata de skills
./skills/skill-sync/assets/sync.sh

# Verificar qué generó (dry-run de sync)
./skills/skill-sync/assets/sync.sh --dry-run
```

---

## Resources

- **Template**: See [assets/AGENTS-TEMPLATE.md](assets/AGENTS-TEMPLATE.md) para el template con placeholders
- **skill-sync**: Ver `skills/skill-sync/SKILL.md` para mantener Auto-invoke sincronizado
- **setup.sh**: Ver `skills/setup.sh` para distribuir a Claude/Gemini/Codex/Copilot
