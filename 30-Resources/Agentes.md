---
type: resource
topic: opencode
tags: [resource, opencode, agents]
up: "[[Nexus]]"
---

# Agentes opencode

> Sistema de agentes para opencode.  
> [[Nexus|🏠 Nexus]] · [[Dev|🛠️ Dev]] · [[30-Resources/Memoria|💾 Memoria]]

| Agente               | Modelo            | Rol                                                |
| -------------------- | ----------------- | -------------------------------------------------- |
| **supervisor**       | Kimi K2.7 Code    | Features, arquitectura, coding (orquesta y delega) |
| **shinobi-reviewer** | DeepSeek V4 Pro   | QA código: bugs, edge cases, regresiones           |
| **data-ninja**       | DeepSeek V4 Pro   | APIs, endpoints, fetch, IndexedDB                  |
| **designer**         | MiniMax M3        | CSS, UI, prototipos + visión nativa                |
| **investigador**     | Mimo V2.5         | Verificación estricta, múltiples fuentes           |
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
- DeepSeek V4 Pro: $0.87
- DeepSeek V4 Flash: $0.28
- MiniMax M3: $2.40
- Qwen 3.7 Plus: $3.00
- Kimi K2.7 Code: $3.50

## Historial de cambios
- **24 Jun 2026:** migrado a deepseek-v4-pro (1M contexto, 97% cache, comunidad valida como frontier-level, reemplaza a qwen3.7-plus en declive -38%)
- **23 Jun 2026:** Reestructuración completa. 7 agentes entrenados con archivos .md. supervisor: rol corregido a orquestar+delegar con tabla de delegación. vision: eliminado (designer tiene visión nativa). temperature, steps y color configurados por agente. Flujo de trabajo de 3 fases documentado.
