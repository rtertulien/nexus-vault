---
type: resource
topic: opencode
tags: [resource, opencode, agents]
up: "[[Nexus]]"
---

# Agentes opencode

> Sistema de agentes para opencode.  
> [[Nexus|🏠 Nexus]] · [[Dev|🛠️ Dev]] · [[30-Resources/Memoria|💾 Memoria]]

> [!warning] Esquema actual (27 Sep 2026) — la tabla de abajo está desactualizada
> Son **8 agentes**. Los modelos vigentes son:
>
> | Agente | Modelo | Cuenta | Rol |
> |---|---|---|---|
> | **supervisor** | `opencode-go/gpt-6-luna` | go | Orquesta y delega (nunca ejecuta) |
> | **shinobi-reviewer** | `opencode-go/deepseek-v4-pro` | go | QA 2 etapas, read-only |
> | **data-ninja** | `opencode-go/gpt-5.6-luna` | go | APIs, fetch, IndexedDB |
> | **designer** | `openai/gpt-6-sol` | **ChatGPT** | **DISEÑADOR PRINCIPAL (único)**: maquetas, bugs de UI, animaciones, completar diseño |
> | **designer-fix** | `opencode-go/glm-5.3-flash` | go | Solo arreglos triviales y mecánicos (1 cambio por prompt) |
> | **investigador** | `opencode-go/deepseek-v4.1-flash` | go | Verificación multi-fuente, solo lectura |
> | **content-curator** | `opencode-go/minimax-m3` | go | Contenido, síntesis, redacción |
> | **obsidian-curator** | `opencode-go/deepseek-v4.1-flash` | go | Vault: frontmatter, wikilinks, templates |
>
> **Eliminados:** `designer-lite` y `designer-max` (27 Sep 2026). El "lite" engañaba: el trabajo de arreglos lo hace `designer-fix`.
> **Rompe-bucles (27 Sep 2026):** `shinobi-reviewer` y `data-ninja` **escalan al investigador** si el mismo problema sobrevive a 2 intentos consecutivos. No pueden invocarlo ellos (los subagentes no lanzan subagentes, profundidad 1): devuelven un bloque `ESCALADO A INVESTIGADOR` con el error literal, el entorno, los dos intentos fallidos, la hipótesis y la petición concreta. Si no saben la causa, deben decir "no lo sé".
> **Ruta real de los archivos:** `C:\Users\terrible019\.config\opencode\agent\` (no la de abajo).
> **Fuente de verdad:** `~/.config/opencode/opencode.jsonc` (bloque `agent`) + `~/.config/opencode/AGENTS.md`.

| Agente               | Modelo            | Rol                                                |
| -------------------- | ----------------- | -------------------------------------------------- |
| **supervisor**       | Kimi K3           | Features, arquitectura, coding (orquesta y delega) |
| **shinobi-reviewer** | DeepSeek V4 Pro   | QA código: 2-stage review (Spec + Code Quality)    |
| **data-ninja**       | GPT-5.6 Luna     | APIs, endpoints, fetch, IndexedDB                  |
| **designer**         | Kimi K2.6         | CSS, UI, prototipos premium                        |
| **investigador**     | DeepSeek V4 Flash | Verificación estricta, múltiples fuentes           |
| **content-curator**  | MiniMax M3        | Investigación contenido, síntesis, redacción       |
| **obsidian-curator** | DeepSeek V4 Flash | Gestor vault Obsidian                              |

| Config           | Temp | Steps | Color     |
| ---------------- | ---- | ----- | --------- |
| supervisor       | 0.1  | 25    | accent    |
| shinobi-reviewer | 0.1  | 20    | error     |
| data-ninja       | 0.1  | 20    | info      |
| designer         | 0.5  | 15    | success   |
| investigador     | 0.1  | 15    | warning   |
| content-curator  | 0.4  | 12    | secondary |
| obsidian-curator | 0.2  | 12    | info      |

## Flujo de trabajo (3 fases)

### Fase 1 — Plan (sin tocar código)
```
@investigador verifica endpoints/APIs → @supervisor crea plan → brainstorming skill
```
**Nunca empezar sin @investigador** — evita implementar APIs muertas

### Fase 2 — Build (implementar)
```
@supervisor delega → @designer UI → @data-ninja APIs → código
```
@supervisor reparte trabajo a agentes especializados

### Fase 3 — Verify (antes de cerrar)
```
@shinobi-reviewer audita → @supervisor corrige → deploy
```
**Nunca cerrar sin @shinobi-reviewer** — bugs detectados antes de producción

## Ubicación de archivos
```
C:\Users\rtert\.config\opencode\agents\
├── supervisor.md
├── shinobi-reviewer.md
├── data-ninja.md
├── designer.md
├── investigador.md
├── content-curator.md
└── obsidian-curator.md
```
Cada archivo contiene frontmatter YAML (description, mode, model, temperature, steps, color) + prompt detallado. OpenCode los carga al inicio de cada sesión.

## Costos (aprox / 1M tokens)
- GPT-5.6 Luna: $1.20 output / $0.20 input (data-ninja)
- DeepSeek V4 Pro: $3.48 (shinobi-reviewer)
- DeepSeek V4 Flash: $0.28 (investigador, obsidian-curator)
- MiniMax M3: $1.20 (content-curator)
- Kimi K3: nuevo modelo supervisor (opencode-go free tier)
- Kimi K2.6: nuevo modelo designer (opencode-go free tier)

## Historial de cambios
- **27 Sep 2026:** Reestructura de diseñadores. `designer` pasa a `openai/gpt-6-sol` (**Sol, ChatGPT**) como **diseñador principal único** — decisión del usuario: "Sol realmente es muy bueno", aunque gaste cuota de ChatGPT. Nuevo `designer-fix` con `opencode-go/glm-5.3-flash` solo para arreglos triviales (el que menos rompe). **Eliminados** `designer-lite` y `designer-max`. Total: **8 agentes**. Probado en la maqueta definitiva de Agentic OS.
- **19 Ago 2026:** Migrados a opencode-go free tier. supervisor → Kimi K3, designer → Kimi K2.6, investigador → DeepSeek V4 Flash. Prompts reescritos: -41% tokens. shinobi-reviewer ahora 2-stage (Spec Compliance → Code Quality), fix loop máx 3 rondas. supervisor flujo: brainstorming → DESIGN.md → proot template → task→review→fix loop → ledger.
- **24 Jun 2026:** migrado a deepseek-v4-pro (1M contexto, 97% cache, comunidad valida como frontier-level, reemplaza a qwen3.7-plus en declive -38%)
- **23 Jun 2026:** Reestructuración completa. 7 agentes entrenados con archivos .md. supervisor: rol corregido a orquestar+delegar con tabla de delegación. vision: eliminado (designer tiene visión nativa). temperature, steps y color configurados por agente. Flujo de trabajo de 3 fases documentado.
