# Matriz de Compatibilidad y Especificaciones Tecnicas

> Registro exhaustivo de versiones validadas, imagenes base, compiladores y directivas de seguridad para los componentes del ecosistema **Stratum**.

---

## 1. Matriz de Componentes del Ecosistema

| Componente | Repositorio Oficial | Version | Runtime / Lenguaje | Imagen Base / Base OS | Persistencia Soportada |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`stratum-cli`** | [`stratum-cli`](https://github.com/isora-cloud-group/stratum-cli) | 2.0.0 | Python 3.10 / 3.11 / 3.12 / 3.13 | Bare-metal / Host OS | N/A (Admin Control Plane) |
| **`stratum-core`** | [`stratum-core`](https://github.com/isora-cloud-group/stratum-core) | 2.0.0 | Docker Engine 24+, Compose v2 | Alpine Linux / Debian Bookworm | PostgreSQL 16.x, MongoDB 7.0.x |
| **`stratum-org`** | [`stratum-org`](https://github.com/isora-cloud-group/stratum-org) | 1.1.0 | Nginx 1.25+ / Redis 7.2+ | `nginx:alpine`, `redis:7-alpine` | Redis 7 (Local Dedicado) |
| **`stratum-python`**| [`stratum-python`](https://github.com/isora-cloud-group/stratum-python) | 1.0.0 | Python 3.12+ (FastAPI / Django) | `python:3.12-slim` | PostgreSQL, Redis Local |
| **`stratum-node`** | [`stratum-node`](https://github.com/isora-cloud-group/stratum-node) | 1.0.0 | Node.js 20.x / 22.x LTS (Nest / Express) | `node:20-alpine` / `node:22-alpine` | PostgreSQL, MongoDB, Redis Local |
| **`stratum-spring`**| [`stratum-spring`](https://github.com/isora-cloud-group/stratum-spring) | 1.0.0 | Java 21 LTS / Spring Boot 3.3+ | `eclipse-temurin:21-jre-alpine` | PostgreSQL, Redis Local |
| **`stratum-laravel`**| [`stratum-laravel`](https://github.com/isora-cloud-group/stratum-laravel) | 2.1.0 | PHP 8.4 / 8.5 / Laravel 11/12+ | `php:8.4-fpm-alpine` | PostgreSQL, Redis Local |
| **`stratum-overleaf`**| [`overleaf-stratum`](https://github.com/isora-cloud-group/overleaf-stratum) | 3.1.0 | Node.js 18+ / Python 3 / TeX Live 2026 | `sharelatex/sharelatex:latest` | MongoDB 7, Redis Local |
| **`stratum-nextcloud`**| [`stratum-nextcloud`](https://github.com/isora-cloud-group/stratum-nextcloud) | 2.0.0 | PHP 8.3+ / Nextcloud 29+ Enterprise | `nextcloud:29-fpm-alpine` | PostgreSQL, Redis Local |

---

## 2. Matriz de Hardening y Restricciones de Kernel

Todos los contenedores desplegados en la suite deben verificar los siguientes parametros en sus descriptores Docker Compose:

| Componente | `cap_drop: [ALL]` | `no-new-privileges: true` | `read_only` Root FS | `init: true` / `tini` | Usuario No-Root (UID) |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **`stratum-dmz-anchor`** | Requerido | Requerido | Habilitado | Nativo | `nobody` (UID 65534) |
| **`stratum-database-nginx`** | Requerido | Requerido | Habilitado | Nativo | `nginx` (UID 101) |
| **`stratum-org-<org>-nginx`** | Requerido | Requerido | Habilitado | Nativo | `nginx` (UID 101) |
| **`stratum-org-<org>-redis`** | Requerido | Requerido | Habilitado | Nativo | `redis` (UID 999) |
| **`stratum-python`** | Requerido | Requerido | Habilitado | Requerido (`tini`) | `appuser` (UID 10001) |
| **`stratum-node`** | Requerido | Requerido | Habilitado | Requerido (`tini`) | `node` (UID 1000) |
| **`stratum-spring`** | Requerido | Requerido | Habilitado | Requerido (`tini`) | `spring` (UID 10001) |
| **`stratum-laravel`** | Requerido | Requerido | Habilitado | Nativo | `www-data` (UID 82 / 33) |
| **`stratum-overleaf`** | Requerido | Requerido | Con montajes tmpfs | Requerido (`init: true`) | Gestionado internamente |
| **`stratum-nextcloud`** | Requerido | Requerido | Con montajes tmpfs | Nativo | `www-data` (UID 82) |

---

## 3. Matriz de Conectividad de Red y Fronteras

| Subred Docker | Alcance de Enrutamiento | Parametro `internal` | Egress hacia Internet | Servicios Conectados |
| :--- | :--- | :---: | :---: | :--- |
| **`stratum_dmz`** | Perimetral compartido | `false` | Unicamente via Ingress Tunnel | `gateway`, `anchor`, `database-nginx`, `org-nginx` |
| **`stratum_database_internal`** | Nucleo de persistencia | `true` | Estrictamente Bloqueado | `postgres`, `mongo`, `database-nginx` |
| **`stratum_<org>_internal`** | Privado por organizacion | `true` | Estrictamente Bloqueado | `org-nginx`, `org-redis`, cargas de trabajo del inquilino |

---

## 4. Requerimientos de Software del Servidor Host

* **Linux Kernel:** 5.15 LTS o superior con soporte completo para cgroups v2.
* **Docker Engine:** Version 24.0 o superior (recomendado 26.x+).
* **Python Runtime (Host):** Version 3.10, 3.11, 3.12 o 3.13 para el ejecutable `stratum`.
* **Almacenamiento:** Unidades SSD o NVMe dedicadas con sistema de archivos ext4 o xfs montados con soporte para cuotas y atributos POSIX.

