---
type: resource
topic: opencode
tags: [resource, opencode, agents, workflow, outcome-loop, proot, stack-premium]
up: "[[Resources MOC]]"
created: 2026-08-26
updated: 2026-08-26
---

# Flujo — Agentes y Outcome Loop

> Cómo se trabaja en opencode: agentes, outcome loop, brainstorming, proots, stack premium y Obsidian como memoria.
> [[Resources MOC|📚 Recursos]] · [[Agentes]] · [[Premium Design Stack]] · [[Plugins opencode]]

---

## 1. Agentes (8 documentados)

| # | Agente | Modelo | Skill | Tarea principal |
|---|--------|--------|-------|-----------------|
| 1 | **supervisor** | kimi-k3 (free tier) | feature-dev | Orquesta y delega. Crea proot, asigna fases, gestiona outcome loop |
| 2 | **shinobi-reviewer** | deepseek-v4-pro | code-review + security-review | Juez QA. Evalúa con rubric (PASS ≥80), 2-stage review (Spec + Code Quality) |
| 3 | **data-ninja** | gpt-5.6-luna | mcp-builder | APIs, endpoints, fetch, IndexedDB, rate limiting |
| 4 | **designer** | kimi-k2.6 (free tier) | frontend-design | CSS, UI, prototipos premium, stack visual |
| 5 | **investigador** | deepseek-v4-flash | agents-md-improver | Verificación estricta, múltiples fuentes, citas |
| 6 | **content-curator** | minimax-m3 | skill-creator | Investigación contenido, síntesis, redacción |
| 7 | **obsidian-curator** | deepseek-v4-flash | agents-md-improver | Gestor vault: frontmatter, wikilinks, templates, memoria |
| 8 | vision *(eliminado 23 Jun 2026)* | — | — | Visión de imágenes. Reemplazado por visión nativa del designer |

### Cuándo usar cada uno

| Situación | Agente |
|-----------|--------|
| Empezar cualquier proyecto / verificar APIs vigentes | **investigador** primero, siempre |
| Arquitectura, features complejas, coding largo | **supervisor** (delega a especialistas) |
| QA final, bugs, edge cases, seguridad, regresiones | **shinobi-reviewer** (nunca cerrar sin él) |
| Endpoints, fetch, IndexedDB, rate limits | **data-ninja** |
| CSS, UI, prototipos visuales premium | **designer** |
| Verificación estricta multi-fuente, documentación | **investigador** |
| Síntesis, redacción, contenido | **content-curator** |
| Vault Obsidian, frontmatter, wikilinks, cierre de sesión | **obsidian-curator** |

---

## 2. Outcome Loop

```
supervisor delega → agente genera → shinobi-reviewer evalúa (rubric)
  ├── PASS (≥80) → siguiente paso
  └── FAIL → feedback específico → agente revisa (max 2 iteraciones)
       └── 2do FAIL → escalar al usuario
```

- **Threshold:** ≥80 para PASS
- **Max iteraciones:** 2 (más empeora según Anthropic 2026)
- **Impacto:** +30% tokens, +10% calidad, -70% fixes manuales

### Rubrics

| Tipo | Dimensiones | Threshold |
|------|-------------|-----------|
| **Código** | Seguridad 30%, Lógica 25%, Edge cases 20%, Convenciones 15%, Rendimiento 10% | ≥80 |
| **Diseño UI** | Consistencia 30%, Responsive 25%, Accesibilidad 20%, Animaciones 15%, Layout 10% | ≥80 |
| **APIs** | Verificación 30%, Error handling 25%, Rate limits 20%, Seguridad 15%, Performance 10% | ≥80 |
| **PWA** | Offline 30%, iOS 25%, Manifest 20%, Performance 15%, Installability 10% | ≥80 |
| **Contenido** | Precisión 30%, Claridad 25%, Completitud 20%, Formato 15%, Idioma 10% | ≥80 |

---

## 3. Brainstorming (plugin superpowers)

Antes de implementar, siempre brainstorming con el skill de superpowers. Tres modos:

| Modo | Cuándo | Qué produce |
|------|--------|-------------|
| **Architectural** | Decisiones de arquitectura, estructura del sistema, stack | Direcciones de diseño + elección razonada |
| **Bounded** | Features acotadas, cambios concretos con requisitos claros | Spec de la feature, checklist de implementación |
| **Spike** | Explorar/prototipar una idea sin compromiso | Prototipo desechable, aprendizaje documentado |

Regla: brainstorming → DESIGN.md/spec → proot → implementación por fases.

---

## 4. Proot obligatorio

- **Sin proot no se empieza ningún proyecto.** El proot es el "plan de vuelo".
- Contiene: fases, agente responsable por fase, rubric de evaluación, outcome loop, stack, features.
- Ubicación: `C:\Users\terrible019\Documents\proot\`
- **Lección 26 Ago 2026:** el tracker v3 de Stoneshard se hizo directo en main chat (vanilla, sin agentes) → violó stack premium y outcome loop. Se corrigió creando proot con 5 fases. **Nunca repetir.**

---

## 5. Stack Premium (obligatorio en proyectos web)

**NO vanilla.** Framework + librerías premium. Ver [[Premium Design Stack]] para detalles y CDNs.

| Capa | Herramienta |
|------|-------------|
| Lenguajes | TypeScript (obligatorio), JS, HTML/CSS, GLSL, Rust (WASM) |
| Framework | Next.js / Astro / Vue |
| Animación | GSAP 3.15 (SplitText, ScrollTrigger, Flip, MorphSVG, Draggable) + Motion (React) |
| Scroll | Lenis + GSAP ScrollTrigger |
| 3D + Shaders | Three.js + GLSL (ShaderMaterial, LYGIA) |
| UI | Tailwind + shadcn/ui + Magic UI + 21st.dev |
| Fonts | Fontshare: Clash Display + Satoshi |
| Carousels | Swiper.js v14.1 |
| Audio | Howler.js |
| Vectoriales | Rive / Lottie |
| Design System | UI UX Pro Max |
| Deploy | Cloudflare Pages + Functions |

---

## 6. Obsidian también es memoria

El vault es **memoria estructurada** — no solo notas sueltas:

- **10-Projects/** → estado y decisiones de cada proyecto
- **20-Daily/** → resumen de cada sesión (cierre de sesión obligatorio)
- **30-Resources/** → conocimiento reutilizable (este flujo, stack, agentes)
- **40-Templates/** → templates de notas

**Regla de cierre de sesión:** (1) nota completa del proyecto en 10-Projects/, (2) resumen en la daily 20-Daily/YYYY-MM/YYYY-MM-DD.md, (3) memoria vectorial (opencode-mem) con tags, (4) AGENTS.md si hay cambios.

Complementa a: AGENTS.md (perfil global), MEMORY.md (proyecto actual), opencode-mem (memoria vectorial). Ver [[Memoria]].

---

## 7. Referencias

- Plugin superpowers: skills brainstorming, writing-plans, TDD, debugging, code-review, security-review, frontend-design, feature-dev, mcp-builder, skill-creator, agents-md-improver (ver [[Plugins opencode]])
- [[Agentes]] — modelos, costos, flujo de 3 fases
- [[Premium Design Stack]] — stack completo con CDNs
- [[Obsidian Curator - Instrucciones]] — reglas del vault

---

*Creado: 2026-08-26 · Cierre especial de sesión*