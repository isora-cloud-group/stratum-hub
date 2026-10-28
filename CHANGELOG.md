# Changelog

All notable changes to the **Stratum Sovereign Multi-Tenant** architecture and platform are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [2.0.0] - 2026-10-28

### Added
- **Central Repository `stratum-hub`:** Master integration catalog, technical reference specifications, and ADR directory for the ecosystem.
- **Four-Tier Zero-Trust Topology:**
  - Tier 0: Ingress perimeter mediated via Cloudflare Zero Trust tunnels.
  - Tier 1: Platform Core with DMZ perimeter subnet and blind internal persistence network (`internal: true`).
  - Tier 2: Tenant Islands featuring dual-homed front routers and dedicated sovereign local Redis 7 instances.
  - Tier 3: Standardized containerized workloads (Python, Node.js, Spring Boot, Laravel, Overleaf, and Nextcloud).
- **Control Plane `stratum-cli` v2.0.0:**
  - Zero-dependency runtime architecture implemented entirely within the Python 3.10+ standard library.
  - Multi-source physical cryptographic entropy harvesting with HMAC-SHA512 sponge compression.
  - Deterministic CLI operations for tenant provisioning (`org create`), database configuration (`db create`), and preventive health auditing (`doctor`).
- **Standardized Container Hardening:**
  - Universal Linux capability dropping (`cap_drop: [ALL]`).
  - Privilege escalation prevention (`security_opt: ["no-new-privileges:true"]`).
  - Immutable root filesystems (`read_only: true`), volatile mounts on `tmpfs`, and PID 1 zombie reaping via `tini`.

### Changed
- Migrated all repository links and references to the official organizational namespace `https://github.com/isora-cloud-group/stratum-*`.
- Decoupled database engines from the DMZ network using strict Layer-4 TCP stream proxies (`stratum-database-nginx`).

### Removed
- Eliminated all host port publications (`ports:`) bound directly to physical host interfaces (`0.0.0.0`) in production environments (Zero Host Ports invariant).

---

## [1.1.0] - 2026-07-15

### Added
- Split-horizon routing support within the per-tenant Nginx routing tier.
- Volatile memory isolation using dedicated local Redis containers per tenant.
- Official workload template for collaborative LaTeX authoring via Overleaf Community Edition.

### Changed
- Refactored Docker network management to enforce `internal: true` directives across tenant subnets.

---

## [1.0.0] - 2026-03-10

### Added
- Foundational architectural synthesis of the sovereign multi-tenant container model.
- Core persistence subsystem prototype supporting PostgreSQL and MongoDB.
- Initial tenant provisioning automation scripts and network namespace isolation policies.
