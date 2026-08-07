---
type: resource
topic: opencode
tags: [resource, opencode, tokens, optimization]
up: "[[Nexus]]"
---
			
# Optimización de Tokens — opencode Go

> Guía práctica para estirar el plan mensual sin sacrificar calidad.

## Reglas de oro

### 1. Sé específico desde el primer mensaje
- Mal: "hazme una app del clima"
- Bien: "app del clima con API de OpenWeather, 3 ciudades, dark mode, sin backend"

Cada vuelta de clarificación = ~500 tokens extras.

### 2. Usa el vault de Obsidian como memoria externa
- Leo archivos vía MCP filesystem — no consumen contexto de sesión
- Guarda specs, notas, decisiones en el vault
- En vez de decirme "recuerda que...", dime "lee la nota X"

### 3. Modelos baratos para tareas simples
| Modelo | Costo relativo | Úsalo para |
|--------|----------------|------------|
| `mimo-v2.5` (investigador) | 1x | Búsquedas, leer archivos, preguntas simples |
| `deepseek-v4-pro` (shinobi-reviewer, data-ninja) | ~9x | Revisiones, APIs, datos complejos |
| `glm-5.1` (designer) | ~40x | Solo CSS/diseño cuando flash no basta |
| `kimi-k2.6` (supervisor) | ~27x | Solo revisión final de seguridad |

**Regla:** empieza con flash, escala solo si es necesario.

### 4. Compress frecuente
- Las conversaciones cerradas se comprimen a resúmenes
- Una sesión de 100 mensajes → ~10 líneas de resumen
- Hazlo conscientemente: cuando un tema esté cerrado, dímelo

### 5. Batch de operaciones
- Pedir 5 cambios de una vez cuesta menos que 5 mensajes separados
- Ej: "corrige estos 3 archivos" en vez de uno por uno

### 6. Edits exactos > reescrituras
- Usar `edit` para cambiar líneas específicas gasta menos que reescribir un archivo entero
- Si solo cambia frontmatter, dilo explícitamente

### 7. No preguntes "qué opinas" sin contexto
- Las preguntas abiertas fuerzan a cargar todo el contexto
- Si quieres feedback, delimita: "qué opinas de X, solo en términos de Y"

## Tu configuración actual

| Métrica | Uso | Límite |
|---------|-----|--------|
| Continuo | 36% | Reinicio en 2h |
| Semanal | 48% | Reinicio en 5d |
| Mensual | 74% | Reinicio en 23d |

Con 74% a 23 días del reinicio, necesitas reducir ~30% el consumo diario.

## Checklist diario
- [ ] ¿Puedo usar `investigador` (flash) en vez de default?
- [ ] ¿Estoy pidiendo lo justo o estoy divagando?
- [ ] ¿He comprimido conversaciones cerradas hoy?
- [ ] ¿Esto que pregunto ya está documentado en el vault?
