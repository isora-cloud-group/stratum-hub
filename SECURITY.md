# Security Policy

**Repository:** `stratum-hub` (Stratum Sovereign Multi-Tenant Architecture Hub)  
**Security Operations:** security@isora.com.co  
**General Inquiries:** contact@isora.com.co  
**Revision:** v2.0.0  
**Status:** Production  

---

## 1. Supported Revisions

Security updates are provided for the following active revisions across the suite:

| Component | Supported Revision | Status |
| :--- | :---: | :--- |
| `stratum-hub` | v2.0.x | Active / Supported |
| `stratum-core` | v2.0.x | Active / Supported |
| `stratum-cli` | v2.0.x | Active / Supported |
| `stratum-org` | v1.1.x | Active / Supported |
| Tier 3 Workloads (`laravel`, `overleaf`, `python`, `node`, `spring`, `nextcloud`) | v1.0.x - v3.x | Active / Supported |
| Legacy Platform Baseline | v1.x | Supported |
| Older releases | < v1.0 | Unsupported |

The current revision of each component is declared in its respective repository descriptor and `README.md`. Consumers are expected to track current releases.

---

## 2. Vulnerability Reporting

Do not report security vulnerabilities through public GitHub issues, discussions, or pull requests.

To report a vulnerability, transmit an encrypted or private disclosure to:

> **security@isora.com.co**

Include the following diagnostic data in the disclosure report:

1. **Vulnerability Classification:** Category identifier (e.g., privilege escalation, lateral movement, information disclosure, container escape, configuration regression).
2. **Affected Component:** Target component (`stratum-hub`, `stratum-core`, `stratum-cli`, `stratum-org`, or specific workload).
3. **Affected Revision:** Revision number declared in the component header or Git commit hash.
4. **Description:** Technical description of the vulnerability and required execution conditions.
5. **Reproduction Steps:** Minimal, deterministic proof-of-concept steps to reproduce the behavior.
6. **Impact Assessment:** Evaluation of potential security implications across tenant perimeters.
7. **Suggested Remediation:** Optional patch or mitigation proposal.

---

## 3. Response Timelines

Security responses adhere to the following service targets:

| Stage | Target Window |
| :--- | :--- |
| Initial Acknowledgment | Within 48 hours |
| Severity Assessment and Triage | Within 7 days |
| Critical Severity Remediation | Immediate priority |
| High Severity Remediation | Within 30 days |
| Medium Severity Remediation | Within 90 days |
| Public Disclosure | Coordinated post-release |

---

## 4. Disclosure Protocol

1. **Intake:** Receipt of vulnerability report is acknowledged within 48 hours.
2. **Triage:** Reproduction and severity scoring under CVSS v3.1.
3. **Remediation:** Patch development and validation within isolated testing environments.
4. **Release:** Coordinated deployment of patched revisions.
5. **Advisory:** Publication of security advisory with attribution to the reporter unless anonymity is requested.

---

## 5. Security Scope

### 5.1 In-Scope Targets

* Configuration errors within Docker Compose descriptors across all components.
* Multi-tenant network isolation failures, lateral movement vectors, or cross-island packet routing.
* Subnet routing flaws permitting communication to unattached network namespaces or physical host interfaces.
* Container capability configurations exceeding required baselines (`cap_drop`, `cap_add`, `no-new-privileges`).
* Secret or credential exposure in version-controlled configurations.
* Boundary Layer-4 stream routing flaws in `stratum-core` persistence proxies.

### 5.2 Out-of-Scope Targets

* Vulnerabilities residing in upstream third-party container images (`postgres`, `mongo`, `redis`, `nginx`, `alpine`, `cloudflared`). Report these to upstream maintainers.
* Vulnerabilities in the Docker Engine daemon or Linux kernel.
* Vulnerabilities in Cloudflare edge infrastructure or protocols.
* Social engineering attacks against infrastructure operators.
* Denial-of-service attacks requiring pre-existing physical host or root access.
* Host operating system misconfigurations outside Stratum automation.

---

## 6. Safe Harbor

Security research conducted in good faith against deployments of Stratum components is authorized. The maintainers will not pursue legal action against researchers who:

* Avoid privacy violations, data destruction, and service degradation.
* Interact exclusively with environments they own or have authorization to test.
* Restrict exploitation strictly to the minimum necessary to validate existence.
* Report vulnerabilities promptly and privately in accordance with Section 2.
* Maintain confidentiality until coordinated disclosure release.

---

## 7. Operator Security Directives

1. Restrict all `.env` configuration files to `chmod 600`.
2. Enforce `.gitignore` controls to prevent credential commits to version control.
3. Rotate Cloudflare Tunnel credentials and database master secrets periodically.
4. Maintain current security patch baselines on Docker Engine and host Linux kernels.
5. Audit upstream container image releases prior to production deployment.
6. Execute automated stateful volume backups prior to container upgrades.
7. Stream Docker container audit trails to centralized external syslog or SIEM collectors.

---

## 8. Document Control

| Field | Value |
| :--- | :--- |
| **Repository** | `stratum-hub` |
| **Security Contact** | security@isora.com.co |
| **General Contact** | contact@isora.com.co |
| **Revision** | v2.0.0 |
| **Status** | Production |
| **Classification** | Public Security Policy |
