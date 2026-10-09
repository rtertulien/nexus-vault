---
title: "Suite de agentes — Endurecimiento"
type: resource
status: activo
created: 2026-10-02
updated: 2026-10-02
up: "[[Resources MOC]]"
related: ["[[Mimir - Protocolo anti-ruptura]]", "[[Mimir]]", "[[Flujo - Agentes y Outcome Loop]]"]
tags: [agentes, guardrails, opencode, suite, seguridad]
---

# Suite de agentes — Endurecimiento (2026-10-02)

> Mejoras aplicadas a la suite tras la investigación de fallos de agentes en proyectos grandes. Principio rector: **lo que funciona es la imposición por entorno, no las reglas en el prompt.**

## Aplicado hoy
1. **AGENTS.md recortado: 982 → 183 líneas.** El historial de sesiones (802 líneas, 82% del archivo) se movió a `~/.config/opencode/SESIONES.md`. Motivo: los archivos de reglas inflados hacen que el modelo ignore las instrucciones. Backup: `AGENTS.md.bak-2026-10-02`.
2. **Denylist de permisos en `opencode.jsonc`** (mecanismo, no prompt):
   - `read`: denegados `.env`, `*.pem`, `*.key`, `id_rsa*`, `.ssh/**`.
   - `bash` deny: `rm *`, `*rm -rf*`, `*rm -fr*`, `git push --force*`, `git push -f*`, `git reset --hard*`, `git clean -fd*`, `npm/pnpm/yarn/cargo publish*`, `sudo *`, `Remove-Item *`, `del /s`/`/q`, `rd /s`, `rmdir /s`, `Format-Volume`, `Clear-Disk`, `diskpart`, `chmod 777`.
   - `bash` ask: `git push*`, `curl *`, `wget *`, `Invoke-WebRequest *`.
   - `doom_loop`: ask.
   - Backup: `opencode.jsonc.bak-2026-10-02`. JSON validado antes de guardar.
3. **`steps` por subagente** (corta bucles): supervisor 30, shinobi-reviewer 20, data-ninja 25, designer 30, designer-fix 5, investigador 20, content-curator 15, obsidian-curator 15.

## Pendiente (a su tiempo, con su requisito)
- **#2 Tests read-only + rúbrica anti-manipulación** → cuando Mimir tenga tests (defensa contra reward hacking).
- **#4 Worktrees por tarea** → cuando haya git instalado.
- **#6 Verificar símbolos antes de escribir** → va en el AGENTS.md del repo, no global.
- **#7 Notas de progreso por spec** (`docs/specs/<slug>.progress.md`) → en el repo.
- **#8 Rúbrica "suite en verde ≠ correcto"** → al endurecer la rúbrica del shinobi-reviewer.

## Acción requerida
Reiniciar opencode para que cargue la config nueva (no hay hot-reload).

## Referencias
- [[Mimir - Protocolo anti-ruptura]]
- Investigación: foros (GitHub issues claude-code #53988, Docker rm -rf, METR reward hacking, Chroma context rot) + guías Anthropic/OpenAI/OpenCode.
