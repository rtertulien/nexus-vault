---
type: resource
up: "[[Nexus]]"
tags: [opencode, plugins, config, agents]
created: 2026-06-08
updated: 2026-08-07
---

# Plugins opencode

> Lista actualizada de plugins, MCP servers y agentes en opencode.
> Config: `C:\Users\rtert\.config\opencode\opencode.jsonc`

---

## Plugins instalados (9)

| Plugin | Versión | Propósito |
|--------|---------|-----------|
| opencode-mem | latest | Memoria vectorial, búsqueda rápida |
| openslimedit | latest | Edición contextual |
| opencode-websearch-cited | 1.2.0 | Búsqueda web con citas |
| @tarquinen/opencode-dcp | latest | Compresión de contexto |
| superpowers | git (obra/superpowers) | Skills: brainstorming, writing-plans, TDD, debugging, code-review, security-review, frontend-design, feature-dev, mcp-builder, skill-creator, agents-md-improver |
| opencode-snip | latest | Snippets de código |
| opencode-token-tracker | latest | Tracking de uso de tokens |
| opencode-autotitle | latest | Títulos automáticos |
| opencode-power-pack | git (waybarrios/opencode-power-pack) | 11 skills de Claude Code portados a OpenCode |

---

## MCP Servers

| Servidor | Acceso | Notas |
|----------|--------|-------|
| nexus | Vault Obsidian (lectura/escritura) | `C:\Users\rtert\Documents\nexus\nexus\` |
| pdf-reader | PDFs (lectura, búsqueda, OCR) | Pure-Rust MCP server |

---

## Agentes (7)

| Agente | Modelo | Skills | Rol |
|--------|--------|--------|-----|
| supervisor | kimi-k2.7-code ($3.50/M) | feature-dev | Orquestador + outcome loop (delega→review→fix) |
| shinobi-reviewer | deepseek-v4-pro ($3.48/M) | code-review + security-review | Juez con rubrics formales (5 tipos, scored 0-100) |
| data-ninja | gpt-5.6-luna ($1.20/M) | mcp-builder | APIs, endpoints, fetch, IndexedDB, rate limiting |
| designer | minimax-m3 ($1.20/M) | frontend-design | CSS, UI, prototipos premium + visión nativa MiniMax M3 |
| investigador | mimo-v2.5 ($0.28/M) | agents-md-improver | Verificación estricta, múltiples fuentes, citas |
| content-curator | minimax-m3 ($1.20/M) | skill-creator | Investigación contenido, síntesis, redacción |
| obsidian-curator | deepseek-v4-flash ($0.28/M) | agents-md-improver | Gestor vault Obsidian (frontmatter, wikilinks, templates) |

---

## Outcome Loop

```
supervisor delega → agente genera → shinobi-reviewer evalúa (rubric)
  ├── PASS (≥80) → siguiente paso
  └── FAIL → feedback específico → agente revisa (max 2 iteraciones)
       └── 2do FAIL → escalar al usuario
```

### Rubrics

| Tipo | Dimensiones | Threshold |
|------|-------------|-----------|
| **Código** | Seguridad 30%, Lógica 25%, Edge cases 20%, Convenciones 15%, Rendimiento 10% | ≥80 |
| **Diseño UI** | Consistencia 30%, Responsive 25%, Accesibilidad 20%, Animaciones 15%, Layout 10% | ≥80 |
| **APIs** | Verificación 30%, Error handling 25%, Rate limits 20%, Seguridad 15%, Performance 10% | ≥80 |
| **PWA** | Offline 30%, iOS 25%, Manifest 20%, Performance 15%, Installability 10% | ≥80 |
| **Contenido** | Precisión 30%, Claridad 25%, Completitud 20%, Formato 15%, Idioma 10% | ≥80 |

---

## Stack Premium Aprobado

| # | Librería | Uso | Precio |
|---|----------|-----|--------|
| 1 | GSAP + ScrollTrigger | Animaciones scroll, parallax | Free (2026) |
| 2 | Three.js | 3D WebGL, escenas inmersivas | Free |
| 3 | Lenis | Smooth scroll premium | MIT free |
| 4 | Rive | Animaciones interactivas vector | Free tier |
| 5 | Tailwind CSS | Estilos utility-first | Free |
| 6 | Framer Motion | Spring physics (React) | MIT free |
| 7 | Lottie | After Effects en web | Free |

---

## Plugins deshabilitados/eliminados

| Plugin | Razón |
|--------|-------|
| opencode-dynamic-context-pruning | Causaba lentitud al cargar |
| MCP vision | Eliminado (designer MiniMax M3 tiene visión nativa) |

---

## Notas técnicas

- **opencode-power-pack** reemplaza a many skills manuales
- **outcome loop** formalizado el 05 Ago 2026
- **rubrics** calibradas contra outputs conocidos
- **max 2 iteraciones** — más empeora (Anthropic 2026)
- **investigador primero** — siempre verificar endpoints antes de implementar

---

*Última actualización: 2026-08-07*