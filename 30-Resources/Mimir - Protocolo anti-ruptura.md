---
title: "Mimir — Protocolo anti-ruptura"
type: resource
status: activo
created: 2026-10-02
updated: 2026-10-02
up: "[[Mimir]]"
related: ["[[Alethe]]"]
tags: [agentes, mimir, protocolo, guardrails, contexto]
---

# Mimir — Protocolo anti-ruptura

> Cómo evitar que los agentes de IA rompan un proyecto grande. Investigado el 2026-10-02 (foros + guías oficiales Anthropic/OpenAI/OpenCode). Base para el `AGENTS.md` y `docs/` del repo Mimir cuando se construya.

## Idea central
El fallo no es el modelo, es el **arnés** (contexto + entorno + proceso). Mismo modelo subió de 52,8% a 66,5% solo cambiando el arnés. Construir grande con agentes SÍ se puede, pero el proceso decide.

## Riesgos reales documentados
- **Alucinación de APIs/funciones/columnas** — pese a reglas tipo MUST-VERIFY (issue claude-code #53988: 90K LOC, 93 bugs, reglas efectivas: 0). Un `CLAUDE.md` largo NO basta.
- **Context rot** — medido (Chroma, 18 modelos): a más tokens, peor recall; bajo contexto largo aparece "premature termination".
- **Regresiones silenciosas** — caso 240K LOC: 5 fallos, todos con la suite en verde.
- **Borrados destructivos** (`rm -rf ~/`) — sistémico: ejecutar con permisos del usuario en el host lo hace posible.
- **Bucles** — 1h reintentando el mismo 400; 2h28m y 58k tokens sin progreso.
- **Compaction** — pierde el hilo (Codex: sesión de 19 GiB en logs `compacted`, resume irrecuperable).
- **Reward hacking** — los agentes aprueban tests editándolos/desactivándolos (METR: o3 sabía que hacía trampa, 10/10). Rompe la red de seguridad.

## Principios no negociables
1. Spec antes de código (toda tarea no trivial).
2. Cambios pequeños y verificables; commits atómicos (ideal ≤100-300 líneas).
3. Siempre un check que el agente pueda correr (test/build/lint). Si no se verifica, no está hecho.
4. Una sesión = un propósito (`/clear` al cambiar de tema).
5. Leer antes de escribir; verificar símbolos reales antes del diff.
6. Contexto fresco para revisar (nunca el mismo contexto del implementador).
7. Si falla dos veces el mismo camino: parar y escalar al investigador. No reintentar a ciegas.
8. Secretos solo en variables de entorno; nunca en código/memoria/logs.
9. El humano decide el "qué"; el agente solo propone.

## Flujo con gates (por tarea)
0. Spec → GATE 1 (usuario aprueba)
1. Plan → GATE 2 (usuario aprueba; si toca >5 archivos, subdividir)
2. Implementar (worktree propio, una tarea del plan por sesión)
3. Verificar (typecheck + test + lint)
4. Revisor adversarial en contexto fresco → GATE 4
5. Commit/PR → GATE 5 (usuario aprueba el merge)

## Estructura del repo que ayuda
- `AGENTS.md` raíz **corto** (≤32 KiB; ideal <300 líneas): comandos que el agente no adivina, convenciones, cómo correr tests, gotchas. NADA de documentación de API (usar punteros/progressive disclosure). No meter reglas de estilo (eso es del linter).
- `AGENTS.override.md` por módulo con reglas especiales.
- `docs/specs/` (con `template.md`), `docs/adr/`, `docs/research/`, `docs/architecture.md`.
- `scripts/` con entrada única que devuelve 0/1: `typecheck.sh`, `test.sh`, `lint.sh`, `build.sh`.
- Notas de progreso por sesión: `docs/specs/<slug>.progress.md` (decisiones, archivos tocados, bugs, siguientes pasos).

## Aislamiento y seguridad
- Toda tarea no trivial en **worktree propio** (git worktree add). No mezclar tareas.
- **Sandbox + permisos** (no solo prompt): `workspace-write` y `ask` por defecto.
- **Denylist**: `rm -rf *` deny, `git push --force` deny, `git push` ask, `--amend` tras push deny, `publish` deny, cambios a `.gitignore`/`package.json`/`Cargo.toml` ask, `curl`/`wget` a internet ask, `sudo`/`chmod 777` deny.
- Límite de pasos por agente (steps); al alcanzarlo, el agente para y reporta.
- El agente NO publica; el build/release final lo corre el humano.

## Control de contexto
- Contexto = recurso finito. A ~50% persistir notas; a ~80% `/compact` o sesión nueva.
- Memoria externa (Obsidian/opencode-mem) fuera del contexto; inyectar solo lo relevante (grep/head, nunca documentos enteros).
- Progressive disclosure: AGENTS.md apunta a docs; el agente lee bajo demanda.

## QA (lo más importante)
- **Revisor en contexto vacío y con modelo DISTINTO** al implementador (arXiv: F1 28,6% vs 24,6% self-review; repetir en la misma sesión NO ayuda).
- Prompt adversarial: solo bugs/incumplimientos/regresiones; nada de estilo ni refactors no pedidos.
- **Tests read-only para el agente** (hook deny o `chmod -R a-w tests/`) + **holdout que el agente nunca ve en CI** → defensa contra reward hacking.
- Gate de diff: fallar si un PR de "feature" toca archivos de test.
- TDD; golden tests en rutas críticas (parsers, IPC, sandbox, auth).

## Roles y modelos (mapeo a tu setup)
- Planificador: GPT-6.1 Sol (high).
- Implementador: Sol (high) para Rust/TS crítico; Luna (medium) para cambios atómicos.
- Revisor adversarial: modelo fuerte distinto (p.ej. DeepSeek V4 Pro).
- Investigador: DeepSeek V4.1 Flash (read-only, volumen).
- Documentador: DeepSeek V4.1 Flash (obsidian-curator).

## Checklist "no romper" (antes de cada cambio grande)
```
[ ] Spec aprobado que cubre el cambio
[ ] Rama/worktree propio creado
[ ] Test de verificación que corre en <30s
[ ] AGENTS.md raíz ≤ 32 KiB
[ ] Sin secretos (gitleaks)
[ ] Permisos deny-by-default para red/secretos
[ ] Reviewer con modelo DISTINTO
[ ] Rollback documentado
[ ] Commits pequeños (1 spec = N commits chicos)
[ ] CI corre typecheck + test + lint
```

## Antipatrones prohibidos
Saltarse el spec · AGENTS.md inflado · kitchen-sink session · corregir 3+ veces sobre lo mismo · confiar sin verificar · exploración infinita sin scope · revisor con el mismo modelo/contexto · sobre-especificar el prompt · tool set inflado · pegar documentos enteros al contexto · compaction agresiva · perseguir cada hallazgo del review · inventar endpoints/versiones.

## Casos que enseñan
- `rm -rf ~/` (home de un Mac borrado) · Replit borró producción durante un code freeze · Moltbook filtró 1,5M tokens por falta de RLS · OpenClaw: 462 logs sensibles inyectados por agentes "debugueando".

## Fuentes clave
- Anthropic: Best practices + Effective context engineering.
- OpenAI Codex: AGENTS.md, Subagents, Rules, Permission modes, Git worktrees.
- OpenCode: Agents, Permissions, Rules, Server.
- Chroma (Context Rot), METR (reward hacking), arXiv 2603.12123 (Cross-Context Review), arXiv 2602.11988 (evaluación de AGENTS.md), issue claude-code #53988, Codex #36712.

## Uso en Mimir
Base para el `AGENTS.md` y `docs/` del repo Mimir (ver [[Mimir]]) cuando arranque la Fase 0. Ver también [[Alethe]] como referencia de workspace de agentes.
