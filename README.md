# architecture-review

> Refactorizar donde el código cambia, no donde se ve feo.

**Revisión de arquitectura** · Skill para agentes de código de la comunidad [nvvdev](https://github.com/nvv-dev). Funciona en Claude Code como plugin o como skill suelta, y en cualquier agente compatible con el formato abierto [`SKILL.md`](https://agentskills.io).

*English summary at the end.*

## Qué hace

Cruza el historial de git (lo que más cambia) con la complejidad para encontrar puntos calientes, busca formas problemáticas (módulos superficiales, cambios en escopeta, capas que se filtran, ciclos, condicionales paralelos) y entrega máximo cinco candidatos de refactorización ordenados por beneficio sobre esfuerzo. Es un reporte, no refactoriza.

## Objetivo

Gastar el esfuerzo de refactorización donde más rinde.

## Cuándo usarla

- Revisión mensual de un código base
- Cuando una zona se siente cada vez más difícil de cambiar

## Cuándo no

- Repos pequeños o recién creados

## Ejemplo

```text
/architecture-review del módulo de facturación.
```

## Cómo se activa

Esta skill **solo se activa a mano** (`disable-model-invocation: true`): no ocupa contexto en cada sesión y la invocas cuando la necesitas:

- Instalada como plugin: `/architecture-review:architecture-review`
- Copiada a `.claude/skills/`: `/architecture-review`

## Instalación

### Opción 1: Claude Code, como plugin (recomendada)

Dentro de Claude Code:

```text
/plugin marketplace add nvv-dev/claude-plugins
/plugin install architecture-review@nvvdev
```

La primera línea agrega el catálogo de nvvdev una sola vez; la segunda instala esta skill. Si prefieres el paquete completo, instala `dev-workflow@nvvdev` en su lugar (incluye esta skill y las demás de [dev-workflow](https://github.com/nvv-dev/dev-workflow)); no instales los dos a la vez.

### Opción 2: Claude Code, copiando la skill

Para un solo proyecto (se comparte con tu equipo por git):

```bash
git clone --depth 1 https://github.com/nvv-dev/architecture-review.git /tmp/architecture-review
mkdir -p .claude/skills
cp -R /tmp/architecture-review/skills/architecture-review .claude/skills/
```

Para todos tus proyectos, cópiala en `~/.claude/skills/` en lugar de `.claude/skills/`.

### Opción 3: otros agentes

Copia la carpeta `skills/architecture-review/` en la carpeta de skills de tu agente (cada agente documenta la suya). Con la CLI [skills](https://github.com/vercel-labs/skills) de Vercel Labs:

```bash
DISABLE_TELEMETRY=1 npx skills add nvv-dev/architecture-review
```

### Verificar

Abre una sesión nueva y pregunta "¿qué skills tienes disponibles?"; `architecture-review` debe aparecer. Con el plugin, `/plugin` la muestra como instalada.

### Actualizar o desinstalar

- Plugin: `/plugin marketplace update nvvdev` para actualizar; `/plugin uninstall architecture-review@nvvdev` para quitarla.
- Copia manual: repite la copia para actualizar, o borra la carpeta `.claude/skills/architecture-review`.

Antes de instalar cualquier skill, incluida esta, conviene leerla: es un solo archivo, [`skills/architecture-review/SKILL.md`](skills/architecture-review/SKILL.md). Ver [vetting-external-skills](https://github.com/nvv-dev/vetting-external-skills).

## Contenido

| Ruta | Qué es |
|---|---|
| `skills/architecture-review/SKILL.md` | La skill: instrucciones en inglés para que los escáneres de seguridad las analicen completas; el agente responde en tu idioma |
| `.claude-plugin/plugin.json` | Manifiesto para instalarla como plugin |
| `.github/workflows/` | Validación de estructura y escaneo con SkillSpector en cada cambio |

## Contribuir

Este repo se **genera** desde [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow), donde vive la versión canónica. Abre issues y PRs allí. La mejor contribución es un caso real en la sección *Known failures* de la skill, con la regla que lo evita.

## Origen

Escrita desde cero por nvvdev. Inspirada en: [mattpocock/skills](https://github.com/mattpocock/skills) (improve-codebase-architecture).

## Licencia

MIT.

---

## English

**architecture-review** — Hotspots from churn × complexity, five ranked refactoring candidates, no changes made.

Install in Claude Code with `/plugin marketplace add nvv-dev/claude-plugins` and `/plugin install architecture-review@nvvdev`, or copy `skills/architecture-review/` into `.claude/skills/` (project) or `~/.claude/skills/` (personal). Other agents: copy the folder into their skills directory, or run `DISABLE_TELEMETRY=1 npx skills add nvv-dev/architecture-review`. Manual-only skill: invoke it with `/architecture-review:architecture-review` (plugin) or `/architecture-review` (copied). Canonical source: [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow). License: MIT.
