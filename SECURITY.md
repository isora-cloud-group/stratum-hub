# Security Policy

**Repository:** `stratum-hub` (Stratum Sovereign Multi-Tenant Architecture Hub)  
**Security Operations:** security@isora.com.co  
**General Contact:** contact@isora.com.co  
**Revision:** 2.0.0  
**Status:** Production  

---

## 1. Supported Versions

Security updates are provided for the following active revisions:

| Component | Supported Revision | Status |
|---|---|---|
| `stratum-hub` | 2.0.x | :white_check_mark: Active / Supported |
| `stratum-core` | 2.0.x | :white_check_mark: Active / Supported |
| `stratum-cli` | 2.0.x | :white_check_mark: Active / Supported |
| `stratum-org` | 1.1.x | :white_check_mark: Active / Supported |
| Tier 3 Workloads (`laravel`, `overleaf`, `python`, `node`, `spring`, `nextcloud`) | 1.0.x - 3.x | :white_check_mark: Active / Supported |
| Platform Baseline | 1.x | :white_check_mark: Supported |
| Older releases | < 1.0 | :x: Unsupported |

The current revision of each component is declared in the header and Document Control table of its respective `docker-compose.yml` and `README.md`. Consumers are expected to track the current revisions of the repository.

---

## 2. Reporting a Vulnerability

**Do not report security vulnerabilities through public GitHub issues, discussions, or any other public channel.**

To report a vulnerability, send an email to:

> **security@isora.com.co**

Include the following information in the report:

1. **Type of vulnerability:** A short classification (e.g., privilege escalation, lateral movement, information disclosure, container escape, misconfiguration).
2. **Affected component:** Specify `dmz`, `gateway`, `database`, or the repository as a whole.
3. **Affected revision:** The revision number declared in the component's header, or the commit hash if the report concerns the repository directly.
4. **Description:** A precise description of the issue and the conditions under which it manifests.
5. **Reproduction steps:** A minimal, reproducible sequence of actions demonstrating the vulnerability.
6. **Impact assessment:** An evaluation of the potential consequences if left unaddressed.
7. **Suggested remediation:** Optional. Any proposed patch or mitigation.

If the report contains sensitive information, encrypt the communication or arrange a secure channel via an initial unencrypted email containing no sensitive details.

---

## 3. Response Timeline

The following timeline is committed on a best-effort basis:

| Stage | Target |
|---|---|
| Acknowledgment of receipt | Within 48 hours |
| Initial assessment and severity classification | Within 7 days |
| Remediation for critical issues | As soon as feasible |
| Remediation for high severity | Within 30 days |
| Remediation for medium severity | Within 90 days |
| Public disclosure | Coordinated with reporter post-release |

---

## 4. Disclosure Process

1. **Report received:** The reporter receives acknowledgment.
2. **Triage:** The issue is reproduced and classified.
3. **Remediation:** A fix is developed and validated in a staging environment.
4. **Release:** The fix is published as a new revision of the affected component.
5. **Advisory:** A public disclosure advisory is issued with credit to the reporter unless anonymity is requested.

---

## 5. Scope

### 5.1 In scope

- Configuration errors in the `docker-compose.yml` files of any component.
- Multi-organization tenant isolation weaknesses, lateral movement vectors, or cross-tunnel leakage in `gateway/`.
- Weaknesses in network topology allowing a container to reach a network or host interface it is not attached to.
- Capability or permission configurations that exceed the minimum required by upstream images (`cap_drop`, `cap_add`, `no-new-privileges`).
- Hardcoded credentials or secrets in version-controlled configurations.
- Weaknesses in the Layer 4 boundary client configuration under `database/nginx/`.
- Vulnerabilities introduced by this orchestration pattern that would not exist in standard Docker setups.

### 5.2 Out of scope

- Vulnerabilities in upstream container images (`postgres`, `mongo`, `redis`, `nginx`, `alpine`, `cloudflared`, `jc21/nginx-proxy-manager`). Report these upstream.
- Vulnerabilities in the Docker Engine or Docker Compose daemon.
- Vulnerabilities in Cloudflare infrastructure or edge protocols.
- Social engineering attacks against infrastructure operators.
- Denial-of-service attacks that require root or physical host-level access.
- Vulnerabilities in host-level OS configuration (firewall, SSH, kernel).

---

## 6. Safe Harbor

Security research conducted in good faith against a deployment of `stratum-core` is considered authorized. The maintainer will not pursue legal action against researchers who:

- Make a good-faith effort to avoid privacy violations, data destruction, and service interruption.
- Interact only with deployments they own, operate, or have explicit permission to test.
- Do not exploit a vulnerability beyond the minimum necessary to demonstrate its existence.
- Report the vulnerability promptly and privately in accordance with Section 2.
- Coordinate disclosure timelines prior to public release.

---

## 7. Security Best Practices for Operators

1. Restrict all `.env` file permissions to `chmod 600`.
2. Never commit `.env` or credential files to version control (`.gitignore` enforced).
3. Rotate Cloudflare Tunnel tokens and database administrative credentials periodically.
4. Keep Docker Engine and host Linux kernels up to date with security patches.
5. Validate upstream image releases before applying production updates.
6. Perform regular backups of stateful database volumes prior to pulling new images.
7. Monitor Docker logs and audit trails through an external SIEM/syslog agent.

---

## 8. Document Control

| Field | Value |
|---|---|
| Repository | `stratum-core` |
| Contact | security@isora.com.co (General: contact@isora.com.co) |
| Revision | 1.2.0 |
| Status | Production |
| Classification | Public Security Policy |