---
type: resource
up: "[[Nexus]]"
tags: [resources, design, premium-stack, animations, APIs]
created: 2026-08-07
updated: 2026-08-07
---

# Premium Design Stack

> Stack completo de diseño premium aprobado por el usuario. **OBLIGATORIO** para nuevos proyectos web.

---

## Filosofía
**NO vanilla.** Framework obligatorio + librerías premium. El estándar del mercado es framework + librerías. Nadie hace landing pages premium en vanilla 2026.

---

## Stack Principal (Top 7)

### 1. GSAP (GreenSock)
- **Qué hace:** Animaciones scroll, parallax, text effects, ScrollTrigger
- **Precio:** 100% free (Webflow lo hizo free en 2026)
- **CDN:** `https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/gsap.min.js`
- **CDN ScrollTrigger:** `https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/ScrollTrigger.min.js`
- **Uso:** SIEMPRE en todo proyecto nuevo
- **Fuentes:** Nike, Apple, Amazon lo usan

### 2. Three.js
- **Qué hace:** 3D WebGL, escenas inmersivas, partículas, wireframes
- **Precio:** 100% free, open source
- **CDN:** `https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`
- **Uso:** Solo si hero 3D o escena inmersiva. NO abusar.
- **Nota:** Usar r128 (UMD stable), NO r150+ (deprecado para UMD)

### 3. Lenis
- **Qué hace:** Smooth scroll premium, buttery scrolling
- **Precio:** MIT free
- **CDN:** `https://cdn.jsdelivr.net/npm/lenis@1.1.18/dist/lenis.min.js`
- **Uso:** SIEMPRE en todo proyecto nuevo. Sync con GSAP ticker.
- **Fuentes:** darkroom.engineering

### 4. Rive
- **Qué hace:** Animaciones interactivas (state machines, vector)
- **Precio:** Free tier (open source runtimes)
- **CDN:** `https://cdn.jsdelivr.net/npm/@rive-app/canvas@2.25.4/dist/rive.min.js`
- **Uso:** Cuando se necesite animación vectorial interactiva

### 5. Tailwind CSS
- **Qué hace:** Estilos utility-first, no CSS vanilla
- **Precio:** Free
- **CDN:** `https://cdn.tailwindcss.com`
- **Uso:** SIEMPRE para estilos. Alternativa: CSS custom properties si Tailwind no cabe.

### 6. Framer Motion (Motion)
- **Qué hace:** Spring physics, layout animations, AI Kit
- **Precio:** MIT free
- **Uso:** Solo si usamos React/Next.js. GSAP es más vanilla-friendly.

### 7. Lottie
- **Qué hace:** Animaciones After Effects nativas
- **Precio:** Free
- **CDN:** `https://cdn.jsdelivr.net/npm/lottie-web@5.12.2/build/player/lottie.min.js`
- **Uso:** Backup de Rive. Tiene librería ENORME de animaciones gratuitas (LottieFiles).

---

## APIs de Datos/Contenido

### Datos Gaming
| API | Qué hace | Gratis | URL |
|-----|----------|--------|-----|
| **AniList** | 20K+ anime, 100K+ manga | Sí | `https://graphql.anilist.co` |
| **MangaDex** | Manga multilingüe | Sí | `https://api.mangadex.org` |
| **Simkl** | 139K TV, 14K anime, 1.1M movies | Sí | `https://api.simkl.com` |
| **Spotify** | 100M+ canciones, Wrapped | Sí | `https://api.spotify.com` |
| **IGDB** | Catálogo de juegos (Twitch) | Sí (key) | `https://api.igdb.com/v4` |

### Noticias (RSS — Gratis, sin key)
| Fuente | Idioma | URL | Notas |
|--------|--------|-----|-------|
| **Vandal** | Español | `https://vandal.elespanol.com/rss/` | Gaming puro, 10 min TTL, ISO-8859-1 |
| **HobbyConsolas** | Español | `https://www.hobbyconsolas.com/rss/` | Gaming + cine, UTF-8 |
| **HardGame2** | Español | `https://www.hardgame2.com/feed/` | Gaming + cine, horario |
| **GameSpot** | Inglés | `https://www.gamespot.com/feeds/mashup/` | Gaming puro |
| **Giant Bomb** | Inglés | `https://www.giantbomb.com/feeds/reviews/` | Reviews detalladas |

### Multimedia
| API | Qué hace | Gratis | Límite |
|-----|----------|--------|--------|
| **Unsplash** | 8.6M fotos premium | Sí | 1K/hora |
| **Pexels** | Fotos + videos stock | Sí | 200/hora |
| **Cloudinary** | Image/video optimization | Sí | 25K transforms/mes |

### Utilidad
| API | Qué hace | Gratis | Límite |
|-----|----------|--------|--------|
| **OpenWeather** | Clima, calidad de aire | Sí | 1K/día |
| **Mapbox GL** | Mapas 3D interactivos | Sí | 50K/mes |
| **Chart.js** | Charts animados | MIT | Ilimitado |
| **Web Push** | Notificaciones nativas | Free | Ilimitado |

---

## Animaciones/Visual APIs

| API | Qué hace | Gratis | Precio |
|-----|----------|--------|--------|
| **GSAP** | Scroll, parallax, text effects | 100% free | — |
| **Three.js** | 3D WebGL | 100% free | — |
| **Lenis** | Smooth scroll | MIT free | — |
| **Rive** | Animaciones interactivas | Free tier | — |
| **Lottie** | After Effects en web | 100% free | — |
| **Anime.js** | SVG morphing (24.5KB) | 100% free | — |
| **Motion** | Spring physics (React) | MIT free | — |

---

## AI/Inteligencia

| API | Qué hace | Gratis |
|-----|----------|--------|
| **OpenAI GPT-5.6** | Chat, imágenes, voz | Trial |
| **Claude API** | Razonamiento, 200K context | Free tier |
| **ElevenLabs** | Voz AI ultra-realista | 10K chars/mes |

---

## Outcome Loop (Calidad)
```
supervisor delega → agente genera → shinobi-reviewer evalúa (rubric)
  ├── PASS (≥80) → siguiente paso
  └── FAIL → feedback específico → agente revisa (max 2 iteraciones)
```
- **Rubrics:** Código, Diseño UI, APIs, PWA, Contenido
- **Impacto:** +30% tokens, +10% calidad, -70% fixes manuales
- **Max iteraciones:** 2 (más empeora según Anthropic 2026)

---

## Reglas de Uso
1. **SIEMPRE GSAP + ScrollTrigger** en todo proyecto nuevo
2. **SIEMPRE Lenis** para smooth scroll
3. **Three.js solo si** hero 3D o escena inmersiva
4. **Tailwind** para estilos (o CSS custom properties)
5. **RSS** para noticias en tiempo real (gratis, sin key)
6. **Investigador primero** — verificar endpoints antes de implementar

---

## Proyectos que usan este stack
- **GLITCH HUD** — GSAP + Three.js + Lenis + Chart.js (completado)
- **Shinobi Track** — AniList + Kitsu + Simkl (activo)
- **Makimono** — MangaDex + Kitsu (pendiente)
- **Coda** — IndexedDB + Notification API (completado)

---

*Última actualización: 2026-08-07*