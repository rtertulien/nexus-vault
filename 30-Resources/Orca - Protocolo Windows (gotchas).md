# Protocolo operativo — Orca + Codex + OpenCode (Windows)

Investigado 2026-10-03 (foros + issues oficiales). Los 3 dolores reales y cómo evitarlos.

## 1. Codex: paste grande no se envía (issue #11343)

**Causa:** Orca escribe el spec (bracketed paste) + el Enter en una sola escritura PTY.
Codex detecta el paste por burst; con payload >~1000 chars el burst sigue vivo y el
Enter se absorbe como newline → queda `[Pasted Content N chars]` sin enviar.

**Solución (hoy, fiable):**
1. Especs largos NO van inline. Escríbelos a un `.md` y pasa un spec corto que lo apunte.
2. Tras cada `worker-start` de Codex: leer el terminal y si hay `[Pasted Content`, un Enter:
   ```powershell
   orca orchestration worker-read --dispatch <d> --json     # ver tail
   orca terminal send --terminal <handle> --enter --json
   ```
3. Ya está automatizado en `orquestar.ps1` (detecta el Paste y manda Enter).

## 2. Comillas dobles en --spec rompen PowerShell (bug real)

**Causa:** PowerShell no escapa `"` al pasar args a comandos nativos; Orca lo
re-divide y cree que el resto es otro comando (`Unknown command: ... bottom top`).

**Solución:**
- Usar **comillas simples** para todo el `--spec`, sin `"` ni `'` dentro.
- O here-string single-quoted: `$s = @'...'@; orca ... --spec $s`
- O spec corto que apunta a un `.md` (lo más robusto).
- NUNCA meter `"..."` dentro del spec inline.

## 2b. Forzar el reporte automático de Codex (worker_done)

**Causa:** Codex, como worker, responde en su terminal pero NO ejecuta el `worker_done`
del preámbulo; Orca deja el dispatch `in_progress` hasta el timeout.

**Solución aplicada (probada 2026-10-04):** regla condicional en `~/.codex/AGENTS.md`
(sección "Reporte obligatorio cuando Codex trabaja como worker de Orca"). Al detectar que
fue lanzado como dispatched worker (preámbulo con Task ID/Dispatch ID), Codex ejecuta
`worker_done` una vez al terminar. Backup: `~/.codex/AGENTS.md.bak-orca-worker-report`.
Prueba `handoff/orquestador-orca/PROOT-CODEX-WORKER-DONE.md`: worker quedó `succeeded`
sin follow-up del coordinador.

Esto elimina el follow-up manual del `orquestar.ps1` para Codex.

## 3. OpenCode (coordinador) se queda "pillado"

**Causas:**
- `check --wait` bloquea por diseño; un worker que nunca manda `worker_done` deja la
  espera hasta el timeout. (Suele ser el Problema A: el brief nunca se envió.)
- El Tool `task` de OpenCode hace `await` sin timeout efectivo → si un subagente se
  atasca, el padre espera para siempre (issues #11865, #13841, #21250).

**Solución:**
- Ventanas de espera CORTAS (60s), no 15 min. Timeout = checkpoint, no fallo.
- Salir del loop si no hay workers activos:
  ```powershell
  while ($true) {
    orca orchestration check --wait --types worker_done,escalation,question --timeout-ms 60000 --json
    $w = orca orchestration worker-list --run <runId> --json | ConvertFrom-Json
    if (-not $w.workers) { break }
  }
  ```
- En OpenCode, timeouts reales:
  ```json
  { "provider": { "anthropic": {"options": {"timeout":300000,"chunkTimeout":30000}},
                  "openai":    {"options": {"timeout":300000,"chunkTimeout":30000}} } }
  ```
  y `$env:OPENCODE_EXPERIMENTAL_TASK_DEFAULT_TIMEOUT_MS = 600000`.

## 4. Workers 'idle' / agent_readiness timeout

**Causa:** trust prompt o sign-in de Codex; o el TUI fullscreen 0.157+ cambió labels
que Orca esperaba (fix shipped en Orca v1.4.218).

**Solución:** pre-autenticar (`codex login`) y confiar el proyecto (`/trust`); mantener
Orca >= v1.4.218; si queda, `worker-stop` y reintentar.

## 5. Verificación visual (navegador Orca)

- `orca scroll` NO dispara ScrollTrigger → usar `orca eval -e "window.scrollTo(0,Y); dispatchEvent(new Event('scroll')); ScrollTrigger.update(); 'ok'"`.
- Screenshot: `orca screenshot --worktree active --json` (viene base64 en `result.data`).
- Device: `orca set device --name "iPhone 12" --worktree active`.

## Checklist de lanzamiento (antes de cada oleada)

1. `orca status --json` → ok.
2. Specs en `.md`; spec inline corto y con comillas simples.
3. Lanzar la oleada completa antes de esperar.
4. Enter auto a Codex si hay `[Pasted Content`.
5. `check --wait` en ventanas de 60s + salida por `worker-list`.
6. `worker-release` de los asentados; `worker-stop` de los colgados.
