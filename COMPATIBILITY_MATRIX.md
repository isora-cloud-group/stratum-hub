# Stratum Compatibility Matrix
> **Matriz de Compatibilidad de Componentes y Versiones Soportadas**  
> *Revisión 2.0.0 — Octubre 2026*

Esta tabla detalla las versiones de software, motores de ejecución y sistemas operativos validados con la suite **Stratum Sovereign Multi-Tenant Architecture**.

---

## 1. Requisitos de Infraestructura y Host

| Componente del Host | Versión Mínima | Versión Recomendada | Notas de Soporte |
| :--- | :--- | :--- | :--- |
| **Sistema Operativo** | Linux Kernel 5.15+ | Linux Kernel 6.5+ / 6.8+ | Ubuntu Server 22.04/24.04 LTS, Debian 12, RHEL 9 |
| **Docker Engine** | 24.0+ | 27.0+ | Soporte nativo para `cgroups v2` y redes internas |
| **Docker Compose** | Compose v2.20+ | Compose v2.29+ | Uso del comando moderno `docker compose` (sin guión) |
| **Arquitectura CPU** | `x86_64` (amd64) | `x86_64` / `arm64` | Soporte multi-arquitectura verificado |

---

## 2. Componentes de Plataforma y Persistencia

| Repositorio / Servicio | Motor / Imagen Base | Versión Probada | Estado |
| :--- | :--- | :--- | :--- |
| **`stratum-core` (DMZ)** | `alpine:3.20` | Última estable | En Producción |
| **`stratum-core` (PostgreSQL)** | `postgres:16-alpine` | 16.4+ | En Producción |
| **`stratum-core` (MongoDB)** | `mongo:7.0` | 7.0.12+ | En Producción |
| **`stratum-org` (Frontier Nginx)** | `nginx:1.27-alpine` | 1.27+ | En Producción |
| **`stratum-org` (Local Redis)** | `redis:7-alpine` | 7.2+ | En Producción |
| **`stratum-cli`** | Python Standard Library | Python 3.10, 3.11, 3.12, 3.13 | En Producción (Zero-Deps) |

---

## 3. Cargas de Trabajo (Consumer Workloads)

| Repositorio | Lenguaje / Runtime | Frameworks Principales | Supervisor Init |
| :--- | :--- | :--- | :--- |
| **`stratum-python`** | Python 3.12+ | FastAPI 0.115+, Django 5.1+, Celery 5.4+ | Tini (v0.19) |
| **`stratum-node`** | Node.js 20 / 22 LTS | NestJS 10+, Express 4/5, TypeScript 5+ | Tini (v0.19) |
| **`stratum-spring`** | Java 21 LTS (Eclipse Temurin) | Spring Boot 3.3+, Spring Data, Hibernate | Tini (v0.19) |
| **`stratum-laravel`** | PHP 8.4 / 8.5 | Laravel 11/12+, FrankenPHP / PHP-FPM | Tini (v0.19) |
| **`stratum-overleaf`** | Node.js + TeX Live 2026 | Overleaf Community Edition 5.x | Tini (`init: true`) |
| **`stratum-nextcloud`** | PHP 8.3 / Apache | Nextcloud Hub 29/30+ Enterprise | Tini (`init: true`) |
