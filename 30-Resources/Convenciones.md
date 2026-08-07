---
type: reference
tags: [resource]
up: "[[Nexus]]"
---

> Convenciones del vault.  
> [[Nexus|🏠 Nexus]]

# Convenciones del vault

## Frontmatter
Toda nota debe tener frontmatter YAML con `tags`.

### Proyectos (#project)
- `status`: activo | en-hold | idea | done
- `prioridad`: alta | media | baja
- `inicio`: fecha ISO
- `fuentes`: lista de APIs/sources

### Dailies (#daily)
- `fecha`: fecha ISO
- `proyectos`: lista de tags de proyectos tocados

## Enlaces
- Proyecto → Daily: `proyectos: [shinobi-track]` en la daily
- Daily → Proyecto: `[[Shinobi Track]]` en la sección proyectos
- Siempre encerrar rutas de código entre backticks

## Tags principales
- `#project` — notas de proyecto
- `#daily` — notas diarias
- `#resource` — recursos y referencias
- `#spec` — especificaciones técnicas

## Templates
- [[30-Resources/Daily Template|Daily Template]] — plantilla notas diarias Ivy Lee
- [[30-Resources/Project Template|Project Template]] — plantilla proyectos
- [[30-Resources/Resource Template|Resource Template]] — plantilla recursos (progressive summarization)
