# Registro de Cambios (Changelog)

Todos los cambios notables en la plataforma y arquitectura **Stratum Sovereign Multi-Tenant** seran documentados en este archivo.

El formato esta basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y este proyecto se adhiere a [Semantic Versioning](https://semver.org/lang/es/).

---

## [2.0.0] - 2026-10-28

### Anadido
- **Repositorio Central `stratum-hub`:** Creacion del punto de aterrizaje, catalogo de integraciones y documentacion maestra para la suite.
- **Topologia Zero-Trust de 4 Capas (Tiers):**
  - Tier 0: Ingress Perimetral mediado por tuneles Cloudflare Zero Trust.
  - Tier 1: Platform Core con DMZ perimetral y red ciega interna para persistencia (`internal: true`).
  - Tier 2: Organizacion e Islas de Inquilino con enrutamiento dual-homed y Redis 7 soberano local.
  - Tier 3: Cargas de trabajo contenerizadas estandarizadas (Python, Node.js, Spring Boot, Laravel, Overleaf y Nextcloud).
- **Plano de Control `stratum-cli` v2.0.0:**
  - Arquitectura Zero-Dependency en tiempo de ejecucion basada exclusivamente en la biblioteca estandar de Python 3.10+.
  - Esponja de entropia criptografica fisica utilizando HMAC-SHA512.
  - Comandos para creacion determinista de organizaciones (`org create`), persistencia (`db create`) y auditoria de salud (`doctor`).
- **Hardening Estandarizado:**
  - Despojo universal de capacidades Linux (`cap_drop: [ALL]`).
  - Prevencion de escalada de privilegios (`no-new-privileges: true`).
  - Sistemas de archivos raiz inmutables (`read_only: true`) y procesos supervisados via `tini`.

### Modificado
- Migracion integral de enlaces al espacio oficial institucional `https://github.com/isora-cloud-group/stratum-*`.
- Desacoplamiento de las bases de datos de la red DMZ mediante mediacion estricta en Capa 4 (TCP Stream Proxy).

### Eliminado
- Eliminada toda publicacion directa de puertos hacia interfaces fisicas del host en entornos de produccion (regla *Zero Host Ports*).

---

## [1.1.0] - 2026-07-15

### Anadido
- Soporte para reenvio Split-Horizon en la capa Nginx de cada organizacion.
- Aislamiento de almacenamiento volatil con Redis dedicado por inquilino.
- Plantilla inicial para cargas de trabajo científicas con Overleaf Community Edition.

### Modificado
- Refactorizacion de la gestion de redes Docker para soportar directivas `internal: true`.

---

## [1.0.0] - 2026-03-10

### Anadido
- Sintesis inicial de la arquitectura multi-inquilino soberana.
- Prototipo del plano de persistencia con PostgreSQL y MongoDB.
- Scripts iniciales de aprovisionamiento de tenants y politicas de aislamiento de red.

