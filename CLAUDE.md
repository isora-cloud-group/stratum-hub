# CLAUDE.md — Stratum-Hub Guidelines & Architecture Directives

> **“Aislamiento por diseño, no por disciplina”** — *(“Isolation by design, not by discipline”)*

## Repository Overview
`stratum-hub` is the master landing page, architectural catalog, and unified directory for the **Stratum Sovereign Multi-Tenant Architecture**. It acts as the central entry point and public reference for all platform components, orchestrators, and consumer workloads.

## Core Architectural Rules (Strict Invariants)
1. **Single Source of Truth:** Centralize navigation, compatibility matrices, and architectural standards across all `stratum-*` repositories.
2. **Zero Host Ports Philosophy:** Reinforce across all documentation that no Stratum workload exposes ports to physical host interfaces.
3. **Deterministic References:** Keep component names, links, and tier classifications strictly aligned with the canonical architecture:
   - Tier 0: Ingress & Edge
   - Tier 1: Platform Core (`stratum-core`)
   - Tier 2: Organization Islands (`stratum-org`)
   - Tier 3: Consumer Workloads (`stratum-python`, `stratum-node`, `stratum-spring`, `stratum-laravel`, `stratum-overleaf`, `stratum-nextcloud`)
   - Control Plane: Administrative CLI (`stratum-cli`)

## Attribution & Legal
- **Author & Copyright:** Sandra Gabriela Puerto Torres / Isora Cloud Group
- **License:** Apache License, Version 2.0 (Apache-2.0)
- **Version:** 2.0.0
