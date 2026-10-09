---
title: Codex Frontend y OpenCode Backend
date: 2026-10-02
tags: [codex, opencode, frontend, backend, diseño]
status: activo
type: project
up: "[[Projects MOC]]"
---

# Codex Frontend y OpenCode Backend

## Decisión operativa

- OpenCode funciona como centro de comando y se encarga de backend, lógica de negocio, datos, APIs, persistencia y seguridad.
- Codex es dueño del frontend: UI/UX, estructura visual, tokens, componentes, estilos y animaciones.
- Para cambios que cruzan esa frontera, Codex define forma y contrato de datos; OpenCode implementa y conecta el backend.

## Regla de integración

El frontend es backend-blind. Los componentes no hacen `fetch` directo; todas las operaciones pasan por `js/api.js`. Codex trabaja con mocks detrás de ese adapter y documenta las formas de entrada/salida. OpenCode sustituye el adapter por la conexión real sin modificar el diseño.

## Entregables del frontend

1. Interfaz completa y funcional con datos mock.
2. `DESIGN.md`: tokens, estructura y componentes.
3. `js/api.js`: mocks encapsulados detrás del adapter.
4. `CONTRATO.md`: método, input, output, errores y lugar de uso en UI por operación.
5. Estados de loading, vacío, error y éxito conectados.

## Guía del diseñador OpenCode

- Respetar framework, dependencias y convenciones que ya existen.
- CSS para animación sencilla; GSAP para secuencias complejas, timelines o scroll.
- Motion como alternativa declarativa en React. No combinarlo con GSAP por defecto.
- Lenis, Three.js, Swiper, Howler, Rive/Lottie, shadcn, Magic UI y MCP son opcionales, condicionados a una necesidad concreta.
- No instalar dependencias sin autorización explícita.
- Responsive, contraste, teclado/foco, accesibilidad y `prefers-reduced-motion` son parte del diseño.

## Skills registradas

- `frontend-design`: diseño nuevo o refactor.
- `impeccable`: diseño, rediseño, crítica y pulido visual.
- `ui-ux-pro-max`: dirección visual, motion y páginas de marketing cuando aplique.
- `ai-slop`: revisión final anti-genérico.
- `code-quality`: revisión de componente.

## Agentes OpenCode conservados

- `designer`: diseño principal.
- `designer-fix`: ajustes visuales puntuales.
- `shinobi-reviewer`: revisión adversarial/read-only.
- `investigador`: verificación de referencias externas cuando haga falta.
- `obsidian-curator`: memoria permanente y organización del vault; mantener habilitado.

## Modelo y esfuerzo

Diseño principal: GPT‑6.1 Sol, effort high. Microajustes de un valor o color: Luna. No usar xhigh/max para diseño.

## Referencias locales

- Brief de rol de Codex: `C:\Users\terrible019\Documents\chat de opencode\AGENTS.md`
- Stack premium opcional: `C:\Users\terrible019\Documents\chat de opencode\codex-brief-stack.md`
- Instrucciones del agente diseñador: `C:\Users\terrible019\.config\opencode\agent\designer.md`
## Memoria vectorial — decisión

- Se aclaró que Memoire es la memoria vectorial asociada a OpenCode; Codex no tiene una memoria vectorial propia conectada en esta configuración.
- Se consideró crear una memoria vectorial local para Codex con índice de Obsidian vía MCP.
- **Decisión del usuario:** dejar la configuración como está por ahora: Obsidian como memoria permanente, mantenida por OpenCode/`obsidian-curator`; no agregar otra base ni índice vectorial de Codex por el momento.
- Reconsiderar solo si buscar entre muchas notas manualmente empieza a ser una limitación.