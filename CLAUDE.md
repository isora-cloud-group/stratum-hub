# CLAUDE.md — Architecture Directives and Guidelines for AI Assistants

> *“Isolation by design, not by discipline”*

## 1. Repository Overview
`stratum-hub` serves as the central directory, navigable catalog, and master architectural reference for the **Stratum Sovereign Multi-Tenant Architecture** suite. It acts as the integration anchor for all satellite repositories and containerized workloads operated under **Isora Cloud Group (ICG)**.

---

## 2. Inviolable Architectural Invariants
Any technical modification or code proposed for the Stratum ecosystem must unconditionally respect:

1. **Zero Host Ports:** Never map ports directly onto physical host network interfaces (`ports: - "8080:8080"` or similar). All ingress traffic enters strictly through outbound encrypted edge tunnels into `stratum_dmz`.
2. **Blind Persistence Network:** The `stratum_database_internal` bridge network must always maintain `internal: true`. It has no default gateway and no route to the internet.
3. **Layer-4 Persistence Mediation:** Database engines (PostgreSQL, MongoDB) are never attached directly to `stratum_dmz`. Tenant access must be mediated via the Layer-4 TCP stream proxy `stratum-database-nginx`.
4. **Blind Tenant Island:** Each tenant bridge network (`stratum_<org>_internal`) is strictly blind (`internal: true`). Tenant workloads have no route to the DMZ and cannot reach neighboring tenant networks.
5. **Dedicated Sovereign Local Redis:** Every tenant operates its own Redis 7 container inside its private network. Shared multi-tenant Redis instances with logical database indices are prohibited.
6. **Zero External Runtime Dependencies in `stratum-cli`:** The administrative CLI must execute exclusively using the Python 3.10+ standard library (zero pip packages required at runtime).
7. **Deterministic Slugs:** All organization and service identifiers must validate against:
   `^[a-z0-9]+(?:-[a-z0-9]+)*$`

---

## 3. Canonical Organizational Links
When referencing Stratum repositories in documentation or automated responses, always use official organizational links:
* Central Directory: `https://github.com/isora-cloud-group/stratum-hub`
* Control Plane: `https://github.com/isora-cloud-group/stratum-cli`
* Base Platform: `https://github.com/isora-cloud-group/stratum-core`
* Tenant Island: `https://github.com/isora-cloud-group/stratum-org`
* Python Workload: `https://github.com/isora-cloud-group/stratum-python`
* Node.js Workload: `https://github.com/isora-cloud-group/stratum-node`
* Java/Spring Workload: `https://github.com/isora-cloud-group/stratum-spring`
* PHP/Laravel Workload: `https://github.com/isora-cloud-group/stratum-laravel`
* LaTeX/Overleaf Workload: `https://github.com/isora-cloud-group/overleaf-stratum`
* Cloud/Nextcloud Workload: `https://github.com/isora-cloud-group/stratum-nextcloud`

---

## 4. Tone and Style Directives
* **Formal Technical Register:** Avoid emojis, informal phrasing, or marketing rhetoric. Maintain a disciplined, professional systems engineering register.
* **Unified Terminology:** Enforce standard technical terms: *isolation by design*, *four-tier zero-trust topology*, *L4 stream proxy*, *kernel hardening*, *unprivileged containers*, *topological island*, *tenant*, *workloads*.
* **Diagram Standards:** Maintain Mermaid diagrams vertically oriented (`flowchart TD`) with concise, clean node labels and no embedded link pollution.

---

## 5. Legal Attribution and Authorship
* **Lead Architect & Copyright Holder:** Sandra Gabriela Puerto Torres (SGPT)
* **Distributor & Brand:** Isora Cloud Group (ICG) — `https://isora.com.co`
* **License:** Apache License, Version 2.0 (Apache-2.0)
