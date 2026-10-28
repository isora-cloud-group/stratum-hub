# Compatibility Matrix and Technical Specifications

> Verified version matrix, base container images, language runtimes, and security directives for the **Stratum** ecosystem.

---

## 1. Ecosystem Component Matrix

| Component | Official Repository | Version | Runtime / Language | Base Image / Base OS | Supported Persistence |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **`stratum-cli`** | [`stratum-cli`](https://github.com/isora-cloud-group/stratum-cli) | v2.0.0 | Python 3.10 / 3.11 / 3.12 / 3.13 | Bare-metal / Host OS | N/A (Admin Control Plane) |
| **`stratum-core`** | [`stratum-core`](https://github.com/isora-cloud-group/stratum-core) | v2.0.0 | Docker Engine 24+, Compose v2 | Alpine Linux / Debian Bookworm | PostgreSQL 16.x, MongoDB 7.0.x |
| **`stratum-org`** | [`stratum-org`](https://github.com/isora-cloud-group/stratum-org) | v1.1.0 | Nginx 1.25+ / Redis 7.2+ | `nginx:alpine`, `redis:7-alpine` | Redis 7 (Dedicated Local) |
| **`stratum-python`**| [`stratum-python`](https://github.com/isora-cloud-group/stratum-python) | v1.0.0 | Python 3.12+ (FastAPI / Django) | `python:3.12-slim` | PostgreSQL, Local Redis |
| **`stratum-node`** | [`stratum-node`](https://github.com/isora-cloud-group/stratum-node) | v1.0.0 | Node.js 20.x / 22.x LTS (Nest / Express) | `node:20-alpine` / `node:22-alpine` | PostgreSQL, MongoDB, Local Redis |
| **`stratum-spring`**| [`stratum-spring`](https://github.com/isora-cloud-group/stratum-spring) | v1.0.0 | Java 21 LTS / Spring Boot 3.3+ | `eclipse-temurin:21-jre-alpine` | PostgreSQL, Local Redis |
| **`stratum-laravel`**| [`stratum-laravel`](https://github.com/isora-cloud-group/stratum-laravel) | v2.1.0 | PHP 8.4 / 8.5 / Laravel 11/12+ | `php:8.4-fpm-alpine` | PostgreSQL, Local Redis |
| **`overleaf-stratum`**| [`overleaf-stratum`](https://github.com/isora-cloud-group/overleaf-stratum) | v3.1.0 | Node.js 18+ / Python 3 / TeX Live 2026 | `sharelatex/sharelatex:latest` | MongoDB 7, Local Redis |
| **`stratum-nextcloud`**| [`stratum-nextcloud`](https://github.com/isora-cloud-group/stratum-nextcloud) | v2.0.0 | PHP 8.3+ / Nextcloud 29+ Enterprise | `nextcloud:29-fpm-alpine` | PostgreSQL, Local Redis |

---

## 2. Hardening and Kernel Constraints Matrix

All containers deployed across the suite enforce the following security parameters in Docker Compose descriptors:

| Component | `cap_drop: [ALL]` | `no-new-privileges: true` | `read_only` Root FS | `init: true` / `tini` | Non-Root User (UID) |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **`stratum-dmz-anchor`** | Required | Required | Enabled | Native | `nobody` (UID 65534) |
| **`stratum-database-nginx`** | Required | Required | Enabled | Native | `nginx` (UID 101) |
| **`stratum-org-<org>-nginx`** | Required | Required | Enabled | Native | `nginx` (UID 101) |
| **`stratum-org-<org>-redis`** | Required | Required | Enabled | Native | `redis` (UID 999) |
| **`stratum-python`** | Required | Required | Enabled | Required (`tini`) | `appuser` (UID 10001) |
| **`stratum-node`** | Required | Required | Enabled | Required (`tini`) | `node` (UID 1000) |
| **`stratum-spring`** | Required | Required | Enabled | Required (`tini`) | `spring` (UID 10001) |
| **`stratum-laravel`** | Required | Required | Enabled | Native | `www-data` (UID 82 / 33) |
| **`overleaf-stratum`** | Required | Required | Configured with tmpfs | Required (`init: true`) | Internally managed |
| **`stratum-nextcloud`** | Required | Required | Configured with tmpfs | Native | `www-data` (UID 82) |

---

## 3. Network Connectivity and Boundaries Matrix

| Docker Subnet | Routing Scope | `internal` Directive | Internet Egress | Attached Services |
| :--- | :--- | :---: | :---: | :--- |
| **`stratum_dmz`** | Shared perimeter | `false` | Ingress tunnel only | `gateway`, `anchor`, `database-nginx`, `org-nginx` |
| **`stratum_database_internal`** | Persistence core | `true` | Strictly blocked | `postgres`, `mongo`, `database-nginx` |
| **`stratum_<org>_internal`** | Private tenant island | `true` | Strictly blocked | `org-nginx`, `org-redis`, tenant workloads |

---

## 4. Host Server Software Baseline

* **Linux Kernel:** 5.15 LTS or higher with full Linux cgroups v2 support.
* **Container Engine:** Docker Engine 24.0 or higher (Docker Engine 26.x+ recommended).
* **Python Runtime (Host):** Version 3.10, 3.11, 3.12, or 3.13 for the `stratum` executable.
* **Storage Subsystem:** Dedicated SSD or NVMe storage backed by ext4 or XFS filesystems mounted with POSIX attribute support.
