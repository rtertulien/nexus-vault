---
title: "PixelLab"
type: reference
status: active
up: "[[Nexus]]"
date: 2026-07-10
tags:
  - pixel-art
  - ai
  - sprites
  - pixellab
  - gamedev
  - api
---

# PixelLab — Plataforma de IA para Sprites

> Generador de assets pixel art para videojuegos. 3,000+ usuarios indie. Empresa de Suecia.

## Qué es

Plataforma de IA especializada en sprites pixel art. Modelos custom sobre Stable Diffusion. No es un generador general — está construido exclusivamente para crear sprites, animaciones, tilesets, y elementos de UI en pixel art.

## Herramientas disponibles (14+)

| Herramienta | Qué hace |
|---|---|
| Create Character | Personajes 4/8 direcciones (48x48 a 168x168) |
| Animate | Animaciones con esqueleto o texto (hasta 16 frames) |
| Rotate | Rota sprites 4/8 direcciones |
| Create Tileset | Wang tiles top-down, sidescroller, isometric |
| Create Map Objects | Objetos para el mapa |
| Image to PixelArt | Convierte fotos a pixel art |
| Generate UI | Health bars, botones, paneles |
| Inpaint | Edita partes específicas de un sprite |
| Remove Background | Elimina fondo → PNG transparente |
| Resize | Redimensiona sin perder estética pixel art |
| Enhance Prompt | Mejora tus descripciones automáticamente |
| Portrait → Character | Convierte retrato a sprite |
| Generate Font | Fuentes pixel art |
| Edit Image | Edita pixel art existente |

## API

- **Base URL:** `https://api.pixellab.ai/v2`
- **Auth:** Bearer token
- **Endpoints:** 60+ organizados por categoría
- **Docs:** `https://api.pixellab.ai/v2/docs`
- **OpenAPI:** `https://api.pixellab.ai/v2/openapi.json`
- **LLMs:** `https://api.pixellab.ai/v2/llms.txt`

### Precios API (por llamada)

| Endpoint | Precio |
|---|---|
| Create image (Pixflux) 64x64 | $0.00793 |
| Create image (Pixflux) 128x128 | $0.00793 |
| Create image (Pixflux) 320x320 | $0.0101 |
| Create image (Pixflux) 400x400 | $0.0132 |
| Character 4 dirs 64x64 | $0.0122 |
| Character 8 dirs 64x64 | $0.0173 |
| Character v3 64x64 | $0.041 |
| Character Pro 168x168 | $0.185 |
| Animate with text 64x64, 4 frames | $0.01565 |
| Animate with skeleton 64x64 | $0.01433 |
| Rotate 64x64 | $0.01057 |
| 8 rotations v3 64x64 | $0.0337 |
| Inpaint 128x128 | $0.00797 |
| Tileset top-down 32x32 | $0.0099 |
| Isometric tile 32x32 | $0.0156 |
| Map object | $0.0099 |
| Enhance prompt | $0.002 |
| Image to pixel art 128x128 | $0.00666 |
| Resize 128x128 | $0.01777 |
| Remove background 128x128 | $0.00554 |

## Integración OpenCode (MCP)

### Configuración

```json
{
  "mcpServers": {
    "pixellab": {
      "url": "https://api.pixellab.ai/mcp",
      "transport": "http",
      "headers": {
        "Authorization": "Bearer TU_TOKEN"
      }
    }
  }
}
```

### One-click setup

`https://www.pixellab.ai/vibe-coding`

### Tools MCP

- `create_character` — Personajes 4/8 direcciones
- `animate_character` — Animar personajes existentes
- `create_topdown_tileset` — Wang tiles top-down
- `create_sidescroller_tileset` — Tiles platformer
- `create_isometric_tile` — Tiles isométricos
- `create_map_object` — Objetos para mapas con style matching

### Clientes soportados

Claude Code, VS Code, Cursor, Gemini CLI, Zed, Cline, Claude Desktop, Windsurf, Continue, OpenCode, y más.

## SDKs

### Python

```bash
pip install pixellab
```

```python
import pixellab
client = pixellab.Client.from_env_file(".env.secrets")
response = client.generate_image_pixflux(
    description="cute dragon",
    image_size={"width": 64, "height": 64}
)
response.image.pil_image()
```

### JavaScript

```bash
npm install @pixellab-code/pixellab
```

```javascript
import { PixelLabClient } from "@pixellab-code/pixellab";
const client = PixelLabClient.fromEnv();
const response = await client.generateImagePixflux({
  description: "cute dragon",
  imageSize: { width: 64, height: 64 }
});
await response.image.saveToFile("dragon.png");
```

## Limitaciones

- Resolución máx real: 128x128-256x256 (Pro hasta 512x512)
- No reemplaza artista humano para AAA
- Consistencia de estilo requiere init images
- Animaciones complejas necesitan edición manual
- No almacena imágenes — si cierras sin download, se pierden

## Licencia

- **Commercial use:** Permitido (Steam, iOS, etc.)
- **Training data:** No usa tus imágenes para entrenar
- **Jurisdicción:** Suecia
- **Modelo:** Open RAIL-M

## Pricing Web (suscripción)

| Tier | Precio | Imágenes/mes | Resolución |
|---|---|---|---|
| Free | $0 | 40 fast + 5/día slow | 200x200 |
| Tier 1 | $12/mes | 2,000 | 320x320 |
| Tier 2 | $24/mes | 5,000 | 512x512 |
| Tier 3 | $50/mes | 10,000 | 512x512 |

## Recomendación

**Para proyectos indie/game jams:** Muy recomendado. $12 de crédito API cubre asset packs completos.

**Alternativa:** Complementar con packs de itch.io ($5-15) para objetos genéricos y ahorrar crédito para lo importante (personajes, bosses).

## Fuentes

- https://pixellab.ai — Homepage
- https://api.pixellab.ai/v2/docs — API docs
- https://github.com/pixellab-code/pixellab-mcp — MCP server (37 stars)
- https://github.com/pixellab-code/pixellab-python — Python SDK (26 stars)
- https://github.com/pixellab-code/pixellab-js — JS SDK (3 stars)
